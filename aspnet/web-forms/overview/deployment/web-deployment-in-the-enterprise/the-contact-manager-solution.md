---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Obraťte se na správce řešení | Microsoft Docs
author: jrjlee
description: Tato série kurzů používá ukázkové řešení&#x2014;řešení obraťte se na správce&#x2014;představují aplikace podnikovém měřítku s realistické leve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30883697"
---
<a name="the-contact-manager-solution"></a>Obraťte se na správce řešení
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> To [série kurzů](web-deployment-in-the-enterprise.md) používá ukázkové řešení&#x2014;řešení obraťte se na správce&#x2014;představují podnikovém měřítku aplikace s úrovní realistické složitější. Toto téma představuje řešení obraťte se na správce, popisuje klíčové komponenty řešení a identifikuje problémy při nasazení tento druh aplikace na různé cílové platformy v podnikovém prostředí.
> 
> Při práci prostřednictvím v tématech v těchto kurzech můžete jako referenční implementace, které ukazuje, jak můžou řešit konkrétní problémy v podnikové scénáře nasazení řešení obraťte se na správce. Dalším tématu [nastavení nahoru řešení obraťte se na správce](setting-up-the-contact-manager-solution.md), popisuje postup stažení a spuštění řešení na pracovní stanici developer.


## <a name="solution-overview"></a>Přehled řešení

Řešení obraťte se na správce se skládá ze čtyř jednotlivých projektů:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Toto je projekt webové aplikace technologie ASP.NET MVC 3, která představuje vstupní bod pro řešení. Nabízí některé funkce základní webové aplikace, jako jsou nabízí uživatelům možnost vytvářet a prohlížet kontaktní údaje. Aplikace spoléhá na službu Windows Communication Foundation (WCF) ke správě kontakty a databázi služeb aplikace ASP.NET ke správě ověřování a autorizace.
- **ContactManager.Database**. Toto je databáze projekt sady Visual Studio. Projekt definuje schéma pro databázi, aby úložiště kontaktní údaje.
- **ContactManager.Service**. Toto je projekt webové služby WCF. Zpřístupňuje služby WCF, koncový bod, který umožňuje volajícím provádět vytvořit, získat, aktualizovat a odstranit operace na **ContactManager** databáze. Služba závisí na **ContactManager** databáze a **ContactManager.Common.dll** sestavení.
- **ContactManager.Common**. Toto je projektu knihovny tříd. Služby WCF spoléhá na typy definované v tomto sestavení.

Řešení zahrnuje také řešení složku s názvem publikovat. Tato položka obsahuje různé soubory vlastních projektů a soubory příkazů, které ukazují, jak můžete řídit a manipulaci s procesem sestavení a nasazení. Tyto jsou podrobněji popsané později v tomto kurzu.

Koncepční úrovni zapadají součástí řešení takto:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Pokud webová aplikace ASP.NET MVC 3 používá poskytovatele členství prostředí ASP.NET, všechny stránky v rámci webové aplikace povolí anonymní přístup. Toto není jasně realistické konfigurace. Řešení je ale nastavit tímto způsobem, aby bylo snazší pro vás k nasazení a testování řešení bez nutnosti konfigurace uživatelských účtů a rolí.


## <a name="deployment-challenges"></a>Nasazení výzvy

Obraťte se na správce řešení znázorňuje několik problémů nasazení, které jsou společné pro velké podnikové scénáře nasazení:

- Řešení se skládá z více závislých projektů. Je třeba nasadit tyto projektů současně.
- Připojovací řetězce a koncové body služby je třeba aktualizovat pro každé prostředí a v mnoha případech nebude tyto informace k dispozici pro vývojáře.
- Při nasazení **ContactManager** databáze do pracovní a provozní prostředí, je nutné zachovat stávající data na následné nasazení.
- Když nasadíte databáze aplikačních služeb ASP.NET, je třeba nasadit některé konfigurační data, ale vynechejte jakákoliv uživatelská data účtu.
- Projekty zahrnují některé soubory a složky, které by neměly být nasazeny. Je potřeba vyloučit tyto soubory a složky z procesu nasazení.
- Řešení musí podporovat automatického nasazení ze serveru Team Foundation Server (TFS) sestavení.

## <a name="conclusion"></a>Závěr

Toto téma poskytuje přehled řešení obraťte se na správce a identifikovat některé z problémů vyplývajících nasazení, které jsou společné pro velké podnikové scénáře nasazení. Zbývající témata v tomto kurzu popisují některé techniky, které můžete použít ke splnění tyto problémy.

Dalším tématu [nastavení nahoru řešení obraťte se na správce](setting-up-the-contact-manager-solution.md), popisuje postup stažení a spuštění řešení na pracovní stanici developer.

> [!div class="step-by-step"]
> [Předchozí](web-deployment-in-the-enterprise.md)
> [další](setting-up-the-contact-manager-solution.md)
