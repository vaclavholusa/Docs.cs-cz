---
uid: visual-studio/overview/2013/using-browser-link
title: Použití funkce Browser Link v sadě Visual Studio 2013 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 9da93c279bfa2af614733e3234ba62429abf688a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822192"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Použití funkce Browser Link v sadě Visual Studio 2013
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Browser Link je nová funkce v sadě Visual Studio 2013, která vytvoří komunikační kanál mezi vývojové prostředí a jeden nebo více webových prohlížečů. Můžete použít odkaz prohlížeče pro aktualizaci webové aplikace v několika prohlížečích najednou, což je užitečné pro testování prohlížečů.

- [Prohlížeč-obnovit](#browser-refresh)
- [Zobrazení řídicím panelu odkazů prohlížeče](#dashboard)
- [Povolení Browser Link pro soubory statický kód HTML](#static-html)
- [Zakázání Browser Link](#disabling)
- [Jak to funguje?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Prohlížeč-obnovit

S aktualizovat prohlížeč můžete aktualizovat více prohlížečů, které jsou připojené ke službě Visual Studio prostřednictvím odkazů prohlížeče.

Pokud chcete použít, aktualizujte prohlížeč, nejprve vytvořte aplikaci ASP.NET pomocí kteréhokoli z šablony projektu. Ladění aplikace pomocí klávesy F5 nebo kliknutím na ikonu šipky v panelu nástrojů:

![](using-browser-link/_static/image1.png)

Můžete také použijete rozevírací seznam pro výběr určitého webového prohlížeče pro ladění.

![](using-browser-link/_static/image2.png)

Chcete-li ladit s více prohlížečů, vyberte **procházet s**. V **procházet s** dialogového okna, podržte stisknutou klávesu CTRL k výběru více než jeden prohlížeč. Klikněte na tlačítko **Procházet** ladění vybrané prohlížeče. Browser Link funguje také v případě spusťte prohlížeč mimo aplikaci Visual Studio a přejděte na adresu URL aplikace.

![](using-browser-link/_static/image3.png)

Browser Link ovládací prvky jsou umístěny v rozevírací nabídce se na ikonu Kruhové šipky. Ikona šipky je **aktualizovat** tlačítko.

![](using-browser-link/_static/image4.png)

Pokud chcete zobrazit, které prohlížeče jsou připojené, najeďte myší **aktualizovat** tlačítko během ladění. Propojených prohlížečů jsou zobrazeny v okně popisek.

![](using-browser-link/_static/image5.png)

Chcete-li aktualizovat propojené prohlížeče, klikněte na tlačítko **aktualizovat** tlačítko nebo stiskněte kombinaci kláves CTRL + ALT + ENTER. Například následující snímek obrazovky ukazuje projekt ASP.NET, který jsem vytvořil pomocí projektu šablony MVC 5. Můžete zobrazit aplikace spuštěná v prohlížečů v horní části. V dolní části je projekt otevřít v sadě Visual Studio.

![](using-browser-link/_static/image6.png)

V sadě Visual Studio po změně &lt;h1&gt; nadpisu na domovské stránce:

![](using-browser-link/_static/image7.png)

Když jsem kliknul / a **aktualizovat** tlačítko, změny se objevil v obě okna prohlížeče:

![](using-browser-link/_static/image8.png)

**Poznámky**

- Chcete-li povolit Browser Link, nastavte `debug=true` v [ &lt;kompilace&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element v souboru Web.config v projektu.
- Aplikace musí být spuštěná v místním hostiteli.
- Aplikace musí cílit na .NET 4.0 nebo novější.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Zobrazení řídicím panelu odkazů prohlížeče

Browser Link řídicí panel s informacemi o připojení odkazů prohlížeče. Chcete-li zobrazit řídicí panel, vyberte rozevírací nabídce Browser Link (na malou šipku vedle položky **aktualizovat** tlačítko). Pak klikněte na tlačítko **řídicím odkazů prohlížeče**.

![](using-browser-link/_static/image9.png)

Řídicí panel obsahuje seznam propojených prohlížečů a adresu URL, na kterou má přešli každým prohlížečem.

![](using-browser-link/_static/image10.png)

**Požadavky** část ukazuje všechny kroky potřebnými k povolení Browser Link pro daný projekt. Například následující snímek obrazovky ukazuje projektu, kde "debug" je nastavena na hodnotu false v souboru Web.config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Povolení Browser Link pro soubory statický kód HTML

Chcete-li povolit Browser Link pro statické soubory HTML, přidejte do souboru Web.config následující.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Z důvodů výkonu odeberte toto nastavení při publikování projektu.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Zakázání Browser Link

Browser Link je standardně povolená. Existuje několik způsobů, jak zakázat:

- V rozevírací nabídce Browser Link, zrušte zaškrtnutí políčka **Povolit odkaz prohlížeče**. 

    ![](using-browser-link/_static/image12.png)
- V souboru Web.config přidejte klíč s názvem "vs: EnableBrowserLink" s hodnotou "false" v sekci appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- V souboru Web.config nastavte ladění na hodnotu false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Jak to funguje?

Browser Link používá [SignalR](../../../signalr/index.md) vytvořit komunikační kanál mezi Visual Studio a prohlížečem. Když je povolené Browser Link, Visual Studio funguje jako více klientů (prohlížečů) můžete připojit k serveru funkce SignalR. Browser Link také zaregistruje modul HTTP pomocí technologie ASP.NET. Tento modul Vloží speciální &lt;skript&gt; odkazy do každého požadavku stránky ze serveru. Zobrazí se odkazy na skript tak, že vyberete "Zdroj zobrazení" v prohlížeči.

![](using-browser-link/_static/image13.png)

Zdrojové soubory se nemění. Modul HTTP dynamicky vkládá skriptových odkazů.

Protože kód prohlížeče straně je všechny JavaScript, funguje ve všech prohlížečích, které [podporuje SignalR](../../../signalr/overview/getting-started/supported-platforms.md), bez nutnosti jakékoli modul plug-in prohlížeče.
