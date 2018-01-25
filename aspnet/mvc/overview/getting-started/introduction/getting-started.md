---
uid: mvc/overview/getting-started/introduction/getting-started
title: "Začínáme s ASP.NET MVC 5 | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici zde pomocí sady Visual Studio 2015. Nový kurz používá ASP.NET Core MVC 6, která poskytuje mnoho improvem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 1616b238612fa9f519418f583c40a46b9d81d8ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>Začínáme s ASP.NET MVC 5
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 V tomto kurzu naučit, základní informace o vytváření ASP.NET MVC 5 webovou aplikaci pomocí [Visual Studio 2017](https://www.visualstudio.com/). Poslední zdroje pro kurz umístěn na [Githubu](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)
 
 
 V tomto kurzu napsal [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , a [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )
 
 Potřebujete účet Azure k nasazení této aplikace na Azure:
 
 - Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.
 - Můžete [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.


## <a name="getting-started"></a>Začínáme

Začněte tím, že instalace a spuštění [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio je IDE, nebo integrované vývojové prostředí. Stejně jako zápisu dokumentů pomocí aplikace Microsoft Word, použijete k vytvoření aplikací rozhraní IDE. V sadě Visual Studio je seznam podél dolního zobrazuje různé možnosti, které jsou k dispozici pro vás. Je také nabídku, která obsahuje jiný způsob, jak provádět úlohy v prostředí IDE. (Například místo výběru **nový projekt** z **spustit** stránky, můžete použít nabídku a vyberte **soubor** &gt; **nový projekt**.)

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Klikněte na tlačítko **nový projekt**, pak vyberte Visual C# na levé straně, potom **webové** a pak vyberte **webové aplikace ASP.NET (rozhraní .NET Framework)**. Název projektu "MvcMovie" a pak klikněte na **OK**.

![](getting-started/_static/image2.png)

V **nový projekt ASP.NET** dialogové okno, klikněte na tlačítko **MVC** a pak klikněte na **OK**.

![](getting-started/_static/image3.png)

Visual Studio použít výchozí šablonu pro projektu ASP.NET MVC, který jste právě vytvořili, takže budete mít funkční aplikaci, ihned bez jakékoli akce! Jedná se jednoduché "Hello, World!" projektu ale je dobrým místem, kde spustit aplikaci.

![](getting-started/_static/image4.png)

Klikněte na tlačítko F5 spusťte ladění. F5 způsobí, že chcete spustit Visual Studio [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) a spouštění vaší webové aplikace. Visual Studio pak spustí prohlížeč a otevře domovskou stránku aplikace. Všimněte si, že panelu Adresa prohlížeče uvádí `localhost:port#` a není něco jako `example.com`. Je to způsobeno `localhost` vždycky směřuje na vlastní místní počítač, který běží v tomto případě aplikace, které jste právě vytvořili. Po spuštění webového projektu sady Visual Studio náhodný port se používá pro webový server. Na obrázku níže číslo portu je 1234. Při spuštění aplikace se zobrazí jiné číslo portu.

![](getting-started/_static/image5.png)

Okamžitě po nasazení této výchozí šablony vám dává domácí, kontaktů a o stránky. Na obrázku výše nezobrazí **Domů**, **o** a **kontaktujte** odkazy. V závislosti na velikosti okna prohlížeče možná budete muset klikněte na ikonu navigace zobrazíte tyto odkazy.

![](getting-started/_static/image6.png)  

Aplikace také poskytuje podporu pro registraci a přihlaste se. Dalším krokem je a změnit tak, jak tato aplikace funguje trochu Další informace o architektuře ASP.NET MVC. Zavřete aplikaci ASP.NET MVC a změňte nějaký kód.

Seznam aktuální kurzy najdete v tématu [MVC Doporučené články](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Tato aplikace spuštěné v Azure

Chcete zobrazit dokončení web spuštěný jako živou webovou aplikaci? Úplnou verzi aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Potřebujete účet Azure k nasazení tohoto řešení do Azure. Pokud již účet nemáte, máte následující možnosti:

- [Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.
- [Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.

>[!div class="step-by-step"]
[Next](adding-a-controller.md)
