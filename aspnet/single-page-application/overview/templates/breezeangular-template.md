---
uid: single-page-application/overview/templates/breezeangular-template
title: "Šablony uloženy/úhlová | Microsoft Docs"
author: madskristensen
description: "Šablona uloženy/úhlová jedné stránky aplikací"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="breezeangular-template"></a>Uloženy/úhlová šablony
====================
podle [Mads Kristensen](https://github.com/madskristensen)

> Šablony MVC uloženy/úhlová napsal Warda zvonku
> 
> [Stáhnout uloženy/úhlová šablonu MVC](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) je knihovna s otevřeným zdrojem z Google pro vytváření jednostránkové aplikace (SPA). Nabízí datové vazby, vkládání závislostí a správu obrazovky. Kombinovat se službou [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), jiné knihovny s otevřeným zdrojem pro modelování dat a správu dat a budete mít základní složek pro skvělé klientskou aplikaci pro HTML/JavaScript.

Šablona uloženy/úhlová SPA je variace na [kódem KnockoutJS SPA šablony](../introduction/knockoutjs-template.md) součástí technologie ASP.NET a webové nástroje 2012.2 aktualizace. Pokud máte k dispozici sady Visual Studio, budete mít příklad SPA spuštěná za méně než 60 sekund.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Vně vyhledá aplikace velmi podobná kódem KnockoutJS SPA šablony. Ale je pod pokličkou výrazně lišit. Šablona kódem KnockoutJS používá Knockout pro datové vazby a raw AJAX pro přístup k datům. Šablona uloženy/úhlová používá úhlová pro datové vazby a uloženy pro přístup k datům. Tyto knihovnami povolit další možnosti, včetně navigaci na stránce a historie.

Zde je stránka o aplikace:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Tato stránka zobrazuje spuštěné protokolu událostí během aktuální relace uživatele, včetně:

- Stránkování. Poznámka: vytváření řadiče úkolů na #2 a #7.
- Vzdálené dotazy (3) a místní mezipaměti (7).
- Uložení nové (č. 5, #6) a změnit entity (č. 4).
- Změny ověření na straně klienta (č. 9), takže uživatel může opravit chyby před potvrzením změny do databáze.

Existuje více prozkoumat v této šabloně, včetně:

- Dynamické načtení šablon zobrazení HTML.
- Vlastní datové vazby prostřednictvím úhlová "direktivy."
- Vkládání modularitu a závislostí.
- Filtry dotazu, řazení, stránkování, projekce a zahrnutí entit v relaci.
- Sdílení dat mezi několika obrazovkách.
- Ukládání více změn jako jediná transakce.
- Ověřovací pravidla rozšíří ze serveru do klienta JavaScript automaticky.

Můžeme začít.

## <a name="create-a-breezeangular-template-project"></a>Vytvoření uloženy/úhlová šablony projektu

Stáhněte a nainstalujte šablony kliknutím na výše uvedené tlačítko Stáhnout. Šablona je zabalené jako soubor rozšíření Visual Studio (VSIX). Možná budete muset restartovat Visual Studio.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**. Název projektu a klikněte na tlačítko **OK**.

V **nový projekt** průvodci vyberte **úhlová SPA uloženy**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Stisknutím Ctrl + F5 sestavení a spuštění aplikace bez ladění nebo stisknutím klávesy F5 spusťte s laděním.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Při prvním spuštění aplikace, zobrazí se přihlašovací obrazovka. Klikněte na odkaz "Registrace" a nová stránka glides do zobrazení, kde můžete zadat uživatelské jméno a heslo. (Přihlášení a registrace stránky jsou vytvořeny pomocí technologie ASP.NET MVC.) Při odesílání registračním formuláři server vygeneruje seznamu úkolů se dvěma položkami pro váš účet. Potom představuje je pro vás na žlutý Poznámka.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Nyní jste v krajině SPA. Všechno, co jste vidí a jak při manipulaci s Todos je vykreslen a spravovat na straně klienta pomocí Knockout a uloženy. Jako uživatel prozkoumejte aplikace... ale s oka pro vývojáře. Pomocí nástrojů pro vývojáře v prohlížeči pro zachycení síťového provozu. (V Internet Exploreru: stiskněte klávesu F12, vyberte **sítě** a klikněte na **spustit zachytávání**.) Teď vyzkoušejte tyto věci:

- Přidáte novou položku úkolů.
- Klepněte na popisek a upravit nadpis položky Todo
- Zaškrtněte políčko k označení položky Hotovo. Všimněte si, že textové pole je zakázaná, tak název již nelze upravovat.
- Klikněte na tlačítko 'x' vpravo popisku. Položka zmizí, se odstraní z databáze.
- Vyberte jinou položku a zrušit její název. Získáte chybu ověření, že název je povinný. Po krátkou pozastaví je obnoven předchozí název.
- Zadejte ridiculously dlouhý název. Získáte chyba různých ověření, že název je příliš dlouhý.
- Klikněte na tlačítko "Přidat seznam úkolů". Nového seznamu se zobrazí vlevo od předchozího seznamu.
- Přehrání se název seznamu úkolů, aktivuje vyžaduje a délka ověření.
- Klikněte do textového pole Název zrušte chybovou zprávu.
- Klikněte na symbol "x" na zobrazený v pravém horním rohu seznam úkolů a jeho todos odstraníte.
- Kliknutím na odkaz "O" v horním pravém zobrazíte protokolu tyto aktivity.

Logiku ověření se provádí straně klienta podle uloženy. Atributy ověřování na třídy modelu serveru jsou rozšíří do klienta a jsou prováděny automaticky předtím, než se klient připojí k serveru.

Zkontrolujte síťový provoz. Všimněte si, že neexistují žádná volání na server při uloženy došlo k chybě. Každé změně platný výsledkem požadavek POST na "/ api/Todo/SaveChanges". Uloženy obsahuje ureitou změny a odešle je společně jako jeden požadavek na kontroleru webového rozhraní API `SaveChanges` metoda. Která se liší od KockoutJS SPA šablony, která umožňuje PUT, POST a odstranit požadavky pro každou položku jednotlivě.

Všimněte si také, že neexistuje žádný síťový provoz při přepínání mezi seznamu úkolů a o stránky. Je to způsobeno dotazu byla omezena do místní mezipaměti uloženy.

## <a name="peek-inside"></a>Prohlížení uvnitř

Tato aplikace má straně klienta a straně serveru. Na straně klienta zásobníku se skládá z trochu HTML a kombinaci modulů JavaScript aplikace (ve složce "aplikace") plus knihovny JavaScript třetích stran (ve složce "Skripty").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

Architektura uživatelského rozhraní odděluje od podpůrné kód prezentace v řadičích pomůcky HTML zobrazení. Úhlová systému datové vazby koordinuje zobrazeních a řadičích, tak, aby každý můžete provést úlohy bez dokonalou znalosti o dalších.

Řadičem požádá kontextu dat získat a uložte modelu entity. Data kontextu deleguje většinu práce, kterou uloženy, který vytvoří vlastním sledováním objekty modelu z výsledků dotazu JSON.

Na straně serveru zásobníku se skládá z nějaký kód developer a tři knihovny .NET princip: webového rozhraní API, rozhraní Entity Framework a Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Základní architektura je stejný jako KockoutJS SPA šablonu. Implementace je však mnohem jednodušší: byly odstraněny DTOs a Breeze.NET delegované většinu podrobnosti Entity Framework.

## <a name="next-steps"></a>Další kroky

Doporučujeme prozkoumávání kód provést podle [rozsáhlé vysvětlení](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) klienta a serveru zásobníky na webu uloženy.

Můžete vyzkoušet přehrávání s dotazem na straně klienta uloženy; Přidejte některé filtrů a řazení. Můžete přidat další vlastnosti modelu a dalších entit získat lepší chování pro vývoj SPA začátku do konce. Pokud jste si jisti návrhu, můžete oddělení se funkce úkolů a nahraďte je vlastními.

Kódování radostí!
