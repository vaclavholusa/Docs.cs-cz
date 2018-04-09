---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET – webové stránky průvodce řešením potíží (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje problémy, které by mohly mít při práci s webových stránek ASP.NET (Razor) a některé navrhovanými řešeními. Verze softwaru ASP.NET Web Pag...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec51169ccea0016712de3fdb28a16a174150a8bd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET – webové stránky průvodce řešením potíží (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje problémy, které by mohly mít při práci s webových stránek ASP.NET (Razor) a některé navrhovanými řešeními.
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2 a ASP.NET Web Pages 1.0.


Toto téma obsahuje následující oddíly:

- [Problémy se spouštěním stránky](#Issues_Running_.cshtml_Pages)
- [Problémy s kódu Razor](#IssuesWithRazorCode)
- [Problémy s členství a zabezpečení](#membership)
- [Problémy s e-mailem](#email)
- [Další zdroje informací](#AdditionalResources)

Obecné otázky, najdete v části [rozhraní ASP.NET Web Pages (Razor) – nejčastější dotazy](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Problémy se spouštěním stránky

Můžete zabránit celou řadu problémů *.cshtml* a *.vbhtml* stránky z správně spuštěna. V této části jsou uvedeny běžné chybové zprávy a pravděpodobně způsobí, že.

### <a name="http-error-403---forbidden-access-is-denied"></a>Chyba protokolu HTTP 403 – Zakázáno: Přístup byl odepřen.

*Nemáte oprávnění k zobrazení tohoto adresáře nebo stránky pomocí zadaných přihlašovacích údajů.*

Této chybě může dojít, pokud server není spuštěn správnou verzi rozhraní .NET Framework. Ujistěte se, že počítač, který je spuštěný server (místně nebo vzdáleně) má alespoň nainstalované rozhraní .NET Framework 4. Také zajistěte, aby vlastní aplikace je nakonfigurovaná pro spuštění správnou verzi.

Pokud tento problém se zobrazí místně při práci ve službě WebMatrix, klikněte na tlačítko **lokality** pracovního prostoru a potom ve stromovém zobrazení, klikněte na tlačítko **nastavení**. V **vyberte verzi rozhraní .NET Framework** vyberte **.NET 4 (integrovaný režim)**. Pokud tato verze je už nastavené, zkuste spustit jako správce služby WebMatrix.

Ověřte, zda kořenovém adresáři vašeho webu má alespoň jeden *.cshtml* souboru v ní.

Pokud se zobrazí tato chyba, když webový server je na vzdáleném serveru, obraťte se na správce serveru. Ujistěte se, že server má rozhraní .NET Framework 4 nebo novější. Také se ujistěte, že je aplikace spuštěna ve fondu aplikací, který je nakonfigurován pro použití této verzi rozhraní.NET Framework.

Pokud budete mít kontrolu nad serveru, ujistěte se, že běží správnou verzi rozhraní .NET Framework. Může také zkuste opravit instalaci spuštěním `aspnet_regiis -iru` příkaz. (Například při instalaci služby IIS po instalaci rozhraní .NET Framework, služba IIS nebude správně konfigurován pro spuštění stránek ASP.NET.) Další informace najdete v tématu [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>Chyba protokolu HTTP 403.14 – zakázáno

*Webový server je nakonfigurován tak, aby nezobrazovat obsah tohoto adresáře.*

Této chybě může dojít v případě požadavku na prostředek, který je chráněn (jako *Web.config* soubor) nebo do složky, který je chráněný, který je (jako *aplikace\_Data* nebo *aplikace\_Kód*).

### <a name="http-error-40417---not-found"></a>Chyba protokolu HTTP 404.17 – Nenalezeno

*Požadovaný obsah pravděpodobně skript který nebude obslužnou rutinou statických souborů.*

Této chybě může dojít, pokud server není správně nakonfigurován pro používání rozhraní .NET Framework 4 nebo novější a proto nemůže rozpoznat kód v `@{ }` bloky. Naleznete v popisu výše pro *HTTP chyby 403 – Zakázáno: přístup byl odepřen*.

### <a name="http-error-4047---not-found"></a>Chyba protokolu HTTP 404.7 - nebyl nalezen

*Modul filtrování požadavků je nakonfigurován tak, aby odepřel příponu souboru*

Této chybě může dojít, pokud *.cshtml* nebo *.vbhtml* rozšíření byla explicitně zablokovaná na serveru. Příznakem tohoto problému je svou práci adresy URL, když není uvedena rozšíření, ale adresy URL, které zahrnují *.cshtml* nebo *.vbhtml* nefungují. Možných řešení je znovu povolit rozšíření na webu *Web.config* souboru. Následující příklad ukazuje, jak povolit *.cshtml* rozšíření.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>Chyba protokolu HTTP 404.8 - nebyla nalezena

*Modul filtrování požadavků je nakonfigurován tak, aby odepřel cestu v adrese URL, která obsahuje oddíl hiddensegment.*

Této chybě může dojít v případě požadavku na prostředek, který je chráněn (jako *Web.config* soubor) nebo do složky, který je chráněný, který je (jako *aplikace\_Data* nebo *aplikace\_Kód*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Tento typ stránky není obsluhovat (Chyba serveru v aplikaci '/')

Naleznete v popisu výše pro 404.17 chyby protokolu HTTP.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Problémy s kódu Razor

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Název '*třída*' neexistuje v aktuálním kontextu

Často, který je důvod se zobrazí tato chyba `class` odkazy pomocné rutiny, ale pomocné rutiny není nainstalována. Například pokud se pokusíte použít pomocné rutiny, ale pokud nemáte nainstalovaný balíček z NuGet, zobrazí se tato chyba. Pomocí Galerie ve službě WebMatrix můžete najít a nainstalovat pomocné rutiny.

Pokud je nainstalován pomocné rutiny, ale stránce stále nerozpoznal ho, pokuste se přidat přidat `using` příkaz ke kódu. V `using` prohlášení, odkaz na obor názvů, který obsahuje pomocné rutiny. Například základní pomocné rutiny, které jsou v balíčku ASP.NET webové pomocné objekty jsou v `System.Web.Helpers` oboru názvů. V horní části stránky, kde chcete použít pomocné rutiny přidejte následující řádek:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Problémy s členství a zabezpečení

Pokud používáte systém integrované zabezpečení (členství) na webových stránkách ASP.NET (Razor), mohou se vyskytnout následující problémy.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Mohli volat tuto metodu, musí být vlastnost "Membership.Provider" instance "ExtendedMembershipProvider"

Tuto chybu můžete určit, že žádné `AspNetSqlMembershipProvider` třída je nakonfigurovaný. (Příznakem je, že lokality funguje bez problémů místně, ale vrátí tuto chybu při publikování serveru poskytovatele hostingu.) Jeden tohoto problému je explicitně povolit jednoduché členství vložením následujícího textu na web *Web.config* souboru:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Problémy s e-mailem

Problémy s e-mailem, může být náročné k ladění. Počáteční problém může být, že se nelze připojit k serveru SMTP. Pokud je připojení úspěšné, ASP.NET předá zprávu na SMTP server. Však může být problémy se zprávou sám sebe, který zabrání odeslání serveru SMTP.

Pokud vaše aplikace není úspěšně odeslat e-mailu, zkuste následující postup:

- Název serveru SMTP je často něco podobného jako `smtp.provider.com` nebo `smtp.provider.net`. Pokud však publikování webu k poskytovateli hostingu, název serveru SMTP v daném okamžiku může být `localhost`. K této situaci dochází, protože když jste publikovali a vaše lokalita běží na serveru poskytovatele, SMTP server může být místní z hlediska vaší aplikace. Tuto změnu v hodnotě názvů serverů může znamenat, že budete muset změnit název serveru SMTP jako součást procesu publikování.
- Číslo portu je obvykle 25. Ale někteří poskytovatelé vyžadovat použití portu 587 nebo některé port. Obraťte se na vlastníka serveru SMTP, jaké číslo portu se předpokládá, že použít.
- Ujistěte se, že používáte správné přihlašovací údaje. Pokud váš web jste publikovali do poskytovatele hostitelských služeb, použijte přihlašovací údaje, které zprostředkovatel má konkrétně označil jsou e-mailu. Tyto přihlašovací údaje se může lišit od přihlašovacích údajů, které můžete použít k publikování.
- Někdy vůbec nepotřebujete přihlašovací údaje. Pokud odesíláte e-mailu pomocí osobní poskytovatel internetových služeb, může poskytovatel e-mailu již znáte svoje přihlašovací údaje. Po publikování, možná budete muset použít jiné pověření než při testování v místním počítači.
- Pokud váš poskytovatel e-mailu používá šifrování, nastavte `WebMail.EnableSsl` k `true`.

Pokud dojde k chybě při odesílání e-mailu, může se zobrazit zpráva standardní chyba ASP.NET, který vypadá takto:

![ASP.NET chybovou zprávu, když dojde k potížím s e-mailu](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Můžete také ladění potíží při odesílání e-mailu pomocí `try-catch` bloku, jako v následujícím příkladu. Při použití `try-catch` bloku ASP.NET nezobrazuje jeho standardní cíl chybové zprávy. Místo toho můžete zaznamenat chybu v `catch` část bloku.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Nahraďte příslušnými hodnotami pro `your-SMTP-server-name`a tak dále. Některé chybové zprávy, které mohou být zobrazeny tímto způsobem zahrnují následující:

- *Chyba při odesílání e-mailu.*

    -nebo-

    *Pokus o připojení se nezdařila, protože připojená strana nereagovala správně po určitou dobu nebo navázané připojení se nezdařila, protože připojení hostitele se nepodařilo reagovat*

    Tato chyba obvykle znamená, že aplikace se nemůže připojit k serveru SMTP. Zkontrolujte název serveru a číslo portu.
- <em>Poštovní schránky k dispozici. Odpověď serveru: 5.1.0 &lt; someuser@invaliddomain &gt; odesílatele odmítl: Neplatný odesílatele domény</em>

    Tuto zprávu můžete určit, že `From` adresa není správná nebo chybí.
- *Zadaný řetězec není ve formátu požadované pro e-mailovou adresu.*

    Tato chyba může znamenat, hodnota `To` nebo `From` vlastnosti nejsou rozpoznány jako e-mailové adresy. (ASP.NET nelze zkontrolovat, že e-mailová adresa je platná, pouze to je ve správném formátu, jako je třeba *name@domain.com*.)

> [!NOTE]
> Odeberte kód, který se zobrazí chyba (`@errorMessage`) před publikováním stránky k lokalitě za provozu. Není vhodné tak, aby uživatelé zobrazit chybové zprávy, které jste získali ze serveru.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Další prostředky

[Webové stránky ASP.NET (Razor) – časté otázky](https://go.microsoft.com/fwlink/?LinkId=253000)

[Služba WebMatrix a webové stránky ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) fórum na webu technologie ASP.NET
