---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Rukou na testovacím: 2013 webové nástroje sady Visual Studio | Microsoft Docs'
author: rick-anderson
description: Visual Studio je vynikající vývoj prostředí pro. Na základě NET Windows a webové projekty. Zahrnuje výkonné textový editor, který lze snadno použít k...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Rukou na testovacího prostředí: Visual Studio 2013 nástroje pro Web
====================
Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](http://aka.ms/webcamps-training-kit)

> Visual Studio je vynikající vývoj prostředí pro. Na základě NET Windows a webové projekty. Zahrnuje výkonné textový editor, který lze snadno upravit samostatné soubory bez projektu.
> 
> Visual Studio udržuje strom analýzy plně vybavená jako upravit každý soubor. To umožňuje poskytovat bezkonkurenční automatické dokončování a na základě dokumentu akce při vytváření vývojového prostředí mnohem rychlejší i zpříjemnit Visual Studio. Tyto funkce jsou zvláště efektivní v dokumentech HTML a CSS.
> 
> Všechna tato napájení je k dispozici pro rozšíření, což jednoduše rozšiřitelný editory nové funkce, aby vyhovovaly potřebám vaší. Web Essentials je kolekce (většinou) týkající se webu vylepšení pro Visual Studio. Obsahuje velké množství nové dokončování IntelliSense (hlavně u šablon stylů CSS), nové funkce Browser Link, automatické soubory JSHint pro jazyk JavaScript, nové upozornění pro HTML a CSS a řadu dalších funkcí, které jsou pro vývoj moderních webových aplikací.
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Používat nové funkce editor HTML součástí Web Essentials, například bohaté fragmenty kódu jazyka HTML5 a Zen kódování
- Používat nové funkce editor šablon stylů CSS součástí Web Essentials, například výběr barvy a prohlížeče matice popisku
- Používat nové funkce JavaScript editor součástí Web Essentials, jako je extrakce souboru a IntelliSense pro všechny elementy HTML
- Výměna dat mezi prohlížečem a sadě Visual Studio pomocí Browser Link

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Pro dokončení této praktické cvičení je vyžadován následující text:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Aby bylo možné spustit praktická cvičení v tomto testovacím prostředí praktické, musíte nejdřív nastavit svoje prostředí.

1. Otevřete okno Průzkumníka Windows a přejděte do tohoto prostředí **zdroj** složky.
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

1. [Práce s Browser Link a webové Essentials](#Exercise1)
2. [Využití výhod fragmenty kódu a IntelliSense](#Exercise2)

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržené tak, aby odpovídala konkrétním vývoj styl a určuje rozložení oken, editor chování, IntelliSense – fragmenty kódu a možnosti dialogového okna. Postupy v tomto testovacím prostředí popisovat akce, které jsou nutné k dokončení daného úkolu v sadě Visual Studio, při použití **obecné nastavení vývoj** kolekce. Pokud si zvolíte jiné nastavení kolekce pro vývojové prostředí, může být rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Cvičení 1: Práce s Browser Link a webové Essentials

**Web Essentials** je rozšíření sady Visual Studio, který přidává celou řadu užitečných funkcí pro vývoj moderních webových, většinou zaměřuje na provedení vývojového prostředí webové mnohem rychlejší i zpříjemnit. Web Essentials můžete nainstalovat z Galerie rozšíření v sadě Visual Studio.

**Browser Link** je nová funkce zahrnuté ve Visual Studiu 2013, který poskytuje kanál mezi Visual Studio IDE a libovolného prohlížeče otevřete pro výměnu dat mezi vaší webové aplikace a Visual Studio. Web Essentials rozšiřuje Browser Link pomocí nástrojů k manipulaci s objektový model DOM a stylů CSS webových stránek přímo z prohlížeče.

V tomto cvičení bude prozkoumávat některé z funkcí podporovaných **Web Essentials** a **Browser Link** k vylepšení jednoduché kvízu stránky.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Úloha 1 – spuštění projektu v více prohlížečů

V této úloze nakonfigurujete webové aplikace na spouštění v několika prohlížeče najednou, což je užitečné pro testování různých prohlížečích.

1. Open **Microsoft Visual Studio**.
2. V **soubor** nabídce vyberte možnost **otevřete | Projekt nebo řešení...**  a přejděte do **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** v **zdroj** složky laboratoře (C:\WebCampsTK\HOL\VSWebTooling\Source). Vyberte **Begin.sln** a klikněte na tlačítko **otevřete**.
3. Na panelu nástrojů Visual Studio rozbalte nabídku prohlížeče a vyberte **procházet s...** .

    ![Procházet s možností nabídky](visual-studio-2013-web-tools/_static/image1.png "procházet s... v nabídce prohlížeče")

    *Procházet s možností nabídky*
4. V **procházet s** dialogové okno, vyberte **Google Chrome** a **Internet Explorer** podržením **CTRL** klíče a klikněte na tlačítko  **Nastavit jako výchozí**.

    ![Procházet s dialogové okno](visual-studio-2013-web-tools/_static/image2.png "procházet s dialogové okno")

    *Výběr více nastavení výchozích prohlížečů*
5. Google Chrome a prohlížeče Internet Explorer by se měla zobrazit jako nastavení výchozích prohlížečů. Klikněte na tlačítko **zrušit** zavřete dialogové okno.

    ![Google Chrome a prohlížeče Internet Explorer jako výchozího prohlížeče](visual-studio-2013-web-tools/_static/image3.png "Google Chrome a prohlížeče Internet Explorer nastavení výchozích prohlížečů")

    *Google Chrome a prohlížeče Internet Explorer jako výchozího prohlížeče*

    > [!NOTE]
    > Po konfiguraci nastavení výchozích prohlížečů **více prohlížečů** je vybraná možnost v nabídce prohlížeče.
    > 
    > ![Více prohlížečů](visual-studio-2013-web-tools/_static/image4.png "více prohlížečů")
6. Stiskněte klávesu **CTRL** + **F5** ke spuštění aplikace bez ladění.
7. Při otevření obě okna prohlížeče, umístěte jeden z nich nad sebou chcete-li zobrazit aktualizace v obou prohlížečích současně. Prohlížečů měli zobrazení webové stránky s obdélníku světle modrou.

    ![Umístění jeden prohlížeč nad sebou](visual-studio-2013-web-tools/_static/image5.png "umístění jeden prohlížeč nad sebou")

    *Umístění jeden prohlížeč nad sebou*
8. Nezavírejte okno prohlížečů. Budete je používat v dalším úkolem.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Úloha 2 – Zen pomocí kódování k vytváření prvků HTML

**Kódování Zen** je editor kódování modul plug-in pro vysokorychlostní HTML, XML, XSL (nebo jiného formátu strukturovaný kód) a úpravy. Základní tento modul plug-in je výkonný zkratka modul, který umožňuje rozšířit výrazy - podobná Selektory šablon stylů CSS – do kódu HTML. Kódování Zen je rychlý způsob, jak zapsat syntaxe selektor formátování HTML pomocí šablony stylů CSS.

V tomto cvičení použije k vygenerování tlačítka HTML, která představují možnosti otázka Zen kódování funkce poskytované Web Essentials.

1. Přepněte zpátky na Visual Studio.
2. Otevřete **Index.cshtml** soubor umístěný ve **zobrazení** | **Domů** složky.
3. Nahraďte **&lt;!--TODO: Přidat zde – možnosti&gt;** komentář s následující kód a stiskněte klávesu **KARTĚ**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Kód by měl rozšířit, aby HTML.

    ![Rozšířit HTML](visual-studio-2013-web-tools/_static/image6.png "rozšířit HTML")

    *Rozšířené HTML*

    > [!NOTE]
    > Další informace o kódování Zen syntaxi najdete v tématu následující [článku](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Klikněte na tlačítko **aktualizovat propojené prohlížeče** tlačítko Aktualizovat i prohlížeče.

    ![Aktualizujte propojené prohlížeče](visual-studio-2013-web-tools/_static/image7.png "aktualizovat propojené prohlížečů")

    *Aktualizujte propojené prohlížečů*

    ![Internet Explorer - stránka aktualizována čtyři tlačítka](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - stránka aktualizována čtyři tlačítka")

    *Internet Explorer - stránka aktualizována čtyři tlačítka*

    ![Google Chrome – stránka aktualizována čtyři tlačítka](visual-studio-2013-web-tools/_static/image9.png "Google Chrome – stránka aktualizována čtyři tlačítka")

    *Google Chrome – stránka aktualizována čtyři tlačítka*
6. Přepněte zpátky na Visual Studio.
7. Jste přidali tlačítka na stránku, ale stále budete muset přidat šablonu otázku. Uděláte to tak, budete používat nové funkce ve webové Essentials názvem **Lorem Ipsum generátor**. Vyhledejte **div** element s **třída** atribut **přední**.
8. Přidejte následující kód jako první podřízený prvek **div**a stiskněte klávesu **KARTĚ**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Kód by měl rozšířit, aby HTML.

    ![Automaticky generovaný Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "automaticky generovaný Lorem Ipsum")

    *Automaticky generovaný Lorem Ipsum*

    > [!NOTE]
    > Jako součást Zen kódování můžete nyní vygenerovat kód Lorem Ipsum přímo v editoru HTML. Jednoduše zadejte **lorem** a počtu **karta** a wordový Lorem Ipsum text se vloží 30. Například *lorem10* vloží 10 Lorem Ipsum slova.
10. Přidáte logo, které se v horní části na otázku pomocí další novou funkcí v Web Essentials názvem **Lorem pixelů generátor**. Přidejte následující kód jako první podřízený prvek **div** element s **kontejneru** jako **třída** hodnotu a stiskněte klávesu **KARTĚ**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Kód by měl rozšířit do HTML.

    ![Automaticky generovaný lorem pixelů](visual-studio-2013-web-tools/_static/image11.png "automaticky generovaný Lorem pixelů")

    *Automaticky generovaný lorem pixelů*

    > [!NOTE]
    > Jako součást Zen kódování můžete také vygenerovat kód Lorem pixelů přímo v editoru HTML. Jednoduše zadejte **obrázky. 200 x 200 zvířat** a počtu **KARTĚ** a **img** značky se 200 x 200 bitovou kopií zvíře bude vložen. Další informace najdete v části [Lorem pixelů](http://www.lorempixel.com).
12. Klikněte na tlačítko **aktualizovat propojené prohlížeče** tlačítko Aktualizovat i prohlížeče.

    ![Internet Explorer - automaticky vygenerovanou image a text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - automaticky vygenerovanou image a text.")

    *Internet Explorer - automaticky vygenerovanou image a text.*

    ![Google Chrome – generován automaticky image a text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome – generován automaticky image a text.")

    *Google Chrome – generován automaticky image a text.*

    > [!NOTE]
    > Vzhledem k tomu, že při přidání fragmentu kódu, vybraná náhodně bitovou kopii, bitovou kopii vidět v prohlížečích se můžou lišit.
13. Nezavírejte okno prohlížečů. Budete je používat v dalším úkolem.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Úloha 3 - aktualizace vlastnost stylu

V této úloze budete používat Browser Link **zkontrolovat režimu** funkci tak, aby zjistit přesné umístění, kde se vygeneruje konkrétní elementu DOM a aktualizujte vlastnost barvu tohoto elementu s použitím výběr barvy, a poskytuje webové Essentials.

1. V prohlížeči Internet Explorer, stiskněte klávesu **CTRL** + **ALT** + **I** aby kontrolovat režimu.
2. Přesuňte ukazatel myši světla modré ohraničení a klikněte na tlačítko.

    ![Ukazatele přes světla modré ohraničení](visual-studio-2013-web-tools/_static/image14.png "ukazatele přes světla modré ohraničení")

    *Ukazatele přes světla modré ohraničení*
3. Přepněte zpátky na Visual Studio. Všimněte si, jak prvek HTML, který jste vybrali v prohlížeči, vybere se také v editoru Visual Studio HTML.

    ![Element HTML v editoru Visual Studio HTML vybrána](visual-studio-2013-web-tools/_static/image15.png "prvek HTML vybraného v editoru Visual Studio HTML")

    *Element HTML vybraného v editoru Visual Studio HTML*
4. Bude nyní aktualizovat **přední** třídy CSS, aby bylo možné změnit stylů vybraného prvku. Chcete-li to provést, stiskněte **CTRL** + **,** otevřete **přejít na** vyhledávacího pole. Typ **site.css** a stiskněte klávesu **ENTER** k otevření souboru.

    ![Otevření souboru Site.css](visual-studio-2013-web-tools/_static/image16.png "při otevírání souboru Site.css")

    *Při otevírání souboru Site.css*
5. Stiskněte klávesu **CTRL** + **F** a typ **.flip kontejneru .front** k vyhledání modulu pro výběr šablon stylů CSS.
6. Klikněte na tlačítko světla blue hranaté ve vlastnosti ohraničení třídy otevřete volby barev.

    ![Otevření dialogového okna Výběr barvy](visual-studio-2013-web-tools/_static/image17.png "otevírání volby barev")

    *Otevření dialogového okna Výběr barvy*
7. Kliknutím tlačítko s dvojitou šipkou rozbalte výběr barvy a vyberte novou barvu.

    ![Rozšiřování volby barev](visual-studio-2013-web-tools/_static/image18.png "rozšiřování volby barev")

    *Rozšiřování volby barev*
8. Stiskněte klávesu **CTRL** + **ALT** + **ENTER** aktualizovat propojené prohlížeče.
9. Přepněte do Internet Exploreru a Všimněte si, jak má změnit barvu ohraničení.

    ![Internet Explorer - aktualizovat barvu ohraničení](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - aktualizovat barvu ohraničení")

    *Internet Explorer - aktualizovat barvu ohraničení*
10. Přepněte do Google Chrome a Všimněte si, jak má změnit barvu ohraničení.

    ![Google Chrome – aktualizovat barvu ohraničení](visual-studio-2013-web-tools/_static/image20.png "Google Chrome – aktualizovat barvu ohraničení")

    *Google Chrome – aktualizovat barvu ohraničení*
11. Přepněte zpátky na Visual Studio.
12. Přejděte na konec **Site.css** souboru a stiskněte klávesu **CTRL** + **F** najít **.btn** selektor.
13. Všimněte si, že **- webkit-border-radius** vlastnost je podtržený zeleně.

    ![Vlastnost - webkit-border-radius selektoru btn](visual-studio-2013-web-tools/_static/image21.png "vlastnost - webkit-border-radius selektoru btn")

    *Vlastnost - webkit-border-radius selektoru btn*
14. Umístěte pomocí kurzoru v **- webkit-border-radius** vlastnost. Modré řádku by se zobrazit v části první písmeno prvního slova vlastnosti. Toto je **inteligentní značky**.
15. Stiskněte klávesu **CTRL** + **.** Otevřete nabídku návrhy a klikněte na tlačítko **přidat chybí vlastnost standardní (border-radius)**.

    ![Přidat chybí vlastnost standardní návrhu](visual-studio-2013-web-tools/_static/image22.png "přidat chybí vlastnost standardní návrhu")

    *Přidejte chybějící standardní vlastnost návrhu*
16. **Border-radius** vlastnost je automaticky přidán do pravidla šablon stylů CSS.

    ![Chybí standardní vlastnost přidat](visual-studio-2013-web-tools/_static/image23.png "chybí standardní vlastnost přidána")

    *Chybí standardní vlastnost přidána*
17. Přesuňte ukazatel myši **border-radius** zobrazovanou vlastnost **prohlížeče matice popisek**. **Prohlížeče matice popisek** uvádí dostupnost vlastnosti v každé prohlížeče.

    ![Popisek matice prohlížeče](visual-studio-2013-web-tools/_static/image24.png "popisek matice prohlížeče")

    *Popisek matice prohlížeče*
18. Všimněte si, že hodnota **border-radius** vlastnost je stále podtržené. Přesuňte ukazatel myši hodnota zobrazíte upozornění.

    ![Upozornění hodnota border-radius vlastnost](visual-studio-2013-web-tools/_static/image25.png "Border-radius vlastnost hodnota upozornění")

    *Upozornění hodnotu vlastnosti border-radius*
19. Odeberte na jednotku **border-radius** hodnotu vlastnosti na návrh popisek.
20. Jako **border-radius** je standardní vlastnost pro definování způsobu zaokrouhlené ohraničení rozích jsou, můžete odebrat **- webkit-border-radius** vlastností a hodnotou z pravidla šablon stylů CSS.
21. Umístěte pomocí kurzoru v **word-wrap** vlastnost a Všimněte si, že inteligentní značky se zobrazí také níže.
22. Otevřete nabídku a klikněte na tlačítko **přidejte chybějící specifika dodavatele**.

    ![Přidejte chybějící dodavatele specifika návrhu](visual-studio-2013-web-tools/_static/image26.png "přidejte chybějící dodavatele specifika návrhu")

    *Přidejte chybějící dodavatele specifika návrhu*
23. **-Ms-zalamování** vlastnost je automaticky přidán do pravidla šablon stylů CSS.

    ![Určité vlastnosti dodavatele přidat](visual-studio-2013-web-tools/_static/image27.png "dodavatele určitou vlastnost přidána")

    *Přidat určitou vlastnost dodavatele*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Úloha 4 – aktualizace kód HTML z prohlížeče

V této úloze budete používat Browser Link **režimu návrhu** funkce Upravit objekt DOM z prohlížeče a přenést změny ke zdrojovému souboru HTML v sadě Visual Studio.

1. V Google Chrome, stiskněte klávesu **CTRL** + **ALT** + **D** povolení režimu návrhu.
2. Přesuňte ukazatel myši **Lorem Ipsum dolor lokality amet** popisku a klikněte na tlačítko.

    ![Úpravy na otázku](visual-studio-2013-web-tools/_static/image28.png "úpravy na otázku")

    *Úpravy na otázku*
3. Kurzor by se zobrazit. Nahradí původní text s *jak ho vypadá při psaní delší otázku?*a potom stiskněte klávesu **ESC** ukončíte režimu návrhu.

    ![Otázka upravit](visual-studio-2013-web-tools/_static/image29.png "otázku upravit")

    *Upravit dotaz*
4. Přepněte zpět do Visual Studio a otevřete **Index.cshtml**, pokud ještě není otevřené. Všimněte si, že vnitřní text **&lt;p&gt;** element se aktualizovala.

    ![Aktualizované otázku na stránce HTML](visual-studio-2013-web-tools/_static/image30.png "aktualizované otázku na stránce HTML")

    *Aktualizované otázku na stránce HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Úloha 5: prostudovali SEO související upozornění

**Hledání modul optimalizace** (vyhledávací weby SEO) je proces převedení vyšší pořadí webu na vyhledávací web seznam výsledků. Tím vyšší určuje pořadí webu a více konzistentně je uvedena, další návštěvníkům webu získají z této vyhledávací web. Web Essentials zahrnuje analytické nástroje, která prověřuje HTML, sestavy problémy najít a nabízí pomoc při jejich odstranění.

1. Přejděte na **zobrazení** nabídky a klikněte na tlačítko **seznam chyb** otevřete **seznam chyb** okno.

    ![Chyba v zobrazení seznamu nabídky](visual-studio-2013-web-tools/_static/image31.png "seznam chyb v nabídce zobrazení")

    *Chyba v zobrazení seznamu nabídky*
2. Všimněte si, že je SEO upozornění s informací, které **&lt;meta&gt;** značky pro popis stránky chybí. Dvakrát klikněte na položku upozornění optimalizace pro vyhledávací weby a opravte ji.

    ![Okno Seznam chyb](visual-studio-2013-web-tools/_static/image32.png "v okně Seznam chyb")

    *Okno Seznam chyb*
3. V **Web Essentials** dialogové okno, klikněte na tlačítko **Ano** Vložit popis &lt;meta&gt; značky.

    ![Dialogové okno Essentials web](visual-studio-2013-web-tools/_static/image33.png "dialogové okno Web Essentials")

    *Dialogové okno Essentials Web*
4. Editor pro  **\_Layout.cshtml** otevře a **&lt;meta&gt;** značky je automaticky přidán do **head** části Soubor HTML.

    ![Značka META automaticky přidá stránce _Layout](visual-studio-2013-web-tools/_static/image34.png "metaznačku automaticky přidány _Layout stránku")

    *Značka META automaticky přidá do \_rozložení stránky*
5. Změňte hodnotu **obsah** atribut *GeekQuiz* a soubor uložte.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Cvičení 2: Využívat výhod fragmenty kódu a IntelliSense

S Web Essentials bylo rozšířeno HTML editor s další funkce. V tomto cvičení zobrazí se některé nové funkce, které jsou užitečné při vývoji webové aplikace.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Úloha 1 – pomocí IntelliSense v dokumentech HTML

První nové funkce, zobrazí se v této úloze se nazývá **dynamické IntelliSense**. Dynamické IntelliSense přečte další značky a atributy, které mají odvození možné ID, které chcete použít.

V této úloze vytvoříte nový element formuláře HTML obsahující štítky a vstupního pole. Pak přidáte **pro** atribut štítek, který chcete navázat jej na vstup a zobrazí se vám IntelliSense návrhy podle ID vstupních hodnot v oboru.

1. Otevřete **Visual Studio Express 2013 pro Web** a **Begin.sln** řešení umístěný v **zdroj/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** složky. Alternativně můžete pokračovat v řešení jste získali v předchozím cvičení.
2. V **Průzkumníku řešení**, otevřete **Index.cshtml** soubor umístěný ve **zobrazení** | **Domů** složky.
3. Přidejte následující formulář uvnitř **&lt;části&gt;** element.

    (Code fragment kódu - *VisualStudio2013WebTooling* - *Ex2* - *formuláře*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Vstupní značky musí předcházet popisek nějaké popis pole. Přidejte následující popisku před vstupní značka.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. **Pro** atribut **&lt;popisek&gt;** Určuje, který element formuláře a popisku je vázán k. Hodnota atributu musí být roven id související elementu. Přidat **pro** atribut **&lt;popisek&gt;** element. Jak je znázorněno na následujícím obrázku &quot;název&quot; hodnotu objeví v dialogovém okně IntelliSense na základě id elementů v rámci stejného oboru (uzavření  **&lt;formuláře&gt;**).

    ![Zobrazení v IntelliSense id](visual-studio-2013-web-tools/_static/image35.png "id zobrazení v IntelliSense")

    *Id zobrazení v IntelliSense*
6. Odstranit nedávno přidané **&lt;formuláře&gt;** elementu a jeho obsah.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Úloha 2 – pomocí fragmenty kódu HTML

HTML5 zavedl víc než 25 nové sémantického značky. Visual Studio již obsahuje podporu technologie IntelliSense pro tyto značky, ale Visual Studio 2013 je rychlejší a snazší psát kód přidáním nové fragmenty kódu. I když tyto značky nejsou složitá, jsou součástí pár malých odlišnosti, jako je například přidávání případech přejít správný kodek pro *zvuk* značky. V této úloze uvidíte fragmenty kódu HTML pro zvuk značku.

1. V **Index.cshtml** souboru, zadejte  **&lt;oblast** uvnitř **&lt;části&gt;** element, jak je znázorněno na následujícím obrázku.

    ![Vkládání audio element](visual-studio-2013-web-tools/_static/image36.png "vkládání audio element")

    *Vkládání audio element*
2. Stiskněte klávesu **KARTĚ** dvakrát a Všimněte si, jak se přidá následující kód na stránce a je umístěn kurzor na **src** atribut první zdroje.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Stisknutím kombinace kláves **KARTĚ** klíče dvakrát fragmentu kódu je vložen. Zvuk fragment kódu ukazuje standardní využití *zvuk* značky s dvěma zdrojové soubory pro lepší podporu.
3. Odstraňte na druhém řádku a aktualizace zdroje prvního řádku s použitím na následující odkaz k zobrazení WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Výsledný kód je uveden níže.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Zdrojový soubor *KatanaProject.mp3* slouží jako příklad. Pokud dáváte přednost, můžete použít jiný zdroj.
4. Stiskněte klávesu **CTRL** + **S** k uložení souboru.
5. Stiskněte klávesu **CTRL** + **F5** a spusťte aplikaci.
6. Všimněte si, že audio player byl přidán do aplikace.

    ![Přehrávač v aplikaci Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "zvuk player v aplikaci Internet Explorer")

    *Přehrávač v Internet Exploreru*

    ![Přehrávač v Google Chrome](visual-studio-2013-web-tools/_static/image38.png "player zvukovém souboru v Google Chrome")

    *Přehrávač v Google Chrome*
7. Nezavírejte okno prohlížečů. Budete je používat v dalším úkolem.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Úloha 3 – pomocí IntelliSense v dokumentech jazyka JavaScript

S Web Essentials 2013 šablony stylů a stránky HTML vytvořit seznam ID a třídy názvů. V této úloze se dozvíte, jak tyto seznamy vylepšit podporu JavaScript IntelliSense v webové Essentials 2013.

1. V **Index.cshtml** soubor, přidejte následující kód k definování **skriptu** značky pro kód jazyka JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Přidejte následující kód do **skriptu** značky definice funkce připravené zpětného volání.

    (Code fragment kódu - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Umístěte pomocí kurzoru v **skriptu** značky a stiskněte klávesu **CTRL** + **.** Otevřete nabídku návrhu.
4. Klikněte na tlačítko **extrahovat do souboru**.

    ![JavaScript extrakci souborů návrhu](visual-studio-2013-web-tools/_static/image39.png "JavaScript extrakci souborů návrhu")

    *JavaScript extrakci souborů návrhu*
5. V **uložit jako** vyberte **skripty** složku, název souboru **init.js** a klikněte na tlačítko **Uložit**.

    ![Okno Uložit jako](visual-studio-2013-web-tools/_static/image40.png "okno Uložit jako")

    *Okno Uložit jako*

    > [!NOTE]
    > **Init.js** soubor je vytvořen a obsah skriptu je přesunut do souboru.
    > 
    > ![Init.js souboru vytvořeného v obsah zahrnutý](visual-studio-2013-web-tools/_static/image41.png "Init.js souboru vytvořeného v obsah zahrnutý")
    > 
    > *Init.js souboru vytvořeného v obsah zahrnutý*
6. Otevřete **Index.cshtml** soubor a zkontrolujte, zda byl nahrazen značky script s odkazem na **init.js** souboru.

    ![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")

    *Init.js html reference*
7. Přejděte na **Průzkumníku řešení** a Všimněte si, že **init.js** souboru je automaticky zahrnutý v řešení.

    ![Soubor Init.js součástí řešení](visual-studio-2013-web-tools/_static/image43.png "Init.js soubor zahrnutý v řešení")

    *Soubor Init.js součástí řešení*
8. Přepněte zpět na **init.js** soubor aktualizovat **připraven** funkce zpětného volání.
9. V definici funkce zpětného volání, který je předán do *připraven*, přidejte následující kód slouží k získání všechny elementy atributem určité třídy.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Stiskněte klávesu **CTRL** + **místo** mezi uvozovky uvnitř **getElementsByClassName** volání funkce.

    ![Zobrazení technologie IntelliSense pro funkci getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "zobrazující IntelliSense pro funkci getElementsByClassName")

    *Zobrazení technologie IntelliSense pro funkci getElementsByClassName*

    > [!NOTE]
    > Všimněte si, že IntelliSense ukazuje třídy definované v projektu šablony stylů.
11. Nahraďte řádek, který jste vytvořili následujícím kódem.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Umístěte kurzor po **au** uvnitř uvozovek v **getElementsByTagName** funkce a stiskněte klávesu **CTRL** + **místo**.

    ![Zobrazení technologie IntelliSense pro metodu getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "zobrazující IntelliSense pro metodu getElementByTagName")

    *Zobrazení technologie IntelliSense pro metodu getElementsByTagName*
13. Vyberte **&quot;zvuk&quot;** ze seznamu a stiskněte klávesu **ENTER**. Výsledkem je znázorněno na následujícím obrázku.

    ![Načítání zvuku elementy](visual-studio-2013-web-tools/_static/image46.png "načítání zvuku elementy")

    *Načítání zvuku elementy*
14. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **init.js** souboru v **skripty** složky a vyberte **Minifikaci JavaScript soubory** z **Web Essentials** nabídky.

    ![Minifikaci soubory JavaScript](visual-studio-2013-web-tools/_static/image47.png "soubory Minifikaci JavaScript")

    *Minifikaci soubory jazyka JavaScript*
15. Po zobrazení výzvy k povolení automatického minimalizace při změní zdrojový soubor, klikněte na tlačítko **Ano**.

    ![Povolení automatického minimalizace upozornění](visual-studio-2013-web-tools/_static/image48.png "povolení automatického minimalizace upozornění")

    *Povolení automatického minimalizace upozornění*

    > [!NOTE]
    > **Init.min.js** je vytvořen a se přidal jako závislost **init.js** souboru.
    > 
    > ![Soubor Init.min.js vytvořený](visual-studio-2013-web-tools/_static/image49.png "Init.min.js soubor vytvořený")
    > 
    > *Soubor Init.min.js vytvořený*
16. Otevřete **init.min.js** souboru a Všimněte si, že soubor je minifikovaný.

    ![Obsah souboru Init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js obsah souboru")

    *Obsah souboru Init.min.js*
17. V **init.js** soubor, přidejte následující kód níže **getElementsByTagName** volání přehrávání zvuku elementy funkce.

    (Code fragment kódu - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Klikněte na tlačítko **CTRL** + **S** k uložení souboru. Vzhledem k tomu, že minifikovaný soubor je již otevřen, zobrazí se dialogové okno s informacemi o tom, že soubor byl změněn mimo editoru zdroje. Klikněte na tlačítko **Ano**.

    ![Microsoft Visual Studio upozornění](visual-studio-2013-web-tools/_static/image51.png "upozornění Microsoft Visual Studio")

    *Microsoft Visual Studio warning*
19. Přepnout zpět **init.min.js** souboru a ověřte, že soubor byl aktualizován s novým kódem.

    ![Aktualizovat soubor Init.min.js](visual-studio-2013-web-tools/_static/image52.png "Init.min.js soubor aktualizovat")

    *Aktualizovat soubor Init.min.js*
20. Klikněte **aktualizovat odkaz prohlížeče** tlačítko.
21. Po aktualizaci obou prohlížečů zvuk hráči, které jste viděli v předchozí úloze se spustí automaticky přehrávání.

    ![Přehrávač zahrnuté v zobrazení](visual-studio-2013-web-tools/_static/image53.png "player zvuk zahrnuté v zobrazení")

    *Přehrávač zahrnuté v zobrazení*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

Pomocí této praktické cvičení jste se naučili, jak:

- Používat nové funkce editor HTML součástí Web Essentials, například bohaté fragmenty kódu jazyka HTML5 a Zen kódování
- Používat nové funkce editor šablon stylů CSS součástí Web Essentials, například výběr barvy a prohlížeče matice popisku
- Používat nové funkce JavaScript editor součástí Web Essentials, jako je extrakce souboru a IntelliSense pro všechny elementy HTML
- Výměna dat mezi prohlížečem a sadě Visual Studio pomocí Browser Link
