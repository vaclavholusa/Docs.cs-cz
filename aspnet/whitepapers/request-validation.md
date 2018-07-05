---
uid: whitepapers/request-validation
title: Ověření požadavku – obrana před Skriptovými útoky | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje žádosti o ověření funkce technologie ASP.NET, pokud ve výchozím nastavení, aplikace nebude zpracování nekódovaného submitt obsahu HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 783fb1ae27d88f9c6d6d3484d26d3e206e7f2fba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372926"
---
<a name="request-validation---preventing-script-attacks"></a>Ověření požadavku – obrana před Skriptovými útoky
====================
> Tento dokument popisuje žádosti o ověření funkce technologie ASP.NET, pokud ve výchozím nastavení, aplikace nebude zpracování nešifrovaného obsahu HTML odeslat na server. Tato žádost o ověření funkce lze zakázat, když aplikace byla navržena tak, aby bezpečně zpracovávat data ve formátu HTML.
> 
> Platí pro technologii ASP.NET 1.1 a ASP.NET 2.0.


Ověření požadavku, která je součástí technologie ASP.NET od verze 1.1, zabrání serveru přijetí obsahu obsahující bez kódování HTML. Tato funkce je určena k zabránění útoků prostřednictvím injektáže skriptu kterým kód skriptu klienta nebo HTML lze neúmyslně odeslané na server, ukládat a předloží ostatním uživatelům. Stále důrazně doporučujeme, že ověření všech vstupních dat a to v případě potřeby použije kódování HTML.

Například můžete vytvořit webovou stránku, která požádá uživatele e-mailovou adresu a potom úložiště, která e-mailovou adresu v databázi. Pokud uživatel zadá &lt;skript&gt;upozornění ("hello ze skriptu")&lt;/SCRIPT&gt; místo platné e-mailové adresy, když se tato data, tento skript může provést Pokud obsah není správně kódovaný. Žádosti o ověření funkce technologie ASP.NET to zabraňuje děje.

## <a name="why-this-feature-is-useful"></a>Proč se tato funkce je užitečná

Mnoho serverů nejsou vědomi, že jsou otevřeny útocích prostřednictvím injektáže jednoduchý skript. Zda účelem tyto útoky je autorskou webu zobrazením HTML nebo potenciálně spusťte skript klienta přesměrovat uživatele na web se hacker, útoky prostřednictvím injektáže skriptu jsou problém, který vývojářům webů musí potýkat s.

Útoků prostřednictvím injektáže skriptu jsou obavy z všechny webové vývojáře, zda používají technologie ASP.NET, ASP nebo jiným vývojovým technologiím, web.

Žádosti o ověření funkce ASP.NET proaktivně zabraňuje těmto útokům díky neumožňuje nekódovaného obsah ve formátu HTML ke zpracování serverem, pokud vývojář rozhodne povolit tento obsah.

## <a name="what-to-expect-error-page"></a>Co můžete očekávat: chybovou stránku

Níže uvedeném snímku obrazovky ukazuje některé ukázkový kód ASP.NET:

![](request-validation/_static/image1.png)

Tento kód výsledky používané jednoduchá stránka, která umožňuje zadejte nějaký text do textového pole, klikněte na tlačítko a zobrazení textu v ovládacím prvku popisek:

![](request-validation/_static/image2.png)

Však byly JavaScriptu, jako například `<script>alert("hello!")</script>` zadali a odeslání, dostali bychom výjimku:

![](request-validation/_static/image3.png)

Chybová zpráva uvádí, že "potenciálně nebezpečné Request.Form byla zjištěna hodnota' a poskytuje další informace naleznete v popisu přesně co se stalo a jak změnit chování. Příklad:

Ověření požadavku byla zjištěna potenciálně nebezpečná klienta vstupní hodnota a zpracování žádosti bylo přerušeno. Tato hodnota může znamenat pokus o narušení zabezpečení aplikace, například s útoky skriptování napříč weby. Zakážete ověření požadavku tak, že nastavíte `validateRequest=false` v direktivě stránky nebo v konfiguračním oddílu. Je však důrazně doporučujeme, aby vaše aplikace explicitně zkontrolovala všechny vstupy v tomto případě.

## <a name="disabling-request-validation-on-a-page"></a>Zakázání ověření požadavku na stránce.

Zakázání ověření požadavku na stránce, je nutné nastavit `validateRequest` atribut direktivy stránky k `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Při žádosti o ověření je zakázané, můžete odeslat obsah na stránku. je odpovědnost vývojáře stránky je zajistit, že obsah je správně kódovaný nebo zpracovány.

## <a name="disabling-request-validation-for-your-application"></a>Zakázání ověření žádosti pro vaši aplikaci

Zakázání ověření žádosti pro vaši aplikaci, musíte upravit nebo vytvořit soubor Web.config pro vaši aplikaci a nastavte atribut validateRequest `<pages />` části `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Pokud chcete zakázat ověření žádosti pro všechny aplikace na serveru, můžete provést tuto změny souboru Machine.config.

> [!CAUTION]
> Při žádosti o ověření je zakázané, můžete obsah odeslat do aplikace; je starosti vývojář aplikace, aby tento obsah je správně kódovaný nebo zpracovány.

Následující kód je upravit tak, aby vypnout ověření žádosti:

![](request-validation/_static/image4.png)

Když teď následující JavaScript byla zadán do textového pole `<script>alert("hello!")</script>` výsledek by byl:

![](request-validation/_static/image5.png)

K tomu nedocházelo, s ověřením požadavku vypnuté, budeme potřebovat do formátu HTML kódování obsahu.

## <a name="how-to-html-encode-content"></a>Jak do formátu HTML kódování obsahu

Pokud jste zakázali žádost o ověření, je vhodné pro obsah s kódováním HTML, který se uloží pro budoucí použití. Kódování HTML se automaticky nahradit libovolný "&lt;'nebo'&gt;" (společně s několika dalších symbolů) s jejich odpovídající kód HTML kódovaný reprezentace. Například "&lt;"nahrazuje"&amp;lt;' a '&gt;"nahrazuje"&amp;gt;". Tyto speciální kódy zobrazíte pomocí prohlížeče "&lt;'nebo'&gt;" v prohlížeči.

Obsah je možné snadno kódovaný jazykem HTML na serveru pomocí `Server.HtmlEncode(string)` rozhraní API. Obsah může být také snadno HTML-dekódovat, to znamená, nastavení bylo vráceno zpět na standardní HTML pomocí `Server.HtmlDecode(string)` metody.

![](request-validation/_static/image6.png)

Výsledek:

![](request-validation/_static/image7.png)
