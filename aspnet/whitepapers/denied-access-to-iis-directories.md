---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET odepřen přístup do adresáře služby IIS | Microsoft Docs
author: rick-anderson
description: Tento dokument White Paper popisuje, co musíte udělat Pokud požadavek na vaši aplikaci ASP.NET vrátí chybu, "byl odepřen přístup do adresáře DirectoryName. Nepodařilo se s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="e7a1c-104">ASP.NET odepřen přístup do adresáře služby IIS</span><span class="sxs-lookup"><span data-stu-id="e7a1c-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="e7a1c-105">Tento dokument White Paper popisuje, co musíte udělat Pokud požadavek na vaši aplikaci ASP.NET vrátí chybu, "odepření přístupu k *DirectoryName* adresáře.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="e7a1c-106">Nepodařilo se spustit sledování změn v adresáři."</span><span class="sxs-lookup"><span data-stu-id="e7a1c-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="e7a1c-107">Platí pro ASP.NET 1.0 a 1.1 technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="e7a1c-108">ASP.NET V1 RTM se teď bude spouštět pomocí méně privilegovaného účtu systému windows - registrován jako "ASPNET" účet v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="e7a1c-109">U některých uzamčené systémy tento účet může ve výchozím nastavení mají přístup pro čtení zabezpečení adresáře s obsahem webu, kořenovém adresáři aplikace nebo kořenový adresář webu.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="e7a1c-110">V takovém případě zobrazí chybová zpráva požadavku stránky od dané webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="e7a1c-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="e7a1c-111">Tento problém lze vyřešit bude muset změnit oprávnění zabezpečení u příslušné adresáře.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="e7a1c-112">Konkrétně technologie ASP.NET vyžaduje čtení, spouštění a přístup k účtu ASPNET kořenový adresář webového serveru (například: c:\inetpub\wwwroot nebo libovolného adresáře alternativní lokality možná jste nakonfigurovali ve službě IIS), adresář s obsahem a kořenového adresáře aplikace Chcete-li sledování změn konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="e7a1c-113">Kořenový adresář aplikace odpovídá cesta ke složce spojené s virtuální adresář aplikace v nástroji pro správu služby IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="e7a1c-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="e7a1c-114">Zvažte například následující hierarchie aplikace ve složce wwwroot.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="e7a1c-115">V tomto příkladu musí účtu ASPNET oprávnění ke čtení definovaný nad obsahu Moje aplikace a adresář wwwroot.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="e7a1c-116">Jeden zděděné ACL v kořenové složce Volitelně lze také pro oba adresáře Pokud jste se vnořený.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="e7a1c-117">Chcete-li přidat oprávnění k adresáři, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e7a1c-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="e7a1c-118">Pomocí Průzkumníka Windows přejděte do adresáře</span><span class="sxs-lookup"><span data-stu-id="e7a1c-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="e7a1c-119">Klikněte pravým tlačítkem na složku, adresář a vyberte možnost "Vlastnosti"</span><span class="sxs-lookup"><span data-stu-id="e7a1c-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="e7a1c-120">Přejděte na kartu "Zabezpečení" v dialogovém okně Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e7a1c-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="e7a1c-121">Kliknutím na tlačítko "Přidat" a zadejte název počítače, za nímž následuje název účtu ASPNET.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="e7a1c-122">Například by na počítači s názvem "webdev", zadejte webdev\ASPNET a stiskněte tlačítko "OK".</span><span class="sxs-lookup"><span data-stu-id="e7a1c-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="e7a1c-123">Zkontrolujte, zda má účet ASPNET "pro čtení &amp; Execute", "Zobrazovat obsah složky" a "Číst" políček zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="e7a1c-124">Klikněte na OK zavřete toto dialogové okno a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="e7a1c-125">V případě potřeby tyto změny je možné automatizovat pomocí skriptů nebo nástroj "cacls.exe", který se dodává s Windows.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="e7a1c-126">Další informace o účtu ASPNET najdete [nejčastější dotazy k dokumentu](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="e7a1c-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="e7a1c-127">Pokud dané webové aplikace spoléhá na nutnosti zápisu nebo změnit oprávnění k určité složce nebo souboru, mohou být udělena podle stejného postupu a kontrola políček "Zápisu" nebo "Upravit".</span><span class="sxs-lookup"><span data-stu-id="e7a1c-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="e7a1c-128">Na počítačích, které umožňují Everyone nebo uživatelům přístup pro čtení skupiny k tyto adresáře (což je výchozí konfigurace), bude došlo k žádné problémy a výše uvedené kroky se nevyžaduje.</span><span class="sxs-lookup"><span data-stu-id="e7a1c-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
