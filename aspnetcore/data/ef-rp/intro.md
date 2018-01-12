---
title: "Entity Framework Core - 8 のチュートリアル 1 で razor ページ"
author: rick-anderson
description: "Entity Framework のコアを使用して、Razor ページのアプリを作成する方法を示します"
keywords: "ASP.NET Core、Entity Framework Core、チュートリアル"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: 86f9eceb5b8646e371811fa4611a4509ff652231
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="939e0-104">Razor ページと Visual Studio (1/8) を使用して Entity Framework Core の概要</span><span class="sxs-lookup"><span data-stu-id="939e0-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="939e0-105">によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="939e0-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="939e0-106">Contoso 大学サンプル web アプリケーションでは、Entity Framework (EF) コア 2.0 と Visual Studio 2017 を使用して ASP.NET Core 2.0 MVC web アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="939e0-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="939e0-107">サンプル アプリは、架空の Contoso 大学の web サイトです。</span><span class="sxs-lookup"><span data-stu-id="939e0-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="939e0-108">学生受付、コースの作成、およびインストラクター割り当てなどの機能が含まれています。</span><span class="sxs-lookup"><span data-stu-id="939e0-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="939e0-109">このページは、最初の一連の Contoso 大学サンプル アプリをビルドする方法を説明するチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="939e0-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="939e0-110">ダウンロードまたは完成したアプリを表示します。</span><span class="sxs-lookup"><span data-stu-id="939e0-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="939e0-111">[ダウンロード命令の](xref:tutorials/index#how-to-download-a-sample)します。</span><span class="sxs-lookup"><span data-stu-id="939e0-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="939e0-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="939e0-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="939e0-113">十分に理解[Razor ページ](xref:mvc/razor-pages/index)です。</span><span class="sxs-lookup"><span data-stu-id="939e0-113">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="939e0-114">新しいプログラマが完了する必要があります[Razor ページの概要](xref:tutorials/razor-pages/razor-pages-start)この系列を開始する前にします。</span><span class="sxs-lookup"><span data-stu-id="939e0-114">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="939e0-115">トラブルシューティング</span><span class="sxs-lookup"><span data-stu-id="939e0-115">Troubleshooting</span></span>

<span data-ttu-id="939e0-116">問題を解決できない場合に発生した場合、コードを比較することでのソリューションを見つけることは通常、[ステージを完了した](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)または[完成したプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final)です。</span><span class="sxs-lookup"><span data-stu-id="939e0-116">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="939e0-117">一般的なエラーとそれらを解決する方法の一覧は、次を参照してください。[系列の最後のチュートリアルの「トラブルシューティング](xref:data/ef-mvc/advanced#common-errors)です。</span><span class="sxs-lookup"><span data-stu-id="939e0-117">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="939e0-118">必要なものがない場合は、StackOverflow.com に質問を投稿することができます[ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)または[EF コア](https://stackoverflow.com/questions/tagged/entity-framework-core)です。</span><span class="sxs-lookup"><span data-stu-id="939e0-118">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="939e0-119">この一連のチュートリアルは、前のチュートリアルでの処理に基づいています。</span><span class="sxs-lookup"><span data-stu-id="939e0-119">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="939e0-120">各チュートリアルが正常に完了した後、プロジェクトのコピーを保存することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="939e0-120">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="939e0-121">問題に遭遇した場合は、前のチュートリアルの先頭に戻るとではなく、経由で開始できます。</span><span class="sxs-lookup"><span data-stu-id="939e0-121">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="939e0-122">代わりに、ダウンロードすることができます、[ステージを完了](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)し、完了した段階を使用して経由で開始します。</span><span class="sxs-lookup"><span data-stu-id="939e0-122">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="939e0-123">Contoso 大学 web アプリ</span><span class="sxs-lookup"><span data-stu-id="939e0-123">The Contoso University web app</span></span>

<span data-ttu-id="939e0-124">これらのチュートリアルでビルドされたアプリは、基本的な大学 web サイトです。</span><span class="sxs-lookup"><span data-stu-id="939e0-124">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="939e0-125">ユーザーでは、表示でき、学生、コース、インストラクターの情報を更新することができます。</span><span class="sxs-lookup"><span data-stu-id="939e0-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="939e0-126">このチュートリアルで作成した画面の一部を次に示します。</span><span class="sxs-lookup"><span data-stu-id="939e0-126">Here are a few of the screens created in the tutorial.</span></span>

![インデックス ページの受講者](intro/_static/students-index.png)

![受講者の編集 ページ](intro/_static/student-edit.png)

<span data-ttu-id="939e0-129">このサイトの UI スタイルは、組み込みのテンプレートによって生成されたものに近いこと。</span><span class="sxs-lookup"><span data-stu-id="939e0-129">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="939e0-130">チュートリアルの焦点は Razor ページ、UI ではない、EF コアです。</span><span class="sxs-lookup"><span data-stu-id="939e0-130">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="939e0-131">Razor ページの web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-131">Create a Razor Pages web app</span></span>

* <span data-ttu-id="939e0-132">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="939e0-132">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="939e0-133">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-133">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="939e0-134">プロジェクトに名前を**ContosoUniversity**です。</span><span class="sxs-lookup"><span data-stu-id="939e0-134">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="939e0-135">プロジェクトに名前をすることが重要*ContosoUniversity*コードは、コピー/貼り付けときに、名前空間に一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="939e0-135">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="939e0-136">![新しい ASP.NET Core Web アプリケーション](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="939e0-136">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="939e0-137">ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="939e0-137">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="939e0-138">![Web アプリケーション (Razor ページ)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="939e0-138">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="939e0-139">**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。</span><span class="sxs-lookup"><span data-stu-id="939e0-139">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="939e0-140">サイトのスタイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="939e0-140">Set up the site style</span></span>

<span data-ttu-id="939e0-141">いくつかの変更は、[サイト] メニューのレイアウト、およびホーム ページを設定します。</span><span class="sxs-lookup"><span data-stu-id="939e0-141">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="939e0-142">開いている*Pages/_Layout.cshtml*次の変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="939e0-142">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="939e0-143">「Contoso 大学」を"ContosoUniversity"の各出現する位置を変更します。</span><span class="sxs-lookup"><span data-stu-id="939e0-143">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="939e0-144">次の 3 つの出現があります。</span><span class="sxs-lookup"><span data-stu-id="939e0-144">There are three occurrences.</span></span>

* <span data-ttu-id="939e0-145">メニュー エントリを追加**受講者**、**コース**、**講習においてインストラクター**、および**部門**、および削除、 **にお問い合わせください**メニュー エントリです。</span><span class="sxs-lookup"><span data-stu-id="939e0-145">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="939e0-146">変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-146">The changes are highlighted.</span></span> <span data-ttu-id="939e0-147">(すべてのマークアップが*いない*が表示されます)。</span><span class="sxs-lookup"><span data-stu-id="939e0-147">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="939e0-148">*Pages/Index.cshtml*ファイルの内容をこのアプリに関するテキストで ASP.NET と MVC に関するテキストを置き換える次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="939e0-148">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="939e0-149">Ctrl キーを押しながら F5 キーを押してプロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="939e0-149">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="939e0-150">ホーム ページには、次のチュートリアルで作成されたタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-150">The home page is displayed with tabs created in the following tutorials:</span></span>

![Contoso 大学のホーム ページ](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="939e0-152">データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-152">Create the data model</span></span>

<span data-ttu-id="939e0-153">Contoso 大学アプリ用にエンティティ クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-153">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="939e0-154">次の 3 つのエンティティで開始します。</span><span class="sxs-lookup"><span data-stu-id="939e0-154">Start with the following three entities:</span></span>

![受講者コース-登録データ モデルのダイアグラム](intro/_static/data-model-diagram.png)

<span data-ttu-id="939e0-156">一対多の関係がある`Student`と`Enrollment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-156">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="939e0-157">一対多の関係がある`Course`と`Enrollment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-157">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="939e0-158">任意の数のコースに受講者を登録できます。</span><span class="sxs-lookup"><span data-stu-id="939e0-158">A student can enroll in any number of courses.</span></span> <span data-ttu-id="939e0-159">コースには、任意の数の受講者がそれに登録されていることができます。</span><span class="sxs-lookup"><span data-stu-id="939e0-159">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="939e0-160">次のセクションでは、これらのエンティティのいずれかのクラスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-160">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="939e0-161">学生エンティティ</span><span class="sxs-lookup"><span data-stu-id="939e0-161">The Student entity</span></span>

![学生のエンティティの図](intro/_static/student-entity.png)

<span data-ttu-id="939e0-163">作成、*モデル*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="939e0-163">Create a *Models* folder.</span></span> <span data-ttu-id="939e0-164">*モデル*フォルダー、という名前のクラス ファイルを作成する*Student.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="939e0-164">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="939e0-165">`ID`プロパティがこのクラスに対応するデータベース (DB) テーブルの主キー列になります。</span><span class="sxs-lookup"><span data-stu-id="939e0-165">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="939e0-166">既定では、EF Core では、解釈というプロパティ`ID`または`classnameID`主キーとして。</span><span class="sxs-lookup"><span data-stu-id="939e0-166">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="939e0-167">`Enrollments`プロパティは、ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="939e0-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="939e0-168">ナビゲーション プロパティは、このエンティティに関連する他のエンティティにリンクします。</span><span class="sxs-lookup"><span data-stu-id="939e0-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="939e0-169">ここで、`Enrollments`のプロパティ、`Student entity`すべて保持、`Enrollment`に関連付けられているエンティティ`Student`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="939e0-170">たとえば場合は、db 学生行が関連する 2 つ登録行、`Enrollments`ナビゲーション プロパティには、これら 2 つが含まれています。`Enrollment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="939e0-171">関連する`Enrollment`そのスチューデントの主キーの値を含む行が、`StudentID`列です。</span><span class="sxs-lookup"><span data-stu-id="939e0-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="939e0-172">たとえば、学生 ID を = 1 2 つの行には、`Enrollment`テーブル。</span><span class="sxs-lookup"><span data-stu-id="939e0-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="939e0-173">`Enrollment`テーブルを持つ 2 つの行には`StudentID`= 1 です。</span><span class="sxs-lookup"><span data-stu-id="939e0-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="939e0-174">`StudentID`外部キーには、`Enrollment`の受講者を指定するテーブル、`Student`テーブル。</span><span class="sxs-lookup"><span data-stu-id="939e0-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="939e0-175">ナビゲーション プロパティがリストの種類をなどでなければなりません場合は、ナビゲーション プロパティは、複数のエンティティを保持できる、`ICollection<T>`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="939e0-176">`ICollection<T>`指定できますが、またはなど型`List<T>`または`HashSet<T>`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="939e0-177">ときに`ICollection<T>`は EF コアを作成、使用、`HashSet<T>`既定のコレクション。</span><span class="sxs-lookup"><span data-stu-id="939e0-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="939e0-178">複数のエンティティを保持するナビゲーション プロパティは、多対多および一対多のリレーションシップから取得されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="939e0-179">登録エンティティ</span><span class="sxs-lookup"><span data-stu-id="939e0-179">The Enrollment entity</span></span>

![エンティティの登録の図](intro/_static/enrollment-entity.png)

<span data-ttu-id="939e0-181">*モデル*フォルダー作成*Enrollment.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="939e0-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="939e0-182">`EnrollmentID`プロパティは、主キー。</span><span class="sxs-lookup"><span data-stu-id="939e0-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="939e0-183">このエンティティを使用して、`classnameID`パターンの代わりに`ID`と同様に、`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="939e0-184">通常の開発者は、1 つのパターンを選択し、データ モデル全体で使用します。</span><span class="sxs-lookup"><span data-stu-id="939e0-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="939e0-185">チュートリアルでは、後で、classname せず ID を使用するが、データ モデルで継承の実装を容易にできるように表示されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="939e0-186">`Grade`プロパティは、`enum`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="939e0-187">後に疑問符 ()、`Grade`型宣言では、ことを示します、`Grade`プロパティが null 値を許容します。</span><span class="sxs-lookup"><span data-stu-id="939e0-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="939e0-188">Null である評価とは異なる、ゼロ グレード--null またはため、評価はまだ割り当てられていません。</span><span class="sxs-lookup"><span data-stu-id="939e0-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="939e0-189">`StudentID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Student`します。</span><span class="sxs-lookup"><span data-stu-id="939e0-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="939e0-190">`Enrollment`エンティティが 1 つに関連付けられた`Student`エンティティ、プロパティには、1 つが含まれるように`Student`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="939e0-191">`Student`エンティティとは異なります、`Student.Enrollments`ナビゲーション プロパティは、複数含まれている`Enrollment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="939e0-192">`CourseID`プロパティは、foreign key、および対応するナビゲーション プロパティは`Course`します。</span><span class="sxs-lookup"><span data-stu-id="939e0-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="939e0-193">`Enrollment`エンティティが 1 つに関連付けられた`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="939e0-194">という名前が場合、外部キーとして解釈されるプロパティを EF コア`<navigation property name><primary key property name>`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="939e0-195">たとえば、`StudentID`の`Student`ナビゲーション プロパティから、`Student`エンティティの主キーが`ID`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="939e0-196">外部キーのプロパティとも呼ばれますできます`<primary key property name>`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="939e0-197">たとえば、`CourseID`ので、`Course`エンティティの主キーが`CourseID`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="939e0-198">Course エンティティ</span><span class="sxs-lookup"><span data-stu-id="939e0-198">The Course entity</span></span>

![Course エンティティの図](intro/_static/course-entity.png)

<span data-ttu-id="939e0-200">*モデル*フォルダー作成*Course.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="939e0-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="939e0-201">`Enrollments`プロパティは、ナビゲーション プロパティ。</span><span class="sxs-lookup"><span data-stu-id="939e0-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="939e0-202">A`Course`エンティティを任意の数に関連付けることができます`Enrollment`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="939e0-203">`DatabaseGenerated` DB 生成ことのではなく、属性を使用して、アプリケーションで主キーを指定します。</span><span class="sxs-lookup"><span data-stu-id="939e0-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="939e0-204">SchoolContext DB コンテキストを作成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="939e0-205">指定されたデータ モデルの EF のコア機能を調整するのメイン クラスは、DB コンテキスト クラスです。</span><span class="sxs-lookup"><span data-stu-id="939e0-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="939e0-206">データ コンテキストがから派生した`Microsoft.EntityFrameworkCore.DbContext`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="939e0-207">データ コンテキストでは、データ モデルのエンティティが含まれているを指定します。</span><span class="sxs-lookup"><span data-stu-id="939e0-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="939e0-208">クラスの名前は、このプロジェクトで`SchoolContext`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="939e0-209">プロジェクト フォルダー内には、という名前のフォルダーを作成*データ*です。</span><span class="sxs-lookup"><span data-stu-id="939e0-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="939e0-210">*データ*フォルダー作成*SchoolContext.cs*を次のコード。</span><span class="sxs-lookup"><span data-stu-id="939e0-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="939e0-211">このコードを作成、`DbSet`各エンティティ セットのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="939e0-212">EF コア用語では。</span><span class="sxs-lookup"><span data-stu-id="939e0-212">In EF Core terminology:</span></span>

* <span data-ttu-id="939e0-213">エンティティ セットは、通常は、DB テーブルに対応します。</span><span class="sxs-lookup"><span data-stu-id="939e0-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="939e0-214">エンティティは、テーブル内の行に対応します。</span><span class="sxs-lookup"><span data-stu-id="939e0-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="939e0-215">`DbSet<Enrollment>`および`DbSet<Course>`を省略できます。</span><span class="sxs-lookup"><span data-stu-id="939e0-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="939e0-216">EF コアそれらを含む暗黙的にあるため、`Student`エンティティ参照、`Enrollment`エンティティ、および`Enrollment`エンティティ参照、`Course`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="939e0-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="939e0-217">このチュートリアルでは、このまま`DbSet<Enrollment>`と`DbSet<Course>`で、`SchoolContext`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="939e0-218">データベースが作成される、EF コア作成と同じ名前を持つテーブルを`DbSet`プロパティの名前。</span><span class="sxs-lookup"><span data-stu-id="939e0-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="939e0-219">コレクションのプロパティ名は通常複数形 (受講者ではなく学生)。</span><span class="sxs-lookup"><span data-stu-id="939e0-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="939e0-220">テーブル名が複数あるかどうかは開発者が一致しません。</span><span class="sxs-lookup"><span data-stu-id="939e0-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="939e0-221">これらのチュートリアルについては、既定の動作は、DbContext で単数形のテーブル名を指定することによってオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="939e0-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="939e0-222">単数形のテーブル名を指定するには、次の強調表示されたコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="939e0-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="939e0-223">依存関係の挿入をコンテキストに登録します。</span><span class="sxs-lookup"><span data-stu-id="939e0-223">Register the context with dependency injection</span></span>

<span data-ttu-id="939e0-224">ASP.NET Core を含む[依存性の注入](xref:fundamentals/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="939e0-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="939e0-225">EF コア DB コンテキスト) などのサービスは、アプリケーションの起動時に依存関係の挿入に登録されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="939e0-226">これらのサービス (Razor ページなど) を必要とするコンポーネントには、コンス トラクターのパラメーターを使用してこれらのサービスが用意されています。</span><span class="sxs-lookup"><span data-stu-id="939e0-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="939e0-227">このチュートリアルで後ほど db コンテキストのインスタンスを取得するコンス トラクター コードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="939e0-228">登録する`SchoolContext`をサービスとして開く*Startup.cs*を強調表示された行を追加し、`ConfigureServices`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="939e0-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="939e0-229">接続文字列の名前によって渡されるコンテキストでメソッドを呼び出す、`DbContextOptionsBuilder`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="939e0-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="939e0-230">ローカルの開発、 [ASP.NET Core 構成システム](xref:fundamentals/configuration/index)から接続文字列を読み取り、*される appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="939e0-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="939e0-231">追加`using`ステートメント`ContosoUniversity.Data`と`Microsoft.EntityFrameworkCore`名前空間。</span><span class="sxs-lookup"><span data-stu-id="939e0-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="939e0-232">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="939e0-232">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="939e0-233">開く、*される appsettings.json*ファイルし、次のコードに示すように、接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="939e0-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="939e0-234">上記の接続文字列を使用して`ConnectRetryCount=0`を防ぐために[SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework)ハングアップします。</span><span class="sxs-lookup"><span data-stu-id="939e0-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="939e0-235">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="939e0-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="939e0-236">接続文字列では、SQL Server LocalDB データベースを指定します。</span><span class="sxs-lookup"><span data-stu-id="939e0-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="939e0-237">LocalDB は、SQL Server Express データベース エンジンの簡易バージョンがあり、アプリの開発では、実稼働環境を使用しないものです。</span><span class="sxs-lookup"><span data-stu-id="939e0-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="939e0-238">LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="939e0-238">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="939e0-239">既定では、LocalDB が作成されます*.mdf* DB ファイル、`C:/Users/<user>`ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="939e0-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="939e0-240">テスト データを使用して DB に初期化するコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="939e0-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="939e0-241">EF Core では、空のデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="939e0-242">このセクションで、*シード*メソッドは、テスト データを設定に書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="939e0-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="939e0-243">*データ*フォルダー、という名前の新しいクラス ファイルを作成*DbInitializer.cs*し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="939e0-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="939e0-244">コードでは、db では、任意の受講者があるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="939e0-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="939e0-245">Db に受講者がしない場合にテスト データ、DB がシード処理します。</span><span class="sxs-lookup"><span data-stu-id="939e0-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="939e0-246">テスト データを配列に読み込むのではなく`List<T>`パフォーマンスを最適化するコレクション。</span><span class="sxs-lookup"><span data-stu-id="939e0-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="939e0-247">`EnsureCreated`メソッドは、DB の DB コンテキストを自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="939e0-248">DB が存在する場合、 `EnsureCreated` DB を変更することがなくを返します。</span><span class="sxs-lookup"><span data-stu-id="939e0-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="939e0-249">*Program.cs*、変更、`Main`メソッドを次の操作します。</span><span class="sxs-lookup"><span data-stu-id="939e0-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="939e0-250">依存関係性の注入コンテナーからの DB コンテキストのインスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="939e0-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="939e0-251">コンテキストを引き渡しシード メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="939e0-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="939e0-252">Seed メソッドの完了時に、コンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="939e0-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="939e0-253">次のコードは、更新された*Program.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="939e0-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="939e0-254">初めてのアプリを実行すると、DB が作成され、テスト データのシード処理します。</span><span class="sxs-lookup"><span data-stu-id="939e0-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="939e0-255">データ モデルが更新されたとき。</span><span class="sxs-lookup"><span data-stu-id="939e0-255">When the data model is updated:</span></span>
* <span data-ttu-id="939e0-256">データベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="939e0-256">Delete the DB.</span></span>
* <span data-ttu-id="939e0-257">Seed メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="939e0-257">Update the seed method.</span></span>
* <span data-ttu-id="939e0-258">アプリを実行して、新しいシード DB を作成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="939e0-259">以降のチュートリアルでは、データ モデルの変更、削除してデータベースを再作成するときに、DB が更新されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="939e0-260">Scaffold ツールを追加します。</span><span class="sxs-lookup"><span data-stu-id="939e0-260">Add scaffold tooling</span></span>

<span data-ttu-id="939e0-261">このセクションでは、パッケージ マネージャー コンソール (PMC) を使用して、Visual Studio web コード生成のパッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="939e0-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="939e0-262">スキャフォールディング エンジンを実行するには、このパッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="939e0-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="939e0-263">**[ツール]** メニューで、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="939e0-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="939e0-264">パッケージ マネージャー コンソール (PMC) では、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="939e0-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="939e0-265">前のコマンドは、\*.csproj ファイルには NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="939e0-265">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="939e0-266">Scaffold モデル</span><span class="sxs-lookup"><span data-stu-id="939e0-266">Scaffold the model</span></span>

* <span data-ttu-id="939e0-267">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="939e0-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="939e0-268">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="939e0-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="939e0-269">場合は、次のエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-269">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="939e0-270">コマンドを再実行し、ページの下部にあるコメントのままにします。</span><span class="sxs-lookup"><span data-stu-id="939e0-270">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="939e0-271">エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="939e0-271">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="939e0-272">プロジェクト ディレクトリ (*Program.cs*、*Startup.cs*、および *.csproj* ファイルを含むディレクトリ) でコマンド ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="939e0-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="939e0-273">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="939e0-273">Build the project.</span></span> <span data-ttu-id="939e0-274">ビルドには、次のようなエラーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-274">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="939e0-275">グローバルに変更する`_context.Student`に`_context.Students`(つまり、"s"を追加する`Student`)。</span><span class="sxs-lookup"><span data-stu-id="939e0-275">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="939e0-276">7 回の出現が検出され、更新します。</span><span class="sxs-lookup"><span data-stu-id="939e0-276">7 occurrences are found and updated.</span></span> <span data-ttu-id="939e0-277">修正される予定[この不具合](https://github.com/aspnet/Scaffolding/issues/633)次のリリースでします。</span><span class="sxs-lookup"><span data-stu-id="939e0-277">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="939e0-278">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="939e0-278">Test the app</span></span>

<span data-ttu-id="939e0-279">アプリの実行を選択して、**受講者**リンクします。</span><span class="sxs-lookup"><span data-stu-id="939e0-279">Run the app and select the **Students** link.</span></span> <span data-ttu-id="939e0-280">ブラウザーの幅に応じて、**受講者**リンクは、ページの上部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-280">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="939e0-281">場合、**受講者**リンクが表示されない、右上隅で、ナビゲーション アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="939e0-281">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Contoso 大学のホーム ページの幅の狭い](intro/_static/home-page-narrow.png)

<span data-ttu-id="939e0-283">テスト、**作成**、**編集**、および**詳細**リンクします。</span><span class="sxs-lookup"><span data-stu-id="939e0-283">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="939e0-284">DB を表示します。</span><span class="sxs-lookup"><span data-stu-id="939e0-284">View the DB</span></span>

<span data-ttu-id="939e0-285">アプリが開始されると、`DbInitializer.Initialize`呼び出し`EnsureCreated`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-285">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="939e0-286">`EnsureCreated`かどうか DB が存在し、必要な場合は作成を検出します。</span><span class="sxs-lookup"><span data-stu-id="939e0-286">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="939e0-287">DB に受講者が存在しない場合、`Initialize`メソッドは、受講者を追加します。</span><span class="sxs-lookup"><span data-stu-id="939e0-287">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="939e0-288">開いている**SQL Server オブジェクト エクスプ ローラー** (SSOX) から、**ビュー** Visual Studio のメニュー。</span><span class="sxs-lookup"><span data-stu-id="939e0-288">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="939e0-289">SSOX、クリックして**(localdb) \MSSQLLocalDB > データベース > ContosoUniversity1**です。</span><span class="sxs-lookup"><span data-stu-id="939e0-289">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="939e0-290">展開して、**テーブル**ノード。</span><span class="sxs-lookup"><span data-stu-id="939e0-290">Expand the **Tables** node.</span></span>

<span data-ttu-id="939e0-291">右クリックし、**学生**テーブルし、をクリックして**データの表示**に作成される列とテーブルに挿入された行を参照してください。</span><span class="sxs-lookup"><span data-stu-id="939e0-291">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="939e0-292">*.Mdf*と*.ldf* DB のファイルは、 *C:\Users\\ <yourusername>* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="939e0-292">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="939e0-293">`EnsureCreated`アプリの起動は、により、次の作業フローに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-293">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="939e0-294">データベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="939e0-294">Delete the DB.</span></span>
* <span data-ttu-id="939e0-295">DB のスキーマを変更 (たとえば、追加、`EmailAddress`フィールド)。</span><span class="sxs-lookup"><span data-stu-id="939e0-295">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="939e0-296">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="939e0-296">Run the app.</span></span>

<span data-ttu-id="939e0-297">`EnsureCreated`使用して DB を作成、`EmailAddress`列です。</span><span class="sxs-lookup"><span data-stu-id="939e0-297">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="939e0-298">規約</span><span class="sxs-lookup"><span data-stu-id="939e0-298">Conventions</span></span>

<span data-ttu-id="939e0-299">EF のコアが完全なデータベースを作成するために記述されたコードの量は、規則、または EF コアは、前提を使用するためは最小限です。</span><span class="sxs-lookup"><span data-stu-id="939e0-299">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="939e0-300">名前`DbSet`プロパティ テーブル名として使用されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-300">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="939e0-301">参照されていないエンティティに対して、`DbSet`プロパティ、エンティティ クラスの名前テーブル名として使用されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-301">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="939e0-302">エンティティのプロパティ名は、列名に使用されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-302">Entity property names are used for column names.</span></span>

* <span data-ttu-id="939e0-303">ID または classnameID という名前はエンティティのプロパティは、主キー プロパティとして認識されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-303">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="939e0-304">という名前が場合、プロパティが外部キーのプロパティとして解釈されます *<navigation property name> <primary key property name>*  (たとえば、`StudentID`の`Student`以降のナビゲーション プロパティ、`Student`エンティティの主キーとは`ID`).</span><span class="sxs-lookup"><span data-stu-id="939e0-304">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="939e0-305">外部キーのプロパティの名前を指定できます *<primary key property name>*  (たとえば、`EnrollmentID`ので、`Enrollment`エンティティの主キーが`EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="939e0-305">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="939e0-306">従来の動作をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="939e0-306">Conventional behavior can be overridden.</span></span> <span data-ttu-id="939e0-307">たとえば、テーブル名できます明示的に指定する、このチュートリアルで前述したとおりです。</span><span class="sxs-lookup"><span data-stu-id="939e0-307">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="939e0-308">列名を明示的に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="939e0-308">The column names can be explicitly set.</span></span> <span data-ttu-id="939e0-309">主キーと外部キーは、明示的に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="939e0-309">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="939e0-310">非同期コード</span><span class="sxs-lookup"><span data-stu-id="939e0-310">Asynchronous code</span></span>

<span data-ttu-id="939e0-311">非同期プログラミングは、ASP.NET Core と EF コアの既定モードです。</span><span class="sxs-lookup"><span data-stu-id="939e0-311">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="939e0-312">Web サーバーは、使用可能なスレッド数を限定を持ち、負荷が高い状況でのすべての利用可能なスレッドがありますで使用します。</span><span class="sxs-lookup"><span data-stu-id="939e0-312">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="939e0-313">そのような場合は、サーバーは、スレッドが解放されるまで新しい要求を処理できません。</span><span class="sxs-lookup"><span data-stu-id="939e0-313">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="939e0-314">同期コードはの I/O 完了を待機しているため、実際には、作業の実行されない中に多数のスレッド関連付ける可能性があります。</span><span class="sxs-lookup"><span data-stu-id="939e0-314">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="939e0-315">非同期コードは、プロセスが完了するには I/O の待機している場合、他の要求を処理するために使用するサーバー用に、スレッドが解放されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-315">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="939e0-316">その結果、非同期コード サーバー リソースをより効率的に使用でき、遅延なしのより多くのトラフィックを処理するサーバーが有効になっています。</span><span class="sxs-lookup"><span data-stu-id="939e0-316">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="939e0-317">非同期コードには、実行時に少量のオーバーヘッドがについて説明します。</span><span class="sxs-lookup"><span data-stu-id="939e0-317">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="939e0-318">トラフィックが少ない場合は、パフォーマンスの低下はごくわずかであり、中に大量のトラフィックの場合、潜在的なパフォーマンスの向上は大きなです。</span><span class="sxs-lookup"><span data-stu-id="939e0-318">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="939e0-319">次のコードで、`async`キーワード、`Task<T>`値を返す`await`キーワード、および`ToListAsync`メソッドが非同期的に実行するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-319">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="939e0-320">`async`キーワードようコンパイラに指示します。</span><span class="sxs-lookup"><span data-stu-id="939e0-320">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="939e0-321">メソッドの本体の各部分のコールバックを生成します。</span><span class="sxs-lookup"><span data-stu-id="939e0-321">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="939e0-322">自動的に作成、[タスク](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7)返されるオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="939e0-322">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="939e0-323">詳細については、次を参照してください。[タスクの戻り値の型](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)です。</span><span class="sxs-lookup"><span data-stu-id="939e0-323">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="939e0-324">暗黙の型を返す`Task`進行中の作業を表します。</span><span class="sxs-lookup"><span data-stu-id="939e0-324">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="939e0-325">`await`キーワードによって、コンパイラにメソッドを 2 つの部分に分割します。</span><span class="sxs-lookup"><span data-stu-id="939e0-325">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="939e0-326">最初の部分は、非同期的に開始される操作を終了します。</span><span class="sxs-lookup"><span data-stu-id="939e0-326">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="939e0-327">2 番目の部分は、操作が完了したときに呼び出されるコールバック メソッドに配置されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-327">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="939e0-328">`ToListAsync`非同期バージョンの`ToList`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="939e0-328">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="939e0-329">EF コアを使用する非同期コードを記述する場合の注意すべき点がいくつか:</span><span class="sxs-lookup"><span data-stu-id="939e0-329">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="939e0-330">クエリまたはコマンドのデータベースに送信されるステートメントだけが非同期的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="939e0-330">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="939e0-331">含む`ToListAsync`、 `SingleOrDefaultAsync`、 `FirstOrDefaultAsync`、および`SaveChangesAsync`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-331">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="939e0-332">だけを変更するステートメントを含まない、`IQueryable`など`var students = context.Students.Where(s => s.LastName == "Davolio")`です。</span><span class="sxs-lookup"><span data-stu-id="939e0-332">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="939e0-333">EF コア コンテキストを安全なスレッド化されていない: を並列で複数の操作を行うにはしないでください。</span><span class="sxs-lookup"><span data-stu-id="939e0-333">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="939e0-334">非同期コードのパフォーマンスの利点を利用するには、DB にクエリを送信する EF コア メソッドを呼び出す場合に、ライブラリ パッケージ (ページングなど) で非同期を使用することを確認します。</span><span class="sxs-lookup"><span data-stu-id="939e0-334">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="939e0-335">.NET における非同期プログラミングの詳細については、次を参照してください。 [Async 概要](https://docs.microsoft.com/dotnet/articles/standard/async)です。</span><span class="sxs-lookup"><span data-stu-id="939e0-335">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="939e0-336">チュートリアルでは、[次へ]、基本的な CRUD (作成、読み取り、更新、削除) の操作について説明します。</span><span class="sxs-lookup"><span data-stu-id="939e0-336">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="939e0-337">次へ</span><span class="sxs-lookup"><span data-stu-id="939e0-337">Next</span></span>](xref:data/ef-rp/crud)