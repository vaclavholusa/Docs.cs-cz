---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Řešení Správce kontaktů | Dokumentace Microsoftu
author: jrjlee
description: Tato série kurzů používá ukázkové řešení&#x2014;řešení Správce kontaktů&#x2014;reprezentující aplikaci podnikové úrovni s realistické leve...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: c8044c37738e9d23ca83648a6b571059338dc1a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835156"
---
<a name="the-contact-manager-solution"></a>Řešení Správce kontaktů
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> To [sérii kurzů](web-deployment-in-the-enterprise.md) používá ukázkové řešení&#x2014;řešení Správce kontaktů&#x2014;reprezentující aplikaci podnikové úrovni s realistické úroveň složitosti. Toto téma představuje řešení Správce kontaktů, popisuje klíčové komponenty řešení a identifikuje problémy při nasazení tohoto typu aplikace na různé cílové platformy v podnikovém prostředí.
> 
> Při práci v těchto kurzech procházení témat, můžete použít jako referenční implementaci, který ukazuje, jak lze splnit určité problémy v podnikových scénářích nasazení řešení Správce kontaktů. Dalším tématu s názvem [nastavení Up the řešení Správce kontaktů](setting-up-the-contact-manager-solution.md), popisuje, jak stáhnout a spustit řešení na vaši vývojářskou pracovní stanici.


## <a name="solution-overview"></a>Přehled řešení

Řešení Správce kontaktů skládá ze čtyř jednotlivé projekty:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Toto je projekt webové aplikace technologie ASP.NET MVC 3, která představuje vstupní bod pro řešení. Nabízí některé funkce základní webové aplikace, jako jsou zároveň uživatelům poskytují možnost vytvářet a zobrazovat kontaktní údaje. Aplikace se spoléhá na službu Windows Communication Foundation (WCF) pro správu kontaktů a databázi služeb aplikaci ASP.NET ke správě ověřování a autorizace.
- **ContactManager.Database**. Toto je databázový projekt sady Visual Studio. Projekt definuje schéma pro databázi, že úložiště kontaktní údaje.
- **ContactManager.Service**. Toto je projekt webové služby WCF. Zpřístupňuje služby WCF, vytvořit koncový bod, který umožňuje volajícím provádět, načítat, aktualizovat a odstraňovat (CRUD) operací **ContactManager** databáze. Služba se může spolehnout **ContactManager** databáze a **ContactManager.Common.dll** sestavení.
- **ContactManager.Common**. Toto je projekt knihovny tříd. Služba WCF spoléhá na typy definované v tomto sestavení.

Toto řešení zahrnuje také řešení složku s názvem publikovat. Tato položka obsahuje různé soubory vlastních projektů a souborů příkazů, které ukazují, jak můžete řídit a manipulaci s procesem sestavení a nasazení. Ty se budeme věnovat jednotlivě podrobněji dále v tomto kurzu.

Koncepční úrovni zapadají součástmi řešení takto:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Když webové aplikace ASP.NET MVC 3 používá poskytovatele členství prostředí ASP.NET, všechny stránky v rámci webové aplikace povolit anonymní přístup. Toto není jasně realistické konfigurace. Toto řešení je ale nastavit tímto způsobem, aby bylo snazší nasadit a testovat řešení bez konfigurace uživatelských účtů a rolí.


## <a name="deployment-challenges"></a>Problémy při nasazení

Řešení Správce kontaktů ukazuje několik problémy při nasazení, které jsou společné pro mnoho podnikových scénářích nasazení:

- Řešení se skládá z více závislých projektů. Potřebujete k nasazení těchto projektů současně.
- Připojovací řetězce a koncových bodů služby je potřeba aktualizovat pro jednotlivá prostředí a v mnoha případech nebude tyto informace k dispozici pro vývojáře.
- Při nasazování **ContactManager** databáze do přípravného a produkčního prostředí, budete muset zachovat stávající data na další nasazení.
- Při nasazení databáze aplikačních služeb ASP.NET, budete muset nasadit některé konfigurační data, ale vynechejte všechny dat účtu uživatele.
- Projekty zahrnují některé soubory a složky, které by se neměly nasazovat. Je třeba vyloučit tyto soubory a složky z procesu nasazení.
- Řešení musí podporovat automatické nasazení sestavení serveru Team Foundation Server (TFS).

## <a name="conclusion"></a>Závěr

Toto téma poskytuje základní přehled o řešení Správce kontaktů a identifikovali několik problémy při vlastní nasazení, které jsou společné pro mnoho podnikových scénářích nasazení. Zbývající témata v tomto kurzu popisují některé techniky, které vám umožní splnit tyto výzvy.

Dalším tématu s názvem [nastavení Up the řešení Správce kontaktů](setting-up-the-contact-manager-solution.md), popisuje, jak stáhnout a spustit řešení na vaši vývojářskou pracovní stanici.

> [!div class="step-by-step"]
> [Předchozí](web-deployment-in-the-enterprise.md)
> [další](setting-up-the-contact-manager-solution.md)
