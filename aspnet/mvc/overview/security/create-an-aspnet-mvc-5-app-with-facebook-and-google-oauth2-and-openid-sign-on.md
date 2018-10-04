---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: MVC 5 vytvořte aplikaci pomocí služby Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5, která umožňuje uživatelům přihlášení pomocí OAuth 2.0 s přihlašovacími údaji z externí authenti...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 611a4b59b2ea2eee771f4060fb5d5af041b2ccc6
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577766"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="1774c-103">Vytvoření aplikace ASP.NET MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování (C#)</span><span class="sxs-lookup"><span data-stu-id="1774c-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="1774c-104">Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="1774c-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="1774c-105">V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5, která umožňuje uživatelům přihlášení pomocí [OAuth 2.0](http://oauth.net/2/) s přihlašovacími údaji z externí zprostředkovatel ověřování, jako je Facebook, Twitter, LinkedIn, Microsoft nebo Google.</span><span class="sxs-lookup"><span data-stu-id="1774c-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="1774c-106">Pro zjednodušení tento kurz se zaměřuje na práci s přihlašovacích údajů z Facebooku nebo Googlu.</span><span class="sxs-lookup"><span data-stu-id="1774c-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="1774c-107">Povolení tyto přihlašovací údaje ve webových stránek poskytuje výraznou výhodu, protože milionům uživatelů již mají účty u těchto externích poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="1774c-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="1774c-108">Tito uživatelé mohou být více ochotni zaregistrujte svůj web, pokud nemají k vytvoření a zapamatovat si novou sadu přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1774c-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="1774c-109">Viz také [aplikace ASP.NET MVC 5 s SMS a e-mailu Dvojúrovňového ověřování](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="1774c-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="1774c-110">Kurz také ukazuje, jak přidat data profilu pro uživatele a jak používat rozhraní API členství k přidání rolí.</span><span class="sxs-lookup"><span data-stu-id="1774c-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="1774c-111">V tomto kurzu zapsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (podle mě prosím na Twitteru: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="1774c-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="1774c-112">Začínáme</span><span class="sxs-lookup"><span data-stu-id="1774c-112">Getting Started</span></span>

<span data-ttu-id="1774c-113">Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="1774c-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="1774c-114">Instalace sady Visual Studio [2013 s aktualizací 3](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="1774c-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="1774c-115">Nápovědu k Dropboxu, Githubu, Linkedin, Instagramu, vyrovnávací paměti, Salesforce, datový proud, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! a další, najdete v tomto [ukázkový projekt](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="1774c-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="1774c-116">Musíte nainstalovat Visual Studio [2013 s aktualizací 3](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší, k používání služby Google OAuth 2 a ladit lokálně bez upozornění protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="1774c-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="1774c-117">Klikněte na tlačítko **nový projekt** z **Start** stránky, nebo můžete použít v nabídce a vyberte **souboru**a potom **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="1774c-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="1774c-118">Vytvoření vaší první aplikace</span><span class="sxs-lookup"><span data-stu-id="1774c-118">Creating Your First Application</span></span>

<span data-ttu-id="1774c-119">Klikněte na tlačítko **nový projekt**a pak vyberte **Visual C#** na levé straně, pak **webové** a pak vyberte **webová aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="1774c-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="1774c-120">Pojmenujte svůj projekt "MvcAuth" a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1774c-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="1774c-121">V **nový projekt ASP.NET** dialogového okna, klikněte na tlačítko **MVC**.</span><span class="sxs-lookup"><span data-stu-id="1774c-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="1774c-122">Pokud není ověřování **jednotlivé uživatelské účty**, klikněte na tlačítko **změna ověřování** tlačítko a vyberte **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="1774c-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="1774c-123">Zaškrtnutím **hostovat v cloudu**, aplikace bude velmi snadno hostovat v Azure.</span><span class="sxs-lookup"><span data-stu-id="1774c-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="1774c-124">Pokud jste vybrali **hostovat v cloudu**, dokončete dialogové okno Konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="1774c-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="1774c-125">Pomocí balíčku NuGet aktualizujte na nejnovější middleware OWIN</span><span class="sxs-lookup"><span data-stu-id="1774c-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="1774c-126">Aktualizovat pomocí Správce balíčků NuGet [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="1774c-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="1774c-127">Vyberte **aktualizace** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="1774c-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="1774c-128">Můžete kliknout na **Aktualizovat vše** tlačítko nebo je můžete vyhledat pouze balíčky OWIN (viz následující obrázek):</span><span class="sxs-lookup"><span data-stu-id="1774c-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="1774c-129">Na následujícím obrázku jsou zobrazeny pouze OWIN balíčky:</span><span class="sxs-lookup"><span data-stu-id="1774c-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="1774c-130">Z balíčku správce konzoly (konzolu PMC), můžete zadat `Update-Package` příkaz, který bude aktualizovat všechny balíčky.</span><span class="sxs-lookup"><span data-stu-id="1774c-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="1774c-131">Stisknutím klávesy **F5** nebo **Ctrl + F5** ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="1774c-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="1774c-132">Na následujícím obrázku je číslo portu 1234.</span><span class="sxs-lookup"><span data-stu-id="1774c-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="1774c-133">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="1774c-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="1774c-134">V závislosti na velikosti okna prohlížeče, možná budete muset klikněte na ikonu navigační zobrazíte **Domů**, **o**, **kontakt**, **zaregistrovat**a **přihlášení** odkazy.</span><span class="sxs-lookup"><span data-stu-id="1774c-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="1774c-135">Nastavení protokolu SSL v projektu</span><span class="sxs-lookup"><span data-stu-id="1774c-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="1774c-136">Pro připojení k zprostředkovatele ověřování, jako je Google nebo Facebook, musíte nastavit službu IIS Express pro použití protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="1774c-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="1774c-137">Je důležité zachovat po přihlášení pomocí protokolu SSL a ne zase k protokolu HTTP, vaše přihlašovací soubor cookie je stejně jako tajný kód jako uživatelské jméno a heslo a bez použití protokolu SSL, které e-mail posíláte, ve formátu prostého textu sítí.</span><span class="sxs-lookup"><span data-stu-id="1774c-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="1774c-138">Kromě toho jsme už používá čas k provedení signalizace a zabezpečený kanál (která je větší část kvůli tomu HTTPS pomalejší než HTTP) předtím, než kanál MVC se spustí, proto přesměrování zpět k protokolu HTTP, poté, co jste se přihlásili neprovede aktuální požadavek nebo budoucnost požadavky na mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="1774c-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="1774c-139">V **Průzkumníka řešení**, klikněte na tlačítko **MvcAuth** projektu.</span><span class="sxs-lookup"><span data-stu-id="1774c-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="1774c-140">Stisknutím klávesy F4 zobrazíte vlastnosti projektu.</span><span class="sxs-lookup"><span data-stu-id="1774c-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="1774c-141">Alternativně z **zobrazení** nabídce vyberete **okno vlastností**.</span><span class="sxs-lookup"><span data-stu-id="1774c-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="1774c-142">Změna **protokol SSL povolený** na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="1774c-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="1774c-143">Zkopírujte adresu URL protokolu SSL (který bude `https://localhost:44300/` Pokud jste vytvořili ostatní projekty SSL).</span><span class="sxs-lookup"><span data-stu-id="1774c-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="1774c-144">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **MvcAuth** projektu a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1774c-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="1774c-145">Vyberte **webové** kartu a pak vložte adresu URL protokolu SSL do **adresa Url projektu** pole.</span><span class="sxs-lookup"><span data-stu-id="1774c-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="1774c-146">Uložte soubor (seznam Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="1774c-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="1774c-147">Budete potřebovat tato adresa URL ke konfiguraci ověřování aplikace Facebook nebo Google.</span><span class="sxs-lookup"><span data-stu-id="1774c-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="1774c-148">Přidat [atribut RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atribut `Home` řadič tak, aby vyžadovala všechny požadavky musí používat protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1774c-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="1774c-149">Více zabezpečený přístup, je přidat [atribut RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtr, aby se aplikace.</span><span class="sxs-lookup"><span data-stu-id="1774c-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="1774c-150">V části &quot;chránit aplikace pomocí protokolu SSL a atribut Autorizovat&quot; v mé tutoral [vytvoření aplikace ASP.NET MVC pomocí ověření a SQL DB a nasazení do služby Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="1774c-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="1774c-151">Část kontroler Home je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="1774c-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="1774c-152">Stisknutím kláves CTRL + F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1774c-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="1774c-153">Pokud jste nainstalovali certifikát v minulosti, můžete přeskočit zbývající část a přejděte do [vytvoření aplikace služby Google OAuth 2 a připojení aplikace k projektu](#goog), v opačném případě postupujte podle pokynů důvěřovat certifikát podepsaný svým držitelem certifikát, který vygenerovala služba IIS Express.</span><span class="sxs-lookup"><span data-stu-id="1774c-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="1774c-154">Čtení **upozornění zabezpečení** dialogového okna a pak klikněte na tlačítko **Ano** Pokud chcete nainstalovat certifikát představující localhost.</span><span class="sxs-lookup"><span data-stu-id="1774c-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="1774c-155">IE ukazuje, *Domů* stránce a nebudou již existovat žádná upozornění protokolu SSL.</span><span class="sxs-lookup"><span data-stu-id="1774c-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="1774c-156">Google Chrome také certifikát přijme a zobrazí obsah HTTPS bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="1774c-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="1774c-157">Firefox používá své vlastní úložiště certifikátů, takže se zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="1774c-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="1774c-158">Pro naši aplikaci můžete bez obav kliknout na **beru na vědomí rizika**.</span><span class="sxs-lookup"><span data-stu-id="1774c-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="1774c-159">Vytvoření aplikace služby Google OAuth 2 a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="1774c-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="1774c-160">Aktuální Google OAuth pokyny najdete v tématu [ověřování Google konfigurace v ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="1774c-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="1774c-161">Přejděte [konzole pro vývojáře Google](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="1774c-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="1774c-162">Pokud jste ještě nevytvořili projektu před, vyberte **pověření** karta vlevo a pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1774c-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="1774c-163">Na levé kartě klikněte **pověření**.</span><span class="sxs-lookup"><span data-stu-id="1774c-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="1774c-164">Klikněte na tlačítko **Vytvořte přihlašovací údaje** pak **ID klienta OAuth**.</span><span class="sxs-lookup"><span data-stu-id="1774c-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="1774c-165">V **vytvořit ID klienta** dialogového okna, ponechte výchozí **webovou aplikaci** pro typ aplikace.</span><span class="sxs-lookup"><span data-stu-id="1774c-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="1774c-166">Nastavte **oprávnění JavaScript** počátky k adresa URL protokolu SSL, které jste použili výše (`https://localhost:44300/` Pokud jste vytvořili ostatní projekty SSL)</span><span class="sxs-lookup"><span data-stu-id="1774c-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="1774c-167">Nastavte **identifikátor URI pro přesměrování autorizovaní** na:</span><span class="sxs-lookup"><span data-stu-id="1774c-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="1774c-168">Kliknutím na položku nabídky souhlas OAuth obrazovky a pak nastavte vaše e-mailovou adresu a název produktu.</span><span class="sxs-lookup"><span data-stu-id="1774c-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="1774c-169">Po dokončení klikněte na formulář **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1774c-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="1774c-170">Kliknutím na položku nabídky knihovny, Hledat **rozhraní API Google +**, klikněte na něj a stiskněte povolit.</span><span class="sxs-lookup"><span data-stu-id="1774c-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="1774c-171">Následující obrázek ukazuje povolených rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="1774c-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="1774c-172">Ze služby Google API rozhraní API Správce, přejděte **pověření** kartu získat **ID klienta**.</span><span class="sxs-lookup"><span data-stu-id="1774c-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="1774c-173">Stažení a uložení souboru JSON s tajných klíčů aplikací.</span><span class="sxs-lookup"><span data-stu-id="1774c-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="1774c-174">Zkopírujte a vložte **ClientId** a **ClientSecret** do `UseGoogleAuthentication` metoda nalezena v *Startup.Auth.cs* ve *App_Start* složky.</span><span class="sxs-lookup"><span data-stu-id="1774c-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="1774c-175">**ClientId** a **ClientSecret** hodnoty zobrazené níže jsou uvedeny ukázky a nefungují.</span><span class="sxs-lookup"><span data-stu-id="1774c-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="1774c-176">Zabezpečení – nikdy ukládání citlivých dat ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="1774c-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="1774c-177">Účet a přihlašovací údaje se přidají do výše uvedený pro zjednodušení ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="1774c-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="1774c-178">Zobrazit [osvědčené postupy pro nasazení hesel a dalších citlivých dat do ASP.NET a službě Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="1774c-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="1774c-179">Stisknutím klávesy **CTRL + F5** sestavíte a spustíte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1774c-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="1774c-180">Klikněte na tlačítko **přihlášení** odkaz.</span><span class="sxs-lookup"><span data-stu-id="1774c-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="1774c-181">V části **přihlášení pomocí jiné služby**, klikněte na tlačítko **Google**.</span><span class="sxs-lookup"><span data-stu-id="1774c-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="1774c-182">Pokud přeskočíte, některé z výše uvedených kroků se zobrazí chyba HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="1774c-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="1774c-183">Znovu zkontrolujte vaše výše uvedených kroků.</span><span class="sxs-lookup"><span data-stu-id="1774c-183">Recheck your steps above.</span></span> <span data-ttu-id="1774c-184">Pokud přeskočíte požadované nastavení (například **název produktu**), přidejte chybějící položky a uložte; může trvat několik minut, aby ověřování fungovalo.</span><span class="sxs-lookup"><span data-stu-id="1774c-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="1774c-185">Budete přesměrováni na web Google, kam zadáte svoje přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="1774c-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="1774c-186">Po zadání svých přihlašovacích údajů se výzva k udělení oprávnění k webové aplikaci, kterou jste právě vytvořili:</span><span class="sxs-lookup"><span data-stu-id="1774c-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="1774c-187">Klikněte na tlačítko **přijmout**.</span><span class="sxs-lookup"><span data-stu-id="1774c-187">Click **Accept**.</span></span> <span data-ttu-id="1774c-188">Budete teď přesměrováni zpět **zaregistrovat** stránky MvcAuth aplikace, ve kterém můžete zaregistrovat účet služby Google.</span><span class="sxs-lookup"><span data-stu-id="1774c-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="1774c-189">Máte možnost změnit název registrace místní e-mailu použitý pro váš účet Gmailu, ale obecně je žádoucí uchovat výchozí e-mailový alias (to znamená, ten, který jste použili pro ověřování).</span><span class="sxs-lookup"><span data-stu-id="1774c-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="1774c-190">Klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="1774c-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="1774c-191">Vytvoření aplikace na Facebooku a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="1774c-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="1774c-192">Aktuální Facebook OAuth2 ověřování najdete v tématu [ověřování konfigurace služby Facebook](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="1774c-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="1774c-193">Prozkoumejte Data členství</span><span class="sxs-lookup"><span data-stu-id="1774c-193">Examine the Membership Data</span></span>

<span data-ttu-id="1774c-194">V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="1774c-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="1774c-195">Rozbalte **objekt DefaultConnection (MvcAuth)**, rozbalte **tabulky**, klikněte pravým tlačítkem myši **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="1774c-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers tabulkových dat](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="1774c-197">Přidání dat profilu do třídy uživatelů</span><span class="sxs-lookup"><span data-stu-id="1774c-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="1774c-198">V této části přidáte datum narození a domácí městě k uživatelským datům během registrace, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="1774c-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![REG s domácí města a Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="1774c-200">Otevřít *Models\IdentityModels.cs* a přidejte narození data a Domovská stránka městě vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1774c-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="1774c-201">Otevřít *Models\AccountViewModels.cs* souboru a sada narození vlastnosti data a Domovská stránka městě v `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="1774c-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="1774c-202">Otevřít *Controllers\AccountController.cs* a přidejte kód pro město Domovská stránka a datum narození v `ExternalLoginConfirmation` metody akce, jak je znázorněno:</span><span class="sxs-lookup"><span data-stu-id="1774c-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="1774c-203">Přidejte datum narození a domácí městě, jež *Views\Account\ExternalLoginConfirmation.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="1774c-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="1774c-204">Odstraňte databázi členství, aby znovu zaregistrovat Facebookový účet s vaší aplikací a ověřte, že můžete přidat nové datum narození a informace o profilu domácí města.</span><span class="sxs-lookup"><span data-stu-id="1774c-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="1774c-205">Z **Průzkumníka řešení**, klikněte na tlačítko **zobrazit všechny soubory** ikonu a pak klikněte pravým tlačítkem *přidat\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* a klikněte na tlačítko **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="1774c-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="1774c-206">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet**, pak klikněte na tlačítko **Konzola správce balíčků** (PMC).</span><span class="sxs-lookup"><span data-stu-id="1774c-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="1774c-207">Zadejte následující příkazy v konzole PMC.</span><span class="sxs-lookup"><span data-stu-id="1774c-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="1774c-208">Povolení migrace</span><span class="sxs-lookup"><span data-stu-id="1774c-208">Enable-Migrations</span></span>
2. <span data-ttu-id="1774c-209">Přidejte migraci Init</span><span class="sxs-lookup"><span data-stu-id="1774c-209">Add-Migration Init</span></span>
3. <span data-ttu-id="1774c-210">Aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="1774c-210">Update-Database</span></span>

<span data-ttu-id="1774c-211">Spusťte aplikaci a používat k přihlášení a registraci někteří uživatelé Facebooku nebo Googlu.</span><span class="sxs-lookup"><span data-stu-id="1774c-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="1774c-212">Prozkoumejte Data členství</span><span class="sxs-lookup"><span data-stu-id="1774c-212">Examine the Membership Data</span></span>

<span data-ttu-id="1774c-213">V **zobrazení** nabídky, klikněte na tlačítko **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="1774c-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="1774c-214">Klikněte pravým tlačítkem myši **AspNetUsers** a klikněte na tlačítko **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="1774c-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="1774c-215">`HomeTown` a `BirthDate` níže jsou zobrazena pole.</span><span class="sxs-lookup"><span data-stu-id="1774c-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="1774c-216">Odhlášení od vaší aplikace a přihlásíte se pomocí jiného účtu</span><span class="sxs-lookup"><span data-stu-id="1774c-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="1774c-217">Pokud přihlášení do vaší aplikace pomocí Facebooku a potom odhlaste se a zkuste se přihlásit znovu s jiným účtem Facebooku (pomocí stejného prohlížeče), můžete se ihned přihlášeni ke předchozí Facebookový účet, který jste použili.</span><span class="sxs-lookup"><span data-stu-id="1774c-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="1774c-218">Chcete-li použít jiný účet, budete muset přejít na Facebook a odhlášení na Facebooku.</span><span class="sxs-lookup"><span data-stu-id="1774c-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="1774c-219">Stejné pravidlo platí pro všechny ostatní 3. stran zprostředkovatele ověřování.</span><span class="sxs-lookup"><span data-stu-id="1774c-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="1774c-220">Alternativně můžete protokolovat se pomocí jiného účtu s použitím jiného prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="1774c-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1774c-221">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1774c-221">Next Steps</span></span>

<span data-ttu-id="1774c-222">V tématu [představení zprostředkovatele zabezpečení Yahoo a OAuth pro LinkedIn pro OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) podle Jerrie Pelser pokyny Yahoo a LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="1774c-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="1774c-223">Naleznete v tématu Jerrie to je dobré přihlášení prostřednictvím sociální sítě tlačítka pro ASP.NET MVC 5 se získat tlačítka povolit přihlášení prostřednictvím sociální sítě.</span><span class="sxs-lookup"><span data-stu-id="1774c-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="1774c-224">Využijte tento kurz [vytvoření aplikace ASP.NET MVC pomocí ověření a SQL DB a nasazení do služby Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), který pokračuje v tomto kurzu a zobrazuje následující:</span><span class="sxs-lookup"><span data-stu-id="1774c-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="1774c-225">Postup nasazení aplikace do Azure.</span><span class="sxs-lookup"><span data-stu-id="1774c-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="1774c-226">Jak zabezpečit aplikaci pomocí rolí.</span><span class="sxs-lookup"><span data-stu-id="1774c-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="1774c-227">Jak zabezpečit svou aplikaci pomocí [atribut RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) a [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtry.</span><span class="sxs-lookup"><span data-stu-id="1774c-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="1774c-228">Jak používat členské rozhraní API pro přidání uživatelů a rolí.</span><span class="sxs-lookup"><span data-stu-id="1774c-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="1774c-229">Jak vám v tomto kurzu líbilo a co můžeme zlepšit nám prosím zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="1774c-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="1774c-230">Můžete také požádat o nový témat na [Show Me jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="1774c-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="1774c-231">Dokonce je možné požádat o a hlasovat o nové funkce, které se přidají do technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1774c-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="1774c-232">Například můžete hlasovat pro nástroj pro [vytvoření a správě uživatelů a rolí.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="1774c-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="1774c-233">Dobré vysvětlení fungování technologie ASP.NET externí ověřovací služby najdete v tématu Robert McMurray [externí ověřovací služby](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="1774c-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="1774c-234">Robert na článek taky obsahuje podrobnosti při povolení ověřování společnosti Microsoft a Twitter.</span><span class="sxs-lookup"><span data-stu-id="1774c-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="1774c-235">Petr Dykstra je vynikající [kurz EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) ukazuje, jak pracovat s rozhraním Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1774c-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
