---
title: "移行の認証と Id"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0db145cb-41a5-448a-b889-72e2d789ad7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/identity
ms.openlocfilehash: b5a9bab4399714c481d4f38eeeaeba19d8bdd5b2
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="e5468-103">移行の認証と Id</span><span class="sxs-lookup"><span data-stu-id="e5468-103">Migrating Authentication and Identity</span></span>

<a name=migration-identity></a>

<span data-ttu-id="e5468-104">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e5468-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e5468-105">前の記事お[ASP.NET MVC プロジェクトから ASP.NET Core MVC に構成を移行した](configuration.md)です。</span><span class="sxs-lookup"><span data-stu-id="e5468-105">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="e5468-106">この記事では、登録、ログイン、およびユーザー管理機能を移行します。</span><span class="sxs-lookup"><span data-stu-id="e5468-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="e5468-107">Id およびメンバーシップを構成します。</span><span class="sxs-lookup"><span data-stu-id="e5468-107">Configure Identity and Membership</span></span>

<span data-ttu-id="e5468-108">ASP.NET mvc で Startup.Auth.cs および IdentityConfig.cs、App_Start フォルダーにある ASP.NET Id を使用して認証と id の機能が構成されます。</span><span class="sxs-lookup"><span data-stu-id="e5468-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="e5468-109">ASP.NET Core mvc でこれらの機能が構成されている*Startup.cs*です。</span><span class="sxs-lookup"><span data-stu-id="e5468-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="e5468-110">インストール、`Microsoft.AspNetCore.Identity.EntityFrameworkCore`と`Microsoft.AspNetCore.Authentication.Cookies`NuGet パッケージの管理。</span><span class="sxs-lookup"><span data-stu-id="e5468-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="e5468-111">その後、Startup.cs と更新プログラムを開く、 `ConfigureServices()` Entity Framework と Id サービスを使用する方法。</span><span class="sxs-lookup"><span data-stu-id="e5468-111">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="e5468-112">この時点では、ASP.NET MVC プロジェクトから移行していないまだ上記のコードで参照されている 2 つの種類があります:`ApplicationDbContext`と`ApplicationUser`です。</span><span class="sxs-lookup"><span data-stu-id="e5468-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="e5468-113">新しい*モデル*ASP.NET Core フォルダー プロジェクト、およびこれらの型に対応する 2 つのクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5468-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="e5468-114">見つかります ASP.NET MVC のバージョンでこれらのクラスの`/Models/IdentityModels.cs`がより明確であるため移行対象のプロジェクト内のクラスごとに 1 ファイルが使用されます。</span><span class="sxs-lookup"><span data-stu-id="e5468-114">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="e5468-115">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="e5468-115">ApplicationUser.cs:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="e5468-116">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="e5468-116">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="e5468-117">ASP.NET Core MVC スターター Web プロジェクトは、多くのカスタマイズのユーザー、または、ApplicationDbContext に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="e5468-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="e5468-118">すべてのカスタム プロパティと、アプリケーションのユーザーと DbContext クラスだけでなく、アプリケーション (たとえば、ある場合、DbContext DbSetを利用して他のモデルクラスのメソッドに移行する必要がありますに実際のアプリケーションを移行する場合<Album>、アルバムのクラスを移行する必要がありますもちろん)。</span><span class="sxs-lookup"><span data-stu-id="e5468-118">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="e5468-119">これらのファイルの場所で、Startup.cs ファイルにできるを使用して更新することでコンパイルするステートメント。</span><span class="sxs-lookup"><span data-stu-id="e5468-119">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="e5468-120">アプリケーションは認証と id サービスをサポートする準備ができました - これらの機能をユーザーに公開するだけで済みます。</span><span class="sxs-lookup"><span data-stu-id="e5468-120">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="e5468-121">登録とログインのロジックを移行します。</span><span class="sxs-lookup"><span data-stu-id="e5468-121">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="e5468-122">Id サービスを使用するアプリケーションおよび Entity Framework および SQL Server を使用して構成データ アクセス用に構成されたおと、アプリケーションへの登録とログイン サポートを追加する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="e5468-122">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="e5468-123">注意してください[移行プロセスの前半](mvc.md#migrate-layout-file)おをコメント アウト _Layout.cshtml で _LoginPartial への参照。</span><span class="sxs-lookup"><span data-stu-id="e5468-123">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="e5468-124">では、そのコードに返すコメントを解除し、必要なコント ローラーとログイン機能をサポートするためにビューに追加します。</span><span class="sxs-lookup"><span data-stu-id="e5468-124">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="e5468-125">_Layout.cshtml; の更新します。コメントを解除、@Html.Partial行。</span><span class="sxs-lookup"><span data-stu-id="e5468-125">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "none"} -->

```none
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="e5468-126">ビュー/共有フォルダーに _LoginPartial と呼ばれる新しい MVC ビュー ページを追加します。</span><span class="sxs-lookup"><span data-stu-id="e5468-126">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="e5468-127">次のコードで _LoginPartial.cshtml を更新 (すべての内容を置き換えます)。</span><span class="sxs-lookup"><span data-stu-id="e5468-127">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="e5468-128">この時点では、ブラウザーでサイトを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="e5468-128">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="e5468-129">概要</span><span class="sxs-lookup"><span data-stu-id="e5468-129">Summary</span></span>

<span data-ttu-id="e5468-130">ASP.NET Core では、ASP.NET Identity の機能への変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="e5468-130">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="e5468-131">この記事では、ASP.NET Core に ASP.NET Id の認証とユーザー管理機能を移行する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="e5468-131">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>