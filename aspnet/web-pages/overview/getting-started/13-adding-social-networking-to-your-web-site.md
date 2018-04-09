---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Přidání sociálních sítí k rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs
author: tfitzmac
description: Tato kapitola vysvětluje, jak integrovat svůj web se služby sociálních sítí. V této kapitole se dozvíte, jak uživatelům záložku/propojení webu umožňuje...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Přidání sociální sítě na webové stránky ASP.NET lokalit (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak přidat sociálních sítí odkazy pro Facebook, Twitter, Reddit a Digg na stránky na webu technologie ASP.NET Web Pages (Razor) a jak se zahrnuje Twitter kanály, Xbox hráči karty a Gravatar obrázků.
> 
> Získáte informace:
> 
> - Jak uživatelé mohou záložku/propojení vaší lokality.
> - Postup přidání informační kanál sítě Twitter.
> - Postup přidání Facebooku **jako** tlačítko stránky.
> - Jak vykreslení obrázků Gravatar.com.
> - Postupy: zobrazení karty hráči Xbox na váš web.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - ASP.NET webové pomocné knihovny (balíček NuGet)
>   
> 
> V tomto kurzu funguje taky s ASP.NET Web Pages 3, s výjimkou částí, které používají pomocné knihovny ASP.NET Web.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Propojování vašeho webu na weby sociálních sítí

Pokud uživatelé jako něco na svém webu, často chtějí sdílet s přátel. Můžete nastavit to snadno zobrazením glyfů (ikon), které lze kliknout a sdílet na stránku v rámci rozhraní Digg, Reddit, Facebook, Twitter nebo podobné weby.

Chcete-li zobrazit tyto glyfů, přidejte `LinkSharecode` pomocná na stránku. Uživateli, kteří navštíví stránku kliknutím na jednotlivé glyfů. Pokud budou mít účet s sociálních sítí lokality, se pak zveřejnit odkaz na stránku na dané lokalitě.

![Obrázek 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste ještě nepřidali jej - vytvořit stránku s názvem *ListLinkShare.cshtml* a přidejte Následující kód:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    V tomto příkladu když `LinkShare` spustí pomocné rutiny, název stránky se předá jako parametr, který pak předá název stránky na web sociálních sítí. Však může předávat v libovolný řetězec, který má být. Tento příklad také určuje, které weby sociálních sítí pro zahrnutí do seznamu. Můžete určit weby sociálních sítí, které jsou relevantní pro váš web.
2. Spustit *ListLinkShare.cshtml* stránku v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.)
3. Klikněte na tlačítko glyf jeden z webů, které jste si pro přihlášení. Odkaz vás zavede na stránku na webu vybrané sociálních sítí, kde můžete sdílet odkaz. Například pokud kliknete na odkaz Reddit, jste jste udělali na `submit to reddit` na Reddit webu.

     ![Obrázek 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Přidání Twitter kanálu

Informace o používání Pomocník Twitter, který je kompatibilní s aktuální verzí API serveru Twitter najdete v tématu [Pomocník Twitter](../ui-layouts-and-themes/twitter-helper.md). Tento příklad ukazuje, jak psát vlastní pomocné rutiny, můžete snadno opakovaně použít kód z mnoha stránky.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Zobrazení Facebook &quot;jako&quot; tlačítko

V některých případech je nejlepší možnost získat kód přímo z poskytovateli sociální sítě, namísto spoléhání na pomocné rutiny. To platí hlavně pokud poskytovateli sociální sítě aktualizace její možnosti rychleji, než se aktualizuje pomocné rutiny.

Chcete-li přidat funkce Facebook (jako je například tlačítko) na váš web, můžete načíst fragmenty kódu z [developers.facebook.com](https://developers.facebook.com/) lokality. Na webu Facebook používat své nástroje pro generování fragment kódu, které se týkají vašeho webu.

Následující zvýrazněný kód je kód, který byl získán z tlačítko jako nástroj na webu developers.facebook.com. Je nutné zadat vlastní ID aplikace.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Vykreslování obrázku Gravatar

A *Gravatar* ( &quot;globálně rozpoznaný miniatury&quot;) je obrázek, který lze použít na více webech jako miniatury &#8212; tedy obrázek, který představuje můžete. Například můžete Gravatar identifikaci osoby v příspěvek ve fóru v blogu komentář a tak dále. (Můžete zaregistrovat vlastní Gravatar na webu Gravatar na [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Pokud chcete pro zobrazení obrázků vedle jména nebo e-mailové adresy uživatelů na vašem webu, můžete použít Gravatar pomocné rutiny.

V tomto příkladu používáte jeden Gravatar, který představuje sami. Další způsob použití Gravatar je umožnit osoby, zadejte jeho adresu Gravatar po jejich registraci na váš web. (Můžete naučit umožníte uživatelům zaregistrovat v [členství na web webových stránek ASP.NET a přidání zabezpečení](https://go.microsoft.com/fwlink/?LinkId=202904).) Potom vždy, když zobrazíte informace pro tohoto uživatele, můžete přidat Gravatar právě kde zobrazit uživatelské jméno.

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.
2. Vytvořit novou webovou stránku s názvem *Gravatar.cshtml*.
3. Přidejte následující kód do souboru: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml` Metoda zobrazí obrázek Gravatar na stránce. Chcete-li změnit velikost bitové kopie, můžete zahrnout číslo jako druhý parametr. Výchozí velikost je 80. Čísla menší než 80 zkontrolujte bitovou kopii menší. Čísla větší než 80 zvětšit bitovou kopii.
4. V `Gravatar.GetHtml` metody, nahraďte `<Your Gravatar account here>` s e-mailovou adresu, který používáte pro váš účet Gravatar. (Pokud nemáte účet Gravatar, můžete e-mailovou adresu osoby, který.)
5. Spusťte stránku v prohlížeči. Na stránce se zobrazí dvě bitové kopie Gravatar pro e-mailovou adresu, kterou jste zadali. Druhý image je menší než první. 

    ![Obrázek 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Zobrazení hráči karty Xbox

Když uživatelé hry pro Microsoft Xbox online, každý uživatel má jedinečný identifikátor. Statistiky jsou v každé player ve formě hráči kartu, která zobrazuje jejich reputaci, hráči skóre a nedávno přehrávají hry. Pokud jste Xbox hráči, můžete zobrazit hráči karty na stránkách ve vaší lokalitě pomocí `GamerCard` pomocné rutiny.

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.
2. Vytvořit novou stránku s názvem *XboxGamer.cshtml* a přidejte následující kód.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Můžete použít `GamerCard.GetHtml` vlastnosti k určení aliasu pro hráči karty, který se má zobrazit.
3. Spusťte stránku v prohlížeči. Na stránce se zobrazí karta hráči Xbox, který jste zadali.

    ![Obrázek 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
