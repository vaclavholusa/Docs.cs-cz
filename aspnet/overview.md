---
uid: overview
title: "Přehled technologie ASP.NET | Microsoft Docs"
author: rick-anderson
description: "Úvod k technologii ASP.NET, volné architektura pro vytváření webů, webových aplikací a webových rozhraní API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: 
msc.type: content
ms.openlocfilehash: 0ba7814d4004b17e678eab9a2a41a6d6f34773e1
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/08/2018
---
# <a name="aspnet-overview"></a>Přehled technologie ASP.NET

ASP.NET je bezplatná webová architektura pro vytváření kvalitních webů a webových aplikací pomocí jazyka HTML, CSS a JavaScript. Můžete také vytvořit rozhraní Web API a používat v reálném čase technologie, jako například objekty Websocket.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) je alternativa k technologie ASP.NET.  Najdete v článku [pokyny o tom, jak zvolit ASP.NET a ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Začínáme

[Visual Studio Community 2017](https://www.visualstudio.com/downloads/), uvolněte IDE pro technologii ASP.NET v systému Windows.

## <a name="websites-and-web-applications"></a>Weby a webové aplikace

 Nabízí tři rozhraní pro vytváření webových aplikací ASP.NET: webových formulářů ASP.NET MVC a webových stránek ASP.NET. Všechny tři architektury jsou stabilní a vyspělá a skvělé webových aplikací můžete vytvořit pomocí některé z nich. Bez ohledu na to, jaké framework zvolíte získáte všechny výhody a funkce everywhere technologie ASP.NET.

Každý framework se zaměřuje na vývoj pro jiný styl. Ten, který zvolíte, závisí na kombinaci vaše programovací prostředky (znalosti, schopnosti a vývojové prostředí), typ aplikace, kterou vytváříte a vývoj přístupů, které vám vyhovuje s.

Dole je přehled o každé rozhraní a některé doporučení, jak vybrat mezi nimi. Pokud upřednostňujete video úvod, najdete v části [vytváření webů s technologií ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) a [co je nástroje pro Web?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Pokud máte zkušenosti | Vývoj styl | Znalosti | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| webové formuláře | Win formuláře WPF, rozhraní .NET | Rychlý vývoj pomocí bohaté knihovny ovládacích prvků, které zapouzdření značka jazyka HTML | Střední úroveň, pokročilé RAD |
| MVC       | Ruby, na které, rozhraní .NET  | Plnou kontrolu nad značka jazyka HTML, kód a značek oddělených a usnadňují zápis testů. Nejlepší volbou pro mobilní a jednostránkové aplikace (SPA). | Střední úroveň, Upřesnit |
| Webové stránky  | Klasické ASP, PHP     | Kód HTML a váš kód společně ve stejném souboru | Nové, střední úrovně |

### <a name="web-forms"></a>webové formuláře

S webovými formuláři ASP.NET můžete vytvořit dynamické weby s využitím známý model přetahování myší, založeného na událostech. Návrhová plocha a stovky ovládacích prvků a komponent vám umožní rychle vytvořit sofistikované řízené uživatelského rozhraní weby s výkonným přístup k datům. 

[Další informace o webových formulářů](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC poskytuje výkonné, na základě vzory způsob, jak vytvářet dynamické weby s umožňující čistou oddělené oblasti zájmu a které vám plnou kontrolu nad značek pro vývoj zábavná, agilní. ASP.NET MVC zahrnuje řadu funkcí, které umožňují rychlý, TDD procházející vývoj pro vytvoření sofistikované aplikace, které používají nejnovější webové standardy. 

[Další informace o MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET – webové stránky

ASP.NET Web Pages a syntaxe Razor poskytují rychlý, schůdný a snadný způsob kombinování kódu serveru s jazykem HTML za účelem vytvoření dynamického webového obsahu. Připojení k databázím, přidat video, odkaz na weby sociálních sítí a zahrnout mnoho funkcí, které vám pomohou vytvářet nádherné weby, které jsou v souladu s nejnovějšími webovými standardy.

[Další informace o webových stránek](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Poznámky o webových formulářů, MVC a webových stránek

Všechny tři rozhraní ASP.NET jsou založené na rozhraní .NET Framework a sdílet základní funkce rozhraní .NET a technologie ASP.NET. Například všechny tři architektury nabízejí kolem členství na základě modelu zabezpečení přihlášení a všechny tři sdílet stejnou zařízení pro správu požadavky, zpracování relací a tak dále, které jsou součástí klíčové funkce ASP.NET.

Kromě toho tři rozhraní nejsou zcela nezávislé a zvolili jeden nebrání použití jiného. Vzhledem k tomu, že rozhraní můžou existovat společně ve stejném webové aplikaci, není zobrazíte jednotlivých součástí aplikace napsané v různých architektury. Například může být zákazníkem částí aplikace vyvinuté v MVC za účelem optimalizace značku, při přístupu k datům a správu části jsou vyvinuté v webových formulářů využívat výhod ovládací prvky datových a jednoduchý data access.

## <a name="web-apis"></a>Webová rozhraní API

Rozhraní ASP.NET Web API je rozhraní, které usnadňuje sestavování služeb HTTP, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení. Rozhraní ASP.NET Web API je ideální platformu pro sestavování aplikací RESTful v rozhraní .NET Framework.

[Další informace o rozhraní Web API](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>V reálném čase technologie

Funkce SignalR technologie ASP.NET je nová knihovna pro vývojáře využívající technologii ASP.NET, která usnadňuje vývoj funkce webu v reálném čase. SignalR umožňuje obousměrnou komunikaci mezi serverem a klientem. Servery můžete nabízet obsah připojeným klientům okamžitě, jakmile je k dispozici. SignalR podporuje objekty Websocket a spadne zpět na jiné kompatibilní techniky pro starší prohlížeče. SignalR zahrnuje rozhraní API pro správu připojení (například připojit a odpojit událostí), seskupování připojení a autorizaci.

[Další informace o funkci SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Mobilní aplikace a weby 

ASP.NET můžete spotřeby nativní mobilních aplikací pomocí webového rozhraní API back-end, stejně jako mobilní weby pomocí přizpůsobivý návrh architektury, jako je Twitter Bootstrap. Pokud vytváříte nativní mobilní aplikace, je snadné vytvořit Web API na základě JSON popisovač přístup k datům, ověřování a nabízených oznámení do vaší aplikace. Pokud vytváříte přizpůsobivý webu mobilní, můžete použít všechny šablon stylů CSS framework nebo otevřete mřížky systému dáváte přednost, nebo vyberte výkonné mobilního systému jako jQuery Mobile nebo Sencha a kvalitních mobilních aplikací s PhoneGap.

[Další informace o vývoji pro mobilní aplikace a lokality](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Jednostránkové aplikace 

ASP.NET jedné stránky aplikace (SPA) pomáhá vytvářet aplikace, které zahrnují významné interakce na straně klienta pomocí standardu HTML 5, 3 šablon stylů CSS a JavaScript. Visual Studio obsahuje šablonu pro vytváření jednostránkové aplikace s použitím knockout.js a ASP.NET Web API. Kromě předdefinovaných SPA šablony vytvořené komunity SPA šablony jsou také k dispozici ke stažení.

[Další informace o vývoj jednostránkové aplikace](single-page-application/index.md)

## <a name="webhooks"></a>WebHooky

Webhooky je lightweight vzor HTTP poskytuje jednoduché pub nebo dílčí model pro připojení služby webovým rozhraním API a SaaS společně. V případě události ve službě je odesláno upozornění ve formě požadavek HTTP POST registrované odběratelům. Požadavek POST obsahuje informace o události, která vám umožní pro příjemce tak, aby fungoval odpovídajícím způsobem.

Webhooky jsou vystavené velký počet služeb včetně Dropbox, Githubu, Instagram, MailChimp, PayPal, Slack, Trello a mnoho dalších. WebHook, jehož můžete například naznačují, že došlo ke změně souboru v Dropbox, nebo změnit kód byl potvrzen v Githubu, nebo byla inicializována platba v PayPal nebo v Trello byl vytvořen na kartě.

[Další informace o Webhooky](webhooks/index.md)





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
