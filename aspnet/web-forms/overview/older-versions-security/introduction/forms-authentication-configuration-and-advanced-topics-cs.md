---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Ověření konfigurace formuláře a pokročilá témata (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu budeme zkoumat různých nastavení ověřování pomocí formulářů a zjistit, jak upravovat pomocí prvku formuláře. To bude mít za následek na konkrétní...
ms.author: aspnetcontent
ms.date: 01/14/2008
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: 3bdbcd5e8f8890fe3fae2063d6f8a44aa8bb5b1e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833268"
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Konfigurace ověřování formuláře a pokročilá témata (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> V tomto kurzu budeme zkoumat různých nastavení ověřování pomocí formulářů a zjistit, jak upravovat pomocí prvku formuláře. To bude mít za následek podrobný přehled o přizpůsobení hodnota časového limitu lístek ověřování formuláře pomocí vlastní adresu URL (například SignIn.aspx místo Login.aspx) a lístků pro ověřování pomocí formulářů bez souborů cookie na přihlašovací stránce.


## <a name="introduction"></a>Úvod

V [předchozí kurz o službě](an-overview-of-forms-authentication-cs.md) jsme se podívali na kroky potřebné pro implementaci ověřování pomocí formulářů v aplikaci ASP.NET z nastavení konfigurace v souboru Web.config pro vytvoření protokolu na stránku pro zobrazení různých obsah pro ověřený a anonymní uživatele. Připomínáme, že jsme nakonfigurovali web tak, že nastavíte atribut režimu použít ověřování pomocí formulářů &lt;ověřování&gt; element do formulářů. &lt;Ověřování&gt; element může volitelně zahrnovat &lt;forms&gt; podřízený element, jehož prostřednictvím je možné zadat celé řady různých doprovodných nastavení ověřování pomocí formulářů.

V tomto kurzu budeme zkoumat různých nastavení ověřování pomocí formulářů a zjistit, jak je prostřednictvím upravit &lt;forms&gt; elementu. To bude mít za následek podrobný přehled o přizpůsobení hodnota časového limitu lístek ověřování formuláře pomocí vlastní adresu URL (například SignIn.aspx místo Login.aspx) a lístků pro ověřování pomocí formulářů bez souborů cookie na přihlašovací stránce. Také jsme strukturu ověřovací lístek prozkoumat podrobněji a najdete v článku opatření, které má technologie ASP.NET tak, aby byl lístku dat zabezpečení z kontroly a manipulaci s. Nakonec se podíváme na tom, jak ukládat další uživatelská data v ověřovací lístek a jak tato data pomocí vlastní objekt modelu.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Krok 1: Zkoumání &lt;forms&gt; nastavení konfigurace

Systémem ověřování pomocí formulářů v ASP.NET nabízí celou řadu nastavení konfigurace, které je možné přizpůsobit na základě aplikace v aplikaci. To zahrnuje nastavení, jako je: životnosti ověřování založené na formulářích lístku; Jaký druh ochrana se použije pro-the-ticket; v části ověřování bez souborů cookie jaké podmínky se používají lístky; cesta na přihlašovací stránku. a další informace. Chcete-li změnit výchozí hodnoty, přidejte [ &lt;formuláře&gt; element](https://msdn.microsoft.com/library/1d3t3c61.aspx) jako podřízený objekt [ &lt;ověřování&gt; element](https://msdn.microsoft.com/library/532aee0e.aspx), zadáním těchto vlastností hodnoty, které chcete přizpůsobit jako atributy XML takto:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Tabulka 1 shrnuje vlastnosti, které je možné přizpůsobit pomocí &lt;forms&gt; elementu. Vzhledem k tomu, že soubor Web.config je soubor XML, názvy atributů v levém sloupci jsou malá a velká písmena.


| <strong>Atribut</strong> |                                                                                                                                                                                                                                     <strong>Popis</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         bez souborů cookie         |                                                                                                                Tento atribut určuje, za jakých podmínek je uložen ověřovací lístek do souboru cookie a jeho vkládání v adrese URL. Povolené hodnoty jsou: UseCookies; UseUri; Automatické rozpoznávání; a UseDeviceProfile (výchozí). Krok 2 prozkoumá podrobněji toto nastavení.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Určuje adresu URL, kterou uživatelé přesměrovaní na po přihlášení z přihlašovací stránky, pokud neexistuje žádná hodnota RedirectUrl podle řetězec dotazu. Výchozí hodnota je default.aspx.                                                                                                                                                         |
|           doména           | Při použití lístků pro ověřování na základě souboru cookie, toto nastavení určuje hodnota domény souboru cookie. Výchozí hodnota je prázdný řetězec, který způsobí, že prohlížeč doménu, ve kterém byl vydán (například www.yourdomain.com). V takovém případě bude soubor cookie <strong>není</strong> odešlou při posílání požadavků na subdomén, třeba admin.yourdomain.com. Pokud chcete soubor cookie se mají předat všechny subdomény, které je třeba přizpůsobit nastavení na vasedomena.com atribut domény. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Logická hodnota označující, zda ověřené uživatele uloží, při přesměrování na adresy URL v další webové aplikace na stejném serveru. Výchozí hodnota je false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Adresa URL přihlašovací stránky. Výchozí hodnota je login.aspx.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Při použití lístků pro ověřování na základě souboru cookie, název souboru cookie. Výchozí hodnota je. ASPXAUTH.                                                                                                                                                                                                   |
|            cesta            |                                                                             Při použití lístků pro ověřování na základě souboru cookie, toto nastavení určuje atribut cesty souboru cookie. Atribut cesty umožňuje vývojáři omezit obor souborů cookie do konkrétního adresářového hierarchie. Výchozí hodnota je /, který informuje prohlížeče k odeslání ověřovacího lístku souboru cookie na všechny žádosti do domény.                                                                              |
|         ochrana         |                                                                                                                                            Určuje, jaké postupy se používají k ochraně lístku ověřování formulářů. Povolené hodnoty jsou: všechny (výchozí); Šifrování. None; a ověřování. Tato nastavení jsou podrobně popsány v kroku 3.                                                                                                                                            |
|         Vlastnost requireSSL         |                                                                                                                                                                                Logická hodnota, která určuje, jestli je k přenosu ověřovacího souboru cookie vyžaduje připojení SSL. Výchozí hodnota je False.                                                                                                                                                                                |
|     parametr slidingExpiration      |                                                                                                 Logická hodnota určující, že zda vypršení časového limitu ověřovacího souboru cookie se vynuluje pokaždé, když uživatel navštíví web během jedné relace. Výchozí hodnota je true. Zásady vypršení časového limitu lístek ověřování je podrobněji popsána podrobněji zadání lístku hodnotu časového limitu oddílu.                                                                                                 |
|          časový limit           |                                                                                                                               Určuje dobu v minutách, po jejichž uplynutí vyprší platnost ověřovacího lístku souboru cookie. Výchozí hodnota je 30. Zásady vypršení časového limitu lístek ověřování je podrobněji popsána podrobněji zadání lístku hodnotu časového limitu oddílu.                                                                                                                               |

**Tabulka 1**: přehled A &lt;forms&gt; atributy elementu

V technologii ASP.NET 2.0 a nad výchozí hodnoty pro ověřování formuláře jsou pevně zakódovaný ve třídě FormsAuthenticationConfiguration v rozhraní .NET Framework. Všechny změny se musí použít na základě aplikace aplikace v souboru Web.config. Tím se liší od ASP.NET 1.x, kde výchozí formuláře ověřování hodnoty byly uloženy v souboru machine.config (a proto je možné upravovat prostřednictvím úpravy souboru machine.config). Při na téma ASP.NET 1.x, je vhodné uvést, že celou řadu nastavení systému ověřování formuláře mají různé výchozí hodnoty v technologii ASP.NET 2.0 i mimo ně než v technologii ASP.NET 1.x. Pokud provádíte migraci aplikace z 1.x prostředí ASP.NET, je potřeba mít na paměti tyto rozdíly. Poraďte [ &lt;formuláře&gt; element technickou dokumentaci](https://msdn.microsoft.com/library/1d3t3c61.aspx) seznam rozdílů.

> [!NOTE]
> Několik nastavení ověřování pomocí formulářů, jako je například vypršení časového limitu, domény a cesty, zadejte podrobnosti pro výsledný formulářů ověřovacího lístku souboru cookie. Další informace o souborech cookie, jak fungují a jejich různé vlastnosti čtení [v tomto kurzu soubory cookie](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Zadání lístku hodnotu časového limitu

Lístek ověřování pomocí formulářů je token, který reprezentuje identitu. Tento token lístků pro ověřování na základě souboru cookie, je uložená ve formě souboru cookie a odeslané na webový server s každým požadavkem. Vlastnictví tokenu, je v podstatě deklaruje, já jsem *uživatelské jméno*, již jste přihlášeni a se používá tak, aby identitu uživatele může uživatel zadat napříč návštěv stránky.

Ověřovací lístek nejen obsahuje identitu uživatele, ale také obsahuje informace k zajištění integrity a zabezpečení z tokenu. Po všech nechceme neslouží pro nekalé účely uživatele, abyste mohli vytvořit token nepravý, nebo pro úpravu legit token nějakým způsobem underhanded.

Je jeden takový bit informace obsažené v lístku *vypršení platnosti*, což je datum a čas-the-ticket již není platný. Pokaždé, když FormsAuthenticationModule kontroluje ověřovací lístek, zajišťuje, že vypršení platnosti lístku nebyl dosud předán. Pokud ano, ignoruje-the-ticket a identifikuje jako anonymní uživatel. Tato ochrana pomáhá chránit před útoky před zneužitím. Bez vypršení, pokud se hacker se podařilo získat jeho praktická platný ověřovací lístek uživatele – možná získat fyzický přístup k jejich počítači a vytvoření kořenového adresáře prostřednictvím jejich soubory cookie – může odeslat požadavek na server s této odcizeného ověřovací lístek a získáte položku. Při vypršení platnosti nezabrání tento scénář, neomezuje okno, během kterého může uspět takového útoku.

> [!NOTE]
> Krok 3 podrobnosti o další techniky použít k ochraně lístku ověřování systémem ověřování formulářů.


Při vytváření lístku ověřování, systém ověřování formulářů určuje jeho vypršení platnosti o nastavení časového limitu. Jak je uvedeno v tabulce 1, časový limit nastavení výchozí hodnoty na 30 minut, což znamená, že když se vytvoří lístek ověřování pomocí formulářů jeho vypršení platnosti nastavený na datum a čas v budoucnosti 30 minut.

Vypršení platnosti definuje absolutním čase v budoucnosti kdy vyprší platnost lístku ověřování formulářů. Ale obvykle vývojáři chtějí implementovat klouzavé vypršení platnosti, který se vynuluje pokaždé, když se uživatel znovu navštíví web. Toto chování je určena nastavením slidingExpiration. Pokud je nastavena na hodnotu true (výchozí) pokaždé, když FormsAuthenticationModule ověřuje uživatele, aktualizuje vypršení platnosti lístku. Pokud není nastaven na hodnotu false, vypršení platnosti aktualizován na každý požadavek, a způsobuje-the-ticket vyprší přesně vypršení časového limitu počet minut po kdy byly první-the-ticket vytvořen.

> [!NOTE]
> Vypršení platnosti uložené v lístku ověřování je absolutní hodnotu data a času, jako je 2. srpna 2008 11:34 dop. Kromě toho data a času jsou relativní vzhledem k místní čas webového serveru. Toto rozhodnutí o návrhu může mít některé zajímavé vedlejší účinky kolem letního času (DST), což je při hodiny ve Spojených státech amerických dopředu Přesunutí jedné hodiny (za předpokladu, že webový server je hostovaný v národním prostředí, ve kterém se vyskytuje letní čas). Zvažte, co by se stalo pro webové stránky ASP.NET 30 minut vypršení platnosti v čase, který začíná letního času (což je ve 2:00). Představte si, že návštěvník přihlásí k webu 11. března 2008 v 1:55:00. To by generovat ověřovací lístek, jejíž platnost vyprší za 11. března 2008 na 2:25:00 (30 minut do budoucna). Ale po 2:00 hodin se zobrazí kolem, hodiny vrací na 3:00:00 kvůli letního času. Pokud uživatel načte nová stránka šest minut po přihlášení (na 3:01:00), FormsAuthenticationModule zaznamená, že vypršela platnost-the-ticket a přesměruje uživatele na přihlašovací stránku. Podrobnější informace o to a dalších oddities vypršení časového limitu lístek ověřování, jakož i řešení, vyberte si kopii Stefan Schackow *Professional ASP.NET 2.0 zabezpečení, členství a rolí správy* (ISBN: 978-0-7645-9698-8).


Obrázek 1 znázorňuje pracovní postup, pokud parametr slidingExpiration nastaven na hodnotu false a časový limit se nastavuje na 30. Všimněte si, že lístek ověřování, které jsou generovány při přihlášení obsahuje datum vypršení platnosti a tato hodnota není aktualizován na následné žádosti. Pokud FormsAuthenticationModule zjistí, že platnost lístku vypršela, zahodí ji a zpracovává žádost jako anonymní.


[![Grafická reprezentace lístek ověřování formulářů vypršení platnosti při slidingExpiration má hodnotu false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Obrázek 01**: A grafická reprezentace lístek ověřování formulářů vypršení platnosti při slidingExpiration má hodnotu false ([kliknutím ji zobrazíte obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


Obrázek 2 ukazuje pracovní postup, pokud parametr slidingExpiration nastaven na hodnotu true a vypršení časového limitu je nastaven na 30. Při přijetí ověřeného požadavku (s-vypršela platnost lístku) jeho vypršení platnosti se aktualizuje a vypršení časového limitu počtu minut do budoucna.


[![Grafická reprezentace lístek ověřování pomocí formulářů Pokud je parametr slidingExpiration true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Obrázek 02**: Grafická reprezentace lístek ověřování formulářů A Pokud je parametr slidingExpiration true ([kliknutím ji zobrazíte obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Při použití lístků pro ověřování na základě souboru cookie (výchozí), stane této diskuse o něco více matoucí, protože soubory cookie může mít také vlastní expiries zadaný. Vypršení platnosti souboru cookie (nebo neexistenci) dostane pokyn při by měl být soubor cookie zničení. Pokud chybí soubor cookie vypršení platnosti, byla při vypnutí v prohlížeči. Je-li vypršení platnosti je k dispozici, ale souboru cookie, který zůstane uložen v počítači uživatele do data a času vypršení platnosti podle. Při zničení soubor cookie v prohlížeči, už nebude odeslán na webový server. Odstranění souboru cookie je proto obdobná uživateli protokolování mimo lokalitu.

> [!NOTE]
> Samozřejmě uživatele může aktivně odebrat všechny soubory cookie uložené ve svém počítači. V aplikaci Internet Explorer 7 bude přejděte na nástroje, možnosti a klikněte na tlačítko Odstranit v části historie procházení. Odtud klikněte na tlačítko Odstranit soubory cookie.


Vytvoří na základě relace nebo vypršení platnosti na soubory cookie v závislosti na hodnotě předané do systému ověřování formulářů *persistCookie* parametru. Vzpomínáte, které využívají metody třídy FormsAuthentication GetAuthCookie, SetAuthCookie a RedirectFromLoginPage v dva vstupní parametry: *uživatelské jméno* a *persistCookie*. Přihlašovací stránka, kterou jsme vytvořili v předchozím kurzu zahrnuté zapamatovat zaškrtávacího políčka, která určuje, zda byl vytvořen trvalého souboru cookie. Trvalé soubory cookie jsou založené na vypršení platnosti; dočasné soubory cookie jsou založeného na relacích.

Časový limit a parametr slidingExpiration již uvedenou koncepci stejné platí pro oba soubory cookie podle relací a vypršení platnosti. Je pouze jeden dílčí rozdíl ve spuštění: při vypršení platnosti na základě souborů cookie pomocí slidingTimeout nastavenou na hodnotu true, vypršení platnosti souboru cookie se aktualizují po víc než poloviny zadané doby uplynul.

Umožňuje aktualizovat zásady vypršení časových limitů lístek ověřování našeho webu, který lístky časový limit po jednu hodinu (60 minut), pomocí klouzavé vypršení platnosti. Tato změna projeví, aktualizujte soubor Web.config, přidávání &lt;formuláře&gt; elementu &lt;ověřování&gt; element následujícím kódem:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Pomocí adresy URL přihlašovací stránky než Login.aspx

Protože neoprávněným uživatelům FormsAuthenticationModule automaticky přesměruje na přihlašovací stránku, je nutné znát adresu URL přihlašovací stránky. Tato adresa URL je určené atributem loginUrl v &lt;forms&gt; elementu a výchozí hodnota je login.aspx. Pokud se přenos přes existující web, možná už přihlašovací stránku s jinou adresu URL, který již záložek a indexovaný vyhledávači. Místo přejmenování stávající přihlašovací stránku login.aspx a přerušení odkazů a záložky uživatelů, můžete místo toho změnit atribut loginUrl tak, aby odkazoval na přihlašovací stránku.

Například pokud vaší přihlašovací stránce označovala jako SignIn.aspx a se nachází v adresáři uživatele, můžete ukázat loginUrl nastavení konfigurace pro ~/Users/SignIn.aspx takto:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Protože naše aktuální aplikace má na přihlašovací stránce Login.aspx s názvem, není nutné zadat vlastní hodnotu v &lt;forms&gt; elementu.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Krok 2: Použití lístků pro ověřování pomocí formulářů bez souborů cookie

Ve výchozím nastavení systému pro ověřování formuláře určuje, zda ukládání lístků pro ověřování v kolekci souborů cookie nebo vložení v adrese URL podle uživatelského agenta následujícím webu. Všechny hlavní fáze technické stolních počítačů, jako je Internet Explorer, Firefox, Opera a Safari, podporu souborů cookie, ale ne všechna mobilní zařízení provést.

Zásady souborů cookie používá systémem ověřování formulářů závisí na nastavení bez souborů cookie v &lt;forms&gt; element, který je možné přiřadit jednu ze čtyř hodnot:

- UseCookies – Určuje, že budou vždy použita lístků pro ověřování na základě souboru cookie.
- UseUri – označuje, že se nikdy použita lístků pro ověřování na základě souboru cookie.
- Automatické rozpoznávání – Pokud profil zařízení nepodporuje soubory cookie, lístků pro ověřování na základě souboru cookie nepoužívají; Pokud je profil zařízení podporuje, mechanismus zjišťování slouží k určení, zda jsou povoleny soubory cookie.
- UseDeviceProfile – výchozí hodnota; lístků pro ověřování na základě souboru cookie používá jenom v případě, že je profil zařízení podporuje. Žádný mechanismus testování, který se používá.

Nastavení automatické rozpoznávání a UseDeviceProfile využívají *profil zařízení* při určování, jestli se má použít lístků pro ověřování na základě souboru cookie nebo bez souborů cookie. ASP.NET udržuje databáze z různých zařízení a jejími funkcemi, jako je například udává, jestli podporují soubory cookie, jakou verzi jazyka JavaScript podporují a tak dále. Pokaždé, když zařízení vyžaduje webovou stránku z webového serveru se odesílá spolu *uživatelského agenta* hlavičky protokolu HTTP, který identifikuje typ zařízení. ASP.NET automaticky odpovídá zadané identifikační řetězec s odpovídající profil zadaný ve své databázi.

> [!NOTE]
> Databáze funkce zařízení je uložena v počtu souborů XML, který využívali [schéma souboru s definicí prohlížeče](https://msdn.microsoft.com/library/ms228122.aspx). Výchozí soubory profilu zařízení jsou umístěny ve složce % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Můžete také přidat vlastní soubory do vaší aplikace aplikace\_složka prohlížeče. Další informace najdete v tématu [postupy: zjištění typy prohlížečů na webových stránkách ASP.NET](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Ve výchozím nastavení je UseDeviceProfile, lístků pro ověřování pomocí formulářů bez souborů cookie se použijí při návštěvě webu zařízení, jejichž profil hlásí, že nepodporuje soubory cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Kódování lístek ověřování v adrese URL

Soubory cookie jsou přirozené prostředí pro vkládání informací z prohlížeče v každé žádosti o konkrétní web, což je důvod, proč výchozí nastavení ověřování formulářů používat soubory cookie, pokud je hostujícími zařízení podporuje. Pokud soubory cookie se nepodporuje, musí být použity alternativní způsob pro předání lístku ověřování z klienta na server. Běžným alternativním řešením používá v prostředích bez souborů cookie je určený ke kódování dat souboru cookie v adrese URL.

Nejlepší způsob, jak zobrazit, jak tyto informace mohou být vloženy do adresy URL je vynutit lokalitu na používání ověřování bez souborů cookie lístky. Můžete to provést tak, že nastavíte nastavení konfigurace bez souborů cookie UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Po provedení této změny najdete na webu pomocí prohlížeče. Při návštěvě jako anonymní uživatel, adresy URL bude vypadat stejně jako před nástupem prostředí. Například při návštěvě stránku Default.aspx adresního řádku prohlížeče ukazuje na následující adrese URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Po přihlášení se však lístek ověřování pomocí formulářů se vloží do adresy URL. Například po navštívit stránku pro přihlášení a přihlásíte se jako Sam, mám teď vrátíte na stránku Default.aspx, ale tato doba je adresa URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

V rámci URL byla vložena lístek ověřování pomocí formulářů. Řetězce (F (jaIOIDTJxIr12xYS. VVgkqKCVAuIoW30Bu0diWi6flQC FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) představuje informace lístku ověřování šestnáctkově zakódovaného a je stejná data, která je obvykle uložen v souboru cookie.

Aby lístků pro ověřování bez souborů cookie pro práci systém musí kódovat všechny adresy URL na stránce zahrnout data lístku ověřování, jinak lístek ověřování bude dojít ke ztrátě, když uživatel klikne na odkaz. Naštěstí tuto logiku vložení probíhá automaticky. Abychom si předvedli tuto funkci, otevřete stránku Default.aspx a přidejte ovládací prvek hypertextového odkazu, nastavením jeho vlastnosti Text a NavigateUrl na odkaz Test a SomePage.aspx, v uvedeném pořadí. Nevadí, že ve skutečnosti není k dispozici na stránce v našem projektu s názvem SomePage.aspx.

Uložte změny do Default.aspx a potom navštivte pomocí prohlížeče. Přihlaste se k webu tak, aby v adrese URL se vloží lístek ověřování pomocí formulářů. V dalším kroku na Default.aspx, klikněte na odkaz Test odkaz. Co se stalo? Pokud neexistuje žádná stránka s názvem SomePage.aspx pak došlo k chybě 404, který je však není co je důležité tady. Místo toho se soustřeďte na panelu Adresa v prohlížeči. Všimněte si, že obsahuje ověřovací lístek v adrese URL!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

SomePage.aspx adresy URL v odkazu byl automaticky převeden na adresu URL, která zahrnutá lístek ověřování - nebyly k dispozici pro zápis ikonu kódu! Lístek ověřování formuláře se automaticky vloží do adresy URL pro všechny hypertextové odkazy, které nezačínají `http://` nebo `/`. Nebude vadit, když na hypertextový odkaz se zobrazí při volání k Response.Redirect, ovládací prvek hypertextového odkazu nebo element anchor HTML (například `<a href="...">...</a>`). Jako adresu URL není něco jako `http://www.someserver.com/SomePage.aspx` nebo `/SomePage.aspx`, pro nás bude vložen lístek ověřování pomocí formulářů.

> [!NOTE]
> Lístků pro ověřování pomocí formulářů bez souborů cookie dodržovat stejné zásady pro vypršení časového limitu jako lístků pro ověřování na základě souboru cookie. Lístků pro ověřování bez souborů cookie ale náchylnější útoky opakováním, protože lístek ověřování je přímo součástí adresy URL. Představte si uživatel navštíví nějaký web, přihlášení a pak vloží adresu URL kolegu e-mailem. Pokud kolegu klikne na tento odkaz před vypršení platnosti je dosaženo, Zaprotokolují se jako uživatel, který poslal e-mail!


## <a name="step-3-securing-the-authentication-ticket"></a>Krok 3: Zabezpečení lístek ověřování

Lístek ověřování pomocí formulářů se přenášejí prostřednictvím sítě jako buď do souboru cookie nebo vložený přímo v rámci adresy URL. Kromě informací o identitě lístek ověřování mohou zahrnovat také data uživatele (jak jsme uvidí v kroku 4). V důsledku toho je důležité, že je lístek data se šifrují z nepovolaným a (i), který může zaručit,-the-ticket nebylo manipulováno se systém ověřování formulářů.

K zajištění ochrany osobních údajů lístku dat, můžete systém ověřování formulářů šifrovat data lístku. Nepodařilo se zašifrovat data lístku odešle potenciálně citlivé informace přes ve formátu prostého textu.

Pokud chcete zajistit lístek pravosti, systém ověřování formulářů musí *ověření* -the-ticket. Ověření je třeba zajistit, že byl změněn konkrétní data a se provádí prostřednictvím  *[zprávy ověřovacího kódu (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*. Řečeno v kostce počítače MAC představuje malou část informací, které identifikuje data, která je potřeba ověřit (v tomto případě-the-ticket). Pokud se změní data reprezentována MAC, pak nebude spárujte MAC a data. Kromě toho je výpočetně obtížné pro přístup k i data upravit a generovat vlastní MAC tak, aby odpovídaly s upravenými daty.

Při vytváření (nebo změny) lístek ověřování formulářů systému MAC vytvoří a připojí ho k datům lístku. Dorazí další požadavek systému ověřování formulářů porovná MAC a lístek data můžou ověřovat jejich pravost dat lístků. Obrázek 3 ilustruje tento pracovní postup graficky.


[![Pravosti lístku je, že jsou splněné prostřednictvím MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Obrázek 03**: lístek The pravost je, že jsou splněné prostřednictvím MACU ([kliknutím ji zobrazíte obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


Jaké bezpečnostní opatření se použijí pro lístek ověřování závisí na nastavení ochrany &lt;forms&gt; elementu. Nastavení ochrany může být přiřazen k jednomu z následujících tří hodnot:

- Všechny --the-ticket je zašifrován a digitálně podepsané (výchozí).
- Šifrování – pouze šifrování se použije – žádné MAC je generován.
- NONE –-the-ticket je šifrované, ani digitálně podepsané.
- Ověřování - MAC se vygeneruje, ale data lístku se odesílají prostřednictvím ve formátu prostého textu.

Společnost Microsoft důrazně doporučuje použití všech nastavení.

### <a name="setting-the-validation-and-decryption-keys"></a>Nastavení ověřovací a dešifrovací klíče

Šifrování a hashování algoritmy používané systémem ověřování formulářů k šifrování a ověření lístek ověřování jsou přizpůsobitelné prostřednictvím [ &lt;machineKey&gt; element](https://msdn.microsoft.com/library/w8h3skw9.aspx) v souboru Web.config. Tabulka 2 jsou podrobněji popsány dále &lt;machineKey&gt; atributy elementu a jejich možných hodnot.

| **Atribut** | **Popis** |
| --- | --- |
| dešifrování | Určuje algoritmus použitý k šifrování. Tento atribut může mít jednu z následujících čtyř hodnot: - Auto - default; Určuje algoritmus, na základě délky decryptionKey atributu. -Používá AES - [Advanced Encryption (Standard AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algoritmus. -Používá DES - [Data Encryption (Standard DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) tento algoritmus se považuje za výpočetně slabé by se neměl používat. -3DES – používá [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algoritmus, který pracuje s použitím algoritmus DES třikrát. |
| decryptionKey | Tajný klíč, který používá algoritmus šifrování. Tato hodnota musí být buď je možné šestnáctkový řetězec odpovídající délky (na základě hodnoty v dešifrování), automaticky vygenerovat nebo buď hodnotu s, IsolateApps. Přidání IsolateApps dává pokyn použít jedinečnou hodnotu pro každou aplikaci technologie ASP.NET. Výchozí hodnota je automaticky vygenerovat, IsolateApps. |
| ověřování | Určuje algoritmus používaný k ověření. Tento atribut může mít jednu z následujících čtyř hodnot: - AES - využívá algoritmus Advanced Encryption (Standard AES). -MD5 – používá [– hodnota hash (MD5 5)](http://en.wikipedia.org/wiki/MD5) algoritmus. -SHA1 – používá [SHA1](http://en.wikipedia.org/wiki/Sha1) algoritmus (výchozí). -3DES - využívá algoritmus Triple DES. |
| validationKey | Tajný klíč, který používá algoritmus pro ověření. Tato hodnota musí být buď je možné šestnáctkový řetězec odpovídající délky (na základě hodnoty ověřování), automaticky vygenerovat nebo buď hodnotu s, IsolateApps. Přidání IsolateApps dává pokyn použít jedinečnou hodnotu pro každou aplikaci technologie ASP.NET. Výchozí hodnota je automaticky vygenerovat, IsolateApps. |

**Tabulka 2**: &lt;machineKey&gt; atributy – Element

Důkladné diskuzi o těchto možností šifrování a ověřování a profesionály v oboru pro a proti různých algoritmů, je nad rámec tohoto kurzu. Pro hlouběji podívejte se na tyto problémy, mezi které patří doprovodné materiály k jaké algoritmy šifrování a ověřování chcete používat, jaké délky klíčů, pokud chcete použít a jak nejlepší ke generování těchto klíčů najdete *Professional ASP.NET 2.0 zabezpečení, členství a Správa rolí* .

Ve výchozím nastavení klíče používané k šifrování a ověřování jsou automaticky generovány pro každou aplikaci, a tyto klíče jsou uložené v místním úřadu zabezpečení (LSA). Stručně řečeno výchozí nastavení zaručit jedinečné klíče na webovém serveru webového serveru a aplikace v aplikaci. Toto výchozí chování v důsledku toho nebude fungovat pro dvě následující scénáře:

- **Webových farem** - v [webové farmy](http://en.wikipedia.org/wiki/Web_farm) scénáři jedné webové aplikace je hostitelem více webových serverů za účelem škálovatelnosti a redundance. Jednotlivých příchozích požadavků je odeslána na server ve farmě, což znamená, že během životního cyklu uživatelské relace, různé servery můžou sloužit k zpracování jeho různých požadavků. V důsledku toho každý server musí používat stejné klíče pro šifrování a ověřování, tak, aby vytvoří lístek ověřování pomocí formulářů, zašifrovaně a je ověřený na jeden server lze dešifrovat a ověřit na jiném serveru ve farmě.
- **Sdílení aplikací lístek pro různé** – jeden webový server může hostovat více aplikací prostředí ASP.NET. Pokud budete potřebovat pro tyto různé aplikace sdílet jeden ověřovací lístek, je nutné, aby jejich šifrování a ověření klíče shodují.

Při práci ve webové farmě nebo nastavení sdílení lístků pro ověřování mezi aplikacemi na stejném serveru, budete muset nakonfigurovat &lt;machineKey&gt; element v ovlivněných aplikace tak, aby jejich decryptionKey a validationKey hodnoty odpovídají.

Zatímco ani jeden z výše uvedených scénářích se vztahuje na naše ukázková aplikace jsme stále zadejte explicitní decryptionKey a hodnoty validationKey a definovat algoritmy, které mají být použity. Přidat &lt;machineKey&gt; nastavení v souboru Web.config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Další informace najdete v [postupy: Konfigurace MachineKey v technologii ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> DecryptionKey a validationKey hodnoty byly získány z [Steve Gibson](http://www.grc.com/stevegibson.htm)společnosti [ideální hesla webové stránky](https://www.grc.com/passwords.htm), který generuje náhodné 64 hexadecimálních znaků na každé návštěvě stránky. Chcete-li snížit pravděpodobnost, že tyto klíče provádění svou cestu do produkčních aplikací, se doporučuje nahraďte výše uvedené klíče náhodně generované ty ze stránky ideální hesla.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Krok 4: Ukládání dalších údajů uživatele v lístku

Mnoho webových aplikací zobrazit informace o nebo zobrazení stránky založit na aktuálně přihlášeného uživatele. Na webové stránce může například zobrazit uživatelské jméno a datum, kdy uživatel naposledy přihlášený v horním rohu každé stránky. Ověřovací lístek uloží uživatelské jméno aktuálně přihlášeného uživatele, ale je potřeba žádné další informace, na stránce musí přejít do úložiště uživatele – obvykle databáze - k vyhledání informací není uloženo v lístek ověřování.

Stačí nepatrné kódu jsme ukládání dalších informací o uživatelích v lístku ověřování formulářů. Tyto údaje lze vyjádřit pomocí [FormsAuthenticationTicket třídy](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)společnosti [UserData vlastnost](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). To je vhodné místo pro malá množství informací o uživateli, který je obvykle potřeba. Hodnota zadaná v UserData vlastnost zahrnuté jako součást ověřovacího lístku souboru cookie a, podobně jako v ostatních polích lístku, je zašifrovaný a ověřen podle konfigurace systému ověřování formulářů. Ve výchozím nastavení je UserData prázdný řetězec.

Za účelem uložení dat uživatele v lístku ověřování, musíme psát hodně kódu na přihlašovací stránce, která vezme informace specifické pro uživatele a uloží jej v lístku. Protože UserData vlastnosti typu String, data uložená v ní musí správně serializován jako řetězec. Představte si například, že naše úložiště uživatele zahrnuté každý uživatel datum narození a název svého zaměstnavatele a My jsme chtěli uložit tyto hodnoty dvou vlastností v lístku ověřování. Tyto hodnoty jsme může serializovat do řetězce zřetězením uživatele datum narození řetězec s svislicí (|), za nímž následuje název zaměstnavatele. Pro uživatele které vznikly až na 15. srpna 1974, který vám vyhovuje Northwind Traders, jsme přiřadíte vlastnost UserData řetězec: 1974-08-15 | Northwind Traders.

Pokaždé, když potřebujeme přístup k datům uloženým v-the-ticket, jsme to tak, že kliknete na aktuální žádost FormsAuthenticationTicket a deserializaci UserData vlastnost. V případě datum narození a zaměstnavatel název příklad jsme by řetězec UserData rozdělit do dvou podřetězců na základě oddělovače (|).


[![Dalších informací o uživatelích, které mohou být uloženy v lístku ověřování](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Obrázek 04**: další uživatele může být uložena v lístku ověřování ([kliknutím ji zobrazíte obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Zápis informací o UserData

Bohužel přidává informace specifické pro uživatele ověřovací lístek není tak přímočaré jako očekávat. Vlastnost UserData třídy FormsAuthenticationTicket je jen pro čtení a lze zadat pouze pomocí konstruktoru třídy FormsAuthenticationTicket. Při zadávání vlastnost UserData v konstruktoru, musíme také zadat jiné hodnoty uživatele-the-ticket: uživatelské jméno, datum vystavení, vypršení platnosti a tak dále. Když na přihlašovací stránku jsme vytvořili v předchozím kurzu, to bylo je všechny zpracoval pro nás třídu ověřování pomocí formulářů. Při přidávání UserData FormsAuthenticationTicket, budeme muset psát kód k většině funkcí už poskytovaný třídou FormsAuthentication replikaci.

Podíváme se na kód nezbytné pro práci s UserData stačí aktualizovat stránku Login.aspx zaznamenat další informace o uživateli lístek ověřování. Předstírat, že, že naše úložiště uživatele obsahuje informace o společnosti, uživatel pracuje pro a jejich funkce a že chceme zachytávání těchto informací v lístku ověřování. Aktualizujte obslužnou rutinu události kliknutí LoginButton na stránku Login.aspx tak, aby kód vypadá takto:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Projděme si tento jeden řádek kódu v čase. Metoda spustí definováním zadána čtyři pole řetězce: uživatelé, hesla, companyName a titleAtCompany. Tyto pole obsahují uživatelská jména, hesla, názvy společností a názvy uživatelských účtů v systému, ze které jsou dostupné tři: Scott, Jisun a Sam. Tyto hodnoty v reálné aplikaci, by měl posílat dotaz z úložiště uživatelů není pevně zakódovaný ve zdrojovém kódu na stránce.

V předchozím kurzu Pokud zadané přihlašovací údaje byly platné zavolali jsme na jednoduše FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked), která provádí následující kroky:

1. Vytvořit lístek ověřování formulářů
2. -The-ticket zapsáno do příslušné úložiště. Pro ověřování na základě souborů cookie lístky se používá kolekce souborů cookie prohlížeče; pro ověřování bez souborů cookie lístky data lístku serializován do adresy URL
3. Přesměruje uživatele na příslušnou stránku

Tyto kroky jsou replikovány ve výše uvedeném kódu. Nejprve je řetězec, který bude nakonec uložíme vlastnost UserData vytvořen kombinací názvu společnosti a názvu omezující dvě hodnoty znakem svislé čáry (|).

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

V dalším kroku FormsAuthentication.GetAuthCookie, je vyvolána metoda, která vytvoří lístek ověřování, zašifruje a ověří podle nastavení konfigurace a umístí ho do objektu HttpCookie.

HttpCookie authCookie = FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked);

Chcete-li pracovat s FormAuthenticationTicket vložený do souboru cookie, musíme volat třídu FormAuthentication [dešifrovat metoda](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)a předejte hodnotu souboru cookie.

Lístek FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value);

Pak vytvoříme *nové* FormsAuthenticationTicket instance na základě existující FormsAuthenticationTicket hodnot. Tento nový lístek ale obsahuje informace specifické pro uživatele (userDataString).

FormsAuthenticationTicket newTicket = new FormsAuthenticationTicket(ticket.Version, ticket.Name, ticket.IssueDate, ticket.Expiration, ticket.IsPersistent, userDataString);

Jsme pak šifrování (a ověření) novou instanci FormsAuthenticationTicket voláním [šifrování metoda](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)a vrátit tato data šifrovaná (a ověřené) do authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Nakonec authCookie se přidá do kolekce Response.Cookies a GetRedirectUrl metoda je volána k určení na příslušnou stránku a odešlete mu.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Veškerý tento kód je potřeba, protože UserData vlastnost je jen pro čtení a FormsAuthentication třída neposkytuje žádné metody pro zadávání informací UserData v jeho GetAuthCookie, SetAuthCookie nebo RedirectFromLoginPage metody.

> [!NOTE]
> Kód, který jsme právě prozkoumat ukládá informace specifické pro uživatele v lístku ověřování na základě souborů cookie. Třídy, která je zodpovědná za serializaci lístek ověřování pomocí formulářů k adrese URL jsou interní v rozhraní .NET Framework. Dlouhý text krátký, nelze uložení dat uživatele v lístku ověřování formulářů bez souborů cookie.


### <a name="accessing-the-userdata-information"></a>Přístup k informacím o UserData

V tomto okamžiku název společnosti a název každého uživatele uložená ve vlastnosti UserData lístek ověřování formulářů při přihlášení. Tyto informace je přístupný z lístek ověřování na libovolné stránce bez nutnosti postoupí do úložiště uživatele. Pro ilustraci, jak tyto informace můžou být načten z vlastnosti UserData, můžeme aktualizovat Default.aspx tak, aby jeho uvítací zpráva obsahuje nejen jméno uživatele, ale také společnosti, které fungují pro a jejich funkce.

V současné době Default.aspx obsahuje Panel AuthenticatedMessagePanel pomocí ovládacího prvku popisek s názvem WelcomeBackMessage. Tento Panel se zobrazí jenom k ověřeným uživatelům. Aktualizovat kód v stránku Default.aspx\_načtení obslužná rutina události vypadat takhle:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Pokud Request.IsAuthenticated má hodnotu true, pak Vítejte zpět, je nejprve nastavit vlastnost Text WelcomeBackMessage *uživatelské jméno*. Potom vlastnost User.Identity přetypovat na objekt FormsIdentity tak, že jsme přístup k podkladové FormsAuthenticationTicket. Jakmile budeme mít FormsAuthenticationTicket, jsme vlastnost UserData deserializovat do názvu společnosti a název. Toho lze dosáhnout rozdělení řetězce na znakem svislé čáry. Název společnosti a název se zobrazí WelcomeBackMessage popisku.

Obrázek 5 ukazuje snímek obrazovky zobrazení v akci. Přihlaste se jako Scott zobrazí Vítejte zpět zprávu, která obsahuje Scottova společnosti a název.


[![Zobrazí se společnosti a názvu aktuálně přihlášení na uživatele](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Obrázek 05**: společnosti The aktuálně přihlášení na uživatele a název se zobrazí ([kliknutím ji zobrazíte obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Lístek ověřování UserData vlastnost slouží jako mezipaměť pro úložiště uživatelů. Stejně jako všechny mezipaměti je potřeba aktualizovat, když se změní podkladová data. Například pokud webové stránky, ze kterého mohou uživatelé aktualizovat svůj profil, musí být pole do mezipaměti ve vlastnosti UserData aktualizovány tak, aby odrážely změny provedené uživatelem.


## <a name="step-5-using-a-custom-principal"></a>Krok 5: Použití vlastních hlavních

Na jednotlivých příchozích požadavků FormsAuthenticationModule pokus o ověření uživatele. Pokud – vypršela platnost ověřovacího lístku je k dispozici, FormsAuthenticationModule Vlastnost HttpContext.User přiřadí nový objekt GenericPrincipal. Tento objekt GenericPrincipal má identitu typu FormsIdentity, která obsahuje odkaz na lístek ověřování pomocí formulářů. Obsahuje třídu objektů GenericPrincipal úplné minimální funkční požadavky potřebné třídou, která implementuje IPrincipal – stačí má vlastnost Identity a metodu IsInRole.

Objekt zabezpečení má dvě odpovědnosti: označíte, ke kterým rolím uživatel patří a k poskytování informací o identitě. To lze provést prostřednictvím rozhraní IPrincipal IsInRole (*roleName*) metody a vlastnosti Identity, v uvedeném pořadí. GenericPrincipal – třída umožňuje pro pole řetězců názvů rolí určené pomocí jeho konstruktoru; jeho IsInRole (*roleName*) metoda zkontroluje pouze a zjistěte, jestli předaná v *roleName* existuje v poli řetězců. Když FormsAuthenticationModule vytvoří objekt GenericPrincipal, předá se v prázdné pole řetězce GenericPrincipal konstruktoru. V důsledku toho voláním IsInRole vždy vrátí hodnotu false.

Třída GenericPrincipal splňuje požadavky pro většinu scénářů ověřování na základě formuláře, nejsou-li použity role. Pro tyto situace, ve kterém není dostatečná zpracování výchozí role nebo když budete potřebovat vlastní objekt IIdentity přidružit k uživateli, můžete vytvořit vlastní objekt IPrincipal průběhu ověřovací pracovní postup a přiřaďte ho k vlastnosti HttpContext.User.

> [!NOTE]
> Jak vidíte v budoucích kurzech, když ASP. Je povolená NET framework role vytváří vlastní objekt typu [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) a přepíše objekt GenericPrincipal vytvořili ověřování formulářů. Dělá to dokážeme daného objektu zabezpečení IsInRole metodu pro rozhraní s rozhraním API v rámci role.


Protože jsme ještě obavy z toho chceme s rolemi ještě, může být pouze z důvodů, proč jsme pro vytvoření vlastní objekt zabezpečení v tomto okamžiku by mít přidružit vlastní objekt IIdentity instančnímu objektu. V kroku 4 jsme se podívali na ukládání dalších informací o uživatelích ve vlastnosti UserData lístek ověřování, zejména uživatelské jméno společnosti a jejich funkce. Informace o UserData je však pouze přístupné prostřednictvím lístek ověřování a pak pouze jako serializovaný řetězec, což znamená, že když chcete zobrazit uživatelské informace uložené v lístku potřebujeme analyzovat vlastnost UserData.

Můžeme vylepšit prostředí pro vývojáře tím, že vytvoříte třídu, která implementuje IIdentity a zahrnuje CompanyName a název vlastnosti. Tímto způsobem, vývojář může získat přístup k názvu společnosti aktuálně přihlášeného uživatele a název přímo prostřednictvím vlastnosti CompanyName a funkce bez potřeby vědět, jak analyzovat vlastnost UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Vytvoření vlastní Identity a hlavní třídy

V tomto kurzu vytvoříme vlastní objekty zabezpečení a identity v aplikaci\_složky s kódem. Začněte přidáním aplikace\_složku do projektu kódu – klikněte pravým tlačítkem na název projektu v Průzkumníku řešení, vyberte možnost Přidat složku ASP.NET a zvolte aplikaci,\_kódu. Aplikace\_kód složka je speciální technologie ASP.NET, který obsahuje třídu soubory specifické pro web.

> [!NOTE]
> Aplikace\_složky s kódem lze používat pouze při správě projektu prostřednictvím modelu projektu webu. Pokud používáte [Model projektu webové aplikace](https://msdn.microsoft.com/asp.net/Aa336618.aspx), vytvořte standardní složku a přidejte do třídy. Například můžete přidat novou složku s názvem třídy a umístěte kód existuje.


V dalším kroku přidejte dva nové soubory tříd do aplikace\_složky s kódem, jednu s názvem CustomIdentity.cs a jeden s názvem CustomPrincipal.cs.


[![Do projektu přidejte CustomIdentity a třídy CustomPrincipal](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Obrázek 06**: Přidání CustomIdentity a CustomPrincipal třídy do projektu knihovny ([kliknutím ji zobrazíte obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


Třída CustomIdentity zodpovídá za implementaci rozhraní IIdentity, který definuje vlastnost AuthenticationType, ověření identity a název vlastnosti. Kromě těchto požadovaných vlastností nás zajímají vystavení základní ověřovací lístek a také vlastnosti pro název společnosti a název daného uživatele. Zadejte následující kód do třídy CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Všimněte si, že třída zahrnuje FormsAuthenticationTicket členské proměnné (\_ticket) a že tyto informace lístku, musí být dodány pomocí konstruktoru. Při vracení identity Name; se používá tato data lístku Vlastnost UserData je analyzována na návratové hodnoty pro vlastnosti CompanyName a funkce.

Dále vytvořte CustomPrincipal třídy. Protože jsme nejsou obeznámeni s rolemi v tomto okamžiku, konstruktor třídy CustomPrincipal přijímá pouze CustomIdentity objektu. jeho metoda IsInRole vždy vrátí hodnotu false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Přiřazení objektu CustomPrincipal kontext zabezpečení příchozího požadavku

Teď máme třídu, která rozšiřuje specifikaci výchozí IIdentity zahrnout CompanyName a název vlastnosti, jakož i vlastní hlavní třída, která používá vlastní identitu. Jsme připraveni krokování s vnořením do kanálu ASP.NET a přiřaďte naše vlastní objekt kontextu zabezpečení příchozího požadavku.

Profilace ASP.NET přijímá příchozí žádosti a zpracovává je procházet celou řadou kroků. V každém kroku je vyvolána konkrétní události, umožňuje vývojářům pronikněte do kanálu technologie ASP.NET a upravovat žádosti v určité fázích životního cyklu. FormsAuthenticationModule, například pro technologii ASP.NET pro vyvolání čeká [AuthenticateRequest události](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), v tomto okamžiku se kontroluje příchozí žádosti o lístek pro ověřování. Pokud se najde lístek pro ověřování se objektů GenericPrincipal a přiřazeno k vlastnosti HttpContext.User.

Po události AuthenticateRequest vyvolá kanálu ASP.NET [PostAuthenticateRequest události](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), což je, pokud jsme nahradit GenericPrincipal objekt vytvořený pomocí FormsAuthenticationModule s instancí naše Objektu CustomPrincipal. Obrázek 7 znázorňuje tento pracovní postup.


[![Objekt GenericPrincipal nahrazuje CustomPrincipal PostAuthenticationRequest události](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Obrázek 07**: The GenericPrincipal nahrazuje CustomPrincipal v události PostAuthenticationRequest ([kliknutím ji zobrazíte obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Aby bylo možné spouštění kódu v reakci na událost kanálu ASP.NET, můžeme vytvořit obslužnou rutinu události v souboru Global.asax nebo vytvořit vlastní modul HTTP. Pro účely tohoto kurzu vytvoříme obslužné rutiny události v souboru Global.asax. Začněte přidáním Global.asax na váš web. Klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a přidejte položku typu globální třída aplikace s názvem souboru Global.asax.


[![Přidat soubor Global.asax na váš web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Obrázek 08**: Přidat soubor Global.asax na svůj web ([kliknutím ji zobrazíte obrázek v plné velikosti](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


Výchozí šablona Global.asax obsahuje obslužné rutiny událostí pro celou řadou události kanálu ASP.NET, včetně počáteční a koncové a [chybová událost](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), mimo jiné. Jak jsme už nejsou potřeba pro tuto aplikaci, můžete bez obav odstranit tyto obslužné rutiny událostí. Je událost, které nás zajímají PostAuthenticateRequest. Aktualizujte si soubor Global.asax tak její kód vypadá nějak takto:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

Aplikace\_OnPostAuthenticateRequest metoda spustí pokaždé, když modul runtime ASP.NET vyvolává událost PostAuthenticateRequest, který se jednou na každý příchozí požadavek na stránku. Obslužná rutina události začne kontroluje se, pokud uživatel je ověřený a došlo k ověření prostřednictvím ověřování pomocí formulářů. V takovém případě se nový objekt CustomIdentity a aktuální žádost ověřovací lístek předaný konstruktoru. Pod objektu CustomPrincipal je vytvořen a předaný konstruktoru objektu CustomIdentity nově vytvořené. Nakonec kontext zabezpečení pro aktuální požadavek se přiřadí nově vytvořeného objektu CustomPrincipal.

Všimněte si, že poslední krok – přidružení objektu CustomPrincipal kontext zabezpečení požadavku - přiřadí dvě vlastnosti objektu zabezpečení: HttpContext.User a vlastnost Thread.CurrentPrincipal. Tyto dvě úlohy jsou nezbytné vzhledem ke způsobu kontexty zabezpečení jsou zpracovány v technologii ASP.NET. Rozhraní .NET Framework přidruží každé spuštěné vlákno; kontext zabezpečení Tyto informace jsou k dispozici jako objekt IPrincipal prostřednictvím [objekt vlákna](https://msdn.microsoft.com/library/system.threading.thread.aspx)společnosti [vlastnost CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Co je matoucími je, že technologie ASP.NET má své vlastní informace kontextu zabezpečení (HttpContext.User).

V některých případech je zkontrolován vlastnost Thread.CurrentPrincipal při určení kontextu zabezpečení; v jiných scénářích se používá HttpContext.User. Například existuje funkce zabezpečení v rozhraní .NET, které umožňují vývojářům deklarativně stavu uživatelů nebo rolí můžete vytvořit instanci třídy nebo vyvolat specifických metod (viz [přidáním autorizačních pravidel a obchodních dat pomocí vrstvy PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Pod pokličkou těchto technik deklarativní určení kontextu zabezpečení přes vlastnost Thread.CurrentPrincipal.

V jiných scénářích se používá vlastnost HttpContext.User. Například v předchozím kurzu jsme použili tuto vlastnost zobrazit uživatelské jméno aktuálně přihlášeného uživatele. Jasně pak je nutné, aby informace kontextu zabezpečení ve vlastnostech vlastnost Thread.CurrentPrincipal a HttpContext.User shodují.

Modul runtime ASP.NET se automaticky synchronizuje hodnoty těchto vlastností pro nás. Však tuto synchronizaci dochází po AuthenticateRequest události, ale *před* PostAuthenticateRequest událostí. V důsledku toho při přidávání vlastní objekt zabezpečení v případě PostAuthenticateRequest musíme být určité ručně přiřadit vlastnost Thread.CurrentPrincipal, jinak se vlastnost Thread.CurrentPrincipal a HttpContext.User bude synchronizován. Zobrazit [Context.User vs. Vlastnost Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) podrobnější informace o tomto problému.

### <a name="accessing-the-companyname-and-title-properties"></a>Přístup k CompanyName a název vlastnosti

Vždy, když žádost o přijetí a odesílá do modulu ASP.NET, aplikace\_se aktivuje OnPostAuthenticateRequest obslužné rutiny události v souboru Global.asax. Pokud požadavek byl úspěšně ověřen pomocí FormsAuthenticationModule, obslužná rutina události vytvoří nový objekt CustomPrincipal s CustomIdentity objekt podle ověřovací lístek. S tuto logiku je neuvěřitelně jednoduché přístup k informacím o název společnosti a název aktuálně přihlášeného uživatele.

Vraťte se na stránku\_zatížení obslužné rutiny události v Default.aspx, kde v kroku 4, jsme napsali kód k získání lístku ověřování formuláře a analyzování vlastnost UserData mohla zobrazit název společnosti a název daného uživatele. S objekty CustomPrincipal a CustomIdentity nyní používá není nutné k parsování hodnot z vlastnosti UserData lístku. Místo toho stačí získat odkaz na objekt CustomIdentity a použít jeho CompanyName a název vlastnosti:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Souhrn

V tomto kurzu jsme se zaměřili na tom, jak přizpůsobit nastavení systému ověřování formuláře pomocí souboru Web.config. Jsme se podívali na způsob zpracování vypršení platnosti lístku ověřování a šifrování a ověřování ochrana použití k ochraně-the-ticket z kontroly a úpravy. Nakonec jsme probírali pomocí vlastnosti UserData lístek ověřování k ukládání dalších informací o uživatelích v lístku samotného a jak používat vlastní objekty zabezpečení a identity vystavit tyto informace více vývojářsky přívětivé způsobem.

V tomto kurzu končí naše zkoumání ověřování pomocí formulářů v ASP.NET. V dalším kurzu se spustí do členství v rámci naší cesty.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Rozbor ověřování pomocí formulářů](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Vysvětlení: Ověřování pomocí formulářů v ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Postupy: Ochrana ověřování pomocí formulářů v ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Profesionální ASP.NET 2.0 zabezpečení, členství a rolí správy](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zabezpečení ovládacími prvky pro přihlášení](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;Ověřování&gt; – Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Forms&gt; – Element pro &lt;ověřování&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt; – Element](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Principy lístek ověřování pomocí formulářů a souboru Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video od Pluralsightu na témata, které jsou obsažené v tomto kurzu

- [Změna vlastností ověřování pomocí formulářů](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Postup instalace a používání ověřování bez souborů Cookie v aplikaci ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Přemístění formulářů ASP pro přihlášení](../../../videos/authentication/asp-forms-login-relocation.md)
- [Konfigurace vlastního klíče přihlašovacích formulářů](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Přidání vlastních dat do metody ověřování](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Použití vlastních hlavních objektů](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, Autor více ASP/ASP.NET knih a zakladatelem 4GuysFromRolla.com, má pracovali Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy  *[Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott může být dostupný na adrese [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Alicja Maziarz. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Předchozí](an-overview-of-forms-authentication-cs.md)
> [další](security-basics-and-asp-net-support-vb.md)
