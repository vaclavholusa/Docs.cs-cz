---
uid: whitepapers/request-validation
title: "Žádosti o ověření - prevence útoků skriptu | Microsoft Docs"
author: rick-anderson
description: "Tento dokument popisuje funkci žádosti o ověření technologie ASP.NET ve výchozím nastavení, kde aplikace je zabránit v zpracování nekódovaného HTML obsahu submitt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a>Žádosti o ověření - prevence útoků skriptu
====================
> Tento dokument popisuje funkci žádosti o ověření technologie ASP.NET ve výchozím nastavení, kde aplikace je zabránit v zpracování nekódovaného obsah HTML odeslána na server. Tato žádost o ověření funkce lze zakázat, když aplikace byl navržen tak, aby bezpečně zpracování dat ve formátu HTML.
> 
> Platí pro technologii ASP.NET 1.1 a ASP.NET 2.0.


Ověření žádosti, funkce technologie ASP.NET od verze 1.1, zabrání serveru přijetí obsahu obsahující bez kódování HTML. Tato funkce je určena k zabránění některými útoky vložení skriptu, při kterém kód skriptu klienta nebo HTML můžete nechtěně odeslat na server, ukládat a poté jsou předloženy ostatním uživatelům. Stále důrazně doporučujeme, aby ověření všech vstupních dat a jeho v případě nutnosti kódování HTML.

Například můžete vytvořit webovou stránku, která požaduje e-mailovou adresu uživatele a potom úložiště, která e-mailovou adresu v databázi. Pokud uživatel zadá &lt;skriptu&gt;výstrahy ("hello ze skriptu")&lt;/SCRIPT&gt; místo platnou e-mailovou adresu, když se tato data, tento skript můžete provést Pokud obsah nebyl správně kódovaný. Funkce ověření žádosti technologie ASP.NET to zabrání situaci.

## <a name="why-this-feature-is-useful"></a>Proč se tato funkce je užitečná

Mnoho webů nejsou vědět, že jsou otevřené pro útok prostřednictvím injektáže jednoduché skriptu. Zda účel útoků těchto je autorskou webu zobrazením HTML nebo potenciálně spuštění skriptu klienta k lokalitě se hacker přesměrovat uživatele, jsou prostřednictvím injektáže skriptu k problému, který se musí soupeří vývojářům webů.

Útok prostřednictvím injektáže skriptu jsou zájmem všechny vývojářům webů, zda používají ASP.NET, ASP ani jiné technologie vývoj pro web.

Funkce ASP.NET žádost o ověření proaktivně zabraňuje tyto útoky tím, že nepovolí nekódovaného obsah HTML, které mají být zpracovány serverem, pokud vývojář se rozhodne povolit tento obsah.

## <a name="what-to-expect-error-page"></a>Co můžete očekávat: chybová stránka

Následující kopie obrazovky zobrazuje ukázkový kód ASP.NET:

![](request-validation/_static/image1.png)

Používá tento kód výsledky v jednoduchá stránka, která umožňuje zadat text do textového pole, klikněte na tlačítko a zobrazit text v ovládacím prvku popisek:

![](request-validation/_static/image2.png)

Ale byly JavaScript, jako například `<script>alert("hello!")</script>` zadali a odeslání by se nám získat výjimku:

![](request-validation/_static/image3.png)

Chybová zpráva uvádí, že 'potenciálně nebezpečná Request.Form byla zjištěna hodnota a poskytuje další podrobnosti v popisu, přesně co se stalo a jak můžete změnit chování. Příklad:

Ověření žádosti zjistil vstupní hodnoty potenciálně nebezpečná klienta a zpracování žádosti bylo přerušeno. Tato hodnota může znamenat pokus o ohrožení zabezpečení vaší aplikace, jako je například útoku skriptování webů. Zakážete ověření požadavku nastavením `validateRequest=false` v direktivě stránky nebo v konfiguračním oddílu. Ale je důrazně doporučujeme, aby vaše aplikace explicitně zkontrolovala všechny vstupní hodnoty v tomto případě.

## <a name="disabling-request-validation-on-a-page"></a>Zakázání ověření požadavku na stránce.

Zakážete ověření požadavku na stránce, musíte nastavit `validateRequest` direktivy stránky k `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Pokud je ověření žádosti je zakázáno, obsah lze odesílat na stránku. je zodpovědností vývojář stránky zajistit, že obsah je správně kódovaný nebo zpracovat.

## <a name="disabling-request-validation-for-your-application"></a>Zakázání ověření žádosti pro vaši aplikaci

Zakázat ověření žádosti pro vaši aplikaci, musíte upravit nebo vytvořit soubor Web.config pro vaši aplikaci a nastavit atribut validateRequest `<pages />` části k `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Pokud chcete zakázat ověření žádosti pro všechny aplikace na vašem serveru, můžete provést tato úprava k souboru Machine.config.

> [!CAUTION]
> Pokud je ověření žádosti je zakázáno, obsah lze odesílat do aplikace; je zodpovědností vývojář aplikace zajistit, že obsah je správně kódovaný nebo zpracovat.

Následující kód je upravit tak, aby vypnout ověření žádosti:

![](request-validation/_static/image4.png)

Nyní, pokud byl zadán následující JavaScript do textového pole `<script>alert("hello!")</script>` výsledkem bude:

![](request-validation/_static/image5.png)

K tomu nedocházelo, s ověření žádosti vypnutý, budeme potřebovat do formátu HTML kódování obsahu.

## <a name="how-to-html-encode-content"></a>Jak do formátu HTML kódování obsahu

Pokud je zakázána ověření žádosti, je dobrým zvykem kódování HTML obsah, který se uloží pro budoucí použití. Kódování HTML bude automaticky nahradit libovolný '&lt;'nebo'&gt;' (spolu s několik dalších symboly) s jejich odpovídající HTML kódovaný reprezentace. Například '&lt;"nahrazuje podle"&amp;lt;' a '&gt;"nahrazuje podle"&amp;gt;'. Tyto speciální kódy v prohlížečích se používá k zobrazení '&lt;"nebo"&gt;se v prohlížeči.

Obsah lze snadno kódovaný jazykem HTML na serveru pomocí `Server.HtmlEncode(string)` rozhraní API. Obsah může být také snadno HTML-dekódovat, který je vráceno zpět na standardní HTML pomocí `Server.HtmlDecode(string)` metoda.

![](request-validation/_static/image6.png)

Výsledkem je:

![](request-validation/_static/image7.png)
