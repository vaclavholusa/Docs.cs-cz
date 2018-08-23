---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: '1. část: Soubor -> Nový projekt | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 1. část obsahuje přehled a soubor/nový projekt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: d2e4b36c9029e86eea9b09974839e96e9aa39ced
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752862"
---
<a name="part-1-file--new-project"></a>1. část: Soubor -> Nový projekt
====================
podle [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 1. část obsahuje přehled a soubor/nový projekt.


## <a id="_Toc260221666"></a>  Přehled

Tento kurz je Úvod do webových formulářů ASP.NET. Jsme budete spuštění pomalu, takže Začátečník úrovně webové vývojové prostředí je v pořádku.

Aplikace, kterou jsme vám sestavení je jednoduché online úložiště.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Návštěvníci můžete přejít pomocí produktů podle kategorie:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Mohou zobrazit jednoho produktu a přidat do košíku jejich:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Jejich košíku odebrat všechny položky, které už nechcete, můžete zkontrolovat:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Přistoupíte k ověření je vyzve k

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Po změně pořadí, se zobrazí obrazovka s potvrzením jednoduchý:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Budete Začneme tím, že vytvoříte nový projekt webových formulářů ASP.NET v sadě Visual Studio 2010 a postupně přidáme funkce, které chcete vytvořit kompletní funkční aplikaci. Cestou se budeme zabývat přístup k databázi, zobrazením seznamu a mřížky, stránky aktualizace dat, ověřování dat pomocí stránek předlohy pro rozložení konzistentní stránek, AJAX, ověřování, členství uživatele a další.

Podrobný postup sledovat, nebo si můžete stáhnout hotovou aplikaci [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Můžete použít Visual Studio 2010 nebo na bezplatné Visual Web Developer 2010 z [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). K sestavení aplikace, můžete použít SQL Server nebo bezplatný systém SQL Server Express pro hostování databáze.

## <a id="_Toc260221667"></a>  Soubor / nový projekt

Začneme tak, že vyberete nový projekt v sadě Visual Studio v nabídce Soubor. Tím se zobrazí dialogové okno Nový projekt.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Vybereme Visual C# / webové šablony skupiny na levé straně a potom v prostředním sloupci zvolte šablonu "Webovou aplikaci ASP.NET". Pojmenujte svůj projekt TailspinSpyworks a kliknutím na tlačítko OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Tím se vytvoří našem projektu. Pojďme se podívat na složky, které jsou součástí naší aplikace v Průzkumníku řešení na pravé straně.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Prázdné řešení není zcela prázdný – přidá strukturu základní složky:

![](tailspin-spyworks-part-1/_static/image1.png)

Všimněte si konvence implementované výchozí šablona projektu ASP.NET 4.

- Složce "Účet" implementuje základní uživatelské rozhraní pro ASP. Subsystém členství vaší sítě.
- Složce "Skripty" slouží jako úložiště pro soubory jazyka JavaScript na straně klienta a souborů .js jQuery core jsou dostupné ve výchozím nastavení.
- Složce "Styly" se používá k uspořádání naše vizuální prvky webového serveru (šablony stylů CSS)

Při stisknutí klávesy F5 ke spuštění aplikace a vykreslování stránku default.aspx jsme v tomto tématu.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Vylepšení našich první aplikaci budete moct soubor Style.css z výchozí šablony webových formulářů nahradit třídy šablony stylů CSS a přidružené obrazových souborů, které budou vykreslovat visual asthetics, která chceme v naší aplikace Tailspin Spyworks.

Až to uděláte naši stránku default.aspx vykreslí následujícím způsobem.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Všimněte si, že odkazy obrázku nahoře napravo od stránce a položky nabídky, které byly přidány k hlavní stránce. Pouze "Sign In" a "Účet" odkazy odkazují na stránky, které existují (generované výchozí šablony) a zbytek stránky, které jsme se implementují jako jsme integrovali naši aplikaci.

Budeme také přemístit stránky předlohy se stránkou k adresáři styly. Je to jenom předvoleb však může být věcí o něco jednodušší Pokud se rozhodneme, aby naši aplikaci "skinable" v budoucnu.

Po této budeme muset změnit na stránce předlohy generované odkazy do všech souborů .aspx ve výchozím nastavení stránky webových formulářů ASP.NET.

> [!div class="step-by-step"]
> [Next](tailspin-spyworks-part-2.md)
