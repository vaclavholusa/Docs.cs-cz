---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Co je nového ve webových formulářů v ASP.NET 4.5 | Dokumentace Microsoftu
author: rick-anderson
description: Novou verzi technologie ASP.NET webové formuláře přináší řadu vylepšení, zaměřuje na vylepšení činnost koncového uživatele při práci s daty. V předchozích verzích nástroje...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 54e0234d6f13ce62803dbe55a836414a93a207b2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756926"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Co je nového ve webových formulářích v ASP.NET 4.5
====================
podle [Campy Web týmu](https://twitter.com/webcamps)

> Novou verzi technologie ASP.NET webové formuláře přináší řadu vylepšení, zaměřuje na vylepšení činnost koncového uživatele při práci s daty.
> 
> V předchozích verzích webových formulářů, při použití datových vazeb a vygenerovat hodnotu členem objektu můžete použít výrazy vázání dat Bind() nebo Eval(). V nové verzi technologie ASP.NET je možné deklarovat, jaký typ dat ovládacího prvku se to být vázány na pomocí nové vlastnosti ItemType. Nastavení této vlastnosti vám umožní použít proměnnou silného typu pro příjem všech výhod vývojové prostředí sady Visual Studio, například IntelliSense, navigace členů a kontrolu za kompilace.
> 
> Pomocí ovládacích prvků vázaných na data můžete teď také zadat vlastní vlastních metod pro výběr, aktualizace, odstraňování a vkládání dat, zjednodušení interakce mezi ovládacími prvky stránky a logice aplikace. Kromě toho byly přidány možnosti vázání modelu ASP.NET, což znamená, že data ze stránky můžete namapovat přímo do parametrů typu metody.
> 
> Ověřování uživatelského vstupu by mělo být také jednodušší s nejnovější verzí webových formulářů. Teď můžete opatřit poznámkami třídách modelu s atributy ověření ze **System.ComponentModel.DataAnnotations** obor názvů a požadavek, který řídí váš web ověření vstupu uživatele s použitím těchto informací. Ověřování na straně klienta ve webových formulářích je teď integrovaná s jQuery čisticí kód na straně klienta a funkce nerušivý JavaScript.
> 
> V oblasti žádosti o ověření byla vylepšena usnadňují selektivně vypnout ověření žádosti pro konkrétní části vašich aplikací nebo číst data zneplatněné žádosti.
> 
> Několik vylepšení byly provedeny do webových formulářů serverové ovládací prvky, abyste mohli využívat nové funkce HTML5:
> 
> - Aktualizovali jsme v textovém režimu vlastnost ovládacího prvku textového pole pro podporu nových typů vstupu HTML5 jako e-mailová data a času a tak dále.
> - Ovládací prvek FileUpload teď podporuje více nahrávání souborů z prohlížečů, které podporují tuto funkci HTML5.
> - Program pro ověření řídí teď podporu ověřování HTML5 vstupních prvků.
> - Nové prvky HTML5, které mají atributy, které představují adresy URL teď podporují runat =&quot;server&quot;. V důsledku toho může používat technologie ASP.NET v cestě adresy URL, jako je třeba ~ – operátor pro reprezentaci kořenový adresář aplikace (například &lt;videa runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - Opravili jsme ovládací prvek UpdatePanel pro podporu vstupních polí účtování HTML5.
> 
> Na portálu oficiální technologie ASP.NET můžete najít další příklady nových funkcí v technologii ASP.NET 4.5 webových formulářů: [co je nového v technologii ASP.NET 4.5 a Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této praktická cvičení se dozvíte, jak:

- Použití silně typovaných výrazů vázání dat
- Používat nové funkce vazby modelu ve webových formulářů
- Použití zprostředkovatele hodnot pro použití modelu code-behind metody mapování data stránky
- Použití anotací dat pro ověřování uživatelského vstupu
- Využijte advange unobstrusive ověřování na straně klienta s jQuery ve webových formulářů
- Implementace detailní žádost o ověření
- Implementace asynchronního zpracování ve webových formulářích stránky

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).

<a id="Setup"></a>
### <a name="setup"></a>Instalace

**Instalace fragmenty kódu**

Pro usnadnění práce velkou část kódu, které budete spravovat podél tohoto testovacího prostředí je k dispozici jako fragmenty kódu sady Visual Studio. K instalaci spustit fragmenty kódu **.\Source\Setup\CodeSnippets.vsi** souboru.

Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha C:](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické testovací prostředí obsahuje následující praktická cvičení:

1. [Cvičení 1: Model vazby ve webových formulářích ASP.NET](#Exercise1)
2. [Cvičení 2: Ověření dat](#Exercise2)
3. [Cvičení 3: Asynchronní zpracování stránky v ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Cvičení 1: Model vazby ve webových formulářích ASP.NET

Novou verzi technologie ASP.NET webové formuláře přináší řadu vylepšení, zaměřuje na zlepšení zkušeností při práci s daty. Během tohoto cvičení se další informace o ovládacích prvcích silného typu dat a vazby modelu.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Úloha 1 – použití silně typované datové vazby

V této úloze bude zjišťovat nové silného typu vazby k dispozici v technologii ASP.NET 4.5.

1. Otevřít **začít** řešení nachází v **zdroj/Ex1-objektuModelBinding/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **Customers.aspx** stránky. Umístěte nečíslovaná seznamu v hlavním ovládacím prvkem a zahrnují ovládacím prvkem repeater uvnitř pro výpis každého zákazníka. Nastavte název opakovače **customersRepeater** jak je znázorněno v následujícím kódu.

    V předchozích verzích technologie webové formuláře při použití datových vazeb a vygenerovat hodnota člena v objektu jste vázání dat, můžete využít vazbový výraz, spolu s volání (v angličtině) metody předáním hodnoty názvu členu jako řetězec.

    Za běhu bude tato volání hodnotě Eval čtení hodnotu člena se zadaným názvem pomocí reflexe pro aktuálně vázaný objekt a zobrazení výsledku v kódu HTML. Tento přístup umožňuje velmi snadno svázat data s daty libovolného, unshaped.

    Bohužel ztratíte mnoha funkcí skvělé možnosti vývoje v sadě Visual Studio, včetně technologie IntelliSense pro názvy členů, podporu pro navigaci (jako přejít k definici) a kontrola za kompilace.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Otevřít **Customers.aspx.cs** souboru.
4. Přidejte následující příkaz using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. V **stránky\_zatížení** metodu, přidejte kód pro naplnění opakovače s seznam zákazníků.

    (Fragment - kódu *webové formuláře Lab – Ex01 - Bind zákazníkům zdroj dat*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Toto řešení využívá EntityFramework spolu s CodeFirst k vytváření a přístup k databázi. V následujícím kódu je vázán customersRepeater na materializovaného dotazu, který vrátí všechny zákazníky z databáze.
6. Stisknutím klávesy **F5** spuštění řešení a přejděte na **zákazníkům** stránku, abyste zobrazili repeater v akci. Jak toto řešení používá CodeFirst, databáze bude vytvořena a naplněna v místní instanci systému SQL Express, při spuštění aplikace.

    ![Výpis zákazníkům s repeateru](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "výpis zákazníkům s repeateru")

    *Zákazníci s repeateru výpis*

    > [!NOTE]
    > V sadě Visual Studio 2012 služba IIS Express je výchozí webový server vývoje.
7. Zavřete prohlížeč a přejděte zpět do sady Visual Studio.
8. Nyní nahraďte implementace použití silného typu vazby. Otevřít **Customers.aspx** stránky a použijte nové **ItemType** atribut v opakovači nastavit **zákazníka** typ jako typ vazby.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Vlastnost ItemType umožňuje deklarovat, jaký typ dat ovládacího prvku je, že to být vázaný na a umožňuje používat silného typu vazby uvnitř ovládací prvek vázaný na data.
9. Nahraďte obsah následujícím kódem ItemTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Jeden nevýhodou pomocí výše uvedených přístupů je, že volání Eval() a Bind() s pozdní vazbou – to znamená, že předáte řetězce představují názvy vlastností. To znamená, že vám technologie Intellisense pro názvy členů, podporu pro navigaci v kódu (jako přejít k definici) ani kontrolu podporu za kompilace.

    Nastavení vlastnosti ItemType způsobí, že dvě nové proměnné typu rozsah výrazy vázání dat vygeneruje: **položky** a **položku BindItem**. Můžete použít tyto silného typu proměnné ve výrazech datové vazby a získáte všechny výhody vývojové prostředí sady Visual Studio.

    &quot; **:** &quot; Použít ve výrazu se automaticky použije kódování HTML výstup, aby se zabránilo problémům zabezpečení (například mezi útoky prostřednictvím skriptování). Tento typ notation byla k dispozici od verze .NET 4 pro psaní odpovědi, ale teď je také k dispozici ve výrazech datové vazby.

    > [!NOTE]
    > Člen položek pracuje pro jednosměrné vazby. Pokud chcete provést pomocí obousměrné vazby **položku BindItem** člena.

    ![Podporu technologie IntelliSense ve vazbě silného typu](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "podporu technologie IntelliSense ve vazbě silného typu")

    *Podporu technologie IntelliSense ve vazbě silného typu*
10. Stisknutím klávesy **F5** spuštění řešení a přejděte na stránku zákazníků a ujistěte se, že změny fungovat podle očekávání.

    ![Zobrazení Podrobnosti o zákazníkovi](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "výpis podrobnosti o zákazníkovi")

    *Zobrazení Podrobnosti o zákazníkovi*
11. Zavřete prohlížeč a přejděte zpět do sady Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Úloha 2 – představení Model vazby ve webových formulářů

V předchozích verzích webových formulářů ASP.NET Pokud byste chtěli provést obousměrnou vazbu dat, jak načítání a aktualizaci dat, museli jste použití zdroj dat objektu. Může to být objektový zdroj dat, zdroji dat SQL, zdroje dat LINQ a tak dále. Pokud váš scénář vyžaduje vlastní kód pro zpracování dat, ale je potřeba použít zdroj dat objektu a to do určité nevýhody. Například byste potřebovali vyhnuli používání komplexních typů a potřebné pro zpracování výjimek při provádění logiku ověřování.

V nové verzi webových formulářů ASP.NET, ovládací prvky vázané na data podporují vazby modelu. To znamená, že můžete zadat vyberte, aktualizovat, vložení a odstranění metody přímo v ovládací prvek vázaný na data pro volání logiky ze souboru kódu na pozadí nebo z jiné třídy.

Další informace o tom, použijete GridView seznam kategorií produktů pomocí nového **metoda SelectMethod** atribut. Tento atribut umožňuje zadat metodu pro načtení dat prvku GridView.

1. Otevřít **Products.aspx** stránce a zahrnout **GridView**. Nakonfigurujte prvku GridView, jak je znázorněno níže silného typu vazby a Povolit řazení a stránkování.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Pomocí nové **metoda SelectMethod** atributů konfigurace GridView pro volání **GetCategories** pro výběr data.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Otevřít **Products.aspx.cs** použití modelu code-behind soubor a přidejte následující příkazy using.

    (Fragment - kódu *webových formulářů Lab – Ex01 - obory názvů*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Přidat člena soukromý v **produkty** třídy a přiřadit novou instanci třídy **ProductsContext**. Tato vlastnost se ukládání kontextu dat Entity Framework, která umožňuje připojení k databázi.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Vytvoření **GetCategories** metodu pro načtení seznamu kategorií pomocí jazyka LINQ. Bude zahrnovat dotaz **produkty** vlastnost tak prvku GridView. můžete zobrazit objem produktů pro každou kategorii. Všimněte si, že metoda vrátí nezpracované objekt IQueryable, které představují dotaz bude proveden později na životní cyklus stránky.

    (Fragment - kódu *webových formulářů Lab – Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > V předchozích verzích webových formulářů ASP.NET umožňuje řazení a stránkování pomocí vlastní logiky úložiště v rámci kontextu zdroj dat objektu potřeba napsat vlastní kód a zobrazí všechny potřebné parametry. Nyní, a metody datových vazeb může vrátit IQueryable to představuje dotaz stále má být spuštěna, ASP.NET zařídit upravit dotaz pro přidání správné řazení a stránkování parametry.
6. Stisknutím klávesy **F5** spuštění ladění lokalitu a přejděte na stránku produktů. Měli byste vidět, že kategorie vrácený metodou GetCategories se načtou prvku GridView.

    ![Naplnění ovládacího prvku GridView, pomocí vazby modelu](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "naplnění ovládacího prvku GridView, pomocí vazby modelu")

    *Naplnění ovládacího prvku GridView, pomocí vazby modelu*
7. Stisknutím klávesy **SHIFT**+**F5** Zastavit ladění.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Úloha 3 – zprostředkovatele hodnot v vazby modelu

Vazby modelu nejen umožňuje zadat vlastní metody pro práci s daty přímo v ovládacím prvku vázané na data, ale také umožňuje mapování dat ze stránky do parametrů z těchto metod. U parametru metody slouží k určení zdroje dat hodnotu atributy zprostředkovatele hodnot. Příklad:

- Ovládací prvky na stránce
- Hodnoty řetězce dotazu
- Zobrazení dat
- Stav relace
- Soubory cookie
- Data odeslaného formuláře
- Stav zobrazení
- Vlastní hodnota poskytovatelé jsou podporovány také

Pokud jste použili technologie ASP.NET MVC 4, můžete si všimnout, že podpora vazby modelu je podobné. Ve skutečnosti byly tyto funkce na základě technologie ASP.NET MVC a přesunout do **System.Web** sestavení lze také použít v aplikaci Web Forms.

V této úloze budete aktualizovat GridView můžete filtrovat výsledky podle množství produktů pro každou kategorii, přijímají parametr filtru s vazbou modelu.

1. Přejděte zpět **Products.aspx** stránky.
2. V horní části stránky prvku GridView, přidejte **popisek** a **– pole se seznamem** vyberte počet produktů pro každou kategorii, jak je znázorněno níže.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Přidat **EmptyDataTemplate** do prvku GridView. Chcete-li zobrazit zprávu, když neexistují žádné kategorie s vybraný počet produktů.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Otevřít **Products.aspx.cs** použití modelu code-behind a přidejte následující příkaz using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Upravit **GetCategories** metodu celé **minProductsCount** argument a filtrování vrácených výsledků. Provedete to tak, nahraďte metodu s následujícím kódem.

    (Fragment - kódu *webových formulářů Lab – Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Nové **[ovládacího prvku]** atribut na **minProductsCount** argument vám umožní zjistit její hodnota musí být vyplněno pomocí ovládacího prvku na stránce ASP.NET. Technologie ASP.NET se najít žádný ovládací prvek odpovídající název argumentu (minProductsCount) a provést nezbytné mapování a převod na parametr vyplnit hodnoty ovládacího prvku.

    Můžete také atribut poskytuje přetížený konstruktor, který umožňuje určit ovládací prvek, ze kterého má být získána hodnota.

    > [!NOTE]
    > Jedním z cílů datové vazby funkcí je snížit objem kódu, který musí být napsaný pro interakce stránky. Kromě zprostředkovatele hodnot [ovládacího prvku] můžete použít dalších poskytovatelů vazby modelu v parametry metody. Některé z nich jsou uvedeny v úvodu úloh.
6. Stisknutím klávesy **F5** spuštění ladění lokalitu a přejděte na stránku produktů. V rozevíracím seznamu vyberte počet produktů a Všimněte si, jak je teď aktualizovaný prvku GridView.

    ![Filtrování ovládacího prvku GridView s hodnotou rozevíracího seznamu](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtrování ovládacího prvku GridView s hodnotou rozevíracího seznamu")

    *Filtrování ovládacího prvku GridView s hodnotou rozevíracího seznamu*
7. Zastavte ladění.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Úloha 4 – Model pomocí vazby pro filtrování

V této úloze přidejte druhý podřízený prvek GridView zobrazit produkty v rámci vybrané kategorie.

1. Otevřít **Products.aspx** stránce a aktualizovat GridView automaticky vygenerovat tlačítko pro výběr kategorie.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Přidejte druhý **GridView** s názvem **productsGrid** v dolní části. Nastavte **ItemType** k **WebFormsLab.Model.Product**, **DataKeyNames** k **ProductId** a **metoda SelectMethod**  k **GetProducts**. Nastavte **AutoGenerateColumns** k **false** a přidejte sloupce pro ID produktu, ProductName, popis a UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Otevřít **Products.aspx.cs** soubor kódu na pozadí. Implementace **GetProducts** metody přijímají ID kategorie z kategorie GridView a filtrovat produkty. Nastaví hodnotu parametru pomocí vybraný řádek v vazby modelu **categoriesGrid**. Protože název argumentu a název ovládacího prvku se neshodují, by měl zadat název ovládacího prvku v e zprostředkovateli hodnoty ovládacího prvku.

    (Fragment - kódu *webových formulářů Lab – Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Tento přístup usnadňuje jednotky testování těchto metod. V kontextu testu jednotky, kde není spuštění webových formulářů, atribut [ovládacího prvku] nebude provádět žádné zvláštní akce.
4. Otevřít **Products.aspx** stránky a vyhledejte produkty ovládacího prvku GridView. Aktualizace produktů GridView obsahovat odkaz pro úpravy vybraných produktů.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Otevřít **ProductDetails.aspx** stránce použití modelu code-behind a nahraďte **SelectProduct** metodu s následujícím kódem.

    (Fragment - kódu *webovou metodu SelectProduct Lab – Ex01 – formuláře*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Všimněte si, že **[QueryString]** atribut slouží k naplnění parametru metody z parametru productId v řetězci dotazu.
6. Stisknutím klávesy **F5** spuštění ladění lokalitu a přejděte na stránku produktů. Vyberte libovolnou kategorii z kategorií GridView a Všimněte si, že se aktualizuje produkty ovládacího prvku GridView.

    ![Zobrazuje produkty z vybrané kategorie](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "zobrazující produkty z vybrané kategorie")

    *Zobrazuje produkty z vybrané kategorie*
7. Klikněte na tlačítko **zobrazení** odkaz na produktu a otevřete stránku ProductDetails.aspx.

    Všimněte si, že na stránce načítá produktu s použitím parametru productId z řetězce dotazu metoda SelectMethod.

    ![Zobrazení podrobností o produktu](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "zobrazení podrobností o produktu")

    *Zobrazení podrobností o produktu*

    > [!NOTE]
    > Možnost zadat popis HTML budou implementovány v dalším cvičení.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Úloha 5: použití modelu vazby pro operace Update

V předchozí úloze jsme použili jste vazby modelu hlavně pro výběr data, při plnění tohoto úkolu se dozvíte, jak pomocí vazby modelu v operacích aktualizace.

Budete aktualizovat kategorie ovládacího prvku GridView, umožníte uživateli aktualizovat kategorie.

1. Otevřít **Products.aspx** stránce a aktualizovat kategorie GridView automaticky generovat tlačítko Upravit a použít novou **UpdateMethod** atributy **UpdateCategory**způsob aktualizace vybranou položku.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Atributu DataKeyNames v prvku GridView definovat, které jsou členy, které jedinečně identifikují objekt svázaný se model a proto, které jsou parametry, jakou metodu aktualizace by měla minimálně.
2. Otevřít **Products.aspx.cs** soubor kódu na pozadí a implementovat **UpdateCategory** metody. Metoda by měla přijímat ID kategorie načíst aktuální kategorii, naplní hodnoty z prvku GridView a pak aktualizujte kategorii.

    (Fragment - kódu *webových formulářů Lab – Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Nové **TryUpdateModel** metody ve třídě stránky zodpovídá naplnění objektu modelu pomocí hodnot z ovládacích prvků na stránce. V tomto případě ho nahradit aktualizovanými hodnotami z aktuálního řádku prvku GridView upravovaný do **kategorie** objektu.

    > [!NOTE]
    > Dalším cvičení se vysvětlují použití ModelState.IsValid pro ověřování dat zadaných uživatelem, při úpravě objektu.
3. Spuštění tohoto webu a přejděte na stránku produktů. Upravte kategorii. Zadejte nový název a potom klikněte na tlačítko **aktualizace** a zachová tak změny.

    ![Úprava kategorií](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "úpravy kategorie")

    *Úprava kategorií*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Cvičení 2: Ověření dat

V tomto cvičení se dozvíte o nových funkcích ověřování dat v technologii ASP.NET 4.5. Nové funkce nerušivý ověření ve webových formulářů, bude rezervovat. Anotací dat ve třídách modelu aplikace bude používat pro ověřování uživatelského vstupu a nakonec se dozvíte, jak zapnout nebo vypnout žádost o ověření jednotlivých ovládacích prvků na stránce.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Úloha 1 – Nerušivý ověření

Tvary s komplexní data včetně validátory často generují příliš mnoho kódu jazyka JavaScript na stránce, což může představovat přibližně 60 % kódu. Pomocí nerušivého ověřování povoleno bude vypadat kód HTML přehlednější a tidier.

V této části vám umožní nerušivý ověření v technologii ASP.NET pro porovnání HTML kód vygenerovaný obě konfigurace.

1. Otevřete **Visual Studio 2012** a otevřete **začít** řešení nachází v **Source\Ex2 Validation\Begin** složka tohoto testovacího prostředí. Alternativně můžete pokračovat v práci na vaše stávající řešení v předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést v Průzkumníku řešení, klikněte na tlačítko **WebFormsLab** projektu **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Stisknutím klávesy **F5** spusťte webovou aplikaci. Přejděte k zákazníkům stránky a klikněte na tlačítko **přidání nového zákazníka** odkaz.
3. Klikněte pravým tlačítkem na stránku v prohlížeči a vyberte **zobrazit zdroj** možnost otevření kód HTML generovanými aplikací.

    ![Zobrazení kódu HTML na stránce](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "stránkou kódu HTML")

    *Stránkou kódu HTML*
4. Projděte si stránku zdrojový kód a Všimněte si, že technologie ASP.NET obsahuje vložený JavaScript kód a data validátory na stránce k provedení ověření a zobrazení seznamu chyb.

    ![Ověření kódu jazyka JavaScript na stránce CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "kódu jazyka JavaScript ověření CustomerDetails stránce")

    *Ověření kódu jazyka JavaScript na stránce CustomerDetails*
5. Zavřete prohlížeč a přejděte zpět do sady Visual Studio.
6. Nyní vám umožní nerušivý ověření. Otevřít **Web.Config** a vyhledejte **ValidationSettings:UnobtrusiveValidationMode** klíče v **AppSettings** části **.** Nastavte hodnotu klíče na **webových formulářů**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Tuto vlastnost lze nastavit &quot; **stránky\_zatížení** &quot; událost v případě budete chtít povolit pouze pro některé stránky Nerušivý ověření.
7. Otevřít **CustomerDetails.aspx** a stiskněte klávesu **F5** spusťte webovou aplikaci.
8. Stisknutím klávesy F12 otevřete Nástroje pro vývojáře aplikace Internet Explorer. Jakmile nástroje pro vývojáře se otevře, vyberte kartu skriptu. Vyberte **CustomerDetails.aspx** v nabídce a využijte byly načteny, že skripty požadované pro spuštění na stránce jQuery poznámku do prohlížeče z místní lokality.

    ![Načítání jQuery JavaScript soubory přímo z místního serveru služby IIS](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "načítání jQuery JavaScript soubory přímo z místního serveru služby IIS")

    *Načtení souborů JavaScriptu jQuery přímo z místního serveru služby IIS*
9. Zavřete prohlížeč se vraťte do sady Visual Studio. Otevřít **Site.Master** soubor znovu a najít **ScriptManager**. Přidejte atribut **EnableCdn** vlastnost s hodnotou **True**. Tato akce vynutí jQuery, který se má načíst z online adresy URL, nikoli z adresy URL místního webu.
10. Otevřít **CustomerDetails.aspx** v sadě Visual Studio. Stiskněte klávesu F5 ke spuštění tohoto webu. Jakmile se otevře aplikace Internet Explorer, stisknutím klávesy F12 otevřete Nástroje pro vývojáře. Vyberte **skript** kartu a potom se podívejte na rozevírací seznam. Všimněte si, že se z místní lokality, ale spíš z online jQuery CDN už načítání souborů jQuery jazyka JavaScript.

    ![Načítání jQuery JavaScript soubory z CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "načítání jQuery JavaScript soubory z CDN")

    *Načtení souborů JavaScriptu jQuery z CDN*
11. Otevřete zdrojový kód HTML stránky znovu pomocí možnosti zobrazení zdroje v prohlížeči. Všimněte si, že tím, že umožňuje nerušivý ověřování ASP.NET nahradil vložený kód jazyka JavaScript data - \*atributy.

    ![Kód pro ověření nerušivého](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Nerušivý ověřovací kód")

    *Kód pro ověření nerušivého*

    > [!NOTE]
    > V tomto příkladu jste viděli, jak se zjednodušenou souhrnu s anotací dat při ověřování jenom pár HTML a JavaScript řádků. Dříve bez nerušivý ověřování, další validačních ovládacích prvků, které přidáte, tím větší kód pro ověření jazyka JavaScript se zvýší.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Úloha 2 – ověření modelu s anotacemi dat

ASP.NET 4.5 zavádí poznámky ověření dat pro webové formuláře. Namísto toho, aby ovládací prvek ověření na každý vstup, teď můžete definovat omezení ve třídách modelu a jejich použití ve vaší webové aplikace. V této části se dozvíte, jak používat anotacemi dat pro ověření nový/upravit formulář zákazníka.

1. Otevřít **CustomerDetail.aspx** stránky. Všimněte si, že zákazník nejprve pojmenujte a druhý v **EditItemTemplate** a **InsertItemTemplate** oddíly se ověřují pomocí ovládacích prvků RequiredFieldValidator. Každý program pro ověření je přidružená k určitou podmínku, takže je potřeba zahrnout tolik validátory jako podmínek pro kontrolu.
2. Přidání anotací dat při ověření třídy modelu zákazníka. Otevřít **Customer.cs** třídy v **modelu** složky a *uspořádání* každou vlastnost pomocí atributů dat poznámky.

    (Fragment - kódu *webových formulářů Lab – Ex02 - datových poznámek*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > Rozhraní .NET framework 4.5 rozšířeno existující kolekci poznámek data. Zde je několik příkladů poznámek dat můžete použít: [CreditCard], [Phone], [EmailAddress], [Oblast], [porovnat], [Url], [FileExtensions], [povinné], [Key], [regulární výraz].
    > 
    > Některé příklady použití:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Můžete také definovat vlastní chybové zprávy v rámci každého atributu.
3. Otevřít **CustomerDetails.aspx** a odeberte všechny RequiredFieldvalidators pro pole jméno a příjmení v oddílech v EditItemTemplate a InsertItemTemplate ovládacího prvku FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Jednou z výhod používání datových poznámek je, že logiku ověřování není duplikovaná na stránkách aplikace. Po definování v modelu a použít na všech stránkách aplikace, které zpracovávají data.
4. Otevřít **CustomerDetails.aspx** použití modelu code-behind a vyhledejte metodu SaveCustomer. Tato metoda je volána při vložení nového zákazníka a přijímá parametr zákazníků od hodnoty ovládacího prvku FormView. Při mapování mezi ovládací prvky stránky a zemře parametr objektu, bude technologie ASP.NET spustit ověření modelu proti všechny poznámky data atributy a vyplnit ModelState slovníku k chybám došlo, pokud existuje.

    ModelState.IsValid pouze vrátí hodnotu true, pokud všechna pole v modelu jsou platné po provedení ověření.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Přidat **ValidationSummary** ovládacího prvku na konci stránky CustomerDetails zobrazíte seznam chyb modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** je nová vlastnost souhrnu ověření ovládacího prvku, pokud je nastavena na **true**, ovládací prvek se zobrazí chyby ze slovníku ModelState. Tyto chyby pocházejí z ověřování dat poznámky.
6. Stisknutím klávesy **F5** ke spuštění webové aplikace. Vyplňte formulář s chybné hodnoty a klikněte na tlačítko **Uložit** k provedení ověření. Všimněte si, že chyba souhrnu v dolní části.

    ![Ověřování s anotacemi dat](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "ověřování s anotacemi dat")

    *Ověřování s anotacemi dat*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Úloha 3 – zpracování chyb vlastní databázi s ModelState

V předchozí verzi aplikace webových formulářů může zpracování chyb databáze, jako je řetězec příliš dlouhý nebo porušení jedinečného klíče zahrnovat generování výjimek ve vašem úložišti kódu a pak na zpracování výjimek ve vašich kódu zobrazuje chybu. Velké množství kódu, je potřeba udělat něco poměrně jednoduché.

V aplikaci Web Forms 4.5 objekt ModelState slouží k zobrazení chyb na stránce z vašeho modelu nebo z databáze, konzistentním způsobem.

V této úloze přidáte kód ke správné zpracování výjimek databáze a zobrazit odpovídající zprávu pro uživatele pomocí objektu ModelState.

1. Zatímco aplikace stále běží, pokuste se aktualizovat název kategorie pomocí duplicitní hodnoty.

    ![Aktualizuje se kategorie s duplicitním názvem](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "aktualizuje kategorii s duplicitním názvem")

    *Aktualizuje se kategorie s duplicitním názvem*

    Všimněte si, že dojde k výjimce z důvodu &quot;jedinečný&quot; omezení **CategoryName** sloupce.

    ![Výjimky pro názvy duplicitních kategorií](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "výjimky pro názvy duplicitních kategorií")

    *Výjimky pro názvy duplicitních kategorií*
2. Zastavte ladění. V **Products.aspx.cs** soubor kódu na pozadí, aktualizace **UpdateCategory** metoda zpracovat výjimky vyvolané z databáze. Metoda SaveChanges() volání a přidejte chybu **ModelState** objektu.

    Nové **TryUpdateModel** metoda aktualizuje objekt kategorie načtených z databáze pomocí data formuláře zadaná uživatelem.

    (Fragment - kódu *webových formulářů Lab – Ex02 - UpdateCategory zpracování chyb*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > V ideálním případě je třeba určit příčinu DbUpdateException a zkontrolujte, jestli je hlavní příčinou porušení omezení unique key.
3. Otevřít **Products.aspx** a přidejte **ValidationSummary** ovládacího prvku pod kategorií GridView zobrazíte seznam chyb modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Spuštění tohoto webu a přejděte na stránku produktů. Zkuste aktualizovat název kategorie pomocí duplicitní hodnoty.

    Všimněte si, že výjimka byla zpracována a zobrazí se chybová zpráva v **ValidationSummary** ovládacího prvku.

    ![Duplicitní kategorie chyby](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "duplicitní kategorie chyby")

    *Chyba duplicitní kategorie*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Úloha 4 – žádost o ověření ve webových formulářích ASP.NET 4.5

Žádosti o ověření funkce v technologii ASP.NET poskytuje určitou úroveň výchozí ochranu před útoky skriptování napříč weby (XSS). V předchozích verzích technologie ASP.NET žádost o ověření se ve výchozím nastavení povolené a může být pouze zakázáno pro celou stránku. V nové verzi webových formulářů ASP.NET teď můžete zakázat ověření žádosti pro jeden ovládací prvek, provádět ověřování požadavků opožděné nebo přístup k datům bez ověřeného požadavku (buďte opatrní, pokud tak učiníte!).

1. Stisknutím klávesy **Ctrl + F5** spustit bez ladění lokalitu a přejděte na stránku produktů. Vyberte kategorii a potom klikněte na tlačítko **upravit** odkaz na všech produktů.
2. Zadejte popis obsahující potenciálně nebezpečný obsah, například včetně značky HTML. Přijmout oznámení o výjimce z důvodu ověření žádosti.

    ![Úpravy produkt s potenciálně nebezpečný obsah](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "úpravy produkt s potenciálně nebezpečný obsah")

    *Úpravy produkt s potenciálně nebezpečný obsah*

    ![Z důvodu ověření žádosti došlo k výjimce](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "výjimce z důvodu ověření žádosti")

    *Výjimka vyvolaná z důvodu ověření žádosti*
3. Zavřete stránku a v sadě Visual Studio, stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.
4. Otevřít **ProductDetails.aspx** stránky a vyhledejte **popis** textového pole.
5. Přidejte nové **ValidateRequestMode** vlastnost do textového pole a nastavte jej na hodnotu **zakázané**.

    Nové **ValidateRequestMode** atribut umožňuje zakázat ověření žádosti o zásady na každý ovládací prvek. To je užitečné, pokud chcete používat vstup, který může přijímat kód HTML, ale chceme zajistit ověřování práce pro zbývající části stránky.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Stisknutím klávesy **F5** ke spuštění webové aplikace. Na stránce produktu úpravy znovu otevřete a dokončete popis produktu, včetně značky HTML. Všimněte si, že teď přidáte obsah ve formátu HTML k popisu.

    ![Žádost o ověření pro popis produktu zakázané](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "žádost o ověření zakázáno pro popis produktu")

    *Žádost o ověření zakázáno pro popis produktu*

    > [!NOTE]
    > V produkční aplikace by měla úpravu kódu HTML zadaného uživatelem, aby se zajistilo se zadávají pouze bezpečné značky HTML (například neexistují žádné &lt;skript&gt; značky). K tomuto účelu můžete použít [knihovny ochrany Web Microsoft](https://www.nuget.org/packages/AntiXSS).
7. Upravte produkt znovu. Do pole Název zadejte kód HTML a klikněte na tlačítko **Uložit**. Všimněte si, že žádost o ověření je zakázané pouze pro pole Popis a zbývající pole re stále ověřen potenciálně nebezpečný obsah.

    ![Žádost o ověření povolené ve zbývající části pole](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "žádost o ověření povolené ve zbývající části pole")

    *Ověření žádosti povoleno ve zbývající části pole*

    Webové formuláře ASP.NET 4.5 obsahuje nový režim ověření požadavku laxně provádět ověřování požadavků. S režimem ověření požadavku nastavte na **4.5**, pokud se část kódu přístupy *Request.Form [&quot;klíč&quot;]*, technologii ASP.NET 4.5 žádost o ověření budou pro aktivaci jenom ověření požadavku. pro tento konkrétní element v kolekci formuláře.

    Kromě toho ASP.NET 4.5 nyní obsahuje základní rutiny kódování z knihovny Microsoft Antimalware XSS v4.0. Rutiny kódování jsou implementovány pomocí nového Anti-XSS *AntiXssEncoder* typ nalezen v novém **System.Web.Security.AntiXss** oboru názvů. S **encoderType** parametr nakonfigurován na použití *AntiXssEncoder*, veškerý výstup kódování automaticky v rámci technologie ASP.NET používá nové rutiny pro kódování.
8. ASP.NET 4.5 žádost o ověření také podporuje zrušení ověřený přístup k datům požadavku. ASP.NET 4.5 přidá nová vlastnost kolekce **HttpRequest** objektu s názvem **Unvalidated**. Když přejdete do **HttpRequest.Unvalidated** mají přístup ke všem běžné údaje žádosti o data, včetně formulářů, řetězci dotazu, soubory cookie, adresy URL a tak dále.

    ![Objekt Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated objektu")

    *Objekt Request.Unvalidated*

    > [!NOTE]
    > **Použijte prosím vlastnost HttpRequest.Unvalidated opatrně!** Ujistěte se, že provádíte pečlivě vlastní ověřovací požadavek nezpracovaných dat zajistíte, že není nebezpečné text odbavovaná a vykreslen si zákazníci!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Cvičení 3: Asynchronní zpracování stránky v ASP.NET Web Forms

V tomto cvičení vám představíme novou stránku asynchronní zpracování funkce ve webových formulářů ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Úloha 1 – aktualizaci produktu stránka k nahrání a zobrazit obrázky podrobností

V této úloze budete aktualizovat na stránce podrobností produktu umožňující uživateli zadat adresu URL obrázku pro produkt a zobrazit v zobrazení jen pro čtení. Stáhněte si ji synchronně vytvoříte místní kopii zadané bitové kopie. V dalším úkolem aktualizujte tuto implementaci, aby to fungovalo asynchronně.

1. Otevřít **Visual Studio 2012** a zatížení **začít** řešení nachází v **Source\Ex3 Async\Begin** ze složky v tomto testovacím prostředí. Alternativně můžete pokračovat v práci na vaše stávající řešení v předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést v Průzkumníku řešení, klikněte na tlačítko **WebFormsLab** projektu a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **ProductDetails.aspx** stránce zdroje a přidat pole do ovládacího prvku FormView ItemTemplate zobrazíte bitovou kopii produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Přidáte pole, které chcete zadat adresu URL obrázku ve třídě FormView EditTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Otevřít **ProductDetails.aspx.cs** použití modelu code-behind a přidejte následující direktivy oboru názvů.

    (Fragment - kódu *webových formulářů Lab – Ex03 - obory názvů*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Vytvoření **UpdateProductImage** metoda k ukládání imagí vzdálený místní **Imagí** složky a aktualizace entitou produkt s novou hodnotu bitové kopie umístění.

    (Fragment - kódu *webových formulářů Lab – Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Aktualizace **UpdateProduct** metoda se má volat **UpdateProductImage** metody.

    (Fragment - kódu *webových formulářů Lab – Ex03 - UpdateProductImage volání*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Spusťte aplikaci a zkuste nahrát obrázek pro produkt. Například můžete použít následující adresu URL image z Galerie umění Office: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Nastavení obrázku pro produkt](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "nastavení image pro produkt")

    *Nastavení obrázku pro produkt*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Úloha 2 – přidání asynchronní zpracování na stránce podrobností produktu

V této úloze budete aktualizovat na stránce podrobností produktu aby to fungovalo asynchronně. Se zlepšila dlouho běžící úloha - procesu stahování image – s použitím zpracování asynchronního stránky technologie ASP.NET 4.5.

Asynchronní metody ve webových aplikacích lze použít k optimalizaci způsob, jakým se používají fondy vláken ASP.NET. V technologii ASP.NET došlo jsou omezenému počtu vláken ve fondu vláken pro vaši účast požadavků, proto při zaneprázdněni všechna vlákna ASP.NET spustí odmítnout nové žádosti o, odešle aplikace chybové zprávy, znepřístupní webu.

Časově náročná operace na vašem webu jsou skvělými kandidáty pro asynchronní programování, protože se po dlouhou dobu zabírají přiřazené vlákno. To zahrnuje dlouho spuštěných požadavků, stránek s mnoha různým elementům a stránek, které vyžadují offline operace, odpovídající dotaz na databázi nebo přístup k externí webový server. Výhodou je, že pokud použití asynchronních metod pro tyto operace, zatímco zpracování na stránce vlákna je uvolněn a vrátí do vlákna fondu a slouží k účasti na žádost o nové stránky. To znamená, stránka se spustí v jednom vlákně z fondu vláken zpracování a může dokončit zpracování v jiný, po dokončení asynchronní zpracování.

1. Otevřít **ProductDetails.aspx** stránky. Přidat **asynchronní** atribut **stránky** elementu a nastavte ho na **true**. Tento atribut oznamuje ASP.NET, aby implementace rozhraní IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Přidejte popisek v dolní části stránky a zobrazit podrobnosti vlákna spuštěná na stránce.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Otevřete **ProductDetails.aspx.cs** a přidejte následující direktivy oboru názvů.

    (Fragment - kódu *webových formulářů Lab – Ex03 - obory názvů 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Upravit **UpdateProductImage** metoda stáhnout bitovou kopii s asynchronní úlohou. Nahradíte **WebClient** **DownloadFile** metodu s **DownloadFileTaskAsync** metoda a zahrnují **await** – klíčové slovo.

    (Fragment - kódu *webových formulářů Lab – Ex03 - UpdateProductImage asynchronní*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask zaregistruje nový asynchronní úkol stránku k provedení v jiném vlákně. Přijme výraz lambda s úlohou (t) má být proveden. **Await** – klíčové slovo v **DownloadFileTaskAsync** metoda převede zbývající metody zpětného volání, které je voláno asynchronně po **DownloadFileTaskAsync** dokončení metody. ASP.NET bude pokračovat provádění metody automaticky udržováním HTTP požadavku původní hodnoty. Nový model asynchronního programování v rozhraní .NET 4.5 umožňuje psát asynchronní kód, který vypadá velmi podobně jako synchronní kód a nechat kompilátor, aby zpracování komplikace funkce zpětného volání nebo pokračování kódu.
    > [!NOTE]
    > RegisterAsyncTask a Executeinparallel již byly k dispozici od verze rozhraní .NET 2.0. Await – klíčové slovo je nového v rozhraní .NET 4.5 asynchronní programovací model a lze použít společně se nových metod TaskAsync z objektu .NET WebClient.
5. Přidejte kód k zobrazení vláken, ve kterých kód spuštění a dokončení provádění. Chcete-li to provést, aktualizujte **UpdateProductImage** metodu s následujícím kódem.

    (Fragment - kódu *Web Forms Lab – Ex03 - Show vlákna*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Otevřít webovou stránku **Web.config** souboru. Přidejte následující proměnnou nastavení aplikace.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Stisknutím klávesy **F5** ke spuštění aplikace a nahrát obrázek pro produkt. Všimněte si, že ID vlákna, kde může být jiný kód spuštění a dokončení. Je to proto, že asynchronní úlohy spustit na samostatném vlákně z fondu vláken ASP.NET. Po dokončení úlohy ASP.NET vloží úlohu zpět do fronty a přiřadí některý z dostupných vláken.

    ![Stahuje se obrázek asynchronně](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "asynchronně stahování obrázku")

    *Asynchronní stahování obrázku*

> [!NOTE]
> Kromě toho můžete tuto aplikaci nasadíte do Azure následujícím [příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V této praktické laboratoři jste zákazníky a vyřešené následující pojmy a jsme vám ukázali:

- Použití silně typovaných výrazů vázání dat
- Používat nové funkce vazby modelu ve webových formulářů
- Použití zprostředkovatele hodnot pro použití modelu code-behind metody mapování data stránky
- Použití anotací dat pro ověřování uživatelského vstupu
- Využijte advange unobstrusive ověřování na straně klienta s jQuery ve webových formulářů
- Implementace detailní žádost o ověření
- Implementace asynchronního zpracování ve webových formulářích stránky

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express for Web dlaždice*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha B: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

Tento dodatek se ukazují, jak vytvořit nový web z portálu Azure Portal a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy publikační funkci poskytovaný platformou Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Úloha 1 – Vytvoření nového webu na webu Azure Portal

1. Přejděte [portálu pro správu Azure](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.

    > [!NOTE]
    > S Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **Compute** | **webu**. Potom vyberte **rychlé vytvoření** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.

    > [!NOTE]
    > Azure je hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat. Možnost rychle vytvořit můžete nasadit hotové webové aplikace do Azure z mimo portál. Nezahrnuje kroky pro vytvoření databáze.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** se vytvoří.
5. Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce. Zkontrolujte, jestli funguje nový web.

    ![Na nový web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "přechodu na nový web")

    *Procházení na nový web*

    ![Spuštění webu](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "spuštění webové stránky")

    *Spuštění webové stránky*
6. Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.

    ![Otevřete správu webových stránek](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "otevřete správu webových stránek")

    *Otevřete správu webových stránek*
7. V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.

    > [!NOTE]
    > *Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací do Azure pro každou metodu povoleno publikování. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací do Azure.

    ![Stahování webové stránky publikovat profil](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "stahování webové stránky profil publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profilu publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci do Azure ze sady Visual Studio pomocí tohoto souboru.

    ![Ukládání souboru profilu publikování](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "ukládá se profil publikování")

    *Ukládá se profil publikování*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.

1. Pro uložení databáze aplikace budete potřebovat databázi SQL serveru. Servery SQL Database můžete zobrazit ze svého předplatného na portálu Azure Management portal na **databází Sql** | **servery** | **řídicího panelu serveru**. Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů. Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly. Nevytvářet databáze, protože vytvoří se v pozdější fázi.

    ![Řídicí panel serveru SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "řídicího panelu serveru SQL Database")

    *Řídicí panel serveru SQL Database*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) tlačítko.

    ![Přidat IP adresu klienta](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Přidat IP adresu klienta*
3. Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.

    ![Potvrzení změn](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Potvrzení změn*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

1. Vraťte se do řešení ASP.NET MVC 4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "publikování aplikace")

    *Publikování na webu*
2. Importujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "import profilu publikování")

    *Import publikačního profilu*
3. Klikněte na tlačítko **ověřit připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.

    ![Ověřuje se připojení](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "ověřuje se připojení")

    *Ověřuje se připojení*
4. V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Konfigurace připojení k databázi následujícím způsobem:

   - V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.
   - Zadejte nový název databáze.

     ![Konfigurace cílový připojovací řetězec](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.

    ![Vytvoření databáze](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "vytvoření řetězce databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL v Azure se zobrazí v textovém poli výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "připojovací řetězec odkazující na SQL Database")

    *Připojovací řetězec odkazující na SQL Database*
8. V **ve verzi Preview** klikněte na **publikovat**.

    ![Publikování webové aplikace](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Příloha C: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

![Začněte psát název fragmentu kódu](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)*** 1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.

1. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
2. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*
