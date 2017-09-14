---
title: "構成を移行します。"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 8468d859-ff32-4a92-9e62-08c4a9e36594
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 62660f7e58467a69f540966df188747b6fde57fe
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-configuration"></a><span data-ttu-id="16b67-103">構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="16b67-103">Migrating Configuration</span></span>

<span data-ttu-id="16b67-104">によって[Steve Smith](https://ardalis.com/)と[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="16b67-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="16b67-105">開始前の記事で[ASP.NET Core MVC に ASP.NET MVC プロジェクトを移行する](mvc.md)です。</span><span class="sxs-lookup"><span data-stu-id="16b67-105">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="16b67-106">この記事では、構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="16b67-106">In this article, we migrate configuration.</span></span>

[<span data-ttu-id="16b67-107">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="16b67-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)

## <a name="setup-configuration"></a><span data-ttu-id="16b67-108">セットアップの構成</span><span class="sxs-lookup"><span data-stu-id="16b67-108">Setup Configuration</span></span>

<span data-ttu-id="16b67-109">ASP.NET Core を使用しない、 *Global.asax*と*web.config* ASP.NET の以前のバージョンを使用するファイル。</span><span class="sxs-lookup"><span data-stu-id="16b67-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="16b67-110">ASP.NET の以前のバージョンではアプリケーションのスタートアップ ロジックに格納された、`Application_StartUp`メソッド内で*Global.asax*です。</span><span class="sxs-lookup"><span data-stu-id="16b67-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="16b67-111">その後、ASP.NET MVC では、 *Startup.cs*ファイルがプロジェクトのルートに含まれているし、アプリケーションの起動時に呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="16b67-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="16b67-112">ASP.NET Core を採用していますこのアプローチ完全にすべてのスタートアップ ロジックを配置することによって、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="16b67-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="16b67-113">*Web.config* ASP.NET Core でのファイルが置き換えられてもいます。</span><span class="sxs-lookup"><span data-stu-id="16b67-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="16b67-114">説明されているアプリケーションのスタートアップ プロシージャの一部として、構成自体を構成ようになりましたことができます*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="16b67-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="16b67-115">構成は、XML ファイルにも引き続き使用できますが、通常 ASP.NET Core プロジェクトが配置構成値 JSON 形式のファイルになど*される appsettings.json*です。</span><span class="sxs-lookup"><span data-stu-id="16b67-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="16b67-116">ASP.NET Core の構成システムは、環境変数は、環境固有の値より安全かつ堅牢な場所を指定することができますをも簡単にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="16b67-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="16b67-117">これは、接続文字列およびソース管理にチェックインする必要がありますいない API キーなどの機密情報に特に当てはまります。</span><span class="sxs-lookup"><span data-stu-id="16b67-117">This is especially true for secrets like connection strings and API keys that should not be checked into source control.</span></span> <span data-ttu-id="16b67-118">参照してください[構成](../fundamentals/configuration.md)ASP.NET Core の構成に関する詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="16b67-118">See [Configuration](../fundamentals/configuration.md) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="16b67-119">この記事では、私たちは、開始から ASP.NET Core プロジェクトを部分的に移行[前の記事](mvc.md)です。</span><span class="sxs-lookup"><span data-stu-id="16b67-119">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="16b67-120">構成をセットアップで、次のコンス トラクターとプロパティを追加する、 *Startup.cs*プロジェクトのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="16b67-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

<span data-ttu-id="16b67-121">[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]</span><span class="sxs-lookup"><span data-stu-id="16b67-121">[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]</span></span>

<span data-ttu-id="16b67-122">この時点で、メモ、 *Startup.cs*ファイルはコンパイルされず、次を追加する必要があります、`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="16b67-122">Note that at this point, the *Startup.cs* file will not compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="16b67-123">追加、*される appsettings.json*適切な項目テンプレートを使用して、プロジェクトのルートにファイル。</span><span class="sxs-lookup"><span data-stu-id="16b67-123">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![AppSettings の JSON を追加します。](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="16b67-125">Web.config ファイルから構成設定を移行します。</span><span class="sxs-lookup"><span data-stu-id="16b67-125">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="16b67-126">ASP.NET MVC プロジェクトに必要なデータベース接続文字列が含まれている*web.config*で、`<connectionStrings>`要素。</span><span class="sxs-lookup"><span data-stu-id="16b67-126">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="16b67-127">プロジェクトでは、ASP.NET Core、この情報を格納するつもりが、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="16b67-127">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="16b67-128">開いている*される appsettings.json*、および次既に含まれていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="16b67-128">Open *appsettings.json*, and note that it already includes the following:</span></span>

<span data-ttu-id="16b67-129">[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="16b67-129">[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]</span></span>


<span data-ttu-id="16b67-130">強調表示された行に上に示されているからデータベースの名前を変更する**_CHANGE_ME**データベースの名前にします。</span><span class="sxs-lookup"><span data-stu-id="16b67-130">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="16b67-131">概要</span><span class="sxs-lookup"><span data-stu-id="16b67-131">Summary</span></span>

<span data-ttu-id="16b67-132">ASP.NET Core は、単一のファイルを順番に必要なサービスと依存関係の定義し、構成に、アプリケーションのすべてのスタートアップ ロジックを配置します。</span><span class="sxs-lookup"><span data-stu-id="16b67-132">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="16b67-133">置き換えられます、 *web.config*ファイルのさまざまな環境変数と同様に、JSON などのファイル形式を利用できる柔軟な構成機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="16b67-133">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>