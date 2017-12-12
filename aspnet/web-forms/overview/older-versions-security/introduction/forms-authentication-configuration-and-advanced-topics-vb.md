---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: "Forms konfiguraci ověřování a pokročilá témata (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu jsme prozkoumá různých nastavení ověřování pomocí formulářů a zjistit, jak je upravit pomocí prvku formuláře. To vede podrobné..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: e92bb3d67141ba0ce594fd17c266bc69dda3cb5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>Konfigurace ověřování formulářů a pokročilá témata (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> V tomto kurzu jsme prozkoumá různých nastavení ověřování pomocí formulářů a zjistit, jak je upravit pomocí prvku formuláře. To bude mít za následek podrobný pohled přizpůsobení hodnota časového limitu lístek ověřování formuláře pomocí přihlašovací stránky s vlastní adresu URL (např. SignIn.aspx místo Login.aspx) a lístků pro ověřování pomocí formulářů bez souborů cookie.


## <a name="introduction"></a>Úvod

V [předchozí kurzu](an-overview-of-forms-authentication-vb.md) jsme se podívali na kroky potřebné pro implementaci ověřování pomocí formulářů v aplikaci ASP.NET, k vytvoření protokolu stránce k zobrazení různých zadat nastavení konfigurace v souboru Web.config. obsah pro ověřený a anonymní uživatele. Odvolat, že jsme nakonfigurovali web nastavením atributu režimu použít ověřování pomocí formulářů &lt;ověřování&gt; element do formulářů. &lt;Ověřování&gt; element může volitelně zahrnovat &lt;forms&gt; podřízený element, pomocí kterého je možné zadat širokou nastavení ověřování pomocí formulářů.

V tomto kurzu jsme zkontrolujte různých nastavení ověřování pomocí formulářů a zjistit, jak změnit prostřednictvím &lt;forms&gt; elementu. To bude mít za následek podrobný pohled přizpůsobení hodnota časového limitu lístek ověřování formuláře pomocí přihlašovací stránky s vlastní adresu URL (např. SignIn.aspx místo Login.aspx) a lístků pro ověřování pomocí formulářů bez souborů cookie. Také jsme přesněji zkontrolujte způsob vytvoření lístku ověřování formulářů a v tématu bezpečnostní opatření, která trvá ASP.NET pro zajištění zabezpečené z kontroly a manipulaci dat lístku. Nakonec se podíváme na tom, jak ukládat další uživatelská data v lístek pro ověřování pomocí formulářů a postup modelu tato data prostřednictvím vlastní objekt zabezpečení.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Krok 1: Prozkoumání &lt;forms&gt; nastavení konfigurace

Systém ověřování formulářů technologie ASP.NET nabízí řadu nastavení konfigurace, které se dají přizpůsobit pro jednotlivé aplikace aplikace. To zahrnuje nastavení, jako je: životnost ověřování založené na formulářích lístek; Jaký způsob ochrany se použije pro lístku; v části ověřování bez souborů cookie jaké podmínky se používají lístky; Cesta k přihlašovací stránce; a další informace. Chcete-li změnit výchozí hodnoty, přidejte [ &lt;forms&gt; element](https://msdn.microsoft.com/en-us/library/1d3t3c61.aspx) jako podřízenou [ &lt;ověřování&gt; element](https://msdn.microsoft.com/en-us/library/532aee0e.aspx), zadání těchto vlastností hodnoty, které chcete přizpůsobit jako atributy XML takto:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

Tabulka 1 shrnuje vlastnosti, které lze přizpůsobit pomocí &lt;forms&gt; elementu. Vzhledem k tomu, že soubor Web.config je soubor XML, názvy atributů v levém sloupci je malá a velká písmena.

| **Atribut** | **Popis** |
| --- | --- |
| bez souborů cookie | Tento atribut určuje, za jakých podmínek lístek ověřování je uložen v souboru cookie a jeho vkládání v adrese URL. Povolené hodnoty jsou: UseCookies; UseUri; Automatické rozpoznání; a UseDeviceProfile (výchozí). Krok 2 prozkoumá podrobněji toto nastavení. |
| defaultUrl | Určuje adresu URL, kterou uživatelé přesměrovaní na po přihlášení z přihlašovací stránky, pokud není žádná hodnota RedirectUrl zadaný v řetězci dotazu. Výchozí hodnota je default.aspx. |
| doména | Při použití lístků pro ověřování na základě souborů cookie, toto nastavení určuje hodnota souboru cookie s domény. Výchozí hodnota je řetězec prázdný, což způsobí, že prohlížeči domény, ze kterého byl vydán (například www.yourdomain.com). V takovém případě bude soubor cookie **není** zasílá po provedení žádosti subdomény, jako je například admin.yourdomain.com. Pokud chcete soubor cookie, které mají být předány všechny subdomény, budete muset přizpůsobit nastavení na vase_domena.com atribut domény. |
| enableCrossAppRedirects | Logická hodnota, která určuje, zda ověřené uživatele uloží, když přesměrován na adresy URL v jiných webových aplikací na stejném serveru. Výchozí hodnota je false. |
| loginUrl | Adresa URL přihlašovací stránky. Výchozí hodnota je login.aspx. |
| name | Při použití lístků pro ověřování na základě souborů cookie, název souboru cookie. Výchozí hodnota je. ASPXAUTH. |
| cesta | Při použití lístků pro ověřování na základě souborů cookie, toto nastavení určuje soubor cookie s atributu path. Atribut path umožňuje vývojáři k omezení oboru soubor cookie pro konkrétní adresář hierarchie. Výchozí hodnota je /, která informuje prohlížeč odeslat lístek ověřovacího souboru cookie na všechny žádosti do domény. |
| ochrana | Určuje, jaké postupy se používají k ochraně lístek pro ověřování pomocí formulářů. Povolené hodnoty jsou: všechny (výchozí); Šifrování; None; a ověření. Tato nastavení jsou podrobněji v kroku 3. |
| requireSSL | Logická hodnota, která určuje, jestli přenos ověřovacího souboru cookie vyžaduje připojení SSL. Výchozí hodnota je False. |
| parametr slidingExpiration | Logická hodnota, která určuje, že zda je časový limit souboru cookie s ověřování se vynuluje pokaždé, když uživatel navštíví web během jedné relace. Výchozí hodnota je true. Zásady ověřování lístkem časový limit je podrobněji popsána v zadání lístku s části hodnotu časového limitu. |
| Časový limit | Určuje dobu v minutách, po kterém vyprší platnost ověřovacího lístku souboru cookie. Výchozí hodnota je 30. Zásady ověřování lístkem časový limit je podrobněji popsána v zadání lístku s části hodnotu časového limitu. |

**Tabulka 1**: Souhrn A &lt;forms&gt; atributy elementu

V technologii ASP.NET 2.0 a nad výchozí hodnoty pro ověřování formuláře jsou pevně zakódovaná ve třídě FormsAuthenticationConfiguration v rozhraní .NET Framework. Všechny změny, musí být použita u jednotlivých aplikací aplikace v souboru Web.config. Tím se liší od ASP.NET 1.x, kde ověřování formulářů výchozí hodnoty byly uloženy v souboru machine.config (a proto je možné upravovat pomocí úpravy souboru machine.config). Chvíli na téma ASP.NET 1.x, je vhodné se zmínit, že počet nastavení systému ověřování pomocí formulářů mají různé výchozí hodnoty v technologii ASP.NET 2.0 a kromě než technologie ASP.NET 1.x. Pokud migrujete z prostředí ASP.NET 1.x vaší aplikace, je potřeba mít na paměti tyto rozdíly. Poraďte se [ &lt;forms&gt; technické dokumentace element](https://msdn.microsoft.com/en-us/library/1d3t3c61.aspx) seznam rozdíly.

> [!NOTE]
> Několik nastavení ověřování pomocí formulářů, jako je například časový limit, domény a cesty, zadejte podrobnosti pro výsledný soubor cookie lístek ověřování formulářů. Další informace o soubory cookie, funkce a jejich různé vlastnosti čtení [tohoto kurzu soubory cookie](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Pokud zadáte hodnotu časového limitu je lístek

Lístek pro ověřování pomocí formulářů je token, který představuje identita. U lístků pro ověřování na základě souborů cookie je tento token uchovávat ve formátu souboru cookie a odeslaný webový server na každý požadavek. U sebe tokenu, v podstatě deklaruje, jsem *uživatelské jméno*, již jste přihlášeni a slouží tak, aby identita uživatele může uživatel zadat napříč návštěv stránky.

Lístek pro ověřování pomocí formulářů nejen zahrnuje identitu uživatele, ale také obsahuje informace k zajištění integrity a zabezpečení tokenu. Po všech Neradi bychom mohli vytvořit token nepravým nebo upravit legit token nějakým způsobem underhanded nefarious uživatele.

Je jeden takový bit informací uvedených v lístku *vypršení platnosti*, což je datum a čas the-ticket je již neplatný. Pokaždé, když FormsAuthenticationModule zkontroluje lístku ověřování zajišťuje, že vypršení platnosti je lístek ještě neprošel. Pokud ano, ignoruje the-ticket a identifikuje jako anonymní uživatel. Tuto ochranu pomáhá chránit před útoky opětovného přehrání. Bez vypršela platnost, pokud se hacker bylo možné získat svůj rukou na uživatele platný ověřovací lístek - možná získání fyzického přístupu k jejich počítači a vytvoření kořenového adresáře prostřednictvím jejich souborů cookie – se může odeslat požadavek na server s tohoto lístku odcizené ověřování a získání položky. Při tomto scénáři není zabránit vypršení platnosti, omezit období, během kterého může uspět takové napadení.

> [!NOTE]
> Krok 3 techniky další podrobnosti, které se používá systém ověřování formulářů k ochraně lístek ověřování.


Při vytváření lístek ověřování, systém ověřování formulářů určuje jeho vypršení platnosti podle konzultace ohledně nastavení časového limitu. Jak jsme uvedli v tabulce 1, časový limit 30 minut, výchozí nastavení znamená, že když se vytvoří lístek pro ověřování pomocí formulářů jeho vypršení platnosti je nastavena na datum a čas v budoucnosti 30 minut.

Vypršení platnosti definuje absolutním čase v budoucnu při vypršení platnosti lístek pro ověřování pomocí formulářů. Ale obvykle vývojáři chcete implementovat klouzavé vypršení platnosti, ten, který se vynuluje pokaždé, když se uživatel znovu navštíví web. Toto chování je určen podle nastavení parametr slidingExpiration. Pokud je nastavena na hodnotu true (výchozí), pokaždé, když FormsAuthenticationModule ověřuje uživatele, aktualizuje lístku vypršení platnosti. Pokud není nastaven na hodnotu false, vypršení platnosti aktualizován na každý požadavek, a způsobuje lístku vyprší přesně časový limit počet minut po při lístek byl první vytvořen.

> [!NOTE]
> Vypršení platnosti uložené v lístku ověřování je absolutní datum a čas hodnotu, jako je 2 srpen 2008 11:34 AM. Datum a čas, kromě toho jsou relativní vzhledem k místní čas webového serveru. Tomuto rozhodnutí o návrhu může mít některé zajímavé vedlejší účinky kolem letní čas (letní čas), který je při hodiny ve Spojených státech amerických přesunou dopředu jednu hodinu (za předpokladu, že webový server je hostován v v národním prostředí, kde se zjištěnými letní čas). Zvažte, co by se stalo pro webové stránky ASP.NET s 30 minut vypršení téměř času začátku DST (což je ve 2:00). Představte si, že návštěvník přihlásí na web 11. března 2008 v 1:55:00. Tím se vygeneruje ověřovací lístek, kdy vyprší platnost na 11. března 2008 na 2:25 hodin (v budoucnu 30 minut). Ale po 2:00 AM vrátí kolem, hodiny přejde 3:00 AM kvůli letního času. Když uživatel načte nová stránka šest minut po přihlášení (na 3:01:00), FormsAuthenticationModule zaznamená, lístku vypršela platnost a přesměruje uživatele na přihlašovací stránku. Podrobnější diskuzi na tato a další oddities časový limit lístek ověřování, jakož i řešení, vyberte si kopii Stefan Schackow *Professional ASP.NET 2.0 zabezpečení, členství a Role správy* (ISBN: 978-0-7645-9698-8).


Obrázek 1 znázorňuje pracovní postup, pokud parametr slidingExpiration nastaven na hodnotu false a vypršení časového limitu je nastaven na 30. Všimněte si, že lístek ověřování vygenerována přihlášení obsahuje datum vypršení platnosti tato hodnota není aktualizován na následné žádosti. Pokud FormsAuthenticationModule zjistí, že platnost lístku vypršela, zahodí se a zpracovává žádost jako anonymní.


[![Grafické znázornění parametr slidingExpiration vypršení platnosti při lístek pro ověřování formulářů je hodnota false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**Obrázek 01**: grafické znázornění parametr slidingExpiration vypršení platnosti při lístek pro ověřování formulářů A má hodnotu false ([Kliknutím zobrazit obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


Obrázek 2 ukazuje pracovní postup, pokud parametr slidingExpiration nastaven na hodnotu true a vypršení časového limitu je nastavena na 30. Když je obdržena požadavek na ověřeného (s-platnost lístku) jeho vypršení platnosti se aktualizuje na časový limit počtu minut v budoucnu.


[![Grafické znázornění lístek pro ověřování formulářů Pokud je parametr slidingExpiration true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**Obrázek 02**: grafické znázornění lístek pro ověřování formulářů A Pokud je parametr slidingExpiration true ([Kliknutím zobrazit obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


Při použití ověřování na základě souborů cookie lístků (výchozí), stane toto pojednání o něco více matoucí, protože soubory cookie může mít vlastní expiries zadaný. Vypršení platnosti souboru cookie (nebo neexistenci) dostane pokyn, když je soubor cookie musí být zničený. V případě, že soubor cookie chybí vypršela platnost, byla při vypnutí prohlížeče. Pokud vypršela platnost je k dispozici, ale souboru cookie, který zůstane uložen v počítači uživatele do data a čas uvedený v vypršení platnosti byla úspěšná. Zničení souboru cookie v prohlížeči, je již nebude odeslána na webový server. Odstranění souboru cookie je proto podobá uživateli protokolování mimo lokalitu.

> [!NOTE]
> Samozřejmě uživatel může proaktivně odebrat všechny soubory cookie, uložené v počítači. V aplikaci Internet Explorer 7 by přejděte na nástroje, možnosti a klikněte na tlačítko Odstranit v části historie procházení. Zde klikněte na tlačítko Odstranit soubory cookie.


Systém forms ověřování vytvoří na základě relace nebo vypršení platnosti na základě souborů cookie v závislosti na hodnotě předané do *persistCookie* parametr. Pro vyvolání metody ověřování pomocí formulářů třída GetAuthCookie, SetAuthCookie a RedirectFromLoginPage trvat v dva vstupní parametry: *uživatelské jméno* a *persistCookie*. Přihlašovací stránka, kterou jsme vytvořili v předchozím kurzu zahrnuty políčko Zapamatovat uživatele, který určuje, zda byl vytvořen trvalého souboru cookie. Soubory cookie jsou založené na vypršení platnosti; dočasné soubory cookie jsou založené na relaci.

Časový limit a parametr slidingExpiration Principy probírané již stejné platí pro obě soubory cookie na základě relace a vypršení platnosti. Je pouze jeden dílčí rozdíl v provádění: při použití souborů cookie na základě vypršení platnosti slidingTimeout nastaveným na hodnotu true, vypršení platnosti souboru cookie se aktualizuje jenom když víc než poloviny zadané doby uplynul.

Umožňuje aktualizovat zásady vypršení časového limitu lístek ověřování náš web, který lístky časový limit za jednu hodinu (60 minut), pomocí klouzavé vypršení platnosti. Tato změna ovlivňuje, aktualizovat soubor Web.config, přidávání &lt;forms&gt; elementu, který chcete &lt;ověřování&gt; element s následující kód:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Pomocí adresy URL přihlašovací stránky než Login.aspx

Vzhledem k tomu, že FormsAuthenticationModule automaticky přesměruje na přihlašovací stránku neoprávnění uživatelé, je nutné znát adresu URL přihlašovací stránky. Tato adresa URL je určené atributem loginUrl v &lt;forms&gt; elementu a výchozí hodnota je login.aspx. Pokud provádíte přenos přes existující web, už můžete mít přihlašovací stránku s jinou adresu URL, který už aktuálně přihlášeni a indexovaný vyhledávači. Místo přejmenování vaší existující přihlašovací stránce stránce Login.aspx a nejnovější vazby a záložky uživatelů, můžete místo toho upravit atribut loginUrl tak, aby odkazoval na přihlašovací stránku.

Například pokud vaší přihlašovací stránce se s názvem SignIn.aspx a se nachází v adresáři uživatele, můžete ukázat loginUrl nastavení konfigurace pro ~/Users/SignIn.aspx takto:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

Vzhledem k tomu, že naše aktuální aplikace již má přihlašovací stránku s názvem Login.aspx, je potřeba zadat vlastní hodnotu v &lt;forms&gt; elementu.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Krok 2: Použití lístků pro ověřování pomocí formulářů bez souborů cookie

Ve výchozím nastavení systém ověřování formulářů určuje, zda se uložit jeho lístků pro ověřování v kolekci souborů cookie nebo je vložit do adresy URL založené na uživatelský agent na společnosti. Všechny všeobecně stolních počítačů, například Internet Explorer, Firefox, Opera a Safari, podporu souborů cookie, ale ne všechna mobilní zařízení udělat.

Zásady souborů cookie používá ověřování systému forms závisí na nastavení bez souborů cookie v &lt;forms&gt; element, který můžete přiřadit jednu ze čtyř hodnot:

- UseCookies - Určuje, že budou vždy použita lístků pro ověřování na základě souboru cookie.
- UseUri – označuje, že nebude nikdy používat lístků pro ověřování na základě souborů cookie.
- Automatické rozpoznání – Pokud profil zařízení nepodporuje soubory cookie, lístků pro ověřování na základě souborů cookie nepoužívají; Pokud je profil zařízení podporuje soubory cookie, zjišťovací mechanismus slouží k určení, pokud jsou povolené soubory cookie.
- UseDeviceProfile - výchozí; lístků pro ověřování na základě souborů cookie používá jenom v případě, že je profil zařízení podporuje. Žádný zjišťovací mechanismus se používá.

Nastavení automaticky rozpozná a UseDeviceProfile spoléhají na *profil zařízení* při určování, zda se má použít lístků pro ověřování na základě souborů cookie nebo bez souborů cookie. ASP.NET udržuje databázi různých zařízení a jejich funkce, například udává, jestli podporují soubory cookie, jaká verze jazyka JavaScript podporují a tak dále. Pokaždé, když zařízení požadavky na webové stránce z webového serveru odešle společně *uživatelský agent* hlavičky protokolu HTTP, který určuje typ zařízení. Zadaný identifikační řetězec ASP.NET automaticky shoduje s odpovídající profil zadaný ve své databázi.

> [!NOTE]
> Tato databáze možnosti zařízení uložena v počtu souborů XML, který dodržovat [prohlížeče definiční soubor schématu](https://msdn.microsoft.com/en-us/library/ms228122.aspx). Výchozí zařízení profil soubory jsou umístěny v % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Můžete také přidat vlastní soubory do vaší aplikace aplikace\_složky prohlížeče. Další informace najdete v tématu [postupy: zjištění typu prohlížeče na webových stránkách ASP.NET](https://msdn.microsoft.com/en-us/library/3yekbd5b.aspx).


Protože výchozí nastavení je UseDeviceProfile, lístků pro ověřování pomocí formulářů bez souborů cookie se použije při návštěvě webu zařízení, jehož profil hlásí, že nepodporuje soubory cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Kódování lístek ověřování v adrese URL

Soubory cookie jsou přirozené střední pro vkládání informací z prohlížeče v každé žádosti o pro určitý web, proto nastavení ověřování pomocí formulářů výchozí používat soubory cookie, pokud je hostujícími zařízení podporuje. Pokud soubory cookie nejsou podporovány, musí být použity alternativní způsob pro předávání lístek ověřování z klienta na server. Běžné řešení používá v prostředích bez souborů cookie je zakódovat data souborů cookie v adrese URL.

Nejlepší způsob, jak zobrazit, jak tyto informace mohou být vloženy do adresy URL je vynutit webu, aby používal lístků pro ověřování bez souborů cookie. To můžete udělat nastavením nastavení konfigurace bez souborů cookie na UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

Po provedení této změny, přejděte na web prostřednictvím prohlížeče. Při návštěvě jako anonymní uživatel, bude vypadat adresy URL, stejně jako tomu bylo před. Například při návštěvě stránku Default.aspx adresním řádku prohlížeče ukazuje na následující adrese URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Po přihlášení však lístek pro ověřování pomocí formulářů je vkládat do adresy URL. Například po návštěvou stránku pro přihlášení a přihlášení jako Sam, I se vrátíte na stránku Default.aspx, ale tato doba je adresa URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

V rámci adresy URL byla vložena lístek pro ověřování pomocí formulářů. Řetězec (F (jaIOIDTJxIr12xYS. VVgkqKCVAuIoW30Bu0diWi6flQC FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) představuje informace lístku ověřování kódováním šestnáctkově a je stejná data, která je obvykle uložen v souboru cookie.

V pořadí pro lístky ověřování bez souborů cookie pro práci v systému musí kódovat všechny adresy URL na stránce zahrnout data lístku ověřování, lístek ověřování budou jinak ztraceny, když uživatel klikne na odkaz. Naštěstí tato vnoření logika je realizována automaticky. K předvedení tuto funkci, otevřete stránku Default.aspx a přidání ovládacího prvku hypertextový odkaz, nastavení jeho vlastnosti textu a NavigateUrl na odkaz Test a SomePage.aspx, v uvedeném pořadí. Nezávisle na tom, že ve skutečnosti není k dispozici na stránce v našem projektu s názvem SomePage.aspx.

Uložit změny do Default.aspx a potom navštivte prostřednictvím prohlížeče. Přihlaste se k webu tak, aby v adrese URL se vloží lístek pro ověřování pomocí formulářů. V dalším kroku z Default.aspx, klikněte na odkaz Test odkaz. Co se stalo? Pokud žádná stránka s názvem SomePage.aspx existuje, pak došlo k chybě chybu 404, je to ale není co je důležité sem. Místo toho se zaměřit na panelu Adresa v prohlížeči. Všimněte si, že obsahuje lístek pro ověřování pomocí formulářů v adrese URL!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

Adresa URL SomePage.aspx v odkazu byla automaticky převést na adresu URL, která zahrnuté lístek ověřování - nebyly k dispozici pro zápis na ikonu kódu! Lístek ověřování formuláře budou vloženy automaticky adresu URL pro všechny hypertextové odkazy, které není začínat http:// nebo /. Není důležité, pokud se zobrazí hypertextový odkaz v volání Response.Redirect, v ovládacím prvku hypertextový odkaz nebo v element anchor HTML (tj, &lt;href = "..."&gt;... &lt;/a&gt;). Adresa URL není něco jako http://www.someserver.com/SomePage.aspx nebo /SomePage.aspx, budou vloženy lístek pro ověřování pomocí formulářů pro nás.

> [!NOTE]
> Lístků pro ověřování pomocí formulářů bez souborů cookie splňovat stejné zásady vypršení časového limitu jako lístků pro ověřování na základě souboru cookie. Ověřování bez souborů cookie lístků jsou však více náchylné k útoky opakováním vzhledem k tomu, že lístek ověřování vložené přímo v adrese URL. Představte si uživatel, který navštíví nějaký web, přihlášení a pak vloží adresu URL v e-mail s kolegy. Pokud kolegu klikne na tento odkaz před dosažením vypršení platnosti, budou se být přihlášení jako uživatel, který poslal e-mailu!


## <a name="step-3-securing-the-authentication-ticket"></a>Krok 3: Zabezpečení lístek ověřování

Lístek pro ověřování pomocí formulářů je odeslány prostřednictvím sítě buď do souboru cookie, nebo vložené přímo v rámci adresy URL. Kromě informací o identitě lístek ověřování může zahrnovat taky uživatelská data (jak jsme uvidí v kroku 4). V důsledku toho je důležité, aby byla zašifrovaná data lístku z nepovolaným a (ještě důležitější), forms ověřování systém nemůže zaručit, že lístek nebylo manipulováno.

Zajistit soukromí dat, je lístek ověřování systému forms můžete šifrovat data lístku. Nepodařilo se zašifrovat data lístku odešle potenciálně citlivé informace přes ve formátu prostého textu.

Pokud chcete zajistit pravosti lístek, musí systém ověřování formulářů *ověření* lístku. Ověření spočívá v zajištění, že se nezměnil konkrétním dat a se provádí prostřednictvím  *[zprávy (MAC) ověřovací kód](http://en.wikipedia.org/wiki/Message_authentication_code)*. Stručně řečeno MAC je malá část informace, které identifikuje data, která má být ověřen (v tomto případě the-ticket). Pokud data reprezentována MAC je změnit, pak nebude odpovídat MAC a data. Kromě toho je u pevných hackerům upravit data i generovat své vlastní MAC tak, aby odpovídaly upravené daty.

Při vytváření (nebo změny) lístek ověřování systému forms vytvoří MAC a připojí jej k datům lístku. Pokud následné požadavek dorazí, porovnávají forms ověřování systému MAC a lístek data k ověření pravosti data lístku. Obrázek 3 znázorňuje graficky tento pracovní postup.


[![Lístku pravosti je zajištěn prostřednictvím MAC](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**Obrázek 03**: pravosti The Ticket je zajištěn pomocí MACU ([Kliknutím zobrazit obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


Jaké bezpečnostní opatření se použijí pro lístek ověřování závisí na nastavení ochrany v &lt;forms&gt; elementu. Nastavení ochrany může být přiřazen jeden z následujících tří hodnot:

- Všechny - the-ticket je zašifrován a digitálně podepsané (výchozí).
- Šifrování - pouze šifrování se použije – žádné MAC se vygeneruje.
- Žádný - the-ticket je zašifrovaná ani digitálně podepsané.
- Ověření - MAC se vygeneruje, ale data lístku je odeslány prostřednictvím sítě ve formátu prostého textu.

Společnost Microsoft důrazně doporučuje, všechna nastavení.

### <a name="setting-the-validation-and-decryption-keys"></a>Nastavení ověřovací a dešifrovací klíče

Šifrování a systémem ověřování formulářů sloužící k šifrování a ověření lístek ověřování lze přizpůsobit pomocí [ &lt;machineKey&gt; element](https://msdn.microsoft.com/en-us/library/w8h3skw9.aspx) v souboru Web.config. Tabulka 2 jsou podrobněji popsány dále &lt;machineKey&gt; atributy elementu a jeho možných hodnot.

| **Atribut** | **Popis** |
| --- | --- |
| dešifrování | Určuje algoritmus použitý k šifrování. Tento atribut může mít jednu z následujících čtyř hodnot: - Auto - výchozí; Určuje algoritmus, na základě délky decryptionKey atributu. -Používá AES - [Advanced Encryption (Standard AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmus. -Šifrování DES - používá [Data Encryption (Standard DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) tento algoritmus se považuje za výpočetně slabé a by se neměla používat. Používá - 3DES - [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmus, který funguje s použitím algoritmu DES třikrát. |
| decryptionKey | Tajný klíč používaný šifrovacího algoritmu. Tato hodnota musí být buď šestnáctkového řetězce odpovídající délky (na základě hodnoty v dešifrování), generovat nebo buď hodnota spolu s, IsolateApps. Přidání IsolateApps dá pokyn, ASP.NET, aby použít jedinečnou hodnotu pro každou aplikaci. Výchozí hodnota je automaticky vytvořit, IsolateApps. |
| ověřování | Určuje algoritmus, použít pro ověření. Tento atribut může mít jednu z následujících čtyř hodnot: - AES - používá algoritmus Advanced Encryption (Standard AES). -Používá MD5 - [Message Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algoritmus. -Používá SHA1 - [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmus (výchozí). -3DES - používá algoritmus Triple DES. |
| validationKey | Tajný klíč, který používá algoritmus pro ověření. Tato hodnota musí být buď šestnáctkového řetězce odpovídající délky (na základě hodnoty ověřování), generovat nebo buď hodnota spolu s, IsolateApps. Přidání IsolateApps dá pokyn, ASP.NET, aby použít jedinečnou hodnotu pro každou aplikaci. Výchozí hodnota je automaticky vytvořit, IsolateApps. |

**Tabulka 2**: &lt;machineKey&gt; atributy prvků

Vyčerpávající diskusi o tyto možnosti šifrování a ověření a profesionálové a nevýhody různých algoritmů, je nad rámec tohoto kurzu. Pro podrobný podívejte se na tyto otázky, včetně pokyny k jaké algoritmy šifrování a ověření chcete použít, jaké délky klíčů, pokud chcete použít a jak nejlepší generování těchto klíčů, použijte *Professional ASP.NET 2.0 zabezpečení, členství a Role správy* .

Ve výchozím nastavení klíčů používaných pro šifrování a ověření jsou automaticky generovány pro každou aplikaci a tyto klíče jsou uložené v místní úřad zabezpečení (LSA). Stručně řečeno výchozí nastavení zaručit jedinečné klíče na webový server webový server a aplikace aplikaci. Toto výchozí chování v důsledku toho nebude fungovat pro dvě následující scénáře:

- **Webových farem** – v [webové farmy](http://en.wikipedia.org/wiki/Web_farm) scénáři jedné webové aplikace je hostitelem více webových serverů pro účely škálovatelnosti a redundance. Každého příchozího požadavku je odeslána na server ve farmě, znamená, že během životního cyklu relace uživatele, může být různé servery použít ke zpracování jeho různých požadavků. V důsledku toho každý server musí používat stejné šifrování a ověření klíče, aby vytvořit lístek pro ověřování pomocí formulářů, šifrovaná a ověřené na jeden server může dešifrovat a ověřit na jiném serveru ve farmě.
- **Mezi aplikace pro sdílení lístku** -jednom webovém serveru může hostovat více aplikací prostředí ASP.NET. Pokud budete potřebovat pro tyto různé aplikace sdílet jeden ověřovací lístek, je nutné, aby jejich šifrování a ověření klíče odpovídat.

Při práci ve webové farmě nastavení nebo sdílení lístků pro ověřování ve všech aplikacích na stejném serveru, budete muset nakonfigurovat &lt;machineKey&gt; element v ovlivněných aplikace tak, aby jejich decryptionKey a validationKey hodnoty odpovídat.

Pokud žádná z výše uvedených scénářích platí pro naše ukázková aplikace, jsme stále zadejte explicitní decryptionKey a hodnoty validationKey a definovat algoritmy, který se má použít. Přidat &lt;machineKey&gt; nastavení v souboru Web.config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

Další informace najdete na [postupy: Konfigurace MachineKey v technologii ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998288.aspx).

> [!NOTE]
> Hodnoty decryptionKey a validationKey byly odebrány z [Steve Gibson](http://www.grc.com/stevegibson.htm)na [ideální hesla webové stránky](https://www.grc.com/passwords.htm), které na každé stránce, navštivte generuje náhodné 64 hexadecimálních znaků. Chcete-li omezit pravděpodobnost, že tyto klíče provedením jejich představ do provozních aplikací, se doporučuje výše klíče nahraďte náhodně generované ty, které jsou na stránce ideální hesla.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Krok 4: Ukládání dat další uživatele v lístku

Mnoho webových aplikací zobrazit informace o nebo základní zobrazení stránky na aktuálně přihlášeného uživatele. Například na webové stránce může zobrazovat uživatelské jméno a datum, kdy se uživatel naposledy přihlášený v horním rohu každé stránky. Lístek pro ověřování pomocí formulářů ukládá uživatelské jméno aktuálně přihlášeného uživatele, ale v případě potřeby všechny ostatní informace stránce musí přejděte na Windows store uživatele – obvykle databázi – k vyhledávání informací nejsou uložené v lístek ověřování.

S chvilku kódu lze uložit informace o další uživatele v lístek pro ověřování pomocí formulářů. Taková data rozdíl lze vyjádřit pomocí [FormsAuthenticationTicket třída](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationticket.aspx)na [UserData vlastnost](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationticket.userdata.aspx). To je užitečné místo pro malé množství informací o uživateli, který je obvykle potřeba. Hodnota zadaná v UserData vlastnost je zahrnut jako součást souboru cookie pro lístek ověřování a, podobně jako v ostatních polích lístku, je zašifrovaná a ověřit podle konfigurace systému ověřování formulářů. Ve výchozím nastavení je UserData prázdný řetězec.

Chcete-li uložit data uživatele v lístku ověřování, musíme zápisu bit kódu na přihlašovací stránce, který získá informace specifické pro uživatele a uloží je v lístku. Vzhledem k tomu, že UserData je vlastnost typu řetězec, musí být data uložená v ní správně serializované jako řetězec. Představte si například, že naše úložiště uživatele zahrnuta každý uživatel datum narození a název svého zaměstnavatele a jsme chtěli uložit tyto dvě vlastnosti hodnoty lístek ověřování. Tyto hodnoty jsme může serializovat do řetězce zřetězením uživatele datum narození na řetězec s kanálu (|), za nímž následuje název zaměstnavatel. Pro uživatele narodila na 15. srpna 1974, který se dá použít pro Northwind Traders, jsme přiřazujete vlastnost UserData řetězec: 1974-08-15 | Northwind Traders.

Vždy, když je potřeba přístup k datům uloženým v lístku, provedeme tak metodou FormsAuthenticationTicket aktuální požadavek a deserializaci UserData vlastnost. V případě datum narození a zaměstnavatel název příkladu jsme by UserData řetězec rozdělit na dvě dílčích řetězců na základě oddělovače (|).


[![Další uživatelské informace mohou být uloženy v lístku ověřování](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**Obrázek 04**: další uživatele může být uložena v lístku ověřování ([Kliknutím zobrazit obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Informace o zápis do UserData

Bohužel se přidávání informace o uživateli do lístek ověřování formulářů není stejně jednoduché jako jeden by se dalo očekávat. Vlastnost UserData třídy FormsAuthenticationTicket je jen pro čtení a lze jej zadat pouze pomocí konstruktoru třídy FormsAuthenticationTicket. Při zadávání vlastnost UserData do konstruktoru, musíme také poskytují the-ticket je další hodnoty: uživatelské jméno, datum vystavení, vypršení platnosti a tak dále. Pokud jsme vytvořili přihlašovací stránky v předchozím kurzu, to byla všechny zajistit nám třídou ověřování pomocí formulářů. Při přidávání UserData do FormsAuthenticationTicket, jsme muset napsat kód pro replikovat většinu funkcí obsaženy ve třídě ověřování pomocí formulářů.

Podíváme se na kód potřebné pro práci s UserData aktualizací na stránku Login.aspx k zaznamenání Další informace o uživateli lístek ověřování. Předstírají, že, naše úložiště uživatele obsahuje informace o uživatel pracuje pro společnost a jejich funkce a že má být zaznamenat tyto informace v lístku ověřování. Aktualizujte obslužné rutiny události LoginButton klikněte na stránku Login.aspx tak, aby kód vypadá takto:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

Pojďme projděte tento kód řádek najednou. Metoda spustí definováním čtyři pole řetězce: uživatelé, hesla, NázevSpolečnosti a titleAtCompany. Tato pole uložení uživatelská jména, hesla, názvy společností a názvy uživatelských účtů v systému, ze které existují tři: Scott, Jisun a Sam. V reálné aplikaci by se tyto hodnoty získaných z úložiště uživatele, není pevně zakódovaná ve zdrojovém kódu stránky.

V předchozích kurzu Pokud zadaná pověření jsou platná jednoduše volali jsme FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked), která provádí následující kroky:

1. Vytvořit lístek pro ověřování formuláře
2. Napsali the-ticket vhodná úložiště. Kolekce souborů cookie v prohlížeči lístků pro ověřování na základě souborů cookie, je použít; pro ověřování bez souborů cookie lístky data lístku serializován do adresy URL
3. Přesměruje uživatele na příslušnou stránku

Tyto kroky jsou replikovány ve výše uvedeném kódu. Řetězec, který jsme se nakonec uloží ve vlastnosti UserData nejprve je tvořen kombinace názvu společnosti a názvu, omezující tyto dvě hodnoty s znak svislé čáry (|).

Dim userDataString As String = String.Concat(companyName(i), "|", titleAtCompany(i))

V dalším kroku FormsAuthentication.GetAuthCookie metoda je volána, který vytvoří lístek ověřování, šifruje a ověří podle nastavení konfigurace a umístí jej do objektu HttpCookie.

Dim authCookie jako HttpCookie = FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked)

Chcete-li pracovat s FormAuthenticationTicket vloženým do souboru cookie, je potřeba volat třídu FormAuthentication [dešifrovat metoda](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.decrypt.aspx), předejte v hodnotě souboru cookie.

Dim lístek jako FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value)

Pak vytvořte *nové* FormsAuthenticationTicket instance na základě existující FormsAuthenticationTicket hodnot. Tento nový lístek, ale obsahuje informace specifické pro uživatele (userDataString).

Dim newTicket jako FormsAuthenticationTicket = nové FormsAuthenticationTicket(ticket. Verze, lístku. Název, lístku. IssueDate, lístku. Vypršení platnosti lístku. IsPersistent, userDataString)

Jsme pak šifrování (a ověření) novou instanci FormsAuthenticationTicket voláním [šifrování metoda](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.encrypt.aspx)a tato data šifrovaná (a ověřené) zpět do authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket)

Nakonec authCookie je přidat do kolekce Response.Cookies a GetRedirectUrl metoda je volána k určení, na příslušnou stránku k odeslání uživatele.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

Všechny tohoto kódu je potřeba, protože vlastnost UserData je jen pro čtení a třída FormsAuthentication neposkytuje žádné metody pro zadávání informací UserData v jeho GetAuthCookie, SetAuthCookie nebo RedirectFromLoginPage metody.

> [!NOTE]
> Kód, který jsme právě zkontrolován ukládá informace o uživateli v lístek ověřování na základě souborů cookie. Zodpovědná za lístek pro ověřování pomocí formulářů na adresu URL, serializaci třídy jsou interní v rozhraní .NET Framework. Dlouhý text krátký, nemůžete uložit data uživatele lístek ověřování formulářů bez souborů cookie.


### <a name="accessing-the-userdata-information"></a>Přístup k informacím o UserData

V tomto okamžiku název společnosti a název každého uživatele je uložené v lístek pro ověřování formulářů UserData vlastnosti při přihlášení. Tyto informace jsou přístupné z lístek ověřování na libovolné stránce bez nutnosti cestě k úložišti uživatele. Pro ilustraci, jak můžete tyto informace načíst z vlastnosti UserData, můžeme aktualizovat Default.aspx tak, že jeho uvítací zprávy obsahují nejen jméno uživatele, ale také společnosti, pro kterou pracují a jejich funkce.

V současné době Default.aspx obsahuje Panel AuthenticatedMessagePanel pomocí ovládacího prvku popisek s názvem WelcomeBackMessage. Tento Panel se zobrazuje pouze ověřeným uživatelům. Aktualizujte kód Default.aspx na stránce\_načíst obslužné rutiny události tak, aby vypadal jako následující:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Pokud Request.IsAuthenticated má hodnotu True, pak WelcomeBackMessage textu je nejdřív nastavena na vás vítá zpět *uživatelské jméno*. Pak vlastnost User.Identity je převést na objekt FormsIdentity, aby jsme přístup základní FormsAuthenticationTicket. Jakmile jsme FormsAuthenticationTicket, jsme deserializovat UserData vlastnost na název společnosti a název. To se provádí rozdělení řetězce na znakem. Název společnosti a název se zobrazí v WelcomeBackMessage popisku.

Obrázek 5 ukazuje snímek obrazovky toto zobrazení v akci. Přihlášení jako Scott zobrazí Vítá zpět zprávu, která obsahuje Scott pro společnosti a název.


[![Zobrazí se společnosti a názvu aktuálně přihlášení na uživatele](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**Obrázek 05**: Zobrazí se současné době protokolují na uživatele společnosti a název ([Kliknutím zobrazit obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> Vlastnost UserData lístek ověřování slouží jako mezipaměť pro úložiště uživatelů. Stejně jako všechny mezipaměť musí aktualizovat při změně podkladového data. Například pokud je na webové stránce, ze kterého mohou uživatelé aktualizovat svůj profil, pole uložené v mezipaměti v vlastnost UserData musí aktualizovat, aby se projevily změny provedené uživatelem.


## <a name="step-5-using-a-custom-principal"></a>Krok 5: Pomocí objekt vlastní

Na jednotlivých příchozích požadavků FormsAuthenticationModule pokusí ověřit uživatele. Pokud se nachází lístek ověřování – platnost vypršela, FormsAuthenticationModule Vlastnost HttpContext.User přiřadí nový objekt GenericPrincipal. Tento objekt GenericPrincipal má identitu typu FormsIdentity, který obsahuje odkaz na lístek pro ověřování pomocí formulářů. GenericPrincipal – třída obsahuje funkci úplné minimální vyžaduje třídu, která implementuje IPrincipal – stačí má vlastnost Identity a metodu IsInRole.

Objekt zabezpečení má dva odpovědnosti: označíte, jaké role uživatel patří a k poskytování informací o identitě. To se provádí prostřednictvím rozhraní IPrincipal IsInRole (*roleName*) metody a vlastnosti Identity, v uvedeném pořadí. Třídy GenericPrincipal pole řetězců názvů rolí umožňuje zadat přes jeho konstruktoru; jeho IsInRole (*roleName*) metoda jenom kontroluje, zda předaná v *roleName* existuje v poli řetězců. Když FormsAuthenticationModule vytvoří objekt GenericPrincipal, předá se v matici prázdný řetězec GenericPrincipal konstruktoru. V důsledku toho budou všechny volání IsInRole vždy vrátí hodnotu False.

Třídy GenericPrincipal splňuje potřeby pro většinu scénářů ověřování založené na formulářích, nejsou-li použity role. Pro tyto situace, kdy je nedostatečná zpracování role výchozí nebo když potřebujete přidružit vlastní objekt identita uživatele, můžete vytvořit vlastní objekt IPrincipal během ověřování pracovního postupu a přiřadit k vlastnosti HttpContext.User.

> [!NOTE]
> Jak jsme se zobrazí v budoucnosti kurzy, když ASP. Je povolena na NET framework role vytvoří vlastní objekt zabezpečení typu [RolePrincipal](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.aspx) a přepíše objekt GenericPrincipal vytvořené ověřování formulářů. Dělá to chcete-li přizpůsobit IsInRole metodu objektu zabezpečení pro rozhraní s rolí rozhraní API.


Vzhledem k tomu, že jsme nebyly problémem označována role ještě, pouze důvod, který jsme by měla pro vytvoření vlastní objekt zabezpečení v této situaci řešit by k přidružení vlastního identita objektu na objekt zabezpečení. V kroku 4 jsme se podívali na ukládání další uživatelské informace ve vlastnosti UserData lístek ověřování, v konkrétní název společnosti uživatele a jejich funkce. Informace UserData je však pouze přístupné prostřednictvím lístek ověřování a potom pouze jako serializovaný řetězec, což znamená, kdykoliv chceme, chcete-li zobrazit informace o uživateli, které jsou uložené v lístku je potřeba analyzovat UserData vlastnost.

Můžeme vylepšit vývojáře vytvořením třídy, který implementuje identita a zahrnují NázevSpolečnosti a název vlastnosti. Tímto způsobem vývojář přístup k název společnosti aktuálně přihlášeného uživatele a název přímo přes NázevSpolečnosti a název vlastnosti bez potřeby vědět, jak analyzovat UserData vlastnost.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Vytváření vlastní identitu a hlavní třídy

V tomto kurzu vytvoříme vlastní objekty zabezpečení a identity v aplikaci\_složky kódu. Začněte přidáním aplikace\_kód složky do projektu – klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, vyberte možnost Přidat složku ASP.NET a vyberte aplikaci,\_kódu. Aplikace\_kód složka je speciální složky ASP.NET, který obsahuje soubory specifické pro web – třída.

> [!NOTE]
> Aplikace\_kód složky musí být použit pouze při správě projektu prostřednictvím modelu projektu webu. Pokud používáte [Model projektu webové aplikace](https://msdn.microsoft.com/en-us/asp.net/Aa336618.aspx), vytvořte standardní složku a přidejte do třídy. Například můžete přidat novou složku s názvem třídy a umístěte kódu existuje.


Dál přidejte dva nové soubory – třída aplikace\_složky kódu, jednu s názvem CustomIdentity.vb a jednu s názvem CustomPrincipal.vb.


[![Do projektu přidejte CustomIdentity a CustomPrincipal třídy](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**Obrázek 06**: přidejte do projektu knihovny CustomIdentity a CustomPrincipal třídy ([Kliknutím zobrazit obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


Třída CustomIdentity je zodpovědná za implementaci rozhraní identita, která definuje AuthenticationType, IsAuthenticated a název vlastnosti. Kromě těchto požadované vlastnosti jsou zajímat vystavení na základní lístek pro ověřování pomocí formulářů a také vlastnosti pro název společnosti a název uživatele. Zadejte následující kód do třídy CustomIdentity.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

Všimněte si, že třída zahrnuje FormsAuthenticationTicket členské proměnné (\_lístku) a že je nutné zadat tyto informace lístku pomocí konstruktoru. Tato data lístku se používají při vracení identity název; jeho vlastnost UserData je analyzována na návratové hodnoty pro vlastnosti NázevSpolečnosti a funkce.

Dále vytvořte třídu CustomPrincipal. Vzhledem k tomu, že jsme nejsou nevadí role v této situaci řešit, konstruktoru třídy CustomPrincipal přijímá pouze objekty CustomIdentity; jeho IsInRole metoda vždy vrátí hodnotu False.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Přiřazování objekt CustomPrincipal kontext zabezpečení příchozího požadavku

Máme teď třídu, která rozšiřuje specifikace výchozí identita zahrnout NázevSpolečnosti a název vlastnosti, jakož i vlastní hlavní třídu, která používá vlastní identity. Jsme připravení krokování s vnořením kanálu ASP.NET a přiřaďte naše vlastní objekt zabezpečení kontext zabezpečení příchozího požadavku.

Kanálu ASP.NET přijímá příchozí žádosti a zpracovává je pomocí několika kroků. Při každém kroku je vyvolána určitá událost, která umožňuje vývojářům procházet do kanálu ASP.NET a upravovat žádosti v určitých bodech, v jeho průběhu životního cyklu. FormsAuthenticationModule, například čeká na technologii ASP.NET vyvolat [AuthenticateRequest událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx), v tomto okamžiku se kontroluje příchozí požadavek na ověřovací lístek. Pokud je na ověřovací lístek, objektů GenericPrincipal je vytvořen a přiřazeno k vlastnosti HttpContext.User.

Po události AuthenticateRequest kanálu ASP.NET vyvolá [PostAuthenticateRequest událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.postauthenticaterequest.aspx), což je, kde jsme můžete nahradit GenericPrincipal objekt vytvořený FormsAuthenticationModule s instancí naše Objekt CustomPrincipal. Obrázek 7 znázorňuje tento pracovní postup.


[![Objekt GenericPrincipal je nahrazena CustomPrincipal v PostAuthenticationRequest události](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**Obrázek 07**: GenericPrincipal je nahrazena CustomPrincipal v události PostAuthenticationRequest ([Kliknutím zobrazit obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


Provádění kódu v reakci na událost kanálu ASP.NET, nemůžeme vytvořit obslužnou rutinu odpovídající události v souboru Global.asax nebo vytvořit vlastní modul HTTP. Pro účely tohoto kurzu vytvoříme obslužné rutiny události v souboru Global.asax. Začněte přidáním Global.asax na váš web. Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a přidejte položky typu globální třídy aplikace s názvem souboru Global.asax.


[![Přidejte soubor Global.asax na váš web](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**Obrázek 08**: Přidá soubor Global.asax na webu ([Kliknutím zobrazit obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


Výchozí šablony Global.asax obsahuje obslužné rutiny události pro určitý počet událostí kanálu ASP.NET, včetně Start a End a [událost chyby](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.error.aspx), mimo jiné. Můžete bez obav odstranit tyto obslužné rutiny událostí, jak jsme je není nutné pro tuto aplikaci. Události, které jsme zajímají je PostAuthenticateRequest. Aktualizujte si soubor Global.asax tak svůj kód bude vypadat podobně jako následující:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

Aplikace\_OnPostAuthenticateRequest metoda spustí pokaždé, když modulem runtime ASP.NET vyvolává událost PostAuthenticateRequest, který se stane jednou na každou příchozí požadavek na stránku. Obslužné rutiny události spustí kontrolou, zda uživatel je ověřen a byl ověřen pomocí ověřování pomocí formulářů. Pokud ano, nový objekt CustomIdentity je vytvořen a aktuální žádost ověřovací lístek předaná jeho konstruktoru. Objekt CustomPrincipal následující, je vytvořen a předaný objekt CustomIdentity vytvořeném v jeho konstruktoru. Nakonec aktuální požadavek kontext zabezpečení je přiřazený k nově vytvořený objekt CustomPrincipal.

Všimněte si, že poslední krok - přiřazení objektu CustomPrincipal kontext zabezpečení žádosti - přiřadí dvě vlastnosti objektu zabezpečení: HttpContext.User a Thread.CurrentPrincipal. Tyto dvě přiřazení jsou nezbytné kvůli způsob, jakým kontexty zabezpečení jsou zpracovávány v technologii ASP.NET. Rozhraní .NET Framework přidruží kontextu zabezpečení jednotlivých spuštěných vláken; Tyto informace jsou k dispozici jako objekt IPrincipal prostřednictvím [objektu vlákna](https://msdn.microsoft.com/en-us/library/system.threading.thread.aspx)na [vlastnost CurrentPrincipal](https://msdn.microsoft.com/en-us/library/system.threading.thread.currentcontext.aspx). Co je složitá je, že technologie ASP.NET má svou vlastní informace kontextu zabezpečení (HttpContext.User).

V některých scénářích je zkontrolován vlastnost Thread.CurrentPrincipal při určování kontext zabezpečení; v jiných scénářích se používá HttpContext.User. Například existují funkce zabezpečení v rozhraní .NET, které umožňují vývojářům deklarativně stavu jaké uživatelů nebo rolí můžete vytvořit instanci třídy nebo vyvolat konkrétní metody (viz [přidáním autorizačních pravidel pro firmy a Data pomocí vrstev. PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Pod zahrnuje určete tyto deklarativní techniky kontext zabezpečení přes Thread.CurrentPrincipal vlastnost.

V jiných scénářích se používá vlastnost HttpContext.User. Například v předchozí kurzu jsme použili tuto vlastnost zobrazit uživatelské jméno aktuálně přihlášeného uživatele. Je zřejmé pak je nutné, aby informace kontextu zabezpečení ve vlastnostech Thread.CurrentPrincipal a HttpContext.User odpovídat.

Modul runtime ASP.NET automaticky synchronizuje se tyto hodnoty vlastností pro nás. Ale k této synchronizace po AuthenticateRequest události, ale *před* PostAuthenticateRequest události. V důsledku toho při přidávání vlastní objekt zabezpečení v události PostAuthenticateRequest potřebujeme mít jistotu, ručně přiřadit Thread.CurrentPrincipal Thread.CurrentPrincipal, jinak a HttpContext.User bude synchronizován. V tématu [Context.User vs. Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) podrobné podrobnější informace o tento problém.

### <a name="accessing-the-companyname-and-title-properties"></a>Přístup k NázevSpolečnosti a název vlastnosti

Vždy, když dorazí požadavek a je odeslaných do modulu ASP.NET, aplikace\_bude platit obslužné rutiny události OnPostAuthenticateRequest v souboru Global.asax. Pokud požadavek byl úspěšně ověřen pomocí FormsAuthenticationModule, obslužné rutiny události vytvoří nový objekt CustomPrincipal s objektem CustomIdentity podle lístek pro ověřování pomocí formulářů. Pomocí této logiky na místě přístup k informacím o název společnosti a názvu aktuálně přihlášeného uživatele je velmi jednoduchá.

Návrat na stránku\_zatížení obslužné rutiny událostí v Default.aspx, kde v kroku 4, jsme napsali kód lístek ověřování formuláře načíst a analyzovat vlastnost UserData aby bylo možné zobrazit název společnosti a název uživatele. S objekty CustomPrincipal a CustomIdentity teď používá není třeba analyzovat hodnoty z vlastnosti UserData lístku. Místo toho jednoduše získat odkaz na objekt CustomIdentity a použít jeho NázevSpolečnosti a název vlastnosti:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>Souhrn

V tomto kurzu jsme se zaměřili na tom, jak přizpůsobit nastavení systému ověřování formulářů pomocí souboru Web.config. Jsme se podívali na zpracování lístek ověřování vypršení platnosti a použití šifrování a ověření bezpečnostní opatření k ochraně lístku z kontroly a úpravy. Nakonec jsme probrali pomocí vlastnosti UserData lístek ověřování k uložení informace o další uživatele v lístku sám a jak používat vlastní objekty zabezpečení a identity vystavit tyto informace způsobem vývojáře popisný.

V tomto kurzu se ukončí součást ověřování pomocí formulářů technologie ASP.NET. V dalším kurzu spustí naše cesty do rozhraní členství.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Rozvěrače ověřování pomocí formulářů](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Vysvětlení: Ověřování pomocí formulářů technologie ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/aa480476.aspx)
- [Postupy: Ochrana ověřování pomocí formulářů technologie ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998310.aspx)
- [Professional ASP.NET 2.0 zabezpečení, členství a Role správy](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zabezpečení přihlašovacích ovládacích prvků](https://msdn.microsoft.com/en-us/library/ms178346.aspx)
- [&lt;Ověřování&gt; – Element](https://msdn.microsoft.com/en-us/library/532aee0e.aspx)
- [&lt;Forms&gt; Element pro &lt;ověřování&gt;](https://msdn.microsoft.com/en-us/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt; – Element](https://msdn.microsoft.com/en-us/library/w8h3skw9.aspx)
- [Pochopení lístek pro ověřování pomocí formulářů a souboru Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video školení na témata, které jsou obsažené v tomto kurzu

- [Postup změny vlastnosti ověřování formulářů](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Postup instalace a používání souborů Cookie bez ověřování v aplikaci ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [ASP Forms přihlášení přemístění](../../../videos/authentication/asp-forms-login-relocation.md)
- [Forms přihlášení vlastní klíče konfigurace](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Přidat vlastní Data na metody ověřování](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Použití vlastní hlavní objekty](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Alicja Maziarz. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Předchozí](an-overview-of-forms-authentication-vb.md)
