---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Přehled rozhraní ASP.NET MVC | Microsoft Docs
author: microsoft
description: Další informace o rozdílech mezi aplikace ASP.NET MVC a aplikace webových formulářů ASP.NET. Postup při rozhodování, kdy vytvořit aplikaci ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26564565"
---
<a name="aspnet-mvc-overview"></a>Přehled rozhraní ASP.NET MVC
====================
podle [Microsoft](https://github.com/microsoft)

> Další informace o rozdílech mezi aplikace ASP.NET MVC a aplikace webových formulářů ASP.NET. Postup při rozhodování, kdy vytvořit aplikaci ASP.NET MVC.


Vzor architektury Model-View-Controller (MVC) rozděluje aplikace do tří hlavních součástí: model, zobrazení a kontroler. Rozhraní ASP.NET MVC poskytuje alternativu ke vzoru webových formulářů ASP.NET pro vytváření založené na MVC webových aplikací. Rozhraní ASP.NET MVC je odlehčený, intenzivního prezentační architektura která (stejně jako u aplikací webových formulářů) je integrována stávajících funkcí technologie ASP.NET, jako jsou hlavní stránky a ověřování na základě členství. Architektura MVC je definována v **System.Web.Mvc** obor názvů a je zásadní, podporované součástí **System.Web** oboru názvů.   
  
MVC je standardní návrhový vzor, který se seznámíte s celá řada vývojářů. Některé typy webových aplikací bude těžit z rozhraní MVC. Ostatní bude, že bude nadále používat tradiční vzor aplikací ASP.NET, který je založen na webové formuláře a zpětná vystavení. Jiné typy webových aplikací budou tyto dva přístupy; kombinovat. nevylučují vyloučí dalších.   
  
Architektura MVC zahrnuje následující součásti:


[![Vyvolání akce kontroleru, která očekává hodnotu parametru](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Obrázek 01**: vyvolání akce kontroleru, která očekává hodnotu parametru ([Kliknutím zobrazit obrázek v plné velikosti](asp-net-mvc-overview/_static/image2.png))


- **Modely**. Objekty modelů jsou části aplikace, které implementují logiku pro datovou doménu s aplikací. Objekty modelů často, načtěte a ukládají stav modelu v databázi. Objekt produktu může například načíst informace z databáze, pracovat s nimi a pak zapsat aktualizované informace zpět do tabulky produktů v systému SQL Server.

V malých aplikacích model je často představuje koncepční oddělení, nikoli fyzické. Například pokud aplikace jenom čte datové sady a odešle ji do zobrazení, aplikace nemá fyzickou vrstvu a související třídy. V takovém případě datová sada přebírá roli objektu modelu.

- **Zobrazení**. Zobrazení jsou komponenty, které zobrazují aplikace s uživatelské rozhraní (UI). Toto uživatelské rozhraní se obvykle je vytvořena z dat modelu. Příkladem může být zobrazení tabulky produktů, které zobrazuje textová pole, rozevírací seznamy a zaškrtávací políčka na základě aktuálního stavu objektu produkty.

- **Řadiče**. Kontrolery jsou komponenty, které zpracovávají interakci s uživatelem, pracují s modelem a konečně vybírají vykreslené zobrazení zobrazující uživatelské rozhraní. V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce. Kontroler například zpracovává hodnoty řetězce dotazu a předává tyto hodnoty modelu, který je následně dotazuje databázi pomocí hodnot.

Vzor MVC pomáhá vytvářet aplikace, které separují jednotlivé aspekty aplikace (logika vstupu, obchodní logika a logika uživatelského rozhraní) současně poskytují volné párování mezi těmito elementy. Vzor Určuje, kde by jednotlivé typy logiky umístěny v aplikaci. Logika uživatelského rozhraní patří do zobrazení. Logika vstupu patří do kontroleru. Obchodní logika patří do modelu. Toto oddělení vám pomůže spravovat složitost při sestavování aplikaci, protože umožňuje zaměřit se na jeden aspekt jejich implementace najednou. Například se můžete soustředit na zobrazení bez závislosti na obchodní logiku.   
  
Kromě správy složitých vzor MVC usnadňuje testování aplikací, než je k otestování aplikace využívající webové formuláře ASP.NET Web. Například v aplikaci využívající webové formuláře ASP.NET Web, jedna třída slouží k zobrazení výstupu i k reakci na vstup uživatele. Vytváření automatizovaných testů pro aplikace ASP.NET využívající webové formuláře může být složité, protože k testování jednotlivých stránek, je třeba vytvořit instanci třídy stránky, všech jeho podřízených ovládacích prvků a dalších závislých tříd v aplikaci. Vzhledem k tomu, že ke spuštění stránky jsou vytvářeny instance tolika tříd, může být obtížné soustředit se výlučně na jednotlivé části aplikace. Testů pro aplikace využívající webové formuláře ASP.NET, proto může být obtížnější než testy v aplikaci MVC implementace. Testy v aplikaci ASP.NET využívající webové formuláře navíc vyžadují webový server. Architektura MVC odděluje jednotlivé komponenty a výrazně využívá rozhraní, což umožňuje testování jednotlivých komponent izolovaně od zbytku architektury.   
  
Volné párování mezi třemi hlavními komponentami architektury MVC rovněž podporuje paralelní vývoj. Například jeden vývojář může pracovat na zobrazení, druhý může pracovat na logice kontroleru a třetí můžete soustředit na obchodní logiku v modelu.

## <a name="deciding-when-to-create-an-mvc-application"></a>Rozhodování, kdy vytvořit aplikaci MVC

Je třeba důkladně zvážit ohledně webovou aplikaci implementovat pomocí rozhraní ASP.NET MVC nebo model webových formulářů ASP.NET. Architektura MVC nenahrazuje model webových formulářů; Můžete buď framework pro webové aplikace. (Pokud máte stávající aplikace využívající webové formuláře, i nadále pracovat stejným způsobem jako mají vždy.)   
  
Předtím, než se rozhodnete použít architekturu MVC nebo model webových formulářů pro konkrétní web, zvažte výhody obou těchto přístupů.

### <a name="advantages-of-an-mvc-based-web-application"></a>Výhody založené na MVC webové aplikace

Rozhraní ASP.NET MVC nabízí následující výhody:

- Ho usnadňuje správu složitých vydělením aplikace do modelu, zobrazení a kontroler.
- Nepoužívá se stav zobrazení ani formuláře umístěné na serveru. Díky rozhraní MVC ideální pro vývojáře, kteří chtějí mít plnou kontrolu nad chováním aplikace.
- Využívá vzor Front Controller, který zpracovává požadavky webové aplikace prostřednictvím jediného kontroleru. To umožňuje návrh aplikací podporující infrastrukturu s rozsáhlým směrováním. Další informace najdete v tématu [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") na webu MSDN.
- Poskytuje lepší podporu pro testy řízený vývoj (TDD).
- Funguje dobře pro webové aplikace, které jsou podporovány velkými týmy vývojářů a webových návrhářů, kteří potřebují značnou míru kontroly nad chováním aplikací.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Výhody webové aplikace webových formulářů

Architektura využívající webové formuláře nabízí následující výhody:

- Podporuje model událostí, který zachovává stav prostřednictvím protokolu HTTP, což je výhodné pro vývoj webových aplikací – obchodní. Aplikace využívající webové formuláře poskytuje desítky událostí, které jsou podporovány stovkami ovládacích prvků serveru.
- Využívá vzor Page Controller, který přidává funkce jednotlivým stránkám. Další informace najdete v tématu [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") na webu MSDN.
- Využívá stav zobrazení ani formuláře umístěné na serveru, které může usnadnit správu informací o stavu.
- Dobře se hodí pro menší týmy webových vývojářů a návrhářů, kteří chtějí využít výhod velkého množství komponent, které jsou k dispozici pro rychlý vývoj aplikací.
- Je obecně méně složitých pro vývoj aplikací, protože komponenty ( **stránky** třídy, ovládací prvky a tak dále) jsou pevně integrovány a obvykle vyžadují méně kódu než MVC model.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funkce architektury ASP.NET MVC

Rozhraní ASP.NET MVC poskytuje následující funkce:

- Oddělení aplikačních úloh (logika vstupu, obchodní logika a logika uživatelského rozhraní), testovatelnost a testy řízený vývoj (TDD) ve výchozím nastavení. Všechny základní kontrakty architektury MVC jsou založeny na rozhraní a lze je testovat pomocí mock objektů, což jsou simulované objekty, které napodobují chování skutečných objektů v aplikaci. Můžete testování aplikace bez nutnosti spouštět kontrolery v procesu ASP.NET, takže je testování jednotek rychlé a flexibilní. Můžete použít libovolnou architekturu testování částí, který je kompatibilní s rozhraním .NET Framework.
- Rozšiřitelná a modulární architektura. Komponenty architektury ASP.NET MVC jsou navrženy tak, aby mohly být snadno nahradit a přizpůsobit. Můžete zařadit vlastní zobrazovací modul, zásady směrování adres URL, serializaci parametrů metody akce a další součásti. Rozhraní ASP.NET MVC dále podporuje použití modelů kontejner vkládání závislostí (DI) a v inverzi ovládací prvek (IOC). DI umožňuje injektovat objekty do tříd, namísto spoléhání na třídu, že objekt vytvoří sama. Technologie IOC Určuje, že když objekt vyžaduje jiný objekt, první objekt by měl získat druhý objekt z vnějšího zdroje, jako je konfigurační soubor. Tím je testování jednodušší.
- Efektivní komponenta mapování adres URL, která umožňuje vytvářet aplikace, které mají srozumitelné a dohledatelné adresy URL. Adresy URL nemusí obsahovat přípony názvu souborů a podporují vzory mapování adres URL, které fungují dobře u vyhledávání modul optimalizace (vyhledávací weby SEO) a adresování representational stavu transfer (REST).
- Podpora pro použití kódu značek v existující stránku ASP.NET (soubory .aspx), uživatelský ovládací prvek (soubory .ascx) a hlavní soubory značek stránek (soubory .master) jako šablon zobrazení. Můžete využívat stávající funkce ASP.NET s rozhraním ASP.NET MVC framework, jako jsou vnořené hlavní stránky, inline výrazy (&lt;% = %&gt;), deklarativní ovládací prvky serveru, šablony, vazby dat, lokalizace a tak dále.
- Podpora stávajících funkcí technologie ASP.NET. ASP.NET MVC umožňuje využívat funkce, jako je ověřování pomocí formulářů a ověřování systému Windows, autorizace adres URL, členství a role, výstupní a ukládaní dat do mezipaměti, správu stavu relací a profilů, sledování stavu, konfigurační systém a zprostředkovatele Architektura.
