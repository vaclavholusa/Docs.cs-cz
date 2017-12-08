---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: "Vylepšení výkonu pomocí výstupní mezipaměti (VB) | Microsoft Docs"
author: microsoft
description: "V tomto kurzu zjistíte, jak může výrazně zvýšit výkon webových aplikací ASP.NET MVC a využívají k ukládání výstupu do mezipaměti. Můžete..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: 3bd4b6c3ac52577cbee451d2986f1167e441f0e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="improving-performance-with-output-caching-vb"></a>Vylepšení výkonu pomocí ukládání výstupu do mezipaměti (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> V tomto kurzu zjistíte, jak může výrazně zvýšit výkon webových aplikací ASP.NET MVC a využívají k ukládání výstupu do mezipaměti. Zjistíte, jak pro ukládání do mezipaměti výsledek vrácený z akce kontroleru, takže není nutné vytvořit každém nového uživatele vyvolá akci stejný obsah.


Cílem tohoto kurzu je vysvětlují, jak může výrazně zlepšit výkon aplikace ASP.NET MVC využitím výstupní mezipaměti. Výstupní mezipaměti umožňuje ukládat do mezipaměti obsah vrácený akce kontroleru. Tímto způsobem stejný obsah se nemusí generovat každé, když je volána stejné akce kontroleru.

Představte si například, že vaše aplikace ASP.NET MVC zobrazí seznam záznamů databáze v zobrazení s názvem Index. Za normálních okolností každé dobu, po kterou uživatel vyvolá akce kontroleru, který vrátí Index zobrazení, sadu záznamů databáze musí být načtena z databáze spuštěním dotazu na databázi.

Pokud na druhé straně využít výhod výstupní mezipaměti můžete vyhnout, provádění dotazu na databázi pokaždé, když každý uživatel, vyvolá stejné akce kontroleru. Zobrazení mohou být načteny z mezipaměti namísto znovu generovány z akce kontroleru. Ukládání do mezipaměti umožňuje můžete vyhnout provádění redundantní fungovat na serveru.

#### <a name="enabling-output-caching"></a>Povolení ukládání výstupu do mezipaměti

Povolit ukládání výstupu do mezipaměti přidáním &lt;OutputCache&gt; atribut akce jednotlivých řadiče nebo třídu celý kontroler. Například řadič v výpis 1 zpřístupní akci s názvem Index(). Výstup Index() akce se uloží do mezipaměti 10 sekund.

**Výpis 1 – Controllers\HomeController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


V Beta verzím rozhraní ASP.NET MVC, ukládání výstupu do mezipaměti nefunguje pro adresu URL podobnou [http://www.MySite.com/](http://www.mysite.com/). Místo toho musíte zadat adresu URL jako [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).


Výpis 1 výstup Index() akce je do mezipaměti 10 sekund. Pokud dáváte přednost, můžete zadat mnohem delší dobu mezipaměti. Například pokud chcete pro ukládání do mezipaměti výstup akce kontroleru pro jeden den je můžete zadat dobu trvání mezipaměti 86400 sekund (60 sekund \* 60 minut \* 24 hodin).

Neexistuje žádná záruka, že obsahu bude do mezipaměti pro množství času, který určíte. Při paměťových prostředků začnou nízkou, čištění obsah v mezipaměti spustí automaticky.

Domovské řadiče v výpis 1 vrátí zobrazení indexu v výpis 2. Není co speciální o tomto zobrazení. Zobrazení indexu jednoduše zobrazí aktuální čas (viz obrázek 1).

**Výpis 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Obrázek 1 – zobrazení indexu ukládaná do mezipaměti**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Pokud vícekrát vyvolání akce Index() zadáním adresy URL/Home nebo indexu na panelu Adresa svého prohlížeče a opakovaně klepnutím na tlačítko Aktualizovat nebo načtěte v prohlížeči, pak času zobrazeného v indexu zobrazení nedojde ke změně 10 sekund. Současně je zobrazit, protože zobrazení je uložený v mezipaměti.

Je důležité si uvědomit, že stejné zobrazení, se uloží do mezipaměti pro všechny uživatele, kteří navštíví vaší aplikace. Každý, kdo vyvolá akci Index() získají uložené v mezipaměti stejnou verzi zobrazení indexu. To znamená, že je výrazně snížit množství práce, který webový server musíte provést k obsluze zobrazení indexu.

Zobrazení v výpis 2 se stane něco skutečně jednoduché to. Zobrazení pouze zobrazuje aktuální čas. Však může stejně jako se snadno mezipaměti zobrazení, které se zobrazí sada záznamů databáze. Sada záznamů databáze v takovém případě nemusí být načtena z databáze každé době vyvolání akce kontroleru, který vrací zobrazení. Ukládání do mezipaměti může snížit množství práce, kterou musí provést webový server a databázový server.

Nepoužívejte stránce &lt;% @ OutputCache %&gt; direktivy v zobrazení MVC. Tato direktiva je vykrvení přes ze světa webových formulářů a nesmí být použita v aplikaci ASP.NET MVC. 

#### <a name="where-content-is-cached"></a>Kde je obsah uložený v mezipaměti

Ve výchozím nastavení se při použití &lt;OutputCache&gt; atribut, obsah se uloží do mezipaměti ve třech umístěních: webový server, všechny proxy servery a webový prohlížeč. Můžete řídit přesně, kde je obsah mezipaměti změnou vlastnost umístění &lt;OutputCache&gt; atribut.

Můžete nastavit vlastnosti umístění na některý z následujících hodnot:

> · Všechny
> 
> · Klienta
> 
> · Příjem dat
> 
> · Server
> 
> · None
> 
> · ServerAndClient


Ve výchozím umístění vlastnost má hodnotu žádné. Existují však situace, ve kterých je vhodné do mezipaměti jenom v prohlížeči nebo pouze na serveru. Například pokud jsou ukládání do mezipaměti informace, které je přizpůsobené pro každého uživatele, pak mezipaměti byste neměli ukládat informace na serveru. Pokud jsou zobrazení různé informace pro různé uživatele by měla mezipaměti informace pouze na straně klienta.

Například řadič v výpis 3 zpřístupní akci s názvem GetName(), který vrací aktuální uživatelské jméno. Pokud konektor protokolů k webu a vyvolá akci GetName() akce vrátí řetězec "Hi konektoru". Pokud, následně Jill protokolů k webu a vyvolá akci GetName() pak uživatel taky dostane řetězec "Hi konektoru". Řetězec se po zkomprimování uloží na webovém serveru pro všechny uživatele konektoru původně vyvolá akce kontroleru.

**Výpis 3 – Controllers\BadUserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

S největší pravděpodobností řadiče v výpis 3 nefunguje požadovaným způsobem. Zobrazí se zpráva "Konektor Hi" k Jill nechcete.

By nikdy mezipaměti přizpůsobený obsah v mezipaměti na serveru. Můžete ale chtít mezipaměti přizpůsobený obsah do mezipaměti prohlížeče ke zlepšení výkonu. Pokud mezipaměti obsahu v prohlížeči, a uživatel vyvolá stejné akce kontroleru vícekrát, můžete obsah načten z mezipaměti prohlížeče. namísto serveru.

Upravené řadiče v výpis 4 ukládá do mezipaměti výstup GetName() akce. Ale obsah se uloží do mezipaměti jen v prohlížeči a není na serveru. Tímto způsobem, pokud více uživatelů vyvolání metody GetName(), každý uživatel získá své vlastní uživatelské jméno a uživatelské jméno není jiného uživatele.

**Výpis 4 – Controllers\UserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Všimněte si, že &lt;OutputCache&gt; atribut v výpis 4 obsahuje vlastnost umístění nastaven na hodnotu OutputCacheLocation.Client. &lt;OutputCache&gt; atribut také obsahuje vlastnost NoStore. Vlastnost NoStore slouží k informování proxy servery a prohlížeče, by neměl uložených trvalá kopie obsahu v mezipaměti.

#### <a name="varying-the-output-cache"></a>Různých výstupní mezipaměti

V některých případech můžete chtít různé verze velmi stejný obsah v mezipaměti. Představte si například, že vytváříte stránky a podrobností. Stránky předlohy zobrazí seznam názvů film. Když kliknete na název, zobrazí podrobnosti pro vybraného videa.

Můžete ukládat do mezipaměti stránce s podrobnostmi o, bude se zobrazí podrobnosti pro stejné film bez ohledu na to, které film kliknutí. Zobrazí se první film první uživatel vybírat pro všechny budoucí uživatele.

Tento problém lze vyřešit využívat výhod vlastnost VaryByParam &lt;OutputCache&gt; atribut. Tato vlastnost umožňuje vytvářet různé verze uložené v mezipaměti velmi stejný obsah, pokud parametr formuláře nebo parametr řetězce dotazu se liší.

Například řadič v výpis 5 zpřístupní dvě akce s názvem Master() a Details(). Akce Master() vrátí seznam názvů film a Details() akce vrátí podrobnosti vybraného videa.

**Výpis 5 – Controllers\MoviesController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

Akce Master() zahrnuje vlastnost VaryByParam s hodnotou "žádný". Vrátí se při zobrazení Master() akce je volána, stejnou verzi v mezipaměti hlavního serveru. Parametrů formuláře nebo řetězce dotazu, které parametry jsou ignorovány (viz obrázek 2).

**Obrázek 2 – /Movies/Master zobrazení**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Obrázek 3: zobrazení nebo filmy/podrobnosti**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

Akce Details() zahrnuje vlastnost VaryByParam s hodnotou "Id". Když různé hodnoty parametru Id jsou předána akce kontroleru, jsou generovány v mezipaměti různé verze zobrazení podrobností.

Je důležité pochopit, že pomocí výsledky vlastnost VaryByParam v další ukládání do mezipaměti a není menší. V mezipaměti jinou verzi zobrazení podrobností se vytvoří pro každou odlišnou verzi parametr Id.

Vlastnost VaryByParam můžete nastavit následující hodnoty:

> \*= Vytvořte jinou verzi v mezipaměti vždy, když se liší parametr řetězce formátu nebo dotazu.
> 
> žádné = nikdy vytvořit různé verze uložené v mezipaměti
> 
> Středník seznam parametrů = vytvořit různé verze uložené v mezipaměti vždy, když některý z parametrů formuláře nebo dotaz, řetězec v seznamu je to různé.


#### <a name="creating-a-cache-profile"></a>Vytvoření profilu mezipaměti

Jako alternativu ke konfiguraci vlastnosti výstupní mezipaměti a úprava vlastností &lt;OutputCache&gt; atribut, můžete vytvořit profil mezipaměti v souboru webové konfigurace (web.config). Vytvoření profilu mezipaměti v konfiguračním souboru webu nabízí několik výhod důležité.

Nejprve nakonfigurováním ukládání výstupu do mezipaměti v konfiguračním souboru webu můžete řídit, jak akce kontroleru ukládat obsah do mezipaměti v jednom centrálním místě. Můžete vytvořit jeden profil mezipaměti a použít profil pro několik řadiče nebo akce kontroleru.

Druhý můžete upravit konfigurační soubor webu bez nutnosti rekompilace vaší aplikace. Pokud je nutné zakázat ukládání do mezipaměti pro aplikaci, která již byla nasazena do produkčního prostředí, můžete jednoduše upravit profily mezipaměti definována v konfiguračním souboru webu. Všechny změny konfigurační soubor webu bude automaticky zjistil a použita.

Například &lt;ukládání do mezipaměti&gt; oddílu konfigurace webu na výpis 6 definuje profil mezipaměti s názvem Cache1Hour. &lt;Ukládání do mezipaměti&gt; část musí být v rámci &lt;system.web&gt; části souboru webové konfigurace.

**Výpis 6 – část ukládání do mezipaměti pro soubor web.config**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

Řadič v výpis 7 znázorňuje, jak můžete použít profil Cache1Hour pro akce kontroleru s &lt;OutputCache&gt; atribut.

**Výpis 7 – Controllers\ProfileController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

Pokud vyvolání akce Index() vystavené řadiče v výpis 7 pak ve stejnou dobu, bude vrácen jednu hodinu.

#### <a name="summary"></a>Souhrn

Ukládání výstupu do mezipaměti umožňuje velmi snadno metoda výrazně zlepšit výkon aplikací ASP.NET MVC. V tomto kurzu jste zjistili, jak používat &lt;OutputCache&gt; atribut pro ukládání do mezipaměti výstup akce kontroleru. Také jste zjistili, jak upravit vlastnosti &lt;OutputCache&gt; atribut třeba doba trvání a VaryByParam vlastnosti, které chcete upravit, jak získá obsah uložený v mezipaměti. Nakonec jste zjistili, jak definovat profily mezipaměti v konfiguračním souboru webu.

>[!div class="step-by-step"]
[Předchozí](understanding-action-filters-vb.md)
[další](adding-dynamic-content-to-a-cached-page-vb.md)
