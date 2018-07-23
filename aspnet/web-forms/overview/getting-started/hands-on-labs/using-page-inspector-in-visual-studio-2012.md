---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Použití Page Inspectoru v sadě Visual Studio 2012 | Dokumentace Microsoftu
author: rick-anderson
description: Ve tohoto praktického testovacího prostředí bude zjišťovat nového nástroje můžete najít a opravit chyby webové stránky v sadě Visual Studio – Page Inspector. Nástroj Page Inspector je nový nástroj pro tuto b...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: ac945a23dc6ef060340320d047f13c8e81057138
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833669"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Použití Page Inspectoru v sadě Visual Studio 2012
====================
podle [Campy Web týmu](https://twitter.com/webcamps)

> Ve tohoto praktického testovacího prostředí bude zjišťovat nového nástroje můžete najít a opravit chyby webové stránky v sadě Visual Studio – Page Inspector.
> 
> Nástroj Page Inspector je nový nástroj, který se sadou Visual Studio přináší diagnostické nástroje prohlížeče a představuje integrovaný celek mezi prohlížečem, ASP.NET a zdrojový kód. Vykreslí webovou stránku (HTML, webových formulářů, ASP.NET MVC nebo webové stránky) přímo v integrovaném vývojovém prostředí sady Visual Studio a umožňuje prozkoumat zdrojový kód a výsledný výstup. Nástroj Page Inspector umožňuje snadno rozložit, rychle vytvořit stránky od základů i rychle diagnostikovat problémy.
> 
> V současné době máme několik webových prostředí, která vytvářet weby s flexibilní a škálovatelná včas, jako je například technologie ASP.NET MVC a webových formulářů. Na druhé straně získá těžší a najít problémy, na stránkách, protože rozhraní IDE nepodporuje návrháře zobrazení založené na šablonách stránek a dynamický obsah. Proto tyto weby muset otevřít v prohlížeči zobrazit, jak se zobrazují na uživatele.
> 
> Weboví vývojáři používat externí nástroje a najít problémy, které pravidelně spustit v prohlížeči. Potom vraťte se do integrovaného vývojového prostředí a začít řešit. To vpřed a zpět aktivity mimo rozhraní IDE, prohlížeče a nástrojů pro profilaci může být neefektivní a v některých případech vyžaduje nové nasazení a pokaždé, když chcete reprodukovat problém, čištění mezipaměti.
> 
> Nástroj Page Inspector přemosťuje mezery ve vývoji webů mezi klientem (Nástroje prohlížeče) a serverem (technologie ASP.NET a zdrojový kód) tím, že spojuje to nejlepší z obou světů pomocí kombinovanou sadu klíčových funkcí.
> 
> Použití Page Inspectoru, zobrazí se prvky, které ve zdrojových souborech (včetně kódu na straně serveru) je tvořen kód HTML, které mají být vykresleny v prohlížeči. Nástroj Page Inspector rovněž umožňuje změnit vlastnosti CSS a atributy elementu DOM na provedené změny se projeví okamžitě v prohlížeči.
> 
> Tato praktická cvičení vás provede funkce Page Inspector a ukazují, jak je můžete využít k řešení problémů ve webových aplikacích. **Toto testovací prostředí obsahuje dva cvičení pomocí podobných toky, ale cílení různých technologií. Pokud jste vývojáři ASP.NET MVC, postupujte podle výkonu. Pokud se výkon použijte pro vývojáře webových formulářů, dva**.
> 
> Tato laboratoř vás provede vylepšení a nových funkcí popsaných dříve použitím menší změny na ukázkovou webovou aplikaci ve zdrojové složce k dispozici.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této praktická cvičení se dozvíte, jak:

- Rozložit webového serveru pomocí nástroje Page Inspector
- Kontrola a náhled změn stylů CSS s nástrojem Page Inspector
- Detekovat a opravovat problémy na webových stránkách použití Page Inspectoru

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha A](#AppendixA) pokyny k jeho instalaci).
- Internet Explorer 9 nebo vyšší

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické testovací prostředí obsahuje následující praktická cvičení:

1. [Cvičení 1: Použití Page Inspectoru v projektech ASP.NET MVC](#Exercise1)
2. [Cvičení 2: Použití Page Inspectoru v projektech webových formulářů](#Exercise2)

> [!NOTE]
> Každý cvičení je přiložena počáteční řešení, umístěný ve složce Begin výkonu, který umožňuje postupovat podle jednotlivých výkon nezávisle na ostatních. Uvnitř zdrojový kód pro cvičení a také najdete složku koncovým obsahující řešení sady Visual Studio s kódem, který je výsledkem dokončení kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické vyzkoušení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Cvičení 1: Použití Page Inspectoru v projektech ASP.NET MVC

V tomto cvičení se dozvíte, jak se ve verzi preview a ladit **ASP.NET MVC 4** řešení pomocí **nástroj Page Inspector**. Nejprve proveďte stručný představení nástroje další funkce, které usnadňují webové ladění procesu. Potom budete pracovat na webové stránce, která obsahuje problémy stylu. Se dozvíte, jak používat nástroj Page Inspector najít zdrojový kód, který generuje tento problém a jeho řešení.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Úloha 1 – zkoumání nástroj Page Inspector

V této úloze se dozvíte, jak používat nástroj Page Inspector v rámci projektu aplikace ASP.NET MVC 4, který zobrazuje Fotogalerie.

1. Otevřít **začít** řešení nachází v **zdroj/Ex1-MVC4/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. V Průzkumníku řešení vyhledejte **Index.cshtml** zobrazení v části **/zobrazení Domů** složce projektu pravým tlačítkem myši a vyberte **zobrazení v nástroje Page Inspector**.

    ![Vyberte soubor, který chcete zobrazit náhled v nástroje Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "vyberete soubor, který chcete zobrazit náhled v nástroje Page Inspector")

    *Vyberte soubor, který chcete zobrazit náhled v nástroje Page Inspector*
3. V okně nástroje Page Inspector se zobrazí */Home/Index* adresa URL namapováno na zdroj zobrazení, které jste vybrali.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Prvním kontaktu s nástrojem Page Inspector*

    Nástroj Page Inspector je integrovaná v prostředí Visual Studio. Inspektor obsahuje vložený prohlížeče, společně s výkonný profiler HTML. Všimněte si, že nemáte ke spuštění řešení zobrazíte vzhled stránky.

    > [!NOTE]
    > Pokud šířka prohlížeče nástroj Page Inspector je menší než šířka otevřené stránky, nebude správně najdete na stránce. Pokud k tomu dojde, umožňuje upravte šířku nástroj Page Inspector.
4. Klikněte na tlačítko **soubory** kartu v nástroje Page Inspector.

    Zobrazí se všechny zdrojové soubory, které jsou sestavování indexovou stránku. Tato funkce pomáhá identifikovat všechny prvky na první pohled, zejména v případě, že pracujete s částečná zobrazení a šablony. Všimněte si, že můžete otevřít také všechny soubory kliknutím na odkazy.

    ![Kartě soubory](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Na kartě soubory*
5. Klikněte na tlačítko **přepnout režim kontroly** tlačítka umístěny na levé straně karty.

    Tento nástroj vám umožní vybrat jakýkoli element na stránce a zobrazit její kód HTML a syntaxe Razor.

    ![Přepnout kontrolu režimu tlačítka](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Přepínací tlačítko režim kontroly*
6. V prohlížeči nástroj Page Inspector přesuňte ukazatel myši nad prvky stránky. Když přesunete ukazatel myši nad libovolné části na vykreslené stránce, zobrazí se typ elementu a odpovídající zdrojový kód nebo kód je zvýrazněn v editoru sady Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Režim kontroly v akci*

    > [!NOTE]
    > Není maximalizujte okno nástroje Page Inspector nebo nebudete moci zobrazit na kartě preview zobrazuje zdrojový kód. Pokud klepnete na tlačítko na prvek v nástroje Page Inspector je maximalizované, zdrojový kód výběru se zobrazí, ale to se skryje okno nástroje Page Inspector.

    Pokud se věnovat pozornost **Index.cshtml** souboru, všimnete si, že je zvýrazněn část zdrojového kódu, který generuje vybraný prvek. Tato funkce usnadňuje úpravy dlouhé zdrojových souborů s přímým přístupem a rychlý způsob, jak přistupovat ke kódu.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Kontrola elementy*
7. Klikněte na tlačítko **přepnout režim kontroly** tlačítko (![vyberte kartu HTML k zobrazení kódu HTML, vykreslení v prohlížeči nástroj Page Inspector.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Vyberte kartu HTML k zobrazení kódu HTML, vykreslení v prohlížeči nástroj Page Inspector.") ) Chcete-li zakázat kurzor.
8. Vyberte **HTML** karet zobrazit další kód HTML, vykreslení v prohlížeči nástroj Page Inspector.
9. V kódu HTML vyhledejte položky seznamu s odkazem medvídek a vyberte ho.

    Všimněte si, že když vyberete kód, odpovídající výstup se automaticky zvýrazní v prohlížeči. Tato funkce je užitečná, pokud chcete zobrazit, jak je vykreslen blok HTML na stránce.

    ![Výběr prvku HTML na stránce](using-page-inspector-in-visual-studio-2012/_static/image8.png "elementu na stránce Výběr jazyka HTML")

    *Výběrem prvku HTML na stránce*
10. Klikněte na tlačítko **přepnout režim kontroly** tlačítko povolíte *režim kontroly* a klikněte na navigačním panelu. Na pravé straně kód HTML, v podokně styly zobrazí se seznam se styly CSS použitý pro vybraný element.

    > [!NOTE]
    > Záhlaví je součástí rozložení lokality, nástroj Page Inspector se také otevřít \_Layout.cshtml Souborová služba a zvýraznění vliv na segment kódu.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Zjišťování – styly a zdrojové soubory vybraný element*
11. Přepnout kontrolu ukazatel povolena přesuňte ukazatel myši níže na modrém panelu vybrané a klikněte na polovinu kruh.

    ![Výběr elementu](using-page-inspector-in-visual-studio-2012/_static/image10.png "výběr elementu")

    *Výběr elementu*
12. V podokně styly, vyhledejte **obrázek pozadí** položku **.main obsah** skupiny. **Zrušte zaškrtnutí políčka** **obrázek pozadí** a podívejte se, co se stane. Můžete si všimnout, že prohlížeč bude okamžitě odrážejí změny a kruhu je skrytá.

    > [!NOTE]
    > Změny, které můžete použít na kartě Page Inspector styly nemají vliv na původní šablony stylů. Můžete zrušit zaškrtnutí styly nebo změnit jejich hodnoty tolikrát, kolikrát chcete, aby je však obnoví po aktualizaci stránky.

    ![Povolení a zakázání styly CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "povolení a zakázání stylů CSS")

    *Povolení a zakázání stylů CSS*
13. Nyní, klikněte "**vaše logo**" textu na záhlaví v režimu kontroly.
14. V **styly** kartu, vyhledejte **velikost písma** šablon stylů CSS atribut **.site název** skupiny. Klikněte dvakrát na hodnotu atributu a nahraďte hodnotu 2.3 em s **3 em**a potom stiskněte klávesu **ENTER**. Všimněte si, že název bude vypadat větší.

    ![Změna hodnot šablon stylů CSS v nástroje Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image12.png "hodnoty změna šablony stylů CSS v nástroje Page Inspector")

    *Změna hodnot šablon stylů CSS v nástroje Page Inspector*
15. Klikněte na tlačítko **styly trasování** kartě nachází v pravém podokně nástroje Page Inspector. Toto je alternativní způsob zobrazíte všechny styly použité k výběru, seřazené podle názvu atributu.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *CSS styly trasování vybraného prvku*
16. Jiné funkce nástroje Page Inspector je panelu rozložení. Pomocí režimu kontroly, vyberte na navigačním panelu a klikněte **rozložení** kartu v pravém podokně. Zobrazí se přesnou velikost vybraného prvku a jeho velikost odsazení, marže, odsazení a ohraničení. Všimněte si, že můžete také změnit hodnoty z tohoto zobrazení.

    ![Element rozložení v nástroje Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "rozložení elementu v nástroje Page Inspector")

    *Element rozložení v nástroje Page Inspector*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Úloha 2 – najít a opravit problémy stylu v galerii fotografií

Jak by diagnostikovat problémy webových stránek s předchozími verzemi sady Visual Studio? Budete pravděpodobně známé ladicí nástroje, které běží mimo Visual Studio IDE, jako jsou nástroje pro vývojáře v Internet Exploreru nebo Firebug webů. Prohlížeče pouze pochopit HTML, skriptů a stylů, zatímco základní architektury generuje kód HTML, který bude vykreslen. Z tohoto důvodu je často potřeba nasadit celý web a zobrazte jak webové stránky vypadat.

Pokud chcete zjistit a opravit chyby na vašem webu měl pravděpodobně postupovali podle těchto kroků:

1. Spuštění řešení v sadě Visual Studio, nebo nasadit stránky na webovém serveru.
2. V prohlížeči otevřete vývojářské nástroje použít, nebo jednoduše otevřete zdrojový kód a styly a zkuste to tak, aby odpovídaly problém. K nalezení souborů zahrnutých, by jste použili &quot;hledání&quot; nebo &quot;hledání v souborech&quot; funkce s názvem třídy stylu.
3. Jakmile se zjistila chyba, zastavte webový prohlížeč a server.
4. Vymažte mezipaměť prohlížeče.
5. Vraťte se do sady Visual Studio použít opravu. Zopakujte všechny kroky k testování.

Protože neexistuje žádný skutečný WYSIWYG v architektuře ASP.NET MVC 4, se zjišťují většinu problémy stylu v pozdější fázi, po spuštěna nebo k nasazení webové aplikace. Nyní s nástrojem Page Inspector je možné zobrazit náhled jakékoli stránky bez spuštění řešení.

V této úloze budete používat nástroj Page inspector a opravit některé problémy aplikace Fotogalerie.

1. Použití Page Inspectoru, vyhledejte **zaregistrovat** a **přihlášení** odkazy na levé straně záhlaví.

    Všimněte si, že odkazy se nezobrazují očekávané místa na pravé straně a zobrazí se jako seznam s odrážkami. Teď budete zarovnat odkazy na pravé straně a změna stylu je odpovídajícím způsobem.

    ![Umístění registru a protokolu v odkazech](using-page-inspector-in-visual-studio-2012/_static/image15.png "umístění registru a protokolu v odkazech")

    *Umístění registru a protokolu v odkazech*
2. Přepnout režim kontroly vybrali klepněte na tlačítko Zavřít, ale ne, zaregistrujte odkaz k otevření jeho kód.

    Všimněte si, že se nachází zdrojový kód z odkazů v  **\_LoginPartial.cshtml** souboru, ne Index.cshtml ani \_Layout.cshtml, které jsou místa, podívejte se na prvním místě. Mít umístěný přímo v souboru správný zdroj.
3. V **styly** kartu, vyhledejte a klikněte **<section> #login</section>** položky, což je kontejner ve formátu HTML pro tyto odkazy.

    Všimněte si, že **#login** styl automaticky nachází v **Site.css** po klepnutí na tlačítko. Kromě toho se teď zvýrazní kód.

    ![Výběr styly CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "výběr stylů CSS")

    *Výběr stylů CSS*
4. Zrušením komentáře u **text-align** atribut zvýrazněný kód tak, že odeberete počátečních a koncových znaků a uložit **Site.css** souboru.

    Nástroj Page Inspector zohledňuje různých souborů, které tvoří aktuální stránku a může zjistit, když se změní některý z těchto souborů. Upozorní vás, když aktuální stránku v prohlížeči se nesynchronizuje se zdrojovými soubory.
5. V prohlížeči nástroj Page Inspector klikněte na panel nacházel pod do adresního řádku načtením stránky.

    ![Znovu načíst tuto stránku](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Znovu načíst tuto stránku*

    Tyto odkazy jsou teď na pravé straně, ale stále vypadat jako seznam s odrážkami. Nyní odeberete odrážky a zarovnat vodorovně odkazy.

    ![Aktualizovaná stránka](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Aktualizovaná stránka*
6. Použití režimu kontroly, vyberte některou z **&lt;li&gt;** položek, které obsahují &quot;zaregistrovat&quot; a &quot;přihlášení&quot; odkazy. Klikněte  **&lt;části&gt; #login** položku přístupu **Styles.css** kódu.

    ![Hledání styl](using-page-inspector-in-visual-studio-2012/_static/image19.png "hledání styl")

    *Hledání styl*
7. V **Style.css**, zrušte komentář kódu pro **#login li** položky. Styl, který chcete přidat, se skrýt odrážky a zobrazí položky vodorovně.

    ![Vytvoření nového stylu odkazy přihlášení](using-page-inspector-in-visual-studio-2012/_static/image20.png "vytvoření nového stylu odkazy přihlášení")

    *Vytvoření nového stylu odkazy přihlášení*
8. Uložit **Style.css** souboru a klikněte jednou na panelu nacházel pod adresu, kterou chcete znovu načíst stránku. Všimněte si, že odkazy zobrazovat správně.

    ![Odkazy zarovnána na pravou stranu](using-page-inspector-in-visual-studio-2012/_static/image21.png "odkazy zarovnána na pravou stranu")

    *Odkazy pravou stranu*
9. Nakonec se změní název hlavičky. Režim kontroly kliknutím **vaše logo** text a pusťte se do zdrojový kód, který jej generuje.
10. Nyní jste v  **\_Layout.cshtml**, nahraďte '**vaše logo**'text s'**Fotogalerie**". Uložte a aktualizujte prohlížeč nástroj Page Inspector.

    ![Přiřazuje se název nového](using-page-inspector-in-visual-studio-2012/_static/image22.png "přiřazení nový nadpis")

    *Přiřazení nový nadpis*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Aktualizovat stránku galerie fotografií*
11. Nakonec vyberte **Fotogalerie** projektu a stiskněte klávesu **F5** ke spuštění aplikace. Přečtěte si všechno, co se změny fungovat podle očekávání.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Cvičení 2: Použití Page Inspectoru v projektech webových formulářů

V tomto cvičení se dozvíte, jak se ve verzi preview a ladit pomocí nástroje Page Inspector řešení webových formulářů. Nejprve proveďte stručný představení nástroje další funkce nástroje Page Inspector, které usnadňují webové ladění procesu. Potom budete pracovat na webové stránce, která obsahuje problémy stylu. Se dozvíte, jak používat nástroj Page Inspector najít zdrojový kód, který generuje tento problém a jeho řešení.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Úloha 1 – zkoumání nástroj Page Inspector

V této úloze se dozvíte, jak používat funkce Page Inspector v kontextu, který zobrazuje Fotogalerie projekt webových formulářů.

1. Otevřít **začít** řešení nachází v **zdroj/Ex2-webovýchformulářů/počáteční/** složky.

   1. Budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. V Průzkumníku řešení vyhledejte **Default.aspx** stránce, pravým tlačítkem myši a vyberte **zobrazení v nástroje Page Inspector**.

    ![Otevírání Default.aspx s nástrojem Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "otevírání Default.aspx s nástrojem Page Inspector")

    *Otevírání Default.aspx s nástrojem Page Inspector*
3. V okně nástroje Page Inspector se zobrazí Default.aspx.

    ![Zobrazení Default.aspx v nástroje Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "zobrazení Default.aspx v nástroje Page Inspector")

    *Zobrazení Default.aspx v nástroje Page Inspector*

    Nástroj Page Inspector je integrovaná v prostředí Visual Studio. Inspektor obsahuje vložený prohlížeče, společně s výkonný profiler HTML, který zobrazí vybraný kód. Všimněte si, že nemáte ke spuštění řešení zobrazíte vzhled stránky.

    > [!NOTE]
    > Pokud šířka prohlížeče nástroj Page Inspector je menší než šířka otevřené stránky, nebude správně najdete na stránce. Pokud k tomu dojde, umožňuje upravte šířku nástroj Page Inspector.
4. Klikněte na tlačítko **soubory** kartu v nástroje Page Inspector.

    Zobrazí se všechny zdrojové soubory, které jsou sestavení na vykreslené stránce výchozí. To je užitečné pro identifikaci všechny prvky na první pohled, zejména v případě, že pracujete s uživatelskými ovládacími prvky a hlavní stránky. Všimněte si, že můžete také přejít na všechny soubory.

    ![Na kartě soubory](using-page-inspector-in-visual-studio-2012/_static/image26.png "kartu soubory")

    *Na kartě soubory*
5. Klikněte na tlačítko **přepnout režim kontroly** tlačítka umístěny na levé straně karty.

    Tento nástroj vám umožní vybrat jakýkoli element na stránce a zobrazit jeho zdrojový kód a aspx HTML.

    ![Režim kontroly přepínač](using-page-inspector-in-visual-studio-2012/_static/image27.png "režim kontroly přepínací tlačítko")

    *Přepínací tlačítko režim kontroly*
6. V prohlížeči nástroj Page Inspector najeďte myší nad prvky stránky. Když přesunete ukazatel myši nad libovolné části na vykreslené stránce, zobrazí se typ elementu a odpovídající zdrojový kód nebo kód je zvýrazněn v editoru sady Visual Studio.

    ![Režim kontroly v akci](using-page-inspector-in-visual-studio-2012/_static/image28.png "režim kontroly v akci")

    *Režim kontroly v akci*

    > [!NOTE]
    > Není maximalizujte okno nástroje Page Inspector nebo nebudete moci zobrazit na kartě preview zobrazuje zdrojový kód. Pokud klepnete na tlačítko na prvek v nástroje Page Inspector je maximalizované, zdrojový kód výběru se zobrazí, ale to se skryje okno nástroje Page Inspector.

    Pokud se věnovat pozornost **Default.aspx** souboru, všimnete si, že je zvýrazněn část zdrojového kódu, který generuje vybraný prvek. Tato funkce usnadňuje edice long zdrojových souborů s přímým přístupem a rychlý způsob, jak přistupovat ke kódu.

    ![Kontrola prvky](using-page-inspector-in-visual-studio-2012/_static/image29.png "kontrola elementy")

    *Kontrola elementy*
7. Klikněte na tlačítko **přepnout režim kontroly** tlačítko (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), umístěný na kartách nástroj Page Inspector, chcete-li zakázat kurzor.
8. Vyberte **HTML** karet zobrazit další kód HTML, vykreslení v prohlížeči nástroj Page Inspector.
9. V kódu HTML vyhledejte položky seznamu s odkazem medvídek a vyberte ho.

    Všimněte si, že když vyberete kód, odpovídající výstup je automaticky zvýrazněné prohlížeče. Tato funkce je užitečná, pokud chcete zobrazit, jak je vykreslen blok HTML na stránce.

    ![Výběrem prvku HTML na stránce](using-page-inspector-in-visual-studio-2012/_static/image31.png "výběrem prvek HTML na stránce")

    *Výběrem prvku HTML na stránce*
10. Klikněte na tlačítko **přepnout režim kontroly** tlačítko povolíte *režim kontroly* a klikněte na navigačním panelu. Na pravé straně kód HTML, v podokně styly zobrazí se seznam se styly CSS použitý pro vybraný element.

    > [!NOTE]
    > záhlaví je součástí rozložení webu, nástroj Page Inspector také otevřete soubor Site.Master a zvýraznit segmentu kódu vliv.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "zjišťování – styly a zdrojové soubory vybraný element")

    *Zjišťování – styly a zdrojové soubory vybraný element*
11. Přepnout kontrolu ukazatel povolena přesuňte ukazatel myši níže nabídek a klikněte na prázdnou půlkruh.

    ![Výběr elementu](using-page-inspector-in-visual-studio-2012/_static/image33.png "výběr elementu")

    *Výběr elementu*
12. V podokně styly, vyhledejte **obrázek pozadí** položku **.main obsah** skupiny. **Zrušte zaškrtnutí políčka** **obrázek pozadí** a podívejte se, co se stane. Můžete si všimnout, že prohlížeč bude okamžitě odrážejí změny a kruhu je skrytá.

    > [!NOTE]
    > Změny, které můžete použít na kartě Page Inspector styly nemají vliv na původní šablony stylů. Můžete zrušit zaškrtnutí styly nebo změnit jejich hodnoty tolikrát, kolikrát chcete, aby je však obnoví po aktualizaci stránky.

    ![Povolení a zakázání šablon stylů CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "povolení a zakázání stylů CSS")

    *Povolení a zakázání stylů CSS*
13. Nyní, klikněte na tlačítko "**vaše** **logo"** textu na záhlaví v režimu kontroly.
14. V **styly** kartu, vyhledejte **velikost písma** šablon stylů CSS atribut **.site název** skupiny. Poklepejte na atribut jednou upravit její hodnotu. Nahraďte 2.3em hodnotu s **3em**, a potom stiskněte klávesu ENTER. Všimněte si, že název bude vypadat větší.

    ![Změna hodnot šablon stylů CSS v Inspector2 stránky](using-page-inspector-in-visual-studio-2012/_static/image35.png "hodnoty změna šablony stylů CSS v nástroje Page Inspector")

    *Změna hodnot šablon stylů CSS v nástroje Page Inspector*
15. Klikněte na tlačítko **styly trasování** kartě nachází v pravém podokně nástroje Page Inspector. Toto je alternativní způsob zobrazíte všechny styly použité k výběru, seřazené podle názvu atributu.

    ![CSS styly trasování vybraného prvku](using-page-inspector-in-visual-studio-2012/_static/image36.png "styly sledování CSS vybraného prvku")

    *CSS styly trasování vybraného prvku*
16. Jiné funkce nástroje Page Inspector je panelu rozložení. Pomocí režimu kontroly, vyberte na navigačním panelu a klikněte **rozložení** kartu v pravém podokně. Zobrazí se přesnou velikost vybraného prvku a jeho velikost odsazení, marže, odsazení a ohraničení. Všimněte si, že můžete také změnit hodnoty z tohoto zobrazení.

    ![Element rozložení v nástroje Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "rozložení elementu v nástroje Page Inspector")

    *Element rozložení v nástroje Page Inspector*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Úloha 2 – najít a opravit problémy stylu v galerii fotografií

Jak by diagnostikovat problémy webových stránek s předchozími verzemi sady Visual Studio? Budete pravděpodobně známé ladicí nástroje, které běží mimo Visual Studio IDE, jako jsou nástroje pro vývojáře v Internet Exploreru nebo Firebug webů. Prohlížeče pouze pochopit HTML, skriptů a stylů, zatímco základní architektury generuje kód HTML, který bude vykreslen. Z tohoto důvodu je často potřeba nasadit celý web a zobrazte jak webové stránky vypadat.

Pokud chcete zjistit a opravit chyby na vašem webu měl pravděpodobně postupovali podle těchto kroků:

1. Spuštění řešení v sadě Visual Studio, nebo nasadit stránky na webovém serveru.
2. V prohlížeči otevřete vývojářské nástroje použít, nebo jednoduše otevřete zdrojový kód a styly a zkuste to tak, aby odpovídaly problém. K nalezení souborů zahrnutých, by jste použili &quot;hledání&quot; nebo &quot;hledání v souborech&quot; funkce s názvem třídy stylu.
3. Jakmile se zjistila chyba, zastavte webový prohlížeč a server.
4. Vymažte mezipaměť prohlížeče.
5. Vraťte se do sady Visual Studio použít opravu. Zopakujte všechny kroky k testování.

Protože není žádné skutečné WYSIWYG ve webových formulářů ASP.NET, některé problémy stylu zjištění v pozdější fázi, po spuštění nebo nasazení. Nyní s nástrojem Page Inspector je možné zobrazit náhled jakékoli stránky bez spuštění řešení.

V této úloze použijete nástroj Page inspector pro řešení některých problémů aplikace Fotogalerie. V následujícím postupu bude zjišťovat a rychle opravit některé jednoduché styly problém v záhlaví.

1. Pomocí stránky kontroly, vyhledejte **zaregistrovat** a **přihlásit** odkazy na levé straně záhlaví.

    Všimněte si, že se nezobrazí odkaz na očekávaný místo na pravé straně. Teď budete zarovnat na odkaz vpravo a změna stylu odpovídajícím způsobem.

    ![Přihlaste se odkaz umístěný na levé straně](using-page-inspector-in-visual-studio-2012/_static/image38.png "přihlášení odkaz umístěný na levé straně")

    *Umístěny na levé straně odkaz přihlásit*
2. Přepnout režim kontroly vybraná vyberte odkaz přihlásit, otevřete jeho kód.

    Všimněte si, že odkaz zdrojového kódu je umístěn v **Site.Master** souboru, není na stránce Default.aspx, což je místem, kde lze najít na prvním místě; vložení přímo v souboru správný zdroj.
3. V **styly** kartu, vyhledejte a klikněte  **&lt;části&gt; #login** položky, což je kontejner ve formátu HTML pro tyto odkazy.

    Všimněte si, že **#login** styl automaticky nachází v **Site.css** po klepnutí na tlačítko. Kromě toho se teď zvýrazní kód.

    ![Výběr styly CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "výběr stylů CSS")

    *Výběr stylů CSS*
4. Zrušením komentáře u **text-align** atribut zvýrazněný kód tak, že odeberete počátečních a koncových znaků a uložit **Site.css** souboru.

    Nástroj Page Inspector zohledňuje různých souborů, které tvoří aktuální stránku a může zjistit, když se změní některý z těchto souborů. Upozorní vás, když aktuální stránku v prohlížeči se nesynchronizuje se zdrojovými soubory.
5. V prohlížeči nástroj Page Inspector klikněte na panel nacházel pod panelu Adresa a uložte změny a znovu načíst stránku.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Znovu načíst tuto stránku*

    Tyto odkazy jsou teď na pravé straně, ale stále vypadat jako seznam s odrážkami. Nyní odeberete odrážky a zarovnat vodorovně odkazy.

    ![Aktualizovaná stránka](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Aktualizovaná stránka*
6. Použití režimu kontroly, vyberte některou z **&lt;li&gt;** položek, které obsahují &quot;zaregistrovat&quot; a &quot;přihlášení&quot; odkazy. Klikněte  **&lt;části&gt; #login** položku přístupu **Styles.css** kódu.

    ![Hledání styl](using-page-inspector-in-visual-studio-2012/_static/image42.png "hledání styl")

    *Hledání styl*
7. V **Style.css**, zrušte komentář kódu pro **#login li** položky. Styl, který chcete přidat, se skrýt odrážky a zobrazí položky vodorovně.

    ![Vytvoření nového stylu odkazy přihlášení](using-page-inspector-in-visual-studio-2012/_static/image43.png "vytvoření nového stylu odkazy přihlášení")

    *Vytvoření nového stylu odkazy přihlášení*
8. Uložit **Style.css** souboru a klikněte jednou na panelu nacházel pod adresu, kterou chcete znovu načíst stránku. Všimněte si, že odkazy zobrazovat správně.

    ![Odkazy zarovnána na pravou stranu](using-page-inspector-in-visual-studio-2012/_static/image44.png "odkazy zarovnána na pravou stranu")

    *Odkazy pravou stranu*
9. Nakonec se změní název hlavičky. Nemusíte hledat "**vaše logo"** text ve všech souborech v režimu kontroly kliknutím na text a získat zdrojový kód, který ho generuje.

    ![Vyhledání názvu webu](using-page-inspector-in-visual-studio-2012/_static/image45.png "hledání názvu webu")

    *Vyhledání názvu serveru*
10. Nyní jste v **Site.Master**, nahraďte "**vaše logo**'text s'**Fotogalerie**". Uložte a aktualizujte prohlížeč nástroj Page Inspector.

    ![Aktualizovat stránku galerie fotografií](using-page-inspector-in-visual-studio-2012/_static/image46.png "aktualizovat stránku galerie fotografií")

    *Aktualizovat stránku galerie fotografií*
11. Nakonec stiskněte **F5** ke spuštění aplikace, prohlédněte si všechny změny fungovat podle očekávání.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Po dokončení tohoto praktického testovacího prostředí, mají učení pomocí nástroje Page Inspector ve verzi preview webové aplikace bez nutnosti znovu sestavit a spustit webovou stránku v prohlížeči. Kromě toho mají dozvědět jak rychle najdete a opravíte chyby díky přístupu do zdrojového kódu přímo z vykresleného výstupu.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Příloha A: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express for Web dlaždice*
