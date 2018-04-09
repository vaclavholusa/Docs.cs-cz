---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Úvod do ladění rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs
author: tfitzmac
description: Ladění je proces vyhledávání a opravy chyb ve znakových stránkách. Tato kapitola se dozvíte, některé nástroje a techniky, které můžete použít k ladění a analyz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: c28d63acda6e585f4aa64f294049c1790faac850
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Úvod do ladění rozhraní ASP.NET Web Pages lokalit (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje různé způsoby, jak ladit stránky na webu technologie ASP.NET Web Pages (Razor). Ladění je proces vyhledávání a opravy chyb ve znakových stránkách.
> 
> **Získáte informace:** 
> 
> - Postup zobrazení informací, která pomáhá analyzovat a ladění stránky.
> - Jak používat ladění nástroje v sadě Visual Studio.
>   
> 
> Toto jsou nové v článku funkce ASP.NET:
> 
> - `ServerInfo` Pomocné rutiny.
> - `ObjectInfo` pomocné rutiny.
>   
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2. Můžete použít službu WebMatrix 3, ale integrované ladicí program není podporován.


Důležitou součást řešení potíží s chybami a problémy ve vašem kódu se vyhnete je na prvním místě. Můžete to udělat umístěním části kódu, které mohou způsobit chyby do `try/catch` bloky. Další informace najdete v části na zpracování chyb v [Úvod do ASP.NET webové programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

`ServerInfo` Helper je diagnostický nástroj, který nabízí přehled informací o prostředí webového serveru, který je hostitelem stránku. Také vám ukazuje informace žádosti HTTP, které odesílají Pokud prohlížeč požaduje stránky. `ServerInfo` Pomocné rutiny zobrazuje aktuální identitu uživatele, typu prohlížeče, který vytvořil požadavek, a tak dále. Tento druh informací může pomoci při odstraňování běžných problémů.

1. Vytvořit novou webovou stránku s názvem *ServerInfo.cshtml*.
2. Na konci stránky, těsně před uzavírací `</body>` značky, přidejte `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Můžete přidat `ServerInfo` kódu kamkoli na stránce. Ale přidání na konci ponechá její výstup oddělené od dalších stránky obsahu, který usnadňuje čtení.

    > [!NOTE] 
    > 
    > **Důležité** byste měli odebrat všechny diagnostický kód z webových stránek před přesunutím webové stránky na provozním serveru. To se týká `ServerInfo` pomocné rutiny, jakož i jiné diagnostické postupy v tomto článku zahrnujících přidání kódu na stránku. Nechcete návštěvníci vašeho webu zobrazíte informace o název serveru, uživatelská jména, cesty na serveru a podobné podrobnosti, protože tento typ informací může být vhodné, když uživatelé se zlými úmysly.
3. Uložit stránky a spusťte ji v prohlížeči.

    ![Debugging-1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo` Pomocník zobrazí čtyři tabulky informace na stránce:

   - Konfigurace serveru. Tato část obsahuje informace o hostování webovém serveru, včetně názvu počítače, verze technologie ASP.NET, které používáte, název domény a čas serveru.
   - Proměnné serveru technologie ASP.NET. Tato část obsahuje podrobné informace o mnoho podrobnosti protokolu HTTP (volané proměnné HTTP) a hodnoty, které jsou součástí každého požadavku na webovou stránku.
   - Informace o běhu programu HTTP. Tato část obsahuje podrobnosti o která verze rozhraní Microsoft .NET Framework je spuštěno webové stránky, cesta, údaje o mezipaměti a tak dále. (Vytvořeným v [Úvod do ASP.NET webové programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890), pomocí syntaxe jsou postaveny na společnosti Microsoft ASP.NET technologii webového serveru, který je sám založený na rozsáhlé softwaru syntaxi Razor rozhraní ASP.NET Web Pages. vývoj knihovny volá rozhraní .NET Framework).
   - Proměnné prostředí. Tato část obsahuje seznam všech proměnných v místním prostředí a jejich hodnoty na webovém serveru.

     Úplný popis všech informací serveru a požadavek je nad rámec tohoto článku, ale můžete uvidíte, že `ServerInfo` helper vrátí spoustu diagnostické informace. Další informace o hodnotách který `ServerInfo` vrátí, najdete v části [rozpoznána proměnné prostředí](https://technet.microsoft.com/library/dd560744(WS.10).aspx) na webu Microsoft TechNet a [proměnné serveru IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) na webu MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Vnoření výstup výrazy k zobrazení hodnot stránky

Jiný způsob, jak zobrazit, co se děje ve vašem kódu je vložení výrazů výstup na stránce. Jak už víte, můžete přímo výstupní hodnotu proměnné přidáním něco podobného jako `@myVariable` nebo `@(subTotal * 12)` na stránku. Pro ladění, můžete umístit tyto výrazy výstup na strategická místa v kódu. To vám umožní zobrazit hodnotu klíče proměnné nebo výsledek výpočty při spuštění stránku. Po dokončení ladění, můžete odebrat výrazy nebo je komentář. Tento postup znázorňuje obvyklý způsob, jak používat vložené výrazy pomoci při ladění na stránce.

1. Vytvoření nové služby WebMatrix stránky s názvem *OutputExpression.cshtml*.
2. Nahraďte stránku obsahu s následujícími službami:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    V příkladu se používá `switch` příkaz zkontrolujte hodnotu `weekday` proměnnou a poté zobrazte zprávu odlišný výstup v závislosti na který den v týdnu je. V příkladu `if` blok v rámci prvního bloku kódu nahodile změní den v týdnu přidáním jeden den na hodnotu aktuální den v týdnu. Jedná se o chybu zavedená pro účely obrázku.
3. Uložit stránky a spusťte ji v prohlížeči.

    Na stránce zobrazuje zpráva nesprávný dne v týdnu. Ať den v týdnu ve skutečnosti je, uvidíte zprávu později jeden den. I když v tomto případě víte, proč se zprávy je vypnutý, (protože kód úmyslně nastaví hodnotu nesprávný den), ve skutečnosti je často obtížné vědět, kde budou věcí nesprávný v kódu. Chcete-li ladit, musíte zjistit, co se děje na hodnotu klíče objektů a proměnné, jako `weekday`.
4. Přidat výstup výrazy vložením `@weekday` jak je znázorněno na dvou místech indikován komentáře v kódu. Tyto výrazy výstupu se zobrazí hodnoty proměnné v tomto bodě v provádění kódu.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Uložte a spusťte stránku v prohlížeči.

    Na stránce se zobrazuje skutečné den v týdnu nejprve pak aktualizované den v týdnu, který výsledkem přidání jeden den a výsledný zprávu od `switch` příkaz. Výstup ze dvou výrazů proměnné (`@weekday`) nemá žádné mezery mezi dny, protože jste nepřidali žádné HTML `<p>` značek k výstupu; výrazy jsou pouze pro testování.

    ![Debugging-2](introduction-to-debugging/_static/image2.jpg)

    Nyní můžete zobrazit, kde je chyba. Při první zobrazení `weekday` proměnných v kódu, se zobrazí správný den. Při zobrazení ji ještě jednou po `if` blokovat v kódu, dne je vypnutý o jednu. Abyste věděli, že něco došlo mezi prvním a druhém vzhled proměnnou den v týdnu. Pokud to byly skutečné chyb, by vám zúžit umístění kód, který je příčinou problému pomoct tento druh přístup.
6. Opravte kód na stránce odebráním dvě výstup výrazy, které jste přidali, a odebrání kód, který změní den v týdnu. Zbývající, kompletní blok kódu vypadá jako v následujícím příkladu:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Spusťte stránku v prohlížeči. Tentokrát uvidíte správné zpráva zobrazí skutečné dne v týdnu.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Využitím pomocné rutiny ObjectInfo k zobrazení hodnot objektu

`ObjectInfo` Pomocné rutiny zobrazuje typ a hodnotu každý objekt můžete předat. Slouží k zobrazení hodnoty proměnných a objektů v kódu (stejně jako výstup výrazy v předchozím příkladu) a navíc zobrazí datový typ informace o objektu.

1. Otevřete soubor s názvem *OutputExpression.cshtml* kterou jste vytvořili dříve.
2. Následující blok kódu nahraďte všechny kódu na stránce:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Uložte a spusťte stránku v prohlížeči.

    ![Debugging-4](introduction-to-debugging/_static/image3.jpg)

    V tomto příkladu `ObjectInfo` Pomocník zobrazí dvě položky:

   - Typ Pro proměnnou první typ je `DayOfWeek`. Pro proměnnou druhý typ je `String`.
   - Hodnota V takovém případě již zobrazují hodnotu proměnné pozdravu na stránce, hodnota se zobrazí znovu při předání proměnnou `ObjectInfo`.

     Pro složitější objekty `ObjectInfo` pomocné rutiny můžete zobrazit další informace &#8212; v podstatě, můžete zobrazit typy a hodnoty všech vlastností objektu.

## <a name="using-debugging-tools-in-visual-studio"></a>Pomocí ladicích nástrojů v sadě Visual Studio

Komplexnější ladění prostředí, použijte Visual Studio 2013 nebo bezplatnou [Visual Studio Express 2013 pro Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Pomocí sady Visual Studio můžete nastavit zarážky v kódu na řádku, který chcete zkontrolovat.

![Sada zarážek](introduction-to-debugging/_static/image1.png)

Při testování webu provádění kódu zastaví u zarážky.

![dosažení zarážek](introduction-to-debugging/_static/image2.png)

Můžete zkontrolovat aktuální hodnoty proměnných a krok prostřednictvím na kód řádek po řádku.

![Zobrazit hodnoty](introduction-to-debugging/_static/image3.png)

Informace o ladění stránek ASP.NET Razor pomocí integrované ladicí program v sadě Visual Studio najdete v tématu [programování webových stránek ASP.NET (Razor) pomocí sady Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Další prostředky

- [Programování webových stránek ASP.NET (Razor) pomocí sady Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Proměnné serveru IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Rozpoznat proměnné prostředí](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
