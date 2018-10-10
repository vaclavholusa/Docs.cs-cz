---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Povolení ověřování Windows v sadě Katana | Dokumentace Microsoftu
author: MikeWasson
description: 'Tento článek ukazuje, jak povolit ověřování Windows v sadě Katana. Popisuje dva scénáře: pomocí služby IIS pro hostování Katana a pomocí HttpListener k samoobslužnému hostování Kat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8afa2c9dfbe03a9874513f7d083adf7608f4218f
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910457"
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="c3edd-104">Povolení ověřování Windows v sadě Katana</span><span class="sxs-lookup"><span data-stu-id="c3edd-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="c3edd-105">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c3edd-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="c3edd-106">Tento článek ukazuje, jak povolit ověřování Windows v sadě Katana.</span><span class="sxs-lookup"><span data-stu-id="c3edd-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="c3edd-107">Popisuje dva scénáře: použití služby IIS pro hostování Katana a pomocí HttpListener Katana samoobslužné hostování ve vlastním procesu.</span><span class="sxs-lookup"><span data-stu-id="c3edd-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="c3edd-108">Děkujeme, že Jiří Dorrans, David Matson a Chris Ross revize v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c3edd-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="c3edd-109">Katana je implementace společnosti Microsoft [OWIN](http://owin.org/), Open Web Interface pro .NET.</span><span class="sxs-lookup"><span data-stu-id="c3edd-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="c3edd-110">Můžete si přečíst Úvod do OWIN a Katana [tady](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="c3edd-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="c3edd-111">Architektura OWIN má několik vrstev:</span><span class="sxs-lookup"><span data-stu-id="c3edd-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="c3edd-112">Hostitel: Spravuje proces, ve kterém se spouští kanál OWIN.</span><span class="sxs-lookup"><span data-stu-id="c3edd-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="c3edd-113">Server: Otevře soket sítě a čeká na požadavky.</span><span class="sxs-lookup"><span data-stu-id="c3edd-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="c3edd-114">Middleware: Zpracuje požadavek HTTP a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="c3edd-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="c3edd-115">Katana v současné době poskytuje dva servery, které podporují integrované ověřování Windows:</span><span class="sxs-lookup"><span data-stu-id="c3edd-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="c3edd-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="c3edd-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="c3edd-117">Používá službu IIS pomocí kanálu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c3edd-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="c3edd-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="c3edd-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="c3edd-119">Používá [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3edd-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="c3edd-120">Tento server je aktuálně výchozí možnost, při hostování na vlastním Katana.</span><span class="sxs-lookup"><span data-stu-id="c3edd-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="c3edd-121">Katana neposkytuje aktuálně OWIN middleware pro ověřování Windows, protože tato funkce je k dispozici na serverech.</span><span class="sxs-lookup"><span data-stu-id="c3edd-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>

## <a name="windows-authentication-in-iis"></a><span data-ttu-id="c3edd-122">Ověřování Windows ve službě IIS</span><span class="sxs-lookup"><span data-stu-id="c3edd-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="c3edd-123">Pomocí Microsoft.Owin.Host.SystemWeb, můžete jednoduše povolit ověřování Windows služby IIS.</span><span class="sxs-lookup"><span data-stu-id="c3edd-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="c3edd-124">Začněme tím, že vytvoříte novou aplikaci ASP.NET, pomocí šablony projektu "Prázdná webová aplikace ASP.NET".</span><span class="sxs-lookup"><span data-stu-id="c3edd-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="c3edd-125">Dále přidejte balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="c3edd-125">Next, add NuGet packages.</span></span> <span data-ttu-id="c3edd-126">Z **nástroje** příkaz **Správce balíčků NuGet**, vyberte **konzoly Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="c3edd-126">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c3edd-127">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c3edd-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="c3edd-128">Nyní přidejte třídu pojmenovanou `Startup` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c3edd-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="c3edd-129">To je všechno, co potřebujete k vytvoření aplikace "Hello world" pro OWIN ve službě IIS.</span><span class="sxs-lookup"><span data-stu-id="c3edd-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="c3edd-130">Stisknutím klávesy F5 pro ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="c3edd-130">Press F5 to debug the application.</span></span> <span data-ttu-id="c3edd-131">Měli byste vidět "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="c3edd-131">You should see "Hello World!"</span></span> <span data-ttu-id="c3edd-132">v okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="c3edd-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="c3edd-133">V dalším kroku vám poskytujeme ověřování Windows služby IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c3edd-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="c3edd-134">Z **zobrazení** nabídce vyberte možnost **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c3edd-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="c3edd-135">Klikněte na název projektu v Průzkumníku řešení Chcete-li zobrazit vlastnosti projektu.</span><span class="sxs-lookup"><span data-stu-id="c3edd-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="c3edd-136">V **vlastnosti** okno, nastavte **anonymní ověřování** k **zakázané** a nastavte **ověřování Windows** k  **Povolené**.</span><span class="sxs-lookup"><span data-stu-id="c3edd-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="c3edd-137">Při spuštění aplikace ze sady Visual Studio, služby IIS Express bude vyžadovat přihlašovací údaje uživatele Windows.</span><span class="sxs-lookup"><span data-stu-id="c3edd-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="c3edd-138">Můžete to zobrazit pomocí [Fiddler](http://fiddler2.com/home) nebo jiného nástroje ladění HTTP.</span><span class="sxs-lookup"><span data-stu-id="c3edd-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="c3edd-139">Tady je příklad odpovědi HTTP:</span><span class="sxs-lookup"><span data-stu-id="c3edd-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="c3edd-140">Hlavičky WWW-Authenticate tato odpověď značí, že server podporuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protokol, který používá protokol Kerberos nebo NTLM.</span><span class="sxs-lookup"><span data-stu-id="c3edd-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="c3edd-141">Později, když nasazujete aplikace na server, využijte [tyto kroky](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) povolení ověřování Windows služby IIS na tomto serveru.</span><span class="sxs-lookup"><span data-stu-id="c3edd-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="c3edd-142">Ověřování Windows v HttpListener</span><span class="sxs-lookup"><span data-stu-id="c3edd-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="c3edd-143">Pokud používáte Microsoft.Owin.Host.HttpListener k samoobslužnému hostování Katana, můžete povolit ověřování Windows přímo na **HttpListener** instance.</span><span class="sxs-lookup"><span data-stu-id="c3edd-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="c3edd-144">Nejprve vytvořte novou konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c3edd-144">First, create a new console application.</span></span> <span data-ttu-id="c3edd-145">Dále přidejte balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="c3edd-145">Next, add NuGet packages.</span></span> <span data-ttu-id="c3edd-146">Z **nástroje** příkaz **Správce balíčků NuGet**, vyberte **konzoly Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="c3edd-146">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c3edd-147">V okně konzoly Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c3edd-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="c3edd-148">Nyní přidejte třídu pojmenovanou `Startup` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c3edd-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="c3edd-149">Tato třída implementuje stejné příkladu "Hello world" z před, ale také nastaví ověřování Windows jako schéma ověřování.</span><span class="sxs-lookup"><span data-stu-id="c3edd-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="c3edd-150">Uvnitř `Main` funkci, spuštění kanálu OWIN:</span><span class="sxs-lookup"><span data-stu-id="c3edd-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="c3edd-151">Odeslat požadavek ve Fiddleru potvrďte, že aplikace používá ověřování Windows:</span><span class="sxs-lookup"><span data-stu-id="c3edd-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="c3edd-152">Související témata</span><span class="sxs-lookup"><span data-stu-id="c3edd-152">Related Topics</span></span>

[<span data-ttu-id="c3edd-153">Přehled projektu Katana</span><span class="sxs-lookup"><span data-stu-id="c3edd-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="c3edd-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="c3edd-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="c3edd-155">Principy ověřování pomocí formulářů OWIN v MVC 5</span><span class="sxs-lookup"><span data-stu-id="c3edd-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
