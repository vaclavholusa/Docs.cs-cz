---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Povolení ověřování systému Windows v Katana | Microsoft Docs
author: MikeWasson
description: 'Tento článek ukazuje, jak povolit ověřování systému Windows v Katana. Pokrývá dva scénáře: pomocí služby IIS na hostitele Katana a pomocí HttpListener hostování na vlastním Kat...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28033967"
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="830de-104">Povolení ověřování systému Windows v Katana</span><span class="sxs-lookup"><span data-stu-id="830de-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="830de-105">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="830de-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="830de-106">Tento článek ukazuje, jak povolit ověřování systému Windows v Katana.</span><span class="sxs-lookup"><span data-stu-id="830de-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="830de-107">Pokrývá dva scénáře: pomocí služby IIS na hostitele Katana a pomocí HttpListener hostování na vlastním Katana ve vlastní proces.</span><span class="sxs-lookup"><span data-stu-id="830de-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="830de-108">Díky Jiří Dorrans, David Matson a Ross Jan kontroly v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="830de-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="830de-109">Implementace společnosti Microsoft je Katana [OWIN](http://owin.org/), Open Web Interface pro .NET.</span><span class="sxs-lookup"><span data-stu-id="830de-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="830de-110">Můžete si přečíst Úvod do OWIN a Katana [zde](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="830de-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="830de-111">Architektura OWIN má několik vrstev:</span><span class="sxs-lookup"><span data-stu-id="830de-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="830de-112">Hostitele: Spravuje proces, ve kterém se spouští kanálu OWIN.</span><span class="sxs-lookup"><span data-stu-id="830de-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="830de-113">Server: Otevře soketu sítě a naslouchá požadavkům.</span><span class="sxs-lookup"><span data-stu-id="830de-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="830de-114">Middleware: Zpracuje požadavek HTTP a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="830de-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="830de-115">Katana aktuálně poskytuje dva servery, které podporují integrované ověřování systému Windows:</span><span class="sxs-lookup"><span data-stu-id="830de-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="830de-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="830de-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="830de-117">Pomocí služby IIS s kanálu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="830de-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="830de-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="830de-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="830de-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="830de-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="830de-120">Tento server je aktuálně výchozí možností při vlastním hostování Katana.</span><span class="sxs-lookup"><span data-stu-id="830de-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="830de-121">Katana neposkytuje aktuálně OWIN middleware pro ověřování systému Windows, protože tato funkce je již k dispozici na serverech.</span><span class="sxs-lookup"><span data-stu-id="830de-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="830de-122">Ověřování systému Windows ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="830de-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="830de-123">Pomocí Microsoft.Owin.Host.SystemWeb, můžete jednoduše povolit ověřování systému Windows ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="830de-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="830de-124">Začněme tím, že vytvoříte novou aplikaci ASP.NET pomocí šablony projektu "Prázdný webové aplikace ASP.NET".</span><span class="sxs-lookup"><span data-stu-id="830de-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="830de-125">V dalším kroku přidání balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="830de-125">Next, add NuGet packages.</span></span> <span data-ttu-id="830de-126">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="830de-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="830de-127">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="830de-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="830de-128">Nyní přidejte třídu s názvem `Startup` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="830de-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="830de-129">To je všechno je potřeba vytvořit aplikaci "Hello, world" pro OWIN, spuštěná ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="830de-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="830de-130">Stisknutím klávesy F5 ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="830de-130">Press F5 to debug the application.</span></span> <span data-ttu-id="830de-131">Měli byste vidět "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="830de-131">You should see "Hello World!"</span></span> <span data-ttu-id="830de-132">v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="830de-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="830de-133">Dál jsme budete povolte ověřování systému Windows ve službě IIS Express.</span><span class="sxs-lookup"><span data-stu-id="830de-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="830de-134">Z **zobrazení** nabídce vyberte možnost **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="830de-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="830de-135">Klikněte na název projektu v Průzkumníku řešení Chcete-li zobrazit vlastnosti projektu.</span><span class="sxs-lookup"><span data-stu-id="830de-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="830de-136">V **vlastnosti** nastavte **anonymní ověřování** k **zakázané** a nastavte **ověřování systému Windows** k  **Povolit**.</span><span class="sxs-lookup"><span data-stu-id="830de-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="830de-137">Při spuštění aplikace ze sady Visual Studio, IIS Express bude vyžadovat přihlašovací údaje uživatele systému Windows.</span><span class="sxs-lookup"><span data-stu-id="830de-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="830de-138">To můžete zobrazit pomocí [Fiddler](http://fiddler2.com/home) nebo jiné HTTP nástroje ladění.</span><span class="sxs-lookup"><span data-stu-id="830de-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="830de-139">Tady je příklad odpověď HTTP:</span><span class="sxs-lookup"><span data-stu-id="830de-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="830de-140">V této odpovědi hlavičky WWW-Authenticate znamenat, že server podporuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protokol, který používá protokol Kerberos nebo NTLM.</span><span class="sxs-lookup"><span data-stu-id="830de-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="830de-141">Později, když nasazujete aplikace na server, použijte [tyto kroky](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) povolení ověřování systému Windows ve službě IIS na příslušném serveru.</span><span class="sxs-lookup"><span data-stu-id="830de-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="830de-142">Ověřování systému Windows v HttpListener</span><span class="sxs-lookup"><span data-stu-id="830de-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="830de-143">Pokud používáte Microsoft.Owin.Host.HttpListener k hostování na vlastním Katana, můžete povolit ověřování systému Windows přímo na **HttpListener** instance.</span><span class="sxs-lookup"><span data-stu-id="830de-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="830de-144">Nejprve vytvořte novou konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="830de-144">First, create a new console application.</span></span> <span data-ttu-id="830de-145">V dalším kroku přidání balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="830de-145">Next, add NuGet packages.</span></span> <span data-ttu-id="830de-146">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="830de-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="830de-147">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="830de-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="830de-148">Nyní přidejte třídu s názvem `Startup` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="830de-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="830de-149">Tato třída implementuje stejné "Hello, world" příklad z před, ale také k nastavení ověřování systému Windows jako schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="830de-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="830de-150">Uvnitř `Main` fungovat, spuštění kanálu OWIN:</span><span class="sxs-lookup"><span data-stu-id="830de-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="830de-151">V aplikaci Fiddler k potvrzení, že aplikace používá ověřování systému Windows můžete odeslat žádost:</span><span class="sxs-lookup"><span data-stu-id="830de-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="830de-152">Související témata</span><span class="sxs-lookup"><span data-stu-id="830de-152">Related Topics</span></span>

[<span data-ttu-id="830de-153">Přehled projektu Katana</span><span class="sxs-lookup"><span data-stu-id="830de-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="830de-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="830de-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="830de-155">Seznámení s OWIN ověřování pomocí formulářů v MVC 5</span><span class="sxs-lookup"><span data-stu-id="830de-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
