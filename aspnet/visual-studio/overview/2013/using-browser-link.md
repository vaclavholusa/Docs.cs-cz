---
uid: visual-studio/overview/2013/using-browser-link
title: "Pomocí odkazů prohlížeče v sadě Visual Studio 2013 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 14f67d81a5b460da591b8fb27fedf53d228e7717
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-browser-link-in-visual-studio-2013"></a>Pomocí odkazů prohlížeče v sadě Visual Studio 2013
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Browser Link je nová funkce v sadě Visual Studio 2013 vytvářející komunikační kanál mezi vývojového prostředí a jeden nebo více webových prohlížečů. Můžete použít Browser Link aktualizujte svoji webovou aplikaci v několika prohlížeče najednou, což je užitečné pro testování různých prohlížečích.

- [Obnovit v prohlížeči](#browser-refresh)
- [Zobrazení řídicím panelu odkazů prohlížeče](#dashboard)
- [Povolení odkazů prohlížeče pro soubory statické HTML](#static-html)
- [Zakázání Browser Link](#disabling)
- [Jak to funguje?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Obnovit v prohlížeči

Pomocí prohlížeče aktualizovat můžete obnovit více prohlížečů, které jsou připojené k sadě Visual Studio prostřednictvím odkazů prohlížeče.

Pokud chcete použít, aktualizujte prohlížeč, nejprve vytvořte aplikace ASP.NET pomocí kteréhokoli z šablon projektu. Ladění aplikace stisknutím klávesy F5 nebo kliknutím na ikonu šipky v panelu nástrojů:

![](using-browser-link/_static/image1.png)

Můžete taky rozevíracího seznamu vyberte konkrétní prohlížeč pro ladění.

![](using-browser-link/_static/image2.png)

Chcete-li ladit s více prohlížečů, vyberte **procházet s**. V **procházet s** dialogové okno, podržíte stisknutou klávesu CTRL k výběru více než jeden prohlížeč. Klikněte na tlačítko **Procházet** k ladění u vybrané prohlížečů. Browser Link lze použít také v případě spusťte prohlížeč mimo aplikaci Visual Studio a přejděte na adresu URL aplikace.

![](using-browser-link/_static/image3.png)

Ovládací prvky Browser Link jsou umístěny v rozevírací nabídce s ikonou cyklické šipku. Je na ikonu šipky **aktualizovat** tlačítko.

![](using-browser-link/_static/image4.png)

Pokud chcete zjistit, které prohlížeče jsou připojeny, najeďte myší **aktualizovat** tlačítko při ladění. Připojené prohlížeče se zobrazí v okně popisu.

![](using-browser-link/_static/image5.png)

Chcete-li aktualizovat připojené prohlížeče, klikněte na tlačítko **aktualizovat** tlačítko nebo stiskněte klávesu CTRL + ALT + ENTER. Například následující snímek obrazovky ukazuje projekt ASP.NET, který byl vytvořen pomocí šablony projektu MVC 5. Zobrazí se aplikace spuštěné v prohlížečích dvě v horní části. V dolní části je projekt otevřete v sadě Visual Studio.

![](using-browser-link/_static/image6.png)

V sadě Visual Studio, bylo změněno &lt;h1&gt; záhlaví pro domovskou stránku:

![](using-browser-link/_static/image7.png)

Když po klepnutí **aktualizovat** tlačítko, změna se objevil v systémech windows prohlížeče:

![](using-browser-link/_static/image8.png)

**Poznámky**

- Chcete-li povolit Browser Link, nastavte `debug=true` v [ &lt;kompilace&gt; ](https://msdn.microsoft.com/en-us/library/s10awwz0(v=vs.85).aspx) element v souboru Web.config projektu.
- Aplikace musí být spuštěn na místním hostiteli.
- Aplikace musí mít jako cíl rozhraní .NET 4.0 nebo novější.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Zobrazení řídicím panelu odkazů prohlížeče

Řídicím panelu odkazů prohlížeče zobrazují informace o připojení odkazů prohlížeče. Chcete-li zobrazit řídicí panel, vyberte v rozevírací nabídce Browser Link (na malou šipku vedle položky **aktualizovat** tlačítko). Pak klikněte na tlačítko **řídicím odkazů prohlížeče**.

![](using-browser-link/_static/image9.png)

Řídicí panel uvádí připojené prohlížeči a adresu URL, na kterou má přešli každým prohlížečem.

![](using-browser-link/_static/image10.png)

**Požadavky** části se zobrazují všechny kroky potřebnými k povolení Browser Link pro tento projekt. Například následující snímek obrazovky ukazuje projektu, kde "ladění" je nastaven na hodnotu false v souboru Web.config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Povolení odkazů prohlížeče pro soubory statické HTML

Chcete-li povolit Browser Link pro statické soubory HTML, přidejte následující do souboru Web.config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Z důvodů výkonu odeberte toto nastavení při publikování projektu.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Zakázání Browser Link

Browser Link je ve výchozím nastavení povolené. Zakázat několika způsoby:

- V rozevírací nabídce Browser Link, zrušte zaškrtnutí políčka **povolit Browser Link**. 

    ![](using-browser-link/_static/image12.png)
- V souboru Web.config přidejte klíč s názvem "vs: EnableBrowserLink" s hodnotou "false" v části appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- V souboru Web.config nastaven na hodnotu false ladění. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Jak to funguje?

Browser Link používá [SignalR](../../../signalr/index.md) vytvořit komunikační kanál mezi Visual Studio a prohlížeči. Pokud je povoleno Browser Link, Visual Studio funguje jako server SignalR, více klientů (prohlížeče) mohou připojit. Browser Link také zaregistruje modul HTTP s technologií ASP.NET. Tento modul Vloží speciální &lt;skriptu&gt; odkazy na každý požadavek na stránku ze serveru. Uvidíte odkazům na skript tak, že vyberete "Zdroj zobrazení" v prohlížeči.

![](using-browser-link/_static/image13.png)

Zdrojové soubory vašeho se nemění. Modul HTTP vloží odkazům na skript dynamicky.

Protože kód prohlížeče na straně je všechny JavaScript, funguje u všech prohlížečů, [SignalR podporuje](../../../signalr/overview/getting-started/supported-platforms.md), bez nutnosti jakékoli modul plug-in prohlížeče.
