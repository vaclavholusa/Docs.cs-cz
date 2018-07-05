---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Horní funkce v rozhraní ASP.NET Web Pages 2 | Dokumentace Microsoftu
author: microsoft
description: Toto téma obsahuje přehled o hlavní nové funkce v ASP.NET Web Pages 2, jednoduché webové programování rozhraní, která je součástí WebMatr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: 3cdb9d83e0f612ad7404bfaa9721b580916e112d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385263"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Hlavní funkce webových stránek v ASP.NET 2
====================
podle [Microsoft](https://github.com/microsoft)

> Tento článek poskytuje přehled o hlavní nové funkce v RC 2 technologie ASP.NET Web Pages, jednoduché webové programování rozhraní, která je součástí [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Co je součástí:** 
> 
> - [Instalace služby WebMatrix](#install)
> - [Nové a vylepšené funkce](#New_and_Enhanced_Features)
> 
>     - [Změny pro verzi RC](#Changes_for_the_RC_Version)
>     - [Změn pro vydání Beta](#Changes_for_the_Beta_Version)
>     - [Pomocí šablony nových a aktualizovaných webů](#templates)
>     - [Ověřování uživatelského vstupu](#validation)
>     - [Povolení přihlášení z Facebooku a dalších lokalit pomocí OAuth a OpenID](#oauthsetup)
>     - [Přidání map použití pomocné rutiny mapy](#maphelper)
>     - [Spuštění webové stránky aplikace vedle sebe](#sidebyside)
>     - [Vykreslování stránek pro mobilní zařízení](#mobile)
> - [Další zdroje informací](#resources)
> 
> > [!NOTE]
> > Toto téma předpokládá, že používáte služby WebMatrix pro práci s vaším kódem ASP.NET Web Pages 2. Ale jako s 1 webové stránky, můžete také vytvořit webové stránky 2 webů pomocí sady Visual Studio, která vám umožňuje rozšířené možnosti technologie IntelliSense a ladění. Pro práci s webovými stránkami v sadě Visual Studio, je nutné nejprve nainstalovat Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 nebo Visual Studio 11 Beta. Nainstalujte ASP.NET MVC 4 Beta, který obsahuje šablony a nástroje pro vytváření aplikací ASP.NET MVC 4 a Web Pages 2 v sadě Visual Studio.
> 
> 
> *Poslední aktualizace: 18. června 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Instalace služby WebMatrix

Chcete-li nainstalovat webové stránky, můžete použít instalačního programu webové platformy, což je bezplatná aplikace, která umožňuje jednoduše nainstalovat a nakonfigurovat související webové technologie. Budete instalovat beta verzi 2 služby WebMatrix zahrnuje Beta 2 webové stránky.

1. Přejděte na instalační stránku pro nejnovější verzi instalačního programu webové platformy:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Pokud už máte nainstalovanou službu WebMatrix 1, tato instalace aktualizuje na beta verzi 2 služby WebMatrix. Můžete spustit weby, které byly vytvořeny pomocí verze 1 nebo 2 na stejném počítači. Další informace najdete v části na [spuštění webové stránky aplikace vedle sebe](#sidebyside).
2. Zvolte **nainstalovat**. 

    Pokud používáte Internet Explorer, přejděte k dalšímu kroku. Pokud používáte jiný prohlížeč, jako je Mozilla Firefox a Google Chrome, zobrazí se výzva k uložení *Webmatrix.exe* soubor do počítače. Uložte soubor a potom klikněte na ni a spustí se instalační služby.
3. Spusťte instalační program a zvolte **nainstalovat** tlačítko. Tím se nainstaluje služba WebMatrix a webové stránky.

## <a id="New_and_Enhanced_Features"></a>  Nové a vylepšené funkce

### <a id="Changes_for_the_RC_Version"></a>  Změny pro verzi RC (červen 2012)

Verze RC verze z června 2012 má několik změn od aktualizace Beta verze, která byla vydána v březnu 2012. Tyto změny jsou:

- A `Validation.AddFormError` metoda byla přidána do `Validation` pomocné rutiny. To je užitečné, pokud je ověření provést ručně (například můžete ověřit hodnotu, která je předána v řetězci dotazu) a chcete přidat chybovou zprávu, která lze zobrazit `Html.ValidationSummary` metody. Další informace najdete v části [ověření, že nebude pocházejí přímo z uživatelé dat](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) v [ověření vstupu uživatele ve webových stránek ASP.NET (Razor) lokality](https://go.microsoft.com/fwlink/?LinkId=253002).
- Funkce pro sdružování a minifikace byla odebrána ze sestavení jádra ASP.NET Web Pages 2. V důsledku toho `Assets` uvedené dále v tomto dokumentu není k dispozici. Místo toho je nutné nainstalovat [ASP.NET optimalizace](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) balíček NuGet. Další informace najdete v tématu [sdružování a Minifikace prostředků na webu rozhraní ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Přidali jsme další sestavení pro podporu technologie ASP.NET Web Pages 2. Pouze znatelný vliv této změny je, že může se zobrazit více sestavení na webu *bin* složky Po vytvoření webu nebo nasazení webu k poskytovateli hostingu.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Změny pro Beta verzi (únor 2012)

Vydáno v únoru 2012 Beta verzi má jen několik změn z verze beta verze, která byla vydána v roce 2011 dne. Tyto změny jsou:

- Razor teď podporuje podmíněné atributy. V kódu HTML odstraňuje prvek, pokud je atribut nastaven na hodnotu, která v serverovém kódu k `false` nebo `null`, technologie ASP.NET nevykresluje atribut vůbec. Představte si například, že máte následující kód pro zaškrtávací políčko:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Pokud hodnota `checked1` přeloží na `false` nebo `null`, `checked` atribut není vykresleno. Toto je zásadní změnu.
- `Validation.GetHtml` Metoda se přejmenovala na `Validation.For`. Toto je zásadní změnu; `Validation.GetHtml` nebudou fungovat ve verzi Beta.
- Nyní můžete zahrnout `~` operátor v kódu tak, aby odkazovaly kořenovém adresáři webu bez použití `Href` funkce. (To znamená, analyzátor Razor teď můžete vyhledat a vyřešit `~` operátor bez nutnosti volání do rozhraní explicitní metoda `Href`.) `Href` Metoda stále funguje, takže to není zásadní změnu.

    Například pokud jste dřív měli kód následujícím způsobem:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Teď můžete použít značky takto:

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts` Pomocné rutiny pro správu prostředků (prostředek) se nahradil údajem `Assets` pomocné rutiny, která má mírně odlišné metod, jako je následující:

  - Pro `Scripts.Add`, použijte `Assets.AddScript`
  - Pro `Scripts.GetScriptTags`, použijte `Assets.GetScripts`

    Toto je zásadní změnu; `Scripts` třída není k dispozici ve verzi Beta. Díky této změně se aktualizovaly příklady kódu v tomto dokumentu, které používají správu prostředků.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Pomocí šablony nových a aktualizovaných webů

**Starter Site** šablony byl aktualizován tak, aby běžel na Web Pages 2 ve výchozím nastavení. Také obsahuje následující nové funkce:

- Vykreslování části stránky mobilní zařízení. Pomocí stylů CSS a `@media` selektor, **Starter Site** poskytuje vylepšené vykreslování stránek na menších obrazovkách, včetně obrazovky mobilního zařízení.
- Vylepšené možnosti členství a ověřování. Pustit se přihlašují uživatelé na webu pomocí účtů z jiných sociálních sítí, jako je Twitter, Facebook a Windows Live. Další informace najdete v tématu [povolení přihlášení z Facebooku a ostatní weby pomocí OAuth a OpenID](#oauthsetup) oddílu.
- Prvky HTML5.

Nové **osobního webu** šablona umožňuje vytvořit web, který obsahuje osobní blog, stránka s fotografiemi a stránku Twitteru. Můžete přizpůsobit na základě lokality **osobního webu** šablony následujícím způsobem:

- Změna vzhledu stránky tak, že upravíte soubor rozložení (*\_SiteLayout.cshtml*) a soubor styly (*Site.css*).
- Instalace balíčků NuGet, které přidávají funkce do vaší lokality. Informace o tom, jak nainstalovat balíčky, včetně knihovnu ASP.NET Web Helpers, najdete v kurzu [instalace pomocné rutiny](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Pro přístup **osobního webu** šablony, zvolte **šablony** na služby WebMatrix **rychlý Start** obrazovky.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

V **šablony** dialogového okna zvolte **osobního webu** šablony.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Na úvodní stránku **osobního webu** stránky a stránka s fotografiemi šablona umožňuje-li nastavit blogu, použijte odkazy na Twitteru.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Ověřování uživatelského vstupu

V 1 webové stránky, pro ověření uživatelského vstupu v odeslané formuláře, použijete `System.Web.WebPages.Html.ModelState` třídy. (To je znázorněno v některé z ukázek kódu v tomto kurzu 1 webové stránky s názvem [práce s daty](../data/5-working-with-data.md).) V Web Pages 2 můžete nadále používat tento přístup. Web Pages 2 ale také nabízí vylepšené nástroje pro ověření vstupu uživatele:

- Nové třídy ověřování, včetně `System.Web.WebPages.ValidationHelper` a `System.Web.WebPages.Validator`, umožňující provádět výkonnou ověření pomocí pár řádků kódu.
- Volitelně ověřování na straně klienta, který poskytuje okamžitou zpětnou vazbu k uživateli nemusíte mít odezvy serveru zkontrolujte výskyt chyb ověření. (Z bezpečnostních důvodů, provede se ověření na serveru i v případě, že kontroly byly provedeny v klientovi předem.)

Pokud chcete použít nové funkce ověřování, postupujte takto:

V kódu stránky zaregistrovat element, který má být ověřen pomocí metody `Validation` pomocné rutiny: `Validation.RequireField`, `Validation.RequireFields` (k registraci více prvků jako povinné.), nebo `Validation.Add`. `Add` Metoda umožňuje určit jiné druhy ověřovací kontroly, jako je datový typ kontroly, položky v různých polí, kontroluje délku řetězce, porovnání a vzory (pomocí regulárních výrazů). Následuje několik příkladů:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Chcete-li zobrazit konkrétní pole chyb, zavolejte `Html.ValidationMessage` ve značkách pro každý prvek ověřován:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Chcete-li zobrazit souhrn (`<ul>` seznamu) všech chyb na stránce `Html.ValidationSummary` v kódu:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Tyto kroky jsou dost informací k implementaci ověřování na straně serveru. Pokud chcete přidat ověřování na straně klienta, kromě postupujte následovně.

Přidejte následující odkazy na soubor skriptu uvnitř `<head>` části webové stránky. První dva odkazy skriptu odkazovat na vzdálené soubory na serveru content delivery network (CDN). Třetí odkaz ukazovat na místní skript. Produkční aplikace by měla implementovat záložní CDN není k dispozici. Testování na náhradní řešení.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Nejjednodušší způsob, jak získat místní kopii *jquery.validate.unobtrusive.min.js* knihovna je pro vytvoření nové webové stránky webu na základě jedné z šablony webu (např. první web). Web vytvořený pomocí šablony obsahuje *jquery.validate.unobtrusive.js* soubor v jeho složky Scripts, ze kterého můžete zkopírovat ho do vaší lokality.

Pokud webová stránka používá<em>\_SiteLayout</em> stránky k řízení rozložení stránky, můžete zahrnout tyto odkazy na skript na této stránce tak, že ověřování je k dispozici pro všechny stránky obsahu. Pokud chcete provádět ověřování pouze na konkrétní stránky, můžete registrovat skripty na pouze na těchto stránkách správce prostředků. Chcete-li to provést, zavolejte `Assets.AddScript(path)` na stránce, kterou chcete ověřit a odkazovat na každém ze souborů skriptu. Pak přidejte volání do `Assets.GetScripts` v  <em>\_SiteLayout</em> stránky k vykreslení zaregistrovanou `<script>` značky. Další informace najdete v části [skripty registrace pomocí Správce prostředků](#resmanagement).

Ve značkách pro jednotlivý element volání `Validation.For` metody. Tato metoda generuje atributy tohoto jQuery lze připojit k ověřování na straně klienta. Příklad:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Následující příklad ukazuje na stránku, která ověřuje vstup uživatele ve formuláři. Chcete-li spustit a otestovat kód pro ověření, postupujte takto:

1. Vytvořit nový web pomocí jedné z šablony webu služby WebMatrix 2, které zahrnuje *skripty* složky, například **Starter Site** šablony.
2. V nové lokalitě, vytvořte nový *.cshtml* stránce a nahraďte jeho obsah na stránce s následujícím kódem.
3. Spuštění stránky v prohlížeči. Zadejte platné a neplatné values a posoudit dopad na ověření. Například povinné pole ponechte prázdné nebo zadejte písmeno v **kredity** pole.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Tady je stránka, když uživatel odešle platný vstup:

[![topSeven platný 1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Tady je stránka, když uživatel odešle s povinné pole prázdné:

[![topSeven platný 2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Tady je stránka, když uživatel odešle s jinou hodnotu než celé číslo v **kredity** pole:

[![topSeven platný 3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Další informace najdete v těchto příspěvcích na blogu:

- [Aktualizovat ověření ve v2 webové stránky](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) základní informace o přidání pomocí ověření `Validation` pomocné rutiny (pouze server strana)
- [Aktualizovat ověření ve verzi v2 webové stránky, část 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) přidání ověřování na straně klienta.
- [Aktualizovat ověření ve verzi v2 webové stránky, část 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) formátování chyby ověření.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Registrace skriptů pomocí Správce prostředků

Správce prostředků je nová funkce, která v serverovém kódu můžete použít k registraci a vykreslit klientských skriptů. Tato funkce je užitečná při práci s kódem z více souborů (jako je rozložení stránek, obsahu stránky, pomocné rutiny, atd.), které jsou sloučeny do jediné stránce v době běhu. Správce prostředků koordinuje zdrojových souborů, abyste měli jistotu, že soubory skriptů jsou odkazovány správně a efektivně na vykreslené stránce bez ohledu na to, soubory kódu, které se volají z nebo kolikrát se nazývají. Správce prostředků také vykresluje `<script>` značek na správném místě, tak, aby na stránce může načíst rychle (bez stažení skripty při vykreslování) a aby nedocházelo k chybám, které může dojít, pokud jsou volány skripty před vykreslením je dokončena.

Předpokládejme například, že vytvoření vlastního pomocného objektu, který volá soubor jazyka JavaScript a volat této pomocné rutiny na třech různých místech v kódu stránky obsahu. Pokud nepoužíváte správce prostředků k registraci skript volá v pomocné rutiny, třemi různými `<script>` značky, které odkazujících na stejný soubor skriptu se zobrazí v vykreslené stránky. Navíc, v závislosti na tom, kde `<script>` značky jsou vloženy na vykreslené stránce, chyb může nastat, pokud se skript pokusí o přístup k některé prvky stránky před stránka úplně načte. Pokud používáte Správce prostředků k registraci skript, vyhněte se těmto problémům.

Skript můžete zaregistrovat pomocí Správce prostředků tímto způsobem:

- V kódu, který musí odkazovat na skript, zavolejte `Assets.AddScript` metody.
- V  *\_SiteLayout* stránky, zavolejte `Assets.GetScripts` metoda k vykreslení `<script>` značky. 

    > [!NOTE]
    > Vložit volání `Assets.GetScripts` jako poslední položka v `<body>` elementu  *\_SiteLayout* stránky. To pomáhá stránky se načítají rychleji a může pomoci zabránit chybám skriptů.

Následující příklad ukazuje, jak funguje správce prostředků. Kód obsahuje následující položky:

- Vlastního pomocného objektu s názvem `MakeNote`. Tohoto pomocníka vykreslí řetězec v poli obalením `div` element kolem něj, který má ve stylu s ohraničením a přidáním &quot;Poznámka:&quot; k němu. Pomocná rutina volá také soubor jazyka JavaScript, která přidá chování za běhu na poznámku. Namísto odkazu na skript s `<script>` pomocné rutiny značky registruje skript voláním `Assets.AddScript` .
- Soubor jazyka JavaScript. Jedná se o soubor, který je volán pomocné rutiny a dočasně zvýší velikost písma položky Poznámka během `mouseover` událostí.
- Stránky obsahu, který odkazuje<em>\_SiteLayout</em> vykreslí část obsahu v těle stránky a pak zavolá `MakeNote` pomocné rutiny.
- A  *\_SiteLayout* stránky. Tato stránka obsahuje běžné záhlaví a strukturu rozložení stránky. Zahrnuje také volání `Assets.GetScripts`, což je, jak správce prostředků vykreslí skript volá na stránce.

Ke spuštění ukázky:

1. Vytvořte prázdný web Web Pages 2. Můžete použít službou WebMatrix **prázdný web** pro tuto šablonu.
2. Vytvořte složku s názvem *skripty* v lokalitě.
3. V *skripty* složce vytvořte soubor s názvem *Test.js*, kopírování *Test.js* obsah do něj z příkladu a uložte soubor...
4. Vytvořte složku s názvem *aplikace\_kód* v lokalitě.
5. V *aplikace\_kód* složce vytvořte soubor s názvem *Helpers.cshtml*, zkopírujte do ní ukázkový kód a uložte ho do složky s názvem *aplikace\_kód*v kořenové složce.
6. V kořenové složce v lokalitě, vytvořte soubor s názvem  *\_SiteLayout.cshtml,* příklad zkopírujte do něj a soubor uložte.
7. V kořenovém adresáři webu vytvořte soubor s názvem *ContentPage.cshtml*, přidejte ukázkový kód a uložte ho.
8. Spustit *ContentPage* v prohlížeči. Byl předán řetězec `MakeNote` pomocné rutiny se vykreslí jako zabalený poznámku.
9. Přesune ukazatel myši na poznámku. Skript dočasně zvýší velikost písma poznámky.
10. Zobrazte zdroj vykreslené stránky. Z důvodu, kterého jste umístili volání `Assets.GetScripts`, vygenerované `<script>` značku, která volá *Test.js* je poslední položka v těla stránky.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

Následující snímek obrazovky ukazuje *ContentPage.cshtml* v prohlížeči, když podržíte ukazatel myši nad poznámky:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Povolení přihlášení z Facebooku a dalších lokalit pomocí OAuth a OpenID

Web Pages 2 poskytuje rozšířené možnosti pro členství a ověřování. Hlavní vylepšení je, že jsou nové [OAuth](http://oauth.net/) a [OpenID](http://openid.net/) poskytovatelů. Pomocí těchto zprostředkovatelů, můžete nechat přihlášení uživatelů do vašeho webu pomocí existujících přihlašovacích údajů z Facebooku, Twitteru, Windows Live, Google a Yahoo. K přihlášení pomocí účtu sítě Facebook, například uživatele můžete prostě vybrat ikonu Facebooku, který přesměruje na přihlašovací stránku Facebooku, kde jsou informace o uživateli zadat. Přihlášení k Facebooku, pak můžete přiřadit ke svému účtu na webu. Související vylepšení funkcí členství webové stránky je, že uživatelé mohou přidružit více přihlašovací jména (včetně přihlašování ze sociálních sítí) pomocí jednoho účtu na vašem webu.

Tento obrázek ukazuje na přihlašovací stránku z **Starter Site** šablony, ve kterém může uživatel vybrat Facebooku, Twitteru nebo Windows Live ikonu povolíte protokolování s využitím externí účet:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Můžete povolit OAuth a OpenID členství pomocí pár řádků kódu. Metody a vlastnosti můžete použít pro práci s OAuth a OpenID zprostředkovatelé jsou v `WebMatrix.Security.OAuthWebSecurity` třídy.

Doporučený způsob, jak začít pracovat s noví zprostředkovatelé namísto použití kódu k povolení přihlášení z jiných webů, ale je použití nového **Starter Site** šablonu, která je součástí beta verzi 2 služby WebMatrix. **Starter Site** Šablona zahrnuje úplné členství infrastruktury, s přihlašovací stránky, databáze členství a veškerý kód, je potřeba nechat přihlášení uživatelů do vašeho webu pomocí pověření pro místní nebo z jiné lokality .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Povolení přihlášení pomocí OAuth a OpenID poskytovatele

Tato část poskytuje příklad toho, jak umožnit uživatelům přihlášení z externích webů (Facebook, Twitter, Windows Live, Google a Yahoo) k lokalitě, která je založena na **Starter Site** šablony. Po vytvoření první web, můžete postupujte (podrobnosti):

- Weby, které používají OAuth zprostředkovatele (Facebook, Twitter nebo Windows Live), vytvořit aplikaci na externí web. To vám dává klíče aplikace, které budete potřebovat k vyvolání funkce přihlášení pro tyto lokality. Pro servery, které využívají poskytovatele OpenID (Google, Yahoo) není nutné k vytvoření aplikace. Pro všechny tyto servery musí mít účet k přihlášení a k vytváření aplikací pro vývojáře. 

    > [!NOTE]
    > Aplikace Windows Live přijímat pouze za provozu adresu URL pro webovou stránku, funkční, tak adresy URL místního webu nelze použít pro testování přihlášení.
- Pokud chcete zadat poskytovatele příslušný ověřovací upravit několik souborů na vašem webu a odeslat přihlášení k webu, kterou chcete použít.

**Povolit Google a Yahoo přihlášení**:

1. Na vašem webu, upravit  *\_AppStart.cshtml* stránku a přidejte následující dva řádky kódu v bloku kódu Razor po volání `WebSecurity.InitializeDatabaseConnection` metody. Tento kód povoluje zprostředkovatele Google a Yahoo OpenID. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. V *~/Account/Login.cshtml* stránce, komentářů z následující `<fieldset>` blok kódu na konci stránky. Zrušte komentář kódu, odeberte `@*` znaky, které předcházet a postupujte podle pokynů `<fieldset>` bloku. Výsledný blok kódu vypadá takto:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Přidat `<input>` – element pro Google nebo Yahoo poskytovatele, který má `<fieldset>` skupiny *~/Account/Login.cshtml* stránky. Aktualizovaný `<fieldset>` skupiny s `<input>` prvky pro Google a Yahoo vypadá jako v následujícím příkladu: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. V *~/Account/AssociateServiceAccount.cshtml* stránce, přidejte `<input>` prvky pro Google nebo Yahoo k `<fieldset>` skupiny na konci souboru. Můžete zkopírovat stejné `<input>` prvky, které jste právě přidali `<fieldset>` tématu *~/Account/Login.cshtml* stránky. 

    *~/Account/AssociateServiceAccount.cshtml* stránky v šabloně Starter Site lze použít, pokud chcete vytvořit stránku, na které uživatelé mohou přidružit více přihlášení z jiných webů pomocí jednoho účtu na vašem webu.

Nyní můžete otestovat přihlašovací údaje Google a Yahoo.

1. Spustit *stránku default.cshtml* stránku vašeho webu a zvolte **přihlášení** tlačítko.
2. Na *přihlášení* stránku, **přihlášení pomocí jiné služby** zvolte buď **Google** nebo **Yahoo** tlačítko Odeslat. Tento příklad používá přihlášení Google. 

    Webové stránky přesměruje požadavek na přihlašovací stránku služby Google.

    [![topSeven. oauth 6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Zadejte přihlašovací údaje pro účet Google.
4. Pokud Google se zeptá, jestli chcete povolit Localhost, chcete-li použít informace z účtu, klikněte na tlačítko **povolit**.

    Kód používá Google token k ověření uživatele a vrátí na tuto stránku na vašem webu. Tato stránka umožňuje uživatelům přihlášení jejich Google přidružit účet na webu, nebo se můžete zaregistrovat nový účet na webu pro přidružení externí přihlášení s.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Zvolte **přidružit** tlačítko. Prohlížeč se vrátí na domovskou stránku vaší aplikace.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Povolit přihlášení Facebooku**:

1. Přejděte [webu vývojáře služby Facebook](https://developers.facebook.com/apps) (v případě se přihlaste se ještě nepřihlásili).
2. Zvolte **vytvořit novou aplikaci** tlačítko a pak postupujte podle výzev a pojmenujte a vytvořit novou aplikaci.
3. V části **vyberte, jak se vaše aplikace bude integrovat s Facebook**, zvolte **webu** oddílu.
4. Vyplňte **adresa URL webu** pole s adresou URL vašeho webu (například [ `http://www.example.com` ](http://www.example.com)). **Domény** pole je volitelné; může být využit k zajištění ověřování pro celou doménu (například *example.com*). 

    > [!NOTE]
    > Pokud používáte v místním počítači pomocí adresy URL webu, jako jsou `http://localhost:12345` (kde číslo je číslo místního portu), můžete přidat tuto hodnotu **adresa URL webu** pole pro testování webu. Však kdykoli číslo portu změny vaší místní sítě, budete muset aktualizovat **adresa URL webu** pole vaší aplikace.
5. Zvolte **uložit změny** tlačítko.
6. Zvolte **aplikace** kartu znovu a potom zobrazit úvodní stránku pro vaši aplikaci.
7. Kopírovat **ID aplikace** a **tajný kód aplikace** hodnoty pro vaši aplikaci a vložte je do dočasné textového souboru. Tyto hodnoty předá k poskytovateli služby Facebook v kódu vašeho webu.
8. Ukončete webu pro vývojáře služby Facebook.

Teď provedete změny dvě stránky na webu tak, aby uživatelé mohli přihlásit k webu pomocí svých účtů služby Facebook.

1. Na vašem webu upravit  *\_AppStart.cshtml* stránce a zrušte komentář kódu pro zprostředkovatele OAuth pro Facebook. Blok neokomentovaném textu kódu vypadá takto: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Kopírovat **ID aplikace** hodnotu z aplikace Facebook jako hodnotu `consumerKey` parametrů (v uvozovkách).
3. Kopírování **tajný kód aplikace** hodnotu z aplikace Facebook, jako `consumerSecret` hodnotu parametru.
4. Soubor uložte a zavřete.
5. Upravit *~/Account/Login.cshtml* stránce a komentářů z `<fieldset>` bloku na konci stránky. Zrušte komentář kódu, odeberte `@*` znaky, které předcházet a postupujte podle pokynů `<fieldset>` bloku. Blok kódu pomocí komentářů odebrány vypadá nějak takto: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Soubor uložte a zavřete.

Nyní můžete otestovat přihlášení k Facebooku.

1. Spuštění tohoto webu *stránku default.cshtml* stránky a zvolte **přihlášení** tlačítko.
2. Na *přihlášení* stránku, **přihlášení pomocí jiné služby** zvolte **Facebook** ikonu. 

    Webové stránky přesměruje požadavek na přihlašovací stránku Facebooku.

    [![topSeven. oauth 2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Přihlaste se ke Facebookový účet. 

    Kód používá token služby Facebook pro ověření a vrátí na stránku, kde můžete přidružit k přihlášení k Facebooku přihlašovací jméno vašeho webu. Uživatelského jména nebo e-mailové adresy se naplní do **e-mailu** pole ve formuláři.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Zvolte **přidružit** tlačítko. 

    Vrátí prohlížeči na domovskou stránku a jste přihlášení.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Pokud chcete povolit Twitteru přihlášení:** 

1. Přejděte [webu vývojáře Twitteru](https://dev.twitter.com/).
2. Zvolte **vytvořit aplikaci** odkaz a přihlaste se do lokality.
3. Na **vytvořit aplikaci** formuláře, vyplňte **název** a **popis** pole.
4. V **webu** pole, zadejte adresu URL vašeho webu (například [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Pokud testujete místně webu (pomocí adresy URL `http://localhost:12345`), Twitter, nemusí přijmout adresu URL. Nicméně je možné používat zpětnou smyčku místní IP adresu (například `http://127.0.0.1:12345`). To zjednodušuje proces testování vaší aplikace v místním prostředí. Ale pokaždé, když se mění číslo portu z místní lokality, bude nutné aktualizovat **webu** pole vaší aplikace.
5. V **adresu URL zpětného volání** pole, zadejte adresu URL pro stránku na vašem webu, kterou mají uživatelé k vrácení po přihlášení na Twitteru. Například pokud chcete odeslat uživatelů na domovskou stránku Starter Site (který rozpozná stav jejich přihlášení), zadejte stejnou adresu URL, kterou jste zadali v **webu** pole.
6. Přijměte podmínky, zvolte **vytvoření aplikace Twitter** tlačítko.
7. Na **Moje aplikace** úvodní stránka, zvolte aplikaci, kterou jste vytvořili.
8. Na **podrobnosti** , posuňte dolů a klikněte na příkaz **vytvořit My Access Token** tlačítko.
9. Na **podrobnosti** kartu, zkopírujte **uživatelský klíč** a **uživatelský tajný klíč** hodnoty pro vaši aplikaci a vložte je do dočasné textového souboru. Tyto hodnoty budete předat poskytovatele Twitteru ve vašem kódu webu.
10. Ukončení serveru Twitter.

Teď provedete změny dvě stránky na webu tak, aby uživatelé se nebudou moct přihlásit k webu pomocí svých účtů na Twitteru.

1. Na vašem webu upravit  *\_AppStart.cshtml* stránce a zrušte komentář kódu pro zprostředkovatele OAuth pro Twitter. Blok neokomentovaném textu kódu vypadá takto: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Kopírovat **uživatelský klíč** hodnota Twitter application jako hodnotu `consumerKey` parametrů (v uvozovkách).
3. Kopírovat **uživatelský tajný klíč** hodnota Twitter application jako hodnotu `consumerSecret` parametru.
4. Soubor uložte a zavřete.
5. Upravit *~/Account/Login.cshtml* stránce a komentářů z `<fieldset>` bloku na konci stránky. Zrušte komentář kódu, odeberte `@*` znaky, které předcházet a postupujte podle pokynů `<fieldset>` bloku. Blok kódu pomocí komentářů odebrány vypadá nějak takto: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Soubor uložte a zavřete.

Nyní můžete otestovat Twitteru přihlášení.

1. Spustit *stránku default.cshtml* stránku vašeho webu a zvolte **přihlášení** tlačítko.
2. Na *přihlášení* stránku, **přihlášení pomocí jiné služby** zvolte **Twitteru** ikonu. 

    Webové stránky přesměruje požadavek na stránku Twitteru přihlášení pro aplikaci, kterou jste vytvořili.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Přihlaste se ke účtu sítě Twitter.
4. Tento kód používá token služby Twitter. k ověření uživatele a pak se vrátíte na stránku ve kterém můžete přidružit vaše přihlášení pomocí svého účtu Web. Jména nebo e-mailové adresy se naplní do **e-mailu** pole ve formuláři.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Zvolte **přidružit** tlačítko. 

    Vrátí prohlížeči na domovskou stránku a jste přihlášení.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Přidání map použití pomocné rutiny mapy

Web Pages 2 obsahuje Tvorba knihovnu ASP.NET Web Helpers, což je balíček doplňků pro web webové stránky. Jeden z nich je součástí mapování poskytované `Microsoft.Web.Helpers.Maps` třídy. Můžete použít `Maps` k vygenerování mapy založené na adresu nebo na sadu souřadnice zeměpisné šířky a délky. `Maps` Třídy volají přímo do oblíbených mapování modulů včetně Bing, Google, Yahoo a MapQuest umožňuje.

Použití nového `Maps` třídy na vašem webu, musíte nejprve nainstalovat verzi 2 webové pomocné rutiny knihovny. Chcete-li to provést, přejděte na pokyny k instalaci na aktuálně vydanou verzi sady [knihovnu ASP.NET Web Helpers](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) a nainstalujte verzi 2.

Kroky pro přidání mapování do stránky jsou stejné bez ohledu na to, které moduly mapy volání. Stačí přidat odkaz na soubor jazyka JavaScript na stránku mapování a potom přidejte volání, který vykreslí `<script>` značky na stránce. Pak na stránce mapování, volání modulu mapu, kterou chcete použít.

Následující příklad ukazuje, jak vytvořit stránku, která vykreslí mapu, na základě adresy a jinou stránku, která vykreslí mapy podle souřadnic zeměpisné šířky a délky. Mapování adresy příkladu mapy Google a souřadnice mapování příkladu mapy Bing. Mějte na paměti následující prvky v kódu:

- Volání `Assets.AddScript` v horní části na dvou stránkách mapování. Tato metoda přidá odkaz na *jquery 1.6.2.min.js* soubor, který je součástí **Starter Site** šablony a, který vyžaduje `Maps` třídy.
- Volání `Assets.GetScripts` metody v souboru rozložení. Tato metoda vykreslí ještě `<script>` značku na dvou stránkách mapování.
- Volání `@Maps.GetGoogleHtml` a `@Maps.GetBingHtml` metody na stránkách mapování. Pokud chcete namapovat adresu, musíte předat řetězec adresy. Převádějí, musíte předat zeměpisné šířky a délky souřadnice. Pro modul služby mapy Bing, je třeba předat také klíče (který získat zdarma registrací v programu na [webu vývojáře mapy Bing](https://www.microsoft.com/maps/developers/web.aspx)). Metody pro ostatní moduly mapy fungují podobným způsobem (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Chcete-li vytvořit mapování stránky:

1. Vytvořte web na základě **Starter Site** šablony.
2. Vytvořte soubor s názvem *MapAddress.cshtml* v kořenové složce webu. Tato stránka bude generovat mapy založené na adresu, která mu předáte.
3. Zkopírujte následující kód do souboru, přepisování stávajícího obsahu. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Vytvořte soubor s názvem  *\_MapLayout.cshtml* v kořenové složce webu. Tato stránka bude na stránce rozložení pro dvě stránky mapování.
5. Zkopírujte následující kód do souboru, přepisování stávajícího obsahu. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Vytvořte soubor s názvem *MapCoordinates.cshtml* v kořenové složce webu. Tato stránka bude generovat mapy na základě sady souřadnic, které můžete předat.
7. Zkopírujte následující kód do souboru, přepisování stávajícího obsahu. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Pro testování stránek mapování:

1. Spuštění stránky *MapAddress.cshtml* souboru.
2. Zadejte úplnou adresu řetězec včetně ulici, stát nebo kraj a poštovní směrovací číslo a klikněte na tlačítko **mapy ho** tlačítko. Na stránce vykreslí mapování z mapy Google: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Najdete sadu zeměpisné šířky a délky pro konkrétní lokalitu.
4. Spuštění stránky *MapCoordinates.cshtml*. Zadejte souřadnice a potom klikněte **mapy ho** tlačítko. Na stránce vykreslí mapování z mapy Bing: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Spuštění webové stránky aplikace vedle sebe

Web Pages 2 přidává možnost spouštět aplikace vedle sebe. Tímto způsobem můžete i nadále spouštět vaše aplikace Web Pages 1, vytvářet nové aplikace Web Pages 2 a spustit všechny z nich na stejném počítači.

Zde jsou některé kroky, nezapomeňte při instalaci Beta 2 webových stránek pomocí služby WebMatrix:

- Ve výchozím nastavení bude existující aplikace Web Pages spouští jako aplikace verze 2 ve vašem počítači. (Sestavení pro verzi 2 jsou nainstalovány v GAC a použije automaticky.)
- Pokud budete chtít spuštění webu pomocí verzi aplikace Web Pages 1 (místo výchozího, stejně jako v předchozím bodě), můžete nakonfigurovat lokality tak, aby to udělat. Pokud váš web už nemá *web.config* souboru v kořenovém adresáři serveru, vytvořte novou a zkopírujte následující kód XML do něj, přepisování stávajícího obsahu. Pokud web již obsahuje *web.config* přidejte `<appSettings>` prvky jako následující ten, který má `<configuration>` oddílu.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  . – Pokud nezadáte verzi v *web.config* soubor, lokalitu je nasazen jako webový server verze 2. (Budou sestavení verze 2 se zkopírují do *bin* složky v nasazené lokality.)
- Nové aplikace, kterou vytvoříte pomocí šablony webu v beta verzi 2 zahrnují budou sestavení verze 2 webové stránky na webu verzi Web Matrix *bin* složky.

Obecně platí, můžete vždy určit, kterou verzi webových stránek pomocí vašeho webu pomocí NuGet nainstalovat odpovídající sestavení na web *bin* složky. Balíčky, najdete v tématu [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Vykreslování stránek pro mobilní zařízení

Web Pages 2 umožňuje vytvářet vlastní zobrazení pro vykreslení obsahu na mobilní telefon nebo jiné zařízení.

`System.Web.WebPages` Obor názvů obsahuje následující třídy, které umožňují pracovat s režimy zobrazení: `DefaultDisplayMode`, `DisplayInfo`, a `DisplayModes`. Můžete přímo použít tyto třídy a napsat kód, který vykreslí správné výstup pro konkrétní zařízení.

Alternativně můžete vytvořit stránek specifických pro zařízení s použitím vzoru pro pojmenovávání souborů následujícím způsobem: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Například můžete vytvořit dvě verze stránky, jednu s názvem <em>MyFile.cshtml</em> a jeden s názvem <em>MyFile.Mobile.cshtml</em>. V době, když mobilní zařízení požádá o spuštění <em>MyFile.cshtml</em>, Web Pages vykreslí obsah z <em>MyFile.Mobile.cshtml</em>. V opačném případě <em>MyFile.cshtml</em> vykreslením.

Následující příklad ukazuje, jak povolit mobilní vykreslování přidáním stránku obsahu pro mobilní zařízení. *Page1.cshtml* obsahuje obsah a bočním panelu navigace. *Page1.Mobile.cshtml* obsahuje stejný obsah, ale vynechá na bočním panelu.

Sestavení a spuštění vzorového kódu:

1. V lokalitě webové stránky, vytvořte soubor s názvem *Page1.cshtml* a zkopírujte *Page1.cshtml* obsahu do něj jako v příkladu.
2. Vytvořte soubor s názvem *Page1.Mobile.cshtml* a zkopírujte *Page1.Mobile.cshtml* obsahu do něj jako v příkladu. Všimněte si, že mobilní verzi stránce vynechá části navigace pro lepší vykreslování na obrazovce menší.
3. Spustit prohlížeč pro stolní počítač a přejděte do *Page1.cshtml*.
4. Spustit mobilní prohlížeče (nebo emulátoru mobilního zařízení) a přejděte do *Page1.cshtml*. Všimněte si, že, který vykreslí tentokrát webové stránky mobilní verze stránky. 

    > [!NOTE]
    > K otestování mobilních stránek, můžete použít simulátor mobilní zařízení, na kterém běží na stolním počítači. Tento nástroj umožňuje testování webových stránek, jak by vypadala na mobilních zařízeních (to znamená, obvykle s mnohem menší umožňuje zobrazit oblast). Jedním z příkladů simulátoru je [doplňků uživatelského agenta přepínání](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) pro Mozilla Firefox, která vám umožňuje emulovat různá mobilní prohlížeče v desktopové verzi Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* vykreslen v desktopovém prohlížeči:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* zobrazí v zobrazení pro iPhone simulátor Apple v prohlížeči Firefox. I když je požadavek *Page1.cshtml*, vykreslení aplikace *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Další prostředky

### <a name="aspnet-web-pages-1-resources"></a>Webové stránky ASP.NET 1 prostředky

> [!NOTE]
> Většina programování webových stránkách 1 a prostředkům API stále platí pro Web Pages 2.

- [Úvod do programování webových stránek ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Prostředky služby WebMatrix

- [Služba WebMatrix 2 co je nového](http://webmatrix.com/next)
- [Microsoft Web služby WebMatrix](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Vývoj pro Web počínaje Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(obsahuje komplexně ukázkovou aplikaci webové stránky)
