---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: "Vytváření konzistentního rozložení v rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs"
author: tfitzmac
description: "Chcete-li efektivnější k vytvoření webové stránky pro svůj web, můžete vytvořit opakovaně použitelný bloky obsahu (například záhlaví a zápatí) pro váš web i c je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Vytváření konzistentního rozložení v lokalitách rozhraní ASP.NET Web Pages (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak můžete použít rozložení stránky na webu technologie ASP.NET Web Pages (Razor) k vytvoření opakovaně použitelné bloky obsahu (například záhlaví a zápatí) a k vytvoření konzistentního vzhledu pro všechny stránky v lokalitě.
> 
> **Získáte informace:** 
> 
> - Jak vytvořit opakovaně použitelný bloky obsahu, jako je záhlaví a zápatí stránky.
> - Postup vytvoření konzistentního vzhledu pro všechny stránky ve vaší lokalitě pomocí rozložení.
> - Jak předat data v době běhu ke stránce rozložení.
> 
> Toto jsou nové v článku funkce ASP.NET:
> 
> - Bloky obsahu, které jsou soubory, které obsahují obsah má být vložen do více stránek ve formátu HTML.
> - Rozložení stránek, které jsou stránky, které obsahují obsah ve formátu HTML, který může být sdílen stránky na webu.
> - `RenderPage`, `RenderBody`, A `RenderSection` metody, které v ASP.NET umožňují vložit prvky na stránce.
> - `PageData` Slovník, který umožňuje sdílet data mezi bloky obsahu a rozložení stránky.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>O rozložení stránky

Mnohé weby mají obsah, který se zobrazí na každé stránce, jako je záhlaví a zápatí nebo pole, které informuje uživatele, že jste přihlášeni. Technologie ASP.NET umožňuje vytvořit samostatný soubor s bloku s obsahem, která může obsahovat text, značek a kódu, stejně jako regulární webové stránky. Pak můžete vložit blok s obsahem v jiné stránky na webu, kam chcete informace zobrazit. Tímto způsobem nemusíte zkopírujte a vložte stejný obsah do každé stránce. Vytváření společného obsahu, jako je to také usnadňuje aktualizovat lokalitu. Pokud potřebujete změnit obsah, můžete aktualizovat jenom jeden soubor a změny se projeví všude, kde byla vložena obsah.

Následující diagram znázorňuje, jak obsah blokuje práci. Pokud prohlížeč požaduje stránky z webového serveru, ASP.NET vloží bloky obsahu do bodu kde `RenderPage` metoda je volána na hlavní stránce. Dokončené (sloučené) stránky se pak posílají do prohlížeče.

![Koncepční diagram znázorňující, jak metodu RenderPage vloží odkazované stránku na aktuální stránku.](3-creating-a-consistent-look/_static/image1.jpg)

V tomto postupu vytvoříte stránky, který odkazuje na dva obsahu bloky (záhlaví a zápatí), které jsou umístěné v samostatné soubory. Můžete tyto stejné bloky obsahu na libovolné stránce ve vaší lokalitě. Když jste hotovi, získáte na stránce takto:

![Snímek obrazovky se stránkou v prohlížeči, který je výsledkem spuštění stránky, která obsahuje volání metodě RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. V kořenové složce vašeho webu, vytvořte soubor s názvem *Index.cshtml*.
2. Nahraďte stávající značky s následujícími službami:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. V kořenové složce vytvořte složku s názvem *sdílené*.

    > [!NOTE]
    > Je obvyklé, k uložení souborů, které jsou sdíleny mezi webové stránky ve složce s názvem *sdílené*.
4. V *sdílené* složky, vytvořte soubor s názvem  *\_Header.cshtml*.
5. Nahraďte existující obsah následující:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Všimněte si, že je název souboru  *\_Header.cshtml*, podtržítko (\_) jako předpony. ASP.NET nebude odeslat stránku v prohlížeči, pokud jeho název začíná podtržítkem. To brání tomu, aby lidé požaduje (nechtěně nebo jiné) tyto stránek přímo. Je vhodné použít podtržítkem na název stránky, které obsahují bloky obsahu, ale Opravdu nechcete uživatelům mít možnost požádat tyto stránek &#8212; Existují výhradně pro vložení do dalších stránek.
6. V *sdílené* složky, vytvořte soubor s názvem  *\_Footer.cshtml* a nahraďte obsah následujícím kódem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. V *Index.cshtml* přidejte dva volání `RenderPage` metoda, jak je vidět tady:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    To ukazuje, jak k vložení obsahu blok do webové stránky. Volání `RenderPage` metoda a předejte ji název souboru, jehož obsah, který chcete vložit v daném okamžiku. Sem, chcete vložit obsah  *\_Header.cshtml* a  *\_Footer.cshtml* souborů do *Index.cshtml* souboru.
8. Spustit *Index.cshtml* stránku v prohlížeči. (Ve službě WebMatrix, v **soubory** pracovní prostor, klikněte pravým tlačítkem na soubor a pak vyberte **spustit v prohlížeči**.)
9. V prohlížeči zobrazte zdrojový kód stránky. (Například v aplikaci Internet Explorer, klikněte pravým tlačítkem na stránku a pak klikněte na tlačítko **zobrazit zdroj**.)

    Tímto způsobem můžete zobrazit kód webové stránky, který je odeslán do prohlížeče, které jsou spojeny značek index stránky s bloky obsahu. Následující příklad ukazuje zdroji stránky, který je vykreslen pro *Index.cshtml*. Volání `RenderPage` kterou jste vložili do *Index.cshtml* nahradil skutečný obsah souborů záhlaví a zápatí stránky.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Vytváření konzistentního vzhledu pomocí rozložení stránek

Pokud už víte, že je snadné obsahovat stejný obsah na více stránkách. Více strukturovanými přístup pro vytváření konzistentního vzhledu lokality je používat rozložení stránky. Stránka rozložení definuje strukturu na webové stránce, ale neobsahuje žádný skutečný obsah. Po vytvoření ke stránce rozložení, můžete vytvořit webové stránky, které obsahují obsah a připojte je ke stránce rozložení. Při zobrazení tyto stránek, budete mít formátu podle ke stránce rozložení. (V tomto smyslu ke stránce rozložení funguje jako šablony pro obsah, který je definován v jiné stránky.)

Ke stránce rozložení je stejně jako jakoukoli stránku HTML s tím rozdílem, že obsahuje volání `RenderBody` metoda. Pozice `RenderBody` metoda na stránce rozložení určuje, kde budou zahrnuty informace ze stránky obsahu.

Následující diagram znázorňuje, jak obsahu stránky a stránky rozložení spojují v době běhu k vytvoření dokončení webové stránky. V prohlížeči požadavků stránku obsahu. Stránky obsahu má kód v něm, který určuje ke stránce rozložení pro strukturu stránky. Na stránce rozložení obsahu vložen v místě, kde `RenderBody` metoda je volána. Obsahu bloky lze také vložit do ke stránce rozložení voláním `RenderPage` metoda, způsob, jak jste to udělali v předchozí části. Po dokončení webové stránky je odesláno prohlížeči.

![Snímek obrazovky se stránkou v prohlížeči, který je výsledkem spuštění stránky, která obsahuje volání metodě RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

Následující postup ukazuje, jak vytvořit rozložení stránky obsahu stránce a odkaz na něj.

1. V *sdílené* složku vašeho webu, vytvořte soubor s názvem  *\_Layout1.cshtml*.
2. Nahraďte existující obsah následující:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Můžete použít `RenderPage` metoda na stránce rozložení k vložení obsahu bloků. Stránka rozložení může obsahovat pouze jedno volání `RenderBody` metoda.
3. V *sdílené* složky, vytvořte soubor s názvem  *\_Header2.cshtml* a nahraďte existující obsah následujícím kódem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. V kořenové složce vytvořte novou složku a pojmenujte ji *styly*.
5. V *styly* složky, vytvořte soubor s názvem *Site.css* a přidejte následující definice:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Tyto definice styl jsou zde pouze k zobrazení, použití šablony stylů s rozložení stránky. Pokud chcete, můžete definovat vlastní styly pro tyto elementy.
6. V kořenové složce vytvořte soubor s názvem *Content1.cshtml* a nahraďte existující obsah následujícím kódem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Toto je stránka, který bude používat ke stránce rozložení. Blok kódu v horní části stránky určuje, které stránky rozložení pomocí formátu tento obsah.
7. Spustit *Content1.cshtml* v prohlížeči. Na vykreslené stránce používá formát a šablony stylů definované v  *\_Layout1.cshtml* a text (obsah) definované v *Content1.cshtml*.

    ![[Obrázek]](3-creating-a-consistent-look/_static/image4.jpg)

    Krok 6 k vytvoření další stránky obsahu, které pak mohou sdílet stejné stránce rozložení, můžete opakovat.

    > [!NOTE]
    > Váš web můžete nastavit tak, aby mohou automaticky používat stejné stránce rozložení pro všechny stránky obsahu ve složce. Podrobnosti najdete v tématu [přizpůsobení chování na webu pro webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Návrh rozložení stránek, které mají více oddílů obsahu

Stránky obsahu může mít více oddílů, což je užitečné, pokud chcete použít rozložení, které mají více oblastí s nahraditelný obsah. Na stránce obsahu dáváte každá část jedinečný název. (Výchozí oddíl je ponechán bez názvu.) Na stránce rozložení přidáte `RenderBody` metoda k určení, kde by se měla objevit části nepojmenované (výchozí). Potom přidáte jako samostatné `RenderSection` metody k vykreslení části s názvem jednotlivě.

Následující diagram znázorňuje, jak ASP.NET zpracovává obsah, který je rozdělen do více oddílů. Každý pojmenovaný oddíl je obsažený v bloku části na stránce obsahu. (Jste se s názvem `Header` a `List` v příkladu.) Rozhraní framework vloží oddíl obsahu do ke stránce rozložení v místě, kde `RenderSection` metoda je volána. Části nepojmenované (výchozí) je vložen v místě, kde `RenderBody` metoda je volána, jako jste viděli dříve.

![Koncepční diagram znázorňující, jak metodu RenderSection vloží části odkazy na aktuální stránku.](3-creating-a-consistent-look/_static/image5.jpg)

Tento postup ukazuje, jak vytvořit stránku obsahu, který má více oddílů obsahu a jak vykreslit ho pomocí rozložení stránky, která podporuje víc oddílů obsahu.

1. V *sdílené* složky, vytvořte soubor s názvem  *\_Layout2.cshtml*.
2. Nahraďte existující obsah následující:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Můžete použít `RenderSection` metoda k vykreslení záhlaví a seznam oddílů.
3. V kořenové složce vytvořte soubor s názvem *Content2.cshtml* a nahraďte existující obsah následujícím kódem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Tato stránka obsahu obsahuje blok kódu v horní části stránky. Každý pojmenovaný oddíl je obsažený v části bloku. Zbývající stránky obsahuje oddíl obsahu výchozí (nepojmenované).
4. Spustit *Content2.cshtml* v prohlížeči.

    ![Snímek obrazovky se stránkou v prohlížeči, který je výsledkem spuštění stránky, která obsahuje volání metodě RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Provedení nepovinné oddíly obsahu

Za normálních okolností oddíly, které vytvoříte v obsahu stránce se musí shodovat oddíly, které jsou definovány v ke stránce rozložení. Pokud dojde k některé z těchto situací, může docházet k chybám:

- Stránky obsahu obsahuje oddíl, který nemá žádné odpovídající části na stránce rozložení.
- Stránka rozložení obsahuje oddíl, který není k dispozici žádný obsah.
- Stránka rozložení obsahuje volání metody, které se pokusí o vykreslení do stejné části více než jednou.

Však můžete přepsat toto chování pro pojmenovaný oddíl pomocí deklarace oddílu, který má být volitelné na stránce rozložení. To umožňuje definovat více obsahu stránek, které můžete sdílet ke stránce rozložení, ale který může nebo nemusí mít obsah pro určitou část.

1. Otevřete *Content2.cshtml* a odeberte v následující části:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Uložit stránky a spusťte jej v prohlížeči. Zobrazí se chybová zpráva, protože obsahu stránce neposkytuje obsah pro oddíl na stránce rozložení, a to v záhlaví části definován.

    ![Snímek obrazovky, který se zobrazuje chyba, k níž dojde, pokud spustíte na stránce, která volá metodu RenderSection, ale není k dispozici odpovídající části.](3-creating-a-consistent-look/_static/image7.jpg)
3. V *sdílené* složku, otevřete  *\_Layout2.cshtml* stránky a nahraďte tento řádek:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    následujícím kódem:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Jako alternativu může na předchozí řádek kódu nahraďte následující blok kódu, který vytváří stejné výsledky:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Spustit *Content2.cshtml* stránku v prohlížeči znovu. (Pokud máte tuto stránku, otevřete v prohlížeči, je můžete stejně ji aktualizovat.) Tentokrát stránka se zobrazí v k žádné chybě, i když má žádné záhlaví.

## <a name="passing-data-to-layout-pages"></a>Předávání dat na rozložení stránky

Můžete mít dat definované na stránce obsahu, který potřebujete naleznete na stránce rozložení. Pokud ano, musíte předat data ze stránky obsahu stránce rozložení. Například můžete chtít zobrazit stav přihlášení uživatele, nebo můžete chtít zobrazit nebo skrýt oblasti obsahu, které jsou založené na vstup uživatele.

Předat data ze stránky obsahu stránce rozložení, které můžete vložit hodnoty do `PageData` vlastnost stránky obsahu. `PageData` Vlastnost je kolekce dvojic název hodnota, které mají data, která chcete předat mezi stránkami. Na stránce rozložení můžete pak čtení hodnoty z `PageData` vlastnost.

Zde je diagram jiné. Tato ukazuje, jak můžete pomocí technologie ASP.NET `PageData` vlastnost, která má předat hodnoty ze stránky obsahu stránce rozložení. Po zahájení vytváření webové stránky ASP.NET vytvoří `PageData` kolekce. Na stránce obsahu můžete napsat kód pro umístit data do `PageData` kolekce. Hodnoty v `PageData` kolekci může také být přístup ostatní části obsahu stránce nebo další bloky obsahu.

![Koncepční diagram, který ukazuje, jak naplnit slovník PageData a předejte tyto informace na stránce rozložení stránky obsahu.](3-creating-a-consistent-look/_static/image8.jpg)

Následující postup ukazuje, jak předat data ze stránky obsahu stránce rozložení. Při spuštění stránky zobrazí tlačítko, které umožňuje uživateli skrytí nebo zobrazení seznamu, který je definován v ke stránce rozložení. Když uživatel klikne na tlačítko, nastaví hodnotu true nebo false (Boolean) `PageData` vlastnost. Ke stránce rozložení přečte tuto hodnotu a pokud je nastavena hodnota false, skryje seznamu. Hodnota se také používá obsahu stránce k určení, jestli se má zobrazit **skrýt seznam** tlačítko nebo **zobrazit seznam** tlačítko.

![[Obrázek]](3-creating-a-consistent-look/_static/image9.jpg)

1. V kořenové složce vytvořte soubor s názvem *Content3.cshtml* a nahraďte existující obsah následujícím kódem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Kód ukládá dva kusy data `PageData` vlastnost &#8212; název webové stránky a hodnotu true nebo false určuje, jestli k zobrazení seznamu.

    Všimněte si, že technologie ASP.NET umožňuje vložena kód HTML stránky podmíněně pomocí bloku kódu. Například `if/else` bloku v těle stránky určuje, jaký typ k zobrazení v závislosti na tom, jestli se `PageData["ShowList"]` je nastaven na hodnotu true.
2. V *sdílené* složky, vytvořte soubor s názvem  *\_Layout3.cshtml* a nahraďte existující obsah následujícím kódem:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Stránka rozložení obsahuje výraz v `<title>` element, který získá hodnotu title z `PageData` vlastnost. Používá také `ShowList` hodnotu `PageData` vlastnosti k určení, jestli se má zobrazit blok s obsahem seznamu.
3. V *sdílené* složky, vytvořte soubor s názvem  *\_List.cshtml* a nahraďte existující obsah následujícím kódem:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Spustit *Content3.cshtml* stránku v prohlížeči. Stránka se zobrazí v seznamu na levé straně stránky zobrazí a **skrýt seznam** tlačítko dole.

    ![Snímek obrazovky se stránkou, která zahrnuje seznamu a tlačítko, které říká "Skrýt seznam".](3-creating-a-consistent-look/_static/image10.jpg)
5. Klikněte na tlačítko **skrýt seznam**. Zmizí seznamu a tlačítko se změní na **zobrazit seznam**.

    ![Snímek obrazovky se stránkou, která nezahrnuje seznamu a tlačítko, které říká zobrazit seznam.](3-creating-a-consistent-look/_static/image11.jpg)
6. Klikněte **zobrazit seznam** tlačítko a v seznamu se zobrazí znovu.

## <a name="additional-resources"></a>Další prostředky


[Přizpůsobení chování na webu pro webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
