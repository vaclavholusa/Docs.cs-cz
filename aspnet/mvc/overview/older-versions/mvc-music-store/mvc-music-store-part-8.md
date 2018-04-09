---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Část 8: Nákupní košík s aktualizace Ajax | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 8 obsahuje nákupní košík s aktualizace Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Část 8: Nákupní košík s aktualizace Ajax
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.  
>   
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 8 obsahuje nákupní košík s aktualizace Ajax.


Jsme vám umožňují uživatelům umístit alb jejich košíku bez registrace, ale budete muset zaregistrovat jako hosty do dokončení checkout. Proces nákupního a najdete v článku věnovaném rozdělena na dva řadiče: ShoppingCart řadiči, který umožňuje anonymně přidávání položek do košíku a řadič Checkout, která zpracovává proces checkout. Jsme budete začínat nákupní košík v této části a pak vytvořit proces najdete v článku věnovaném v následující části.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Přidání třídy modelu košíku, pořadím a OrderDetail

Naše nákupní košík a najdete v článku věnovaném procesy budou používat některých nových tříd. Klikněte pravým tlačítkem na složku modely a přidejte třídu košíku (Cart.cs) s následujícím kódem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Tato třída je poměrně podobně jako ostatní, které jste použili jsme dosavadní práce, s výjimkou [klíč] atributu pro vlastnost RecordId. Naše položky v košíku budou mít identifikátor řetězec s názvem CartID umožnit anonymní nákupy, ale v tabulce jsou zahrnuty celé číslo primární klíč s názvem RecordId. Podle konvence Entity Framework Code-první očekává primární klíč pro tabulku s názvem košíku budou CartId nebo ID, že jsme můžete snadno přepsat, prostřednictvím poznámky nebo kód pokud chceme. Toto je příklad, jak můžete použít jednoduchý konvence v Entity Framework Code-první při jejich nám vyhovovat, ale nemůžeme nejsou omezeny je při ne.

Dál přidejte třídu pořadí (Order.cs) s následujícím kódem.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Tato třída sleduje informace o pořadí souhrn a doručení. **Nebude zkompilován ještě**, protože obsahuje rozpis objednávek navigační vlastnost, na které závisí na třídě jsme ještě nevytvořili. Umožňuje opravit, že nyní přidáním třídu s názvem OrderDetail.cs, přidání následující kód.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Vytočit jednu poslední aktualizace našich MusicStoreEntities třídy zahrnout DbSets, který vystavit tyto nové třídy modelu, včetně také DbSet&lt;umělcem&gt;. Aktualizované třídy MusicStoreEntities se zobrazí jako níže.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Správa obchodní logiky nákupní košík

V dalším kroku vytvoříme třídu ShoppingCart ve složce modelů. ShoppingCart model zpracovává data přístup k tabulce košíku. Kromě toho zpracuje obchodní logiku pro přidávání a odebírání položek z nákupního košíku.

Vzhledem k tomu, že jsme nechcete, aby tak, aby vyžadovala uživatelům zaregistrovat pro účet, stačí k přidávání položek do jejich nákupní košík, jsme přiřadí uživatelům dočasné jedinečný identifikátor (pomocí identifikátoru GUID nebo globálně jedinečný identifikátor) při přístupu k nákupní košík. Uložíme toto ID pomocí třídy relace ASP.NET.

*Poznámka: Relace ASP.NET je vhodné místo k uložení informace specifické pro uživatele, který vyprší po jejich opustit web. Při zneužití stav relace může mít vliv na výkon na větší lokalit, naše světla používání bude fungovat i pro demonstrační účely.*

Třída ShoppingCart poskytuje následující metody:

**AddToCart** trvá Album jako parametr a přidá ji do košíku uživatele. Vzhledem k tomu, že v tabulce košíku sleduje množství pro každý alb, obsahuje logiku a v případě potřeby vytvořte nový řádek nebo právě zvýšit množství, pokud uživatel již má seřazené jedna kopie alba.

**RemoveFromCart** trvá ID alba a odstraní ji z košíku uživatele. Pokud má uživatel, jenom jedna kopie alba v jejich košíku, odeberou se řádek.

**EmptyCart** odebere všechny položky z nákupní košík uživatele.

**GetCartItems** načte seznam CartItems pro zobrazení nebo zpracování.

**GetCount –** načte celkový počet alb má uživatel v jejich nákupní košík.

**GetTotal** vypočítá celkové náklady na všechny položky v košíku.

**CreateOrder** převede nákupní košík pořadí během fáze checkout.

**GetCart** je statickou metodu, která umožňuje naše řadiče pro získání objektu košíku. Použije **GetCartId** metodu ke zpracování čtení CartId z relace uživatele. Metoda GetCartId vyžaduje hodnotu HttpContextBase tak, aby ho může číst CartId uživatele z relace uživatele.

Tady je kompletní **ShoppingCart třída**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModels

Naše nákupního košíku řadiče muset komunikovat některé komplexní informace k jeho zobrazení, který není řádně namapován na našem objekty modelu. Neradi upravit naše modely tak, aby vyhovovala naše zobrazení; Třídy modelu by měl představovat naše domény, ne uživatelské rozhraní. Jedno řešení by k předání informací o našem zobrazeními pomocí třídy ViewBag, jako jsme to udělali s informace rozevírací správce obchodu, avšak předávání velké množství informací prostřednictvím ViewBag získá obtížné spravovat.

Toto řešení je použití *ViewModel* vzor. Při použití tohoto vzoru vytvoříme silně typované třídy, který jsou optimalizované pro naše zobrazení konkrétní scénáře, a který vystavit vlastnosti pro dynamické hodnoty nebo obsah vyžaduje našich šablon zobrazení. Naše třídy kontroleru můžete naplnit a předat naše zobrazit šablonu použít tyto třídy optimalizované zobrazení. To umožňuje bezpečnost typů, který kontroluje kompilaci a editor IntelliSense v rámci šablon zobrazení.

Vytvoříme dva modely zobrazení pro použití v kontroleru nákupní košík: ShoppingCartViewModel bude obsahovat obsah nákupní košík uživatele a ShoppingCartRemoveViewModel se použije k zobrazení informací potvrzení, když uživatel něco vyjme z jejich košíku.

Umožňuje vytvořit novou složku ViewModels v kořenovém naše projektu k uspořádání informací. Klikněte pravým tlačítkem na projekt, vyberte možnost Přidat nebo novou složku.

![](mvc-music-store-part-8/_static/image1.jpg)

Název složky ViewModels.

![](mvc-music-store-part-8/_static/image1.png)

V dalším kroku přidejte třídu ShoppingCartViewModel ve složce ViewModels. Má dvě vlastnosti: seznam položek košíku a desetinnou hodnotu pro uložení celková cena pro všechny položky v košíku.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Teď můžete přidáte ShoppingCartRemoveViewModel ke složce ViewModels s následujícími vlastnostmi čtyři.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Řadičem nákupní košík

Řadičem nákupní košík má tři hlavní funkce: přidávání položek do košíku, odebrání položky z košíku a zobrazení položky v košíku. Bude nutné používat tří tříd jsme právě vytvořili: ShoppingCartViewModel, ShoppingCartRemoveViewModel a ShoppingCart. Jako StoreController a StoreManagerController přidáme pole pro instanci MusicStoreEntities.

Přidejte nový řadič nákupního košíku do projektu pomocí šablony prázdný kontroler.

![](mvc-music-store-part-8/_static/image2.png)

Zde je kompletní ShoppingCart řadiče. Index a přidat kontroler akce velmi povědomé. Akce kontroleru odebrat a CartSummary zpracovávat dva speciální případů, které probereme v následující části.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Aktualizace AJAX s jQuery

Dále vytvoříme nákupního košíku Index stránky, který je pro ShoppingCartViewModel silného typu a, použije se stejným způsobem jako před šablona zobrazení seznamu.

![](mvc-music-store-part-8/_static/image3.png)

Však místo použití Html.ActionLink odebrat položky z košíku, použijeme jQuery k "připojit až" klikněte na událost pro všechna propojení v tomto zobrazení, které mají třídu HTML při odebírání odkazu. Místo publikování formuláře, této obslužné rutiny události klikněte na právě budou zpětného volání AJAX do našich RemoveFromCart akce kontroleru. RemoveFromCart vrací výsledek serializován do formátu JSON, který pak analyzuje a provede čtyři rychlé aktualizace na stránku pomocí jQuery naše jQuery zpětného volání:

- 1. Odebere odstraněné album ze seznamu
- 2. Aktualizace počet košíku v hlavičce
- 3. Zobrazí aktualizace zprávu pro uživatele
- 4. Celková cena košíku aktualizací

Vzhledem k tomu, že zpětného volání Ajax v rámci zobrazení indexu je zpracovanou scénář odebrat, není potřebujeme další zobrazení RemoveFromCart akce. Tady je kompletní kód /ShoppingCart/Index zobrazení:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Chcete-li otestovat to zjistit, potřebujeme mít k přidávání položek do naše řešení nákupního košíku. Budete aktualizujeme naše **podrobnosti úložiště** aby zobrazení zahrnovalo tlačítko "Přidat do košíku". Když jsme se na to, jsme jsou některé z alba Další informace, které jsme přidali vzhledem k tomu, že jsme v tomto zobrazení poslední aktualizace: Genre, umělcem, ceny a alba. Kód zobrazení aktualizované podrobnosti úložiště se zobrazí, jak je uvedeno níže.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Nyní jsme klikněte na tlačítko prostřednictvím úložiště a otestovat přidávání a odebírání alb do a z naše řešení nákupního košíku. Spusťte aplikaci a přejděte do Index úložiště.

![](mvc-music-store-part-8/_static/image4.png)

Klikněte na tlačítko na Genre, chcete-li zobrazit seznam alb.

![](mvc-music-store-part-8/_static/image5.png)

Kliknutím na název alba nyní zobrazuje naše aktualizované zobrazení podrobností alb, včetně tlačítko "Přidat do košíku".

![](mvc-music-store-part-8/_static/image6.png)

Kliknutím na tlačítko "Přidat do košíku" ukazuje naše nákupní košík Index zobrazení seznamu souhrn nákupní košík.

![](mvc-music-store-part-8/_static/image7.png)

Po načtení do nákupního košíku, můžete kliknutím na odebrat z košíku odkaz zobrazíte aktualizace Ajax do nákupního košíku.

![](mvc-music-store-part-8/_static/image8.png)

Sestavili jsme si funkční nákupní košík, což umožňuje uživatelům zrušit registraci k přidávání položek do jejich košíku. V následující části jsme budete mohly k registraci a dokončete proces checkout.


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-7.md)
> [další](mvc-music-store-part-9.md)
