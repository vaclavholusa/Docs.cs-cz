---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Vytvoření základní technologie ASP.NET 4.5 webové stránky formuláře v sadě Visual Studio 2013 | Dokumentace Microsoftu
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: c2051b166b8800654a107c5100ad5ed94af93407
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394450"
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Vytvoření základní technologie ASP.NET 4.5 webové stránky formuláře v sadě Visual Studio 2013
====================
podle [Erik Reitan](https://github.com/Erikre)

Tento názorný postup obsahuje úvod do prostředí vývoje webu v [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) a [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Tento názorný postup vás provede procesem vytvoření jednoduché stránky webových formulářů ASP.NET a znázorňuje základní postupy vytvoření nové stránky, přidání ovládacích prvků a psaním kódu.

Úlohy v tomto návodu zahrnují:

- Vytvoření projektu aplikace webových formulářů systému souborů.
- Seznámení se sadou Visual Studio.
- Vytvoření stránky technologie ASP.NET.
- Přidání ovládacích prvků.
- Přidání obslužných rutin událostí.
- Spuštění a testování stránky ze sady Visual Studio.

## <a name="prerequisites"></a>Požadavky


K dokončení tohoto návodu budete potřebovat:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Rozhraní .NET Framework se instaluje automaticky. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 for Web bude často se označuje jako Visual Studio v celé této sérii kurzů.  
    >   
    > Pokud používáte Visual Studio, Tento názorný průvodce předpokládá, že jste vybrali **vývoj pro Web** kolekce nastavení při prvním spuštění sady Visual Studio. Další informace najdete v tématu [postupy: Výběr nastavení prostředí vývoje webu](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Vytvoření projektu webové aplikace a na stránce

<a id="sectionToggle0"></a>

V této části tohoto návodu vytvoříte projekt webové aplikace a přidejte novou stránku do ní. Bude také přidat HTML text a spuštění stránky v prohlížeči.

### <a name="to-create-a-web-application-project"></a>Chcete-li vytvořit projekt webové aplikace

1. Otevřete Microsoft Visual Studio.
2. Na **souboru** nabídce vyberte možnost **nový projekt**.  
    ![Nabídka Soubor](creating-a-basic-web-forms-page/_static/image1.png)

    **Nový projekt** zobrazí se dialogové okno.
3. Vyberte **šablony**  - &gt; **Visual C#**  - &gt; **webové** šablony skupiny na levé straně.
4. Zvolte **webová aplikace ASP.NET** šablon v prostředním sloupci.
5. Pojmenujte svůj projekt ***BasicWebApp*** a klikněte na tlačítko **OK** tlačítko.   
![Dialogové okno Nový projekt](creating-a-basic-web-forms-page/_static/image2.png)
6. V dalším kroku vyberte **webových formulářů** šablony a kliknutím **OK** tlačítko pro vytvoření projektu.  
![Dialogové okno Nový projekt ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio vytvoří nový projekt, který obsahuje předem připravených funkce na základě šablony webových formulářů. To poskytuje nejen vám *Home.aspx* stránky, *About.aspx* stránky, *Contact.aspx* stránce, ale také zahrnuje funkce členství, který registruje uživatele a uloží jejich pověření tak, aby se můžete přihlásit na web. Když se vytvoří nová stránka, ve výchozím nastavení sada Visual Studio zobrazí stránku v **zdroj** zobrazení, kde můžete vidět prvky jazyka HTML na stránce. Následující obrázek znázorňuje, co se zobrazí v **zdroj** zobrazit v případě, že jste vytvořili novou webovou stránku s názvem *BasicWebApp.aspx*.  
    ![Zobrazení zdroje](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Prohlídka prostředí vývoj webu sady Visual Studio


Než budete pokračovat úpravou stránky, je vhodné se seznámit s vývojovým prostředím sady Visual Studio. Následující obrázek ukazuje windows a nástroje, které jsou k dispozici v sadě Visual Studio a Visual Studio Express for Web.

> [!NOTE] 
> 
> Tento diagram zobrazuje výchozí a umístění oken. **Zobrazení** nabídka umožňuje zobrazit další okna a změna uspořádání a změna velikosti windows podle vlastních potřeb. Pokud již byly provedeny změny uspořádání oken, nebudou odpovídat, co vidíte na obrázku.


 Prostředí sady Visual Studio

![Prostředí sady Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Seznamte se s webový Návrhář

Prohlédněte si výše uvedené obrázek a hledat shodu textů v následujícím seznamu, který popisuje nejčastěji používaná windows a nástroje. (Ne všechna okna a nástroje, které se zobrazí, tady jsou uvedené pouze ty označené v předchozí ilustraci.)

- Panely nástrojů. Poskytují příkazy pro formátování textu, vyhledání textu a tak dále. Některé panely nástrojů jsou k dispozici pouze tehdy, když pracujete **návrhu** zobrazení.
- **Průzkumník řešení** okna. Zobrazí soubory a složky ve webové aplikaci.
- Okno dokumentu. Zobrazuje dokumenty, které pracujete v systému windows s kartami. Můžete přepínat mezi dokumenty kliknutím na karty.
- **Vlastnosti** okna. Umožňuje změnit nastavení na stránce, elementy HTML, ovládací prvky a dalších objektů.
- Zobrazení karty. K dispozici s různá zobrazení stejný dokument. **Návrh** zobrazení je téměř WYSIWYG úprav plochy. **Zdroj** zobrazení je editor HTML stránky. **Rozdělení** zobrazení ukazuje, jak **návrhu** zobrazení a **zdroj** zobrazení dokumentu. Bude pracovat **návrhu** a **zdroj** zobrazení později v tomto návodu. Pokud dáváte přednost k otevírání webových stránek v **návrhu** zobrazení na **nástroje** nabídky, klikněte na tlačítko **možnosti**, vyberte **Návrháři HTML** uzlu a změňte **Spustit stránek v** možnost.
- **Panel nástrojů**. Obsahuje ovládací prvky a elementy HTML, které můžete přetáhnout do stránky. **Panel nástrojů** prvky jsou seskupeny podle běžné funkce.
- S **erver Explorer**. Zobrazí připojení k databázi. Pokud se nezobrazí Průzkumník serveru, v nabídce zobrazit kliknutím na Průzkumníka serveru.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Vytváří se nové technologie ASP.NET webové stránky s formuláři


Při vytváření nové aplikace webových formulářů pomocí **webová aplikace ASP.NET** šablony projektu, Visual Studio přidá stránky ASP.NET (webové formuláře – stránka) s názvem *Default.aspx*, stejně jako několik dalších souborů a složky. Můžete použít *Default.aspx* stránku jako domovské stránky pro webové aplikace. Ale v tomto návodu vytvoříte a pracovat s novou stránku.

### <a name="to-add-a-page-to-the-web-application"></a>Přidání stránky do webové aplikace


1. Zavřít *Default.aspx* stránky. Chcete-li to provést, klikněte na kartu, která zobrazuje název souboru a pak klikněte na možnost Zavřít.
2. V **Průzkumníka řešení**, klikněte pravým tlačítkem na název webové aplikace (v tomto kurzu je název aplikace **BasicWebSite**) a potom klikněte na tlačítko **přidat**  - &gt; **Nová položka**.   
**Přidat novou položku** se zobrazí dialogové okno.
3. Vyberte **Visual C#**  - &gt; **webové** šablony skupiny na levé straně. Vyberte **webový formulář** uprostřed seznamu a pojmenujte ho *FirstWebPage.aspx*.   
    ![Přidat novou položku – dialogové okno](creating-a-basic-web-forms-page/_static/image6.png)
4. Klikněte na tlačítko **přidat** do svého projektu přidat webovou stránku.  
Visual Studio vytvoří novou stránku a otevře jej.


### <a name="adding-html-to-the-page"></a>Přidání kódu HTML na stránce


V této části Průvodce přidáte některé statický text na stránce.

### <a name="to-add-text-to-the-page"></a>Chcete-li přidat text na stránce


1. V dolní části okna dokumentu, klikněte na tlačítko **návrhu** tab přepnete na **návrhu** zobrazení.

    Návrhové zobrazení zobrazí aktuální stránku WYSIWYG jako způsobem. V tuto chvíli nemáte žádné text nebo ovládací prvky na stránce tak stránka je prázdná, s výjimkou na přerušovanou čáru, která bude uvádět obdélníku. Představuje obdélníku **div** elementu na stránce.
2. Klikněte do obdélníku, který je popsaný přerušovanou čárou.
3. Typ **Vítá vás Visual Web Developer** a stiskněte klávesu **ENTER** dvakrát.

    Následující obrázek znázorňuje text, který jste zadali v **návrhu** zobrazení.

    ![Úvodní text v návrhovém zobrazení](creating-a-basic-web-forms-page/_static/image7.png "uvítací text v návrhovém zobrazení")
4. Přepnout na **zdroj** zobrazení.

    Zobrazí se kód HTML v **zdroj** zobrazení, které jste vytvořili, když jste zadali v **návrhu** zobrazení.  
    ![Webovou stránku pomocí statického textu](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Spuštění stránky


Než budete pokračovat přidáním ovládacích prvků na stránce, které můžete ji spustit.

### <a name="to-run-the-page"></a>Ke spuštění stránky


1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na *FirstWebPage.aspx* a vyberte **nastavit jako úvodní stránku**.
2. Stisknutím klávesy **CTRL + F5** ke spuštění stránky.

    Na stránce se zobrazí v prohlížeči. I když jste vytvořili stránka obsahuje příponu názvu souboru *.aspx*, stejně jako libovolnou stránku HTML aktuálně běží.

    Zobrazení stránky v prohlížeči můžete můžete také kliknout pravým tlačítkem na stránce v **Průzkumníka řešení** a vyberte **zobrazit v prohlížeči**.
3. Zavřete prohlížeč Zastavit webovou aplikaci.


## <a name="adding-and-programming-controls"></a>Přidání a programování ovládacích prvků


<a id="sectionToggle1"></a>

Nyní přidáte serverových ovládacích prvků na stránce. Serverové ovládací prvky, jako jsou tlačítka, popisky, textová pole a další dobře známé ovládací prvky poskytují typické možnosti zpracování formulářů pro stránky webových formulářů. Však můžete naprogramovat ovládacích prvků pomocí kódu, který běží na serveru, nikoli na klientovi.

Přidáte [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládací prvek, [textového pole](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) ovládací prvek a [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládací prvek na stránce a napište kód pro zpracování [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) událost pro [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku.

### <a name="to-add-controls-to-the-page"></a>Chcete-li přidat ovládací prvky na stránce


1. Klikněte na tlačítko **návrhu** tab přepnete na **návrhu** zobrazení.
2. Umístěte kurzor na konci **Vítá vás Visual Web Developer** text a stiskněte klávesu **ENTER** uvolnit místo v pěti nebo několikrát **div** pole elementu.
3. V **nástrojů**, rozbalte **standardní** skupiny, pokud ještě není rozbalen.  
Všimněte si, že budete muset Rozbalit **nástrojů** na levé straně zobrazte okno.
4. Přetáhněte [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) na stránce ovládací prvek a umístěte ho uprostřed **div** pole elementu, který má **Vítá vás Visual Web Developer** na prvním řádku.
5. Přetáhněte [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) na stránce ovládací prvek a umístěte ho na pravé straně [textového pole](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) ovládacího prvku.
6. Přetáhněte [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) na stránce ovládací prvek a umístěte ho na samostatný řádek níže [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku.
7. Umístěte kurzor nad [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) ovládací prvek a potom zadejte **zadejte své jméno:** .

    Tento statický text ve formátu HTML je titulek [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) ovládacího prvku. Můžete kombinovat statický kód HTML a serverových ovládacích prvků na stejné stránce. Následující obrázek znázorňuje, jak se zobrazí tři ovládací prvky v **návrhu** zobrazení.

    ![Tři ovládací prvky v návrhovém zobrazení](creating-a-basic-web-forms-page/_static/image9.png "tři ovládací prvky v návrhovém zobrazení")


### <a name="setting-control-properties"></a>Nastavení vlastností ovládacího prvku


Visual Studio nabízí různé způsoby, jak nastavit vlastnosti ovládacích prvků na stránce. V této části Průvodce se nastavit vlastnosti v obou **návrhu** zobrazení a **zdroj** zobrazení.

### <a name="to-set-control-properties"></a>Chcete-li nastavit vlastnosti ovládacího prvku


1. Nejprve zobrazte **vlastnosti** windows tak, že vyberete **zobrazení** nabídka -&gt; **ostatní Windows**  - &gt; **Properies okno**. Můžete také vybrat **F4** zobrazíte **vlastnosti** okna.
2. Vyberte [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku a pak v **vlastnosti** okno, nastavte hodnotu **Text** k **zobrazovaný název**. Text, který jste zadali se zobrazí na tlačítku v návrháři, jak je znázorněno na následujícím obrázku.

    ![Nastavit text na tlačítku](creating-a-basic-web-forms-page/_static/image10.png "tlačítko Nastavit text")
3. Přepnout na **zdroj** zobrazení.

    **Zdroj** zobrazení ve zdroji HTML na stránce, včetně prvků, které vytvořila aplikace Visual Studio pro ovládací prvky serveru. Ovládací prvky jsou deklarovány pomocí HTML syntaxe, s tím rozdílem, že značky použijte předponu **asp:** a obsahují atribut **runat =&quot;server&quot;**.

    Vlastnosti ovládacích prvků jsou deklarovány jako atributy. Například pokud nastavíte [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) vlastnost [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládací prvek, v kroku 1, byly ve skutečnosti nastavení **Text** atribut ve značce ovládacího prvku.

    > [!NOTE] 
    > 
    > Všechny ovládací prvky jsou uvnitř **formuláře** element, který má také atribut **runat =&quot;server&quot;**. **Runat =&quot;server&quot;**  atribut a **asp:** předpona značky ovládacího prvku označení ovládacích prvků tak, aby se při spuštění stránky zpracovávají na serveru pomocí technologie ASP.NET. Kód mimo **&lt;tvoří runat =&quot;server&quot; &gt;** a **&lt;skriptu runat =&quot;server&quot; &gt;** prvky se odešle do prohlížeče, což je důvod, proč kódu ASP.NET musí být uvnitř elementu, jehož počáteční značka obsahuje beze změny **runat =&quot;server&quot;**  atribut.
4. V dalším kroku přidáte další vlastnost [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku. Umístěte kurzor přímo po **asp: Label** v **&lt;asp: Label&gt;** značku a potom stiskněte klávesu **MEZERNÍK**.

    Se zobrazí rozevírací seznam, který zobrazí seznam dostupných vlastností pro můžete nastavit [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku. Tato funkce označuje jako **IntelliSense**, pomůže vám v **zdroj** zobrazení se syntaxí File://Server.Fabrikam.com/SD serverové ovládací prvky, prvků HTML a dalších položek na stránce. Je vidět na následujícím obrázku **IntelliSense** rozevíracího seznamu pro [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku.

    ![Technologie IntelliSense atributy](creating-a-basic-web-forms-page/_static/image11.png "atributy technologie IntelliSense")
5. Vyberte **ForeColor** a potom zadejte znaménko rovná se.

    Technologie IntelliSense zobrazí seznam barev.

    > [!NOTE] 
    > 
    > Můžete zobrazit **IntelliSense** rozevíracího seznamu kdykoli stisknutím kombinace kláves **CTRL + J** při prohlížení kódu.
6. Vyberte barvu pro **[popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** text ovládacího prvku. Ujistěte se, zda že jste vybrali barva, která je dostatečně tmavá ke čtení na bílé pozadí.

    **ForeColor** atribut se dokončila s barvu, která jste vybrali, včetně uzavírací uvozovky.


### <a name="programming-the-button-control"></a>Programování ovládacího prvku tlačítko


V tomto návodu budete psát kód, který čte název, že uživatel zadá do textového pole a potom zobrazí název v [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku.

### <a name="add-a-default-button-event-handler"></a>Přidat obslužnou rutinu výchozí tlačítko události


1. Přepnout na **návrhu** zobrazení.
2. Dvakrát klikněte [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku.

    Ve výchozím nastavení, Visual Studio se přepne do souboru kódu na pozadí a vytvoří kostru obslužné rutiny události pro [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) výchozí událost ovládacího prvku [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) událostí. Soubor modelu code-behind odděluje váš kód uživatelského rozhraní (jako je HTML) z kódu serveru (jako je C#).   
   Kurzor je nastavený na Přidat kód pro tuto obslužnou rutinu události.

    > [!NOTE] 
    > 
    > Poklepání na ovládací prvek v **návrhu** zobrazení je pouze jedním z několika způsoby, jak můžete vytvářet obslužné rutiny událostí.
3. Uvnitř **Button1\_klikněte na tlačítko** obslužná rutina události, typ **Label1** následovaných tečkou (**.**).

    Když zadáte tečku po **ID** popisku (**Label1**), Visual Studio zobrazí seznam dostupných členů pro [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) řídit, jak je znázorněno v následujícím obrázek. Člen běžně vlastnosti, metody nebo události.

    ![Technologie IntelliSense v zobrazení kódu](creating-a-basic-web-forms-page/_static/image12.png "technologie IntelliSense v zobrazení kódu")
4. Dokončit **klikněte na tlačítko** obslužné rutiny události pro tlačítko tak, že přečte, jak je znázorněno v následujícím příkladu kódu.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Přepněte zpět na zobrazení **zdroj** zobrazení kódu HTML kliknutím pravým tlačítkem myši *FirstWebPage.aspx* v **Průzkumníka řešení** a vyberete **zobrazení Značky**.
6. Přejděte **&lt;asp: Button&gt;** elementu. Všimněte si, že **&lt;asp: Button&gt;** element má nyní atribut **onclick =&quot;Button1\_klikněte na tlačítko&quot;**.

    Tento atribut vytvoří vazbu na tlačítko [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) událostí k metodě obslužné rutiny zakódované v předchozím kroku.

    Obslužné rutiny události můžete mít libovolný název. název, který se zobrazí se výchozí název vytvořený pomocí sady Visual Studio. Důležité je, že bude použit název **OnClick** atribut v kódu HTML musí odpovídat názvu metody definované v modelu code-behind.


### <a name="running-the-page"></a>Spuštění stránky


Teď můžete otestovat serverových ovládacích prvků na stránce.

### <a name="to-run-the-page"></a>Ke spuštění stránky


1. Stisknutím klávesy **CTRL + F5** ke spuštění stránky v prohlížeči. Pokud dojde k chybě, spusťte opětovnou kontrolu výše uvedených kroků.
2. Zadejte název do textového pole a klikněte na tlačítko **zobrazovaný název** tlačítko.

    Zadaný název se zobrazí v [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku. Všimněte si, že když kliknete na tlačítko, na stránce publikování na webový server. ASP.NET potom znovu vytvoří na stránce, spustí váš kód (v tomto případě [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) spustí obslužnou rutinu události) a pak odešle novou stránku v prohlížeči. Pokud sledujete stavový řádek v prohlížeči, uvidíte, že na stránce dosahuje odezvy na webový server pokaždé, když kliknete na tlačítko.
3. V prohlížeči zobrazit zdroj stránky spustíte tak, že kliknete pravým tlačítkem na stránce a vyberete **zobrazit zdroj**.

    Ve zdrojovém kódu stránky zobrazí bez jakékoli serverový kód HTML. Konkrétně se nezobrazí **&lt;asp:&gt;** prvky, které jste pracovali v **zdroj** zobrazení. Při spuštění stránky ASP.NET zpracovává serverové ovládací prvky a vykreslí elementy HTML na stránce, které provádějí funkce, které představují ovládací prvek. Například **&lt;asp: Button&gt;** ovládací prvek vykreslen jako kód HTML **&lt;typ vstupu =&quot;odeslat&quot; &gt;** element.
4. Zavřete prohlížeč.


## <a name="working-with-additional-controls"></a>Práce s další ovládací prvky

<a id="sectionToggle2"></a>

V této části tohoto návodu budete pracovat s [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) ovládací prvek, který zobrazuje data za měsíc v čase. [Kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) ovládací prvek je ovládací prvek složitější než tlačítko, textového pole a popisek, pracujete s a ukazuje některé další možnosti serverových ovládacích prvků.

V této části, které přidáte [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) ovládací prvek na stránce a naformátovat ho.

### <a name="to-add-a-calendar-control"></a>Přidání ovládacího prvku Kalendář


1. V sadě Visual Studio, přepněte na **návrhu** zobrazení.
2. Z **standardní** část **nástrojů**, přetáhněte [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) na stránce ovládací prvek a umístěte ho níže **div** element, který obsahuje další ovládací prvky.

    Zobrazí se panel inteligentních značek v kalendáři. Panel zobrazuje příkazy, které usnadňují provádění nejběžnějších úloh pro vybraný ovládací prvek. Je vidět na následujícím obrázku [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) řídit, jak je vykreslen v **návrhu** zobrazení.

    ![Ovládací prvek v návrhovém zobrazení Kalendář](creating-a-basic-web-forms-page/_static/image13.png "ovládací prvek v návrhovém zobrazení Kalendář")
3. V panelu inteligentních značek zvolte **automatický formát**.

    **Automatický formát** se zobrazí dialogové okno, které vám umožní vybrat schéma formátování pro kalendář. Je vidět na následujícím obrázku **automatický formát** dialogové okno pro [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) ovládacího prvku.

    ![Dialogové okno formátu automaticky (ovládací prvek Calendar)](creating-a-basic-web-forms-page/_static/image14.png "dialogové okno automatického formátu (ovládací prvek Kalendář)")
4. Z **vyberte schéma** vyberte **jednoduché** a potom klikněte na tlačítko **OK**.
5. Přepnout na **zdroj** zobrazení.

    Zobrazí se **&lt;asp: kalendáře&gt;** elementu. Tento element má mnohem déle, než prvky pro jednoduché ovládací prvky, které jste vytvořili dříve. Také obsahuje dílčí prvky, jako například  **&lt;WeekEndDayStyle&gt;**, které představují různé nastavení formátování. Je vidět na následujícím obrázku [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) v ovládacím prvku **zdroj** zobrazení. (Přesný kód, který se zobrazí v **zdroj** zobrazení se mohou mírně lišit od na obrázku.)

    ![Ovládací prvek ve zdrojovém zobrazení Kalendář](creating-a-basic-web-forms-page/_static/image15.png "ovládacího prvku v zobrazení zdroje v kalendáři")


### <a name="programming-the-calendar-control"></a>Programování ovládacího prvku Kalendář


V této části programu [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) ovládací prvek pro zobrazení aktuálně vybrané datum.

### <a name="to-program-the-calendar-control"></a>Chcete-li aplikaci prvku kalendáře


1. V **návrhu** , poklikejte na [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) ovládacího prvku.

    Novou obslužnou rutinu události se vytvoří a zobrazí v souboru kódu na pozadí s názvem *FirstWebPage.aspx.cs*.
2. Dokončit [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) obslužné rutiny události s následujícím kódem.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Ve výše uvedeném kódu nastaví text ovládacího prvku popisku k vybraným datem ovládacího prvku kalendář.


### <a name="running-the-page"></a>Spuštění stránky


Teď můžete otestovat v kalendáři.

### <a name="to-run-the-page"></a>Ke spuštění stránky


1. Stisknutím klávesy **CTRL + F5** ke spuštění stránky v prohlížeči.
2. Klikněte na datum v kalendáři.

    Datum jste klikli, se zobrazí v [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku.
3. V prohlížeči zobrazte zdrojový kód pro stránku.

    Všimněte si, že [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) poskytla ovládací prvek na stránce jako **tabulky**, se každý den jako **td** elementu.
4. Zavřete prohlížeč.


## <a name="next-steps"></a>Další kroky


<a id="nextStepsToggle"></a>

Tento návod znázorňuje základní funkce stránky návrháře aplikace Visual Studio. Teď, když víte, jak vytvářet a upravovat stránky s webovými formuláři v aplikaci Visual Studio, můžete chtít prozkoumat další funkce. Můžete například chtít postupujte takto:

- Další informace o webových formulářích ASP.NET podle série podrobných kurzů [Začínáme se službou webových formulářů ASP.NET 4.5 a Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Další informace o šablonách Cascading šablony stylů (CSS). Podrobnosti najdete v tématu [šablon stylů CSS Přehled práce s](https://msdn.microsoft.com/library/bb398931.aspx).
