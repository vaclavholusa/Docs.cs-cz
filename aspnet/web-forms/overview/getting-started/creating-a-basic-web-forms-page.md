---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: "Vytvoření základní technologie ASP.NET 4.5 webové stránky formuláře v sadě Visual Studio 2013 | Microsoft Docs"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 6b699cc939292b7ab0167dba7cfa6a00b681ef3a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Vytvoření základní technologie ASP.NET 4.5 webové stránky formuláře v sadě Visual Studio 2013
====================
Podle [Erik Reitan](https://github.com/Erikre)

Tento návod vám poskytne úvod do vývojového prostředí webu v [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) a v [Microsoft Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Tento návod provede vás procesem vytvoření jednoduché stránky webových formulářů ASP.NET a znázorňuje základní postupy při vytváření nové stránky, přidání ovládacích prvků a psaní kódu.

Úkoly v tomto návodu zahrnují:

- Vytvoření projektu aplikace webových formulářů systému souborů.
- Seznámení se s Visual Studio.
- Vytvoření stránky ASP.NET.
- Přidání ovládacích prvků.
- Přidání obslužných rutin událostí.
- Spuštění a testování stránka v sadě Visual Studio.

## <a name="prerequisites"></a>Požadavky


K dokončení tohoto návodu, budete potřebovat:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [sady Microsoft Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Rozhraní .NET Framework se instaluje automaticky. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 pro Web se často se označuje jako Visual Studio v rámci tohoto kurzu řady.  
    >   
    > Pokud používáte Visual Studio, tento návod předpokládá, že jste vybrali **vývoj webů** kolekce nastavení při prvním spuštění sady Visual Studio. Další informace najdete v tématu [postupy: nastavení prostředí vyberte vývoj webové](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Vytvoření projektu webové aplikace a stránky

<a id="sectionToggle0"></a>

V této části průvodce vytvoříte projekt webové aplikace a do ní přidejte nová stránka. Také přidat text, HTML a spusťte stránku v prohlížeči.

### <a name="to-create-a-web-application-project"></a>Chcete-li vytvořit projekt webové aplikace

1. Open Microsoft Visual Studio.
2. Na **soubor** nabídce vyberte možnost **nový projekt**.  
    ![Nabídka Soubor](creating-a-basic-web-forms-page/_static/image1.png)

    **Nový projekt** zobrazí se dialogové okno.
3. Vyberte **šablony**  - &gt; **Visual C#**  - &gt; **webové** skupiny šablony na levé straně.
4. Vyberte **webové aplikace ASP.NET** šablony v prostředním sloupci.
5. Název projektu ***BasicWebApp*** a klikněte na **OK** tlačítko.   
![Dialogové okno Nový projekt](creating-a-basic-web-forms-page/_static/image2.png)
6. Potom vyberte **webových formulářů** šablonu a klikněte **OK** tlačítko pro vytvoření projektu.  
![Dialogové okno Nový projekt ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio vytvoří nový projekt, který zahrnuje předkompilované funkce na základě šablony webových formulářů. Nejenom, poskytne vám s *Home.aspx* stránka, *About.aspx* stránky, *Contact.aspx* stránky, ale také zahrnuje funkce členství, který registruje uživatele a uloží své přihlašovací údaje, aby se můžete přihlásit na váš web. Když se vytvoří novou stránku, ve výchozím nastavení sady Visual Studio zobrazí stránku v **zdroj** zobrazení, kde se můžete podívat elementů HTML stránky. Následující obrázek znázorňuje, co by uvidí v **zdroj** zobrazit, pokud jste vytvořili novou webovou stránku s názvem *BasicWebApp.aspx*.  
    ![Zobrazení zdroje](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Prohlídka prostředí vývoj webu sady Visual Studio


Před pokračováním úpravou stránky, je vhodné se seznámit s vývojovém prostředí sady Visual Studio. Následující obrázek ukazuje, systém windows a nástroje, které jsou k dispozici v sadě Visual Studio a Visual Studio Express pro Web.

> [!NOTE] 
> 
> Tento diagram znázorňuje výchozí a okno umístění. **Zobrazení** nabídky umožňuje zobrazit další okna a změna uspořádání a změna velikosti windows podle vlastních potřeb. Pokud již byly provedeny změny uspořádání oken, nebude odpovídat co vidíte na obrázku.


 Prostředí sady Visual Studio

![Visual Studio Environment](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Seznamte se s návrhářem webu

Prohlédněte si výše uvedený obrázek a shodují s textem následující seznam, který popisuje nejčastěji používaná windows a nástroje. (Ne všechny windows a nástroje, které vidíte, že jsou zde uvedeny, pouze ty označené v předchozí ilustraci.)

- Panely nástrojů. Zadejte příkazy pro formátování textu, hledání textu a tak dále. Některé panely nástrojů jsou k dispozici jenom v případě, že pracujete **návrhu** zobrazení.
- **Průzkumník řešení** okno. Zobrazí soubory a složky, ve vaší webové aplikaci.
- Okna dokumentu. Zobrazí dokumenty, které pracujete v záložkách systému windows. Můžete přepínat mezi dokumenty kliknutím na karty.
- **Vlastnosti** okno. Umožňuje změnit nastavení pro stránky, elementy HTML, ovládací prvky a jiné objekty.
- Zobrazení karty. Znamenat různá zobrazení stejného dokumentu. **Návrh** zobrazení je téměř WYSIWYG úpravy plochy. **Zdroj** zobrazení je editor HTML pro stránku. **Rozdělení** zobrazení ukazuje, jak **návrhu** zobrazení a **zdroj** zobrazení pro dokument. Bude fungovat s **návrhu** a **zdroj** zobrazení dále v tomto návodu. Pokud dáváte přednost k otevírání webových stránek v **návrhu** zobrazení na **nástroje** nabídky, klikněte na tlačítko **možnosti**, vyberte **Návrhář HTML** uzlu a změňte **Spouštět stránky v** možnost.
- **Sada nástrojů**. Poskytuje a ovládacích prvků HTML, které můžete přetáhnout na stránku. **Sada nástrojů** prvky jsou seskupeny podle běžné funkce.
- S **erver Explorer**. Zobrazí připojení databáze. Pokud Průzkumníka serveru není viditelný, klikněte v nabídce Zobrazení Průzkumníka serveru.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Vytvoření nové stránky webového formuláře ASP.NET


Při vytváření nové aplikace webových formulářů pomocí **webové aplikace ASP.NET** šablona projektu sady Visual Studio přidá stránku ASP.NET (stránky webových formulářů) s názvem *Default.aspx*, stejně jako několik dalších souborů a složky. Můžete použít *Default.aspx* stránky jako domovské stránky pro webovou aplikaci. Ale v tomto návodu vytvoříte a pracovat s novou stránku.

### <a name="to-add-a-page-to-the-web-application"></a>Přidání stránky do webové aplikace


1. Zavřít *Default.aspx* stránky. Chcete-li to provést, klikněte na kartu, která zobrazuje název souboru a pak klikněte na možnost Zavřít.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název webové aplikace (v tomto kurzu, název aplikace je **BasicWebSite**) a potom klikněte na **přidat**  - &gt; **Novou položku**.   
**Přidat novou položku** se zobrazí dialogové okno.
3. Vyberte **Visual C#**  - &gt; **webové** skupiny šablony na levé straně. Pak vyberte **webového formuláře** ze středu seznamu a pojmenujte ji *FirstWebPage.aspx*.   
    ![Přidat novou položku – dialogové okno](creating-a-basic-web-forms-page/_static/image6.png)
4. Klikněte na tlačítko **přidat** přidání webové stránky do projektu.  
Visual Studio vytvoří novou stránku a otevře ji.


### <a name="adding-html-to-the-page"></a>Přidání stránky HTML


V této části Průvodce přidáte některé statický text na stránku.

### <a name="to-add-text-to-the-page"></a>Chcete-li přidat text na stránku


1. V dolní části okna dokumentu, klikněte na položku **návrhu** tab přepněte do **návrhu** zobrazení.

    Návrhové zobrazení zobrazí aktuální stránku způsobem zobrazení WYSIWYG podobné. V tomto okamžiku jste jakýkoli text nebo ovládací prvky na stránce, stránka je prázdná, až na přerušovanou čáru, která popisuje obdélníku. Představuje obdélníku **div** elementu na stránce.
2. Klikněte do obdélníku, která je podle uvedeného přerušovanou čárou.
3. Typ **Vítá vás Visual Web Developer** a stiskněte klávesu **ENTER** dvakrát.

    Následující obrázek znázorňuje text, který jste zadali v **návrhu** zobrazení.

    ![Úvodní text v zobrazení návrhu](creating-a-basic-web-forms-page/_static/image7.png "uvítací text v zobrazení návrhu")
4. Přepnout na **zdroj** zobrazení.

    Zobrazí se ve formátu HTML v **zdroj** zobrazení, které jste vytvořili, když jste zadali v **návrhu** zobrazení.  
    ![Webové stránky s statický Text](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Spuštění stránky


Než budete pokračovat přidáním ovládacích prvků na stránku, můžete ji nejprve spustit.

### <a name="to-run-the-page"></a>Ke spuštění stránky


1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na *FirstWebPage.aspx* a vyberte **nastavit jako úvodní stránku**.
2. Stiskněte klávesu **CTRL + F5** ke spuštění stránky.

    Stránka se zobrazí v prohlížeči. Přestože jste vytvořili stránku s příponou názvu souboru z *.aspx*, aktuálně běží jako jakoukoli stránku HTML.

    Má zobrazit stránku v prohlížeči je můžete také kliknout pravým tlačítkem na stránku **Průzkumníku řešení** a vyberte **zobrazit v prohlížeči**.
3. Zavřete prohlížeč k zastavení webové aplikace.


## <a name="adding-and-programming-controls"></a>Přidání a programování ovládacích prvků


<a id="sectionToggle1"></a>

Nyní přidáte ovládací prvky serveru na stránku. Ovládací prvky serveru, např. tlačítka, popisky, textová pole a další dobře známé ovládací prvky, poskytují typické možnosti zpracování formulářů pro stránky webových formulářů. Můžete však programu ovládacích prvků pomocí kódu, který běží na serveru, nikoli klienta.

Přidáte [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) řízení, [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) ovládací prvek a [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) řízení na stránku a napsat kód pro zpracování [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) událost pro [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku.

### <a name="to-add-controls-to-the-page"></a>K přidávání ovládacích prvků na stránku


1. Klikněte **návrhu** tab přepněte do **návrhu** zobrazení.
2. Umístěte kurzor na konci **Vítá vás Visual Web Developer** text a stiskněte klávesu **ENTER** pět nebo vícekrát, aby některé místnosti v **div** pole elementu.
3. V **sada nástrojů**, rozbalte **standardní** skupiny, pokud ještě není rozbalená.  
Všimněte si, že budete muset Rozbalit **sada nástrojů** okno na levé straně si ji zobrazit.
4. Přetáhněte [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) řízení na stránku a umístěte jej uprostřed **div** pole element, který má **Vítá vás Visual Web Developer** na prvním řádku.
5. Přetáhněte [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) řízení na stránku a umístěte jej napravo od [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) ovládacího prvku.
6. Přetáhněte [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) řízení na stránku a umístěte jej na samostatném řádku níže [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku.
7. Umístěte kurzor výše [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) řízení a poté zadejte **zadejte své jméno:** .

    Je titulek pro tento statický text ve formátu HTML [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) ovládacího prvku. Je možné kombinovat statické HTML a serverových ovládacích prvků na stejné stránce. Následující obrázek znázorňuje, jak se zobrazují tři ovládací prvky v **návrhu** zobrazení.

    ![Tři ovládacích prvků v zobrazení návrhu](creating-a-basic-web-forms-page/_static/image9.png "tři ovládacích prvků v zobrazení návrhu")


### <a name="setting-control-properties"></a>Nastavení vlastností ovládacího prvku


Visual Studio nabízí různé způsoby, jak nastavit vlastnosti ovládacích prvků na stránce. V této části průvodce, můžete nastavit vlastnosti v obou **návrhu** zobrazení a **zdroj** zobrazení.

### <a name="to-set-control-properties"></a>Nastavení vlastností ovládacího prvku


1. Nejprve zobrazte **vlastnosti** windows tak, že vyberete z **zobrazení** nabídky -&gt; **ostatní okna**  - &gt; **Properies okno**. Můžete taky vybrat **F4** zobrazíte **vlastnosti** okno.
2. Vyberte [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) řízení a potom v **vlastnosti** okno, nastavte hodnotu **Text** k **zobrazovaný název**. Text, který jste zadali se zobrazí na tlačítku v návrháři, jak je znázorněno na následujícím obrázku.

    ![Nastavit text tlačítka](creating-a-basic-web-forms-page/_static/image10.png "text nastavit tlačítka")
3. Přepnout na **zdroj** zobrazení.

    **Zdroj** zobrazení se zobrazí kód HTML stránky, včetně prvků, které vytvořila aplikace Visual Studio pro ovládací prvky serveru. Ovládací prvky jsou deklarovány pomocí HTML syntaxe, s tím rozdílem, že značek použijte předponu **asp:** a zahrňte atribut **runat =&quot;server&quot;**.

    Vlastnosti ovládacích prvků jsou deklarovány jako atributy. Například pokud nastavíte [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) vlastnost [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) řídit, v kroku 1, byly ve skutečnosti nastavení **Text** atribut ve značce ovládacího prvku.

    > [!NOTE] 
    > 
    > Všechny ovládací prvky jsou uvnitř **formuláře** element, který má také atribut **runat =&quot;server&quot;**. **Runat =&quot;server&quot;**  atribut a **asp:** předponu značky ovládacího prvku značí ovládací prvky tak, aby se při spuštění stránky zpracovány na serveru pomocí technologie ASP.NET. Kód vně  **&lt;formuláři runat =&quot;server&quot; &gt;**  a  **&lt;skript runat =&quot;server&quot; &gt;**  elementy odeslán nezměněn do prohlížeče, což je důvod, proč kódu ASP.NET musí být uvnitř elementu, jehož počáteční značka obsahuje **runat =&quot;server&quot;**  atribut.
4. V dalším kroku přidejte další vlastnosti [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku. Umístěte kurzor přímo po **asp: Label** v  **&lt;asp: Label&gt;**  značku a potom stiskněte klávesu **MEZERNÍK**.

    Zobrazí se zobrazí seznam dostupných vlastností můžete nastavit pro rozevírací seznam [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku. Tato funkce označuje jako **IntelliSense**, pomáhá v **zdroj** zobrazení se syntaxí ovládací prvky serveru, elementů HTML a dalších položek na stránce. Následující obrázek ukazuje **IntelliSense** rozevíracího seznamu pro [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku.

    ![Atributy IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "atributy IntelliSense")
5. Vyberte **ForeColor** a pak zadejte symbolem rovná.

    IntelliSense zobrazí seznam barev.

    > [!NOTE] 
    > 
    > Můžete zobrazit **IntelliSense** rozevíracího seznamu kdykoli stisknutím **CTRL + J** při zobrazení kódu.
6. Vyberte barvu pro  **[popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)**  text ovládacího prvku. Ujistěte se, zda že jste vybrali barvu, která je dostatečně tmavý ke čtení proti bílé pozadí.

    **ForeColor** atribut byla dokončena s barvu, která jste vybrali, včetně uzavírací uvozovky.


### <a name="programming-the-button-control"></a>Programování ovládacího prvku tlačítko


V tomto návodu budete psát kód, který čte jméno, které uživatel zadá do textového pole a potom zobrazí název v [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku.

### <a name="add-a-default-button-event-handler"></a>Přidat výchozí obslužnou rutinu události tlačítko


1. Přepnout na **návrhu** zobrazení.
2. Dvakrát klikněte [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku.

    Ve výchozím nastavení, Visual Studio se přepne do souboru kódu na pozadí a vytvoří "kostra" obslužnou rutinu pro [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) výchozí událost ovládacího prvku [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) událostí. Soubor kódu odděluje váš kód uživatelského rozhraní (například HTML) z vašeho kódu serveru (například C#).   
Kurzor je nastavený přidat kód pro této obslužné rutiny události.

    > [!NOTE] 
    > 
    > Dvakrát klikněte na ovládací prvek v **návrhu** zobrazení je pouze jedním z několika způsoby, můžete vytvořit obslužné rutiny událostí.
3. Uvnitř **Button1\_klikněte na tlačítko** obslužné rutiny události, typ **Label1** následovaný tečkou (**.**).

    Pokud zadáte dobu, po **ID** popisku (**Label1**), Visual Studio zobrazí seznam dostupných členů pro [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) řídit, jak je znázorněno v následujícím obrázek. Člen běžně vlastností, metoda nebo událostí.

    ![IntelliSense v zobrazení kódu](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense v zobrazení kódu")
4. Dokončit **klikněte na tlačítko** obslužné rutiny události pro tlačítko tak, aby byla zobrazena, jak je znázorněno v následujícím příkladu kódu.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Přepněte zpět na zobrazení **zdroj** zobrazení vaší značka jazyka HTML, kliknutím pravým tlačítkem na *FirstWebPage.aspx* v **Průzkumníku řešení** a výběrem **zobrazení Značka**.
6. Přejděte k položce  **&lt;asp: tlačítko&gt;**  element. Všimněte si, že  **&lt;asp: tlačítko&gt;**  element má nyní atribut **onclick =&quot;Button1\_klikněte na tlačítko&quot;**.

    Tento atribut váže na tlačítko [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) událostí na metodu obslužné rutiny programového v předchozím kroku.

    Metody obslužné rutiny události může mít libovolný název; název, který se zobrazí se výchozí název vytvořený pomocí sady Visual Studio. Důležité je, že tento název používá pro **OnClick** v kódu HTML atributu musí odpovídat názvu metody definované v modelu code-behind.


### <a name="running-the-page"></a>Spuštění stránky


Nyní můžete otestovat serverové ovládací prvky na stránce.

### <a name="to-run-the-page"></a>Ke spuštění stránky


1. Stiskněte klávesu **CTRL + F5** ke spuštění stránky v prohlížeči. Pokud dojde k chybě, znovu zkontrolujte výše uvedených kroků.
2. Zadejte název do textového pole a klikněte **zobrazovaný název** tlačítko.

    Zadaný název se zobrazí v [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku. Všimněte si, že když kliknete na tlačítko, je stránka vrácena na webový server. ASP.NET pak znovu vytvoří stránku, spustí váš kód (v takovém případě [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) spustí obslužnou rutinu události) a poté odešle novou stránku v prohlížeči. Pokud sledujete stavového řádku v prohlížeči, uvidíte, že stránka vytváří dobu odezvy na webový server pokaždé, když kliknete na tlačítko.
3. V prohlížeči zobrazit zdroj stránce spustíte tak, že kliknete pravým tlačítkem na stránku a výběr **zobrazit zdroj**.

    Ve zdrojovém kódu stránky zobrazí bez jakékoli serveru kódu HTML. Konkrétně se nezobrazí  **&lt;asp:&gt;**  prvky, které jste pracovali v **zdroj** zobrazení. Při spuštění stránky ASP.NET zpracovává ovládací prvky serveru a vykreslí prvků HTML na stránku, které provádějí funkce, které představují ovládací prvek. Například  **&lt;asp: tlačítko&gt;**  vykreslení ovládacího prvku jako kód HTML  **&lt;typ vstupu =&quot;odeslání&quot; &gt;**  element.
4. Zavřete prohlížeč.


## <a name="working-with-additional-controls"></a>Práce s další ovládací prvky

<a id="sectionToggle2"></a>

V této části návodu budete pracovat s [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) řízení, které zobrazuje data měsíce v čase. [Kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) řízení je ovládací prvek složitější než tlačítko, textového pole a popisek, kterou jste pracovali a ilustruje některé další funkce ovládacích prvků serveru.

V této části přidáte [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) řízení na stránku a naformátujte ho.

### <a name="to-add-a-calendar-control"></a>Přidání ovládacího prvku Kalendář


1. V sadě Visual Studio, přepněte do **návrhu** zobrazení.
2. Z **standardní** části **sada nástrojů**, přetáhněte [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) řízení na stránku a umístěte jej níže **div** element, obsahuje další ovládací prvky.

    Zobrazí se panelu inteligentních značek v kalendáři. V panelu zobrazí příkazy, které umožňují snadnou můžete provádět běžné úkoly pro vybraný ovládací prvek. Následující obrázek ukazuje [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) řídit, jak je vykreslen v **návrhu** zobrazení.

    ![V ovládacím prvku v zobrazení návrhu kalendář](creating-a-basic-web-forms-page/_static/image13.png "v ovládacím prvku v zobrazení návrhu kalendář")
3. V panelu inteligentních značek vyberte **automatický formát**.

    **Automatický formát** se zobrazí dialogové okno, které umožňuje vybrat schéma formátování kalendáře. Následující obrázek ukazuje **automatický formát** dialogové okno pro [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) ovládacího prvku.

    ![Dialogové okno Formát automaticky (ovládacího prvku Kalendář)](creating-a-basic-web-forms-page/_static/image14.png "dialogové okno Automatický formát (kalendáře řízení)")
4. Z **vyberte schéma** vyberte **jednoduché** a pak klikněte na **OK**.
5. Přepnout na **zdroj** zobrazení.

    Můžete zobrazit  **&lt;asp: kalendáře&gt;**  element. Tento element je mnohem delší než elementy pro jednoduché ovládací prvky, které jste vytvořili dříve. Také obsahuje dílčí prvky, jako například  **&lt;WeekEndDayStyle&gt;**, které představují různé nastavení formátování. Následující obrázek ukazuje [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) řídit ve **zdroj** zobrazení. (Přesný kód, který se zobrazí v **zdroj** zobrazení může mírně lišit od na obrázku.)

    ![V ovládacím prvku v zobrazení zdroje kalendář](creating-a-basic-web-forms-page/_static/image15.png "v ovládacím prvku v zobrazení zdroje kalendář")


### <a name="programming-the-calendar-control"></a>Programování ovládacího prvku Kalendář


V této části, bude program [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) zobrazte aktuálně vybrané datum ovládacího prvku.

### <a name="to-program-the-calendar-control"></a>Programu ovládacího prvku Kalendář


1. V **návrhu** klikněte dvakrát na [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) ovládacího prvku.

    Novou obslužnou rutinu události se vytvoří a zobrazí v souboru kódu s názvem *FirstWebPage.aspx.cs*.
2. Dokončit [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) obslužné rutiny události s následujícím kódem.


    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

 Ve výše uvedeném kódu nastaví textový popisek ovládacího prvku na vybraným datem ovládacího prvku kalendář.


### <a name="running-the-page"></a>Spuštění stránky


Nyní můžete otestovat kalendáři.

### <a name="to-run-the-page"></a>Ke spuštění stránky


1. Stiskněte klávesu **CTRL + F5** ke spuštění stránky v prohlížeči.
2. Klepněte na datum v kalendáři.

    Datum kliknuli jste na tlačítko se zobrazí v [popisek](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) ovládacího prvku.
3. V prohlížeči zobrazte zdrojový kód pro stránku.

    Všimněte si, že [kalendáře](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) vykreslena ovládacího prvku na stránku jako **tabulky**, se každý den jako **td** element.
4. Zavřete prohlížeč.


## <a name="next-steps"></a>Další kroky


<a id="nextStepsToggle"></a>

Tento návod znázorňuje základní funkce návrháře stránky sady Visual Studio. Teď, když chápete, jak můžete vytvářet a upravovat stránky s webovými formuláři v sadě Visual Studio, můžete chtít prozkoumat další funkce. Například můžete chtít postupujte takto:

- Další informace o webových formulářů ASP.NET pomocí následujících podrobný kurz řady [Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Další informace o šablonách Cascading listů stylů (CSS). Podrobnosti najdete v tématu [práce s Přehled šablon stylů CSS](https://msdn.microsoft.com/library/bb398931.aspx).
