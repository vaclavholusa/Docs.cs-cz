---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: Souboru ReadMe pro rozhraní ASP.NET Web Pages 2 Developer Preview | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 09/14/2011
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 0a89216c0f65d49d00c96a27a5ab33e3872f9bc5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818343"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>Souboru ReadMe pro rozhraní ASP.NET Web Pages 2 Developer Preview
====================
podle [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>Souboru ReadMe pro rozhraní ASP.NET Web Pages 2 Developer Preview

14. září 2011

### <a name="contents"></a>Obsah

#### <a id="_Toc303701284"></a>  Poznámky k instalaci

Chcete-li nainstalovat webové stránky 2 Developer Preview, máte tyto možnosti:

- Instalovat pomocí služby WebMatrix 2 Beta [instalačního programu webové platformy](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix je sada nástrojů pro vývoj bezplatná webová, který obsahuje rozhraní ASP.NET Web Pages. Další informace najdete v části instalace v [začátek funkce ve verzi Preview pro vývojáře ASP.NET Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

- Nainstalovat Web Pages 2 Developer Preview přímo pomocí [odkaz ke stažení](https://go.microsoft.com/fwlink/?LinkID=226335). Tuto metodu použijte, pokud chcete vytvářet aplikace Web Pages pomocí textového editoru, jako je například Poznámkový blok. Aby bylo možné spouštět aplikace Web Pages 2, musíte mít službu IIS 7.5 Express. (To je součástí automaticky pomocí služby WebMatrix.) Tipy, jak otestovat stránku webových stránek pomocí služby IIS Express, naleznete v části "Vytvoření a testování ASP.NET stránky pomocí svůj vlastní textový Editor" na bočním panelu v [Začínáme se službou WebMatrix a webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).

Rozhraní ASP.NET Web Pages 2 Developer Preview je možné nainstalovat a spustit vedle sebe s 1 webové stránky ASP.NET. <a id="a"></a>Podrobnosti najdete v tématu v části "Spuštění webové stránky aplikace – souběžně" [horní části funkce ve verzi Preview pro vývojáře Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Dokumentace ke službě

Kurzy a další informace o webových stránkách ASP.NET jsou k dispozici na stránce webové stránky na webu technologie ASP.NET ([https://www.asp.net/web-pages/](../../index.md)). Informace o nových funkcích a vylepšeních v Web Pages 2 najdete v tématu [horní části funkce ve verzi Preview pro vývojáře Web Pages 2](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Podpora

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Toto je verze preview a není oficiálně podporován. Pokud máte dotazy týkající se práce v této vydané verzi, příspěvek do fóra ASP.NET Web Pages ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), kde jsou často schopni poskytovat podporu neformální členové komunity technologie ASP.NET.

#### <a id="_Toc303701287"></a>  Požadavky na software

ASP.NET Web Pages 2 vyžaduje rozhraní .NET Framework 4. Funguje i v rozhraní .NET Framework 4.5 Developer Preview verzi.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Opravy známých problémů a nejnovější změny

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Je\* metod (například IsDateTime) nyní návratový správné hodnoty pro všechny jazykové verze.** Některé metody, jako jsou *IsDateTime* dříve vrácené *false* při má být vrácen *true* vzhledem k tomu, že byly dříve provádění kontrol specifické pro jazykovou verzi. Tyto metody jsou opravená nyní vzít v úvahu jazykovou verzi. Toto je zásadní změnu; Pokud vaše aplikace závisí na staré chování, přeruší.
- **Chování metody Href změnila.** Volání Href("~/SomeFile") dříve, vrátí adresu URL relativní k aktuálně spuštěném souboru. Nyní Href("~/SomeFile") vždy vrátí absolutní cesta z kořenového adresáře aplikace. Většině případů toto chování neprovede rozdíl v návratové hodnotě. Tato změna byla provedena k vyřešení některých scénářích Ajax. Zvažte například následující příklad kódu: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Tento kód dříve by přeložit Images/Logo.jpg, který bude pro požadavek Ajax na tuto stránku. Se teď vyřeší do kořenového adresáře (/ MySite/Images/Logo.jpg).
- **Metoda HttpContext.RedirectLocal změněna**. Tato metoda nyní přijímá pouze adresy URL, které jsou relativní vzhledem k aktuální aplikaci. Plně kvalifikovaný adresy URL odmítají.
- **Metoda ModelState.IsValid teď vyžaduje, abyste metodu Validate volejte nejdříve**. Pokud převádíte aplikace pomocí nových metod ověření vstupu a že voláte *ModelState.IsValid* metodu, musíte teď volat *Validation.Validate* předem. Například nyní postupujte podle tohoto vzoru: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Doporučujeme však, pokud používáte nové metody ověření vstupu, nepoužívejte *ModelState.IsValid*. Místo toho strukturovat váš kód tímto způsobem: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **V aplikaci Internet Explorer 8 a Internet Explorer 7, ověřování na straně klienta nebude fungovat**. Ověřování na straně klienta nefunguje z důvodu nekompatibility s jQuery 1.6.2, který je součástí výchozí šablony projektu. (Ověřování na straně serveru funguje.).

#### <a id="_Toc303701289"></a>  Právní omezení

© 2011 Microsoft Corporation. Všechna práva vyhrazena. Tento dokument se poskytuje "jako-je." Informace a názory vyjádřené v tomto dokumentu včetně adres URL a jiných internetových webových odkazů, mohou změnit bez předchozího upozornění. Nesete veškerá rizika s jejich použitím.
