---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: "イテレーション 2 – は、検索 nice (c#) アプリケーションを作成する |。Microsoft ドキュメント"
author: microsoft
description: "このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 10379f5321773155aaff4c384d8e0716d7e0e874
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="iteration-2--make-the-application-look-nice-c"></a><span data-ttu-id="fee3d-103">イテレーション 2 – は、検索 nice (c#) アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-103">Iteration #2 – Make the application look nice (C#)</span></span>
====================
<span data-ttu-id="fee3d-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fee3d-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="fee3d-105">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="fee3d-105">Download Code</span></span>](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> <span data-ttu-id="fee3d-106">このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。</span><span class="sxs-lookup"><span data-stu-id="fee3d-106">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a><span data-ttu-id="fee3d-107">連絡先管理 ASP.NET MVC アプリケーション (c#) の構築</span><span class="sxs-lookup"><span data-stu-id="fee3d-107">Building a Contact Management ASP.NET MVC Application (C#)</span></span>
  

<span data-ttu-id="fee3d-108">この一連のチュートリアルは、連絡先管理アプリケーション全体が開始されてから完了するを構築します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-108">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="fee3d-109">お問い合わせのマネージャー アプリケーションでは、人のユーザーの一覧については使用すると、連絡先情報の名前、電話番号、電子メール アドレスを格納できます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-109">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="fee3d-110">私たちは、複数のイテレーションにおける、アプリケーションを構築します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-110">We build the application over multiple iterations.</span></span> <span data-ttu-id="fee3d-111">各イテレーションで、アプリケーション、徐々 に向上します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-111">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="fee3d-112">この複数のイテレーション アプローチの目的は、各変更の理由を理解するためです。</span><span class="sxs-lookup"><span data-stu-id="fee3d-112">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="fee3d-113">イテレーション 1 には、アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-113">Iteration #1 - Create the application.</span></span> <span data-ttu-id="fee3d-114">最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-114">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="fee3d-115">基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="fee3d-115">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="fee3d-116">イテレーション 2 では、素敵に見えるアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-116">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="fee3d-117">このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。</span><span class="sxs-lookup"><span data-stu-id="fee3d-117">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="fee3d-118">イテレーション 3 - フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-118">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="fee3d-119">3 番目のイテレーションは、基本フォーム検証を追加します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-119">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="fee3d-120">おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。</span><span class="sxs-lookup"><span data-stu-id="fee3d-120">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="fee3d-121">私たちも電子メール アドレスと電話番号を検証します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-121">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="fee3d-122">4: イテレーションは、疎結合アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-122">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="fee3d-123">この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。</span><span class="sxs-lookup"><span data-stu-id="fee3d-123">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="fee3d-124">たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。</span><span class="sxs-lookup"><span data-stu-id="fee3d-124">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="fee3d-125">イテレーション #5 - 単体テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-125">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="fee3d-126">5 番目のイテレーションでおやすく、アプリケーションを維持し、単体テストを追加して変更できます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-126">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="fee3d-127">データ モデル クラスを模擬表示し、コント ローラーと検証ロジックの単体テストをビルドします。</span><span class="sxs-lookup"><span data-stu-id="fee3d-127">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="fee3d-128">イテレーション 6 - テスト駆動開発を使用します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-128">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="fee3d-129">この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-129">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="fee3d-130">このイテレーションは、連絡先グループを追加します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-130">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="fee3d-131">イテレーション #7 - Ajax 機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-131">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="fee3d-132">7 番目のイテレーションでお、応答性およびパフォーマンスの向上、アプリケーションの Ajax のサポートを追加することで。</span><span class="sxs-lookup"><span data-stu-id="fee3d-132">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>

## <a name="this-iteration"></a><span data-ttu-id="fee3d-133">このイテレーション</span><span class="sxs-lookup"><span data-stu-id="fee3d-133">This Iteration</span></span>

<span data-ttu-id="fee3d-134">このイテレーションの目標は、連絡先のマネージャー アプリケーションの外観を向上させるためにです。</span><span class="sxs-lookup"><span data-stu-id="fee3d-134">The goal of this iteration is to improve the appearance of the Contact Manager application.</span></span> <span data-ttu-id="fee3d-135">現時点では、連絡先のマネージャーは、(図 1 を参照してください) の既定の ASP.NET MVC ビュー マスター ページおよびカスケード スタイル シートを使用します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-135">Currently, the Contact Manager uses the default ASP.NET MVC view master page and cascading style sheet (see Figure 1).</span></span> <span data-ttu-id="fee3d-136">これらがない表示が不良ですが、他のすべての ASP.NET MVC web サイトと同じように検索する連絡先のマネージャーたくないです。</span><span class="sxs-lookup"><span data-stu-id="fee3d-136">These don t look bad, but I don t want the Contact Manager to look just like every other ASP.NET MVC website.</span></span> <span data-ttu-id="fee3d-137">これらのファイルをカスタムのファイルに置き換える必要があります。</span><span class="sxs-lookup"><span data-stu-id="fee3d-137">I want to replace these files with custom files.</span></span>


<span data-ttu-id="fee3d-138">[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fee3d-138">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)</span></span>

<span data-ttu-id="fee3d-139">**図 01**: ASP.NET MVC アプリケーションの既定の外観 ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="fee3d-139">**Figure 01**: The default appearance of an ASP.NET MVC Application ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image2.png))</span></span>


<span data-ttu-id="fee3d-140">このイテレーションでは、アプリケーションのビジュアル デ ザインを向上させるために 2 つの方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-140">In this iteration, I discuss two approaches to improving the visual design of our application.</span></span> <span data-ttu-id="fee3d-141">最初を表示する空き ASP.NET MVC デザイン テンプレートをダウンロードする ASP.NET MVC デザイン ギャラリーを活用する方法です。</span><span class="sxs-lookup"><span data-stu-id="fee3d-141">First, I show you how to take advantage of the ASP.NET MVC Design gallery to download a free ASP.NET MVC design template.</span></span> <span data-ttu-id="fee3d-142">ASP.NET MVC デザイン ギャラリーでは、作業なしプロフェッショナル向けの web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-142">The ASP.NET MVC Design gallery enables you to create a professional web application without doing any work.</span></span>

<span data-ttu-id="fee3d-143">連絡先のマネージャー アプリケーションの ASP.NET MVC デザイン ギャラリーから、テンプレートを使用することにしました。</span><span class="sxs-lookup"><span data-stu-id="fee3d-143">I decided to not use a template from the ASP.NET MVC Design gallery for the Contact Manager application.</span></span> <span data-ttu-id="fee3d-144">代わりに、プロフェッショナルなデザイン会社によって作成されたカスタム デザインがありました。</span><span class="sxs-lookup"><span data-stu-id="fee3d-144">Instead, I had a custom design created by a professional design firm.</span></span> <span data-ttu-id="fee3d-145">このチュートリアルの 2 番目の部分で ASP.NET MVC の最終的な設計を作成するプロフェッショナルなデザイン会社と提携方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-145">In the second part of this tutorial, I explain how I worked with a professional design company to create the final ASP.NET MVC design.</span></span>

## <a name="the-aspnet-mvc-design-gallery"></a><span data-ttu-id="fee3d-146">ASP.NET MVC デザイン ギャラリー</span><span class="sxs-lookup"><span data-stu-id="fee3d-146">The ASP.NET MVC Design Gallery</span></span>

<span data-ttu-id="fee3d-147">ASP.NET MVC デザイン ギャラリーは、Microsoft によって提供される無料リソースです。</span><span class="sxs-lookup"><span data-stu-id="fee3d-147">The ASP.NET MVC Design Gallery is a free resource provided by Microsoft.</span></span> <span data-ttu-id="fee3d-148">ASP.NET MVC ギャラリーは、次のアドレスです。</span><span class="sxs-lookup"><span data-stu-id="fee3d-148">The ASP.NET MVC Gallery is located at the following address:</span></span>

[<span data-ttu-id="fee3d-149">https://www.asp.net/mvc/gallery</span><span class="sxs-lookup"><span data-stu-id="fee3d-149">https://www.asp.net/mvc/gallery</span></span>](https://www.asp.net/mvc/gallery)

<span data-ttu-id="fee3d-150">ASP.NET MVC デザイン ギャラリーは、ASP.NET MVC プロジェクトで使用するためには、具体的に作成された無料の web サイトの設計のコレクションをホストします。</span><span class="sxs-lookup"><span data-stu-id="fee3d-150">The ASP.NET MVC Design Gallery hosts a collection of free website designs that were created specifically for using in an ASP.NET MVC project.</span></span> <span data-ttu-id="fee3d-151">設計は、コミュニティのメンバーでアップロードされます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-151">Designs are uploaded by members of the community.</span></span> <span data-ttu-id="fee3d-152">ギャラリーへの訪問者が投票できる、お気に入りの設計 (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="fee3d-152">Visitors to the Gallery can vote for their favorite designs (see Figure 2).</span></span>


<span data-ttu-id="fee3d-153">[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fee3d-153">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)</span></span>

<span data-ttu-id="fee3d-154">**図 02**: ASP.NET MVC デザイン ギャラリー ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="fee3d-154">**Figure 02**: The ASP.NET MVC Design Gallery ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image4.png))</span></span>


<span data-ttu-id="fee3d-155">このチュートリアルを書いてギャラリーで最も一般的なデザイン、年 10 月を付けた David Hauser 設計です。</span><span class="sxs-lookup"><span data-stu-id="fee3d-155">As I write this tutorial, the most popular design in the gallery is a design named October by David Hauser.</span></span> <span data-ttu-id="fee3d-156">ASP.NET MVC プロジェクトのこの設計を使用するには、次の手順を完了します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-156">You can use this design for an ASP.NET MVC project by completing the following steps:</span></span>

1. <span data-ttu-id="fee3d-157">クリックして、**ダウンロード**October.zip ファイルをコンピューターにダウンロードするボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fee3d-157">Click the **Download** button to download the October.zip file to your computer.</span></span>
2. <span data-ttu-id="fee3d-158">ダウンロードした October.zip ファイルを右クリックし、をクリックして、**ブロックを解除する**(図 3 を参照してください) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fee3d-158">Right-click the downloaded October.zip file and click the **Unblock** button (see Figure 3).</span></span>
3. <span data-ttu-id="fee3d-159">年 10 月をという名前のフォルダーにファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-159">Unzip the file to a folder named October.</span></span>
4. <span data-ttu-id="fee3d-160">年 10 月フォルダーに含まれているレイアウト フォルダーからすべてのファイルを選択し、ファイルを右クリックし、メニュー オプションを選択**コピー**です。</span><span class="sxs-lookup"><span data-stu-id="fee3d-160">Select all of the files from the DesignTemplate folder contained in the October folder, right-click the files, and select the menu option **Copy**.</span></span>
5. <span data-ttu-id="fee3d-161">Visual Studio ソリューション エクスプ ローラー ウィンドウで ContactManager プロジェクト ノードを右クリックし、メニュー オプションを選択**貼り付け**(図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="fee3d-161">Right-click the ContactManager project node in the Visual Studio Solution Explorer window and select the menu option **Paste** (see Figure 4).</span></span>
6. <span data-ttu-id="fee3d-162">Visual Studio のメニュー オプションを選択**編集、検索し、置換 クイック置換**と置換*[MyProjectName]*で*ContactManager* (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="fee3d-162">Select the Visual Studio menu option **Edit, Find and Replace, Quick Replace** and replace *[MyProjectName]* with *ContactManager* (see Figure 5).</span></span>


<span data-ttu-id="fee3d-163">[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fee3d-163">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)</span></span>

<span data-ttu-id="fee3d-164">**図 03**: web からダウンロードしたファイルのブロックを解除 ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fee3d-164">**Figure 03**: Unblocking a file downloaded from the web ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image6.png))</span></span>


<span data-ttu-id="fee3d-165">[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="fee3d-165">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)</span></span>

<span data-ttu-id="fee3d-166">**図 04**: ソリューション エクスプ ローラーでファイルを上書きする ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="fee3d-166">**Figure 04**: Overwriting files in the Solution Explorer ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image8.png))</span></span>


<span data-ttu-id="fee3d-167">[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="fee3d-167">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)</span></span>

<span data-ttu-id="fee3d-168">**図 05**: [プロジェクト名] に置き換えて ContactManager ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="fee3d-168">**Figure 05**: Replacing [ProjectName] with ContactManager ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image10.png))</span></span>


<span data-ttu-id="fee3d-169">次の手順を完了すると、web アプリケーションは、新しいデザインを使用します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-169">After you complete these steps, your web application will use the new design.</span></span> <span data-ttu-id="fee3d-170">図 6 内のページは、年 10 月の設計と連絡先のマネージャー アプリケーションの外観を示しています。</span><span class="sxs-lookup"><span data-stu-id="fee3d-170">The page in Figure 6 illustrates the appearance of the Contact Manager application with the October design.</span></span>


<span data-ttu-id="fee3d-171">[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="fee3d-171">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)</span></span>

<span data-ttu-id="fee3d-172">**図 06**: 年 10 月テンプレートを使用して ContactManager ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="fee3d-172">**Figure 06**: ContactManager with the October template ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image12.png))</span></span>


## <a name="creating-a-custom-aspnet-mvc-design"></a><span data-ttu-id="fee3d-173">カスタム ASP.NET MVC の設計の作成</span><span class="sxs-lookup"><span data-stu-id="fee3d-173">Creating a Custom ASP.NET MVC Design</span></span>

<span data-ttu-id="fee3d-174">ASP.NET MVC デザイン ギャラリーには、さまざまなデザイン スタイルの適切な選択があります。</span><span class="sxs-lookup"><span data-stu-id="fee3d-174">The ASP.NET MVC Design Gallery has a good selection of different design styles.</span></span> <span data-ttu-id="fee3d-175">ギャラリーでは、ASP.NET MVC アプリケーションの外観をカスタマイズする最も簡単に提供します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-175">The Gallery provides you with a painless way to customize the appearance of your ASP.NET MVC applications.</span></span> <span data-ttu-id="fee3d-176">また、もちろん、ギャラリーには、完全に解放されるの大きな利点。</span><span class="sxs-lookup"><span data-stu-id="fee3d-176">And, of course, the Gallery has the big advantage of being completely free.</span></span>

<span data-ttu-id="fee3d-177">ただし、web サイトの完全に独自のデザインを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fee3d-177">However, you might need to create a completely unique design for your website.</span></span> <span data-ttu-id="fee3d-178">その場合は、意味が web サイト デザイン会社を使用します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-178">In that case, it makes sense to work with a website design company.</span></span> <span data-ttu-id="fee3d-179">連絡先のマネージャー アプリケーションの設計のためには、この方法を採用することにしました。</span><span class="sxs-lookup"><span data-stu-id="fee3d-179">I decided to take this approach for the design for the Contact Manager application.</span></span>

<span data-ttu-id="fee3d-180">私は、イテレーション 1 から連絡先マネージャーを圧縮し、デザイン会社にプロジェクトを送信します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-180">I zipped up the Contact Manager from Iteration #1 and sent the project to the design company.</span></span> <span data-ttu-id="fee3d-181">でした t 問題が発生することが (残念なことにです)、Visual Studio を所有するしなかったされません。</span><span class="sxs-lookup"><span data-stu-id="fee3d-181">They did not own Visual Studio (shame on them!), but that didn t present a problem.</span></span> <span data-ttu-id="fee3d-182">Microsoft Visual Web Developer を無料でダウンロードすることができました、 [https://www.asp.net](https://www.asp.net) web サイトと Visual Web Developer で、連絡先のマネージャー アプリケーションを開きます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-182">They were able to download Microsoft Visual Web Developer for free from the [https://www.asp.net](https://www.asp.net) website and open the Contact Manager application in Visual Web Developer.</span></span> <span data-ttu-id="fee3d-183">日数のいくつかで図 7 にデザインが生成される必要があります。</span><span class="sxs-lookup"><span data-stu-id="fee3d-183">In a couple of days, they had produced the design in Figure 7.</span></span>


<span data-ttu-id="fee3d-184">[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="fee3d-184">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)</span></span>

<span data-ttu-id="fee3d-185">**図 07**: ASP.NET MVC の連絡先のマネージャー デザイン ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="fee3d-185">**Figure 07**: The ASP.NET MVC Contact Manager Design ([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image14.png))</span></span>


<span data-ttu-id="fee3d-186">この新しい設計は、2 つのメイン ファイルで構成されている: 新しいカスケード スタイル シート ファイルと新しいビュー マスター ページ ファイルです。</span><span class="sxs-lookup"><span data-stu-id="fee3d-186">The new design consisted of two main files: a new cascading style sheet file and a new view master page file.</span></span> <span data-ttu-id="fee3d-187">ビュー マスター ページには、レイアウトと ASP.NET MVC アプリケーションでのビューの共有のコンテンツが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fee3d-187">A view master page contains the layout and shared content for views in an ASP.NET MVC application.</span></span> <span data-ttu-id="fee3d-188">たとえば、ビュー マスター ページが含まれています、ヘッダー、ナビゲーション タブおよび表示されるフッター図 7 にします。</span><span class="sxs-lookup"><span data-stu-id="fee3d-188">For example, the view master page includes the header, navigation tabs, and footer that appear in Figure 7.</span></span> <span data-ttu-id="fee3d-189">\Shared フォルダーに、Site.Master 既存ビュー マスター ページを上書きしたデザイン会社から新しい Site.Master ファイルには</span><span class="sxs-lookup"><span data-stu-id="fee3d-189">I overwrote the existing Site.Master view master page in the Views\Shared folder with the new Site.Master file from the design company,</span></span>

<span data-ttu-id="fee3d-190">デザイン会社では、新しいカスケード スタイル シートとイメージのセットも作成されます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-190">The design company also created a new cascading style sheet and set of images.</span></span> <span data-ttu-id="fee3d-191">これらの新しいファイルをコンテンツ フォルダーに配置して、既存の Site.css ファイルが上書きされました。</span><span class="sxs-lookup"><span data-stu-id="fee3d-191">I placed these new files in the Content folder and overwrote the existing Site.css file.</span></span> <span data-ttu-id="fee3d-192">すべての静的なコンテンツは、コンテンツのフォルダーに配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fee3d-192">You should place all static content in the Content folder.</span></span>

<span data-ttu-id="fee3d-193">連絡先マネージャー用の新しいデザインには編集、および連絡先を削除するイメージが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-193">Notice that the new design for the Contact Manager includes images for editing and deleting contacts.</span></span> <span data-ttu-id="fee3d-194">連絡先の HTML テーブルでは、各連絡先の横にある編集および削除イメージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-194">An Edit and Delete image appear next to each contact in the HTML table of contacts.</span></span>

<span data-ttu-id="fee3d-195">最初に、これらのリンク、HTML でレンダリングされました。次のように ActionLink() ヘルパー。</span><span class="sxs-lookup"><span data-stu-id="fee3d-195">Originally, these links that were rendered with the HTML.ActionLink() helper like this:</span></span>

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

<span data-ttu-id="fee3d-196">Html.ActionLink() メソッドは、イメージ (HTML、メソッドは、セキュリティ上の理由には、このリンク テキストをエンコード) をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="fee3d-196">The Html.ActionLink() method does not support images (the method HTML encodes the link text for security reasons).</span></span> <span data-ttu-id="fee3d-197">そのため、次のように Url.Action() への呼び出しに Html.ActionLink() への呼び出しは置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-197">Therefore, I replaced the calls to Html.ActionLink() with calls to Url.Action() like this:</span></span>

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

<span data-ttu-id="fee3d-198">Html.ActionLink() メソッドでは、HTML ハイパーリンク全体を表示します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-198">The Html.ActionLink() method renders an entire HTML hyperlink.</span></span> <span data-ttu-id="fee3d-199">Url.Action() メソッドがせず URL だけをその一方で、表示、 &lt;、&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="fee3d-199">The Url.Action() method, on the other hand, renders just the URL without the &lt;a&gt; tag.</span></span>

<span data-ttu-id="fee3d-200">さらに、新しいデザインが選択および選択されていない [両方] タブが含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="fee3d-200">Notice, furthermore, that the new design includes both selected and unselected tabs.</span></span> <span data-ttu-id="fee3d-201">たとえば、図 8 に、**の連絡先の新規作成**タブが選択されていると、**連絡先** タブが選択されていません。</span><span class="sxs-lookup"><span data-stu-id="fee3d-201">For example, in Figure 8, the **Create New Contact** tab is selected and the **My Contacts** tab is not selected.</span></span>


<span data-ttu-id="fee3d-202">[![[新しいプロジェクト] ダイアログ ボックス](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="fee3d-202">[![The New Project dialog box](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)</span></span>

<span data-ttu-id="fee3d-203">**図 08**: 選択されているし、タブの選択を解除 ([フルサイズのイメージを表示するをクリックして](iteration-2-make-the-application-look-nice-cs/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="fee3d-203">**Figure 08**: Selected and unselected tabs([Click to view full-size image](iteration-2-make-the-application-look-nice-cs/_static/image16.png))</span></span>


<span data-ttu-id="fee3d-204">選択したおよび選択されていない [両方] タブの表示をサポートするには、MenuItemHelper をという名前のカスタム HTML ヘルパーを作成しました。</span><span class="sxs-lookup"><span data-stu-id="fee3d-204">To support rendering both selected and unselected tabs, I created a custom HTML helper named the MenuItemHelper.</span></span> <span data-ttu-id="fee3d-205">このヘルパー メソッドを表示するか、 &lt;li&gt;タグまたは&lt;li クラス =「選択されている」&gt;タグは、現在のコント ローラーとアクションがヘルパーに渡されるコント ローラーとアクション名に対応しているかどうかによって異なります。</span><span class="sxs-lookup"><span data-stu-id="fee3d-205">This helper method renders either a &lt;li&gt; tag or a &lt;li class="selected"&gt; tag depending on whether the current controller and action corresponds to the controller and action name passed to the helper.</span></span> <span data-ttu-id="fee3d-206">MenuItemHelper のコードは、1 のリストに含まれます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-206">The code for the MenuItemHelper is contained in Listing 1.</span></span>

<span data-ttu-id="fee3d-207">**1 - Helpers\MenuItemHelper.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="fee3d-207">**Listing 1 - Helpers\MenuItemHelper.cs**</span></span>

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

<span data-ttu-id="fee3d-208">MenuItemHelper クラスを使用して、TagBuilder 内部的に作成する、 &lt;li&gt; HTML タグ。</span><span class="sxs-lookup"><span data-stu-id="fee3d-208">The MenuItemHelper uses the TagBuilder class internally to build the &lt;li&gt; HTML tag.</span></span> <span data-ttu-id="fee3d-209">TagBuilder クラスは、新しい HTML タグを構築する必要がある場合に使用できる非常に便利なユーティリティ クラスです。</span><span class="sxs-lookup"><span data-stu-id="fee3d-209">The TagBuilder class is a very useful utility class that you can use whenever you need to build up a new HTML tag.</span></span> <span data-ttu-id="fee3d-210">属性を追加する、CSS クラスを追加する、Id を生成および s タグを変更するメソッドが含まれていますの内部 HTML。</span><span class="sxs-lookup"><span data-stu-id="fee3d-210">It includes methods for adding attributes, adding CSS classes, generating Ids, and modifying the tag s inner HTML.</span></span>

## <a name="summary"></a><span data-ttu-id="fee3d-211">概要</span><span class="sxs-lookup"><span data-stu-id="fee3d-211">Summary</span></span>

<span data-ttu-id="fee3d-212">このイテレーションは、ASP.NET MVC アプリケーションのビジュアル デ ザインを向上します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-212">In this iteration, we improved the visual design of our ASP.NET MVC application.</span></span> <span data-ttu-id="fee3d-213">最初に、ASP.NET MVC デザイン ギャラリーに導入されています。</span><span class="sxs-lookup"><span data-stu-id="fee3d-213">First, you were introduced to the ASP.NET MVC Design Gallery.</span></span> <span data-ttu-id="fee3d-214">ASP.NET MVC アプリケーションで使用できる ASP.NET MVC デザイン ギャラリーから無料のデザインのテンプレートをダウンロードする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="fee3d-214">You learned how to download free design templates from the ASP.NET MVC Design Gallery that you can use in your ASP.NET MVC applications.</span></span>

<span data-ttu-id="fee3d-215">次に、既定のカスケード スタイル シート ファイルおよびマスター ビュー ページのファイルを変更してカスタム デザインを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-215">Next, we discussed how you can create a custom design by modifying the default cascading style sheet file and master view page file.</span></span> <span data-ttu-id="fee3d-216">新しい設計をサポートするために、連絡先のマネージャー アプリケーションに若干の変更を行いました。</span><span class="sxs-lookup"><span data-stu-id="fee3d-216">In order to support the new design, we had to make some minor changes to our Contact Manager application.</span></span> <span data-ttu-id="fee3d-217">たとえば、および選択されていない [選択] タブを表示する MenuItemHelper をという名前の新しい HTML ヘルパーを追加します。</span><span class="sxs-lookup"><span data-stu-id="fee3d-217">For example, we added a new HTML helper named the MenuItemHelper that displays selected and unselected tabs.</span></span>

<span data-ttu-id="fee3d-218">次のイテレーションで検証の非常に重要なサブジェクトに取り組みます。</span><span class="sxs-lookup"><span data-stu-id="fee3d-218">In the next iteration, we tackle the very important subject of validation.</span></span> <span data-ttu-id="fee3d-219">アプリケーションに検証コードを追加おユーザーできません最初 s の利用者などの必要な値を指定せず新しい連絡先を作成し、姓、名ようにします。</span><span class="sxs-lookup"><span data-stu-id="fee3d-219">We add validation code to our application so that a user cannot create a new contact without supplying required values such as a person s first and last name.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fee3d-220">[前へ](iteration-1-create-the-application-cs.md)
[次へ](iteration-3-add-form-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="fee3d-220">[Previous](iteration-1-create-the-application-cs.md)
[Next](iteration-3-add-form-validation-cs.md)</span></span>