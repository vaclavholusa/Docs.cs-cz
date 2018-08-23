---
uid: single-page-application/overview/templates/breezeknockout-template
title: Šablona breeze/Knockout | Dokumentace Microsoftu
author: madskristensen
description: Šablona breeze/Knockout jednostránkové aplikace
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 006d360748674a645ceddb82017f68b0f80f041b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751886"
---
<a name="breezeknockout-template"></a>Šablona breeze/Knockout
====================
podle [Autor: Mads Kristensen](https://github.com/madskristensen)

> Šablona Breeze/Knockout MVC zapsal pře zvonku
> 
> [Stažení šablony MVC Breeze/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Vyslyšeli jsme "jednostránkové aplikace" (SPA) a přemýšleli, co to je. Když si může přečíst o tom, neprovádějí místo pro sebe. Ale který má čas Stáhnout ukázku? Pokud máte Visual Studio, budete mít příklad SPA a spuštěn v menší než 60 sekund s ASP.NET MVC 4 šablony "Breeze/Knockout jednostránkové aplikace".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Co je jednostránková aplikace šablona Breeze/Knockout?

Většina šablon projektů generovat kostře aplikaci. Vložit věnovat v těchto kosti přidáním kódu a konečnou funkční aplikaci poskytovat. Šablona Breeze/Knockout SPA se liší. Generuje ukázkové aplikace můžete zkoumat. Ukazuje návrh aplikace SPA a řadu techniky pro vytváření SPA.

Šablona Breeze/Knockout varianta nachází [šablona KnockoutJS SPA](../introduction/knockoutjs-template.md) součástí technologie ASP.NET a Web Tools 2012.2 Update. Šablona Breeze SPA generuje aplikace pomocí stejné prostředí pro uživatele, ale má jinou implementaci, pomocí Breeze pro správu dat na.

Šablona KnockoutJS SPA díky žádostí o službu pomocí nezpracované jQuery jazyka AJAX, který je vhodný pro jednoduchou aplikaci. Ale mít náročnějších požadavků správy dat sofistikovanějších aplikací. Například většina aplikací:

- Dotaz a znovu zadat dotaz na server během relace rozšířených uživatele.
- Přidáte dotaz filtry, řazení a stránkování.
- Stejná data sdílejte mezi více obrazovek.
- Accumulate změny mnoho objektů a pak je ukládejte jako jedna transakce.
- Ověření změn na straně klienta, takže uživatel může opravit chyby před potvrzením změn do databáze.

Knihovna BreezeJS zpracovává tyto vás ujmeme rutinních úkolů za vás, můžete k vývoji aplikace logiky a uživatelského prostředí, který nejvíce zajímají.

[**Rychlé** ](http://www.breezejs.com/?utm_source=ms-spa) je open source knihovna pro vytváření aplikací velké množství dat v jazyce JavaScript a HTML, typy aplikací, které v minulosti se dodávají jako samostatné aplikace klasické pracovní plochy.

Šablona Breeze/Knockout pomáhá tento první zásadní krok k více robustní infrastruktury pro správu dat trvat. Vytvoří ukázkovou aplikaci seznamu úkolů, která je vně stejný jako šablona KnockoutJS SPA. Uvnitř nahradí Datová vrstva AJAX Breeze, abyste mohli porovnat dva přístupy vedle sebe. Samozřejmě i neziskovky dotýká potenciál Breeze aplikace. Ale uvidíte, jak Breeze funguje a jak trochu potřeba ke zpřístupnění tohoto přechodu.

Pusťme se do práce.

## <a name="create-a-breezeknockout-template-project"></a>Vytvoření projektu šablona Breeze/Knockout

Stáhněte a nainstalujte šablony kliknutím na tlačítko Stáhnout výše. Šablona je zabalena jako soubor rozšíření aplikace Visual Studio (VSIX). Můžete potřebovat restartovat Visual Studio.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **webové**. V seznamu šablon projektu vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt a klikněte na tlačítko **OK**.

V **nový projekt** průvodce, vyberte **Breeze Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Stisknutím kláves Ctrl-F5 sestavte a spusťte aplikaci bez ladění nebo stisknutím klávesy F5 spusťte ladění.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Při prvním spuštění aplikace, zobrazí přihlašovací obrazovka. Klikněte na odkaz "Registrace" a novou stránku glides do zobrazení, ve kterém můžete zadat uživatelské jméno a heslo. (Přihlášení a registraci stránky jsou vytvořené pomocí ASP.NET MVC.) Když odešlete registrační formulář, server vygeneruje seznamu úkolů se dvěma položkami pro váš účet. Potom ho prezentuje je pro vás na žlutou poznámku.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Nyní jste v pozemního jednostránková aplikace. Všechno, co můžete prohlédnout a vyzkoušet při zpracování úloh, ať už je vykreslen a spravované v klientském počítači díky Knockout a rychlé. Prozkoumání aplikace jako uživatele... ale s okem vývojáře. Zachycení síťového provozu pomocí vývojářských nástrojů v prohlížeči. (V Internet Exploreru: stisknutím klávesy F12, vyberte **sítě** kartu a klikněte na tlačítko **spustit zachytávání**.) Teď zkuste následující:

- Přidáte novou položku seznamu úkolů.
- Po kliknutí na popisek a upravit nadpis položek Todo
- Zaškrtnutím políčka označíte položky Hotovo. Všimněte si, že textového pole, protože je zakázán název již nelze upravovat.
- Klikněte na tlačítko "x" napravo od popisku. Položka zmizí a je odstraněna z databáze.
- Vyberte jinou položku a vymažte její název. Zobrazí se chyba ověření, že název je povinný. Po krátké pozastavení se obnoví předchozí název.
- Zadejte absurdně dlouhý název. Zobrazí se chyba různých ověření, že název je příliš dlouhý.
- Klikněte na tlačítko "Přidat seznam úkolů". Nový seznam se zobrazí nalevo od předchozího seznamu.
- Pohrajte si s název seznamu úkolů, aktivuje se vyžaduje a délka ověření.
- Klikněte do textového pole Název vymazat chybovou zprávu.
- Klikněte na tlačítko "x" v kruhu v pravém horním rohu na Odstranit seznamu úkolů a úloh, ať už jeho.

Logiku ověřování se provádí straně klienta podle Breeze. Ověřování atributů na serverové třídy modelu jsou šířeny do klienta a spouštěny automaticky, než se klient připojí k serveru.

Zkontrolujte síťový provoz. Všimněte si, že nebyly žádné volání serveru při Breeze došlo k chybě. Každý platný změnu výsledkem požadavku POST na "/ api/Todo/SaveChanges". Rychlé obsahuje ureitou změny a odesílá je společně jako jeden požadavek do kontroleru webového rozhraní API `SaveChanges` metody. Který se liší od KockoutJS SPA šablonu, která umožňuje PUT, POST a odstranit zastaralé požadavky pro každou položku jednotlivě.

## <a name="peek-inside"></a>Operace Peek uvnitř

Tato aplikace má na straně klienta a na straně serveru. Klientské zásobník se skládá z trochu HTML a kombinaci modulech JavaScript aplikace (ve složce "aplikace") a navíc JavaScript knihovny třetích stran (ve složce "Skripty").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Pokud jste prozkoumat šablona KnockoutJS SPA, to velmi povědomé. Zaměřte se na modrých polích. Architektura uživatelského rozhraní je Model-View-ViewModel (MVVM), ve kterém jsou pomůcky HTML zobrazení čistě oddělená od podpůrné prezentaci kódu v modelu zobrazení. Systém vazby dat (v tomto případě Knockout) koordinuje zobrazení a modelu zobrazení tak, aby každý může fungovala bez vědomí každou z nich.

Model zapouzdřuje Todo data. Entity v modelu jsou vytvořeny pomocí Breeze pozorovatelných vlastnostmi Knockout, proto mohou být vázány přímo na pomůckách v zobrazení. Model zobrazení požádá kontext dat, které chcete získat a uložit model entity. Kontext dat deleguje většinu práce, kterou Breeze.

Stack na straně serveru se skládá z kódu pro vývojáře a knihoven .NET tři principu: webového rozhraní API, Entity Framework a Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Základní architektura je stejný jako KockoutJS SPA šablony. Implementace je však mnohem jednodušší: The DTO se odstranily a většina podrobnosti Entity Framework byla delegována do Breeze.NET.

## <a name="next-steps"></a>Další kroky

Doporučujeme zkoumání kódu, podle [rozsáhlé diskuse](http://www.breezejs.com/spa-template?utm_source=ms-spa) klientských a serverových sad na webu Breeze.

Můžete se pokusit přehrávání s podrobným dotaz na straně klienta; Přidejte několik filtrů a řazení. Můžete například přidat další vlastnosti modelu a další entity, které pro vývoj SPA začátku do konce získat lepší představu. Pokud jste si jisti návrhu, můžete odstraňovat funkce Todo a nahraďte je vlastními.

Jakmile budete připraveni na další velký krok: přidání nových obrazovek na straně klienta a navigace mezi nimi. Budete moct nechat tuto šablonu jednostránková aplikace za bránou a zapnout na další celý zásobník jednostránková aplikace, jako například [Hot Towel John Papa](https://github.com/johnpapa/HotTowel#readme "Hot Towel"), k podrobným a Knockout kombinaci přidává Durandal.
