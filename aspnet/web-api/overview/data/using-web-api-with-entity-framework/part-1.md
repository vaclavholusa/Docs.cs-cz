---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Použití rozhraní Web API 2 s Entity Framework 6 | Dokumentace Microsoftu
author: MikeWasson
description: V tomto kurzu se dozvíte, že se se základy vytváření webovou aplikaci s webovým rozhraním API technologie ASP.NET back-endu. Tento kurz používá Entity Framework 6 pro uspořádání dat...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 4abe0e06dfd927765efd8e566584e111cf4117d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753026"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Použití rozhraní Web API 2 s Entity Framework 6
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](https://github.com/MikeWasson/BookService)

> V tomto kurzu se dozvíte, že se se základy vytváření webovou aplikaci s webovým rozhraním API technologie ASP.NET back-endu. Tento kurz používá Entity Framework 6 pro datová vrstva a knihovnou Knockout.js pro aplikace JavaScript na straně klienta. Tento kurz také ukazuje, jak nasadit aplikaci do Azure App Service Web Apps.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Rozhraní Knockout.js](http://knockoutjs.com/) 3.1


Tento kurz používá prostředí ASP.NET Web API 2 s Entity Framework 6 a vytvořte webovou aplikaci, která zpracovává back-end databáze. Zde je snímek obrazovky aplikace, kterou vytvoříte.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Aplikace používá návrhu jednostránkové aplikace (SPA). "Jednostránkovou aplikaci" je obecný termín pro webovou aplikaci, která načte jednu stránku HTML a následně aktualizuje dynamicky, místo načtení nové stránky na stránku. Po načtení počáteční stránky aplikace komunikuje se serverem přes odesílání požadavků AJAX. AJAX žádosti vrácená data JSON, který aplikace používá k aktualizaci uživatelského rozhraní.

AJAX není novinka, ale ještě dnes existují architektury JavaScriptu, které usnadňují tvorbu a udržování rozsáhlé sofistikované aplikace SPA. Tento kurz používá [knihovnou Knockout.js](http://knockoutjs.com/), ale můžete použít libovolnou architekturu JavaScript klienta.

Tady jsou hlavní stavební bloky pro tuto aplikaci:

- ASP.NET MVC vytvoří na stránce HTML.
- Rozhraní ASP.NET Web API zpracovává požadavky AJAX a vrací JSON data.
- Rozhraní Knockout.js data vazbu elementů HTML JSON data.
- Entity Framework a k databázi.

## <a name="see-this-app-running-on-azure"></a>Zobrazit tuto aplikaci běžící v Azure

Chcete zobrazit dokončené web spuštěný jako živou webovou aplikaci? Kompletní verze aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Potřebujete účet Azure k nasazení tohoto řešení do Azure. Pokud ještě nemáte účet, máte následující možnosti:

- [Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – budete dostávat kredity můžete použít k vyzkoušení placených služeb Azure a dokonce i po jejich použití až můžete účet ponechat a dál používat bezplatné služby Azure.
- [Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) – vaše předplatné MSDN vám dává kredity každý měsíc, můžete použít k placení za služby Azure.

## <a name="create-the-project"></a>Vytvoření projektu

Otevřít Visual Studio. Z **souboru** nabídce vyberte možnost **nový**a pak vyberte **projektu**. (Nebo klikněte na tlačítko **nový projekt** na úvodní stránce.)

V **nový projekt** dialogového okna, klikněte na tlačítko **webové** v levém podokně a **webová aplikace ASP.NET** v prostředním podokně. Pojmenujte projekt BookService a klikněte na tlačítko **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

V **nový projekt ASP.NET** dialogového okna, vyberte **webového rozhraní API** šablony.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Pokud chcete k hostování projektu ve službě Azure App Service, nechat **hostovat v cloudu** zaškrtnuté políčko.

Klikněte na tlačítko **OK** pro vytvoření projektu.

## <a name="configure-azure-settings-optional"></a>Konfigurace nastavení služby Azure (volitelné)

Pokud jste nechali **hostitel v cloudu** zaškrtnutá možnost, sada Visual Studio vás vyzve k přihlášení k Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Po přihlášení do Azure, Visual Studio vás vyzve k konfigurace webové aplikace. Zadejte název lokality, vyberte své předplatné Azure a vyberte zeměpisnou oblast. V části **databázový server**vyberte **vytvořit nový server**. Zadejte uživatelské jméno správce a heslo.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Next](part-2.md)
