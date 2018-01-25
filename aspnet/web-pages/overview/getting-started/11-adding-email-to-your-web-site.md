---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: "Odesílání e-mailu z rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tato kapitola vysvětluje postup odesílání automatizovaných e-mailovou zprávu z webu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: c5878c3bc468daef050dcebee99f64441066409a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Odesílání e-mailu z webové stránky ASP.NET s stránky (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje postup odesílání e-mailovou zprávu z webu při použití technologie ASP.NET Web Pages (Razor).
> 
> Získáte informace:
> 
> - Postup odesílání e-mailovou zprávu z vašeho webu.
> - Postup připojení souboru k e-mailovou zprávu.
> 
> Toto je funkce technologie ASP.NET byla zavedená v článku:
> 
> - `WebMail` Pomocné rutiny.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Odesílání e-mailové zprávy z webu

Existují nejrůznějším z důvodů, proč může být nutné odeslat e-mailu z vašeho webu. Můžete odeslat potvrzující zprávy pro uživatele, nebo může odesílat oznámení sami sobě (například zaregistrované nového uživatele.) `WebMail` Pomocník umožňuje snadno můžete odeslat e-mailu.

Chcete-li použít `WebMail` pomocné rutiny, je nutné mít přístup k serveru SMTP. (SMTP znamená *Simple Mail Transfer Protocol*.) SMTP server je e-mailový server, který pouze přeposílá zprávy na server příjemce &#8212; je odchozí straně e-mailů. Pokud používáte poskytovatele hostitelských služeb pro svůj web, budou pravděpodobně nastavit můžete s e-mailu a budou se dozvíte, co je název serveru SMTP. Pokud pracujete v podnikové síti, správce nebo oddělení IT může obvykle získáte informace o serveru SMTP, který můžete použít. Pokud pracujete v domácnostech, dokonce je možné otestovat pomocí zprostředkovatele obyčejnou e-mailu, který se dá zjistit název serveru SMTP. Obvykle potřebujete:

- Název serveru SMTP.
- Číslo portu. Toto je téměř vždy 25. Poskytovatel internetových služeb však může vyžadovat použití portu 587. Pokud používáte vrstvu secure Sockets Layer (SSL) pro e-mailu, bude pravděpodobně nutné jiný port. Zeptejte se svého poskytovatele e-mailu.
- Přihlašovací údaje (uživatelské jméno a heslo).

V tomto postupu vytvoříte dvě stránky. Na první stránku má formulář, který umožňuje uživatelům zadejte popis, jako kdyby byly vyplňování formuláře technické podpory. První stránka odešle její informace na druhé stránce. Na druhé stránce kód extrahuje informace o uživateli a odešle e-mailovou zprávu. Také zobrazuje výzvu k potvrzení, že hlášení problému byla přijata.

![[Obrázek]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Pro zjednodušení tento příklad inicializuje kód `WebMail` pomocná přímo na stránku, kde můžete ji použít. Pro skutečné weby, je však lepší představu uvést do soubor globální inicializace kód jako to, aby inicializaci `WebMail` Pomocník pro všechny soubory ve vašem webu. Další informace najdete v tématu [přizpůsobení chování na webu pro webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Vytvoření nového webu.
2. Přidat novou stránku s názvem *EmailRequest.cshtml* a přidejte následující kód: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Všimněte si, že `action` atribut form element byla nastavena na *ProcessRequest.cshtml*. To znamená, že bude formulář odeslat na této stránce místo zpět na aktuální stránce.
3. Přidat novou stránku s názvem *ProcessRequest.cshtml* na web a přidejte následující kód a značky:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    V kódu můžete získat hodnoty polí formuláře, které byly odeslány na stránku. Potom zavolejte `WebMail` pomocné rutiny pro `Send` metodu pro vytvoření a odeslání e-mailové zprávy. V takovém případě hodnot pro použití vytvořené text, který řetězení s hodnotami, které byly odeslány z formuláře.

    Kód pro tuto stránku je uvnitř `try/catch` bloku. Pokud pro některý důvodu pokus o odeslání e-mailu nefunguje (například nastavení není pravé), kód `catch` bloku běží a nastaví `errorMessage` proměnnou chybu, která se má došlo k chybě. (Další informace o `try/catch` bloky nebo `<text>` značky, najdete v části [Úvod k technologii ASP.NET Web Pages programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    V těle stránky Pokud `errorMessage` proměnné je prázdný (výchozí), uživateli se zobrazí zpráva, která byla odeslána e-mailové zprávy. Pokud `errorMessage` proměnná je nastavená na hodnotu true, uživatel vidí a zpráva, že byl problém odeslání zprávy.

    Všimněte si, že v části stránky, která zobrazí chybovou zprávu, je další test: `if(debuggingFlag)`. To je proměnná, která můžete nastavit na hodnotu true, pokud máte problémy při odesílání e-mailu. Když `debuggingFlag` má hodnotu true, a pokud dojde k problému odesílání e-mailu, další chybová zpráva se zobrazí zobrazující ať ASP.NET ohlásil při pokusu o odeslání e-mailové zprávy. Správného upozornění, když: chybové zprávy, které ASP.NET nahlásí, když ho nelze odeslat e-mailové zprávy mohou být obecný. Například pokud ASP.NET nemůže připojit k serveru SMTP (například proto jste udělali chybu v názvu serveru), chyba je `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Důležité** při zobrazí chybové hlášení z objektu výjimky (`ex` v kódu), proveďte *není* pravidelně předávat zprávy prostřednictvím uživatelům. Objekty výjimek často obsahují informace, které by se neměly zobrazovat uživatelům a který může být i chyba zabezpečení. Proto tento kód obsahuje proměnnou `debuggingFlag` který slouží jako přepínač zobrazíte chybovou zprávu a proč proměnná ve výchozím nastavení bude nastavena na hodnotu false. Měli byste nastavit tuto proměnnou na hodnotu true (a proto zobrazí chybová zpráva) *pouze* Pokud máte problémy s e-mailem s a potřebujete k ladění. Po opravě potíže nastavit `debuggingFlag` zpět na hodnotu false.

    Upravit následující související nastavení v kódu e-mailu:

    - Nastavit `your-SMTP-host` na název serveru SMTP, který máte přístup.
    - Nastavit `your-user-name-here` na uživatelské jméno pro svůj účet serveru SMTP.
    - Nastavit `your-account-password` na heslo pro svůj účet serveru SMTP.
    - Nastavit `your-email-address-here` vlastní e-mailovou adresu. Toto je e-mailovou adresu, které je zpráva odeslána z. (Někteří poskytovatelé e-mailu Nenechte si můžete nastavit jinou `From` adres a bude používat vaše uživatelské jméno jako `From` adresu.)

    > [!TIP] 
    > 
    > <a id="configuring_email_settings"></a>
    > ### <a name="configuring-email-settings"></a>Konfigurace nastavení e-mailu
    > 
    > Může být složité někdy zkontrolujte, zda že máte správné nastavení pro server SMTP, číslo portu a tak dále. Zde je několik tipů.
    > 
    > - Název serveru SMTP je často něco podobného jako `smtp.provider.com` nebo `smtp.provider.net`. Pokud však publikování webu k poskytovateli hostingu, název serveru SMTP v daném okamžiku může být `localhost`. Je to proto, že když jste publikovali a vaše lokalita běží na serveru poskytovatele, e-mailový server může být místní z hlediska vaší aplikace. Tuto změnu v hodnotě názvů serverů může znamenat, že budete muset změnit název serveru SMTP jako součást procesu publikování.
    > - Číslo portu je obvykle 25. Ale někteří poskytovatelé vyžadovat použití portu 587 nebo některé port.
    > - Ujistěte se, že používáte správné přihlašovací údaje. Pokud váš web jste publikovali do poskytovatele hostitelských služeb, použijte přihlašovací údaje, které zprostředkovatel má konkrétně označil jsou e-mailu. To může být liší od přihlašovacích údajů, které můžete použít k publikování.
    > - Někdy vůbec nepotřebujete přihlašovací údaje. Pokud odesíláte e-mailu pomocí osobní poskytovatel internetových služeb, může poskytovatel e-mailu již znáte svoje přihlašovací údaje. Po publikování, možná budete muset použít jiné pověření než při testování v místním počítači.
    > - Pokud váš poskytovatel e-mailu používá šifrování, budete muset nastavit `WebMail.EnableSsl` k `true`.
4. Spustit *EmailRequest.cshtml* stránku v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.)
5. Zadejte název a popis problému a poté klikněte **odeslání** tlačítko. Budete přesměrováni na *ProcessRequest.cshtml* stránky, která potvrdí zprávu a které vám pošle e-mailovou zprávu. 

    ![[Obrázek]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Odeslání souboru pomocí e-mailu

Můžete také odeslat soubory, které jsou připojené k e-mailové zprávy. V tomto postupu vytvoříte textového souboru a dvě stránky HTML. Použijete textový soubor jako přílohu e-mailu.

1. Na webu, přidejte nový textový soubor a pojmenujte ji *MyFile.txt*.
2. Zkopírujte následující text a vložte ji do souboru: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Vytvoření stránky s názvem *SendFile.cshtml* a přidejte následující kód: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Vytvoření stránky s názvem *ProcessFile.cshtml* a přidejte následující kód: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Upravit následující související nastavení v kódu z příkladu e-mailu:

    - Nastavit `your-SMTP-host` na název serveru SMTP, máte přístup.
    - Nastavit `your-user-name-here` na uživatelské jméno pro svůj účet serveru SMTP.
    - Nastavit `your-email-address-here` vlastní e-mailovou adresu. Toto je e-mailovou adresu, které je zpráva odeslána z.
    - Nastavit `your-account-password` na heslo pro svůj účet serveru SMTP.
    - Nastavit `target-email-address-here` vlastní e-mailovou adresu. (Jako dříve, by za normálních okolností odeslat e-mailem někomu jinému, ale pro testování, můžete ho odeslat sami sobě.)
6. Spustit *SendFile.cshtml* stránku v prohlížeči.
7. Zadejte své jméno, řádek předmětu a název textového souboru, připojit (*MyFile.txt*).
8. Klikněte `Submit` tlačítko. Jak před, budete přesměrováni na *ProcessFile.cshtml* stránky, která potvrdí zprávu a které vám pošle e-mailovou zprávu s přiložený soubor.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


- [Webové stránky ASP.NET (Razor) – průvodce řešením potíží](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Protokol SMTP](https://msdn.microsoft.com/library/aa480435.aspx)
- [Přizpůsobení chování na webu pro webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
