---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Co je nového v architektuře ASP.NET MVC 4 | Dokumentace Microsoftu
author: rick-anderson
description: ASP.NET MVC 4 je rozhraní pro vytváření aplikací škálovatelná webů založené na standardech pomocí zavedených návrhových postupů a sílu technologie ASP.NET a...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: b1d80928d765bc71ea1579272662b6697371c47b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752023"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Co je nového v architektuře ASP.NET MVC 4

podle [Campy Web týmu](https://twitter.com/webcamps)

[Stáhněte si Web Campy školení Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 je rozhraní pro vytváření aplikací škálovatelná webů založené na standardech pomocí zavedených návrhových postupů a sílu technologie ASP.NET a .NET framework. Tento nový, čtvrtý verzi rozhraní framework se zaměřuje na zjednodušení vývoj mobilní webové aplikace.

Než začneme když vytvoříte nový projekt ASP.NET MVC 4 není nyní šablonu projektu mobilních aplikací, které můžete použít k vytvoření samostatné aplikace speciálně pro mobilní zařízení. ASP.NET MVC 4 navíc integruje jQuery Mobile prostřednictvím balíčku NuGet jQuery.Mobile.MVC. jQuery Mobile je rozhraní založeného na technologii HTML5 umožňující vývoj webových aplikací, které jsou kompatibilní s všechny platformy oblíbených mobilních zařízení, včetně Windows Phone, iPhone, Android a tak dále. Ale pokud potřebujete specializace, ASP.NET MVC 4 také umožňuje obsluhovat různá zobrazení pro různá zařízení a poskytují optimalizace pro konkrétní zařízení.

V této praktických cvičení začnete s ASP.NET MVC 4 &quot;internetovou aplikaci&quot; šablona projektu pro vytvoření aplikace Fotogalerie. Postupně se zlepšila aplikace pomocí jQuery Mobile a nové funkce technologie ASP.NET MVC 4, aby byl kompatibilní s různých mobilních zařízení a desktopového webového prohlížeče. Dozvíte se taky o nový kód návodů pro generování kódu a jak ASP.NET MVC 4 usnadňuje psaní metody asynchronní akce díky podpoře úloh&lt;ActionResult&gt; návratové typy.

> [!NOTE]
> Všechny ukázky kódu a fragmenty kódu jsou součástí této webové Campy školicí sady, k dispozici na [verzích Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specifické pro toto testovací prostředí je k dispozici na [co je nového ve webových formulářích v ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V této praktická cvičení se dozvíte, jak:

- Využijte výhod rozšíření, která chcete šablony – včetně projektu ASP.NET MVC nové šablony projektů mobilních aplikací
- Použít atribut zobrazení HTML5 a CSS media dotazy ke zlepšení zobrazení na mobilních zařízeních
- Použít jQuery Mobile pro progresivní vylepšení a vytváření dotykového webového uživatelského rozhraní
- Vytvořit zobrazení specifické pro mobilní zařízení
- Chcete-li přepnout mezi mobilními a desktopovými zobrazení v aplikaci použijte komponentu přepínači zobrazení
- Vytvoření asynchronní řadiče pomocí podpory úkolu

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Musíte mít následující položky k dokončení tohoto testovacího prostředí:

- [Microsoft Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) nebo i vyšší (čtení [dodatku B](#AppendixB) pokyny k jeho instalaci).
- [ASP.NET MVC 4](../../../mvc4.md) (součástí instalace – Microsoft Visual Studio 2012)
- Emulátor Windows Phone (součástí [7.1.1 Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Volitelné – [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) s **Electric Plum iPhone simulátor** rozšíření (pouze pro cvičení 3 používaná k prohlížení webové aplikace se simulátor pro zařízení iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Instalace

V celém dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění je k dispozici většinu kódu jako Visual Studio fragmenty kódu, který můžete použít z Visual Studia abyste ho nemuseli znovu přidat ručně.

Chcete-li nainstalovat fragmenty kódu:

1. Otevřete okno Průzkumníka Windows a přejděte do testovacího prostředí **Source\Setup** složky.
2. Dvakrát klikněte **Setup.cmd** soubor v této složce instalace fragmenty kódu sady Visual Studio.

Pokud nejste obeznámeni s fragmenty kódu Visual Studio a chcete další informace o jejich použití, najdete dodatku z tohoto dokumentu &quot; [fragmenty kódu pomocí dodatku A:](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické testovací prostředí obsahuje následující praktická cvičení:

1. [Nové šablony projektu ASP.NET MVC 4](#Exercise1)
2. [Vytvoření webové aplikace Fotogalerie](#Exercise2)
3. [Přidání podpory pro mobilní zařízení](#Exercise3)
4. [Použití asynchronního řadiče](#Exercise4)

> [!NOTE]
> Se sadou každý cvičení **koncové** složku, která obsahuje výsledný řešení byste měli získat po dokončení cvičení. Toto řešení můžete použít jako vodítko, pokud potřebujete další pomoc prostřednictvím praktická cvičení.


Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Cvičení 1: Nové šablony projektu ASP.NET MVC 4

V tomto cvičení bude prozkoumat rozšíření v šablonách projektu ASP.NET MVC 4. Kromě šabloně Internetové aplikace již v MVC 3, tato verze nyní zahrnuje samostatné šablony pro mobilní aplikace. Nejprve se podíváte na některé důležité funkce všechny šablony. Potom bude fungovat na vykreslení stránky správně na různých platformách pomocí správný přístup.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Úloha 1 – prohlížení Internetu šablony aplikace

1. Otevřít **sady Visual Studio**.
2. Vyberte **soubor | Nové | Projekt** příkazu nabídky. V **nový projekt** dialogového okna, vyberte **Visual C# | Web** šablony v levém podokně stromové struktury a zvolte **webové aplikace ASP.NET MVC 4.** Pojmenujte projekt **Fotogalerie**, vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK**.

    > [!NOTE]
    > Později budete přizpůsobovat Fotogalerie ASP.NET MVC 4 řešení, které se právě vytváří.

    ![Vytvoření nového projektu](whats-new-in-aspnet-mvc-4/_static/image1.png "vytvoření nového projektu")

    *Vytvoření nového projektu*
3. V **nového projektu ASP.NET MVC 4** dialogového okna, vyberte **internetovou aplikaci** šablony projektu a klikněte na tlačítko **OK**. Ujistěte se, že jste zvolili jako zobrazovací modul Razor.

    ![Vytvoření nové aplikace ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image2.png "vytvoření nové aplikace ASP.NET MVC 4 Internet")

    *Vytvoření nové aplikace ASP.NET MVC 4 Internet*

    > [!NOTE]
    > Syntaxe Razor byla zavedena v architektuře ASP.NET MVC 3. Jeho cílem je minimalizovat počet znaků a stisknutí kláves vyžaduje v souboru, umožňuje rychlé a dynamika kódování pracovního postupu. Razor využívá existující C# / VB (nebo jiné) jazykové dovednosti a poskytuje šablonu syntaxe značek, umožňující pracovním Super konstrukce jazyka HTML.
4. Stisknutím klávesy **F5** obnovené šablony a spuštění řešení. Si můžete prohlédnout následující funkce:

    **Moderní styl šablony**

    Šablony byly obnoveny, poskytuje další styly moderního vzhledu.

    ![Šablony ASP.NET MVC 4 restyled](whats-new-in-aspnet-mvc-4/_static/image3.png "restyled šablony MVC 4")

    *Šablony ASP.NET MVC 4 restyled*

    ![Nová stránka kontaktu](whats-new-in-aspnet-mvc-4/_static/image4.png "nový kontakt stránky")

    *Nová stránka kontaktu*

    **Adaptivní vykreslování**

    Podívejte se na změně velikosti okna prohlížeče a Všimněte si, jak rozložení stránky dynamicky přizpůsobí novou velikost okna. Tyto šablony pomocí adaptivního vykreslování techniku správně vykreslit, desktopových a mobilních platforem bez jakéhokoli přizpůsobení.

    ![Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti](whats-new-in-aspnet-mvc-4/_static/image5.png "šablonu projektu ASP.NET MVC 4 v jiném prohlížeči velikosti")

    *Šablona projektu ASP.NET MVC 4 v jiném prohlížeči velikosti*

    **Bohatší uživatelského rozhraní pomocí jazyka JavaScript**

    Mezi další vylepšení patří do výchozí šablony projektu je použití jazyka JavaScript poskytovat vyšší interaktivitou jazyka JavaScript. Přihlášení a zaregistrujte odkazů použité v šabloně exemplify použití jQuery ověření pro ověření vstupních polí ze strany klienta.

    ![jQuery ověření](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery ověření*

    > [!NOTE]
    > Všimněte si, že dvě přihlášení v oddílech v první části můžete protokolovat zaregistrovaný účet na webu a v druhé části můžete altenativelly přihlásit se pomocí jiná ověřovací služba jako google (ve výchozím nastavení vypnutá).
5. Zavřete prohlížeč zastavení ladicího programu a vrátíte se do sady Visual Studio.
6. Otevřete soubor **AuthConfig.cs** umístěna ve složce **aplikace\_Start** složky.
7. Poslední řádek registrace klienta Google pro odebrání komentář *OAuth* ověřování.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Všimněte si, že můžete snadno povolit ověřování pomocí libovolné služby OpenID nebo OAuth, jako je Facebook, Twitter, Microsoft, atd.
8. Stisknutím klávesy **F5** ke spuštění řešení a přejděte na stránku pro přihlášení.
9. Vyberte **Google** službu pro přihlášení.

    ![Výběr protokolu ve službě](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Výběr protokolu ve službě*
10. Přihlaste se pomocí účtu Google.
11. Povolit server (localhost) k načtení informací z účtu Google.
12. Nakonec budete muset zaregistrovat na webu, který přidružíte k účtu Google.

   ![Přidružení účtu Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Přidružení účtu Google*
13. Zavřete prohlížeč zastavení ladicího programu a vrátíte se do sady Visual Studio.
14. Teď si projděte řešení rezervovat několik nových funkcí zavedených v architektuře ASP.NET MVC 4 v šabloně projektu.

   ![Šablony projektu aplikace ASP.NET MVC 4 Internet](whats-new-in-aspnet-mvc-4/_static/image9.png "šablonu projektu ASP.NET MVC 4 Internetové aplikace")

   *Šablona projektu ASP.NET MVC 4 Internetové aplikace*

   - **HTML 5 značek**

       Procházejte šablony zobrazení a zjistěte, nový motiv značek.

       ![Nové šablony, pomocí syntaxe Razor a HTML5 značek About.cshtml. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Novou šablonu, pomocí syntaxe Razor a HTML5 značek About.cshtml.")

       *Nové šablony pomocí značky Razor a HTML5 (About.cshtml).*
   - **Aktualizované knihovny jazyka JavaScript**

       Výchozí šablony ASP.NET MVC 4 nyní zahrnuje KnockoutJS a architektura MVVM jazyka JavaScript, která vám umožní vytvářet bohaté s velmi rychlou odezvou webové aplikace pomocí jazyků JavaScript a HTML. Stejně jako v MVC3, jQuery a knihovny uživatelského rozhraní jQuery jsou také zahrnuté v architektuře ASP.NET MVC 4.

     > [!NOTE]
     > Můžete získat další informace o knihovně KnockOutJS v tomto odkazu: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Kromě toho se dozvíte o jQuery a uživatelské rozhraní jQuery v [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Úloha 2 – zkoumání šablona mobilních aplikací

ASP.NET MVC 4 usnadňuje vývoj webů pro mobilní zařízení a prohlížečů tablet. Tato šablona má stejnou strukturu aplikace jako šablony internetovou aplikaci (Všimněte si, že kódu kontroleru je prakticky shodný), ale jeho styl byl upraven správně vykreslovat v mobilním zařízení s dotykovým ovládáním.

1. Vyberte **soubor | Nové | Projekt** příkazu nabídky. V **nový projekt** dialogového okna, vyberte **Visual C# | Web** šablony v levém podokně stromové struktury a zvolte **webové aplikace ASP.NET MVC 4.** Pojmenujte projekt **PhotoGallery.Mobile**, vyberte umístění (nebo ponechte výchozí nastavení), vyberte &quot;přidat do řešení&quot; a klikněte na tlačítko **OK**.
2. V **nového projektu ASP.NET MVC 4** dialogového okna, vyberte **aplikaci Mobile** šablony projektu a klikněte na tlačítko **OK**. Ujistěte se, že jste zvolili jako zobrazovací modul Razor.

    ![Vytvoření nové aplikace ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "vytvoření nové aplikace ASP.NET MVC 4 Mobile")

    *Vytvoření nové aplikace ASP.NET MVC 4 Mobile*
3. Nyní budete moci prozkoumat řešení a podívejte se na některé z nových funkcích zavedených v architektuře ASP.NET MVC 4 šablonu řešení pro mobilní zařízení:

    - **jQuery Mobile knihovny**

        Šablona projektu mobilní aplikace obsahuje mobilní knihovny jQuery, což je open source knihovna pro kompatibilitu mobilní prohlížeče. jQuery Mobile postupném rozšiřování platí pro mobilní prohlížeče, které podporují šablon stylů CSS a JavaScript. Postupném rozšiřování umožňuje všechny prohlížeče zobrazíte základní obsah na webové stránce, zatímco umožňuje procesorově nejvýkonnější prohlížeče zobrazí formátovaný obsah. Soubory jazyka JavaScript a CSS součástí jQuery Mobile styl nápovědy mobilní prohlížeče a přizpůsobit obsah na obrazovce bez provedení jakékoli změny v kódu stránky.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *knihovny jQuery mobile zahrnuty v šabloně*
    - **HTML5 podle značky**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Šablony mobilních aplikací pomocí značek HTML5, (Login.cshtml a index.cshtml)*
4. Stisknutím klávesy **F5** ke spuštění řešení.
5. Otevřít **Windows Phone 7 emulátor**.
6. Na obrazovce start telefonu otevřete Internet Explorer. Podívejte se na adresu URL začínali desktopovou aplikaci a přejděte na tuto adresu URL z telefonu (třeba `http://localhost:[PortNumber]/`).
7. Nyní máte možnost zadat přihlašovací stránky nebo se podívejte o stránce. Všimněte si, že styl webové stránky je založen na novou aplikaci Metro pro mobilní zařízení. Šablona projektu ASP.NET MVC 4 je správně zobrazen na mobilních zařízeních, ujistěte se, že jsou všechny prvky na stránce viditelný a povolený. Všimněte si, že jsou dostatečně velké na to, aby kliknul nebo klepnul na odkazy v hlavičce.

    ![Projekt šablony stránky v mobilním zařízení](whats-new-in-aspnet-mvc-4/_static/image14.png "projektu šablony stránky v mobilním zařízení")

    *Šablona stránky projektu v mobilním zařízení*
8. Nová šablona také používá **zobrazení metaznačku**. Většina mobilních prohlížečů definovat šířku okna virtuálního prohlížeče nebo &quot;zobrazení&quot;, což je větší než skutečná šířka mobilních zařízení. To umožňuje mobilní prohlížeče pro zobrazení celé webové stránky uvnitř virtuální zobrazení. **Zobrazení metaznačku** umožňuje vývojářům nastavena na šířku, výšku a škálování oblasti prohlížeče na mobilních zařízeních **.** Šablony ASP.NET MVC 4 pro mobilní aplikace nastaví zobrazení na šířku zařízení (&quot;width = šířka zařízení&quot;) v šabloně rozložení (*Views\Shared\_Layout.cshtml*) tak, aby všechny stránky budou mít jejich zobrazení nastavena na šířku obrazovce zařízení. Všimněte si, že zobrazení metaznačku nedojde ke změně výchozího zobrazení prohlížeče.
9. Otevřít  **\_Layout.cshtml**, který je umístěn v **zobrazení | Sdílené** složky a komentář metaznačku zobrazení. Spuštění aplikace, není-li již otevřít a prohlédněte si rozdíly.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Lokality po komentování metaznačku zobrazení](whats-new-in-aspnet-mvc-4/_static/image15.png "lokality po metaznačku zobrazení komentářů")

*Lokality po metaznačku zobrazení komentářů*
10. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.
11. Zrušením komentáře u metaznačku zobrazení.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Úloha 3 – pomocí adaptivního vykreslování

V této úloze se dozvíte jinou metodu k vykreslení stránky správně na mobilních zařízeních a webových prohlížečů ve stejnou dobu bez jakéhokoli přizpůsobení. Zobrazení metaznačku jste už použili s k podobnému účelu. Nyní bude vyhovovat jinou metodu výkonné: *adaptivního vykreslování*.

Adaptivní vykreslování je technika, která používá **dotazy na média CSS3** přizpůsobit styl použitý pro stránku. Dotazy na média definovat podmínky uvnitř šablony stylů CSS styly pod určitou podmínku seskupení. Pouze v případě, že je podmínka pravdivá, styl platí na objekty deklarované.

Flexibilitu, kterou adaptivního vykreslování technika umožňuje jakéhokoli přizpůsobení pro zobrazení webu na různých zařízeních. Můžete definovat tolik styly, které chcete na jedinou šablonu stylů bez psaní kódu logiku zvolte styl. Proto je velmi úhledné způsob přizpůsobení stylů stránky, protože snižuje množství duplicitním kódem a logiku pro účely vykreslování. Na druhé straně byste zvýšit využití šířky pásma, jak může nepatrně zvětšit velikost souborů šablon stylů CSS.

Pomocí adaptivního vykreslování techniku, bude váš web **zobrazovat správně, bez ohledu na to, v prohlížeči.** Nicméně byste měli zvážit, jestli zatížení šířky pásma, navíc je důležité zajistit.

> [!NOTE]
> Je základní formát dotazu médií: @media \[obor: všechny | handheld | tisk | projekce | obrazovky\] ([hodnota: vlastnosti] a... [: hodnota vlastnosti])


Příklady dotazů média: &gt;  <strong>@media všechny a (maximální šířka: 1000px) a (minimální šířka: 700px) {}:</strong> pro všechna rozlišení mezi 700px a 1000px.

> <strong>@media obrazovky a (minimální šířka: 400 px) a (maximální šířka: 700px) {…}:</strong> pouze pro obrazovky. Rozlišení musí být v rozsahu od 400 do 700px.
> 
> <strong>@media Ruční a (minimální šířka: 20em), obrazovky a (minimální šířka: 20em) {…}:</strong> kapesní zařízení (mobile a zařízení) a obrazovky se zadáváním. Minimální šířka musí být větší než 20em.
> 
> Další informace o tomto najdete na [webu W3C](http://www.w3.org/TR/css3-mediaqueries/).


Můžete se teď si projděte fungování adaptivní vykreslování, zlepšení čitelnosti ASP.NET MVC 4 výchozí šablony webu.

1. Otevřít **PhotoGallery.sln** řešení vytvořené v úloze 1 a vyberte **Fotogalerie** projektu. Stisknutím klávesy **F5** ke spuštění řešení.
2. Změna velikosti prohlížeče šířku, nastavení systému windows polovina nebo méně než čtvrtletí původní velikosti. Všimněte si, co se stane s položkami v hlavičce: některé prvky se už nebude viditelná oblast záhlaví.
3. Otevřít <strong>Site.css</strong> souboru v Průzkumníku řešení v sadě Visual Studio, umístěný ve <strong>obsahu</strong> složky projektu. Stisknutím klávesy <strong>CTRL + F</strong> otevřete integrované hledání sady Visual Studio a zapisovat <strong>@media</strong> vyhledejte <strong>šablon stylů CSS media query</strong>.

    Podmínka media dotaz definovaný v této šabloně funguje takto: když je velikost okna prohlížeče níže **850 px**, pravidel šablon stylů CSS použitý jsou ty, které jsou definované v tomto bloku média.

    ![Multimediální dotaz pro vyhledání](whats-new-in-aspnet-mvc-4/_static/image16.png "multimediální dotaz pro vyhledání")

    *Multimediální dotaz pro vyhledání*
4. Nahraďte hodnotu maximální šířka atributu nastavit v 850 px s **10px**, aby bylo možné zakázat adaptivního vykreslování a stiskněte klávesu **stisknutím CTRL + S** a uložte změny. Vraťte se do prohlížeče a stisknutím klávesy **CTRL + F5** aktualizovat stránku, provedené změny. Všimněte si rozdílů v obou stránek při úpravě šířku okna.

    ![Na levé straně stránky použití @media stylu, v pravém styl vynecháte](whats-new-in-aspnet-mvc-4/_static/image17.png "na levé straně stránky použití @media stylu, v pravém styl je vynechán.")

    <em>Na levé straně stránky aplikuje @media stylu, v pravém styl je vynechán.</em>

    Teď se podíváme, co se stane, že na mobilních zařízeních:

    ![Na levé straně stránky použití @media stylu, v pravém styl vynecháte](whats-new-in-aspnet-mvc-4/_static/image18.png "na levé straně stránky použití @media stylu, v pravém styl je vynechán.")

    <em>Na levé straně stránky aplikuje @media stylu, v pravém styl je vynechán.</em>

    I když si všimnete, že změny při vykreslování stránky ve webovém prohlížeči nejsou velmi důležité při používání mobilních zařízení budou rozdíly zřetelnější. Na levé straně na obrázku vidíme, že vlastní styl zlepšila čitelnost.

    V mnoha scénářích další, což usnadňuje použití podmíněného stylu na web a řešení běžných problémů styl úhledné přístupu lze použít adaptivní vykreslování.

    Zobrazení metaznačku a CSS media dotazy se netýkají technologie ASP.NET MVC 4, tak můžete využít tyto funkce v jakékoli webové aplikaci.
5. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Cvičení 2: Vytvoření webové aplikace Fotogalerie

V tomto cvičení bude fungovat u aplikace Fotogalerie pro zobrazení fotografií. Začnete pomocí šablony projektu ASP.NET MVC 4 a pak je přidána funkce k načtení fotografie ze služby a jejich zobrazení na domovské stránce.

V následujícím cvičení budete aktualizovat toto řešení k vylepšení způsob, jakým se zobrazí na mobilních zařízeních.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Úloha 1 – vytvoření služby Mock fotografií

V této úloze vytvoříte model služby fotografii k načtení obsahu, který se zobrazí v galerii. Chcete-li to provést, přidáte nový kontroler, který jednoduše vrátí soubor JSON s daty každou fotografii.

1. Otevřít **sady Visual Studio** Pokud ještě není otevřen.
2. Vyberte **soubor | Nové | Projekt** příkazu nabídky. V **nový projekt** dialogového okna, vyberte **Visual C# | Web** šablony v levém podokně stromové struktury a zvolte **webové aplikace ASP.NET MVC 4.** Pojmenujte projekt **Fotogalerie**, vyberte umístění (nebo ponechte výchozí nastavení) a klikněte na tlačítko **OK**. Alternativně můžete pokračovat v práci z existující ASP.NET MVC 4 **internetovou aplikaci** řešení z **cvičení 1** a přejděte na další krok.
3. V **nového projektu ASP.NET MVC 4** dialogové okno, vyberte **internetovou aplikaci** šablony projektu a klikněte na tlačítko **OK**. Ujistěte se, že jste vybrali jako zobrazovací modul Razor.
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **aplikace\_Data** složky vašeho projektu a vyberte **přidat | Existující položka**. Přejděte **Source\Assets\App\_Data** složka tohoto testovacího prostředí a přidejte **Photos.json** souboru.
5. Vytvořte nový kontroler s názvem **PhotoController**. Chcete-li to provést, klikněte pravým tlačítkem na **řadiče** složku, přejděte na **přidat** a vyberte **Kontroleru.** Úplný název kontroleru, nechat **prázdný kontroler MVC** šablonu a klikněte na tlačítko **přidat**.

    ![Přidávání PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "přidání PhotoController")

    *Přidávání PhotoController*
6. Nahradit **Index** metoda s tímto **Galerie** akce a vrátí obsah ze souboru JSON, které jste nedávno přidali do projektu.

    (Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex02 - Galerie*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Stisknutím klávesy **F5** pro spuštění řešení a pak přejděte na následující adresu URL k otestování služby imitaci fotografii: `http://localhost:[port]/photo/gallery` (hodnota [port] závisí na aktuální port, ve kterém se spustila aplikace). Požadavek na tuto adresu URL, načtěte obsah **Photos.json** souboru.

    ![Testování služby imitaci fotografii](whats-new-in-aspnet-mvc-4/_static/image20.png "testování služby imitaci fotografií")

    *Testování služby imitaci fotografií*

Ve skutečné implementaci může používat [rozhraní ASP.NET Web API](../../../../web-api/index.md) pro implementaci této služby Fotogalerie. ASP.NET Web API je architektura, která usnadňuje sestavování služeb HTTP, které jsou poskytovány širokému spektru klientů, včetně prohlížečů a mobilních zařízení. ASP.NET Web API je ideální platformu pro sestavování aplikací RESTful v rozhraní .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Úloha 2 – zobrazení galerie fotografií

V této úloze aktualizujte domovskou stránku galerie fotografií zobrazit pomocí imitaci službu, kterou jste vytvořili v prvním úkolem v tomto cvičení. Přidáte soubory modelu a aktualizace zobrazení galerie.

1. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.
2. Vytvořte **fotografii** třídy v **modely** složky. Chcete-li to provést, klikněte pravým tlačítkem na **modely** složky, vyberte **přidat** a klikněte na tlačítko **třídy**. Nastavte název na **Photo.cs** a klikněte na tlačítko **přidat**.
3. Přidat následující členy do **fotografii** třídy.

    (Fragment - kódu *modelu ASP.NET MVC 4 Lab – Ex02 – Photo*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Otevřít **HomeController.cs** soubor **řadiče** složky.
5. Přidejte následující příkazy using.

    (Fragment - kódu *direktivy using HomeController Lab – Ex02 – ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Aktualizace **Index** použití akce **HttpClient** k načtení dat Galerie a pak použít **JavaScriptSerializer** k deserializaci na model zobrazení.

    (Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex02 - Index akce*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Otevřít **Index.cshtml** soubor umístěný **Views\Home** složky a nahradit veškerý obsah následujícím kódem.

    Tento kód projde všechny fotky načtených z této služby a zobrazí ho na Neseřazený seznam.

    (Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex02 – Photo List*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **obsahu** složky vašeho projektu a vyberte **přidat | Existující položka**. Přejděte **Source\Assets\Content** složka tohoto testovacího prostředí a přidejte **Site.css** souboru. Musíte potvrdit jeho nahrazení. Pokud máte **Site.css** otevřít soubor, budete muset potvrdit také znovu načíst soubor.
9. Otevřete Průzkumníka souborů a zkopírujte celý **fotografie** složky umístěna ve složce **Source\Assets** složka tohoto testovacího prostředí do kořenové složky vašeho projektu v Průzkumníku řešení.
10. Spusťte aplikaci. Měli byste vidět na domovské stránce zobrazení fotografií v galerii.

    ![Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotogalerie")

    *Fotogalerie*
11. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Cvičení 3: Přidání podpory pro mobilní zařízení

Jednou z klíčových aktualizací v architektuře ASP.NET MVC 4 je podpora pro vývoj pro mobilní zařízení. V tomto cvičení bude prozkoumání architektury ASP.NET MVC 4 nových funkcí pro mobilní aplikace tím, že rozšíří Fotogalerie řešení, které jste vytvořili v předchozím cvičení.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Úloha 1 – instalace jQuery Mobile v aplikaci ASP.NET MVC 4

1. Otevřít **začít** řešení nachází v **zdroj/EX3.-MobileSupport/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **Konzola správce balíčků** kliknutím **nástroje** &gt; **Správce balíčků knihoven** &gt; **Správce balíčků Konzola** nabídky.

    ![Otevření konzole Správce balíčků NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "otevření konzole Správce balíčků NuGet")

    *Otevření konzole Správce balíčků NuGet*
3. V konzole Správce balíčků spusťte následující příkaz k instalaci **jQuery.Mobile.MVC** balíčku.

    jQuery Mobile je open source knihovna pro vytváření dotykového webového uživatelského rozhraní. Balíček NuGet jQuery.Mobile.MVC obsahuje pomocné rutiny pro použití s aplikací ASP.NET MVC 4 jQuery Mobile.

    > [!NOTE]
    > Spuštěním následujícího příkazu budete stahovat jQuery.Mobile.MVC knihovny z Nuget.

    ODP.

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Tento příkaz nainstaluje jQuery Mobile a některé soubory pomocné rutiny, včetně následujících:

    - **Zobrazení/Shared/\_Layout.Mobile.cshtml**: je optimalizované pro menší obrazovky založené na mobilní rozložení jQuery. Když na webu obdrží žádost z mobilní prohlížeče, nahradí původní rozložení (\_Layout.cshtml) s touto položkou.
    - Komponenta přepínači zobrazení: se skládá z **zobrazení/Shared/\_ViewSwitcher.cshtml** částečné zobrazení a **ViewSwitcherController.cs** kontroleru. Tato součást se zobrazí odkaz na mobilních prohlížečích umožňuje uživatelům přepínat desktopová verze stránky.  
        ![Fotogalerie projektu s podporou mobilních](whats-new-in-aspnet-mvc-4/_static/image23.png "Fotogalerie projekt mobilní podpora")

        *Projekt Fotogalerie mobilní podpora*
4. Zaregistrujte mobilní sady. Chcete-li to provést, otevřete **Global.asax.cs** a přidejte následující řádek.

    (Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex03 – zaregistrovat mobilní sady*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Spuštění aplikace klasické pracovní plochy webového prohlížeče.
6. Otevřít **emulátoru pro Windows Phone 7,** umístěné v **nabídky Start | Všechny programy | Windows Phone SDK 7.1 | Emulátor Windows Phone.**
7. Na obrazovce start telefonu otevřete Internet Explorer. Podívejte se na adresu URL, kde je aplikace spuštěna a přejděte na tuto adresu URL s prohlížeči telefonu (třeba `http://localhost:[PortNumber]/`).

    Můžete si všimnout, že vaše aplikace bude vypadat jinak v emulátoru Windows Phone, protože jQuery.Mobile.MVC vytvořil nové prostředky ve vašem projektu, který zobrazení optimalizovány pro mobilní zařízení.

    Všimněte si, že zpráva v horní části telefon, zobrazuje odkaz, který se přepne do zobrazení plochy. Kromě toho  **\_Layout.Mobile.cshtml** rozložení, který byl vytvořen balíček nainstalujete patří jiné rozložení v aplikaci.

    > [!NOTE]
    > Zatím není žádný odkaz se můžete vrátit k mobilní zobrazení. Se zahrne v pozdějších verzích.

    ![Mobilní zobrazení fotografií Galerie domovské stránky](whats-new-in-aspnet-mvc-4/_static/image24.png "mobilní zobrazení fotografií Galerie domovské stránky")

    *Mobilní zobrazení fotografií Galerie domovské stránky*
8. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Úloha 2 – Vytvoření mobilní zobrazení

V této úloze vytvoříte mobilní verzi zobrazení indexu s obsahem přizpůsobený pro lepší appareance v mobilních zařízeních.

1. Kopírovat **Views\Home\Index.cshtml** zobrazení a vložte ho, chcete-li vytvořit kopii, přejmenujte nový soubor, který **Index.Mobile.cshtml**.
2. Otevřít nové vytvoření **Index.Mobile.cshtml** zobrazení a nahraďte existující &lt;ul&gt; značky s tímto kódem. Tím se aktualizuje &lt;ul&gt; značka s jQuery mobilních dat poznámky k použití mobilní motivů z jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Všimněte si, že:
    > 
    > - **Role dat** atribut nastaven na **listview** vykreslí seznamu pomocí ListView – styly.
    > 
    > - **Data inset** atribut nastavený na hodnotu true se zobrazí v seznamu s zakulacený okraj a okraj.
    > 
    > - **Filtr dat** atribut nastaven na **true** vygeneruje vyhledávací pole.
    > 
    > Další informace o konvencích pro jQuery Mobile v dokumentaci k projektu: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.
4. Přepněte **emulátoru Windows Phone** a aktualizovat lokalitu. Všimněte si, že nový vzhled a chování seznamu galerie, stejně jako nové pole hledání umístěný v horní části. Potom zadejte do vyhledávacího pole slovo (například **Tulipány**) spustíte hledání v galerii fotografií.

    ![Galerie pomocí filtrování styl listview](whats-new-in-aspnet-mvc-4/_static/image25.png "Galerie pomocí filtrování styl listview")

    *Styl listview pomocí filtrování Galerie*

    Souhrnně řečeno, jste použili předpisu Mobilizer zobrazení k vytvoření kopie zobrazení indexu s &quot;mobilní&quot; příponu. Tato přípona znamená na ASP.NET MVC 4, že každý požadavek vygenerované z mobilního zařízení bude používat tuto kopii indexu. Kromě toho aktualizována mobilní verzi zobrazení indexu pro použití jQuery Mobile pro zlepšení vzhledu a chování webu v mobilním zařízení.
5. Vraťte se zpět do sady Visual Studio a otevřete **Site.Mobile.css** umístěna ve složce **obsahu** složky.
6. Opravte umístění název fotografie zobrazí na pravé straně bitové kopie. Chcete-li to provést, přidejte následující kód k **Site.Mobile.css** souboru.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.
8. Přepněte zpět **emulátoru Windows Phone** a aktualizovat lokalitu. Všimněte si, že název fotografie správně je nyní umístěn.

    ![Název umístěn na pravé straně obrázku](whats-new-in-aspnet-mvc-4/_static/image26.png "název umístěn na pravé straně bitové kopie")

    *Název na pravé straně bitové kopie*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Úloha 3 – motivy jQuery Mobile

Každý rozložení a widget jQuery Mobile navržené s ohledem na nové šablony stylů CSS framework objektově orientované, která umožňuje použít kompletní jednotné vizuálním návrhem motiv k webům a aplikacím.

jQuery Mobile výchozí motiv zahrnuje 5 vzorník, která jsou uvedena písmena (a, b, c, d e) pro rychlou referenci.

V této úloze budete aktualizovat mobilní rozložení chcete použít jiný motiv než výchozí.

1. Přepněte zpět do sady Visual Studio.
2. Otevřít  **\_Layout.Mobile.cshtml** soubor umístěný ve **Views\Shared**.
3. Najděte prvek div s nastavena na role dat &quot;stránky&quot; a aktualizovat **data-theme** k &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.
5. Aktualizace v lokalitě **emulátoru Windows Phone** a Všimněte si, že nové schéma barvy.

    ![Mobilní rozložení se schématem jinou barvu](whats-new-in-aspnet-mvc-4/_static/image27.png "mobilní rozložení se schématem odlišnou barvou")

    *Mobilní rozložení se schématem odlišnou barvou*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Úloha 4 – součást přepínači zobrazení a prohlížeče přepsání funkce

Konvence pro webové stránky optimalizovaných pro mobilní zařízení se přidat odkaz, jejichž text je něco jako zobrazení plochy nebo režim celý web, který umožňuje uživatelům přepínat desktopová verze stránky. Balíček jQuery.Mobile.MVC obsahuje ukázkový **přepínači zobrazení** komponenty pro tento účel použít v  **\_Layout.Mobile.cshtml** zobrazení.

![Odkazu k přepnutí na zobrazení plochy](whats-new-in-aspnet-mvc-4/_static/image28.png "odkaz k přepnutí na zobrazení plochy")

*Odkaz k přepnutí na zobrazení plochy*

Přepínání zobrazení používá novou funkci nazvanou **přepsání prohlížeče**. Tato funkce umožňuje aplikaci zacházet se žádostmi, jako kdyby kdyby přicházely z jiného prohlížeče (uživatelského agenta) než ten, který ve skutečnosti přicházejí z.

V této úloze bude prozkoumat ukázková implementace přepínači zobrazení přidal jQuery.Mobile.MVC a nový prohlížeč přepsání funkce v architektuře ASP.NET MVC 4.

1. Přepněte zpět do sady Visual Studio.
2. Otevřít  **\_Layout.Mobile.cshtml** zobrazení umístěna ve složce **Views\Shared** složky a Všimněte si, že součást přepínači zobrazení je odkazováno jako částečné zobrazení.

    ![Mobilní rozložení pomocí komponenty přepínači zobrazení](whats-new-in-aspnet-mvc-4/_static/image29.png "mobilní rozložení pomocí komponenty přepínači zobrazení")

    *Mobilní rozložení pomocí komponenty přepínači zobrazení*
3. Otevřít  **\_ViewSwitcher.cshtml** částečné zobrazení.

    Částečné zobrazení používá novou metodu **ViewContext.HttpContext.GetOverriddenBrowser()** určit původ na webový požadavek a zobrazit na příslušný odkaz přejděte buď do zobrazení Desktop nebo Mobile.

    **GetOverridenBrowser** vrátí metoda **HttpBrowserCapabilitiesBase** instanci, která odpovídá pro uživatelského agenta, který je aktuálně nastavený pro žádost (skutečné nebo přepsané). Tato hodnota slouží k získání vlastností, jako **IsMobileDevice**.

    ![Částečné zobrazení ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher částečného zobrazení")

    *ViewSwitcher částečného zobrazení*
4. Otevřít **ViewSwitcherController.cs** třídy umístěny v **řadiče** složky. Podívejte se na tuto funkci SwitchView akci je volán na odkaz v komponentě ViewSwitcher a Všimněte si nových metod objektu HttpContext.

    - **HttpContext.ClearOverridenBrowser()** metoda odebere každého přepsaného uživatelského agenta pro aktuální požadavek.
    - **HttpContext.SetOverridenBrowser()** metoda přepíše hodnotu skutečného uživatelského agenta žádosti pomocí zadaného uživatelského agenta.  
        ![Kontroler ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Kontroleru")  
*ViewSwitcher Kontroleru*

        Přepíše prohlížeče je funkce jádra ASP.NET MVC 4, což je také k dispozici i v případě, že není nainstalovaný balíček jQuery.Mobile.MVC. Ale tuto funkci ovlivňuje pouze zobrazení, rozložení a částečného zobrazení a nemá žádnou z funkcí, které jsou závislé na objektu Request.Browser vliv.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Úloha 5: Přidání přepínači zobrazení v zobrazení plochy

V této úloze budete aktualizovat rozložení pro počítače zahrnout přepínači zobrazení. To vám umožní mobilní uživatelé se vrátíte do zobrazení mobilní, při procházení zobrazení plochy.

1. Aktualizace v lokalitě **emulátoru Windows Phone**.
2. Klikněte na **zobrazení plochy** odkazu v horní části v galerii. Všimněte si, že není žádná přepínači zobrazení v zobrazení plochy tak, aby umožnily že vrátit mobilní zobrazení.
3. Vraťte se zpět do sady Visual Studio a otevřete  **\_Layout.cshtml** zobrazení.
4. Najít v části přihlášení a vložit volání pro vykreslení  **\_ViewSwitcher** částečné zobrazení níže  **\_LogOnPartial** částečné zobrazení. Poté stiskněte **stisknutím CTRL + S** a uložte změny.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Stisknutím klávesy **stisknutím CTRL + S** a uložte změny.
6. Aktualizujte stránku v emulátoru Windows Phone a dvakrát klikněte na obrazovce přiblížit. Všimněte si, že na domovské stránce se teď zobrazí **mobilní zobrazení** odkaz, který od mobilních se přepne do zobrazení plochy.

    ![Zobrazit přepínání zobrazení v zobrazení plochy](whats-new-in-aspnet-mvc-4/_static/image32.png "přepínači zobrazení se zobrazují v zobrazení plochy")

    *Přepínání zobrazení se zobrazují v zobrazení plochy*
7. Znovu přejděte do zobrazení mobilních a přejděte do <strong>o</strong> stránky (http://localhost[port] / Home/o). Všimněte si, že i v případě, že jste ještě nevytvořili pohledu About.Mobile.cshtml, o stránka se zobrazí pomocí mobilní rozložení (\_Layout.Mobile.cshtml).

    ![O stránku](whats-new-in-aspnet-mvc-4/_static/image33.png "o stránce")

    *O stránku*
8. Nakonec otevřete web ve webovém prohlížeči klientů. Všimněte si, že žádná z předchozích aktualizací má vliv na zobrazení plochy.

    ![Zobrazení plochy Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image34.png "Fotogalerie zobrazení plochy")

    *Zobrazení plochy Fotogalerie*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Krok 6 – Vytvoření nového režimů zobrazení

Nová funkce režimy zobrazení umožní aplikaci vyberte zobrazení v závislosti na prohlížeči, který generuje požadavek. Například pokud prohlížeč pro stolní počítač požadavků na domovskou stránku, aplikace se vrátí **Views\Home\Index.cshtml** šablony. Potom, pokud mobilního prohlížeče požadavků na domovskou stránku, aplikaci vrátí **Views\Home\Index.mobile.cshtml** šablony.

V této úloze vytvoříte vlastní rozložení pro zařízení iPhone a budete muset simulovat žádosti v zařízení iPhone. K tomuto účelu můžete použít buď emulátor nebo simulátor pro zařízení iPhone (jako je [Electric simulátor Mobile](http://www.electricplum.com/)) nebo v prohlížeči pomocí doplňků, které upravují uživatelského agenta. Pokyny o tom, jak nastavit v prohlížeči Safari identifikační řetězec prohlížeče pro emulaci Iphonu najdete v tématu [jak chcete, aby Safari, které předstírají, že je aplikace Internet Explorer](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison blogu.

**Všimněte si, že tato úloha je volitelné a můžete pokračovat v testovacím prostředí bez ale jeho spuštění.**

1. V sadě Visual Studio, stiskněte klávesu **SHIFT** + **F5** chcete zastavit ladění aplikace.
2. Otevřít **Global.asax.cs** a přidejte následující příkaz using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Přidejte následující zvýrazněný kód do aplikace\_začátek metody.

    (Fragment - kódu *ASP.NET MVC 4 Lab – Ex03 – iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Jste si zaregistrovali nový **DefaultDisplayMode s názvem &quot;iPhone&quot;**, v rámci statických **DisplayModeProvider.Instance.Modes** statické seznamu, bude hledána jednotlivých příchozích požadavků. Pokud příchozí požadavek obsahuje řetězec &quot;iPhone&quot;, ASP.NET MVC najdete zobrazení, jejichž název obsahuje &quot;iPhone&quot; příponu. Parametr 0 označuje, jak se konkrétní nový režim; například toto zobrazení je konkrétnější než Obecné &quot;.mobile&quot; pravidlo, které odpovídá požadavky mobilních zařízení.

Jakmile tento kód se spustí, když prohlížeč zařízení iPhone vygeneruje žádost, bude aplikace používat **Views\Shared\\_Layout.iPhone.cshtml** rozložení, které vytvoříte v dalších krocích.

> [!NOTE]
> Tento způsob testování požadavku pro iPhone je jednodušší pro účely ukázky a nemusí fungovat podle očekávání pro každý řetězec uživatelského agenta pro iPhone (pro příklad testu je velká a malá písmena).

4. Vytvoření kopie  **\_Layout.Mobile.cshtml** soubor **Views\Shared** složku a přejmenujte ji na &quot; **\_Layout.iPhone.csthml**&quot;.
5. Otevřít  **\_Layout.iPhone.csthml** jste vytvořili v předchozím kroku.
6. Najít div element s atribut data-role nastaven na **stránky** a změňte **data-theme** atribut &quot; **a**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Nyní máte 3 rozložení v aplikaci ASP.NET MVC 4:

1. **\_Layout.cshtml**: výchozí rozložení se používá u stolních počítačů.
2. **\_Layout.Mobile.cshtml**: výchozí rozložení pro mobilní zařízení.
3. **\_Layout.iPhone.cshtml**: specifické rozložení pro zařízení iPhone, pomocí jiné barevné schéma k odlišení od \_Layout.mobile.cshtml.
7. Stisknutím klávesy **F5** ke spuštění aplikace a procházení webu v **emulátoru Windows Phone**.
8. Otevřít **simulátoru Iphonu** (naleznete v tématu [dodatku C](#AppendixC) pokyny o tom, jak nainstalovat a nakonfigurovat simulátor pro zařízení iPhone) a přejděte na web příliš. Všimněte si, že každý phone používá konkrétní šablonu.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Použití různá zobrazení pro jednotlivá mobilní zařízení*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Cvičení 4: Použití asynchronní řadiče

Microsoft .NET Framework 4.5 zavádí nové funkce jazyků C# a Visual Basic a poskytuje novou platformu pro asynchronii v programování rozhraní .NET. Tento nový foundation umožňuje asynchronní programování podobný – a přibližně stejně jednoduché jako - synchronní programování. Nyní budete moci napsat metody asynchronní akce v architektuře ASP.NET MVC 4 s využitím **AsyncController** třídy. Můžete použít asynchronní akce metody pro dlouho běžící, procesor vázán požadavky. Tím je zabráněno zablokování webového serveru z provádění práce během zpracování požadavku. Třída AsyncController se obvykle používá pro dlouhého volání webové služby.

V tomto cvičení vysvětluje základy tohoto asynchronní operace v architektuře ASP.NET MVC 4. Pokud se chcete dozvědět více o, podívejte se na následující článek: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Úloha 1 – implementace asynchronní kontroler

1. Otevřít **začít** řešení nachází v **zdroj/Ex4-Async/počáteční/** složky. V opačném případě může nadále používat **End** řešení získat provedením předchozím cvičení.

   1. Pokud jste otevřeli zadaných **začít** řešení, budete muset stáhnout některé chybějící balíčky NuGet než budete pokračovat. Chcete-li to provést, klikněte na tlačítko **projektu** nabídky a vybereme **spravovat balíčky NuGet**.
   2. V **spravovat balíčky NuGet** dialogového okna, klikněte na tlačítko **obnovení** aby bylo možné stáhnout chybějící balíčky.
   3. Nakonec sestavte řešení kliknutím **sestavení** | **sestavit řešení**.

      > [!NOTE]
      > Jednou z výhod pomocí nástroje NuGet je, že není nutné dodávat všechny knihovny ve vašem projektu, zmenšení velikosti projektu. Pomocí nástroje NuGet zadáním verze balíčku v souboru Packages.config, budete moct stáhnout požadované knihovny při prvním spuštění projektu. To je důvod, proč budete muset projít tyto kroky po otevření existujícího řešení z tohoto testovacího prostředí.
2. Otevřít **HomeController.cs** třídy z **řadiče** složky.
3. Přidejte následující příkaz using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Aktualizace **HomeController** třída dědí **AsyncController**. Kontrolery, které jsou odvozeny z AsyncController povolit technologii ASP.NET ke zpracování asynchronních žádostí a mohou stále služby akce synchronní metody.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Přidat **asynchronní** – klíčové slovo do **Index** metoda a nastavte ji typ vrácené **úloh&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > **Asynchronní** – klíčové slovo je jednou z nových klíčových slov poskytuje rozhraní .NET Framework 4.5, sděluje kompilátoru, že tato metoda obsahuje asynchronní kód. A **úloh** objekt představuje asynchronní operaci, která mohou být dokončeny v určitém okamžiku v budoucnu.
6. Nahradit **klienta. GetAsync()** volání s použitím úplné asynchronní verze – klíčové slovo await, jak je znázorněno níže.

    (Fragment - kódu *GetAsync Lab – Ex04 – ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > V předchozí verzi, kterou jste používali **výsledek** vlastnost z **úloh** objekt blokovat vlákno, dokud se výsledek se vrátí (verze synchronizace %).
    > 
    > Přidávání **await** – klíčové slovo instruuje kompilátor, aby asynchronní čekání na úlohu vrácená z volání metody. To znamená, že zbytek kódu se spustí jako zpětné volání až po dokončení očekávané metody. Další věc a Všimněte si je, že není potřeba změnit váš bloku try-catch – aby bylo možné tuto práci: bez další práce pomocí obslužnou rutinu poskytovaného rámcem bude stále zachycena výjimky, ke kterým dochází v pozadí nebo v popředí.
7. Změňte kód a pokračujte asynchronní provádění nahrazením řádky s novým kódem, jak je znázorněno níže

    (Fragment - kódu *ReadAsStringAsync Lab – Ex04 – ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Spusťte aplikaci. Uvidíte žádné větší změny, ale váš kód neblokuje vlákno z fondu vláken provádění lepší využití serverových prostředků a zlepšení výkonu.

    > [!NOTE]
    > Další informace o nových funkcích asynchronního programování v testovacím prostředí &quot; **asynchronní programování v rozhraní .NET 4.5 s C# a Visual Basic** &quot; součástí této školicí sady Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Úloha 2 – zpracování vypršení časových limitů s tokeny zrušení

Metody asynchronní akce, které vracejí instance může také podporovat vypršení časových limitů. V této úloze budete aktualizovat Index metoda kód pro zpracování vypršení časového limitu scénář pomocí tokenu zrušení.

1. Vraťte se zpět do sady Visual Studio a stiskněte klávesu **SHIFT + F5** chcete zastavit ladění.
2. Přidejte následující příkaz k použití **HomeController.cs** souboru.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Akce indexu přijímat aktualizace **CancellationToken** argument.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Aktualizace **GetAsync** volání předat token zrušení.

    (Fragment - kódu *SendAsync Lab – Ex04 – ASP.NET MVC 4 s CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Uspořádání *Index* metodou **hodnota vlastnosti AsyncTimeout** atribut nastaven na hodnotu 500 milisekund a **HandleError** nakonfigurovaný pro zpracování atributu  **TaskCanceledException** přesměrováním na **vypršel časový limit** zobrazení.

    (Fragment - kódu *architektury ASP.NET MVC 4 Lab – Ex04 – atributy*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Otevřít **PhotoController** třídy a aktualizace **Galerie** metoda zpoždění spuštění 1000 milisekund (1 sekunda) pro simulaci dlouho běžící úloha.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Otevřít **Web.config** souboru a povolit tak, že přidáte následující element vlastních chyb.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Vytvoření nového zobrazení v **Views\Shared** s názvem **vypršel časový limit** a použijte výchozí rozložení. V Průzkumníku řešení klikněte pravým tlačítkem myši **Views\Shared** a pak zvolte položku **přidat | Zobrazení**.

    ![Použití různá zobrazení pro jednotlivá mobilní zařízení,](whats-new-in-aspnet-mvc-4/_static/image36.png "pomocí různá zobrazení pro jednotlivá mobilní zařízení")

    *Použití různá zobrazení pro jednotlivá mobilní zařízení*
9. Aktualizace **vypršel časový limit** zobrazit obsah, jak je znázorněno níže.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Spusťte aplikaci a přejděte do kořenové adresy URL. Jak jste přidali **Thread.Sleep** 1000 milisekund, získáte k vypršení časového limitu, vygenerovaná **hodnota vlastnosti AsyncTimeout** atribut a zachytit pomocí **HandleError** atribut.

    ![Výjimka časového limitu zpracování](whats-new-in-aspnet-mvc-4/_static/image37.png "zpracována výjimka časového limitu")

    *Výjimka časového limitu zpracování*

> [!NOTE]
> Kromě toho můžete nasadit tuto aplikaci následující weby Windows Azure [příloha D: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V tomto praktická-testovací prostředí jste jsme zaznamenali některé nové funkce v architektuře ASP.NET MVC 4. Projednat následující pojmy:

- Využijte výhod rozšíření, která chcete šablony – včetně projektu ASP.NET MVC nové šablony projektů mobilních aplikací
- Použít atribut zobrazení HTML5 a CSS media dotazy ke zlepšení zobrazení na mobilních zařízeních
- Použít jQuery Mobile pro progresivní vylepšení a vytváření dotykového webového uživatelského rozhraní
- Vytvořit zobrazení specifické pro mobilní zařízení
- Chcete-li přepnout mezi mobilními a desktopovými zobrazení v aplikaci použijte komponentu přepínači zobrazení
- Vytvoření asynchronní řadiče pomocí podpory úkolu

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Příloha A: používání fragmentů kódu

Pomocí fragmentů kódu máte všechny kód, který je třeba na dosah ruky. Testovací prostředí dokumentu zjistíte přesně kdy je můžete využít, jak je znázorněno na následujícím obrázku.

![Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu](whats-new-in-aspnet-mvc-4/_static/image38.png "pomocí sady Visual Studio fragmenty kódu pro vložení kódu do projektu")

*Používání fragmentů kódu sady Visual Studio pro vložení kódu do projektu*

***Chcete-li přidat fragment kódu pomocí klávesnice (C# jenom)***

1. Umístěte kurzor, kam chcete vložit kód.
2. Začněte psát název fragmentu kódu (bez mezer nebo pomlčky).
3. Podívejte se na jako IntelliSense zobrazí odpovídající názvy fragmenty kódu.
4. Vyberte správný fragment kódu (nebo pokračujte v psaní dokud nebude vybraný celý fragment název).
5. Stiskněte klávesu Tabulátor dvakrát pro vložení fragmentu do umístění kurzoru.

![Začněte psát název fragmentu kódu](whats-new-in-aspnet-mvc-4/_static/image39.png "začněte psát název fragmentu kódu")

*Začněte psát název fragmentu kódu*

![Stiskněte klávesu Tab k vybrání fragmentu zvýrazněné](whats-new-in-aspnet-mvc-4/_static/image40.png "stisknutím klávesy Tab k výběru zvýrazněné fragment kódu")

*Stiskněte klávesu Tab k výběru zvýrazněné fragment kódu*

![Stisknutím klávesy Tab znovu a fragment kódu se rozbalí](whats-new-in-aspnet-mvc-4/_static/image41.png "znovu stisknutím klávesy Tab a fragment kódu se rozbalí.")

*Stisknutím klávesy Tab znovu a fragment kódu se rozbalí.*

***Chcete-li přidat fragment kódu pomocí myši (C#, Visual Basic a XML)***

1. Klikněte pravým tlačítkem na místo, kam chcete vložit fragment kódu.
2. Vyberte **Vložit fragment** následovaný **Moje fragmenty kódu**.
3. Kliknutím na vyberte relevantní fragment kódu ze seznamu.

![Klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment](whats-new-in-aspnet-mvc-4/_static/image42.png "klikněte pravým tlačítkem, ve které chcete vložit fragment kódu a vyberte Vložit fragment")

*Klikněte pravým tlačítkem na, ve které chcete vložit fragment kódu a vyberte Vložit fragment*

![Vyberte si relevantní fragment kódu ze seznamu, kliknutím na](whats-new-in-aspnet-mvc-4/_static/image43.png "relevantní fragment kódu ze seznamu vyberte kliknutím na")

*Vyberte si relevantní fragment kódu ze seznamu, kliknutím na*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Příloha B: instalaci sady Visual Studio Express 2012 pro Web

Můžete nainstalovat **Microsoft Visual Studio Express 2012 pro Web** nebo jiném &quot;Express&quot; verzí pomocí **[instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Postupujte podle následujících pokynů vás provede kroky potřebné k instalaci *Visual studio Express 2012 pro Web* pomocí *instalačního programu webové platformy Microsoft*.

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>Visual Studio Express 2012 pro Web se sadou Windows Azure SDK</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace sady Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instalace sady Visual Studio Express")

    *Instalace sady Visual Studio Express*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena.](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.
8. Chcete-li spustit nástroj Visual Studio Express for Web, přejděte **Start** obrazovky a začít psát &quot; **VS Express**&quot;, klikněte na **VS Express for Web** dlaždice.

    ![VS Express for Web dlaždice](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express for Web dlaždice*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Příloha C: instalace služby WebMatrix 2 a simulátoru Iphonu

Provoz vašeho webu v Iphonu s Simulovaná zařízení můžete použít rozšíření prostředí WebMatrix &quot;Electric simulátor Mobile pro iPhone&quot;. Navíc můžete nakonfigurovat stejnou příponu ke spuštění simulátoru ze sady Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Úloha 1 – instalace služby WebMatrix 2

1. Přejděte na [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Případně, pokud jste již nainstalovali instalačního programu webové platformy, můžete otevřít a vyhledejte produkt &quot; <em>WebMatrix 2</em>&quot;.
2. Klikněte na **nainstalovat**. Pokud nemáte **instalačního programu webové platformy** budete přesměrováni na stáhněte a nainstalujte ji jako první.
3. Jednou **instalačního programu webové platformy** je otevřený, klikněte na tlačítko **nainstalovat** spustit instalační program.

    ![Instalace služby WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "instalace služby WebMatrix 2")

    *Instalace služby WebMatrix 2*
4. Čtení všech produktů licence a podmínky a klikněte na tlačítko **souhlasím** pokračujte.

    ![Přijetí podmínek licence](whats-new-in-aspnet-mvc-4/_static/image50.png "přijetí podmínek licence")

    *Přijetí podmínek licence*
5. Počkejte na dokončení procesu stahování a instalaci.

    ![Průběh instalace](whats-new-in-aspnet-mvc-4/_static/image51.png "průběh instalace")

    *Průběh instalace*
6. Až instalace skončí, klikněte na tlačítko **Dokončit**.

    ![Instalace byla dokončena](whats-new-in-aspnet-mvc-4/_static/image52.png "instalace byla dokončena.")

    *Instalace byla dokončena.*
7. Klikněte na tlačítko **ukončovací** zavřete instalačního programu webové platformy.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Úloha 2 – instalace rozšíření simulátoru Iphonu

1. Spustit **WebMatrix** a otevřít všechny existující webovou stránku nebo vytvořte novou.
2. Klikněte na tlačítko **spustit** tlačítko **Domů** pásu karet a vyberte **přidat nový**.

    ![Přidání nové rozšíření prostředí WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "přidání nové rozšíření nástroje WebMatrix")

    *Přidání nové rozšíření nástroje WebMatrix*
3. Vyberte **iPhone simulátor** a klikněte na tlačítko **nainstalovat**.

    ![Procházení rozšíření nástroje WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "rozšíření nástroje WebMatrix procházení")

    *Procházení rozšíření nástroje WebMatrix*
4. Podrobnosti balíčku, klikněte na tlačítko **nainstalovat** pokračujte v instalaci rozšíření.

    ![iPhone simulátor rozšíření](whats-new-in-aspnet-mvc-4/_static/image55.png "rozšíření simulátoru Iphonu")

    *rozšíření simulátoru Iphonu*
5. Přečtěte si a přijměte smlouvu EULA rozšíření.

    ![Rozšíření prostředí WebMatrix smlouva EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "rozšíření prostředí WebMatrix smlouvy EULA")

    *Rozšíření prostředí WebMatrix smlouvy EULA*
6. Teď můžete spouštět webu ze služby WebMatrix pomocí možnosti simulátoru Iphonu.

    ![Spuštění pomocí Iphonu](whats-new-in-aspnet-mvc-4/_static/image57.png "spuštění pomocí Iphonu")

    *Spuštění pomocí Iphonu*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Úloha 3 – konfigurace sady Visual Studio 2012 ke spuštění simulátoru Iphonu

1. Otevřete **Visual Studio 2012** a otevřít libovolný web, nebo vytvořte nový projekt.
2. Klikněte na šipku dolů na panelu spuštění a vyberte **procházet s**.

    ![Procházení s](whats-new-in-aspnet-mvc-4/_static/image58.png "nastavit prohlížeče")

    *Nastavit prohlížeče*
3. V &quot;procházet s&quot; dialogového okna, klikněte na tlačítko **přidat**.
4. V &quot;přidat Program&quot; dialogového okna, použijte následující hodnoty:

   - <strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (aktualizovat cestu odpovídajícím způsobem)</em>
   - **Argumenty**: &quot;1&quot;
   - **Popisný název**: simulátoru Iphonu

     ![Přidat program](whats-new-in-aspnet-mvc-4/_static/image59.png "přidat program")

     *Přidat program pro procházení s*
5. Klikněte na tlačítko **OK** a zavřete dialogová okna.
6. Nyní budete moci spouštět webové aplikace v simulátoru Iphonu ze sady Visual Studio 2012.

    ![Procházení pomocí Iphonu simulátor](whats-new-in-aspnet-mvc-4/_static/image60.png "procházet pomocí simulátoru Iphonu")

    *Procházení s simulátoru Iphonu*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Příloha D: publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

Tento dodatek se ukazují, jak vytvořit nový web z portálu správy Windows Azure a publikovat aplikace, kterou jste získali podle testovacího prostředí, využít Webdeploy funkce publikování ve Windows Azure k dispozici.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Úloha 1 – Vytvoření nového webu z Windows webu Azure Portal

1. Přejděte [Windows Azure Management Portal](https://manage.windowsazure.com/) a přihlaste se pomocí přihlašovacích údajů Microsoft spojených s vaším předplatným.

    > [!NOTE]
    > Windows Azure můžete zadarmo hostovat 10 webů ASP.NET a pak škálujte podle rozšiřujícího se provozu. Můžete se zaregistrovat [tady](http://aka.ms/aspnet-hol-azure).

    ![Přihlaste se k portálu Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Přihlaste se k portálu Windows Azure")

    *Přihlaste se k portálu pro správu Azure Windows*
2. Klikněte na tlačítko **nový** na panelu příkazů.

    ![Vytvoření nového webu](whats-new-in-aspnet-mvc-4/_static/image62.png "vytváření nového webu")

    *Vytvoření nového webu*
3. Klikněte na tlačítko **Compute** | **webu**. Potom vyberte **rychlé vytvoření** možnost. Zadejte adresu URL k dispozici pro nový web a klikněte na tlačítko **vytvořit web**.

    > [!NOTE]
    > Hostitel pro webovou aplikaci spuštěnou v cloudu, který může řídit a spravovat je web Windows Azure. Možnost rychle vytvořit můžete nasadit hotové webové aplikace na Windows Azure web z mimo portál. Nezahrnuje kroky pro vytvoření databáze.

    ![Vytvoření nového webu pomocí metody rychlého vytvoření](whats-new-in-aspnet-mvc-4/_static/image63.png "vytváření nového webu pomocí metody rychlého vytvoření")

    *Vytvoření nového webu pomocí metody rychlého vytvoření*
4. Počkejte, dokud nové **webu** se vytvoří.
5. Po vytvoření webové stránky, klikněte na odkaz v části **URL** sloupce. Zkontrolujte, jestli funguje nový web.

    ![Na nový web](whats-new-in-aspnet-mvc-4/_static/image64.png "přechodu na nový web")

    *Procházení na nový web*

    ![Spuštění webu](whats-new-in-aspnet-mvc-4/_static/image65.png "spuštění webové stránky")

    *Spuštění webové stránky*
6. Přejděte zpět na portál a klikněte na název webové stránky v části **název** sloupec, který se zobrazí na stránkách správy.

    ![Otevřete správu webových stránek](whats-new-in-aspnet-mvc-4/_static/image66.png "otevřete správu webových stránek")

    *Otevřete správu webových stránek*
7. V **řídicí panel** stránce v části **rychlý přehled** klikněte na tlačítko **stáhnout profil publikování** odkaz.

    > [!NOTE]
    > *Profil publikování* obsahuje všechny informace požadované pro publikování webových aplikací na webu Windows Azure pro každou metodu povoleno publikování. Profil publikování obsahuje adresy URL, přihlašovací údaje uživatele a databázové řetězce požadované k připojení a ověřování na základě jednotlivých koncových bodů, u kterých je povolena metoda publikace. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** a **Microsoft Visual Studio 2012** podporují čtení publikační profily k automatizaci konfigurace z těchto programů pro publikování webových aplikací na Windows Azure websites.

    ![Stahování webové stránky publikovat profil](whats-new-in-aspnet-mvc-4/_static/image67.png "stahování webové stránky profil publikování")

    *Na webu stažení profilu publikování*
8. Stáhněte si soubor profilu publikování do vhodného umístění. Dále v tomto cvičení uvidíte jak publikovat webovou aplikaci na weby Windows Azure ze sady Visual Studio pomocí tohoto souboru.

    ![Ukládání souboru profilu publikování](whats-new-in-aspnet-mvc-4/_static/image68.png "ukládá se profil publikování")

    *Ukládá se profil publikování*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Úloha 2 – konfigurování serveru databáze

Pokud vaše aplikace využívá SQL Server databáze, budete muset vytvořit server služby SQL Database. Pokud chcete nasadit jednoduchou aplikaci, která nepoužívá SQL Server může tuto úlohu přeskočit.

1. Pro uložení databáze aplikace budete potřebovat databázi SQL serveru. Servery SQL Database můžete zobrazit ze svého předplatného na portálu správy Windows Azure na **databází Sql** | **servery** | **serveru Řídicí panel**. Pokud nemáte server vytvořili, můžete vytvořit jednu **přidat** tlačítko na panelu příkazů. Poznamenejte si **název serveru a adresu URL, správce přihlašovací jméno a heslo**, jak je použijete v další úkoly. Nevytvářet databáze, protože vytvoří se v pozdější fázi.

    ![Řídicí panel serveru SQL Database](whats-new-in-aspnet-mvc-4/_static/image69.png "řídicího panelu serveru SQL Database")

    *Řídicí panel serveru SQL Database*
2. V dalším úkolem budete testovat připojení k databázi ze sady Visual Studio z tohoto důvodu je nutné zahrnout místní IP adresa serveru seznamu **povolené IP adresy**. Chcete-li to mohli udělat, klikněte na tlačítko **konfigurovat**, vyberte IP adresu z **aktuální IP adresa klienta** a vložte ho na **počáteční IP adresa** a **Koncová IP adresa** textová pole a klikněte na tlačítko ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) tlačítko.

    ![Přidat IP adresu klienta](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Přidat IP adresu klienta*
3. Jednou **IP adresa klienta** je přidat do povolených IP adres klikněte na tlačítko na **Uložit** potvrďte provedené změny.

    ![Potvrzení změn](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Potvrzení změn*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Úloha 3 – publikování aplikace ASP.NET MVC 4 pomocí nasazení webu

1. Vraťte se do řešení ASP.NET MVC 4. V **Průzkumníka řešení**, klikněte pravým tlačítkem na webový projekt a vyberte **publikovat**.

    ![Publikování aplikace](whats-new-in-aspnet-mvc-4/_static/image73.png "publikování aplikace")

    *Publikování na webu*
2. Importujte profil publikování, který jste uložili v první úloze.

    ![Import profilu publikování](whats-new-in-aspnet-mvc-4/_static/image74.png "import profilu publikování")

    *Import publikačního profilu*
3. Klikněte na tlačítko **ověřit připojení**. Po dokončení ověření klikněte na tlačítko **Další**.

    > [!NOTE]
    > Ověření bylo dokončeno, jakmile se zobrazí zelené zaškrtnutí vedle tlačítka ověřit připojení.

    ![Ověřuje se připojení](whats-new-in-aspnet-mvc-4/_static/image75.png "ověřuje se připojení")

    *Ověřuje se připojení*
4. V **nastavení** stránce v části **databází** klikněte na tlačítko vedle textového pole připojení k databázi (to znamená **objekt DefaultConnection**).

    ![Konfigurace nasazení webu](whats-new-in-aspnet-mvc-4/_static/image76.png "konfigurace nasazení webu")

    *Konfigurace nasazení webu*
5. Konfigurace připojení k databázi následujícím způsobem:

   - V **název serveru** zadejte vaše pomocí adresy URL databáze SQL serveru *tcp:* předponu.
   - V **uživatelské jméno** zadejte vaše přihlašovací jméno správce serveru.
   - V **heslo** zadejte heslo pro přihlašovací jméno správce serveru.
   - Zadejte nový název databáze, například: *MVC4SampleDB*.

     ![Konfigurace cílový připojovací řetězec](whats-new-in-aspnet-mvc-4/_static/image77.png "konfigurace cílový připojovací řetězec")

     *Konfigurace cílový připojovací řetězec*
6. Pak klikněte na tlačítko **OK**. Po zobrazení výzvy k vytvoření databáze klikněte na **Ano**.

    ![Vytvoření databáze](whats-new-in-aspnet-mvc-4/_static/image78.png "vytvoření řetězce databáze")

    *Vytvoření databáze*
7. Připojovací řetězec, který budete používat pro připojení k databázi SQL ve službě Windows Azure se zobrazí v textovém poli výchozí připojení. Pak klikněte na tlačítko **Další**.

    ![Připojovací řetězec odkazující na SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "připojovací řetězec odkazující na SQL Database")

    *Připojovací řetězec odkazující na SQL Database*
8. V **ve verzi Preview** klikněte na **publikovat**.

    ![Publikování webové aplikace](whats-new-in-aspnet-mvc-4/_static/image80.png "publikování webové aplikace")

    *Publikování webové aplikace*
9. Až se proces publikování dokončí, otevře se váš výchozí prohlížeč publikovaného webu.

    ![Publikování aplikace do Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "publikování aplikace do Windows Azure")

    *Aplikace publikovaná do Windows Azure*
