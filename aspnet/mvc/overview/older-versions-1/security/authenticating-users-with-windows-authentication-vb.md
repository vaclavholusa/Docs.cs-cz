---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Ověřování uživatelů pomocí ověřování systému Windows (VB) | Microsoft Docs
author: microsoft
description: Další informace o použití ověřování systému Windows v rámci aplikace MVC. Zjistíte, jak povolit ověřování systému Windows v rámci převeďte webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: cf711d44a05d2457493998ed61e86536c65b5984
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-windows-authentication-vb"></a>Ověřování uživatelů pomocí ověřování systému Windows (VB)
====================
podle [Microsoft](https://github.com/microsoft)

> Další informace o použití ověřování systému Windows v rámci aplikace MVC. Zjistíte, jak povolit ověřování systému Windows v souboru konfigurace webové aplikace a jak nakonfigurovat ověřování pomocí služby IIS. Nakonec můžete Naučte se používat atribut [autorizovat] omezit přístup k akce kontroleru konkrétní Windows uživatelům nebo skupinám.


Cílem tohoto kurzu je vysvětlují, jak můžete využít výhod zabezpečení funkcí integrovaných do služby IIS heslo k ochraně zobrazení v aplikacích MVC. Zjistíte, jak umožnit akce kontroleru má být volána pouze pomocí konkrétního uživatele systému Windows nebo uživatelů, kteří jsou členy určité skupiny systému Windows.

Pomocí ověřování systému Windows má smysl, když vytváříte web interní společnosti (v síti intranet) a chcete, aby uživatelé mohli používat své standardní Windows uživatelská jména a hesla pro přístup k webu. Pokud vytváříte ven, kterým čelí webu (webové stránky Internetu) zvažte použití ověřování pomocí formulářů.

#### <a name="enabling-windows-authentication"></a>Povolení ověřování systému Windows

Když vytvoříte novou aplikaci ASP.NET MVC, ověřování systému Windows není povoleno ve výchozím nastavení. Ověřování pomocí formulářů je výchozím typem ověřování povoleno pro aplikace MVC. Upravovat soubor konfigurace (web.config) webové aplikace MVC, musíte povolit ověřování systému Windows. Najít &lt;ověřování&gt; část a upravte ho na použití Windows místo ověřování pomocí formulářů takto:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Pokud povolíte ověřování systému Windows, bude váš webový server zodpovědný za účelem ověřování totožnosti uživatelů. Obvykle existují dva různé typy webových serverů, které používají při vytváření a nasazování aplikace ASP.NET MVC.

Nejprve při vývoji aplikace MVC, pomocí webového serveru vývoj technologie ASP.NET zahrnutá v sadě Visual Studio. Ve výchozím nastavení spustí webový vývojový Server ASP.NET všechny stránky v kontextu aktuálního účtu systému Windows (ať účet jste použili k přihlášení do systému Windows).

Webový vývojový Server ASP.NET také podporuje ověřování protokolem NTLM. Pravým tlačítkem myši na název projektu v okně Průzkumníka řešení a výběrem vlastnosti můžete povolit ověřování protokolem NTLM. V dalším kroku vyberte kartu Web a zaškrtnutím políčka NTLM (viz obrázek 1).

**Obrázek 1 – povolení ověřování protokolem NTLM pro webový vývojový Server ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Pro provozní webové aplikace na straně, použijete službu IIS jako webový server. Služba IIS podporuje několik typů ověřování, včetně:

- Základní ověřování – definované v rámci protokolu HTTP 1.0. Odešle uživatelská jména a hesla ve formátu prostého textu (kódováním Base64) přes Internet. -Ověřování algoritmem Digest – odešle hodnotu hash hesla, namísto hesla samostatně, přes internet. -Integrované ověřování systému Windows (NTLM) – nejlepší typ ověřování pro použití v prostředí intranetu pomocí systému windows. -Certificate ověřování – umožňuje ověřování pomocí certifikátu klienta. Certifikát se mapuje na uživatelský účet systému Windows.

> [!NOTE] 
> 
> Podrobnější přehled tyto různé typy ověřování, najdete v tématu [ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).


Chcete-li povolit konkrétní typ ověřování můžete použít Správce Internetové informační služby. Upozorňujeme, že všechny typy ověřování nejsou k dispozici v případě každý operační systém. Kromě toho pokud používáte službu IIS 7.0 se systémem Windows Vista, musíte povolit různé typy ověřování systému Windows, než se objeví v Správce Internetové informační služby. Otevřete **ovládacích panelů, programů, programy a funkce, nebo vypnout funkce systému Windows**a rozbalte uzel Internetová informační služba (viz obrázek 2).

**Obrázek 2 – povolení Windows IIS funkce**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Pomocí Internetové informační služby, můžete povolit nebo zakázat různé typy ověřování. Obrázek 3 znázorňuje například anonymní ověřování zakázání a povolení ověřování systému Windows integrované (NTLM) při použití služby IIS 7.0.

**Obrázek 3: povolení integrovaného ověřování systému Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorizace Windows uživatelů a skupin

Po povolení ověřování systému Windows, můžete použít &lt;Autorizovat&gt; atribut pro řízení přístupu k řadiče nebo akce kontroleru. Tento atribut lze použít pro celý kontroler MVC nebo určitý kontroler akce.

Například řadič domovské v výpis 1 zpřístupní tři akce s názvem Index() CompanySecrets() a StephenSecrets(). Každý, kdo může vyvolat akci Index(). Pouze členové místní skupiny správců Windows však můžete vyvolat CompanySecrets() akce. Nakonec pouze Windows uživatele domény s názvem Stephen (v doméně Redmond) můžete vyvolat StephenSecrets() akce.

**Výpis 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Z důvodu Windows řízení uživatelských účtů (UAC), při práci s Windows Vista nebo Windows Server 2008, místní skupiny Administrators budou chovat jinak než ostatní skupiny. &lt;Autorizovat&gt; atribut nerozpozná správně členem místní skupiny Administrators nezměníte nastavení nástroje Řízení uživatelských účtů v počítači.


Přesně co se stane při pokusu o vyvolání akce kontroleru, aniž by musel být správná oprávnění závisí na typu ověřování povoleno. Ve výchozím nastavení se při použití vývojový Server ASP.NET získáte jednoduše prázdné stránky. Požadovanou stránku je zpracovat s **401 neautorizovaných** stav odpovědi HTTP.

Pokud na druhé straně používáte službu IIS zakázaný anonymní přístup a základní ověřování je povoleno a pak zachovat získávání přihlašovací dialogové okno výzvu pokaždé, když požádáte o stránce chráněné (viz obrázek 4).

**Obrázek 4 – základní ověřování přihlašovací dialogové okno**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Souhrn

V tomto kurzu vysvětlení, jak můžete použít ověřování systému Windows v rámci aplikace ASP.NET MVC. Jste se dozvěděli, jak povolit ověřování systému Windows v souboru konfigurace webové aplikace a jak nakonfigurovat ověřování pomocí služby IIS. Nakonec jste zjistili, jak používat &lt;Autorizovat&gt; atribut omezit přístup k akce kontroleru konkrétní Windows uživatelům nebo skupinám.

> [!div class="step-by-step"]
> [Předchozí](authenticating-users-with-forms-authentication-vb.md)
> [další](preventing-javascript-injection-attacks-vb.md)
