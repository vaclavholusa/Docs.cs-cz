---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Rukou na testovacím: Jeden ASP.NET: integrace webové formuláře ASP.NET, MVC a webových rozhraní API | Microsoft Docs'
author: rick-anderson
description: ASP.NET je architektura pro vytváření webů, aplikacím a službám pomocí speciální technologie jako MVC, Web API a dalších. Rozšiřující ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Rukou na testovacím: Jeden ASP.NET: integrace webové formuláře ASP.NET, MVC a webových rozhraní API
====================
podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](http://aka.ms/webcamps-training-kit)

> ASP.NET je architektura pro vytváření webů, aplikacím a službám pomocí speciální technologie jako MVC, Web API a dalších. Rozšiřující ASP.NET zaznamenal od svého vytvoření a uvedeného musí mít tyto technologie integrované, došlo ke poslední úsilí naplňování **jeden ASP.NET**.
> 
> Visual Studio 2013 zavádí nový systém jednotná projektu, která vám umožní sestavit aplikaci a použít všechny technologie ASP.NET v jednom projektu. Tato funkce eliminuje potřebu vyberte jeden technologie na začátku projektu a Flash disk s ním a místo toho umožňuje použití více rozhraní ASP.NET v rámci jednoho projektu.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Vytvoření webu na základě **jeden ASP.NET** typ projektu
- Použití různých **ASP.NET** rozhraní jako **MVC** a **webového rozhraní API** ve stejném projektu
- Určení hlavních komponent **ASP.NET** aplikace
- Využít **ASP.NET generování uživatelského rozhraní** framework automaticky vytvořit Kontrolery a zobrazení, které slouží k provádění operací CRUD podle tříd modelu
- Vystavení stejnou sadu informace v počítači - a čitelná pro člověka formátech pomocí ten nejvhodnější nástroj pro každou úlohu

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Pro dokončení této praktické cvičení je vyžadován následující text:

- [Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/) nebo vyšší
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Aby bylo možné spustit praktická cvičení v tomto testovacím prostředí praktické, musíte nejdřív nastavit svoje prostředí.

1. Otevřete Průzkumníka Windows a přejděte do testovacího prostředí na **zdroj** složky.
2. Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a instalaci sady Visual Studio fragmenty kódu pro toto testovací prostředí.
3. Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, která bude pokračovat.

> [!NOTE]
> Zkontrolujte, zda že jste zkontrolovali všechny závislosti pro toto testovací prostředí před spuštěním instalačního programu.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Používání fragmentů kódu

V dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění vaší práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, kterým můžete přistupovat z v rámci Visual Studio 2013, abyste nemuseli přidat ručně.

> [!NOTE]
> Každý úkol je přiložena počáteční řešení umístěný v **začít** složky cvičení, která umožňuje postupujte podle jednotlivých cvičení nezávisle na ostatních. Upozorňujeme, že fragmenty kódu, které jsou přidány během cvičení chybí z nich spuštění řešení a nemusí fungovat, dokud nedokončíte výkonu. Uvnitř zdrojový kód pro cvičení, je také k dispozici **End** složku obsahující řešení sady Visual Studio s kódem, který je výsledkem kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické cvičení.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické cvičení zahrnuje následující cvičení:

1. [Vytvoření nového projektu webové formuláře](#Exercise1)
2. [Vytvoření řadiče MVC pomocí generování uživatelského rozhraní](#Exercise2)
3. [Vytvoření řadiče webové rozhraní API pomocí generování uživatelského rozhraní](#Exercise3)

Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržené tak, aby odpovídala konkrétním vývoj styl a určuje rozložení oken, editor chování, IntelliSense – fragmenty kódu a možnosti dialogového okna. Postupy v tomto testovacím prostředí popisovat akce, které jsou nutné k dokončení daného úkolu v sadě Visual Studio, při použití **obecné nastavení vývoj** kolekce. Pokud si zvolíte jiné nastavení kolekce pro vývojové prostředí, může být rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Cvičení 1: Vytvoření nového projektu webové formuláře

V tomto cvičení vytvoříte novou lokalitu webové formuláře v sadě Visual Studio 2013 pomocí **jeden ASP.NET** jednotné rozhraní projektu, který vám umožní snadno integrovat webových formulářů, MVC a webového rozhraní API součásti ve stejné aplikaci. Potom zkoumat generovaného řešení a identifikovat jeho části, a nakonec bude naleznete na webu v akci.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Úloha 1 – Vytvoření nové lokality pomocí jedné prostředí ASP.NET

V této úloze se spustí vytváření nového webu v sadě Visual Studio na základě **jeden ASP.NET** typ projektu. **Jeden ASP.NET** kombinuje všechny technologie ASP.NET a vám dává možnost kombinovat a párovat je podle potřeby. Různé komponenty webových formulářů, MVC a webového rozhraní API, které za provozu se pak rozpoznat vedle sebe v rámci vaší aplikace.

1. Otevřete **Visual Studio Express 2013 pro Web** a vyberte **souboru | Nový projekt...**  zahájíte nové řešení.

    ![Vytvoření nového projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Vytvoření nového projektu*
2. V **nový projekt** dialogové okno, vyberte **webové aplikace ASP.NET** pod **Visual C# | Webové** kartě a zajistěte, aby **rozhraní .NET Framework 4.5** je vybrána. Název projektu *MyHybridSite*, vyberte **umístění** a klikněte na tlačítko **OK**.

    ![Nový projekt webové aplikace ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Vytvoření nového projektu webové aplikace ASP.NET*
3. V **nový projekt ASP.NET** dialogové okno, vyberte **webových formulářů** šablony a vyberte **MVC** a **webového rozhraní API** možnosti. Také zkontrolujte, zda **ověřování** je možnost nastavena na **jednotlivé uživatelské účty**. Klikněte na tlačítko **OK** pokračujte.

    ![Vytvoření nového projektu pomocí šablony webových formulářů, včetně součástí webového rozhraní API a MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Vytvoření nového projektu pomocí šablony webových formulářů, včetně součástí webového rozhraní API a MVC*
4. Nyní můžete prozkoumat strukturu generovaného řešení.

    ![Zkoumání generovaného řešení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Zkoumání generovaného řešení*

    1. **Účet:** tato složka obsahuje stránky webového formuláře, které chcete zaregistrovat, přihlaste se k a spravovat aplikace uživatelské účty. Tato složka je vytvořena při **jednotlivé uživatelské účty** během konfigurace webové formuláře v šabloně projektů je vybraná možnost ověřování.
    2. **Modely:** tato složka bude obsahovat třídy, které představují data aplikací.
    3. **Řadiče** a **zobrazení**: jsou požadovány pro tyto složky **ASP.NET MVC** a **rozhraní ASP.NET Web API** součásti. Zaměříte MVC a webového rozhraní API technologie v dalším cvičení.
    4. **Default.aspx**, **Contact.aspx** a **About.aspx** soubory jsou předem definované stránky webového formuláře, které můžete použít jako výchozí body k vytvoření stránky, které jsou specifické pro vaše aplikace. Programovací logiku těchto souborů se nachází v samostatném souboru označuje jako &quot;kódu&quot; souboru, který má &quot;. aspx&quot; nebo &quot;. aspx.cs&quot; rozšíření (v závislosti na jazyk použitý). Logika kódu na pozadí běží na serveru a dynamicky vytvoří výstupu protokolu HTML pro stránku.
    5. **Site.Master** a **Site.Mobile.Master** stránky definujte vzhled a chování standardní všechny stránky v aplikaci.
5. Dvakrát klikněte **Default.aspx** souboru a prozkoumejte obsah stránky.

    ![Prohlížení stránky Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Prohlížení stránky Default.aspx*

    > [!NOTE]
    > **Stránky** – direktiva v horní části souboru definuje atributy stránky webových formulářů. Například **MasterPageFile** atribut určuje cestu k hlavní stránce – v takovém případě *Site.Master* stránky – a **Inherits** atribut definuje třída kódu pro stránku dědění. Tato třída se nachází v souboru dáno **CodeBehind** atribut.
    > 
    > **Asp: obsah** řízení obsahuje skutečný obsah stránky (text, značky a ovládací prvky) a je namapovaná na **asp: ContentPlaceHolder** ovládacího prvku na hlavní stránku. V takovém případě bude možné vykreslit obsah stránky v rámci *MainContent* prvek definovaný v *Site.Master* stránky.
6. Rozbalte **aplikace\_spustit** složky a Všimněte si **WebApiConfig.cs** souboru. Visual Studio součástí souboru generovaného řešení jste do webového rozhraní API, při konfiguraci projektu pomocí šablony jeden ASP.NET.
7. Otevřete **WebApiConfig.cs** souboru. V *WebApiConfig* třída zjistíte konfiguraci přidružené webové rozhraní API, který mapuje HTTP směruje na **řadiče webového rozhraní API**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Otevřete **soubor RouteConfig.cs** souboru. Uvnitř *RegisterRoutes* metoda zjistíte konfigurace přidružené MVC, která se mapuje trasy HTTP k **řadiče MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Úloha 2 – spuštění řešení

V této úloze se spustit generovaného řešení, prozkoumat aplikace a některé jeho funkce, jako je přepisování adres URL a integrované ověřování.

1. Chcete-li spustit řešení, stiskněte **F5** nebo klikněte na tlačítko **spustit** tlačítko nachází na panelu nástrojů. Domovskou stránku aplikace by měla otevřít v prohlížeči.

    ![Spuštění řešení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Ověřte, že jsou volané stránky webových formulářů. Chcete-li to provést, připojte **/contact.aspx** adresy URL v adresním řádku a stiskněte klávesu **Enter**.

    ![Přátelské adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Přátelské adresy URL*

    > [!NOTE]
    > Jak vidíte, se změní adresa URL pro **/obraťte se na**. Od **ASP.NET 4**, možnosti směrování URL byly přidány do webových formulářů, takže můžete napsat adresy URL jako *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* místo  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Další informace najdete v části [směrování adres URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Nyní zaměříte tok ověřování integrovaná do aplikace. Chcete-li to provést, klikněte na tlačítko **zaregistrovat** v pravém horním rohu stránky.

    ![Registrace nového uživatele](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrace nového uživatele*
4. V **zaregistrovat** stránky, zadejte **uživatelské jméno** a **heslo**a potom klikněte na **zaregistrovat**.

    ![Zaregistrovat stránky](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Zaregistrovat stránky*
5. Aplikace zaregistruje nový účet a ověření uživatele.

    ![Uživatel ověřený](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Uživatel ověřený*
6. Přejděte zpět na Visual Studio a stiskněte klávesu **SHIFT + F5** Zastavit ladění.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Cvičení 2: Vytvoření řadiče MVC pomocí generování uživatelského rozhraní

V tomto cvičení bude využít výhod rozhraní ASP.NET generování uživatelského rozhraní poskytované sadě Visual Studio k vytvoření řadiče ASP.NET MVC 5 s akcemi a zobrazeními Razor k provádění operací CRUD, bez nutnosti napsat jediný řádek kódu. Proces generování uživatelského rozhraní použije Entity Framework Code First pro generování kontextu dat a schéma databáze v databázi SQL.

**O rozhraní Entity Framework Code nejprve**

Entity Framework (EF) je objekt relační mapper (ORM), která umožňuje vytvořit programování s modelem koncepční aplikace místo programování přímo pomocí schématu relační úložiště dat přístup k aplikacím.

Entity Framework Code First pracovního postupu modelování vám umožní používat vlastní třídy domény představující model, který využívá EF při provádění dotazu, funkce sledování změn a aktualizací. Pomocí Code First pracovní postup vývoje, není nutné zahájíte aplikace vytvoření databáze nebo zadáním schéma. Místo toho můžete napsat standardní tříd rozhraní .NET, které definují nejvhodnější objekty modelu domény pro vaši aplikaci a Entity Framework pro vytvoření databáze.

> [!NOTE]
> Další informace o rozhraní Entity Framework [zde](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Úloha 1 – Vytvoření nového modelu

Nyní nadefinujete **osoba** třídy, který bude používán procesem generování uživatelského rozhraní pro vytvoření řadiče MVC a zobrazení modelu. Začněte vytvořením **osoba** třídy modelu a operace CRUD v kontroleru se automaticky vytvoří pomocí funkce generování uživatelského rozhraní.

1. Otevřete **Visual Studio Express 2013 pro Web** a **MyHybridSite.sln** řešení umístěný v **zdroj/Ex2-MvcScaffolding/Begin** složky. Alternativně můžete pokračovat v řešení jste získali v předchozím cvičení.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **modely** složky **MyHybridSite** projektu a vyberte **přidat | Třída...** .

    ![Přidání třídy modelu osoba](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Přidání třídy modelu osoba*
3. V **přidat novou položku** dialogové okno, název souboru *Person.cs* a klikněte na tlačítko **přidat**.

    ![Vytvoření třídy modelu osoba](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Vytvoření třídy modelu osoba*
4. Nahradí obsah **Person.cs** soubor s následujícím kódem. Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.

    (Code fragment kódu - *BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **MyHybridSite** projektu a vyberte **sestavení**, nebo stiskněte klávesu **CTRL + SHIFT + B** a tím projekt sestavit.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Úloha 2 – Vytvoření řadič MVC

Teď, když **osoba** model je vytvořený, použijete generování uživatelského rozhraní ASP.NET MVC rozhraní Entity Framework pro vytvoření řadiče akcemi CRUD a zobrazení pro **osoba**.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **MyHybridSite** projektu a vyberte **přidat | Nové vygenerované položky...** .

    ![Vytvoření nového vygenerované řadiče](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Vytvoření nového řadiče generované uživatelské rozhraní*
2. V **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** a pak klikněte na tlačítko **přidat.**

    ![Výběr kontroler MVC 5 s zobrazení a Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Výběr kontroler MVC 5 s zobrazení a Entity Framework*
3. Nastavit *MvcPersonController* jako **názvu Kontroleru**, vyberte **použít asynchronní akce kontroleru** možnost a vyberte **osoba (MyHybridSite.Models)**  jako **třída modelu**.

    ![Přidání kontroler MVC pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Přidání kontroler MVC pomocí generování uživatelského rozhraní*
4. V části **třída kontextu dat**, klikněte na tlačítko **nový kontext data...** .

    ![Vytvoření nové kontextu dat](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Vytvoření nové kontextu dat*
5. V **nový kontext dat** dialogové okno, název nový kontext dat *PersonContext* a klikněte na tlačítko **přidat**.

    ![Vytvoření nové PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Vytvoření nového typu PersonContext*
6. Klikněte na tlačítko **přidat** k vytvoření nového řadiče pro **osoba** pomocí generování uživatelského rozhraní. Visual Studio pak vygeneruje akce kontroleru, kontextu dat osoby a zobrazení syntaxe Razor.

    ![Po vytvoření řadiče MVC pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Po vytvoření řadiče MVC pomocí generování uživatelského rozhraní*
7. Otevřete **MvcPersonController.cs** v soubor **řadiče** složky. Všimněte si, že byly automaticky generovány metody akce CRUD.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Výběrem **použít asynchronní akce kontroleru** možnosti zaškrtnutí políčka generování uživatelského rozhraní v předchozích krocích, Visual Studio vygeneruje metody asynchronní akce pro všechny akce, které se týkají přístupu k kontextu dat osoby. Doporučujeme, použít asynchronní akce metody pro dlouhodobé, bez procesoru vázaný požadavky k zabránění blokování webový server z provede práci během zpracování požadavku.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Úloha 3 – spuštění řešení

V této úloze budete spouštět řešení znovu a ověřte, zda zobrazení pro **osoba** fungují podle očekávání. Přidejte nové osobě, chcete-li ověřit, že je úspěšně uložen do databáze.

1. Stiskněte klávesu **F5** ke spuštění řešení.
2. Přejděte na **/MvcPerson**. Vygenerované zobrazení, který zobrazuje seznam lidí, kteří by se zobrazit.
3. Klikněte na tlačítko **vytvořit nový** chcete přidat nového uživatele.

    ![Přejdete na vygenerované zobrazení MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Přejdete na vygenerované zobrazení MVC*
4. V **vytvořit** zobrazit, zadejte **název** a **stáří** pro osoby a klikněte na **vytvořit**.

    ![Přidání nové osobě](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Přidání nové osobě*
5. Nový uživatel se přidá do seznamu. Element seznamu, klikněte na **podrobnosti** zobrazit podrobnosti, osoby. Potom v **podrobnosti** zobrazit, klikněte na tlačítko **zpět do seznamu** se vrátíte k zobrazení seznamu.

    ![Zobrazení podrobností osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Zobrazení podrobností osoby*
6. Klikněte **odstranit** odkaz odstraníte osoby. V **odstranit** zobrazit, klikněte na tlačítko **odstranit** k potvrzení operace.

    ![Odstraňování osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Odstraňování osoby*
7. Přejděte zpět na Visual Studio a stiskněte klávesu **SHIFT + F5** Zastavit ladění.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Cvičení 3: Vytvoření řadiče webové rozhraní API pomocí generování uživatelského rozhraní

Rozhraní Web API je součástí zásobníku technologie ASP.NET a navržený tak, aby implementující služeb HTTP jednodušší, obecně odesílání a příjmu dat prostřednictvím rozhraní RESTful API ve formátu JSON nebo XML.

V tomto cvičení použijete ASP.NET generování uživatelského rozhraní znovu vygenerovat kontroleru webového rozhraní API. Budete používat stejný **osoba** a **PersonContext** třídy z předchozím cvičení zajistit stejná osoba data ve formátu JSON. Zobrazí se, jak můžou stejné prostředky zpřístupnit různými způsoby v rámci stejné aplikace ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Úloha 1 – Vytvoření Kontroleru webového rozhraní API

V této úloze se vytvoří nové **kontroler Web API** který zveřejní osoba data ve formátu počítač consumable jako JSON.

1. Pokud ještě není otevřené, otevřete **Visual Studio Express 2013 pro Web** a otevřete **MyHybridSite.sln** řešení umístěný v **zdroj/EX3.-WebAPI/Begin** složky. Alternativně můžete pokračovat v řešení jste získali v předchozím cvičení.

    > [!NOTE]
    > Pokud spustíte s řešením Begin z cvičení 3, stiskněte klávesu **CTRL + SHIFT + B** sestavte řešení.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **MyHybridSite** projektu a vyberte **přidat | Nové vygenerované položky...** .

    ![Vytvoření nového vygenerované řadiče](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Vytvoření nového vygenerované řadiče*
3. V **přidat vygenerované uživatelské rozhraní** dialogové okno, vyberte **webového rozhraní API** v levém podokně, pak **webové 2 kontroler API s akcemi používající rozhraní Entity Framework** v prostředním podokně a pak klikněte na tlačítko  **Přidáte.**

    ![Výběr kontroler Web API 2 s akcemi a Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "výběr kontroler Web API s akcemi a Entity Framework 2")

    *Výběr kontroler Web API 2 s akcemi a Entity Framework*
4. Nastavit *ApiPersonController* jako **názvu Kontroleru**, vyberte **použít asynchronní akce kontroleru** možnost a vyberte **osoba (MyHybridSite.Models)**  a **PersonContext (MyHybridSite.Models)** jako **modelu** a **kontextu dat** třídy v uvedeném pořadí. Pak klikněte na tlačítko **přidat**.

    ![Přidání kontroler Web API s generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní")

    *Přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní*
5. Visual Studio pak vygeneruje **ApiPersonController** třídy s čtyři akcemi CRUD pro práci s daty.

    ![Po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní")

    *Po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní*
6. Otevřete **ApiPersonController.cs** souboru a zkontrolovat *GetPeople* metody akce. Tato metoda dotazuje pole db **PersonContext** typu, aby bylo možné získat data uživatele.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Nyní Všimněte si komentář výše definici metody. Poskytuje identifikátor URI, který zveřejňuje tuto akci, kterou chcete používat v dalším úkolem.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Ve výchozím nastavení, je nakonfigurované catch dotazů do rozhraní API webové */api* cestu k zabrání se tím kolizím s kontrolery MVC. Pokud potřebujete změnit toto nastavení, podívejte se na [směrování v rozhraní ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Úloha 2 – spuštění řešení

V této úloze budete používat v Internet Exploreru **nástroje pro vývojáře F12** kontrola úplnou odpověď z kontroleru webového rozhraní API. Zobrazí se, jak můžete zaznamenávat provoz sítě, chcete-li získat další vhled do vašich dat aplikace.

> [!NOTE]
> Ujistěte se, že **Internet Explorer** vybrán **spustit** tlačítko nachází na panelu nástrojů Visual Studio.
> 
> ![Možnost aplikace Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **Nástroje pro vývojáře F12** mají celou sadu funkcí, které nejsou zahrnuty v tomto hands-na-testovacím prostředí. Pokud chcete další informace o tom, podívejte se na [používání nástrojů pro vývojáře F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Stiskněte klávesu **F5** ke spuštění řešení.

    > [!NOTE]
    > Chcete-li provést tuto úlohu správně, musí mít data aplikace. Pokud vaše databáze je prázdná, můžete přejít zpět k úloze 3 v cvičení 2 a postupujte podle kroků pro vytvoření nového uživatele, kteří používají zobrazení MVC.
2. V prohlížeči stiskněte **F12** otevřete **Developer Tools** panelu. Stiskněte klávesu **CTRL** + **4** nebo klikněte na tlačítko **sítě** ikonu a pak klikněte na tlačítko se zelenou šipkou zahájíte zachytávání síťového provozu.

    ![Probíhá inicializace zachycení dat ze sítě webového rozhraní API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "zachycení dat ze sítě inicializaci webového rozhraní API")

    *Inicializace webového rozhraní API zachycení dat ze sítě*
3. Připojit **rozhraní api nebo ApiPerson** na adresu URL v adresním řádku prohlížeče. Nyní bude kontrolovat podrobnosti o odpověď **ApiPersonController**.

    ![Načítání dat osoba prostřednictvím webového rozhraní API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "načítání dat osoba prostřednictvím webového rozhraní API")

    *Načítání dat osoba prostřednictvím webového rozhraní API*

    > [!NOTE]
    > Po dokončení stahování se výzva, aby akce s stažený soubor. Ponechte dialogové okno otevřené, aby bylo možné sledovat obsahu odpovědi prostřednictvím okno nástroje pro vývojáře.
4. Nyní bude kontrolovat text odpovědi. Chcete-li to provést, klikněte na tlačítko **podrobnosti** a pak klikněte **odpovědi**. Můžete zkontrolovat, že stažená data jsou seznam objektů s vlastnostmi **Id**, **název** a **stáří** , odpovídají **osoba** Třída.

    ![Zobrazení webové rozhraní API odpovědi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "zobrazení webové rozhraní API odpovědi")

    *Zobrazení webové rozhraní API odpovědi*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Úloha 3 – Přidání webové stránky nápovědy rozhraní API

Při vytváření webového rozhraní API, je vhodné vytvořit stránku nápovědy, aby ostatní vývojáři věděli, jak volat rozhraní API. Můžete vytvářet a aktualizovat stránky dokumentace k ručně, ale je lepší pro automatické generování je, abyste nemuseli dělat údržbu práci. V této úloze budete používat balíčku Nuget k automatickému generování stránky nápovědy webového rozhraní API k řešení.

1. Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a potom klikněte na **Konzola správce balíčků**.
2. V **Konzola správce balíčků** okno, spusťte následující příkaz:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage** balíček nainstaluje potřebné sestavení a přidá zobrazení MVC pro stránky nápovědy v části **oblasti nebo HelpPage** složky.

    ![Oblasti HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage oblasti")

    *HelpPage oblasti*
3. Ve výchozím nastavení v nápovědě stránky máte řetězce zástupný symbol pro dokumentaci. Dokumentační komentáře XML slouží k vytvoření v dokumentaci. Chcete-li povolit tuto funkci, otevřete **HelpPageConfig.cs** soubor umístěný ve **oblasti nebo HelpPage nebo aplikace\_spustit** složky a zrušte komentář u následujícího řádku:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt **MyHybridSite**, vyberte **vlastnosti** a klikněte na tlačítko **sestavení** kartě.

    ![Sestavení karta](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "sestavení oddílu")

    *Karta sestavení*
5. V části **výstup**, vyberte **souborů dokumentace XML**. V dialogovém okně Upravit typ **aplikace\_Data/XmlDocument.xml**.

    ![Výstup části na kartě sestavení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "výstup části na kartě sestavení")

    *Výstup části na kartě sestavení*
6. Stiskněte klávesu **CTRL** + **S** a uložte změny.
7. Otevřete **ApiPersonController.cs** souboru z **řadiče** složky.
8. Zadejte nový řádek mezi *GetPeople* podpis metody a */ / GET rozhraní api nebo ApiPerson* komentář a potom zadejte tři lomítka.

    > [!NOTE]
    > Visual Studio automaticky vloží elementy XML, které definují metoda dokumentaci.
9. Přidat text shrnutí a návratovou hodnotu pro *GetPeople* metoda. By měl vypadat jako následující.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Stiskněte klávesu **F5** ke spuštění řešení.
11. Připojit **/help** na adresu URL v adresním řádku a přejděte na stránku nápovědy.

    ![Stránka nápovědy rozhraní ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "stránka nápovědy rozhraní ASP.NET Web API")

    *Stránku nápovědy rozhraní ASP.NET Web API*

    > [!NOTE]
    > Hlavní obsah stránky je tabulka rozhraní API, seskupené podle řadiče. Položky tabulky jsou generována dynamicky, pomocí **IApiExplorer** rozhraní. Pokud přidáte nebo aktualizujete kontroler API, v tabulce budou automaticky aktualizovány při příštím sestavení aplikace.
    > 
    > **Rozhraní API** sloupec obsahuje metodu HTTP a relativní identifikátor URI. **Popis** sloupec obsahuje informace, které jste extrahovali z dokumentace metody.
12. Všimněte si, že popis, který jste přidali výše definici metody se zobrazí ve sloupci Popis.

    ![Popis rozhraní API metoda](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "popis metody rozhraní API")

    *Popis rozhraní API – metoda*
13. Klikněte na jednu z metod rozhraní API přejít na stránku s podrobnější informace, včetně ukázkové těla odpovědi.

    ![Stránka informace o podrobností](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "podrobné informace o stránky")

    *Podrobné informace stránky*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Pomocí této praktické cvičení jste se naučili, jak:

- Vytvoření nové webové aplikace pomocí jednoho prostředí ASP.NET v sadě Visual Studio 2013
- Integrace více technologie ASP.NET do jednoho jednoho projektu
- Generování řadiče MVC a zobrazení z tříd modelu pomocí ASP.NET generování uživatelského rozhraní
- Generovat řadičů webového rozhraní API, která používá funkce, jako jsou asynchronní programování a přístup k datům prostřednictvím rozhraní Entity Framework
- Automatické generování stránky nápovědy webové rozhraní API pro vaše řadiče
