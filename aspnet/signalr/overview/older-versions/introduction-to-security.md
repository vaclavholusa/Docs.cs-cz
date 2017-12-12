---
uid: signalr/overview/older-versions/introduction-to-security
title: "Úvod k zabezpečení SignalR (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Popisuje problémy se zabezpečením, které musíte zvážit při vývoji aplikace SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 04487614b219f8f6f8f0524c3b5f1aa42480c4d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-signalr-security-signalr-1x"></a>Úvod k zabezpečení SignalR (SignalR 1.x)
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje problémy se zabezpečením, které musíte zvážit při vývoji aplikace SignalR.


## <a name="overview"></a>Přehled

Tento dokument obsahuje následující části:

- [Koncepty zabezpečení SignalR](#concepts)

    - [Ověřování a autorizace](#authentication)
    - [Token připojení](#connectiontoken)
    - [Při opětovné připojení, opětovné připojování ke skupinám](#rejoingroup)
- [Jak SignalR brání padělání požadavku posílaného mezi weby](#csrf)
- [Doporučení zabezpečení SignalR](#recommendations)

    - [Zabezpečený protokol soketu vrstvy (SSL)](#ssl)
    - [Nepoužívejte skupiny jako mechanismus zabezpečení](#groupsecurity)
    - [Bezpečně zpracování vstupu z klientů](#input)
    - [Sjednocování změnu stavu uživatele s aktivního připojení](#reconcile)
    - [Automaticky generované soubory proxy JavaScript](#autogen)
    - [Výjimky](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Koncepty zabezpečení SignalR

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Ověřování a autorizace

SignalR je určena pro integrovat do stávající struktury ověřování pro aplikaci. Neposkytuje všechny funkce pro ověřování uživatelů. Místo toho můžete ověřování uživatelů, jako by normálně ve vaší aplikaci a pak pracovat s výsledky ověřování ve vašem kódu SignalR. Například můžete ověřit uživatele pomocí ověřování ASP.NET pomocí formulářů a pak vynutit v centru, kteří uživatelé nebo role mají oprávnění k volání metody. V centru můžete předat také informace o ověřování, jako je například uživatelské jméno nebo jestli se uživatel patří do role, do klienta.

Funkce SignalR poskytuje [Autorizovat](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) atribut k určení uživatelů, kteří mají přístup do rozbočovače nebo metoda. Použít atribut autorizovat k rozbočovači nebo konkrétní metody v rozbočovači. Bez atribut autorizovat jsou k dispozici pro klienta, který je připojený k rozbočovači všechny veřejné metody v rozbočovači. Další informace o centrech najdete v tématu [ověřování a autorizace pro rozbočovače SignalR](../security/hub-authorization.md).

`Authorize` Atribut se používá jenom s rozbočovači. K vynucení pravidel autorizace při použití `PersistentConnection` je nutné přepsat `AuthorizeRequest` metoda. Další informace o trvalé připojení najdete v tématu [ověřování a autorizace pro trvalé připojení SignalR](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token připojení

SignalR snižuje riziko provádění škodlivý příkazy ověřením identity odesílatele. Token připojení obsahující id připojení a uživatelské jméno pro ověřené uživatele, se předávají mezi klientem a serverem pro každý požadavek. Id připojení je jedinečný identifikátor, který je náhodně generovaný serverem, když se vytvoří nové připojení a je trvalé po dobu trvání připojení. Uživatelské jméno je poskytovaný ověřovací mechanismus pro webovou aplikaci. Token připojení je chránit pomocí šifrování a digitální podpis.

![](introduction-to-security/_static/image2.png)

Pro každý požadavek server ověřuje obsah tokenu, aby zajistila, že požadavek pochází ze zadaného uživatele. Uživatelské jméno musí odpovídat id připojení. Tím, že ověří id připojení a uživatelské jméno, SignalR zabrání uživatel se zlými úmysly snadno zosobnění jiného uživatele. Pokud server nelze ověřit token připojení, žádost skončí s chybou.

![](introduction-to-security/_static/image4.png)

Id připojení je součástí procesu ověřování, by neměl odhalit id připojení pro jednoho uživatele jiným uživatelům nebo uložení hodnoty na straně klienta, jako v souboru cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Při opětovné připojení, opětovné připojování ke skupinám

Ve výchozím nastavení SignalR aplikace bude automaticky znovu přiřadit uživatele do příslušných skupin po opětovném připojení z dočasné přerušení, jako je například když je připojení vyřazen a znovu vytvořit předtím, než vyprší časový limit připojení. Při opětovné připojení, klient bude provedeno úspěšně skupiny token, který obsahuje id připojení a skupiny přiřazené. Skupiny tokenu je digitálně podepsat a zašifrovat. Klient zachová stejné id připojení po opětovné připojení; id připojení předán z opětovného připojení klienta proto musí odpovídat id předchozího připojení používaná klientem. Toto ověření zabrání předání žádosti o připojení neoprávněným skupiny při opětovném připojení uživatel se zlými úmysly.

Je ale důležité si uvědomit, že token skupiny nevyprší platnost. Pokud uživatel patřil do skupiny v minulosti, ale byl vyloučen z této skupiny, může tento uživatel moci napodobovat skupiny token, který obsahuje zakázané skupiny. Pokud potřebujete bezpečně spravovat uživatele, kteří patří do skupiny, které potřebujete k ukládání dat na serveru, například v databázi. Potom si do vaší aplikace, která ověřuje na serveru, zda uživatel patří do skupiny přidejte logiku. Příklad ověření členství ve skupině, najdete v části [práce se skupinami](../guide-to-the-api/working-with-groups.md).

Automaticky opětovné připojování ke skupinám platí pouze při připojení je znovu připojeny po dočasné přerušení. Pokud uživatel odpojí podle přechodem z aplikace nebo se aplikace restartuje, aplikace musí zpracovávat postup přidání tohoto uživatele správný skupinám. Další informace najdete v tématu [práce se skupinami](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Jak SignalR brání padělání požadavku posílaného mezi weby

Proti útokům webů padělání požadavku (CSRF) je útok, při kterém škodlivé weby odešle požadavek na citlivé lokality, kde uživatel momentálně přihlášen. SignalR zabraňuje proti útokům CSRF tím, že velmi pravděpodobně škodlivý lokality za účelem vytvoření žádost platná pro vaši aplikaci SignalR.

### <a name="description-of-csrf-attack"></a>Popis útoku proti útokům CSRF

Tady je příklad útoku proti útokům CSRF:

1. Uživatel přihlásí do www.example.com, ověřování pomocí formulářů.
2. Server ověřuje uživatele. Odpověď ze serveru obsahuje soubor cookie ověřování.
3. Bez protokolování uživatel navštíví škodlivý web. Tento škodlivý lokalita obsahuje následující formuláře HTML: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

 Všimněte si, že akce formuláře požadavky na server, snadno napadnutelný, aby škodlivé weby. Toto je část "webů" proti útokům CSRF.
4. Uživatel kliknutím na tlačítko pro odeslání. V prohlížeči zahrnuje ověřovacího souboru cookie s požadavkem.
5. Žádost se spustí na serveru example.com s kontext ověřování uživatele a dělat vše, co ověřený uživatel může provádět.

I když tento příklad vyžaduje, aby uživatel klepnutím na tlačítko formuláře, na stránce škodlivého může stejným způsobem jako snadno spustit skript, který odešle požadavek AJAX do vaší aplikace SignalR. Kromě toho pomocí protokolu SSL nebrání útoku proti útokům CSRF protože škodlivé weby můžete odeslat požadavek "https://".

Obvykle je možné proti webové stránky, které používají soubory cookie pro ověřování, proti útokům CSRF útokům, protože prohlížeče odesílají všechny relevantní soubory cookie k cílovému webu. Útoky proti útokům CSRF však nejsou omezeny na zneužitím soubory cookie. Ověřování Basic a Digest jsou například také snadno napadnutelný. Po přihlášení uživatele pomocí ověřování základní nebo Digest, v prohlížeči automaticky odesílá přihlašovací údaje až do ukončení relace.

### <a name="csrf-mitigations-taken-by-signalr"></a>Způsoby zmírnění proti útokům CSRF provedenou SignalR

SignalR provede následující kroky na škodlivé weby zabránit ve vytváření platné požadavky do vaší aplikace SignalR. Tyto kroky se provádějí ve výchozím nastavení a nevyžadují žádnou akci v kódu.

- **Zakázat žádosti napříč doménami**  
 Ve výchozím nastavení křížové domény požadavky jsou zakázané v aplikaci SignalR uživatelům zabránit v volání koncový bod SignalR z externí domény. Každá žádost, která pochází z externí domény je automaticky považovány za neplatné a budou blokovány. Doporučujeme, abyste toto výchozí chování; v opačném škodlivé weby by mohl přimět uživatele odesílání příkazů na váš web. Pokud budete muset použít napříč požadavky domény, najdete v části [postup připojení mezi doménami](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Předejte token připojení řetězce dotazu, není soubor cookie**  
 SignalR předá token připojení jako hodnotu řetězce dotazu místo jako soubor cookie. Token připojení není uložíte jako soubor cookie, nastavení není token připojení v prohlížeči předají nechtěně vyskytne škodlivý kód. Token připojení navíc není trvalý nad rámec aktuální připojení. Uživatel se zlými úmysly proto nelze provést žádost pověřeními ověřování jiným uživatelem.
- **Ověřit token připojení**  
 Jak je popsáno v [token připojení](#connectiontoken) části serveru zná, které id připojení souvisí s každou ověřeného uživatele. Server nezpracovává každá žádost z id připojení, která neodpovídá uživatelské jméno. Není pravděpodobné, že uživatel se zlými úmysly může uhodnout žádost platná, protože uživatel se zlými úmysly musel předem znát uživatelské jméno a aktuální id náhodně generované připojení. Toto id připojení stává neplatným, jakmile je připojení ukončeno. Anonymní uživatelé by neměl mít přístup k žádné citlivé informace.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Doporučení zabezpečení SignalR

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Zabezpečený protokol soketu vrstvy (SSL)

Protokol SSL používá šifrování k zabezpečení přenosu dat mezi klientem a serverem. Pokud vaše aplikace SignalR přenáší citlivé informace mezi klientem a serverem, použijte protokol SSL pro přenos. Další informace o nastavení protokolu SSL najdete v tématu [postup nastavení protokolu SSL ve službě IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>Nepoužívejte skupiny jako mechanismus zabezpečení

Skupiny jsou pohodlný způsob shromažďování související uživatele, ale nejsou zabezpečené mechanismus pro omezení přístupu k důvěrným informacím. To platí hlavně když uživatelé mohou automaticky znovu připojí skupiny během o obnovení. Místo toho zvažte přidání mohou uživatelé s oprávněním k roli a omezení přístupu k metodě rozbočovače pouze členům této role. Příklad omezení přístupu na základě role, naleznete v části [ověřování a autorizace pro rozbočovače SignalR](../security/hub-authorization.md). Příklad ověření přístupu uživatelů do skupin, pokud připojení, naleznete v části [práce se skupinami](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Bezpečně zpracování vstupu z klientů

Veškerý vstup z klientů, který je určený pro vysílání na jiných klientů musí být kódovaný zajistit, že uživatel se zlými úmysly neodesílá skriptu do jiných uživatelů. Je nejvhodnější kódování zprávy v přijímající klientů, nikoli na server, protože aplikace SignalR může mít mnoho různých typů klientů. Kódování HTML funguje proto webovému klientovi, ale ne pro jiné typy klientů. Například metoda klienta webové zobrazíte chatovací zprávy by bezpečně popisovač uživatelské jméno a zpráva voláním `html()` funkce.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Sjednocování změnu stavu uživatele s aktivního připojení

Pokud se stav ověření uživatele, změní během aktivního připojení existuje, uživateli se zobrazí chyba s oznámením, "identity uživatele se nemůže změnit během aktivního připojení SignalR." V takovém případě aplikace by měl znovu připojit k serveru a ujistěte se, že jsou koordinované id připojení a uživatelské jméno. Například pokud vaše aplikace umožňuje uživatelům odhlášení během aktivního připojení existuje, uživatelské jméno pro připojení se už nebude shodovat s názvem, je předaná pro další požadavek. Bude chtít zastavit připojení, než odhlášení uživatele a pak ji znovu spusťte.

Je ale důležité si uvědomit, že většina aplikací nebude potřeba ručně zastavit a spustit připojení. Pokud vaše aplikace přesměruje uživatele na samostatné stránce po přihlášení, jako je například výchozí chování v aplikaci Web Forms nebo aplikace MVC nebo aktualizuje aktuální stránku po odhlášení, aktivní připojení automaticky odpojí a ne nevyžaduje žádnou další akci.

Následující příklad ukazuje, jak zastavit a spustit připojení v případě, že došlo ke změně stavu uživatele.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

Nebo ověřování stavu uživatele může změnit, pokud váš web používá klouzavé vypršení platnosti se ověřování pomocí formulářů a neaktivní zachovat ověřovacího souboru cookie platné. V takovém případě se odhlásit uživatele a uživatelské jméno se již nebudou shodovat s názvem uživatele v token připojení. Tento problém můžete vyřešit tak, že přidáte některé skript, který pravidelně požádá o prostředek na webovém serveru chcete zachovat ověřovacího souboru cookie platné. Následující příklad ukazuje, jak požádat o prostředku každých 30 minut.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Automaticky generované soubory proxy JavaScript

Pokud nechcete zahrnout všechny rozbočovače a metody soubor proxy JavaScript pro každého uživatele, můžete zakázat automatické generování souboru. Tuto možnost můžete zvolit, pokud máte více rozbočovače a metody, ale nechcete, aby každý uživatel znát všechny metody. Zakázat automatické generování nastavením **EnableJavaScriptProxies** k **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Další informace o souborech proxy JavaScript najdete v tématu [vygenerovaný proxy server a jakým způsobem vám](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Výjimky

Vyhněte se předávání objektů výjimky pro klienty, vzhledem k tomu objekty mohou být citlivé informace pro klienty. Místo toho volání metody na straně klienta, který zobrazí relevantní chybovou zprávu.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
