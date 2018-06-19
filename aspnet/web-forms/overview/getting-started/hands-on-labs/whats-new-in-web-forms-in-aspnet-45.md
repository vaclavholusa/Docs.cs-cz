---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Co je nového ve webových formulářů technologie ASP.NET 4.5 | Microsoft Docs
author: rick-anderson
description: Nové verze aplikace webových formulářů ASP.NET zavádí několik vylepšení funkce zaměřené na vylepšení činnost koncového uživatele při práci s daty. V předchozích verzích...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: e230faac0dc81b67d74945dc98eee80f83205f65
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306777"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Co je nového ve webových formulářů technologie ASP.NET 4.5
====================
Podle [webové táborech Team](https://twitter.com/webcamps)

> Nové verze aplikace webových formulářů ASP.NET zavádí několik vylepšení funkce zaměřené na vylepšení činnost koncového uživatele při práci s daty.
> 
> V předchozích verzích webových formulářů, při použití datových vazeb pro vydávání hodnotu členem objektu používá výrazy datové vazby Bind() nebo Eval(). V nové verzi technologie ASP.NET budete moci deklarovat, jaký typ dat ovládacího prvku přechází svázat s použitím novou vlastnost typ položky. Nastavení této vlastnosti vám umožní používat proměnné silného typu pro příjem všech výhod vývojové prostředí sady Visual Studio, například IntelliSense, navigace člen a kompilaci kontrolu.
> 
> S ovládací prvky vázané na data nyní můžete také určit vlastní vlastních metod pro výběr, aktualizaci, odstranění a vkládání dat, ke zjednodušení interakce mezi ovládací prvky stránky a aplikace logiky. Kromě toho funkce vazby modelu byly přidány do ASP.NET, což znamená, že data ze stránky můžete namapovat přímo do typu parametry metody.
> 
> Ověřování uživatelského vstupu musí být také snazší s nejnovější verzí webových formulářů. Teď může opatřit poznámkami tříd modelu s atributy ověření ze **System.ComponentModel.DataAnnotations** obor názvů a požadavek, který váš web ovládací prvky ověřit vstup uživatele s použitím těchto informací. Ověřování na straně klienta v webových formulářů je nyní integrována jQuery, obsahuje čisticí kódu na straně klienta a funkce, nerušivý JavaScript.
> 
> V oblasti žádosti o ověření vylepšily aby bylo snazší selektivně vypnutí ověření žádosti pro konkrétní části aplikací nebo číst data zneplatněné požadavku.
> 
> Některé vylepšení byla provedena pro webové formuláře ovládací prvky serveru, abyste mohli využívat nové funkce HTML5:
> 
> - V textovém režimu vlastnost ovládacího prvku TextBox aktualizoval na podporu nových typů HTML5 vstupní jako e-mailu, data a času a tak dále.
> - Odesílání souborů při odpovědích řízení nyní podporuje více nahrávání souborů z prohlížečů, které podporují tuto funkci HTML5.
> - Validátor řídí teď podporu ověřování HTML5 elementy vstupu.
> - Nové prvky HTML5, které mají atributy, které představují adresu URL nyní podporují runat =&quot;server&quot;. V důsledku toho můžete použít ASP.NET konvence v URL cesty, jako je třeba ~ operátor představují kořenový adresář aplikace (například &lt;video runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - Ovládací prvek UpdatePanel byl opraven pro podporu příspěvků HTML5 vstupních polí.
> 
> Na portálu oficiální ASP.NET můžete najít další příklady nových funkcí v technologii ASP.NET 4.5 WebForms: [co je nového v technologii ASP.NET 4.5 a Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Použití datových vazeb výrazů silného typu
- Používat nové funkce vazby modelu v webových formulářů
- Použití zprostředkovatele hodnot pro mapování stránky data na metody kódu
- Pomocí datových poznámek pro ověřování uživatelského vstupu
- Proveďte advange ověřování na straně klienta unobstrusive s jQuery webových formulářů
- Implementace granulární žádosti o ověření
- Implementace asynchronního zpracování ve formulářích webové stránky

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).

<a id="Setup"></a>
### <a name="setup"></a>Instalace

**Instalace fragmenty kódu**

Pro usnadnění práce většinu kódu, který bude spravovat podél toto testovací prostředí je k dispozici jako fragmenty kódu v sadě Visual Studio. K instalaci fragmenty kódu spustit **.\Source\Setup\CodeSnippets.vsi** souboru.

Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické cvičení zahrnuje následující cvičení:

1. [Cvičení 1: Model vazby v webové formuláře ASP.NET](#Exercise1)
2. [Cvičení 2: Ověřování dat](#Exercise2)
3. [Cvičení 3: Asynchronní stránky zpracování v rozhraní ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Cvičení 1: Model vazby v webové formuláře ASP.NET

Nové verze aplikace webových formulářů ASP.NET zavádí řadu vylepšení, které jsou zaměřené na vylepšení zkušeností při práci s daty. Během tohoto cvičení bude další informace o ovládacích prvcích silného typu dat a model vazby.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Úloha 1 – pomocí dat vazeb silného typu

V této úloze bude zjišťovat nové silného typu vazby k dispozici v technologii ASP.NET 4.5.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex1-objektuModelBinding/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **Customers.aspx** stránky. Umístěte nečíslovaná seznamu v ovládacím prvku hlavní a zahrnují ovládacího prvku opakovače uvnitř pro výpis každého zákazníka. Nastavte název opakovače na **customersRepeater** jak je znázorněno v následujícím kódu.

    V předchozích verzích webových formulářů při použití datových vazeb pro vydávání hodnota člena v objektu jste vazby dat, byste použili výraz vazby dat, společně s volání metody Eval předávání názvu člena jako řetězec.

    Za běhu budou tyto volání Eval pomocí reflexe proti aktuálně vázaný objekt načíst hodnotu člena s daným názvem a zobrazit výsledek v kódu HTML. Tuto metodu lze velmi snadno k vazbě dat na libovolný, unshaped data.

    Bohužel ztratíte mnoho funkcí optimálního uživatelského prostředí době vývoje v sadě Visual Studio, včetně technologie IntelliSense pro názvy členů, podporu pro navigaci (např. přejít k definici) a kontrola kompilaci.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Otevřete **Customers.aspx.cs** souboru.
4. Přidejte následující příkaz using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. V **stránky\_zatížení** metoda, přidejte kód k vyplnění opakovače s do seznamu zákazníků.

    (Code fragment kódu - *webové zdroje dat formuláře laboratoř - Ex01 - vazby zákazníků*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Toto řešení využívá EntityFramework společně s CodeFirst k vytváření a přístup k databázi. V následujícím kódu je vázána customersRepeater materializovaného dotazu, který vrací všechny zákazníky z databáze.
6. Stiskněte klávesu **F5** ke spuštění řešení a přejděte na **zákazníci** stránky zobrazíte opakovače v akci. Jako řešení používá CodeFirst, databáze bude vytvořen a vložené do místní instance SQL Express, při spuštění aplikace.

    ![Výpis zákazníkům prvku repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "výpis zákazníkům prvku repeater")

    *Výpis zákazníkům prvku repeater*

    > [!NOTE]
    > V sadě Visual Studio 2012 služba IIS Express je výchozí webový vývojový server.
7. Zavřete prohlížeč a přejděte zpátky na Visual Studio.
8. Nyní nahraďte implementace používat silného typu vazby. Otevřete **Customers.aspx** stránky a použít novou **ItemType** atribut v opakovači nastavit **zákazníka** typ jako typ vazby.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Vlastnost ItemType umožňuje deklarovat, jaký typ dat ovládacího prvku se blíží vázat a umožňuje používat silného typu vazby uvnitř ovládacího prvku vázané na data.
9. Nahraďte ItemTemplate obsahu s následujícím kódem.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Jeden nevýhodou s výše uvedených přístupů je, že volání Eval() a Bind() pozdní vazbou – což znamená, že předáváte řetězce představující názvy vlastností. To znamená, že neobdržíte Intellisense pro názvy členů, podporu pro navigaci v kódu (například přejít k definici) ani kontrola podporu kompilaci.

    Nastavení vlastnosti ItemType způsobí, že dvě nové typové proměnné, které má být vygenerován v oboru datové vazby výrazů: **položky** a **BindItem**. Můžete použít tyto silného typu proměnné ve výrazech datové vazby a plně využít vývojového prostředí sady Visual Studio.

    &quot; **:** &quot; Použít ve výrazu automaticky použije kódování HTML výstupu, abyste předešli problémům s zabezpečení (například mezi útoky skriptování). Tento zápis byl k dispozici od verze .NET 4 pro psaní odpovědi, ale nyní je k dispozici také ve výrazech datové vazby.

    > [!NOTE]
    > Člen položky funguje pro jednosměrné vazby. Pokud chcete provést pomocí obousměrné vazby **BindItem** člen.

    ![Podporu technologie IntelliSense v silného typu vazby](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "podporu technologie IntelliSense v silného typu vazby")

    *Podporu technologie IntelliSense v silného typu vazby*
10. Stiskněte klávesu **F5** ke spuštění řešení a přejděte na stránku zákazníků a zajistěte, aby se změny fungovat podle očekávání.

    ![Výpis podrobnosti o odběrateli](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "výpis podrobnosti o odběrateli")

    *Podrobnosti o odběrateli výpis*
11. Zavřete prohlížeč a přejděte zpátky na Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Úloha 2 - představení Model vazby ve webové formuláře

V předchozích verzích webových formulářů ASP.NET Pokud jste chtěli provést obousměrné vazby dat, načítání i aktualizace dat, je potřeba k použití objekt zdroje dat. To může být zdroj dat objektu, zdroj dat SQL, zdroj dat LINQ a tak dále. Pokud váš scénář vyžaduje vlastní kód pro zpracování dat, ale je potřeba k použití zdroj dat objektu a to nebude některé nevýhody. Například jste museli vyhnuli používání komplexních typů a potřeby zpracování výjimek při provádění logiku ověření.

Ovládací prvky vázané na data v nové verzi webových formulářů ASP.NET podporují vazby modelu. To znamená, že můžete zadat vyberte, aktualizovat, Vložit a odstranit metody přímo v ovládacím prvku vázané na data volat logiku z vašeho kódu souboru nebo z jiné třídy.

Další informace o to, budete používat GridView seznam kategorií produktů pomocí nové **metody SelectMethod** atribut. Tento atribut umožňuje zadat metodu pro načítání dat GridView.

1. Otevřete **Products.aspx** stránky a zahrnout **GridView**. GridView nakonfigurujte, jak je uvedeno dále používání silného typu vazby a Povolit řazení a stránkování.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Použít novou **metody SelectMethod** atribut ke konfiguraci GridView k volání **GetCategories** metoda vyberte data.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Otevřete **Products.aspx.cs** kódu souboru a přidejte následující příkazy using.

    (Code fragment kódu - *Web Forms laboratoř - Ex01 - obory názvů*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Přidat člena privátní v **produkty** třídy a přiřadit novou instanci třídy **ProductsContext**. Tato vlastnost bude ukládat data kontext Entity Framework, který umožňuje připojení k databázi.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Vytvoření **GetCategories** metoda pro načtení seznamu kategorií pomocí LINQ. Bude zahrnovat dotaz **produkty** vlastnost tak GridView můžete zobrazit množství produktů pro každou kategorii. Všimněte si, že metoda vrátí objekt nezpracovaná IQueryable, který představuje dotaz tak, aby se provést později na stránce životního cyklu.

    (Code fragment kódu - *Web Forms laboratoř - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > V předchozích verzích webových formulářů ASP.NET povolení řazení a stránkování pomocí vlastní logiky úložiště v rámci kontextu zdroj dat objektu potřeba psát vlastní kód a přijímat všechny potřebné parametry. Nyní jako metody vazby dat může vrátit IQueryable a to představuje dotaz stále mají být provedeny, ASP.NET zabýváme změnu dotazu pro přidání správné řazení a stránkování parametry.
6. Stiskněte klávesu **F5** spustit ladění lokalitu a přejděte na stránku produktů. Měli byste vidět, že je GridView naplněný kategorií vráceném metodou GetCategories.

    ![Vyplnění pomocí vazby modelu GridView](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "naplnění GridView pomocí vazby modelu")

    *Naplnění GridView pomocí vazby modelu*
7. Stiskněte klávesu **SHIFT**+**F5** Zastavte ladění.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Úloha 3 – zprostředkovatele hodnot v vazby modelu

Vazby modelu nejen umožňuje určit vlastní metody pro práci s daty přímo v ovládacím prvku vázané na data, ale taky umožňuje mapování dat ze stránky na parametry z těchto metod. U parametru metody můžete zadat zdroj dat hodnotu atributy zprostředkovatele hodnot. Příklad:

- Ovládací prvky na stránce
- Hodnoty řetězce dotazu
- Zobrazení dat
- Stav relace
- Soubory cookie
- Data odeslaného formuláře
- Stav zobrazení
- Vlastní hodnota poskytovatelé jsou podporovány také

Pokud jste použili ASP.NET MVC 4, si všimnete, že je podobný podpora vazby modelu. Ve skutečnosti tyto funkce byly převzaty z rozhraní ASP.NET MVC a přesunout do **System.Web** sestavení mohli také používat na webové formuláře.

V této úloze bude aktualizovat GridView filtrování své výsledky podle množství produktů pro každou kategorii přijetí parametr filtru s vazby modelu.

1. Přejděte zpět **Products.aspx** stránky.
2. V horní části GridView, přidejte **popisek** a **ComboBox** vyberte počet produktů, pro každou kategorii, jak je uvedeno níže.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Přidat **EmptyDataTemplate** k GridView zobrazíte zprávu, pokud nejsou žádné kategorie s vybraný počet produktů.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Otevřete **Products.aspx.cs** kódu a přidejte následující příkaz using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Změnit **GetCategories** metodu celé **minProductsCount** argument a filtrování vrácených výsledků. K tomu, nahraďte následujícím kódem metodu.

    (Code fragment kódu - *Web Forms laboratoř - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Nové **[řízení]** atributu u **minProductsCount** argument vám umožní prostředí ASP.NET informuje její hodnota musí být naplněn pomocí ovládacího prvku na stránce. ASP.NET bude hledat libovolný ovládací prvek odpovídající názvu argumentu (minProductsCount) a proveďte potřebné mapování a převod k vyplnění parametr s hodnotou ovládacího prvku.

    Alternativně atribut poskytuje přetížené konstruktor, který umožňuje určit, odkud má být získána hodnota ovládacího prvku.

    > [!NOTE]
    > Jeden cíl funkcí vazby dat je ke snížení objemu kód, který musí být napsané pro stránku interakce. Kromě zprostředkovatele hodnot [řízení] můžete použít dalších zprostředkovatelů vazby modelu v parametry metody. Některé z nich jsou uvedeny v úvodu úloh.
6. Stiskněte klávesu **F5** spustit ladění lokalitu a přejděte na stránku produktů. V rozevíracím seznamu vyberte počet produktů a Všimněte si, jak je GridView nyní aktualizovat.

    ![Filtrování GridView s hodnotou rozevíracího seznamu](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtrování GridView s hodnotou rozevíracího seznamu")

    *Filtrování GridView s hodnotou rozevíracího seznamu*
7. Zastavte ladění.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Úloha 4 – vazby pro filtrování pomocí modelu.

V této úloze přidáte druhou, podřízené GridView zobrazíte produkty v rámci vybrané kategorie.

1. Otevřete **Products.aspx** stránky a aktualizovat kategorie GridView pro automatické generování tlačítka Vybrat.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Přidejte druhý **GridView** s názvem **productsGrid** v dolní části. Nastavte **ItemType** k **WebFormsLab.Model.Product**, **DataKeyNames** k **ProductId** a **metody SelectMethod**  k **GetProducts**. Nastavit **AutoGenerateColumns** k **false** a přidejte sloupcům ProductId, ProductName, popis a UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Otevřete **Products.aspx.cs** souboru kódu na pozadí. Implementace **GetProducts** metoda přijmout ID kategorie z kategorie GridView a filtrovat produkty. Nastaví hodnotu parametru pomocí vybraný řádek v vazby modelu **categoriesGrid**. Vzhledem k tomu, že argument název a název ovládacího prvku se neshodují, by měl zadejte název ovládacího prvku ve zprostředkovateli hodnoty ovládacího prvku.

    (Code fragment kódu - *Web Forms laboratoř - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Tento přístup je jednodušší jednotkou testování těchto metod. V kontextu testu jednotky, kde není provádění webových formulářů, nebude atribut [ovládacího prvku] provádět žádnou zvláštní akci.
4. Otevřete **Products.aspx** stránky a vyhledejte produkty GridView. Aktualizace produktů GridView zobrazíte odkaz pro úpravy vybrané produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Otevřete **ProductDetails.aspx** stránky kódu a nahraďte **SelectProduct** metoda následujícím kódem.

    (Code fragment kódu - *Web Forms laboratoř - Ex01 - SelectProduct metoda*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Všimněte si, že **[QueryString]** atribut se používá k vyplnění parametru metody z productId parametr v řetězci dotazu.
6. Stiskněte klávesu **F5** spustit ladění lokalitu a přejděte na stránku produktů. Vyberte všechny kategorie z kategorií GridView a Všimněte si, že se aktualizuje produkty GridView.

    ![Zobrazuje produkty z vybrané kategorie](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "zobrazující produkty z vybrané kategorie")

    *Zobrazuje produkty z vybrané kategorie*
7. Klikněte **zobrazení** odkaz na produktu chcete otevřít stránku ProductDetails.aspx.

    Všimněte si, že stránce načítá produktu pomocí metody SelectMethod pomocí parametru productId z řetězce dotazu.

    ![Zobrazení podrobností produktu](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "zobrazení Podrobnosti o produktu")

    *Zobrazení Podrobnosti o produktu*

    > [!NOTE]
    > Možnost zadat popis HTML budou prováděny v dalším cvičení.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Úloha 5: vazby pro operace aktualizace pomocí modelu.

V předchozí úloze především pro výběr data používáte vazby modelu, v této úloze se dozvíte, jak používat vazby modelu v operacích aktualizace.

Aktualizujte kategorií GridView, aby mohl uživatel aktualizovat kategorie.

1. Otevřete **Products.aspx** stránky a aktualizovat kategorie GridView automaticky generovat tlačítko Upravit a použít novou **metody UpdateMethod** žádné atributy **UpdateCategory**metoda se aktualizovat vybranou položku.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Atribut DataKeyNames v GridView definovat, které jsou členy, které jedinečně identifikují objektu svázaného se model a proto, které jsou parametry, které se alespoň obdrželi metodu aktualizace.
2. Otevřete **Products.aspx.cs** souboru kódu na pozadí a implementujte **UpdateCategory** metoda. Metoda měli obdržet ID kategorie se načíst aktuální kategorii, naplní hodnoty z GridView a aktualizujte kategorii.

    (Code fragment kódu - *Web Forms laboratoř - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Nové **TryUpdateModel** metodu v třídě stránky zodpovídá vyplní objekt modelu pomocí hodnot z ovládacích prvků na stránce. V takovém případě nahradí aktualizovanými hodnotami z aktuálního řádku GridView upravovaný do **kategorie** objektu.

    > [!NOTE]
    > Dalším cvičení vysvětluje použití ModelState.IsValid pro ověření, že data zadaná uživatelem při úpravě objektu.
3. Spuštění tohoto webu a přejděte na stránku produktů. Upravte kategorii. Zadejte nový název a potom klikněte na **aktualizace** a zachová tak změny.

    ![Úpravy kategorie](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "úpravy kategorií")

    *Úpravy kategorií*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Cvičení 2: Ověřování dat

V tomto cvičení se dozvíte o nových funkcích ověřování dat v technologii ASP.NET 4.5. Bude rezervovat nových funkcí nerušivý ověření v webových formulářů. Datových poznámek ve třídách modelu aplikace bude používat pro ověřování uživatelského vstupu a nakonec se dozvíte, jak zapnout nebo vypnout ověřování požadavku na jednotlivé ovládací prvky na stránce.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Úloha 1 – ověření Nerušivého

Formuláře s komplexní data, včetně validátory často generovat příliš mnoho kód JavaScript na stránce, což může představovat přibližně 60 % kód. S nerušivý ověřování povoleno bude váš HTML kód vypadat čisticí a tidier.

V této části bude povolíte nerušivý ověření technologie ASP.NET pro porovnání HTML kód vygenerovaný obě konfigurace.

1. Otevřete **Visual Studio 2012** a otevřete **začít** řešení umístěný v **Source\Ex2 Validation\Begin** složky tohoto testovacího prostředí. Alternativně můžete pokračovat v práci na vaše stávající řešení z předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést v Průzkumníku řešení, klikněte na tlačítko **WebFormsLab** projektu **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Stiskněte klávesu **F5** spuštění webové aplikace. Přejděte na zákazníky a klikněte na tlačítko **přidat nový zákazník** odkaz.
3. Klikněte pravým tlačítkem na stránce prohlížeče a vyberte **zobrazit zdroj** možnost otevřete generovaný aplikací, kódu HTML.

    ![Stránkou HTML kód](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "stránkou kódu HTML")

    *Stránkou kódu HTML*
4. Procházejte stránky zdrojový kód a Všimněte si, že technologie ASP.NET má vložit JavaScript kód a data validátory na stránce provedou ověření a zobrazit v seznamu chyb.

    ![Ověření kódu jazyka JavaScript na stránce CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "kódu jazyka JavaScript ověření CustomerDetails stránce")

    *Ověření kódu jazyka JavaScript na stránce CustomerDetails*
5. Zavřete prohlížeč a přejděte zpátky na Visual Studio.
6. Nyní povolíte nerušivý ověření. Otevřete **Web.Config** a vyhledejte **ValidationSettings:UnobtrusiveValidationMode** klíče v **AppSettings** části **.** Nastavte hodnotu klíče na **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Tuto vlastnost lze nastavit i v &quot; **stránky\_zatížení** &quot; událost v případě, které chcete povolit Nerušivý ověřování jenom pro některé stránky.
7. Otevřete **CustomerDetails.aspx** a stiskněte klávesu **F5** spuštění webové aplikace.
8. Stisknutím klávesy F12 otevření nástrojů pro vývojáře aplikace Internet Explorer. Jakmile nástrojů pro vývojáře otevřít, vyberte kartu skriptu. Vyberte **CustomerDetails.aspx** z nabídky a proveďte Všimněte si, že tyto skripty nutná k provozování jQuery na stránce byla načtena do prohlížeče z místní lokality.

    ![Načítání jQuery JavaScript soubory přímo z místního serveru služby IIS](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "načítání jQuery JavaScript soubory přímo z místního serveru služby IIS")

    *Načítání souborů jQuery JavaScript přímo z místního serveru služby IIS*
9. Zavřete prohlížeč se vraťte do sady Visual Studio. Otevřete **Site.Master** znovu a najděte **ScriptManager**. Přidat atribut **EnableCdn** vlastnosti s hodnotou **True**. Tato akce vynutí jQuery načíst tuto adresu URL online, nikoli z místní adresy URL.
10. Otevřete **CustomerDetails.aspx** v sadě Visual Studio. Stisknutím klávesy F5 spusťte web. Po spuštění aplikace Internet Explorer, stisknutím klávesy F12 otevření nástrojů pro vývojáře. Vyberte **skriptu** kartě a potom si prohlédněte rozevíracího seznamu. Všimněte si, že soubory jQuery JavaScript jsou už načítá z místní lokality, ale místo z online jQuery CDN.

    ![Načítání jQuery JavaScript souborů z CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "načítání jQuery JavaScript souborů z CDN")

    *Načítání souborů JavaScript jQuery od CDN*
11. Otevřete zdrojový kód HTML stránky znovu pomocí možnosti zobrazení zdroje v prohlížeči. Všimněte si, že povolením nerušivý ověření ASP.NET nahradili vloženého kódu jazyka JavaScript s data - \*atributy.

    ![Nerušivý ověřovacího kódu](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Nerušivý ověřovacího kódu")

    *Nerušivý ověřovacího kódu*

    > [!NOTE]
    > V tomto příkladu jste viděli, jak byl souhrnu s anotacemi dat ověřování zjednodušenou jenom pár HTML a JavaScript řádků. Dříve bez nerušivý ověřování, další ovládací prvky ověřování, které přidáte, tím větší ověření kódu JavaScript se zvýší.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Úloha 2 – ověření modelu s anotacemi dat

Technologie ASP.NET 4.5 přináší poznámky ověření dat pro webové formuláře. Místo nutnosti prvek ověřování v každém vstupu, teď můžete definovat omezení ve třídách modelu a používejte je ve vaší webové aplikaci. V této části se dozvíte, jak pomocí datových poznámek pro ověřování formuláře zákazníka novou nebo upravit.

1. Otevřete **CustomerDetail.aspx** stránky. Všimněte si, že zákazník nejprve název a druhý názvu **EditItemTemplate** a **InsertItemTemplate** oddíly se ověřují pomocí ovládacích prvků RequiredFieldValidator. Každý validátoru je přidružena k určitá podmínka, takže je nutné zahrnout tolik validátory jako podmínek pro kontrolu.
2. Přidejte datových poznámek k ověření třídy modelu zákazníka. Otevřete **Customer.cs** třídy v **modelu** složky a *uspořádání* každou vlastnost pomocí datové poznámky atributů.

    (Code fragment kódu - *Web Forms laboratoř - Ex02 - datových poznámek*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > Rozhraní .NET framework 4.5 má rozšířit existující kolekci dat poznámky. Toto jsou některé anotací dat můžete použít: [CreditCard], [Phone], [EmailAddress], [Oblast], [porovnat], [Url], [FileExtensions], [potřeby] [klíč], [regulární výraz].
    > 
    > Některé příklady použití:
    > 
    > [klíč]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Můžete také definovat vlastní chybové zprávy v rámci každého atributu.
3. Otevřete **CustomerDetails.aspx** a odeberte všechny RequiredFieldvalidators křestní jméno a příjmení pole v částech v EditItemTemplate a InsertItemTemplate FormView ovládacího prvku.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Jednou z výhod pomocí datových poznámek je, že logiku ověření není duplikovaná na stránkách aplikace. Můžete definovat jednou v modelu a použít na všech stránkách aplikace, které zpracovávají data.
4. Otevřete **CustomerDetails.aspx** kódu a vyhledejte metodu SaveCustomer. Tato metoda je volána při vkládání nových zákazníků a přijímá od hodnoty řízení FormView parametr zákazníka. Při mapování mezi ovládacími prvky stránky a zemře objekt parametr, ASP.NET, budou spuštěny při ověřování modelu pro všechny datové poznámky atributy a vyplnění slovníku ModelState k chybám došlo, pokud existuje.

    ModelState.IsValid pouze vrátí hodnotu true, pokud všechna pole na modelu jsou platné po provedení ověřování.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Přidat **ValidationSummary** ovládacího prvku na konci stránky CustomerDetails zobrazíte seznam chyb modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** je novou vlastnost na ValidationSummary řízení, pokud je nastavena na **true**, ovládacího prvku se zobrazí chyby ze slovníku ModelState. Tyto chyby pocházet z ověřování dat poznámky.
6. Stiskněte klávesu **F5** ke spuštění webové aplikace. Vyplňte formulář s některé chybné hodnoty a klikněte na tlačítko **Uložit** provést ověření. Všimněte si souhrn v dolní části chyb.

    ![Ověřování s anotacemi dat](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "ověřování s anotacemi dat")

    *Ověřování s anotacemi dat*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Úloha 3 – zpracování chyb vlastní databázi s ModelState

V předchozí verzi webových formulářů zpracování chyb databáze, jako je řetězec příliš dlouhý nebo porušení jedinečného klíče může patřit: generování výjimek ve vašem kódu úložiště a pak zpracování výjimek ve vašem kódu zobrazuje chybu. Velký objem kódu je potřeba dělat něco poměrně jednoduché.

V aplikaci Web Forms 4.5 objekt ModelState slouží k zobrazení chyby na stránce, buď z modelu, nebo z databáze, konzistentním způsobem.

V této úloze přidáte kód, aby správně zpracování výjimek databáze a zobrazit příslušnou zprávu pro uživatele pomocí objektu ModelState.

1. Zatímco aplikace stále běží, pokuste se aktualizovat název kategorie pomocí duplicitní hodnoty.

    ![Aktualizace kategorie s duplicitní název](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "aktualizace s duplicitní název kategorie")

    *Aktualizace kategorie s duplicitní název*

    Všimněte si, že je vyvolána výjimka, kvůli &quot;jedinečný&quot; omezení **CategoryName** sloupce.

    ![Výjimky pro názvy duplicitní kategorií](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "výjimky pro názvy duplicitní kategorií")

    *Výjimky pro názvy duplicitní kategorií*
2. Zastavte ladění. V **Products.aspx.cs** souboru kódu na pozadí, aktualizovat **UpdateCategory** metodu ke zpracování výjimky vyvolané databáze. Metoda SaveChanges() volání a přidejte chybu **ModelState** objektu.

    Nové **TryUpdateModel** metoda aktualizuje objekt kategorie načtena z databáze pomocí zadané uživatelem data formuláře.

    (Code fragment kódu - *Web Forms laboratoř - Ex02 - UpdateCategory zpracování chyb*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > V ideálním případě by mít Identifikujte příčinu DbUpdateException a zkontrolujte, jestli je hlavní příčinou porušení omezení jedinečnosti klíče.
3. Otevřete **Products.aspx** a přidejte **ValidationSummary** řízení pod kategorií GridView zobrazíte seznam chyb modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Spuštění tohoto webu a přejděte na stránku produktů. Pokuste se aktualizovat název kategorie pomocí duplicitní hodnoty.

    Všimněte si, že bylo zpracováno výjimku a zobrazí se chybová zpráva v **ValidationSummary** ovládacího prvku.

    ![Duplicitní kategorie chyby](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "duplicitní kategorie chyby")

    *Chyba duplicitní kategorie*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Úloha 4 - žádosti o ověření v webové formuláře ASP.NET 4.5

Funkce ověření žádosti technologie ASP.NET poskytuje určitou úroveň výchozí ochranu proti útokům na skriptování (XSS). V předchozích verzích technologie ASP.NET ověření se ve výchozím nastavení povolené a může být zakázáno pouze pro celou stránku. V nové verzi webových formulářů ASP.NET můžete nyní zakázat ověření žádosti pro jeden ovládací prvek, provádět ověřování opožděného žádosti nebo přístup k datům zrušení ověřené žádosti (buďte opatrní, pokud tak učiníte!).

1. Stiskněte klávesu **Ctrl + F5** spuštění webu bez ladění a přejít na stránku produktů. Vyberte kategorii a klikněte **upravit** odkaz na všechny produkty.
2. Zadejte popis obsahující potenciálně nebezpečný obsah, včetně například značky HTML. Vzít v úvahu výjimce z důvodu ověření žádosti.

    ![Úpravy produkt potenciálně nebezpečný obsah](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "úpravy produkt potenciálně nebezpečný obsah")

    *Úpravy produkt potenciálně nebezpečný obsah*

    ![Došlo k výjimce z důvodu ověření žádosti](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "došlo k výjimce z důvodu ověření žádosti")

    *Došlo k výjimce z důvodu ověření žádosti*
3. Zavřete stránku a v sadě Visual Studio, stiskněte **SHIFT + F5** Zastavit ladění.
4. Otevřete **ProductDetails.aspx** stránky a najděte **popis** textové pole.
5. Přidejte nové **ValidateRequestMode** vlastnosti textového pole a jeho hodnotu nastavte **zakázané**.

    Nové **ValidateRequestMode** atribut umožňuje ověření žádosti granularly na každý ovládací prvek zakázat. To je užitečné, pokud chcete použít, může přijímat kód HTML, ale chcete zachovat ověření práce pro zbytek stránce vstup.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Stiskněte klávesu **F5** ke spuštění webové aplikace. Znovu otevřete stránku upravit produktu a dokončete popis produktu, včetně značky HTML. Všimněte si, že nyní můžete přidat obsah HTML popisu.

    ![Žádosti o ověření pro popis produktu zakázáno](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "žádosti o ověření zakázána pro popis produktu")

    *Ověření žádosti pro popis produktu zakázáno*

    > [!NOTE]
    > V případě produkční aplikace by měla úpravu kódu HTML zadaného uživatelem, jsou-li zadána pouze bezpečné značky HTML (například neexistují žádné &lt;skriptu&gt; značek). Chcete-li to provést, můžete použít [knihovna Microsoft webových ochrany](https://www.nuget.org/packages/AntiXSS).
7. Upravte produktu znovu. Do pole Název zadejte kód HTML a klikněte na tlačítko **Uložit**. Všimněte si, že žádosti o ověření je zakázané pouze pro pole Popis a zbývající pole re stále ověřený proti potenciálně nebezpečný obsah.

    ![Žádosti o ověření v ostatních polích povolené](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "žádosti o ověření v ostatních polích povolené")

    *Ověření žádosti povoleno ve zbývající části pole*

    Webové formuláře ASP.NET 4.5 zahrnuje nový režim ověření požadavku líné provést ověření žádosti. S režim ověření požadavku, který je nastavený na **4.5**, pokud úsek kódu přístupů *Request.Form [&quot;klíč&quot;]*, technologie ASP.NET 4.5 žádosti o ověření budou pouze aktivace žádosti o ověření pro tento konkrétní element v kolekci formuláře.

    Kromě toho technologie ASP.NET 4.5 nyní zahrnuje rutiny kódování základní z knihovny Microsoft Anti-XSS v4.0. Anti-XSS kódování rutiny jsou implementované nové *AntiXssEncoder* typ se nenašel v novém **System.Web.Security.AntiXss** oboru názvů. S **encoderType** parametr nakonfigurované na používání *AntiXssEncoder*, všechny výstup kódování v prostředí ASP.NET automaticky použije nové rutiny pro kódování.
8. ASP.NET 4.5 žádosti o ověření také podporuje zrušení ověřený přístup k datům požadavku. Technologie ASP.NET 4.5 přidává novou vlastnost kolekce, která má **požadavku HTTP** objektu s názvem **Unvalidated**. Když přejdete do **HttpRequest.Unvalidated** mají přístup ke všem běžné kusů dat požadavku, včetně formulářů, QueryStrings, soubory cookie, adresy URL a tak dále.

    ![Objekt Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated objektu")

    *Objekt Request.Unvalidated*

    > [!NOTE]
    > **Použijte prosím vlastnost HttpRequest.Unvalidated opatrně!** Zkontrolujte, zda že je pečlivě provést vlastní ověřování na požadavek nezpracovaných dat k zajistěte, aby neobsahoval nebezpečné text není zpětné odbavení a vykresleny si zákazníci!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Cvičení 3: Asynchronní stránky zpracování v rozhraní ASP.NET Web Forms

V tomto cvičení jste se seznámili na stránku nový asynchronní zpracování funkce webových formulářů ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Úloha 1 – aktualizaci produktu podrobnosti stránky a zobrazení obrázků

V této úloze aktualizujte stránku Podrobnosti o produktu umožňující uživateli zadat adresu URL obrázku pro produkt a zobrazit v zobrazení jen pro čtení. Vytvoříte místní kopii zadanou bitovou kopii stažením synchronně. V dalším úkolem aktualizujte tuto implementaci ke spolupráci asynchronně.

1. Otevřete **Visual Studio 2012** a zatížení **začít** řešení umístěný v **Source\Ex3 Async\Begin** ze složky toto testovací prostředí. Alternativně můžete pokračovat v práci na vaše stávající řešení z předchozí cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést v Průzkumníku řešení, klikněte na tlačítko **WebFormsLab** projektu a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **ProductDetails.aspx** stránky zdroje a přidání pole v třídě FormView ItemTemplate zobrazíte bitovou kopii produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Přidáte pole, které chcete zadat adresu URL obrázku v třídě FormView EditTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Otevřete **ProductDetails.aspx.cs** kódu souboru a přidejte následující direktivy oboru názvů.

    (Code fragment kódu - *Web Forms laboratoř - Ex03 - obory názvů*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Vytvoření **UpdateProductImage** metodu pro uložení bitové kopie vzdáleného místní **bitové kopie** složku a aktualizace produktu entity s novou hodnotou umístění bitové kopie.

    (Code fragment kódu - *Web Forms laboratoř - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Aktualizace **UpdateProduct** metoda k volání **UpdateProductImage** metoda.

    (Code fragment kódu - *Web Forms laboratoř - Ex03 - UpdateProductImage volání*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Spusťte aplikaci a zkuste odeslat bitovou kopii pro produkt. Například můžete použít následující adresa URL obrázku z přístroje klip Office: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Nastavení bitovou kopii pro produkt](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "nastavení bitovou kopii pro produkt")

    *Nastavení bitovou kopii pro produkt*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Úloha 2 – přidání asynchronní zpracování na stránku podrobností produktu

V této úloze aktualizujte stránku Podrobnosti o produktu ke spolupráci asynchronně. Dlouho spuštěná úloha - proces stahování image - bude vylepšit pomocí technologie ASP.NET 4.5 zpracování asynchronní stránky.

Asynchronní metody ve webových aplikacích můžete použít k optimalizaci způsob, jakým se používají fondy vláken ASP.NET. Technologie ASP.NET došlo jsou omezený počet vláken ve fondu vláken pro vaši účast požadavků, proto po zaneprázdněný, všechna vlákna ASP.NET začne odmítnout nové požadavky, odešle aplikace chybové zprávy a váš web, nebude možné.

Časově náročná operace na vašem webu jsou skvělí kandidáti pro asynchronní programování vzhledem k tomu, že zabírají přiřazené vlákno po dlouhou dobu. To zahrnuje dlouho spuštěných požadavků, stránky s mnoha různých elementů a stránek, které vyžadují offline operace, takový dotaz na databázi nebo přístup k externí webový server. Výhoda spočívá v tom, že pokud používáte asynchronních metod pro tyto operace během zpracování stránky, vlákno je uvolněno a vrátí vlákno fondu a může sloužit k účasti na žádost o nové stránky. To znamená, stránka se spustí v jedno vlákno z fondu vláken zpracování a může dokončit zpracování v nějaký jiný po dokončení asynchronní zpracování.

1. Otevřete **ProductDetails.aspx** stránky. Přidat **asynchronní** atribut **stránky** elementu a nastavte ji na **true**. Tento atribut informuje technologii ASP.NET pro implementaci rozhraní IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Přidání popisku v dolní části stránky a zobrazit podrobnosti vláken spuštění stránky.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Otevřete **ProductDetails.aspx.cs** a přidejte následující direktivy oboru názvů.

    (Code fragment kódu - *webové obory názvů laboratoř - Ex03 - Forms 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Změnit **UpdateProductImage** metoda stažení bitové kopie s asynchronní úlohu. Nahradíte **WebClient** **DownloadFile** metoda s **DownloadFileTaskAsync** metoda a zahrnout **await** – klíčové slovo.

    (Code fragment kódu - *Web Forms laboratoř - Ex03 - UpdateProductImage asynchronní*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask zaregistruje nový asynchronní úkol stránku spouštění v jiném podprocesu. Obdrží výrazu lambda s úlohy (t) ke spuštění. **Await** – klíčové slovo v **DownloadFileTaskAsync** metoda převede zbytek metoda zpětného volání, která je volána asynchronně po **DownloadFileTaskAsync** metoda byla dokončena. ASP.NET bude pokračovat v provádění metody automaticky udržováním všechny žádosti o původní hodnoty HTTP. Nový model asynchronního programování v rozhraní .NET 4.5 umožňuje psát asynchronní kód, který vypadá hodně podobá synchronní kódu a nechat kompilátoru zpracování komplikace funkce zpětného volání nebo pokračování kód.
    > [!NOTE]
    > RegisterAsyncTask a PageAsyncTask již byly k dispozici od verze rozhraní .NET 2.0. Klíčové slovo await od asynchronní programovací model rozhraní .NET 4.5 a můžete použít společně s nové metody TaskAsync z objektu .NET WebClient.
5. Přidejte kód k zobrazení vláken, na kterých kód, spuštění a dokončení provádění. Chcete-li to provést, aktualizujte **UpdateProductImage** metoda následujícím kódem.

    (Code fragment kódu - *Web Forms laboratoř - Ex03 - zobrazit vláken*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Otevřete na webu **Web.config** souboru. Přidejte následující proměnné appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Stiskněte klávesu **F5** spusťte aplikaci a odeslat bitovou kopii produktu. Všimněte si, kde může být jiný kód, spuštění a dokončení ID vlákna. To je proto asynchronních úloh spustit na samostatné vlákno z fondu podprocesů ASP.NET. Po dokončení úlohy ASP.NET vloží úlohu zpět ve frontě a přiřadí některé z dostupných vláken.

    ![Asynchronní stahování bitovou kopii](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "asynchronně stahování obrázku")

    *Asynchronní stahování obrázku*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci do Azure následující [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V tomto testovacím prostředí praktických byly následující koncepty řešit a ukázán:

- Použití datových vazeb výrazů silného typu
- Používat nové funkce vazby modelu v webových formulářů
- Použití zprostředkovatele hodnot pro mapování stránky data na metody kódu
- Pomocí datových poznámek pro ověřování uživatelského vstupu
- Proveďte advange ověřování na straně klienta unobstrusive s jQuery webových formulářů
- Implementace granulární žádosti o ověření
- Implementace asynchronního zpracování ve formulářích webové stránky

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Azure SDK</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express pro Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

Tento dodatek vám ukáže, jak vytvořit nový web z portálu Azure a publikování aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované platformou Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Úloha 1 – Vytvoření nového webu z portálu Azure

1. Přejděte na [portálu pro správu Azure](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.

    > [!NOTE]
    > S Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **výpočetní** | **webu**. Potom vyberte **rychle vytvořit** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.

    > [!NOTE]
    > Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci. Možnost rychle vytvořit můžete nasadit hotové webové aplikace do Azure z mimo portál. Postup pro nastavení databáze neobsahuje.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** je vytvořena.
5. Po vytvoření webu klikněte na odkaz v části **URL** sloupce. Zkontrolujte, zda je funkční nový web.

    ![Procházení na nový web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "procházení na nový web")

    *Procházení na nový web*

    ![Webový server spuštěn](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "webu systémem")

    *Spuštění webu*
6. Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.

    ![Otevření stránky Správa webu](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "otevření stránek správu webového serveru")

    *Otevření stránek správu webového serveru*
7. V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.

    > [!NOTE]
    > *Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace do Azure pro každou metodu povoleno publikace. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webových aplikací do Azure.

    ![Na webu stažení profilu publikování](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "stahování webové stránky profilu publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profil publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace do Azure ze sady Visual Studio.

    ![Ukládání souboru profilu publikování](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "ukládání profilu publikování")

    *Ukládání souboru profilu publikování*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.

1. Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace. Databáze SQL servery můžete zobrazit ze svého předplatného v portálu Azure Management portal na **databází Sql** | **servery** | **řídicího panelu serveru**. Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů. Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly. Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.

    ![Řídicí panel serveru databáze SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "řídicího panelu serveru databáze SQL")

    *Řídicí panel serveru databáze SQL*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) tlačítko.

    ![Přidávání IP adresy klienta](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Přidávání IP adresy klienta*
3. Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.

    ![Potvrzení změn](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Potvrzení změn*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

1. Přejděte zpět na ASP.NET MVC 4 řešení. V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "publikování aplikace")

    *Publikování webu*
2. Umožňuje naimportujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "import profilu publikování")

    *Import profilu publikování*
3. Klikněte na tlačítko **ověření připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.

    ![Ověření připojení](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "ověřování připojení")

    *Ověření připojení*
4. V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Připojení k databázi nakonfigurujte následujícím způsobem:

   - V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte přihlašovací heslo správce serveru.
   - Zadejte nový název databáze.

     ![Konfigurace cílový připojovací řetězec](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.

    ![Vytvoření databáze](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "vytváření řetězec databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL v Azure je uvedené v rámci textbox výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na databázi SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "připojovací řetězec odkazující na databázi SQL")

    *Připojovací řetězec odkazující na databázi SQL*
8. V **Preview** klikněte na tlačítko **publikovat**.

    ![Publikování webové aplikace](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Příloha C: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

![Začněte psát název fragmentu](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

![Stisknutím klávesy Tab znovu a fragmentu rozšíří](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
2. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*
