---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Co je nového v architektuře ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 je rozhraní pro vytváření škálovatelných standardy webových aplikací pomocí vzory zavedené návrhu a výkonu technologie ASP.NET a...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 977a6b5a84825ebd087752dcc2ebc0c5410e1657
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Co je nového v architektuře ASP.NET MVC 4

Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 je rozhraní pro vytváření škálovatelných standardy webových aplikací pomocí vzory zavedené návrhu a výkonu technologie ASP.NET a rozhraní .NET framework. Tento nový, čtvrté verzi rozhraní framework se zaměřuje na snadněji vývoj mobilních webových aplikací.

Pokud vytvoříte nový projekt ASP.NET MVC 4 je nyní šablona projektu mobilních aplikací, které můžete použít k vytvoření samostatné aplikace speciálně pro mobilní zařízení. Kromě toho ASP.NET MVC 4 se integruje s jQuery Mobile prostřednictvím balíčku NuGet jQuery.Mobile.MVC. jQuery Mobile je základě HTML5 rozhraní pro vývoj webových aplikací, které jsou kompatibilní s všechny platformy oblíbených mobilních zařízení, včetně Windows Phone, iPhone, Android a tak dále. Ale pokud budete potřebovat specializace, ASP.NET MVC 4 také umožňuje obsluhovat různá zobrazení pro různá zařízení a poskytovat optimalizace pro konkrétní zařízení.

V tomto testovacím prostředí praktických se spustí s ASP.NET MVC 4 &quot;Internetové aplikace&quot; šablona projektu pro vytvoření aplikace Fotogalerie. Progresivně se zlepšila aplikace pomocí jQuery Mobile a nové funkce ASP.NET MVC 4, aby byl kompatibilní s různých mobilních zařízení a klientů webových prohlížečů. Naučíte se také o nový kód recepty pro generování kódu a jak ASP.NET MVC 4 usnadňuje psaní metody asynchronní akce díky podpoře úloh&lt;ActionResult&gt; návratové typy.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [verze Microsoft-webové/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [co je nového ve webových formulářů v technologii ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Využívat výhod rozšíření, která chcete šablony včetně projektu ASP.NET MVC šabloně nový projekt mobilních aplikací
- Pomocí zobrazení atributů jazyka HTML5 a dotazy na média šablon stylů CSS ke zlepšení zobrazení na mobilních zařízeních
- Pro progresivní vylepšení a vytváření dotykovým webového uživatelského rozhraní použijte jQuery Mobile
- Vytvoření mobilní konkrétní zobrazení
- Používání komponent přepínači zobrazení lze přepínat mezi mobilních a desktopových zobrazení v aplikaci
- Vytvořte asynchronní řadičů pomocí úloh podpory

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [příloha B](#AppendixB) pokyny o tom, jak ji nainstalovat).
- [ASP.NET MVC 4](../../../mvc4.md) (zahrnutá v instalaci sady Microsoft Visual Studio 2012)
- Emulátor Windows Phone (součástí [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Volitelné - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) s **Electric Plum iPhone simulátoru** rozšíření (pouze pro cvičení 3 umožňuje procházet webové aplikace s simulátor pro zařízení iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalace

V dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění vaší práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, který můžete použít z Visual Studia abyste nemuseli přidat ručně.

Chcete-li nainstalovat fragmenty kódu:

1. Otevřete okno Průzkumníka Windows a přejděte do tohoto prostředí **Source\Setup** složky.
2. Dvakrát klikněte **Setup.cmd** soubor v této složce pro instalaci fragmenty kódu sadě Visual Studio.

Pokud si nejste obeznámeni s fragmenty kódu Visual Studio a chcete se dozvíte, jak používat, můžete odkazovat na příloze z tohoto dokumentu &quot; [fragmenty kódu pomocí příloha A:](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické cvičení zahrnuje následující cvičení:

1. [Nové šablony projektu ASP.NET MVC 4](#Exercise1)
2. [Vytváření webové aplikace Fotogalerie](#Exercise2)
3. [Přidání podpory pro mobilní zařízení](#Exercise3)
4. [Použití asynchronní řadičů](#Exercise4)

> [!NOTE]
> Každý úkol je přiložena **End** složku obsahující výsledné řešení by si měly opatřit po dokončení cvičeních. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc při procházení cvičeních.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Cvičení 1: Nové šablony projektu ASP.NET MVC 4

V tomto cvičení zaměříte vylepšení v šablonách projektu ASP.NET MVC 4. Kromě šabloně Internetové aplikace již existuje v MVC 3, tato verze nyní obsahuje samostatné šablonu pro mobilní aplikace. Nejprve bude vypadat v některé důležité funkce každé ze šablony. Pak bude fungovat na vykreslování stránku správně na různých platformách pomocí správný přístup.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Úloha 1 – prohlížení Internetu šablona aplikací

1. Otevřete **sady Visual Studio**.
2. Vyberte **souboru | Nové | Projekt** příkazu nabídky. V **nový projekt** dialogovém okně, vyberte **Visual C# | Webové** šablony v levém podokně stromu a vyberte **webové aplikace ASP.NET MVC 4.** Název projektu **Fotogalerie**, vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK**.

    > [!NOTE]
    > Bude později upravit Fotogalerie ASP.NET MVC 4 řešení, které teď vytváříte.

    ![Vytvoření nového projektu](whats-new-in-aspnet-mvc-4/_static/image1.png "vytvoření nového projektu")

    *Vytvoření nového projektu*
3. V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **Internetové aplikace** šablona projektu a klikněte na tlačítko **OK**. Ujistěte se, že vyberete jako zobrazovací modul Razor.

    ![Vytvoření nové aplikace ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "vytvoření nové aplikace ASP.NET MVC 4 Internetu")

    *Vytvoření nové aplikace ASP.NET MVC 4 Internetu*

    > [!NOTE]
    > Syntaxe Razor byla zavedena v architektuře ASP.NET MVC 3. Jeho cílem je minimalizovat počet znaků a stisknutí kláves požadované v souboru, povolení fast a kapaliny kódování pracovního postupu. Syntaxe Razor využívá existující C# / VB (nebo jiných) znalosti jazyka a doručí syntaxi značek šablony, která umožňuje pracovním postupu vytváření Super HTML.
4. Stiskněte klávesu **F5** a spuštění řešení obnoveného šablony. Můžete zkontrolovat na následující funkce:

    **Styl moderních šablony**

    Byla obnovena šablony, poskytuje další styly moderních vyhledávání.

    ![Šablony MVC ASP.NET 4 přepracován tak](whats-new-in-aspnet-mvc-4/_static/image3.png "přepracován tak šablony MVC 4")

    *ASP.NET MVC 4 přepracován tak šablony*

    ![Nová stránka kontaktujte](whats-new-in-aspnet-mvc-4/_static/image4.png "nového kontaktu stránky")

    *Nová stránka kontakt*

    **Adaptivního vykreslování**

    Podívejte se na změnu velikosti okna prohlížeče a Všimněte si, jak rozložení stránky dynamicky přizpůsobení novou velikost okna. Tyto šablony pomocí technik adaptivního vykreslování stolní počítače a mobilní platformy bez nutnosti přizpůsobení řádně vykreslit.

    ![Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti](whats-new-in-aspnet-mvc-4/_static/image5.png "šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti")

    *Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti*

    **Širší uživatelského rozhraní v jazyce JavaScript**

    Další vylepšení výchozích šablon projektu je použití JavaScript zajistit více interaktivní JavaScript. Přihlášení a registrace odkazy, které jsou použité v šabloně exemplify použití jQuery ověření k ověření vstupních polí z klienta.

    ![jQuery ověření](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery ověření*

    > [!NOTE]
    > Všimněte si, že dva protokolu v části v první části se můžou přihlásit pomocí účtu zaregistrovaný z webu a v druhé části můžete altenativelly přihlásit pomocí jiné služby ověřování, jako je google (zakázané ve výchozím nastavení).
5. Zavřete prohlížeč zastavení ladicího programu a vrátíte se k sadě Visual Studio.
6. Otevřete soubor **AuthConfig.cs** umístěná **aplikace\_spustit** složky.
7. Odebrat z posledního řádku registrace klienta Google pro komentář *OAuth* ověřování.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

> [!NOTE]
> Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.
~~~
8. Stiskněte klávesu **F5** spustíte řešení v a přejít na stránku přihlášení.
9. Vyberte **Google** službu pro přihlášení.

    ![Vyberte protokol ve službě](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Vyberte protokol ve službě*
10. Přihlaste se pomocí účtu Google.
11. Povolit web (localhost) za účelem načtení informací z účtu Google.
12. Nakonec budete muset zaregistrovat v lokalitě za účelem přidružení účtu Google.

   ![Přidružení účtu Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Přidružení účtu Google*
13. Zavřete prohlížeč zastavení ladicího programu a vrátíte se k sadě Visual Studio.
14. Nyní prozkoumejte řešení podívejte se na některé další nové funkce, zavedená v šabloně projektu ASP.NET MVC 4.

   ![Šablona projektu ASP.NET MVC 4 Internet aplikace](whats-new-in-aspnet-mvc-4/_static/image9.png "šablona projektu ASP.NET MVC 4 Internetové aplikace")

   *Šablona projektu ASP.NET MVC 4 Internetové aplikace*

   - **HTML 5 značek**

       Procházejte šablony zobrazení a zjistěte, kód nový motiv.

       ![Nové šablony, pomocí syntaxe Razor a HTML5 značek About.cshtml. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Novou šablonu, pomocí syntaxe Razor a HTML5 značek About.cshtml.")

       *Nové šablony, pomocí syntaxe Razor a HTML5 značek (About.cshtml).*
   - **Aktualizované knihoven jazyka JavaScript**

       Výchozí šablony ASP.NET MVC 4 nyní zahrnuje kódem KnockoutJS, rozhraní MVVM JavaScript rozhraní, které umožňuje vytvářet a vysoce přizpůsobivém webových aplikací pomocí jazyka JavaScript a HTML. Jako v MVC3, jQuery a knihovny uživatelského rozhraní jQuery jsou také zahrnuté v architektuře ASP.NET MVC 4.

     > [!NOTE]
     > Můžete získat další informace o knihovně kódem KnockOutJS v tento odkaz: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Kromě toho se dozvíte jQuery a kalendáře jQuery UI v [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Úloha 2 – prohlížení šablona mobilních aplikací

ASP.NET MVC 4 usnadňuje vývoj webů pro mobilní a tablet prohlížeče. Tato šablona má stejnou strukturu aplikací jako šablonu internetovou aplikaci (Všimněte si, že kódu kontroleru je prakticky shodný), ale jeho styl byla změněna k vykreslení správně v mobilních zařízeních, na základě touch.

1. Vyberte **souboru | Nové | Projekt** příkazu nabídky. V **nový projekt** dialogovém okně, vyberte **Visual C# | Webové** šablony v levém podokně stromu a vyberte **webové aplikace ASP.NET MVC 4.** Název projektu **PhotoGallery.Mobile**, vyberte umístění (nebo ponechte výchozí nastavení), vyberte &quot;přidat do řešení&quot; a klikněte na tlačítko **OK**.
2. V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **mobilní aplikace** šablona projektu a klikněte na tlačítko **OK**. Ujistěte se, že vyberete jako zobrazovací modul Razor.

    ![Vytvoření nové aplikace ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "vytvoření nové aplikace ASP.NET MVC 4 Mobile")

    *Vytvoření nové aplikace ASP.NET MVC 4 Mobile*
3. Nyní budete moci zkoumat řešení a podívejte se na některé z nových funkcí zaváděné řešení šablony ASP.NET MVC 4 pro mobile:

    - **jQuery Mobile knihovny**

        Šablona projektu mobilní aplikace obsahuje mobilní knihovny jQuery, která je s otevřeným zdrojem knihovna pro kompatibilitu prohlížeč pro mobilní zařízení. jQuery Mobile platí postupném rozšiřování do mobilních prohlížečů, které podporují šablon stylů CSS a JavaScript. Progresivní vylepšení umožňuje všechny prohlížeče zobrazíte základní obsah na webové stránce, zatímco umožňuje nejúčinnějších prohlížeče zobrazíte bohaté obsah. Souborů JavaScript a CSS součástí jQuery mobilní styl, pomůže mobilní prohlížeče a přizpůsobit obsah na obrazovce bez provedení jakékoli změny v kódu stránky.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *mobilní knihovny jQuery zahrnuté v šabloně*
    - **HTML5 na základě značek**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Šablona mobilních aplikací pomocí značek HTML5, (Login.cshtml a index.cshtml)*
4. Stiskněte klávesu **F5** ke spuštění řešení.
5. Otevřete **Windows Phone 7 emulátoru**.
6. Na obrazovce start phone otevřete Internet Explorer. Podívejte se na adresu URL, kde plochy aplikaci spustit a přejděte na tuto adresu URL z telefonu (například `http://localhost:[PortNumber]/`).
7. Nyní budete moci zadat přihlašovací stránku nebo podívejte se na o stránce. Všimněte si, že styl webové stránky je založen na novou aplikaci Metro Mobile. Šablona projektu ASP.NET MVC 4 je správně zobrazen na mobilních zařízeních, ujistěte se, že všechny prvky stránky jsou viditelné a povolené. Všimněte si, že jsou odkazy na v hlavičce dost klikli nebo stisknuté.

    ![Projektu šablony stránky v mobilním zařízení](whats-new-in-aspnet-mvc-4/_static/image14.png "projektu šablony stránky v mobilních zařízení")

    *Projekt šablony stránky v mobilních zařízení*
8. Nová šablona používá také **zobrazení metaznačku**. Většina mobilních prohlížečů definovat šířku pro okno virtuální prohlížeče nebo &quot;zobrazení&quot;, která je větší než skutečná šířka mobilního zařízení. To umožňuje mobilní prohlížeče zobrazíte celou webovou stránku uvnitř virtuální zobrazení. **Zobrazení metaznačku** umožňuje vývojářům webů nastavení šířky, výšky a škále oblasti prohlížeče na mobilních zařízeních **.** Šablony ASP.NET MVC 4 pro mobilní aplikace nastaví zobrazení na šířku zařízení (&quot;šířka = šířkou zařízení&quot;) v šabloně rozložení (*Views\Shared\_Layout.cshtml*) tak, aby všechny stránky budou mít jejich zobrazení nastavena na šířku obrazovky zařízení. Všimněte si, že zobrazení metaznačku nedojde ke změně zobrazení výchozí prohlížeč.
9. Otevřete ** \_Layout.cshtml**, který je umístěn v **zobrazení | Sdílené** složky a komentář metaznačku zobrazení. Spuštění aplikace, není-li již otevřít a podívejte se na rozdíly.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")

*The site after commenting the viewport meta tag*
~~~
10. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** Zastavit ladění aplikace.
11. Zrušením komentáře u metaznačku zobrazení.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]
~~~

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Úloha 3 – pomocí adaptivního vykreslování

V této úloze se dozvíte další metodou pro vykreslení stránky správně na mobilních zařízeních a webových prohlížečů ve stejnou dobu bez nutnosti přizpůsobení. Už jste použili zobrazení metaznačku s podobným účelem. Nyní bude vyhovovat jinou efektivní metodu: *adaptivního vykreslování*.

Adaptivního vykreslování je technika, který používá **dotazy na média CSS3** Chcete-li přizpůsobit styl použitý na stránku. Dotazy na média zadejte podmínky uvnitř šablony stylů CSS styly za určité podmínky seskupení. Jenom v případě, že je podmínka pravdivá, styl se použije na deklarované objekty.

Flexibilita poskytované adaptivního vykreslování technika umožňuje všechny vlastní nastavení pro zobrazení webu na různých zařízeních. Můžete definovat libovolný počet styly tak, jak chcete na jedinou šablonu stylů bez nutnosti psaní kódu logiku zvolit styl. Proto je velmi úhledné způsob přizpůsobení stylů stránky, protože snižuje množství duplicitní kód a logiku pro vykreslování účely. Na druhé straně byste zvýšit spotřeba šířky pásma, jak může málo zvětšit velikost souborů šablon stylů CSS.

Pomocí adaptivního vykreslování techniku, bude web **zobrazí správně, bez ohledu na to, v prohlížeči.** Ale byste měli zvážit, není vážný, pokud načíst navíc šířky pásma.

> [!NOTE]
> Je základní formát media dotaz: @media \[oboru: všechny | kapesních | tisku | projekce | obrazovky\] ([vlastnost: hodnota] a... [vlastnost: hodnota])


Příklady dotazů média: &gt; <strong> @media všechny a (max-width: 1000px) a (min-width: 700px) {}:</strong> pro všechny rozlišení mezi 700px a 1000px.

> <strong>@media obrazovky a (min-width: 400 px) a (max-width: 700px) {...}:</strong> pouze pro obrazovky. Řešení musí být v rozsahu od 400 do 700px.
> 
> <strong>@media kapesních a (min-width: 20em), obrazovky a (min-width: 20em) {...}:</strong> kapesní zařízení (mobile a zařízení) a obrazovky. Minimální šířka musí být větší než 20em.
> 
> Další informace o tom najdete na [W3C lokality](http://www.w3.org/TR/css3-mediaqueries/).


Bude nyní prozkoumat, jak funguje adaptivního vykreslování, zlepšení čitelnosti ASP.NET MVC 4 výchozí šablony webové stránky.

1. Otevřete **PhotoGallery.sln** řešení, které jste vytvořili v úloze 1 a vyberte **Fotogalerie** projektu. Stiskněte klávesu **F5** ke spuštění řešení.
2. Změnit šířku prohlížeče, nastavení windows polovina nebo méně než čtvrtletí původní velikosti. Všimněte si, co se stane s položkami v hlavičce: některé prvky se nezobrazí v oblasti viditelné hlavičky.
3. Otevřete <strong>Site.css</strong> soubor v Průzkumníku řešení Visual Studio, umístěný v <strong>obsahu</strong> složce projektu. Stiskněte klávesu <strong>kombinaci kláves CTRL + F</strong> otevřete Visual Studio integrované hledání a zapisovat <strong> @media </strong> najít <strong>šablon stylů CSS media dotaz</strong>.

    Tímto způsobem lze použít média dotazu podmínka, která je definována v této šabloně: když je velikost okna prohlížeče pod **850 px**, použita pravidla stylu CSS jsou ty, které jsou definované v tomto bloku média.

    ![Vyhledání media dotaz](whats-new-in-aspnet-mvc-4/_static/image16.png "vyhledání media dotaz")

    *Vyhledání media dotaz*
4. Nahraďte hodnotu atributu maximální šířka nastavenou v 850 px s **10px**, aby bylo možné zakázat adaptivního vykreslování a stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny. Vraťte do prohlížeče a stiskněte klávesu **kombinaci kláves CTRL + F5** obnovíte stránku s provedené změny. Všimněte si rozdíly v obou stránek, při úpravě šířky okna.

    ![Na levé straně stránky aplikuje @media styl, v pravém styl je vynechán](whats-new-in-aspnet-mvc-4/_static/image17.png "na levé straně stránky použití @media styl, v pravém styl je vynechán.")

    <em>Na levé straně stránky aplikuje @media styl, v pravém styl je vynechán.</em>

    Nyní Pojďme podívejte se na co se stane, že na mobilních zařízeních:

    ![Na levé straně stránky aplikuje @media styl, v pravém styl je vynechán](whats-new-in-aspnet-mvc-4/_static/image18.png "na levé straně stránky použití @media styl, v pravém styl je vynechán.")

    <em>Na levé straně stránky aplikuje @media styl, v pravém styl je vynechán.</em>

    I když si všimnete, že změny při vykreslení stránky ve webovém prohlížeči nejsou velmi důležité, pokud používáte mobilní zařízení stane zřejmější rozdíly. Na levé straně bitové kopie můžete vidíte, že vlastní styl lepší čitelnost.

    Adaptivního vykreslování lze použít v mnoha scénářích další, což usnadňuje použít podmíněného styly na web a řešení běžných problémů styl s úhledné přístup.

    Zobrazení metaznačku a dotazy na média šablon stylů CSS nejsou specifické pro ASP.NET MVC 4, takže můžete využít výhod těchto funkcí v jakékoli webové aplikace.
5. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** Zastavit ladění aplikace.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Cvičení 2: Vytvoření webové aplikace Fotogalerie

V tomto cvičení budete pracovat na aplikaci Fotogalerie zobrazíte fotografie. Bude spuštěn v šabloně projektu ASP.NET MVC 4, a pak přidáte funkci načtení fotografií ze služby a je zobrazit na domovské stránce.

V následujícím cvičení aktualizujte toto řešení pro zlepšení způsob, jakým se zobrazí na mobilních zařízeních.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Úloha 1 – vytvoření služby Imitované fotografie

V této úloze se vytvoří model služby fotografii k získání obsahu, který se zobrazí v galerii. K tomuto účelu přidáte nový řadič, jednoduše vrátí soubor JSON s daty každé fotografie.

1. Otevřete **Visual Studio** Pokud ještě není otevřené.
2. Vyberte **souboru | Nové | Projekt** příkazu nabídky. V **nový projekt** dialogovém okně, vyberte **Visual C# | Webové** šablony v levém podokně stromu a vyberte **webové aplikace ASP.NET MVC 4.** Název projektu **Fotogalerie**, vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK**. Alternativně můžete pokračovat v práci z existující ASP.NET MVC 4 **Internetové aplikace** řešení z **cvičení 1** a přejděte na další krok.
3. V **nový ASP.NET MVC 4 projekt** dialogové okno, vyberte **Internetové aplikace** šablona projektu a klikněte na tlačítko **OK**. Ujistěte se, že jste vybrali jako zobrazovací modul Razor.
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **aplikace\_Data** složku projekt a vyberte **přidat | Existující položka**. Vyhledejte **Source\Assets\App\_Data** složky tohoto testovacího prostředí a přidejte **Photos.json** souboru.
5. Vytvořte nový řadič s názvem **PhotoController**. Chcete-li to provést, klikněte pravým tlačítkem na **řadiče** složku, přejděte na **přidat** a vyberte **řadiče.** Dokončení názvu kontroleru, ponechte **kontroler MVC prázdný** šablonu a klikněte na tlačítko **přidat**.

    ![Přidávání PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "přidání PhotoController")

    *Přidávání PhotoController*
6. Nahraďte **Index** metoda s následující **Galerie** akce a vrátí obsah ze souboru JSON jste nedávno přidali do projektu.

    (Code fragment kódu - *architektury ASP.NET MVC 4 laboratoř - Ex02 - Galerie akce*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
~~~
7. Stiskněte klávesu **F5** pro spuštění řešení a pak přejděte na následující adresu URL k testování služby mocked fotografií: `http://localhost:[port]/photo/gallery` (hodnoty [port] závisí na aktuální portu, kde byla aplikace spuštěná). Požadavek na tuto adresu URL by měla načíst obsah **Photos.json** souboru.

    ![Testování službu mocked fotografií](whats-new-in-aspnet-mvc-4/_static/image20.png "testování mocked fotografií služby")

    *Testování mocked fotografií služby*

V implementaci skutečné můžete použít [rozhraní ASP.NET Web API](../../../../web-api/index.md) k implementaci služby Fotogalerie. Rozhraní ASP.NET Web API je rozhraní, které usnadňuje sestavování služeb HTTP, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení. Rozhraní ASP.NET Web API je ideální platformu pro sestavování aplikací RESTful v rozhraní .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Úloha 2 – zobrazení galerie fotografií

V této úloze aktualizujte domovské stránce k zobrazení galerie fotografií pomocí mocked službu, kterou jste vytvořili v první úloze tohoto cvičení. Přidáte soubory modelu a aktualizovat zobrazení galerie.

1. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** Zastavit ladění aplikace.
2. Vytvořte **fotografií** třídy v **modely** složky. Chcete-li to provést, klikněte pravým tlačítkem na **modely** složky, vyberte **přidat** a klikněte na tlačítko **třída**. Potom nastavte název na **Photo.cs** a klikněte na tlačítko **přidat**.
3. Přidejte následující členy **fotografií** třídy.

    (Code fragment kódu - *modelu ASP.NET MVC 4 laboratoř - Ex02 - fotografie*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
~~~
4. Otevřete **HomeController.cs** souboru z **řadiče** složky.
5. Přidejte následující příkazy using.

    (Code fragment kódu - *architektury ASP.NET MVC 4 laboratoř - Ex02 - HomeController direktiv Using*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
~~~
6. Aktualizace **Index** akci použít **HttpClient** k načtení dat Galerie a potom pomocí **JavaScriptSerializer** k deserializaci do modelu zobrazení.

    (Code fragment kódu - *architektury ASP.NET MVC 4 laboratoř - Ex02 - indexu akce*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
~~~
7. Otevřete **Index.cshtml** soubor umístěný v části **Views\Home** složky a nahradit veškerý obsah následujícím kódem.

    Tento kód projde všech fotografií získaný ze služby a zobrazí je do neuspořádaný seznam.

    (Code fragment kódu - *architektury ASP.NET MVC 4 laboratoř - Ex02 - fotografií seznamu*)


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
~~~
8. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **obsahu** složku projekt a vyberte **přidat | Existující položka**. Vyhledejte **Source\Assets\Content** složky tohoto testovacího prostředí a přidejte **Site.css** souboru. Budete muset potvrdit jeho nahrazení. Pokud máte **Site.css** soubor otevřít, budete muset potvrďte také znovu načíst soubor.
9. Otevřete Průzkumníka souborů a zkopírujte celou **fotografie** složka umístěná **Source\Assets** složky tohoto testovacího prostředí do kořenové složky vašeho projektu v Průzkumníku řešení.
10. Spusťte aplikaci. Měli byste nyní vidět na domovskou stránku zobrazení fotografií v galerii.

    ![Galerie fotografií](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotogalerie")

    *Galerie fotografií*
11. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** Zastavit ladění aplikace.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Cvičení 3: Přidání podpory pro mobilní zařízení

Jednou z klíčových aktualizací v architektuře ASP.NET MVC 4 je podpora pro vývoj mobilních řešení. V tomto cvičení zaměříte ASP.NET MVC 4 nové funkce pro mobilní aplikace tím, že rozšíří Fotogalerie řešení, které jste vytvořili v předchozím cvičení.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Úloha 1 - instalaci jQuery Mobile v aplikaci ASP.NET MVC 4

1. Otevřete **začít** řešení nacházející se v **zdroj/EX3.-MobileSupport/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **Konzola správce balíčků** kliknutím **nástroje** &gt; **Správce balíčků knihoven** &gt; **Správce balíčků Konzole** možnost nabídky.

    ![Otevření konzole Správce balíčků NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "otevření konzole Správce balíčků NuGet")

    *Otevření konzoly Správce balíčků NuGet*
3. V konzole Správce balíčků spusťte následující příkaz k instalaci **jQuery.Mobile.MVC** balíčku.

    jQuery Mobile je knihovna s otevřeným zdrojem pro vytváření dotykovým webového uživatelského rozhraní. Balíček NuGet jQuery.Mobile.MVC zahrnuje pomocné rutiny pro použití jQuery Mobile pomocí aplikace ASP.NET MVC 4.

    > [!NOTE]
    > Spuštěním následujícího příkazu můžete bude stahování knihovně jQuery.Mobile.MVC z Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Tento příkaz nainstaluje jQuery Mobile a některé pomocné soubory, včetně následujících:

    - **Zobrazení a sdílených nebo\_Layout.Mobile.cshtml**: je optimalizovaná pro menší obrazovky systémem Mobile rozložení jQuery. Když web obdrží žádost od prohlížeč pro mobilní zařízení, nahradí původní rozložení (\_Layout.cshtml) tímto tématem.
    - Součást přepínači zobrazení: se skládá z **zobrazení a sdílených nebo\_ViewSwitcher.cshtml** částečné zobrazení a **ViewSwitcherController.cs** řadiče. Tato součást se zobrazí odkaz na mobilní prohlížeče, aby mohli uživatelé přepnout na verzi pro stolní počítače stránky.  
        ![Galerie fotografií projektu s podporou mobilních](whats-new-in-aspnet-mvc-4/_static/image23.png "Fotogalerie projektu s podporou mobilních")

        *Galerie fotografií projektu s podporou mobilních*
4. Zaregistrujte mobilní sady. Chcete-li to provést, otevřete **Global.asax.cs** souboru a přidejte následující řádek.

    (Code fragment kódu - *architektury ASP.NET MVC 4 laboratoř - Ex03 - zaregistrovat mobilní sady*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
~~~
5. Spusťte aplikaci pomocí plochy webového prohlížeče.
6. Otevřete **Windows Phone 7 emulátoru,** umístěný v **nabídce Start | Všechny programy | Windows Phone SDK 7.1 | Emulátor Windows Phone.**
7. Na obrazovce start phone otevřete Internet Explorer. Podívejte se na adresu URL, kde je aplikace spuštěna a přejděte na tuto adresu URL pomocí prohlížeče telefonu (například `http://localhost:[PortNumber]/`).

    Si všimnete, že aplikace bude vypadat různých v emulátoru Windows Phone, jako jQuery.Mobile.MVC vytvořil nové prostředky ve vašem projektu, který zobrazí zobrazení optimalizované pro mobilní zařízení.

    Všimněte si zpráv v horní části telefonu, zobrazuje odkaz, který se přepne do zobrazení plochy. Kromě toho ** \_Layout.Mobile.cshtml** rozložení, který byl vytvořen jste nainstalovali balíček je včetně rozložení v aplikaci.

    > [!NOTE]
    > Zatím není žádný odkaz na mobilní zobrazení nelze vrátit zpět. Mají být zahrnuty v novějších verzích.

    ![Mobilní zobrazení fotografií Galerie domovské stránky](whats-new-in-aspnet-mvc-4/_static/image24.png "mobilní zobrazení fotografií Galerie domovské stránky")

    *Mobilní zobrazení fotografií Galerie domovské stránky*
8. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** Zastavit ladění aplikace.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Úloha 2 – Vytvoření mobilní zobrazení

V této úloze vytvoříte mobilní verzi zobrazení pro index s obsah přizpůsobit pro lepší appareance v mobilních zařízeních.

1. Kopírování **Views\Home\Index.cshtml** zobrazení a vložte ji k vytvoření kopie, přejmenujte nového souboru **Index.Mobile.cshtml**.
2. Otevřete nový vytvořit **Index.Mobile.cshtml** zobrazení a nahradit existující &lt;ul&gt; značky s tímto kódem. Díky tomu bude aktualizace &lt;ul&gt; značky s anotacemi mobilních dat jQuery pomocí mobilních motivů z jQuery.


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

> [!NOTE] 
> 
> Notice that:
> 
> - The **data-role** attribute set to **listview** will render the list using the listview styles.
> 
> - The **data-inset** attribute set to true will show the list with rounded border and margin.
> 
> - The **data-filter** attribute set to **true** will generate a search box.
> 
> You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
~~~
3. Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.
4. Přepnout **emulátoru Windows Phone** a aktualizovat lokalitu. Všimněte si nového vzhledu a chování seznamu galerie, jakož i nové vyhledávacího pole umístěné v horní části. Poté do vyhledávacího pole zadejte slovo (například **Tulipány**) Chcete-li hledat v galerii fotografií.

    ![Pomocí filtrování listview styl galerie](whats-new-in-aspnet-mvc-4/_static/image25.png "Galerie pomocí filtrování styl listview")

    *Galerie pomocí filtrování styl listview*

    To Shrneme, jste použili recepturách zobrazení Mobilizer Pokud chcete vytvořit kopii zobrazení Index se &quot;mobilní&quot; příponu. Tato přípona ASP.NET MVC 4 označuje, že každou žádost vygeneruje z mobilního zařízení bude používat tuto kopii index. Kromě toho jste aktualizovali mobilní verzi Index zobrazení pro použití jQuery Mobile rozšíření lokality vzhledu a chování v mobilních zařízeních.
5. Přejděte zpět na Visual Studio a otevřete **Site.Mobile.css** umístěná **obsahu** složky.
6. Opravte umístění titulek fotografie, chcete-li zobrazit na pravé straně bitové kopie. K tomu, přidejte následující kód, který **Site.Mobile.css** souboru.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.
8. Přepnout zpět **emulátoru Windows Phone** a aktualizovat lokalitu. Všimněte si, že název fotografií je správně nastavený teď.

    ![Název umístěný na pravé straně bitové kopie](whats-new-in-aspnet-mvc-4/_static/image26.png "Title umístěný na pravé straně bitové kopie")

    *Název umístěný na pravé straně bitové kopie*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Úloha 3 – motivy jQuery Mobile

Každé rozložení a widget jQuery Mobile je uspořádaná kolem nové objektově orientované šablon stylů CSS rozhraní, které vám umožní použít dokončení jednotná visual návrhu motiv k webům a aplikacím.

jQuery Mobile výchozí motiv zahrnuje 5 vzorník, které jsou uvedeny písmena (a, b, c, d e) pro rychlou referenci.

V této úloze aktualizujte mobilní rozložení používat jiný než výchozí motiv.

1. Přepněte zpátky na Visual Studio.
2. Otevřete ** \_Layout.Mobile.cshtml** soubor umístěný ve **Views\Shared**.
3. Najít div element k roli dat nastavena na &quot;stránky&quot; a aktualizovat **data-theme** k &quot; **e**&quot;.


~~~
[!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
~~~
4. Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.
5. Aktualizujte lokality v **emulátoru Windows Phone** a Všimněte si nové schéma barev.

    ![Mobilní rozložení s jiné barevné schéma](whats-new-in-aspnet-mvc-4/_static/image27.png "mobilní rozložení s jiné barevné schéma")

    *Mobilní rozložení s jiné barevné schéma*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Úloha 4 – pomocí komponentu přepínači zobrazení a prohlížeče přepisování funkcí

Konvence pro optimalizované mobilní webové stránky je přidat odkaz, jejíž text je něco jako zobrazení plochy nebo režim plnou verzi webu, který umožňuje uživatelům přepnout na ploše verzi stránky. Balíček jQuery.Mobile.MVC obsahuje ukázkový **přepínači zobrazení** součásti pro tento účel použít v ** \_Layout.Mobile.cshtml** zobrazení.

![Odkaz na přepnout na zobrazení plochy](whats-new-in-aspnet-mvc-4/_static/image28.png "odkaz na přepnout na zobrazení plochy")

*Propojit přepnout do zobrazení plochy*

K přepínači zobrazení používá novou funkci s názvem **přepsání prohlížeče**. Tato funkce umožňuje aplikaci zacházet se žádostmi, jako kdyby kdyby přicházely z jiného prohlížeče (uživatelského agenta) než je ta, kterou ve skutečnosti přicházejí z.

V této úloze bude prozkoumat ukázkové implementace přepínači zobrazení přidal jQuery.Mobile.MVC a nový prohlížeč přepisování funkce v rozhraní ASP.NET MVC 4.

1. Přepněte zpátky na Visual Studio.
2. Otevřete ** \_Layout.Mobile.cshtml** zobrazení umístěná **Views\Shared** složky a Všimněte si komponentu přepínači zobrazení, který je odkazováno jako částečné zobrazení.

    ![Mobilní rozložení pomocí součásti přepínači zobrazení](whats-new-in-aspnet-mvc-4/_static/image29.png "mobilní rozložení pomocí součásti přepínači zobrazení")

    *Mobilní rozložení pomocí součásti přepínači zobrazení*
3. Otevřete ** \_ViewSwitcher.cshtml** částečné zobrazení.

    Částečné zobrazení používá nová metoda **ViewContext.HttpContext.GetOverriddenBrowser()** zobrazit na odpovídající odkaz přepnout buď zobrazení Desktop nebo Mobile a určení původu webového požadavku.

    **GetOverridenBrowser** metoda vrátí **HttpBrowserCapabilitiesBase** instance, která odpovídá uživatelský agent nastaveno pro požadavek (skutečné nebo přepsané). Tuto hodnotu můžete získat vlastnosti, jako **IsMobileDevice**.

    ![Částečné zobrazení ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher částečné zobrazení")

    *ViewSwitcher částečné zobrazení*
4. Otevřete **ViewSwitcherController.cs** třída umístěný v **řadiče** složky. Podívejte se na této funkci SwitchView akce je volána metodou odkaz v komponentě ViewSwitcher a Všimněte si nových metod položka HttpContext.

    - **HttpContext.ClearOverridenBrowser()** metoda odebere každého přepsaného uživatelského agenta pro aktuální požadavek.
    - **HttpContext.SetOverridenBrowser()** metoda přepíše hodnotu skutečného uživatelského agenta žádosti pomocí zadaného uživatelského agenta.  
        ![Řadič ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher řadiče")  
*ViewSwitcher řadiče*

        Přepíše prohlížeče je základní funkce ASP.NET MVC 4, což je také k dispozici i v případě, že nenainstalujete jQuery.Mobile.MVC balíčku. Ale tato funkce ovlivňuje pouze zobrazení, rozložení a částečného zobrazení a neovlivní žádné funkce, které jsou závislé na Request.Browser objektu.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Úloha 5 – Přidání k přepínači zobrazení v zobrazení plochy

V této úloze aktualizujte plochy rozložení zahrnout k přepínači zobrazení. To vám umožní mobilním uživatelům přejděte zpět na mobilní zobrazení při procházení zobrazení plochy.

1. Aktualizujte lokality v **emulátoru Windows Phone**.
2. Klikněte na **zobrazení plochy** odkaz v horní části galerie. Všimněte si, že v zobrazení plochy tak, aby umožnily že vrátíte na mobilní zobrazení žádné přepínači zobrazení.
3. Přejděte zpět na Visual Studio a otevřete ** \_Layout.cshtml** zobrazení.
4. Najít oddíl přihlášení a vložení volání k vykreslení ** \_ViewSwitcher** částečné zobrazení níže ** \_LogOnPartial** částečné zobrazení. Potom stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
~~~
5. Stiskněte klávesu **kombinaci kláves CTRL + S** a uložte změny.
6. Aktualizujte stránku v emulátoru Windows Phone a dvakrát klikněte na obrazovce a přiblížení. Všimněte si, že nyní zobrazuje domovské stránce **mobilní zobrazení** odkaz, který přepíná z mobilních na zobrazení plochy.

    ![Zobrazit v zobrazení plochy přepínači](whats-new-in-aspnet-mvc-4/_static/image32.png "přepínači zobrazení se zobrazují v zobrazení plochy")

    *Přepínači zobrazení se zobrazují v zobrazení plochy*
7. Přepněte do pohledu mobilní znovu a přejděte do <strong>o</strong> stránky (http://localhost[port] / Home/o). Všimněte si, že i v případě, že jste nevytvořili zobrazení About.Mobile.cshtml, o stránka se zobrazí pomocí mobilních rozložení (\_Layout.Mobile.cshtml).

    ![O stránku](whats-new-in-aspnet-mvc-4/_static/image33.png "o stránce")

    *O stránku*
8. Nakonec otevřete web ve webovém prohlížeči klientů. Všimněte si, že žádnou z předchozích aktualizací ovlivnil zobrazení plochy.

    ![Zobrazení plochy Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image34.png "Fotogalerie zobrazení plochy")

    *Zobrazení plochy Fotogalerie*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Úloha 6 – Vytvoření nové režimy zobrazení

Nová funkce režimy zobrazení umožňuje aplikaci, vyberte zobrazení v závislosti na prohlížeči, který vytváří žádosti. Například pokud prohlížeč pro stolní počítač požadavky na domovskou stránku, aplikace se vrátit **Views\Home\Index.cshtml** šablony. Pokud prohlížeč pro mobilní zařízení požadavky na domovskou stránku, aplikace se vrátíte **Views\Home\Index.mobile.cshtml** šablony.

V této úloze se vytvoří vlastní rozložení pro zařízení iPhone a bude mít k simulaci požadavky ze zařízení iPhone. K tomuto účelu můžete použít buď emulátoru nebo simulátor pro zařízení iPhone (jako je [Electric Mobile simulátoru](http://www.electricplum.com/)) nebo prohlížeči s doplňků, které upravit uživatelský agent. Pokyny k nastavení řetězec uživatelského agenta v prohlížeči Safari emulovat zařízení typu iPhone najdete v tématu [jak umožníte Safari předstírají, že je aplikace Internet Explorer](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) v blogu David Alison.

**Všimněte si, že tato úloha je volitelný a může pokračovat v testovacím prostředí bez její provedení.**

1. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** Zastavit ladění aplikace.
2. Otevřete **Global.asax.cs** a přidejte následující příkaz using.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
~~~
3. Přidejte následující zvýrazněný kód do aplikace\_Start – metoda.

    (Code fragment kódu - *ASP.NET MVC 4 laboratoř - Ex03 - iPhone DisplayMode*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request. If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix. The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.

After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.

> [!NOTE]
> This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).
~~~
4. Vytvořit kopii ** \_Layout.Mobile.cshtml** v soubor **Views\Shared** složku a přejmenujte kopírovat do &quot; ** \_Layout.iPhone.csthml **&quot;.
5. Otevřete ** \_Layout.iPhone.csthml** jste vytvořili v předchozím kroku.
6. Najít div element s atribut data-role nastaven na **stránky** a změňte **data-theme** atribut &quot; **a**&quot;.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Now you have 3 layouts in your ASP.NET MVC 4 application:

1. **\_Layout.cshtml**: default layout used for desktop browsers.
2. **\_Layout.mobile.cshtml**: default layout used for mobile devices.
3. **\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.
~~~
7. Stiskněte klávesu **F5** ke spuštění aplikace a přejděte do lokality v **emulátoru Windows Phone**.
8. Otevřete **iPhone simulátoru** (najdete v části [příloha C](#AppendixC) pokyny o tom, jak nainstalovat a nakonfigurovat simulátor pro zařízení iPhone) a přejděte na web příliš. Všimněte si, že každý phone používá konkrétní šablonu.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Použití různých zobrazení pro každý mobilní zařízení*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Cvičení 4: Použití asynchronní řadičů

Rozhraní Microsoft .NET Framework 4.5 zavádí nové jazykové funkce v C# a Visual Basic zajistit nové platformu pro asynchrony v .NET – programování. Tento nový foundation umožňuje asynchronní programování podobná - a o stejně jednoduché jako - synchronní programování. Je nyní možné zapisovat pomocí metody asynchronní akce v architektuře ASP.NET MVC 4 **AsyncController** třídy. Můžete použít asynchronní akce metody pro dlouhodobé, bez procesoru vázaný požadavky. Tím je zabráněno blokování webový server z provede práci během zpracování požadavku. Třída AsyncController se obvykle používá pro dlouhodobé volání webové služby.

Tento postup vysvětluje základy asynchronní operace v rozhraní ASP.NET MVC 4. Pokud chcete o podrobnější prohlídku, naleznete v následujícím článku na: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Úloha 1 – implementace asynchronní kontroler

1. Otevřete **začít** řešení nacházející se v **zdroj/Ex4-asynchronní/počáteční/** složky. Jinak, může pokračovat, pomocí **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli poskytnutého **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídku a vyberte **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogové okno, klikněte na tlačítko **obnovení** Chcete-li stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod použití NuGet je, že nemáte pro odeslání všech knihoven v projektu, zmenšení velikosti projektu. Napájení nástroje NuGet zadáním verze balíčku v souboru Packages.config, nebudete moct stáhnout všechny požadované knihovny při prvním spuštění projektu. Z tohoto důvodu je nutné provést tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřete **HomeController.cs** třídy z **řadiče** složky.
3. Přidejte následující příkaz using.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
~~~
4. Aktualizace **HomeController** třídy dědí **AsyncController**. Řadiče, které jsou odvozeny od AsyncController povolit technologii ASP.NET pro zpracování asynchronní požadavky a mohou stále metody synchronní akce služby.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
~~~
5. Přidat **asynchronní** – klíčové slovo k **Index** metoda a nastavit jej vrátí typ **úloh&lt;ActionResult&gt;**.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

> [!NOTE]
> The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code. A **Task** object represents an asynchronous operation that may complete at some point in the future.
~~~
6. Nahraďte **klienta. GetAsync()** volání s použitím verze úplné asynchronní – klíčové slovo await, jak je uvedeno níže.

    (Code fragment kódu - *architektury ASP.NET MVC 4 laboratoř - Ex04 - GetAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

> [!NOTE]
> In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).
> 
> Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call. This means that the rest of the code will be executed as a callback only after the awaited method completes. Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.
~~~
7. Změnit kód pokračujte s asynchronní implementace nahrazením řádky s novým kódem, jak je uvedeno níže

    (Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
~~~
8. Spusťte aplikaci. Všimnete si žádné větší změny, ale váš kód nebude blokování vlákna z fondu podprocesů Příprava lepší využití serverových prostředků a zlepšení výkonu.

    > [!NOTE]
    > Další informace o nových funkcích asynchronního programování v testovacím prostředí &quot; **asynchronní programování v rozhraní .NET 4.5 s C# a Visual Basic** &quot; zahrnuté v sadě Visual Studio školení Kit.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Úloha 2 – zpracování vypršení časových limitů s zrušení tokenů

Asynchronní akce metody, které vracejí instance úloh může také podporovat vypršení časových limitů. V této úloze aktualizujte kód metoda Index pro zpracování časový limit scénář používající token zrušení.

1. Přejděte zpět na Visual Studio a stiskněte klávesu **SHIFT + F5** Zastavit ladění.
2. Přidejte následující příkaz k použití **HomeController.cs** souboru.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
~~~
3. Akce indexu přijímat aktualizace **CancellationToken** argument.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
~~~
4. Aktualizace **GetAsync** volání předat token zrušení.

    (Code fragment kódu - *SendAsync laboratoř - Ex04 - architektury ASP.NET MVC 4 s CancellationToken*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
~~~
5. Uspořádání *Index* metoda s **hodnota vlastnosti AsyncTimeout** atributu nastavena na 500 milisekund a **HandleError** atributu nakonfigurovaného pro zpracování ** TaskCanceledException** přesměrováním na **TimedOut** zobrazení.

    (Code fragment kódu - *atributy architektury ASP.NET MVC 4 laboratoř - Ex04 -*)


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
~~~
6. Otevřete **PhotoController** třídy a aktualizace **Galerie** metoda zpoždění spuštění 1000 miliseconds (1 sekunda) simulovat dlouhotrvající úlohy.


~~~
[!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
~~~
7. Otevřete **Web.config** souboru a povolit vlastní chyby přidáním následující element.


~~~
[!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
~~~
8. Vytvořit nové zobrazení v **Views\Shared** s názvem **TimedOut** a použít výchozí rozložení. V Průzkumníku řešení klikněte pravým tlačítkem myši **Views\Shared** složky a vyberte **přidat | Zobrazení**.

    ![Pro každé mobilních zařízení pomocí různých zobrazení](whats-new-in-aspnet-mvc-4/_static/image36.png "použití různých zobrazení pro každý mobilní zařízení")

    *Použití různých zobrazení pro každý mobilní zařízení*
9. Aktualizace **TimedOut** zobrazit obsah, jak je uvedeno níže.


~~~
[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
~~~
10. Spusťte aplikaci a přejděte do adresy URL kořenového adresáře. Jak jste přidali **Thread.Sleep** na 1000 milisekund, budete mít k vypršení časového limitu, generovaných **hodnota vlastnosti AsyncTimeout** atribut a catch podle **HandleError** atribut.

    ![Časový limit výjimka zpracovává](whats-new-in-aspnet-mvc-4/_static/image37.png "zpracovává výjimka časového limitu")

    *Časový limit výjimka zpracovává*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci do následující weby systému Windows Azure [Dodatek D: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V tomto hands-na-testovacím prostředí můžete jsme zaznamenali, některé z nových funkcí v architektuře ASP.NET MVC 4 trvalé. Projednat následující koncepty:

- Využívat výhod rozšíření, která chcete šablony včetně projektu ASP.NET MVC šabloně nový projekt mobilních aplikací
- Pomocí zobrazení atributů jazyka HTML5 a dotazy na média šablon stylů CSS ke zlepšení zobrazení na mobilních zařízeních
- Pro progresivní vylepšení a vytváření dotykovým webového uživatelského rozhraní použijte jQuery Mobile
- Vytvoření mobilní konkrétní zobrazení
- Používání komponent přepínači zobrazení lze přepínat mezi mobilních a desktopových zobrazení v aplikaci
- Vytvořte asynchronní řadičů pomocí úloh podpory

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Příloha A: používání fragmentů kódu

S fragmenty kódu máte všechny kód, který je nutné na dosah ruky. Dokument testovacího prostředí vás bude informovat přesně Pokud můžete, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu](whats-new-in-aspnet-mvc-4/_static/image38.png "fragmenty kódu pomocí sady Visual Studio pro vložení kódu do projektu")

*Používání fragmentů kódu v sadě Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# pouze)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezery nebo spojovníky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající fragmenty názvy.
4. Vyberte správný fragment kódu (nebo ponechte zadáním dokud je vybraný název celý fragmentu).
5. Stisknutím klávesy Tab dvakrát můžete vložit fragment v umístění kurzoru.

![Začněte psát název fragmentu](whats-new-in-aspnet-mvc-4/_static/image39.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stisknutím klávesy Tab vyberte fragmentu zvýrazněná](whats-new-in-aspnet-mvc-4/_static/image40.png "stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu")

*Stisknutím klávesy Tab vyberte zvýrazněný fragmentu kódu*

![Stisknutím klávesy Tab znovu a fragmentu rozšíří](whats-new-in-aspnet-mvc-4/_static/image41.png "stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.")

*Stisknutím klávesy Tab znovu a fragmentu bude rozšiřovat.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)***

1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.
2. Vyberte **Vložit fragment** následuje **Moje fragmenty kódu**.
3. Vyberte relevantní fragment kódu ze seznamu, kliknutím na.

![Klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment](whats-new-in-aspnet-mvc-4/_static/image42.png "klikněte pravým tlačítkem, kam chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem myši, kam chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte relevantní fragment kódu ze seznamu, kliknutím na](whats-new-in-aspnet-mvc-4/_static/image43.png "vyberte relevantní fragment kódu ze seznamu, kliknutím na")

*Vyberte relevantní fragment kódu ze seznamu, kliknutím na*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Příloha B: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiný &quot;Express&quot; pomocí verze ** [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) **. Následující pokyny vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Nainstalovat Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "nainstalovat Visual Studio Express")

    *Nainstalovat Visual Studio Express*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express pro Web, přejděte na **spustit** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express pro Web** dlaždice.

    ![VS Express pro Web dlaždice](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express pro Web dlaždice*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Příloha C: instalace služby WebMatrix 2 a iPhone simulátoru

Spusťte svůj web ve iPhone simulované zařízení můžete rozšíření prostředí WebMatrix &quot;Electric simulátoru Mobile pro iPhone&quot;. Také můžete nakonfigurovat stejné rozšiřující spouštět simulátoru ze sady Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Úloha 1 – instalace služby WebMatrix 2

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produktu &quot; <em>WebMatrix 2</em>&quot;.
2. Klikněte na **nyní nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** zahájíte instalaci.

    ![Instalace služby WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalace služby WebMatrix 2")

    *Instalace služby WebMatrix 2*
4. Číst všechny produkty se licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Vyjádření souhlasu s podmínkami licence](whats-new-in-aspnet-mvc-4/_static/image50.png "vyjádření souhlasu s podmínkami licence")

    *Vyjádření souhlasu s podmínkami licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](whats-new-in-aspnet-mvc-4/_static/image51.png "průběh instalace")

    *Průběh instalace*
6. Po dokončení instalace, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena](whats-new-in-aspnet-mvc-4/_static/image52.png "instalace byla dokončena.")

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončení** ukončíte instalační program webové platformy.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Úloha 2 – instalace iPhone simulátoru rozšíření

1. Spustit **WebMatrix** a otevřete všechny existující webovou stránku nebo vytvořte novou.
2. Klikněte **spustit** tlačítko z **Domů** pásu karet a vyberte **přidat nový**.

    ![Přidání nové rozšíření prostředí WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "přidání nové rozšíření prostředí WebMatrix")

    *Přidání nové rozšíření prostředí WebMatrix*
3. Vyberte **iPhone simulátoru** a klikněte na tlačítko **nainstalovat**.

    ![Procházení rozšíření prostředí WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "rozšíření procházení služby WebMatrix")

    *Procházení rozšíření pro službu WebMatrix*
4. V podrobnosti balíčku, klikněte na **nainstalovat** Chcete-li pokračovat v instalaci rozšíření.

    ![iPhone simulátoru rozšíření](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone simulátoru rozšíření")

    *iPhone simulátoru rozšíření*
5. Přečtěte si a přijměte rozšíření smlouvy EULA.

    ![Rozšíření prostředí WebMatrix smlouvy EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "rozšíření prostředí WebMatrix smlouvy EULA")

    *Rozšíření prostředí WebMatrix smlouvy EULA*
6. Webové stránky můžete nyní spustit ze služby WebMatrix pomocí simulátoru možnost iPhone.

    ![Spouštět s využitím iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "spouštět s využitím iPhone")

    *Spouštět s využitím iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Úloha 3 – konfigurace sady Visual Studio 2012 ke spuštění iPhone simulátoru

1. Otevřete **Visual Studio 2012** a otevřít libovolný web, nebo vytvořte nový projekt.
2. Klikněte na šipku dolů z tlačítko Spustit a vyberte **procházet s**.

    ![Procházet s](whats-new-in-aspnet-mvc-4/_static/image58.png "procházet s")

    *Procházet s*
3. V &quot;procházet s&quot; dialogové okno, klikněte na tlačítko **přidat**.
4. V &quot;přidat Program&quot; dialogové okno, použijte následující hodnoty:

   - <strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (aktualizace odpovídajícím způsobem cestu)</em>
   - **Argumenty**: &quot;1&quot;
   - **Popisný název**: iPhone simulátoru

     ![Přidat program](whats-new-in-aspnet-mvc-4/_static/image59.png "přidat program")

     *Přidejte program procházet s*
5. Klikněte na tlačítko **OK** a zavřete dialogová okna.
6. Nyní budete moci spustit vaší webové aplikace v simulátoru iPhone ze sady Visual Studio 2012.

    ![Procházet s iPhone simulátoru](whats-new-in-aspnet-mvc-4/_static/image60.png "procházet s iPhone simulátoru")

    *Procházet s iPhone simulátoru*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha D: publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

Tento dodatek vám ukáže, jak vytvořit nový web z portálu Windows Azure Management Portal a publikovat aplikace, kterou jste získali podle testovacím prostředí, využívat výhod nasazení webu publikování funkce poskytované služby Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Úloha 1 – Vytvoření nového webu ze systému Windows Azure Portal

1. Přejděte na [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů společnosti Microsoft, které jsou spojené s vaším předplatným.

    > [!NOTE]
    > S Windows Azure můžete bezplatné hostování 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete si zaregistrovat [zde](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k Windows Azure Management Portal*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](whats-new-in-aspnet-mvc-4/_static/image62.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **výpočetní** | **webu**. Potom vyberte **rychle vytvořit** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvoření webu**.

    > [!NOTE]
    > Web systému Windows Azure je hostitel pro spouštění v cloudu, ve kterém můžete řídit a spravovat webovou aplikaci. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Postup pro nastavení databáze neobsahuje.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-aspnet-mvc-4/_static/image63.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** je vytvořena.
5. Po vytvoření webu klikněte na odkaz v části **URL** sloupce. Zkontrolujte, zda je funkční nový web.

    ![Procházení na nový web](whats-new-in-aspnet-mvc-4/_static/image64.png "procházení na nový web")

    *Procházení na nový web*

    ![Webový server spuštěn](whats-new-in-aspnet-mvc-4/_static/image65.png "webu systémem")

    *Spuštění webu*
6. Přejděte zpět na portál a klikněte na název webu v části **název** sloupec pro zobrazení stránky pro správu.

    ![Otevření stránky Správa webu](whats-new-in-aspnet-mvc-4/_static/image66.png "otevření stránek správu webového serveru")

    *Otevření stránek správu webového serveru*
7. V **řídicí panel** v části **rychlého přehledu** klikněte na položku **stažení profilu publikování** odkaz.

    > [!NOTE]
    > *Profilu publikování* obsahuje všechny informace požadované pro publikování webové aplikace na web služby Windows Azure pro každou metodu povoleno publikace. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a řetězců databází, které jsou potřebné k připojení k a ověřování na základě těchto koncových bodů, pro které je metoda publikace povolena. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pro Web** a **sadu Microsoft Visual Studio 2012** podporu čtení publikační profily k automatické konfiguraci těchto programů pro publikování webové aplikace na weby služby Windows Azure.

    ![Na webu stažení profilu publikování](whats-new-in-aspnet-mvc-4/_static/image67.png "stahování webové stránky profilu publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profil publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak používat tento soubor k publikování webové aplikace na weby systému Windows Azure ze sady Visual Studio.

    ![Ukládání souboru profilu publikování](whats-new-in-aspnet-mvc-4/_static/image68.png "ukládání profilu publikování")

    *Ukládání souboru profilu publikování*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá systému SQL Server, databáze, budete muset vytvořit databázi SQL server. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá systém SQL Server může tuto úlohu přeskočit.

1. Budete potřebovat databázi SQL serveru pro ukládání databázi aplikace. Databáze SQL servery můžete zobrazit ze svého předplatného na portál Windows Azure Management **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořeno, můžete vytvořit jeden pomocí **přidat** tlačítka na panelu příkazů. Poznamenejte si **název serveru a adresa URL, správce přihlašovací jméno a heslo**, jako je použijete v další úkoly. Nevytvářejte databáze ještě, jak bude vytvořen v pozdější fázi.

    ![Řídicí panel serveru databáze SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "řídicího panelu serveru databáze SQL")

    *Řídicí panel serveru databáze SQL*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio, proto je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. To lze provést, klikněte na tlačítko **konfigurace**, vyberte IP adresu z **aktuální IP adresa klienta** a vkládání na **počáteční IP adresa** a **Koncová IP adresa** textová pole a kliknutím ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) tlačítko.

    ![Přidávání IP adresy klienta](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Přidávání IP adresy klienta*
3. Jednou **IP adresa klienta** je povolené IP adresy do seznamu, klikněte na **Uložit** potvrďte změny.

    ![Potvrzení změn](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Potvrzení změn*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nástroje nasazení webu

1. Přejděte zpět na ASP.NET MVC 4 řešení. V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](whats-new-in-aspnet-mvc-4/_static/image73.png "publikování aplikace")

    *Publikování webu*
2. Umožňuje naimportujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](whats-new-in-aspnet-mvc-4/_static/image74.png "import profilu publikování")

    *Import profilu publikování*
3. Klikněte na tlačítko **ověření připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření je hotová, jakmile se zobrazí zelené zaškrtnutí zobrazí vedle tlačítko ověřit připojení.

    ![Ověření připojení](whats-new-in-aspnet-mvc-4/_static/image75.png "ověřování připojení")

    *Ověření připojení*
4. V **nastavení** v části **databáze** části, klikněte na tlačítko vedle připojení databáze textové pole (tj. **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](whats-new-in-aspnet-mvc-4/_static/image76.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Připojení k databázi nakonfigurujte následujícím způsobem:

   - V **název serveru** zadejte vaše databáze SQL serveru adresu URL pomocí *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte přihlašovací heslo správce serveru.
   - Zadejte nový název databáze, například: *MVC4SampleDB*.

     ![Konfigurace cílový připojovací řetězec](whats-new-in-aspnet-mvc-4/_static/image77.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze, klikněte na tlačítko **Ano**.

    ![Vytvoření databáze](whats-new-in-aspnet-mvc-4/_static/image78.png "vytváření řetězec databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL v systému Windows Azure je uvedené v rámci textbox výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na databázi SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "připojovací řetězec odkazující na databázi SQL")

    *Připojovací řetězec odkazující na databázi SQL*
8. V **Preview** klikněte na tlačítko **publikovat**.

    ![Publikování webové aplikace](whats-new-in-aspnet-mvc-4/_static/image80.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Jakmile proces publikování dokončí, otevře se výchozí prohlížeč publikované webové stránky.

    ![Aplikace publikována do služby Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplikace publikována do služby Windows Azure")

    *Aplikace, které jsou publikovány do služby Windows Azure*
