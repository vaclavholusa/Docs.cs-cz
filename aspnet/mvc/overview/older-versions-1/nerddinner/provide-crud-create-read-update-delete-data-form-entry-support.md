---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Zadejte CRUD (vytvořit, číst, aktualizovat, odstraňovat) Data tvoří položka podporu | Microsoft Docs
author: microsoft
description: Krok 5 ukazuje, jak povolit podporu pro úpravy, vytváření a odstraňování večeří s ním také vám umožní Naše třída DinnersController Další.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: bd906282db5c620476966ffbe09cecb5ade66ee4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876745"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Zadejte CRUD (vytvořit, číst, aktualizovat, odstraňovat) Data tvoří položka podpory
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 5 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 5 ukazuje, jak povolit podporu pro úpravy, vytváření a odstraňování večeří s ním také vám umožní Naše třída DinnersController Další.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner krok 5: Vytvoření, aktualizace, odstranění scénáře formuláře

Jsme zavedená kontrolery a zobrazení a zahrnutých jejich použití k implementaci výpis/podrobnosti prostředí pro večeří v lokalitě. Naše dalším krokem bude provádět další Naše třída DinnersController a povolení podpory pro úpravy, vytváření a odstraňování večeří s ním také.

### <a name="urls-handled-by-dinnerscontroller"></a>Adresy URL zpracovávaných DinnersController

Metody akce jsme dříve přidány do DinnersController, která implementována podporu dvou adres URL: */Dinners* a */Dinners/podrobnosti / [id]*.

| **URL** | **VERB** | **Účel** |
| --- | --- | --- |
| */Dinners/* | GET | Zobrazí seznam nadcházející večeří HTML. |
| */Dinners/podrobnosti / [id]* | GET | Zobrazit podrobnosti o konkrétní večeři. |

Teď přidáme akce metody k implementaci tři další adresy URL: <em>/Dinners/Edit / [id], nebo večeří nebo vytvořit,</em>a<em>/Dinners/odstranění / [id]</em>. Tyto adresy URL se povoluje podporu pro úpravu stávající večeří, vytvoření nové večeří nebo odstranění večeří.

Podporujeme HTTP GET a POST protokolu HTTP příkaz interakce s těmito novými adresami URL. Požadavky HTTP GET pro tyto adresy URL se zobrazí počáteční zobrazení HTML dat (formuláře naplněný daty večeři v případě "upravit", prázdného formuláře v případě "vytvořit" a obrazovka s potvrzením odstranit v případě "odstranit"). Požadavky HTTP POST na tyto adresy URL se uložit, aktualizace nebo odstranění večeři data v našem DinnerRepository (a z ní k databázi).

| **URL** | **VERB** | **Účel** |
| --- | --- | --- |
| */Dinners/edit / [id]* | GET | Zobrazte upravitelné naplněný daty večeři formuláře HTML. |
| POST | Uložte změny formuláře pro konkrétní večeři do databáze. |
| */ Večeří nebo vytvořit* | GET | Zobrazí prázdné formuláře HTML, který umožňuje uživatelům definovat nové večeří. |
| POST | Vytvořte nový večeři a uložte jej v databázi. |
| */Dinners/odstranění / [id]* | GET | Zobrazení odstranit potvrzovací obrazovce. |
| POST | Odstraní zadaný večeři z databáze. |

### <a name="edit-support"></a>Upravit podpory

Začněme tím, že implementujete scénář "upravit".

#### <a name="the-http-get-edit-action-method"></a>Metoda HTTP GET upravit akce

Začneme implementací chování HTTP "GET" naše metody akce úpravy. Tato metoda bude volána, když */Dinners/Edit / [id]* je vyžadována adresa URL. Naše implementace bude vypadat jako:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Výše uvedený kód používá k načtení objektu večeři DinnerRepository. Pak vykreslí zobrazení šablony pomocí večeři objektu. Protože jsme nebyly předána explicitně název šablony, čímž *View()* Pomocná metoda, se bude používat výchozí cestu založené na konvenci pro vyřešit zobrazit šablonu: /Views/Dinners/Edit.aspx.

Nyní vytvoříme tuto šablonu zobrazení. Uděláme to tak, že kliknete pravým tlačítkem v rámci metody upravit a výběr příkaz "Přidat zobrazení" kontextové nabídky:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

V dialogovém okně "Přidat zobrazení" jsme budete znamenat, že jsme předali objekt večeři do našich zobrazit šablonu jako svůj model a rozhodnete automaticky vygenerované uživatelské rozhraní k šabloně "Upravit":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Když kliknete na tlačítko "Přidat", Visual Studio přidat nový soubor šablony "Edit.aspx" zobrazení pro nás v adresáři "\Views\Dinners". Otevře se také si novou šablonu zobrazení "Edit.aspx" v rámci editoru kódu – naplněný počáteční "Upravit" vygenerované uživatelské rozhraní implementace jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Umožňuje provést několik změny výchozí "upravit" vygenerované uživatelské rozhraní generované a aktualizovat šablonu zobrazení upravit tak, aby měl obsah níže (který odebere několik vlastností, které jsme si chcete vystavit):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Jsme spuštění aplikace a požadavek *"/ večeří/Edit/1"* URL vidíte na následující stránce:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Značka jazyka HTML generované naše zobrazení vypadá níže. Je standardní HTML – s &lt;formuláře&gt; element, který provádí HTTP POST do */Dinners/Edit/1* adresy URL při "Uložit" &lt;typ vstupu = "Odeslat" /&gt; je tlačítko Poslat. HTML &lt;typ vstupu = "text" /&gt; element byla výstupu pro každou upravovat vlastnost:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Html.BeginForm() a Html.TextBox() metody pomocné rutiny Html

Používá několik metod "Pomocné rutiny Html" naše "Edit.aspx" Zobrazit šablonu: Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() a Html.ValidationMessage(). Kromě generování kód HTML pro nás, zadejte tyto pomocné metody zpracování předdefinované chyb a ověření podporovat.

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() Pomocná metoda

Pomocná metoda Html.BeginForm() je výstup HTML &lt;formuláře&gt; element v našem značek. V našem Edit.aspx zobrazit šablonu můžete si všimnout, že se má použít C# "pomocí" příkazu při použití této metody. Otevřete složené závorky označuje začátek &lt;formuláře&gt; obsah a složené závorky uzavírací je co označuje konec &lt;/form&gt; element:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Případně pokud se vám najít prohlášení "použití" přístupu nepřirozené pro scénář takto, můžete použít kombinaci Html.BeginForm() a Html.EndForm() (který provede stejnou akci):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Volání metody Html.BeginForm() bez parametrů způsobí, že k vypsání element formuláře, který nemá HTTP POST na adresu URL aktuální žádosti. To znamená, proč generuje naše zobrazení *&lt;akci formuláře = "/ večeří/Edit/1" metoda = "post"&gt;* element. Jsme může být případně předaný explicitní parametry Html.BeginForm() jsme chtěli post na jinou adresu URL.

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() Pomocná metoda

Naše Edit.aspx zobrazení používá metodu helper Html.TextBox() výstup &lt;typ vstupu = "text" /&gt; prvky:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Výše uvedené Html.TextBox() metoda přebírá jediný parametr – sloužící k určení atributy id nebo název služby &lt;typ vstupu = "text" /&gt; element výstup, stejně jako vlastnost modelu k naplnění hodnotu pole textbox z. Například bylo na večeři objekt jsme předaná do zobrazení upravit hodnotu vlastnosti "Title" služby ".NET Futures", a proto volání metody naše Html.TextBox("Title") výstup: *&lt;vstupní id = "Title" name = "Title" type = "text" value = ".NET Futures" /&gt;*.

První parametr Html.TextBox() jsme Alternativně můžete použít pro zadejte id nebo název elementu a poté explicitně předat hodnoty, které chcete použít jako druhý parametr:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Často jsme budete chtít provádět vlastní formátování na hodnotu, která je výstup. Statickou metodu zabudovaná .NET String.Format() je užitečné pro tyto scénáře. Naše Edit.aspx zobrazit šablonu je pomocí této k formátování EventDate hodnotu (která je typu datum a čas), tak, aby nezobrazí sekund po dobu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Třetí parametr, který se Html.TextBox() volitelně slouží k vypsání další atributy HTML. Následující fragment kódu ukazuje, jak k vykreslení další velikost = "30" atribut a třídy = "mycssclass" atribut na &lt;typ vstupu = "text" /&gt; elementu. Všimněte si, jak jsme jsou uvozovací znaky název třídy pomocí atributu "@" character because "třída" je vyhrazené klíčové slovo v jazyce C#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementace metody akce HTTP POST úpravy

Nyní je k dispozici verze HTTP GET naše metoda akce úpravy implementována. Když uživatel požádá o */Dinners/Edit/1* URL dostanou stránku HTML takto:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Kliknutím na tlačítko "Uložit" způsobí, že post formuláře na */Dinners/Edit/1* adresu URL, a odešle HTML &lt;vstupní&gt; tvoří hodnoty pomocí příkazu HTTP POST. Teď umožňuje implementovat chování HTTP POST metody akce naše upravit – které bude zpracovávat ukládání večeře.

Začneme budete přidáním přetížené metody akce "Upravit" do našich DinnersController, který má atribut "AcceptVerbs" na označuje, že zpracovává HTTP POST scénáře:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Při použití metody přetížené akce atribut [AcceptVerbs], ASP.NET MVC automaticky zpracovává odesílající požadavky na vhodnou metodu v závislosti na příchozí příkaz HTTP. Požadavky HTTP POST do <em>/Dinners/Edit / [id]</em> adresy URL přejde na výše uvedené metoda úprav, při všech ostatních požadavků HTTP příkaz k <em>/Dinners/Edit / [id]</em>adresy URL přejde na první metoda úprav jsme (které nebyla implementována nemá atribut [AcceptVerbs]).

| **Téma straně: Proč rozlišit pomocí příkazů HTTP?** |
| --- |
| Se může požádat – proč jsme se pomocí jedné adresy URL a rozlišení své chování pomocí příkazu HTTP? Proč ne máte jenom dva samostatné adresy URL pro zpracování načítání a ukládají se změny upravit? Příklad: /Dinners/Edit / [id] zobrazíte počáteční formulář a /Dinners/Save / [id] pro zpracování post formuláře ji uložit? Nevýhodou s dvě samostatné adresy URL pro publikování je, že v případech, kdy jsme odeslání na /Dinners/Save/2 a pak budete muset znovu zobrazit formulář HTML z důvodu chyby vstupu, koncový uživatel skončí tím, že v panelu Adresa svého prohlížeče (vzhledem k tomu, které se mají adresu URL večeří/Save/2 Adresa URL formuláře publikované). Pokud koncový uživatel záložky tuto redisplayed stránku do seznamu oblíbených položek jejich prohlížeče, nebo kopírování/vloží adresu URL a e-maily přítele, se budou skončili ukládání adresu URL, která nebude fungovat v budoucnu (vzhledem k tomu, že adresa URL závisí na hodnoty post). Díky zpřístupnění jednu adresu URL (například: /Dinners/Edit/[id]) a rozlišení zpracování ho pomocí příkazu HTTP, je bezpečné pro přístup koncových uživatelů k bookmark stránce Upravit nebo odeslání ostatním uživatelům adresu URL. |

#### <a name="retrieving-form-post-values"></a>Načítání hodnot Post formuláře

Existují různé způsoby, které jsme získat přístup odeslány parametrů formuláře v rámci naší metodu HTTP POST "Upravit". Jeden jednoduchý přístup je právě pomocí vlastnost požadavku na základní třídy Controller přístup ke kolekci formuláře a načíst přímo odeslaných hodnot:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Výše uvedený přístup je trochu podrobné nastavení, ale hlavně po přidáme zpracování logiky chyb.

A lépe přístupu pro tento scénář je možné využít integrované *UpdateModel()* metodu helper na základní třídy Kontroleru. Podporuje aktualizace vlastností objektu, který jsme předat pomocí příchozí parametrů formuláře. Reflexe používá k určení názvů vlastností v objektu a poté automaticky převede a přiřazuje hodnoty na základě vstupních hodnot odeslaných klientem.

Pomocí této metody UpdateModel() jsme může zjednodušit naše HTTP POST upravit akci pomocí tohoto kódu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Můžete nyní navštěvují */Dinners/Edit/1* adresy URL a změňte název naše večeři:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Když kliknete na tlačítko "Uložit", jsme naše upravit akce provedete post formuláře a aktualizovanými hodnotami bude natrvalo v databázi. Jsme pak přesměrováni na adresu URL podrobnosti večeře (který se zobrazí nově uložené hodnoty):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Zpracování chyb úpravy

Naše aktuální implementace funguje HTTP POST přesně – s výjimkou případů, kdy se vyskytly chyby.

Když uživatel provede chyba úpravy formuláře, potřebujeme zajistit, že formulář se zobrazí znovu s informativní chybovou zprávu, která je to opravit provede. To zahrnuje případy, kdy koncovým odešle nesprávné vstup (například: řetězec chybná data), jak a také případech, kdy je platný formát vstupu, ale je narušení obchodní pravidlo. Pokud dochází k chybám, že formulář musí zachovat vstupních dat zadané uživatelem původně tak, aby nemají k doplnění jejich změny ručně. Tento proces musí tolikrát, kolikrát podle potřeby opakujte, dokud formulář úspěšně dokončí.

ASP.NET MVC zahrnuje některé dobrý integrované funkce, které usnadnění zpracování chyb a redisplay formuláře. Umožňuje zobrazit tyto funkce v akci aktualizace našich metoda akce upravit následujícím kódem:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Ve výše uvedeném kódu je podobná naše předchozí implementace – s tím rozdílem, že jsme jsou blok try/catch – zpracování chyb nyní zabalení kolem našich pracovních. Pokud dojde k výjimce při volání metody UpdateModel() nebo pokud jsme zkuste a uložte DinnerRepository (která bude vyvolána výjimka, pokud večeři objekt, který se s vámi snažíme pro uložení je neplatný z důvodu narušení pravidla v rámci našeho modelu), naše blok catch chyba zpracování bude Spusťte. V něm jsme žádné porušení pravidel, které existují v objektu večeři opakovací smyčku a přidat je do objekt ModelState (který probereme krátce). Jsme znovu zobrazíte zobrazení.

Zobrazení tohoto, práce, umožňuje aplikaci znovu spustit, upravit večeři a změňte jej tak mít prázdný název, EventDate "BOGUS" a používat telefonní číslo Spojené království s hodnotou země USA. Pokud jsme klikněte na tlačítko "Uložit" naše metoda HTTP POST upravit nebudou moci ukládat večeře (protože nejsou k dispozici chyby) a bude třeba znovu zobrazit formulář:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Naše aplikace má dostatečnou chyby prostředí. Neplatný vstup elementy textu se zobrazí červeně a chybových zpráv ověření jsou zobrazeny pro koncového uživatele o nich. Formulář je také zachování uživatel původně zadaná – vstupní data tak, aby nemají k doplnění nic.

Jak se může požádat, tato situace nastat? Jak textových polí název, EventDate a ContactPhone zvýrazněte sami červeně a vědět o výstup hodnoty původně zadané uživatele? A jak chybové zprávy nezobrazí se v seznamu nahoře? Dobrá zpráva je, že to nebylo provádí magic – místo byl vzhledem k tomu, že jsme některé integrované funkce ASP.NET MVC, které zajistit, aby ověření vstupu a Chyba zpracování scénáře snadno použít.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Principy ModelState a metody pomocné rutiny HTML, ověření

Řadič třídy mají vlastnost kolekce "ModelState", který poskytuje způsob, jak znamenat, že existují chyby s objektem modelu předávány do zobrazení. Chyba položky v kolekci ModelState určit název vlastnosti modelu s problém (například: "Title", "EventDate" nebo "ContactPhone") a povolit lidské friendly chybová zpráva zadat (například: "Je vyžadován název").

*UpdateModel()* Pomocná metoda automaticky naplní kolekci ModelState při výskytu chyby při pokusu o přiřazení hodnoty formuláře na vlastnosti v objektu modelu. Například vlastnost EventDate naše večeři objekt je typu datum a čas. Pokud metoda UpdateModel() nelze přiřadit hodnotu řetězce "BOGUS" v tomto scénáři výše, metoda UpdateModel() přidat položku do kolekce ModelState indikující chybu přiřazení měl došlo u této vlastnosti.

Vývojáři mohou také napsat kód pro explicitně přidat chyba položky do kolekce ModelState jako jsme to děláte níže v rámci naše "" Chyba zpracování blok catch, které je naplnění položek založené na active porušení pravidel v kolekci ModelState Večeři objektu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Pomocné rutiny HTML integrace s ModelState

Metody helper HTML - jako Html.TextBox() - zkontrolujte kolekci ModelState při vykreslování výstup. Pokud existuje chybu pro položku, vykreslení uživatelem zadaná hodnota a chyba třídu CSS.

Například v našem "Upravit" zobrazení používáme pomocnou metodu Html.TextBox() k vykreslení EventDate naše večeři objektu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Při zobrazení byl vykreslen v případě chyby, zkontrolovat metodu Html.TextBox() kolekci ModelState chcete zobrazit, pokud byly všechny chyby související s vlastnost "EventDate" naše večeři objektu. Pokud bylo zjištěno, že došlo k chybě vykresluje odeslaná uživatelský vstup ("BOGUS") jako hodnotu a přidat třídu css chyba k &lt;typ vstupu = "textbox" /&gt; značek vygeneroval:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Můžete přizpůsobit vzhled chyba třídy css, ale chcete hledat. Výchozí třídy chyba šablon stylů CSS – "input-validation-error" – je definována v *\content\site.css* šablony stylů a vypadá jako níže:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Toto pravidlo šablon stylů CSS je co způsobilo naše Neplatný vstupní elementů mít zvýrazněná jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() Pomocná metoda

Pomocná metoda Html.ValidationMessage() slouží k výstupu spojené s konkrétním modelu vlastnost ModelState chybová zpráva:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Ve výše uvedeném kódu výstupy:  *&lt;span – třída = "pole validation-error"&gt; hodnota 'BOGUS' je neplatná &lt; /span&gt;*

Pomocná metoda Html.ValidationMessage() také podporuje druhý parametr, který umožňuje vývojářům přepsat chybová zpráva text, který se zobrazí:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Ve výše uvedeném kódu výstupy:  <em>&lt;span – třída = "pole validation-error"&gt;\*&lt;/span&gt;</em>místo text výchozí chyby, pokud je k dispozici pro chybu Vlastnost EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() Pomocná metoda

Pomocná metoda Html.ValidationSummary() můžete použít k vykreslení souhrnu chybových zpráv, doplněny &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; seznam všech podrobné chybové zprávy v ModelState kolekce:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Pomocná metoda Html.ValidationSummary() přebírá parametr volitelný řetězec – definující Souhrnné chybovou zprávu zobrazíte výše seznam podrobné chyby:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Volitelně můžete šablon stylů CSS přepsat, jak vypadá v seznamu chyb.

#### <a name="using-a-addruleviolations-helper-method"></a>Pomocí AddRuleViolations Pomocná metoda

Naše počáteční HTTP POST upravit implementace používá foreach – příkaz jeho zachytávacího bloku smyčku objekt večeři porušení pravidel a jejich přidání do kolekce ModelState kontroleru:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Abychom mohli provádět tento kód je málo automaticky přidáním "ControllerHelpers" třídu do projektu NerdDinner a implementovat metody rozšíření "AddRuleViolations" v něm, přidá pomocnou metodu do třídy ASP.NET MVC ModelStateDictionary. Tato metoda rozšíření může zapouzdřit logice potřebné k naplnění ModelStateDictionary seznam RuleViolation chyby:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Můžeme aktualizovat naše metoda akce HTTP POST upravit chcete použít tuto metodu rozšíření k naplnění kolekci ModelState naše večeři porušení pravidel.

#### <a name="complete-edit-action-method-implementations"></a>Dokončení implementace metody akce úpravy

Následující kód implementuje všechny logice kontroleru potřebné pro náš scénář upravit:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

O naše implementace úpravy skvělé na tom je, že naše Třída Controller ani naše zobrazit šablonu má vědět nic o ověření konkrétní nebo obchodní pravidla, které vynucuje našeho večeři modelu. Můžete přidat další pravidla pro náš model v budoucnu jsme nemají žádné změny kódu do našich řadiče nebo zobrazení, aby mohly být podporována. To nám poskytuje flexibilitu snadno vyvíjet naše požadavky aplikací v budoucnu s minimální změn kódu.

### <a name="create-support"></a>Vytvoření podpory

Implementace chování "Upravit" Naše třída DinnersController byla dokončena. Pojďme nyní přesunout k implementaci "Vytvořit" podpory na něm – to umožní uživatelům přidávat nové večeří.

#### <a name="the-http-get-create-action-method"></a>GET protokolu HTTP vytvořit metody akce

Začneme budete implementací protokolu HTTP "GET" chování naše vytvořit metody akce. Tato metoda bude volána, když někdo navštíví */večeří/vytvořit* adresy URL. Naše implementace vypadá takto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Výše uvedený kód vytvoří nový objekt večeři a přiřadí jeho vlastnost EventDate jako jeden týden v budoucnu. Pak vykreslí zobrazení, která je založená na nový objekt večeři. Protože jsme nebyly předána explicitně název, který *View()* Pomocná metoda, se bude používat výchozí cestu založené na konvenci pro vyřešit zobrazit šablonu: /Views/Dinners/Create.aspx.

Nyní vytvoříme tuto šablonu zobrazení. Jsme to můžete provést kliknutím pravým tlačítkem myši v rámci vytvořit metody akce a výběrem příkaz "Přidat zobrazení" kontextové nabídky. V dialogovém okně "Přidat zobrazení" jsme budete znamenat, že jsme předali objekt večeři do zobrazit šablonu a rozhodnete automaticky vygenerované uživatelské rozhraní šablonu "Vytvořit":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Když kliknete na tlačítko "Přidat", Visual Studio bude uložit nové "Create.aspx" zobrazení vygenerovaného uživatelského rozhraní k adresáři "\Views\Dinners" a ho otevřít v prostředí IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Umožňuje provést několik změn výchozí "vytvořit" vygenerované uživatelské rozhraní soubor, který byl vygenerován pro nás a upravit ho, aby vypadala jako níže:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

A teď když jsme spustit naše aplikace a přístup *"/ večeří/vytvořit"* adresy URL v prohlížeči se budou vykreslovat od našich implementace akce vytvoření uživatelského rozhraní jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementace protokolu HTTP POST vytvořit metody akce

Máme HTTP GET verzi naše metoda akce vytvořit implementována. Když uživatel klikne na tlačítko "Uložit" provede metodu POST směřující formuláře */večeří/vytvořit* adresu URL, a odešle HTML &lt;vstupní&gt; tvoří hodnoty pomocí příkazu HTTP POST.

Teď umožňuje implementovat chování HTTP POST naše vytvořit metody akce. Začneme budete přidáním přetížené metody akce "Vytvořit" do našich DinnersController, který má atribut "AcceptVerbs" na označuje, že zpracovává HTTP POST scénáře:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Existují různé způsoby jsme získat přístup parametry odeslaného formuláře v rámci naší metoda HTTP POST povoleno "Vytvořit".

Jeden z přístupů je můžete vytvořit nový objekt večeři a pomocí *UpdateModel()* metodu helper (jako jsme to udělali s akci Upravit) k naplnění hodnoty odeslaného formuláře. Jsme můžete přidat do našich DinnerRepository, zachovat k databázi a přesměruje uživatele na našem akce podrobnosti zobrazíte nově vytvořený večeři pomocí následující kód:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Alternativně můžete používáme přístup kam máme naše Create() metoda akce trvat objekt večeři jako parametr metody. ASP.NET MVC se pak automaticky vytvořit nový objekt večeři pro nás, naplnit její vlastnosti pomocí formuláře vstupy a předejte ji do našich metodu akce:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Naše výše uvedených metod akce ověřuje, že objekt večeři úspěšně naplněné s hodnotami post formuláře kontrolou ModelState.IsValid vlastnosti. Tato možnost vrátí hodnotu false, pokud jsou vstupní problémů s převodem (například: řetězec "BOGUS" pro vlastnost EventDate), a pokud jsou nějaké problémy naše metoda akce znovu zobrazí formulář.

Pokud jsou platné vstupní hodnoty, poté metody akce se pokusí přidat a uložte nový večeři DinnerRepository. To zabalí tuto práci v rámci bloku try/catch – a znovu zobrazí formulář, pokud jsou všechny porušení pravidel obchodní (což by způsobilo metody dinnerRepository.Save() vyvolána výjimka).

Se tato chyba zpracování chování v akci, jsme požadavku */večeří/vytvořit* adresy URL a vyplňte si podrobnosti o nové večeři. Nesprávný vstup nebo hodnoty způsobí, že vytvoření formuláře se zobrazí znovu, s chybami zvýrazněná jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Všimněte si, jak je naše vytvořit formulář ctít zásady přesný stejná ověření a obchodní pravidla jako naše upravit formuláře. Je to proto, že naše pravidel ověřování a business byly definované v modelu a nebyla vložena v rámci uživatelského rozhraní nebo řadič aplikace. To znamená, že jsme můžete později změnit nebo momentální naše ověření nebo obchodní pravidla v jediném umístění a potom kliknul platí v celé aplikaci. Jsme nebude mít žádný kód v rámci buď změnit naše upravit nebo vytvořit metody akce, aby automaticky respektovali žádné nové pravidel nebo úpravy stávajících.

Pokud jsme opravit vstupní hodnoty a klikněte na tlačítko "Uložit" znovu, bude úspěšné naše přidání do DinnerRepository a nové večeři budou přidány do databáze. Potom jsme bude přesměrován na */Dinners/podrobnosti / [id]* adresa URL – kde jsme zobrazí se informace o nově vytvořený večeři:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Odstranit podpory

Přidejme "odstranit. podpora nyní na našem DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Metoda akce Delete HTTP GET

Začneme budete implementací chování naše metodu akce delete HTTP GET. Tato metoda bude zavolána, když někdo navštíví */Dinners/odstranění / [id]* adresy URL. Dole je implementace:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Metody akce, pokusí se načíst večeři k odstranění. Pokud večeře existuje vykreslí zobrazení založená na objekt večeři. Pokud objekt neexistuje (nebo byl již odstraněn) vrátí zobrazení, který vykreslí "NotFound" Zobrazit šablonu, kterou jsme vytvořili předtím pro metodu akce naše "Podrobnosti".

Můžeme vytvořit šablony zobrazení "odstranit. pravým tlačítkem myši v rámci metodu akce Delete a výběrem příkaz"Přidat zobrazení"kontextové nabídky. V dialogovém okně "Přidat zobrazení" jsme budete znamenat, že jsme předali objekt večeři do našich zobrazit šablonu jako svůj model a můžete vytvořit prázdnou šablonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Když kliknete na tlačítko "Přidat", přidejte Visual Studio nový soubor šablony "Delete.aspx" zobrazení pro nás v adresáři naše "\Views\Dinners". Přidáme některé HTML a kódu v šabloně implementovat obrazovka s potvrzením odstranění jako níže:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Výše uvedený kód zobrazí název večeři k odstranění a výstupy &lt;formuláře&gt; element, který nepodporuje metodu POST SMĚŘUJÍCÍ na /Dinners/odstranění / [id] adresu URL, pokud koncový uživatel klikne tlačítko Odstranit. v něm.

Jsme spuštění naše aplikace a přístup *"/ večeří/odstranění / [id]"* adresa URL platná večeře objektu se vykreslí uživatelského rozhraní, jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Téma straně: Proč jsme dělají příspěvku na?** |
| --- |
| Může požádat – a proč jsme projít úsilí vytváření &lt;formuláře&gt; v rámci naší potvrzovací obrazovce a odstranit? Proč ne jenom pomocí standardní hypertextový odkaz odkaz na metodu akce, která nepodporuje operace skutečného odstranění? Důvodem je, protože chceme pečlivě chránit před webové prohledávací moduly a vyhledávací weby zjišťování naše adresy URL a nechtěně způsobit data odstranit, pokud se na následujících odkazech. HTTP GET, na základě adresy URL jsou považovány za "bezpečnou" jim přístup nebo procházení a mají podle ty, které jsou HTTP POST. Doporučujeme zajistit můžete vždy uvést destruktivní nebo operace změny dat za požadavky HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementace metody akce Delete HTTP POST

Nyní je k dispozici HTTP GET verze naše metoda akce odstranění implementována, který se zobrazí obrazovka s potvrzením odstranit. Když koncový uživatel klikne tlačítko Odstranit. provede post formuláře na */Dinners/večeři / [id]* adresy URL.

Teď umožňuje implementovat chování "POST" HTTP delete metody akce pomocí následující kód:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Verze HTTP POST naše metodu akce Delete, pokusí se načíst objekt večeři k odstranění. Pokud tento soubor nelze nalézt (protože je již byl odstraněn) vykreslením naše "NotFound" šablony. Pokud najde večeře, odstraní se z DinnerRepository. Pak vykreslí šablonu "Odstraněno".

K implementaci šabloně "Odstraněno" jsme budete klikněte pravým tlačítkem na metodu akce a vyberte v místní nabídce "Přidat zobrazení". Jsme budete název naše zobrazení "Odstraněno" a mají být prázdné šablony (a nemusí provádět žádné objekt modelem silného typu). Potom přidáme některé obsah HTML k němu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

A teď když jsme spustit naše aplikace a přístup *"/ večeří/odstranění / [id]"* adresa URL platná večeře objekt se budou vykreslovat naše večeři odstranit potvrzení obrazovky jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Když jsme klikněte tlačítko Odstranit. provede POST protokolu HTTP na */Dinners/odstranění / [id]* adresu URL, která se z databáze odstranit večeře a zobrazí naše "Odstraněno" Zobrazit šablonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Zabezpečení vazeb modelu

Jsme probrali dva různé způsoby použití integrované funkce vazby modelu rozhraní ASP.NET MVC. První pomocí metody UpdateModel() aktualizovat vlastnosti na existující objekt modelu a druhý pomocí rozhraní ASP.NET MVC podporu pro předávání objektů modelu v jako parametry metody akce. Obě tyto postupy jsou velmi výkonný a velmi užitečné.

Tato power také přesune s ním zodpovědnost. Je důležité, aby vždy paranoid o zabezpečení při přijetí vstupu uživatele, a to platí taky při vytváření vazby objekty pro vstup formuláře. Měli být opatrní a vždy použije kódování HTML. všechny hodnoty zadané uživatele chcete zabránit útokům vkládání HTML a JavaScript a pečlivě prostřednictvím injektáže SQL (Poznámka: používáme pro naši aplikaci, která automaticky kóduje parametry, aby se tyto technologie LINQ to SQL typy útoků). Doporučujeme nikdy spoléhají na ověření na straně klienta samostatně a vždy využívají ověřování na straně serveru pro ochranu proti hackerům, pokus o neplatnou hodnoty odešle.

Jednu položku zvýšit zabezpečení a ujistěte se, že si myslíte o při použití vazby funkcí rozhraní ASP.NET MVC je rozsah objektů, které jsou vazby. Konkrétně budete chtít Ujistěte se, že rozumíte důsledkům zabezpečení vlastnosti, které chcete povolit možné svázat a ujistěte se, že můžete povolit jenom tyto vlastnosti, které by měly být opravdu aktualizovat aktualizovat koncovým uživatelem.

Ve výchozím nastavení se pokusí metodu UpdateModel() aktualizaci všech vlastností na objekt modelu, které odpovídají příchozí hodnoty parametrů formuláře. Podobně můžete objekty předané jako parametry metody akce také ve výchozím nastavení mají všechny jejich vlastnosti nastavit prostřednictvím parametrů formuláře.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Zamykání vazbu na základě za využití

Můžete uzamknout vazby zásad na základě za využití podle poskytnete explicitní "zahrnout seznam" vlastností, které lze aktualizovat. To lze provést pomocí předání parametru pole navíc řetězec metoda UpdateModel() jako níže:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objekty předané jako parametry metody akce také podporují [vazby] atribut, který umožňuje "patří seznamu" povolené vlastnosti, které chcete zadat jako níže:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Vazba na základě typu zamykání

Můžete také uzamknout vazbu pravidla na základě-type. Tímto způsobem lze určit pravidla vazby jednou a potom kliknul aplikaci ve všech scénářích (včetně UpdateModel a akce scénáře metoda parametr) ve všech kontrolerů a metod akcí.

Pravidla vazby na typ můžete přizpůsobit přidáním atribut [Bind] na typ, nebo tak, že zaregistrujete v souboru Global.asax aplikace (je to užitečné pro scénáře, kde nevlastníte typ). Pak můžete použít atribut vazby zahrnout a vyloučit vlastnosti pro řízení vlastnosti, které jsou vazbu pro určité třídy nebo rozhraní.

Jsme vám tento postup použijte pro třídu večeři v naší aplikaci NerdDinner a přidání atributu [vazby] k němu, která omezuje seznam vazbu vlastnosti na následující:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Všimněte si, že nejsou bychom povolovat kolekci RSVPs manipulaci s těmito prostřednictvím vazby, ani bychom povolovat DinnerID nebo HostedBy vlastnosti, které chcete nastavit prostřednictvím vazby. Z bezpečnostních důvodů jsme budete místo pouze upravit tyto konkrétní vlastnosti pomocí explicitní kódu v našem metody akce.

### <a name="crud-wrap-up"></a>CRUD Wrap-Up

ASP.NET MVC zahrnuje řadu integrovaných funkcí, které pomáhají s implementace formuláře publikování scénáře. Můžeme použít celou řadu tyto funkce k podpoře CRUD uživatelského rozhraní nad naše DinnerRepository.

Přístup se zaměřuje model se používá k implementaci naše aplikace. To znamená, že naše ověření a obchodní pravidlo, že logika je definována v rámci naší vrstva modelu – a není v rámci našich řadiče nebo zobrazení. Naše třída Controller ani naše zobrazení šablony vědět nic o konkrétní obchodní pravidla, které vynucuje naše večeři třídu modelu.

To ponechá Naše architektura aplikace Vyčištění a usnadňují otestovat. Jsme můžete přidat další obchodní pravidla v budoucnu naše vrstvy modelu a *není nutné provádět žádné změny kódu* naše řadiče nebo zobrazení, aby je podporovaná. Bude to nám poskytnout značnou část možnost vyvíjejí a mění naší aplikaci v budoucnu.

Naše DinnersController teď umožňuje večeři výpisech/podrobnosti, a také vytvářet, upravovat a odstraňovat podpory. Kód dokončení pro třídu naleznete níže:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Další krok

Nyní je základní podpora CRUD (vytvoření, čtení, aktualizaci a odstraňování) implementovat v rámci naší DinnersController třídy.

Nyní podíváme, jak můžete použít třídy ViewData a ViewModel povolit i bohatší uživatelského rozhraní na našem formulářů.

> [!div class="step-by-step"]
> [Předchozí](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [další](use-viewdata-and-implement-viewmodel-classes.md)
