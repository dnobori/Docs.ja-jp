---
title: "ASP.NET MVC を持つコアを EF コアな移行 - 4 10"
author: tdykstra
description: "このチュートリアルでは、ASP.NET Core MVC アプリケーションでデータ モデルの変更を管理するための EF 中核となる移行機能の使用を開始します。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 9081ddd14e6ed9192c6bd8ce8b265d14dbca7e23
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="17e2b-103">移行 - ASP.NET Core MVC のチュートリアル (10 の 4) と EF コア</span><span class="sxs-lookup"><span data-stu-id="17e2b-103">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="17e2b-104">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17e2b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="17e2b-105">Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="17e2b-106">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="17e2b-107">このチュートリアルでは、データ モデルの変更を管理するための EF 中核となる移行機能の使用を開始します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="17e2b-108">後のチュートリアルでは、データ モデルを変更すると、複数の移行を追加します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="17e2b-109">移行の概要</span><span class="sxs-lookup"><span data-stu-id="17e2b-109">Introduction to migrations</span></span>

<span data-ttu-id="17e2b-110">新しいアプリケーションを開発するときに、データ モデルの変更するたびにし、多くの場合、モデルの変更、データベースとの同期を取得します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="17e2b-111">これらのチュートリアルを開始するには、Entity Framework が存在しない場合、データベースの作成を構成します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="17e2b-112">データ モデルの変更--追加、削除、またはエンティティ クラスを変更または--DbContext クラスを変更するたびにデータベースを削除することができ、EF を新規作成、モデルと一致し、テスト データのシードを設定します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="17e2b-113">データベースのデータ モデルとの同期を維持するには、このメソッドは、実稼働環境にアプリケーションを配置するまでに適切に動作します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="17e2b-114">アプリケーションが、保持して、毎回すべてが失われるしたくないデータを格納するは、通常実稼働環境で実行されている場合は、新しい列を追加するなど変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="17e2b-114">When the application is running in production it is usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="17e2b-115">EF コア移行機能を新しいデータベースを作成する代わりにデータベース スキーマを更新する EF を有効にしてこの問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="17e2b-116">移行用の Entity Framework Core NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="17e2b-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="17e2b-117">移行を使用して作業を行うこともできます、 **Package Manager Console** (PMC) またはコマンド ライン インターフェイス (CLI)。</span><span class="sxs-lookup"><span data-stu-id="17e2b-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="17e2b-118">これらのチュートリアルでは、CLI コマンドを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="17e2b-119">PMC に関するについては、「[このチュートリアルの最後の](#pmc)します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="17e2b-120">コマンド ライン インターフェイス (CLI) の EF ツールが [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) で提供されています。</span><span class="sxs-lookup"><span data-stu-id="17e2b-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="17e2b-121">このパッケージをインストールする追加して、`DotNetCliToolReference`内のコレクション、 *.csproj*ファイルを示すようにします。</span><span class="sxs-lookup"><span data-stu-id="17e2b-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="17e2b-122">**注:** *.csproj* ファイルを編集して、このパッケージをインストールする必要があります。`install-package` コマンドやパッケージ マネージャー GUI を使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="17e2b-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="17e2b-123">編集することができます、 *.csproj*でプロジェクト名を右クリックしてファイル**ソリューション エクスプ ローラー**を選択して**編集 ContosoUniversity.csproj**です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="17e2b-124">(この例では、バージョン番号は、このチュートリアルに書き込まれたときに現在でした)。</span><span class="sxs-lookup"><span data-stu-id="17e2b-124">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="17e2b-125">接続文字列を変更します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-125">Change the connection string</span></span>

<span data-ttu-id="17e2b-126">*される appsettings.json*ファイル、ContosoUniversity2 または使用しているコンピューターで使用していない別の名前に、接続文字列にデータベースの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="17e2b-127">この変更は、最初の移行が新しいデータベースを作成できるように、プロジェクトを設定します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="17e2b-128">これは、移行には、使用を開始するために必要はありませんが、後述する理由をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="17e2b-128">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="17e2b-129">データベース名を変更する代わりに、データベースを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="17e2b-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="17e2b-130">使用して**SQL Server オブジェクト エクスプ ローラー** (SSOX) または`database drop`CLI コマンド。</span><span class="sxs-lookup"><span data-stu-id="17e2b-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="17e2b-131">次のセクションでは、CLI コマンドを実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="17e2b-132">最初の移行を作成します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-132">Create an initial migration</span></span>

<span data-ttu-id="17e2b-133">変更を保存し、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="17e2b-133">Save your changes and build the project.</span></span> <span data-ttu-id="17e2b-134">コマンド ウィンドウを開き、プロジェクト フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="17e2b-135">実行する簡単な方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="17e2b-136">**ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**エクスプ ローラーで開く**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="17e2b-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![ファイル エクスプ ローラーのメニュー項目で開く](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="17e2b-138">アドレス バーに「cmd」を入力し、、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![開いているコマンド ウィンドウ](migrations/_static/open-command-window.png)

<span data-ttu-id="17e2b-140">コマンド ウィンドウで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="17e2b-141">コマンド ウィンドウで次のような出力を参照してください。</span><span class="sxs-lookup"><span data-stu-id="17e2b-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="17e2b-142">エラー メッセージを表示する場合は*実行可能ファイルに一致するコマンド"dotnet-ef"が見つかりませんでした*を参照してください[このブログの投稿](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)トラブルシューティングのヘルプ。</span><span class="sxs-lookup"><span data-stu-id="17e2b-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="17e2b-143">エラー メッセージを表示する場合は"*... ファイルにアクセスできませんContosoUniversity.dll 別のプロセスによって使用されているためです。*"] し、Windows システム トレイに、IIS Express のアイコンを検索し、右クリックして、[ **ContosoUniversity > Stop サイト**です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="17e2b-144">上矢印を確認し、下矢印メソッド</span><span class="sxs-lookup"><span data-stu-id="17e2b-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="17e2b-145">実行すると、`migrations add`コマンド、EF は最初からデータベースを作成するコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="17e2b-146">このコードは、*移行*という名前のファイル内のフォルダー *\<タイムスタンプ > _InitialCreate.cs*です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="17e2b-147">`Up`のメソッド、`InitialCreate`クラスは、データ モデルのエンティティ セットに対応するデータベース テーブルを作成し、`Down`メソッドでは、それらが削除される次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="17e2b-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="17e2b-148">移行の呼び出し、`Up`移行のためのデータ モデルの変更を実装するメソッド。</span><span class="sxs-lookup"><span data-stu-id="17e2b-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="17e2b-149">更新プログラム、移行の呼び出しをロールバックするためのコマンドを入力すると、`Down`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="17e2b-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="17e2b-150">このコードの入力時に作成された最初の移行は、`migrations add InitialCreate`コマンド。</span><span class="sxs-lookup"><span data-stu-id="17e2b-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="17e2b-151">移行の name パラメーター (この例では"InitialCreate") は、ファイル名が使用され、希望することができます。</span><span class="sxs-lookup"><span data-stu-id="17e2b-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="17e2b-152">単語または語句、移行で行われている新機能をまとめたものを選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="17e2b-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="17e2b-153">たとえば、後で移行"AddDepartmentTable"の名前を付けて可能性があります。</span><span class="sxs-lookup"><span data-stu-id="17e2b-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="17e2b-154">データベースが既に存在する場合、最初の移行を作成した場合は、データベース作成コードが生成されますが、データベースに既にデータ モデルと一致するために実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="17e2b-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="17e2b-155">ここで、データベースが存在しない、このコードはまだ実行されて、データベースを作成する別の環境にアプリを展開するときにこれは最初にテストすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="17e2b-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="17e2b-156">理由です--前の接続文字列でデータベースの名前を変更した移行は、最初から新しい 1 つを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="17e2b-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="17e2b-157">データ モデルのスナップショットを確認します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-157">Examine the data model snapshot</span></span>

<span data-ttu-id="17e2b-158">また、移行を作成、*スナップショット*で現在のデータベース スキーマの*Migrations/SchoolContextModelSnapshot.cs*です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-158">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="17e2b-159">そのコードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="17e2b-159">Here's what that code looks like:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="17e2b-160">現在のデータベース スキーマは、コードで表される、ための移行を作成するデータベースとやり取りする EF コアがありません。</span><span class="sxs-lookup"><span data-stu-id="17e2b-160">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="17e2b-161">移行を追加すると、EF は、スナップショット ファイルにデータ モデルを比較することによって変更内容を判断します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-161">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="17e2b-162">EF は、データベースを更新する必要がある場合にのみ、データベースとやり取りします。</span><span class="sxs-lookup"><span data-stu-id="17e2b-162">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="17e2b-163">スナップショット ファイルが作成して、という名前のファイルを削除するだけで、移行を削除することはできませんので、マイグレーションと同期して保持するが*\<タイムスタンプ > _\<migrationname > .cs*です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-163">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="17e2b-164">そのファイルを削除すると、残りのマイグレーションは、データベース スナップショット ファイルと同期されます。</span><span class="sxs-lookup"><span data-stu-id="17e2b-164">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="17e2b-165">削除する追加した前回の移行を使用して、 [dotnet ef 移行削除](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)コマンド。</span><span class="sxs-lookup"><span data-stu-id="17e2b-165">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="17e2b-166">移行をデータベースに適用します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-166">Apply the migration to the database</span></span>

<span data-ttu-id="17e2b-167">コマンド ウィンドウで、データベースとテーブルが作成するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-167">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="17e2b-168">コマンドの出力がに似ていますが、`migrations add`を使用して、点を除いて、SQL コマンドのデータベースを設定するためにログを参照してください。</span><span class="sxs-lookup"><span data-stu-id="17e2b-168">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="17e2b-169">ログのほとんどは、次のサンプル出力で除外されます。</span><span class="sxs-lookup"><span data-stu-id="17e2b-169">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="17e2b-170">このレベルのログ メッセージで詳細を表示しないようにする場合は、ログ レベルを変更できます、 *appsettings です。Development.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="17e2b-170">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="17e2b-171">詳細については、次を参照してください。[ログ記録の概要](xref:fundamentals/logging/index)です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-171">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="17e2b-172">使用して**SQL Server オブジェクト エクスプ ローラー**の最初のチュートリアルで行ったように、データベースを調査します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-172">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="17e2b-173">移行の種類は、データベースに適用されているの追跡を __EFMigrationsHistory テーブルの追加がわかります。</span><span class="sxs-lookup"><span data-stu-id="17e2b-173">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="17e2b-174">そのテーブルにデータを表示し、1 行の最初の移行データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="17e2b-174">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="17e2b-175">(前の CLI 出力例では、最後のログは、この行が作成される INSERT ステートメントを示しています)。</span><span class="sxs-lookup"><span data-stu-id="17e2b-175">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="17e2b-176">以前と同じを引き続き動作するすべてのものを確認するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-176">Run the application to verify that everything still works the same as before.</span></span>

![インデックス ページの受講者](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="17e2b-178">コマンド ライン インターフェイス (CLI) とします。Package Manager Console (PMC)</span><span class="sxs-lookup"><span data-stu-id="17e2b-178">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="17e2b-179">移行を管理することは .NET Core CLI コマンドとは、Visual Studio での PowerShell コマンドレットからの EF tooling **Package Manager Console** (PMC) ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="17e2b-179">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="17e2b-180">このチュートリアルは、CLI を使用する方法を示しますが、したい場合は、PMC を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="17e2b-180">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="17e2b-181">PMC コマンドの EF コマンドが、 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="17e2b-181">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="17e2b-182">このパッケージが既に含まれている、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage、ためをインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="17e2b-182">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="17e2b-183">**重要:**を編集して、CLI をインストールするものと同じパッケージではない、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="17e2b-183">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="17e2b-184">この種類の名前で終わる`Tools`、終わる CLI パッケージ名とは異なり`Tools.DotNet`です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-184">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="17e2b-185">CLI コマンドの詳細については、次を参照してください。 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-185">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="17e2b-186">PMC コマンドの詳細については、次を参照してください。 [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)です。</span><span class="sxs-lookup"><span data-stu-id="17e2b-186">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="17e2b-187">まとめ</span><span class="sxs-lookup"><span data-stu-id="17e2b-187">Summary</span></span>

<span data-ttu-id="17e2b-188">このチュートリアルでは、作成して、最初の移行を適用する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="17e2b-188">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="17e2b-189">次のチュートリアルでは、データ モデルを展開してより高度なトピックを見るが始めます。</span><span class="sxs-lookup"><span data-stu-id="17e2b-189">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="17e2b-190">その過程を作成し、追加の移行を適用します。</span><span class="sxs-lookup"><span data-stu-id="17e2b-190">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="17e2b-191">[前へ](sort-filter-page.md)
[次へ](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="17e2b-191">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  