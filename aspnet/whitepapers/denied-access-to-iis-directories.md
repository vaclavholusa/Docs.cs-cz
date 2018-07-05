---
uid: whitepapers/denied-access-to-iis-directories
title: Aplikace ASP.NET zamítla přístup k adresářům služby IIS | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument White Paper popisuje, co musíte udělat, pokud požadavek na vaše aplikace ASP.NET vrátí chyba "přístup byl odepřen do DirectoryName. Nepovedlo se s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 22868738e02472f5f433c729967ac324131a0fdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383059"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="5fc3b-104">Aplikace ASP.NET zamítla přístup k adresářům služby IIS</span><span class="sxs-lookup"><span data-stu-id="5fc3b-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="5fc3b-105">Tento dokument White Paper popisuje, co musíte udělat, pokud požadavek na vaše aplikace ASP.NET vrátí chybu, "odepření přístupu k *NazevAdresare* adresáře.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="5fc3b-106">Nepovedlo se spustit sledování změn v adresáři."</span><span class="sxs-lookup"><span data-stu-id="5fc3b-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="5fc3b-107">Platí pro technologii ASP.NET 1.0 a ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="5fc3b-108">Technologie ASP.NET verze 1 RTM se teď bude spouštět pomocí méně privilegovaného účtu systému windows - zaregistrovaný jako "ASPNET" účet na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="5fc3b-109">U některých systémů uzamčen tento účet může ve výchozím nastavení přečetl(a) security umožněn přístup k obsahu adresáře webu, kořenový adresář aplikace nebo kořenový adresář webu.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="5fc3b-110">V tomto případě zobrazí následující chyba požadavku stránky z dané webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="5fc3b-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="5fc3b-111">Chcete-li tento problém vyřešit, je potřeba měnit oprávnění zabezpečení pro příslušné adresáře.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="5fc3b-112">Konkrétně technologie ASP.NET vyžaduje čtení, spouštění a zápis k účtu ASPNET kořenový adresář webového serveru (například: c:\inetpub\wwwroot nebo jakéhokoli adresáře alternativní lokality možná jste nakonfigurovali ve službě IIS), adresář s obsahem a kořenový adresář aplikace aby bylo možné sledovat změny v konfiguraci souboru.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="5fc3b-113">Kořenový adresář aplikace odpovídá cesta ke složce, které jsou spojené s virtuálním adresářem aplikace v nástroji pro správu služby IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="5fc3b-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="5fc3b-114">Představte si třeba následující hierarchie aplikace pod složku wwwroot.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="5fc3b-115">V tomto příkladu musí účtu ASPNET oprávnění ke čtení, které jsou definované výše pro obsah Moje aplikace a adresář wwwroot.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="5fc3b-116">Jeden zděděné seznamu ACL pro kořenovou složku Volitelně lze také pro oba adresáře v případě, že jsou vnořené.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="5fc3b-117">Chcete-li přidat oprávnění k adresáři, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5fc3b-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="5fc3b-118">Pomocí Průzkumníka Windows přejděte do adresáře</span><span class="sxs-lookup"><span data-stu-id="5fc3b-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="5fc3b-119">Klikněte pravým tlačítkem na složku adresář a zvolte možnost "Properties"</span><span class="sxs-lookup"><span data-stu-id="5fc3b-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="5fc3b-120">Přejděte na kartu "Zabezpečení" v dialogovém okně Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="5fc3b-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="5fc3b-121">Klikněte na tlačítko "Přidat" a zadejte název počítače, za nímž následuje název účtu ASPNET.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="5fc3b-122">Například by na počítači s názvem "webdev", zadejte webdev\ASPNET a klikněte na "OK".</span><span class="sxs-lookup"><span data-stu-id="5fc3b-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="5fc3b-123">Zkontrolujte, zda má účet ASPNET "čtení &amp; Execute", "Zobrazovat obsah složky" a "Číst" zaškrtnutých políček.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="5fc3b-124">Klikněte na OK a zavřete toto dialogové okno a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="5fc3b-125">V případě potřeby je možné tyto změny automatizovat pomocí skriptů nebo "cacls.exe" nástroj, který se dodává s Windows.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="5fc3b-126">Další informace o účtu ASPNET, najdete v tématu [nejčastější dotazy k dokumentu](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="5fc3b-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="5fc3b-127">Pokud dané webové aplikace spoléhá na zápis nebo upravit oprávnění do konkrétní složky nebo souboru, to může udělit pomocí stejného postupu a kontrola zaškrtávací políčka "Write" nebo "Upravit".</span><span class="sxs-lookup"><span data-stu-id="5fc3b-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="5fc3b-128">Na počítačích, které umožňují všem uživatelům nebo přístup ke čtení skupiny uživatelů pro tyto adresáře (to je výchozí konfigurace), se žádné problémy setkáte a proveďte následující kroky se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="5fc3b-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
