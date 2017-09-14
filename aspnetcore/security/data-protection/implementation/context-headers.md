---
title: "コンテキスト ヘッダー"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 7befd983f6a45839868639708ec5cf45bf2df35f
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="context-headers"></a><span data-ttu-id="ea557-103">コンテキスト ヘッダー</span><span class="sxs-lookup"><span data-stu-id="ea557-103">Context headers</span></span>

<a name=data-protection-implementation-context-headers></a>

## <a name="background-and-theory"></a><span data-ttu-id="ea557-104">背景と理論的には</span><span class="sxs-lookup"><span data-stu-id="ea557-104">Background and theory</span></span>

<span data-ttu-id="ea557-105">データ保護システムでは、「キー」は、認証済み暗号化サービスを提供できるオブジェクトを意味します。</span><span class="sxs-lookup"><span data-stu-id="ea557-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="ea557-106">各キーは一意の id (GUID) によって識別され、それが伴うことアルゴリズム情報および entropic マテリアルです。</span><span class="sxs-lookup"><span data-stu-id="ea557-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="ea557-107">これは、こと各キーが一意のエントロピを実行がシステムを実施できないことも開発者キー リング内の既存のキーのアルゴリズム情報を変更することにより、キーのリングを手動で変更可能性がありますを考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ea557-107">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="ea557-108">このような場合のセキュリティ要件を実現するために、データ保護システムがの概念[暗号化方式の指定](https://www.microsoft.com/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D121045)、これにより、複数の暗号化アルゴリズムの間で安全に entropic 1 つの値を使用しています。</span><span class="sxs-lookup"><span data-stu-id="ea557-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/?from=http%3A%2F%2Fresearch.microsoft.com%2Fapps%2Fpubs%2Fdefault.aspx%3Fid%3D121045), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="ea557-109">暗号化方式の指定をサポートするほとんどのシステムでは、ペイロードの内側のアルゴリズムについていくつかの識別情報を含めることによってようにします。</span><span class="sxs-lookup"><span data-stu-id="ea557-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="ea557-110">アルゴリズムの OID は、これに適した候補では、通常です。</span><span class="sxs-lookup"><span data-stu-id="ea557-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="ea557-111">ただし、発生しましました問題の 1 つは、同じアルゴリズムを指定する複数の方法があること"AES"(CNG) と、マネージ Aes、AesManaged、AesCryptoServiceProvider、AesCng、および (指定した特定のパラメーター) RijndaelManaged クラスは、実際にすべて同じです。操作を実行し正しい OID をこれらすべてのマッピングを維持するために必要があります。</span><span class="sxs-lookup"><span data-stu-id="ea557-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="ea557-112">開発者は、カスタム アルゴリズム (または AES の別の実装) を提供する場合は、その OID を連絡する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ea557-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="ea557-113">この余分な登録ステップでは、システム構成が特に苦痛を伴う作業します。</span><span class="sxs-lookup"><span data-stu-id="ea557-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="ea557-114">戻すステップ実行、おされたに近づいていること、問題、誤った方向から用意しました。</span><span class="sxs-lookup"><span data-stu-id="ea557-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="ea557-115">OID は、アルゴリズムを示しますが、実際にこの考慮しないことです。</span><span class="sxs-lookup"><span data-stu-id="ea557-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="ea557-116">2 つのアルゴリズムで安全に 1 つの entropic 値を使用する必要がある場合、アルゴリズムが実際には何を理解するうえで必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ea557-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="ea557-117">どのような実際に配慮して、どのように動作するです。</span><span class="sxs-lookup"><span data-stu-id="ea557-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="ea557-118">任意の適切な対称ブロック暗号アルゴリズムも強力な擬似順列 (PRP): (キー、モード、IV、プレーン テキストの組み合わせ) の入力を修正し、暗号化テキストの出力は確率の過負荷になります他の対称ブロック暗号と異なるアルゴリズムは、同じ入力します。</span><span class="sxs-lookup"><span data-stu-id="ea557-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="ea557-119">同様に、任意の適切なキー付きハッシュ関数も強力な擬似関数 (PRF) は、特定の固定入力セットの出力は圧倒的になりますの他のキー付きハッシュ関数とは異なります。</span><span class="sxs-lookup"><span data-stu-id="ea557-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="ea557-120">コンテキスト ヘッダーを構築するこの強力な PRPs と PRFs の概念を使用します。</span><span class="sxs-lookup"><span data-stu-id="ea557-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="ea557-121">このコンテキスト ヘッダーが本質的には、指定された操作に使用するアルゴリズムを安定した拇印として機能し、データ保護システムに必要な暗号化方式の指定を提供します。</span><span class="sxs-lookup"><span data-stu-id="ea557-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="ea557-122">このヘッダーは、再現可能なとの一部として後で使用されて、[サブキーの派生プロセス](subkeyderivation.md#data-protection-implementation-subkey-derivation)です。</span><span class="sxs-lookup"><span data-stu-id="ea557-122">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="ea557-123">基になるアルゴリズムの動作モードに応じて、コンテキスト ヘッダーを構築する 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="ea557-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="ea557-124">CBC モードの暗号化 + HMAC 認証</span><span class="sxs-lookup"><span data-stu-id="ea557-124">CBC-mode encryption + HMAC authentication</span></span>

<a name=data-protection-implementation-context-headers-cbc-components></a>

<span data-ttu-id="ea557-125">コンテキスト ヘッダーは、次のコンポーネントで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ea557-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="ea557-126">[16 ビット]値 00 00 で、マーカーは、「CBC 暗号化 + HMAC 認証」を意味します。</span><span class="sxs-lookup"><span data-stu-id="ea557-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="ea557-127">[32 ビット]対称ブロック暗号アルゴリズムのキーの長さ (単位はバイト、ビッグ エンディアン)。</span><span class="sxs-lookup"><span data-stu-id="ea557-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="ea557-128">[32 ビット]ブロックのサイズ (バイト、ビッグ エンディアン) 対称ブロック暗号アルゴリズム。</span><span class="sxs-lookup"><span data-stu-id="ea557-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="ea557-129">[32 ビット]HMAC アルゴリズムのキーの長さ (単位はバイト、ビッグ エンディアン)。</span><span class="sxs-lookup"><span data-stu-id="ea557-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="ea557-130">(現在のキー サイズ常にサイズに一致ダイジェスト。)</span><span class="sxs-lookup"><span data-stu-id="ea557-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="ea557-131">[32 ビット]ダイジェストのサイズ (バイト、ビッグ エンディアン) HMAC アルゴリズムです。</span><span class="sxs-lookup"><span data-stu-id="ea557-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="ea557-132">EncCBC (K_E、IV、"")、これは、空の文字列入力を使用する対称ブロック暗号アルゴリズムの出力および IV がすべてゼロ ベクターがします。</span><span class="sxs-lookup"><span data-stu-id="ea557-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="ea557-133">K_E の構築は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ea557-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="ea557-134">MAC (K_H、"")、これは、空の文字列入力を使用する HMAC アルゴリズムの出力。</span><span class="sxs-lookup"><span data-stu-id="ea557-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="ea557-135">K_H の構築は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ea557-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="ea557-136">理想的にはすべてゼロのベクトル K_E および K_H 渡します可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ea557-136">Ideally we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="ea557-137">ただし、基になるアルゴリズムがすべてゼロ ベクターのような単純型または反復可能なパターンを使用してを除外する (特に DES、3 des) の操作を実行する前に弱いキーの有無を確認するような状況を回避します。</span><span class="sxs-lookup"><span data-stu-id="ea557-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="ea557-138">代わりに、NIST SP800 108 KDF カウンター モードで使用 (を参照してください[NIST SP800 108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf)、秒 5.1) 長さ 0 キー、ラベル、およびコンテキストおよび基になる PRF として HMACSHA512 を使用します。</span><span class="sxs-lookup"><span data-stu-id="ea557-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="ea557-139">派生お |K_E |+ |K_H |出力のバイトし、結果に分解 K_E と K_H 自体です。</span><span class="sxs-lookup"><span data-stu-id="ea557-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="ea557-140">数学的に、これはように表されます。</span><span class="sxs-lookup"><span data-stu-id="ea557-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="ea557-141">(K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512、key =""、ラベル =""、コンテキスト ="")</span><span class="sxs-lookup"><span data-stu-id="ea557-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="ea557-142">例: AES、192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="ea557-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="ea557-143">例として、対称ブロック暗号アルゴリズムは、AES 192-CBC し検証アルゴリズムが HMACSHA256 場合を検討してください。</span><span class="sxs-lookup"><span data-stu-id="ea557-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="ea557-144">システムには、次の手順を使用して、コンテキスト ヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ea557-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="ea557-145">まず、(K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512、キー =""、ラベル =""、コンテキスト ="")、|K_E |= 192 ビットおよび |K_H |指定したアルゴリズムごとの 256 ビットを = です。</span><span class="sxs-lookup"><span data-stu-id="ea557-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="ea557-146">これは、ため、K_E に = 5BB6.21DD と K_H = A04A.次の例で 00A9:</span><span class="sxs-lookup"><span data-stu-id="ea557-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
   61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
   49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
   B7 92 3D BF 59 90 00 A9
   ```

<span data-ttu-id="ea557-147">次に、コンピューティング Enc_CBC (K_E、IV、"") CBC 192 AES IV を指定された = 0 * と上と K_E です。</span><span class="sxs-lookup"><span data-stu-id="ea557-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="ea557-148">結果: F474B1872B3B53E4721DE19C0841DB6F を =</span><span class="sxs-lookup"><span data-stu-id="ea557-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="ea557-149">次に、MAC の計算 (K_H、"") HMACSHA256 K_H 上記として指定されているためです。</span><span class="sxs-lookup"><span data-stu-id="ea557-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="ea557-150">結果: D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C を =</span><span class="sxs-lookup"><span data-stu-id="ea557-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="ea557-151">これには、以下の内容を含むヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ea557-151">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
   00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
   DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
   8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
   22 0C
   ```

<span data-ttu-id="ea557-152">このコンテキスト ヘッダーは、認証済み暗号化アルゴリズムのペア (CBC 192、AES で暗号化 + HMACSHA256 検証) のサムプリントです。</span><span class="sxs-lookup"><span data-stu-id="ea557-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="ea557-153">コンポーネントの説明に従って[上](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components)は。</span><span class="sxs-lookup"><span data-stu-id="ea557-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="ea557-154">マーカー (00 00)</span><span class="sxs-lookup"><span data-stu-id="ea557-154">the marker (00 00)</span></span>

* <span data-ttu-id="ea557-155">ブロック暗号キーの長さ (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="ea557-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="ea557-156">ブロック暗号ブロック サイズ (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="ea557-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="ea557-157">HMAC キーの長さ (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="ea557-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="ea557-158">HMAC ダイジェストのサイズ (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="ea557-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="ea557-159">ブロック暗号 PRP 出力 (F4 74 - DB 6 f) と</span><span class="sxs-lookup"><span data-stu-id="ea557-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="ea557-160">HMAC PRF 出力 (D4 79 - 終了)。</span><span class="sxs-lookup"><span data-stu-id="ea557-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="ea557-161">CBC モードの暗号化 + HMAC 認証コンテキスト ヘッダーは、Windows CNG またはマネージ対称アルゴリズムおよび KeyedHashAlgorithm 型にアルゴリズムの実装を提供するかどうかに関係なく同じ方法を構築します。</span><span class="sxs-lookup"><span data-stu-id="ea557-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="ea557-162">これにより、アルゴリズムの実装は Os によって異なる場合でも、同じコンテキスト ヘッダーを確実に生成するためにさまざまなオペレーティング システムで実行されているアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="ea557-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="ea557-163">(実際には、KeyedHashAlgorithm しないが適切な HMAC です。</span><span class="sxs-lookup"><span data-stu-id="ea557-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="ea557-164">だということ、キー付きハッシュ アルゴリズムの種類です。)</span><span class="sxs-lookup"><span data-stu-id="ea557-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="ea557-165">例: 3 des 192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="ea557-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="ea557-166">まず、(K_E | |K_H) = SP800_108_CTR (prf = HMACSHA512、キー =""、ラベル =""、コンテキスト ="")、|K_E |= 192 ビットおよび |K_H |指定したアルゴリズムごとの 160 ビットを = です。</span><span class="sxs-lookup"><span data-stu-id="ea557-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="ea557-167">これは、ため、K_E に = A219.E2BB と K_H = DC4A.次の例で B464:</span><span class="sxs-lookup"><span data-stu-id="ea557-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
   61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
   D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
   ```

<span data-ttu-id="ea557-168">次に、コンピューティング Enc_CBC (K_E、IV、"") CBC 192 3 des IV を指定された = 0 * と上と K_E です。</span><span class="sxs-lookup"><span data-stu-id="ea557-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="ea557-169">結果: ABB100F81E53E10E を =</span><span class="sxs-lookup"><span data-stu-id="ea557-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="ea557-170">次に、MAC の計算 (K_H、"") HMACSHA1 K_H 上記として指定されているためです。</span><span class="sxs-lookup"><span data-stu-id="ea557-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="ea557-171">結果: 76EB189B35CF03461DDF877CD9F4B1B4D63A7555 を =</span><span class="sxs-lookup"><span data-stu-id="ea557-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="ea557-172">これにより、認証の拇印である完全コンテキスト ヘッダーが生成されます。 暗号化アルゴリズムの組み合わせ (3 des 192-CBC 暗号化 + HMACSHA1 検証)、次に示します。</span><span class="sxs-lookup"><span data-stu-id="ea557-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
   00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
   03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
   ```

<span data-ttu-id="ea557-173">コンポーネントが次のように分割します。</span><span class="sxs-lookup"><span data-stu-id="ea557-173">The components break down as follows:</span></span>

* <span data-ttu-id="ea557-174">マーカー (00 00)</span><span class="sxs-lookup"><span data-stu-id="ea557-174">the marker (00 00)</span></span>

* <span data-ttu-id="ea557-175">ブロック暗号キーの長さ (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="ea557-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="ea557-176">ブロック暗号ブロック サイズ (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="ea557-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="ea557-177">HMAC キーの長さ (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="ea557-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="ea557-178">HMAC ダイジェストのサイズ (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="ea557-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="ea557-179">PRP 出力 (AB B1 - E1 0 e) ブロック暗号と</span><span class="sxs-lookup"><span data-stu-id="ea557-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="ea557-180">HMAC PRF 出力 (76 EB - 終了)。</span><span class="sxs-lookup"><span data-stu-id="ea557-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="ea557-181">Galois/カウンター モードの暗号化と認証</span><span class="sxs-lookup"><span data-stu-id="ea557-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="ea557-182">コンテキスト ヘッダーは、次のコンポーネントで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ea557-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="ea557-183">[16 ビット]値 00 はマーカー 01「GCM 暗号化 + 認証」を意味します。</span><span class="sxs-lookup"><span data-stu-id="ea557-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="ea557-184">[32 ビット]対称ブロック暗号アルゴリズムのキーの長さ (単位はバイト、ビッグ エンディアン)。</span><span class="sxs-lookup"><span data-stu-id="ea557-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="ea557-185">[32 ビット]Nonce サイズ (バイト単位、ビッグ エンディアン) 中に使用するには、暗号化操作が認証されます。</span><span class="sxs-lookup"><span data-stu-id="ea557-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="ea557-186">(当社のシステムでの nonce のサイズに固定これは 96 ビットを = です)。</span><span class="sxs-lookup"><span data-stu-id="ea557-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="ea557-187">[32 ビット]ブロックのサイズ (バイト、ビッグ エンディアン) 対称ブロック暗号アルゴリズム。</span><span class="sxs-lookup"><span data-stu-id="ea557-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="ea557-188">(GCM、これが固定ブロック サイズでは 128 ビットを = です)。</span><span class="sxs-lookup"><span data-stu-id="ea557-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="ea557-189">[32 ビット]認証タグ サイズ (バイト単位、ビッグ エンディアン)、認証済み暗号化関数によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="ea557-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="ea557-190">(当社のシステムでのタグのサイズに固定これは、128 ビットを = です)。</span><span class="sxs-lookup"><span data-stu-id="ea557-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="ea557-191">[128 ビット]Enc_GCM のタグ (K_E、nonce、"")、これは、空の文字列入力を使用する対称ブロック暗号アルゴリズムの出力および nonce が 96 ビットすべてゼロのベクトルがします。</span><span class="sxs-lookup"><span data-stu-id="ea557-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="ea557-192">CBC 暗号化 + HMAC 認証のシナリオと同様に同じメカニズムを使用して K_E が派生します。</span><span class="sxs-lookup"><span data-stu-id="ea557-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="ea557-193">ただし、このアプローチで K_H がしないため、本質的にある |K_H |= 0 の場合、アルゴリズムを解除し、フォームの下。</span><span class="sxs-lookup"><span data-stu-id="ea557-193">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="ea557-194">K_E = SP800_108_CTR (prf = HMACSHA512、key =""、ラベル =""、コンテキスト ="")</span><span class="sxs-lookup"><span data-stu-id="ea557-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="ea557-195">例: AES 256 GCM</span><span class="sxs-lookup"><span data-stu-id="ea557-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="ea557-196">まず、K_E = SP800_108_CTR (prf = HMACSHA512、キー =""、ラベル =""、コンテキスト ="") ここで、|K_E |= 256 ビットです。</span><span class="sxs-lookup"><span data-stu-id="ea557-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="ea557-197">K_E: 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8 を =</span><span class="sxs-lookup"><span data-stu-id="ea557-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="ea557-198">Enc_GCM の認証タグを次に、コンピューティング (K_E、nonce、"") GCM 256 AES nonce を指定された = 096 と K_E 上記と同じです。</span><span class="sxs-lookup"><span data-stu-id="ea557-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="ea557-199">結果: E7DCCE66DF855A323A6BB7BD7A59BE45 を =</span><span class="sxs-lookup"><span data-stu-id="ea557-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="ea557-200">これには、以下の内容を含むヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ea557-200">This produces the full context header below:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
   00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
   BE 45
   ```

<span data-ttu-id="ea557-201">コンポーネントが次のように分割します。</span><span class="sxs-lookup"><span data-stu-id="ea557-201">The components break down as follows:</span></span>

   * <span data-ttu-id="ea557-202">マーカー (00 01)</span><span class="sxs-lookup"><span data-stu-id="ea557-202">the marker (00 01)</span></span>

   * <span data-ttu-id="ea557-203">ブロック暗号キーの長さ (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="ea557-203">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="ea557-204">nonce のサイズ (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="ea557-204">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="ea557-205">ブロック暗号ブロック サイズ (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="ea557-205">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="ea557-206">認証タグのサイズ (00 00 00 10) と</span><span class="sxs-lookup"><span data-stu-id="ea557-206">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="ea557-207">実行中のブロック暗号から認証タグ (E7 DC - 終了)。</span><span class="sxs-lookup"><span data-stu-id="ea557-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>