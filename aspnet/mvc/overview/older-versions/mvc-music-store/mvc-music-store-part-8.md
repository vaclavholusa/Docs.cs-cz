---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: '8. část: Nákupní košík s aktualizacemi Ajax | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 8. část se věnuje nákupní košík s aktualizacemi Ajax.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 881d47b5b4df5a4d310a1b3a7eec6ee97b0d42ea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823836"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>8. část: Nákupní košík s aktualizacemi Ajax
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.  
>   
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 8. část se věnuje nákupní košík s aktualizacemi Ajax.


Jsme vám umožňují uživatelům umístit alb v jejich košíku i bez registrace, ale budete muset zaregistrovat jako hosty do dokončení registrace. Proces nákupu a registrace bude možné rozdělit na dva řadiče: ShoppingCart kontroler, který umožňuje anonymně přidávání položek do košíku a řadiči Checkout, která zpracovává proces platby u pokladny. Jsme budete začínat nákupního košíku v této části a pak vytvořit proces platby u pokladny v následující části.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Přidání třídy modelu košíku, pořadí a OrderDetail

Způsobí, že naše nákupního košíku a Git Checkout procesy pomocí některých nových tříd. Klikněte pravým tlačítkem na složku modely a přidejte třídu košíku (Cart.cs) následujícím kódem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Tato třída je hodně podobné jiným, které jsme zatím použili s výjimkou atribut [Key] pro vlastnost RecordId. Naše košíku položky budou mít identifikátor řetězce s názvem CartID umožňující anonymní nákupu, ale v tabulce obsahuje celé číslo primární klíč s názvem RecordId. Podle konvence Entity Framework Code-First očekává, že primární klíč pro tabulku s názvem košíku bude CartId nebo ID, ale jsme můžete snadno změnit prostřednictvím poznámky nebo kód pokud chceme. Toto je příklad, jak můžete použít jednoduchý konvence v Entity Framework Code-First při vyhovují nám ale jsme nejsou omezeny je když ne.

V dalším kroku přidejte třídu pořadí (Order.cs) následujícím kódem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Tato třída sleduje informace o souhrnné informace a doručování pro objednávky. **Nebude zkompilován ještě**, protože má OrderDetails navigační vlastnost, na které závisí na třídě jsme ještě nevytvořili. Opravte Řekněme, že nyní přidáním třída s názvem OrderDetail.cs, přidáním následujícího kódu.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Teď uděláme jeden poslední aktualizace našich MusicStoreEntities třídy, aby obsahoval DbSets, která zpřístupňují tyto nové třídy modelu, včetně také DbSet&lt;interpreta&gt;. Aktualizované MusicStoreEntities třídy se zobrazí jako níže.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Správa obchodní logiky nákupního košíku

V dalším kroku vytvoříme třídy ShoppingCart ve složce modely. ShoppingCart model zajišťuje přístup k datům do košíku tabulky. Kromě toho zajistí obchodní logiku pro přidávání a odebírání položek z nákupního košíku.

Protože nechceme budou muset uživatelé zaregistrovat účet pouze pro přidání položky do jejich nákupního košíku, přiřadíme uživatele dočasné jedinečný identifikátor (pomocí identifikátoru GUID, nebo globálně jedinečný identifikátor) když přistupují k nákupního košíku. Uložíme toto ID horizontálních oddílů pomocí třídy relace technologie ASP.NET.

*Poznámka: Relace technologie ASP.NET je vhodné místo pro ukládání informací specifických pro uživatele, jejíž platnost vyprší po opuštění webu. Zatímco zneužití stav relace může mít vliv na výkon na větších serverech, používáme světla budou fungovat dobře pro demonstrační účely.*

Třída ShoppingCart poskytuje následující metody:

**AddToCart** přebírá jako parametr alba a přidá jej do košíku uživatele. Protože tabulky košíku sleduje množství pro každý obrázek, obsahuje logiku pouze zvýšit množství, pokud uživatel už má seřazené jednu kopii alba a v případě potřeby vytvořte nový řádek.

**RemoveFromCart** přijímá ID alba a odstraní ji z nákupního košíku uživatele. Pokud má uživatel, jenom jednu kopii alba v jejich košíku, odeberou se řádek.

**EmptyCart** odebere všechny položky z nákupního košíku uživatele.

**GetCartItems** načte seznam CartItems k zobrazení nebo zpracování.

**GetCount** načte celkový počet alb uživatele má své nákupní košík je prázdný.

**GetTotal** vypočítá celkové náklady na seznam všech položek v košíku.

**CreateOrder** během fáze checkout převede nákupního košíku objednávku.

**GetCart** je statická metoda, která umožňuje naše řadiče pro získání objektu košíku. Používá **GetCartId** metodu ke zpracování čtení CartId z uživatelské relace. Metoda GetCartId vyžaduje hodnotu HttpContextBase tak, aby jej číst CartId uživatele z uživatelské relace.

Tady je úplný **ShoppingCart třídy**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>Modely ViewModels

Naše řadič nákupního košíku se potřebují komunikovat některé komplexní informace pro jeho zobrazení, které není přímo namapován na naše objekty modelu. Nechceme změnit náš modely tak, aby odpovídala naše zobrazení; Model třídy by měly představovat naše domény, nikoli uživatelského rozhraní. Jedním řešením může být k předání informací o našich zobrazeními pomocí třídy položek ViewBag, jako jsme to udělali s informacemi o Store správce rozevírací seznam, ale předáním velké množství informací prostřednictvím objekt ViewBag získá náročná na správu.

Toto řešení je použít *ViewModel* vzor. Při použití tohoto modelu se nám vytvořit třídy silného typu, které jsou optimalizované pro naše konkrétní zobrazení scénáře a které vystavit vlastnosti pro dynamické hodnoty/obsah potřebný pomocí našich šablon zobrazení. Naše třídy kontroleru můžete naplnit a předat zobrazit šablonu pro použití těchto tříd optimalizovaných pro zobrazení. Díky tomu bezpečnost typů, kontrola za kompilace a editor technologie IntelliSense v rámci šablon zobrazení.

Vytvoříme dvě zobrazení modelů pro použití v kontroleru nákupního košíku: ShoppingCartViewModel bude obsahovat obsah nákupního košíku uživatele a ShoppingCartRemoveViewModel se použije k zobrazení informací potvrzení, když uživatel něco z jejich košíku.

Pojďme vytvořit novou složku modely ViewModels v kořenové složce projektu z důvodu zjednodušení uspořádané. Klikněte pravým tlačítkem na projekt, vyberte možnost Přidat / novou složku.

![](mvc-music-store-part-8/_static/image1.jpg)

Název složky modely ViewModel.

![](mvc-music-store-part-8/_static/image1.png)

V dalším kroku přidejte třídu ShoppingCartViewModel ve složce modely ViewModel. Má dvě vlastnosti: seznam položek v košíku a desítkovou hodnotu pro uložení celkovou cenu pro všechny položky v košíku.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Nyní přidejte ShoppingCartRemoveViewModel ke složce modely ViewModels s následující čtyři vlastnosti.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Řadič nákupního košíku

Řadič nákupního košíku se třem hlavním účelům: Přidání položky do košíku, odebírání položek z košíku a zobrazení položek v košíku. Provede pomocí tří tříd jsme právě vytvořili: ShoppingCartViewModel ShoppingCartRemoveViewModel a ShoppingCart. Stejně jako v StoreController a StoreManagerController přidáme pole pro uložení instance MusicStoreEntities.

Nový řadič nákupního košíku přidáte do projektu pomocí šablony prázdný kontroler.

![](mvc-music-store-part-8/_static/image2.png)

Tady je úplný ShoppingCart Kontroleru. Index a přidat kontroler akce velmi povědomé. Akce kontroleru odebrat a CartSummary zpracování dva speciální případy, které probereme v následující části.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Aktualizace AJAX pomocí jQuery

Dále vytvoříme stránku nákupní košík Index, který je silně typováno do ShoppingCartViewModel a používá šablonu zobrazení seznamu stejným způsobem jako před.

![](mvc-music-store-part-8/_static/image3.png)

Ale nemusíte používat Html.ActionLink k odebrání položek z košíku, použijeme jQuery "propojí" událost click pro všechny odkazy v tomto zobrazení, které mají třídy HTML při odebírání odkazu. Místo odesílání formuláře, tuto obslužnou rutinu události kliknutí stačí provede zpětné volání AJAX naše RemoveFromCart akce kontroleru. RemoveFromCart vrátí výsledek serializován do formátu JSON, který pak analyzuje a provádí čtyři rychlé aktualizace stránky pomocí jQuery naše jQuery zpětného volání:

- 1. Odebere odstraněné alba ze seznamu
- 2. Aktualizuje počet košíku v hlavičce
- 3. Zobrazí uživateli zprávu aktualizace
- 4. Aktualizuje celková cena košíku

Protože odebrat scénář je zpracovávaných zpětného volání Ajax v rámci zobrazení indexu, nebudeme potřebovat další zobrazení RemoveFromCart akce. Tady je kompletní kód pro zobrazení /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Pokud chcete otestovat to zjistit, potřebujeme mít možnost přidávat položky na naše řešení nákupního košíku. Aktualizujeme naše **Store podrobnosti** aby zobrazení zahrnovalo na tlačítko "Přidat do košíku". Když jsme na to, můžeme zahrnout některé z alba Další informace, které jsme přidali protože jsme toto zobrazení poslední aktualizace: Genre, interpreta, ceny a alb. Aktualizovaný kód Store podrobnosti zobrazení se zobrazí, jak je znázorněno níže.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Nyní jsme klikněte prostřednictvím obchodu a vyzkoušejte přidávání a odebírání alb do a z naše řešení nákupního košíku. Spusťte aplikaci a přejděte do indexu Store.

![](mvc-music-store-part-8/_static/image4.png)

Pak klikněte na Genre, chcete-li zobrazit seznam alb.

![](mvc-music-store-part-8/_static/image5.png)

Kliknutí na nadpisech alba teď se teď zobrazují naše aktualizované alba podrobnosti, včetně tlačítko "Přidat do košíku".

![](mvc-music-store-part-8/_static/image6.png)

Kliknutím na tlačítko "Přidat do košíku" se teď zobrazují naše nákupního košíku Index s souhrnný seznam nákupního košíku.

![](mvc-music-store-part-8/_static/image7.png)

Po načtení do nákupního košíku, můžete kliknout na odebrat z nákupního košíku odkazu zobrazíte aktualizace Ajax do nákupního košíku.

![](mvc-music-store-part-8/_static/image8.png)

Jsme sestavili funkční nákupní košík, což umožňuje neregistrovaným uživatelů k přidávání položek do jejich košíku. V následující části jsme vám zajistí, aby registraci a dokončete proces platby u pokladny.


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-7.md)
> [další](mvc-music-store-part-9.md)
