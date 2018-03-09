---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: "Rozhraní ASP.NET Web Pages 2 Developer Preview ReadMe | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 119265c62abb3f3110cdc7f0b94a7c9b16b4251c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>Rozhraní ASP.NET Web Pages 2 Developer Preview ReadMe
====================
podle [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>Rozhraní ASP.NET Web Pages 2 Developer Preview ReadMe

14 září 2011

### <a name="contents"></a>Obsah

#### <a id="_Toc303701284"></a>Poznámky k instalaci

Abyste mohli nainstalovat Web Pages 2 Developer Preview, máte tyto možnosti:

- Instalovat pomocí služby WebMatrix 2 Beta [instalačního programu webové platformy](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix je sada nástrojů volné web development tools, obsahující rozhraní ASP.NET Web Pages. Další informace najdete v části instalace v [horní funkce v ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Nainstalujte Web Pages 2 Developer Preview přímo pomocí [stáhnout odkaz](https://go.microsoft.com/fwlink/?LinkID=226335). Tuto metodu použijte, pokud chcete vytvořit webové stránky aplikace pomocí textového editoru, například Poznámkový blok. Aby bylo možné spouštět aplikace Web Pages 2, musíte mít službu IIS 7.5 Express. (To je zahrnuté automaticky pomocí služby WebMatrix.) Tipy pro testovací stránka Web Pages pomocí služby IIS Express, najdete v tématu "Vytváření a testování ASP.NET stránky pomocí vaše vlastní textového editoru" na bočním panelu v [Začínáme se službou WebMatrix a webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).

Rozhraní ASP.NET Web Pages 2 Developer Preview lze nainstalovat a spustit souběžně sdílená s 1 webové stránky ASP.NET. <a id="a"></a>Podrobnosti najdete v části "Spuštění webové stránky aplikace-souběžného" v [horní funkce v Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>Dokumentace

Kurzy a další informace o webových stránkách ASP.NET, které jsou k dispozici na stránce webové stránky ASP.NET webu ([https://www.asp.net/web-pages/](../../index.md)). Informace o nových funkcích a vylepšeních v Web Pages 2 najdete v tématu [horní funkce v Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>Podpora

<a id="_Toc209852135"></a><a id="_Toc255833657"></a>Toto je verze preview a není oficiálně podporován. Pokud máte dotazy týkající se práce s touto verzí, odešlete je na fóru rozhraní ASP.NET Web Pages ([https://forums.asp.net/1224.aspx/1?WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) ), kde jsou často schopen poskytnout neformální podporu členové komunity služby ASP.NET.

#### <a id="_Toc303701287"></a>Požadavky na software

ASP.NET Web Pages 2 vyžaduje rozhraní .NET Framework 4. Funguje taky s verzí rozhraní .NET Framework 4.5 Developer Preview.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Opravy známých problémů a nejnovější změny

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Je\* metody (například IsDateTime) teď návratový správné hodnoty pro všechny jazykové verze.** Některé metody, jako *IsDateTime* předtím vrátila *false* Pokud má být vrácen *true* vzhledem k tomu, že byly dříve provádění kontroly specifické pro jazykovou verzi. Tyto metody bylo opraveno teď vzít v úvahu jazykovou verzi. Toto je narušující změně; Pokud vaše aplikace spoléhá na staré chování, budou přerušeny.
- **Došlo ke změně chování Href metody.** Volání metody Href("~/SomeFile") dříve, vrátí adresu URL relativní k aktuálně spuštěném souboru. Nyní Href("~/SomeFile") vždy vrátí absolutní cestu z kořenového adresáře aplikace. Většině případů nebude toto chování je nějaký rozdíl v návratovou hodnotu. Tato změna byla provedená opravit určitých scénářích Ajax. Zvažte například následující příklad kódu: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Tento kód dříve by vyřešit Images/Logo.jpg, který bude pro požadavek Ajax na této stránce správný. Nyní bude vyřešen do kořenového adresáře (nebo MySite/Images/Logo.jpg).
- **Došlo ke změně metodu HttpContext.RedirectLocal**. Tato metoda nyní přijímá pouze adresy URL, které jsou relativní vzhledem k aktuální aplikaci. Plně kvalifikovaný byly zamítnuty adresy URL.
- **Metoda ModelState.IsValid teď vyžaduje, abyste nejprve volání ověřením**. Pokud převádíte aplikace k používání nové metody ověřování vstupu a jsou volání *ModelState.IsValid* metodu, musíte teď volat *Validation.Validate* předem. Například nyní postupujte podle tohoto vzoru: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

 Doporučujeme však, pokud používáte nové metody ověření vstupu, nepoužívejte *ModelState.IsValid*. Místo toho struktury kódu takto: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **V aplikaci Internet Explorer 8 a Internet Explorer 7, nefunguje ověřování na straně klienta**. Ověřování na straně klienta nefunguje z důvodu nekompatibility s jQuery 1.6.2, který je součástí výchozí šablona projektu. (Ověřování na straně serveru funguje.).

#### <a id="_Toc303701289"></a>Právní omezení

© Microsoft Corporation. 2011. Všechna práva vyhrazena. Tento dokument je poskytován "jako-je." Informace a názory vyjádřené v tomto dokumentu včetně adres URL a dalších odkazů na internetové weby, mohou změnit bez předchozího upozornění. Můžete na sebe rizika spojená s jejím používáním.
