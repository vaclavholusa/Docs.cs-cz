---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Zajištění akcí CRUD (vytváření, čtení, aktualizace nebo odstranění) podporujících zápis dat do formuláře | Dokumentace Microsoftu
author: microsoft
description: Krok 5 ukazuje, jak povolit podporu pro úpravy, vytváření a odstraňování večeří s ním i v této třídy Naše DinnersController Další.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 45d74249a34fc7e37e9776a398615d2f613a7582
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755000"
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Zajištění akcí CRUD (vytváření, čtení, aktualizace nebo odstranění) podporujících zápis dat do formuláře
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je kroku 5 tohoto bezplatného [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 5 ukazuje, jak povolit podporu pro úpravy, vytváření a odstraňování večeří s ním i v této třídy Naše DinnersController Další.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>NerdDinner krok 5: Vytvoření, aktualizace nebo odstranění scénáře formuláře

Jsme zavedené kontrolerů a zobrazení a popisuje způsoby se dají použít k implementaci prostředí seznamu a podrobností pro večeří v lokalitě. Naším dalším krokem bude Naše třída DinnersController jít dál a povolení podpory pro úpravy, vytváření a odstraňování večeří s ním také.

### <a name="urls-handled-by-dinnerscontroller"></a>Zpracovat DinnersController adresy URL

Dříve jsme přidali metody akce k DinnersController implementované podporu pro dvě adresy URL: */Dinners* a */Dinners/podrobnosti / [id]*.

| **ADRESA URL** | **PŘÍKAZ** | **Účel** |
| --- | --- | --- |
| */Dinners/* | ZÍSKAT | Zobrazení HTML seznam nadcházejících večeří. |
| */Dinners/podrobnosti / [id]* | ZÍSKAT | Zobrazit podrobnosti o konkrétní večeři. |

Teď přidáme akci metody k implementaci tři další adresy URL: <em>/Dinners/Edit / [id], / večeří/vytvoření,</em>a<em>/Dinners/Delete / [id]</em>. Podpora pro úpravu existující večeří, vytváření nových večeří a odstraňování večeří vám umožní tyto adresy URL.

Budeme podporovat HTTP GET a POST protokolu HTTP příkaz interakce se tyto nové adresy URL. Požadavky HTTP GET na tyto adresy URL se zobrazí počáteční HTML zobrazení dat (formuláře naplněný daty večeře v případě "upravit", prázdný formulář v případě "vytvořit" a potvrzovací obrazovce a delete v případě "odstranit"). Požadavky HTTP POST na tyto adresy URL se uložit, aktualizace nebo odstranění Dinner data v našich DinnerRepository (a z něj k databázi).

| **ADRESA URL** | **PŘÍKAZ** | **Účel** |
| --- | --- | --- |
| */Dinners/edit / [id]* | ZÍSKAT | Zobrazí Upravitelný formulář HTML naplněný daty večeři. |
| PŘÍSPĚVEK | Uložte změny formuláře pro konkrétní web Dinner do databáze. |
| */ Večeří/vytvoření* | ZÍSKAT | Zobrazte prázdný formulář HTML, který umožňuje uživatelům definovat nové večeří. |
| PŘÍSPĚVEK | Vytvořit nový web Dinner a uložte jej v databázi. |
| */Dinners/delete / [id]* | ZÍSKAT | Odstraňování potvrzovací obrazovce a zobrazit. |
| PŘÍSPĚVEK | Odstraní zadaný dinner z databáze. |

### <a name="edit-support"></a>Upravit podpory

Začněme implementací scénář "upravit".

#### <a name="the-http-get-edit-action-method"></a>Metoda akce HTTP GET úprav

Začneme implementací protokolu HTTP "GET" chování metodě akce úpravy. Tato metoda bude vyvolána v případě */Dinners/Edit / [id]* požadované adresy URL. Naše implementace bude vypadat takto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

Výše uvedený kód používá DinnerRepository k načtení objektu večeři. Pak vykreslí zobrazení šablony pomocí objektu večeři. Protože jsme nebyly předány explicitně název šablony, čímž *View()* Pomocná metoda, použije výchozí cestu založené na konvenci vyřešit zobrazit šablonu: /Views/Dinners/Edit.aspx.

Teď vytvoříme Tato šablona zobrazení. Uděláme to tak, že pravým tlačítkem myši v rámci metody úpravy a vyberete příkaz "Přidat zobrazení" kontextové nabídky:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

V dialogovém okně "Přidat zobrazení" jsme bude znamenat, že jsme prochází Dinner objektu do našich zobrazení šablony jako svůj model a rozhodnete automaticky vygenerované uživatelské rozhraní "Upravit" šablonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Když kliknete na tlačítko "Přidat", Visual Studio přidá nový soubor šablony zobrazení "Edit.aspx" nám v adresáři "\Views\Dinners". Otevře se také si novou šablonu zobrazení "Edit.aspx" v rámci – editor kódu – vyplní počáteční "Edit" vygenerované uživatelské rozhraní implementace jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Pojďme provést několik změn do vygenerované scaffold "edit" výchozí a aktualizujte šablony zobrazení pro úpravy mít následující obsah (tím se zruší některé vlastnosti, které jsme nechcete vystavit):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Pokud provozujeme aplikace a požadavek *"/ večeří/Edit/1"* URL vidíte na následující stránce:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

Značka jazyka HTML generovaných naše zobrazení vypadá níže. Je standardní HTML – s &lt;formuláře&gt; prvek, který provede HTTP POST do */Dinners/Edit/1* adresy URL při "Save" &lt;vstupní typ = "Odeslat" /&gt; vloženo tlačítko. HTML &lt;vstupní typ = "text" /&gt; element byl výstup pro každou upravovat vlastnost:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Metody Html.TextBox() pomocné rutiny Html a Html.BeginForm()

Naše "Edit.aspx" Zobrazit šablonu používá několik metod "Pomocné rutiny Html": Html.ValidationSummary(), Html.BeginForm(), Html.TextBox() a Html.ValidationMessage(). Kromě generování kód HTML pro USA, tyto pomocné metody poskytují integrovanou chyba zpracování a ověřování podporovat.

##### <a name="htmlbeginform-helper-method"></a>Html.BeginForm() Pomocná metoda

Pomocná metoda Html.BeginForm() je výstup HTML &lt;formuláře&gt; element v našem kódu. V našem Edit.aspx zobrazit šablonu si všimnete, že se má použít "" příkaz using při použití této metody jazyka C#. Otevřít složená závorka označuje začátek &lt;formuláře&gt; obsah a pravou složenou závorkou je co označuje konec &lt;/form&gt; element:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Případně pokud zjistíte příkazem "using" přístup nepřirozené pro takovéhoto scénáře, můžete použít kombinaci Html.BeginForm() a Html.EndForm() (což provede stejnou akci):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Volání Html.BeginForm() bez parametrů způsobí, že výstup element formuláře, který nemá HTTP POST na adresu URL aktuální žádosti. Proč náš zobrazení pro úpravy, který je generuje *&lt;tvoří akce = "/ večeří/Edit/1" metoda = "post"&gt;* elementu. Jsme může mít také předat explicitní parametry Html.BeginForm() pokud chceme na jinou adresu URL odeslat požadavek POST.

##### <a name="htmltextbox-helper-method"></a>Html.TextBox() Pomocná metoda

Naše Edit.aspx zobrazení používá pomocnou metodu Html.TextBox() výstup &lt;vstupní typ = "text" /&gt; prvky:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

Výše uvedené metody Html.TextBox() přijímá jeden parametr – sloužící k určení atributy id nebo název sady &lt;vstupní typ = "text" /&gt; element výstupu, jakož i vlastnost modelu k naplnění hodnotu textového pole. Například bylo na objekt Dinner jsme předána do zobrazení pro úpravy hodnoty vlastnosti "Title" z ".NET termínu" a proto volání metody naše Html.TextBox("Title") výstup: *&lt;vstupní id = "Title" name = "Title" type = "text" value = ".NET termínu" /&gt;*.

První parametr Html.TextBox() jsme Alternativně lze použít k určení id nebo název elementu a explicitně předávat hodnoty pro použití jako druhý parametr:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Často nám budete chtít provádět vlastní formátování na základě hodnoty, který je výstup. Statická metoda integrovaná do .NET String.Format() je užitečné pro tyto scénáře. Naše Edit.aspx zobrazit šablonu to se používá k formátování EventDate hodnotu (která je typu datum a čas), tak, aby nezobrazí sekund, čas:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Třetí parametr Html.TextBox() lze volitelně výstup další atributy HTML. Následující fragment kódu ukazuje, jak další velikost vykreslení = "30" atribut a třídy = "mycssclass" atribut na &lt;vstupní typ = "text" /&gt; elementu. Všimněte si, jak jsme se uvození název třídy pomocí atributu "@" character because "třída" je vyhrazené klíčové slovo v jazyce C#:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementace metody akce HTTP POST úpravy

Teď máme verzi HTTP GET metodě akce úpravy implementované. Když si uživatel vyžádá */Dinners/Edit/1* dostanou stránku HTML jako následující adresa URL:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Stisknutím tlačítka "Uložit" způsobí, že odeslaného formuláře do */Dinners/Edit/1* adresu URL, a odešle HTML &lt;vstupní&gt; tvoří hodnoty pomocí příkazu HTTP POST. Pojďme nyní implementují chování HTTP POST metodě akce úpravy – který bude zpracovávat ukládání společnosti Dinner.

Začneme budete přidáním přetížené metody akce "Edit" pro naše DinnersController, který má atribut "AcceptVerbs", která označuje, že zpracovává HTTP POST scénáře:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Při použití atributu [AcceptVerbs] u akce přetížené metody, ASP.NET MVC automaticky zpracovává dispatching požadavky na metody odpovídající akce v závislosti na příchozí příkaz protokolu HTTP. Požadavky HTTP POST do <em>/Dinners/Edit / [id]</em> adresy URL přejde do výše uvedené metody úpravy při všechny ostatní operace požadavky HTTP na <em>/Dinners/Edit / [id]</em>půjdou adresy URL pro první způsob úpravy implementovali jsme (které nebyla není definován atribut [AcceptVerbs]).

| **Téma na straně: Proč rozlišit pomocí příkazů HTTP?** |
| --- |
| Můžete pokládat – proč jsme pomocí jedné adresy URL a odlišení těchto své chování přes příkaz protokolu HTTP jsou? Stačí mít případně proč bezpečná není dvě samostatné adresy URL pro zpracování načítají a ukládají se úpravy změny? Příklad: /Dinners/Edit / [id] k zobrazení úvodní formulář a /Dinners/Save / [id] pro zpracování odeslaného formuláře ho uložit? Pomocí dvou samostatných adresy URL pro publikování nevýhodou je, že v případech, kdy budeme účtovat /Dinners/Save/2 a pak budete muset znovu zobrazit formulář HTML z důvodu chyby vstupu, se koncový uživatel skončí s večeří/Save/2 adresy URL do adresního řádku svého prohlížeče (vzhledem k tomu, který byl Adresa URL, publikuje se do formuláře). Pokud koncový uživatel záložky tuto redisplayed stránku do seznamu oblíbených položek jejich prohlížeče, nebo kopírování/vloží adresu URL a potom e-mailem friend, že skončí ukládá adresu URL, která nebude fungovat v budoucnu (protože tuto adresu URL, závisí na hodnoty post). Zveřejněním jednu adresu URL (například: /Dinners/Edit/[id]) a odlišení těchto zpracování pomocí příkazu HTTP, je bezpečný pro koncové uživatele na záložku stránky pro úpravu nebo odeslat adresu URL ostatním uživatelům. |

#### <a name="retrieving-form-post-values"></a>Načítání hodnoty Post formuláře

Existuje řada různých způsobů, jak se můžeme přístup k publikované parametrů formuláře v metodě "Edit" HTTP POST. Jednoduché jedním z přístupů je používat pouze vlastnost Request v základní třídě Controller přístup ke kolekci formuláře a načíst přímo odeslaných hodnot:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

Výše uvedené přístup je ale trochu verbose, zejména po můžeme přidat logiku zpracování chyb.

A lépe přístup pro tento scénář, je použít integrované *UpdateModel()* pomocnou metodu v základní třídě Controller. Podporuje, aktualizují se vlastnosti objektu, který předáme pomocí příchozí parametrů formuláře. Používá reflexi k určení názvů vlastností v objektu a pak automaticky převede a přiřazuje hodnoty na základě vstupních hodnot, odeslány klientem.

Pomocí této metody UpdateModel() jsme může zjednodušit naše HTTP POST upravit akci pomocí následujícího kódu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Můžete teď používáte */Dinners/Edit/1* adresy URL a změnit název naší společnosti Dinner:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Když kliknete na tlačítko "Save", jsme naši upravit akci provedete odeslaného formuláře a aktualizované hodnoty se nastavit jako trvalý, v databázi. Jsme pak přesměruje vás to podrobnosti adresy URL pro web Dinner (který se zobrazí nově uložené hodnoty):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Zpracování chyb úpravy

Naše aktuální implementace funguje HTTP POST jemné – s výjimkou případů, kdy jsou chyby.

Když uživatel provede chybu se upravuje formulář, musíme Ujistěte se, že je formulář zobrazí znovu, s informativními chybová zpráva, že je to opravit provede. To zahrnuje případy, kdy koncový uživatel odešle nesprávný vstup (například: řetězec poškozená data), jak dobře případech, kdy je platný formát vstupu, ale je porušení pravidla. Když dojít k chybám, že formulář by měl zachovat vstupní data tak, aby si uživatelé nebudou muset ručně doplňovat jejich změny původně zadané uživatelem. Tento proces by měl tolikrát, kolikrát podle potřeby opakujte, dokud formuláře se úspěšně dokončí.

ASP.NET MVC zahrnuje některé skvělé integrované funkce, které usnadňují zpracování chyb a redisplay formuláře. Umožňuje zobrazit tyto funkce v akci aktualizovat metodě akce upravit následujícím kódem:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

Ve výše uvedeném kódu je podobný naše předchozí implementace – s tím rozdílem, že jsme se nyní obtékání bloku try/catch – zpracování chyb naší práci. Pokud dojde k výjimce při volání metody UpdateModel() nebo po akci a uložit DinnerRepository (která bude vyvolána výjimka, pokud Dinner objekt, který jsme se pokusu o uložení je neplatný kvůli porušení pravidla v rámci našeho modelu), naše bloku catch chyba zpracování bude Spusťte. V něm jsme porušení pravidel, které existují v objektu Dinner ve smyčce a přidat je do objekt ModelState (který podíváme za chvíli). My potom zobrazte zobrazení.

Tento údaj zobrazíte práce, můžeme znovu spustit aplikaci, upravit Dinner a změňte ho na mít prázdný název, EventDate "BOGUS" a použijte telefonní číslo Spojené království s hodnotou země USA. Při stisknutí klávesy na tlačítko "Save" naše HTTP POST upravit metodu, nebudou moct uložit společnosti Dinner (protože nejsou chyby) a bude znovu zobrazit formulář:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Naše aplikace má vrazíme chyby prostředí. Elementy textu Neplatné zadání zvýrazněné červeně a chybových zpráv ověření se zobrazí koncovým uživatelům o nich. Formulář je také zachování vstupní data původně zadal uživatel – tak, aby si uživatelé nebudou muset nic doplnění.

Jak můžete pokládat, tato situace nastat? Jak textová pole názvu, EventDate a ContactPhone sami červeně zvýraznit a vědět, abyste výstupní hodnoty původně zadané uživatele? A jak chybové zprávy se zobrazí v seznamu v horní části? Dobrou zprávou je, že to neměli být magic – místo toho se vzhledem k tomu, že jsme použili některé integrované funkce rozhraní ASP.NET MVC, které usnadňují ověření vstupu a Chyba zpracování scénářů.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Principy ModelState a pomocné rutiny HTML metody ověřování

Třídy kontroleru mají "ModelState" kolekce vlastností, které poskytuje způsob, jak určit, že existují chyby s objektem modelu předávaný do zobrazení. Chyba položek v kolekci ModelState určují název vlastnosti modelu se problém (například: "Title", "EventDate" nebo "ContactPhone") a povolit lidských přívětivá chybovou zprávu zadat (například: "Je vyžadován název").

*UpdateModel()* Pomocná metoda automaticky naplní kolekci ModelState, pokud se setká chyby při pokusu o přiřazení hodnoty formuláře na vlastnosti objektu modelu. Například vlastnost EventDate náš objekt Dinner je typu datum a čas. Když metoda UpdateModel() se nepodařilo přiřadit hodnotu řetězce "BOGUS" ve výše uvedeném scénáři, metoda UpdateModel() přidat položku do kolekce ModelState udávající chybu přiřazení nedošlo k mají tuto vlastnost.

Vývojáři taky můžete napsat kód explicitně přidat chybové položky do kolekce ModelState jako provádíme níže v rámci naší "Chyťte" Chyba zpracování blok, který je naplnění položek založené na active porušení pravidel v kolekci ModelState Společnost dinner objektu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Pomocné rutiny HTML integrace s ModelState

Metody pomocné rutiny HTML – například Html.TextBox() – zkontrolujte ModelState kolekce při generování výstupu. Pokud existuje chybu pro položku vykreslují hodnotu zadanou uživatelem a chyba třídu šablony stylů CSS.

Například v našich "Upravit" zobrazení používáme Html.TextBox() pomocnou metodu k vykreslení EventDate náš objekt Dinner:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Po zobrazení byl vykreslen v případě chyby, metoda Html.TextBox() kontroluje ModelState kolekci zobrazíte, pokud byly všechny chyby spojené s vlastnost "EventDate" naše společnost Dinner objektu. Pokud to bylo zjištěno, že došlo k chybě vykreslen odeslané uživatelský vstup ("BOGUS") jako hodnota a přidá třídu css chyba na &lt;vstupní typ = "textového pole" /&gt; často generovaná značka:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Můžete přizpůsobit vzhled chyba třídu šablony stylů css vás pod rouškou libovolně. Chyba třídu šablony stylů CSS výchozí – "vstup-– Chyba ověřování" – je definován v *\content\site.css* šablony stylů a vyhledá podobná níže uvedenému příkladu:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Toto pravidlo šablony stylů CSS je, co způsobilo naše Neplatný vstupní prvky zvýrazněny jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Html.ValidationMessage() Pomocná metoda

Pomocná metoda Html.ValidationMessage() dá výstup chybové zprávy ModelState spojené s konkrétním modelu vlastnost:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

Výstupem kódu uvedeného výše:  *&lt;span třídy = "pole Chyba ověřování"&gt; je neplatná hodnota "BOGUS" &lt; /span&gt;*

Pomocná metoda Html.ValidationMessage() podporuje také druhý parametr, který vývojářům umožňuje přepsat chybová zpráva text, který se zobrazí:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

Výstupem kódu uvedeného výše:  <em>&lt;span třídy = "pole Chyba ověřování"&gt;\*&lt;/span&gt;</em>místo výchozí text chyby při chybě není dostupná Vlastnost EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Html.ValidationSummary() Pomocná metoda

Html.ValidationSummary() pomocnou metodu můžete použít k vykreslení souhrnu chybovou zprávu, spolu &lt;ul&gt;&lt;li /&gt;&lt;/ul&gt; seznam všechny podrobné chybové zprávy v ModelState kolekce:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

Pomocná metoda Html.ValidationSummary() přijímá parametr volitelný řetězec – definující souhrnu chybovou zprávu zobrazíte nad seznamem podrobné chyby:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Šablony stylů CSS můžete volitelně použít k přepsání, jak vypadá v seznamu chyb.

#### <a name="using-a-addruleviolations-helper-method"></a>Pomocí AddRuleViolations Pomocná metoda

Naše počáteční implementace upravit HTTP POST použít příkaz foreach v rámci bloku catch ve smyčce porušení pravidel Dinner objektu a jejich přidání do kolekce ModelState kontroleru:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Můžeme provádět tento kód trochu čistější přidáním "ControllerHelpers" třídy do projektu NerdDinner a implementaci metody rozšíření "AddRuleViolations" v ní, který přidá pomocnou metodu do třídy ASP.NET MVC ModelStateDictionary. Tato metoda rozšíření může zapouzdřit logiku potřebnou k naplnění ModelStateDictionary se seznamem chyb RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Společnost Microsoft může aktualizovat také metodě akce HTTP POST upravit aby používali tuto metodu rozšíření k naplnění kolekce ModelState s naší porušení pravidel Dinner.

#### <a name="complete-edit-action-method-implementations"></a>Proveďte úpravy implementace metody akce

Následující kód implementuje všechny řadiče logiku potřebnou pro náš scénář úpravy:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

O našich implementace úpravy skvělé na tom je, že naše třída Kontroleru ani naše zobrazit šablonu má vědět nic o konkrétní ověření nebo obchodní pravidla, které vynucuje náš model Dinner. Můžete přidat další pravidla pro náš model v budoucnu jsme není potřeba žádné změny kódu naše kontroler nebo zobrazení je podporovaná. To nám poskytuje flexibilitu při snadno vyvíjet naše požadavky aplikace v budoucnu s minimální změn kódu.

### <a name="create-support"></a>Vytvoření podpory

Jsme dokončili implementace chování "Upravit" náš DinnersController třídy. Teď přejdeme k implementaci podpory "Vytvořit" na něm – což umožní uživatelům přidávat nové večeří.

#### <a name="the-http-get-create-action-method"></a>Vytvořit metodu akce HTTP GET

Začneme budete implementací protokolu HTTP "GET" chování našich vytvořit metody akce. Tato metoda bude volána, když uživatel navštíví */večeří/vytvořit* adresy URL. Naše implementace vypadá takto:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

Výše uvedený kód vytvoří nový objekt Dinner a přiřadí jeho vlastnost EventDate jako jeden týden v budoucnu. Pak vykreslí zobrazení, které je založená na nový objekt večeři. Protože jsme nebyly předány explicitně název, který *View()* Pomocná metoda, použije výchozí cestu založené na konvenci vyřešit zobrazit šablonu: /Views/Dinners/Create.aspx.

Teď vytvoříme Tato šablona zobrazení. Můžeme to udělat tak, že kliknete pravým tlačítkem v rámci metody akce vytvořit a vyberete příkaz "Přidat zobrazení" kontextové nabídky. V dialogovém okně "Přidat zobrazení" budete Udáváme jsme prochází Dinner objektu do zobrazení šablony a šablonu "Vytvořit" se rozhodnete automaticky vygenerované uživatelské rozhraní:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Když kliknete na tlačítko "Přidat", Visual Studio uložit nové "Create.aspx" zobrazení vygenerovaného uživatelského rozhraní k adresáři "\Views\Dinners" a otevřete ho v rámci integrovaného vývojového prostředí:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Pojďme provést několik změn do výchozího "vytvořit" vygenerované uživatelské rozhraní souboru, který byl vygenerován pro nás a upravte si ho nahoru vypadat následovně:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

A teď jsme spouštění našich aplikací a přístup *"/ večeří/vytvořit"* adresy URL v prohlížeči, zobrazí se pak uživatelského rozhraní jako níže z naší implementace akce vytvořit:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementace HTTP POST vytvořit metody akce

Máme verzi protokolu HTTP GET naše vytvořit metody akce implementované. Když uživatel klikne na tlačítko "Save" provádí odeslaného formuláře do */večeří/vytvořit* adresu URL, a odešle HTML &lt;vstupní&gt; tvoří hodnoty pomocí příkazu HTTP POST.

Pojďme nyní implementují HTTP POST chování našich vytvořit metody akce. Začneme budete přidáním přetížené metody akce "Vytvořit" pro naše DinnersController, který má atribut "AcceptVerbs", která označuje, že zpracovává HTTP POST scénáře:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Existuje široká škála způsoby jsme parametry formuláře odeslaného přístup v metodě "Vytvořit" HTTP-POST povolena.

Jedním z přístupů je k vytvoření nového objektu Dinner a následné použití *UpdateModel()* pomocnou metodu (jak jsme to udělali s akcí upravit) a naplnit hodnotami odeslaného formuláře. Jsme můžete přidat do našich DinnerRepository, zachovat v databázi a přesměruje uživatele na naše akce podrobnosti se zobrazí nově vytvořený web Dinner pomocí níže uvedeného kódu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Můžete také můžeme použít přístup kde máme metodě akce Create() získat objekt Dinner jako parametr metody. ASP.NET MVC se pak automaticky vytvoření instance nového objektu Dinner pro nás naplnit její vlastnosti pomocí formuláře vstupů a předat metodě akce:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Naše výše uvedené metody akce ověřuje, že večeře objektu je úspěšně vyplněný hodnoty post formuláře tak, že zkontrolujete vlastnost ModelState.IsValid. Vrátí hodnotu false, pokud jsou vstupní problémů s převodem (například: řetězec pro vlastnost EventDate "BOGUS"), a pokud se objeví jakékoli problémy metodě akce znovu zobrazí formulář.

Pokud jsou vstupní hodnoty platné, metoda akce pokusí se přidejte a uložte novou večeře DinnerRepository. Zabalí tuto práci v rámci bloku try/catch a znovu zobrazí formulář, pokud jsou porušení obchodní pravidlo (které by způsobilo dinnerRepository.Save() metoda k vyvolání výjimky).

Se tato chyba zpracování chování v akci, nám pošlete */večeří/vytvořit* adresy URL a zadejte podrobnosti o nový web Dinner. Nesprávný vstup nebo hodnot způsobí vytvořit formulář se zobrazí znovu, s chybami zvýrazní jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Všimněte si, jak je naše vytvořit formulář dodržením přesně stejné ověřování a obchodních pravidel jako naše formulář pro úpravy. Je to proto, že naše ověření a obchodních pravidel byly definovány v modelu a nebyly vloženy v rámci uživatelského rozhraní nebo kontroleru aplikace. To znamená, že můžete později změnit/rozvíjíme naše ověření nebo obchodní pravidla v jednom umístění a potom kliknul použít v celé aplikaci. Jsme nebudete muset měnit žádný kód v rámci buď naše upravit nebo vytvořit metody akce automaticky případném dalším sdílení dodržovat všechny nová pravidla nebo upravovat stávající.

Když jsme opravit vstupní hodnoty a klikněte na tlačítko "Save" znovu, naše rozšířením DinnerRepository proběhne úspěšně a nové Dinner se přidají do databáze. My pak přesměruje vás to */Dinners/podrobnosti / [id]* adresa URL – kde jsme se zobrazí podrobnosti o nově vytvořený web Dinner:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Odstranit podpory

Přidejme "Odstranit" podpora nyní do našich DinnersController.

#### <a name="the-http-get-delete-action-method"></a>Metoda akce Delete HTTP GET

Začneme budete implementací chování metodě akce delete HTTP GET. Tato metoda bude zavolána, když uživatel navštíví */Dinners/Delete / [id]* adresy URL. Tady je implementace:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

Metoda akce se pokusí načíst Dinner odstranit. Společnosti Dinner existuje vykresluje zobrazení na základě objektu večeři. Pokud objekt neexistuje. (nebo už je odstraněné) se vrátí zobrazení, která vykresluje "Serveru" Zobrazit šablonu, kterou jsme vytvořili dříve pro metodu akce naše "Details".

Zobrazit šablonu "Odstranit" vytvoříme kliknutím pravým tlačítkem v rámci metody akce Delete a vyberete příkaz "Přidat zobrazení" kontextové nabídky. V dialogovém okně "Přidat zobrazení" jsme bude znamenat, že jsme se předá objekt Dinner do našich zobrazení šablony jako svůj model a můžete vytvořit prázdnou šablonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Když kliknete na tlačítko "Přidat", Visual Studio přidá nový soubor šablony zobrazení "Delete.aspx" pro nás v rámci naší adresáře "\Views\Dinners". Přidáme některá HTML a kódu do šablony pro implementaci potvrzovací obrazovce a odstranění podobná níže uvedenému příkladu:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

Výše uvedený kód zobrazuje se na večeři odstraníme a výstupů &lt;formuláře&gt; element, který provádí příspěvek /Dinners/Delete / [id] adresu URL, pokud koncový uživatel klikne tlačítko Odstranit. v něm.

Pokud provozujeme naše aplikace a přístup *"/ večeří/Delete / [id]"* adresa URL platná večeře objektu vykreslení uživatelského rozhraní, jako je následující:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Téma na straně: Proč se nám to jde příspěvek?** |
| --- |
| Můžete pokládat – proč jsme najdu prostřednictvím snaha o vytvoření &lt;formuláře&gt; v rámci naší potvrzovací obrazovce a odstranit? Proč prostě nepoužít standardní hypertextový odkaz na metodu akce, která provádí operace skutečné odstranění? Důvodem je, protože chceme dejte pozor, abyste pomáhalo chránit před webové prohledávací moduly a vyhledávací weby zjišťování adresy URL a nechtěně způsobit data odstranit, pokud jsou následujících odkazech. HTTP GET, na základě adresy URL jsou považovány za "bezpečné" jim přístup/procházení a mají podle ty HTTP-POST. Doporučujeme je zajistit, je vždy umístěte destruktivní nebo změnu operace s daty za požadavky HTTP POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementace metody akce Delete HTTP POST

Teď máme verzi HTTP GET naše Delete metody akce implementované, která se zobrazí obrazovka s potvrzením odstranit. Když koncový uživatel klikne tlačítko Odstranit. bude provedeno odeslaného formuláře do */Dinners/Dinner / [id]* adresy URL.

Teď můžeme implementovat chování "POST" HTTP delete metody akce pomocí níže uvedeného kódu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

Verze HTTP POST metodě akce odstranění, pokusí se načíst objekt dinner k odstranění. Pokud ji nemůže najít (protože je už Odstraněná) vykreslí naše šablony "Serveru". Pokud zjistí, večeře, odstraní se z DinnerRepository. Pak vykreslí šablonu "Odstraněno".

K implementaci "Odstraněno" šablony vytvoříme klikněte pravým tlačítkem na metodu akce a zvolte místní nabídce "Přidat zobrazení". Vytvoříme název naší zobrazení "Odstraněno" a mají být prázdné šablony (a nemusí provádět žádné objekt modelem silného typu). Potom přidáme nějaký obsah HTML k ní:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

A teď jsme spouštění našich aplikací a přístup *"/ večeří/Delete / [id]"* adresa URL platná večeře potvrzení odstranění objektu, zobrazí se pak naše společnost Dinner obrazovky jako níže:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Když kliknete na tlačítko "Odstranit" bude provedeno typu HTTP POST na */Dinners/Delete / [id]* adresu URL, která se z databáze odstranit společnosti Dinner a zobrazí naše "Odstraněno" Zobrazit šablonu:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Zabezpečení vazby modelu

Výše dva různé způsoby použití integrované funkce vazby modelu ASP.NET MVC. První metodou UpdateModel() aktualizovat vlastnosti existujícího objektu modelu a druhý pomocí podpory ASP.NET MVC pro předání objekty modelu v jako parametry metod akce. Oba tyto postupy jsou velmi výkonné a velmi užitečné.

Tento výkon také přináší odpovědnost. Je důležité, aby vždycky byl paranoid o zabezpečení při přijímání vstupu uživatele, a to platí taky při vytváření vazby objektů na vstupu formuláře. By měl být pozor, abyste vždy použije kódování HTML. všechny hodnoty uživatel zadal, pokud chcete zabránit útokům prostřednictvím injektáže kódu HTML a JavaScript a buďte opatrní útoků prostřednictvím injektáže SQL (Poznámka: používáme pro naši aplikaci, která automaticky zakóduje parametry, aby se zabránilo tyto technologie LINQ to SQL typy útoků). Nikdy byste využívají ověřování na straně klienta samostatně a vždy využívají ověřování na straně serveru pro ochranu proti hackerům pokusu o odeslání fiktivní hodnoty.

Jedna položka další zabezpečení zajistit, aby že si myslíte o při použití vazby funkce rozhraní ASP.NET MVC je rozsah objektů, které jsou vazby. Konkrétně budete chtít Ujistěte se, že chápete důsledky zabezpečení vlastnosti, které chcete povolit potvrzujete a ujistěte se, že je povolený jenom ty vlastnosti, které by měl být opravdu aktualizovat aktualizovat koncovým uživatelem.

Ve výchozím nastavení UpdateModel() metoda se pokusí aktualizovat všechny vlastnosti objektu modelu, které odpovídají příchozí hodnoty parametrů formuláře. Podobně může mít objekty, které jsou předány jako parametry metod akce také ve výchozím nastavení všechny jejich vlastnosti nastavené přes parametrů formuláře.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Uzamčení vazby na základě za využití

Můžete uzamknout pomocí poskytující explicitní "seznamu pro zahrnutí" vlastností, které se dají aktualizovat zásady vazeb na základě za využití. To lze provést předáním parametr pole další řetězec metodě UpdateModel() jako níže:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objekty, které jsou předány jako parametry metod akce také podporují atribut [vazby], který umožňuje "seznamu pro zahrnutí" z vlastnosti níže uvedené jako Povolené:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Omezovat se jenom na základě typu vazby

Můžete také uzamknout pravidel vazby na základě-type. To umožňuje zadat pravidel vazby jednou a pak je mít znovu použít ve všech scénářích (včetně UpdateModel a akce scénáře metoda parametr) ve všech kontrolerů a metod akce.

Pravidla na typ vazby můžete přizpůsobit tak, že přidáte atribut [vazby] na typ, nebo když si zaregistrujete v souboru Global.asax aplikace (režim vhodný pro scénáře, ve kterém nevlastníte typ). Pak můžete použít atribut vazby zahrnutí a vyloučení vlastností do ovládacího prvku vlastnosti, které jsou pro dané třídy nebo rozhraní s možností vazby.

Použijeme tuto techniku pro třídu večeře v naší aplikace NerdDinner a přidejte atribut [vazby], který omezuje seznam vlastnosti umožňující vazbu na následující:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Všimněte si, že jsme nepovoluje kolekci RSVPs manipulovat prostřednictvím vazby ani jsme povolují DinnerID nebo HostedBy vlastnosti, které mají být nastaveno prostřednictvím vazby. Z bezpečnostních důvodů jsme vám místo toho manipulovat pouze s tyto konkrétní vlastnosti pomocí explicitní kódu v rámci naší metody akce.

### <a name="crud-wrap-up"></a>CRUD Wrap-Up

ASP.NET MVC zahrnuje řadu integrovaných funkcí, které pomáhají s implementací formuláře účtování scénáře. Jsme celou řadu tyto funkce používají k zajištění podpory uživatelského rozhraní CRUD nad rámec naší DinnerRepository.

Přístup zaměřený na model se používá k implementaci naši aplikaci. To znamená, že veškerému našemu ověřování a obchodní pravidlo, že logika je definována v rámci naší modelu vrstvy – a není v rozsahu naší zobrazení nebo kontrolery. Naše třída Kontroleru ani naše šablony zobrazení vědět nic o konkrétní obchodní pravidla, které vynucuje Naše třída modelu Dinner.

Se udržovat čisté naší aplikace architektury a zjednodušíme proces testování. Můžete přidáme další obchodní pravidla pro náš model vrstvy v budoucnu a *není nutné provádět žádné změny kódu* naše Kontroleru nebo zobrazení je podporována. Bude to nabízí spoustu Flexibilita se vyvíjet a změnit v budoucnu naši aplikaci.

Naše DinnersController teď umožňuje Dinner výpisy/podrobnosti, také vytvořit, upravit a odstranit podpory. Níže najdete kompletní kód pro třídu:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Dalším krokem

Nyní je k dispozici podpora na úrovni basic CRUD (vytvoření, čtení, aktualizace a odstranění) implementace v rámci naší DinnersController třídy.

Nyní Podívejme se na jsme použití slovníku ViewData a ViewModel třídy umožňující ještě podrobnější uživatelského rozhraní na naše formuláře.

> [!div class="step-by-step"]
> [Předchozí](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [další](use-viewdata-and-implement-viewmodel-classes.md)
