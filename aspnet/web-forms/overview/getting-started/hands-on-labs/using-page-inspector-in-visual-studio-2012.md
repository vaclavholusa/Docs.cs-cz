---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Díky nástroji Page Inspector v sadě Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: V tomto testovacím prostředí Hands-on bude zjišťovat nové nástroj, který najít a opravit problémy, webová stránka v sadě Visual Studio – nástroj Page Inspector. Nástroj Page Inspector je nový nástroj této b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 052d29dba170d403c2b1c1667c55fc2c34045615
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Díky nástroji Page Inspector v sadě Visual Studio 2012
====================
Podle [webové táborech Team](https://twitter.com/webcamps)

> V tomto testovacím prostředí Hands-on bude zjišťovat nové nástroj, který najít a opravit problémy, webová stránka v sadě Visual Studio – nástroj Page Inspector.
> 
> Nástroj Page Inspector je nový nástroj, který k sadě Visual Studio přináší diagnostické nástroje prohlížeče a poskytuje integrované možnosti mezi prohlížeče, ASP.NET a zdrojový kód. Se vykreslí webovou stránku (HTML, webových formulářů, ASP.NET MVC nebo webové stránky) přímo v prostředí Visual Studio IDE a umožňuje Zkontrolujte zdrojový kód a výsledný výstup. Nástroj Page Inspector umožňuje snadno rozložit, rychle vytvořit stránky od základů i rychle diagnostikovat problémy.
> 
> V současné době máme několik webové platformy, které vytvářejí přizpůsobitelný a škálovatelný weby v časovém limitu, jako je například technologie ASP.NET MVC a webových formulářů. Na druhé straně získá těžší najít problémy na stránkách, protože rozhraní IDE nepodporuje návrháře zobrazení založené na šablonách stránky a dynamický obsah. Proto tyto weby muset otevřít v prohlížeči zobrazit, jak se zobrazují na uživatele.
> 
> Webovým vývojářům používat externí nástroje k vyhledání problémů, které pravidelně spouštění v prohlížeči. Poté vrátit IDE a zahájení oprav. To a zpět aktivity mezi rozhraní IDE, prohlížeče a nástrojů pro profilaci může být neefektivní a někdy vyžaduje čištění pokaždé, když chcete reprodukujte problém mezipaměti a nová nasazení.
> 
> Nástroj Page Inspector přemosťuje spojením nejlepší z obou světů používá kombinovanou sadu funkcí mezera ve vývoji webů mezi klientem (Nástroje prohlížeče) a serverem (technologie ASP.NET a zdrojový kód).
> 
> Pomocí nástroje Page Inspector, můžete zobrazit, které elementy ve zdrojových souborech (včetně kódu na straně serveru) je tvořen kód HTML k vykreslení v prohlížeči. Nástroj Page Inspector rovněž umožňuje změnit vlastnosti CSS a atributy elementu DOM zobrazíte okamžitě změny zobrazit v prohlížeči.
> 
> Toto praktické cvičení se vás provede funkcích nástroje Page Inspector a ukazují, jak je můžete využít k opravte problémy v webových aplikací. **Toto testovací prostředí obsahuje dvě cvičení pomocí podobné toky ale cílení na různé technologie. Pokud jste vývojáři ASP.NET MVC, postupujte podle cvičení jeden; Pokud jste cvičení postupujte podle vývojáře webových formulářů, dva**.
> 
> Tato laboratoř vás provede procesem vylepšení a nových funkcí popsaných výše použitím malých změn na ukázkové webové aplikaci ve zdrojové složce zadané.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Rozložit webu pomocí nástroje Page Inspector
- Zkontrolujte a zobrazení náhledu změn stylů CSS s nástrojem Page Inspector
- Zjistit a opravit problémy na webových stránkách díky nástroji Page Inspector

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny o tom, jak ji nainstalovat).
- Internet Explorer 9 nebo vyšší

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické cvičení zahrnuje následující cvičení:

1. [Cvičení 1: Použití nástroje Page Inspector ve projekty ASP.NET MVC](#Exercise1)
2. [Cvičení 2: Díky nástroji Page Inspector v projektech webových formulářů](#Exercise2)

> [!NOTE]
> Každý úkol je přiložena počáteční řešení, umístěný ve složce Begin cvičení, která umožňuje postupujte podle jednotlivých cvičení nezávisle na ostatních. Uvnitř zdrojový kód pro cvičení je také k dispozici na koncové složku obsahující řešení sady Visual Studio s kódem, který je výsledkem kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické cvičení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Cvičení 1: Použití nástroje Page Inspector ve projekty ASP.NET MVC

V tomto cvičení se dozvíte, jak zobrazit náhled a ladit **ASP.NET MVC 4** řešení pomocí **nástroj Page Inspector**. Nejprve budete provádět stručný okruhu kolem nástroj další funkce, které usnadňují webu ladění procesu. Pak bude fungovat na webové stránce, který obsahuje problémy stylů. Se dozvíte, jak používat nástroj Page Inspector zdrojový kód, který generuje problém najít a opravit.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Úloha 1 – prohlížení nástroj Page Inspector

V této úloze se dozvíte, jak používat nástroj Page Inspector v kontextu projektu ASP.NET MVC 4, který ukazuje Galerie fotografií.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex1-MVC4/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. V Průzkumníku řešení vyhledejte **Index.cshtml** zobrazit v části **nebo zobrazení, domácí** projektu složky, pravým tlačítkem myši a vyberte **zobrazení v nástroj Page Inspector**.

    ![Vyberte soubor, který chcete zobrazit náhled v nástroj Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "vyberete soubor, který chcete zobrazit náhled v nástroj Page Inspector")

    *Vyberte soubor, který chcete zobrazit náhled v nástroj Page Inspector*
3. V okně nástroje Page Inspector se zobrazí */Home/Index* adresa URL namapovaná na zdroji zobrazení, které jste vybrali.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Prvním kontaktu s nástrojem Page Inspector*

    Nástroj Page Inspector je integrovaná v prostředí Visual Studio. Kontrolor obsahuje vložené prohlížeče, společně s výkonné profileru HTML. Všimněte si, že není nutné spustit řešení můžete zobrazit vzhled stránky.

    > [!NOTE]
    > Když šířku prohlížeč nástroje Page Inspector je menší než šířka otevřenou stránku, nebude správně naleznete na stránce. Pokud k tomu dojde, umožňuje upravte šířku nástroje Page Inspector.
4. Klikněte **soubory** ve nástroj Page Inspector.

    Zobrazí se všechny zdrojové soubory, které jsou skládání indexovou stránku. Tato funkce vám pomůže identifikovat všechny elementy v kostce, zejména v případě, že pracujete s částečné zobrazení a šablony. Všimněte si, že můžete otevřít také každý ze souborů Pokud kliknutím na odkazy.

    ![The-Files-tab](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Na kartě soubory*
5. Klikněte **přepnout režim kontroly** tlačítko, které se nachází v levé části karty.

    Tento nástroj vám umožní vybrat libovolný element stránky a zobrazit jeho kód HTML a syntaxe Razor.

    ![Přepnutí kontroly režimu tlačítka](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Přepínací tlačítko kontroly režimu*
6. V prohlížeči nástroj Page Inspector přesuňte ukazatel myši nad prvky stránky. Při přesunutí ukazatele myši libovolnou část na vykreslené stránce, zobrazí se typ elementu a odpovídající zdrojový kód nebo kód je zvýrazněn v editoru Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Režim kontroly v akci*

    > [!NOTE]
    > Není maximalizujte okno nástroj Page Inspector nebo nebudete moci zobrazit karta náhled zobrazující zdrojového kódu. Pokud klepnete na tlačítko element v nástroj Page Inspector je Maximalizovaný, zdrojový kód výběru se zobrazí, ale ho bude skrýt okna nástroje Page Inspector.

    Pokud byste věnovat pozornost **Index.cshtml** souboru, si všimnete, že je označený část zdrojový kód, který generuje vybraný prvek. Tato funkce usnadňuje úpravy dlouho zdrojové soubory, které poskytuje přímý a rychlým způsobem získat přístup ke kódu.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Probíhá kontrola elementy*
7. Klikněte **přepnout režim kontroly** tlačítko (![vyberte kartu HTML k zobrazení kódu HTML, vykreslení v prohlížeči nástroj Page Inspector.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Vyberte kartu HTML k zobrazení kódu HTML, vykreslení v prohlížeči nástroj Page Inspector.") ) Chcete-li zakázat kurzor.
8. Vyberte **HTML** zobrazíte kód HTML, vykreslení v prohlížeči nástroj Page Inspector.
9. V značka jazyka HTML vyhledejte položku seznamu s odkaz medvídek a vyberte ho.

    Všimněte si, že pokud vyberete kód, odpovídající výstup automaticky zvýrazněn v prohlížeči. Tato funkce je užitečná, pokud chcete zobrazit vykreslení bloku HTML na stránce.

    ![Element HTML zvolíte na stránce](using-page-inspector-in-visual-studio-2012/_static/image8.png "HTML výběr elementu na stránce")

    *Na stránce Výběr elementu HTML*
10. Klikněte na tlačítko **přepnout režim kontroly** tlačítko umožníte *kontroly režimu* a klikněte na navigačním panelu. Na pravé straně kód HTML, v podokně styly zobrazí se seznam se styly CSS u vybraného elementu.

    > [!NOTE]
    > Vzhledem k tomu, že hlavičku je součástí rozložení lokality, bude také otevřít nástroj Page Inspector \_Layout.cshtml souborové služby a zvýraznění segmentu kódu vliv.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Zjišťování styly a zdrojových souborů vybraného elementu*
11. S ukazatele kontroly přepnutí povolené přesuňte ukazatel myši pod panelem blue doporučenou a klikněte na tlačítko poloviční kruhu.

    ![Výběr elementu](using-page-inspector-in-visual-studio-2012/_static/image10.png "výběr elementu")

    *Výběr elementu*
12. V podokně styly, vyhledejte **obrázku pozadí** položku **.main obsah** skupiny. **Zrušte zaškrtnutí políčka** **obrázku pozadí** a zjistit, co se stane. Si všimnete, že v prohlížeči se okamžitě projeví změny a je skrytý na kruh.

    > [!NOTE]
    > Změny se uplatní na kartě Page Inspector styly neovlivní původní šablony stylů. Můžete zrušit zaškrtnutí styly nebo změnit jejich hodnoty kolikrát chcete, ale obnoví po aktualizaci stránky.

    ![Povolení a zakázání stylů CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "povolení a zakázání stylů CSS")

    *Povolení a zakázání stylů CSS*
13. Nyní, klikněte na tlačítko '**zde bude vaše logo**' text v hlavičce pomocí režimu kontroly.
14. V **styly** kartě, vyhledejte **velikost písma** atribut šablon stylů CSS v části **.site-title** skupiny. Klikněte dvakrát na hodnotu atributu a nahradit hodnotu 2.3 em **3 em**a potom stiskněte klávesu **ENTER**. Všimněte si, že název vypadá větší.

    ![Změna hodnot šablon stylů CSS v nástroj Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image12.png "CSS změna hodnot v nástroj Page Inspector")

    *Změna hodnot šablon stylů CSS v nástroj Page Inspector*
15. Klikněte **styly trasování** karta, v pravém podokně nástroj Page Inspector. Toto je alternativní způsob zobrazíte všechny styly použité pro výběr, seřazené podle názvu atributu.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Trasování šablon stylů CSS stylů vybraného prvku*
16. Jiné funkce nástroje Page Inspector je podokně rozložení. V režimu kontroly vyberte navigačním panelu a klikněte **rozložení** karty v pravém podokně. Zobrazí se přesný velikost vybraný prvek a jeho velikost odsazení, marže, odsazení a ohraničení. Všimněte si, že můžete také upravit hodnoty z tohoto zobrazení.

    ![Element rozložení v nástroj Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "Element rozložení v nástroj Page Inspector")

    *Element rozložení v nástroj Page Inspector*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Úloha 2 – najít a opravit problémy styl v galerii fotografií

Jak by diagnostikovat problémy webové stránky s předchozími verzemi sady Visual Studio? Jste pravděpodobně neznáte webové nástroje, které spustit mimo prostředí Visual Studio IDE, jako jsou nástroje pro vývojáře Internet Explorer nebo Firebug ladění. Prohlížeče pouze pochopit HTML, skriptů a stylů, zatímco základní architektury generuje HTML, které bude vykresleno. Z tohoto důvodu musíte často nasadit celý web, pokud chcete zobrazit, jak webové stránky vypadat.

Pokud chcete zjistit a opravit problém ve vašem webu měl pravděpodobně postupovali podle těchto kroků:

1. Spuštění řešení ze sady Visual Studio, nebo nasadit stránky na webovém serveru.
2. V prohlížeči otevřete nástrojů pro vývojáře použít, nebo jednoduše otevřete zdrojový kód a styly a spusťte tak, aby odpovídaly problém. Najít soubory související se situací, byste použili jste &quot;vyhledávání&quot; nebo &quot;hledání v souborech&quot; funkce s názvem třídy stylů.
3. Jakmile je zjištěna chyba, zastavte webový prohlížeč a serverem.
4. Vymazání mezipaměti prohlížeče.
5. Vraťte se k sadě Visual Studio pro použití opravy. Zopakujte všechny kroky k testování.

V architektuře ASP.NET MVC 4 není k dispozici žádná skutečná WYSIWYG, většina styl problémů se zjišťují v pozdější fázi, po spuštěna nebo k nasazení webové aplikace. S nástrojem Page Inspector je nyní možné náhled jakoukoli stránku bez spuštění řešení.

V této úloze budete používat nástroj Page inspector a opravit některé problémy s aplikací Fotogalerie.

1. Díky nástroji Page Inspector, vyhledejte **zaregistrovat** a **přihlásit** odkazy na levou stranu záhlaví.

    Všimněte si, že odkazy nejsou zobrazeny na očekávaný místě na pravé straně a jsou zobrazeny jako seznam s odrážkami. Nyní budete zarovnat odkazy napravo a změnit odpovídajícím způsobem styl.

    ![Vyhledávání registrace a protokolu v odkazy](using-page-inspector-in-visual-studio-2012/_static/image15.png "vyhledávání registrace a protokolu v odkazy")

    *Vyhledávání registrace a protokolu v odkazy*
2. Přepnout režim kontroly vybraný klikněte na Zavřít, ale ne, registrace odkazu k otevření jeho kód.

    Všimněte si, že se nachází zdrojový kód z odkazů v  **\_LoginPartial.cshtml** souboru, ne Index.cshtml ani \_Layout.cshtml, které jsou na místech může vypadat na prvním místě. Můžete byly umístěny přímo v souboru správný zdroj.
3. V **styly** , vyhledejte a klikněte **<section> #login</section>** položku, která je kontejner HTML pro tyto odkazy.

    Všimněte si, že **#login** styl automaticky nachází v **Site.css** po kliknutí na tlačítko. Kromě toho je nyní zvýrazněný kód.

    ![Výběr stylů CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "výběr stylů CSS")

    *Výběr stylů CSS*
4. Zrušením komentáře u **text-align** v zvýrazněný odebráním počátečních a koncových znaků a uložit **Site.css** souboru.

    Nástroj Page Inspector si je vědoma různých souborů, které tvoří aktuální stránce a může zjistit. Pokud některý z těchto souborů změnit. Zobrazí se vám vždy, když aktuální stránku v prohlížeči není synchronizována se zdrojovými soubory.
5. V prohlížeči nástroj Page Inspector klikněte na panel nacházel pod panelu Adresa načtením této stránky.

    ![Opětovné načtení stránky](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Opětovné načtení stránky*

    Nyní jsou odkazy na vpravo, ale stále zobrazují se jako seznam s odrážkami. Nyní odeberte odrážek a vodorovně zarovnat odkazy.

    ![Aktualizovanou stránku](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Aktualizovanou stránku*
6. Pomocí režimu kontroly, vyberte některé z **&lt;li&gt;** položky, které obsahují &quot;zaregistrovat&quot; a &quot;přihlásit&quot; odkazy. Potom klikněte  **&lt;části&gt; #login** položky pro přístup k **Styles.css** kódu.

    ![Hledání styl](using-page-inspector-in-visual-studio-2012/_static/image19.png "hledání styl")

    *Hledání styl*
7. V **Style.css**, zrušte komentář kódu pro **#login li** položky. Styl, který chcete přidat se skrýt odrážek a zobrazit seznam položek vodorovně.

    ![Vytvoření nového stylu odkazy přihlášení](using-page-inspector-in-visual-studio-2012/_static/image20.png "vytvoření nového stylu odkazy přihlášení")

    *Vytvoření nového stylu odkazy přihlášení*
8. Uložit **Style.css** souboru a klikněte na tlačítko jednou na panelu nacházel pod adresu načtením této stránky. Všimněte si, že odkazy zobrazovat správně.

    ![Zarovnání odkazy na pravé straně](using-page-inspector-in-visual-studio-2012/_static/image21.png "zarovnání odkazy na pravé straně")

    *Odkazy pravou stranu*
9. Nakonec se změní název hlavičky. Použijte režim kontroly klikněte na **zde bude vaše logo** text a získání přístupu do zdrojového kódu, který ho generuje.
10. Nyní jste v  **\_Layout.cshtml**, nahraďte '**zde bude vaše logo**'text s'**Fotogalerie**'. Uložte a aktualizujte prohlížeč nástroje Page Inspector.

    ![Přiřazení nové název](using-page-inspector-in-visual-studio-2012/_static/image22.png "přiřazení nový název")

    *Přiřazení nové název*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Stránka Fotogalerie aktualizovat*
11. Nakonec selet **Fotogalerie** projektu a stiskněte klávesu **F5** a spusťte aplikaci. Podívejte se na všechny změny fungovat podle očekávání.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Cvičení 2: Díky nástroji Page Inspector v projektech webových formulářů

V tomto cvičení se dozvíte, jak zobrazit náhled a ladění řešení WebForms díky nástroji Page Inspector. Nejprve budete provádět stručný okruhu kolem nástroj další nástroj Page Inspector funkce, které usnadňují webu ladění procesu. Pak bude fungovat na webové stránce, který obsahuje problémy stylů. Se dozvíte, jak používat nástroj Page Inspector zdrojový kód, který generuje problém najít a opravit.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Úloha 1 – prohlížení nástroj Page Inspector

V této úloze se dozvíte, jak používat nástroj Page Inspector funkce v kontextu WebForms projekt, který ukazuje Galerie fotografií.

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex2-WebForms/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. V Průzkumníku řešení vyhledejte **Default.aspx** , pravým tlačítkem myši a vyberte **zobrazení v nástroj Page Inspector**.

    ![Otevírání Default.aspx s nástrojem Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "otevření Default.aspx s nástrojem Page Inspector.")

    *Otevírání Default.aspx s nástrojem Page Inspector*
3. Nástroj Page Inspector okně zobrazí Default.aspx.

    ![Zobrazení Default.aspx v nástroj Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "zobrazení Default.aspx v nástroj Page Inspector")

    *Zobrazení Default.aspx v nástroj Page Inspector*

    Nástroj Page Inspector je integrovaná v prostředí Visual Studio. Kontrolor obsahuje vložené prohlížeče, společně s výkonné profileru HTML, který se zobrazí na vybraný úsek kódu. Všimněte si, že není nutné spustit řešení můžete zobrazit vzhled stránky.

    > [!NOTE]
    > Když šířku prohlížeč nástroje Page Inspector je menší než šířka otevřenou stránku, nebude správně naleznete na stránce. Pokud k tomu dojde, umožňuje upravte šířku nástroje Page Inspector.
4. Klikněte **soubory** ve nástroj Page Inspector.

    Zobrazí se všechny zdrojové soubory, které jsou na vykreslené stránce výchozí skládání. To je užitečné funkce pro identifikaci všechny elementy v kostce, zejména v případě, že pracujete s uživatelské ovládací prvky a stránky předlohy. Všimněte si, že můžete také přejít na každý ze souborů.

    ![Na kartě soubory](using-page-inspector-in-visual-studio-2012/_static/image26.png "karta The soubory")

    *Na kartě soubory*
5. Klikněte **přepnout režim kontroly** tlačítko, které se nachází v levé části karty.

    Tento nástroj vám umožní vyberte libovolný element stránky a zobrazit zdrojový kód a .aspx jeho HTML.

    ![Přepínací tlačítko kontroly režimu](using-page-inspector-in-visual-studio-2012/_static/image27.png "tlačítko Přepnout režim kontroly")

    *Přepínací tlačítko kontroly režimu*
6. V prohlížeči nástroj Page Inspector najeďte myší prvky stránky. Při přesunutí ukazatele myši libovolnou část na vykreslené stránce, zobrazí se typ elementu a odpovídající zdrojový kód nebo kód je zvýrazněn v editoru Visual Studio.

    ![Režim kontroly v akci](using-page-inspector-in-visual-studio-2012/_static/image28.png "režimu kontroly v akci")

    *Režim kontroly v akci*

    > [!NOTE]
    > Není maximalizujte okno nástroj Page Inspector nebo nebudete moci zobrazit karta náhled zobrazující zdrojového kódu. Pokud klepnete na tlačítko element v nástroj Page Inspector je Maximalizovaný, zdrojový kód výběru se zobrazí, ale ho bude skrýt okna nástroje Page Inspector.

    Pokud byste věnovat pozornost **Default.aspx** souboru, si všimnete, že je označený část zdrojový kód, který generuje vybraný prvek. Tato funkce usnadňuje edice dlouho zdrojové soubory, které poskytuje přímý a rychlým způsobem získat přístup ke kódu.

    ![Probíhá kontrola elementy](using-page-inspector-in-visual-studio-2012/_static/image29.png "zkontrolujete elementy")

    *Probíhá kontrola elementy*
7. Klikněte **přepnout režim kontroly** tlačítko (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), umístěný na kartách nástroj Page Inspector zakázat kurzor.
8. Vyberte **HTML** zobrazíte kód HTML, vykreslení v prohlížeči nástroj Page Inspector.
9. V kódu HTML vyhledejte položku seznamu s odkaz medvídek a vyberte ho.

    Všimněte si, že při výběru kód odpovídající výstup automaticky zvýrazněná prohlížeče. Tato funkce je užitečná, pokud chcete zobrazit vykreslení bloku HTML na stránce.

    ![Výběr prvku HTML na stránce](using-page-inspector-in-visual-studio-2012/_static/image31.png "výběrem HTML element na stránce")

    *Na stránce Výběr elementu HTML*
10. Klikněte na tlačítko **přepnout režim kontroly** tlačítko umožníte *kontroly režimu* a klikněte na navigačním panelu. Na pravé straně kód HTML, v podokně styly zobrazí se seznam se styly CSS u vybraného elementu.

    > [!NOTE]
    > vzhledem k tomu, že záhlaví je součástí rozložení lokality, nástroj Page Inspector také otevřít soubor Site.Master a zvýrazněte segmentu kódu vliv.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "zjišťování styly a zdrojových souborů vybraného elementu")

    *Zjišťování styly a zdrojových souborů vybraného elementu*
11. S ukazatele kontroly přepnutí povolené přesuňte ukazatel myši pod panelem nabídek a klikněte na prázdný kruh poloviční.

    ![Výběr elementu](using-page-inspector-in-visual-studio-2012/_static/image33.png "výběr elementu")

    *Výběr elementu*
12. V podokně styly, vyhledejte **obrázku pozadí** položku **.main obsah** skupiny. **Zrušte zaškrtnutí políčka** **obrázku pozadí** a zjistit, co se stane. Si všimnete, že v prohlížeči se okamžitě projeví změny a je skrytý na kruh.

    > [!NOTE]
    > Změny se uplatní na kartě Page Inspector styly neovlivní původní šablony stylů. Můžete zrušit zaškrtnutí styly nebo změnit jejich hodnoty kolikrát chcete, ale obnoví po aktualizaci stránky.

    ![Povolení a zakázání šablon stylů CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "povolení a zakázání stylů CSS")

    *Povolení a zakázání stylů CSS*
13. Nyní, klikněte na tlačítko '**vaše** **zde logo'** text v hlavičce pomocí režimu kontroly.
14. V **styly** kartě, vyhledejte **velikost písma** atribut šablon stylů CSS v části **.site-title** skupiny. Chcete-li upravit jeho hodnota dvakrát klikněte na atribut jednou. Nahraďte 2.3em hodnotu s **3em**, a stiskněte klávesu ENTER. Všimněte si, že název vypadá větší.

    ![Změna hodnot šablon stylů CSS v Inspector2 stránky](using-page-inspector-in-visual-studio-2012/_static/image35.png "CSS změna hodnot v nástroj Page Inspector")

    *Změna hodnot šablon stylů CSS v nástroj Page Inspector*
15. Klikněte **styly trasování** karta, v pravém podokně nástroj Page Inspector. Toto je alternativní způsob zobrazíte všechny styly použité pro výběr, seřazené podle názvu atributu.

    ![Trasování šablon stylů CSS stylů vybraného prvku](using-page-inspector-in-visual-studio-2012/_static/image36.png "trasování šablon stylů CSS stylů vybraného prvku")

    *Trasování šablon stylů CSS stylů vybraného prvku*
16. Jiné funkce nástroje Page Inspector je podokně rozložení. V režimu kontroly vyberte navigačním panelu a klikněte **rozložení** karty v pravém podokně. Zobrazí se přesný velikost vybraný prvek a jeho velikost odsazení, marže, odsazení a ohraničení. Všimněte si, že můžete také upravit hodnoty z tohoto zobrazení.

    ![Element rozložení v nástroj Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "Element rozložení v nástroj Page Inspector")

    *Element rozložení v nástroj Page Inspector*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Úloha 2 – najít a opravit problémy styl v galerii fotografií

Jak by diagnostikovat problémy webové stránky s předchozími verzemi sady Visual Studio? Jste pravděpodobně neznáte webové nástroje, které spustit mimo prostředí Visual Studio IDE, jako jsou nástroje pro vývojáře Internet Explorer nebo Firebug ladění. Prohlížeče pouze pochopit HTML, skriptů a stylů, zatímco základní architektury generuje HTML, které bude vykresleno. Z tohoto důvodu musíte často nasadit celý web, pokud chcete zobrazit, jak webové stránky vypadat.

Pokud chcete zjistit a opravit problém ve vašem webu měl pravděpodobně postupovali podle těchto kroků:

1. Spuštění řešení ze sady Visual Studio, nebo nasadit stránky na webovém serveru.
2. V prohlížeči otevřete nástrojů pro vývojáře použít, nebo jednoduše otevřete zdrojový kód a styly a spusťte tak, aby odpovídaly problém. Najít soubory související se situací, byste použili jste &quot;vyhledávání&quot; nebo &quot;hledání v souborech&quot; funkce s názvem třídy stylů.
3. Jakmile je zjištěna chyba, zastavte webový prohlížeč a serverem.
4. Vymazání mezipaměti prohlížeče.
5. Vraťte se k sadě Visual Studio pro použití opravy. Zopakujte všechny kroky k testování.

Protože je již skutečných WYSIWYG v ASP.NET WebForms, jsou některé problémy styl zjistila v pozdější fázi, po spuštěna nebo k nasazení. S nástrojem Page Inspector je nyní možné náhled jakoukoli stránku bez spuštění řešení.

V této úloze použijete nástroj Page inspector pro opravit některé problémy aplikace Fotogalerie. V následujících krocích bude zjišťovat a rychle některé jednoduché stylů problém opravte v hlavičce.

1. Pomocí stránky kontroly, vyhledejte **zaregistrovat** a **protokolu v** odkazy na levou stranu záhlaví.

    Všimněte si, že se nezobrazí odkaz na očekávaný místě na pravé straně. Nyní budete zarovnat na odkaz vpravo a změnit styl odpovídajícím způsobem.

    ![Přihlaste se odkaz umístěný na levé straně](using-page-inspector-in-visual-studio-2012/_static/image38.png "protokolu v odkazu nastavený na levé straně")

    *Přihlašovací odkaz umístěný na levé straně*
2. S přepnout režim kontroly vybrána vyberte odkaz Přihlásit otevřete svůj kód.

    Všimněte si, že odkaz zdrojového kódu se nachází v **Site.Master** souboru není na stránce Default.aspx, která je na místě může vypadat na prvním místě; vložení přímo v souboru správný zdroj.
3. V **styly** , vyhledejte a klikněte  **&lt;části&gt; #login** položku, která je kontejner HTML pro tyto odkazy.

    Všimněte si, že **#login** styl automaticky nachází v **Site.css** po kliknutí na tlačítko. Kromě toho je nyní zvýrazněný kód.

    ![Výběr stylů CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "výběr stylů CSS")

    *Výběr stylů CSS*
4. Zrušením komentáře u **text-align** v zvýrazněný odebráním počátečních a koncových znaků a uložit **Site.css** souboru.

    Nástroj Page Inspector si je vědoma různých souborů, které tvoří aktuální stránce a může zjistit. Pokud některý z těchto souborů změnit. Zobrazí se vám vždy, když aktuální stránku v prohlížeči není synchronizována se zdrojovými soubory.
5. V prohlížeči nástroj Page Inspector klikněte na panel nacházel pod panelu Adresa uložte změny a načtením této stránky.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Opětovné načtení stránky*

    Nyní jsou odkazy na vpravo, ale stále zobrazují se jako seznam s odrážkami. Nyní odeberte odrážek a vodorovně zarovnat odkazy.

    ![Aktualizovanou stránku](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Aktualizovanou stránku*
6. Pomocí režimu kontroly, vyberte některé z **&lt;li&gt;** položky, které obsahují &quot;zaregistrovat&quot; a &quot;přihlásit&quot; odkazy. Potom klikněte  **&lt;části&gt; #login** položky pro přístup k **Styles.css** kódu.

    ![Hledání styl](using-page-inspector-in-visual-studio-2012/_static/image42.png "hledání styl")

    *Hledání styl*
7. V **Style.css**, zrušte komentář kódu pro **#login li** položky. Styl, který chcete přidat se skrýt odrážek a zobrazit seznam položek vodorovně.

    ![Vytvoření nového stylu odkazy přihlášení](using-page-inspector-in-visual-studio-2012/_static/image43.png "vytvoření nového stylu odkazy přihlášení")

    *Vytvoření nového stylu odkazy přihlášení*
8. Uložit **Style.css** souboru a klikněte na tlačítko jednou na panelu nacházel pod adresu načtením této stránky. Všimněte si, že odkazy zobrazovat správně.

    ![Zarovnání odkazy na pravé straně](using-page-inspector-in-visual-studio-2012/_static/image44.png "zarovnání odkazy na pravé straně")

    *Odkazy pravou stranu*
9. Nakonec se změní název hlavičky. Namísto hledání '**zde bude vaše logo'** textu v všechny soubory, použijte režim kontroly kliknutím na text a získat ke zdrojovému kódu, který ho generuje.

    ![Hledání názvu webu](using-page-inspector-in-visual-studio-2012/_static/image45.png "hledání názvu webu")

    *Hledání názvu webu*
10. Nyní jste v **Site.Master**, nahraďte '**zde bude vaše logo**'text s'**Fotogalerie**'. Uložte a aktualizujte prohlížeč nástroje Page Inspector.

    ![Stránka Fotogalerie aktualizovat](using-page-inspector-in-visual-studio-2012/_static/image46.png "Fotogalerie stránku aktualizovat")

    *Stránka Fotogalerie aktualizovat*
11. Nakonec stiskněte **F5** a spusťte aplikaci, podívejte se na všechny změny fungovat podle očekávání.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Provedením tohoto testovacího prostředí Hands-On mít dozvědět, jak používat nástroj Page Inspector náhled vaší webové aplikaci bez nutnosti znovu sestavit a spustit webovou stránku v prohlížeči. Kromě toho mají dozvědí jak rychle najít a opravit chyby přístupu k přímo z vykreslené výstup do zdrojového kódu.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express pro Web dlaždice*
