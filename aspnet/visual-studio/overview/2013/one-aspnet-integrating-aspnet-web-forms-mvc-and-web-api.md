---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Praktické cvičení: Jeden ASP.NET: integrace webových formulářů ASP.NET, MVC a webového rozhraní API | Dokumentace Microsoftu'
author: rick-anderson
description: ASP.NET je architektura určená k vytváření webů, aplikací a služeb pomocí specializované technologie, jako je MVC, webové rozhraní API a další. S rozšiřující se ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7dec4daffa66621acaee1c76fda7b2e7550ad925
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382943"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Praktické cvičení: Jeden ASP.NET: integrace webových formulářů ASP.NET, MVC a webového rozhraní API
====================
podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](http://aka.ms/webcamps-training-kit)

> ASP.NET je architektura určená k vytváření webů, aplikací a služeb pomocí specializované technologie, jako je MVC, webové rozhraní API a další. Pomocí rozšíření ASP.NET zaznamenal od jejího vytvoření a uvedeného musí mít tyto technologie integrované, aby byla poslední úsilí o naplňování **One ASP.NET**.
> 
> Visual Studio 2013 přináší nový sjednocený projektový systém, který umožňuje vytvářet aplikace a použít všechny technologie ASP.NET v jednom projektu. Tato funkce eliminuje potřebu vyberte jeden technologii na začátku projektu a stonek s ním a místo toho doporučuje použití více rozhraní ASP.NET v rámci jednoho projektu.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této praktická cvičení se dozvíte, jak:

- Vytvoření webu na základě **One ASP.NET** typ projektu
- Použití různých **ASP.NET** architektur, jako jsou **MVC** a **webového rozhraní API** ve stejném projektu
- Identifikovat hlavní součásti **ASP.NET** aplikace
- Využijte výhod **ASP.NET generování uživatelského rozhraní** framework pro automatické vytvoření Kontrolerů a zobrazení k provádění operací CRUD založen na třídách modelu
- Vystavení stejnou sadu informace v počítači – a lidsky čitelném formátu nejvhodnější nástroj pro každou úlohu

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) nebo vyšší
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Chcete-li spustit praktická cvičení v této praktické testovací prostředí, musíte nejdřív nastavit prostředí.

1. Otevřete Průzkumníka Windows a přejděte do testovacího prostředí **zdroj** složky.
2. Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a nainstalujte Visual Studio fragmenty kódu pro toto testovací prostředí.
3. Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, aby bylo možné pokračovat.

> [!NOTE]
> Ujistěte se, že jste zaškrtli všechny závislosti pro toto testovací prostředí před spuštěním instalace.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Používání fragmentů kódu

V celém dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, který se dá dostat z v rámci Visual Studio 2013, abyste ho nemuseli znovu přidat ručně.

> [!NOTE]
> Každý cvičení se sadou počáteční řešení nachází v **začít** složky výkonu, který umožňuje postupovat podle jednotlivých výkon nezávisle na ostatních. Uvědomte si, že chybí z těchto řešení od fragmenty kódu, které se přidávají během cvičení a nemusí fungovat, dokud nedokončíte výkonu. Uvnitř zdrojový kód pro cvičení, můžete také najdete **End** složku, která obsahuje řešení sady Visual Studio s kódem, který je výsledkem dokončení kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické vyzkoušení.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické testovací prostředí obsahuje následující praktická cvičení:

1. [Vytváří se nový projekt webových formulářů](#Exercise1)
2. [Vytvoření Kontroleru MVC pomocí generování uživatelského rozhraní](#Exercise2)
3. [Vytvoření Kontroleru webového rozhraní API používat generování uživatelského rozhraní](#Exercise3)

Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržená tak, aby odpovídala konkrétním vývojářským styl a určuje rozložení oken, chování editoru, fragmenty kódu technologie IntelliSense a možnosti dialogového okna. Postupy v tomto testovacím prostředí jsou uvedené akce potřebné k provedení dané úlohy v sadě Visual Studio při použití **obecným vývojovým nastavením** kolekce. Pokud se rozhodnete různá nastavení kolekce pro vaše vývojové prostředí, mohou existovat rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Cvičení 1: Vytvoření nového projektu webových formulářů

V tomto cvičení vytvoříte novou lokalitu webové formuláře v sadě Visual Studio 2013 pomocí **One ASP.NET** jednotné rozhraní projektu, který vám umožní snadno integrovat komponenty webové formuláře, MVC a webového rozhraní API ve stejné aplikaci. Potom seznámení s řešením generované a určit její části, a nakonec se zobrazí na webu v akci.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Úloha 1 – vytváření nového webu pomocí jedné prostředí ASP.NET

V této úloze se spustí vytváření nového webu v sadě Visual Studio na základě **One ASP.NET** typ projektu. **One ASP.NET** sjednocuje všechny technologie ASP.NET a dává vám možnost kombinovat a párovat podle potřeby. Pak poznáte různých komponent nástroje webové formuláře, MVC a webového rozhraní API, kteří žijí vedle sebe v rámci vaší aplikace.

1. Otevřít **Visual Studio Express 2013 for Web** a vyberte **soubor | Nový projekt...**  spusťte nové řešení.

    ![Vytvoření nového projektu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Vytvoření nového projektu*
2. V **nový projekt** dialogu **webová aplikace ASP.NET** pod **Visual C# | Web** kartu a ujistěte se, že **rozhraní .NET Framework 4.5** zaškrtnuto. Pojmenujte projekt *MyHybridSite*, zvolte **umístění** a klikněte na tlačítko **OK**.

    ![Nový projekt webové aplikace ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Vytvoření nového projektu webové aplikace ASP.NET*
3. V **nový projekt ASP.NET** dialogové okno, vyberte **webových formulářů** šablony a vyberte **MVC** a **webového rozhraní API** možnosti. Také, ujistěte se, že **ověřování** je možnost nastavená na **jednotlivé uživatelské účty**. Klikněte na tlačítko **OK** pokračujte.

    ![Vytvoření nového projektu pomocí šablony webových formulářů, včetně součástí webového rozhraní API a MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Vytvoření nového projektu pomocí šablony webových formulářů, včetně součástí webového rozhraní API a MVC*
4. Teď si můžete projít strukturu generované řešení.

    ![Prozkoumání vygenerované řešení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Prozkoumání vygenerované řešení*

    1. **Účet:** tato složka obsahuje webový formulář stránky, které chcete zaregistrovat, přihlaste se k a spravovat účty pro uživatele vaší aplikace. Tato složka je vytvořena při **jednotlivé uživatelské účty** během konfigurace šablony projektu webových formulářů je vybraná možnost ověřování.
    2. **Modely:** tato složka bude obsahovat třídy, které představují data vaší aplikace.
    3. **Řadiče** a **zobrazení**: tyto složky jsou vyžadována pro **ASP.NET MVC** a **rozhraní ASP.NET Web API** komponenty. Prozkoumáte technologie MVC a webového rozhraní API v dalším cvičení.
    4. **Default.aspx**, **Contact.aspx** a **About.aspx** soubory jsou předem definovaných stránky webového formuláře, které můžete použít jako odrazový můstek k vytváření stránek, které jsou specifické pro vaše aplikace. Programovou logiku těchto souborů se nachází v samostatném souboru, který se označuje jako &quot;použití modelu code-behind&quot; souboru, který má &quot;. aspx&quot; nebo &quot;. aspx.cs&quot; rozšíření (v závislosti na tom jazyk používaný). Použití modelu code-behind logiku běží na serveru a dynamicky vytvoří výstup ve formátu HTML stránky.
    5. **Site.Master** a **Site.Mobile.Master** stránky definují vzhled a chování a standardní chování všechny stránky v aplikaci.
5. Dvakrát klikněte **Default.aspx** souboru zkoumání obsahu stránky.

    ![Zkoumání stránku Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Zkoumání stránku Default.aspx*

    > [!NOTE]
    > **Stránky** – direktiva v horní části souboru definuje atributy stránky webových formulářů. Například **MasterPageFile** atribut určuje cestu k hlavní stránce – v takovém případě *Site.Master* stránky – a **Inherits** definuje atribut použití modelu Code-behind třída stránky zdědí. Tato třída je umístěn v souboru dáno **CodeBehind** atribut.
    > 
    > **Asp: Content** ovládací prvek obsahuje skutečný obsah na stránce (text, značky a ovládací prvky) a je namapovaná na **asp: ContentPlaceHolder** ovládacího prvku na hlavní stránku. V takovém případě obsah stránky bude možné vykreslit v rámci *MainContent* ovládací prvek definovaný v *Site.Master* stránky.
6. Rozbalte **aplikace\_Start** složky a Všimněte si, že **WebApiConfig.cs** souboru. Visual Studio zahrnout generované řešení tento soubor, protože jste zahrnuli webového rozhraní API, pokud konfigurace vašeho projektu pomocí šablony One ASP.NET.
7. Otevřít **WebApiConfig.cs** souboru. V *WebApiConfig* třídy zjistíte konfigurace související s webovým rozhraním API, který mapuje HTTP směruje do **kontrolerů rozhraní Web API**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Otevřít **RouteConfig.cs** souboru. Uvnitř *RegisterRoutes* metoda najdete konfigurace související s MVC, který mapuje trasy protokolu HTTP a **kontrolery MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Úloha 2 – spuštění řešení

V této úloze se spustit generované řešení, prozkoumejte aplikace a některé z jejích funkcí, jako jsou přepisování adres URL a integrované ověřování.

1. Chcete-li spustit řešení, stiskněte **F5** nebo klikněte na tlačítko **Start** tlačítka umístěny na panelu nástrojů. Domovská stránka aplikace by měla otevřít v prohlížeči.

    ![Spuštění řešení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Ověřte, že jsou vyvolání stránky webových formulářů. Chcete-li to provést, přidejte **/contact.aspx** adresy URL v adresním řádku a stisknutím klávesy **Enter**.

    ![Přátelské adresy URL](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Přátelské adresy URL*

    > [!NOTE]
    > Jak je vidět, se změní adresa URL k **/kontaktovat**. Počínaje **ASP.NET 4**, adresy URL směrování funkce byly přidány do webových formulářů, takže můžete psát, jako jsou adresy URL *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* místo  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Další informace najdete v [směrování adres URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Tok ověření integrované do aplikace se teď si projděte. Chcete-li to provést, klikněte na tlačítko **zaregistrovat** v pravém horním rohu stránky.

    ![Registrace nového uživatele](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrace nového uživatele*
4. V **zaregistrovat** stránky, zadejte **uživatelské jméno** a **heslo**a potom klikněte na tlačítko **zaregistrovat**.

    ![Stránka pro registraci](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Stránka pro registraci*
5. Aplikace registruje nový účet a uživatel je ověřen.

    ![Uživatel byl ověřen](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Uživatel byl ověřen*
6. Vraťte se zpět do sady Visual Studio a stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Cvičení 2: Vytvoření Kontroleru MVC pomocí generování uživatelského rozhraní

V tomto cvičení bude využívat technologie ASP.NET generování uživatelského rozhraní framework poskytovaný sadou Visual Studio k vytvoření kontroler ASP.NET MVC 5 s akcemi a zobrazeními Razor k provádění operací CRUD, aniž byste museli napsat jediný řádek kódu. Proces generování uživatelského rozhraní pomocí platformy Entity Framework Code First vygenerovat kontext dat a schéma databáze ve službě SQL database.

**O rozhraní Entity Framework Code nejprve**

Entity Framework (EF) je objektově relační Mapovač (ORM), který vám umožní vytvářet aplikace přistupující k datům v programování s modelem koncepční aplikace namísto programování přímo pomocí schématu relační úložiště.

Entity Framework Code First pracovního postupu modelování vám umožní používat vlastní třídy domény k vyjádření modelu EF spoléhá na při provádění dotazu, funkce sledování změn a aktualizace. Pomocí pracovního postupu vývoje Code First, není potřeba začněte tím, že vytvoříte databázi nebo zadání schéma vaší aplikace. Místo toho můžete psát standardní třídy .NET, které definují nejvhodnější objekty modelu domény pro vaši aplikaci a Entity Framework pro vytvoření databáze.

> [!NOTE]
> Další informace o rozhraní Entity Framework [tady](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Úloha 1 – Vytvoření nového modelu

Nyní budou definovat **osoba** třídu, která se používá procesem generování uživatelského rozhraní pro vytvoření kontroleru MVC a zobrazení modelu. Začne tím, že vytvoříte **osoba** třídy modelu a operace CRUD v kontroleru se automaticky vytvoří pomocí funkce generování uživatelského rozhraní.

1. Otevřít **Visual Studio Express 2013 for Web** a **MyHybridSite.sln** řešení nachází v **zdroj/Ex2-MvcScaffolding/Begin** složky. Alternativně můžete pokračovat v řešení, který jste získali v předchozím cvičení.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **modely** složky **MyHybridSite** projektu a vyberte **přidat | Třída...** .

    ![Přidání třídy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Přidání třídy modelu osoby*
3. V **přidat novou položku** dialogové okno, pojmenujte soubor *Person.cs* a klikněte na tlačítko **přidat**.

    ![Vytvoření třídy modelu osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Vytvoření třídy modelu osoby*
4. Nahradí obsah **Person.cs** souboru následujícím kódem. Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.

    (Fragment - kódu *BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. V **Průzkumníka řešení**, klikněte pravým tlačítkem na **MyHybridSite** projektu a vyberte **sestavení**, nebo stiskněte klávesu **CTRL + SHIFT + B** k sestavení projektu.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Úloha 2 – Vytvoření Kontroleru MVC

Teď, když **osoba** model je vytvořený, budete používat generování uživatelského rozhraní ASP.NET MVC s Entity Framework pro vytvoření akce kontroleru CRUD a zobrazení pro **osoba**.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **MyHybridSite** projektu a vyberte **přidat | Nová vygenerovaná položka...** .

    ![Vytvoření nové vygenerované Kontroleru](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Vytvoření nové automaticky generovaný Kontroleru*
2. V **přidat vygenerované uživatelské rozhraní** dialogu **kontroler MVC 5 se zobrazeními, používá nástroj Entity Framework** a potom klikněte na tlačítko **přidat.**

    ![Vyberte kontroler MVC 5 se zobrazeními a Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Vyberte kontroler MVC 5 se zobrazeními a Entity Framework*
3. Nastavte *MvcPersonController* jako **názvu Kontroleru**, vyberte **použít asynchronní akce kontroleru** možnost a vyberte **osoba (MyHybridSite.Models)**  jako **třída modelu**.

    ![Přidat kontroler MVC s generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Přidat kontroler MVC s generování uživatelského rozhraní*
4. V části **třída kontextu dat**, klikněte na tlačítko **nový kontext dat...** .

    ![Vytváří se nový kontext dat.](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Vytváří se nový kontext dat.*
5. V **nový kontext dat.** dialogové okno, název nový kontext dat *PersonContext* a klikněte na tlačítko **přidat**.

    ![Vytváří se nový PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Vytváří se nový typ PersonContext*
6. Klikněte na tlačítko **přidat** k vytvoření nového řadiče pro **osoba** pomocí generování uživatelského rozhraní. Visual Studio pak vygeneruje akce kontroleru, kontext dat osoby a zobrazeními Razor.

    ![Po vytvoření kontroleru MVC pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Po vytvoření kontroleru MVC pomocí generování uživatelského rozhraní*
7. Otevřít **MvcPersonController.cs** soubor **řadiče** složky. Všimněte si, že metody akcí CRUD byly vytvořeny automaticky.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Výběrem **použít asynchronní akce kontroleru** zaškrtnutí políčka základní kostry aplikace možnosti v předchozích krocích, sada Visual Studio generuje asynchronní akce metody pro všechny akce, které zahrnují přístup ke kontextu dat osoby. Doporučuje se, že použití metod asynchronní akce pro dlouho běžící, procesor vázán požadavky k zabránění blokování webový server z provádění práce během zpracování požadavku.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Úloha 3 – spuštění řešení

V této úloze budete spouštět řešení znovu ověřte, že zobrazení pro **osoba** fungují podle očekávání. Přidejte nové osobě, chcete-li ověřit, že je úspěšně uložen do databáze.

1. Stisknutím klávesy **F5** ke spuštění řešení.
2. Přejděte do **/MvcPerson**. Vygenerovaná zobrazení, který zobrazuje seznam lidí, kteří by se zobrazit.
3. Klikněte na tlačítko **vytvořit nový** chcete přidat nové uživatele.

    ![Přejděte na automaticky generovaný zobrazení MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Přejděte na automaticky generovaný zobrazení MVC*
4. V **vytvořit** zobrazení, zadejte **název** a **stáří** osoba, a klikněte na **vytvořit**.

    ![Přidání nové osobě](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Přidání nové osobě*
5. Nový uživatel se přidá do seznamu. V seznamu prvků, klikněte na tlačítko **podrobnosti** k zobrazení podrobností osoby. Potom v **podrobnosti** zobrazení, klikněte na tlačítko **zpět do seznamu** chcete přejít zpátky k zobrazení seznamu.

    ![Zobrazení podrobností o osoby](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Zobrazení podrobností o osoby*
6. Klikněte na tlačítko **odstranit** odkaz se odstranit uživatele. V **odstranit** zobrazení, klikněte na tlačítko **odstranit** potvrďte operaci.

    ![Odstraňuje se uživatel](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Odstraňuje se uživatel*
7. Vraťte se zpět do sady Visual Studio a stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Cvičení 3: Vytvoření Kontroleru webového rozhraní API používat generování uživatelského rozhraní

Rozhraní Web API je součástí technologie ASP.NET zásobníku a navržená tak, aby implementace služby HTTP snadnější, obecná odesílání a přijímání dat ve formátu JSON nebo XML pomocí rozhraní RESTful API.

V tomto cvičení použijete ASP.NET generování uživatelského rozhraní znovu generovat kontroler Web API. Budete používat stejný **osoba** a **PersonContext** třídy z předchozí cvičení poskytovat stejné osobě data ve formátu JSON. Zobrazí se, jak můžete stejné prostředky zpřístupnit různými způsoby v rámci stejné aplikace ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Úloha 1 – Vytvoření Kontroleru webového rozhraní API

V této úloze vytvoříte nový **Kontroleru webového rozhraní API** , který bude vystavovat osoba data ve formátu použitelnost počítače jako JSON.

1. Pokud ještě není otevřen, otevřete **Visual Studio Express 2013 for Web** a otevřete **MyHybridSite.sln** řešení nachází v **zdroj/EX3.-WebAPI/Begin** složky. Alternativně můžete pokračovat v řešení, který jste získali v předchozím cvičení.

    > [!NOTE]
    > Pokud byste začali s počáteční řešení od cvičení 3, stiskněte klávesu **CTRL + SHIFT + B** k sestavení řešení.
2. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **řadiče** složky **MyHybridSite** projektu a vyberte **přidat | Nová vygenerovaná položka...** .

    ![Vytvoření nové vygenerované Kontroleru](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Vytvoření nové vygenerované Kontroleru*
3. V **přidat vygenerované uživatelské rozhraní** dialogu **webového rozhraní API** v levém podokně, pak **Kontroleru webového rozhraní API 2 s akcemi používající nástroj Entity Framework** v prostředním podokně a pak klikněte na tlačítko  **Přidáte.**

    ![Vyberte kontroler rozhraní Web API 2 s akcemi a Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "výběrem Kontroleru webového rozhraní API s akcemi a Entity Framework 2")

    *Vyberte kontroler rozhraní Web API 2 s akcemi a Entity Framework*
4. Nastavte *ApiPersonController* jako **názvu Kontroleru**, vyberte **použít asynchronní akce kontroleru** možnost a vyberte **osoba (MyHybridSite.Models)**  a **PersonContext (MyHybridSite.Models)** jako **modelu** a **kontext dat** třídy v uvedeném pořadí. Pak klikněte na tlačítko **přidat**.

    ![Přidání Kontroleru webového rozhraní API pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní")

    *Přidání kontroleru webového rozhraní API pomocí generování uživatelského rozhraní*
5. Visual Studio pak vygeneruje **ApiPersonController** třída s atributem čtyři akcemi CRUD pro práci s vašimi daty.

    ![Po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní")

    *Po vytvoření kontroleru webového rozhraní API pomocí generování uživatelského rozhraní*
6. Otevřít **ApiPersonController.cs** soubor a zkontrolujte *GetPeople* metody akce. Tato metoda dotazuje pole db **PersonContext** aby bylo možné získat data osob.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Nyní Všimněte si, že komentář nad definice metody. Poskytuje identifikátor URI, který zpřístupňuje tuto akci, kterou použijete v dalším úkolem.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > Ve výchozím nastavení, webové rozhraní API umožňují odchytit dotazy na */webové rozhraní API* cestu pro zabránění kolizím s kontrolery MVC. Pokud potřebujete změnit toto nastavení, podívejte se na [směrování v rozhraní ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Úloha 2 – spuštění řešení

V této úloze bude používat Internet Explorer **vývojářské nástroje F12 pomáhají** ke kontrole úplnou odpověď z kontroleru webového rozhraní API. Zobrazí se, jak můžete zaznamenávat provoz sítě získat lepší přehled o vašich aplikačních dat.

> [!NOTE]
> Ujistěte se, že **aplikace Internet Explorer** výběru v **Start** tlačítka umístěny na panelu nástrojů sady Visual Studio.
> 
> ![Možnosti aplikace Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **Vývojářské nástroje F12 pomáhají** mají celou sadu funkcí, které nejsou uvedeny v tomto praktická-testovací prostředí. Pokud chcete další informace o tom, podívejte se na [pomocí vývojářských nástrojů F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Stisknutím klávesy **F5** ke spuštění řešení.

    > [!NOTE]
    > Aby bylo možné provést tento úkol správně, musí mít data vaší aplikace. Pokud vaše databáze je prázdná, můžete přejít zpět k úloze 3 v cvičení 2 a postupujte podle kroků pro vytvoření nové osobě pomocí zobrazení MVC.
2. V prohlížeči stiskněte **F12** otevřít **vývojářské nástroje** panelu. Stisknutím klávesy **CTRL** + **4** nebo klikněte na tlačítko **sítě** ikony a pak klikněte na tlačítko začít zachytávání síťového provozu na zelenou šipku.

    ![Inicializace webového rozhraní API zachytávání sítě](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "zachytávání sítě inicializaci webového rozhraní API")

    *Spouštění Zachytávání síťových rozhraní Web API*
3. Připojit **rozhraní api/ApiPerson** na adresu URL v adresním řádku prohlížeče. Nyní zkontrolujete podrobnosti odpovědi **ApiPersonController**.

    ![Načítají se data osob prostřednictvím webového rozhraní API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "načítání dat osoby prostřednictvím webového rozhraní API")

    *Načítají se data osob prostřednictvím webového rozhraní API*

    > [!NOTE]
    > Jakmile se stahování dokončí, vyzve k akci udělat pomocí staženého souboru. Ponechte dialogové okno otevřené, aby bylo možné sledovat obsah odpovědi prostřednictvím panel nástrojů pro vývojáře.
4. Nyní bude kontrolovat textu odpovědi. Chcete-li to provést, klikněte na tlačítko **podrobnosti** kartu a potom klikněte na tlačítko **tělo odpovědi**. Můžete zkontrolovat, že stažených dat je seznam objektů s vlastnostmi **Id**, **název** a **stáří** , které odpovídají **osoba** Třída.

    ![Text odpovědi rozhraní API webové zobrazení](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "text odpovědi rozhraní API webové zobrazení")

    *Text odpovědi rozhraní API webové zobrazení*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Úloha 3 – Přidání webové stránky nápovědy k rozhraní API

Při vytváření webové rozhraní API, je užitečné vytvořit stránku nápovědy tak, aby ostatní vývojáři vědět, jak volat rozhraní API. Můžete vytvořit a aktualizovat stránky dokumentace ručně, ale je vhodnější pro automatické generování je vyhnout se nutnosti údržby práci. V této úloze používáte balíček Nuget k automatickému generování stránek nápovědy webového rozhraní API do řešení.

1. Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a potom klikněte na tlačítko **Konzola správce balíčků**.
2. V **Konzola správce balíčků** okno, že spustíte následující příkaz:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage** balíček nainstaluje potřebná sestavení a přidá zobrazení MVC pro stránky nápovědy v rámci **oblasti/HelpPage** složky.

    ![Oblast HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage oblasti")

    *HelpPage oblasti*
3. Ve výchozím nastavení, v nápovědě stránky obsahují zástupný text řetězce pro dokumentaci. Dokumentační komentáře XML slouží k vytvoření v dokumentaci. Chcete-li tuto funkci povolit, otevřete **HelpPageConfig.cs** soubor umístěný ve **oblasti/HelpPage/aplikace\_Start** složky a zrušte komentář u následujícího řádku:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt **MyHybridSite**vyberte **vlastnosti** a klikněte na tlačítko **sestavení** kartu.

    ![Vytvořit kartu](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "sestavení oddílu")

    *Vytvořit kartu*
5. V části **výstup**vyberte **soubor dokumentace XML**. Do textového pole zadejte **aplikace\_Data/XmlDocument.xml**.

    ![Výstup v kartě sestavení části](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "výstupní sekce kartě sestavení")

    *Výstupní sekce kartě sestavení*
6. Stisknutím klávesy **CTRL** + **S** a uložte změny.
7. Otevřít **ApiPersonController.cs** soubor **řadiče** složky.
8. Zadejte nový řádek mezi *GetPeople* podpisu metody a */ / api/ApiPerson GET* komentář a potom zadejte třemi lomítky.

    > [!NOTE]
    > Sada Visual Studio automaticky vloží elementy XML, které definují v dokumentaci k metodě.
9. Přidejte souhrnný text a návratová hodnota pro *GetPeople* metody. By měl vypadat nějak takto.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Stisknutím klávesy **F5** ke spuštění řešení.
11. Připojit **/help** na adresu URL do adresního řádku a přejděte na stránku nápovědy.

    ![Stránka nápovědy pro rozhraní ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "stránku s nápovědou rozhraní ASP.NET Web API")

    *Stránka nápovědy webové rozhraní API technologie ASP.NET*

    > [!NOTE]
    > Hlavní obsah stránky je tabulka rozhraní API, seskupené podle kontroleru. Položky tabulky generuje dynamicky pomocí **IApiExplorer** rozhraní. Pokud přidáte nebo aktualizujete kontroler API, v tabulce budou automaticky aktualizovány při příštím sestavení aplikace.
    > 
    > **API** sloupec uvádí metodu HTTP a relativní identifikátor URI. **Popis** sloupec obsahuje informace, které byly extrahovány z metody dokumentace.
12. Všimněte si, že popis, který jste přidali výše definici metody se zobrazí ve sloupci Popis.

    ![Popis metody rozhraní API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "popis rozhraní API – metoda")

    *Popis rozhraní API – metoda*
13. Klikněte na jednu z metod rozhraní API přejít na stránku podrobné informace, včetně těla odpovědi vzorku.

    ![Stránka informace o podrobnosti](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "podrobné informace o stránce")

    *Podrobné informace o stránce*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Po dokončení tohoto praktických cvičení jste se naučili, jak:

- Vytvořit novou webovou aplikaci pomocí jednoho rozhraní ASP.NET v sadě Visual Studio 2013
- Integrace více technologie ASP.NET do jednoho jediného projektu
- Generování kontrolerů a zobrazení MVC z tříd modelu pomocí technologie ASP.NET generování uživatelského rozhraní
- Generování kontrolerů rozhraní Web API, které používají funkce, jako je asynchronní programování a přístup k datům pomocí Entity Frameworku
- Automatické generování stránky nápovědy webového rozhraní API pro vaše řadiče
