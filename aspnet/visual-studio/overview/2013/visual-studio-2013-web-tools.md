---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Praktické cvičení: Visual Studio 2013 nástroje pro Web | Dokumentace Microsoftu'
author: rick-anderson
description: Visual Studio je skvělé vývojové prostředí pro. Na základě NET Windows a webové projekty. Obsahuje výkonné textový editor, který můžete jednoduše použít na...
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 9553d3129ff4c942eacbba628d1daaf6c4e33635
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807525"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Praktické cvičení: Visual Studio 2013 webové nástroje
====================
podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](http://aka.ms/webcamps-training-kit)

> Visual Studio je skvělé vývojové prostředí pro. Na základě NET Windows a webové projekty. Obsahuje výkonné textový editor, který lze snadno použít k úpravě samostatné soubory bez projektu.
> 
> Visual Studio udržuje strom plně funkční analýzy při úpravě každého souboru. Díky tomu Visual Studio a poskytují bezkonkurenční automatického dokončování a založenou na dokumentech akce při vytváření vývojové prostředí, příjemnější a mnohem rychlejší. Tyto funkce jsou zvláště efektivní v dokumentech HTML a CSS.
> 
> Veškerý tento výkon je k dispozici pro rozšíření, usnadňuje rozšíření editorů výkonné nové funkce tak, aby odpovídala vašim potřebám. Web Essentials je kolekce (většinou) související s webem vylepšení sady Visual Studio. Obsahuje i velké množství nového dokončování IntelliSense (hlavně u šablon stylů CSS), nové funkce Browser Link, automatické soubory JSHint pro jazyk JavaScript, nová upozornění pro HTML a CSS a mnoho dalších funkcí, které jsou nezbytné pro moderního webového vývoje.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této praktická cvičení se dozvíte, jak:

- Použít nové funkce editoru HTML součástí Web Essentials, například bohaté fragmenty kódu HTML5 a Zen kódování
- Použít nové funkce editoru šablon stylů CSS součástí Web Essentials, jako je například výběr barvy a popisu matice prohlížeče
- Použít nové funkce editoru jazyka JavaScript součástí Web Essentials, jako je například extrahování do souboru a technologii IntelliSense pro všechny prvky jazyka HTML
- Výměna dat mezi prohlížeče a použití funkce Browser Link sady Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

K dokončení této praktické testovací prostředí jsou vyžadovány následující položky:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) nebo vyšší
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Chcete-li spustit praktická cvičení v této praktické testovací prostředí, musíte nejdřív nastavit prostředí.

1. Otevřete okno Průzkumníka Windows a přejděte do testovacího prostředí **zdroj** složky.
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

1. [Práce s Browser Link a Web Essentials](#Exercise1)
2. [Využití výhod fragmenty kódu a technologii IntelliSense](#Exercise2)

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržená tak, aby odpovídala konkrétním vývojářským styl a určuje rozložení oken, chování editoru, fragmenty kódu technologie IntelliSense a možnosti dialogového okna. Postupy v tomto testovacím prostředí jsou uvedené akce potřebné k provedení dané úlohy v sadě Visual Studio při použití **obecným vývojovým nastavením** kolekce. Pokud se rozhodnete různá nastavení kolekce pro vaše vývojové prostředí, mohou existovat rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Cvičení 1: Práce s Browser Link a Web Essentials

**Web Essentials** je rozšíření sady Visual Studio, která vytváří celou řadu užitečných funkcí pro vývoj moderních webových, především soustředí se na měřitelné webové vývojové prostředí příjemnější a mnohem rychlejší. Web Essentials můžete nainstalovat z Galerie rozšíření v sadě Visual Studio.

**Browser Link** je nová funkce zahrnuté v sadě Visual Studio 2013, která poskytuje kanál mezi integrovaném vývojovém prostředí sady Visual Studio a jakýmkoli spuštěným prohlížečem pro výměnu dat mezi vaší webové aplikace a sadě Visual Studio. Web Essentials rozšiřuje Browser Link s nástroji pro manipulaci s objektový model DOM a CSS stylů webových stránek přímo z prohlížeče.

V tomto cvičení bude prozkoumat některé z funkcí podporovaných **Web Essentials** a **Browser Link** k vylepšení stránku jednoduché kvíz.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Úloha 1 – spouštění projektu do více prohlížečů

V této úloze nakonfigurujete webové aplikace na spouštění v několika prohlížečích najednou, což se hodí při testování prohlížečů.

1. Otevřít **Microsoft Visual Studio**.
2. V **souboru** příkaz **Open | Projekt nebo řešení...**  a přejděte do **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** v **zdroj** složky testovacího prostředí (C:\WebCampsTK\HOL\VSWebTooling\Source). Vyberte **Begin.sln** a klikněte na tlačítko **otevřít**.
3. Na panelu nástrojů sady Visual Studio v nabídce prohlížeče rozbalte a vyberte **procházet s...** .

    ![Možnost nabídky Procházet se](visual-studio-2013-web-tools/_static/image1.png "procházet s... v nabídce prohlížeče")

    *Procházet pomocí nabídky*
4. V **procházet s** dialogovém okně vyberte **Google Chrome** a **aplikace Internet Explorer** současným **CTRL** key a klikněte na tlačítko  **Nastavit jako výchozí**.

    ![Procházet s dialogovým oknem](visual-studio-2013-web-tools/_static/image2.png "procházet s dialogovým oknem")

    *Výběr více výchozích prohlížečů*
5. Google Chrome a prohlížeče Internet Explorer by měla nyní zobrazují jako nastavení výchozích prohlížečů. Klikněte na tlačítko **zrušit** zavřete dialogové okno.

    ![Google Chrome a prohlížeče Internet Explorer jako výchozího prohlížeče](visual-studio-2013-web-tools/_static/image3.png "výchozího prohlížeče Google Chrome a Internet Explorer")

    *Google Chrome a prohlížeče Internet Explorer jako výchozího prohlížeče*

    > [!NOTE]
    > Po konfiguraci nastavení výchozích prohlížečů **více prohlížečů** je vybraná možnost v nabídce prohlížeče.
    > 
    > ![Více prohlížečů](visual-studio-2013-web-tools/_static/image4.png "více prohlížečů")
6. Stisknutím klávesy **CTRL** + **F5** ke spuštění aplikace bez ladění.
7. Při otevření obě okna prohlížeče, umístíte jeden z nich nad nich chcete-li zobrazit aktualizace v prohlížečích, oba současně. Prohlížečů zobrazit webovou stránku pomocí světle modrý obdélník.

    ![Umístit jeden prohlížeč nad sebou](visual-studio-2013-web-tools/_static/image5.png "umístit jeden prohlížeč nad sebou")

    *Umístit jeden prohlížeč nad sebou*
8. Nezavírejte prohlížečů. Je použijete v dalším úkolem.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Úloha 2 – Zen pomocí kódování k vytváření prvků HTML

**Kódování Zen** je editor kódování modul plug-in pro vysokorychlostní HTML, XML, XSL (nebo jiný formát strukturovaný kód) a úpravy. Základní tento modul plug-in je zkratka pro efektivní modul, který umožňuje rozšířit výrazů - podobný selektorů CSS - do kódu HTML. Kódování Zen je rychlý způsob zápisu selektor syntaxe stylu HTML pomocí šablony stylů CSS.

V tomto cvičení se použije k vygenerování tlačítka jazyka HTML představující možnosti dotaz Zen kódování funkcí poskytovaných Web Essentials.

1. Přepněte zpět do sady Visual Studio.
2. Otevřít **Index.cshtml** soubor umístěný ve **zobrazení** | **Domů** složky.
3. Nahraďte **&lt;!--TODO: přidejte možnosti--&gt;** komentář s následujícím kódem a stisknutím klávesy **kartu**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Kód by měly být rozbalené do formátu HTML.

    ![Rozbalit HTML](visual-studio-2013-web-tools/_static/image6.png "rozšířit HTML")

    *Rozšířené HTML*

    > [!NOTE]
    > Další informace o psaní kódu Zen syntaxe, naleznete na následujícím [článku](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Klikněte na tlačítko **aktualizovat propojené prohlížeče** tlačítko Aktualizovat i prohlížeče.

    ![Aktualizovat propojené prohlížeče](visual-studio-2013-web-tools/_static/image7.png "aktualizovat propojené prohlížeče")

    *Aktualizovat propojené prohlížeče*

    ![Internet Explorer - stránka aktualizována čtyři tlačítka](visual-studio-2013-web-tools/_static/image8.png "aplikace Internet Explorer - aktualizaci stránky se čtyřmi tlačítky")

    *Internet Explorer - aktualizaci stránky se čtyřmi tlačítky*

    ![Google Chrome – stránka aktualizována čtyři tlačítka](visual-studio-2013-web-tools/_static/image9.png "Google Chrome – stránka aktualizuje se čtyřmi tlačítky")

    *Google Chrome – stránka aktualizuje se čtyřmi tlačítky*
6. Přepněte zpět do sady Visual Studio.
7. Tlačítka jste přidali na stránku, ale je stále potřeba přidat otázku šablony. K tomu použijete novou funkci v Web Essentials volá **Lorem Ipsum generátor**. Vyhledejte **div** křížkem **třídy** atribut **front**.
8. Přidejte následující kód jako první podřízený element **div**a stiskněte klávesu **kartu**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Kód by měly být rozbalené do formátu HTML.

    ![Automaticky generované Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "automaticky generované Lorem Ipsum")

    *Automaticky generované Lorem Ipsum*

    > [!NOTE]
    > Jako součást Zen kódování teď můžete vygenerovat Lorem Ipsum kódu přímo v editoru jazyka HTML. Jednoduše zadáte **lorem** a přístupů **kartu** a word Lorem Ipsum text bude vložen 30. Například *lorem10* vloží 10 Lorem Ipsum slova.
10. Přidáte logo, které se v horní části na otázku pomocí další novou funkcí ve Web Essentials volá **Lorem Pixel generátor**. Přidejte následující kód jako první podřízený element **div** element s **kontejneru** jako **třídy** hodnotu a stiskněte klávesu **kartu**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Kód by měl rozšířit do formátu HTML.

    ![Automaticky generované lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "automaticky generované Lorem pixelů")

    *Automaticky generované lorem pixelů*

    > [!NOTE]
    > Jako součást Zen psaní kódu můžete vygenerovat také Lorem Pixel kódu přímo v editoru jazyka HTML. Jednoduše zadáte **pix. 200 x 200 zvířat** a přístupů **kartu** a **img** vloží značku s 200 x 200 obrázků zvířat. Další informace najdete v [Lorem Pixel](http://www.lorempixel.com).
12. Klikněte na tlačítko **aktualizovat propojené prohlížeče** tlačítko Aktualizovat i prohlížeče.

    ![Internet Explorer - automaticky generované obrázků a textu](visual-studio-2013-web-tools/_static/image12.png "aplikace Internet Explorer - automaticky generované obrázků a textu")

    *Internet Explorer - automaticky generované obrázků a textu*

    ![Google Chrome – automaticky generované obrázků a textu](visual-studio-2013-web-tools/_static/image13.png "Google Chrome – automaticky generované obrázků a textu")

    *Google Chrome – automaticky generované obrázků a textu*

    > [!NOTE]
    > Protože image je vybrán náhodně při přidání fragmentu kódu, se může lišit na obrázku je znázorněno v prohlížečích.
13. Nezavírejte prohlížečů. Je použijete v dalším úkolem.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Úloha 3 - aktualizace vlastnosti stylu

V této úloze budete používat Browser Link **kontrolovat režimu** funkce ke zjišťování přesné umístění, ve kterém se vygeneruje konkrétní elementu DOM a pak aktualizujte vlastnost barvu tohoto elementu s použitím barvu ovládacího prvku pro výběr k dispozici na webu Základy.

1. V prohlížeči Internet Explorer, stiskněte klávesu **CTRL** + **ALT** + **můžu** umožňující kontrolovat režimu.
2. Přesuňte ukazatel nad světla modré ohraničení a klikněte na tlačítko.

    ![Ukazatele přes hranice světle modrý](visual-studio-2013-web-tools/_static/image14.png "ukazatele přes hranice světle modrá")

    *Ukazatele přes hranice světle modrá*
3. Přepněte zpět do sady Visual Studio. Všimněte si, jak prvek HTML, který jste vybrali v prohlížeči také vybírá v editoru Visual Studio.

    ![Element HTML zvoleným v editoru Visual Studio HTML](visual-studio-2013-web-tools/_static/image15.png "zvoleným v editoru Visual Studio HTML elementu HTML")

    *Element HTML zvoleným v editoru Visual Studio HTML*
4. Bude nyní aktualizovat **přední** třídu šablony stylů CSS, chcete-li změnit stylu vybraného prvku. Chcete-li to provést, stiskněte **CTRL** + **,** otevřít **přejít na** vyhledávacího pole. Typ **site.css** a stiskněte klávesu **ENTER** k otevření souboru.

    ![Otevření souboru Site.css](visual-studio-2013-web-tools/_static/image16.png "otevírání souboru Site.css")

    *Otevírání souboru Site.css*
5. Stisknutím klávesy **CTRL** + **F** a typ **.flip containter .front** najít selektor šablon stylů CSS.
6. Klikněte na tlačítko světle modrý čtverec ve vlastnosti třídy pro otevření výběru barvy ohraničení.

    ![Otevření výběru barvy](visual-studio-2013-web-tools/_static/image17.png "otevření výběru barvy")

    *Otevření dialogu pro výběr barvy*
7. Rozbalit výběr barvy kliknutím tlačítko s dvojitou šipkou a vyberte novou barvu.

    ![Rozšíření výběru barvy](visual-studio-2013-web-tools/_static/image18.png "rozšíření výběr barvy")

    *Rozšíření výběr barvy*
8. Stisknutím klávesy **CTRL** + **ALT** + **ENTER** nutné aktualizovat propojené prohlížeče.
9. Přepněte do aplikace Internet Explorer a Všimněte si, jak se změní barva ohraničení.

    ![Internet Explorer – Barva ohraničení aktualizovat](visual-studio-2013-web-tools/_static/image19.png "aplikace Internet Explorer – Barva ohraničení aktualizovat")

    *Internet Explorer – Barva ohraničení aktualizovat*
10. Přepněte do Google Chrome a Všimněte si, jak se změní barva ohraničení.

    ![Google Chrome – Barva ohraničení aktualizovat](visual-studio-2013-web-tools/_static/image20.png "Google Chrome – Barva ohraničení aktualizovat")

    *Google Chrome – Barva ohraničení aktualizovat*
11. Přepněte zpět do sady Visual Studio.
12. Přejděte na konec objektu **Site.css** souboru a stiskněte klávesu **CTRL** + **F** vyhledejte **.btn** selektor.
13. Všimněte si, že **vlastnost - webkit-border-radius** vlastnost je podtržený zeleně.

    ![Vlastnost - webkit-border-radius selektoru btn](visual-studio-2013-web-tools/_static/image21.png "vlastnost - webkit-border-radius selektoru btn")

    *Vlastnost - webkit-border-radius selektoru btn*
14. Umístěte kurzor v **vlastnost - webkit-border-radius** vlastnost. Modrá čára by se zobrazit v rámci první písmeno prvního slova vlastnosti. Toto je **inteligentní značka**.
15. Stisknutím klávesy **CTRL** + **.** Otevření nabídky návrhy a klepněte na tlačítko **přidat chybí vlastnost standardní (border-radius)**.

    ![Přidat chybějící standardní vlastnosti návrh](visual-studio-2013-web-tools/_static/image22.png "přidat chybějící návrh standardní vlastnosti")

    *Přidat chybějící návrh standardní vlastnosti*
16. **Border-radius** automaticky přidána vlastnost do pravidla šablon stylů CSS.

    ![Chybí standard byla přidána vlastnost](visual-studio-2013-web-tools/_static/image23.png "chybí standard byla přidána vlastnost")

    *Chybí standard byla přidána vlastnost*
17. Přesunutí ukazatele myši **border-radius** vlastnost pro zobrazení **prohlížeče matice popisek**. **Prohlížeče matice popisek** zobrazuje dostupnost vlastnosti v každým prohlížečem.

    ![Popis matice prohlížeče](visual-studio-2013-web-tools/_static/image24.png "prohlížeče matice tooltip")

    *Popis matice prohlížeče*
18. Všimněte si, že hodnota **border-radius** vlastnost je stále podtržené. Přesuňte ukazatel nad hodnotu, která se zobrazí zpráva upozorňující.

    ![Poloměr ohraničení vlastnost hodnotu upozornění](visual-studio-2013-web-tools/_static/image25.png "Border-radius vlastnost hodnotu upozornění")

    *Poloměr ohraničení vlastnost hodnotu upozornění*
19. Odebrání jednotky **border-radius** hodnotu vlastnosti, jak je navrženo v popisu tlačítka.
20. Jako **border-radius** je vlastnost standard pro definování jak zaoblené rohy jsou, můžete odebrat ohraničení **vlastnost - webkit-border-radius** vlastnosti a hodnoty z pravidla šablon stylů CSS.
21. Umístěte kurzor v **zalamování slov** vlastnost a Všimněte si, že se inteligentní značky se zobrazí také níže.
22. Otevřete nabídku a klikněte na tlačítko **přidat chybějící specifika dodavatele**.

    ![Přidat chybějící dodavatele specifika návrhu](visual-studio-2013-web-tools/_static/image26.png "přidat chybějící dodavatele specifika návrhu")

    *Přidat chybějící dodavatele specifika návrhu*
23. **-Ms-zalamování** automaticky přidána vlastnost do pravidla šablon stylů CSS.

    ![Byla přidána vlastnost konkrétního dodavatele](visual-studio-2013-web-tools/_static/image27.png "byla přidána vlastnost konkrétního dodavatele")

    *Byla přidána vlastnost konkrétního dodavatele*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Úloha 4 – aktualizace kódu HTML z prohlížeče

V této úloze budete používat Browser Link **režimu návrhu** funkce Upravit objekt modelu DOM z prohlížeče a přenést změny do zdrojového souboru HTML v sadě Visual Studio.

1. V prohlížeči Google Chrome, stiskněte klávesu **CTRL** + **ALT** + **D** povolení režimu návrhu.
2. Přesunutí ukazatele myši **Lorem Ipsum dolor sit amet** označovat popisky a klikněte na tlačítko.

    ![Úpravy na otázku](visual-studio-2013-web-tools/_static/image28.png "úpravy na otázku")

    *Úpravy na otázku*
3. Kurzor by se zobrazit. Nahraďte původní text s *co to vypadá při psaní delší dotaz?* a potom stiskněte klávesu **ESC** pro ukončení režimu návrhu.

    ![Upravit dotaz](visual-studio-2013-web-tools/_static/image29.png "upravit dotaz")

    *Upravit dotaz*
4. Přepněte zpět do sady Visual Studio a otevřete **Index.cshtml**, pokud ještě není otevřen. Všimněte si, že vnitřní text **&lt;p&gt;** element byl aktualizován.

    ![Aktualizované dotaz na stránce HTML](visual-studio-2013-web-tools/_static/image30.png "aktualizované dotaz na stránce HTML")

    *Aktualizované dotaz na stránce HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Úloha 5: revize SEO související upozornění

**Optimalizace vyhledávacích** (vyhledávací weby SEO) je proces zpřístupnění vyšší webu pořadí na vyhledávací web ze seznamu výsledků. Vyšší webu řadí a čím více konzistentně je uveden, další návštěvníků webu získáte z tohoto vyhledávací web. Web Essentials zahrnuje analytického nástroje, který prověří HTML, sestavy problémy najít a poskytuje pomoc a opravte je.

1. Přejděte **zobrazení** nabídky a klikněte na **seznam chyb** otevřete **seznam chyb** okno.

    ![Seznam chyb v zobrazení nabídky](visual-studio-2013-web-tools/_static/image31.png "seznam chyb v nabídce zobrazení")

    *Seznam chyb v zobrazení nabídky*
2. Všimněte si, že dochází optimalizace pro vyhledávací weby upozornění s informací, které **&lt;meta&gt;** označit pro chybí popis stránky. Dvakrát klikněte na položku upozornění optimalizace pro vyhledávací weby ho opravit.

    ![Okno Seznam chyb](visual-studio-2013-web-tools/_static/image32.png "okna Seznam chyb")

    *Okno Seznam chyb*
3. V **Web Essentials** dialogové okno, klikněte na tlačítko **Ano** Vložit popis &lt;meta&gt; značky.

    ![Dialogové okno Web Essentials](visual-studio-2013-web-tools/_static/image33.png "dialogové okno Web Essentials")

    *Dialogové okno Web Essentials*
4. Editor pro  **\_Layout.cshtml** otevře a **&lt;meta&gt;** značky se automaticky přidá do **head** část Soubor HTML.

    ![Značka META automaticky přidá _rozložení stránce](visual-studio-2013-web-tools/_static/image34.png "metaznačku automaticky přidán do stránky _rozložení")

    *Značka META automaticky přidá do \_stránku rozložení*
5. Změňte hodnotu **obsah** atribut *GeekQuiz* a soubor uložte.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Cvičení 2: Využití výhod fragmenty kódu a technologii IntelliSense

S Web Essentials bylo rozšířeno pomocí další funkce editoru jazyka HTML. V tomto cvičení zobrazí se několik nových funkcí, které jsou užitečné při vývoji webových aplikací.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Úloha 1 – použití technologie IntelliSense do HTML dokumentů

První nové funkce, zobrazí se v této úloze se označuje jako **dynamické IntelliSense**. Dynamické IntelliSense načte další značky a atributů, které mají odvodit možné ID, které bude používat.

V této úloze vytvoříte nový prvek formuláře HTML, který obsahuje popisek a vstupní pole. Potom přidáte **pro** atribut label a vytvořte jeho vazbu na vstup a zobrazí se návrhy IntelliSense podle ID vstupů v oboru.

1. Otevřít **Visual Studio Express 2013 for Web** a **Begin.sln** řešení nachází v **zdroj/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** složky. Alternativně můžete pokračovat v řešení, který jste získali v předchozím cvičení.
2. V **Průzkumníka řešení**, otevřete **Index.cshtml** soubor umístěný ve **zobrazení** | **Domů** složky.
3. Přidejte následující formulář uvnitř **&lt;části&gt;** elementu.

    (Fragment - kódu *VisualStudio2013WebTooling* - *Ex2* - *formuláře*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Vstupní značky by měl předcházet popisek s přijde nějaký popis pole. Přidejte následující popisek před vstupní značka.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **Pro** atribut **&lt;popisek&gt;** Určuje, který element formuláře a popisku je vázán na. Hodnota atributu musí být rovna id elementu související. Přidat **pro** atribut **&lt;popisek&gt;** elementu. Jak je znázorněno na následujícím obrázku &quot;název&quot; hodnota se zobrazí v dialogovém okně technologie IntelliSense na základě id elementů v rámci stejného oboru (nadřazený  **&lt;formuláře&gt;**).

    ![Zobrazuje id v technologii IntelliSense](visual-studio-2013-web-tools/_static/image35.png "zobrazuje id v technologii IntelliSense")

    *Zobrazuje id v technologii IntelliSense*
6. Odstranit naposledy přidané **&lt;formuláře&gt;** elementu a jeho obsah.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Úloha 2 – používání fragmentů kódu HTML

HTML5 zavedené více než 25 nové sémantické značky. Visual Studio už měli podporu technologie IntelliSense pro tyto značky, ale Visual Studio 2013 umožňuje rychlejší a snazší psát kód tak, že přidáte nový fragmenty kódu. I když tyto značky nejsou složité, jsou součástí několik odlišností malé, jako je například přidávání náhrad správný kodek pro *zvuku* značky. V této úloze zobrazí se fragmenty kódu HTML pro značku pro zvuk.

1. V **Index.cshtml** soubor, zadejte  **&lt;aud** uvnitř **&lt;části&gt;** elementu, jak je znázorněno na následujícím obrázku.

    ![Vložení zvuku elementu](visual-studio-2013-web-tools/_static/image36.png "vložení zvuku elementu")

    *Vložení zvuku elementu*
2. Stisknutím klávesy **kartu** dvakrát a Všimněte si, jak se na stránku přidá následující kód a je kurzor umístěn na **src** atribut první zdroj.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Stisknutím klávesy **kartu** klíče dvakrát vložení fragmentu kódu. Zvukový fragment kódu ukazuje standardní využití *zvuku* značky pomocí dvou zdrojových souborů pro lepší podporu.
3. Odstraňte druhý řádek a aktualizovat zdroj prvního řádku na následující odkaz k zobrazení WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Výsledný kód je uveden níže.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Zdrojový soubor *KatanaProject.mp3* slouží jako příklad. Pokud dáváte přednost, můžete použít jiný zdroj.
4. Stisknutím klávesy **CTRL** + **S** k uložení souboru.
5. Stisknutím klávesy **CTRL** + **F5** ke spuštění aplikace.
6. Všimněte si, že zvukový přehrávač přidal do aplikace.

    ![Zvukový přehrávač v aplikaci Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "zvukový přehrávač v aplikaci Internet Explorer")

    *Zvukový přehrávač v aplikaci Internet Explorer*

    ![Zvukový přehrávač v prohlížeči Google Chrome](visual-studio-2013-web-tools/_static/image38.png "zvukový přehrávač v prohlížeči Google Chrome")

    *Zvukový přehrávač v prohlížeči Google Chrome*
7. Nezavírejte prohlížečů. Je použijete v dalším úkolem.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Úloha 3 – pomocí technologie IntelliSense jazyka JavaScript dokumentů

Web Essentials 2013 vytvoření seznamu ID tak názvy tříd šablon stylů a stránky HTML. V této úloze se dozvíte, jak jsou tyto seznamy vylepšit podporu technologie IntelliSense jazyka JavaScript v sadě Web Essentials 2013.

1. V **Index.cshtml** přidejte následující kód, který definuje **skript** značky pro kód jazyka JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Přidejte následující kód **skript** značka definice funkce připravené zpětného volání.

    (Fragment - kódu *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Umístěte kurzor v **skript** značky a stiskněte klávesu **CTRL** + **.** Otevřete nabídku návrh.
4. Klikněte na tlačítko **extrahovat soubor**.

    ![Extrahovat jazyka JavaScript do souboru návrh](visual-studio-2013-web-tools/_static/image39.png "JavaScript extrahovat soubor návrh")

    *Extrahovat jazyka JavaScript do souboru návrh*
5. V **uložit jako** okna, vyberte **skripty** složku, název souboru **init.js** a klikněte na tlačítko **Uložit**.

    ![Uložit jako okno](visual-studio-2013-web-tools/_static/image40.png "okně Uložit jako")

    *Okno Uložit jako*

    > [!NOTE]
    > **Init.js** se vytvoří soubor a obsah souboru, který je přesunut do souboru.
    > 
    > ![Vytvořené pomocí obsahu zahrnutého souboru Init.js](visual-studio-2013-web-tools/_static/image41.png "Init.js soubor vytvořený pomocí zahrnutého obsahu")
    > 
    > *Init.js soubor vytvořený pomocí zahrnutého obsahu*
6. Otevřít **Index.cshtml** soubor a zkontrolujte, že byl nahrazen značku skriptu s odkazem na **init.js** souboru.

    ![Odkaz html Init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js odkaz html")

    *Odkaz html Init.js*
7. Přejděte **Průzkumníku řešení** a Všimněte si, že **init.js** soubor byl automaticky součástí řešení.

    ![Soubor Init.js zahrnutý v řešení](visual-studio-2013-web-tools/_static/image43.png "Init.js soubor zahrnutý v řešení")

    *Soubor Init.js zahrnutý v řešení*
8. Přepněte zpátky na **init.js** souboru se má aktualizovat **připravené** funkce zpětného volání.
9. Uvnitř definice funkce zpětného volání, která je předána *připravené*, přidejte následující kód se získat všechny elementy s konkrétní třídu atribut.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Stisknutím klávesy **CTRL** + **místo** mezi uvozovky uvnitř **getElementsByClassName** volání funkce.

    ![Zobrazení technologie IntelliSense pro funkci getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "zobrazující technologie IntelliSense pro getElementsByClassName – funkce")

    *Zobrazení technologie IntelliSense pro getElementsByClassName – funkce*

    > [!NOTE]
    > Všimněte si, že technologie IntelliSense zobrazuje tříd definovaných v šabloně stylů projektu.
11. Nahraďte řádek, který jste vytvořili s následujícím kódem.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Umístěte kurzor po **au** v uvozovkách v **getElementsByTagName** funkce a stiskněte klávesu **CTRL** + **místo**.

    ![Zobrazení technologie IntelliSense pro metodu getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "zobrazující technologie IntelliSense pro getElementByTagName – metoda")

    *Zobrazení technologie IntelliSense pro getElementsByTagName – metoda*
13. Vyberte **&quot;zvuku&quot;** ze seznamu a stisknutím klávesy **ENTER**. Výsledek je znázorněno na následujícím obrázku.

    ![Načítání zvuku prvků](visual-studio-2013-web-tools/_static/image46.png "načítání zvuku prvků")

    *Načítání zvuku prvků*
14. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **init.js** soubor **skripty** a pak zvolte položku **soubory JavaScript zrušíte Minifikaci** z **Web Essentials** nabídky.

    ![Soubory JavaScript zrušíte minifikaci](visual-studio-2013-web-tools/_static/image47.png "zrušíte Minifikaci Javascriptové soubory")

    *Zrušíte minifikaci soubory jazyka JavaScript*
15. Po zobrazení výzvy k povolení automatického připravenost k minifikaci při změně zdrojových souborů, klikněte na tlačítko **Ano**.

    ![Povolení automatického připravenost k minifikaci upozornění](visual-studio-2013-web-tools/_static/image48.png "povolení automatické připravenost k minifikaci upozornění")

    *Povolení automatického připravenost k minifikaci upozornění*

    > [!NOTE]
    > **Init.min.js** je vytvořen a přidán jako závislost **init.js** souboru.
    > 
    > ![Vytvořený soubor Init.min.js](visual-studio-2013-web-tools/_static/image49.png "Init.min.js soubor vytvořený")
    > 
    > *Vytvořený soubor Init.min.js*
16. Otevřít **init.min.js** soubor a Všimněte si, že soubor je minifikovaný.

    ![Obsah souboru Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js obsah souboru")

    *Obsah souboru Init.min.js*
17. V **init.js** přidejte následující kód uvedený níže **getElementsByTagName** volání přehrávání zvuku elementy funkce.

    (Fragment - kódu *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Klikněte na tlačítko **CTRL** + **S** k uložení souboru. Protože minifikovaný soubor je už otevřený, zobrazí se dialogové okno s informacemi o tom, že soubor byl změněn mimo editoru zdrojového kódu. Klikněte na tlačítko **Ano**.

    ![Microsoft Visual Studio upozornění](visual-studio-2013-web-tools/_static/image51.png "upozornění Microsoft Visual Studio")

    *Microsoft Visual Studio upozornění*
19. Přepněte zpět **init.min.js** souboru můžete ověřit, že soubor byl aktualizován s novým kódem.

    ![Aktualizovat soubor Init.min.js](visual-studio-2013-web-tools/_static/image52.png "aktualizovat soubor Init.min.js")

    *Aktualizovat soubor Init.min.js*
20. Klikněte na tlačítko **aktualizovat odkaz prohlížeče** tlačítko.
21. Jakmile se aktualizují i prohlížeče hudebních přehrávačů, které jste viděli v předchozí úloze začne přehrávat automaticky.

    ![Zvukový přehrávač zahrnuté v zobrazení](visual-studio-2013-web-tools/_static/image53.png "zvukový přehrávač zahrnuté v zobrazení")

    *Zvukový přehrávač zahrnuté v zobrazení*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Po dokončení tohoto praktických cvičení jste se naučili, jak:

- Použít nové funkce editoru HTML součástí Web Essentials, například bohaté fragmenty kódu HTML5 a Zen kódování
- Použít nové funkce editoru šablon stylů CSS součástí Web Essentials, jako je například výběr barvy a popisu matice prohlížeče
- Použít nové funkce editoru jazyka JavaScript součástí Web Essentials, jako je například extrahování do souboru a technologii IntelliSense pro všechny prvky jazyka HTML
- Výměna dat mezi prohlížeče a použití funkce Browser Link sady Visual Studio
