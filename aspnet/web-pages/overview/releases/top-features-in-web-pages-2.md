---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Horní funkce v rozhraní ASP.NET Web Pages 2 | Microsoft Docs
author: microsoft
description: Toto téma obsahuje přehled hlavní nové funkce v ASP.NET Web Pages 2, jednoduché webové programování framework, která je součástí WebMatr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: f0d32edd3ab54c55aa06c803cd91e01cbbb8f08a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899379"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Hlavní funkce v rozhraní ASP.NET Web Pages 2
====================
podle [Microsoft](https://github.com/microsoft)

> Tento článek obsahuje přehled hlavní nové funkce v 2 RC webových stránek ASP.NET, jednoduché webové programování framework, která je součástí [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **Zahrnuté součásti:** 
> 
> - [Instalace služby WebMatrix](#install)
> - [Nové a vylepšené funkce](#New_and_Enhanced_Features)
> 
>     - [Změny ve verzi RC](#Changes_for_the_RC_Version)
>     - [Změny ve verzi Beta](#Changes_for_the_Beta_Version)
>     - [Pomocí šablony nové a aktualizované webů](#templates)
>     - [Ověřování uživatelského vstupu](#validation)
>     - [Povolení přihlášení ze sítě Facebook a jiných lokalitách pomocí OAuth a OpenID](#oauthsetup)
>     - [Přidání mapy využitím pomocné rutiny mapy](#maphelper)
>     - [Spouštění webových stránek aplikací vedle sebe](#sidebyside)
>     - [Vykreslování stránek pro mobilní zařízení](#mobile)
> - [Další zdroje informací](#resources)
> 
> > [!NOTE]
> > Toto téma předpokládá, že používáte službu WebMatrix pro práci s kódu ASP.NET Web Pages 2. Však jako s 1 webové stránky, mohou také vytvářet weby 2 webových stránek pomocí sady Visual Studio, který vám dává rozšířené funkce IntelliSense a ladění. Pro práci s webovými stránkami v sadě Visual Studio, musíte nejprve nainstalovat Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1 nebo Visual Studio 11 Beta. Nainstalujte technologie ASP.NET MVC 4 Beta, která obsahuje šablony a nástroje pro vytváření aplikací ASP.NET MVC 4 a Web Pages 2 v sadě Visual Studio.
> 
> 
> *Poslední aktualizace: 18. června 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Instalace služby WebMatrix

K instalaci webové stránky, můžete použít Microsoft webové platformy, které je bezplatná aplikace, která lze snadno nainstalovat a nakonfigurovat související webové technologie. Budete instalovat beta verzi 2 WebMatrix zahrnuje Beta 2 webové stránky.

1. Přejděte na stránku instalace na nejnovější verzi instalačního programu webové platformy:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Pokud již máte nainstalovanou službu WebMatrix 1, tato instalace se aktualizuje na beta verzi služby WebMatrix 2. Můžete spustit weby, které byly vytvořené pomocí verze 1 nebo 2 na stejném počítači. Další informace najdete v části na [spuštění webové stránky aplikace vedle sebe](#sidebyside).
2. Zvolte **nyní nainstalovat**. 

    Pokud používáte Internet Explorer, přejděte k dalšímu kroku. Pokud používáte jiný prohlížeč jako Mozilla Firefox nebo Google Chrome, zobrazí se výzva k uložení *Webmatrix.exe* soubor do počítače. Uložte soubor a klikněte na něj spustíte instalační program.
3. Spusťte instalační program a vybrat **nainstalovat** tlačítko. Tím se nainstaluje službu WebMatrix a webové stránky.

## <a id="New_and_Enhanced_Features"></a>  Nové a vylepšené funkce

### <a id="Changes_for_the_RC_Version"></a>  Změny pro verzi RC (červen 2012)

Verze RC verze v červen 2012 obsahuje několik změny z aktualizace Beta verze, která byla vydána v březnu 2012. Tyto změny jsou:

- A `Validation.AddFormError` metoda byla přidána do `Validation` pomocné rutiny. To je užitečné, pokud je ověření provést ručně (například můžete ověřit hodnotu, která je předán v řetězci dotazu) a chcete přidat chybovou zprávu, která lze zobrazit `Html.ValidationSummary` metoda. Další informace najdete v části [ověření dat, nebude pocházet přímo z uživatelů](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) v [ověření vstupu uživatele na webech rozhraní ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253002).
- Odebrali jsme funkci pro sdružování a minimalizace ze základních ASP.NET Web Pages 2 sestavení. V důsledku toho `Assets` pomocné rutiny, které jsou uvedeny dále v tomto dokumentu není k dispozici. Místo toho je nutné nainstalovat [ASP.NET optimalizace](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) balíček NuGet. Další informace najdete v tématu [sdružování a Minifikace prostředky v stránku ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Byly přidány další sestavení pro ASP.NET Web Pages 2 podporu. Pouze znatelný vliv této změny je, že, může se zobrazit další sestavení na webu *bin* složky Po vytvoření lokality nebo lokality nasazení do hostujícího zprostředkovatele.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Změny pro Beta verzi (únor 2012)

Beta verze vydané v únoru 2012 má pouze několik změn z verze Beta, která byla vydána v prosince 2011. Tyto změny jsou:

- Syntaxe Razor teď podporuje podmíněné atributy. V kódu HTML elementu, pokud je atribut nastaven na hodnotu, v kód serveru, který přeloží `false` nebo `null`, technologie ASP.NET nevykresluje atribut vůbec. Představte si například, že máte následující kód pro zaškrtávací políčko:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Pokud hodnota `checked1` přeloží na `false` nebo `null`, `checked` atribut není vykreslen. Toto je narušující změně.
- `Validation.GetHtml` Metoda byla přejmenována na `Validation.For`. Toto je narušující změně; `Validation.GetHtml` ve verzi beta verzi nebude fungovat.
- Teď můžete použít `~` operátor v kódu odkazovat kořenovému adresáři webu bez použití `Href` funkce. (To znamená, můžete najít a vyřešit analyzátor Razor nyní `~` operátor bez nutnosti volání explicitní metoda `Href`.) `Href` Metoda pořád funguje, takže tento není narušující změně.

    Například pokud jste dřív měli kód takto:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Teď můžete použít kód takto:

    `<a href="~/Default.cshtml">Home</a>`
- `Scripts` Pomocné rutiny pro správu prostředků (prostředků) se nahradil údajem `Assets` pomocné rutiny, která má mírně odlišné metody, jako jsou následující:

  - Pro `Scripts.Add`, použijte `Assets.AddScript`
  - Pro `Scripts.GetScriptTags`, použijte `Assets.GetScripts`

    Toto je narušující změně; `Scripts` třída není k dispozici ve verzi Beta. Příklady kódu v tomto dokumentu, které používají správu asset byly aktualizovány s tuto změnu.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Pomocí šablony nové a aktualizované webů

**Starter Site** šablony je aktualizovaná tak, aby běžel na Web Pages 2 ve výchozím nastavení. Obsahuje taky následující nové funkce:

- Vykreslování stránky mobilní zařízení. Pomocí stylů CSS a `@media` selektoru **Starter Site** poskytuje lepší vykreslování stránek na menších obrazovkách, včetně mobilních zařízení obrazovky.
- Vylepšené možnosti členství a ověřování. Můžete je nechat protokolu uživatele na váš web pomocí účtů z jiných sociálních sítí, jako je například Twitteru, Facebooku a Windows Live. Další informace najdete v tématu [povolení přihlášení ze sítě Facebook a další lokality pomocí OAuth a OpenID](#oauthsetup) části.
- HTML5 elements.

Nové **osobní stránku** šablona umožňuje vytvořit web, který obsahuje osobní blog, stránka s fotografiemi a na stránce služby Twitter. Web na základě můžete přizpůsobit **osobní stránku** šablony pomocí těchto kroků:

- Změna vzhledu stránky úpravou souboru rozložení (*\_SiteLayout.cshtml*) a soubor styly (*Site.css*).
- Instalace balíčků NuGet, které přidávají funkce do vaší lokality. Informace o tom, jak nainstalovat balíčky, včetně knihovnu ASP.NET Web Helpers zobrazit kurz [instalaci pomocné rutiny](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Pro přístup k **osobní stránku** šablony, vyberte **šablony** na WebMatrix **rychlý Start** obrazovky.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

V **šablony** dialogovém okně vyberte **osobní stránku** šablony.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Cílová stránka **osobní stránku** šablona umožňuje odkazech na blogu, Twitter, stránky a stránky fotografie.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Ověřování uživatelského vstupu

V 1 webové stránky, k ověření vstupu uživatele na odeslaného formuláře, použijete `System.Web.WebPages.Html.ModelState` třídy. (To je znázorněno v některé z ukázky kódu v tomto kurzu 1 webové stránky s názvem [práci s daty](../data/5-working-with-data.md).) Stále můžete tento přístup na Web Pages 2. Web Pages 2 však nabízí vylepšené nástroje pro ověřování vstupu uživatele:

- Nové třídy ověření, včetně `System.Web.WebPages.ValidationHelper` a `System.Web.WebPages.Validator`, které umožňují provádět efektivní ověření úkoly zadání několika řádků kódu.
- Volitelně ověřování na straně klienta, který poskytuje okamžitou zpětnou vazbu pro uživatele místo nutnosti kontaktování serveru ke kontrole chyb při ověřování. (Z bezpečnostních důvodů, ověření se provádí na serveru i v případě, že kontroly prováděly v klientovi předem.)

Pokud chcete používat nové funkce ověřování, postupujte takto:

V kódu stránky zaregistrovat element má být ověřen pomocí metody `Validation` Pomocník: `Validation.RequireField`, `Validation.RequireFields` (k registraci více elementů potřeba), nebo `Validation.Add`. `Add` Metoda slouží k určení dalších typů ověřovací kontroly, jako je datový typ kontroly, porovnání položek v různých polí, délka řetězce kontroly a vzory (s použitím regulárních výrazů). Následuje několik příkladů:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Chcete-li zobrazit chyba specifické pro pole, volejte `Html.ValidationMessage` v kódu pro každý prvek ověřován:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Chcete-li zobrazit souhrn (`<ul>` seznamu) všech chyb na stránce `Html.ValidationSummary` do kódu:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Tyto kroky jsou dost implementovat ověřování na straně serveru. Pokud chcete přidat ověřování na straně klienta, kromě toho proveďte následující kroky.

Přidejte následující odkazy na soubor skriptu uvnitř `<head>` část webové stránky. První dva odkazům na skript, přejděte na vzdálených souborů na serveru doručování obsahu (CDN) sítě. Odkaz na třetí odkazuje na soubor skriptu místní. Když není k dispozici CDN produkční aplikace by měla implementovat zálohu. Test záložní.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

Nejjednodušší způsob, jak získat místní kopii *jquery.validate.unobtrusive.min.js* knihovna je vytvoření nové webové stránky lokality na základě jedné z šablony webů (například Starter Site). Zahrnuje webu vytvořených šablonou *jquery.validate.unobtrusive.js* souboru ve složce jeho skripty, ze kterého můžete zkopírovat ho na server.

Pokud váš web používá<em>\_SiteLayout</em> stránky k řízení rozložení stránky, můžete zahrnout tyto odkazy skript v této stránce tak, aby ověření je k dispozici pro všechny stránky obsahu. Pokud chcete provést ověření pouze na konkrétní stránky, můžete pro registraci skriptů na pouze stránky, správce prostředků. Chcete-li to provést, volejte `Assets.AddScript(path)` na stránce, který chcete ověřit a odkazovat na každý ze souborů skriptu. Pak přidejte volání `Assets.GetScripts` v  <em>\_SiteLayout</em> stránky k vykreslení zaregistrovanou `<script>` značky. Další informace najdete v části [skripty registrace pomocí Správce prostředků](#resmanagement).

Volání do kódu pro jednotlivý prvek `Validation.For` metoda. Tato metoda vysílá atributy této jQuery můžete připojit k ověřování na straně klienta. Příklad:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

Následující příklad ukazuje na stránku, která ověřuje uživatelský vstup ve formuláři. Chcete-li spustit a otestovat kód pro ověření, postupujte takto:

1. Vytvořit nový web pomocí jedné z šablony webu služby WebMatrix 2, které zahrnuje *skripty* složky, například **Starter Site** šablony.
2. V nové lokalitě, vytvořte novou *.cshtml* stránky a obsah na stránku nahraďte následujícím kódem.
3. Spusťte stránku v prohlížeči. Zadejte platné a neplatné hodnoty zobrazíte důsledky pro ověření. Například povinné pole ponechte prázdné nebo zadejte písmeno v **kredity** pole.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Zde je stránka, když uživatel odešle platný vstup:

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Když uživatel odešle s povinné pole prázdné, zde je stránka:

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Zde je stránka, když uživatel odešle s něco jiného než celé číslo **kredity** pole:

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Další informace najdete v následujících příspěvcích na blogu:

- [Aktualizovat ověření u webové stránky v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) základní informace o přidání ověření pomocí `Validation` pomocné rutiny (pouze server straně)
- [Aktualizovat ověření u webové stránky v2, část 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) přidání ověřování na straně klienta.
- [Aktualizovat ověření u webové stránky v2, část 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) formátování chyby ověření.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Registrace pomocí Správce prostředků skriptů

Správce prostředků je nová funkce, která můžete použít k registraci a vykreslit klienta skripty v serverovém kódu. Tato funkce je užitečná při práci s kódem z více souborů (například rozložení stránky, stránky obsahu, pomocné rutiny atd.), které jsou sloučeny do jediné stránce za běhu. Správce prostředků koordinuje a přesvědčte se, že soubory skriptu se správně odkazují a efektivně na vykreslené stránce, bez ohledu na to, které soubory kódu jsou volány z nebo kolikrát se nazývají zdrojové soubory. Správce prostředků také vykreslí `<script>` značky na správném místě, aby stránce může načíst rychle (aniž byste museli stáhnout skripty při vykreslování) a abyste se vyhnuli chybám, které může dojít, pokud jsou skripty volána před provedením vykreslování je dokončena.

Předpokládejme například, že vytvoříte vlastní pomocné rutiny, která volá soubor JavaScript a volat tohoto pomocníka ve třech různých místech v kódu stránky obsahu. Pokud nepoužijete správce prostředků k registraci volání skriptu do pomocné rutiny, tři různé `<script>` značky, které vykreslené stránce se zobrazí všechny bod do stejného souboru skriptu. A, v závislosti na tom, kde `<script>` značky jsou vloženy na vykreslené stránce, chyb, může dojít, pokud se skript pokusí o přístup k určité prvky na stránce před plně načtení stránky. Pokud použijete Správce prostředků k registraci skript, vyhněte se těmto problémům.

Skript můžete zaregistrovat pomocí Správce prostředků tímto způsobem:

- V kódu, který musí odkazovat na skript, zavolejte `Assets.AddScript` metoda.
- V  *\_SiteLayout* stránky, volejte `Assets.GetScripts` metoda k vykreslení `<script>` značky. 

    > [!NOTE]
    > Vkládat volání `Assets.GetScripts` jako velmi poslední položky `<body>` element  *\_SiteLayout* stránky. To pomáhá stránce načíst rychlejší a může pomoct vyhnout se chybám skriptu.

Následující příklad ukazuje, jak funguje správce prostředků. Kód obsahuje následující položky:

- Vlastního pomocného objektu s názvem `MakeNote`. Tato pomocná vykreslí řetězec v poli nástrojem pro zabalení `div` element kolem něj, naformátovat ohraničením a přidáním &quot;Poznámka:&quot; k němu. Pomocné rutiny také voláním soubor JavaScript, který přidá běhového chování pro poznámku. Místo reference skriptu pomocí `<script>` značky, pomocné rutiny zaregistruje skript voláním `Assets.AddScript` .
- Soubor JavaScript. Toto je soubor, který volá pomocné rutiny a dočasně zvětšuje velikost písma položek Poznámka během `mouseover` událostí.
- Stránky obsahu, která odkazuje<em>\_SiteLayout</em> stránku, vykreslí obsah v textu a pak zavolá `MakeNote` pomocné rutiny.
- A  *\_SiteLayout* stránky. Tato stránka obsahuje hlavičku běžné a strukturou rozložení stránky. Zahrnuje také volání `Assets.GetScripts`, což je, jak správce prostředků vykreslí skript volá na stránce.

Spustit ukázku:

1. Vytvoření webu k prázdný Web Pages 2. Můžete použít WebMatrix **prázdný web** pro tuto šablonu.
2. Vytvořte složku s názvem *skripty* v lokalitě.
3. V *skripty* složky, vytvořte soubor s názvem *Test.js*, kopie *Test.js* obsahu do ní z příkladu a uložte soubor...
4. Vytvořte složku s názvem *aplikace\_kód* v lokalitě.
5. V *aplikace\_kód* složky, vytvořte soubor s názvem *Helpers.cshtml*, zkopírujte do ní ukázkový kód a uložte je do složky s názvem *aplikace\_kódu*v kořenové složce.
6. V kořenové složce webu, vytvořte soubor s názvem  *\_SiteLayout.cshtml,* příklad zkopírujte do něj a soubor uložte.
7. V kořenovém adresáři webu, vytvořte soubor s názvem *ContentPage.cshtml*, přidejte ukázkový kód a uložte ho.
8. Spustit *ContentPage* v prohlížeči. Byl předán řetězec `MakeNote` pomocné rutiny, které je vykresleno jako zabalené Poznámka.
9. Přesune ukazatel myši nad Poznámka. Skript dočasně zvětšuje velikost písma poznámky.
10. Zobrazte zdroj na vykreslené stránce. Z důvodu, kam jste umístili volání `Assets.GetScripts`, vygenerované `<script>` značku, která volá *Test.js* je velmi poslední položky v těle stránky.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

Následující snímek obrazovky ukazuje *ContentPage.cshtml* v prohlížeči, když podržíte ukazatel myši nad Poznámka:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Povolení přihlášení ze sítě Facebook a jiných lokalitách pomocí OAuth a OpenID

Web Pages 2 poskytuje rozšířené možnosti pro členství a ověřování. Hlavní vylepšení je, že jsou nové [OAuth](http://oauth.net/) a [OpenID](http://openid.net/) zprostředkovatele. Pomocí těchto poskytovatelů, můžete je nechat protokolu uživatele na váš web pomocí svých existujících přihlašovacích údajů ze sítě Facebook, Twitter, Windows Live, Google a Yahoo. K přihlášení pomocí účtu sítě Facebook, například uživatelé mohou zvolit právě Facebook ikonu, která je přesměruje na přihlašovací stránku služby Facebook, kde zadat informace o uživateli. Přihlášení k síti Facebook se pak můžete přidružit ke svému účtu na váš web. Související vylepšení funkcí členství webové stránky je, že uživatelé mohou přidružit více přihlášení (včetně přihlášení ze sociálních sítí) s jeden účet na vašem webu.

Tento obrázek zobrazuje stránku pro přihlášení z **Starter Site** šablony, které může uživatel Facebook, Twitter nebo Windows Live ikonu povolíte protokolování s externím účtu:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Můžete povolit OAuth a OpenID členství pomocí několika řádků kódu. Metody a vlastnosti, použijete pro práci s OAuth a OpenID poskytovatelé jsou v `WebMatrix.Security.OAuthWebSecurity` třídy.

Doporučený způsob, jak začít pracovat s nové zprostředkovatele místo použití kódu povolit přihlášení z jiných webů, ale je použití nového **Starter Site** šablony, která je součástí služby WebMatrix 2 Beta. **Starter Site** Šablona zahrnuje úplné členství infrastruktury, s přihlašovací stránky, databáze členství a všechny kód, je třeba dát protokolu uživatele na váš web pomocí přihlašovacích údajů místního nebo ty z jiné lokality .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Postup povolení přihlášení pomocí OAuth a OpenID zprostředkovatelé

Tato část představuje příklad, jak umožnit uživatelům přihlášení z externí servery (Facebook, Twitter, Windows Live, Google a Yahoo) k lokalitě, která je založena na **Starter Site** šablony. Po vytvoření úvodní lokality, proveďte tento (podrobnosti použijte):

- Weby, které používají OAuth vytvoření zprostředkovatele (Facebook, Twitter a Windows Live), aplikace na webu externího. To vám dává klíče aplikace, které budete potřebovat k vyvolání funkce přihlášení pro tyto lokality. Weby, které používají poskytovatele OpenID (Google, Yahoo) není nutné k vytvoření aplikace. Pro všechny tyto servery musí mít účet k přihlášení a vytvořit vývojář aplikace. 

    > [!NOTE]
    > Aplikace Windows Live přijímají pouze za provozu adresu URL pro funkčního webu, tak pro testování přihlášení nemůžete použít adresu URL místního webu.
- Chcete-li zadat poskytovatele příslušný ověřovací upravit několik souborů ve vašem webu a odeslat přihlášení k webu, kterou chcete použít.

**Chcete-li povolit přihlášení Google a Yahoo**:

1. Ve vašem webu upravit  *\_AppStart.cshtml* stránky a přidejte následující dva řádky kódu v bloku kódu Razor po volání `WebSecurity.InitializeDatabaseConnection` metoda. Tento kód umožňuje poskytovatelům Google a Yahoo OpenID. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. V *~/Account/Login.cshtml* stránky, odeberte komentáře z následující `<fieldset>` blok kódu téměř na konci stránky. Chcete-li zrušte komentář kódu, odeberte `@*` znaky, které předcházet a postupujte podle pokynů `<fieldset>` bloku. Výsledný blok kódu vypadá takto:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Přidat `<input>` element pro Google nebo Yahoo zprostředkovatele, který má `<fieldset>` v *~/Account/Login.cshtml* stránky. Aktualizovaný `<fieldset>` skupiny s `<input>` prvky pro Google a Yahoo vypadá jako v následujícím příkladu: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. V *~/Account/AssociateServiceAccount.cshtml* přidejte `<input>` prvky pro Google nebo Yahoo k `<fieldset>` skupiny u konce souboru. Můžete zkopírovat stejné `<input>` prvky, které jste právě přidali `<fieldset>` v tématu *~/Account/Login.cshtml* stránky. 

    *~/Account/AssociateServiceAccount.cshtml* stránky v šabloně Starter Site lze použít, pokud chcete vytvořit stránku, na kterém uživatelé mohou přidružit více přihlášení z ostatních lokalit k jeden účet na vašem webu.

Nyní můžete otestovat Google a Yahoo přihlášení.

1. Spustit *default.cshtml* stránku vašeho webu a zvolte **přihlásit** tlačítko.
2. Na *přihlášení* stránky v **přihlásit pomocí jiné služby** vyberte buď **Google** nebo **Yahoo** tlačítko Odeslat. Tento příklad používá přihlášení Google. 

    Webová stránka přesměruje požadavek na přihlašovací stránku Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Zadejte přihlašovací údaje pro existující účet Google.
4. Pokud Google dotazem, zda chcete povolit Localhost použít informace z účtu, klikněte na tlačítko **povolit**.

    Kód používá Google token k ověření uživatele a vrátí na tuto stránku na vašem webu. Tato stránka umožňuje uživatelům přidružení jejich Google přihlášení s existujícím účtem na vašem webu, nebo se můžete zaregistrovat nový účet na svém webu přidružit externí přihlášení s.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Vyberte **přidružit** tlačítko. V prohlížeči vrátí na domovskou stránku vaší aplikace.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Chcete-li povolit přihlášení Facebook**:

1. Přejděte na [Facebook vývojáři lokality](https://developers.facebook.com/apps) (protokolu případě, že jste již přihlášeni).
2. Vyberte **vytvořit novou aplikaci** tlačítko a pak postupujte podle pokynů k pojmenování a vytvořit novou aplikaci.
3. V části **vyberte, jak bude vaše aplikace integrovat Facebook**, vyberte **webu** části.
4. Vyplňte **adresa URL webu** pole s adresou URL vašeho webu (například [ `http://www.example.com` ](http://www.example.com)). **Domény** pole je volitelné; můžete použít k ověření pro celou doménu (například *example.com*). 

    > [!NOTE]
    > Pokud používáte lokalitu v místním počítači s adresou URL jako `http://localhost:12345` (kde číslo je číslem místního portu), přidáním této hodnoty **adresa URL webu** pole pro testování vaší lokality. Ale kdykoli se číslo portu změny místní lokality, budete muset aktualizovat **adresa URL webu** pole vaší aplikace.
5. Vyberte **uložit změny** tlačítko.
6. Vyberte **aplikace** znovu a poté zobrazte úvodní stránky pro vaši aplikaci.
7. Kopírování **ID aplikace** a **tajný klíč aplikace** hodnoty pro vaši aplikaci a vložte je do dočasného textového souboru. Tyto hodnoty budou předat poskytovatele sítě Facebook v kódu webové stránky.
8. Ukončení webu pro vývojáře Facebook.

Teď provedete změny dvě stránky ve vašem webu tak, aby uživatelé budou moci přihlásit do lokality pomocí účtů služby Facebook.

1. Ve vašem webu upravit  *\_AppStart.cshtml* stránky a zrušte komentář kódu pro zprostředkovatele OAuth pro Facebook. Blok uncommented kódu vypadá takto: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Kopírování **ID aplikace** hodnotu z aplikace Facebook jako hodnotu `consumerKey` parametr (v uvozovkách).
3. Kopírování **tajný klíč aplikace** hodnotu z aplikace Facebook, jako `consumerSecret` hodnota parametru.
4. Soubor uložte a zavřete.
5. Upravit *~/Account/Login.cshtml* stránky a odebere komentáře z `<fieldset>` bloku téměř na konci stránky. Chcete-li zrušte komentář kódu, odeberte `@*` znaky, které předcházet a postupujte podle pokynů `<fieldset>` bloku. Blok kódu s komentáři odebrán vypadá takto: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Soubor uložte a zavřete.

Nyní můžete otestovat přihlášení k síti Facebook.

1. Spuštění tohoto webu *default.cshtml* stránce a vyberte položku **přihlášení** tlačítko.
2. Na *přihlášení* stránky v **přihlásit pomocí jiné služby** zvolte **Facebook** ikonu. 

    Webová stránka přesměruje požadavek na přihlašovací stránku služby Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Přihlaste se k účtu sítě Facebook. 

    Kód k vašemu ověření používá token služby Facebook a vrátí na stránky, kde je možné přidružit Facebook přihlašovací jméno vašeho webu přihlášení. Uživatelského jména nebo e-mailové adresy se naplní do **e-mailu** pole ve formuláři.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Vyberte **přidružit** tlačítko. 

    Vrátí prohlížeči na domovskou stránku a jste přihlášeni.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Povolení služby Twitter přihlášení:** 

1. Vyhledejte [Twitter vývojáři lokality](https://dev.twitter.com/).
2. Vyberte **vytvořit aplikaci** propojit a přihlaste se k webu.
3. Na **vytvořit aplikaci** formuláři, zadejte **název** a **popis** pole.
4. V **webu** pole, zadejte adresu URL vašeho webu (například [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Pokud testujete váš web místně (pomocí adresy URL jako `http://localhost:12345`), služby Twitter nemusí přijmout adresu URL. Nicméně je možné použít místní smyčky IP adresu (například `http://127.0.0.1:12345`). Tato funkce zjednodušuje proces testování aplikace s místně. Však pokaždé, když číslo portu místní lokality změní, budete muset aktualizovat **webu** pole vaší aplikace.
5. V **adresu URL zpětné volání** pole, zadejte adresu URL stránky ve vašem webu, kterou mají uživatelé se vraťte do po přihlášení do služby Twitter. Například uživatelé odeslat na domovskou stránku Starter Site (který rozpozná stav jejich přihlášení), zadejte stejnou adresu URL, kterou jste zadali v **webu** pole.
6. Přijměte podmínky a vyberte **vytvořit aplikaci služby Twitter** tlačítko.
7. Na **Moje aplikace** úvodní stránka, vyberte aplikaci, kterou jste vytvořili.
8. Na **podrobnosti** , posuňte se dolů a klikněte na příkaz **vytvořit Moje přístup Token** tlačítko.
9. Na **podrobnosti** kartě, zkopírujte **uživatelský klíč** a **uživatelský tajný klíč** hodnoty pro vaši aplikaci a vložte je do dočasného textového souboru. Tyto hodnoty budete předáte v kódu webové stránky k poskytovateli služby Twitter.
10. Ukončení webu služby Twitter.

Nyní můžete měnit dvě stránky ve vašem webu tak, aby uživatelé budou moci přihlásit do lokality pomocí účtů služby Twitter.

1. Ve vašem webu upravit  *\_AppStart.cshtml* stránky a zrušte komentář kódu pro zprostředkovatele služby Twitter OAuth. Blok uncommented kódu vypadá takto: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Kopírování **uživatelský klíč** hodnotu z aplikace služby Twitter jako hodnotu `consumerKey` parametr (v uvozovkách).
3. Kopírování **uživatelský tajný klíč** hodnotu z aplikace služby Twitter jako hodnotu `consumerSecret` parametr.
4. Soubor uložte a zavřete.
5. Upravit *~/Account/Login.cshtml* stránky a odebere komentáře z `<fieldset>` bloku téměř na konci stránky. Chcete-li zrušte komentář kódu, odeberte `@*` znaky, které předcházet a postupujte podle pokynů `<fieldset>` bloku. Blok kódu s komentáři odebrán vypadá takto: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Soubor uložte a zavřete.

Nyní můžete otestovat přihlášení služby Twitter.

1. Spustit *default.cshtml* stránku vašeho webu a zvolte **přihlášení** tlačítko.
2. Na *přihlášení* stránky v **přihlásit pomocí jiné služby** zvolte **Twitter** ikonu. 

    Webová stránka přesměruje požadavek na přihlašovací stránku služby Twitter pro aplikaci, kterou jste vytvořili.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Přihlaste se k účtu sítě Twitter.
4. Kód používá token služby Twitter. k ověření uživatele a vrátí můžete na stránky, kde můžete přidružit vaše přihlášení pomocí účtu webu. Jméno nebo e-mailovou adresu se naplní do **e-mailu** pole ve formuláři.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Vyberte **přidružit** tlačítko. 

    Vrátí prohlížeči na domovskou stránku a jste přihlášeni.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Přidání mapy využitím pomocné rutiny mapy

Web Pages 2 obsahuje Tvorba knihovnu ASP.NET Web Helpers, což je balíček doplňků pro web webové stránky. Jedním z nich je komponenta mapování poskytované `Microsoft.Web.Helpers.Maps` třídy. Můžete použít `Maps` k vygenerování založené na adresu nebo na sadu zeměpisné šířky a délky souřadnice mapy. `Maps` Třída umožňuje volají přímo do oblíbených mapy moduly včetně Bing, Google, MapQuest a Yahoo.

Použití nového `Maps` třídy ve vašem webu, je nutné nejprve nainstalovat verzi 2 webové pomocné knihovny. Chcete-li to provést, přejděte na pokyny k instalaci aktuálně prodejní verze nástroje [knihovnu ASP.NET Web Helpers](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) a nainstalujte verzi 2.

Postup přidání mapování na stránku jsou stejné bez ohledu na to, které z modulů mapy volání. Stačí přidat odkaz na soubor JavaScript na stránku mapování a potom přidejte volání, který vykreslí `<script>` značky na stránku. Pak na stránku mapování volání, které chcete použít modul mapy.

Následující příklad ukazuje postup vytvoření stránky, který se vykreslí mapu založené na adresu a jinou stránku, který vykreslí mapu podle souřadnice zeměpisné šířky a délky. Příklad mapování adres používá službu mapy Google a příklad souřadnic mapování používá Bing Maps. Vezměte na vědomí následující prvky v kódu:

- Volání `Assets.AddScript` v horní části se dvě stránky. Tato metoda přidá odkaz na *jquery 1.6.2.min.js* soubor, který je součástí **Starter Site** šablony a který je vyžadován na základě `Maps` třídy.
- Volání `Assets.GetScripts` metoda v souboru rozložení. Tato metoda vykreslí `<script>` značky na dvě stránky mapování.
- Volání `@Maps.GetGoogleHtml` a `@Maps.GetBingHtml` metody na stránkách mapování. Chcete-li mapování adresy, musíte zadat řetězec adresy. Pokud chcete mapovat souřadnice, je nutné předat zeměpisné šířky a délky souřadnice. Pro modul mapy Bing, je třeba předat také klíče (což získat zadarmo registrací v [Bing Maps vývojáři lokality](https://www.microsoft.com/maps/developers/web.aspx)). Metody pro jiné moduly mapy fungovat podobným způsobem (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Chcete-li vytvořit mapování stránky:

1. Vytvoření webu na základě **Starter Site** šablony.
2. Vytvořte soubor s názvem *MapAddress.cshtml* v kořenovém adresáři serveru. Tato stránka bude Generovat mapu založené na adresu, která můžete předat.
3. Zkopírujte následující kód do souboru, přepsání existujícího obsahu. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Vytvořte soubor s názvem  *\_MapLayout.cshtml* v kořenovém adresáři serveru. Tato stránka bude ke stránce rozložení pro dvě stránky mapování.
5. Zkopírujte následující kód do souboru, přepsání existujícího obsahu. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Vytvořte soubor s názvem *MapCoordinates.cshtml* v kořenovém adresáři serveru. Tato stránka bude Generovat mapu na základě sady souřadnic, které můžete předat.
7. Zkopírujte následující kód do souboru, přepsání existujícího obsahu. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Pro testování stránek mapování:

1. Spuštění stránky *MapAddress.cshtml* souboru.
2. Zadejte úplnou adresu řetězec včetně adresu, státu nebo oblasti a PSČ a potom vyberte **mapy ho** tlačítko. Stránka vykreslí mapu z Google Maps: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Najít sadu zeměpisné šířky a délky pro konkrétního umístění.
4. Spuštění stránky *MapCoordinates.cshtml*. Zadejte souřadnice a pak vyberte **mapy ho** tlačítko. Stránka vykreslí mapu z mapy Bing: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Spouštění webových stránek aplikací vedle sebe

Web Pages 2 navíc umožňuje spouštění aplikací vedle sebe. Díky tomu můžete i nadále spouštět aplikace Web Pages 1, vytvářet nové aplikace Web Pages 2 a spustit všechny z nich na stejném počítači.

Zde jsou některé věci na mějte na paměti, když instalujete na používání beta verze 2 webových stránek pomocí služby WebMatrix:

- Ve výchozím nastavení bude existující webové stránky aplikace v počítači spustit jako verze 2 aplikace. (Sestavení pro verze 2 jsou nainstalovány v mezipaměti GAC a použije se automaticky.)
- Pokud chcete spustit a lokalitě používající verzi aplikace Web Pages 1 (namísto výchozí, jako v předchozím bodu), můžete nakonfigurovat k tomuto webu. Pokud váš web již nemá *web.config* souboru v kořenovém adresáři serveru, vytvořte novou a zkopírujte následující kód XML do ní, přepsání existujícího obsahu. Pokud lokalita již obsahuje *web.config* soubor, přidejte `<appSettings>` element stejný, jako je následující k `<configuration>` oddílu.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  ' – Pokud nezadáte verzi v *web.config* soubor, lokalitu je nasadit jako webový server verze 2. (Verze 2 sestavení se zkopírují do *bin* složku v nasazené lokalitě.)
- Nové aplikace, kterou vytvoříte pomocí šablony webů v Web Matrix verzi 2 Beta zahrnout sestavení verze 2 webové stránky na webu *bin* složky.

Obecně platí, můžete vždy řídit kterou verzi webové stránky pro použití s vaší lokality pomocí NuGet pro instalaci příslušné sestavení do lokality *bin* složky. Balíčky naleznete [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Vykreslování stránek pro mobilní zařízení

Web Pages 2 umožňuje vytvářet vlastní zobrazení pro vykreslování obsah na mobilní telefon nebo jiné zařízení.

`System.Web.WebPages` Obor názvů obsahuje následující třídy, které umožňují pracovat s režimy zobrazení: `DefaultDisplayMode`, `DisplayInfo`, a `DisplayModes`. Můžete používat tyto třídy přímo a napsat kód, který vykreslí správné výstup pro konkrétní zařízení.

Alternativně můžete vytvořit stránek specifických pro zařízení s použitím vzor pojmenovávání souborů takto: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Například můžete vytvořit dvě verze stránky, jednu s názvem <em>MyFile.cshtml</em> a jednu s názvem <em>MyFile.Mobile.cshtml</em>. V době, kdy požadavky na mobilní zařízení spuštění <em>MyFile.cshtml</em>, webové stránky vykreslí obsah z <em>MyFile.Mobile.cshtml</em>. V opačném <em>MyFile.cshtml</em> je vykreslen.

Následující příklad ukazuje, jak můžete povolit mobilní vykreslování přidáním stránky obsahu pro mobilní zařízení. *Page1.cshtml* obsahuje obsah a bočním panelu navigace. *Page1.Mobile.cshtml* obsahuje stejný obsah, ale vynechá na bočním panelu.

Sestavení a spuštění ukázkového kódu:

1. V lokalitě webové stránky, vytvořte soubor s názvem *Page1.cshtml* a zkopírujte *Page1.cshtml* obsahu do ní z příkladu.
2. Vytvořte soubor s názvem *Page1.Mobile.cshtml* a zkopírujte *Page1.Mobile.cshtml* obsahu do ní z příkladu. Všimněte si, že mobilní verzi stránce vynechá části navigace pro lepší vykreslování na menší obrazovce.
3. Spustit prohlížeč pro stolní počítač a přejděte do *Page1.cshtml*.
4. Spustit prohlížeč pro mobilní zařízení (nebo emulátoru mobilního zařízení) a přejděte do *Page1.cshtml*. Všimněte si, která vykreslí tentokrát webové stránky mobilní verzi stránky. 

    > [!NOTE]
    > K testování mobilních stránek, můžete použít simulátoru mobilních zařízení, která běží na stolním počítači. Tento nástroj umožňuje testovací webové stránky, jako by vypadal na mobilních zařízeních (to znamená, obvykle s mnohem menšími umožňuje zobrazit oblast). Jedním z příkladů simulátoru je [uživatele agenta přepínači rozšíření](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) pro aplikaci Mozilla Firefox, která vám umožňuje emulovat různé mobilní prohlížeče z plochy verzi Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* v prohlížeč pro stolní počítač:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* zobrazí v zobrazení Apple iPhone simulátoru v prohlížeči Firefox. I když požadavek je pro *Page1.cshtml*, vykreslí aplikace *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Další prostředky

### <a name="aspnet-web-pages-1-resources"></a>ASP.NET Web Pages 1 prostředky

> [!NOTE]
> Většina programování webových stránek 1 a rozhraní API prostředky platit i 2 webové stránky.

- [Úvod do rozhraní ASP.NET Web Pages programování](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Služba WebMatrix prostředky

- [Služba WebMatrix 2 co je nového](http://webmatrix.com/next)
- [Web Microsoft WebMatrix](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Vývoj webů od verze Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(obsahuje i délku ukázkovou aplikaci webové stránky)
