---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Používání ovládacího prvku HTML Editor (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor je ovládací prvek ASP.NET AJAX, který umožňuje snadno vytvářet a upravovat obsah HTML pomocí tlačítek na panelu nástrojů.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4833949a54fa9ae12eaf7b596a5fe1ddfd1f7b7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>Používání ovládacího prvku HTML Editor (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> HTMLEditor je ovládací prvek ASP.NET AJAX, který umožňuje snadno vytvářet a upravovat obsah HTML pomocí tlačítek na panelu nástrojů.


Cílem tohoto kurzu je poskytnout Přehled ovládacího prvku HTML Editor součástí Toolkitu AJAX. HTML Editor obsahuje možnosti pro změnu velikosti písma, výběr písmo, změna barvy pozadí, úprava barvu popředí přidávání odkazů, přidávání obrázků, Změna zarovnání textu a provádění operací vyjmutí, kopírování a vložení operací (viz obrázek 1).


[![HTML Editor](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Obrázek 01**: editoru HTML ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


HTML editor můžete zadat obsah pomocí režimu návrhu nebo HTML můžete zadat přímo. Rovněž jsou k dispozici možnost zobrazte náhled vašeho obsahu HTML (viz obrázek 2).


[![Návrh, HTML a náhled tlačítka](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Obrázek 02**: návrh, HTML a náhled tlačítka ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


V tomto kurzu zjistíte, jak zobrazit HTML Editor, postup přizpůsobení tlačítka panelu nástrojů, který se zobrazí v editoru HTML a jak se vyhnout skriptování útoků webů.

## <a name="displaying-the-html-editor"></a>Zobrazování editoru HTML

Předtím, než můžete použít HTML Editor na stránce technologie ASP.NET, je nejprve nutno přidat ovládací prvek ScriptManager na stránku. Ovládací prvek ScriptManager se nachází pod kartě Rozšíření AJAX v sadě nástrojů Visual Studio nebo Visual Web Developer Express.

V horní části stránky před všechny ovládací prvky na stránce by měl umístěte ovládací prvek ScriptManager. Například můžete umístit ji okamžitě níže serverovou otevírání &lt;formuláře&gt; značky.

Ovládací prvek HTML Editor se nachází v sadě nástrojů se zbytkem prvky AJAX Control Toolkit. Je název ovládacího prvku Editor (viz obrázek 3).


[![Ovládací prvek HTML Editor](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Obrázek 03**: ovládací prvek editoru HTML ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


Po přetáhnete HTML Editor na stránce, můžete nastavit jeho vlastnosti v seznamu vlastností. Například chcete normálně nastavit vlastnosti Šířka a výška. Výpis 1 obsahuje daný zdroj pro stránku ASP.NET, který obsahuje HTML editor.

**Výpis 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

Stránka v výpis 1 obsahuje ovládací prvek HTML, ovládacího prvku tlačítko a prvku Literal control. Když kliknete na tlačítko, obsah HTML Editor se zobrazí v ovládacím prvku Literal control (viz obrázek 4).


[![Odeslání formuláře se Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Obrázek 04**: odeslání formuláře se Editor HTML ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


HTML Editor obsah vlastnost se používá k získání obsahu HTML zadali do editoru HTML. Upozorňujeme, že tento obsah HTML může obsahovat JavaScript. V další části probereme, jak můžete zabránit útokům vkládání JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Přizpůsobení panelu nástrojů editoru HTML

Můžete přizpůsobit přesně tlačítek, která se zobrazí v editoru. Například můžete chtít odebrat Karta HTML uživatelům zabránit v editoru HTML přepnout do režimu HTML. Nebo můžete chtít odebrat rozevíracího seznamu velikost písma uživatelům zabránit ve vytváření příliš velké textu ve fóru zprávy post (viz obrázek 5).


[![Vlastní Editor HTML](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Obrázek 05**: A přizpůsobit HTML Editor ([Kliknutím zobrazit obrázek v plné velikosti](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


Tlačítka panelu nástrojů můžete přizpůsobit odvozením nové Editor HTML ze základní třídy Editor. Například vlastní editor v výpis 2 obsahuje jenom tlačítka panelu nástrojů pro tučné písmo a kurzíva. Byly odebrány všechny další tlačítka panelu nástrojů. Kromě toho Karta HTML byla odebrána v dolní části editoru (ale karty návrh a Preview stále existují).

**Výpis 2 – aplikace\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

Třída výpis 2 musíte přidat do vaší aplikace\_Code složky tak, aby se automaticky zkompiluje třídy. Pokud aplikace\_kód složka neexistuje ve vašem webu potom můžete jednoduše přidat složku.

Po vytvoření vlastního editoru, můžete ho přidat do stránky ASP.NET stejným způsobem, jak přidat normální Editor HTML (viz seznam 3).

**Listing 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Vyhněte útoky skriptování (XSS)

Vždy, když přijímají vstup od uživatele a znovu zobrazit tento vstup na vašem webu, potenciálně otevřete webovou stránku webů Skriptování útoky. Teoreticky může kyberzločinci odeslat kód jazyka JavaScript, která se provede při vstupu se zobrazí znovu. Jazyk JavaScript může vám ukrást hesla uživatele a dalších citlivých údajů.

Za normálních okolností vůbec nemělo útoky XSS pomocí kódování ať vstup načíst od uživatele před jejich zobrazením na webové stránce HTML. Však nebude pouze kódování HTML kódování výstup HTML Editor &lt;skriptu&gt; značky, by ho také kódování všechny značky HTML. Jinými slovy by přijdete o veškeré formátování například typ písma, velikosti písma a barvy pozadí.

Pokud shromažďujete citlivé informace od uživatelů – třeba hesla, čísla platebních karet a čísel sociálního pojištění - nesmí zobrazit zrušení kódovaného obsah, který je načíst z uživatele s Editor HTML. HTML Editor používejte jenom v situacích, ve kterých nejsou opakované zobrazování obsah HTML nebo obsah HTML, který je odesílán na váš web důvěryhodná strana.

Představte si například, že vytváříte aplikace blogu. V takovém případě má smysl pro použití editoru HTML při sestavování příspěvcích na blogu. Jsou pouze jeden, který odešle příspěvku na blogu a pravděpodobně z důvodu, můžete důvěřovat sami nechcete odeslat škodlivý JavaScript. Nedává však smysl pro použití editoru HTML při povolení anonymní uživatelé odeslat komentáře. Byste měli být opatrní hlavně v situacích, ve kterých uživatelé odesílat citlivé informace, jako jsou hesla. Uživatel se zlými úmysly může potenciálně, post komentář, který obsahuje správné JavaScript pro krádež hesla.

## <a name="summary"></a>Souhrn

V tomto kurzu byly poskytnuty s stručný přehled ovládacího prvku HTML Editor součástí Toolkitu AJAX. Jste zjistili, jak pomocí editoru HTML přijmout bohaté obsah od uživatele a odeslání obsahu na server. Také popsané, jak můžete přizpůsobit tlačítka panelu nástrojů, která se zobrazí HTML Editor. Nakonec jste zjistili, jak se vyhnout skriptování útoků webů při použití editoru HTML tak, aby přijímal potenciálně škodlivý vstup.

> [!div class="step-by-step"]
> [Předchozí](how-do-i-use-the-html-editor-control-cs.md)
