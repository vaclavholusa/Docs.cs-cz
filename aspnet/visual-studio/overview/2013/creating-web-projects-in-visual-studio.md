---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: "Vytváření ASP.NET – webové projekty v sadě Visual Studio 2013 | Microsoft Docs"
author: tdykstra
description: "Toto téma vysvětluje, že možnosti pro vytváření webových projektů ASP.NET v sadě Visual Studio 2013 s aktualizací 3 tady jsou některé nové funkce pro vývoj c webové..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: aacae7a9ccf483b21d3c6796c0411d558fa3c75b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Vytváření webové projekty ASP.NET v sadě Visual Studio 2013
====================
podle [tní Dykstra](https://github.com/tdykstra)

> Toto téma popisuje možnosti pro vytvoření webové projekty ASP.NET v sadě Visual Studio 2013 s aktualizací 3
> 
> Tady jsou některé nové funkce pro vývoj webů porovnání s předchozími verzemi sady Visual Studio:
> 
> - Jednoduché uživatelského rozhraní pro vytváření projektů v rámci této nabídky [podporu pro více rozhraní ASP.NET](#add) (webových formulářů, MVC a webového rozhraní API).
> - [ASP.NET Identity](#indauth), nový systém členství technologie ASP.NET, který funguje stejně ve všech rozhraní ASP.NET a funguje s webového hostingu softwaru než služby IIS.
> - Použití [Bootstrap](#bootstrap) zajistit přizpůsobivý možnosti návrhu a motivů.
> - Nové funkce pro webové formuláře, který používá k budou k dispozici pouze na MVC, jako například [test automatického vytváření projektu](#testproj) a [šablona stránky intranetu](#winauth).
> 
> Informace o tom, jak vytvořit webové projekty pro Azure Cloud Services nebo Azure Mobile Services najdete v tématu [Začínáme s Azure Cloud Services a technologií ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) a [vytvoření žebříček aplikace pomocí Azure Mobile Services .NET Back-end](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Požadavky

Tento článek se týká [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) s [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) nainstalována.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projekty webových aplikací versus webové projekty

ASP.NET vám dává na výběr mezi dva druhy webové projekty: *webové projekty aplikací* a *webové projekty*. Doporučujeme, abyste projekty webových aplikací pro nový vývoj a tento článek se týká pouze na projekty webových aplikací. Další informace najdete v tématu [projekty webových aplikací a webových projektů v sadě Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) na webu MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Přehled vytváření projektu webové aplikace

Následující kroky ukazují, jak vytvořit webového projektu:

1. Klikněte na tlačítko **nový projekt** v **spustit** stránce nebo v **souboru** nabídky.
2. V **nový projekt** dialogové okno, klikněte na tlačítko **webové** v levém podokně a **webové aplikace ASP.NET** v prostředním podokně.

    ![Dialogové okno Nový projekt](creating-web-projects-in-visual-studio/_static/image1.png)

    Můžete zvolit **cloudu** v levém podokně vytvořit [Azure Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile Service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), nebo [webové úlohy Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Toto téma nezahrnuje tyto šablony.
3. V pravém podokně klikněte na **přidat službu Application Insights do projektu** zaškrtávací políčko, pokud chcete stav a sledování využití pro vaši aplikaci. Další informace najdete v tématu [sledování výkonu ve webových aplikacích](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Zadejte projektu **název**, **umístění**a další možnosti a pak klikněte na tlačítko **OK**.

    **Nový projekt ASP.NET** otevře se dialogové okno.

    ![Dialogové okno Nový projekt](creating-web-projects-in-visual-studio/_static/image2.png)
5. Klikněte na šablonu.

    ![Vyberte šablonu](creating-web-projects-in-visual-studio/_static/image3.png)
6. Pokud chcete přidat podporu pro další rozhraní, které nejsou zahrnuty v šabloně, klikněte na příslušné políčko. (Ukazuje příklad, můžete přidat MVC nebo webového rozhraní API do projektu webové formuláře.)

    ![Přidání rozhraní](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Pokud chcete přidat projektu testů jednotek, klikněte na tlačítko **přidat testování částí**.

    ![Přidání testů jednotek](creating-web-projects-in-visual-studio/_static/image5.png)
8. Pokud chcete metodu ověřování jiný než šablony poskytuje ve výchozím nastavení, klikněte na tlačítko **změna ověřování**.

    ![Konfigurace ověřování tlačítko](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Konfigurace ověřování dialogové okno](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Vytvořit webovou aplikaci nebo virtuální počítač v Azure

Visual Studio obsahuje funkce, které usnadňují pracovat s Azure services pro hostování webových aplikací. Například můžete provést všechny tyto přímo na Visual Studio IDE:

- Vytvoření a Správa webové aplikace nebo virtuální počítače, které vaše aplikace zpřístupnit přes Internet.
- Zobrazit protokoly vytvořené aplikací při jeho spuštění v cloudu.
- Spusťte v režimu ladění vzdáleně, když aplikace běží v cloudu.
- Viiew a spravovat jinými službami Azure, jako jsou třeba databáze SQL.

Můžete [vytvoření účtu Azure](https://www.windowsazure.com/pricing/free-trial/) zdarma obsahující základní služby, jako jsou webové aplikace, a pokud jste odběratel MSDN můžete [aktivovat výhody](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) , získáte měsíční kredity směrem k další Azure služby. 

Ve výchozím nastavení **nový projekt ASP.NET** dialogové okno můžete vytvořit webovou aplikaci nebo virtuální počítač pro nového webového projektu. Pokud nechcete vytvořit novou webovou aplikaci nebo virtuální počítač, zrušte **hostitel v cloudu** zaškrtávací políčko.

![Vytvořit vzdálené prostředky](creating-web-projects-in-visual-studio/_static/image8.png)

Zaškrtněte políčko popisek může být **hostitel v cloudu** nebo **vytvořit vzdálené prostředky**, a v obou případech účinek je stejný. Pokud necháte políčko zaškrtnuto, Visual Studio vytvoří webovou aplikaci v Azure App Service ve výchozím nastavení. Rozevíracím seznamu můžete změnit tak, aby **virtuální počítač** Pokud dáváte přednost. Pokud jste již přihlášení do Azure, se zobrazí výzva k přihlašovacích údajů služby Azure. Po přihlášení, dialogové okno umožňuje nakonfigurovat prostředky, které vytvoří sada Visual Studio pro projekt. Následující obrázek znázorňuje dialogové okno pro webovou aplikaci; Pokud zvolíte možnost vytvořit virtuální počítač, zobrazí se různé možnosti.

![Konfigurace nastavení aplikace Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Další informace o tom, jak používat tento proces pro vytváření prostředků Azure najdete v tématu [Začínáme s Azure a ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) a [vytvoření virtuálního počítače pro webový server pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Zbývající část tohoto článku poskytuje další informace o dostupných šablon a jejich možnosti. V článku také zavádí Bootstrap, rozložení a motivů framework se používá v šablonách.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Visual Studio 2013 Web Project Templates

Visual Studio využívá šablony k vytvoření webové projekty. Šablona projektu můžete vytvořit soubory a složky v novém projektu, instalace balíčků NuGet a zadejte ukázkový kód pro základní funkční aplikaci. Šablony implementovat nejnovější webové standardy a jsou určeny k předvedení osvědčené postupy, jak používat technologie ASP.NET a také získáte přechod spustit na vytvoření vlastní aplikace.

Visual Studio 2013 poskytuje následující možnosti pro šablony webových projektů pro projekty, které cílí na rozhraní .NET 4.5 nebo novější verze rozhraní .NET Framework:

- [Prázdné šablony](#empty)
- [Šablony webových formulářů](#wf)
- [Šablony MVC](#mvc)
- [Šablona webového rozhraní API](#webapi)
- [Šablona pro jednu stránku aplikace](#spa)
- [Šablony Azure Mobile Service](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Visual Studio 2012 Templates](#vs2012)

Můžete taky nainstalovat rozšíření sady Visual Studio, který poskytuje [šablona pro síť Facebook](#facebook).

Informace o tom, jak vytvořit projektů cílených na rozhraní .NET 4 najdete v tématu [šablony sady Visual Studio 2012](#vs2012) dál v tomto tématu.

Informace o tom, jak vytvářet aplikace ASP.NET pro mobilních klientů najdete v tématu [Mobile podporu technologie ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Prázdné šablony

Prázdná šablona poskytuje úplné minimální složek a souborů pro webové aplikace ASP.NET, jako je soubor projektu (*.csproj* nebo. *vbproj*) a *Web.config* souboru. Můžete přidat podporu pro webové formuláře, MVC nebo webového rozhraní API pomocí zaškrtávacích políček v části **přidat složky a odkazy na základní:** popisek.

Pro prázdnou šablonu jsou k dispozici žádné možnosti ověřování. Funkce ověřování je implementovaná v ukázkové aplikace a prázdné šablonu nevytvoří ukázkovou aplikaci.

<a id="wf"></a>
### <a name="web-forms-template"></a>Šablony webových formulářů

Webové formuláře, které framework poskytuje následující funkce, které umožňují rychle vytvářet weby, které jsou bohaté v uživatelském rozhraní a data přístup k funkcím:

- WYSIWYG návrháře v sadě Visual Studio.
- Ovládací prvky serveru, které vykreslení HTML a že můžete přizpůsobit nastavení vlastností a stylů.
- Bohaté širokou ovládací prvky pro přístup k datům a data zobrazení.
- Model událostí, který zpřístupní události, které můžete naprogramovat stejně, jako by programu klientská aplikace, jako je například WPF.
- Automatické zachování stavu (data) mezi požadavky HTTP.

Obecně platí vytváření aplikací webových formulářů vyžaduje méně programování úsilí než vytvoření stejná aplikace pomocí rozhraní ASP.NET MVC. Ale webových formulářů není právě k rychlému vývoji aplikací. Existuje mnoho komplexní komerční aplikace a rozhraní postavená na webové formuláře.

Protože stránky s webovými formuláři a ovládací prvky na stránce automaticky tuto vygenerovat, velkou část značky, které je odesláno prohlížeči, nemáte druh jemně odstupňovanou kontrolu nad HTML, které nabízí technologie ASP.NET MVC. Deklarativní model pro konfiguraci stránky a ovládací prvky minimalizuje množství kódu, máte k zápisu, ale skryje některé chování HTML a HTTP. Například není vždy možné určit, jaké značek přesně může být generována ovládacího prvku.

Rozhraní webových formulářů není jít snadno ASP.NET MVC pro vývoj na základě vzory postupy, jako [vývoje řízeného testováním](http://en.wikipedia.org/wiki/Test-driven_development), [oddělené oblasti zájmu](http://en.wikipedia.org/wiki/Separation_of_concerns), [vzájemný ovládací prvek](http://en.wikipedia.org/wiki/Inversion_of_control), a [vkládání závislostí](http://en.wikipedia.org/wiki/Dependency_injection). Pokud chcete k zápisu kódu promítnou tímto způsobem, můžete; není právě automatické, protože je v rozhraní ASP.NET MVC. [ASP.NET Web Forms MVP](http://webformsmvp.com/) projektu ukazuje postup, který zajišťuje oddělení otázky a testovatelnosti při zachování rychlý vývoj, který byl postavený webové formuláře k poskytování. Microsoft SharePoint je založen na Web Forms MVP.

Webové formuláře šablona vytváří ukázkovou aplikaci webových formulářů, která používá [Bootstrap](#bootstrap) a zajistit tak funkce přizpůsobivý návrh a motivů. Následující obrázek znázorňuje domovské stránce.

![Webové formuláře šablony aplikace – Domovská stránka](creating-web-projects-in-visual-studio/_static/image10.png)

Další informace o webových formulářů, najdete v části [webových formulářů ASP.NET](https://asp.net/web-forms). Informace o šabloně webových formulářů jaké jsou pro vás najdete v tématu [vytvoření základní aplikace webových formulářů pomocí sady Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Šablony MVC

ASP.NET MVC byla navržena pro usnadnění na základě vzory vývoj postupy, jako [vývoje řízeného testováním](http://en.wikipedia.org/wiki/Test-driven_development), [oddělené oblasti zájmu](http://en.wikipedia.org/wiki/Separation_of_concerns), [inverzi řízení](http://en.wikipedia.org/wiki/Inversion_of_control), a [vkládání závislostí](http://en.wikipedia.org/wiki/Dependency_injection). Toto rozhraní umožňuje oddělení vrstvu obchodní logiky webové aplikace z jeho prezentační vrstvy. Rozdělením aplikace do modely (M), zobrazení (V) a řadiče (C) rozhraní ASP.NET MVC lze usnadňují správu složitých v větší aplikace.

S architekturou ASP.NET MVC práce více přímo s HTML a HTTP než v webových formulářů. Například webové formuláře může automaticky zachovat stav mezi požadavky HTTP, ale budete muset kód který explicitně v MVC. Výhodou modelu MVC je možné přepnout plnou kontrolu nad přesně činnosti vaší aplikace a jak se chová v prostředí webové. Nevýhodou je, že budete muset napsat další kód.

MVC byla navržená tak, aby rozšiřitelnost, poskytuje vývojářům power přizpůsobit rozhraní pro jejich potřebám aplikace. Kromě toho je k dispozici v části licenci OSI zdrojový kód rozhraní ASP.NET MVC.

Šablony MVC vytvoří ukázkovou aplikaci MVC 5, který používá [Bootstrap](#bootstrap) a zajistit tak funkce přizpůsobivý návrh a motivů. Následující obrázek znázorňuje domovské stránce.

![Ukázkovou aplikaci MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Další informace o MVC najdete v tématu [ASP.NET MVC](https://asp.net/mvc). Informace o tom, jak vyberte šablonu MVC 4 najdete v tématu [šablony sady Visual Studio 2012](#vs2012) dále v tomto článku.

<a id="webapi"></a>
### <a name="web-api-template"></a>Šablona webové rozhraní API

Šablona webového rozhraní API vytvoří ukázkové webové služby podle webového rozhraní API, včetně stránky nápovědy API založený na rozhraní MVC.

Rozhraní ASP.NET Web API je rozhraní, které usnadňuje sestavování služeb HTTP, které využity širokou škálou klientů včetně prohlížečů a mobilních zařízení. Rozhraní ASP.NET Web API je ideální platformu pro vytváření služeb RESTful v rozhraní .NET Framework.

Šablona webového rozhraní API vytvoří ukázkové webové služby. Následující ilustrace znázorňuje ukázkové stránky nápovědy.

![Stránka nápovědy webové rozhraní API](creating-web-projects-in-visual-studio/_static/image12.png)

![Stránka nápovědy webové rozhraní API pro získání rozhraní API](creating-web-projects-in-visual-studio/_static/image13.png)

Další informace o rozhraní Web API najdete v tématu [rozhraní ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Šablona jednostránkové aplikace

Jednu stránku aplikace (SPA) šablona vytváří ukázkovou aplikaci, která používá JavaScript, HTML 5 a [kódem KnockoutJS](http://knockoutjs.com/) na klientovi a ASP.NET Web API na serveru.

Pouze ověřování možnost SPA šablony je [jednotlivé uživatelské účty](#indauth).

Následující obrázek znázorňuje počáteční stav ukázkové aplikace, šablony SPA sestavení.

![SPA ukázkové aplikace](creating-web-projects-in-visual-studio/_static/image14.png)

Informace o tom, jak vytvořit aplikaci pomocí šablony SPA najdete v tématu [webového rozhraní API - externí ověřovací služby](../../../web-api/overview/security/external-authentication-services.md).

Další informace o jednostránkové aplikace ASP.NET a o další SPA šablony, které používají JavaScript rozhraní než kódem KnockoutJS najdete v následujících zdrojích informací:

- [ASP.NET jedné stránky aplikace](../../../single-page-application/index.md).
- [Princip funkce zabezpečení v šabloně SPA pro VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Jednostránkové aplikace: Vytvářet moderní a pohotově reagujících webové aplikace s technologií ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Šablona pro síť Facebook

Můžete nainstalovat [rozšíření sady Visual Studio, poskytující šablonu Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Tato šablona vytváří ukázkovou aplikaci, která je určená ke spuštění uvnitř webu Facebook. Ho je založený na rozhraní ASP.NET MVC a používá webového rozhraní API pro funkci aktualizace v reálném čase.

Žádné možnosti ověřování jsou k dispozici pro šablonu Facebook, protože aplikace Facebook spustit v rámci sítě Facebook a závisí na ověřování Facebook pro.

Další informace o aplikacích ASP.NET Facebook najdete v tématu [aktualizace rozhraní API sítě Facebook MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Visual Studio 2012 Templates

Dialogové okno Visual Studio 2013 webový projekt vytvoření neposkytuje přístup k některé šablony, které byly k dispozici v sadě Visual Studio 2012. Pokud chcete použít jednu z těchto šablon, můžete kliknout na Visual Studio 2012 uzlu v levém podokně dialogového okna Nový projekt sady Visual Studio.

![Šablony sady Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

**Visual Studio 2012** uzel umožňuje zvolit následující webové šablony, které ještě nemají přístup k v seznamu výchozích šablon pro Visual Studio 2013:

- Webové aplikace ASP.NET MVC 4
- Webovou aplikaci ASP.NET dynamické dat entity
- Ovládací prvek ASP.NET AJAX serveru
- Rozšiřující serverový ovládací prvek ASP.NET AJAX
- Ovládací prvek ASP.NET serveru

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Bootstrap v šablony webových projektů Visual Studio 2013

Šablony projektů Visual Studio 2013 pomocí [Bootstrap](http://getbootstrap.com/), rozložení a motivů rozhraní vytvořené Twitter. K poskytování přizpůsobivý návrh, což znamená, že rozložení se může dynamicky přizpůsobit velikost okna jiný prohlížeč používá Bootstrap CSS3. V okně prohlížeče široké například domovské stránce vytvořené šablony webových formulářů vypadá jako na následujícím obrázku:

![Webové formuláře šablony aplikace – Domovská stránka](creating-web-projects-in-visual-studio/_static/image16.png)

Vytvoření okna užší a vodorovně uspořádaných sloupce, přesuňte do svislém uspořádání:

![Uspořádání Bootstrap svislém sloupci](creating-web-projects-in-visual-studio/_static/image17.png)

Další úzké okno, vodorovné horní nabídce se zobrazí ikona, která můžete kliknout na rozšířit svisle orientované nabídky:

![Ikona Bootstrap nabídky](creating-web-projects-in-visual-studio/_static/image18.png)

![Zavedení svislé nabídky](creating-web-projects-in-visual-studio/_static/image19.png)

Funkce motivů Bootstrap je také můžete snadno ovlivňuje změnu aplikace vzhled a chování. Například můžete provést následující postup Změna motivu.

1. V prohlížeči přejděte na [http://Bootswatch.com](http://Bootswatch.com), zvolte jen motiv a pak klikněte na tlačítko **Stáhnout**. (Tato akce stáhne *bootstrap.min.css* ve výchozím nastavení; Pokud chcete prozkoumat kód šablon stylů CSS, získat *bootstrap.css* místo minifikovaný verzi.)
2. Zkopírujte obsah stažený soubor CSS.
3. V sadě Visual Studio vytvořte novou **list stylu** soubor s názvem *bootstrap theme.css* v *obsahu* složky a vložit stažené CSS kódu do ní.
4. Otevřete *aplikace\_Start/Bundle.config* a změňte *bootstrap.css* k *bootstrap theme.css*.

Spusťte projekt znovu a aplikace má nový vzhled. Následující obrázek znázorňuje účinku Amelia motivu:

![Zavedení Amelia motiv](creating-web-projects-in-visual-studio/_static/image20.png)

Mnoho Bootstrap motivy jsou k dispozici, verze volné a premium. Bootstrap také nabízí širokou škálu součásti uživatelského rozhraní, jako například [rozevírací seznamy](http://twitter.github.io/bootstrap/components.html#dropdowns), [tlačítko skupiny](http://twitter.github.io/bootstrap/components.html#buttonGroups), a [ikony](http://twitter.github.io/bootstrap/base-css.html#images). Další informace o Bootstrap najdete v tématu [webu Bootstrap](http://twitter.github.io/bootstrap/).

Pokud použijete návrháře webové formuláře v sadě Visual Studio, Všimněte si, že návrháře nepodporuje CSS3, takže nezobrazí přesně všechny účinky zavedení motivy nebo změny přizpůsobivé rozložení. Stránky webových formulářů, se však zobrazí správně, při zobrazení s prohlížečem.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Přidání podpory pro další rozhraní

Když vyberete šablonu, zaškrtněte políčko pro framework(s) používané šablony je automaticky vybrán. Například, pokud jste vybrali **webových formulářů** šablony, **webových formulářů** políčko a nejde ji zrušit.

![Vyberte šablonu](creating-web-projects-in-visual-studio/_static/image21.png)

![Přidání rozhraní](creating-web-projects-in-visual-studio/_static/image22.png)

Můžete vybrat zaškrtnutí políčka pro framework, který není zahrnutý v šabloně, chcete-li přidat podporu dané platformy při vytvoření projektu. Chcete-li například povolení používání webových formulářů *.aspx* stránky, pokud jste vybrali šablonu MVC, vyberte **webových formulářů** zaškrtávací políčko. Nebo pokud chcete povolit MVC při použití šablony webových formulářů, klikněte na tlačítko **MVC** zaškrtávací políčko. Přidání rozhraní umožňuje podporu návrhu, jakož i běhu. Například pokud přidáte podporu MVC do projektu webových formulářů, bude možné vygenerovat uživatelské rozhraní, kontrolery a zobrazení položku.

Pokud můžete kombinovat webových formulářů a MVC v projektu a povolit [přátelské adresy URL](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) v webových formulářů, může neočekávaná směrování problémy, kde jedna adresa URL má několik možných cílů. Tras, které jsou definovány nejprve bude mít přednost. Pokud máte například `Home` řadiče a *Home.aspx* stránky, `http://contoso.com/home` URL přejde na *Home.aspx* při volání `EnableFriendlyUrls` metoda před voláním `MapRoute`metoda v *soubor RouteConfig.cs*, nebo se na výchozí zobrazení pro stejnou adresu URL vašeho `Home` řadiče při volání `MapRoute` před `EnableFriendlyUrls`.

Přidání rozhraní nepřidá žádné funkce ukázkové aplikace. Například pokud přidáte webových formulářů podporovat, pokud jste vybrali šablonu MVC, ne *Default.aspx* se vytvoří soubor domovské stránky. Se přidají jenom složky, soubory a odkazy, které jsou potřeba pro podporu rozhraní. Z tohoto důvodu přidávání rozhraní nemění možnosti ověřování, která jsou implementovaná pomocí kódu v ukázkové aplikace vytvořené šablony. Například pokud vyberete prázdnou šablonu a přidejte webových formulářů nebo MVC podporovat, **konfigurace ověřování** tlačítko bude stále zakázáno.

Následující části popisují stručně účinek každé zaškrtávací políčko.

### <a name="add-web-forms-support"></a>Přidání podpory webového formuláře

Vytvoří prázdný *aplikace\_Data* a *modely* složek a *Global.asax* souboru. Tyto jsou již vytvořeny ve všech šablon jiné než prázdné šablonu, zaškrtněte políčko webových formulářů umožňuje žádné rozdíly pro další šablony.

Šablony webových formulářů povolí přátelské adresy URL ve výchozím nastavení, ale když přidáte podporu webových formulářů jiných šablon zaškrtnutím políčka webových formulářů, které přátelské adresy URL nejsou povolené automaticky.

### <a name="add-mvc-support"></a>Přidání podpory MVC

Nainstaluje balíčky MVC Razor a NuGet webové stránky, vytvoří prázdný *aplikace\_Data*, *řadiče*, *modely*, a *zobrazení*složek, vytvoří *aplikace\_spustit* složku s *soubor RouteConfig.cs* souboru a vytvoří *Global.asax* souboru.

### <a name="add-web-api-support"></a>Přidání podpory webového rozhraní API

Nainstaluje balíčky WebApi a Newtonsoft.Json NuGet, vytvoří prázdný *aplikace\_Data*, *řadiče*, a *modely* složek, vytvoří  *Aplikace\_spustit* složku s *WebApiConfig.cs* souboru a vytvoří *Global.asax* souboru.

<a id="auth"></a>
## <a name="authentication-methods"></a>Metody ověřování

Visual Studio 2013 nabízí několik možností ověřování pro šablony webových formulářů, MVC a webového rozhraní API:

- [Bez ověřování](#noauth)
- [Jednotlivé uživatelské účty](#indauth) (ASP.NET Identity, dřív označované jako členství technologie ASP.NET)
- [Účty organizace](#orgauth) (Windows Server Active Directory nebo Azure Active Directory)
- [Ověřování systému Windows](#winauth) (intranetu)

![Konfigurace ověřování dialogové okno](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Bez ověřování

Pokud vyberete **bez ověřování**, ukázkovou aplikaci bude obsahovat žádné webové stránky pro přihlášení, ne uživatelského rozhraní, která určuje, který je přihlášen, žádné entity třídy pro databázi členství a žádný připojovací řetězec databáze členství.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Jednotlivých uživatelských účtů

Pokud vyberete **jednotlivé uživatelské účty**, ukázkovou aplikaci bude nakonfigurován na používání technologie ASP.NET Identity (dříve označované jako členství technologie ASP.NET) pro ověřování uživatelů. ASP.NET Identity umožňuje uživatelům zaregistrovat účet, vytváření uživatelské jméno a heslo na webu nebo po přihlášení pomocí sociálních sítí, jako je Facebook, Google, Microsoft Account nebo Twitteru. Výchozí úložiště dat pro profily uživatelů v identitě ASP.NET Identity je databáze SQL serveru LocalDB, která můžete nasadit do systému SQL Server nebo Azure SQL Database pro pracoviště.

V sadě Visual Studio 2013 tyto funkce jsou stejné jako v sadě Visual Studio 2012, ale jsou přepsána kódu pro systém členství technologie ASP.NET. K výhodám tohoto nového základu kódu, patří:

- Nový systém členství je založen na [OWIN](http://owin.org/) místo modul ověřování ASP.NET pomocí formulářů. To znamená, které můžete použít stejný mechanismus ověřování, ať už používáte webové formuláře nebo MVC ve službě IIS, nebo máte vlastní hostování webového rozhraní API nebo SignalR.
- Novou databázi členství je spravovaná Entity Framework Code First a všechny tabulky jsou reprezentované pomocí tříd entit, které lze upravit. To znamená, že si můžete snadno přizpůsobit schéma databáze a související profil webového uživatelského rozhraní podle vlastních potřeb a můžete snadno nasadit vaše aktualizace použít migrace Code First.

Nový systém členství je automaticky implementované v nové šablony, a může být implementováno ručně v jakékoli projektu, jehož cílem .NET 4.5 nebo novější.

ASP.NET Identity je vhodné použít, pokud vytváříte webovou stránku Internet, což je především pro externí zákazníky. Pokud vaše organizace používá služby Active Directory nebo Office 365 a chcete vytvořit projekt, který umožňuje jednotného přihlašování pro zaměstnance a obchodními partnery **účty organizace** možnost může být vhodnější.

Další informace o možnosti jednotlivé uživatelské účty najdete v následujících zdrojích informací:

- [www.asp.net/identity](../../../identity/index.md). Dokumentaci o ASP.NET Identity na webu technologie ASP.NET.
- [Vytvoření aplikace ASP.NET MVC 5 s použitím Facebook a Google OAuth2 a přihlašování OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Také ukazuje, jak přizpůsobit data uživatelského profilu.
- [Webové rozhraní API - externí ověřovací služby](../../../web-api/overview/security/external-authentication-services.md)
- [Přidávání externích přihlášení do aplikace ASP.NET v sadě Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Účty organizace

Pokud vyberete **účty organizace**, nakonfigurujete ukázkovou aplikaci pro ověřování na základě uživatelských účtů ve službě Azure Active Directory (Azure AD, která zahrnuje Office 365) pomocí Windows Identity Foundation (WIF) nebo Windows Server Active Directory. Další informace najdete v tématu [možnosti ověřování účtu organizace](#orgauthoptions) dál v tomto tématu.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Ověřování systému Windows

Pokud vyberete **ověřování systému Windows**, nakonfigurujete ukázkovou aplikaci pro ověřování pomocí modulu IIS ověřování systému Windows. Aplikace se zobrazí doména a uživatelské ID služby Active directory nebo účet místního počítače, která je zaznamenána do systému Windows, ale nebude obsahovat registrace uživatele nebo protokolu v uživatelském rozhraní. Tato možnost je určena pro intranetové weby.

Alternativně můžete vytvořit intranetový server, který používá ověřování AD výběrem [místní možnosti v části účty organizace](#orgauthonprem). Možnost On-Premises používá Windows Identity Foundation (WIF) namísto modul ověřování systému Windows. Některé další kroky jsou požadovány k nastavit možnost On-Premises, ale WIF umožňuje funkce, které nejsou k dispozici s modulem ověřování systému Windows. Například pomocí WIF lze nakonfigurovat přístup k aplikaci v dat adresáře služby Active Directory a dotazů.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Možnosti ověřování účtu organizace

**Konfigurace ověřování** dialogové okno poskytuje několik možností pro Azure Active Directory (Azure AD, která zahrnuje Office 365) nebo účet ověřování systému Windows Server Active Directory (AD):

- [Cloud - jednotný](#orgauthsingle) (Azure AD ani AD pomocí integrace adresáře s Azure AD)
- [Cloud - organizace více](#orgauthmulti) (Azure AD ani AD pomocí integrace adresáře s Azure AD)
- [Místní](#orgauthonprem) (AD)

Pokud chcete vyzkoušet jednu z možností Azure AD, ale ještě nemáte účet [klikněte sem a zaregistrujte si účet Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Pokud si zvolíte jednu z možností Azure AD, projekt vyžaduje databázi a budete se muset přihlásit k účtu globálního správce pro vašeho tenanta Azure AD. Zadejte jméno a heslo pro účet organizace (například admin@contoso.onmicrosoft.com), má oprávnění správce pro vašeho tenanta Azure AD.
> 
> **Nezadávejte pověření pro účet Microsoft (například contoso@hotmail.com) v přihlašovací dialogové okno.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud - jednotný ověřování

![Jedna organizace ověřování](creating-web-projects-in-visual-studio/_static/image24.png)

Tuto možnost zvolte, pokud chcete povolit ověřování pro uživatelské účty, které jsou definovány v jedné službě Azure AD [klienta](https://technet.microsoft.com/library/jj573650.aspx). Například web je contoso.com a ho bude k dispozici na zaměstnance společnosti Contoso, kteří jsou v klientovi contoso.onmicrosoft.com. Nebudete moci konfigurovat Azure AD, aby uživatelé z jiných klientů pro přístup k aplikaci.

#### <a name="domain"></a>Domain

Zadejte doménu služby Azure AD, který chcete nastavit aplikaci, například: `contoso.onmicrosoft.com`. Pokud máte [vlastní domény](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), například `contoso.com` místo `contoso.onmicrosoft.com`, který sem můžete zadat.

#### <a name="access-level"></a>Úroveň přístupu

Pokud aplikace potřebuje k dotazu nebo aktualizovat informace o adresář pomocí rozhraní Graph API, zvolte **jednotné přihlašování, čtení dat adresáře** nebo **jednotné přihlašování, čtení a zápis dat adresáře**. Jinak, vyberte **jednotné přihlašování**. Další informace najdete v tématu [úrovně přístupu aplikace](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) a [pomocí rozhraní Graph API k dotazu služby Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Identifikátor ID URI aplikace

Ve výchozím nastavení vytvoří šablona dílčí identifikátor ID URI aplikace pro vás připojením názvu projektu k doméně služby Azure AD. Například, pokud je název projektu `Example` a doména je `contoso.onmicrosoft.com`, bude identifikátor ID URI aplikace `https://contoso.onmicrosoft.com/Example`. Pokud chcete ručně zadat identifikátor ID URI aplikace, rozbalte **další možnosti** a do textového pole zadejte identifikátor ID URI aplikace. Aplikace musí začínat identifikátor ID URI `https://`.

Ve výchozím nastavení Pokud aplikace, která je již zřízeno ve službě Azure AD má aplikace stejný identifikátor ID URI jako ten, který používá Visual Studio pro projekt, projekt se připojí k existující aplikaci místo nový zajišťování. Pokud chcete zřídit v takovém případě novou aplikaci, zrušte **přepsat aplikaci, pokud nějaká se stejným ID už existuje** zaškrtávací políčko.

Pokud **přepsat** políčko není zaškrtnuté a Visual Studio najde existující aplikace stejný identifikátor ID URI aplikace, vytvoří nový identifikátor URI připojit číslo a k identifikátoru URI se má použít. Předpokládejme například, je název projektu `Example`textového pole necháte prázdné, zrušíte zaškrtnutí **přepsat** zaškrtávací políčko a klient Azure AD již aplikace s identifikátorem URI `https://contoso.onmicrosoft.com/Example`. V takovém případě nová aplikace se budou zřizovat s jako identifikátor ID URI aplikace `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Zřizování aplikace ve službě Azure AD

Aby bylo možné zřídit aplikaci ve službě Azure AD nebo připojit k existující aplikaci projektu, Visual Studio potřebuje přihlašovací údaje globálního správce pro doménu. Když kliknete na tlačítko **OK** v **konfigurace ověřování** dialogové okno, budete vyzváni k uživatelské jméno a heslo pro globálního správce pro zadanou doménu. Později po kliknutí na tlačítko **vytvoření projektu** v **nový projekt ASP.NET** dialogové okno, Visual Studio zřídí aplikace ve službě Azure AD. Všimněte si, že v rámci tohoto procesu Visual Studio vloží klienta tajný hodnoty v souboru Web.config, který vyprší platnost jeden rok po vytvoření.

Informace o tom, jak vytvářet aplikace, které používají **cloudu – jednotný** ověřování, naleznete v následujících zdrojích:

- [Ověřování Azure](../2012/windows-azure-authentication.md)
- [Přidání přihlašování do webové aplikace pomocí služby Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Vývoj aplikací ASP.NET s použitím Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Zabezpečení rozhraní ASP.NET Web API s Azure AD a komponenty Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

Kurzů k ještě nebyly aktualizovány pro Visual Studio 2013; v sadě Visual Studio 2013 je automatizované některé jaké kurzů k přímé lze provádět ručně.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud - ověřování organizace více

![Více ověřování organizace](creating-web-projects-in-visual-studio/_static/image25.png)

Tuto možnost zvolte, pokud chcete povolit ověřování pro uživatelské účty, které jsou definovány v několika Azure AD [klienty](https://technet.microsoft.com/library/jj573650.aspx). Například web je contoso.com a ho zpřístupní zaměstnanci společnosti Contoso, kteří jsou v klientovi contoso.onmicrosoft.com a zaměstnanci společnosti Fabrikam, kteří jsou v klientovi fabrikam.onmicrosoft.com.

Nastavení, která zadáte a aplikací, zřizování kroku jsou podobné [jednotný ověřování](#orgauthsingle).

Informace o tom, jak vytvářet aplikace, které používají **cloudu - organizace více** ověřování, naleznete v následujících zdrojích:

- [Snadná integrace webové aplikace s Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) na blogu týmu pro službu Active Directory.
- [Vývoj víceklientské webových aplikací s Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) kurzu. Tento kurz nebyla aktualizována pro Visual Studio 2013; v sadě Visual Studio 2013 je automatizovat některé co kurzu přesměruje, lze provádět ručně.
- [Budete muset zaregistrovat s vlastní několik organizací aplikace ASP.NET, než se můžete přihlásit](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog podle Vittorio Bertocci, která vysvětluje, jak vyřešit společné osoby problém dojde při vytvoření projektu, který používá ověřování více organizace.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>Pro místní ověřování organizace

![Pro místní ověřování organizace](creating-web-projects-in-visual-studio/_static/image26.png)

Tuto možnost zvolte, pokud chcete povolit ověřování pro uživatelské účty, které jsou definované v systému Windows Server Active Directory (AD), a nechcete použít Azure AD. Tato možnost slouží k vytvoření v síti intranet nebo na webu na Internetu. Pro web na Internetu použijte Active Directory Federation Services (ADFS) pro poskytnutí přístupu k AD. Další informace najdete v tématu [pomocí místní organizace ověřování možnost (ADFS) pomocí technologie ASP.NET v sadě Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

U v síti intranet, jako alternativu můžete zvolit [ověřování systému Windows](#winauth) místo tuto možnost. Pro možnost ověřování systému Windows nemáte zadejte adresu URL dokumentu metadat. Ale ověřování systému Windows není poskytnuta možnost pro řízení přístupu aplikace ve službě Active Directory nebo do adresáře dotaz na data.

#### <a name="on-premises-authority"></a>Místní autorita

Zadejte adresu URL, která ukazuje na dokument metadat. Dokument metadat obsahuje souřadnice autority. Aplikace bude Tyhle souřadnice používat k řízení toku webové přihlášení.

#### <a name="application-id-uri"></a>Identifikátor ID URI aplikace

Zadejte jedinečný identifikátor URI, který AD můžete použít k identifikaci této aplikace, nebo ponechat prázdné nechte nástroj Visual Studio vytvořit.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Další kroky

Tento dokument je poskytován některé základní nápovědu pro vytvoření nového webového projektu ASP.NET v sadě Visual Studio 2013. Další informace o používání pro sadu Visual Studio pro vývoj webů najdete v tématu [https://www.asp.net/visual-studio/](../../index.md).
