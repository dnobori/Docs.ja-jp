---
title: "グローバリゼーションとローカリゼーション ASP.NET Core"
author: rick-anderson
description: "コンテンツをさまざまな言語およびカルチャにローカライズするため、ASP.NET Core がミドルウェアとサービスを提供する方法について説明します。"
keywords: "ASP.NET Core、ローカリゼーション、カルチャ、言語、リソース ファイル、グローバリゼーション、国際化、ロケール"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: 2a760343566d2c2be591983e20830b5207a2199b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="globalization-and-localization-in-aspnet-core"></a><span data-ttu-id="5cd85-104">グローバリゼーションとローカリゼーション ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5cd85-104">Globalization and localization in ASP.NET Core</span></span>

<span data-ttu-id="5cd85-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Damien Bowden](https://twitter.com/damien_bod)、 [Bart Calixto](https://twitter.com/bartmax)、 [Nadeem Afana](https://twitter.com/NadeemAfana)、および[Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="5cd85-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="5cd85-106">ASP.NET Core 多言語 web サイトを作成すると、多数の対象者に到達するようにサイトが許可されます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-106">Creating a multilingual website with ASP.NET Core will allow your site to reach a wider audience.</span></span> <span data-ttu-id="5cd85-107">ASP.NET Core は、さまざまな言語およびカルチャにローカライズするためのサービスとミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-107">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="5cd85-108">国際化では、[グローバリゼーション](https://docs.microsoft.com/dotnet/api/system.globalization)と[ローカリゼーション](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization)です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-108">Internationalization involves [Globalization](https://docs.microsoft.com/dotnet/api/system.globalization) and [Localization](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization).</span></span> <span data-ttu-id="5cd85-109">グローバリゼーションとは異なるカルチャをサポートするアプリを設計するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-109">Globalization is the process of designing apps that support different cultures.</span></span> <span data-ttu-id="5cd85-110">グローバリゼーションでは、入力、表示、および定義済みの一連の特定の地理的領域に関連する言語のスクリプトの出力のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-110">Globalization adds support for input, display, and output of a defined set of language scripts that relate to specific geographic areas.</span></span>

<span data-ttu-id="5cd85-111">ローカリゼーションとは、グローバライズされたアプリ、特定のカルチャとロケールに、ローカライズの可能性が既に処理に対応させるプロセス。</span><span class="sxs-lookup"><span data-stu-id="5cd85-111">Localization is the process of adapting a globalized app, which you have already processed for localizability, to a particular culture/locale.</span></span>  <span data-ttu-id="5cd85-112">詳細については、次を参照してください。**グローバリゼーションおよびローカリゼーションの条項**このドキュメントの末尾近くです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-112">For more information see **Globalization and localization terms** near the end of this document.</span></span>

<span data-ttu-id="5cd85-113">アプリのローカライズには、次の項目が含まれます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-113">App localization involves the following:</span></span>

1. <span data-ttu-id="5cd85-114">アプリのコンテンツをローカライズできるようにします</span><span class="sxs-lookup"><span data-stu-id="5cd85-114">Make the app's content localizable</span></span>

2. <span data-ttu-id="5cd85-115">言語およびサポートするカルチャのローカライズされたリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-115">Provide localized resources for the languages and cultures you support</span></span>

3. <span data-ttu-id="5cd85-116">要求ごとに言語/カルチャを選択するための戦略を実装します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-116">Implement a strategy to select the language/culture for each request</span></span>

## <a name="make-the-apps-content-localizable"></a><span data-ttu-id="5cd85-117">アプリのコンテンツをローカライズできるようにします</span><span class="sxs-lookup"><span data-stu-id="5cd85-117">Make the app's content localizable</span></span>

<span data-ttu-id="5cd85-118">ASP.NET Core で導入された`IStringLocalizer`と`IStringLocalizer<T>`ローカライズされたアプリの開発時に、生産性を向上させるために設計されています。</span><span class="sxs-lookup"><span data-stu-id="5cd85-118">Introduced in ASP.NET Core, `IStringLocalizer` and `IStringLocalizer<T>` were architected to improve productivity when developing localized apps.</span></span> <span data-ttu-id="5cd85-119">`IStringLocalizer`使用して、 [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)と[ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader)を実行時にカルチャ固有のリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-119">`IStringLocalizer` uses the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) and [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) to provide culture-specific resources at run time.</span></span> <span data-ttu-id="5cd85-120">シンプルなインターフェイスが、インデクサー、および`IEnumerable`ローカライズされた文字列を返すためです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-120">The simple interface has an indexer and an `IEnumerable` for returning localized strings.</span></span> <span data-ttu-id="5cd85-121">`IStringLocalizer`リソース ファイルに既定の言語識別文字列を格納するには不要です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-121">`IStringLocalizer` doesn't require you to store the default language strings in a resource file.</span></span> <span data-ttu-id="5cd85-122">開発の早い段階でリソース ファイルを作成する必要はなく、ローカリゼーションの対象となるアプリを開発できます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-122">You can develop an app targeted for localization and not need to create resource files early in development.</span></span> <span data-ttu-id="5cd85-123">次のコードでは、ローカライズ文字列「タイトルは」をラップする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-123">The code below shows how to wrap the string "About Title" for localization.</span></span>

<span data-ttu-id="5cd85-124">[!code-csharp[Main](localization/sample/Controllers/AboutController.cs)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-124">[!code-csharp[Main](localization/sample/Controllers/AboutController.cs)]</span></span>

<span data-ttu-id="5cd85-125">上記のコードで、`IStringLocalizer<T>`実装に由来[依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-125">In the code above, the `IStringLocalizer<T>` implementation comes from [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="5cd85-126">「タイトルは」のローカライズされた値が見つからないかどうかは、インデクサーのキーが返されます、つまり、文字列「タイトルは」です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-126">If the localized value of "About Title" is not found, then the indexer key is returned, that is, the string "About Title".</span></span> <span data-ttu-id="5cd85-127">アプリで、既定の言語のリテラル文字列をそのままし、ローカライザーにラップされるよう、アプリの開発に専念することができます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-127">You can leave the default language literal strings in the app and wrap them in the localizer, so that you can focus on developing the app.</span></span> <span data-ttu-id="5cd85-128">既定の言語でアプリを開発し、既定のリソース ファイルを作成しなくても、ローカリゼーション手順用に準備します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-128">You develop your app with your default language and prepare it for the localization step without first creating a default resource file.</span></span> <span data-ttu-id="5cd85-129">または、従来のアプローチを使用し、既定の言語文字列を取得するキーを指定できます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-129">Alternatively, you can use the traditional approach and provide a key to retrieve the default language string.</span></span> <span data-ttu-id="5cd85-130">多くの開発者にとって新しいワークフローの既定の言語がない*.resx*ファイルと、文字列リテラルをラップすることだけがアプリをローカライズするオーバーヘッドを削減できます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-130">For many developers the new workflow of not having a default language *.resx* file and simply wrapping the string literals can reduce the overhead of localizing an app.</span></span> <span data-ttu-id="5cd85-131">他の開発者には、そのやすく長い文字列リテラルを操作およびローカライズされた文字列を更新しやすくように従来の作業の流れを選びます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-131">Other developers will prefer the traditional work flow as it can make it easier to work with longer string literals and make it easier to update localized strings.</span></span>

<span data-ttu-id="5cd85-132">使用して、 `IHtmlLocalizer<T>` HTML を格納しているリソースの実装です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-132">Use the `IHtmlLocalizer<T>` implementation for resources that contain HTML.</span></span> <span data-ttu-id="5cd85-133">`IHtmlLocalizer`HTML では、リソース文字列はリソース文字列ではなくでフォーマットされている引数をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="5cd85-133">`IHtmlLocalizer` HTML encodes arguments that are formatted in the resource string, but not the resource string.</span></span> <span data-ttu-id="5cd85-134">サンプルでは、次の値のみ強調表示されます`name`パラメーターは、HTML エンコードします。</span><span class="sxs-lookup"><span data-stu-id="5cd85-134">In the sample highlighted below, only the value of `name` parameter is HTML encoded.</span></span>

<span data-ttu-id="5cd85-135">[!code-csharp[Main](../fundamentals/localization/sample/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-135">[!code-csharp[Main](../fundamentals/localization/sample/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]</span></span>

<span data-ttu-id="5cd85-136">注: 一般的にだけをローカライズするテキストと HTML ではありません。</span><span class="sxs-lookup"><span data-stu-id="5cd85-136">Note: You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="5cd85-137">最下位のレベルを取得できます`IStringLocalizerFactory`不在[依存性の注入](dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="5cd85-137">At the lowest level, you can get `IStringLocalizerFactory` out of [Dependency Injection](dependency-injection.md):</span></span>

<span data-ttu-id="5cd85-138">[!code-csharp[Main](localization/sample/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-138">[!code-csharp[Main](localization/sample/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]</span></span>

<span data-ttu-id="5cd85-139">上記のコードを示します、2 つのファクトリの各メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-139">The code above demonstrates each of the two factory create methods.</span></span>

<span data-ttu-id="5cd85-140">領域で、コント ローラーによって、ローカライズされた文字列をパーティション分割または 1 つのコンテナーを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-140">You can partition your localized strings by controller, area, or have just one container.</span></span> <span data-ttu-id="5cd85-141">サンプル アプリで、ダミーという名前のクラス`SharedResource`共有リソースのために使用します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-141">In the sample app, a dummy class named `SharedResource` is used for shared resources.</span></span>

<span data-ttu-id="5cd85-142">[!code-csharp[Main](localization/sample/Resources/SharedResource.cs)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-142">[!code-csharp[Main](localization/sample/Resources/SharedResource.cs)]</span></span>

<span data-ttu-id="5cd85-143">一部の開発者が使用して、`Startup`グローバルまたは共有の文字列を格納するクラス。</span><span class="sxs-lookup"><span data-stu-id="5cd85-143">Some developers use the `Startup` class to contain global or shared strings.</span></span>  <span data-ttu-id="5cd85-144">以下のサンプルで、`InfoController`と`SharedResource`ローカライザーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-144">In the sample below, the `InfoController` and the `SharedResource` localizers are used:</span></span>

<span data-ttu-id="5cd85-145">[!code-csharp[Main](localization/sample/Controllers/InfoController.cs?range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-145">[!code-csharp[Main](localization/sample/Controllers/InfoController.cs?range=9-26)]</span></span>

## <a name="view-localization"></a><span data-ttu-id="5cd85-146">ビューのローカライズ</span><span class="sxs-lookup"><span data-stu-id="5cd85-146">View localization</span></span>

<span data-ttu-id="5cd85-147">`IViewLocalizer`サービス提供のローカライズされた文字列、[ビュー](https://docs.microsoft.com/aspnet/core)です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-147">The `IViewLocalizer` service provides localized strings for a [view](https://docs.microsoft.com/aspnet/core).</span></span> <span data-ttu-id="5cd85-148">`ViewLocalizer`クラスは、このインターフェイスを実装し、ビューのファイル パスからリソースの場所を検索します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-148">The `ViewLocalizer` class implements this interface and finds the resource location from the view file path.</span></span> <span data-ttu-id="5cd85-149">次のコードは、の既定の実装を使用する方法を示しています`IViewLocalizer`:。</span><span class="sxs-lookup"><span data-stu-id="5cd85-149">The following code shows how to use the default implementation of `IViewLocalizer`:</span></span>

<span data-ttu-id="5cd85-150">[!code-HTML[Main](localization/sample/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-150">[!code-HTML[Main](localization/sample/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="5cd85-151">既定の実装`IViewLocalizer`ビューのファイルの名前に基づいてリソース ファイルを検索します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-151">The default implementation of `IViewLocalizer` finds the resource file based on the view's file name.</span></span> <span data-ttu-id="5cd85-152">共有のグローバル リソース ファイルを使用するオプションはありません。</span><span class="sxs-lookup"><span data-stu-id="5cd85-152">There is no option to use a global shared resource file.</span></span> <span data-ttu-id="5cd85-153">`ViewLocalizer`実装を使用してローカライザー `IHtmlLocalizer`Razor しない HTML のため、ローカライズされた文字列をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="5cd85-153">`ViewLocalizer` implements the localizer using `IHtmlLocalizer`, so Razor doesn't HTML encode the localized string.</span></span> <span data-ttu-id="5cd85-154">リソース文字列をパラメーター化できると`IViewLocalizer`HTML エンコードされますが、パラメーター、リソース文字列ではありません。</span><span class="sxs-lookup"><span data-stu-id="5cd85-154">You can parameterize resource strings and `IViewLocalizer` will HTML encode the parameters, but not the resource string.</span></span> <span data-ttu-id="5cd85-155">次の Razor マークアップを考慮してください。</span><span class="sxs-lookup"><span data-stu-id="5cd85-155">Consider the following Razor markup:</span></span>

```HTML
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
   ```

<span data-ttu-id="5cd85-156">フランス語のリソース ファイルに次が可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5cd85-156">A French resource file could contain the following:</span></span>

| <span data-ttu-id="5cd85-157">キー</span><span class="sxs-lookup"><span data-stu-id="5cd85-157">Key</span></span> | <span data-ttu-id="5cd85-158">値</span><span class="sxs-lookup"><span data-stu-id="5cd85-158">Value</span></span> |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

<span data-ttu-id="5cd85-159">描画のビューには、リソース ファイルから HTML マークアップが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-159">The rendered view would contain the HTML markup from the resource file.</span></span>

<span data-ttu-id="5cd85-160">メモ:</span><span class="sxs-lookup"><span data-stu-id="5cd85-160">Notes:</span></span>
- <span data-ttu-id="5cd85-161">ローカライズのビューには、"Localization.AspNetCore.TagHelpers"NuGet パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-161">View localization requires the "Localization.AspNetCore.TagHelpers" NuGet package.</span></span>
- <span data-ttu-id="5cd85-162">一般にだけをローカライズするテキストと HTML ではありません。</span><span class="sxs-lookup"><span data-stu-id="5cd85-162">You generally want to only localize text and not HTML.</span></span>

<span data-ttu-id="5cd85-163">ビュー内の共有リソース ファイルを使用するのには、挿入`IHtmlLocalizer<T>`:</span><span class="sxs-lookup"><span data-stu-id="5cd85-163">To use a shared resource file in a view, inject `IHtmlLocalizer<T>`:</span></span>

<span data-ttu-id="5cd85-164">[!code-HTML[Main](../fundamentals/localization/sample/Views/Test/About.cshtml?highlight=5,12)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-164">[!code-HTML[Main](../fundamentals/localization/sample/Views/Test/About.cshtml?highlight=5,12)]</span></span>

## <a name="dataannotations-localization"></a><span data-ttu-id="5cd85-165">DataAnnotations ローカリゼーション</span><span class="sxs-lookup"><span data-stu-id="5cd85-165">DataAnnotations localization</span></span>

<span data-ttu-id="5cd85-166">DataAnnotations エラー メッセージとローカライズ`IStringLocalizer<T>`です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-166">DataAnnotations error messages are localized with `IStringLocalizer<T>`.</span></span> <span data-ttu-id="5cd85-167">オプションを使用して`ResourcesPath = "Resources"`、エラー メッセージで`RegisterViewModel`は次のパスのいずれかで格納されていることができます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-167">Using the option `ResourcesPath = "Resources"`, the error messages in `RegisterViewModel` can be stored in either of the following paths:</span></span>

* <span data-ttu-id="5cd85-168">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="5cd85-168">Resources/ViewModels.Account.RegisterViewModel.fr.resx</span></span>
* <span data-ttu-id="5cd85-169">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span><span class="sxs-lookup"><span data-stu-id="5cd85-169">Resources/ViewModels/Account/RegisterViewModel.fr.resx</span></span>

<span data-ttu-id="5cd85-170">[!code-csharp[Main](localization/sample/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-170">[!code-csharp[Main](localization/sample/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]</span></span>

<span data-ttu-id="5cd85-171">ASP.NET Core MVC 1.1.0 および以上、非検証属性のローカライズされています。</span><span class="sxs-lookup"><span data-stu-id="5cd85-171">In ASP.NET Core MVC 1.1.0 and higher, non-validation attributes are localized.</span></span> <span data-ttu-id="5cd85-172">ASP.NET Core MVC 1.0 は**いない**の非検証属性のローカライズされた文字列を検索します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-172">ASP.NET Core MVC 1.0 does **not** look up localized strings for non-validation attributes.</span></span>

<a name=one-resource-string-multiple-classes></a>
### <a name="using-one-resource-string-for-multiple-classes"></a><span data-ttu-id="5cd85-173">複数のクラスの 1 つのリソース文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-173">Using one resource string for multiple classes</span></span>

<span data-ttu-id="5cd85-174">次のコードは、複数のクラスに検証属性の 1 つのリソース文字列を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5cd85-174">The following code shows how to use one resource string for validation attributes with multiple classes:</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

<span data-ttu-id="5cd85-175">上記のコード、 `SharedResource` resx に対応するクラスには、検証メッセージを格納します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-175">In the preceeding code, `SharedResource` is the class corresponding to the resx where your validation messages are stored.</span></span> <span data-ttu-id="5cd85-176">この方法を DataAnnotations を使ってのみ`SharedResource`、各クラスのリソースではなくです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-176">With this approach,  DataAnnotations will only use `SharedResource`, rather than the resource for each class.</span></span> 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a><span data-ttu-id="5cd85-177">言語およびサポートするカルチャのローカライズされたリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-177">Provide localized resources for the languages and cultures you support</span></span>  

### <a name="supportedcultures-and-supporteduicultures"></a><span data-ttu-id="5cd85-178">SupportedCultures と SupportedUICultures</span><span class="sxs-lookup"><span data-stu-id="5cd85-178">SupportedCultures and SupportedUICultures</span></span>

<span data-ttu-id="5cd85-179">ASP.NET Core では、2 つのカルチャ値を指定できます。`SupportedCultures`と`SupportedUICultures`です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-179">ASP.NET Core allows you to specify two culture values, `SupportedCultures` and `SupportedUICultures`.</span></span> <span data-ttu-id="5cd85-180">[CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo)オブジェクトに対する`SupportedCultures`日付、時刻、数、および通貨の書式設定など、カルチャに依存する関数の結果を決定します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-180">The [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) object for `SupportedCultures` determines the results of culture-dependent functions, such as date, time, number, and currency formatting.</span></span> <span data-ttu-id="5cd85-181">`SupportedCultures`テキスト、大文字と小文字の表記規則、および文字列比較の並べ替え順序を決定します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-181">`SupportedCultures` also determines the sorting order of text, casing conventions, and string comparisons.</span></span> <span data-ttu-id="5cd85-182">参照してください[CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture)サーバーがカルチャを取得する方法の詳細。</span><span class="sxs-lookup"><span data-stu-id="5cd85-182">See [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) for more info on how the server gets the Culture.</span></span> <span data-ttu-id="5cd85-183">`SupportedUICultures`を決定し、これは文字列 (から*.resx*ファイル) を検索、 [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager)です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-183">The `SupportedUICultures` determines which translates strings (from *.resx* files) are looked up by the [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager).</span></span> <span data-ttu-id="5cd85-184">`ResourceManager`によって決定されるカルチャ固有の文字列を単に検索`CurrentUICulture`です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-184">The `ResourceManager` simply looks up culture-specific strings that is determined by `CurrentUICulture`.</span></span> <span data-ttu-id="5cd85-185">.NET のすべてのスレッドが`CurrentCulture`と`CurrentUICulture`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5cd85-185">Every thread in .NET has `CurrentCulture` and `CurrentUICulture` objects.</span></span> <span data-ttu-id="5cd85-186">ASP.NET Core は、カルチャに依存する関数を表示するときに、これらの値を検査します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-186">ASP.NET Core inspects these values when rendering culture-dependent functions.</span></span> <span data-ttu-id="5cd85-187">たとえば、現在のスレッドのカルチャが"EN-US"(英語、米国) に設定されている`DateTime.Now.ToLongDateString()`場合は、「木曜日、2016 年 2 月 18日」が表示されます`CurrentCulture`設定されている"ES-ES"(スペイン語、スペイン) に出力になります"jueves、18 de febrero de 2016" です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-187">For example, if the current thread's culture is set to "en-US" (English, United States), `DateTime.Now.ToLongDateString()` displays "Thursday, February 18, 2016", but if `CurrentCulture` is set to "es-ES" (Spanish, Spain) the output will be "jueves, 18 de febrero de 2016".</span></span>

## <a name="working-with-resource-files"></a><span data-ttu-id="5cd85-188">リソース ファイルの操作</span><span class="sxs-lookup"><span data-stu-id="5cd85-188">Working with resource files</span></span>

<span data-ttu-id="5cd85-189">リソース ファイルは、ローカライズ可能な文字列をコードから分離するために役立ちますメカニズムです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-189">A resource file is a useful mechanism for separating localizable strings from code.</span></span> <span data-ttu-id="5cd85-190">既定以外の言語の翻訳された文字列は分離*.resx*リソース ファイル。</span><span class="sxs-lookup"><span data-stu-id="5cd85-190">Translated strings for the non-default language are isolated *.resx* resource files.</span></span> <span data-ttu-id="5cd85-191">という名前のスペイン語のリソース ファイルを作成するなど、 *Welcome.es.resx*を含む文字列を変換します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-191">For example, you might want to create Spanish resource file named *Welcome.es.resx* containing translated strings.</span></span> <span data-ttu-id="5cd85-192">"es"では、スペイン語の言語コードを示します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-192">"es" is the language code for Spanish.</span></span> <span data-ttu-id="5cd85-193">Visual Studio でこのリソース ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-193">To create this resource file in Visual Studio:</span></span>

1. <span data-ttu-id="5cd85-194">**ソリューション エクスプ ローラー**、リソース ファイルを格納するフォルダーを右クリックして >**追加** > **新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-194">In **Solution Explorer**, right click on the folder which will contain the resource file > **Add** > **New Item**.</span></span>

    ![入れ子になったコンテキスト メニュー: ソリューション エクスプ ローラーでコンテキスト メニューは、リソース用に開かれています。](localization/_static/newi.png)

2. <span data-ttu-id="5cd85-197">**検索には、テンプレートがインストールされている**ボックスで、「リソース」を入力し、ファイルの名前します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-197">In the **Search installed templates** box, enter "resource" and name the file.</span></span>

    ![新しい項目 ダイアログ ボックスを追加します。](localization/_static/res.png)

3. <span data-ttu-id="5cd85-199">キーの値 (ネイティブの文字列) で入力、**名前**列と翻訳された文字列を**値**列です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-199">Enter the key value (native string) in the **Name** column and the translated string in the **Value** column.</span></span>

    ![Hello の 名前 列と 値 列内の単語 Hola (スペイン語でのこんにちは) Welcome.es.resx ファイル (へようこそ のリソース ファイルのスペイン語)](localization/_static/hola.png)

    <span data-ttu-id="5cd85-201">Visual Studio の表示、 *Welcome.es.resx*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5cd85-201">Visual Studio shows the *Welcome.es.resx* file.</span></span>

    ![ソリューション エクスプ ローラーのようこそスペイン語 (es) のリソース ファイルを表示](localization/_static/se.png)

<a name="error"></a>

<span data-ttu-id="5cd85-203">Visual Studio 2017 Preview バージョン 15.3 を使用している場合、リソース エディターでエラー インジケーターが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-203">If you are using Visual Studio 2017 Preview version 15.3, you'll get an error indicator in the resource editor.</span></span> <span data-ttu-id="5cd85-204">削除、 *ResXFileCodeGenerator*値から、*カスタム ツール*このエラー メッセージを防ぐためにプロパティ グリッド。</span><span class="sxs-lookup"><span data-stu-id="5cd85-204">Remove the *ResXFileCodeGenerator*  value from the *Custom Tool*  properties grid to prevent this error message:</span></span>

![Resx エディター](localization/_static/err.png)

<span data-ttu-id="5cd85-206">また、このエラーを無視することができます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-206">Alternatively, you can ignore this error.</span></span> <span data-ttu-id="5cd85-207">次のリリースでこの問題を解決する予定です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-207">We hope to fix this in the next release.</span></span>

## <a name="resource-file-naming"></a><span data-ttu-id="5cd85-208">リソース ファイルの名前付け</span><span class="sxs-lookup"><span data-stu-id="5cd85-208">Resource file naming</span></span>

<span data-ttu-id="5cd85-209">リソースは、アセンブリ名を除いて、クラスの完全な型名の名前です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-209">Resources are named for the full type name of their class minus the assembly name.</span></span> <span data-ttu-id="5cd85-210">フランス語のリソースをメインのアセンブリがプロジェクトになど`LocalizationWebsite.Web.dll`クラスの`LocalizationWebsite.Web.Startup`という名前になります*Startup.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-210">For example, a French resource in a project whose main assembly is `LocalizationWebsite.Web.dll` for the class `LocalizationWebsite.Web.Startup` would be named *Startup.fr.resx*.</span></span> <span data-ttu-id="5cd85-211">クラスのリソース`LocalizationWebsite.Web.Controllers.HomeController`という名前になります*Controllers.HomeController.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-211">A resource for the class `LocalizationWebsite.Web.Controllers.HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="5cd85-212">対象となるクラスの名前空間は、アセンブリ名と同じではない場合は、完全な型名を必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cd85-212">If your targeted class's namespace is not the same as the assembly name you will need the full type name.</span></span> <span data-ttu-id="5cd85-213">たとえば、サンプルには、プロジェクトの種類用のリソース`ExtraNamespace.Tools`という名前になります*ExtraNamespace.Tools.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-213">For example, in the sample project a resource for the type `ExtraNamespace.Tools` would be named *ExtraNamespace.Tools.fr.resx*.</span></span>

<span data-ttu-id="5cd85-214">サンプル プロジェクトで、`ConfigureServices`メソッドのセット、 `ResourcesPath` 「リソース」をそのため、home コント ローラーのフランス語りソース ファイルのプロジェクトの相対パスは*Resources/Controllers.HomeController.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-214">In the sample project, the `ConfigureServices` method sets the `ResourcesPath` to "Resources", so the project relative path for the home controller's French resource file is *Resources/Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="5cd85-215">また、リソース ファイルを整理するのにフォルダーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-215">Alternatively, you can use folders to organize resource files.</span></span> <span data-ttu-id="5cd85-216">Home コント ローラーで、パスはなります*Resources/Controllers/HomeController.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-216">For the home controller, the path would be *Resources/Controllers/HomeController.fr.resx*.</span></span> <span data-ttu-id="5cd85-217">使用しない場合、`ResourcesPath`オプション、 *.resx*ファイルは、プロジェクトの基本ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-217">If you don't use the `ResourcesPath` option, the *.resx* file would go in the project base directory.</span></span> <span data-ttu-id="5cd85-218">リソース ファイルの`HomeController`という名前になります*Controllers.HomeController.fr.resx*です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-218">The resource file for `HomeController` would be named *Controllers.HomeController.fr.resx*.</span></span> <span data-ttu-id="5cd85-219">ドットまたはパスの名前付け規則を選択した場合は、リソース ファイルを整理する方法によって異なります。</span><span class="sxs-lookup"><span data-stu-id="5cd85-219">The choice of using the dot or path naming convention depends on how you want to organize your resource files.</span></span>

| <span data-ttu-id="5cd85-220">リソース名</span><span class="sxs-lookup"><span data-stu-id="5cd85-220">Resource name</span></span> | <span data-ttu-id="5cd85-221">ドットまたはパスの名前を付ける</span><span class="sxs-lookup"><span data-stu-id="5cd85-221">Dot or path naming</span></span> |
| ------------   | ------------- |
| <span data-ttu-id="5cd85-222">Resources/Controllers.HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="5cd85-222">Resources/Controllers.HomeController.fr.resx</span></span> | <span data-ttu-id="5cd85-223">ドット</span><span class="sxs-lookup"><span data-stu-id="5cd85-223">Dot</span></span>  |
| <span data-ttu-id="5cd85-224">Resources/Controllers/HomeController.fr.resx</span><span class="sxs-lookup"><span data-stu-id="5cd85-224">Resources/Controllers/HomeController.fr.resx</span></span>  | <span data-ttu-id="5cd85-225">パス</span><span class="sxs-lookup"><span data-stu-id="5cd85-225">Path</span></span> |
|    |     |

<span data-ttu-id="5cd85-226">リソース ファイルを使用して`@inject IViewLocalizer`Razor ビューで、同様のパターンに従います。</span><span class="sxs-lookup"><span data-stu-id="5cd85-226">Resource files using `@inject IViewLocalizer` in Razor views follow a similar pattern.</span></span> <span data-ttu-id="5cd85-227">ビュー用のリソース ファイルは、ドット名前またはパスの名前付けを使用して名前付きことができます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-227">The resource file for a view can be named using either dot naming or path naming.</span></span> <span data-ttu-id="5cd85-228">Razor ビューのリソース ファイルは、関連するビューのファイルのパスを模倣します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-228">Razor view resource files mimic the path of their associated view file.</span></span> <span data-ttu-id="5cd85-229">設定と仮定した場合、`ResourcesPath`に「リソース」、フランス語のリソース ファイルに関連付けられて、 *Views/Home/About.cshtml*ビューでは、次のいずれかの可能性があります。</span><span class="sxs-lookup"><span data-stu-id="5cd85-229">Assuming we set the `ResourcesPath` to "Resources", the French resource file associated with the *Views/Home/About.cshtml* view could be either of the following:</span></span>

* <span data-ttu-id="5cd85-230">Resources/Views/Home/About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="5cd85-230">Resources/Views/Home/About.fr.resx</span></span>

* <span data-ttu-id="5cd85-231">Resources/Views.Home.About.fr.resx</span><span class="sxs-lookup"><span data-stu-id="5cd85-231">Resources/Views.Home.About.fr.resx</span></span>

<span data-ttu-id="5cd85-232">使用しない場合、`ResourcesPath`オプション、 *.resx*ファイルのビューは、ビューと同じフォルダーに配置するとします。</span><span class="sxs-lookup"><span data-stu-id="5cd85-232">If you don't use the `ResourcesPath` option, the *.resx* file for a view would be located in the same folder as the view.</span></span>

<span data-ttu-id="5cd85-233">".Fr"カルチャの指定子を削除し、カルチャをフランス語 (cookie または他の機構) 経由での設定を使用している場合は、既定のリソース ファイルは読み取りし、文字列はローカライズします。</span><span class="sxs-lookup"><span data-stu-id="5cd85-233">If you remove the ".fr" culture designator AND you have the culture set to French (via cookie or other mechanism), the default resource file is read and strings are localized.</span></span> <span data-ttu-id="5cd85-234">リソース マネージャーは、何も行われませんが、カルチャの指定子なし *.resx ファイルを処理している、要求されたカルチャを満たす場合、既定値またはフォールバック リソースを指定します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-234">The Resource manager designates a default or fallback resource, when nothing meets your requested culture you're served the *.resx file without a culture designator.</span></span> <span data-ttu-id="5cd85-235">要求されたカルチャのリソースが見つかりません必要があります、既定のリソース ファイルは持っていない場合にだけ、キーを返す場合は。</span><span class="sxs-lookup"><span data-stu-id="5cd85-235">If you want to just return the key when missing a resource for the requested culture you must not have a default resource file.</span></span>

### <a name="generating-resource-files-with-visual-studio"></a><span data-ttu-id="5cd85-236">Visual Studio でのリソース ファイルを生成します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-236">Generating resource files with Visual Studio</span></span>

<span data-ttu-id="5cd85-237">Visual Studio で、ファイル名にカルチャせず、リソース ファイルを作成した場合 (たとえば、 *Welcome.resx*)、Visual Studio は各文字列のプロパティを使用して、c# クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-237">If you create a resource file in Visual Studio without a culture in the file name (for example, *Welcome.resx*), Visual Studio will create a C# class with a property for each string.</span></span> <span data-ttu-id="5cd85-238">通常、たくないと ASP.NET Core です。通常、既定値がない*.resx*リソース ファイル (A *.resx*カルチャ名のないファイル)。</span><span class="sxs-lookup"><span data-stu-id="5cd85-238">That's usually not what you want with ASP.NET Core; you typically don't have a default *.resx* resource file (A *.resx* file without the culture name).</span></span> <span data-ttu-id="5cd85-239">作成することをお勧めします。、 *.resx*カルチャ名を持つファイル (たとえば*Welcome.fr.resx*)。</span><span class="sxs-lookup"><span data-stu-id="5cd85-239">We suggest you create the *.resx* file with a culture name (for example *Welcome.fr.resx*).</span></span> <span data-ttu-id="5cd85-240">作成するときに、 *.resx*カルチャ名、Visual Studio でのファイルは、クラス ファイルを生成しません。</span><span class="sxs-lookup"><span data-stu-id="5cd85-240">When you create a *.resx* file with a culture name, Visual Studio will not generate the class file.</span></span> <span data-ttu-id="5cd85-241">多くの開発者に**いない**既定の言語リソース ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-241">We anticipate that many developers will **not** create a default language resource file.</span></span>

### <a name="adding-other-cultures"></a><span data-ttu-id="5cd85-242">その他のカルチャを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-242">Adding Other Cultures</span></span>

<span data-ttu-id="5cd85-243">(既定の言語) 以外の言語とカルチャの組み合わせごとに一意のリソース ファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-243">Each language and culture combination (other than the default language) requires a unique resource file.</span></span> <span data-ttu-id="5cd85-244">ISO 言語コードがファイル名の一部となるリソース ファイルの新規作成して異なるカルチャとロケールのリソース ファイルを作成する (たとえば、 **en-us**、 **fr ca**、および**en gb**)。</span><span class="sxs-lookup"><span data-stu-id="5cd85-244">You create resource files for different cultures and locales by creating new resource files in which the ISO language codes are part of the file name (for example, **en-us**, **fr-ca**, and **en-gb**).</span></span> <span data-ttu-id="5cd85-245">これらの ISO コードを配置しているファイル名の間、 *.resx*ファイル名拡張子として*Welcome.es MX.resx* (スペイン語/メキシコ)。</span><span class="sxs-lookup"><span data-stu-id="5cd85-245">These ISO codes are placed between the file name and the *.resx* file name extension, as in *Welcome.es-MX.resx* (Spanish/Mexico).</span></span> <span data-ttu-id="5cd85-246">カルチャ ニュートラル言語を指定するには、国コードを削除 (`MX`前の例で)。</span><span class="sxs-lookup"><span data-stu-id="5cd85-246">To specify a culturally neutral language, remove the country code (`MX` in the preceding example).</span></span> <span data-ttu-id="5cd85-247">スペイン語のニュートラル カルチャのリソース ファイル名が*Welcome.es.resx*です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-247">The culturally neutral Spanish resource file name is *Welcome.es.resx*.</span></span>

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a><span data-ttu-id="5cd85-248">要求ごとに言語/カルチャを選択するための戦略を実装します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-248">Implement a strategy to select the language/culture for each request</span></span>  

### <a name="configuring-localization"></a><span data-ttu-id="5cd85-249">ローカリゼーションの構成</span><span class="sxs-lookup"><span data-stu-id="5cd85-249">Configuring localization</span></span>

<span data-ttu-id="5cd85-250">ローカリゼーションがで構成されている、`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5cd85-250">Localization is configured in the `ConfigureServices` method:</span></span>

<span data-ttu-id="5cd85-251">[!code-csharp[Main](localization/sample/Program.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-251">[!code-csharp[Main](localization/sample/Program.cs?name=snippet1)]</span></span>

* <span data-ttu-id="5cd85-252">`AddLocalization`ローカリゼーション サービスをサービス コンテナーに追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-252">`AddLocalization` Adds the localization services to the services container.</span></span> <span data-ttu-id="5cd85-253">上記のコードは、「リソース」のリソース パスを設定します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-253">The code above also sets the resources path to "Resources".</span></span>

* <span data-ttu-id="5cd85-254">`AddViewLocalization`ファイルのローカライズされた表示のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-254">`AddViewLocalization` Adds support for localized view files.</span></span> <span data-ttu-id="5cd85-255">このサンプル ビューでのローカライズをビュー ファイルのサフィックスに基づきます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-255">In this sample view localization is based on the view file suffix.</span></span> <span data-ttu-id="5cd85-256">たとえば"fr"で、 *Index.fr.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5cd85-256">For example "fr" in the *Index.fr.cshtml* file.</span></span>

* <span data-ttu-id="5cd85-257">`AddDataAnnotationsLocalization`ローカライズのサポートが追加されて`DataAnnotations`を介してメッセージを検証`IStringLocalizer`抽象化します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-257">`AddDataAnnotationsLocalization` Adds support for localized `DataAnnotations` validation messages through `IStringLocalizer` abstractions.</span></span>

### <a name="localization-middleware"></a><span data-ttu-id="5cd85-258">ローカリゼーション ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="5cd85-258">Localization middleware</span></span>

<span data-ttu-id="5cd85-259">要求時に、現在のカルチャは、ローカリゼーションで設定[ミドルウェア](middleware.md)です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-259">The current culture on a request is set in the localization [Middleware](middleware.md).</span></span> <span data-ttu-id="5cd85-260">ローカライズ ミドルウェアが有効になっている、`Configure`メソッドの*Program.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5cd85-260">The localization middleware is enabled in the `Configure` method of *Program.cs* file.</span></span> <span data-ttu-id="5cd85-261">要求のカルチャをチェックするすべてのミドルウェアの前に、ローカリゼーション ミドルウェアを構成する必要があります、注意してください (たとえば、 `app.UseMvcWithDefaultRoute()`)。</span><span class="sxs-lookup"><span data-stu-id="5cd85-261">Note, the localization middleware must be configured before any middleware which might check the request culture (for example, `app.UseMvcWithDefaultRoute()`).</span></span>

<span data-ttu-id="5cd85-262">[!code-csharp[Main](localization/sample/Program.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-262">[!code-csharp[Main](localization/sample/Program.cs?name=snippet2)]</span></span>

<span data-ttu-id="5cd85-263">`UseRequestLocalization`初期化、`RequestLocalizationOptions`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="5cd85-263">`UseRequestLocalization` initializes a `RequestLocalizationOptions` object.</span></span> <span data-ttu-id="5cd85-264">すべての要求リストの`RequestCultureProvider`で、`RequestLocalizationOptions`が列挙され、要求のカルチャが正常に決定できる最初のプロバイダーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-264">On every request the list of `RequestCultureProvider` in the `RequestLocalizationOptions` is enumerated and the first provider that can successfully determine the request culture is used.</span></span> <span data-ttu-id="5cd85-265">既定のプロバイダーに由来、`RequestLocalizationOptions`クラス。</span><span class="sxs-lookup"><span data-stu-id="5cd85-265">The default providers come from the `RequestLocalizationOptions` class:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="5cd85-266">既定の一覧は、最も限定的に最も固有から移動します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-266">The default list goes from most specific to least specific.</span></span> <span data-ttu-id="5cd85-267">記事の後半では、順序を変更し、であっても、カスタム プロバイダーを追加する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-267">Later in the article we'll see how you can change the order and even add a custom culture provider.</span></span> <span data-ttu-id="5cd85-268">要求のカルチャを判断、プロバイダーの場合、`DefaultRequestCulture`を使用します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-268">If none of the providers can determine the request culture, the `DefaultRequestCulture` is used.</span></span>

### <a name="querystringrequestcultureprovider"></a><span data-ttu-id="5cd85-269">QueryStringRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="5cd85-269">QueryStringRequestCultureProvider</span></span>

<span data-ttu-id="5cd85-270">いくつかのアプリでは、クエリ文字列を使用して設定は、[カルチャおよび UI カルチャ](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-270">Some apps will use a query string to set the [culture and UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx).</span></span> <span data-ttu-id="5cd85-271">をクッキーまたは Accept-language ヘッダーのアプローチを使用するアプリの URL にクエリ文字列を追加することはデバッグおよびコードのテストに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-271">For apps that use the cookie or Accept-Language header approach, adding a query string to the URL is useful for debugging and testing code.</span></span> <span data-ttu-id="5cd85-272">既定では、`QueryStringRequestCultureProvider`の最初のローカリゼーション プロバイダーとして登録されて、 `RequestCultureProvider`  ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-272">By default, the `QueryStringRequestCultureProvider` is registered as the first localization provider in the `RequestCultureProvider` list.</span></span> <span data-ttu-id="5cd85-273">クエリ文字列パラメーターを渡す`culture`と`ui-culture`です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-273">You pass the query string parameters `culture` and `ui-culture`.</span></span> <span data-ttu-id="5cd85-274">次の例では、特定のカルチャ (言語および地域) をスペイン語/メキシコに設定します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-274">The following example sets the specific culture (language and region) to Spanish/Mexico:</span></span>

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

<span data-ttu-id="5cd85-275">2 つのいずれかのみを渡す場合 (`culture`または`ui-culture`)、クエリ文字列のプロバイダーに渡された 1 つを使用して両方の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-275">If you only pass in one of the two (`culture` or `ui-culture`), the query string provider will set both values using the one you passed in.</span></span> <span data-ttu-id="5cd85-276">たとえば、カルチャだけを設定は両方を設定、`Culture`と`UICulture`:</span><span class="sxs-lookup"><span data-stu-id="5cd85-276">For example, setting just the culture will set both the `Culture` and the `UICulture`:</span></span>

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a><span data-ttu-id="5cd85-277">CookieRequestCultureProvider</span><span class="sxs-lookup"><span data-stu-id="5cd85-277">CookieRequestCultureProvider</span></span>

<span data-ttu-id="5cd85-278">実稼働アプリケーションは、ASP.NET Core カルチャ cookie を使用してカルチャを設定するためのメカニズムを提供して多くの場合、します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-278">Production apps will often provide a mechanism to set the culture with the ASP.NET Core culture cookie.</span></span> <span data-ttu-id="5cd85-279">使用して、 `MakeCookieValue` cookie を作成します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-279">Use the `MakeCookieValue`  method to create a cookie.</span></span>

<span data-ttu-id="5cd85-280">`CookieRequestCultureProvider` `DefaultCookieName`カルチャ情報を指定したユーザーを追跡するために使用する既定の cookie 名を返します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-280">The `CookieRequestCultureProvider` `DefaultCookieName`  returns the default cookie name used to track the user’s preferred culture information.</span></span> <span data-ttu-id="5cd85-281">既定の cookie 名は"です。AspNetCore.Culture"です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-281">The default cookie  name is ".AspNetCore.Culture".</span></span>

<span data-ttu-id="5cd85-282">Cookie の形式が`c=%LANGCODE%|uic=%LANGCODE%`ここで、`c`は`Culture`と`uic`は`UICulture`例。</span><span class="sxs-lookup"><span data-stu-id="5cd85-282">The cookie format is `c=%LANGCODE%|uic=%LANGCODE%`, where `c` is `Culture` and `uic` is `UICulture`, for example:</span></span>

    c=en-UK|uic=en-US

<span data-ttu-id="5cd85-283">のみを指定するカルチャ情報と UI カルチャのいずれかの場合、指定されたカルチャはカルチャ情報と UI カルチャの両方に使用されます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-283">If you only specify one of culture info and UI culture, the specified culture will be used for both culture info and UI culture.</span></span>

### <a name="the-accept-language-http-header"></a><span data-ttu-id="5cd85-284">ブラウザーの言語の HTTP ヘッダー</span><span class="sxs-lookup"><span data-stu-id="5cd85-284">The Accept-Language HTTP header</span></span>

<span data-ttu-id="5cd85-285">[Accept-language ヘッダー](https://www.w3.org/International/questions/qa-accept-lang-locales)ほとんどのブラウザーで設定可能であり、ユーザーの言語を指定するためのものが最初。</span><span class="sxs-lookup"><span data-stu-id="5cd85-285">The [Accept-Language header](https://www.w3.org/International/questions/qa-accept-lang-locales) is settable in most browsers and was originally intended to specify the user's language.</span></span> <span data-ttu-id="5cd85-286">この設定は、ブラウザーが送信に設定されているかは、基になるオペレーティング システムから継承する新機能を示します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-286">This setting indicates what the browser has been set to send or has inherited from the underlying operating system.</span></span> <span data-ttu-id="5cd85-287">ブラウザーの要求で Accept Language HTTP ヘッダーは、ユーザーの優先言語を検出するない方法ではありません (を参照してください[ブラウザーの言語設定を設定する](https://www.w3.org/International/questions/qa-lang-priorities.en.php))。</span><span class="sxs-lookup"><span data-stu-id="5cd85-287">The Accept-Language HTTP header from a browser request is not an infallible way to detect the user's preferred language (see [Setting language preferences in a browser](https://www.w3.org/International/questions/qa-lang-priorities.en.php)).</span></span> <span data-ttu-id="5cd85-288">運用アプリには、ユーザーの任意のカルチャをカスタマイズする方法を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="5cd85-288">A production app should include a way for a user to customize their choice of culture.</span></span>

### <a name="setting-the-accept-language-http-header-in-ie"></a><span data-ttu-id="5cd85-289">IE で Accept Language HTTP ヘッダーの設定</span><span class="sxs-lookup"><span data-stu-id="5cd85-289">Setting the Accept-Language HTTP header in IE</span></span>

1. <span data-ttu-id="5cd85-290">歯車アイコンをタップ**インターネット オプション**です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-290">From the gear icon, tap **Internet Options**.</span></span>

2. <span data-ttu-id="5cd85-291">タップ**言語**します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-291">Tap **Languages**.</span></span>

    ![インターネット オプション](localization/_static/lang.png)

3. <span data-ttu-id="5cd85-293">タップ**言語設定**です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-293">Tap **Set Language Preferences**.</span></span>

4. <span data-ttu-id="5cd85-294">タップ**言語を追加**です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-294">Tap **Add a language**.</span></span>

5. <span data-ttu-id="5cd85-295">言語を追加します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-295">Add the language.</span></span>

6. <span data-ttu-id="5cd85-296">言語をタップしてタップ**上へ移動**です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-296">Tap the language, then tap **Move Up**.</span></span>

### <a name="using-a-custom-provider"></a><span data-ttu-id="5cd85-297">カスタム プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-297">Using a custom provider</span></span>

<span data-ttu-id="5cd85-298">お客様は、データベース内の言語とカルチャを格納できるようにするとします。</span><span class="sxs-lookup"><span data-stu-id="5cd85-298">Suppose you want to let your customers store their language and culture in your databases.</span></span> <span data-ttu-id="5cd85-299">ユーザーの姓名を検索するプロバイダーを記述することもできます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-299">You could write a provider to look up these values for the user.</span></span> <span data-ttu-id="5cd85-300">次のコードは、カスタム プロバイダーを追加する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="5cd85-300">The following code shows how to add a custom provider:</span></span>

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

<span data-ttu-id="5cd85-301">使用して`RequestLocalizationOptions`を追加またはローカライズのプロバイダーを削除します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-301">Use `RequestLocalizationOptions` to add or remove localization providers.</span></span>

### <a name="setting-the-culture-programmatically"></a><span data-ttu-id="5cd85-302">プログラムによる、カルチャの設定</span><span class="sxs-lookup"><span data-stu-id="5cd85-302">Setting the culture programmatically</span></span>

<span data-ttu-id="5cd85-303">このサンプル**Localization.StarterWeb**プロジェクトでは、 [GitHub](https://github.com/aspnet/entropy)を設定するための UI が含まれています、`Culture`です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-303">This sample **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) contains UI to set the `Culture`.</span></span> <span data-ttu-id="5cd85-304">*Views/Shared/_SelectLanguagePartial.cshtml*ファイルでは、サポートされているカルチャの一覧から、カルチャを選択することができます。</span><span class="sxs-lookup"><span data-stu-id="5cd85-304">The *Views/Shared/_SelectLanguagePartial.cshtml* file allows you to select the culture from the list of supported cultures:</span></span>

<span data-ttu-id="5cd85-305">[!code-HTML[Main](localization/sample/Views/Shared/_SelectLanguagePartial.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-305">[!code-HTML[Main](localization/sample/Views/Shared/_SelectLanguagePartial.cshtml)]</span></span>

<span data-ttu-id="5cd85-306">*Views/Shared/_SelectLanguagePartial.cshtml*にファイルが追加、`footer`できるすべてのビューに使用できるようにレイアウト ファイルのセクション。</span><span class="sxs-lookup"><span data-stu-id="5cd85-306">The *Views/Shared/_SelectLanguagePartial.cshtml* file is added to the `footer` section of the layout file so it will be available to all views:</span></span>

<span data-ttu-id="5cd85-307">[!code-HTML[Main](localization/sample/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-307">[!code-HTML[Main](localization/sample/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]</span></span>

<span data-ttu-id="5cd85-308">`SetLanguage`メソッドはカルチャ cookie を設定します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-308">The `SetLanguage` method sets the culture cookie.</span></span>

<span data-ttu-id="5cd85-309">[!code-csharp[Main](localization/sample/Controllers/HomeController.cs?range=57-67)]</span><span class="sxs-lookup"><span data-stu-id="5cd85-309">[!code-csharp[Main](localization/sample/Controllers/HomeController.cs?range=57-67)]</span></span>

<span data-ttu-id="5cd85-310">接続することはできません、 *_SelectLanguagePartial.cshtml*このプロジェクトのサンプル コードにします。</span><span class="sxs-lookup"><span data-stu-id="5cd85-310">You can't plug in the *_SelectLanguagePartial.cshtml* to sample code for this project.</span></span> <span data-ttu-id="5cd85-311">**Localization.StarterWeb**プロジェクトでは、 [GitHub](https://github.com/aspnet/entropy)フローするコードが含ま、`RequestLocalizationOptions`を通じて部分 Razor を[依存性の注入](dependency-injection.md)コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-311">The **Localization.StarterWeb** project on [GitHub](https://github.com/aspnet/entropy) has code to flow the `RequestLocalizationOptions` to a Razor partial through the [Dependency Injection](dependency-injection.md) container.</span></span>

## <a name="globalization-and-localization-terms"></a><span data-ttu-id="5cd85-312">グローバリゼーションとローカリゼーションの用語</span><span class="sxs-lookup"><span data-stu-id="5cd85-312">Globalization and localization terms</span></span>

<span data-ttu-id="5cd85-313">アプリをローカライズするプロセスは、最新のソフトウェアの開発でよく使用される、関連する文字セットの基本的な理解とそれらに関連した問題の理解にも必要です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-313">The process of localizing your app also requires a basic understanding of relevant character sets commonly used in modern software development and an understanding of the issues associated with them.</span></span> <span data-ttu-id="5cd85-314">すべてのコンピューターは、数値 (コード) としてテキストを格納、さまざまなシステムは別の番号を使用して、同じテキストを格納します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-314">Although all computers store text as numbers (codes), different systems store the same text using different numbers.</span></span> <span data-ttu-id="5cd85-315">ローカリゼーション処理は、特定のカルチャとロケールのアプリのユーザー インターフェイス (UI) の変換を指します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-315">The localization process refers to translating the app user interface (UI) for a specific culture/locale.</span></span>

<span data-ttu-id="5cd85-316">[ローカライズ化](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review)は、グローバライズされたアプリのローカライズ可能を確認するための中間プロセスです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-316">[Localizability](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) is an intermediate process for verifying that a globalized app is ready for localization.</span></span>

<span data-ttu-id="5cd85-317">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt)カルチャ名の書式を設定"<languagecode2>-< country/regioncode2 >"ここで、<languagecode2>言語コードは、< country/regioncode2 > となりです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-317">The [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format for the culture name is "<languagecode2>-<country/regioncode2>", where <languagecode2> is the language code and <country/regioncode2> is the subculture code.</span></span> <span data-ttu-id="5cd85-318">たとえば、`es-CL`スペイン語 (チリ) 用`en-US`英語 (米国) の場合と`en-AU`英語 (オーストラリア) 用です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-318">For example, `es-CL` for Spanish (Chile), `en-US` for English (United States), and `en-AU` for English (Australia).</span></span> <span data-ttu-id="5cd85-319">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) ISO 639 言語に関連付けられている 2 文字の小文字のカルチャ コードと国または地域に関連付けられている 2 文字の大文字となり、ISO 3166 の組み合わせです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-319">[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) is a combination of an ISO 639 two-letter lowercase culture code associated with a language and an ISO 3166 two-letter uppercase subculture code associated with a country or region.</span></span>  <span data-ttu-id="5cd85-320">参照してください[言語カルチャ名](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-320">See [Language Culture Name](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).</span></span>

<span data-ttu-id="5cd85-321">国際化は、"I18N"に多くの場合、省略されています。</span><span class="sxs-lookup"><span data-stu-id="5cd85-321">Internationalization is often abbreviated to "I18N".</span></span> <span data-ttu-id="5cd85-322">省略形は、最初と最後の文字を受け取り、それらの間でため 18 文字の数は、最後の"N"、"I"、最初の文字の数を意味します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-322">The abbreviation takes the first and last letters and the number of letters between them, so 18 stands for the number of letters between the first "I" and the last "N".</span></span> <span data-ttu-id="5cd85-323">(G11N) のグローバリゼーションおよびローカリゼーション (L10N) にも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="5cd85-323">The same applies to Globalization (G11N), and Localization (L10N).</span></span>

<span data-ttu-id="5cd85-324">条件:</span><span class="sxs-lookup"><span data-stu-id="5cd85-324">Terms:</span></span>

* <span data-ttu-id="5cd85-325">グローバリゼーション (G11N): アプリ別の言語と地域をサポートするためのプロセスです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-325">Globalization (G11N): The process of making an app support different languages and regions.</span></span>
* <span data-ttu-id="5cd85-326">ローカライズ (L10N): のカスタマイズ プロセスの特定の言語および地域のアプリです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-326">Localization (L10N): The process of customizing an app for a given language and region.</span></span>
* <span data-ttu-id="5cd85-327">国際化 (I18N): は、グローバリゼーションおよびローカリゼーションの両方について説明します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-327">Internationalization (I18N): Describes both globalization and localization.</span></span>
* <span data-ttu-id="5cd85-328">カルチャ: は、言語および、必要に応じて、領域です。</span><span class="sxs-lookup"><span data-stu-id="5cd85-328">Culture: It is a language and, optionally, a region.</span></span>
* <span data-ttu-id="5cd85-329">ニュートラル カルチャ: 指定した言語が地域ではないカルチャ。</span><span class="sxs-lookup"><span data-stu-id="5cd85-329">Neutral culture: A culture that has a specified language, but not a region.</span></span> <span data-ttu-id="5cd85-330">(たとえば"en"、"es")</span><span class="sxs-lookup"><span data-stu-id="5cd85-330">(for example "en", "es")</span></span>
* <span data-ttu-id="5cd85-331">特定のカルチャ: 指定された言語と地域を持つカルチャ。</span><span class="sxs-lookup"><span data-stu-id="5cd85-331">Specific culture: A culture that has a specified language and region.</span></span> <span data-ttu-id="5cd85-332">(例"EN-US"、"EN-GB"、"es CL") の</span><span class="sxs-lookup"><span data-stu-id="5cd85-332">(for example "en-US", "en-GB", "es-CL")</span></span>
* <span data-ttu-id="5cd85-333">ロケール: ロケールとは、カルチャと同じです。</span><span class="sxs-lookup"><span data-stu-id="5cd85-333">Locale: A locale is the same as a culture.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5cd85-334">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="5cd85-334">Additional resources</span></span>

* <span data-ttu-id="5cd85-335">[Localization.StarterWeb プロジェクト](https://github.com/aspnet/entropy)アーティクルで使用します。</span><span class="sxs-lookup"><span data-stu-id="5cd85-335">[Localization.StarterWeb project](https://github.com/aspnet/entropy) used in the article.</span></span>
* [<span data-ttu-id="5cd85-336">Visual Studio でのリソース ファイル</span><span class="sxs-lookup"><span data-stu-id="5cd85-336">Resource Files in Visual Studio</span></span>](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [<span data-ttu-id="5cd85-337">.Resx ファイル内のリソース</span><span class="sxs-lookup"><span data-stu-id="5cd85-337">Resources in .resx Files</span></span>](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)