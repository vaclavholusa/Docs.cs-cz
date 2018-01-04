---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "Představení technologie ASP.NET Web Pages – odstranění dat z databáze | Microsoft Docs"
author: tfitzmac
description: "V tomto kurzu se dozvíte, jak odstranit položku jednotlivé databáze. Předpokládá se, že jste dokončili řady prostřednictvím aktualizace dat databáze v adrese poskytovatele. technologie ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 5bc92b5d40e7a55dcd730d552c71031d913b277e
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/03/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Představení technologie ASP.NET Web Pages – odstranění dat z databáze
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> V tomto kurzu se dozvíte, jak odstranit položku jednotlivé databáze. Předpokládá, že jste dokončili řady prostřednictvím [aktualizace dat databáze na webových stránkách ASP.NET](updating-data.md).
> 
> Získáte informace:
> 
> - Jak vybrat jednotlivé záznamy ze seznamu záznamů.
> - Jak odstranit jeden záznam z databáze.
> - Postup kontroly, aby se určité tlačítko označeného ve formuláři.
>   
> 
> Funkce nebo technologie popsané:
> 
> - `WebGrid` Pomocné rutiny.
> - SQL `Delete` příkaz.
> - `Database.Execute` Metodu pro spuštění SQL `Delete` příkaz.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu předchozí jste zjistili, jak k aktualizaci záznamů existující databáze. V tomto kurzu je podobný, s tím rozdílem, že místo aktualizace záznamu, budete ho odstranit. Procesů, které jsou podobné, s tím rozdílem, že odstranění je jednodušší, tak v tomto kurzu bude krátký.

V *filmy* stránky, budete aktualizovat `WebGrid` pomocné rutiny, které se zobrazí **odstranit** odkaz vedle jednotlivých film, který může doprovázet **upravit** propojení, které jste přidali dříve.

![Filmy stránky zobrazující odstraní propojení pro každý film](deleting-data/_static/image1.png)

Stejně jako u úpravy, když kliknete **odstranit** odkaz, tím přejdete na jinou stránku, kde informace film je již ve formuláři:

![Odstranit stránky film film zobrazí](deleting-data/_static/image2.png)

Klikněte na tlačítko trvale odstranit tento záznam.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Přidání odkazu odstranění na film výpis

Budete Začněte přidáním **odstranit** propojit `WebGrid` pomocné rutiny. Tento odkaz je podobná **upravit** propojení, které jste přidali v předchozím kurzu.

Otevřete *Movies.cshtml* souboru.

Změna `WebGrid` značek v těle stránky přidáním sloupce. Tady je upravených značek:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Nový sloupec, je tato:

[!code-html[Main](deleting-data/samples/sample2.html)]

Způsob, jak je nakonfigurovaný mřížky, **upravit** sloupec je krajní levá v mřížce a **odstranit** sloupec je nejvíce vpravo. (Středník po `Year` teď sloupce v případě, že nebyla zjistíte, že.) Není co speciální o tom, kde navštěvují tyto sloupce odkaz a by snadno mohlo vedle sebe. V takovém případě jsou samostatný, aby byly těžší získat promíchají.

![Označena filmy stránka s odkazy upravit a podrobnosti zobrazíte, že nejsou vedle sebe](deleting-data/_static/image3.png)

Nové sloupci se zobrazuje odkaz (`<a>` element) jejíž text říká "Odstranit". Cíl odkazu (jeho `href` atribut) je kód, který se nakonec přeloží na něco podobného jako tato adresa URL se `id` hodnotu pro každý film jiné:

[!code-css[Main](deleting-data/samples/sample3.css)]

Tento odkaz bude vyvolat stránku s názvem *DeleteMovie* a předejte ji ID film jste vybrali.

V tomto kurzu nepřejde do podrobnosti o tom, jak je vytvořený tento odkaz, protože je téměř identický **upravit** odkaz z předchozí kurzu ([aktualizace dat databáze na webových stránkách ASP.NET](updating-data.md)).

## <a name="creating-the-delete-page"></a>Vytvoření stránky odstranění

Nyní můžete vytvořit stránku, která budou cílem pro **odstranit** odkaz v mřížce.

> [!NOTE] 
> 
> **Důležité** technika výběrem položky záznam odstranit a potom pomocí samostatné stránce a tlačítko potvrďte proces je velmi důležité pro zabezpečení. Protože jste si přečetli v předchozí kurzy, což *žádné* řazení změny na váš web by měl *vždy* provést pomocí formuláře &mdash; tedy pomocí operace HTTP POST. Pokud je možné změnit webu právě kliknutím na odkaz (který používá operaci GET), osoby může provádět jednoduché požadavky na váš web a odstraňovat data. I vyhledávání prohledávací modul, který je indexování webu může nechtěně odstranit data jenom pomocí následujících odkazů.
> 
> Pokud vaše aplikace umožňuje změnit záznam osoby, budete muset prezentovat záznam uživatele pro úpravy přesto. Ale mohou být tendenci Přeskočit tento krok pro odstranění záznamu. Není ale Přeskočit tento krok. (Je také užitečné pro uživatele, které chcete zobrazit záznam a potvrďte, že se při odstraňování záznamů, které jsou určeny.)
> 
> V dalších kurz sady zobrazí se postup přidání funkce přihlášení, takže uživatel bude mít k přihlášení před odstraněním záznamu.


Vytvoření stránky s názvem *DeleteMovie.cshtml* a nahraďte, co je v souboru s následující kód:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Tento kód je stejná jako *EditMovie* stránky, vyjma toho, že místo použití polí (`<input type="text">`), obsahuje kód `<span>` elementy. Není co zde můžete upravit. Všechny, které musíte udělat, je zobrazit podrobnosti o film tak, aby uživatelé měli jistotu, že se při odstraňování správné film.

Kód již obsahuje odkaz, který umožňuje uživateli návrat na stránku film výpis.

Jako v *EditMovie* stránky, ID vybrané film je uložený ve skrytém poli. (Předána na stránku na prvním místě jako hodnotu řetězce dotazu.) Došlo `Html.ValidationSummary` volání, které se zobrazí chyby ověření. V takovém případě chyba může být, že žádné ID film byla předána na stránku nebo že film ID je neplatné. Tato situace může nastat, pokud někdo spuštěn této stránky bez první výběr video v *filmy* stránky.

Tlačítko titulek **odstranit film**, a jeho název atributu je nastavena na `buttonDelete`. `name` Atribut bude použit v kódu k identifikaci tlačítko odeslání formuláře.

Budete muset napsat kód pro 1) přečíst podrobnosti o film při prvním zobrazení stránky a 2) ve skutečnosti odstraňte video, když uživatel klikne na tlačítko.

## <a name="adding-code-to-read-a-single-movie"></a>Přidání kódu ke čtení jeden film

V horní části *DeleteMovie.cshtml* přidejte následující blok kódu:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Tento kód je stejné jako odpovídající kód *EditMovie* stránky. Získá ID film mimo řetězec dotazu a používá ID přečíst záznam z databáze. Tento kód obsahuje ověřovací test (`IsInt()` a `row != null`) a ujistěte se, zda je platné ID film předávány na stránku.

Mějte na paměti, že tento kód měly být spuštěny pouze při prvním spuštění stránky. Nechcete, aby znovu načíst film záznam z databáze, když uživatel klikne **odstranit film** tlačítko. Proto, chcete-li číst film je uvnitř testu, která uvádí, že kód `if(!IsPost)` &mdash; tedy *Pokud se nejedná o požadavek operaci post (odeslání formuláře)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Přidání kódu k odstranění vybraného videa

Pokud chcete odstranit videa, když uživatel klikne na tlačítko, přidejte následující kód pouze uvnitř složená závorka `@` bloku:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Tento kód je podobný kódu pro aktualizaci stávajícího záznamu, ale jednodušší. Spuštění kódu v podstatě SQL `Delete` příkaz.

 Jako v *EditMovie* stránky, je kód v `if(IsPost)` bloku. Tato doba `if()` je trochu složitější podmínky: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Existují dvě podmínky sem. První je, že je odeslání stránky, jako jste se seznámili s před &mdash; `if(IsPost)`.

Druhou podmínku, která je `!Request["buttonDelete"].IsEmpty()`, což znamená, že žádost má objekt s názvem `buttonDelete`. Admittedly je nepřímým způsobem testování, které tlačítko odeslání formuláře. Pokud formulář obsahuje více tlačítka pro odeslání dat, zobrazí se pouze název tlačítka, které bylo kliknuto v požadavku. Proto, logicky Pokud název konkrétní tlačítko se zobrazí v požadavku &mdash; nebo jak jsme uvedli v kódu, pokud toto tlačítko není prázdný &mdash; , je tlačítko odeslání formuláře.

`&&` Operátor znamená "a" (logické a). Proto celý `if` podmínka je...

*Tento požadavek je post (ne prvního požadavku)*  
  
 AND  
  
*Tlačítko* `buttonDelete` *byl tlačítko odeslání formuláře.*

Tento formulář (v faktu, tato stránka) obsahuje pouze jedno tlačítko, takže další test pro `buttonDelete` není technicky povinný. Stále Chystáte se provést operaci, která bude trvale odebrat data. Chcete proto ujistěte se, co nejblíže se jenom v případě, že uživatel ji explicitně vyžaduje provedení operace. Předpokládejme například, rozšířit tuto stránku později a do ní přidat další tlačítka. Dokonce i pak kód, který odstraní videa se spustí pouze v případě `buttonDelete` bylo stisknuto tlačítko.

Jako v *EditMovie* stránky, od skryté pole získat ID a pak spustit příkaz SQL. Syntaxe `Delete` příkaz je:

`DELETE FROM table WHERE ID = value`

Je důležité zahrnout `WHERE` klauzule a ID. Pokud necháte out klauzuli WHERE *se odstraní všechny záznamy v tabulce*. Protože jste viděli, předat hodnotu ID do příkazu SQL pomocí zástupný symbol.

## <a name="testing-the-movie-delete-process"></a>Testování procesu odstranění filmu

Nyní můžete otestovat. Spustit *filmy* a klikněte na tlačítko **odstranit** vedle film. Když *DeleteMovie* stránky se zobrazí, klikněte na tlačítko **odstranit film**.

![Odstranit stránku film s tlačítkem odstranit filmu](deleting-data/_static/image4.png)

Když kliknete na tlačítko, kód odstraní filmy a vrátí na film výpis. Existuje vyhledejte odstraněné film a ověřte, zda je odstraněn.

## <a name="coming-up-next"></a>Objevuje další

V dalším kurzu se dozvíte, jak poskytnout všechny stránky na svém webu běžných vzhled a rozložení.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Úplný seznam pro stránku Movie (aktualizované odkazy odstranění)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Úplný seznam DeleteMovie stránky

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Další prostředky

- [Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor](../introducing-razor-syntax-c.md)
- [Příkaz DELETE SQL](http://www.w3schools.com/sql/sql_delete.asp) na webu W3Schools

>[!div class="step-by-step"]
[Předchozí](updating-data.md)
[další](layouts.md)
