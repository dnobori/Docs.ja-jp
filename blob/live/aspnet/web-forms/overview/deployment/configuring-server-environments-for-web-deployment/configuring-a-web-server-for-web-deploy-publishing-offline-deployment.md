---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: "Web サーバーを構成する web デプロイの発行 (オフライン展開) |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、オフライン web 発行および配置をサポートするために IIS web サーバーを構成する方法について説明します。 使用するときにインターネット インフォメーション サービス (すれば..。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: cd3343f58cbb9bb868d15a91152f07444c2bd68e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a><span data-ttu-id="6177a-104">Web デプロイの発行 (オフライン展開) 用の Web サーバーの構成</span><span class="sxs-lookup"><span data-stu-id="6177a-104">Configuring a Web Server for Web Deploy Publishing (Offline Deployment)</span></span>
====================
<span data-ttu-id="6177a-105">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="6177a-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="6177a-106">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="6177a-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="6177a-107">このトピックでは、オフライン web 発行および配置をサポートするために IIS web サーバーを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="6177a-107">This topic describes how to configure an IIS web server to support offline web publishing and deployment.</span></span>
> 
> <span data-ttu-id="6177a-108">インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合は、3 つの主な方法がアプリケーションまたは web サーバー上にサイトを取得するを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="6177a-108">When you work with Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 2.0 or later, there are three main approaches you can use to get your applications or sites onto a web server.</span></span> <span data-ttu-id="6177a-109">次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="6177a-109">You can:</span></span>
> 
> - <span data-ttu-id="6177a-110">使用して、 *Web デプロイ エージェントのリモート サービス*です。</span><span class="sxs-lookup"><span data-stu-id="6177a-110">Use the *Web Deploy Remote Agent Service*.</span></span> <span data-ttu-id="6177a-111">このアプローチには、web サーバーの以下の構成が必要ですが、何もサーバーに配置するためにローカル サーバーの管理者の資格情報を提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-111">This approach requires less configuration of the web server, but you need to provide the credentials of a local server administrator in order to deploy anything to the server.</span></span>
> - <span data-ttu-id="6177a-112">使用して、 *Web Deploy ハンドラー*です。</span><span class="sxs-lookup"><span data-stu-id="6177a-112">Use the *Web Deploy Handler*.</span></span> <span data-ttu-id="6177a-113">このアプローチはもっと複雑であり、web サーバーを設定する初期多くの労力が必要です。</span><span class="sxs-lookup"><span data-stu-id="6177a-113">This approach is a lot more complex and requires more initial effort to set up the web server.</span></span> <span data-ttu-id="6177a-114">ただし、このアプローチを使用する場合は、配置を実行する管理者以外のユーザーを許可するように IIS を構成できます。</span><span class="sxs-lookup"><span data-stu-id="6177a-114">However, when you use this approach, you can configure IIS to allow non-administrator users to perform the deployment.</span></span> <span data-ttu-id="6177a-115">Web 展開ハンドラーは IIS 7 以降のバージョンにできるだけです。</span><span class="sxs-lookup"><span data-stu-id="6177a-115">The Web Deploy Handler is only available in IIS version 7 or later.</span></span>
> - <span data-ttu-id="6177a-116">使用して*オフライン展開*です。</span><span class="sxs-lookup"><span data-stu-id="6177a-116">Use *offline deployment*.</span></span> <span data-ttu-id="6177a-117">このアプローチには、web サーバーの最低限の構成が必要ですが、サーバー管理者は、する必要があります手動でサーバーに web パッケージをコピーして IIS マネージャーからインポートします。</span><span class="sxs-lookup"><span data-stu-id="6177a-117">This approach requires the least configuration of the web server, but a server administrator must manually copy the web package onto the server and import it through IIS Manager.</span></span>
> 
> <span data-ttu-id="6177a-118">主な機能、利点、およびこれらのアプローチの欠点の詳細については、次を参照してください。 [Web 配置を右側の方法を選択する](choosing-the-right-approach-to-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="6177a-118">For more information on the key features, advantages, and disadvantages of these approaches, see [Choosing the Right Approach to Web Deployment](choosing-the-right-approach-to-web-deployment.md).</span></span>


<span data-ttu-id="6177a-119">ネットワーク インフラストラクチャやセキュリティの制限は、リモート展開を防ぐ場合ははい。</span><span class="sxs-lookup"><span data-stu-id="6177a-119">Yes, if your network infrastructure or security restrictions prevent remote deployment.</span></span> <span data-ttu-id="6177a-120">インターネットに接続された運用環境で web サーバーは分離 & #x 2014; ケースである可能性がありますか、物理的にまたはファイアウォールおよびサブネット & #x 2014 以外の場合は、サーバー インフラストラクチャの残りの部分からです。</span><span class="sxs-lookup"><span data-stu-id="6177a-120">This is most likely to be the case in Internet-facing production environments, where the web servers are isolated&#x2014;either physically or by firewalls and subnets&#x2014;from the rest of your server infrastructure.</span></span>

<span data-ttu-id="6177a-121">当然ながら、この方法は、web アプリケーションが、定期的に更新された場合に望ましいになります。</span><span class="sxs-lookup"><span data-stu-id="6177a-121">Obviously, this approach becomes less desirable if your web applications are updated on a regular basis.</span></span> <span data-ttu-id="6177a-122">インフラストラクチャで許可されている、可能性がある場合、リモートの展開を有効にしてください Web 展開ハンドラーまたは Web デプロイ リモート エージェント サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="6177a-122">If your infrastructure allows it, you may want to consider enabling remote deployment, using either the Web Deploy Handler or the Web Deploy Remote Agent Service.</span></span>

## <a name="task-overview"></a><span data-ttu-id="6177a-123">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="6177a-123">Task Overview</span></span>

<span data-ttu-id="6177a-124">オフラインでのインポートと web のパッケージの展開をサポートするために web サーバーを構成するには、する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-124">To configure the web server to support offline import and deployment of web packages, you'll need to:</span></span>

- <span data-ttu-id="6177a-125">IIS 7.5、および IIS 7 の推奨構成をインストールします。</span><span class="sxs-lookup"><span data-stu-id="6177a-125">Install IIS 7.5 and the IIS 7 recommended configuration.</span></span>
- <span data-ttu-id="6177a-126">Web Deploy 2.1 以降をインストールします。</span><span class="sxs-lookup"><span data-stu-id="6177a-126">Install Web Deploy 2.1 or later.</span></span>
- <span data-ttu-id="6177a-127">展開されたコンテンツをホストする IIS の web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="6177a-127">Create an IIS website to host the deployed content.</span></span>
- <span data-ttu-id="6177a-128">Web Deployment Agent サービスを無効にします。</span><span class="sxs-lookup"><span data-stu-id="6177a-128">Disable the Web Deployment Agent Service.</span></span>

<span data-ttu-id="6177a-129">サンプル ソリューションを具体的にはホストにもする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-129">To host the sample solution specifically, you'll also need to:</span></span>

- <span data-ttu-id="6177a-130">.NET Framework 4.0 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="6177a-130">Install the .NET Framework 4.0.</span></span>
- <span data-ttu-id="6177a-131">ASP.NET MVC 3 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="6177a-131">Install ASP.NET MVC 3.</span></span>

<span data-ttu-id="6177a-132">このトピックでは、各手順を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6177a-132">This topic will show you how to perform each of these procedures.</span></span> <span data-ttu-id="6177a-133">タスクとここでチュートリアルは、Windows Server 2008 R2 を実行するクリーン サーバー ビルドを開始していることを想定しています。</span><span class="sxs-lookup"><span data-stu-id="6177a-133">The tasks and walkthroughs in this topic assume that you're starting with a clean server build running Windows Server 2008 R2.</span></span> <span data-ttu-id="6177a-134">続行する前に以下のことを確認します。</span><span class="sxs-lookup"><span data-stu-id="6177a-134">Before you continue, ensure that:</span></span>

- <span data-ttu-id="6177a-135">Windows Server 2008 R2 Service Pack 1 とすべての利用可能な更新プログラムがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="6177a-135">Windows Server 2008 R2 Service Pack 1 and all available updates are installed.</span></span>
- <span data-ttu-id="6177a-136">サーバーがドメインに参加します。</span><span class="sxs-lookup"><span data-stu-id="6177a-136">The server is domain-joined.</span></span>
- <span data-ttu-id="6177a-137">サーバーは、静的 IP アドレスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="6177a-137">The server has a static IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="6177a-138">コンピューターをドメインに参加させる方法については、次を参照してください。[に参加するコンピューター、ドメインとログオン](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="6177a-138">For more information on joining computers to a domain, see [Joining Computers to the Domain and Logging On](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx).</span></span> <span data-ttu-id="6177a-139">静的 IP アドレスを構成する方法については、次を参照してください。[静的 IP アドレスを構成](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="6177a-139">For more information on configuring static IP addresses, see [Configure a Static IP Address](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx).</span></span>


## <a name="install-products-and-components"></a><span data-ttu-id="6177a-140">製品とコンポーネントをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6177a-140">Install Products and Components</span></span>

<span data-ttu-id="6177a-141">このセクションでは、web サーバーに必要な製品とコンポーネントをインストールを説明します。</span><span class="sxs-lookup"><span data-stu-id="6177a-141">This section will guide you through installing the required products and components on the web server.</span></span> <span data-ttu-id="6177a-142">開始する前に、お勧め、サーバーが最新であることを確認する Windows Update を実行します。</span><span class="sxs-lookup"><span data-stu-id="6177a-142">Before you begin, a good practice is to run Windows Update to ensure that your server is fully up to date.</span></span>

<span data-ttu-id="6177a-143">この場合、これらをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-143">In this case, you need to install these things:</span></span>

- <span data-ttu-id="6177a-144">**IIS 7 の推奨構成**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-144">**IIS 7 Recommended Configuration**.</span></span> <span data-ttu-id="6177a-145">これにより、 **Web サーバー (IIS)**ロール、web サーバー上の IIS モジュールおよび ASP.NET アプリケーションをホストするために必要なコンポーネントのセットをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6177a-145">This enables the **Web Server (IIS)** role on your web server and installs the set of IIS modules and components that you need in order to host an ASP.NET application.</span></span>
- <span data-ttu-id="6177a-146">**.NET framework 4.0**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-146">**.NET Framework 4.0**.</span></span> <span data-ttu-id="6177a-147">これは、このバージョンの .NET Framework で構築されたアプリケーションの実行に必要です。</span><span class="sxs-lookup"><span data-stu-id="6177a-147">This is required to run applications that were built on this version of the .NET Framework.</span></span>
- <span data-ttu-id="6177a-148">**Web 配置ツール 2.1 以降**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-148">**Web Deployment Tool 2.1 or later**.</span></span> <span data-ttu-id="6177a-149">これにより、Web Deploy (とその基になる実行可能ファイル、MSDeploy.exe) がサーバーにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="6177a-149">This installs Web Deploy (and its underlying executable, MSDeploy.exe) on your server.</span></span> <span data-ttu-id="6177a-150">Web Deploy では、IIS と統合され、web のパッケージ インポートおよびエクスポートできます。</span><span class="sxs-lookup"><span data-stu-id="6177a-150">Web Deploy integrates with IIS and lets you import and export web packages.</span></span>
- <span data-ttu-id="6177a-151">**ASP.NET MVC 3**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-151">**ASP.NET MVC 3**.</span></span> <span data-ttu-id="6177a-152">これは、MVC 3 アプリケーションを実行する必要があるアセンブリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6177a-152">This installs the assemblies you need to run MVC 3 applications.</span></span>

> [!NOTE]
> <span data-ttu-id="6177a-153">このチュートリアルでは、Web Platform Installer をインストールして構成するさまざまなコンポーネントの使用について説明します。</span><span class="sxs-lookup"><span data-stu-id="6177a-153">This walkthrough describes the use of the Web Platform Installer to install and configure various components.</span></span> <span data-ttu-id="6177a-154">Web Platform Installer を使用できますが、自動的に依存関係を検出して、常に製品の最新バージョンを取得することを確認して、インストール プロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="6177a-154">Although you don't have to use the Web Platform Installer, it simplifies the installation process by automatically detecting dependencies and ensuring that you always get the latest product versions.</span></span> <span data-ttu-id="6177a-155">詳細については、次を参照してください。 [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118)です。</span><span class="sxs-lookup"><span data-stu-id="6177a-155">For more information, see [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).</span></span>


<span data-ttu-id="6177a-156">**必要な製品とコンポーネントをインストールするには**</span><span class="sxs-lookup"><span data-stu-id="6177a-156">**To install the required products and components**</span></span>

1. <span data-ttu-id="6177a-157">ダウンロードしてインストール、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)です。</span><span class="sxs-lookup"><span data-stu-id="6177a-157">Download and install the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>
2. <span data-ttu-id="6177a-158">インストールが完了したら、Web Platform Installer が自動的に起動します。</span><span class="sxs-lookup"><span data-stu-id="6177a-158">When installation is complete, the Web Platform Installer will launch automatically.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6177a-159">いつでも、Web Platform Installer を起動できるようになりました、**開始**メニュー。</span><span class="sxs-lookup"><span data-stu-id="6177a-159">You can now launch the Web Platform Installer at any time from the **Start** menu.</span></span> <span data-ttu-id="6177a-160">[、**開始**] メニューのをクリックして**すべてのプログラム**、順にクリック**Microsoft Web Platform Installer**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-160">To do this, on the **Start** menu, click **All Programs**, and then click **Microsoft Web Platform Installer**.</span></span>
3. <span data-ttu-id="6177a-161">上部にある、 **Web Platform Installer 3.0**ウィンドウで、をクリックして**製品**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-161">At the top of the **Web Platform Installer 3.0** window, click **Products**.</span></span>
4. <span data-ttu-id="6177a-162">ナビゲーション ウィンドウで、ウィンドウの左側にあるをクリックして**フレームワーク**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-162">On the left side of the window, in the navigation pane, click **Frameworks**.</span></span>
5. <span data-ttu-id="6177a-163">**Microsoft .NET Framework 4** 、.NET Framework がインストールされていない場合、行をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-163">In the **Microsoft .NET Framework 4** row, if the .NET Framework is not already installed, click **Add**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6177a-164">既にインストールしている Windows Update から .NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="6177a-164">You may have already installed the .NET Framework 4.0 through Windows Update.</span></span> <span data-ttu-id="6177a-165">製品またはコンポーネントがインストール済みの場合、Web Platform Installer は、これに置き換えることで、**追加**ボタン テキストを**インストール**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-165">If a product or component is already installed, the Web Platform Installer will indicate this by replacing the **Add** button with the text **Installed**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. <span data-ttu-id="6177a-166">**ASP.NET MVC 3 (Visual Studio 2010)**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-166">In the **ASP.NET MVC 3 (Visual Studio 2010)** row, click **Add**.</span></span>
7. <span data-ttu-id="6177a-167">ナビゲーション ウィンドウで **サーバー**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-167">In the navigation pane, click **Server**.</span></span>
8. <span data-ttu-id="6177a-168">**IIS 7 の推奨構成**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-168">In the **IIS 7 Recommended Configuration** row, click **Add**.</span></span>
9. <span data-ttu-id="6177a-169">**Web 配置ツール 2.1**行で、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-169">In the **Web Deployment Tool 2.1** row, click **Add**.</span></span>
10. <span data-ttu-id="6177a-170">**[インストール]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6177a-170">Click **Install**.</span></span> <span data-ttu-id="6177a-171">Web Platform Installer をインストールするには、関連する依存関係 & #x 2014; と共に; 製品 & #x 2014 の一覧が表示され、ライセンス条項に同意するように求められます。</span><span class="sxs-lookup"><span data-stu-id="6177a-171">The Web Platform Installer will show you a list of products&#x2014;together with any associated dependencies&#x2014;to be installed and will prompt you to accept the license terms.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. <span data-ttu-id="6177a-172">ライセンス条項を確認し、条項に同意した場合にをクリックして**同意**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-172">Review the license terms, and if you consent to the terms, click **I Accept**.</span></span>
12. <span data-ttu-id="6177a-173">インストールが完了したらをクリックして**完了**、し、閉じます、 **Web Platform Installer 3.0**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="6177a-173">When the installation is complete, click **Finish**, and then close the **Web Platform Installer 3.0** window.</span></span>

<span data-ttu-id="6177a-174">IIS をインストールする前に、.NET Framework 4.0 をインストールした場合を実行する必要があります、 [ASP.NET IIS 登録ツール](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx)(aspnet\_regiis.exe) を IIS と ASP.NET の最新バージョンを登録します。</span><span class="sxs-lookup"><span data-stu-id="6177a-174">If you installed the .NET Framework 4.0 before you installed IIS, you'll need to run the [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) to register the latest version of ASP.NET with IIS.</span></span> <span data-ttu-id="6177a-175">これを行わないと、ことがわかります (HTML ファイル) などの静的なコンテンツを IIS が提供する、問題なくが返されます**HTTP エラー 404.0-Not Found** ASP.NET のコンテンツを参照しようとします。</span><span class="sxs-lookup"><span data-stu-id="6177a-175">If you don't do this, you'll find that IIS will serve static content (like HTML files) without any problems, but it will return **HTTP Error 404.0 – Not Found** when you attempt to browse to ASP.NET content.</span></span> <span data-ttu-id="6177a-176">次の手順を使用すると、ASP.NET 4.0 が登録されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="6177a-176">You can use the next procedure to ensure that ASP.NET 4.0 is registered.</span></span>

<span data-ttu-id="6177a-177">**ASP.NET 4.0 を IIS に登録するには**</span><span class="sxs-lookup"><span data-stu-id="6177a-177">**To register ASP.NET 4.0 with IIS**</span></span>

1. <span data-ttu-id="6177a-178">をクリックして**開始**、し、入力**コマンド プロンプト**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-178">Click **Start**, and then type **Command Prompt**.</span></span>
2. <span data-ttu-id="6177a-179">検索結果を右クリックして**コマンド プロンプト**、クリックして**管理者として実行**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-179">In the search results, right-click **Command Prompt**, and then click **Run as administrator**.</span></span>
3. <span data-ttu-id="6177a-180">コマンド プロンプト ウィンドウに移動、 **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="6177a-180">In the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** directory.</span></span>
4. <span data-ttu-id="6177a-181">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="6177a-181">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. <span data-ttu-id="6177a-182">任意の時点での 64 ビットの web アプリケーションをホストする場合は、IIS と 64 ビット バージョンの ASP.NET を登録することも必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-182">If you plan to host 64-bit web applications at any point, you should also register the 64-bit version of ASP.NET with IIS.</span></span> <span data-ttu-id="6177a-183">コマンド プロンプト ウィンドウで、これに移動、 **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="6177a-183">To do this, in the Command Prompt window, navigate to the **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** directory.</span></span>
6. <span data-ttu-id="6177a-184">このコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="6177a-184">Type this command, and then press Enter:</span></span>

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

<span data-ttu-id="6177a-185">をお勧め Windows Update を使用してもう一度この時点でダウンロードして、新しい製品とコンポーネントがインストールされている使用可能な更新プログラムをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6177a-185">As a good practice, use Windows Update again at this point to download and install any available updates for the new products and components you've installed.</span></span>

## <a name="configure-the-iis-website"></a><span data-ttu-id="6177a-186">IIS web サイトを構成します。</span><span class="sxs-lookup"><span data-stu-id="6177a-186">Configure the IIS Website</span></span>

<span data-ttu-id="6177a-187">Web コンテンツを展開するには、サーバーに、前に作成し、コンテンツをホストする IIS の web サイトを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-187">Before you can deploy web content to your server, you need to create and configure an IIS website to host the content.</span></span> <span data-ttu-id="6177a-188">Web Deploy にしかデプロイできません web パッケージ既存の IIS web サイトです。web サイトを作成できません。</span><span class="sxs-lookup"><span data-stu-id="6177a-188">Web Deploy can only deploy web packages to an existing IIS website; it can't create the website for you.</span></span> <span data-ttu-id="6177a-189">大まかに言えば、これらのタスクを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-189">At a high level, you'll need to complete these tasks:</span></span>

- <span data-ttu-id="6177a-190">コンテンツをホストするファイル システムにフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="6177a-190">Create a folder on the file system to host your content.</span></span>
- <span data-ttu-id="6177a-191">コンテンツを提供する IIS の web サイトを作成し、ローカル フォルダーに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="6177a-191">Create an IIS website to serve the content, and associate it with the local folder.</span></span>
- <span data-ttu-id="6177a-192">読み取り、アプリケーション プール id に、ローカル フォルダーに対する権限を付与します。</span><span class="sxs-lookup"><span data-stu-id="6177a-192">Grant read permissions to the application pool identity on the local folder.</span></span>

<span data-ttu-id="6177a-193">阻止する IIS の既定の web サイトにコンテンツを展開するからですが、この方法はテストまたはデモのシナリオ以外の何かの推奨されません。</span><span class="sxs-lookup"><span data-stu-id="6177a-193">Although there's nothing stopping you from deploying content to the default website in IIS, this approach is not recommended for anything other than test or demonstration scenarios.</span></span> <span data-ttu-id="6177a-194">実稼働環境をシミュレートするには、アプリケーションの要件に固有の設定で新しい IIS web サイトを作成してください。</span><span class="sxs-lookup"><span data-stu-id="6177a-194">To simulate a production environment, you should create a new IIS website with settings that are specific to the requirements of your application.</span></span>

<span data-ttu-id="6177a-195">**作成し、IIS の web サイトを構成します。**</span><span class="sxs-lookup"><span data-stu-id="6177a-195">**To create and configure an IIS website**</span></span>

1. <span data-ttu-id="6177a-196">ローカル ファイル システムにコンテンツを保存するフォルダーを作成します (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6177a-196">On the local file system, create a folder to store your content (for example, **C:\DemoSite**).</span></span>
2. <span data-ttu-id="6177a-197">**開始** メニューのをポイント**管理ツール**、クリックして**インターネット インフォメーション サービス (IIS) マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-197">On the **Start** menu, point to **Administrative Tools**, and then click **Internet Information Services (IIS) Manager**.</span></span>
3. <span data-ttu-id="6177a-198">IIS マネージャーで、**接続** ウィンドウで、サーバー ノードを展開 (たとえば、 **PROWEB1**)。</span><span class="sxs-lookup"><span data-stu-id="6177a-198">In IIS Manager, in the **Connections** pane, expand the server node (for example, **PROWEB1**).</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. <span data-ttu-id="6177a-199">右クリックし、**サイト**ノードをクリックして**Web サイトの追加**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-199">Right-click the **Sites** node, and then click **Add Web Site**.</span></span>
5. <span data-ttu-id="6177a-200">**サイト名**ボックスで、IIS の web サイトの名前を入力 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6177a-200">In the **Site name** box, type a name for the IIS website (for example, **DemoSite**).</span></span>
6. <span data-ttu-id="6177a-201">**物理パス**ボックスに入力 (またはを参照)、ローカル フォルダーへのパス (たとえば、 **C:\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6177a-201">In the **Physical path** box, type (or browse to) the path to your local folder (for example, **C:\DemoSite**).</span></span>
7. <span data-ttu-id="6177a-202">**ポート**ボックスに、web サイトをホストするポート番号を入力 (たとえば、 **85**)。</span><span class="sxs-lookup"><span data-stu-id="6177a-202">In the **Port** box, type the port number on which you want to host the website (for example, **85**).</span></span>

    > [!NOTE]
    > <span data-ttu-id="6177a-203">標準のポート番号は http が 80 および HTTPS の場合は 443 です。</span><span class="sxs-lookup"><span data-stu-id="6177a-203">The standard port numbers are 80 for HTTP and 443 for HTTPS.</span></span> <span data-ttu-id="6177a-204">ただし、ポート 80 では、この web サイトをホストしている場合は、サイトにアクセスするには、既定の web サイトを停止する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-204">However, if you host this website on port 80, you'll need to stop the default website before you can access your site.</span></span>
8. <span data-ttu-id="6177a-205">ままにして、**ホスト名**ボックスをクリックして、web サイトのドメイン ネーム システム (DNS) レコードを構成するのでない限り、空白**OK**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-205">Leave the **Host name** box blank, unless you want to configure a Domain Name System (DNS) record for the website, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="6177a-206">実稼働環境にすることができますをポート 80 で web サイトをホストし、DNS レコードを一致すると共に、ホスト ヘッダーを構成します。</span><span class="sxs-lookup"><span data-stu-id="6177a-206">In a production environment, you'll likely want to host your website on port 80 and configure a host header, together with matching DNS records.</span></span> <span data-ttu-id="6177a-207">IIS 7 でのホスト ヘッダーを構成する方法については、次を参照してください。 [(IIS 7) の Web サイトのホスト ヘッダーを構成する](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="6177a-207">For more information on configuring host headers in IIS 7, see [Configure a Host Header for a Web Site (IIS 7)](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx).</span></span> <span data-ttu-id="6177a-208">Windows Server 2008 R2 の DNS サーバーの役割の詳細については、次を参照してください。 [DNS サーバーの概要](https://technet.microsoft.com/en-gb/library/cc770392.aspx)と[DNS Server](https://technet.microsoft.com/en-us/windowsserver/dd448607)です。</span><span class="sxs-lookup"><span data-stu-id="6177a-208">For more information on the DNS Server role in Windows Server 2008 R2, see [DNS Server Overview](https://technet.microsoft.com/en-gb/library/cc770392.aspx) and [DNS Server](https://technet.microsoft.com/en-us/windowsserver/dd448607).</span></span>
9. <span data-ttu-id="6177a-209">**[アクション]** ウィンドウの **[サイトの編集]** の下にある **[バインド]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6177a-209">In the **Actions** pane, under **Edit Site**, click **Bindings**.</span></span>
10. <span data-ttu-id="6177a-210">**サイト バインド**ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-210">In the **Site Bindings** dialog box, click **Add**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. <span data-ttu-id="6177a-211">**サイト バインドの追加**ダイアログ ボックスで、設定、 **IP アドレス**と**ポート**既存サイトの構成に一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="6177a-211">In the **Add Site Binding** dialog box, set the **IP address** and **Port** to match your existing site configuration.</span></span>
12. <span data-ttu-id="6177a-212">**ホスト名**ボックス、web サーバーの名前を入力 (たとえば、 **PROWEB1**)、をクリックし、 **[ok]**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-212">In the **Host name** box, type the name of your web server (for example, **PROWEB1**), and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > <span data-ttu-id="6177a-213">最初のサイトのバインドでは、IP アドレスとポートを使用してローカル サイトにアクセスできます。 または`http://localhost:85`です。</span><span class="sxs-lookup"><span data-stu-id="6177a-213">The first site binding allows you to access the site locally using the IP address and port or `http://localhost:85`.</span></span> <span data-ttu-id="6177a-214">2 つ目のサイト バインドでは、コンピューター名 (たとえば、http://proweb1:85) を使用して、ドメインの他のコンピューターからサイトにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="6177a-214">The second site binding allows you to access the site from other computers on the domain using the machine name (for example, http://proweb1:85).</span></span>
13. <span data-ttu-id="6177a-215">**サイト バインド**ダイアログ ボックスで、をクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-215">In the **Site Bindings** dialog box, click **Close**.</span></span>
14. <span data-ttu-id="6177a-216">**接続** ウィンドウで、をクリックして**アプリケーション プール**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-216">In the **Connections** pane, click **Application Pools**.</span></span>
15. <span data-ttu-id="6177a-217">**アプリケーション プール** ウィンドウでは、アプリケーション プールの名前を右クリックし、をクリックして**基本設定**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-217">In the **Application Pools** pane, right-click the name of your application pool, and then click **Basic Settings**.</span></span> <span data-ttu-id="6177a-218">既定では、アプリケーション プールの名前が、web サイトの名前に一致 (たとえば、 **DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6177a-218">By default, the name of your application pool will match the name of your website (for example, **DemoSite**).</span></span>
16. <span data-ttu-id="6177a-219">**.NET Framework のバージョン**一覧で、 **.NET Framework v4.0.30319**、順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-219">In the **.NET Framework version** list, select **.NET Framework v4.0.30319**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="6177a-220">サンプル ソリューションには、.NET Framework 4.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="6177a-220">The sample solution requires .NET Framework 4.0.</span></span> <span data-ttu-id="6177a-221">これは、要件ではありません、Web Deploy の一般にします。</span><span class="sxs-lookup"><span data-stu-id="6177a-221">This is not a requirement for Web Deploy in general.</span></span>

<span data-ttu-id="6177a-222">Web サイト コンテンツを提供するためには、アプリケーション プール id 読み取り権限が必要、コンテンツを格納するローカル フォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="6177a-222">In order for your website to serve content, the application pool identity must have read permissions on the local folder that stores the content.</span></span> <span data-ttu-id="6177a-223">IIS 7.5、アプリケーション プールは、(ここでアプリケーション プールは通常の実行、Network Service アカウントを使用して、IIS の以前のバージョン) とは異なり、既定では、一意のアプリケーション プール id で実行します。</span><span class="sxs-lookup"><span data-stu-id="6177a-223">In IIS 7.5, application pools run with a unique application pool identity by default (in contrast to previous versions of IIS, where application pools would typically run using the Network Service account).</span></span> <span data-ttu-id="6177a-224">アプリケーション プール id が実際のユーザー アカウントではないと、ユーザーまたはグループ & #x 2014 のすべてのリストに表示されないです。 代わりに、それが動的に作成、アプリケーション プールが開始されたときにします。</span><span class="sxs-lookup"><span data-stu-id="6177a-224">The application pool identity is not a real user account and does not show up on any lists of users or groups&#x2014;instead, it's created dynamically when the application pool is started.</span></span> <span data-ttu-id="6177a-225">各アプリケーション プール id がローカルに追加**IIS\_IUSRS**セキュリティ グループを非表示のアイテムとして。</span><span class="sxs-lookup"><span data-stu-id="6177a-225">Each application pool identity is added to the local **IIS\_IUSRS** security group as a hidden item.</span></span>

<span data-ttu-id="6177a-226">ファイルまたはフォルダーでのアプリケーション プール id へのアクセス許可を付与するには、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="6177a-226">To grant permissions to an application pool identity on a file or folder, you have two options:</span></span>

- <span data-ttu-id="6177a-227">アクセス許可を割り当てるアプリケーション プール id に直接、形式を使用して**IIS AppPool\***[アプリケーション プール名] * (たとえば、 **IIS AppPool\DemoSite**)。</span><span class="sxs-lookup"><span data-stu-id="6177a-227">Assign permissions to the application pool identity directly, using the format **IIS AppPool\***[application pool name]*(for example, **IIS AppPool\DemoSite**).</span></span>
- <span data-ttu-id="6177a-228">アクセス許可を割り当てる、 **IIS\_IUSRS**グループ。</span><span class="sxs-lookup"><span data-stu-id="6177a-228">Assign permissions to the **IIS\_IUSRS** group.</span></span>

<span data-ttu-id="6177a-229">最も一般的な方法は、ローカルに権限を割り当てる、 **IIS\_IUSRS**このアプローチでは、ファイル システム アクセス許可を再構成することがなくアプリケーション プールを変更することができますので、グループ化します。</span><span class="sxs-lookup"><span data-stu-id="6177a-229">The most common approach is to assign permissions to the local **IIS\_IUSRS** group, because this approach lets you change application pools without reconfiguring file system permissions.</span></span> <span data-ttu-id="6177a-230">次の手順では、このグループ ベースのアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="6177a-230">The next procedure uses this group-based approach.</span></span>

> [!NOTE]
> <span data-ttu-id="6177a-231">IIS 7.5 におけるアプリケーション プール id の詳細については、次を参照してください。[アプリケーション プール Id](https://go.microsoft.com/?linkid=9805123)です。</span><span class="sxs-lookup"><span data-stu-id="6177a-231">For more information on application pool identities in IIS 7.5, see [Application Pool Identities](https://go.microsoft.com/?linkid=9805123).</span></span>


<span data-ttu-id="6177a-232">**IIS の web サイトのフォルダーのアクセス許可を構成するには**</span><span class="sxs-lookup"><span data-stu-id="6177a-232">**To configure folder permissions for an IIS website**</span></span>

1. <span data-ttu-id="6177a-233">Windows エクスプ ローラーで、ローカル フォルダーの場所を参照します。</span><span class="sxs-lookup"><span data-stu-id="6177a-233">In Windows Explorer, browse to the location of your local folder.</span></span>
2. <span data-ttu-id="6177a-234">フォルダーを右クリックし、をクリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-234">Right-click the folder, and then click **Properties**.</span></span>
3. <span data-ttu-id="6177a-235">**セキュリティ** タブで、をクリックして**編集**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-235">On the **Security** tab, click **Edit**, and then click **Add**.</span></span>
4. <span data-ttu-id="6177a-236">をクリックして**場所**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-236">Click **Locations**.</span></span> <span data-ttu-id="6177a-237">**場所**ダイアログ ボックスでは、ローカル サーバーを選択し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-237">In the **Locations** dialog box, select the local server, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. <span data-ttu-id="6177a-238">**ユーザーまたはグループ** ダイアログ ボックスで、「 **IIS\_IUSRS**、 をクリックして**名前の確認**順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-238">In the **Select Users or Groups** dialog box, type **IIS\_IUSRS**, click **Check Names**, and then click **OK**.</span></span>
6. <span data-ttu-id="6177a-239">**のアクセス許可***[フォルダー名]*  ダイアログ ボックスで、新しいグループが割り当てられている、**読み取り&amp;実行**、**フォルダーの一覧内容**、および**読み取り**既定のアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="6177a-239">In the **Permissions for***[folder name]* dialog box, notice that the new group has been assigned the **Read &amp; execute**, **List folder contents**, and **Read** permissions by default.</span></span> <span data-ttu-id="6177a-240">これを変更せずのままにし、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-240">Leave this unchanged and click **OK**.</span></span>
7. <span data-ttu-id="6177a-241">をクリックして**OK**を閉じる、 *[フォルダー名]***プロパティ** ダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="6177a-241">Click **OK** to close the *[folder name]***Properties** dialog box.</span></span>

## <a name="disable-the-remote-agent-service"></a><span data-ttu-id="6177a-242">リモート エージェント サービスを無効にします。</span><span class="sxs-lookup"><span data-stu-id="6177a-242">Disable the Remote Agent Service</span></span>

<span data-ttu-id="6177a-243">Web Deploy をインストールするときに Web Deployment Agent サービスがインストールされ、自動的に開始します。</span><span class="sxs-lookup"><span data-stu-id="6177a-243">When you install Web Deploy, the Web Deployment Agent Service is installed and started automatically.</span></span> <span data-ttu-id="6177a-244">このサービスを使用すると、展開し、リモートの場所から web パッケージを公開できます。</span><span class="sxs-lookup"><span data-stu-id="6177a-244">This service allows you to deploy and publish web packages from a remote location.</span></span> <span data-ttu-id="6177a-245">使用しないリモート展開機能このシナリオでため、停止し、サービスを無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6177a-245">You won't be using the remote deployment capability in this scenario, so you should stop and disable the service.</span></span>

> [!NOTE]
> <span data-ttu-id="6177a-246">インポートおよび web パッケージを手動で展開するためにリモート エージェント サービスを停止する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6177a-246">You don't need to stop the remote agent service in order to import and deploy a web package manually.</span></span> <span data-ttu-id="6177a-247">ただしは停止し、それを使用する予定がない場合は、サービスを無効にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6177a-247">However, it's a good practice to stop and disable the service if you don't plan to use it.</span></span>


<span data-ttu-id="6177a-248">停止して、さまざまなコマンド ライン ユーティリティまたは Windows PowerShell コマンドレットを使用して、複数の方法でサービスを無効にできます。</span><span class="sxs-lookup"><span data-stu-id="6177a-248">You can stop and disable a service in multiple ways, using various command-line utilities or Windows PowerShell cmdlets.</span></span> <span data-ttu-id="6177a-249">この手順では、UI ベースの簡単な方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="6177a-249">This procedure describes a straightforward UI-based approach.</span></span>

<span data-ttu-id="6177a-250">**停止し、リモート エージェント サービスを無効にします。**</span><span class="sxs-lookup"><span data-stu-id="6177a-250">**To stop and disable the remote agent service**</span></span>

1. <span data-ttu-id="6177a-251">**[スタート]** メニューで、 **[管理ツール]**をポイントして、 **[サービス]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6177a-251">On the **Start** menu, point to **Administrative Tools**, and then click **Services**.</span></span>
2. <span data-ttu-id="6177a-252">サービス コンソールで、検索、 **Web Deployment Agent サービス**行です。</span><span class="sxs-lookup"><span data-stu-id="6177a-252">In the Services console, locate the **Web Deployment Agent Service** row.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. <span data-ttu-id="6177a-253">右クリック**Web Deployment Agent サービス**、クリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-253">Right-click **Web Deployment Agent Service**, and then click **Properties**.</span></span>
4. <span data-ttu-id="6177a-254">**Web デプロイ エージェントのサービス プロパティ**ダイアログ ボックスで、をクリックして**停止**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-254">In the **Web Deployment Agent Service Properties** dialog box, click **Stop**.</span></span>
5. <span data-ttu-id="6177a-255">**スタートアップの種類**一覧で、**無効になっている**、順にクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="6177a-255">In the **Startup type** list, select **Disabled**, and then click **OK**.</span></span>

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a><span data-ttu-id="6177a-256">まとめ</span><span class="sxs-lookup"><span data-stu-id="6177a-256">Conclusion</span></span>

<span data-ttu-id="6177a-257">この時点では、web サーバーはオフライン web パッケージの配置の準備完了です。</span><span class="sxs-lookup"><span data-stu-id="6177a-257">At this point, your web server is ready for offline web package deployment.</span></span> <span data-ttu-id="6177a-258">IIS の web サイトに web パッケージをインポートする前に、これらの要点をチェックすることがあります。</span><span class="sxs-lookup"><span data-stu-id="6177a-258">Before you attempt to import web packages to an IIS website, you may want to check these key points:</span></span>

- <span data-ttu-id="6177a-259">IIS で ASP.NET 4.0 を登録したしますか。</span><span class="sxs-lookup"><span data-stu-id="6177a-259">Have you registered ASP.NET 4.0 with IIS?</span></span>
- <span data-ttu-id="6177a-260">アプリケーション プール id は、web サイトのソース フォルダーに読み取りアクセスがしますか。</span><span class="sxs-lookup"><span data-stu-id="6177a-260">Does the application pool identity have read access to the source folder for your website?</span></span>
- <span data-ttu-id="6177a-261">Web Deployment Agent サービスを停止していますか。</span><span class="sxs-lookup"><span data-stu-id="6177a-261">Have you stopped the Web Deployment Agent Service?</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6177a-262">[前へ](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[次へ](configuring-a-database-server-for-web-deploy-publishing.md)</span><span class="sxs-lookup"><span data-stu-id="6177a-262">[Previous](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[Next](configuring-a-database-server-for-web-deploy-publishing.md)</span></span>