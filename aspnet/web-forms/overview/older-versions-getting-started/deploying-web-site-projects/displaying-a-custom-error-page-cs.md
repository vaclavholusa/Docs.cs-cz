---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: "Zobrazení vlastní chybovou stránku (C#) | Microsoft Docs"
author: rick-anderson
description: "Co uživatel uvidí když dojde k chybě za běhu ve webové aplikaci ASP.NET? Odpověď závisí na tom webu &lt;customErrors&gt; konfigurace..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 92a7e945a6f82e78b848bae8f4f362e16a567b1f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-custom-error-page-c"></a>Zobrazení vlastní chybovou stránku (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Co uživatel uvidí když dojde k chybě za běhu ve webové aplikaci ASP.NET? Odpověď závisí na tom webu &lt;customErrors&gt; konfigurace. Ve výchozím nastavení uživatelům se zobrazí nevzhledné žlutý obrazovky, proclaiming, že došlo k chybě za běhu. Tento kurz ukazuje, jak přizpůsobit zobrazení,-vkusnou vlastní chybovou stránku, který odpovídá vaší lokality vzhled a chování těchto nastavení.


## <a name="introduction"></a>Úvod

V ideálním by existovat žádné chyby. Programátory byste měli psát kód s nary chyby a ověření vstupu uživatele robustní a externí prostředky jako databázové servery a servery e-mailu přejde nikdy offline. Ve skutečnosti jsou samozřejmě nevyhnutelné chyby. Třídy v rozhraní .NET Framework signál chybu podle došlo k výjimce. Například volání SqlConnection otevřete metody objektu naváže připojení k do databáze určené parametrem připojovací řetězec. Ale pokud se databáze nachází mimo provoz nebo pokud neplatných přihlašovacích údajů v připojovacím řetězci potom otevřete vyvolá metoda `SqlException`. Může být ke zpracování výjimek pomocí `try/catch/finally` bloky. Pokud kódu v rámci `try` bloku vyvolá výjimku, ovládací prvek je přenesen na blok catch odpovídající, kde může vývojář pokusí o zotavení z chyby. Pokud neexistuje žádný odpovídající blok catch, nebo pokud není kód, který vyvolal výjimku do bloku try, výjimka percolates zásobníkem volání search z `try/catch/finally` bloky.

Pokud výjimka bubliny až modulem runtime ASP.NET bez zpracování, [ `HttpApplication` třída](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx)na [ `Error` událostí](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.error.aspx) se vyvolá a nakonfigurované *chybovou stránku*  se zobrazí. Ve výchozím nastavení, rozhraní ASP.NET zobrazí chybovou stránku s affectionately odkazuje jako [žlutý obrazovka smrti](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Existují dvě verze YSOD: jeden zobrazuje podrobnosti o výjimce, trasování zásobníku a další informace užitečné pro vývojáře ladění aplikace (najdete v části **obrázek 1**); dalších jednoduše stavy, že došlo k chybě spuštění (viz  **Obrázek 2**).

Podrobnosti o výjimce YSOD je velmi užitečné pro vývojáře ladění aplikace, ale zobrazuje YSOD koncovým uživatelům je tacky a sobě podáváte neprofesionální. Místo toho koncoví uživatelé měli přesměrováni na chybovou stránku, která udržuje lokality vzhled a chování s přívětivější prose popisující situaci. Dobrá zpráva je, že je poměrně snadné vytváření vlastní chybovou stránku. V tomto kurzu začíná podívejte se na ASP. NET na různé chybové stránky. Potom ukazuje, jak nakonfigurovat webovou aplikaci zobrazit uživatelům vlastní chybovou stránku při krátkodobém k chybě.

### <a name="examining-the-three-types-of-error-pages"></a>Zkoumání tři typy chybové stránky

Když dojde k neošetřené výjimce v aplikaci ASP.NET, jeden ze tří typů chybové stránky se zobrazí:

- Výjimka podrobnosti žlutý obrazovka smrti chybové stránky,
- Modul Runtime chyba žlutý obrazovka smrti chybové stránky, nebo
- Vlastní chybovou stránku

Vývojáři Chyba stránek nejvíce obeznámeni s je YSOD podrobnosti o výjimce. Ve výchozím nastavení tato stránka se zobrazí uživatelům, kteří navštěvují místně a proto je stránka, která se zobrazí, když dojde k chybě při testování webu ve vývojovém prostředí. Jak již název napovídá, YSOD podrobnosti o výjimce poskytuje podrobnosti o výjimce - typ, zprávu a trasování zásobníku. Navíc pokud kód ve třídě kódu stránky ASP.NET byla vyvolána výjimka, a pokud je aplikace konfigurována pro ladění pak YSOD podrobnosti výjimky se zobrazí také tohoto řádku kódu (a zadání několika řádků kódu výše a pod ním).

**Obrázek 1** zobrazuje stránku YSOD podrobnosti o výjimce. Poznamenejte si adresu URL v okně prohlížeče adresu: `http://localhost:62275/Genre.aspx?ID=foo`. Odvolat, který `Genre.aspx` stránka obsahuje seznam recenze adresáře v konkrétní genre. Vyžaduje, aby `GenreId` hodnotu ( `uniqueidentifier`) být předána querystring; je třeba příslušnou adresou URL zobrazíte recenze fiction `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Pokud jinou hodnotu než`uniqueidentifier` byla předána hodnota v pomocí řetězce dotazu (jako je "foo") je vyvolána výjimka.

> [!NOTE]
> Reprodukujte této chybě v ukázkové webové aplikace k dispozici ke stažení může buď návštěvu `Genre.aspx?ID=foo` přímo nebo klikněte na odkaz "Generovat za běhu chyba" v `Default.aspx`.


Všimněte si informace o výjimce uvedené v **obrázek 1**. Zpráva o výjimce, "při převodu ze znakového řetězce na typ uniqueidentifier" se nachází v horní části stránky. Typ výjimky, `System.Data.SqlClient.SqlException`, je uvedena také. Je také trasování zásobníku.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Obrázek 1**: Podrobnosti o výjimce YSOD obsahuje informace o výjimce  
 ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image3.png))

Jiný typ YSOD je YSOD chyba Runtime a zobrazen v **na obrázku 2**. Návštěvník, který došlo k chybě spuštění informuje YSOD chyby za běhu, ale neobsahuje žádné informace o výjimku, která byla vyvolána. (Ji, ale poskytují pokyny o tom, chcete-li zobrazit podrobnosti o chybě změnou `Web.config` souboru, který je součástí díky takové YSOD, neprofesionálně.)

Ve výchozím nastavení, YSOD chyby za běhu, je zobrazena uživatelům, kteří navštíví vzdáleně (prostřednictvím http://www.yoursite.com), jako formu cenných adresu URL v adresním řádku prohlížeče v **na obrázku 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Existují dva různé YSOD obrazovky se vývojáři dozvědět podrobnosti o chybě, protože tyto informace by neměly být uváděny na živý web může odhalit, potenciální ohrožení zabezpečení nebo jiných citlivých informací všem uživatelům, kteří navštíví váš lokalita.

> [!NOTE]
> Pokud postupujete podle a používají DiscountASP.NET jako webového hostitele, můžete si všimnout, že YSOD chyba Runtime nezobrazí při návštěvě webu za provozu. Je to proto DiscountASP.NET má své servery ve výchozím nastavení nakonfigurované zobrazíte YSOD podrobnosti o výjimce. Dobrá zpráva je, že toto výchozí chování můžete přepsat tak, že přidáte `<customErrors>` části k vaší `Web.config` souboru. V části "Konfigurace které stránky se zobrazí chyba" prověří `<customErrors>` části podrobně.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Obrázek 2**: Chyba za běhu YSOD neobsahuje žádné podrobnosti o chybě  
 ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image6.png))

Třetí typ chybová stránka je vlastní chybovou stránku, která je webová stránka, kterou vytvoříte. Výhodou vlastní chybovou stránku je, že máte plnou kontrolu nad informace, které se zobrazí uživateli spolu s stránky vzhled a chování; vlastní chybovou stránku můžete použít stejný hlavní stránky a styly jako svoje jiné stránky. V části "Použití vlastní chybovou stránku" provede procesem vytvoření vlastní chybovou stránku a nastavit ho na zobrazení v případě neošetřené výjimce. **Obrázek 3** nabízí vás zajímá ve špičce této vlastní chybové stránky. Jak je vidět vzhledu a chování chybové stránky je mnohem víc profesionální než buď žlutý obrazovky smrti znázorňuje obrázek 1 a 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Obrázek 3**: vlastní chybovou stránku nabízí více přizpůsobený vzhled a chování  
 ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image9.png))

Za chvíli ke kontrole adresním řádku prohlížeče v **obrázek 3**. Všimněte si, že na adresním řádku zobrazuje adresu URL vlastní chybové stránky (`/ErrorPages/Oops.aspx`). Na obrázcích 1 a 2 žlutý obrazovky smrti se zobrazují na stejné stránce, která chyba pochází z (`Genre.aspx`). Vlastní chybové stránky je předán adresu URL stránky, kde se stala chyba prostřednictvím `aspxerrorpath` parametr řetězce dotazu.

## <a name="configuring-which-error-page-is-displayed"></a>Zobrazí se konfigurace který chybová stránka

Které tři možné chybové stránky se zobrazí je založena na dvě proměnné:

- Informace o konfiguraci v `<customErrors>` části, a
- Jestli návštěvy webu místně nebo vzdáleně.

[ `<customErrors>` Části](https://msdn.microsoft.com/en-us/library/h0hfz6fc.aspx) v `Web.config` má dva atributy, které ovlivňují, jaké chybová stránka se zobrazí: `defaultRedirect` a `mode`. `defaultRedirect` Atribut je volitelný. Pokud je zadán, určuje adresu URL vlastní chybové stránky a určuje, že má být zobrazena vlastní chybovou stránku místo YSOD chyba modulu Runtime. `mode` Atribut je povinný a přijímá jednu ze tří hodnot: `On`, `Off`, nebo `RemoteOnly`. Tyto hodnoty mají následující chování:

- `On`-Určuje, že vlastní chybovou stránku nebo YSOD chyba Runtime se zobrazí všechny návštěvníky, bez ohledu na to, zda jsou místní nebo vzdálené.
- `Off`-Určuje, že je pro všechny návštěvníky, bez ohledu na to, zda jsou místní nebo vzdálené zobrazí YSOD podrobnosti o výjimce.
- `RemoteOnly`-Určuje, že vlastní chybovou stránku nebo YSOD chyba Runtime se zobrazí vzdálené návštěvníky při YSOD podrobnosti výjimky se zobrazí místní návštěvníky.

Pokud neurčíte jinak, ASP.NET chová, jako měl nastavte atribut režimu na `RemoteOnly` a nebyl zadán `defaultRedirect` hodnotu. Jinými slovy výchozí chování je, že YSOD podrobnosti výjimky se zobrazí místní návštěvníky při vzdálené návštěvníky se zobrazí YSOD chyby za běhu. Toto výchozí chování můžete přepsat tak, že přidáte `<customErrors>` části k vaší webové aplikaci`Web.config file.`

## <a name="using-a-custom-error-page"></a>Pomocí vlastní chybové stránky.

Každá webová aplikace by měla mít vlastní chybovou stránku. Poskytuje více profesionální alternativu, která umožňuje YSOD chyby za běhu, je snadné vytvořit a konfigurace aplikace pro používání vlastní chybovou stránku pouze chvíli trvá. Prvním krokem je vytvoření vlastní chybovou stránku. Byly přidány nové složky do adresáře recenze aplikaci s názvem `ErrorPages` a přidat do, že nová stránka ASP.NET s názvem `Oops.aspx`. Máte stejný hlavní stránku použít jako ostatní stránky na váš web tak, aby automaticky zdědí stejné vzhledu a chování stránky.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Obrázek 4**: vytvoření vlastní chybovou stránku

V dalším kroku Věnujte několik minut, vytváření obsahu pro chybovou stránku. Vytvořili jste spíše jednoduché vlastní chybovou stránku s zpráva označující, že došlo k neočekávané chybě a odkaz zpět na domovskou stránku webu.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Obrázek 5**: návrh vlastní chybovou stránku  
 ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image14.png))

Pomocí chybová stránka dokončení nakonfigurujte webovou aplikaci pro použití se vlastní chybová stránka místo YSOD chyby za běhu. Toho dosahuje tak, že zadáte adresu URL stránky s chybou v `<customErrors>` oddílu `defaultRedirect` atribut. Přidejte následující kód do vaší aplikace `Web.config` souboru:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Výše uvedený kód konfiguruje aplikaci, aby uživatelům, kteří navštíví místně, při použití vlastní chybovou stránku Oops.aspx pro uživatele, kteří navštěvují vzdáleně zobrazit YSOD podrobnosti o výjimce. Zobrazení tohoto v akci, nasazení webu do provozního prostředí a potom navštivte stránku Genre.aspx na živý web s hodnotou neplatný řetězec dotazu. Zobrazí se vlastní chybová stránka (odkazuje zpět na **obrázek 3**).

Pokud chcete ověřit, že se vlastní chybová stránka se zobrazí pouze pro vzdálené uživatele, přejděte `Genre.aspx` stránka s neplatný řetězec dotazu z vývojového prostředí. Měli byste vidět stále YSOD podrobnosti výjimky (odkazuje zpět na **obrázek 1**). `RemoteOnly` Nastavení zajistí, že uživatelům, kteří navštíví web v provozním prostředí uvidí vlastní chybovou stránku, zatímco vývojáře, kteří pracují místně dále najdete v podrobnostech výjimky.

## <a name="notifying-developers-and-logging-error-details"></a>Upozornění vývojáři a protokolování podrobnosti o chybě

Chyby, ke kterým dochází ve vývojovém prostředí byla způsobena vývojáře pracoval na svém počítači. Jana se zobrazují informace v výjimky v YSOD podrobnosti o výjimce, a Jana ví, jaké kroky Jana byl výkon, když se stala chyba. Ale když dojde k chybě na produkční, vývojář nemá žádné informace, pokud koncový uživatel navštívil tento web trvá dobu ohlaste chybu došlo k chybě. A i v případě, že uživatel přejde z jeho způsob, jak výstrahy vývojový tým, který došlo k chybě, aniž by věděly, typ výjimky, zprávu a může být obtížné diagnostikovat příčinu chyby, let alone opravit trasování zásobníku.

Z těchto důvodů, které je prvořadá, že všechny chyby v provozním prostředí je zaznamenána některé trvalého úložiště (například databáze) a že se zobrazí výstraha vývojáři této chyby. Vlastní chybové stránky mohou jevit jako místo pro tento protokolování a oznámení. Bohužel se vlastní chybová stránka nemá přístup k podrobnosti o chybě a proto jej nelze použít k přihlášení tyto informace. Dobrá zpráva je, že existuje několik způsobů, jak zachytit podrobnosti o chybě a k jejich přihlášení, a následující tři kurzy prozkoumat Toto téma podrobněji.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Pomocí různých vlastní chybové stránky pro stavy, které jsou různé chyb HTTP

Když je vyvolané stránku ASP.NET a nejsou zpracovávány výjimka, výjimka percolates až modul runtime ASP.NET, který zobrazí stránku nakonfigurované chyby. Pokud požadavek obsahuje do modulu ASP.NET, ale z nějakého důvodu nelze zpracovat – pravděpodobně požadovaný soubor není nalezen nebo číst oprávnění byla zakázána pro soubor - pak modul ASP.NET vyvolá `HttpException`. Tato výjimka, jako je výjimek vyvolaných ze stránek ASP.NET, bubliny až modulu runtime způsobuje odpovídající chybovou stránku, který se má zobrazit.

Co to znamená pro webovou aplikaci v produkčním prostředí je, že pokud uživatel požádá o stránky, který nebyl nalezen a zobrazí se vlastní chybovou stránku. **Obrázek 6** takové slouží jako příklad. Protože se jedná o požadavek na neexistující stránce (`NoSuchPage.aspx`), `HttpException` je vyvolána a zobrazí se vlastní chybová stránka (Poznámka: odkaz na `NoSuchPage.aspx` v `aspxerrorpath` parametr řetězce dotazu).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Obrázek 6**: modulem Runtime ASP.NET zobrazí nakonfigurované stránky v odpovědi na chybu neplatná požádat o ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image17.png))

Ve výchozím nastavení všechny typy chyb způsobit stejné vlastní chybovou stránku, který se má zobrazit. Můžete však zadat jiný vlastní chybovou stránku pro konkrétní HTTP stav kódu pomocí `<error>` podřízených elementů v rámci `<customErrors>` části. Například máte různé chybovou stránku, zobrazí se v případě stránka nebyla nalezena chyba, která má kód stavu HTTP 404, aktualizovat `<customErrors>` přidány následující kód:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Díky této změně zavedené vždy, když uživatel navštěvující vzdáleně požádá o prostředek ASP.NET, který ještě neexistuje, bude přesměrován na `404.aspx` vlastní chybovou stránku místo `Oops.aspx`. Jako **na obrázku 7** znázorňuje, `404.aspx` stránky může obsahovat více konkrétní zprávou než obecné vlastní chybovou stránku.

> [!NOTE]
> Podívejte se na [404 chybové stránky, další jednou](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) pokyny k vytváření efektivní 404 chybové stránky.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Obrázek 7**: vlastní 404 chybovou stránku zobrazí zprávu cílenější než`Oops.aspx`  
 ([Kliknutím zobrazit obrázek v plné velikosti](displaying-a-custom-error-page-cs/_static/image20.png)) 

Vzhledem k tomu, že víte, že `404.aspx` stránky je dosaženo, pouze když uživatel odešle žádost pro stránku, který nebyl nalezen, můžete vylepšit tento vlastní chybovou stránku zahrnout funkce, které pomůžou uživatele adres konkrétního typu chyby. Například můžete vytvořit tabulku databáze, která mapuje známé chybný adresy URL na dobrý adresy URL a pak `404.aspx` vlastní chybovou stránku spuštění dotazu na tabulky a navrhněte stránky uživatel pokouší připojit.

> [!NOTE]
> Vlastní chybové stránky se zobrazí pouze po odeslání žádosti k prostředku zpracovává modul ASP.NET. Jak již bylo zmíněno [základní rozdíly mezi IIS a ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) kurz, webový server může zpracovat určitých požadavků sám sebe. Ve výchozím nastavení webové služby IIS serveru procesy požadavky na statický obsah, jako jsou bitové kopie a soubory HTML bez vyvolání modul ASP.NET. V důsledku toho pokud uživatel požádá o nějaký soubor bitové kopie neexistující se dostane zpět služby IIS výchozí 404 chybovou zprávu, a nikoli ASP. NET je nakonfigurovat chybovou stránku.


## <a name="summary"></a>Souhrn

Když dojde k neošetřené výjimce v aplikaci ASP.NET, uživateli se zobrazí jednu z tři chybové stránky: výjimka podrobnosti žlutý obrazovky z smrti; Chyba za běhu žlutý obrazovky smrti. nebo vlastní chybovou stránku. Stránku, která chyba se zobrazí, závisí na aplikace `<customErrors>` konfigurace a jestli je uživatel navštěvující místně nebo vzdáleně. Výchozí chování je pro zobrazení YSOD podrobnosti výjimky pro místní návštěvníky a YSOD chyba modulu Runtime pro vzdálené návštěvníky.

Při YSOD chyba Runtime skryje informace o chybě potenciálně citlivých od uživatele, kteří navštěvují webu, dělí z vaší lokality vzhled a chování a umožňuje aplikaci vypadat buggy. Lepší možných přístupů je použít vlastní chybovou stránku, která zahrnuje vytváření a návrhu vlastní chybovou stránku a určení jeho adresy URL v `<customErrors>` oddílu `defaultRedirect` atribut. Můžete mít i více vlastní chybové stránky pro různé stavy chyby protokolu HTTP.

Vlastní chybové stránky je prvním krokem při komplexní chyba zpracování strategie pro web v produkčním prostředí. Důležité kroky jsou také vývojáře chyby výstrahy a protokolování její podrobnosti. Následující tři kurzy prozkoumejte techniky pro chyby oznámení a provádí protokolování.

Radostí programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Chybové stránky, ještě jednou](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Pokyny pro návrh pro výjimky](https://msdn.microsoft.com/en-us/library/ms229014.aspx)
- [Uživatelsky přívětivý chybové stránky](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Zpracování a generování výjimek](https://msdn.microsoft.com/en-us/library/5b2yeyab.aspx)
- [Správně pomocí vlastní chybové stránky technologie ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

>[!div class="step-by-step"]
[Předchozí](strategies-for-database-development-and-deployment-cs.md)
[další](processing-unhandled-exceptions-cs.md)
