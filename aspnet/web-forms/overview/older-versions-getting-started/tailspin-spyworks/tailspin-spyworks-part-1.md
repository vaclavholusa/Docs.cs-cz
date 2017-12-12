---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: "Část 1: Soubor -> Nový projekt | Microsoft Docs"
author: JoeStagner
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 1 obsahuje přehled a soubor nebo nový projekt."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a>Část 1: Soubor -> Nový projekt
====================
podle [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET. Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 1 obsahuje přehled a soubor nebo nový projekt.


## <a id="_Toc260221666"></a>Přehled

Tento kurz je určen Úvod do webových formulářů ASP.NET. Jsme budete od pomalu, takže vývoj webů úrovni začátečník prostředí je to v pořádku.

Aplikace, kterou jsme budete sestavení je jednoduchý online úložiště.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Návštěvníky můžete vyhledat produkty podle kategorie:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Mohou zobrazovat jednoho produktu a přidat jej do jejich košíku:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Jejich můžete zkontrolovat jejich košíku odebrání všech položek, které už mají:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Přistoupíte k ověření se výzva k

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Po změně pořadí, uvidí jednoduché potvrzovací obrazovce:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Budete začneme vytvořením nového projektu ASP.NET WebForms v sadě Visual Studio 2010 a přidáme přírůstkově funkce k vytvoření kompletní funkční aplikaci. Na této cestě jsme zaměříme přístup k databázi, zobrazení seznamu a mřížky, stránky aktualizace dat, ověření dat pomocí stránky předlohy pro rozložení konzistentní stránky, AJAX, ověření, členství uživatele a další.

Můžete absolvovat krok za krokem, nebo si můžete stáhnout hotová aplikace z [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Můžete použít Visual Studio 2010 nebo bezplatné Visual Web Developer 2010 z [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Jestli Pokud chcete vytvářet aplikace, můžete použít SQL Server nebo volné SQL Server Express na hostitele databáze.

## <a id="_Toc260221667"></a>Soubor nový projekt

Začneme výběrem nový projekt v sadě Visual Studio v nabídce Soubor. Otevře dialogové okno Nový projekt.

![](tailspin-spyworks-part-1/_static/image8.jpg)

Jsme vyberte Visual C# / skupiny na levé straně webové šablony a potom vyberte šablonu "Webové aplikace ASP.NET" v prostředním sloupci. Název projektu TailspinSpyworks a klepněte na tlačítko OK.

![](tailspin-spyworks-part-1/_static/image9.jpg)

Tím se vytvoří naše projektu. Podívejme se na složky, které jsou zahrnuty v naší aplikaci v Průzkumníku řešení na pravé straně.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Prázdný řešení není úplně prázdný – přidá do základní složky struktury:

![](tailspin-spyworks-part-1/_static/image1.png)

Poznámka: konvence implementované výchozí šablona projektu ASP.NET 4.

- Složce "Účet" implementuje základní uživatelské rozhraní pro ASP. Subsystém pro NET členství.
- Složce "Skripty" slouží jako úložiště pro klientské soubory JavaScript a soubory .js jQuery jádra, která jsou k dispozici ve výchozím nastavení.
- Složce "Styly" slouží k uspořádání naše webu vizuály (šablony stylů CSS)

Když jsme stisknutím klávesy F5 spusťte aplikaci a vykreslení stránky default.aspx jsme v tomto tématu.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Naše první vylepšení aplikace bude nahradit soubor Style.css z výchozí šablony webových formulářů s tříd CSS a přidruženou bitovou kopii souborů, které budou vykreslovat visual asthetics, který má být pro naši aplikaci Tailspin Spyworks.

Až to uděláte naší stránce s default.aspx vykreslí následujícím způsobem.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Všimněte si odkazy bitové kopie v horní pravé části stránky a položky nabídky, které byly přidány k hlavní stránce. Pouze odkazy "Přihlášení" a "Účet" bodu stránek, které existují (generované výchozí šablony) a zbytek stránky, které bude implementaci jako jsme sestavení aplikace.

Vytvoříme také přemístit na adresáři stylů stránky předlohy. I když toto je pouze předvolbu ho může usnadnily trochu Pokud jsme rozhodnout, aby naše aplikace "skinable" v budoucnu.

Po této budeme potřebovat změnit hlavní stránce odkazy v všechny soubory .aspx generována výchozí stránky webových formulářů ASP.NET.

>[!div class="step-by-step"]
[Další](tailspin-spyworks-part-2.md)
