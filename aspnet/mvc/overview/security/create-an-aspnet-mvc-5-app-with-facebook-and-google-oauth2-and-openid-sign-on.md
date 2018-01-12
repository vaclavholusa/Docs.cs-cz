---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: "Vytvoření MVC 5 aplikace pomocí služby Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování (C#) | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5, který umožňuje uživatelům přihlásit pomocí OAuth 2.0 přihlašovací údaje z externí authenti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: aaa061e61b9bab5b33083851624f0487b2cf6473
ms.sourcegitcommit: ccf08615ad59bc6f654560de33b93396113a2eb0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="6022b-103">Vytvoření aplikace ASP.NET MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování (C#)</span><span class="sxs-lookup"><span data-stu-id="6022b-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="6022b-104">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6022b-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="6022b-105">V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5, který umožňuje uživatelům přihlásit se pomocí [OAuth 2.0](http://oauth.net/2/) s přihlašovací údaje z externí zprostředkovatel ověřování, jako je Facebook, Twitter, LinkedIn, Microsoft nebo Google.</span><span class="sxs-lookup"><span data-stu-id="6022b-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="6022b-106">Pro zjednodušení tento kurz se zaměřuje na práci s přihlašovacími údaji ze sítě Facebook a Google.</span><span class="sxs-lookup"><span data-stu-id="6022b-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="6022b-107">Povolení tyto přihlašovací údaje ve své weby nabízí významné výhody, protože milionům uživatelů již mají účty s tyto externí zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="6022b-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="6022b-108">Tyto uživatele může být více nakloněné zaregistrovat pro svůj web, pokud nemají k vytvoření a zapamatovat si novou sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="6022b-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="6022b-109">Viz také [aplikaci ASP.NET MVC 5 s SMS a e-mailu dvoufaktorové ověřování](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="6022b-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="6022b-110">Tento kurz také ukazuje, jak přidat data profilu pro uživatele a jak používat rozhraní API členství a přidejte role.</span><span class="sxs-lookup"><span data-stu-id="6022b-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="6022b-111">V tomto kurzu napsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (postupujte mi na Twitteru: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="6022b-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="6022b-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="6022b-112">Getting Started</span></span>

<span data-ttu-id="6022b-113">Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="6022b-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="6022b-114">Instalaci sady Visual Studio [2013 s aktualizací 3](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="6022b-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="6022b-115">Pomoc s Dropbox, Githubu, Linkedin, Instagram, vyrovnávací paměť, salesforce, datový proud, zásobník Exchange, Tripit, twitch, Twitter, Yahoo a další najdete v tématu to [jeden stop průvodce](http://www.oauthforaspnet.com/).</span><span class="sxs-lookup"><span data-stu-id="6022b-115">For help with Dropbox, GitHub, Linkedin, Instagram, buffer, salesforce, STEAM, Stack Exchange, Tripit, twitch, Twitter, Yahoo and more, see this [one stop guide](http://www.oauthforaspnet.com/).</span></span>

> [!NOTE]
> <span data-ttu-id="6022b-116">Je nutné nainstalovat Visual Studio [2013 s aktualizací 3](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší pro použijte Google OAuth 2 a ladění místně bez upozornění týkající se SSL.</span><span class="sxs-lookup"><span data-stu-id="6022b-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="6022b-117">Klikněte na tlačítko **nový projekt** z **spustit** stránky, nebo můžete použít nabídku a vyberte **soubor**a potom **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="6022b-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="6022b-118">Vytvoření vaší první aplikace</span><span class="sxs-lookup"><span data-stu-id="6022b-118">Creating Your First Application</span></span>

<span data-ttu-id="6022b-119">Klikněte na tlačítko **nový projekt**, pak vyberte **Visual C#** na levé straně, pak **webové** a pak vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6022b-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="6022b-120">Název projektu "MvcAuth" a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="6022b-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="6022b-121">V **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **MVC**.</span><span class="sxs-lookup"><span data-stu-id="6022b-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="6022b-122">Pokud ověření není **jednotlivé uživatelské účty**, klikněte **změna ověřování** tlačítko a vyberte **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="6022b-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="6022b-123">Kontrolou **hostitel v cloudu**, aplikace bude velmi snadné hostovat v Azure.</span><span class="sxs-lookup"><span data-stu-id="6022b-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="6022b-124">Pokud jste vybrali **hostitel v cloudu**, dokončete dialogové okno Konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="6022b-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="6022b-125">Použít aktualizace na nejnovější middlewaru OWIN, který NuGet</span><span class="sxs-lookup"><span data-stu-id="6022b-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="6022b-126">Pomocí Správce balíčků NuGet se aktualizovat [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="6022b-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="6022b-127">Vyberte **aktualizace** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="6022b-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="6022b-128">Kliknutím na **Aktualizovat vše** tlačítko, nebo můžete vyhledat pouze balíčky OWIN (viz další obrázek):</span><span class="sxs-lookup"><span data-stu-id="6022b-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="6022b-129">Na obrázku níže jsou uvedeny pouze OWIN balíčky:</span><span class="sxs-lookup"><span data-stu-id="6022b-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="6022b-130">Z balíček správce konzoly (pomocí PMC), můžete zadat `Update-Package` příkaz, který aktualizuje všechny balíčky.</span><span class="sxs-lookup"><span data-stu-id="6022b-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="6022b-131">Stiskněte klávesu **F5** nebo **Ctrl + F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6022b-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="6022b-132">Na obrázku níže číslo portu je 1234.</span><span class="sxs-lookup"><span data-stu-id="6022b-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="6022b-133">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="6022b-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="6022b-134">V závislosti na velikosti okna prohlížeče, možná budete muset klikněte na ikonu navigace zobrazíte **Domů**, **o**, **kontaktujte**, **zaregistrovat**a **přihlásit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="6022b-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="6022b-135">Nastavení protokolu SSL v projektu</span><span class="sxs-lookup"><span data-stu-id="6022b-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="6022b-136">Pro připojení k zprostředkovatele ověřování, jako je Google a Facebook, musíte nastavit službu IIS Express pro použití protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="6022b-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="6022b-137">Je důležité udržovat po přihlášení pomocí protokolu SSL a není vyřadit zpět k protokolu HTTP, vaše přihlašovací soubor cookie je stejně jako tajný klíč jako uživatelské jméno a heslo a bez použití protokolu SSL odesíláte ho v prostém textu v drátové síti.</span><span class="sxs-lookup"><span data-stu-id="6022b-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="6022b-138">Kromě toho jste už udělali čas k provedení handshake a zabezpečený kanál (což je velkou část díky HTTPS pomalejší než HTTP) před spuštěním kanál MVC, proto přesměrovat zpět na HTTP po jste přihlášeni nebude nastavit aktuální požadavek nebo budoucí požadavky na mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="6022b-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="6022b-139">V **Průzkumníku řešení**, klikněte **MvcAuth** projektu.</span><span class="sxs-lookup"><span data-stu-id="6022b-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="6022b-140">Stisknutím klávesy F4 zobrazit vlastnosti projektu.</span><span class="sxs-lookup"><span data-stu-id="6022b-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="6022b-141">Můžete také z **zobrazení** nabídky můžete vybrat **vlastnosti – okno**.</span><span class="sxs-lookup"><span data-stu-id="6022b-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="6022b-142">Změna **SSL povoleno** na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="6022b-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="6022b-143">Zkopírujte adresu URL protokolu SSL (který bude `https://localhost:44300/` Pokud jste vytvořili další projekty SSL).</span><span class="sxs-lookup"><span data-stu-id="6022b-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="6022b-144">V **Průzkumníku řešení**, klikněte pravým tlačítkem **MvcAuth** projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6022b-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="6022b-145">Vyberte **webové** kartě a vložte adresu URL SSL do **adresa Url projektu** pole.</span><span class="sxs-lookup"><span data-stu-id="6022b-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="6022b-146">Uložte soubor (seznam Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="6022b-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="6022b-147">Budete potřebovat adresu URL konfigurace aplikací pro ověřování sítě Facebook a Google.</span><span class="sxs-lookup"><span data-stu-id="6022b-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="6022b-148">Přidat [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) atribut `Home` řadiče tak, aby vyžadovala všechny požadavky musí používat protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6022b-148">Add the [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="6022b-149">Zabezpečení způsobů je přidání [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) filtru do aplikace.</span><span class="sxs-lookup"><span data-stu-id="6022b-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="6022b-150">Najdete v části &quot;chránit aplikace pomocí protokolu SSL a atribut Autorizovat&quot; v mé tutoral [vytvoření aplikace ASP.NET MVC pomocí ověřování a databázi SQL a nasazení do Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="6022b-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="6022b-151">Část domovské řadiče jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="6022b-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="6022b-152">Stisknutím kombinace kláves CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6022b-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="6022b-153">Pokud jste nainstalovali certifikát v minulosti, můžete přeskočit zbývající část tohoto oddílu a přejít na [vytvoření aplikace na Google OAuth 2 a připojení aplikace k projektu](#goog), jinak postupujte podle pokynů důvěřovat podepsaný certifikát, který služba IIS Express vygenerovala.</span><span class="sxs-lookup"><span data-stu-id="6022b-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="6022b-154">Pro čtení **upozornění zabezpečení** dialogové okno a potom klikněte na **Ano** Pokud chcete nainstalovat certifikát představující localhost.</span><span class="sxs-lookup"><span data-stu-id="6022b-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="6022b-155">IE ukazuje *Domů* stránky a neexistují žádná upozornění týkající se SSL.</span><span class="sxs-lookup"><span data-stu-id="6022b-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="6022b-156">Google Chrome také certifikát přijme a zobrazí obsah protokolu HTTPS bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="6022b-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="6022b-157">Firefox používá vlastní úložiště certifikátů, proto se zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="6022b-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="6022b-158">Pro naši aplikaci můžete bezpečně kliknout na **beru na vědomí rizika**.</span><span class="sxs-lookup"><span data-stu-id="6022b-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="6022b-159">Vytvoření aplikace na Google OAuth 2 a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="6022b-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

1. <span data-ttu-id="6022b-160">Přejděte na [konzole pro vývojáře Google](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="6022b-160">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
1. <span data-ttu-id="6022b-161">Pokud jste dosud nevytvořili projektu před, vyberte **pověření** v levé kartě a potom vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6022b-161">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
1. <span data-ttu-id="6022b-162">V levé kartě klikněte na **pověření**.</span><span class="sxs-lookup"><span data-stu-id="6022b-162">In the left tab, click **Credentials**.</span></span>
1. <span data-ttu-id="6022b-163">Klikněte na tlačítko **vytvořit přihlašovací údaje** pak **ID klienta OAuth**.</span><span class="sxs-lookup"><span data-stu-id="6022b-163">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="6022b-164">V **vytvořit ID klienta** dialogové okno, ponechte výchozí **webové aplikace** pro typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="6022b-164">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="6022b-165">Nastavte **oprávnění JavaScript** zdroje na adresu URL SSL, který jste použili výše (`https://localhost:44300/` Pokud jste vytvořili další projekty SSL)</span><span class="sxs-lookup"><span data-stu-id="6022b-165">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="6022b-166">Nastavte **identifikátor URI pro přesměrování autorizováno** na:</span><span class="sxs-lookup"><span data-stu-id="6022b-166">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="6022b-167">Klikněte na položku nabídky obrazovky souhlas OAuth a potom nastavit vaše e-mailovou adresu a název produktu.</span><span class="sxs-lookup"><span data-stu-id="6022b-167">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="6022b-168">Když jste dokončili a klikněte na formulář **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6022b-168">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="6022b-169">Klikněte na položku nabídky knihovny, hledání **rozhraní API Google +**, klikněte na něm potom stiskněte klávesu povolit.</span><span class="sxs-lookup"><span data-stu-id="6022b-169">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
 <span data-ttu-id="6022b-170">Následující obrázek ukazuje povoleno rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6022b-170">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="6022b-171">Z Google rozhraní API rozhraní API Správce, přejděte **pověření** kartě získat **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="6022b-171">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="6022b-172">Stáhněte si uložte soubor JSON s tajné klíče aplikace.</span><span class="sxs-lookup"><span data-stu-id="6022b-172">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="6022b-173">Zkopírujte a vložte **ClientId** a **ClientSecret** do `UseGoogleAuthentication` nalezena metoda v *Startup.Auth.cs* v soubor *App_Start* složky.</span><span class="sxs-lookup"><span data-stu-id="6022b-173">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="6022b-174">**ClientId** a **ClientSecret** hodnoty zobrazené níže jsou příklady a nefungují.</span><span class="sxs-lookup"><span data-stu-id="6022b-174">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="6022b-175">Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="6022b-175">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="6022b-176">Účet a přihlašovací údaje se přidají do výše pro zjednodušení v ukázkovém kódu.</span><span class="sxs-lookup"><span data-stu-id="6022b-176">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="6022b-177">V tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a službě Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="6022b-177">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="6022b-178">Stiskněte klávesu **CTRL + F5** sestavení a spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6022b-178">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="6022b-179">Klikněte **přihlásit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6022b-179">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="6022b-180">V části **přihlásit pomocí jiné služby**, klikněte na tlačítko **Google**.</span><span class="sxs-lookup"><span data-stu-id="6022b-180">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="6022b-181">V případě, že byste zapomněli na některý z výše uvedených kroků zobrazí se chyba HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="6022b-181">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="6022b-182">Znovu zkontrolujte vaše kroky uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="6022b-182">Recheck your steps above.</span></span> <span data-ttu-id="6022b-183">Pokud přeskočíte požadované nastavení (například **název produktu**), přidejte chybějící položky a uložte, může trvat několik minut, než ověřování pracovat.</span><span class="sxs-lookup"><span data-stu-id="6022b-183">If you miss a required setting (for example **product name**), add the missing item and save, it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="6022b-184">Budete přesměrováni na web google, kde bude zadejte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="6022b-184">You will be redirected to the google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="6022b-185">Po zadání přihlašovacích údajů, zobrazí se výzva k udělit oprávnění k webové aplikaci, kterou jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="6022b-185">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="6022b-186">Klikněte na tlačítko **přijmout**.</span><span class="sxs-lookup"><span data-stu-id="6022b-186">Click **Accept**.</span></span> <span data-ttu-id="6022b-187">Budete teď přesměrováni zpět **zaregistrovat** stránka MvcAuth aplikace, kde můžete zaregistrovat účet služby Google.</span><span class="sxs-lookup"><span data-stu-id="6022b-187">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="6022b-188">Máte možnost změny názvu registrace místní e-mailu, který se používá pro váš účet z Gmailu, ale chcete obecně zachovat výchozí e-mailový alias (to znamená, ten, který jste použili pro ověřování).</span><span class="sxs-lookup"><span data-stu-id="6022b-188">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="6022b-189">Klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="6022b-189">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="6022b-190">Vytvoření aplikace v síti Facebook a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="6022b-190">Creating the app in Facebook and connecting the app to the project</span></span>

<span data-ttu-id="6022b-191">Pro ověřování sítě Facebook OAuth2 musíte zkopírovat do projektu některá nastavení z aplikace, která vytvoříte ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="6022b-191">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="6022b-192">V prohlížeči přejděte na [https://developers.facebook.com/apps](https://developers.facebook.com/apps) a přihlaste se zadáním přihlašovacích údajů služby Facebook.</span><span class="sxs-lookup"><span data-stu-id="6022b-192">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="6022b-193">Pokud nejsou už registrovaný jako vývojář Facebook, klikněte na tlačítko **zaregistrujte se jako vývojář** a postupujte podle pokynů k registraci.</span><span class="sxs-lookup"><span data-stu-id="6022b-193">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="6022b-194">Na **aplikace** , klikněte na **vytvořit novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="6022b-194">On the **Apps** tab, click **Create New App**.</span></span>

    ![Vytvoření nové aplikace](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="6022b-196">Zadejte **název aplikace** a **kategorie**, pak klikněte na tlačítko **vytvořit aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="6022b-196">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="6022b-197">Toto musí být jedinečný mezi Facebook.</span><span class="sxs-lookup"><span data-stu-id="6022b-197">This must be unique across Facebook.</span></span> <span data-ttu-id="6022b-198">**Aplikace Namespace** je součástí adresu URL, kterou vaše aplikace bude používat pro přístup k aplikaci Facebook pro ověření (například https://apps.facebook.com/ {aplikace Namespace}).</span><span class="sxs-lookup"><span data-stu-id="6022b-198">The **App Namespace** is the part of the URL that your App will use to access the Facebook application for authentication (for example, https://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="6022b-199">Pokud neurčíte **aplikace Namespace**, **ID aplikace** se použije pro adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6022b-199">If you don't specify an **App Namespace**, the **App ID** will be used for the URL.</span></span> <span data-ttu-id="6022b-200">**ID aplikace** je číslo dlouho generované systémem, který se zobrazí v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="6022b-200">The **App ID** is a long system-generated number that you will see in the next step.</span></span>

    ![Vytvořit novou aplikaci dialogové okno](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="6022b-202">Odešlete kontrola zabezpečení Standardní.</span><span class="sxs-lookup"><span data-stu-id="6022b-202">Submit the standard security check.</span></span>

    ![Kontrola zabezpečení](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="6022b-204">Vyberte **nastavení** pro pruhu levé nabídce![ Panel nabídek Facebook pro vývojáře](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="6022b-204">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="6022b-205">Na **základní** vyberte nastavení části stránky **přidejte platformu** k určení, že přidáváte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6022b-205">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="6022b-206">![Základní nastavení](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="6022b-206">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="6022b-207">Vyberte **webu** z možností platformu.</span><span class="sxs-lookup"><span data-stu-id="6022b-207">Select **Website** from the platform choices.</span></span>  
  
    ![Volby platformy](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="6022b-209">Poznamenejte si vaše **ID aplikace** a **tajný klíč aplikace** tak, aby později v tomto kurzu můžete přidat i do aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="6022b-209">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="6022b-210">Také přidat adresu URL vašeho webu (`https://localhost:44300/`) k testování aplikace MVC.</span><span class="sxs-lookup"><span data-stu-id="6022b-210">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="6022b-211">Také přidat **e-mailu kontaktujte**.</span><span class="sxs-lookup"><span data-stu-id="6022b-211">Also, add a **Contact Email**.</span></span> <span data-ttu-id="6022b-212">Pak vyberte **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="6022b-212">Then, select **Save Changes**.</span></span>   

    ![Stránka Podrobnosti základní aplikace](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="6022b-214">Všimněte si, že pouze bude možné provést ověření pomocí e-mailový alias, který je zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="6022b-214">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="6022b-215">Ostatní uživatele a testovací účty, nebude možné k registraci.</span><span class="sxs-lookup"><span data-stu-id="6022b-215">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="6022b-216">Můžete udělit další Facebook účty přístup k aplikaci na k síti Facebook **vývojáře role** kartě.</span><span class="sxs-lookup"><span data-stu-id="6022b-216">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="6022b-217">V sadě Visual Studio otevřete *aplikace\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="6022b-217">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="6022b-218">Zkopírujte a vložte **AppId** a **tajný klíč aplikace** do `UseFacebookAuthentication` metoda.</span><span class="sxs-lookup"><span data-stu-id="6022b-218">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="6022b-219">**AppId** a **tajný klíč aplikace** hodnoty zobrazené níže jsou příklady a nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="6022b-219">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="6022b-220">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="6022b-220">Click **Save Changes**.</span></span>
13. <span data-ttu-id="6022b-221">Stiskněte klávesu **CTRL + F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="6022b-221">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="6022b-222">Vyberte **přihlásit** zobrazíte stránku přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6022b-222">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="6022b-223">Klikněte na tlačítko **Facebook** pod **přihlásit pomocí jiné služby.**</span><span class="sxs-lookup"><span data-stu-id="6022b-223">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="6022b-224">Zadejte přihlašovací údaje služby Facebook.</span><span class="sxs-lookup"><span data-stu-id="6022b-224">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="6022b-225">Zobrazí se výzva k udělení oprávnění pro přístup k vašim veřejný profil a seznamu friend aplikace.</span><span class="sxs-lookup"><span data-stu-id="6022b-225">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Podrobnosti o aplikaci Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="6022b-227">Jste nyní přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="6022b-227">You are now logged in.</span></span> <span data-ttu-id="6022b-228">Nyní můžete zaregistrovat tento účet s aplikací.</span><span class="sxs-lookup"><span data-stu-id="6022b-228">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="6022b-229">Při registraci, bude položka přidána do *uživatelé* tabulky databáze členství.</span><span class="sxs-lookup"><span data-stu-id="6022b-229">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="6022b-230">Zkontrolujte Data členství</span><span class="sxs-lookup"><span data-stu-id="6022b-230">Examine the Membership Data</span></span>

<span data-ttu-id="6022b-231">V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="6022b-231">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="6022b-232">Rozbalte položku **objekt DefaultConnection (MvcAuth)**, rozbalte položku **tabulky**, klikněte pravým tlačítkem na **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="6022b-232">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![data tabulky aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="6022b-234">Přidat Data profilu pro třídu uživatele</span><span class="sxs-lookup"><span data-stu-id="6022b-234">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="6022b-235">V této části přidáte datum narození a domácí městě k datům uživatele během registrace, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="6022b-235">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![REG s domácí města a Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="6022b-237">Otevřete *Models\IdentityModels.cs* souboru a přidejte narození datum a z domova městě vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="6022b-237">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="6022b-238">Otevřete *Models\AccountViewModels.cs* souboru a sada narození datum a z domova městě vlastnosti v `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="6022b-238">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="6022b-239">Otevřete *Controllers\AccountController.cs* souboru a přidejte kód pro narození datum a z domova městě ve `ExternalLoginConfirmation` metody akce, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="6022b-239">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="6022b-240">Přidat datum narození a domácí městě, jež *Views\Account\ExternalLoginConfirmation.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="6022b-240">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="6022b-241">Abyste mohli znovu zaregistrovat svůj účet Facebook s vaší aplikací a ověřte, že můžete přidat nové datum narození a informace o profilu domácí města, odstraňte tuto databázi členství.</span><span class="sxs-lookup"><span data-stu-id="6022b-241">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="6022b-242">Z **Průzkumníku řešení**, klikněte **zobrazit všechny soubory** ikonu a potom klikněte pravým tlačítkem *přidat\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="6022b-242">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="6022b-243">Z **nástroje** nabídky, klikněte na tlačítko **Manager balíček NuGet**, pak klikněte na tlačítko **Konzola správce balíčků** (pomocí PMC).</span><span class="sxs-lookup"><span data-stu-id="6022b-243">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="6022b-244">Zadejte následující příkazy v pomocí PMC.</span><span class="sxs-lookup"><span data-stu-id="6022b-244">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="6022b-245">Příkaz enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="6022b-245">Enable-Migrations</span></span>
2. <span data-ttu-id="6022b-246">Init – přidat migrace</span><span class="sxs-lookup"><span data-stu-id="6022b-246">Add-Migration Init</span></span>
3. <span data-ttu-id="6022b-247">Update-Database</span><span class="sxs-lookup"><span data-stu-id="6022b-247">Update-Database</span></span>

<span data-ttu-id="6022b-248">Spusťte aplikaci a používat sítě FaceBook a Google přihlášení a registrace někteří uživatelé.</span><span class="sxs-lookup"><span data-stu-id="6022b-248">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="6022b-249">Zkontrolujte Data členství</span><span class="sxs-lookup"><span data-stu-id="6022b-249">Examine the Membership Data</span></span>

<span data-ttu-id="6022b-250">V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="6022b-250">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="6022b-251">Klikněte pravým tlačítkem na **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="6022b-251">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="6022b-252">`HomeTown` a `BirthDate` pole, které jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="6022b-252">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="6022b-253">Odhlášení vaší aplikace a přihlášením pomocí jiného účtu</span><span class="sxs-lookup"><span data-stu-id="6022b-253">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="6022b-254">Přihlaste se k vaší aplikace pomocí služby Facebook,,, potom se přihlaste a pokuste se přihlásit znovu s jiným účtem Facebook (pomocí stejného prohlížeče), budete okamžitě přihlášeni předchozí, které jste použili účet služby Facebook.</span><span class="sxs-lookup"><span data-stu-id="6022b-254">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="6022b-255">Chcete-li použít jiný účet, budete muset přejděte do sítě Facebook a odhlaste v síti Facebook.</span><span class="sxs-lookup"><span data-stu-id="6022b-255">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="6022b-256">Stejné pravidlo se vztahuje na všechny ostatní 3. stran zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="6022b-256">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="6022b-257">Alternativně můžete protokolovat se pomocí jiného účtu pomocí jiné prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6022b-257">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6022b-258">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6022b-258">Next Steps</span></span>

<span data-ttu-id="6022b-259">V tématu [představení zprostředkovatele zabezpečení Yahoo a OAuth pro LinkedIn pro OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) podle Jerrie Pelser pokyny Yahoo a LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="6022b-259">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="6022b-260">Najdete v článku na Jerrie velmi přihlášení prostřednictvím sociální sítě tlačítka pro ASP.NET MVC 5 se získat povolení přihlášení prostřednictvím sociální sítě tlačítka.</span><span class="sxs-lookup"><span data-stu-id="6022b-260">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="6022b-261">Využijte tento kurz [vytvoření aplikace ASP.NET MVC pomocí ověřování a databázi SQL a nasazení do Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), který pokračuje v tomto kurzu a zobrazuje následující:</span><span class="sxs-lookup"><span data-stu-id="6022b-261">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="6022b-262">Postup nasazení aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="6022b-262">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="6022b-263">Jak zabezpečit aplikace s rolemi.</span><span class="sxs-lookup"><span data-stu-id="6022b-263">How to secure you app with roles.</span></span>
3. <span data-ttu-id="6022b-264">Jak zabezpečit vaší aplikace pomocí [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) a [Autorizovat](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtry.</span><span class="sxs-lookup"><span data-stu-id="6022b-264">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/en-us/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="6022b-265">Jak používat členské rozhraní API pro přidání uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="6022b-265">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="6022b-266">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit.</span><span class="sxs-lookup"><span data-stu-id="6022b-266">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="6022b-267">Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="6022b-267">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="6022b-268">Dokonce můžete požádat o a hlasovat o nové funkce, které mají být přidány do technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6022b-268">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="6022b-269">Například můžete hlasovat pro nástroj, který [vytvoření a správě uživatelů a rolí.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="6022b-269">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="6022b-270">Dobrý vysvětlení o fungování technologie ASP.NET externí ověřovací služby najdete v tématu Roberta Mcmurrayho [externí ověřovací služby](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="6022b-270">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="6022b-271">Robert je článek také obsahuje podrobnosti v povolení ověřování Microsoft a Twitter.</span><span class="sxs-lookup"><span data-stu-id="6022b-271">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="6022b-272">Tní Dykstra je vynikající [kurz EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) ukazuje, jak pracovat s rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6022b-272">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>