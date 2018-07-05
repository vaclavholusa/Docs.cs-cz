---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Zlepšení výkonu ukládáním výstupní mezipaměti (VB) | Dokumentace Microsoftu
author: microsoft
description: V tomto kurzu se dozvíte, jak můžete výrazně vylepšit výkon webových aplikací ASP.NET MVC s využitím ukládání výstupu do mezipaměti. Budete...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: debc456531ee771a9ecb24b81e35d3084d8e49b8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398356"
---
<a name="improving-performance-with-output-caching-vb"></a>Zlepšení výkonu ukládáním výstupů do mezipaměti (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> V tomto kurzu se dozvíte, jak můžete výrazně vylepšit výkon webových aplikací ASP.NET MVC s využitím ukládání výstupu do mezipaměti. Se dozvíte, jak pro ukládání do mezipaměti výsledek vrácený z akce kontroleru, tak, aby se stejný obsah není nutné vytvořit nový uživatel vyvolá akci každého čas.


Cílem tohoto kurzu je vysvětlují, jak může výrazně zlepšit výkon aplikace ASP.NET MVC s využitím do výstupní mezipaměti. Do výstupní mezipaměti můžete ukládat do mezipaměti obsah vrácený akce kontroleru. Tímto způsobem stejný obsah není potřeba generovat každé, když se vyvolá stejné akce kontroleru.

Představte si například, že vaše aplikace ASP.NET MVC zobrazí seznam záznamů databáze v zobrazení s názvem Index. Za normálních okolností každý čas, který uživatel vyvolá akce kontroleru, který vrací Index zobrazení sadu záznamů databáze musí načíst z databáze spuštěním databázového dotazu.

Pokud na druhé straně využít výhod výstupní mezipaměti můžete vyhnout, provádí dotaz na databázi vždy, když kterýkoli uživatel vyvolá stejné akce kontroleru. Zobrazení můžete získat z mezipaměti namísto tabulky znovu se generuje z akce kontroleru. Ukládání do mezipaměti umožňuje můžete vyhnout provedením redundantní fungovat na serveru.

#### <a name="enabling-output-caching"></a>Povolení ukládání výstupu do mezipaměti

Povolit ukládání výstupu do mezipaměti tak, že přidáte &lt;OutputCache&gt; atribut akci individuální řadiče nebo třídu celý kontroler. Například kontroler v informacích 1 zpřístupňuje akci s názvem Index(). Výstup akce Index() je uložené v mezipaměti 10 sekund.

**Výpis 1 – Controllers\HomeController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]


Ve verzi Beta verzím rozhraní ASP.NET MVC, ukládání výstupu do mezipaměti nefunguje pro adresu URL podobnou [ http://www.MySite.com/ ](http://www.mysite.com/). Místo toho musíte zadat adresu URL jako [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index).


V 1 výpis výstupu akce Index() je do mezipaměti 10 sekund. Pokud dáváte přednost, můžete zadat mnohem delší dobu mezipaměti. Například pokud chcete mezipaměť výstupu akce kontroleru za jeden den potom můžete zadat dobu trvání mezipaměti 86 400 sekund (60 sekund \* 60 minut \* 24 hodin).

Neexistuje žádná záruka tento obsah bude množství času, který zadáte do mezipaměti. Až budou prostředky paměti s nízkou, čištění obsah v mezipaměti spustí automaticky.

Kontroler Home v informacích 1 vrátí Index zobrazení výpisu 2. Není nic zvláštního o toto zobrazení. Index zobrazení jednoduše zobrazí aktuální čas (viz obrázek 1).

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Obrázek 1 – zobrazení indexu v mezipaměti**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Pokud vyvoláte více než jednou pro akci Index() zadáním adresy URL/Home/Index do adresního řádku prohlížeče a klepnutím na tlačítko Aktualizovat a znovu načíst v prohlížeči opakovaně, pak čas zobrazí v zobrazení Index se nezmění po dobu 10 sekund. Současně je zobrazit, protože zobrazení se uloží do mezipaměti.

Je důležité pochopit, že stejné zobrazení je do mezipaměti pro všechny uživatele, kteří navštíví vaší aplikace. Každý, kdo vyvolá akci Index() dostane stejnou verzi v mezipaměti zobrazení indexu. To znamená, že množství práce, které webový server musíte provést při zobrazení indexu bude sloužit k podstatnému omezení objemu.

Zobrazení výpisu 2 dochází k opravdu jednoduchý něco dělat. Zobrazení pouze zobrazuje aktuální čas. Však může stejně snadno mezipaměti zobrazení, které se zobrazí sada záznamů databáze. V takovém případě sadu záznamů databáze nemusí být načtena z databáze každého čas vyvolání akce kontroleru, který vrátí zobrazení. Ukládání do mezipaměti může snížit množství práce, které musíte provést webový server a databázový server.

Nepoužívejte stránce &lt;% @ OutputCache %&gt; direktiv v zobrazení MVC. Tato direktiva je vykrvení přes z webových formulářů světa by se neměl používat v aplikaci ASP.NET MVC. 

#### <a name="where-content-is-cached"></a>Pokud se obsah ukládá do mezipaměti

Ve výchozím nastavení při použití &lt;OutputCache&gt; atribut, obsah se uloží do mezipaměti na třech místech: webový server, všechny servery proxy a webový prohlížeč. Můžete řídit přesně kde obsah se uchová v mezipaměti tak, že upravíte vlastnosti umístění &lt;OutputCache&gt; atribut.

Nastavit vlastnost umístění na některý z následujících hodnot:

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


Ve výchozím nastavení vlastnost umístění má hodnotu Any. Existují však situace, ve kterých můžete ukládat do mezipaměti pouze v prohlížeči nebo pouze na serveru. Například pokud jsou ukládání do mezipaměti informace, které je přizpůsobené pro každého uživatele pak můžete by neměl mít informace v mezipaměti na serveru. Pokud jsou zobrazení různých informací o různých uživatelů by měla mezipaměti informace pouze na straně klienta.

Například kontroler v informacích 3 zpřístupňuje akci s názvem GetName(), který vrací aktuální uživatelské jméno. Pokud se konektoru se přihlásí k webu a vyvolá akci GetName() akce vrátí řetězec "Hi Jack". Pokud následně Jill se přihlásí k webu a vyvolá akci GetName() pak Jana také zobrazí řetězec "Hi Jack". Řetězec je do mezipaměti na webovém serveru pro všechny uživatele, po Jack zpočátku vyvolá akce kontroleru.

**Výpis 3 – Controllers\BadUserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

Řadič v informacích 3 největší pravděpodobností nebude fungovat tak, jak chcete, aby. Nechcete zobrazit zpráva "Konektor Hi" Jill.

Nikdy byste mezipaměti přizpůsobeného obsahu v mezipaměti. Můžete však chtít individuální obsah v mezipaměti prohlížeče. Chcete-li zvýšit výkon mezipaměti. Pokud mezipaměti obsahu v prohlížeči, a uživatel víckrát vyvolává stejné akce kontroleru, obsahu je možné načíst z mezipaměti prohlížeče. namísto serveru.

Upravené kontroleru v zobrazení 4 výstup akce GetName() ukládá do mezipaměti. Ale obsah je ukládat do mezipaměti pouze v prohlížeči a ne na serveru. Tímto způsobem, při více uživatelů vyvolat metodu GetName(), každý uživatel, který získá vlastní uživatelské jméno a uživatelské jméno není jinou osobu.

**Část 4 – Controllers\UserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Všimněte si, &lt;OutputCache&gt; atribut v informacích 4 obsahuje vlastnost umístění nastaveno na hodnotu OutputCacheLocation.Client. &lt;OutputCache&gt; atribut také zahrnuje NoStore vlastnost. Vlastnost NoStore slouží k informování proxy servery a prohlížeče, že jsou v nich by neměl uložené trvalé kopie obsahu v mezipaměti.

#### <a name="varying-the-output-cache"></a>Různé výstupní mezipaměti

V některých situacích může být vhodné různé verze uložené v mezipaměti velmi stejný obsah. Představte si například, že jsou vytvoření stránky hlavního záznamu/podrobností. Na hlavní stránce zobrazí seznam názvů video. Po kliknutí na název, získat podrobnosti pro vybranou videa.

Pokud jste do mezipaměti na stránce podrobností, pak podrobnosti pro stejný film se zobrazí bez ohledu na to, které video kliknete. Zobrazí se první video vybrali první uživatel pro všechny budoucí uživatele.

Tento problém můžete vyřešit s využitím vlastnost VaryByParam &lt;OutputCache&gt; atribut. Tato vlastnost umožňuje vytvářet různé verze uložené v mezipaměti velmi stejný obsah, když formulář parametr nebo parametr řetězce dotazu se liší.

Například kontroler v informacích 5 poskytuje dvě akce s názvem Master() a Details(). Akce Master() vrátí seznam názvů filmů a akce Details() vrátí podrobnosti o vybrané videa.

**Výpis 5 – Controllers\MoviesController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

Akce Master() zahrnuje VaryByParam vlastnost s hodnotou "none". Vrátí se při vyvolání akce Master() stejné verze uložené v mezipaměti hlavního zobrazení. Parametry formuláře nebo řetězce dotazu, parametry jsou ignorovány (viz obrázek 2).

**Obrázek 2 – /Movies/Master zobrazení**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Obrázek 3: zobrazení podrobností/filmy /**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

Akce Details() zahrnuje VaryByParam vlastnost s hodnotou "Id". Když různé hodnoty Id parametru jsou předány na akce kontroleru, jsou generovány různé verze uložené v mezipaměti zobrazení podrobností.

Je důležité si uvědomit, že pomocí vlastnosti výsledky VaryByParam v další ukládání do mezipaměti a nikoli menší. Různé verze uložené v mezipaměti ze zobrazení podrobností se vytvoří pro každou odlišnou verzi parametr Id.

Nastavit vlastnost VaryByParam na následující hodnoty:

> \* = Když se vytvoří různé verze uložené v mezipaměti řetězcový parametr formuláře nebo dotazů se liší.
> 
> Žádný = nikdy vytvořit různé verze uložené v mezipaměti
> 
> Seznam středníky, parametry = vytvořit různé verze uložené v mezipaměti, vždy, když některý z parametrů řetězce formuláře nebo dotazu v seznamu se liší


#### <a name="creating-a-cache-profile"></a>Vytvoření profilu mezipaměti

Jako alternativu ke konfiguraci výstupní mezipaměti vlastnosti tak, že upravíte vlastnosti &lt;OutputCache&gt; atribut, můžete vytvořit profil mezipaměti v souboru webové konfigurace (web.config). Vytvoření profilu mezipaměti v konfiguračním souboru webu nabízí několik výhod důležité.

Nejprve nakonfigurováním ukládání výstupu do mezipaměti v konfiguračním souboru webu můžete řídit, jak akce kontroleru mezipaměti obsah v jednom centrálním místě. Můžete vytvořit jeden profil mezipaměti a použít profil pro několik řadičů nebo akce kontroleru.

Za druhé můžete upravit konfigurační soubor webového bez opětovné kompilace aplikace. Pokud je potřeba zakázat ukládání do mezipaměti pro aplikace, která již byla nasazena do produkčního prostředí, můžete jednoduše upravit mezipaměti profily definovanými v souboru konfigurace webu. Všechny změny do souboru webové konfigurace bude automaticky zjistil a použita.

Například &lt;ukládání do mezipaměti&gt; profil mezipaměti s názvem Cache1Hour definuje oddíl konfigurace webu v informacích 6. &lt;Ukládání do mezipaměti&gt; oddílu musí být uvedena v rámci &lt;system.web&gt; část souboru webové konfigurace.

**Výpis 6 – ukládání do mezipaměti části souboru web.config**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

Kontroler v informacích 7 znázorňuje, jak je možné použít profil Cache1Hour k akci kontroleru se &lt;OutputCache&gt; atribut.

**Výpis 7 – Controllers\ProfileController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

Je-li vyvolat akci Index() vystavené kontroleru v zobrazení 7 se stejnou dobu vrátit za 1 hodinu.

#### <a name="summary"></a>Souhrn

Ukládání výstupu do mezipaměti nabízí velmi snadné metoda výrazně zlepšit výkon aplikací ASP.NET MVC. V tomto kurzu jste zjistili, jak používat &lt;OutputCache&gt; atribut pro ukládání do mezipaměti výstupu akce kontroleru. Také jste se naučili úpravy vlastnosti &lt;OutputCache&gt; atribut jako je například doba trvání a VaryByParam vlastnosti, které chcete upravit, jak získá obsah ukládá do mezipaměti. Nakonec jste zjistili, jak definovat profily mezipaměti v konfiguračním souboru webu.

> [!div class="step-by-step"]
> [Předchozí](understanding-action-filters-vb.md)
> [další](adding-dynamic-content-to-a-cached-page-vb.md)
