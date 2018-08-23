---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Zobrazení vlastní chybové stránky (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Co uživateli zobrazí když dojde k chybě za běhu ve webové aplikaci ASP.NET? Odpověď závisí na tom, na webu &lt;customErrors&gt; konfigurace...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 449b8eb26f3f6018fdd6c6dcc1de1c8d58214ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756422"
---
<a name="displaying-a-custom-error-page-c"></a>Zobrazení vlastní chybové stránky (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Co uživateli zobrazí když dojde k chybě za běhu ve webové aplikaci ASP.NET? Odpověď závisí na tom, na webu &lt;customErrors&gt; konfigurace. Ve výchozím nastavení uživatelům se zobrazí mít za následek nápadně žlutý obrazovky proclaiming, že došlo k chybě za běhu. Tento kurz ukazuje, jak upravit tato nastavení pro-vkusnou zobrazení vlastní chybové stránky, která odpovídá vaší lokality vzhled a chování.


## <a name="introduction"></a>Úvod

V ideálním by žádné chyby za běhu. Programátoři byste psát kód s nary chybu a ověřování robustního uživatelského vstupu a externí zdroje, jako jsou databázové servery a servery e-mailu by nikdy přejdou do režimu offline. Samozřejmě ve skutečnosti jsou nevyhnutelné chyby. Třídy v rozhraní .NET Framework signalizujete chybu vyvoláním výjimky. Například volání objekt SqlConnection metodu Open objektu naváže připojení k databázi pomocí připojovacího řetězce. Ale pokud databáze je mimo provoz nebo pokud jsou neplatné přihlašovací údaje v připojovacím řetězci pak otevřít vyvolá metoda výjimku `SqlException`. Výjimky mohou být zpracovány použití `try/catch/finally` bloky. Pokud kódu v rámci `try` bloku vyvolá výjimku, ovládací prvek bude převeden do bloku catch odpovídající, kde může vývojář pokusí o zotavení z chyby. Pokud neexistuje žádný odpovídající blok catch, nebo pokud není kód, který vyvolal výjimku v bloku try, výjimka percolates zásobníkem volání z search `try/catch/finally` bloky.

Pokud výjimka bubliny úplně až modul runtime ASP.NET bez zpracování, [ `HttpApplication` třídy](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)společnosti [ `Error` události](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) se vyvolá a nakonfigurovanou *chybovou stránku*  se zobrazí. Ve výchozím nastavení, rozhraní ASP.NET zobrazí chybovou stránku, která se affectionately označuje jako [žlutý obrazovky smrti](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Existují dvě verze YSOD: ukazuje podrobnosti o výjimce, trasování zásobníku a další informace užitečné při ladění aplikace vývojáře (naleznete v tématu **obrázek 1**); druhá jednoduše uvádí, že došlo k chybě za běhu (viz  **Obrázek 2**).

Podrobnosti o výjimce YSOD je velmi užitečné pro vývojáře ladění aplikace, ale zobrazují YSOD koncovým uživatelům tacky a neprofesionálně. Koncoví uživatelé měli místo toho přesměrováni na chybovou stránku, která udržuje webu vzhled a chování s přívětivější prose popisující situace. Dobrou zprávou je, že je poměrně snadné vytváření vlastní chybové stránky. Tento kurz pracuje s podívat, ASP. NET pro různé chybové stránky. Následně ukazuje, jak nakonfigurovat webovou aplikaci k zobrazení vlastní chybové stránky i v případě chyby uživatele.

### <a name="examining-the-three-types-of-error-pages"></a>Zkoumání tři typy chybové stránky

Zobrazí se při dojde k neošetřené výjimce v aplikaci ASP.NET, jeden ze tří typů chybové stránky:

- Chybová stránka výjimka podrobnosti žlutý obrazovka smrti,
- Modul Runtime chybě žlutý obrazovka smrti chybovou stránku, nebo
- Vlastní chybové stránky

Chyba vývojářům stránky jsou největší zkušenosti se YSOD podrobnosti o výjimce. Ve výchozím nastavení tato stránka se zobrazí uživatelům, kteří navštěvují místně a proto je stránka, která se zobrazí, když dojde k chybě při testování webu ve vývojovém prostředí. Jak již název napovídá, YSOD podrobnosti o výjimce poskytuje podrobnosti o výjimce - typ, zprávy a trasování zásobníku. A co víc Pokud byla výjimka vyvolána kódem ve třídě použití modelu code-behind stránky ASP.NET a aplikace je nakonfigurovaná pro ladění pak YSOD podrobnosti o výjimce se také zobrazí tento řádek kódu (a zadání několika řádků kódu výše a pod ním).

**Obrázek 1** zobrazuje stránku YSOD podrobnosti o výjimce. Poznamenejte si adresu URL v prohlížeči adresu: `http://localhost:62275/Genre.aspx?ID=foo`. Vzpomeňte si, že `Genre.aspx` stránce uvedeny recenzí v konkrétní žánr. Vyžaduje, aby `GenreId` hodnotu ( `uniqueidentifier`) se má předat řetězec dotazu, je třeba na příslušnou adresu URL, chcete-li zobrazit fiction revize `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Pokud non -`uniqueidentifier` hodnota předaná v pomocí řetězce dotazu (jako je "foo") je vyvolána výjimka.

> [!NOTE]
> Chcete-li reprodukovat tuto chybu v ukázkové webové aplikace k dispozici ke stažení můžete buď návštěvu `Genre.aspx?ID=foo` přímo nebo kliknutím na odkaz "Generovat za běhu chyba" v `Default.aspx`.


Poznamenejte si informace o výjimce v **obrázek 1**. Zpráva o výjimce, "převod se nezdařil při převodu ze znakového řetězce na typ uniqueidentifier" se nachází v horní části stránky. Typ výjimky, `System.Data.SqlClient.SqlException`, je uvedený také. Je také trasování zásobníku.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Obrázek 1**: Podrobnosti o výjimce YSOD obsahuje informace o výjimce  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image3.png))

Jiný typ YSOD je YSOD chyba modulu Runtime a je zobrazena ve **obrázek 2**. Chyba YSOD Runtime informuje návštěvník, který došlo k chybě za běhu, ale neobsahuje žádné informace o výjimce, která byla vyvolána. (To však, poskytují pokyny o tom, chcete-li zobrazit podrobnosti o chybě úpravou `Web.config` soubor, který je součástí kvůli tomu takové YSOD, neprofesionálně.)

Ve výchozím nastavení, YSOD chyba modulu Runtime se zobrazí uživatelům, kteří navštíví vzdáleně (prostřednictvím http://www.yoursite.com), jak dokazuje adresu URL v adresním řádku prohlížeče v **obrázek 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Existují dva různé obrazovky YSOD vývojáři jsou zajímat, podrobnosti o chybě, protože tyto informace by neměl být zobrazeno na živý web jako může odhalit potenciální ohrožení zabezpečení a dalších citlivých údajů všem uživatelům, kteří navštíví váš lokalita.

> [!NOTE]
> Pokud sledujete a používáte DiscountASP.NET jako webového hostitele, můžete si všimnout, že chyba YSOD Runtime nezobrazí při návštěvě živého webu. Je to proto DiscountASP.NET má své servery ve výchozím nastavení nakonfigurované zobrazíte YSOD podrobnosti o výjimce. Dobrou zprávou je, že toto výchozí chování můžete přepsat tak, že přidáte `<customErrors>` části vašeho `Web.config` souboru. V části "Konfigurace které stránky se zobrazí chyba" prověří `<customErrors>` části podrobně.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Obrázek 2**: Chyba za běhu YSOD nezahrnuje žádné podrobnosti o chybě  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image6.png))

Typ třetí chybové stránky je vlastní chybovou stránku, která je webová stránka, kterou vytvoříte. Výhodou vlastní chybové stránky je, že máte plnou kontrolu nad informace, které se zobrazí uživateli spolu s vzhled a chování stránky; Vlastní chybové stránky můžete použít stejnou stránku předlohy a styly jako vaše stránky. V části "Použití vlastní chybovou stránku" vás provede vytvořením vlastní chybovou stránku a nastavit ho na zobrazení v případě neošetřené výjimce. **Obrázek 3** nabízí stručný ve špičce, tento vlastní chybové stránky. Jak je vidět, vzhled a chování chybové stránky je mnohem více profesionálního než buď žlutý obrazovky smrti, znázorňuje obrázek 1 a 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Obrázek 3**: vlastní chybovou stránku nabízí lépe přizpůsobené vzhled a chování  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image9.png))

Věnujte chvíli kontrole adresního řádku prohlížeče v **obrázek 3**. Všimněte si, že do adresního řádku zobrazí adresu URL vlastní chybové stránky (`/ErrorPages/Oops.aspx`). Na obrázcích 1 a 2 žlutý obrazovky smrti se zobrazí na stejné stránce, která chyba pochází z (`Genre.aspx`). Vlastní chybové stránky se předá adresu URL stránky, kde došlo k chybě prostřednictvím `aspxerrorpath` parametr řetězce dotazu.

## <a name="configuring-which-error-page-is-displayed"></a>Zobrazí se konfigurace, které chybovou stránku

Který tři možné chybové stránky se zobrazí jsou založené na dvou proměnných:

- Informace o konfiguraci `<customErrors>` části, a
- Určuje, zda je uživatel následujícím webu, místně nebo vzdáleně.

[ `<customErrors>` Části](https://msdn.microsoft.com/library/h0hfz6fc.aspx) v `Web.config` má dva atributy, které ovlivňují, jaká chybová stránka je zobrazena: `defaultRedirect` a `mode`. `defaultRedirect` Atribut je volitelný. Pokud zadaná, určuje adresu URL vlastní chybové stránky a označuje, že má vlastní chybovou stránku zobrazit místo YSOD chyba modulu Runtime. `mode` Atribut je vyžadován a přijímá jednu ze tří hodnot: `On`, `Off`, nebo `RemoteOnly`. Tyto hodnoty mají následující chování:

- `On` – Označuje, že vlastní chybové stránky nebo YSOD chyba modulu Runtime se zobrazí všechny návštěvníci, bez ohledu na to, zda jsou místní nebo vzdálené.
- `Off` -Určuje, zda je zobrazen YSOD podrobnosti o výjimce pro všechny návštěvníky, bez ohledu na to, zda jsou místní nebo vzdálené.
- `RemoteOnly` – Označuje, že vlastní chybové stránky nebo YSOD chyba modulu Runtime se zobrazí vzdálené návštěvníci, zatímco YSOD podrobnosti výjimek se zobrazí místní návštěvníků.

Pokud neurčíte jinak, ASP.NET funguje jakoby měl nastavte atribut mode `RemoteOnly` a nebyl zadán `defaultRedirect` hodnotu. Jinými slovy výchozí chování je, že YSOD podrobnosti o výjimce se zobrazí místní návštěvníků při YSOD chyba modulu Runtime se zobrazí návštěvníkům vzdálené. Toto výchozí chování můžete přepsat tak, že přidáte `<customErrors>` části do vaší webové aplikace `Web.config file.`

## <a name="using-a-custom-error-page"></a>Použití vlastní chybové stránky

Každá webová aplikace, musí mít vlastní chybové stránky. Poskytuje více profesionálně vypadajících alternativu YSOD chyba modulu Runtime, je snadné vytváření a konfigurace aplikace pro použití se vlastní chybová stránka jenom chvíli trvá. Prvním krokem je vytvoření vlastní chybovou stránku. Byly přidány nové složky do recenzí aplikaci s názvem `ErrorPages` a přidán do, s názvem novou stránku ASP.NET `Oops.aspx`. Jste na stránce použít stejnou stránku předlohy jako zbytek stránky na webu tak, aby automaticky dědí stejné vzhledu a chování.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Obrázek 4**: vytvoření vlastní chybové stránky

V dalším kroku Věnujte několik minut, vytváření obsahu pro chybovou stránku. Vytvořili jste velmi jednoduchá vlastní chybovou stránku s zpráva oznamující, že došlo k neočekávané chybě a odkaz zpět na domovskou stránku webu.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Obrázek 5**: návrh vlastní chybovou stránku  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image14.png))

S chybovou stránku dokončit nakonfigurujte webovou aplikaci používat vlastní chybovou stránku namísto YSOD chyba modulu Runtime. To lze provést zadáním adresy URL v chybové stránky `<customErrors>` oddílu `defaultRedirect` atribut. Přidejte následující kód do vaší aplikace `Web.config` souboru:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Výše uvedené značky konfiguruje aplikaci, aby uživatelům, kteří navštíví místně, při použití vlastní chybovou stránku Oops.aspx pro tyto uživatele vzdáleně na Zobrazit YSOD podrobnosti o výjimce. Tento údaj zobrazíte v akci, nasazení webu do produkčního prostředí a pak na stránce Genre.aspx na živém webu s hodnotou neplatný řetězec dotazu. Zobrazí se vlastní chybové stránky (vrátit zpět k **obrázek 3**).

Pokud chcete ověřit, že se vlastní chybová stránka se zobrazí pouze na vzdálené uživatele, najdete `Genre.aspx` stránka s neplatný řetězec dotazu z vývojového prostředí. Stále byste měli vidět podrobnosti YSOD výjimky (vrátit zpět k **obrázek 1**). `RemoteOnly` Nastavení zajistí, že uživatelům, kteří navštíví web na produkční prostředí uvidí vlastní chybovou stránku, když vývojáři pracující místně nadále zobrazovat podrobnosti o výjimce.

## <a name="notifying-developers-and-logging-error-details"></a>Upozornění vývojáři a protokolování podrobností o chybách

Chyby, ke kterým dochází ve vývojovém prostředí byly způsobeny developer pracoval na svém počítači. Marcela se zobrazí informace o této výjimky v YSOD podrobnosti o výjimce a ví, jaké kroky, které byl výkon, když došlo k chybě. Ale při výskytu chyby v produkčním prostředí, vývojář nemá žádné informace, že pokud koncový uživatel na web trvá určitou dobu ohlaste chybu došlo k chybě. A i v případě, že uživatel dostane mimo jeho způsob, jak výstrahy vývojový tým, ke které došlo k chybě bez znalosti typ výjimky, zprávu a trasování zásobníku, může být obtížné diagnostikovat příčinu chyby, natož jeho řešení.

Z těchto důvodů je prvořadá, že všechny chyby v provozním prostředí přihlášený a některé trvalého úložiště (například databáze), který se zobrazí výstraha vývojáři této chyby. Vlastní chybová stránka může jevit jako vhodné místo pro tento protokolování a oznámení. Vlastní chybové stránky bohužel nemá přístup k podrobnosti o chybě a proto jej nelze použít pro přihlášení tyto informace. Dobrou zprávou je, že existuje mnoho způsobů, jak zachytit podrobnosti o chybě a k protokolování je a prozkoumat Toto téma podrobněji v následujících třech kurzech.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Použití různých vlastní chybové stránky pro stavy různých chyb HTTP

Když výjimka je vyvolána stránky ASP.NET a není zpracována, výjimka percolates až runtime ASP.NET, která zobrazuje nakonfigurované chybovou stránku. Pokud žádost o vstupu do modulu ASP.NET, ale z nějakého důvodu nejde zpracovat – možná požadovaný soubor není nalezen nebo si přečtěte zakázali oprávnění k souboru - pak vydá modul ASP.NET `HttpException`. Tato výjimka, jako jsou výjimky vyvolané z stránek ASP.NET, bubliny až do modulu runtime, způsobí odpovídající chybovou stránku, který se má zobrazit.

Co to znamená, že pro webovou aplikaci v produkčním prostředí je, že pokud uživatel požádá o stránku, která není nenajde, zobrazí se vlastní chybová stránka. **Obrázek 6** ukazuje takových příkladů. Protože se jedná o požadavek na neexistující stránku (`NoSuchPage.aspx`), `HttpException` je vyvolána a zobrazí se vlastní chybové stránky (Poznámka: odkaz na `NoSuchPage.aspx` v `aspxerrorpath` parametr řetězce dotazu).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Obrázek 6**: modul Runtime ASP.NET zobrazí nakonfigurované stránky v odpovědi na chybu neplatná požádat o ([kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image17.png))

Ve výchozím nastavení všechny typy chyb způsobit stejné vlastní chybovou stránku, který se má zobrazit. Můžete však zadat jiné vlastní chybové stránky pro konkrétní HTTP stavový kód pomocí `<error>` podřízené prvky v rámci `<customErrors>` oddílu. Například pokud chcete mít různé chybové stránky zobrazí v případě stránka nebyla nalezena chyba, která má stavový kód HTTP 404, aktualizujte `<customErrors>` části zahrnout následující kód:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Díky této změně na místě pokaždé, když se uživatel navštívit vzdáleně požádá o prostředek technologie ASP.NET, která neexistuje, že budete přesměrováni na `404.aspx` vlastní chybovou stránku místo `Oops.aspx`. Jako **obrázek 7** znázorňuje, `404.aspx` stránka může obsahovat zprávu konkrétnější než obecné vlastní chybovou stránku.

> [!NOTE]
> Podívejte se na [404 chybové stránky, další jednou](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) pokyny k vytváření efektivní 404 chybové stránky.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Obrázek 7**: vlastní 404 chybovou stránku zobrazí zprávu cílenější než `Oops.aspx`  
 ([Kliknutím ji zobrazíte obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image20.png)) 

Protože víte, že `404.aspx` stránky se dosáhne, pouze když uživatel odešle požadavek pro stránku, který nebyl nalezen, můžete vylepšit tento vlastní chybové stránky zahrnují funkce, které pomůžou uživatele vyřešit tento konkrétní typ chyby. Například můžete vytvořit tabulku databáze, která mapuje známé špatné adresy URL do správné adresy URL a pak je mít `404.aspx` vlastní chybovou stránku spuštění dotazu na tabulky a navrhnout stránek, které uživatel může být pokouší o spojení.

> [!NOTE]
> Vlastní chybové stránky se zobrazí pouze po odeslání žádosti na prostředek zpracovává modul ASP.NET. Jak jsme probírali v [základní rozdíly mezi IIS a serveru ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) výukový program, webový server může zpracovat určité požadavky samotné. Ve výchozím nastavení webové služby IIS serveru procesy požadavky pro statický obsah – třeba obrázky nebo soubory HTML bez vyvolání modul ASP.NET. V důsledku toho pokud uživatel požaduje soubor obrázku neexistující dostanou zpět služby IIS výchozí chyby 404, nikoli ASP. NET společnosti nakonfigurovat chybovou stránku.


## <a name="summary"></a>Souhrn

Když dojde k neošetřené výjimce v aplikaci ASP.NET, uživateli se zobrazí jeden ze tří chybové stránky: výjimka podrobnosti žlutý obrazovky z smrti; Chyba za běhu žlutý obrazovky smrti. nebo vlastní chybové stránky. Na stránce chyby, které se zobrazí, závisí na vaší aplikace `<customErrors>` konfigurace a zda je uživatel hostujících místně nebo vzdáleně. Výchozí chování je zobrazit YSOD podrobnosti o výjimce pro místní návštěvníky a YSOD chyba modulu Runtime na vzdálené návštěvníků.

Zatímco YSOD chyba modulu Runtime skryje chyby potenciálně citlivé informace zadané uživatelem následujícím webu, přeruší z vašeho webu vzhled a chování a zpřístupňuje buggy vaší aplikace. Lepším řešením je použití vlastní chybovou stránku, která zahrnuje vytváření a návrhu vlastní chybové stránky a zadání jeho adresu URL `<customErrors>` oddílu `defaultRedirect` atribut. Může probíhat dokonce na více vlastní chybové stránky pro různé stavy chyby protokolu HTTP.

Vlastní chybové stránky je prvním krokem při chybu komplexní strategie pro web v produkčním prostředí pro zpracování. Pro vývojáře chyby výstrahy a protokolování její podrobnosti jsou také důležité kroky. Následující tři kurzy prozkoumat techniky pro oznámení o chybě a protokolování.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Chybové stránky, ještě jednou](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Pokyny k návrhu pro výjimky](https://msdn.microsoft.com/library/ms229014.aspx)
- [Uživatelsky přívětivé chybové stránky](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Zpracování a vyvolání výjimek](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Správně pomocí vlastní chybové stránky technologie ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Předchozí](strategies-for-database-development-and-deployment-cs.md)
> [další](processing-unhandled-exceptions-cs.md)
