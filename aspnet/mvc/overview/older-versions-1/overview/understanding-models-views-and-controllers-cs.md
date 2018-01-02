---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: "Principy modely, zobrazení a Kontrolery (C#) | Microsoft Docs"
author: StephenWalther
description: "Nerozumíte o modely, zobrazení a Kontrolery? V tomto kurzu Stephen Walther vás seznámí s různé části aplikaci ASP.NET MVC."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c9a0cbbf6f786944d7892fbb14859939f21bdd5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="understanding-models-views-and-controllers-c"></a>Principy modely, zobrazení a Kontrolery (C#)
====================
podle [Stephen Walther](https://github.com/StephenWalther)

> Nerozumíte o modely, zobrazení a Kontrolery? V tomto kurzu Stephen Walther vás seznámí s různé části aplikaci ASP.NET MVC.


V tomto kurzu poskytuje souhrnné informace o architektuře ASP.NET MVC modely, zobrazení a kontrolery. Jinými slovy, vysvětluje M', V "a C' v architektuře ASP.NET MVC.

Po přečtení tohoto kurzu, byste měli porozumět, jak fungují společně různé části aplikaci ASP.NET MVC. Také byste měli porozumět, jak architektuře aplikace ASP.NET MVC se liší od aplikace webových formulářů ASP.NET nebo aplikace Active Server Pages.

## <a name="the-sample-aspnet-mvc-application"></a>Ukázkovou aplikaci ASP.NET MVC

Výchozí šablony sady Visual Studio pro vytváření webových aplikací ASP.NET MVC zahrnuje velmi jednoduché ukázkové aplikace, která slouží k pochopení různých součástí aplikace ASP.NET MVC. Můžeme využít této jednoduché aplikace v tomto kurzu.

Můžete vytvořit novou aplikaci ASP.NET MVC pomocí šablony MVC spuštěním Visual Studio 2008 a vyberete možnost nabídky soubor, New Project (viz obrázek 1). V dialogovém okně Nový projekt, vyberte svůj oblíbený programovací jazyk v části typy projektů (Visual Basic a C#) a vyberte **webové aplikace ASP.NET MVC** v rámci šablony. Klikněte na tlačítko OK.


[![Dialogové okno Nový projekt](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Obrázek 01**: dialogové okno Nový projekt ([Kliknutím zobrazit obrázek v plné velikosti](understanding-models-views-and-controllers-cs/_static/image2.png))


Při vytváření nové aplikace ASP.NET MVC **vytvoření projektu testování částí** otevře se dialogové okno (viz obrázek 2). Tento dialog umožňuje vytvoření samostatné projektu ve vašem řešení pro testování vašich aplikací ASP.NET MVC. Vyberte možnost **Ne, nevytvářejte projektu testování částí** a klikněte na **OK** tlačítko.


[![Vytvořte jednotku Test – dialogové okno](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Obrázek 02**: dialogové okno Vytvořit testovací jednotky ([Kliknutím zobrazit obrázek v plné velikosti](understanding-models-views-and-controllers-cs/_static/image4.png))


Aplikace je vytvořena po nové rozhraní ASP.NET MVC. Zobrazí se několik složek a souborů v okně Průzkumníka řešení. Konkrétně se zobrazí tři složky s názvem modely, zobrazení a Kontrolery. Jak vám může uhodnout z názvy složek, tyto složky obsahují soubory pro implementaci modely, zobrazení a kontrolery.

Pokud rozbalíte složce řadiče, měli byste vidět soubor s názvem AccountController.cs a soubor s názvem HomeController.cs. Pokud rozbalíte složce zobrazení, měli byste vidět tři podsložky s názvem účet domů a sdílený. Pokud rozbalíte Domovská složka, se zobrazí dva další soubory s názvem About.aspx a Index.aspx (viz obrázek 3). Tyto soubory tvoří ukázkovou aplikaci součástí výchozí šablony ASP.NET MVC.


[![Okno Průzkumník řešení](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Obrázek 03**: okno Průzkumníka řešení ([Kliknutím zobrazit obrázek v plné velikosti](understanding-models-views-and-controllers-cs/_static/image6.png))


Ukázkovou aplikaci můžete spustit výběrem možnosti nabídky **ladit, spustit ladění**. Alternativně můžete stisknutím klávesy F5.

Při prvním spuštění aplikace ASP.NET, se zobrazí dialogové okno Obrázek 4, který se doporučuje povolit režim ladění. Klikněte na tlačítko OK a bude aplikace spuštěna.


[![Ladění není povoleno dialogové okno](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Obrázek 04**: ladění není povoleno dialogové okno ([Kliknutím zobrazit obrázek v plné velikosti](understanding-models-views-and-controllers-cs/_static/image8.png))


Když spustíte aplikaci ASP.NET MVC, Visual Studio spustí aplikaci ve webovém prohlížeči. Ukázkové aplikace se skládá z pouze dvě stránky: indexovou stránku a o stránky. Při prvním spuštění aplikace, zobrazí se stránka indexu (viz obrázek 5). Můžete přejít na stránku o kliknutím na odkaz nabídce v horní části napravo od aplikace.


[![Index stránky](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Obrázek 05**: indexovou stránku ([Kliknutím zobrazit obrázek v plné velikosti](understanding-models-views-and-controllers-cs/_static/image11.png))


Všimněte si, adresy URL v adresním řádku prohlížeče. Například když kliknete na odkaz nabídky o, adresu URL v adresním řádku prohlížeče, které se změní na **/domácí/o**.

Pokud zavřete okno prohlížeče a vrátit se k sadě Visual Studio, nebudete moci vyhledat soubor s domovskou cesta/o. Soubory neexistují. Jak je to možné?

## <a name="a-url-does-not-equal-a-page"></a>Adresa URL se nerovná stránky

Při sestavování tradiční aplikace webových formulářů ASP.NET nebo aplikace Active Server Pages je souvislosti mezi adresu URL a na stránce. Pokud budete požadovat stránku s názvem SomePage.aspx ze serveru, pak měl lépe existovat stránky na disk s názvem SomePage.aspx. Pokud soubor SomePage.aspx neexistuje, dojde ugly **404 - Stránka nebyla nalezena** chyby.

Při vytváření aplikace ASP.NET MVC, spočívá v tom souvztažnost mezi adresu URL, kterou zadáte do adresního řádku prohlížeče a soubory, které můžete najít ve vaší aplikaci. V aplikaci ASP.NET MVC odpovídá adrese URL akce kontroleru místo na disku na stránce.

V tradiční aplikaci ASP.NET nebo ASP prohlížeče požadavek mapovaný na stránky. V aplikaci ASP.NET MVC naopak požadavků prohlížeče jsou namapované na akce kontroleru. Aplikace webových formulářů ASP.NET je zaměřená na obsah. Aplikace ASP.NET MVC spočívá v tom, zaměřená na aplikace logiky.

## <a name="understanding-aspnet-routing"></a>Vysvětlení směrování ASP.NET

Získá žádost prohlížeče namapované na akce kontroleru prostřednictvím funkce rozhraní ASP.NET volat *směrování ASP.NET*. Směrování ASP.NET používá rozhraní ASP.NET MVC pro *trasy* příchozí požadavky na akce kontroleru.

Směrování ASP.NET používá směrovací tabulku pro zpracování příchozích požadavků. Tato tabulka trasy se vytvoří při prvním spuštění webové aplikace. Směrovací tabulka je instalační program v souboru Global.asax. Výchozí soubor MVC Global.asax je součástí výpis 1.

**Výpis 1 - Global.asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Při prvním spuštění aplikace ASP.NET, aplikace\_Start() metoda je volána. V výpis 1 Tato metoda volá metodu RegisterRoutes() a RegisterRoutes() metoda vytvoří výchozí směrovací tabulka.

Výchozí směrovací tabulka se skládá z jednoho z nich. Této výchozí trase všechny příchozí požadavky dělí do tří segmenty (segment adresy URL je nic mezi lomítka). První segment se mapuje na název řadiče, druhý segment je namapovaná na název akce a poslední segment je namapována na parametr předaný akci s názvem ID.

Zvažte například následující adresu URL:

/ Produktu/podrobnosti/3

Tato adresa URL rozdělena na tři parametry takto:

Řadič = produktu

Akce = podrobnosti

ID = 3

Výchozí trasy definované v souboru Global.asax obsahuje výchozí hodnoty pro všechny tři parametry. Je výchozí řadič Domů, výchozí akce je Index a výchozí hodnota Id je prázdný řetězec. Tyto výchozí hodnoty na paměti zvažte, jak je analyzována na následující adrese URL:

/ Zaměstnance

Tato adresa URL rozdělena na tři parametry takto:

Řadič = zaměstnance

Akce = indexu

ID =

Nakonec otevřete aplikaci ASP.NET MVC bez nutnosti zadávat libovolná adresa URL (například `http://localhost`) a adresa URL je analyzována takto:

Řadič = Domů

Akce = indexu

ID =

Žádost se směruje na Index() akce v třídě HomeController.

## <a name="understanding-controllers"></a>Vysvětlení Kontrolerů

Řadič slouží k řízení způsobu, jakým uživatel pracuje s aplikaci MVC. Řadič obsahuje logiku toku řízení pro aplikaci ASP.NET MVC. Řadič Určuje, jaké odpověď k odeslání zpět na uživatele, když uživatel provede žádost prohlížeče.

Řadič je právě třídy (například třída Visual Basic a C#). Ukázkové aplikace ASP.NET MVC zahrnuje řadič HomeController.cs umístěný ve složce řadiče. Obsah souboru HomeController.cs je uveden v výpis 2.

**Výpis 2 - HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Všimněte si, že HomeController má dvě metody s názvem Index() a About(). Tyto dvě metody odpovídají dvě akce vystavené kontroleru. Adresa URL/Home nebo Index vyvolá metodu HomeController.Index() a adresy URL/Home nebo o vyvolá metodu HomeController.About().

Všechny veřejná metoda v kontroleru je k dispozici jako akce kontroleru. Budete muset to velmi opatrně. To znamená, že všechny veřejná metoda obsažené v řadič může vyvolat každý, kdo má přístup k Internetu zadáním správné adresy URL do prohlížeče.

## <a name="understanding-views"></a>Principy zobrazení

Dva řadiče akce vystavené třídě HomeController Index() a About(), jak vrátit zobrazení. Zobrazení obsahuje kód HTML a obsah, který je odesláno prohlížeči. Zobrazení je ekvivalentem stránky při práci s aplikaci ASP.NET MVC.

Je třeba vytvořit zobrazení na správné místo. Akce HomeController.Index() vrátí zobrazení umístěný v následující cestě:

\Views\Home\Index.aspx

Akce HomeController.About() vrátí zobrazení umístěný v následující cestě:

\Views\Home\About.aspx

Obecně platí Pokud chcete vrátit zobrazení pro akce kontroleru, bude nutné vytvořit podsložku ve složce zobrazení se stejným názvem jako řadiči. V rámci podsložku musíte vytvořit soubor .aspx se stejným názvem jako akce kontroleru.

Soubor v výpis 3 obsahuje About.aspx zobrazení.

**Výpis 3 – About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Pokud budete ignorovat v prvním řádku výpis 3, se skládá z standardní HTML většinu zbytek zobrazení. Obsah zobrazení můžete upravit tak, že zadáte všechny HTML, který chcete v tomto poli.

Zobrazení je velmi podobná stránce Active Server Pages nebo webových formulářů ASP.NET. Zobrazení může obsahovat obsah HTML a skripty. Skripty můžete napsat v vaše oblíbené .NET programovací jazyk (například C# nebo Visual Basic .NET). Pomocí skriptů zobrazíte dynamický obsah, jako jsou data databáze.

## <a name="understanding-models"></a>Principy modely

Mít jsme probrali řadiče a mít jsme probrali zobrazení. Poslední téma, která potřebujeme k zabývat se modelů. Co je MVC model?

MVC model obsahuje všechny vaše aplikace logiky, která není obsažen v zobrazení nebo kontroleru. Model musí obsahovat všechny vaše obchodní logiku aplikace, logiku ověření a logiku přístup k databázi. Například pokud používáte Microsoft Entity Framework pro přístup k vaší databázi, pak vytvoříte vaší třídy rozhraní Entity Framework (souboru .edmx) ve složce modelů.

Zobrazení by měl obsahovat pouze logiku související generování uživatelského rozhraní. Řadič by měly obsahovat jenom úplné minimum logiku potřebnou k vrátí pravého zobrazení nebo přesměruje uživatele na další akci (řízení toku). Všem ostatním musí být obsažené v modelu.

Obecně platí by měla zajistit fat modely a tenké řadiče. Vaše řadiče metody by měly obsahovat jenom pár řádků kódu. Akce kontroleru získá příliš fat, měli zvážit, přesunutí logiku na nové třídy ve složce modelů.

## <a name="summary"></a>Souhrn

V tomto kurzu k dispozici nejvyšší úrovni přehled různých součástí ASP.NET MVC webové aplikace. Jste se dozvěděli, jak směrování ASP.NET mapuje příchozí požadavky na prohlížeč pro určitý kontroler akce. Jste se dozvěděli, jak řadiče orchestraci, jak se zobrazení vrátí do prohlížeče. Nakonec jste zjistili, jak modely obsahovat obchodní aplikace, ověřování a logiku přístup k databázi.
