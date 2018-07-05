---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Použití ovládací prvek editoru HTML (C#) | Dokumentace Microsoftu
author: microsoft
description: HTMLEditor je ovládací prvek ASP.NET AJAX, který umožňuje snadno vytvářet a upravovat obsah ve formátu HTML pomocí tlačítek na panelu nástrojů.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ef8fba82a77ed570c800dce0b1c1378fd36ea56
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397616"
---
<a name="how-do-i-use-the-html-editor-control-c"></a>Použití ovládací prvek editoru HTML (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> HTMLEditor je ovládací prvek ASP.NET AJAX, který umožňuje snadno vytvářet a upravovat obsah ve formátu HTML pomocí tlačítek na panelu nástrojů.


Cílem tohoto kurzu je poskytnout přehled o ovládací prvek editoru HTML, který je součástí sadou nástrojů AJAX Control Toolkit. HTML Editor obsahuje možnosti pro změnu velikosti písma, výběru písma, změna barvy pozadí, úprava barvu popředí přidávat odkazy, obrázky, přidání, Změna zarovnání textu a provádění vyjmutí, kopírování a vložení operace (viz obrázek 1).


[![HTML Editor](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Obrázek 01**: editoru HTML ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image2.png))


HTML editor umožňuje zadat obsah pomocí režimu návrhu nebo HTML můžete zadat přímo. Také k dispozici máte možnost zobrazit náhled vašeho obsahu HTML (viz obrázek 2).


[![Návrh, HTML a ve verzi Preview tlačítka](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Obrázek 02**: tlačítka návrhu, HTML a ve verzi Preview ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image4.png))


V tomto kurzu se dozvíte, jak zobrazení editoru HTML, přizpůsobení tlačítek na panelu nástrojů, který se zobrazí v editoru jazyka HTML a jak se vyhnout, skriptování mezi weby útoků.

## <a name="displaying-the-html-editor"></a>Zobrazování editoru HTML

Před použitím editoru jazyka HTML na stránce ASP.NET, musíte nejprve přidat ovládací prvek ScriptManager na stránku. Ovládací prvek ScriptManager se nachází pod na kartě Rozšíření AJAX v sadě nástrojů Visual Studio nebo Visual Web Developer Express.

V horní části stránky před všechny ovládací prvky na stránce by měl umístit ovládací prvek ScriptManager. Například je možné je umístit bezprostředně pod levou serverové &lt;formuláře&gt; značky.

Ovládací prvek editoru HTML se nachází na panelu nástrojů se zbytkem ovládacích prvků AJAX Control Toolkit. Je název ovládací prvek editoru (viz obrázek 3).


[![Ovládací prvek editoru HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Obrázek 03**: ovládací prvek editoru HTML ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image6.png))


Po přetažení editoru HTML na stránce můžete nastavit jeho vlastnosti v seznamu vlastností. Například chcete obvykle nastavení vlastností šířky a výšky. Výpis 1 obsahuje zdroj, který obsahuje HTML editor stránky ASP.NET.

**Výpis 1 - SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

Na stránce v informacích 1 obsahuje ovládací prvek editoru HTML, ovládací prvek Button a prvku Literal control. Když kliknete na tlačítko, obsah editoru HTML se zobrazí v ovládacím prvku Literal control (viz obrázek 4).


[![Odeslání formuláře pomocí editoru HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Obrázek 04**: odeslání formuláře pomocí editoru HTML ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image8.png))


HTML Editor obsah vlastnosti slouží k načtení HTML obsah, zadaný do editoru jazyka HTML. Mějte na paměti, že tento obsah ve formátu HTML může obsahovat jazyk JavaScript. V další části si popíšeme, jak můžete zabránit, útoky prostřednictvím injektáže skriptu JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Přizpůsobení panelu nástrojů editoru HTML

Můžete přizpůsobit přesně tlačítek, která se zobrazí v editoru. Například můžete chtít odebrat kartu HTML uživatelům zabránit v editoru HTML přepnutí do režimu HTML. Nebo můžete chtít odebrat rozevíracího seznamu velikost písma zabránit uživatelům ve vytváření příliš velký text ve fóru po zprávy (viz obrázek 5).


[![Vlastní Editor HTML](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Obrázek 05**: A přizpůsobit HTML Editor ([kliknutím ji zobrazíte obrázek v plné velikosti](how-do-i-use-the-html-editor-control-cs/_static/image10.png))


Nový Editor HTML odvozené ze základní třídy Editor přizpůsobíte tlačítka panelu nástrojů. Například vlastní editor ve výpisu 2 obsahuje pouze tlačítka panelu nástrojů pro tučné písmo a kurzíva. Byly odebrány všechny další tlačítka panelu nástrojů. Kromě toho Karta HTML se odebral z dolního okraje editoru (ale karty návrh a Preview stále existují).

**Výpis 2 - App\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Třídy v informacích 2 musíte přidat do vaší aplikace\_kódu složku tak, aby se automaticky zkompiluje třídy. Pokud aplikace\_složky s kódem na vašem webu neexistuje. potom můžete jednoduše přidat složku.

Po vytvoření vlastního editoru přidáte ho na stránku ASP.NET stejně jako při přidávání normální editoru HTML (viz seznam 3).

**Výpis 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Nejkonkrétnější – vyhněte útoky skriptování napříč weby (XSS)

Pokaždé, když se přijímají vstup od uživatele a znovu tento vstup na vašem webu, potenciálně otevřete webovou stránku s útoky skriptování napříč weby (XSS). Teoreticky může kyberzločinci odeslání kódu jazyka JavaScript, která se provede při vstupu se zobrazí znovu. JavaScript může používá ke krádeži uživatelských hesel a dalších citlivých údajů.

Za normálních okolností zhatila útoky XSS kódování libovolné vstup načíst od uživatele před jejich zobrazením na webové stránce HTML. Však nebude pouze použije kódování HTML. kódování výstupu HTML Editor &lt;skript&gt; značky, by také kódují všechny značky HTML. Jinými slovy přišli byste o všechny formátování, jako je například písmo, velikost písma a barvy pozadí.

Pokud shromažďujete citlivé informace od uživatelů – třeba hesla, čísla platebních karet a čísla sociálního pojištění - by neměl zobrazit zrušení kódovaného obsahu, která se načítají z uživatele pomocí editoru jazyka HTML. HTML Editor používejte jenom v situacích, ve kterých nejsou opětovné zobrazení HTML obsah nebo obsah HTML, který se právě odesílá na váš web důvěryhodná strana.

Představte si například, že jsou tvorbou blogovací aplikace. V takovém případě je vhodné použít HTML Editor při vytváření blogové příspěvky. Jsou pouze jeden, který odešle blogový příspěvek a pravděpodobně sami nechcete odeslat škodlivý JavaScript můžete důvěřovat. Nicméně to nemá smysl použití editoru HTML při povolování publikovat komentáře anonymním uživatelům. Měli byste být opatrní hlavně v situacích, ve kterých uživatelé odeslat citlivé informace, jako jsou hesla. Potenciálně uživatel se zlými úmysly může zadat komentář, který obsahuje správný jazyka JavaScript pro krádež hesla.

## <a name="summary"></a>Souhrn

V tomto kurzu byly poskytnuty s stručný přehled o ovládací prvek editoru HTML, který je součástí sadou nástrojů AJAX Control Toolkit. Jste zjistili, jak pomocí editoru jazyka HTML přijmout formátovaného obsahu od uživatele a odeslání obsahu na server. Také jsme zmínili, jak můžete přizpůsobit tlačítka panelu nástrojů, která se zobrazí v editoru jazyka HTML. Nakonec jste zjistili, jak se vyhnout při použití editoru HTML tak, aby přijímal potenciálně škodlivý vstup skriptování napříč weby útoků.

> [!div class="step-by-step"]
> [Next](how-do-i-use-the-html-editor-control-vb.md)
