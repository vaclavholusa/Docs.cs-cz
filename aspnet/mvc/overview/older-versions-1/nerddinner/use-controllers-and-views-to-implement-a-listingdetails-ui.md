---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: "Implementace výpis/podrobnosti uživatelského rozhraní pomocí řadiče a zobrazení | Microsoft Docs"
author: microsoft
description: "Krok 4 ukazuje, jak přidat řadič k aplikaci, která využívá našeho modelu uživatelům prostředí navigační výpis/podrobnosti dat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 2f9148a2d419863229e2c5a2a0c98984001fcee5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Implementace výpis/podrobnosti uživatelského rozhraní pomocí řadiče a zobrazení
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Toto je krok 4 bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.
> 
> Krok 4 ukazuje, jak přidat řadič k aplikaci, která využívá našeho modelu poskytnout uživatelům dat prostředí navigační výpis/podrobnosti pro večeří na našem NerdDinner serveru.
> 
> Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner krok 4: Kontrolery a zobrazení

Tradiční webové rozhraní (klasické ASP, PHP, webových formulářů ASP.NET atd.) jsou příchozí adresy URL obvykle mapovány na soubory na disku. Příklad: žádost o adresu URL, například "/ Products.aspx" nebo "/ Products.php" může zpracovat soubor "Products.aspx" nebo "Products.php".

Webové rozhraní MVC mapování adres URL na serverový kód mírně jiným způsobem. Místo mapování příchozí adresy URL k souborům, mapují místo adresy URL pro metody na třídách. Tyto třídy, se nazývají "Řadiče" a jsou zodpovědná za zpracování příchozí požadavky protokolu HTTP a zpracování uživatelského vstupu, načítání a ukládání dat a určení odpověď k odeslání zpět do klienta (zobrazení HTML, stažení souboru, přesměrování na jiný Adresa URL atd).

Teď, když jsme jste vytvořili základní model pro naši aplikaci NerdDinner, naše dalším krokem bude přidáte řadič k aplikaci, která využívá ji, aby poskytovala uživatelé dat prostředí navigační výpis/podrobnosti pro večeří na svém webovém serveru.

### <a name="adding-a-dinnerscontroller-controller"></a>Přidávání DinnersController řadiče

Jsme budete začněte tím, že pravým tlačítkem na složku "Řadiče" v rámci naší webový projekt a pak vyberte **Add -&gt;řadič** příkazu nabídky (můžete také spustit tento příkaz zadáním Ctrl-M, Ctrl-C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Otevře se dialog "Přidat kontroler":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Jsme budete název nového řadiče "DinnersController" a klikněte na tlačítko "Přidat". Visual Studio pak přidá soubor DinnersController.cs v našem \Controllers adresáři:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Otevře se také si novou třídu DinnersController v editoru kódu.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Přidávání do třídy DinnersController Index() a Details() metody akce

Chceme, aby návštěvníkům používajícím naše aplikace procházet seznam nadcházející večeří a umožňuje jim klikněte na všechny večeři v seznamu zobrazíte konkrétní podrobnosti o něm. Provedeme to tím, že publikujete následující adresy URL z našich aplikace:

| **ADRESA URL** | **Účel** |
| --- | --- |
| */Dinners/* | Zobrazení seznamu HTML z nadcházející večeří |
| */Dinners/podrobnosti / [id]* | Zobrazit podrobnosti o konkrétní večeři indikován parametrem "id" vkládán adresu URL –, která bude odpovídat DinnerID večeři v databázi. Příklad: /Dinners/Details/2 by zobrazit stránku HTML s podrobnostmi o večeři, jehož hodnota DinnerID je 2. |

Přidáním dvě veřejné "metody akce" do našich DinnersController třídy, jako třeba níže budeme publikovat počáteční implementace tyto adresy URL:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Jsme budete pak spusťte aplikaci NerdDinner a pomocí našich prohlížeče je vyvolat. Zadáním v *"/ večeří /"* způsobí, že adresa URL naše *Index()* má metoda spustit a odešle zpět odpověď na následující:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Zadáním v *"/ večeří/podrobnosti/2"* způsobí, že adresa URL naše *Details()* metoda ke spuštění a odeslání zpět následující odpověď:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Možná se ptáte – jak rozhraní ASP.NET MVC věděli vytvoření Naše třída DinnersController a volat tyto metody? Zjistit, který umožňuje rychle zobrazit v tom, jak směrování funguje.

### <a name="understanding-aspnet-mvc-routing"></a>Vysvětlení rozhraní ASP.NET MVC směrování

ASP.NET MVC zahrnuje modul Směrování výkonné adresu URL, který poskytuje značnou flexibilitu při řízení, jak jsou adresy URL mapovat na třídy kontroleru. Umožňuje přizpůsobit úplně jak rozhraní ASP.NET MVC zvolí které třídy kontroleru k vytvoření, která metoda k vyvolání na něm a také nakonfigurovat různými způsoby, proměnné můžete být automaticky analyzovat z adresy URL řetězce dotazu a předaný metodě jako parametr. argumenty. Poskytuje možnost zcela optimalizovat lokalitu pro SEO (optimalizaci pro vyhledávací weby) a také publikovat všechny struktury adres URL, kterou chceme z aplikace.

Ve výchozím nastavení nové projekty ASP.NET MVC pocházet předkonfigurované sadu pravidel směrování adres URL již zaregistrován. To umožňuje nám snadno začít pracovat na aplikaci bez nutnosti explicitně nakonfigurovat nic. Pravidla registrace směrování výchozí jsou k dispozici v rámci třídy "Aplikace" naše projekty – které jsme můžete otevřít dvojitým kliknutím na soubor "Global.asax" v kořenovém naše projektu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Výchozí pravidla směrování ASP.NET MVC jsou zaregistrovány v rámci metody "RegisterRoutes" této třídy:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"Trasy. MapRoute() "volání metody výše zaregistruje výchozí směrování pravidlo, které mapuje příchozí adresy URL do třídy kontroleru formátu adresy URL:" / {controller} / {action} / {id} "–"controller"je název třídy controller k vytváření instancí,"action"je název veřejná metoda k vyvolání na a "id" je volitelný parametr vkládán adresu URL, kterou lze předat jako argument metodu. Třetí parametr předaný volání metody "MapRoute()" je sada výchozích hodnot pro použití pro kontroler nebo akce nebo id hodnoty v případě, že se nenachází v adrese URL (řadiče = "Domů", akce = "Index", Id = "").

Níže uvádíme tabulku, která ukazuje, jak různé adresy URL jsou mapovány pomocí výchozího "*/ {řadiče} / {action} / {id}"*pravidlo trasy:

| **ADRESA URL** | **Třída kontroleru** | **Metody akce** | **Parametry předané** |
| --- | --- | --- | --- |
| */ Večeří/podrobnosti/2* | DinnersController | Details(ID) | ID = 2 |
| */ Večeří/Edit/5* | DinnersController | Edit(ID) | ID = 5 |
| */ Večeří nebo vytvořit* | DinnersController | Create() | Není k dispozici |
| */ Večeří* | DinnersController | Index() | Není k dispozici |
| *Domů* | HomeController | Index() | Není k dispozici |
| */* | HomeController | Index() | Není k dispozici |

Poslední tři řádky zobrazit výchozí hodnoty (řadiče = Domů, akce = Index, Id = "") používá. Protože metodu "Index" je registrován jako výchozí název akce, pokud není zadaná, "/ večeří" a "/ Domů" příčina adresy URL Index() metoda akce má být volána v jejich třídy Kontroleru. Protože řadičem "Domů" je zaregistrován jako výchozí řadič, pokud není zadaná, "/" adresa URL způsobí, že HomeController, který se má vytvořit a metody akce Index() na něm být volána.

Pokud chcete tyto výchozí pravidel směrování adres URL, je dobrá zpráva, aby byly snadno změnit – je upravovat v rámci výše RegisterRoutes metoda. Pro naši aplikaci NerdDinner ale jsme nebudete změnit výchozí pravidla směrování URL – místo právě použijeme je jako-je.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Pomocí DinnerRepository z našich DinnersController

Pojďme nyní nahraďte naše aktuální implementace DinnersController Index() a Details() metody akce s implementací, které můžete pomocí našeho modelu.

Použijeme třídě DinnerRepository jsme vytvořili dříve k implementaci chování. Jsme budete začít přidáním "pomocí" příkaz, který odkazuje na obor názvů "NerdDinner.Models" a pak na naše třída DinnerController deklarovat instanci naše DinnerRepository jako pole.

Dále v této kapitole jsme budete zavést koncept "Vkládání závislostí" a zobrazit jiný způsob pro naše řadiče získat odkaz na DinnerRepository, která umožňuje lepší jednotky testování – ale vpravo teď právě vytvoříme instanci naše DinnerRepository vložené jako níže.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Nyní jsme připraveni ke generování odpovědi HTML zpět pomocí našich objekty načtená data modelu.

### <a name="using-views-with-our-controller"></a>Pomocí zobrazení Kontroleru

Když je možné k zápisu kódu v našem metody akce sestavte HTML a potom pomocí *Response.Write()* Pomocná metoda do něj poslat zpět klientovi, že přístup se poměrně nepraktické rychle. Mnohem lepší přístup je pro nás umožní provádět jenom aplikace a data logiku uvnitř naše DinnersController metody akce a poté předat data potřebná k vykreslení odpovědi HTML samostatné "Zobrazit" šablonu, která je zodpovědná za výstup vytvořeného reprezentace HTML to. Jak jsme zobrazí ve chvíli, šablona "Zobrazit" je textový soubor, který obvykle obsahuje kombinaci kód HTML a kód vložený vykreslování.

Oddělení řadiče logiku z našich vykreslování zobrazení přináší několik velkých výhod. Zejména pomáhá vynutit jasně "oddělené oblasti zájmu" mezi kódu aplikace a formátování/vykreslování kód uživatelského rozhraní. Díky tomu mnohem snazší pro testování aplikace logiky izolovaně od vykreslování logika uživatelského rozhraní. Ho usnadňuje později upravit šablony vykreslování uživatelské rozhraní bez nutnosti provádět změny kódu aplikace. A ho můžete usnadnit vývojářů a návrhářů, spolupracovat společně na projekty.

Aktualizujeme Naše třída DinnersController indikující, že má být použít šablonu zobrazení má být zaslán zpět odpověď uživatelského rozhraní HTML změnou metoda signatur naše metody dvě akce s návratovým typem "void" místo toho má návratový typ "ActionResult". Můžete pak volat *View()* metodu helper na základní třídy Kontroleru vrátit zpět objekt "ViewResult" jako níže:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

Podpis *View()* Pomocná metoda používáme výše vypadá níže:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

První parametr *View()* Pomocná metoda představuje název souboru šablony zobrazení má být použita k vykreslení odpovědi HTML. Druhý parametr je objekt modelu, který obsahuje data, která je šablona zobrazení k vykreslení odpovědi HTML.

V rámci naší metoda akce Index() voláme *View()* Pomocná metoda a indikující, že má k vykreslení HTML seznam večeří pomocí šablony "Index" zobrazení. Zobrazit šablonu jsme posloupnost večeři objekty, které chcete-li vytvořit seznam z předali:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

V rámci naší metoda akce Details() jsme pokusí získat objekt večeři pomocí zadané id v rámci adresy URL. Pokud se najde platný večeři říkáme *View()* Pomocná metoda, označující chceme použijte šablonu "Podrobnosti" zobrazení k vykreslení načtené večeři objektu. Pokud se vyžaduje neplatný večeři, jsme vykreslení užitečné chybovou zprávu, která označuje, že večeře neexistuje, pomocí zobrazení šablony "NotFound" (a přetížené verzi *View()* Pomocná metoda, která stačí pouze název šablony ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Teď umožňuje implementovat zobrazit šablony "NotFound", "Podrobnosti" a "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementace zobrazit šablonu "NotFound"

Začneme budete implementací zobrazit šablonu "NotFound" – které zobrazí popisný chyba zpráva označující, že nebyl nalezen požadovaný večeři.

Jsme budete vytvořte novou šablonu zobrazení tak, že umístíte naše kurzoru text v rámci metodu akce kontroleru a potom klikněte pravým tlačítkem a vyberte příkaz nabídky "Přidat zobrazení" (můžeme také spouštět tento příkaz zadáním Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Tím zobrazíte dialog "Přidat zobrazení" jako níže. Ve výchozím nastavení dialogové okno bude předem zadat název zobrazení pro vytvoření odpovídal názvu metody akce kurzor byla, když byla spuštěná dialogové okno (v tomto případě "Informace"). Vzhledem k tomu, že chceme první implementaci šabloně "NotFound", jsme budete tento název zobrazení přepsání a nastavte ji na místo toho se "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Když kliknete na tlačítko "Přidat", Visual Studio vytvoří novou šablonu zobrazení "NotFound.aspx" pro nás v adresáři "\Views\Dinners" (což také se vytvoří, pokud ještě neexistuje adresáři):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Otevře se také si naše novou šablonu zobrazení "NotFound.aspx" v editoru kódu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Zobrazení šablony ve výchozím nastavení mají dvě "oblasti obsahu" kde nyní můžete přidat obsah a kód. První umožňuje přizpůsobit "title" stránky HTML odeslána zpět. Druhá umožňuje přizpůsobit "hlavní obsah" stránky HTML odeslána zpět.

K implementaci naše "NotFound" Zobrazit šablonu přidáme některé základní obsahu:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Jsme mohou zkuste to v prohlížeči. K tomu můžeme požadavek *"/ večeří/podrobnosti/9999"* adresy URL. To bude odkazovat na večeři, který aktuálně neexistuje v databázi a způsobí, že naše DinnersController.Details() metody akce k vykreslení zobrazení šablony naše "NotFound":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Jednou z věcí, které můžete si všimnout v snímek obrazovky výše je, že naše základní zobrazit šablonu dědí spoustu HTML obklopující hlavní obsah na obrazovce. Je to proto, že naše zobrazit šablonu používá šablonu "stránka předlohy", která umožňuje nám pro aplikaci konzistentního rozložení ve všech zobrazení v lokalitě. Probereme, jak fungují v pozdější části tohoto kurzu další stránky předlohy.

### <a name="implementing-the-details-view-template"></a>Implementace "Podrobnosti" Zobrazit šablonu

Teď umožňuje implementovat zobrazit šablonu "Podrobnosti" – což se generují kód HTML pro jeden model večeři.

Jsme budete tomu umístění kurzoru naše text v rámci podrobnosti metody akce a potom klikněte pravým tlačítkem a zvolte příkaz "Přidat zobrazení" nabídky (nebo stiskněte klávesu Ctrl-M, Ctrl-V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Otevře se dialog "Přidat zobrazení". Jsme budete ponechat výchozí název zobrazení ("informace"). Také jsme budete v dialogovém okně zaškrtněte políčko "Vytvořit zobrazení silného typu" a vyberte (pomocí rozevíracího seznamu combobox) název typu modelu, který jsme předali z řadiče do zobrazení. Pro toto zobrazení jsme předali objekt večeři (plně kvalifikovaný název pro tento typ je: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Na rozdíl od předchozích šablony, které jsme zvolili vytvoření "Prázdné zobrazení", tentokrát, které jsme bude automaticky rozhodnete "vygenerovat" zobrazení pomocí šablony "Podrobnosti". Jsme to znamenat změnou "Zobrazení obsahu" rozevíracího seznamu v dialogovém okně výše.

"Generováním" vygeneruje počáteční provádění naše podrobnosti zobrazení šablony založené na večeři objekt, který jsme se předávání do ní. To poskytuje snadný způsob, abychom mohli rychle začít na naše implementace šablony zobrazení.

Když kliknete na tlačítko "Přidat", Visual Studio vytvoří nový soubor šablony "Details.aspx" zobrazení pro nás v adresáři naše "\Views\Dinners":

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Otevře se také si naše novou šablonu zobrazení "Details.aspx" v editoru kódu. Bude obsahovat implementace počáteční vygenerované uživatelské rozhraní na základě modelu večeři zobrazení podrobností. Modul generování uživatelského rozhraní pomocí reflexe .NET podívejte se na veřejné vlastnosti, které jsou zveřejněné na třídě je předán a přidá příslušný obsah na základě jednotlivých typu, které najde:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Můžeme požádat o *"/ večeří/podrobnosti/1"* adresu URL, které chcete zobrazit, jak tato implementace "Podrobnosti" vygenerované uživatelské rozhraní vypadá v prohlížeči. Pomocí této adresy URL se zobrazí jednu z večeří, kterou jsme ručně přidali do databáze hotový když jsme nejdřív vytvořili:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

To nám získá rychle vytvořit a spustit a poskytuje nám počáteční provádění naše zobrazením Details.aspx. Pak můžeme přejděte a upravit ho pro přizpůsobení uživatelského rozhraní pro naše spokojenost.

Když jsme přesněji podíváte na šabloně Details.aspx, jsme tu, aby ji obsahuje statické HTML a také vložený kód pro vykreslování. &lt;%%&gt; útržky kódu spuštění kódu při vykreslí zobrazení šablony, a &lt;% = %&gt; útržky kódu spouštění kódu vytvořeného v nich obsažená a následnému vykreslení výsledek šablony do výstupního datového proudu.

V rámci naší zobrazení, který přistupuje k objekt modelu "Večeři", který byl předán z našich řadiče pomocí vlastnosti silného typu "Modelu" jsme můžete napsat kód. Visual Studio poskytuje nám s úplnou kódu intellisense při přístupu k této vlastnosti "Modelu" v editoru:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Pojďme zajistit několik vylepšení, takže zdroj pro naše poslední zobrazení šablony podrobnosti vypadá níže:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Když jsme získat přístup *"/ večeří/podrobnosti/1"* bude adresa URL znovu ho teď vykreslení jako níže:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementace "Index" Zobrazit šablonu

Teď umožňuje implementovat zobrazit šablonu "Index" – které vygeneruje seznam nadcházející večeří. Úkolů to jsme budete pozice kurzoru naše text v rámci metody akce indexu a pak pravým klikněte na tlačítko a zvolte příkaz "Přidat zobrazení" nabídky (nebo stiskněte klávesu Ctrl-M, Ctrl-V).

V dialogovém okně "Přidat zobrazení" jsme budete zachovat zobrazit šablonu s názvem "Index" a zaškrtněte políčko "Vytvořit zobrazení silného typu". Tentokrát vybereme možnost automaticky vygenerovat zobrazit šablonu "seznam" a vyberte "NerdDinner.Models.Dinner" jako typ modelu předaná do zobrazení (který vzhledem k tomu, že jsme označili vytváříme "Seznam" vygenerované uživatelské rozhraní způsobí, že předpokládat, že jsme se dialogové okno Přidat zobrazení předání posloupnost večeři objekty z našich řadiče zobrazení):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Když kliknete na tlačítko "Přidat", Visual Studio vytvoří nový soubor šablony "Index.aspx" zobrazení pro nám v adresáři naše "\Views\Dinners". Ho bude "vygenerovat" počáteční implementace v něm, která poskytuje HTML tabulky seznam večeří jsme předat do zobrazení.

Když jsme spouštět aplikace a přístup *"/ večeří /"* adresu URL, které se budou vykreslovat seznam večeří takto:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Výše uvedené tabulce řešení nám dává rozložení mřížky naše večeři dat – to není dost co chceme pro naše příjemce čelí večeři výpis. Můžeme aktualizovat šablonu zobrazení Index.aspx a upravit tak, aby seznam méně sloupců dat a použít &lt;ul&gt; elementu, který chcete vykreslit je namísto tabulky s použitím následující kód:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Klíčové slovo "var" v rámci výše uvedeného příkazu foreach se používá jako jsme smyčku každý večeři v našem modelu. Ty obeznámeni s C# 3.0 může myslíte, že použití "var" znamená, že se objekt večeři pozdní vazbu. Místo toho znamená, že kompilátor používá odvození typu pro vlastnost silného typu "Modelu" (která je typu "IEnumerable&lt;večeři&gt;") a jako typ večeři – to znamená nám získat úplný kompilace proměnnou místní "večeři" IntelliSense a kompilaci vyhledává v rámci bloků kódu:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Když jsme dosáhl aktualizace */Dinners* adresu URL do prohlížeče naše teď vypadá aktualizované zobrazení níže:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

To je vyhledávání lepší – ale ještě není zcela existuje. Naše posledním krokem je povolit koncovým uživatelům, aby kliknutím na jednotlivé večeří v seznamu a zobrazit podrobnosti o nich. To jsme budete implementovat pomocí vykreslení elementů HTML hypertextový odkaz, které odkazují na metodu akce podrobnosti na našem DinnersController.

Jsme můžete vytvořit tyto hypertextové odkazy v rámci naší zobrazení indexu v jednom ze dvou způsobů. První je ruční vytvoření HTML &lt;&gt; elementy jako níže, kde jsme vložení &lt;%%&gt; blokuje v rámci &lt;&gt; prvek HTML:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Můžeme použít alternativní způsob je využít předdefinovanou metodu helper "Html.ActionLink()" v rámci rozhraní ASP.NET MVC, která podporuje programové vytvoření HTML &lt;&gt; element, který odkazuje na jinou metodu akce na Řadič:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

První parametr pomocné metody Html.ActionLink() je text odkazu pro zobrazení (v tomto případě název systému večeře), druhý parametr je název akce Kontroleru chceme generovat odkaz (v tomto případě metodu podrobnosti) a třetí parametr není sadu parametrů k odeslání do akce (implementovaný jako anonymní typ s názvem/hodnoty vlastností). V takovém případě zadáváme parametr "id" večeři jsme chcete propojit a protože směrování adres URL výchozí pravidlo v architektuře ASP.NET MVC je "{Controller} / {Action} / {id}" pomocnou metodu Html.ActionLink() vygeneruje následující výstup:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Pro naše Index.aspx zobrazení jsme budete, použijte postup Html.ActionLink() Pomocná metoda a mají každý večeři v seznamu odkazu na adresu URL, příslušné podrobnosti:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

A teď když jsme klikněte na tlačítko */Dinners* URL seznamu večeři vypadá níže:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Když kliknete na některé z večeří v seznamu jsme budete přejděte zobrazíte její podrobnosti:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Na základě konvence pojmenování a strukturu adresáře \Views

Ve výchozím nastavení aplikace ASP.NET MVC používat adresář založené na konvenci pojmenování struktura při rozpoznávání šablon zobrazení. To umožňuje vývojářům nemuseli plně-kvalifikovaný cestu k umístění při odkazování na zobrazení z v rámci třídy Kontroleru. Ve výchozím nastavení rozhraní ASP.NET MVC bude hledat soubor šablony zobrazení v rámci * \Views\[ControllerName]\* adresář pod aplikace.

Například jste byla pracujeme na třídu DinnersController – které výslovně odkazuje na tři šablony zobrazení: "Index", "Podrobnosti" a "NotFound". ASP.NET MVC ve výchozím nastavení vyhledá tato zobrazení v rámci *\Views\Dinners* adresář pod naše kořenový adresář aplikace:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Všimněte si výše jak tam jsou aktuálně tři řadiče tříd v rámci projektu (DinnersController, HomeController a AccountController – poslední dva byly přidány ve výchozím nastavení, když jsme vytvořili projekt), a existují tři podadresářích (jeden pro každou řadič) v adresáři \Views.

Zobrazení na něj odkazovat z řadičů Home a účty budou automaticky vyřešit jejich zobrazení šablon z příslušné *\Views\Home* a *\Views\Account* adresáře. *\Views\Shared* podadresář poskytuje způsob, jak uložit zobrazit šablony, které jsou znovu použít napříč několika řadičů v aplikaci. Když ASP.NET MVC se pokusí přeložit šablonu zobrazení, bude nejprve zkontrolujte v rámci *\Views\[řadiče]* konkrétního adresáře, a pokud nemůže najít zobrazit šablonu existuje bude vypadat v rámci *\Views\ Sdílené* adresáře.

Pokud jde o názvy jednotlivých zobrazit šablony, je doporučené pokyny zobrazit šablonu sdílet stejný název jako metodu akce, který způsobuje její neočekávané k vykreslení. Například výše naše "Index" je metoda akce pomocí "Index" zobrazení k vykreslení zobrazení výsledků a metoda akce "Podrobnosti" je pomocí "Podrobnosti" zobrazení k vykreslení své výsledky. To usnadňuje rychle zobrazit šablonu, která souvisí s každou akci.

Vývojáři, není potřeba explicitně zadejte název šablony zobrazení, když šablona zobrazení má stejný název jako akce metody volané na řadiči. Můžete místo toho stačí předat objekt modelu "View()" pomocné metody (bez zadání názvu zobrazení) a ASP.NET MVC automaticky odvodí, že má být použít *\Views\[ControllerName]\[název akce]* zobrazit šablonu na disku pro vykreslení.

To umožňuje nám trochu vyčištění naše kódu kontroleru a vyhnout se duplikování název dvakrát v našem kódu:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Ve výše uvedeném kódu je, že všechny, které je potřebné pro provedení dobrý výpis večeři/podrobnosti prostředí pro tuto lokalitu.

#### <a name="next-step"></a>Další krok

Nyní je k dispozici dobrý večeři procházení prostředí sestaven.

Teď umožňuje povolit úprav podporu CRUD (vytvořit, číst, Update, Delete) dat formuláře.

>[!div class="step-by-step"]
[Předchozí](build-a-model-with-business-rule-validations.md)
[další](provide-crud-create-read-update-delete-data-form-entry-support.md)
