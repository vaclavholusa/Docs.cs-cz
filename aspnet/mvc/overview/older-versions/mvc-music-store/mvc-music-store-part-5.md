---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Část 5: Úpravy formulářů a ukázka | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 5 popisuje úpravy formulářů a ukázka.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-5-edit-forms-and-templating"></a>Část 5: Úpravy formulářů a ukázka
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.
> 
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 5 popisuje úpravy formulářů a ukázka.


V posledních kapitoly jsme byly načítání dat z databáze a jeho zobrazení. V této kapitole jsme budete také povolit úpravy data.

## <a name="creating-the-storemanagercontroller"></a>Vytváření StoreManagerController

Budete začneme vytvořením nového řadiče názvem **StoreManagerController**. Pro tento kontroler můžeme využít výhod funkce generování uživatelského rozhraní dostupné v ASP.NET MVC 3 nástroje pro aktualizaci. Nastavení možností pro dialogové okno Přidat kontroler, jak je uvedeno níže.

![](mvc-music-store-part-5/_static/image1.png)

Když kliknete na tlačítko Přidat, uvidíte, že mechanismus generování uživatelského rozhraní ASP.NET MVC 3 nemá dobré množství práce, můžete:

- Vytvoří nový StoreManagerController s místní proměnné Entity Framework
- Přidá do projektu zobrazení složky StoreManager složku
- Přidá Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml a Index.cshtml zobrazení silného typu k třídě Album

![](mvc-music-store-part-5/_static/image2.png)

Nové třídy kontroleru StoreManager zahrnuje CRUD (vytvořit, číst, aktualizovat, odstraňovat) akce kontroleru, které vědět, jak pracovat s alba třída modelu a použije naše kontext Entity Framework pro přístup k databázi.

## <a name="modifying-a-scaffolded-view"></a>Úprava vygenerované zobrazení

Je důležité si pamatovat, že když tento kód se vygeneroval pro nás, je standardní kódu ASP.NET MVC, stejně jako jste jsme byla zápis v rámci tohoto kurzu. Je určena k ušetříte čas, který by potřebují na psaní kódu kontroleru standardní a ruční vytvoření zobrazení se silnými typy, ale nejedná se o typ generovaného kódu může viděli jste, kterými pří upozornění v komentářích o tom, jak nesmí změnit kód. Toto je váš kód a jste chtěli ho změnit.

Ano začneme rychlé upravit pro zobrazení indexu StoreManager (nebo Views/StoreManager/Index.cshtml). Toto zobrazení zobrazí tabulky, který uvádí alb v našem úložišti pomocí operace upravit / podrobnosti / odstranit odkazy a zahrnuje alba veřejné vlastnosti. Odstraníme AlbumArtUrl pole, protože se nejedná o velmi užitečná v tomto zobrazení. V &lt;tabulky&gt; část zobrazení kódu, odeberte &lt;tý&gt; a &lt;td&gt; elementy, které obaluje AlbumArtUrl odkazy, jak znázorňuje následující zvýrazněné řádky:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Kód upravené zobrazení bude vypadat takto:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>První podívejte se na správce úložiště

Nyní spusťte aplikaci a vyhledejte/StoreManager /. Zobrazí se jsme právě změněna, zobrazující seznam alba v úložišti s odkazy na Upravit podrobnosti a odstraňte Index Správce úložiště.

![](mvc-music-store-part-5/_static/image3.png)

Kliknutím na odkaz pro úpravy zobrazí formulář upravit s poli alba, včetně rozevírací seznamy pro Genre a umělcem.

![](mvc-music-store-part-5/_static/image4.png)

Klikněte na odkaz "Zpět na seznam" v dolní části, a potom klikněte na odkaz na podrobnosti pro Album. Zobrazí podrobné informace o jednotlivých alb.

![](mvc-music-store-part-5/_static/image5.png)

Znovu klikněte na tlačítko zpět do seznamu odkazů a potom klikněte na odkaz pro odstranění. Zobrazí se dialogové okno s potvrzením, zobrazení podrobností alba a s dotazem, pokud jsme si jisti, že chceme odstranit.

![](mvc-music-store-part-5/_static/image6.png)

Kliknutím na tlačítko Odstranit v dolní části odstraňte alba a vrátíte se na indexovou stránku, který ukazuje album odstranit.

Jsme neprovádí se správcem úložiště, ale máme řadiče a zobrazení kódu pro operace CRUD spuštění z.

## <a name="looking-at-the-store-manager-controller-code"></a>Prohlížení kódu Kontroleru Správce úložiště

Řadič Správce úložiště obsahuje dobrý množství kódu. Přejděte přes tento shora dolů. Řadičem zahrnuje některé standardní obory názvů pro kontroler MVC, stejně jako odkaz na obor názvů naše modely. Řadičem má privátní instanci MusicStoreEntities, každý z akce kontroleru používané pro přístup k datům.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Uložení akce správce Index a podrobnosti

Zobrazení indexu načte seznam alb, včetně každé album odkazované Genre a umělcem informace, jak jsme viděli dříve při práci na metodě procházet úložiště. Zobrazení indexu je následující odkazy na propojené objekty tak, že ho můžete zobrazení každé album Genre a umělcem název, kontroler, která má být efektivní a dotazování pro tyto informace v původní žádosti.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Akce kontroleru podrobnosti řadičem StoreManager funguje stejně jako akce podrobnosti řadič úložiště jsme napsali dříve – vyžádá alba podle ID pomocí metody Find(), vrátí ji do zobrazení.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Vytvořit metody akce

Vytvořit metody akce jsou jen málo liší od těch, které jsou, které jste viděli dosavadní práce, protože vstup formuláře, které zpracovávají. Pokud uživatel nejprve navštíví /StoreManager nebo vytvořit nebo se zobrazí prázdné formuláře. Tato stránka HTML bude obsahovat &lt;formuláře&gt; elementy, kde můžete zadat podrobnosti alba vstupní element, který obsahuje rozevírací seznam a textové pole.

Poté, co uživatel vyplní hodnot formuláře alb, jejich stiskněte tlačítko "Uložit" k odeslání, že tyto změny zpět do aplikace pro uložení v databázi. Uživatel stiskne tlačítko "uložit" &lt;formuláře&gt; provede POST protokolu HTTP zpět na /StoreManager nebo vytvořit nebo adresa URL a odeslat &lt;formuláře&gt; hodnoty v rámci protokolu HTTP POST.

ASP.NET MVC umožňuje snadno rozložit logiku z těchto dvou scénářů volání adresy URL povolením nám implementovat dvě samostatné metody akce "Vytvořit", v rámci třídy Naše StoreManagerController – jednu pro zpracování počáteční procházet HTTP GET a /StoreManager/Create Nebo adresa URL a dalších pro zpracování požadavku HTTP POST odeslané změny.

### <a name="passing-information-to-a-view-using-viewbag"></a>Předání informací zobrazení použití položek ViewBag

Jsme dříve v tomto kurzu jste použili objekt ViewBag, ale nebyly mluvili mnohem o něm. Objekt ViewBag umožňuje nám předávat informace k zobrazení bez použití objekt silného typu modelu. V takovém případě naší akce kontroleru HTTP GET upravit musí předat i seznam žánry a umělci do formuláře k naplnění rozevíracích seznamů a nejjednodušší způsob, jak to udělat je mohli je vrátit jako položek ViewBag.

Objekt ViewBag je dynamický objekt, což znamená, že můžete zadat ViewBag.Foo nebo ViewBag.YourNameHere, aniž byste museli psát kód, který definuje tyto vlastnosti. Kód řadiče používá v tomto případě ViewBag.GenreId a ViewBag.ArtistId tak, aby hodnoty rozevírací odeslána s formuláře budou GenreId a ArtistId, které jsou vlastnostmi alb, které bude nastavení.

Tyto hodnoty rozevírací seznam se vrátíte na formuláři pomocí SelectList objekt, který je vytvořen pouze k tomuto účelu. Děje se tak pomocí kódu takto:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Jak je vidět z kódu metoda akce tři parametry jsou používány k vytvoření tohoto objektu:

- Seznam položek, které bude možné zobrazení rozevíracího seznamu. Všimněte si, že tato akce není právě řetězec – seznam žánry jsme se předávání.
- Další parametr předána SelectList je vybraná hodnota. Tento způsob SelectList umí předem vyberte položku v seznamu. To bude jednodušší zjistit, když se podíváme na úpravy formuláře, který je poměrně podobné.
- Konečný parametr je vlastnost, která má být zobrazen. V tomto případě to je označující, zda je vlastnost Genre.Name, které se zobrazí uživateli.

Si uvědomit pak je poměrně jednoduché akce HTTP GET vytvořit – dvě SelectLists jsou přidány do objekt ViewBag a předán žádný objekt modelu formuláře (protože nebyl ještě vytvořen).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Pomocné rutiny HTML zobrazení seznamy vyřadit v zobrazení vytvořit

Vzhledem k tomu, že jste už jsme mluvili o tom, jak se předávají rozevíracího seznamu hodnot do zobrazení, podívejme rychlý přehled najdete v zobrazení tyto hodnoty zobrazení. V zobrazení kódu (nebo Views/StoreManager/Create.cshtml), zobrazí se následující volání přišla zobrazíte rozevírací Genre dolů.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

To se označuje jako HTML Helper - nástroj metodu, která provádí úlohu běžné zobrazení. Pomocné rutiny HTML jsou velmi užitečné při ochraně našich kód zobrazení stručná a čitelná. Poskytuje pomocné rutiny Html.DropDownList ASP.NET MVC, ale jako ukážeme později je možné vytvořit vlastní pomocné rutiny pro zobrazení kód, který jsme budete znovu použít v naší aplikaci.

Volání Html.DropDownList právě musí být sdělili dvě věci –, kde k získání seznamu zobrazíte a jaké hodnota (pokud existuje) by měla být předem vybrané. Určuje první parametr, GenreId, rozevírací seznam a hledat hodnotu s názvem GenreId ve model nebo ViewBag. Druhý parametr je slouží k určení hodnota, která má zobrazit jako zpočátku vybrali v rozevíracím seznamu. Vzhledem k tomu, že tento formulář je formulář pro vytvoření, neexistuje žádná hodnota být Zkontrolujte předem vybrané a je předán String.Empty.

### <a name="handling-the-posted-form-values"></a>Zpracování odeslání formuláře hodnoty

Jak již bylo zmíněno před, existují dvě metody akce, které jsou spojené s každou formuláře. První zpracuje požadavek HTTP GET a zobrazí formulář. Druhý zpracuje požadavek HTTP POST, který obsahuje hodnoty odeslaného formuláře. Všimněte si, že má akce kontroleru [HttpPost] atributu, který aplikaci ASP.NET MVC oznámeno, že má pouze odpovědět na požadavky HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Tato akce má čtyři zodpovědnosti:

- 1. Čtení hodnot formuláře
- 2. Zkontrolujte, pokud formuláře hodnoty předejte všechna pravidla ověřování
- 3. Pokud je odeslání formuláře je platný, uložte data a zobrazit aktualizovaný seznam
- 4. Pokud odeslání formuláře není platný, je třeba znovu zobrazte formulář s chyby ověření

#### <a name="reading-form-values-with-model-binding"></a>Čtení hodnot formuláře s vazby modelu

Odeslání formuláře, který obsahuje hodnoty pro GenreId a ArtistId (z rozevíracího seznamu) a hodnoty textového pole pro název, ceny a AlbumArtUrl zpracovává akce kontroleru. I když je možné přímý přístup k hodnotám formuláře, je vhodnější je použití možností vytvoření vazby modelu integrovaných v architektuře ASP.NET MVC. Když akce kontroleru přebírá jako parametr typu modelu, ASP.NET MVC se pokusí naplnit objekt daného typu pomocí formuláře vstupy (stejně jako hodnoty trasy a řetězce dotazu). Dělá to tak, že vyhledá hodnoty, jejichž názvy odpovídají vlastností objektu modelu, například při nastavení hodnoty GenreId nový objekt alb, hledá vstup s názvem GenreId. Při vytváření zobrazení pomocí standardních metod v architektuře ASP.NET MVC formuláře budou vždy být vykreslen pomocí názvů vlastností jako vstupní pole názvy, tak to názvy polí se právě odpovídat.

#### <a name="validating-the-model"></a>Ověření modelu

Model byl ověřen pomocí jednoduché volání ModelState.IsValid. Jsme nepřidali žádné ověřovací pravidla Naše třída Album ještě - provedeme, které za chvilku – proto nyní tato kontrola nemá mnohem udělat. Co je důležité je, že tato kontrola ModelStat.IsValid se přizpůsobí se jí ověřovacích pravidel, které jsme umístí našeho modelu, takže budoucí změny pravidel ověřování nevyžadují žádné aktualizace kódu akce kontroleru.

#### <a name="saving-the-submitted-values"></a>Ukládání odeslaná hodnoty

Pokud odeslání formuláře ověřením úspěšně projde, je čas k uložení hodnoty do databáze. S platformou Entity Framework, který právě vyžaduje přidávání modelu do kolekce alb a volání SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Rozhraní Entity Framework generuje příslušnými příkazy SQL k zachování této hodnoty. Po uložení dat, můžeme přesměrovat zpět do seznamu alb, uvidíme náš aktualizace. K tomu je potřeba vrácení RedirectToAction s názvem akce kontroleru, který jsme má zobrazit. V takovém případě, je metoda Index.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Zobrazení odesílání neplatný formuláře s chybami ověření

V případě vstup neplatný formuláře hodnoty rozevírací jsou přidány do objekt ViewBag (jako v případě HTTP GET) a model vazby hodnoty jsou předán zpět do zobrazení pro zobrazení. Chyby ověření se automaticky zobrazí pomocí @Html.ValidationMessageFor pomocné rutiny HTML.

#### <a name="testing-the-create-form"></a>Testování vytvořit formulář

K otestování tohoto limitu, spusťte aplikaci a přejděte k /StoreManager/Create / - zobrazí se prázdný formulář, který byl vrácen metodou HTTP pro vytvoření StoreController-GET.

Vyplňte hodnoty a klikněte na tlačítko Vytvořit k odeslání formuláře.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Zpracování úprav

Upravit dvojici akce (HTTP GET a HTTP POST) jsou velmi podobné vytvořit metody akce, kterou jsme právě prohlédli. Vzhledem k tomu, že Upravit scénář zahrnuje práci s existujícího alba, upravit HTTP-GET metoda načte založené na parametr "id" Album předána prostřednictvím trasy. Tento kód pro načtení album od AlbumId je stejný, jako jsme jste dříve prohlédl v podrobnosti akce kontroleru. Stejně jako u vytvořením / metoda HTTP GET rozevíracího seznamu hodnoty jsou vráceny prostřednictvím objekt ViewBag. To umožňuje nám vrátit Album jako naše model objektu do zobrazení (což je pro třídu Album silného typu) při předání další data (např. seznam žánry) prostřednictvím objekt ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Akce HTTP POST upravit je velmi podobné akce HTTP POST vytvořit. Jediným rozdílem je, že místo přidávání nové album k databázi. Kolekce alb, jsme se hledání aktuální instance alba pomocí db. Entry(album) a nastavení jeho stavu na změněné. Tato hodnota informuje Entity Framework upravovat existujícího alba a vytvoření nového.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Abychom mohli otestovat to zjistit tak, že spuštění aplikace a procházení/StoreManger / pak kliknutím na odkaz pro úpravy pro album.

![](mvc-music-store-part-5/_static/image9.png)

Zobrazí se upravit formu jako je uvedené metodou HTTP GET upravit. Vyplňte hodnoty a klikněte na tlačítko Uložit.

![](mvc-music-store-part-5/_static/image10.png)

To formulář odešle, uloží hodnoty a vrátí nám do seznamu alb, zobrazující, že hodnoty byly aktualizovány.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Zpracování odstranění

Odstranění následující stejné jako upravit a vytvořit, pomocí akce jeden řadič k zobrazení formuláře potvrzení a jiné akce kontroleru pro zpracování odeslání formuláře.

Akce kontroleru HTTP GET odstranit je přesně stejný jako naše předchozí akce kontroleru podrobnosti Správce úložiště.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Zobrazuje se formulář, který je silného typu typ alb, pomocí šablony odstranit zobrazení obsahu.

![](mvc-music-store-part-5/_static/image12.png)

Odstranit šablonu zobrazí všechna pole pro model, ale nemůžeme odlišují zjednodušení této nižší. Zobrazení kódu v /Views/StoreManager/Delete.cshtml změňte na následující.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Zobrazí zjednodušená potvrzení odstranění.

![](mvc-music-store-part-5/_static/image13.png)

Kliknutím na tlačítko Odstranit způsobí, že formulář poslat zpět na server, který provede akci DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Naše HTTP POST odstranit řadiče akce provede následující akce:

- 1. Načte Album podle ID
- 2. Neodstraní alba a uložit změny
- 3. Provede přesměrování Index, zobrazující, že alba byla odebrána ze seznamu

Abyste to mohli otestovat, spusťte aplikaci a přejděte do /StoreManager. Vyberte album ze seznamu a klikněte na odkaz odstranit.

![](mvc-music-store-part-5/_static/image14.png)

Zobrazí se naše potvrzovací obrazovce a odstranění.

![](mvc-music-store-part-5/_static/image15.png)

Kliknutím na tlačítko Odstranit odebere alba a vrátí nám na stránku Index Správce úložiště, který ukazuje, že alba byla odstraněna.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Použití vlastní pomocné rutiny HTML zkrátit text

My jsme jedním z možných problémů s naší stránce s Index Správce úložiště. Naše název alba a umělcem název vlastnosti můžete obě být dostatečně dlouhé, může vyvolat vypnout naše formátování tabulky. Vytvoříme vlastní pomocné rutiny HTML umožněte nám snadno zkrátit těchto a dalších vlastností v našem zobrazení.

![](mvc-music-store-part-5/_static/image17.png)

Syntaxe Razor pro @helper syntaxe má provedené velmi snadné vytvoření vlastní pomocné funkce pro použití v zobrazení. Otevření zobrazení /Views/StoreManager/Index.cshtml a následující kód přidejte přímo po @model řádku.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Tato pomocná metoda přebírá řetězec a umožňuje maximální délku. Pokud zadaný text je kratší než zadaná délka, pomocné rutiny výstupy jej jako-je. Pokud je delší, zkrátí text a vykreslí "..." pro zbytek.

Nyní jsme vám pomůže naše Truncate pomocníka název alba i umělcem název vlastnosti musí být kratší než 25 znaků. Kód dokončení zobrazení pomocí našeho nového pomocníka Truncate se zobrazí níže.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Nyní když jsme procházejí /StoreManager/ adresu URL, alba a názvy jsou zachovány níže naše maximální délky.

![](mvc-music-store-part-5/_static/image18.png)

Poznámka: Zobrazí v případě jednoduchého vytváření a používání pomocné rutiny v jednom zobrazení. Další informace o vytváření Pomocníci, které můžete použít v celé vaší lokality, najdete v části Moje blogu: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-4.md)
> [další](mvc-music-store-part-6.md)
