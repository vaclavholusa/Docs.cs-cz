---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Použití rozhraní Web API 2 s platformou Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: V tomto kurzu se naučit, že základní informace o vytvoření webové aplikace pomocí rozhraní ASP.NET Web API back-end. Tento kurz používá Entity Framework 6 pro uspořádání dat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871971"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Použití rozhraní Web API 2 s platformou Entity Framework 6
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

> V tomto kurzu se naučit, že základní informace o vytvoření webové aplikace pomocí rozhraní ASP.NET Web API back-end. Tento kurz používá Entity Framework 6 Datová vrstva a Knockout.js aplikace JavaScript na straně klienta. Tento kurz také ukazuje, jak nasadit aplikace do Azure App Service Web Apps.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


Tento kurz používá k vytvoření webové aplikace, která zpracovává databázi back-end ASP.NET Web API 2 s Entity Framework 6. Zde je snímek obrazovky aplikace, které vytvoříte.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Aplikace používá návrh jednostránkové aplikace (SPA). "Jednostránkové aplikace" je obecný termín pro webovou aplikaci, která načte jediné stránce HTML a pak aktualizuje dynamicky, místo načítání stránky, nové stránky. Po úvodní stránky zatížení aplikace komunikuje se serverem prostřednictvím požadavky AJAX. AJAX požadavků načíst data JSON, které aplikace používá k aktualizaci uživatelského rozhraní.

AJAX není nový, ale existují ještě dnes JavaScript rozhraní, které usnadňují tvorbu a udržování rozsáhlé sofistikované SPA aplikace. Tento kurz používá [Knockout.js](http://knockoutjs.com/), ale můžete použít libovolnou architekturu JavaScript klienta.

Zde jsou hlavní stavební bloky pro tuto aplikaci:

- ASP.NET MVC vytvoří stránku HTML.
- Rozhraní ASP.NET Web API zpracovává požadavky AJAX a vrací JSON data.
- Knockout.js data vazby elementů HTML pro JSON data.
- Rozhraní Entity Framework komunikuje do databáze.

## <a name="see-this-app-running-on-azure"></a>Tato aplikace spuštěné v Azure

Chcete zobrazit dokončení web spuštěný jako živou webovou aplikaci? Úplnou verzi aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Potřebujete účet Azure k nasazení tohoto řešení do Azure. Pokud již účet nemáte, máte následující možnosti:

- [Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.
- [Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.

## <a name="create-the-project"></a>Vytvoření projektu

Otevřete Visual Studio. Z **soubor** nabídce vyberte možnost **nový**, pak vyberte **projektu**. (Nebo klikněte na tlačítko **nový projekt** na stránce Start.)

V **nový projekt** dialogové okno, klikněte na tlačítko **webové** v levém podokně a **webové aplikace ASP.NET** v prostředním podokně. Název projektu BookService a klikněte na tlačítko **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

V **nový projekt ASP.NET** dialogovém okně, vyberte **webového rozhraní API** šablony.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Pokud chcete k hostování projektu v Azure App Service, ponechte **hostitel v cloudu** zaškrtnutým políčkem.

Klikněte na tlačítko **OK** a vytvořte tak projekt.

## <a name="configure-azure-settings-optional"></a>Konfigurace nastavení Azure (volitelné)

Pokud jste nechali **hostitel v cloudu** zaškrtnutá možnost Visual Studio zobrazí výzvu k přihlášení do služby Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Po přihlášení do Azure, Visual Studio zobrazí výzvu k konfigurace webové aplikace. Zadejte název lokality, vyberte předplatné Azure a vyberte zeměpisnou oblast. V části **databázový server**, vyberte **vytvořit nový server**. Zadejte uživatelské jméno správce a heslo.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Next](part-2.md)
