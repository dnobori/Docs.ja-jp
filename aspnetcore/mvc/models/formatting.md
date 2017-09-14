---
title: "ASP.NET Core MVC での応答データを書式設定"
author: ardalis
description: "ASP.NET Core MVC での応答データを書式設定する方法を説明します。"
keywords: "ASP.NET Core、応答データ、IOutputFormatter、IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7fd1ecb61c7b1559f8bd304c30f7201864bd448
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="12f56-104">ASP.NET Core MVC での応答データの書式設定の概要</span><span class="sxs-lookup"><span data-stu-id="12f56-104">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="12f56-105">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="12f56-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="12f56-106">ASP.NET Core MVC には、クライアントの仕様に応答したり固定形式を使用して、応答データを書式設定するための組み込みサポートがあります。</span><span class="sxs-lookup"><span data-stu-id="12f56-106">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="12f56-107">[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample)です。</span><span class="sxs-lookup"><span data-stu-id="12f56-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="12f56-108">ファイル形式に固有のアクションの結果</span><span class="sxs-lookup"><span data-stu-id="12f56-108">Format-Specific Action Results</span></span>

<span data-ttu-id="12f56-109">などが特定の形式に固有のいくつかのアクション結果型は`JsonResult`と`ContentResult`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-109">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="12f56-110">アクションは、特定の方法で常にフォーマットされている特定の結果を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="12f56-110">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="12f56-111">たとえばを返す、`JsonResult`クライアント設定に関係なく、JSON 形式のデータが返されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-111">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="12f56-112">同様を返す、 `ContentResult` (単に文字列を返すことが)、プレーン テキスト形式の文字列データが返されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-112">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="12f56-113">アクションを特定の種類を返す必要はありません。MVC には、任意のオブジェクトの戻り値がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="12f56-113">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="12f56-114">アクションを返す場合、`IActionResult`実装とコント ローラーから継承`Controller`開発者の選択肢の多くに対応する多数のヘルパー メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="12f56-114">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="12f56-115">オブジェクトを返すアクションからの結果は`IActionResult`、適切な型をシリアル化される`IOutputFormatter`実装します。</span><span class="sxs-lookup"><span data-stu-id="12f56-115">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="12f56-116">継承するコント ローラーから特定の形式でデータを返す、`Controller`基底クラス、組み込みのヘルパー メソッドを使用して`Json`を JSON を返すと`Content`のプレーン テキスト。</span><span class="sxs-lookup"><span data-stu-id="12f56-116">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="12f56-117">アクション メソッドは、特定の結果の型を返す必要があります (たとえば、 `JsonResult`) または`IActionResult`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-117">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="12f56-118">JSON 形式のデータを返す:</span><span class="sxs-lookup"><span data-stu-id="12f56-118">Returning JSON-formatted data:</span></span>

<span data-ttu-id="12f56-119">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]</span><span class="sxs-lookup"><span data-stu-id="12f56-119">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]</span></span>

<span data-ttu-id="12f56-120">このアクションから応答のサンプル:</span><span class="sxs-lookup"><span data-stu-id="12f56-120">Sample response from this action:</span></span>

![応答のコンテンツの種類を示す Microsoft edge 開発者ツールの [ネットワーク] タブは、アプリケーション/json です。](formatting/_static/json-response.png)

<span data-ttu-id="12f56-122">なお、応答のコンテンツの種類は`application/json`ネットワーク要求の一覧と、応答ヘッダーのセクションの両方が表示される。</span><span class="sxs-lookup"><span data-stu-id="12f56-122">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="12f56-123">要求ヘッダーのセクションの Accept ヘッダーで (ここでは、Microsoft Edge) ブラウザーで表示されるオプションの一覧にも注意してください。</span><span class="sxs-lookup"><span data-stu-id="12f56-123">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="12f56-124">現在の手法では、このヘッダーを無視します。その名実は後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="12f56-124">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="12f56-125">プレーン テキストの書式設定されたデータを返すには使用`ContentResult`と`Content`ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="12f56-125">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

<span data-ttu-id="12f56-126">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]</span><span class="sxs-lookup"><span data-stu-id="12f56-126">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]</span></span>

<span data-ttu-id="12f56-127">この操作からの応答:</span><span class="sxs-lookup"><span data-stu-id="12f56-127">A response from this action:</span></span>

![応答のコンテンツの種類を示す Microsoft edge 開発者ツールの [ネットワーク] タブは、テキスト/プレーンです。](formatting/_static/text-response.png)

<span data-ttu-id="12f56-129">この場合、`Content-Type`が返される`text/plain`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-129">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="12f56-130">また、文字列の応答タイプだけを使用して同じ動作を実現できます。</span><span class="sxs-lookup"><span data-stu-id="12f56-130">You can also achieve this same behavior using just a string response type:</span></span>

<span data-ttu-id="12f56-131">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]</span><span class="sxs-lookup"><span data-stu-id="12f56-131">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]</span></span>

>[!TIP]
> <span data-ttu-id="12f56-132">複数の重要な操作の種類またはオプション (たとえば、さまざまな HTTP ステータス コードが実行される操作の結果に基づく) を返す、必要に応じて`IActionResult`戻り値の型として。</span><span class="sxs-lookup"><span data-stu-id="12f56-132">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="12f56-133">コンテンツ ネゴシエーション</span><span class="sxs-lookup"><span data-stu-id="12f56-133">Content Negotiation</span></span>

<span data-ttu-id="12f56-134">コンテンツのネゴシエーション (*conneg*略して) クライアントを指定するときに発生、 [Accept ヘッダー](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)です。</span><span class="sxs-lookup"><span data-stu-id="12f56-134">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="12f56-135">ASP.NET Core MVC で使用される既定の形式は、JSON です。</span><span class="sxs-lookup"><span data-stu-id="12f56-135">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="12f56-136">コンテンツ ネゴシエーションはによって実装`ObjectResult`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-136">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="12f56-137">特定のアクション結果のヘルパー メソッドから返されるステータス コードにも組み込まれている (すべてに基づいて`ObjectResult`)。</span><span class="sxs-lookup"><span data-stu-id="12f56-137">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="12f56-138">モデルの種類 (データ転送の種類として定義したクラス) を取得することもでき、フレームワークは自動的にそれを囲みで、`ObjectResult`します。</span><span class="sxs-lookup"><span data-stu-id="12f56-138">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="12f56-139">次のアクション メソッドを使用して、`Ok`と`NotFound`ヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="12f56-139">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

<span data-ttu-id="12f56-140">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]</span><span class="sxs-lookup"><span data-stu-id="12f56-140">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]</span></span>

<span data-ttu-id="12f56-141">ない場合は、別の形式が要求されたサーバーは要求された形式で返すことができます、JSON 形式の応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-141">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="12f56-142">などのツールを使用する[Fiddler](http://www.telerik.com/fiddler)を Accept ヘッダーが含まれる要求を作成し、別の形式を指定します。</span><span class="sxs-lookup"><span data-stu-id="12f56-142">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="12f56-143">その場合、サーバーがある場合、*フォーマッタ*要求の形式で応答を生成することができます、結果が返されますクライアント優先形式です。</span><span class="sxs-lookup"><span data-stu-id="12f56-143">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![手動で作成を示す fiddler コンソール アプリケーションまたは xml の Accept ヘッダー値を持つ要求を取得します。](formatting/_static/fiddler-composer.png)

<span data-ttu-id="12f56-145">上のスクリーン ショットでは、要求を生成する、Fiddler 作成ツールが使用されています指定`Accept: application/xml`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-145">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="12f56-146">既定では、ASP.NET Core MVC のみをサポートしている JSON でも返される結果は JSON 形式も、別の形式を指定すると、します。</span><span class="sxs-lookup"><span data-stu-id="12f56-146">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="12f56-147">次のセクションの他のフォーマッタを追加する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-147">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="12f56-148">コント ローラーのアクションはした poco から (Plain Old CLR Object) を返すことができる場合に ASP.NET MVC が自動的に作成、`ObjectResult`オブジェクトをラップします。</span><span class="sxs-lookup"><span data-stu-id="12f56-148">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="12f56-149">クライアントが形式のシリアル化されたオブジェクトを取得 (JSON 形式では、既定以外の場合は XML またはその他の形式を構成することができます)。</span><span class="sxs-lookup"><span data-stu-id="12f56-149">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="12f56-150">対象のオブジェクトが返された場合`null`、フレームワークは戻ります、`204 No Content`応答します。</span><span class="sxs-lookup"><span data-stu-id="12f56-150">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="12f56-151">オブジェクトの種類を取得します。</span><span class="sxs-lookup"><span data-stu-id="12f56-151">Returning an object type:</span></span>

<span data-ttu-id="12f56-152">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]</span><span class="sxs-lookup"><span data-stu-id="12f56-152">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]</span></span>

<span data-ttu-id="12f56-153">サンプルでは、有効な作成者エイリアスの要求は、著者のデータを 200 OK の応答に表示されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-153">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="12f56-154">無効なエイリアスの要求は、204 No Content 応答に表示されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-154">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="12f56-155">XML および JSON 形式で応答を示すスクリーン ショットは、以下に示します。</span><span class="sxs-lookup"><span data-stu-id="12f56-155">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="12f56-156">コンテンツ ネゴシエーションのプロセス</span><span class="sxs-lookup"><span data-stu-id="12f56-156">Content Negotiation Process</span></span>

<span data-ttu-id="12f56-157">コンテンツ*ネゴシエーション*のみが配置の場合、`Accept`要求のヘッダーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-157">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="12f56-158">要求には、accept ヘッダーが含まれている、フレームワークの優先順位で accept ヘッダーのメディアの種類を列挙しようし、します accept ヘッダーで指定された形式のいずれかで応答を生成できるフォーマッタを検出します。</span><span class="sxs-lookup"><span data-stu-id="12f56-158">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="12f56-159">フォーマッタが見つからない、クライアントの要求を満たすことができる場合に、フレームワークは、応答を生成する最初のフォーマッタを検索しよう (開発者でオプションを構成しない限り、`MvcOptions`を返す 406 Not Acceptable 代わりに)。</span><span class="sxs-lookup"><span data-stu-id="12f56-159">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="12f56-160">要求は、XML を指定、XML フォーマッタが構成されていない場合は、JSON フォーマッタが使用されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-160">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="12f56-161">一般的には、フォーマッタが構成されていない場合は、要求された形式を提供することができますし、オブジェクトの書式を設定よりも、最初のフォーマッタを使用します。</span><span class="sxs-lookup"><span data-stu-id="12f56-161">More generally, if no formatter is configured that can provide the requested format, then the first formatter than can format the object is used.</span></span> <span data-ttu-id="12f56-162">ヘッダーが指定されていない場合、返されるオブジェクトを処理できる最初のフォーマッタは、応答をシリアル化するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-162">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="12f56-163">この場合、ネゴシエーションの取得の任意の場所がありません - サーバーには、どのような形式が使用されますが決定することです。</span><span class="sxs-lookup"><span data-stu-id="12f56-163">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="12f56-164">Accept ヘッダーが含まれている場合`*/*`、しない限り、ヘッダーは無視されます`RespectBrowserAcceptHeader`に設定されている場合は true`MvcOptions`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-164">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="12f56-165">ブラウザーとコンテンツ ネゴシエーション</span><span class="sxs-lookup"><span data-stu-id="12f56-165">Browsers and Content Negotiation</span></span>

<span data-ttu-id="12f56-166">一般的な API は、クライアントとは異なりを指定する傾向が web ブラウザー`Accept`をさまざまな形式と、ワイルドカードを含むヘッダー。</span><span class="sxs-lookup"><span data-stu-id="12f56-166">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="12f56-167">既定では、フレームワークは、ブラウザーから、要求が送信されたことが検出された場合は無視されます、`Accept`ヘッダーと戻り値の代わりに、アプリケーション内のコンテンツの構成済みの既定の形式 (JSON 設定しない場合)。</span><span class="sxs-lookup"><span data-stu-id="12f56-167">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="12f56-168">これにより、さまざまなブラウザーを使用して、Api を使用する場合より一貫した使用環境が提供します。</span><span class="sxs-lookup"><span data-stu-id="12f56-168">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="12f56-169">場合は、アプリケーションの記念ブラウザー accept ヘッダーを使用する場合、するこの MVC の構成の一部として設定して構成できます`RespectBrowserAcceptHeader`に`true`で、`ConfigureServices`メソッド*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="12f56-169">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
}
```

## <a name="configuring-formatters"></a><span data-ttu-id="12f56-170">フォーマッタを構成します。</span><span class="sxs-lookup"><span data-stu-id="12f56-170">Configuring Formatters</span></span>

<span data-ttu-id="12f56-171">アプリケーションは、JSON の既定値を超える追加の形式をサポートする必要がある場合、は、NuGet パッケージを追加して、それらをサポートする MVC を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="12f56-171">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="12f56-172">入力と出力に別のフォーマッタがあります。</span><span class="sxs-lookup"><span data-stu-id="12f56-172">There are separate formatters for input and output.</span></span> <span data-ttu-id="12f56-173">入力のフォーマッタが使用[モデル バインド](model-binding.md); 出力フォーマッタが応答を書式設定に使用されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-173">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="12f56-174">構成することも[カスタム フォーマッタ](../advanced/custom-formatters.md)です。</span><span class="sxs-lookup"><span data-stu-id="12f56-174">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="12f56-175">XML 形式のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="12f56-175">Adding XML Format Support</span></span>

<span data-ttu-id="12f56-176">XML が書式設定のサポートを追加するには、インストール、 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="12f56-176">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="12f56-177">MVC の構成に、XmlSerializerFormatters を追加*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="12f56-177">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

<span data-ttu-id="12f56-178">[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="12f56-178">[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]</span></span>

<span data-ttu-id="12f56-179">代わりに、出力フォーマッタだけを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="12f56-179">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="12f56-180">これら 2 つの方法が使用して結果をシリアル化`System.Xml.Serialization.XmlSerializer`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-180">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="12f56-181">使用することができます、したい場合、`System.Runtime.Serialization.DataContractSerializer`その関連付けられたフォーマッタを追加することで。</span><span class="sxs-lookup"><span data-stu-id="12f56-181">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="12f56-182">コント ローラーのメソッドが要求のに基づいて、適切な形式を返す必要があります XML が書式設定のサポートを追加したら、`Accept`ヘッダーがこの Fiddler を例に示します。</span><span class="sxs-lookup"><span data-stu-id="12f56-182">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler コンソール: Accept ヘッダーの値は、アプリケーションまたは xml 要求の生のタブを示しています。](formatting/_static/xml-response.png)

<span data-ttu-id="12f56-185">生の GET 要求が行われたインスペクター タブで確認できます、`Accept: application/xml`ヘッダーが設定されています。</span><span class="sxs-lookup"><span data-stu-id="12f56-185">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="12f56-186">応答のペインの表示、`Content-Type: application/xml`ヘッダー、および`Author`オブジェクトを XML にシリアル化されました。</span><span class="sxs-lookup"><span data-stu-id="12f56-186">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="12f56-187">指定する要求を変更する作成ツール タブを使用して`application/json`で、`Accept`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="12f56-187">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="12f56-188">要求を実行して、応答は、JSON として書式設定されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-188">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler コンソール: Accept ヘッダーの値は application/json と要求の生のタブを示しています。](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="12f56-191">このスクリーン ショットでは、要求のヘッダーに設定を確認できます`Accept: application/json`応答を指定と同じとその`Content-Type`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-191">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="12f56-192">`Author` JSON 形式で、応答の本文にオブジェクトが示すようにします。</span><span class="sxs-lookup"><span data-stu-id="12f56-192">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="12f56-193">特定の形式の強制</span><span class="sxs-lookup"><span data-stu-id="12f56-193">Forcing a Particular Format</span></span>

<span data-ttu-id="12f56-194">特定の操作の応答形式を制限したい場合にすることができます、適用することができます、`[Produces]`フィルター。</span><span class="sxs-lookup"><span data-stu-id="12f56-194">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="12f56-195">`[Produces]`フィルターは、特定のアクション (またはコント ローラー) の応答形式を指定します。</span><span class="sxs-lookup"><span data-stu-id="12f56-195">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="12f56-196">などのほとんど[フィルター](../controllers/filters.md)、これは、アクション、コント ローラー、またはグローバル スコープで適用できます。</span><span class="sxs-lookup"><span data-stu-id="12f56-196">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="12f56-197">`[Produces]`フィルター内のすべてのアクションが強制的に実行、`AuthorsController`を JSON 形式の応答を返す、他のフォーマッタは、アプリケーションと指定されたクライアント用に構成された場合でも、`Accept`別、要求ヘッダー使用可能な形式です。</span><span class="sxs-lookup"><span data-stu-id="12f56-197">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="12f56-198">参照してください[フィルター](../controllers/filters.md)全体にフィルターを適用する方法を含む詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="12f56-198">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="12f56-199">特殊なケース フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="12f56-199">Special Case Formatters</span></span>

<span data-ttu-id="12f56-200">特殊なケースは、組み込みのフォーマッタを使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="12f56-200">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="12f56-201">既定では、`string`戻り値の型として書式設定*テキスト/プレーン*(*テキスト/html*経由で要求された場合`Accept`ヘッダー)。</span><span class="sxs-lookup"><span data-stu-id="12f56-201">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="12f56-202">この動作を削除することで削除することができます、`TextOutputFormatter`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-202">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="12f56-203">フォーマッタを削除する、`Configure`メソッド*Startup.cs* (下図参照)。</span><span class="sxs-lookup"><span data-stu-id="12f56-203">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="12f56-204">型を返す、204 コンテンツ応答がない場合を返すモデル オブジェクトがあるアクションを返す`null`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-204">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="12f56-205">この動作を削除することで削除することができます、`HttpNoContentOutputFormatter`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-205">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="12f56-206">次のコードを削除、`TextOutputFormatter`と`HttpNoContentOutputFormatter`です。</span><span class="sxs-lookup"><span data-stu-id="12f56-206">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="12f56-207">なし、 `TextOutputFormatter`、`string`型を返す 406 を返す例については、許容できません。</span><span class="sxs-lookup"><span data-stu-id="12f56-207">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="12f56-208">XML フォーマッタが存在する場合、これにフォーマットすることに注意してください`string`型を返す場合、`TextOutputFormatter`を削除します。</span><span class="sxs-lookup"><span data-stu-id="12f56-208">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="12f56-209">なし、 `HttpNoContentOutputFormatter`、null オブジェクトは構成済みのフォーマッタを使用して書式設定します。</span><span class="sxs-lookup"><span data-stu-id="12f56-209">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="12f56-210">たとえば、JSON フォーマッタが返されるだけ応答の本体を持つ`null`XML フォーマッタは、属性を持つ空の XML 要素を返すときに、`xsi:nil="true"`を設定します。</span><span class="sxs-lookup"><span data-stu-id="12f56-210">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="12f56-211">応答の形式の URL マッピング</span><span class="sxs-lookup"><span data-stu-id="12f56-211">Response Format URL Mappings</span></span>

<span data-ttu-id="12f56-212">クライアントで要求できますを特定の形式、URL の一部としてなど、クエリ文字列またはまたは .xml または .json などの形式に固有のファイル拡張子を使用して、パスの一部です。</span><span class="sxs-lookup"><span data-stu-id="12f56-212">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="12f56-213">API を使用してルート上では、要求パスからのマッピングを指定してください。</span><span class="sxs-lookup"><span data-stu-id="12f56-213">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="12f56-214">例:</span><span class="sxs-lookup"><span data-stu-id="12f56-214">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="12f56-215">このルートには、省略可能なファイル拡張子として指定する要求の形式ができるようにします。</span><span class="sxs-lookup"><span data-stu-id="12f56-215">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="12f56-216">`[FormatFilter]`属性の形式の値の存在を確認、`RouteData`し、応答の作成時に、応答形式を適切なフォーマッタにマップされます。</span><span class="sxs-lookup"><span data-stu-id="12f56-216">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="12f56-217">ルート</span><span class="sxs-lookup"><span data-stu-id="12f56-217">Route</span></span>                      | <span data-ttu-id="12f56-218">フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="12f56-218">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="12f56-219">既定の出力フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="12f56-219">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="12f56-220">(構成されている) 場合は、JSON フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="12f56-220">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="12f56-221">(構成されている) 場合は、XML フォーマッタ</span><span class="sxs-lookup"><span data-stu-id="12f56-221">The XML formatter (if configured)</span></span>  |