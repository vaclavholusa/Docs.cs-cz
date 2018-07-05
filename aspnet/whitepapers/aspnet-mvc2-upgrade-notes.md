---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Upgrade aplikace ASP.NET MVC 1,0 do architektury ASP.NET MVC 2 | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje, jak tom, jak upgradovat ručně a pomocí Průvodce aplikace ASP.NET MVC 1,0 ASP.NET MVC 2. Tento dokument je také k dispozici pro d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 354dbab3057068eb13b16c9aa31f66c72371ce7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381913"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="2c71c-104">Upgrade aplikace ASP.NET MVC 1,0 do architektury ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="2c71c-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="2c71c-105">Tento dokument popisuje, jak tom, jak upgradovat ručně a pomocí Průvodce aplikace ASP.NET MVC 1,0 ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="2c71c-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="2c71c-106">Tento dokument je také k dispozici pro [stáhnout](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="2c71c-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="2c71c-107">Úvod</span><span class="sxs-lookup"><span data-stu-id="2c71c-107">Introduction</span></span>

<span data-ttu-id="2c71c-108">ASP.NET MVC 2 lze nainstalovat souběžně s ASP.NET MVC 1,0 na stejném serveru.</span><span class="sxs-lookup"><span data-stu-id="2c71c-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="2c71c-109">Díky tomu aplikace vývojáři flexibilitu při výběru, kdy se má upgradovat aplikaci ASP.NET MVC 1,0 do ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="2c71c-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="2c71c-110">Visual Studio 2010 obsahuje průvodce tento upgrade existující projekty ASP.NET MVC 1,0 vytvořených pomocí sady Visual Studio 2008 na ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="2c71c-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="2c71c-111">Průvodce upgradem je zahájeno otevřením projektu aplikace ASP.NET MVC 1,0 v sadě Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2c71c-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="2c71c-112">Průvodce pro technologii ASP.NET MVC 1,0 na Visual Studio 2008 SP1 upgradu</span><span class="sxs-lookup"><span data-stu-id="2c71c-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="2c71c-113">Pokud chcete upgradovat aplikaci ASP.NET MVC 1,0 do ASP.NET MVC 2 v aplikaci Visual Studio 2008 SP1, pomocí (nepodporované) MvcAppConverter aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c71c-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="2c71c-114">Tuto aplikaci si můžete stáhnout z následující adresy URL:</span><span class="sxs-lookup"><span data-stu-id="2c71c-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="2c71c-115">Ruční upgrade projektu aplikace ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="2c71c-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="2c71c-116">Ruční upgrade stávající aplikace ASP.NET MVC 1,0 do verze 2, postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="2c71c-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="2c71c-117">Vytvořte záložní kopii existujícího projektu.</span><span class="sxs-lookup"><span data-stu-id="2c71c-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="2c71c-118">V textovém editoru otevřete soubor projektu (soubor s příponou souboru .csproj nebo .vbproj) a najděte ProjectTypeGuid element.</span><span class="sxs-lookup"><span data-stu-id="2c71c-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="2c71c-119">Jako hodnotu tohoto prvku nahradí GUID {603c0e0b-db56-11dc-be95-000d561079b0} s {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="2c71c-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="2c71c-120">Jakmile budete hotovi, hodnota tohoto prvku by měla být následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2c71c-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="2c71c-121">V kořenové složce webové aplikace upravte soubor Web.config.</span><span class="sxs-lookup"><span data-stu-id="2c71c-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="2c71c-122">Hledat System.Web.Mvc, verze = 1.0.0.0 a nahradí všechny instance System.Web.Mvc, verze = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="2c71c-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="2c71c-123">Opakujte předchozí krok pro soubor Web.config umístěný ve složce zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2c71c-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="2c71c-124">Otevřete projekt pomocí sady Visual Studio a v **Průzkumníka řešení**, rozbalte **odkazy** uzlu.</span><span class="sxs-lookup"><span data-stu-id="2c71c-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="2c71c-125">Odstraňte odkazy na System.Web.Mvc (která odkazuje na sestavení verze 1.0).</span><span class="sxs-lookup"><span data-stu-id="2c71c-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="2c71c-126">Přidejte odkaz na System.Web.Mvc (v2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="2c71c-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="2c71c-127">Přidejte následující prvek bindingRedirect v souboru Web.config v kořenovém adresáři aplikace v sekci configuraton:</span><span class="sxs-lookup"><span data-stu-id="2c71c-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="2c71c-128">Vytvořte novou prázdnou aplikaci ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="2c71c-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="2c71c-129">Zkopírujte soubory ze složky skripty nové aplikace do složky Scripts existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="2c71c-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="2c71c-130">Aktualizovat existující € applicationâ™ soubor CSS s pomocí definice stylů CSS v souboru Site.css.</span><span class="sxs-lookup"><span data-stu-id="2c71c-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="2c71c-131">Zkompilujte aplikaci a spustíme ji.</span><span class="sxs-lookup"><span data-stu-id="2c71c-131">Compile the application and run it.</span></span> <span data-ttu-id="2c71c-132">Pokud dojde k nějaké chybě, podívejte se na část Rozbíjející změny [co je nového v ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) stránky.</span><span class="sxs-lookup"><span data-stu-id="2c71c-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
