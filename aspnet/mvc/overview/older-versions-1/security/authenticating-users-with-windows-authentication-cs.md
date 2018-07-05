---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: Ověřování uživatelů pomocí ověřování Windows (C#) | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak používat ověřování Windows v rámci aplikace MVC. Zjistíte, jak povolit ověřování Windows v rámci co webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 0261f3b989d721e6e9aae8fa5766e02c8bca7b63
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397801"
---
<a name="authenticating-users-with-windows-authentication-c"></a>Ověřování uživatelů pomocí ověřování Windows (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak používat ověřování Windows v rámci aplikace MVC. Zjistíte, jak povolit ověřování Windows v rámci souboru konfigurace webové aplikace a postup konfigurace ověřování pomocí služby IIS. Nakonec se dozvíte, jak použít atribut [Authorize] k omezení přístupu na akce kontroleru, zejména Windows uživatelům nebo skupinám.


Cílem tohoto kurzu je vysvětlují, jak můžete využít výhod zabezpečení integrované Internetová informační služba heslo chránit zobrazení v aplikacích MVC. Zjistíte, jak umožnit akce kontroleru má být volána pouze pomocí určitým uživatelům Windows nebo uživatele, kteří jsou členy určitých skupin Windows.

Ověřování Windows dává smysl, když vytváříte webu k interní firemní (intranetový server) a chcete, aby uživatelé mohli používat jejich standardní Windows uživatelská jména a hesla pro přístup k webu. Pokud vytváříte vně směřující webu (na webu Internet) zvažte místo toho použití ověřování pomocí formulářů.

#### <a name="enabling-windows-authentication"></a>Povolení ověřování Windows

Při vytváření nové aplikace ASP.NET MVC, není ve výchozím nastavení povolené ověřování Windows. Ověřování pomocí formulářů je výchozím typem ověřování povolené pro aplikace MVC. Je třeba povolit ověřování Windows tak, že upravíte soubor konfigurace (web.config) webové aplikace MVC. Najít &lt;ověřování&gt; oddílu a upravit ho na použití Windows namísto ověřování pomocí formulářů takto:

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

Když povolíte ověřování Windows, webový server je zodpovědný za účelem ověřování totožnosti uživatelů. Obvykle jsou dva různé typy webových serverů, které používáte při vytváření a nasazení aplikace ASP.NET MVC.

Nejprve při vývoji aplikace MVC, pomocí technologie ASP.NET vývojového webového serveru součástí sady Visual Studio. Ve výchozím nastavení spustí webový Server ASP.NET Development všechny stránky v kontextu aktuálního Windows účtu (kterou jste použili k přihlášení do Windows).

Webový Server ASP.NET Development podporuje také ověřování protokolem NTLM. Povolit ověřování NTLM tak, že pravým tlačítkem na název vašeho projektu v okně Průzkumník řešení a vyberte možnost Vlastnosti. V dalším kroku vyberte kartu Web a zaškrtněte políčko NTLM (viz obrázek 1).

**Obrázek 1 – povolení ověřování NTLM pro vývojový Server ASP.NET Web**

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

Pro provozní webové aplikace na stranu, použijte služby IIS jako webový server. Služba IIS podporuje několik typů ověřování, včetně:

- Základní ověřování – definované jako součást protokolu HTTP 1.0. Odešle uživatelská jména a hesla v nešifrovaném textu (základ64 kódovaný) přes Internet. -Ověřování algoritmem Digest – odešle hodnotu hash hesla, místo hesla, přes internet. – Integrované ověřování Windows (NTLM) – nejlepší typ ověřování pro použití v prostředí intranetu pomocí systému windows. -Certifikátu ověřování – umožňuje ověřování pomocí certifikátu na straně klienta. Certifikát se mapuje na uživatelský účet Windows.

> [!NOTE] 
> 
> Podrobnější přehled těchto různých typů ověřování, najdete v tématu [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Správce Internetové informační služby můžete povolit konkrétní typ ověřování. Mějte na paměti, že všechny typy ověřování nejsou k dispozici v případě každý operační systém. Kromě toho pokud používáte IIS 7.0 s Windows Vista, je potřeba povolit různé typy ověřování Windows, předtím, než se objeví v Správce Internetové informační služby. Otevřít **ovládací panely, programy, programy a funkce Windows zapnout nebo vypnout funkce**a rozbalte uzel Internetová informační služba (viz obrázek 2).

**Obrázek 2 – funkce povolení Windows služby IIS**

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

Pomocí Internetové informační služby, můžete povolit nebo zakázat různé typy ověřování. Obrázek 3 znázorňuje například zakázat anonymní ověřování a povolení ověření integrované Windows (NTLM) při použití služby IIS 7.0.

**Obrázek 3: povolení ověření integrované Windows**

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorizace Windows uživatele a skupiny

Když povolíte ověřování Windows, můžete použít atribut [Authorize] pro řízení přístupu k řadiči nebo akce kontroleru. Tento atribut lze použít pro celý kontroler MVC nebo určitý kontroler akce.

Například kontroler Home v informacích 1 poskytuje tři akce s názvem Index() CompanySecrets() a StephenSecrets(). Kdokoli může vyvolat akci Index(). Akci CompanySecrets() však lze vyvolat pouze členové místní skupiny Správci Windows. Akci StephenSecrets() nakonec můžete vyvolat pouze Windows uživatele domény s názvem Stephen (v doméně Redmond).

**Výpis 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> Z důvodu Windows řízení uživatelských účtů (UAC), při práci s Windows Vista nebo Windows Server 2008, místní skupiny Administrators se chovat jinak než ostatní skupiny. Atribut [Authorize] jej nerozpozná správně členem místní skupiny Administrators, není-li změnit nastavení nástroje Řízení uživatelských účtů v počítači.


Přesně co se stane při pokusu o vyvolání akce kontroleru bez oprávnění závisí na typu ověřování povoleno. Ve výchozím nastavení při použití serveru ASP.NET Development Server stačí získat prázdnou stránku. Na stránce obsluhuje s **401 Neautorizováno** stav odpovědi HTTP.

Pokud, na druhé straně při použití služby IIS se anonymní ověřování zakázané a základní ověřování je povoleno, pak bude dále zobrazovat výzvy dialogové okno přihlášení pokaždé, když požádáte o stránce chráněné (viz obrázek 4).

**Obrázek 4 – dialogové okno přihlášení základní ověřování**

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a>Souhrn

V tomto kurzu je vysvětleno, jak můžete používat ověřování Windows v rámci aplikace ASP.NET MVC. Jste se dozvěděli, jak povolit ověřování Windows v rámci souboru konfigurace webové aplikace a postup konfigurace ověřování pomocí služby IIS. Nakonec jste zjistili, jak používat atribut [Authorize] k omezení přístupu na akce kontroleru, zejména Windows uživatelům nebo skupinám.

> [!div class="step-by-step"]
> [Předchozí](authenticating-users-with-forms-authentication-cs.md)
> [další](preventing-javascript-injection-attacks-cs.md)
