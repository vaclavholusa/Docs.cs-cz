---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: '5. část: Editační formuláře a Šablonování | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 5 obsahuje úpravy formuláře a šablonování textu.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: acb4a4c427870e375ff19823f0bdfa208438e899
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835998"
---
<a name="part-5-edit-forms-and-templating"></a>5. část: Editační formuláře a Šablonování
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.
> 
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 5 obsahuje úpravy formuláře a šablonování textu.


V posledních kapitole jsme se načítání dat z databáze a jeho zobrazení. V této kapitole jsme zároveň umožní úpravy data.

## <a name="creating-the-storemanagercontroller"></a>Vytváří StoreManagerController

Budete Začneme tím, že vytvoříte nový kontroler volá **StoreManagerController**. Pro tento kontroler jsme se využití výhod funkcí generování uživatelského rozhraní dostupných v ASP.NET MVC 3 nástroje Update. Nastavení možností v dialogovém okně Přidat kontroler, jak je znázorněno níže.

![](mvc-music-store-part-5/_static/image1.png)

Když kliknete na tlačítko Přidat, uvidíte, že mechanismus generování uživatelského rozhraní ASP.NET MVC 3 dělá správné množství práce za vás:

- Vytvoří nový StoreManagerController s lokální proměnná Entity Framework
- Přidá do zobrazení složky projektu StoreManager složku
- Přidá zobrazení Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml a Index.cshtml do alb třídy silného typu

![](mvc-music-store-part-5/_static/image2.png)

Nová třída kontroleru StoreManager zahrnuje CRUD (vytváření, čtení, aktualizace nebo odstranění) akce kontroleru, které už víte, jak pracovat s alba třída modelu a používat náš kontext Entity Framework pro přístup k databázi.

## <a name="modifying-a-scaffolded-view"></a>Úprava vygenerovaná zobrazení

Je dobré si uvědomit, že když tento kód byl generován pro nás, je standardní kód technologie ASP.NET MVC, stejně jako My jsme byl zápis v celém tomto kurzu. Je určená a ušetřete čas strávený by na psaní kódu kontroleru standardizované a ruční vytvoření zobrazení se silnými typy, ale to není typ generovaného kódu jste možná viděli začíná v komentářích, o jak nesmí změnit adresář upozornění kód. To je váš kód a už má ho změnit.

Takže začneme rychlé úpravy zobrazení indexu StoreManager (/ Views/StoreManager/Index.cshtml). Toto zobrazení zobrazí tabulku, která obsahuje seznam alb v našem úložišti pomocí operace upravit / podrobnosti / odstranit odkazy a zahrnuje alba veřejné vlastnosti. Pole AlbumArtUrl Odebereme, protože není v tomto zobrazení velmi užitečné. V &lt;tabulky&gt; části zobrazení kódu, odeberte &lt;th&gt; a &lt;td&gt; prvky okolní AlbumArtUrl odkazy, jak je uvedeno ve níže zvýrazněné řádky:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Upravené zobrazit kód bude vypadat následovně:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>První pohled na Store správce

Nyní spusťte aplikaci a přejděte do/StoreManager /. Zobrazí se správce Index Store jsme právě upravili, seznamem alba v úložišti s odkazy na podrobnosti, upravit nebo odebrat.

![](mvc-music-store-part-5/_static/image3.png)

Kliknutím na odkaz pro úpravy zobrazí formulář pro úpravy s poli alba, včetně rozevírací seznamy pro žánr a interpreta.

![](mvc-music-store-part-5/_static/image4.png)

Klikněte na odkaz "Zpět do seznamu" v dolní části a potom klikněte na odkaz podrobnosti pro alba. Zobrazí podrobné informace o jednotlivých Album.

![](mvc-music-store-part-5/_static/image5.png)

Znovu klikněte na tlačítko Zpět na odkaz na seznamu a potom klikněte na odkaz pro odstranění. Zobrazí se potvrzovací dialogové okno, zobrazující podrobnosti alba a s dotazem, pokud jsme si jisti, že chceme, aby k jeho odstranění.

![](mvc-music-store-part-5/_static/image6.png)

Kliknutím na tlačítko Odstranit v dolní části odstranit alba a vrátí na indexovou stránku, která zobrazuje alba odstraněn.

Pomocí Správce Store nezvládli jsme, ale máme řadič a zobrazit kód pro spuštění z operace CRUD.

## <a name="looking-at-the-store-manager-controller-code"></a>Prohlížení kódu Store správce Kontroleru

Správce řadiče Store obsahuje správné množství kódu. Podívejme to shora dolů. Kontroler zahrnuje některé standardní obory názvů pro kontroler MVC, stejně jako odkaz na našem oboru názvů modelů. Kontroler má privátní instanci MusicStoreEntities používaný jednotlivými akce kontroleru pro přístup k datům.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Store správce Index a podrobnosti akce

Index zobrazení načte seznam alb, včetně každé album odkazované žánr a interpreta informace, jak jsme viděli dříve při práci na metodu Store Procházet. Zobrazení indexu je následující odkazy na propojené objekty tak, aby ji můžete zobrazit název žánr každý alba a interpret, tak kontroleru se ještě efektivní a dotazování pro tuto informaci v původní požadavek.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Akce kontroleru podrobnosti o adaptéru StoreManager funguje stejně jako akce Kontroleru Details Store jsme napsali dříve – dotazuje na alba podle ID, pomocí metody Find(), pak jej vrátí do zobrazení.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Vytvořit metody akce

Vytvořit metody akce jsou mírně liší od těch, které jsme zatím viděli, protože vstup formuláře, které zpracovávají. Pokud uživatel nejprve navštíví /StoreManager/vytvoření /, zobrazí se prázdný formulář. Na této stránce HTML &lt;formuláře&gt; prvků, ve kterém můžete zadat podrobnosti alba vstupní element, který obsahuje rozevírací seznam a textové pole.

Jakmile uživatel vyplní alba hodnot formuláře, stisknutím "Save" tlačítko pro odeslání, že tyto změny zpět do naší aplikace uložte do databáze. Když uživatel stiskne tlačítko "save" &lt;formuláře&gt; provede HTTP-POST zpět na /StoreManager/vytvoření/adresu URL a odešlete &lt;formuláře&gt; hodnoty jako součást požadavku HTTP POST.

ASP.NET MVC umožňuje snadno rozdělit logiku z těchto dvou scénářů volání adresy URL tím, že abychom mohli implementovat dvě samostatné metody akce "Vytvořit", v rámci naší třídy StoreManagerController – z nich se má zpracovat počáteční procházet HTTP GET /StoreManager/Create / Adresa URL a druhý pro zpracování požadavku HTTP POST odevzdané změny.

### <a name="passing-information-to-a-view-using-viewbag"></a>Předávání informací do zobrazení použití položek ViewBag

Jsme využili objekt ViewBag dříve v tomto kurzu, ale nebyly mluvili většinu o něm. Objekt ViewBag umožňuje k předávání informací do zobrazení bez použití objektu model silného typu. V tomto případě naše akce kontroleru HTTP GET úpravy je potřeba předat i seznam žánry a umělci do formuláře k naplnění a nejjednodušší způsob, jak to udělat, je vrátit jako objekt ViewBag položky.

Objekt ViewBag je dynamický objekt, což znamená, že můžete zadat ViewBag.Foo nebo ViewBag.YourNameHere, aniž byste museli psát kód, který definuje tyto vlastnosti. Kód kontroler použije v tomto případě ViewBag.GenreId a ViewBag.ArtistId tak, aby odeslané s formuláři rozevíracího seznamu hodnoty budou GenreId a ArtistId, které jsou vlastnosti, které se nastavení.

Tyto hodnoty rozevírací seznam se vrátí do formuláře pomocí SelectList objektu, který je vytvořenou přesně pro tento účel. To se provádí pomocí kódu takto:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Jak je vidět z kódu metody akce, tři parametry jsou použity k vytvoření tohoto objektu:

- Seznam položek, které rozevírací seznam bude zobrazení. Všimněte si, že to není jenom řetězec – jsme se předají seznam žánrů.
- Další parametr předávaný do SelectList je vybraná hodnota. Tento způsob SelectList ví, jak předem vyberete položku v seznamu. To bude srozumitelnější, když se podíváme na formulář pro úpravy, které je hodně podobné.
- Poslední parametr je vlastnost, která má být zobrazena. V tomto případě to je, že vlastnost Genre.Name co se zobrazí uživateli.

To na paměti pak je docela jednoduché akce HTTP GET vytvoření – dva SelectLists jsou přidány do objekt ViewBag a žádný objekt modelu je předán do formuláře (protože nebyl ještě vytvořen).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Pomocné rutiny HTML zobrazíte rozevírací nabídky v zobrazení pro vytváření

Vzhledem k tomu, že už jsme mluvili o jak rozevírací seznam hodnot jsou předány do zobrazení, Pojďme se na rychlý pohled na zobrazení, abyste viděli, jak tyto hodnoty jsou zobrazeny. V zobrazení kódu (/ Views/StoreManager/Create.cshtml), zobrazí se následující při volání k zobrazení seznamu žánr dolů.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

To se označuje jako pomocné rutiny HTML – nástroj pro metodu, která provádí běžné zobrazení úlohy. Pomocné rutiny HTML jsou velmi užitečné při zachování našeho kódu zobrazení stručnější a čitelnější. Poskytuje pomocné rutiny Html.DropDownList ASP.NET MVC, ale jak uvidíme dále je možné vytvořit vlastní pomocné rutiny pro zobrazení kódu, které nám budete znovu použít v naší aplikaci.

Volání Html.DropDownList je jenom nutné až k tomu dojde dvě věci – tam, kde k získání seznamu zobrazíte a jakou hodnotu (pokud existuje) musí být předem vybraná. První parametr GenreId, říká DropDownList hledat hodnotu s názvem GenreId v modelu nebo objekt ViewBag. Druhý parametr slouží k označení hodnoty zobrazíte původně vybrali v rozevíracím seznamu. Protože tento formulář je formulář pro vytvoření, neexistuje žádná hodnota bude Zkontrolujte předem vybrané a je předán String.Empty.

### <a name="handling-the-posted-form-values"></a>Zpracování hodnot publikování formuláře

Jak jsme probírali před, existují dvě metody akce přidružené každý formulář. První zpracuje požadavek HTTP GET a zobrazí formulář. Druhá zpracovává žádost HTTP POST, který obsahuje hodnoty odeslané formuláře. Všimněte si, že má akce kontroleru atributu [HttpPost], který technologii ASP.NET MVC řekne, že by měl pouze reagovat na požadavky HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Tato akce má čtyři odpovědnosti:

- 1. Čtení hodnot formuláře
- 2. Zaškrtněte, pokud hodnot formuláře předejte všechna pravidla ověřování
- 3. Pokud se odeslání formuláře je platný, uložit data a zobrazí aktualizovaný seznam
- 4. Pokud odeslání formuláře není platná, znovu zobrazte formulář s chyby ověření

#### <a name="reading-form-values-with-model-binding"></a>Čtení hodnot formuláře pomocí vazby modelu

Odeslání formuláře, který obsahuje hodnoty GenreId a ArtistId (z rozevíracího seznamu) a textové pole hodnot pro název, cena a AlbumArtUrl zpracovává akce kontroleru. I když je možné pro přímý přístup k hodnotám formuláře, lepším řešením je použití do ASP.NET MVC integrované možnosti vazby modelu. Typ modelu, který jako parametr pořízením akce kontroleru ASP.NET MVC se pokusí o naplnění objektu tohoto typu pomocí formuláře vstupy (stejně jako hodnoty trasy a řetězce dotazu). Dělá to pomocí hodnot, jejichž názvy odpovídají vlastností objektu modelu, třeba při nastavování hodnoty GenreId nový objekt alb, hledá vstupní hodnota s názvem GenreId. Při vytváření zobrazení pomocí standardních metod v architektuře ASP.NET MVC formuláře bude vždy být vykreslen pomocí názvy vlastností jako názvy vstupních polí, takže to názvy polí se právě spárujte.

#### <a name="validating-the-model"></a>Ověření modelu

Model se ověří pomocí jednoduchého volání do ModelState.IsValid. Jsme nepřidali žádná pravidla ověřování do naší třídy alba ještě – uděláme, které se za chvilku – teď tuto kontrolu nemusí dělat v podstatě. Důležité je, že tato kontrola ModelStat.IsValid se bude přizpůsobovat ověřovacích pravidel, které jsme na náš model, takže budoucí změny pravidel ověřování nevyžadují žádné aktualizace kódu akce kontroleru.

#### <a name="saving-the-submitted-values"></a>Ukládá zadané hodnoty

Pokud odeslání formuláře projdou ověřením, je čas k uložení hodnoty do databáze. S Entity Framework, která vyžaduje jen přidáním modelu do kolekce alba a volání SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework generuje příslušnými příkazy SQL pro uchování hodnoty. Po uložení dat, jsme přesměrovat zpět do seznamu alb, takže uvidíme náš aktualizace. To se provádí tak, že vrací RedirectToAction s názvem, které chceme zobrazená akce kontroleru. V tomto případě to je metoda Index.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Neplatná notace odesílání chyb ověření zobrazení

V případě neplatná notace vstupní hodnoty rozevírací seznam se přidají do objekt ViewBag (stejně jako v případě protokolu HTTP GET) a hodnoty vazby modelu jsou předány zpět do zobrazení pro zobrazení. Chyby ověřování se automaticky zobrazí pomocí @Html.ValidationMessageFor pomocné rutiny HTML.

#### <a name="testing-the-create-form"></a>Testování vytvořit formulář

K otestování to, spusťte aplikaci a přejděte na /StoreManager/vytvoření / – zobrazí se prázdný formulář, který byl vrácen metodou HTTP pro vytvoření StoreController-GET.

Vyplňte hodnoty a klikněte na tlačítko Vytvořit k odeslání formuláře.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Zpracování úprav

Jsou velmi podobné vytvořit metody akce, které zvažovali jsme i jen pár akce úpravy (HTTP GET a POST protokolu HTTP). Protože Upravit scénář zahrnuje práce s existující album, upravit HTTP-GET načte metoda na základě parametru "id" Album předaný prostřednictvím trasy. Tento kód pro načtení alba podle AlbumId je stejný, jako jsme dříve se zabývali v podrobnosti akce kontroleru. Stejně jako u vytvořením / metody GET protokolu HTTP, rozevírací seznam hodnot jsou vráceny prostřednictvím objekt ViewBag. To umožňuje vrátit alba jako náš model objektu zobrazení (které je silně typováno do třídy alba) při předání dalších dat (např. seznam žánry) přes objekt ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Upravit HTTP-POST akce je velmi podobný akce vytvoření HTTP-POST. Jediným rozdílem je, že místo přidání nové album k databázi. Kolekce alb, jsme hledáte aktuální instance alba pomocí databáze. Entry(album) a nastavení jeho stavu na změněné. To říká Entity Framework, upravovat stávající album na rozdíl od vytvoření nového.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Můžeme to zjistit otestovat spuštění aplikace a přejdete do/StoreManger/a poté kliknutím na odkaz pro úpravy pro alba.

![](mvc-music-store-part-5/_static/image9.png)

Zobrazí se zobrazí metodou HTTP GET upravit formulář pro úpravy. Vyplňte hodnoty a klikněte na tlačítko Uložit.

![](mvc-music-store-part-5/_static/image10.png)

To formulář odešle, uloží hodnoty a vrátí nám do seznamu obrázek znázorňující, že hodnoty byly aktualizovány.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Zpracování odstranění

Odstranění používá stejný vzor jako Edit and Create, pomocí akce jeden kontroler k potvrzení formulář pro zobrazení a další akce kontroleru pro zpracování odeslání formuláře.

Akce kontroleru HTTP GET odstranit je přesně stejný jako naše předchozí akce kontroleru Store správce podrobnosti.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Formulář, který je silně typováno zobrazíme na typ alb, šablona odstranit zobrazení obsahu.

![](mvc-music-store-part-5/_static/image12.png)

Odstranit šablonu zobrazí všechna pole pro model, ale můžeme odlišují zjednodušit tohoto seznamu. Změňte zobrazení kódu v /Views/StoreManager/Delete.cshtml takto.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Zobrazí se zjednodušený potvrzení odstranění.

![](mvc-music-store-part-5/_static/image13.png)

Kliknutím na tlačítko Odstranit způsobí, že formulář, který se má publikovat zpět na server, který provede akci DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Naše HTTP POST odstranit akce Kontroleru provede následující akce:

- 1. Načte alba podle ID
- 2. Neodstraní alba a uložit změny
- 3. Provede přesměrování Index, zobrazuje, zda alba byl odebrán ze seznamu

Abyste to mohli otestovat, spusťte aplikaci a přejděte do /StoreManager. Vyberte alba ze seznamu a klikněte na odkaz pro odstranění.

![](mvc-music-store-part-5/_static/image14.png)

Zobrazí se potvrzovací obrazovce a naše odstranit.

![](mvc-music-store-part-5/_static/image15.png)

Kliknutím na tlačítko Odstranit odebere alba a vrátí nám na Store správce indexovou stránku, který ukazuje, že alba byl odstraněn.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Použití vlastní pomocné rutiny HTML došlo ke zkrácení text

Máme jedním potenciálním problémem s naší Store správce indexovou stránku. Naše alba a interpret vlastnosti mohou být dostatečně dlouhá, že může vyvolat vypnout naše formátování tabulky. Vytvoříme vlastní pomocné rutiny HTML k umožňuje snadno zkrátit těchto a dalších vlastnostech v našich zobrazení.

![](mvc-music-store-part-5/_static/image17.png)

Syntaxe Razor pro @helper syntaxe bylo poměrně snadno vytvořit vlastní pomocné funkce pro použití v zobrazení. Otevření zobrazení /Views/StoreManager/Index.cshtml a následující kód přidejte přímo po @model řádku.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Tato pomocná metoda přijímá řetězec a umožňující maximální délku. Pokud zadaný text je kratší než délka zadaná, pomocné rutiny uloží jej jako-je. Pokud je delší, zkrátí text a vykreslí "..." pro zbytek.

Teď můžeme použít naše Truncate pomocné rutiny pro interpret i název vlastnosti musí být kratší než 25 znaků. Kompletní přehled kódu pomocí našich nových Truncate pomocné rutiny se zobrazí pod.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Nyní když jsme procházejí /StoreManager/ adresy URL, alba a názvy jsou zachovány nižší než naše maximální délky.

![](mvc-music-store-part-5/_static/image18.png)

Poznámka: Zobrazí jednoduchý případ vytváření a použití pomocné rutiny v jednom zobrazení. Další informace o vytvoření pomocných rutin, které můžete použít v celém webu, najdete v mé blogovém příspěvku: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-4.md)
> [další](mvc-music-store-part-6.md)
