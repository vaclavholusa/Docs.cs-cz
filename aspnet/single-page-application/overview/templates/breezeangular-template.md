---
uid: single-page-application/overview/templates/breezeangular-template
title: Šablona breeze/Angular | Dokumentace Microsoftu
author: madskristensen
description: Šablona breeze/Angular jednostránkové aplikace
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: a3e8b42cdadf99df6971a278834b1429e129ce72
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756016"
---
<a name="breezeangular-template"></a>Šablona breeze/Angular
====================
podle [Autor: Mads Kristensen](https://github.com/madskristensen)

> Šablona Breeze/Angular MVC zapsal pře zvonku
> 
> [Stáhněte si šablonu MVC Breeze/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) je open source knihovna z Googlu pro vytváření jednostránkové aplikace (SPA). Nabízí správu obrazovky, vkládání závislostí a datové vazby. Kombinaci se službou [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), jiné opensourcové knihovně pro modelování dat a správu dat a mít základní složky skvělé aplikace pro klienta HTML/JavaScript.

Šablona Breeze/Angular SPA je varianta na [šablona KnockoutJS SPA](../introduction/knockoutjs-template.md) součástí technologie ASP.NET a Web Tools 2012.2 Update. Pokud máte Visual Studio, budete mít příklad SPA zprovoznit za míň než 60 sekund.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Vně aplikace velmi podobná šablona KnockoutJS jednostránková aplikace. Ale je značně odlišná pod pokličkou. Šablona KnockoutJS používá Knockout pro nezpracované AJAX pro přístup k datům a datové vazby. Šablona Breeze/Angular Angular používá pro rychlé a datové vazby pro přístup k datům. Tyto knihovnami povolit další možnosti, včetně navigace stránky a historii.

Tady je stránka o vaší aplikace:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Tato stránka zobrazuje spuštěné protokol událostí během aktuální relace uživatele, včetně:

- Stránkování. Poznámka: vytvoření kontroleru Todo v #2 a #7.
- Vzdálené dotazy (č. 3) a místní mezipaměti (#7).
- Ukládání nového (č. 5, #6) a upravovat entity (č. 4).
- Změny ověřit na straně klienta (9), takže uživatel může opravit chyby před potvrzením změn do databáze.

Existuje více chcete data prozkoumat v šabloně, včetně:

- Dynamické načtení šablon zobrazení HTML.
- Vlastní datové vazby pomocí Angular "direktivy."
- Vkládání modularitu a závislostí.
- Dotaz filtry, řazení, stránkování, projekce a zařazení souvisejících entit.
- Sdílení dat napříč více obrazovek.
- Uložení více změn jako jedna transakce.
- Ověřovací pravidla rozšíří ze serveru klientovi JavaScript automaticky.

Pusťme se do práce.

## <a name="create-a-breezeangular-template-project"></a>Vytvořte projekt šablona Breeze/Angular

Stáhněte a nainstalujte šablony kliknutím na tlačítko Stáhnout výše. Šablona je zabalena jako soubor rozšíření aplikace Visual Studio (VSIX). Můžete potřebovat restartovat Visual Studio.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **webové**. V seznamu šablon projektu vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt a klikněte na tlačítko **OK**.

V **nový projekt** průvodce, vyberte **Breeze Angular SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Stisknutím kláves Ctrl-F5 sestavte a spusťte aplikaci bez ladění nebo stisknutím klávesy F5 spusťte ladění.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Klikněte na odkaz "O" v pravém horním rohu zobrazíte protokolu těchto činností.

Logiku ověřování se provádí straně klienta podle Breeze. Ověřování atributů na serverové třídy modelu jsou šířeny do klienta a spouštěny automaticky, než se klient připojí k serveru.

Zkontrolujte síťový provoz. Všimněte si, že nebyly žádné volání serveru při Breeze došlo k chybě. Každý platný změnu výsledkem požadavku POST na "/ api/Todo/SaveChanges". Rychlé obsahuje ureitou změny a odesílá je společně jako jeden požadavek do kontroleru webového rozhraní API `SaveChanges` metody. Který se liší od KockoutJS SPA šablonu, která umožňuje PUT, POST a odstranit zastaralé požadavky pro každou položku jednotlivě.

Všimněte si také, že není žádné síťové přenosy při přepínání mezi seznamu úkolů a o stránkách. Důvodem je, dotaz se omezila na místní mezipaměti uloženy.

## <a name="peek-inside"></a>Operace Peek uvnitř

Tato aplikace má na straně klienta a na straně serveru. Klientské zásobník se skládá z trochu HTML a kombinaci modulech JavaScript aplikace (ve složce "aplikace") a navíc JavaScript knihovny třetích stran (ve složce "Skripty").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Architektura uživatelského rozhraní odděluje pomůcky HTML zobrazení podpůrné prezentaci kódu v řadičích. Angular systému datové vazby koordinuje zobrazení a kontrolery, tak, aby každý může fungovala bez vědomí každou z nich.

Kontroler se vás zeptá kontext dat, které chcete získat a uložit model entity. Kontext dat deleguje většinu práce, kterou Breeze, který vytvoří vlastní sledování objektů modelu z výsledků dotazu JSON.

Stack na straně serveru se skládá z kódu pro vývojáře a knihoven .NET tři principu: webového rozhraní API, Entity Framework a Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Základní architektura je stejný jako KockoutJS SPA šablony. Implementace je však mnohem jednodušší: The DTO se odstranily a většina podrobnosti Entity Framework byla delegována do Breeze.NET.

## <a name="next-steps"></a>Další kroky

Doporučujeme zkoumání kódu, podle [rozsáhlé diskuse](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) klientských a serverových sad na webu Breeze.

Můžete se pokusit přehrávání s podrobným dotaz na straně klienta; Přidejte několik filtrů a řazení. Můžete například přidat další vlastnosti modelu a další entity, které pro vývoj SPA začátku do konce získat lepší představu. Pokud jste si jisti návrhu, můžete odstraňovat funkce Todo a nahraďte je vlastními.

Šťastné kódování!
