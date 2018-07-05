---
uid: overview
title: 'ASP.NET: Přehled | Dokumentace Microsoftu'
author: rick-anderson
description: Úvod do ASP.NET, bezplatná rozhraní pro vytváření webů, webových aplikací a webových rozhraní API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: ''
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: de094258984e3ca09e4eadf21169cad9bfc67b22
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362259"
---
# <a name="aspnet-overview"></a>ASP.NET: Přehled

ASP.NET je bezplatné webové rozhraní pro vytváření skvělých webů a webových aplikací s využitím HTML, CSS a JavaScriptu. Můžete také vytvářet webová rozhraní API a používat technologie pro reálný čas, jako jsou webové sokety.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) představuje alternativu k ASP.NET.  Zobrazit [pokyny o tom, jak zvolte mezi ASP.NET a ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Začínáme

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), bezplatné integrované vývojové prostředí pro ASP.NET ve Windows.

## <a name="websites-and-web-applications"></a>Weby a webové aplikace

 Nabízí tři architektury pro vytváření webových aplikací ASP.NET: webové formuláře ASP.NET MVC a ASP.NET Web Pages. Všechny tři architektury jsou stabilní a až po zralé a vývoje skvělé nové webové aplikace můžete vytvořit pomocí některé z nich. Bez ohledu na to, jakou architekturu zvolíte získáte všechny výhody a funkce technologie ASP.NET všude.

Každé rozhraní, zaměřuje na jiný vývojářský style. Ten, který zvolíte, závisí na kombinaci vaše programovací prostředky (znalostní báze, znalosti a zkušenosti s vývojem), typ aplikace, kterou vytváříte a přístupu k vývoji, které už znáte.

Níže je uveden přehled každé rozhraní a několik nápadů, jak zvolit mezi nimi. Pokud dáváte přednost úvodní video, přečtěte si téma [vytváření webů s ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) a [novinky nástroje pro Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Pokud máte prostředí | Stylu vývoje | Odborných znalostí | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| webové formuláře | Win Forms, WPF, .NET | Rychlý vývoj pomocí bohatá Knihovna ovládacích prvků, které provádí zapouzdření kódu HTML | Střední úroveň, pokročilé RAD |
| MVC       | Aplikace Ruby on Rails, .NET  | Plnou kontrolu nad značky HTML, kód a značky oddělené a usnadňují psaní testů. Nejlepší volbou pro mobilní a jednostránkové aplikace (SPA). | Střední úroveň, Upřesnit |
| Webové stránky  | Klasické ASP, PHP     | Značka jazyka HTML a váš kód společně ve stejném souboru | Nové, střední úroveň |

### <a name="web-forms"></a>webové formuláře

Pomocí webových formulářů ASP.NET můžete vytvářet dynamické weby s využitím známý model přetažení myší, založené na událostech. Návrhová plocha a stovky ovládacích prvků a komponent vám umožní rychle vytvořit sofistikované, výkonné uživatelského rozhraní – weby s přístup k datům. 

[Další informace o webových formulářů](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC poskytuje výkonný, na základě vzorů způsob tvorby dynamických webů, která umožňuje jasně oddělit oblasti zájmu a dává vám plnou kontrolu nad značkami pro prostřednictvím agilního vývoje. ASP.NET MVC zahrnuje řadu funkcí, které umožňují rychlé a podporou TDD vývoj pro vytváření sofistikovaných aplikací s využitím nejnovějších webových standardů. 

[Další informace o MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET – webové stránky

ASP.NET Web Pages a syntaxe Razor poskytují rychlý, přístupný a jednoduchý způsob kombinování kódu serveru s jazykem HTML za účelem vytvoření dynamického webového obsahu. Připojení k databázím, přidat video, odkaz na weby sociálních sítí a zahrnout mnoho funkcí, které vám pomůžou vytvářet nádherné weby, které v souladu s nejnovějšími webovými standardy.

[Další informace o webových stránkách](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Poznámky o webové formuláře, MVC a Web Pages

Všechny tři architektury ASP.NET jsou založeny na rozhraní .NET Framework a sdílet základní funkce rozhraní .NET a technologie ASP.NET. Například všechny tři architektury nabízejí kolem členství na základě modelu zabezpečení přihlášení a všechny tři sdílení stejných prostředků pro správu požadavků, zpracování relace a tak dále, které jsou součástí sady funkcí technologie ASP.NET core.

Kromě toho tři rozhraní nejsou zcela nezávislé a výběrem jedné nebrání použití jiného. Jelikož rozhraní mohou existovat vedle sebe v stejnou webovou aplikaci, není nic neobvyklého, najdete v článku jednotlivých součástí aplikace napsané v různých rozhraní. Například určených pro zákazníky části aplikace mohou být vytvořeny v MVC pro optimalizaci kódu při přístupu k datům a správu části jsou vyvíjeny ve webových formulářích výhod ovládací prvky dat a přístup k datům jednoduché.

## <a name="web-apis"></a>Webová rozhraní API

ASP.NET Web API je architektura, která usnadňuje sestavování služeb HTTP, které jsou poskytovány širokému spektru klientů, včetně prohlížečů a mobilních zařízení. ASP.NET Web API je ideální platformu pro sestavování aplikací RESTful v rozhraní .NET Framework.

[Další informace o rozhraní Web API](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Technologie pro reálný čas

Funkce SignalR technologie ASP.NET je nová knihovna pro vývojáře využívající technologii ASP.NET, který usnadňuje vývoj funkcí v reálném čase. Funkce SignalR umožňuje obousměrnou komunikaci mezi serverem a klientem. Servery můžete nabízet obsah připojeným klientům okamžitě, jakmile je k dispozici. Podporuje objekty Websocket SignalR a spadne zpět na jiné kompatibilní techniky pro starší prohlížeče. Funkce SignalR zahrnuje rozhraní API pro správu připojení (pro instanci, připojení a odpojení události), seskupování připojení a autorizaci.

[Další informace o funkci SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Mobilní aplikace a weby 

ASP.NET může power nativních mobilních aplikací pomocí back-end rozhraní Web API, mobilní weby pomocí přizpůsobivý návrh architektury, jako je Twitter Bootstrap. Pokud vytváříte nativní mobilní aplikace, je snadné vytvořit na základě JSON webové rozhraní API pro přístup k datům popisovač, ověřování a nabízená oznámení pro vaši aplikaci. Pokud vytváříte responzivní webu pro mobilní zařízení, můžete použít jakékoli architektury šablon stylů CSS nebo otevřít mřížky systému dáváte přednost, nebo vyberte výkonné mobilní systém, jako je jQuery Mobile nebo Sencha a skvělé mobilní aplikace s PhoneGap.

[Další informace o vývoji pro mobilní aplikace a lokality](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Jednostránkové aplikace 

ASP.NET jedním jednostránková aplikace (SPA) pomáhá vytvářet aplikace, které obsahují důležité interakce na straně klienta pomocí HTML 5, CSS 3 a JavaScript. Visual Studio obsahuje šablony pro vytváření jednostránkové aplikace pomocí rozhraní knockout.js a ASP.NET Web API. Kromě předdefinovaných šablon SPA komunitou vytvořených SPA šablony jsou také k dispozici ke stažení.

[Další informace o vývoji pro jednostránkové aplikace](single-page-application/index.md)

## <a name="webhooks"></a>WebHooky

Webhooky se odlehčeného vzoru HTTP poskytuje jednoduché pub/sub model pro vzájemné propojení dohromady webová rozhraní API a služby SaaS. Případě určité události ve službě, oznámení se posílá ve formuláři požadavku HTTP POST pro registrované předplatitele. Požadavek POST obsahuje informace o události, která umožňuje příjemci příslušně na ně reagovat.

Webhooky jsou vystavené velkým množstvím služeb včetně Dropboxu, Githubu, Instagramu, MailChimp, PayPal, Slack, Trello a mnoho dalších. Například Webhooku může znamenat, že došlo ke změně souboru na Dropboxu, změny kódu byl potvrzen v Githubu, nebo platby byl zahájen v PayPal nebo vytvoření karty v Trellu.

[Další informace o Webhooků](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
