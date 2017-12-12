---
uid: single-page-application/overview/templates/breezeknockout-template
title: "Šablony uloženy/Knockout | Microsoft Docs"
author: madskristensen
description: "Šablona uloženy/Knockout jedné stránky aplikací"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="breezeknockout-template"></a>Uloženy/Knockout šablony
====================
podle [Mads Kristensen](https://github.com/madskristensen)

> Šablony MVC uloženy/Knockout napsal Warda zvonku
> 
> [Stažení šablony MVC uloženy/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Jste slyšeli o "jednostránkové aplikace" (SPA) zajímalo, co je. Při čtení může o tom, které by místo prostředí pro sami. Ale který má čas Stáhnout ukázku? Pokud máte k dispozici sady Visual Studio, budete mít příklad SPA a spuštěnou v menší než 60 sekund s architekturou ASP.NET MVC 4 šablony "Uloženy/Knockout jedné stránky aplikace".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Co je šablona SPA uloženy/Knockout?

Většina šablony projektů generovat kostru aplikace. Vložit dužina na těchto kosti přidáním kódu a nakonec poskytovat funkční aplikaci. Šablony uloženy/Knockout SPA se liší. Vygeneruje ukázková aplikace pro vás zkoumat. Ukazuje SPA návrh aplikace a řadu techniky pro vytváření SPA.

Šablona uloženy/Knockout je variace na [kódem KnockoutJS SPA šablony](../introduction/knockoutjs-template.md) součástí technologie ASP.NET a webové nástroje 2012.2 aktualizace. Šablona uloženy SPA generuje aplikace pomocí stejné prostředí pro uživatele, ale má jinou implementaci, pomocí uloženy dat pro správu.

Šablony kódem KnockoutJS SPA vytváří žádosti o službu s nezpracovaná jQuery AJAX, který je dostačující pro jednoduchou aplikaci. Ale sofistikovanější aplikací mít náročnějšími požadavky na správu data. Například většina aplikací:

- Dotaz a znovu zadat dotaz na server během relace rozšířených uživatele.
- Přidání dotazu filtrů, řazení a stránkování.
- Stejná data sdílet mezi více obrazovky.
- Accumulate změny mnoho objektů a potom je uložit jako jediná transakce.
- Ověřte změny na straně klienta, takže uživatel může opravit chyby před potvrzením změny do databáze.

Knihovna BreezeJS zpracovává tyto chores pro vás, uvolnění můžete vyvíjet aplikace logiky a uživatelské prostředí, které vás zajímají.

[**Uloženy** ](http://www.breezejs.com/?utm_source=ms-spa) otevřeným zdrojem knihovnu pro vytváření bohaté data aplikací v jazyce JavaScript a HTML, typy aplikací, které byly obsaženy v minulosti jako samostatné aplikací klasické pracovní plochy.

Šablony uloženy/Knockout pomáhá vám tento první zásadní krok k robustnější infrastrukturou pro správu dat. Vyvolá ukázkové aplikace Todo, která je vně totožný s kódem KnockoutJS SPA šablony. Uvnitř nahradí Datová vrstva AJAX uloženy, takže je můžete porovnat dva blíží vedle sebe. Samozřejmě sotva dotykem potenciál aplikace uloženy. Ale uvidíte, jak uloženy funguje a jak trochu je nutná ke správnému tento přechod.

Můžeme začít.

## <a name="create-a-breezeknockout-template-project"></a>Vytvoření projektu šablony uloženy/Knockout

Stáhněte a nainstalujte šablony kliknutím na výše uvedené tlačítko Stáhnout. Šablona je zabalené jako soubor rozšíření Visual Studio (VSIX). Možná budete muset restartovat Visual Studio.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**. Název projektu a klikněte na tlačítko **OK**.

V **nový projekt** průvodci vyberte **uloženy Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Stisknutím Ctrl + F5 sestavení a spuštění aplikace bez ladění nebo stisknutím klávesy F5 spusťte s laděním.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

Logiku ověření se provádí straně klienta podle uloženy. Atributy ověřování na třídy modelu serveru jsou rozšíří do klienta a jsou prováděny automaticky předtím, než se klient připojí k serveru.

Zkontrolujte síťový provoz. Všimněte si, že neexistují žádná volání na server při uloženy došlo k chybě. Každé změně platný výsledkem požadavek POST na "/ api/Todo/SaveChanges". Uloženy obsahuje ureitou změny a odešle je společně jako jeden požadavek na kontroleru webového rozhraní API `SaveChanges` metoda. Která se liší od KockoutJS SPA šablony, která umožňuje PUT, POST a odstranit požadavky pro každou položku jednotlivě.

## <a name="peek-inside"></a>Prohlížení uvnitř

Tato aplikace má straně klienta a straně serveru. Na straně klienta zásobníku se skládá z trochu HTML a kombinaci modulů JavaScript aplikace (ve složce "aplikace") plus knihovny JavaScript třetích stran (ve složce "Skripty").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Pokud jste prozkoumat šabloně kódem KnockoutJS SPA, to velmi povědomé. Soustřeďte se na blue polí. Architektura uživatelského rozhraní je Model-View-ViewModel (modelem MVVM), ve které jsou pomůcky HTML zobrazení řádně odděleny z podpůrné kódu prezentace v zobrazení modelu. Systém vazby dat (v tomto případě Knockout) koordinuje zobrazení a modelu zobrazení tak, aby každý můžete provést úlohy bez dokonalou znalosti o dalších.

Model zapouzdří data úkolů. Entity v modelu se vytvářejí pomocí uloženy pozorovatelné vlastnostmi Knockout, proto mohou být vázány přímo na pomůckách v zobrazení. Model zobrazení zobrazí data kontext k získání a uložte modelu entity. Data kontextu deleguje většinu práce, kterou uloženy.

Na straně serveru zásobníku se skládá z nějaký kód developer a tři knihovny .NET princip: webového rozhraní API, rozhraní Entity Framework a Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

Základní architektura je stejný jako KockoutJS SPA šablonu. Implementace je však mnohem jednodušší: byly odstraněny DTOs a Breeze.NET delegované většinu podrobnosti Entity Framework.

## <a name="next-steps"></a>Další kroky

Doporučujeme prozkoumávání kód provést podle [rozsáhlé vysvětlení](http://www.breezejs.com/spa-template?utm_source=ms-spa) klienta a serveru zásobníky na webu uloženy.

Můžete vyzkoušet přehrávání s dotazem na straně klienta uloženy; Přidejte některé filtrů a řazení. Můžete přidat další vlastnosti modelu a dalších entit získat lepší chování pro vývoj SPA začátku do konce. Pokud jste si jisti návrhu, můžete oddělení se funkce úkolů a nahraďte je vlastními.

Brzy budete připraveno pro další krok velký: přidání obrazovky na straně klienta a navigace mezi nimi. Budete nechte Tato šablona SPA a zapněte k více celý zásobník SPA, jako například [aktivní ručníků Jan Papa](https://github.com/johnpapa/HotTowel#readme "aktivní ručníků"), k kombinace uloženy a Knockout přidává Durandal.
