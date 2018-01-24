---
title: "キー管理"
author: rick-anderson
description: "このドキュメントでは、ASP.NET Core データ保護キー管理 Api の実装の詳細について説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 53adb067751917a9539a310bb7d91e599696f213
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="key-management"></a><span data-ttu-id="70e00-103">キー管理</span><span class="sxs-lookup"><span data-stu-id="70e00-103">Key Management</span></span>

<a name="data-protection-implementation-key-management"></a>

<span data-ttu-id="70e00-104">データ保護システムは、自動的に保護し、ペイロードの保護を解除するために使用するマスター _ キーの有効期間を管理します。</span><span class="sxs-lookup"><span data-stu-id="70e00-104">The data protection system automatically manages the lifetime of master keys used to protect and unprotect payloads.</span></span> <span data-ttu-id="70e00-105">各キーは、4 つの段階のいずれかに存在できます。</span><span class="sxs-lookup"><span data-stu-id="70e00-105">Each key can exist in one of four stages:</span></span>

* <span data-ttu-id="70e00-106">作成されるキーはキー リング内に存在するが、まだアクティブになっていません。</span><span class="sxs-lookup"><span data-stu-id="70e00-106">Created - the key exists in the key ring but has not yet been activated.</span></span> <span data-ttu-id="70e00-107">キー使用できない新しい保護する操作のための十分な時間が経過するまで、キーがこのキーのリングを消費しているすべてのマシンに反映されるまでの機会がいること。</span><span class="sxs-lookup"><span data-stu-id="70e00-107">The key shouldn't be used for new Protect operations until sufficient time has elapsed that the key has had a chance to propagate to all machines that are consuming this key ring.</span></span>

* <span data-ttu-id="70e00-108">アクティブ - キーは、キー リングに存在し、すべての新しい保護操作に使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="70e00-108">Active - the key exists in the key ring and should be used for all new Protect operations.</span></span>

* <span data-ttu-id="70e00-109">期限切れのキーは、自然な有効期間は実行し、保護する操作では使用できなくする必要があります。</span><span class="sxs-lookup"><span data-stu-id="70e00-109">Expired - the key has run its natural lifetime and should no longer be used for new Protect operations.</span></span>

* <span data-ttu-id="70e00-110">失効のキーが侵害され、保護する操作では使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="70e00-110">Revoked - the key is compromised and must not be used for new Protect operations.</span></span>

<span data-ttu-id="70e00-111">作成された、アクティブ、および有効期限が切れたキーにすべて受信ペイロードを解除するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="70e00-111">Created, active, and expired keys may all be used to unprotect incoming payloads.</span></span> <span data-ttu-id="70e00-112">、ペイロードを解除するために、既定では失効のキーを使用しない場合がありますが、アプリケーション開発者[この動作をオーバーライド](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect)必要な場合です。</span><span class="sxs-lookup"><span data-stu-id="70e00-112">Revoked keys by default may not be used to unprotect payloads, but the application developer can [override this behavior](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) if necessary.</span></span>

>[!WARNING]
> <span data-ttu-id="70e00-113">開発者は、(例: ファイル システムから対応するファイルの削除) によってキー リングからキーを削除したくなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="70e00-113">The developer might be tempted to delete a key from the key ring (e.g., by deleting the corresponding file from the file system).</span></span> <span data-ttu-id="70e00-114">その時点で、キーによって保護されているすべてのデータが完全に解読され、緊急の上書きには、失効したキーではありません。</span><span class="sxs-lookup"><span data-stu-id="70e00-114">At that point, all data protected by the key is permanently undecipherable, and there is no emergency override like there is with revoked keys.</span></span> <span data-ttu-id="70e00-115">キーの削除は、完全に破壊的な動作とその結果、データ保護システム公開ありませんファースト クラスの API この操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="70e00-115">Deleting a key is truly destructive behavior, and consequently the data protection system exposes no first-class API for performing this operation.</span></span>

## <a name="default-key-selection"></a><span data-ttu-id="70e00-116">既定のキーの選択</span><span class="sxs-lookup"><span data-stu-id="70e00-116">Default key selection</span></span>

<span data-ttu-id="70e00-117">読み取りときに、データ保護システム キー リング バッキング リポジトリから、キー リングから"default"キーを検索を試みます。</span><span class="sxs-lookup"><span data-stu-id="70e00-117">When the data protection system reads the key ring from the backing repository, it will attempt to locate a "default" key from the key ring.</span></span> <span data-ttu-id="70e00-118">既定のキーは、新しい保護操作に使用されます。</span><span class="sxs-lookup"><span data-stu-id="70e00-118">The default key is used for new Protect operations.</span></span>

<span data-ttu-id="70e00-119">一般的なヒューリスティックは、データ保護システムが、キーの既定のキーとして、最新のライセンス認証日を選択します。</span><span class="sxs-lookup"><span data-stu-id="70e00-119">The general heuristic is that the data protection system chooses the key with the most recent activation date as the default key.</span></span> <span data-ttu-id="70e00-120">(が許可するようにサーバーのクロック スキューの小規模の一種です。)かどうか、アプリケーションが無効になっていない自動、およびキーの生成、キーの期限切れか失効されたかどうか、は、新しいキーが即時のライセンス認証ごとに生成されます、[キーの有効期限とローリング](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration)ポリシー以下です。</span><span class="sxs-lookup"><span data-stu-id="70e00-120">(There's a small fudge factor to allow for server-to-server clock skew.) If the key is expired or revoked, and if the application has not disabled automatic key generation, then a new key will be generated with immediate activation per the [key expiration and rolling](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) policy below.</span></span>

<span data-ttu-id="70e00-121">新しいキーの生成を新しいキーの前にライセンス認証されたすべてのキーの有効期限が暗黙の型として扱うことが別のキーにフォールバックするのではなく、データ保護システムの理由がすぐに新しいキーを生成します。</span><span class="sxs-lookup"><span data-stu-id="70e00-121">The reason the data protection system generates a new key immediately rather than falling back to a different key is that new key generation should be treated as an implicit expiration of all keys that were activated prior to the new key.</span></span> <span data-ttu-id="70e00-122">基本的な考え方されている新しいキーが別のアルゴリズムまたはより古いキー、暗号化の両方のメカニズムで構成されたシステムはフォールバック経由では、現在の構成を優先する必要があります。</span><span class="sxs-lookup"><span data-stu-id="70e00-122">The general idea is that new keys may have been configured with different algorithms or encryption-at-rest mechanisms than old keys, and the system should prefer the current configuration over falling back.</span></span>

<span data-ttu-id="70e00-123">例外が発生しました。</span><span class="sxs-lookup"><span data-stu-id="70e00-123">There is an exception.</span></span> <span data-ttu-id="70e00-124">場合は、アプリケーション開発者がある[自動キーの生成を無効になっている](xref:security/data-protection/configuration/overview#disableautomatickeygeneration)、データ保護システムで、既定のキーとしてものを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="70e00-124">If the application developer has [disabled automatic key generation](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), then the data protection system must choose something as the default key.</span></span> <span data-ttu-id="70e00-125">このフォールバック シナリオでは、システムは、クラスター内の他のマシンに反映されるまでの時間をかけてキーに指定された基本設定の最新のライセンス認証日、失効して非キーを選択します。</span><span class="sxs-lookup"><span data-stu-id="70e00-125">In this fallback scenario, the system will choose the non-revoked key with the most recent activation date, with preference given to keys that have had time to propagate to other machines in the cluster.</span></span> <span data-ttu-id="70e00-126">フォールバック システムは、その結果、既定の有効期限が切れたキーの選択をなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="70e00-126">The fallback system may end up choosing an expired default key as a result.</span></span> <span data-ttu-id="70e00-127">フォールバック システムは既定のキーと失効したキーを選択しないと、キー リングが空か、すべてのキーが失効していない場合は、システムが初期化時にエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="70e00-127">The fallback system will never choose a revoked key as the default key, and if the key ring is empty or every key has been revoked then the system will produce an error upon initialization.</span></span>

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a><span data-ttu-id="70e00-128">キーの期限切れとロール</span><span class="sxs-lookup"><span data-stu-id="70e00-128">Key expiration and rolling</span></span>

<span data-ttu-id="70e00-129">キーが作成されるが自動的に与えられます、アクティブ化された日付の {今すぐ + 2 日間} と {今すぐ + 90 日間} の有効期限。</span><span class="sxs-lookup"><span data-stu-id="70e00-129">When a key is created, it is automatically given an activation date of { now + 2 days } and an expiration date of { now + 90 days }.</span></span> <span data-ttu-id="70e00-130">アクティブ化する前に 2 日間の遅延は、システムを通じて伝達されるまでキー時刻を示します。</span><span class="sxs-lookup"><span data-stu-id="70e00-130">The 2-day delay before activation gives the key time to propagate through the system.</span></span> <span data-ttu-id="70e00-131">つまり、そのため、キーのリングが伝達になるはアクティブなときにする必要があるすべてのアプリケーション使用ことになる可能性を最大化、[次へ] の自動更新期間に、キーを観察するバッキング ストアを指して他のアプリケーションことができます。</span><span class="sxs-lookup"><span data-stu-id="70e00-131">That is, it allows other applications pointing at the backing store to observe the key at their next auto-refresh period, thus maximizing the chances that when the key ring does become active it has propagated to all applications that might need to use it.</span></span>

<span data-ttu-id="70e00-132">既定のキーが 2 日以内に失効して、キー リング既にキーを持たない、既定のキーの有効期限時にアクティブになる場合は、キー リングに新しいキーが自動的にデータ保護システムに保持します。</span><span class="sxs-lookup"><span data-stu-id="70e00-132">If the default key will expire within 2 days and if the key ring does not already have a key that will be active upon expiration of the default key, then the data protection system will automatically persist a new key to the key ring.</span></span> <span data-ttu-id="70e00-133">この新しいキーがあり、アクティブ化された日付の既定のキーの有効期限 {date} {今すぐ + 90 日間} の有効期限。</span><span class="sxs-lookup"><span data-stu-id="70e00-133">This new key has an activation date of { default key's expiration date } and an expiration date of { now + 90 days }.</span></span> <span data-ttu-id="70e00-134">これにより、システムがサービスの中断することなくが、定期的にキーを自動的にロールバックできます。</span><span class="sxs-lookup"><span data-stu-id="70e00-134">This allows the system to automatically roll keys on a regular basis with no interruption of service.</span></span>

<span data-ttu-id="70e00-135">ある可能性がありますの状況で即時のライセンス認証キーを作成する場所です。</span><span class="sxs-lookup"><span data-stu-id="70e00-135">There might be circumstances where a key will be created with immediate activation.</span></span> <span data-ttu-id="70e00-136">アプリケーションも、時間実行されていないし、キーのリングのすべてのキーの期限が切れている場合、1 つの例が挙げられます。</span><span class="sxs-lookup"><span data-stu-id="70e00-136">One example would be when the application hasn't run for a time and all keys in the key ring are expired.</span></span> <span data-ttu-id="70e00-137">この場合、キーは {now} の通常の 2 日間ライセンス認証遅延なしのアクティブ化日付を付与します。</span><span class="sxs-lookup"><span data-stu-id="70e00-137">When this happens, the key is given an activation date of { now } without the normal 2-day activation delay.</span></span>

<span data-ttu-id="70e00-138">これが次の例のように構成可能な既定キーの有効期間は 90 日間、意味がします。</span><span class="sxs-lookup"><span data-stu-id="70e00-138">The default key lifetime is 90 days, though this is configurable as in the following example.</span></span>

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

<span data-ttu-id="70e00-139">管理者を変更することも、システム全体で既定値もを明示的に呼び出す`SetDefaultKeyLifetime`システム全体のポリシーよりも優先されます。</span><span class="sxs-lookup"><span data-stu-id="70e00-139">An administrator can also change the default system-wide, though an explicit call to `SetDefaultKeyLifetime` will override any system-wide policy.</span></span> <span data-ttu-id="70e00-140">既定キーの有効期間は 7 日間よりも短くすることはできません。</span><span class="sxs-lookup"><span data-stu-id="70e00-140">The default key lifetime cannot be shorter than 7 days.</span></span>

## <a name="automatic-key-ring-refresh"></a><span data-ttu-id="70e00-141">自動キー リング更新</span><span class="sxs-lookup"><span data-stu-id="70e00-141">Automatic key ring refresh</span></span>

<span data-ttu-id="70e00-142">データ保護システム初期化すると、基になるリポジトリからのキー リングの読み取りし、メモリ内キャッシュします。</span><span class="sxs-lookup"><span data-stu-id="70e00-142">When the data protection system initializes, it reads the key ring from the underlying repository and caches it in memory.</span></span> <span data-ttu-id="70e00-143">このキャッシュは、保護と保護の解除の操作をバッキング ストアに達することなしに続行できます。</span><span class="sxs-lookup"><span data-stu-id="70e00-143">This cache allows Protect and Unprotect operations to proceed without hitting the backing store.</span></span> <span data-ttu-id="70e00-144">システムでは、約 24 時間ごと、またはどちらか早い方が、現在の既定のキーが経過すると、変更のバッキング ストアが自動的にチェックします。</span><span class="sxs-lookup"><span data-stu-id="70e00-144">The system will automatically check the backing store for changes approximately every 24 hours or when the current default key expires, whichever comes first.</span></span>

>[!WARNING]
> <span data-ttu-id="70e00-145">場合は、開発者が非常にまれには、キー管理 Api を直接使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="70e00-145">Developers should very rarely (if ever) need to use the key management APIs directly.</span></span> <span data-ttu-id="70e00-146">データ保護システムは上記のように、自動キー管理を実行します。</span><span class="sxs-lookup"><span data-stu-id="70e00-146">The data protection system will perform automatic key management as described above.</span></span>

<span data-ttu-id="70e00-147">インターフェイスを公開するデータ保護システム`IKeyManager`を使用して、検査し、キー リングに変更を加えることができます。</span><span class="sxs-lookup"><span data-stu-id="70e00-147">The data protection system exposes an interface `IKeyManager` that can be used to inspect and make changes to the key ring.</span></span> <span data-ttu-id="70e00-148">インスタンスを提供する DI システム`IDataProtectionProvider`のインスタンスを指定できますも`IKeyManager`の使用量に対するです。</span><span class="sxs-lookup"><span data-stu-id="70e00-148">The DI system that provided the instance of `IDataProtectionProvider` can also provide an instance of `IKeyManager` for your consumption.</span></span> <span data-ttu-id="70e00-149">プルする代わりに、`IKeyManager`から直接、`IServiceProvider`次の例のようにします。</span><span class="sxs-lookup"><span data-stu-id="70e00-149">Alternatively, you can pull the `IKeyManager` straight from the `IServiceProvider` as in the example below.</span></span>

<span data-ttu-id="70e00-150">キーのリング (明示的に新しいキーを作成または失効を実行する) を変更するすべての操作は、メモリ内キャッシュを無効になります。</span><span class="sxs-lookup"><span data-stu-id="70e00-150">Any operation which modifies the key ring (creating a new key explicitly or performing a revocation) will invalidate the in-memory cache.</span></span> <span data-ttu-id="70e00-151">次に呼び出した`Protect`または`Unprotect`キー リングを再度読み取るし、キャッシュを再作成するデータ保護システムが発生します。</span><span class="sxs-lookup"><span data-stu-id="70e00-151">The next call to `Protect` or `Unprotect` will cause the data protection system to reread the key ring and recreate the cache.</span></span>

<span data-ttu-id="70e00-152">次の例では、使用方法を示します、`IKeyManager`を検査および取り消しの既存のキーや、新しいキーを手動で生成するなど、キーのリングを操作するインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="70e00-152">The sample below demonstrates using the `IKeyManager` interface to inspect and manipulate the key ring, including revoking existing keys and generating a new key manually.</span></span>

[!code-csharp[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a><span data-ttu-id="70e00-153">キーの格納</span><span class="sxs-lookup"><span data-stu-id="70e00-153">Key storage</span></span>

<span data-ttu-id="70e00-154">データ保護システムには、適切なキー記憶域の場所と rest のメカニズムに暗号化を自動的に推測しようとする、ヒューリスティックがあります。</span><span class="sxs-lookup"><span data-stu-id="70e00-154">The data protection system has a heuristic whereby it tries to deduce an appropriate key storage location and encryption at rest mechanism automatically.</span></span> <span data-ttu-id="70e00-155">これは、アプリの開発者によって構成できます。</span><span class="sxs-lookup"><span data-stu-id="70e00-155">This is also configurable by the app developer.</span></span> <span data-ttu-id="70e00-156">次のドキュメントでは、これらのメカニズムのボックスでの実装について説明します。</span><span class="sxs-lookup"><span data-stu-id="70e00-156">The following documents discuss the in-box implementations of these mechanisms:</span></span>

* [<span data-ttu-id="70e00-157">ボックス内のキー記憶域プロバイダー</span><span class="sxs-lookup"><span data-stu-id="70e00-157">In-box key storage providers</span></span>](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [<span data-ttu-id="70e00-158">プロバイダーの rest のボックスでキーの暗号化</span><span class="sxs-lookup"><span data-stu-id="70e00-158">In-box key encryption at rest providers</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)