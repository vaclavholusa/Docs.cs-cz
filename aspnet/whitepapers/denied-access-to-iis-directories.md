---
uid: whitepapers/denied-access-to-iis-directories
title: Aplikace ASP.NET zamítla přístup k adresářům služby IIS | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument White Paper popisuje, co musíte udělat, pokud požadavek na vaše aplikace ASP.NET vrátí chyba "přístup byl odepřen do DirectoryName. Nepovedlo se s...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 4853ee29d2468c4b67375123c5b2ec15089fe09b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842407"
---
<a name="aspnet-denied-access-to-iis-directories"></a>Aplikace ASP.NET zamítla přístup k adresářům služby IIS
====================
> Tento dokument White Paper popisuje, co musíte udělat, pokud požadavek na vaše aplikace ASP.NET vrátí chybu, "odepření přístupu k *NazevAdresare* adresáře. Nepovedlo se spustit sledování změn v adresáři."
> 
> Platí pro technologii ASP.NET 1.0 a ASP.NET 1.1.


Technologie ASP.NET verze 1 RTM se teď bude spouštět pomocí méně privilegovaného účtu systému windows - zaregistrovaný jako "ASPNET" účet na místním počítači.

U některých systémů uzamčen tento účet může ve výchozím nastavení přečetl(a) security umožněn přístup k obsahu adresáře webu, kořenový adresář aplikace nebo kořenový adresář webu. V tomto případě zobrazí následující chyba požadavku stránky z dané webové aplikace:

![](denied-access-to-iis-directories/_static/image1.jpg)

Chcete-li tento problém vyřešit, je potřeba měnit oprávnění zabezpečení pro příslušné adresáře.

Konkrétně technologie ASP.NET vyžaduje čtení, spouštění a zápis k účtu ASPNET kořenový adresář webového serveru (například: c:\inetpub\wwwroot nebo jakéhokoli adresáře alternativní lokality možná jste nakonfigurovali ve službě IIS), adresář s obsahem a kořenový adresář aplikace aby bylo možné sledovat změny v konfiguraci souboru. Kořenový adresář aplikace odpovídá cesta ke složce, které jsou spojené s virtuálním adresářem aplikace v nástroji pro správu služby IIS (inetmgr).

Představte si třeba následující hierarchie aplikace pod složku wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

V tomto příkladu musí účtu ASPNET oprávnění ke čtení, které jsou definované výše pro obsah Moje aplikace a adresář wwwroot. Jeden zděděné seznamu ACL pro kořenovou složku Volitelně lze také pro oba adresáře v případě, že jsou vnořené.

Chcete-li přidat oprávnění k adresáři, proveďte následující kroky:

- Pomocí Průzkumníka Windows přejděte do adresáře
- Klikněte pravým tlačítkem na složku adresář a zvolte možnost "Properties"
- Přejděte na kartu "Zabezpečení" v dialogovém okně Vlastnosti
- Klikněte na tlačítko "Přidat" a zadejte název počítače, za nímž následuje název účtu ASPNET. Například by na počítači s názvem "webdev", zadejte webdev\ASPNET a klikněte na "OK".
- Zkontrolujte, zda má účet ASPNET "čtení &amp; Execute", "Zobrazovat obsah složky" a "Číst" zaškrtnutých políček.
- Klikněte na OK a zavřete toto dialogové okno a uložte změny.

![](denied-access-to-iis-directories/_static/image2.jpg)

V případě potřeby je možné tyto změny automatizovat pomocí skriptů nebo "cacls.exe" nástroj, který se dodává s Windows. Další informace o účtu ASPNET, najdete v tématu [nejčastější dotazy k dokumentu](https://go.microsoft.com/fwlink/?LinkId=5828).

Pokud dané webové aplikace spoléhá na zápis nebo upravit oprávnění do konkrétní složky nebo souboru, to může udělit pomocí stejného postupu a kontrola zaškrtávací políčka "Write" nebo "Upravit".

Na počítačích, které umožňují všem uživatelům nebo přístup ke čtení skupiny uživatelů pro tyto adresáře (to je výchozí konfigurace), se žádné problémy setkáte a proveďte následující kroky se nevyžaduje.
