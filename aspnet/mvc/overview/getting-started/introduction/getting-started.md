---
uid: mvc/overview/getting-started/introduction/getting-started
title: Začínáme s ASP.NET MVC 5 | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici zde pomocí sady Visual Studio 2015. Nové kurzu se používá technologie ASP.NET Core MVC 6, které poskytuje mnoho improvem...'
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823213"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Začínáme s ASP.NET MVC 5
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 V tomto kurzu se seznámíte se základy vytváření webové aplikace ASP.NET MVC 5 pomocí [Visual Studio 2017](https://www.visualstudio.com/). Konečné zdroj kurzu [Githubu](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 V tomto kurzu zapsal [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , a [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Potřebujete účet Azure tak, aby tuto aplikaci nasadit do Azure:

 - Je možné [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – budete dostávat kredity můžete použít k vyzkoušení placených služeb Azure a dokonce i po jejich použití až můžete účet ponechat a dál používat bezplatné služby Azure.
 - Je možné [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) – vaše předplatné MSDN vám dává kredity každý měsíc, můžete použít k placení za služby Azure.


## <a name="getting-started"></a>Začínáme

Začněte tím, že instalaci a používání [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio je integrované vývojové prostředí, nebo integrovaného vývojového prostředí. Stejným způsobem, jako používáte k tvorbě dokumenty Microsoft Wordu, budete používat integrované vývojové prostředí pro vytváření aplikací. V sadě Visual Studio je seznam v dolní části zobrazující různé možnosti, které jsou k dispozici pro vás. Je také nabídka, která nabízí další způsob, jak provádět úkoly v integrovaném vývojovém prostředí. (Například místo výběru **nový projekt** z **Start** stránky, můžete použít nabídku a vyberte **souboru** &gt; **nový projekt**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Klikněte na tlačítko **nový projekt**, vyberte jazyk Visual C# na levé straně, pak **webové** a pak vyberte **webová aplikace ASP.NET (.NET Framework)**. Pojmenujte svůj projekt "MvcMovie" a potom klikněte na tlačítko **OK**.

![](getting-started/_static/image2.png)

V **nový projekt ASP.NET** dialogového okna, klikněte na tlačítko **MVC** a potom klikněte na tlačítko **OK**.

![](getting-started/_static/image3.png)

Visual Studio použít výchozí šablonu projektu ASP.NET MVC, které jste právě vytvořili, takže Teď máte funkční aplikaci bez teď zrovna nic nedělá! Jde jednoduchý "Hello World!" projekt a je vhodné místo pro spuštění vaší aplikace.

![](getting-started/_static/image4.png)

Klikněte na tlačítko F5 spusťte ladění. F5 způsobí, že Visual Studio spustíte [služby IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) a spouštění vaší webové aplikace. Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace. Všimněte si, že na panelu Adresa prohlížeče `localhost:port#` a nemít něco podobného `example.com`. Důvodem je, že `localhost` vždy odkazuje na vaše vlastní místní počítače v tomto případě je spuštěna aplikace právě jste vytvořili. Při spuštění webový projekt sady Visual Studio náhodný port se používá pro webový server. Na následujícím obrázku je číslo portu 1234. Při spuštění aplikace se zobrazí jiné číslo portu.

![](getting-started/_static/image5.png)

Předem připravené tuto výchozí šablonu vám Home, kontaktů a o stránky. Na obrázku výše nezobrazuje **Domů**, **o** a **kontakt** odkazy. V závislosti na velikosti okna prohlížeče můžete potřebovat kliknutím na ikonu navigační naleznete pod následujícími odkazy.

![](getting-started/_static/image6.png)  

Aplikace také poskytuje podporu pro registraci a přihlaste se. Dalším krokem je změnit, jak tato aplikace funguje a přečtěte si ještě něco o architektuře ASP.NET MVC. Ukončete aplikaci ASP.NET MVC a Změníme kód.

Seznam aktuální kurzy najdete v tématu [MVC Doporučené články](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Zobrazit tuto aplikaci běžící v Azure

Chcete zobrazit dokončené web spuštěný jako živou webovou aplikaci? Kompletní verze aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Potřebujete účet Azure k nasazení tohoto řešení do Azure. Pokud ještě nemáte účet, máte následující možnosti:

- [Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – budete dostávat kredity můžete použít k vyzkoušení placených služeb Azure a dokonce i po jejich použití až můžete účet ponechat a dál používat bezplatné služby Azure.
- [Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) – vaše předplatné MSDN vám dává kredity každý měsíc, můžete použít k placení za služby Azure.

> [!div class="step-by-step"]
> [Next](adding-a-controller.md)
