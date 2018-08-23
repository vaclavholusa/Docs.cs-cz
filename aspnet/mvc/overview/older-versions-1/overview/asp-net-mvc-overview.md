---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: ASP.NET MVC – přehled | Dokumentace Microsoftu
author: microsoft
description: Přečtěte si o rozdílech mezi aplikace ASP.NET MVC a aplikace webových formulářů ASP.NET. Zjistěte, jak rozhodnout, kdy k sestavení aplikace ASP.NET MVC.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 61a7841ee238ec365b7d1909221bbe3d834faf84
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752240"
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC – přehled
====================
podle [Microsoft](https://github.com/microsoft)

> Přečtěte si o rozdílech mezi aplikace ASP.NET MVC a aplikace webových formulářů ASP.NET. Zjistěte, jak rozhodnout, kdy k sestavení aplikace ASP.NET MVC.


Vzor architektury Model-View-Controller (MVC) rozděluje aplikace do tří hlavních součástí: model, zobrazení a kontroler. Architektura ASP.NET MVC poskytuje alternativu ke vzoru webových formulářů ASP.NET pro vytváření webové aplikace založené na MVC. Architektura ASP.NET MVC je jednoduchý, s možností intenzivního testování prezentační platforma, která (stejně jako u aplikací webových formulářů) je integrovaná s stávajících funkcí technologie ASP.NET, jako je například stránky předlohy a ověřování na základě členství. Architektura MVC je definována v **System.Web.Mvc** obor názvů a je součástí základní, podporované **System.Web** oboru názvů.   
  
MVC je standardní návrhový vzor, který mnoho vývojářů znají. Některé typy webových aplikací budou těžit z rozhraní MVC. Ostatní bude dál používat tradiční vzor aplikací ASP.NET, která je založena na webové formuláře a zpětná vystavení. Jiné typy webových aplikací budou tyto dva přístupy; kombinovat. žádný přístup nezahrnuje druhé.   
  
Architektura MVC zahrnuje následující součásti:


[![Vyvolání akce kontroleru, který očekává, že hodnota parametru](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Obrázek 01**: vyvolání akce kontroleru, který očekává, že hodnota parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](asp-net-mvc-overview/_static/image2.png))


- **Modely**. Objekty modelů jsou části aplikace, které implementují logiku pro datovou doménu s aplikací. Objekty modelu často, získávání a ukládání stavu modelu v databázi. Objekt produktu může například získat informace z databáze, pracovat s nimi a pak zapsat aktualizované informace zpět do tabulky produktů v systému SQL Server.

V malých aplikacích model je často představuje koncepční oddělení, nikoli fyzické. Například pokud aplikace pouze čte datové sady a odesílá je do zobrazení, aplikace nemá fyzickou vrstvu a přidružených tříd. V takovém případě datová sada přebírá roli objektu modelu.

- **Zobrazení**. Zobrazení jsou komponenty, které zobrazují aplikace s uživatelské rozhraní (UI). Standardně toto uživatelské rozhraní je vytvořena z dat modelu. Příkladem může být zobrazení tabulky produktů, které zobrazuje textová pole, rozevírací seznamy a zaškrtávací políčka na základě aktuálního stavu objektu produktů.

- **Kontrolery**. Kontrolery jsou komponenty, které zpracovávají interakci s uživatelem, pracují s modelem a konečně vybírají vykreslené zobrazení zobrazující uživatelské rozhraní. V aplikaci MVC zobrazení pouze zobrazuje informace; kontroler zpracovává a reaguje na vstup uživatele a interakce. Kontroler například zpracovává hodnoty řetězce dotazu a předává tyto hodnoty modelu, který se dotazuje databáze pomocí hodnoty.

Vzor MVC pomáhá vytvářet aplikace, které separují jednotlivé aspekty aplikace (logika vstupu, obchodní logika a logika uživatelského rozhraní) současně poskytují volné párování mezi těmito elementy. Vzor Určuje, kde jednotlivé typy logiky se musí nacházet v aplikaci. Logika uživatelského rozhraní patří do zobrazení. Logika vstupu patří do kontroleru. Obchodní logika patří do modelu. Tato separace pomáhá zvládnout složitost při vytváření aplikace, protože umožňuje zaměřit se na jeden aspekt jejich implementace v čase. Například se můžete soustředit na zobrazení bez závislosti na obchodní logiku.   
  
Kromě správy složitost, vzor MVC usnadňuje testování aplikací, než je k otestování aplikace využívající webové formuláře ASP.NET Web. Například v případě aplikace využívající webové formuláře ASP.NET Web jednu třídu slouží k zobrazení výstupu i k reakci na vstup uživatele. Vytváření automatizovaných testů pro aplikace ASP.NET využívající webové formuláře může být složité, protože k testování jednotlivých stránek, musíte vytvořit instanci třídy stránky, všech jeho podřízených ovládacích prvků a dalších závislých tříd v aplikaci. Vzhledem k tomu, že ke spuštění stránky jsou vytvořena instance tolika tříd, může být obtížné soustředit se výlučně na jednotlivé části aplikace. Testy pro aplikace využívající webové formuláře ASP.NET, proto může být obtížnější než testy v aplikaci MVC implementace. Testy v aplikaci ASP.NET využívající webové formuláře navíc vyžadují webový server. Architektura MVC odděluje jednotlivé komponenty a výrazně využívá rozhraní, což umožňuje testování jednotlivých komponent izolovaně od zbytku architektury.   
  
Volné párování mezi třemi hlavními komponentami architektury MVC rovněž podporuje paralelní vývoj. Například jeden vývojář může pracovat na zobrazení, druhý může pracovat na logice kontroleru a třetí se můžete soustředit na obchodní logiku v modelu.

## <a name="deciding-when-to-create-an-mvc-application"></a>Rozhodování o tom, kdy vytvořit aplikaci MVC

Je třeba důkladně zvážit, jestli se má implementovat webovou aplikaci pomocí rozhraní ASP.NET MVC nebo model webových formulářů ASP.NET. Architektura MVC nenahrazuje model webových formulářů; můžete použít buď rozhraní pro webové aplikace. (Pokud máte existující aplikace využívající webové formuláře, i nadále pracovat stejným způsobem jako vždy.)   
  
Předtím, než se rozhodnete použít architekturu MVC nebo model webových formulářů pro konkrétní web, zvažte výhody obou těchto přístupů.

### <a name="advantages-of-an-mvc-based-web-application"></a>Výhody využívající architekturu MVC webové aplikace

Architektura ASP.NET MVC nabízí následující výhody:

- To usnadňuje správa složitých aplikací vydělením aplikaci na model, zobrazení a kontroler.
- Nevyužívá stav zobrazení ani formuláře umístěné na serveru. Díky tomu architektura MVC ideální pro vývojáře, kteří chtějí mít plnou kontrolu nad chováním aplikace.
- Využívá vzor Front Controller, který zpracovává požadavky webové aplikace prostřednictvím jediného kontroleru. To umožňuje návrh aplikace, který podporuje infrastrukturu s rozsáhlým směrováním. Další informace najdete v tématu [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") na webové stránce MSDN.
- Poskytuje lepší podporu pro vývoj řízený testováním (TDD).
- To funguje dobře pro webové aplikace, které jsou podporovány velkými týmy vývojářů a webových návrhářů, kteří potřebují značnou míru kontroly nad chováním aplikací.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Výhody webové aplikace webových formulářů

Architektura využívající webové formuláře nabízí následující výhody:

- Podporuje model událostí, který zachovává stav prostřednictvím protokolu HTTP, což je výhodné pro vývoj webových aplikací podnikové aplikace. Aplikace využívající webové formuláře poskytuje desítky událostí, které jsou podporovány stovkami ovládacích prvků serveru.
- Využívá vzor Page Controller, který přidává funkce jednotlivým stránkám. Další informace najdete v tématu [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") na webové stránce MSDN.
- Využívá stav zobrazení ani formuláře na serveru, jimž lze usnadnit správu informací o stavu.
- Je vhodný pro malé týmy webových vývojářů a návrhářů, kteří chtějí využít nabídky velkého množství komponent vhodných pro rychlý vývoj aplikací.
- Obecně je méně složitý pro vývoj aplikací, protože komponenty ( **stránky** třídy ovládacích prvků a tak dále) jsou pevně integrovány a obvykle vyžadují méně kódu než MVC model.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funkce architektury ASP.NET MVC

Architektura ASP.NET MVC poskytuje následující funkce:

- Oddělení aplikačních úloh (logika vstupu, obchodní logika a logika uživatelského rozhraní), testovatelnost a testy řízený vývoj (TDD) ve výchozím nastavení. Všechny základní kontrakty architektury MVC jsou založeny na rozhraní a lze je testovat pomocí mock objektů, což jsou simulované objekty, které napodobují chování skutečných objektů v aplikaci. Můžete testovat aplikaci bez nutnosti spouštět kontrolery v procesu ASP.NET, takže je testování jednotek rychlé a flexibilní. Můžete použít jakékoli prostředí testování jednotek, která je kompatibilní s rozhraním .NET Framework.
- Rozšiřitelná a modulární architektura. Komponenty architektury ASP.NET MVC jsou navrženy tak, aby mohly být snadno nahradit a přizpůsobit. Můžete zařadit vlastní zobrazovací modul, zásady směrování adres URL, serializaci parametrů metody akce a další komponenty. Architektura ASP.NET MVC dále podporuje použití modelů kontejnerů Dependency Injection (DI) a IOC (Inversion of Control). DI umožňuje injektovat objekty do tříd, aniž byste museli spoléhat na třídu, že objekt vytvoří sama. Technologie IOC Určuje, že když objekt vyžaduje jiný objekt, první objekt by měl získat druhý objekt z vnějšího zdroje, jako je konfigurační soubor. Tím je testování jednodušší.
- Výkonné komponenta mapování adres URL, která vám umožňuje vytvářet aplikace, které mají srozumitelné a dohledatelné adresy URL. Adresy URL nemusí obsahovat přípony názvu souborů a jsou navrženy pro podporu vzory mapování adres URL, které fungují dobře u search engine optimization (optimalizace pro vyhledávací weby) a REST representational state transfer () adresování.
- Podpora pro použití kódu značek v stávající stránku ASP.NET (soubory .aspx), uživatelský ovládací prvek (soubory .ascx) a hlavní soubory značek stránek (soubory .master) jako šablon zobrazení. Můžete využívat stávající funkce ASP.NET s ASP.NET MVC framework, jako například vložené hlavní stránky, inline výrazy (&lt;% = %&gt;), deklarativní ovládací prvky serveru, šablony, vázání dat, lokalizace a tak dále.
- Podpora stávajících funkcí technologie ASP.NET. ASP.NET MVC umožňuje využívat funkce, jako je ověřování pomocí formulářů a ověřování Windows, autorizace adres URL, členství a role, výstup a ukládání dat do mezipaměti, správa stavů relací a profilů, sledování stavu, konfigurace systému a zprostředkovatele Architektura.
