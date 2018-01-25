---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: "Přehled ověřování pomocí formulářů (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu jsme se změní z pouhé diskusi k implementaci; Konkrétně se podíváme na implementace ověřování pomocí formulářů. W webové aplikace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 90bcff91d0642e6af66f43fd807b253cc516d277
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-forms-authentication-vb"></a>Přehled ověřování pomocí formulářů (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> V tomto kurzu jsme se změní z pouhé diskusi k implementaci; Konkrétně se podíváme na implementace ověřování pomocí formulářů. Webové aplikace začneme vytvořením v tomto kurzu se bude nadále postavena v následujících kurzech, jak jsme přesunout z jednoduchého ověřování pomocí formulářů, členství a rolí.
> 
> Toto video pro další informace najdete v tomto tématu: [pomocí základní ověřování pomocí formulářů technologie ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Úvod

V [předchozí kurzu](security-basics-and-asp-net-support-vb.md) jsme probrali různých možností účet ověřování, autorizaci a uživatel technologii ASP.NET. V tomto kurzu jsme se změní z pouhé diskusi k implementaci; Konkrétně se podíváme na implementace ověřování pomocí formulářů. Webové aplikace začneme vytvořením v tomto kurzu se bude nadále postavena v následujících kurzech, jak jsme přesunout z jednoduchého ověřování pomocí formulářů, členství a rolí.

V tomto kurzu začíná hlubší pohled na pracovním ověřování formulářů tématu, které jsme dotýkal při v předchozí kurzu. Následující, vytvoříme webu ASP.NET, pomocí kterého je možné ukázku koncepty ověřování pomocí formulářů. V dalším kroku jsme se konfigurace lokality k ověřování pomocí formulářů, vytvořit jednoduché přihlašovací stránku a zjistit, jak zjistit, v kódu, zda je uživatel ověřen, a pokud ano, uživatelské jméno se přihlášení.

Principy formulářů, pracovních postupů ověřování, povolení ve webové aplikaci a vytváření stránek přihlášení a odhlášení všechny důležité kroky při vytváření aplikace ASP.NET, která podporuje uživatelské účty a ověřuje uživatele pomocí webové stránky. Proto - a protože tyto kurzy stavět na jiné - I by doporučujeme, abyste absolvování tohoto kurzu plně před přejde k dalšímu i v případě, že jste již máte zkušenosti konfigurace ověřování formulářů v projektech posledních.

## <a name="understanding-the-forms-authentication-workflow"></a>Seznámení s pracovním postupem ověřování formulářů

Když modulem runtime ASP.NET zpracovává žádost o prostředek ASP.NET, například stránky ASP.NET nebo webové služby ASP.NET, vyvolá požadavek určitý počet událostí během životního cyklu. Nejsou k dispozici události vyvolané na konci velmi začátku a na velmi požadavku, ty, které vyvolá, když požadavek při ověřování a autorizaci, událost vyvolána v případě neošetřené výjimky a tak dále. Pokud chcete zobrazit úplný seznam událostí, naleznete [třídě HttpApplication objekt události](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Vytváření modulů HTTP v* jsou spravované třídy, jejichž kód se spustí v reakci na určité události v průběhu životního cyklu požadavku. ASP.NET se dodává s počtem modulů HTTP, který provádění základních úloh na pozadí. Dva integrované moduly HTTP, které jsou obzvláště důležité pro tato diskuse se:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -ověřuje uživatele zkontrolováním lístek ověřování pomocí formulářů, který je obvykle součástí kolekce souborů cookie uživatele. Pokud je k dispozici žádné ověřovací lístek, je anonymní uživatel.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -Určuje, zda je aktuální uživatel oprávnění pro přístup k požadované adresy URL. Tento modul určuje oprávnění na základě konzultace ohledně autorizační pravidla, zadaný v konfiguračních souborech aplikace. Technologie ASP.NET obsahuje také [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) který určuje autoritu podle konzultace ohledně požadované soubory seznamy ACL.

FormsAuthenticationModule pokus o ověření uživatelů před UrlAuthorizationModule (a FileAuthorizationModule) provádění. Pokud uživatel zadal žádost nemá oprávnění k přístupu k požadovanému zdroji, modul autorizace ukončí požadavek a vrátí [HTTP 401 neoprávněný](http://www.checkupdown.com/status/E401.html) stavu. Ve scénářích ověřování systému Windows je vrácen stav HTTP 401 do prohlížeče. Tento kód stavu způsobí, že prohlížeč zobrazí výzva k zadání přihlašovacích údajů prostřednictvím modální dialogové okno. Ověřování pomocí formulářů, ale HTTP 401 Unauthorized status je nikdy odeslán do prohlížeče vzhledem k tomu, že FormsAuthenticationModule zjistí tento stav a upraví ho přesměruje uživatele na přihlašovací stránku místo (prostřednictvím [HTTP 302 přesměrování](http://www.checkupdown.com/status/E302.html) stav).

Na přihlašovací stránku úkolem je zjistit, zda jsou platná pověření uživatele, a pokud ano, vytvořit lístek ověřování formulářů a přesměruje uživatele zpět na stránku se pokoušeli navštívit. Lístek ověřování je součástí následné žádosti na stránky na webu, který FormsAuthenticationModule používá k identifikaci uživatele.


[![Pracovní postup ověřování formulářů](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Obrázek 01**: The Forms ověřovací pracovní postup ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Zapamatování lístek ověřování napříč návštěv stránky

Po přihlášení, musí se poslat lístek pro ověřování pomocí formulářů zpět na webový server na každý požadavek, aby uživatel zůstane přihlášený jako procházení webu. To se obvykle provádí tak, že lístek ověřování kolekce souborů cookie uživatele. [Soubory cookie](http://en.wikipedia.org/wiki/HTTP_cookie) jsou malé textové soubory, které jsou umístěné v počítači uživatele a přenášejí v hlavičkách protokolu HTTP na každý požadavek na web, který vytvořili souboru cookie. Proto po lístek pro ověřování pomocí formulářů byl vytvořen a uložen v souborech cookie v prohlížeči, každý další návštěvě této lokality odešle lístek ověřování společně se žádostí, čímž identifikace uživatele.

> [!NOTE]
> Použít v každém kurzu ukázkové webové aplikace je k dispozici ke stažení. Tato aplikace ke stažení byl vytvořen s Visual Web Developer 2008 určený pro rozhraní .NET Framework verze 3.5. Vzhledem k tomu, že aplikace je určená pro rozhraní .NET 3.5, jeho soubor Web.config obsahuje další, 3.5 konkrétní konfigurační prvky. Dlouhý text short, pokud máte ještě instalace rozhraní .NET 3.5 v počítači pak ke stažení webové aplikace nebude fungovat bez první odebrání značek 3.5 konkrétní ze souboru Web.config.


Jeden aspekt jejich souborů cookie je vypršením jejich platnosti, což je datum a čas, kdy prohlížeč zahodí souboru cookie. Když vyprší platnost souboru cookie ověřování pomocí formulářů, můžete uživatele už ověření a proto se anonymní. Když návštěvy z veřejného terminálu pravděpodobné, že chtějí jejich lístku ověřování vyprší při uzavření svého prohlížeče. Při návštěvě z domova, ale tento stejný uživatel může být vhodné lístek ověřování na se uloží v prohlížeči restartování, takže nemají znovu přihlásit se pokaždé, když se přejděte na web. Toto rozhodnutí je často provedené uživatelem ve formě políčko Zapamatovat uživatele na přihlašovací stránku. V kroku 3 vyzkoušíme implementaci zaškrtávací políčko Zapamatovat uživatele na přihlašovací stránce. V následujícím kurzu řeší nastavení časového limitu lístek ověřování podrobně.

> [!NOTE]
> Je možné, že uživatelský agent používá k přihlášení na web pravděpodobně nepodporuje soubory cookie. V takovém případě můžete použít ASP.NET lístků pro ověřování pomocí formulářů bez souborů cookie. V tomto režimu je zakódován lístek ověřování k adrese URL. Podíváme se na při použití lístků pro ověřování bez souborů cookie a jak se vytváří a spravují v dalším kurzu.


### <a name="the-scope-of-forms-authentication"></a>Rozsah ověřování pomocí formulářů

Je spravován FormsAuthenticationModule kód který je součástí modulu runtime ASP.NET. Starší než verze 7 společnosti Microsoft [Internetové informační služby (IIS)](https://www.iis.net/) webový server byl odlišné bariéry mezi kanál protokolu HTTP služby IIS a modulu runtime ASP.NET kanálu. Stručně řečeno, ve službě IIS 6 a starší FormsAuthenticationModule pouze provede, když se žádost o deleguje ze služby IIS na modulem runtime ASP.NET. Ve výchozím nastavení služba IIS zpracovává statický obsah samotné – například stránky HTML a CSS a soubory obrázků - a pouze rukou vypnout požadavky modulu runtime ASP.NET při požadavku na stránku s příponou .aspx, .asmx nebo ASHX.

Integrované služby IIS a ASP.NET IIS 7, ale umožňuje kanály. S několik nastavení konfigurace se dá nastavit službu IIS 7 k vyvolání FormsAuthenticationModule pro *všechny* požadavky. Kromě toho se službou IIS 7 můžete definovat autorizačních pravidel adres URL pro soubory libovolného typu. Další informace najdete v tématu [IIS7 zabezpečení a změny mezi IIS6](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [vaše webové platformy zabezpečení](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), a [autorizace adres URL pro službu IIS7 Principy](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Dlouhý text krátké verze starší než IIS 7, můžete použít pouze ověřování pomocí formulářů pro ochranu prostředků, které jsou zpracovávány modulem ASP.NET runtime. Podobně autorizačních pravidel adres URL platí pouze pro prostředky, které jsou zpracovávány modulem ASP.NET runtime. Se službou IIS 7 je však možné integrovat FormsAuthenticationModule a UrlAuthorizationModule do kanálu HTTP služby IIS, a tím rozšíření tuto funkci pro všechny požadavky.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Krok 1: Vytvoření webu ASP.NET pro tento kurz řady

Aby bylo dosaženo nejširší možnou cílovou skupinou, bude vytvořen webu ASP.NET jsme se sestavení v rámci této série s bezplatnou verzi společnosti Microsoft Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Bude implementaci úložišti uživatele SqlMembershipProvider [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) databáze. Pokud používáte Visual Studio 2005 nebo na jinou edici Visual Studio 2008 nebo SQL Server, nemusíte si dělat starosti – kroky budou skoro stejné a případné rozdíly netriviální zdůraznit.

Než budeme moct nakonfigurovat ověřování pomocí formulářů, potřebujeme nejprve webu ASP.NET. Začněte vytvořením webu ASP.NET na základě systému nový soubor. K tomu, spusťte aplikaci Visual Web Developer a přejděte do nabídky soubor a vyberte nový web zobrazení dialogového okna Nový web. Výběr šablony webu ASP.NET, nastavte rozevírací seznam umístění do systému souborů, vyberte složku pro umístění na webu a nastavení jazyka pro VB. Tím se vytvoří nový web s stránku Default.aspx ASP.NET, aplikace\_složky dat a v souboru Web.config.

> [!NOTE]
> Visual Studio podporuje dva režimy řízení projektu: webové projekty a projekty webových aplikací. Webové projekty chybí soubor projektu, zatímco projekty webových aplikací napodobovat architektuře projektu ve Visual Studio .NET 2002 nebo 2003 – se můžete zahrnout soubor projektu a kompilaci zdrojového kódu projektu do jednoho sestavení, které je umístěn ve složce/Bin. Visual Studio 2005 původně pouze podporované webu projekty, i když modelu projektu webové aplikace byla znovu zavedena s aktualizací Service Pack 1; Visual Studio 2008 nabízí obou modelů projektu. Visual Web Developer 2005 a edice 2008, ale podporují pouze webové projekty. Bude možné používat webový projekt modelu. Pokud používáte jiný Express edition a chcete použít [modelu projektu webové aplikace](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) místo toho zaregistrované, můžete tak učinit, ale uvědomte si, že mohou být některé rozdíly mezi zobrazené na obrazovce a kroky je nutné provést porovnání snímek obrazovky ukazuje a pokynů v těchto kurzech.


[![Vytvoření nového souboru na základě systému webového serveru](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Obrázek 02**: vytvoření webu New File System-Based ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Přidání stránky předlohy

Do lokality v kořenovém adresáři s názvem Site.master přidáte další, nové stránky předlohy. [Hlavní stránky](https://msdn.microsoft.com/library/wtxbf3hh.aspx) povolit vývojáři stránky definovat šablonu na webu, který lze použít na stránky ASP.NET. Hlavní výhodou hlavní stránky je, že celkový vzhled lokality lze definovat na jednom místě, a díky snadno aktualizovat nebo upravit rozložení v lokalitě.


[![Přidat stránku předlohy Site.master na web s názvem](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Obrázek 03**: Přidání webu hlavní stránku s názvem Site.master ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image9.png))


Zadejte sem rozložení stránky na webu na hlavní stránce. Můžete použít zobrazení návrhu a přidat libovolnou rozložení a webové kontroly je nutné, nebo můžete ručně přidat kód ručně v zobrazení zdroje. I strukturovaná rozložení Moje hlavní stránky tak, aby napodoboval rozložení použít v mé  *[práci s daty v technologii ASP.NET 2.0](../../data-access/index.md)*  kurz řady (viz obrázek 4). Hlavní stránka používá [kaskádových stylů](http://www.w3schools.com/css/default.asp) pro umístění a stylů CSS nastavení definované v souboru Style.css (která je obsažena v tomto kurzu přidružené ke stažení). Při nelze zjistit z značek vidíte níže, jsou definovaná pravidla šablon stylů CSS tak, aby navigaci &lt;div&gt;na obsah je absolutně nastavený tak, aby se zobrazí na levé straně a má pevnou šířku 200 pixelů.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Na hlavní stránce definuje rozložení statické stránky a oblastí, které lze upravit pomocí stránek ASP.NET, které používají stránky předlohy. Tyto obsahu upravitelné oblasti jsou označeny ContentPlaceHolder řízení, které jsou viditelné v rámci obsah &lt;div&gt;. Naše stránka předlohy má jeden ContentPlaceHolder (MainContent), ale stránky předlohy mohou mít více ContentPlaceHolders.

Pomocí výše uvedených kód přepnutí do návrhového zobrazení ukazuje rozložení stránky předlohy. Všechny stránek ASP.NET, které používají tuto stránku předlohy bude mít toto uniform rozložení možnost zadejte kód pro oblast MainContent.


[![Stránky předlohy, při zobrazení v zobrazení návrhu](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Obrázek 04**: stránku předlohy, při prohlížení prostřednictvím zobrazení návrhu ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Vytváření obsahu stránky

V tomto okamžiku máme stránku Default.aspx v našem webu, ale nepoužívá stránku předlohy, kterou jsme právě vytvořili. Když je možné k manipulaci s deklarativní webové stránky na hlavní stránku, pokud stránce neobsahuje žádný obsah ještě je snazší právě odstranit stránku a znovu jej přidejte do projektu, zadání stránku předlohy k použití. Proto začínají odstraněním Default.aspx z projektu.

V dalším kroku klikněte pravým tlačítkem na název projektu v Průzkumníku řešení a vyberte, chcete-li přidat nový webový formulář s názvem Default.aspx. Tento čas, zaškrtněte políčko vyberte stránku předlohy a stránky předlohy Site.master zvolte ze seznamu.


[![Přidat novou stránku Default.aspx rozhodnete vybrat stránku předlohy](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Obrázek 05**: Přidejte nové Default.aspx stránky výběr a vyberte stránku předlohy ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Pomocí stránky předlohy Site.master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Obrázek 06**: použijte stránky předlohy Site.master ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Pokud používáte Model projektu webové aplikace dialogového okna Přidat novou položku nezahrnuje zaškrtávací políčko vyberte stránku předlohy. Místo toho je nutné přidat položky typu webového obsahu formuláře. Visual Studio zobrazí po výběru možnosti webového obsahu formuláře a kliknutím na tlačítko Přidat, vyberte stejný hlavní dialogové okno vidíte na obrázku 6.


Nová stránka Default.aspx deklarativní zahrnuje právě @Page – direktiva určení cesty k hlavní stránce soubor a obsah ovládacího prvku pro MainContent ContentPlaceHolder stránky předlohy.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Teď ponechte Default.aspx prázdné. Vrátíme se k němu později v tomto kurzu přidání obsahu.

> [!NOTE]
> Naše stránky předlohy obsahuje oddíl pro nabídky nebo některých jiných rozhraní navigace. V budoucích kurzu vytvoříme takového rozhraní.


## <a name="step-2-enabling-forms-authentication"></a>Krok 2: Povolení ověřování pomocí formulářů

S vytvoření webu ASP.NET naše dalším krokem je povolit ověřování pomocí formulářů. Konfigurace ověřování aplikace se specifikuje prostřednictvím [ &lt;ověřování&gt; element](https://msdn.microsoft.com/library/532aee0e.aspx) v souboru Web.config. &lt;Ověřování&gt; element obsahuje jeden atribut s názvem režim, který určuje model ověřování používá aplikace. Tento atribut může mít jednu z následujících čtyř hodnot:

- **Windows** – jak je popsáno v předchozí kurzu, pokud aplikace používá ověřování systému Windows je webový server zodpovědnost za ověření návštěvníka, a to se obvykle provádí pomocí základní ověřování, hodnotou hash nebo integrované ověřování systému Windows ověřování.
- **Forms**-uživatelé se ověřují pomocí formuláře na webové stránce.
- **Passport**-uživatelé se ověřují pomocí sítě společnosti Microsoft Passport.
- **Žádný**-je použit bez ověřování modelu, všechny návštěvníky jsou anonymní.

Ve výchozím nastavení aplikace ASP.NET pomocí ověřování systému Windows. Chcete-li změnit typ ověřování pro ověřování pomocí formulářů, pak je potřeba upravit &lt;ověřování&gt; atribut režimu elementu do formulářů.

Pokud váš projekt ještě neobsahuje soubor Web.config, přidejte jeden teď tím pravým tlačítkem myši na název projektu v Průzkumníku řešení, zvolit přidat novou položku a následným přidáním soubor webové konfigurace.


[![Pokud váš projekt ještě Web.config neobsahuje, přidejte ji nyní](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Obrázek 07**: Pokud váš projekt nemá není ještě zahrnují souboru Web.config, přidejte ho teď ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image21.png))


Dále vyhledejte &lt;ověřování&gt; elementu a aktualizace ho na použití ověřování pomocí formulářů. Po této změně souboru Web.config značek by měl vypadat takto:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Vzhledem k tomu, že soubor Web.config je soubor XML, je důležité velká a malá písmena. Nezapomeňte nastavit atribut režimu do formulářů, s velkým F. Pokud použijete jiné velká a malá písmena, například formuláře, obdržíte chybu konfigurace při návštěvě webu prostřednictvím prohlížeče.


&lt;Ověřování&gt; element může volitelně zahrnovat &lt;forms&gt; podřízený element, který obsahuje nastavení specifická pro ověřování pomocí formulářů. Prozatím se můžeme právě použít výchozí nastavení ověřování formulářů. Se podíváme &lt;forms&gt; podřízený element podrobněji v dalším kurzu.

## <a name="step-3-building-the-login-page"></a>Krok 3: Vytvoření přihlašovací stránky

Kvůli podpoře ověřování pomocí formulářů náš web potřebuje přihlašovací stránku. Jak je popsáno v Principy části pracovního postupu ověřování formulářů FormsAuthenticationModule automaticky přesměruje uživatele na přihlašovací stránku Jestliže se pokusí přistoupit ke stránce, která nejsou oprávnění k zobrazení. Existují také ovládacích prvků technologie ASP.NET, které se zobrazí odkaz na přihlašovací stránku pro anonymní uživatele. To begs na otázku, jaká je adresa URL přihlašovací stránky?

Ve výchozím nastavení systém ověřování formulářů očekává přihlašovací stránky s názvem Login.aspx a umístěné v kořenovém adresáři webové aplikace. Pokud chcete použít adresu URL stránky jiné přihlašovací údaje, můžete tak učinit zadáním v souboru Web.config. Jak to provést v následných kurzu jsme se zobrazí.

Na přihlašovací stránku má tři zodpovědnosti:

1. Poskytují rozhraní, které umožňuje návštěvníka zadat své přihlašovací údaje.
2. Určí, jestli odeslaná přihlašovací údaje jsou platné.
3. Přihlášení uživatele tak, že vytvoříte formuláře lístek ověřování.

### <a name="creating-the-login-pages-user-interface"></a>Vytváření přihlašovací stránku uživatelského rozhraní

Můžeme začít pracovat s první úlohou. Přidejte novou stránku ASP.NET do kořenového adresáře webu s názvem Login.aspx a přidružte ji k hlavní stránce Site.master.


[![Přidat novou stránku ASP.NET s názvem Login.aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Obrázek 08**: Přidejte nové ASP.NET stránky s názvem Login.aspx ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image24.png))


V typické přihlašovací stránky rozhraní se skládá z dvou textových polí – jeden pro uživatelské jméno, jednu pro svoje heslo – a tlačítko pro odeslání formuláře. Weby často zahrnují Zapamatovat uživatele zaškrtávací políčko, pokud je zaškrtnuto, potrvají výsledné lístek ověřování napříč restartování prohlížeče.

Přidejte dva textových polí stránce Login.aspx a nastavte jejich ID vlastnosti na uživatelské jméno a heslo, v uvedeném pořadí. Nastavte také vlastnost hesla v textovém režimu heslo. Dál přidejte ovládací prvek zaškrtávací políčko, jeho vlastnost ID nastavení obdobou a jeho vlastnost Text k zapamatovat si mě. Následující, přidejte tlačítko s názvem LoginButton, jehož vlastnost textu je nastavena na přihlášení. A nakonec přidání ovládacího prvku popisek a nastavte vlastnost ID InvalidCredentialsMessage, jeho vlastnost Text na vaše uživatelské jméno nebo heslo je neplatná. Zkuste to prosím znovu., jeho vlastnost ForeColor na červený a jeho viditelné vlastnost na hodnotu False.

V tomto okamžiku by mělo vypadat jako na snímku na obrázku 9 obrazovky obrazovky a deklarativní syntaxe vaší stránky by měl jako následující:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![Na přihlašovací stránku obsahuje dvě textová pole, zaškrtávací políčko, tlačítka a štítek](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Obrázek 09**: přihlašovací stránku obsahuje dvě textová pole, zaškrtávací políčko, tlačítka a štítek ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image27.png))


Nakonec vytvořte obslužnou rutinu události pro získáte kliknutím LoginButton událostí. Z Návrháře jednoduše dvakrát klikněte tlačítko – ovládací prvek pro vytvoření této obslužné rutiny události.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Určení, zda jsou zadaná pověření platné

Nyní potřebujeme implementovat úloha 2 kliknutím na tlačítko obslužná rutina události – určení, zda jsou zadaná pověření platná. Chcete-li tomu je potřeba úložiště uživatele, který obsahuje všechny přihlašovací údaje uživatele, aby jsme můžete určit, pokud zadaná pověření se shodovat s všechny známé přihlašovací údaje.

Před aplikaci ASP.NET 2.0 se vývojáři zodpovědná za implementace i vlastní úložiště uživatele a psaní kódu k ověření zadané přihlašovací údaje pro Windows store. Většina vývojářů by implementovat úložiště uživatele v databázi, vytvoření tabulky s názvem uživatelé s sloupce jako uživatelské jméno, heslo, e-mailu, LastLoginDate a tak dále. Tuto tabulku, pak by mít jeden záznam na uživatelský účet. Ověření zadané přihlašovací údaje uživatele by zahrnovat dotaz na databázi k vyhledání uživatelského jména a pak zajistit, aby odpovídaly hesla v databázi zadané heslo.

S prostředím ASP.NET 2.0 vývojáři měli jednoho z poskytovatelů členství ke správě používat úložiště uživatele. Tento kurz série použijeme SqlMembershipProvider, která používá databázi systému SQL Server pro úložiště uživatelů. Při použití SqlMembershipProvider musíme implementovat schéma konkrétní databáze, které obsahuje tabulky, zobrazení a uložených procedur očekává zprostředkovatelem. Vyzkoušíme jak implementovat v tomto schématu  *[vytváření schématu členství v systému SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)*  kurzu. S zprostředkovatel členství v místní ověřování přihlašovacích údajů uživatele je jednoduché, volání [třída členství](https://msdn.microsoft.com/library/system.web.security.membership.aspx)na [ValidateUser (*uživatelské jméno*, *heslo*) Metoda](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), která vrací logickou hodnotu, která určuje zda platnost *uživatelské jméno* a *heslo* kombinaci. Zjistíte, jak jsme nebyla ještě implementována úložiště uživatele SqlMembershipProvider, nemůžeme použít metoda ValidateUser třída členství v tuto chvíli.

Místo čekat na sestavení vlastní vlastní uživatelé databázové tabulky (to by být zastaralé, jakmile implementovali jsme SqlMembershipProvider), můžeme místo pevně stránky platné přihlašovací údaje v rámci přihlášení sám sebe. V LoginButton klikněte na obslužnou rutinu události, přidejte následující kód:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Jak vidíte, existují tři platné uživatelské účty - Scott, Jisun a Sam - a všechny tři mít stejné heslo (heslo). Kód prochází pole uživatele a hesla vyhledávání shody platné uživatelské jméno a heslo. Pokud uživatelské jméno a heslo platné, musíme přihlásit uživatele a přesměruje je na příslušnou stránku. Pokud přihlašovací údaje jsou neplatné, zobrazujeme InvalidCredentialsMessage popisku.

Když uživatel zadá platné přihlašovací údaje, uvedených se, že se pak přesměrují na příslušnou stránku. Co když je na příslušnou stránku? Odvolat, že když uživatel navštíví stránku, kterou uživatel nemá oprávnění k zobrazení, FormsAuthenticationModule automaticky přesměrován na přihlašovací stránku. Při tom obsahuje požadovanou adresu URL v řetězci dotazu pomocí parametru ReturnUrl. To znamená pokud uživatel se pokusil k navštívení ProtectedPage.aspx a nebyly autorizovány Uděláte to tak, FormsAuthenticationModule by přesměruje je na:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Po úspěšném přihlášení by měl být uživatel přesměrován zpět na ProtectedPage.aspx. Alternativně může uživatelé navštívit stránku pro přihlášení na své vlastní vůle. V takovém případě po přihlášení uživatele se by měly být odeslány na stránku Default.aspx kořenové složce.

### <a name="logging-in-the-user"></a>Přihlášení uživatele

Za předpokladu, že zadané přihlašovací údaje jsou platné, je potřeba vytvořit lístek ověřování formulářů tím přihlášení uživatele k webu. [FormsAuthentication třída](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) v [obor názvů System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) poskytuje různé metody pro protokolování v a odhlašování uživatelů prostřednictvím formuláře ověřování systému. Existuje několik metod ve třídě, ověřování pomocí formulářů, jsou tři, které jsme zajímají v této situaci řešit:

- [GetAuthCookie (*uživatelské jméno*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -vytvoří ověřovací lístek pro zadaný název *uživatelské jméno*. Dále tato metoda vytvoří a vrátí objekt HttpCookie, který obsahuje obsah lístek ověřování. Pokud *persistCookie* má hodnotu True, je vytvoření trvalého souboru cookie.
- [SetAuthCookie (*uživatelské jméno*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -volá GetAuthCookie (*uživatelské jméno*, *persistCookie*) Metoda pro generování souboru cookie ověřování pomocí formulářů. Tato metoda pak přidá soubor cookie vrácený GetAuthCookie ke kolekci souborů cookie (za předpokladu, že ověřování pomocí formulářů na základě souborů cookie se použít; jinak, tato metoda volá interní třída, která zpracovává logiku bez souborů cookie lístku).
- [RedirectFromLoginPage (*uživatelské jméno*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) -tato metoda volá SetAuthCookie (*uživatelské jméno*, *persistCookie* ) a pak přesměruje uživatele na příslušnou stránku.

GetAuthCookie je užitečné, když budete muset upravit lístek ověřování před zápisem soubor cookie ke kolekci souborů cookie. SetAuthCookie je užitečné, pokud chcete vytvořit lístek pro ověřování formuláře a přidat jej do kolekce souborů cookie, ale nechcete přesměruje uživatele na příslušnou stránku. Možná budete chtít zachovat jejich na přihlašovací stránku nebo poslat některé alternativní stránku.

Vzhledem k tomu, že chceme přihlásit uživatele a přesměruje je na příslušnou stránku, použijeme RedirectFromLoginPage. Aktualizovat kliknutím LoginButton obslužné rutiny události, nahraďte dva řádky komentářů TODO následující řádek kódu:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

Při vytváření formuláře lístek ověřování používáme vlastnost Text textového pole uživatelské jméno pro ověřovací lístek *uživatelské jméno* parametr a stav zaškrtnutí políčka obdobou  *persistCookie* parametr.

K testování přihlašovací stránky, přejděte v prohlížeči. Spustit zadáním neplatné přihlašovací údaje, jako je například Nope uživatelské jméno a heslo nesprávné. Po kliknutí na tlačítko přihlášení dojde ke zpětné volání a zobrazí se popisek InvalidCredentialsMessage.


[![Popisek InvalidCredentialsMessage se zobrazí při zadávání neplatné přihlašovací údaje](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Obrázek 10**: The InvalidCredentialsMessage popisek se zobrazí při zadávání neplatné přihlašovací údaje ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image30.png))


Potom zadejte platné přihlašovací údaje a klikněte na tlačítko přihlášení. Tentokrát zpětné volání v případech lístek ověřování formulářů je vytvořený a budete automaticky přesměrováni zpět na Default.aspx. V tomto okamžiku jste byli přihlášeni k webu, i když neexistují žádné vizuální upozornění k označení, že jste aktuálně přihlášeni. V kroku 4 zjistíme, jak programově určit, jestli uživatel přihlášen v nebo není a také jak k identifikaci uživatele na stránce.

Krok 5 prověří techniky pro protokolování uživatele z webu.

### <a name="securing-the-login-page"></a>Zabezpečení přihlašovací stránky

Když uživatel zadá své přihlašovací údaje a přihlašovací stránky formulář odešle, přihlašovací údaje – včetně jeho hesla - přenášejí přes Internet k webovému serveru v *prostý text*. To znamená, že všechny poskytuje sledování toku dat síťový provoz můžete zobrazit uživatelské jméno a heslo. Chcete-li tomu zabránit, je nezbytné k šifrování síťového provozu pomocí [vrstvy SSL (Secure Socket)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Tím bude zajištěno, že přihlašovací údaje (stejně jako značka jazyka HTML celou stránku) jsou šifrována od okamžiku, kdy opustí prohlížeče, dokud nedorazí k příjemci webový server.

Pokud váš web obsahuje citlivé informace, jenom musíte používat protokol SSL na přihlašovací stránku a na jiných stránkách, kde hesla by jinak být odeslány prostřednictvím sítě jako ve formátu prostého textu. Nemusíte si dělat starosti o zabezpečení formuláře lístek ověřování, protože ve výchozím nastavení, jak šifrována a digitálně podepsané (Chcete-li zabránit manipulaci). Podrobnější informace o bezpečnostních lístek ověřování formulářů je uvedené v následujícím kurzu.

> [!NOTE]
> Mnohé weby finančních a lékařské jsou nakonfigurovány pro použití protokolu SSL na *všechny* stránky, které jsou přístupné pro ověřené uživatele. Pokud vytváříte web, systém ověřování formulářů můžete nakonfigurovat tak, aby lístek pro ověřování pomocí formulářů je pouze přenést přes zabezpečené připojení. Podíváme se na různé možnosti konfigurace ověřování formulářů v dalším kurzu  *[konfiguraci ověřování formulářů a rozšířené témata](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Krok 4: Zjišťování ověřené návštěvníky a určení Identity

V tuto chvíli jsme povolit ověřování pomocí formulářů a vytvořit elementární přihlašovací stránku, ale musíme ještě zkontrolujte, jak jsme můžete určit, zda je uživatel ověřený nebo anonymní. V některých případech může chceme zobrazit různé data nebo informace v závislosti na tom, jestli ověřený nebo anonymní uživatele je na stránce. Kromě toho je často potřeba znát identitu ověřeného uživatele.

Umožňuje rozšířit existující stránku Default.aspx k objasnění těchto postupů. Default.aspx přidejte dva ovládací prvky Panel, jednu s názvem AuthenticatedMessagePanel a jiné pojmenované AnonymousMessagePanel. Přidání ovládacího prvku popisek s názvem WelcomeBackMessage v prvním panelu. V druhém panelu Přidání ovládacího prvku hypertextový odkaz, nastavte vlastnost Text na přihlášení a jeho vlastnost NavigateUrl na ~ / Login.aspx. Deklarativní Default.aspx v tomto okamžiku by měl vypadat podobně jako následující:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Mít pravděpodobně uhádnout nyní, nápad sem je zobrazíte právě AuthenticatedMessagePanel ověřené návštěvníky a právě AnonymousMessagePanel pro anonymní návštěvníky. K tomu je potřeba nastavit tyto panely vlastnosti viditelné v závislosti na tom, zda uživatel je přihlášen nebo ne.

[Request.IsAuthenticated vlastnost](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) vrací logickou hodnotu udávající, zda byl požadavek ověřen. Zadejte následující kód na stránku\_načíst kód obslužné rutiny událostí:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

S tímto kódem na místě navštivte Default.aspx prostřednictvím prohlížeče. Za předpokladu, že máte ještě přihlásit, zobrazí se odkaz na přihlašovací stránku (viz obrázek 11). Kliknutím na tento odkaz a přihlaste se k webu. Jak jsme viděli v kroku 3, po zadání přihlašovacích údajů se vrátíte na stránku Default.aspx, ale tentokrát stránce zobrazuje vás vítá zpět! zpráva (viz obrázek 12).


[![Při návštěvě anonymně, protokolu v odkazu se zobrazí](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Obrázek 11**: při návštěvě anonymně, protokolu v odkazu se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image33.png))


[![Ověření uživatelé jsou uvedeny vás vítá zpět! Zpráva](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Obrázek 12**: Authenticated Users se zobrazují vás vítá zpět! Zprávy ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image36.png))


Můžeme určit identitu aktuálně přihlášeného uživatele prostřednictvím [HttpContext objekt](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)na [vlastnost uživatele](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). Objekt HttpContext představuje informace o aktuálním požadavku a je domovská stránka pro tyto objekty běžné ASP.NET jako odpověď, žádost a relace, mimo jiné. Představuje kontext zabezpečení aktuálního požadavku HTTP a implementuje vlastnost uživatele [rozhraní IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Pomocí FormsAuthenticationModule nastavena vlastnost uživatele. Konkrétně když FormsAuthenticationModule vyhledá lístek ověřování formulářů v příchozím požadavku, se vytvoří nový objekt GenericPrincipal a přiřadí ji k vlastnost uživatele.

Hlavní objekty (např. GenericPrincipal) poskytují informace o identitu uživatele a role, do kterých patří. Rozhraní IPrincipal definuje dva členy:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -metodu, která vrací logickou hodnotu udávající, pokud objekt patří do zadané roli.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) – vlastnosti, která vrátí objekt, který implementuje [identita rozhraní](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). Definuje rozhraní IIdentity tři vlastnosti: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), a [název](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Můžeme určit název aktuální návštěvníka pomocí následujícího kódu:

Dim currentUsersName As String = User.Identity.Name

Při ověřování pomocí formulářů [FormsIdentity objekt](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) je pro vlastnost Identity GenericPrincipal vytvořena. Třída FormsIdentity vždy vrátí hodnotu řetězec formuláře pro jeho vlastnost AuthenticationType a hodnotu True pro vlastnost jeho IsAuthenticated. Vlastnost názvu vrátí zadané uživatelské jméno při vytváření formuláře lístek ověřování. Kromě těchto tří vlastností FormsIdentity zahrnuje přístup k podkladové lístek ověřování přes jeho [lístku vlastnost](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Vlastnosti Ticket vrátí objekt typu [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), který má vlastnosti, například vypršení platnosti, IsPersistent, IssueDate, název a tak dále.

Důležité vzít v ulici, zde je, že *uživatelské jméno* parametr zadaný v FormsAuthentication.GetAuthCookie (*uživatelské jméno*, *persistCookie*), FormsAuthentication.SetAuthCookie (*uživatelské jméno*, *persistCookie*) a FormsAuthentication.RedirectFromLoginPage (*uživatelské jméno*, *persistCookie*) metody je stejné hodnoty vrácené User.Identity.Name. Lístek ověřování vytvořené tyto metody je navíc k dispozici přetypování User.Identity na FormsIdentity objekt a poté přístup k vlastnosti Ticket.:

Dim ident jako FormsIdentity = CType (User.Identity, FormsIdentity)

Dim authTicket jako FormsAuthenticationTicket = ident. Lístek

Umožňuje přidat více přizpůsobené zprávu ve Default.aspx. Aktualizovat stránku\_načíst obslužné rutiny události tak, aby byl přiřazen řetězec Vítá zpět, vlastnost Text popisku WelcomeBackMessage *uživatelské jméno*!

WelcomeBackMessage.Text = "Vítejte zpět," &amp; User.Identity.Name &amp; "!"

Obrázek 13 ukazuje účinek této změny (při přihlášení se jako uživatel Scott).


[![Zobrazení uvítací zprávy obsahuje aktuálně přihlášeného uživatelského jména](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Obrázek 13**: zobrazení uvítací zprávy zahrnuje aktuálně zaznamenána v uživatelské jméno ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Pomocí LoginView a LoginName ovládací prvky

Zobrazení jiný obsah ověřený a anonymním uživatelům je běžné požadavek; Proto je zobrazení jméno aktuálně přihlášeného uživatele. Z tohoto důvodu technologie ASP.NET obsahuje dva webové ovládací prvky, které nabízí stejnou funkčnost zobrazí obrázek 13, ale bez nutnosti napsat jediný řádek kódu.

[Ovládací prvek LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) je založené na šablonách webové ovládací prvek, který lze snadno zobrazit různé datové ověřený a anonymním uživatelům. LoginView obsahuje dvě předdefinované šablony:

- AnonymousTemplate – všechny značky, přidat do této šablony se zobrazí pouze anonymní návštěvníky.
- LoggedInTemplate - značek této šablony se zobrazí pouze ověřeným uživatelům.

Umožňuje přidání ovládacího prvku LoginView náš web stránku předlohy, Site.master. Místo přidávání právě ovládacího prvku LoginView, ale umožňuje přidat i nové ContentPlaceHolder ovládací prvek a pak přesuňte LoginView ovládací prvek v rámci této nové ContentPlaceHolder. Důvody toto rozhodnutí se stane zřejmá za chvíli.

> [!NOTE]
> Kromě AnonymousTemplate a LoggedInTemplate může zahrnovat ovládacího prvku LoginView šablony pro konkrétní role. Šablony pro konkrétní role Zobrazit revize jenom na uživatele, kteří patří do zadané roli. Na základě rolí funkce řízení LoginView vyzkoušíme budoucí kurzu.


Začněte přidáním ContentPlaceHolder, s názvem LoginContent na hlavní stránku v rámci navigaci &lt;div&gt; elementu. Můžete jednoduše přetáhněte ovládací prvek ContentPlaceHolder z panelu nástrojů na zobrazení zdroje umístění výsledný kód vpravo nahoře TODO: nabídky se sem zadejte text.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

V dalším kroku přidáte LoginView ovládací prvek v rámci LoginContent ContentPlaceHolder. Obsah umístí do ovládacích prvků ContentPlaceHolder stránky předlohy jsou považovány za *výchozí obsah* pro ContentPlaceHolder. To znamená stránek ASP.NET, které používají tuto stránku předlohy můžete zadat své vlastní obsah pro každý ContentPlaceHolder nebo použít výchozí obsah stránky předlohy.

Na kartě přihlášení v panelu nástrojů jsou umístěny LoginView a jiných ovládacích prvků související s přihlášením.


[![Ovládací prvek LoginView v panelu nástrojů](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Obrázek 14**: LoginView řídicí panelu nástrojů ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image42.png))


Dál přidejte dva &lt;Brazílie /&gt; elementy ihned po ovládacího prvku LoginView, ale pořád se nachází v ContentPlaceHolder. V tomto okamžiku navigační &lt;div&gt; elementu značek by měl vypadat třeba takto:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Z Návrháře nebo deklarativní lze definovat LoginView šablony. V sadě Visual Studio Designer rozbalte LoginView inteligentních značek, které jsou uvedeny nakonfigurované šablony v rozevíracím seznamu. Zadejte text Hello, cizím do AnonymousTemplate; v dalším kroku přidání ovládacího prvku hypertextový odkaz a nastavit jeho Text a NavigateUrl vlastnosti pro přihlášení a ~ / Login.aspx, v uvedeném pořadí.

Po dokončení konfigurace AnonymousTemplate, přepněte do LoggedInTemplate a zadejte text, "Vás vítá zpět,". Pak přetáhněte ovládací prvek LoginName z panelu nástrojů do LoggedInTemplate, jeho umístění ihned po "Vítá zpět," text. [Ovládací prvek LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), jako její název znamená, zobrazí se název aktuálně přihlášeného uživatele. Interně ovládací prvek LoginName jednoduše výstupy vlastnost User.Identity.Name

Po provedení těchto dodatky LoginView šablon, by měl vypadat podobně jako následující kód:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Pomocí tohoto přidání k hlavní stránce Site.master každé stránce v našem webu zobrazí jiná zpráva v závislosti na tom, jestli je uživatel ověřený. Obrázek 15 zobrazuje stránku Default.aspx při návštěvě uživatele Jisun prostřednictvím prohlížeče. Vítejte zpět, Jisun zpráva se opakuje dvakrát: jednou v části navigace stránky předlohy na levé straně (prostřednictvím řízení LoginView, kterou jsme právě přidali) a jednou Default.aspx obsahu oblasti (prostřednictvím – ovládací prvky Panel a programové logiky).


[![LoginView ovládací prvek zobrazí Vítejte zpět Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Obrázek 15**: LoginView ovládací prvek zobrazí vás vítá zpět, Jisun. ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image45.png))


Vzhledem k tomu, že jsme přidali LoginView k hlavní stránce, může se objevit v každé stránky na svém webovém serveru. Však může existovat webové stránky kde jsme nechcete zobrazit tato zpráva. Tyto stránek je přihlašovací stránku, protože odkaz na přihlašovací stránku zdá se, že mimo místě existuje. Vzhledem k tomu, že jsme umístění ovládacího prvku LoginView v ContentPlaceHolder na hlavní stránce, jsme naše obsahu stránce přepsat tato výchozí značka. Otevřete Login.aspx a přejděte do návrháře. Vzhledem k tomu, že jsme nejsou výslovně definované obsahu ovládacího prvku v Login.aspx pro LoginContent ContentPlaceHolder na hlavní stránce přihlašovací stránky se zobrazí kód výchozí stránky předlohy pro tento ContentPlaceHolder. Zobrazí se to prostřednictvím návrháře - LoginContent ContentPlaceHolder ukazuje výchozí značky (LoginView řízení).


[![Přihlašovací stránky zobrazuje výchozí nastavení obsahu pro LoginContent ContentPlaceHolder stránky předlohy](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Obrázek 16**: přihlašovací stránky zobrazí výchozí obsahu pro LoginContent ContentPlaceHolder stránky předlohy ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image48.png))


Pokud chcete přepsat výchozí značka pro LoginContent ContentPlaceHolder, jednoduše klikněte pravým tlačítkem na oblasti v návrháři a zvolte možnost vytvořit vlastní obsah v místní nabídce. (Když pomocí sady Visual Studio 2008 ContentPlaceHolder zahrnuje smart značek, které, pokud vybraná, nabízí stejné možnost.) Tento postup přidá nový obsah ovládacího prvku značek a tím umožňuje definovat vlastní obsah pro tuto stránku. Může přidat vlastní zprávu tady, jako je například Přihlaste se, ale můžeme právě nechte pole prázdné.

> [!NOTE]
> V sadě Visual Studio 2005, vytvoření vlastního obsahu vytvoří prázdnou obsahu ovládacího prvku do stránky ASP.NET. V sadě Visual Studio 2008 ale vytvoření vlastního obsahu zkopíruje obsah výchozí stránky předlohy do nově vytvořený obsah ovládacího prvku. Pokud používáte Visual Studio 2008, pak po vytvoření nového obsahu ovládacího prvku nezapomeňte vymažte obsah zkopírovali ze stránky předlohy.


Obrázek 17 ukazuje na stránku Login.aspx při navštívené z prohlížeče po provedení této změny. Všimněte si, že žádné Hello cizí osoby nebo vás vítá zpět *uživatelské jméno* zpráva v levé navigaci &lt;div&gt; jako v případě návštěvou Default.aspx.


[![Na přihlašovací stránku skryje LoginContent ContentPlaceHolder výchozí značka](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Obrázek 17**: skryje značek výchozí LoginContent ContentPlaceHolder na přihlašovací stránku ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Krok 5: Protokolování

V kroku 3 jsme se podívali na vytváření přihlašovací stránku k přihlášení uživatele v lokalitě, ale máme ještě chcete zjistit, jak se odhlásit uživatele. Kromě metod pro protokolování uživatel ve třídě ověřování pomocí formulářů také poskytuje [odhlášení metoda](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Metoda odhlášení jednoduše zničí lístek ověřování pomocí formulářů, tak protokolování uživatele mimo lokalitu.

Nabídka odhlášení odkaz je běžnou funkcí této technologie ASP.NET obsahuje ovládací prvek určená speciálně pro odhlášení uživatele. [Ovládací prvek LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) zobrazí LinkButton přihlášení nebo odhlášení LinkButton, v závislosti na stavu ověřování uživatele. LinkButton přihlášení je vykreslen pro anonymní uživatele, zatímco LinkButton odhlášení se zobrazí ověřeným uživatelům. Text pro přihlášení a odhlášení LinkButtons je možné nakonfigurovat přes LoginStatus LoginText a LogoutText vlastnosti.

Kliknutím na přihlášení LinkButton způsobí, že zpětné volání, ze kterého se objeví přesměrování na stránku přihlášení. Kliknutím na tlačítko LinkButton odhlášení způsobí, že ovládací prvek LoginStatus k vyvolání metody FormsAuthentication.SignOff a pak přesměruje uživatele na stránku. Stránka přihlášeného vypnout uživatele přesměruje na závisí na vlastnost LogoutAction, která lze přiřadit k jednu ze tří následujících hodnot:

- Aktualizace – výchozí; přesměruje uživatele na stránku, měla právě návštěvou. Pokud stránka, kterou právě byly návštěvou nedovoluje anonymní uživatelé, pak FormsAuthenticationModule automaticky přesměruje uživatele na přihlašovací stránku.

Můžete mít zvědaví, proč je zde provedeno přesměrování. Pokud chce uživatel zůstat na stejné stránce, proč potřebu explicitní přesměrování? Důvodem je, protože při kliknutí na LinkButton odhlášení uživatele stále má lístek pro ověřování pomocí formulářů v jejich kolekce souborů cookie. V důsledku toho postback žádosti je požadavek na ověřeného. Ovládací prvek LoginStatus volá metodu odhlášení, ale k tomu dojde poté, co FormsAuthenticationModule ověření uživatele. Proto explicitní přesměrování způsobí, že prohlížeč znovu požadavek stránku. Při požadavku prohlížeče znovu stránce byl odebrán lístek pro ověřování pomocí formulářů a proto příchozí požadavek je pro anonymní.

- Přesměrování - uživatel se přesměruje na adresu URL zadanou v LoginStatus LogoutPageUrl vlastnost.
- RedirectToLoginPage - uživatel se přesměruje na přihlašovací stránku.

Umožňuje přidání ovládacího prvku LoginStatus k hlavní stránce a nakonfigurovat ji používat k odesílání uživatele na stránku, který zobrazí výzvu k potvrzení, že jste odhlášeni možnost přesměrování. Začněte vytvořením stránky v kořenovém adresáři s názvem Logout.aspx. Nezapomeňte si tuto stránku přidružit Site.master stránky předlohy. Potom zadejte zprávu v značek vysvětlením uživateli, které byly zaprotokolovány limitu.

V dalším kroku návrat na hlavní stránku Site.master a přidání ovládacího prvku LoginStatus pod LoginView v LoginContent ContentPlaceHolder. Nastavte vlastnosti LogoutAction ovládací prvek LoginStatus přesměrování a vlastnosti LogoutPageUrl na ~/Logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Vzhledem k tomu, že LoginStatus je mimo ovládací prvek LoginView, zobrazí se pro anonymní i ověřené uživatele, ale je to způsobeno LoginStatus správně zobrazí přihlašovací nebo odhlašovací LinkButton OK. Po přidání ovládacího prvku LoginStatus protokolu v HyperLink v AnonymousTemplate je nadbytečné, takže jej odebrat.

Obrázek 18 zobrazuje Default.aspx, když navštíví Jisun. Všimněte si, že v levém sloupci zobrazí zprávu, Vítejte zpět, Jisun spolu s odkazem na odhlášení. Kliknutím na protokol se LinkButton způsobí, že zpětné volání, podepíše Jisun mimo systém a jí pak přesměruje do Logout.aspx. Jak ukazuje obrázek 19, o dobu, kterou Jisun dosáhne Logout.aspx, která má již odhlášeni a je proto anonymní. V důsledku toho levém sloupci se zobrazuje text Vítá cizím a odkaz na přihlašovací stránku.


[![Default.aspx ukazuje vás vítá zpět, Jisun společně s LinkButton odhlášení](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Obrázek 18**: Default.aspx ukazuje Vítejte zpět, Jisun společně s LinkButton odhlášení ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx zobrazí úvodní, cizím společně s LinkButton přihlášení](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Obrázek 19**: Logout.aspx zobrazí úvodní, cizím spolu s přihlašovací LinkButton ([Kliknutím zobrazit obrázek v plné velikosti](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> I doporučujeme přizpůsobení stránce Logout.aspx ke skrytí LoginContent ContentPlaceHolder stránky předlohy (jako jsme to udělali pro Login.aspx v kroku 4). Důvodem je, protože LinkButton přihlášení pro vykreslení ovládacího prvku LoginStatus (jeden pod Hello, cizím) odešle uživatele na přihlašovací stránku v parametru řetězce dotazu ReturnUrl předávání aktuální adresa URL. Stručně řečeno pokud uživatel, který se přihlásil se kliknutím na tento LoginStatus LinkButton přihlášení a pak přihlásí, bude možné přesměrován zpět na Logout.aspx, který by mohl snadno zmást uživatele.


## <a name="summary"></a>Souhrn

V tomto kurzu jsme začít s prošetření pracovního postupu ověřování formulářů a pak vrátit na implementace ověřování pomocí formulářů v aplikaci ASP.NET. Ověřování pomocí formulářů používá technologii FormsAuthenticationModule, která má dva odpovědnosti: identifikace uživatelů podle jejich lístek pro ověřování pomocí formulářů a přesměrování na přihlašovací stránku neoprávnění uživatelé.

Ověřování pomocí formulářů rozhraní .NET Framework – třída obsahuje metody pro vytváření, kontroly a odebrání lístků pro ověřování pomocí formulářů. Vlastnost Request.IsAuthenticated a objekt uživatele poskytovat další podporu programový při určování, zda je žádost o ověření a informace o identitě uživatele. Existují také LoginView LoginStatus a LoginName webové ovládací prvky, které poskytují vývojářům rychle a bez kódu způsob k provedení mnoha běžné úlohy související s přihlášením. V budoucích kurzech vyzkoušíme tyto a další související s přihlášením ovládací prvky webového podrobněji.

V tomto kurzu k dispozici zběžnou Přehled ověřování pomocí formulářů. Jsme není zkontrolujte možnosti různé konfigurace, podívejte se na jak cookieless pracovní lístků pro ověřování formulářů nebo prozkoumat jak ASP.NET chrání obsah lístek pro ověřování pomocí formulářů. Pojednává o těchto tématech a další v [další kurz](forms-authentication-configuration-and-advanced-topics-vb.md).

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Změny mezi služby IIS 6 a službu IIS7 zabezpečení](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Ovládací prvky ASP.NET přihlášení](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 zabezpečení, členství a Role správy](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [&lt;Ověřování&gt; – Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Forms&gt; Element pro &lt;ověřování&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video školení na témata, které jsou obsažené v tomto kurzu

- [Použití ověřování založeného na základních formulářích v ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>O autorovi

Scott Meisnerová, vytvořit více knih ASP/ASP.NET a zakladatele 4GuysFromRolla.com, má byla od 1998 práce s technologií Microsoft Web. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k  *[Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott lze dosáhnout za [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) nebo prostřednictvím svého blogu v [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu zahrnují Alicja Maziarz, Jan Suru a Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Předchozí](security-basics-and-asp-net-support-vb.md)
[další](forms-authentication-configuration-and-advanced-topics-vb.md)
