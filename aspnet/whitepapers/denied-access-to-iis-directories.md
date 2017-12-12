---
uid: whitepapers/denied-access-to-iis-directories
title: "ASP.NET odepřen přístup do adresáře služby IIS | Microsoft Docs"
author: rick-anderson
description: "Tento dokument White Paper popisuje, co musíte udělat Pokud požadavek na vaši aplikaci ASP.NET vrátí chybu, \"byl odepřen přístup do adresáře DirectoryName. Nepodařilo se s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a>ASP.NET odepřen přístup do adresáře služby IIS
====================
> Tento dokument White Paper popisuje, co musíte udělat Pokud požadavek na vaši aplikaci ASP.NET vrátí chybu, "odepření přístupu k *DirectoryName* adresáře. Nepodařilo se spustit sledování directory chaanges."
> 
> Platí pro ASP.NET 1.0 a 1.1 technologie ASP.NET.


ASP.NET V1 RTM se teď bude spouštět pomocí méně privilegovaného účtu systému windows - registrován jako "ASPNET" účet v místním počítači.

U některých uzamčené systémy tento účet může ve výchozím nastavení mají přístup pro čtení zabezpečení adresáře s obsahem webu, kořenovém adresáři aplikace nebo kořenový adresář webu. V takovém případě zobrazí chybová zpráva požadavku stránky od dané webové aplikace:

![](denied-access-to-iis-directories/_static/image1.jpg)

Tento problém lze vyřešit bude muset změnit oprávnění zabezpečení u příslušné adresáře.

Konkrétně technologie ASP.NET vyžaduje čtení, spouštění a přístup k účtu ASPNET kořenový adresář webového serveru (například: c:\inetpub\wwwroot nebo libovolného adresáře alternativní lokality možná jste nakonfigurovali ve službě IIS), adresář s obsahem a kořenového adresáře aplikace Chcete-li sledování změn konfigurace. Kořenový adresář aplikace odpovídá cesta ke složce spojené s virtuální adresář aplikace v nástroji pro správu služby IIS (inetmgr).

Zvažte například následující hierarchie aplikace ve složce wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

V tomto příkladu musí účtu ASPNET oprávnění ke čtení definovaný nad obsahu Moje aplikace a adresář wwwroot. Jeden zděděné ACL v kořenové složce Volitelně lze také pro oba adresáře Pokud jste se vnořený.

Chcete-li přidat oprávnění k adresáři, proveďte následující kroky:

- Pomocí Průzkumníka Windows přejděte do adresáře
- Klikněte pravým tlačítkem na složku, adresář a vyberte možnost "Vlastnosti"
- Přejděte na kartu "Zabezpečení" v dialogovém okně Vlastnosti
- Kliknutím na tlačítko "Přidat" a zadejte název počítače, za nímž následuje název účtu ASPNET. Například by na počítači s názvem "webdev", zadejte webdev\ASPNET a stiskněte tlačítko "OK".
- Zkontrolujte, zda má účet ASPNET "pro čtení &amp; Execute", "Zobrazovat obsah složky" a "Číst" políček zaškrtnutí.
- Klikněte na OK zavřete toto dialogové okno a uložte změny.

![](denied-access-to-iis-directories/_static/image2.jpg)

V případě potřeby tyto změny je možné automatizovat pomocí skriptů nebo nástroj "cacls.exe", který se dodává s Windows. Další informace o účtu ASPNET najdete [nejčastější dotazy k dokumentu](https://go.microsoft.com/fwlink/?LinkId=5828).

Pokud dané webové aplikace spoléhá na nutnosti zápisu nebo změnit oprávnění k určité složce nebo souboru, mohou být udělena podle stejného postupu a kontrola políček "Zápisu" nebo "Upravit".

Na počítačích, které umožňují Everyone nebo uživatelům přístup pro čtení skupiny k tyto adresáře (což je výchozí konfigurace), bude došlo k žádné problémy a výše uvedené kroky se nevyžaduje.
