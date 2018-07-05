---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: Vytváření projektů v prostředí ASP.NET v sadě Visual Studio 2013 | Dokumentace Microsoftu
author: tdykstra
description: Toto téma popisuje možnosti pro vytváření webových projektů ASP.NET v sadě Visual Studio 2013 s aktualizací 3 zde jsou některé nové funkce pro jazyk c vývoj pro web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 6033163b7b8e9e1d0524935217dc53f938470cb6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363465"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a>Vytváření webových projektů ASP.NET v sadě Visual Studio 2013
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Toto téma popisuje možnosti pro vytváření webových projektů ASP.NET v sadě Visual Studio 2013 s aktualizací Update 3
> 
> Tady jsou některé z nových funkcí pro vývoj webů porovnání s předchozími verzemi sady Visual Studio:
> 
> - Jednoduché uživatelské rozhraní pro vytváření projektů nabídku [podporu pro více platforem ASP.NET](#add) (webové formuláře, MVC a webového rozhraní API).
> - [ASP.NET Identity](#indauth), nový systém členství technologie ASP.NET, který funguje stejně ve všech platforem ASP.NET a funguje s webhosting softwaru než služby IIS.
> - Použití [Bootstrap](#bootstrap) poskytnout interaktivní možnosti návrhu a motivů.
> - Nové funkce pro webové formuláře, kterého chcete nabízet pouze pro architekturu MVC, jako například [vytváření automatických testů projektu](#testproj) a [šablony webu intranetu](#winauth).
> 
> Informace o tom, jak vytvořit webové projekty pro Azure Cloud Services nebo Azure Mobile Services, najdete v části [Začínáme s Azure Cloud Services a ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) a [vytvoření Žebříčkové aplikace pomocí .NET v Azure Mobile Services Back-endu](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).


<a id="prerequisites"></a>
## <a name="prerequisites"></a>Požadavky

Tento článek se týká [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) s [s aktualizací Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) nainstalované.

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a>Projekty webových aplikací a webové projekty

ASP.NET vám dává na výběr mezi těmito dvěma typy webových projektů: *webových projektů aplikace* a *webových projektů*. Doporučujeme, abyste projekty webových aplikací pro vývoj nových projektů, a tento článek se týká pouze projektů webových aplikací. Další informace najdete v tématu [Web Application Projects versus webových projektů v sadě Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) na webu MSDN.

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a>Přehled vytvoření projektu webové aplikace

Následující kroky ukazují, jak na vytvoření webového projektu:

1. Klikněte na tlačítko **nový projekt** v **spustit** stránky nebo **souboru** nabídky.
2. V **nový projekt** dialogového okna, klikněte na tlačítko **webové** v levém podokně a **webová aplikace ASP.NET** v prostředním podokně.

    ![Dialogové okno nového projektu](creating-web-projects-in-visual-studio/_static/image1.png)

    Můžete použít **cloudu** v levém podokně vytvoření [cloudové služby Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile Service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), nebo [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs). Toto téma nepopisuje tyto šablony.
3. V pravém podokně klikněte **přidat službu Application Insights do projektu** zaškrtávací políčko, pokud chcete stav a sledování využití pro vaši aplikaci. Další informace najdete v tématu [monitorování výkonu webových aplikací](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).
4. Zadejte projekt **název**, **umístění**a další možnosti a pak klikněte na tlačítko **OK**.

    **Nový projekt ASP.NET** se zobrazí dialogové okno.

    ![Dialogové okno nového projektu](creating-web-projects-in-visual-studio/_static/image2.png)
5. Klikněte na šablonu.

    ![Vybrat šablonu](creating-web-projects-in-visual-studio/_static/image3.png)
6. Pokud chcete přidat podporu pro další architektury, které nejsou zahrnuty v šabloně, klikněte na příslušné políčko. (Z příkladu, můžete přidat MVC nebo webové rozhraní API na projekt webových formulářů.)

    ![Přidat rozhraní](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a>Pokud chcete přidat projekt testování částí, klikněte na tlačítko **přidání jednotkových testů**.

    ![Přidat testy jednotek](creating-web-projects-in-visual-studio/_static/image5.png)
8. Pokud chcete metodu ověřování jiný než Šablona nabízí ve výchozím nastavení, klikněte na tlačítko **změna ověřování**.

    ![Konfigurace ověřování tlačítko](creating-web-projects-in-visual-studio/_static/image6.png)

    ![Konfigurace dialog ověřování.](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a>Vytvoření webové aplikace nebo virtuálního počítače v Azure

Visual Studio obsahuje funkce, které usnadňují práci se službami Azure pro hostování webových aplikací. Například všechny z následujících akcí můžete provést přímo z integrovaného vývojového prostředí sady Visual Studio:

- Vytvoření a Správa webové aplikace nebo virtuální počítače, které vaší aplikaci zpřístupnit přes Internet.
- Zobrazit protokoly, které vytvořila aplikace při jejím spuštění v cloudu.
- Vzdálené spouštění v režimu ladění při spuštění aplikace v cloudu.
- Viiew a spravovat další služby Azure, jako jsou databáze SQL.

Je možné [vytvořit účet Azure](https://www.windowsazure.com/pricing/free-trial/) zdarma, který obsahuje základní služby, jako jsou například webové aplikace, a pokud jste předplatitelem MSDN můžete [aktivovat výhody](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) , který získáte měsíční kredit na další Azure služby. 

Ve výchozím nastavení **nový projekt ASP.NET** dialogové okno umožňuje vytvářet webové aplikace nebo virtuálního počítače pro nového webového projektu. Pokud nechcete vytvořit novou webovou aplikaci nebo virtuální počítač, zrušte zaškrtnutí políčka **hostovat v cloudu** zaškrtávací políčko.

![Vytvořit vzdálené prostředky](creating-web-projects-in-visual-studio/_static/image8.png)

Titulek zaškrtávací políčko může být **hostovat v cloudu** nebo **vytvořit vzdálené prostředky**, a v obou případech efekt je stejný. Pokud necháte políčko zaškrtnuté, Visual Studio vytvoří webovou aplikaci ve službě Azure App Service ve výchozím nastavení. Pole rozevíracího seznamu můžete změnit tak, aby **virtuálního počítače** Pokud dáváte přednost. Pokud jste ještě nejste přihlášení do Azure, se zobrazí výzva k zadání přihlašovacích údajů Azure. Po přihlášení, dialogové okno umožňuje konfigurovat prostředky, které pro váš projekt vytvoří Visual Studio. Následující obrázek ukazuje dialogové okno pro webové aplikace; Pokud budete chtít vytvořit virtuální počítač se zobrazí různé možnosti.

![Konfigurace nastavení aplikace Azure](creating-web-projects-in-visual-studio/_static/image9.png)

Další informace o tom, jak používat tento proces pro vytváření prostředků Azure najdete v tématu [Začínáme s Azure a ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) a [vytvoření virtuálního počítače pro webovou stránku pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).

Zbývající část tohoto článku poskytuje další informace o dostupných šablon a jejich možnosti. Tento článek také zavádí Bootstrap, rozložení a motivy framework používá v šablonách.

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a>Šablony webových projektů Visual Studio 2013

Visual Studio používá šablony k vytváření webových projektů. Šablona projektu můžete vytvořit soubory a složky v novém projektu, instalace balíčků NuGet a ukázkový kód stanovit základní funkční aplikaci. Šablony implementovat nejnovější webové standardy a jsou určeny k předvedení osvědčené postupy, jak používat technologie ASP.NET, stejně jako vám přechod začít na vytváření vlastních aplikací.

Visual Studio 2013 nabízí následující možnosti pro šablony webových projektů pro projekty, které cílí na rozhraní .NET 4.5 nebo novější verze rozhraní .NET Framework:

- [Prázdná šablona](#empty)
- [Šablony webových formulářů](#wf)
- [Šablona MVC](#mvc)
- [Šablona webového rozhraní API](#webapi)
- [Šablona pro jednu stránku aplikace](#spa)
- [Šablony Azure Mobile Service](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [Šablony sady Visual Studio 2012](#vs2012)

Můžete také nainstalovat rozšíření sady Visual Studio, které poskytuje [šabloně pro síť Facebook](#facebook).

Informace o tom, jak vytvářet projekty, které jsou cíleny na rozhraní .NET 4, najdete v části [šablony sady Visual Studio 2012](#vs2012) dále v tomto tématu.

Informace o vytváření aplikací ASP.NET pro mobilní klienty najdete v tématu [mobilní podpora v technologii ASP.NET](../../../mobile/index.md).

<a id="empty"></a>
### <a name="empty-template"></a>Prázdná šablona

Prázdná šablona nabízí úplné minimální složky a soubory pro webové aplikace ASP.NET, jako je například soubor projektu (*.csproj* nebo. *vbproj*) a *Web.config* souboru. Můžete přidat podporu pro webové formuláře, MVC a/nebo webové rozhraní API pomocí zaškrtávacích políček v rámci **přidat složky a základní odkazy pro:** popisek.

Pro prázdnou šablonu nejsou dostupné žádné možnosti ověřování. Funkce ověřování je implementována v ukázkové aplikace a prázdnou šablonu nevytvoří ukázkovou aplikaci.

<a id="wf"></a>
### <a name="web-forms-template"></a>Šablony webových formulářů

Webové formuláře, který framework poskytuje následující funkce, které vám umožní rychle vytvářet weby, které jsou bohaté v uživatelském rozhraní a data přístup k funkcím:

- WYSIWYG návrháře v sadě Visual Studio.
- Serverové ovládací prvky, které vykreslení HTML a, které můžete přizpůsobit tak, že nastavíte vlastnosti a styly.
- Bohaté sortiment ovládací prvky pro přístup k datům a data zobrazení.
- Model událostí, který zpřístupňuje události, ke kterým můžete programovat podobně, jako by programu klientská aplikace, jako je například WPF.
- Automatické zachování stavu (data) mezi požadavky HTTP.

Vytvoření aplikace webových formulářů obecně vyžaduje méně programátorského úsilí než vytvoření stejnou aplikaci pomocí rozhraní ASP.NET MVC. Webové formuláře je však není jen pro rychlý vývoj aplikací. Existuje mnoho složitých obchodních aplikací a architektur postavené na webových formulářů.

Protože stránky s webovými formuláři a ovládacích prvků na stránce automaticky velkou část značky, které je odesláno prohlížeči, není nutné druh velice přesně kontrolovat, HTML, který nabízí technologie ASP.NET MVC. Deklarativní model pro konfiguraci stránek a ovládacích prvků minimalizuje množství kódu, máte k zápisu, ale některé chování kódu HTML a HTTP skryje. Například není vždy možné určit, jaký kód přesně může vygenerovat ovládacím prvkem.

Rozhraní webových formulářů nemá řešení snadno ASP.NET MVC na základě vzorů vývojové postupy, jako [vývoj řízený testováním](http://en.wikipedia.org/wiki/Test-driven_development), [oddělení oblastí zájmu](http://en.wikipedia.org/wiki/Separation_of_concerns), [vzájemný ovládací prvek](http://en.wikipedia.org/wiki/Inversion_of_control), a [injektáž závislostí](http://en.wikipedia.org/wiki/Dependency_injection). Pokud chcete napsat kód dostaneme tímto způsobem, je to možné není právě automatické, protože je v rozhraní ASP.NET MVC. [ASP.NET Web Forms MVP](http://webformsmvp.com/) projektu se zobrazí, která usnadňuje oddělení připomínky a testovatelnosti při zachování rychlý vývoj, který webových formulářů byla vytvořena k zajištění přístupu. Microsoft SharePoint je postavená na webových formulářů MVP.

Šablony webových formulářů vytvoří ukázkovou aplikaci webových formulářů, který používá [Bootstrap](#bootstrap) a zajistit tak interaktivní funkce návrhu a motivů. Následující obrázek ukazuje na domovské stránce.

![Domovská stránka šablony aplikace webových formulářů](creating-web-projects-in-visual-studio/_static/image10.png)

Další informace o webových formulářích najdete v tématu [webových formulářů ASP.NET](https://asp.net/web-forms). Informace o šabloně webových formulářů udělá za vás, najdete v tématu [sestavení základní aplikace webových formulářů pomocí sady Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).

<a id="mvc"></a>
### <a name="mvc-template"></a>Šablona MVC

ASP.NET MVC je navržená pro usnadnění postupů vývoje na základě vzorů, jako [vývoj řízený testováním](http://en.wikipedia.org/wiki/Test-driven_development), [oddělení oblastí zájmu](http://en.wikipedia.org/wiki/Separation_of_concerns), [inverzi ovládacího prvku](http://en.wikipedia.org/wiki/Inversion_of_control), a [injektáž závislostí](http://en.wikipedia.org/wiki/Dependency_injection). Toto rozhraní umožňuje oddělení vrstvy obchodní logiky webové aplikace z jeho prezentační vrstvy. Rozdělením architektury do modelů (M), zobrazení (V) a kontrolerů (C), ASP.NET MVC lze usnadňují Správa složitých aplikací ve větších aplikací.

ASP.NET MVC můžete pracovat přímo se HTML a protokol HTTP, než ve webových formulářů. Například webové formuláře může automaticky zachování stavu mezi požadavky HTTP, ale budete muset kód, který explicitně v aplikaci MVC. Výhod modelu MVC je, abyste provedli úplnou kontrolu nad přesně co aplikace dělá a jak se chová v prostředí webové umožňuje. Nevýhodou je, že budete muset napsat další kód.

MVC je navržená pro rozšiřitelnost, možnost přizpůsobení rozhraní pro jejich potřeby aplikace, že vývojáři power. Kromě toho je k dispozici pod licenci OSI zdrojové stránky ASP.NET MVC.

Šablona MVC vytvoří ukázkovou aplikaci MVC 5 využívající [Bootstrap](#bootstrap) a zajistit tak interaktivní funkce návrhu a motivů. Následující obrázek ukazuje na domovské stránce.

![Ukázková aplikace MVC](creating-web-projects-in-visual-studio/_static/image11.png)

Další informace o MVC najdete v tématu [ASP.NET MVC](https://asp.net/mvc). Informace o tom, jak vybrat šablonu MVC 4, najdete v části [šablony sady Visual Studio 2012](#vs2012) dále v tomto článku.

<a id="webapi"></a>
### <a name="web-api-template"></a>Šablona webového rozhraní API

Šablona webového rozhraní API vytvoří ukázkové webové služby založené na webové rozhraní API, včetně stránek nápovědy rozhraní API založené na MVC.

ASP.NET Web API je architektura, která usnadňuje sestavování služeb HTTP, které jsou poskytovány širokému spektru klientů, včetně prohlížečů a mobilních zařízení. ASP.NET Web API je ideální platformu pro vytváření služby RESTful v rozhraní .NET Framework.

Šablona webového rozhraní API vytvoří ukázkové webové služby. Ukázkové stránky nápovědy na následujících obrázcích.

![Stránka nápovědy webové rozhraní API](creating-web-projects-in-visual-studio/_static/image12.png)

![Stránka nápovědy webové rozhraní API pro získání rozhraní API](creating-web-projects-in-visual-studio/_static/image13.png)

Další informace o rozhraní Web API najdete v tématu [rozhraní ASP.NET Web API](https://asp.net/web-api).

<a id="spa"></a>
### <a name="single-page-application-template"></a>Šablona jednostránkové aplikace

Šablona jedné stránky aplikace (SPA) vytvoří ukázkovou aplikaci, která používá jazyk JavaScript, HTML 5, a [KnockoutJS](http://knockoutjs.com/) na klientovi a webového rozhraní API ASP.NET na serveru.

Pouze ověřování možnost SPA šablony je [jednotlivé uživatelské účty](#indauth).

Následující obrázek znázorňuje ukázkové aplikace, který vytváří šablon SPA počáteční stav.

![Ukázková aplikace SPA](creating-web-projects-in-visual-studio/_static/image14.png)

Informace o tom, jak vytvořit aplikaci pomocí šablony jednostránková aplikace najdete v tématu [webového rozhraní API – externí ověřovací služby](../../../web-api/overview/security/external-authentication-services.md).

Další informace o jednostránkové aplikace ASP.NET a další šablony jednostránková aplikace, které používají rozhraní JavaScript než KnockoutJS naleznete na následujících odkazech:

- [ASP.NET jedné stránce aplikace](../../../single-page-application/index.md).
- [Princip funkce zabezpečení v šabloně SPA for VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [Jednostránková aplikace: Vytváření moderních, interaktivních webových aplikací pomocí ASP.NET](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a>Šablona pro síť Facebook

Můžete nainstalovat [rozšíření sady Visual Studio, který je k dispozici šablona Facebook](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409). Tato šablona vytvoří ukázkovou aplikaci, která slouží ke spouštění uvnitř webovou stránku Facebooku. Je založený na rozhraní ASP.NET MVC a používá webového rozhraní API pro funkci aktualizace v reálném čase.

Žádné možnosti ověřování jsou dostupné pro šabloně pro síť Facebook, protože aplikace Facebook spuštění v rámci sítě Facebook a Spolehněte se na ověření na Facebooku.

Další informace o aplikacích technologie ASP.NET Facebook, naleznete v tématu [aktualizuje se rozhraní API Facebooku MVC](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a>Šablony sady Visual Studio 2012

Dialogové okno Vytvoření projektu webové aplikace Visual Studio 2013 neposkytuje přístup k některé šablony, které byly k dispozici v sadě Visual Studio 2012. Pokud chcete použít jednu z těchto šablon, můžete kliknout na uzel sady Visual Studio 2012 v levém podokně dialogového okna Nový projekt sady Visual Studio.

![Šablony sady Visual Studio 2012](creating-web-projects-in-visual-studio/_static/image15.png)

**Visual Studio 2012** uzel umožní vybrat následující webové šablony, které nemáte přístup k v seznamu výchozích šablon pro Visual Studio 2013:

- Webové aplikace ASP.NET MVC 4
- Webová aplikace ASP.NET s dynamickými datovými entitami
- Serverový ovládací prvek ASP.NET AJAX
- ASP.NET AJAX – Extender ovládacího prvku serveru
- Serverový ovládací prvek ASP.NET

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a>Spuštění v šablony webových projektů Visual Studio 2013

Použijte šablony projektů Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), rozložení a motivy rozhraní vytvořené Twitter. K zajištění přizpůsobivý návrh, což znamená, že rozložení můžete dynamicky přizpůsobit velikosti okna jiný prohlížeč používá Bootstrap CSS3. V okně prohlížeče široké například na domovské stránce vytvořený pomocí šablony webových formulářů vypadá jako na následujícím obrázku:

![Domovská stránka šablony aplikace webových formulářů](creating-web-projects-in-visual-studio/_static/image16.png)

Aby se okno užší a vodorovně uspořádanými sloupce přesunout do svislém uspořádání:

![Uspořádání Bootstrap svislý sloupec](creating-web-projects-in-visual-studio/_static/image17.png)

Zúžení okna trošku lépe a horizontální horní nabídce se změní ikona, která můžete kliknout a rozšířit svisle orientovaný nabídky:

![Ikony Bootstrap nabídky](creating-web-projects-in-visual-studio/_static/image18.png)

![Spuštění svislé nabídky](creating-web-projects-in-visual-studio/_static/image19.png)

Funkce motivů Bootstrap můžete také snadno provést změnu v aplikačním vzhled a chování. Například můžete provést následující kroky, chcete-li změnit motiv.

1. V prohlížeči přejděte na [ http://Bootswatch.com ](http://Bootswatch.com), zvolte jen motiv a potom klikněte na tlačítko **Stáhnout**. (Tato akce stáhne *bootstrap.min.css* ve výchozím nastavení; Pokud chcete prozkoumat kód šablony stylů CSS, získejte *bootstrap.css* namísto minifikovaný verze.)
2. Zkopírujte obsah stažený soubor šablony stylů CSS.
3. V sadě Visual Studio vytvořte nový **stylů** soubor s názvem *bootstrap-theme.css* v *obsahu* složky a vložit stažené šablony stylů CSS kódu do něj.
4. Otevřít *aplikace\_Start/Bundle.config* a změňte *bootstrap.css* k *bootstrap-theme.css*.

Spusťte projekt znovu a má nový vzhled aplikace. Následující obrázek znázorňuje vliv Amelia motivu:

![Spuštění Amelia motiv](creating-web-projects-in-visual-studio/_static/image20.png)

Mnoho Bootstrap motivy jsou k dispozici, verze free a premium. Bootstrap také nabízí širokou škálu součásti uživatelského rozhraní, jako například [rozevírací seznamy](http://twitter.github.io/bootstrap/components.html#dropdowns), [tlačítko skupiny](http://twitter.github.io/bootstrap/components.html#buttonGroups), a [ikony](http://twitter.github.io/bootstrap/base-css.html#images). Další informace o spuštění najdete v tématu [Bootstrap lokality](http://twitter.github.io/bootstrap/).

Pokud použijete Návrhář webových formulářů v sadě Visual Studio, mějte na paměti, že návrhář nepodporuje CSS3, takže ho přesně nezobrazí všechny účinky Bootstrap motivy nebo změny responzivního rozložení. Stránky webových formulářů však bude zobrazovat správně při zobrazení v prohlížeči.

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a>Přidání podpory pro další architektury

Když vyberete šablonu, zaškrtněte políčko pro architekturu používá šablonu vybere automaticky. Například, pokud jste vybrali **webových formulářů** šablony, **webových formulářů** zaškrtávací políčko zaškrtnuto a nejde ji zrušit.

![Vybrat šablonu](creating-web-projects-in-visual-studio/_static/image21.png)

![Přidat rozhraní](creating-web-projects-in-visual-studio/_static/image22.png)

Můžete vybrat zaškrtávací políčko pro systém, který není zahrnutý v šabloně, chcete-li přidat podporu pro dané rozhraní při vytvoření projektu. Například chcete povolit používání webových formulářů *.aspx* stránky při vámi zvolená šablona MVC, vyberte **webových formulářů** zaškrtávací políčko. Nebo pokud chcete povolit MVC při použití šablony webových formulářů, klikněte na tlačítko **MVC** zaškrtávací políčko. Přidání rozhraní umožňuje podporu návrhu, jakož i za běhu. Například pokud přidáte podpora MVC pro projekt webových formulářů, bude možné scaffold kontrolerů a zobrazení.

Pokud můžete kombinovat webové formuláře a MVC v projektu a povolit [přátelské adresy URL](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) ve webových formulářích, může existovat neočekávané směrování potíží, kdy jedna adresa URL má několik možných cílů. Tras, které jsou definovány nejprve bude mít přednost. Například, pokud máte `Home` kontroleru a *Home.aspx* stránky, `http://contoso.com/home` adresy URL budou moct *Home.aspx* při volání `EnableFriendlyUrls` metoda před voláním `MapRoute`metoda *RouteConfig.cs*, nebo stejná adresa URL přejde do výchozího zobrazení pro vaše `Home` řadič při volání `MapRoute` před `EnableFriendlyUrls`.

Přidání rozhraní nepřidá žádné ukázkové aplikace funkce. Například pokud chcete přidat webové formuláře podporu při vámi zvolená šablona MVC, ne *Default.aspx* se vytvoří soubor domovské stránky. Jsou přidány pouze složky, soubory a odkazy, které jsou potřeba pro podporu rozhraní. Z tohoto důvodu přidání architektury nezmění možnosti ověřování, které jsou implementovány pomocí kódu v ukázkových aplikací vytvořili pomocí šablon. Například pokud vyberete prázdnou šablonu a přidejte webové formuláře nebo MVC podporovat, **konfigurace ověřování** tlačítko bude stále zakázán.

Následující části popisují stručně efekt každé zaškrtávací políčko.

### <a name="add-web-forms-support"></a>Přidání podpory webového formuláře

Vytvoří prázdný *aplikace\_Data* a *modely* složky a *Global.asax* souboru. Tyto jsou už vytvořené všechny šablony jiné než prázdné šablony, tak výběrem zaškrtávacího políčka webových formulářů díky žádné rozdíly pro další šablony.

Šablony webových formulářů povolí přátelské adresy URL ve výchozím nastavení, ale když přidáte podporu webových formulářů s ostatními šablonami zaškrtnutím políčka webových formulářů, které přátelské adresy URL nejsou povolené automaticky.

### <a name="add-mvc-support"></a>Přidání podpory MVC

Nainstaluje balíčky NuGet webové stránky, MVC a Razor, vytvoří prázdný *aplikace\_Data*, *řadiče*, *modely*, a *zobrazení*složek, vytvoří *aplikace\_Start* složka s *RouteConfig.cs* souboru a vytvoří *Global.asax* souboru.

### <a name="add-web-api-support"></a>Přidání podpory webového rozhraní API

Nainstaluje WebApi a Newtonsoft.Json NuGet, vytvoří prázdný *aplikace\_Data*, *řadiče*, a *modely* složek, vytvoří  *Aplikace\_Start* složka s *WebApiConfig.cs* souboru a vytvoří *Global.asax* souboru.

<a id="auth"></a>
## <a name="authentication-methods"></a>Metody ověřování

Visual Studio 2013 nabízí několik možností ověřování šablony webové formuláře, MVC a webového rozhraní API:

- [Bez ověřování](#noauth)
- [Individuální uživatelské účty](#indauth) (ASP.NET Identity, dřív označované jako členství technologie ASP.NET)
- [Účty organizací](#orgauth) (Windows Server Active Directory nebo Azure Active Directory)
- [Ověřování Windows](#winauth) (intranetu)

![Konfigurace dialog ověřování.](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a>Bez ověřování

Pokud vyberete **bez ověřování**, ukázkové aplikace bude obsahovat žádné webové stránky pro přihlášení, žádné uživatelské rozhraní určující, který je přihlášen, žádné tříd entit databáze členství a žádný připojovací řetězec pro databázi členství.

<a id="indauth"></a>
### <a name="individual-user-accounts"></a>Individuální uživatelské účty

Pokud vyberete **jednotlivé uživatelské účty**, ukázkovou aplikaci nakonfigurujete pro použití technologie ASP.NET Identity (dříve označované jako členství technologie ASP.NET) k ověřování uživatelů. ASP.NET Identity umožňuje uživateli zaregistrovat účet, tak, že vytvoříte uživatelské jméno a heslo na webu nebo když se přihlásíte pomocí poskytovatelé služeb sociálních sítí, jako je Facebook, Google, Account Microsoft nebo Twitter. Výchozí úložiště dat pro profily uživatelů v ASP.NET Identity je databáze SQL Server LocalDB, který můžete nasadit do systému SQL Server nebo Azure SQL Database pro produkční lokality.

V sadě Visual Studio 2013 tyto funkce jsou stejné jako v sadě Visual Studio 2012, ale přepsali jsme základní kód pro systém členství technologie ASP.NET. Výhody nových základu kódu, patří:

- Nový systém členství je založen na [OWIN](http://owin.org/) místo modulu ověřování formulářů ASP.NET. To znamená, že můžete použít stejný mechanismus ověřování, ať už používáte webové formuláře nebo MVC ve službě IIS, nebo jste samoobslužné hostování webového rozhraní API nebo SignalR.
- Nová databáze členství se spravuje pomocí platformy Entity Framework Code First a všechny tabulky jsou reprezentovány tříd entit, které můžete upravit. To znamená, že můžete snadno přizpůsobit schéma databáze a souvisejícím profilu webovém uživatelském rozhraní podle svých potřeb a snadno můžete nasadit aktualizace pomocí migrace Code First.

Nový systém členství je automaticky implementována v nové šablony a může být implementováno ručně v jakémkoli projektu, který cílí na rozhraní .NET 4.5 nebo novější.

ASP.NET Identity je dobrou volbou, pokud vytváříte web na Internetu, který je zaměřen především na externí zákazníky. Pokud vaše organizace používá Active Directory nebo Office 365 a chcete vytvořit projekt, který umožňuje single-sign-on pro zaměstnance a obchodními partnery **účty organizace** možnost může být lepší volbou.

Další informace o možnosti jednotlivých uživatelských účtů naleznete na následujících odkazech:

- [www.asp.net/identity](../../../identity/index.md). Dokumentaci k ASP.NET Identity na webové stránce ASP.NET.
- [Vytvoření aplikace ASP.NET MVC 5 pomocí Facebooku a Google OAuth2 nebo OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md). Také ukazuje, jak přizpůsobit data uživatelského profilu.
- [Webové rozhraní API – externí ověřovací služby](../../../web-api/overview/security/external-authentication-services.md)
- [Přidávání externích přihlášení do aplikace ASP.NET v sadě Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a>Účty organizací

Pokud vyberete **účty organizace**, se nakonfigurují ukázkovou aplikaci pro ověřování na základě uživatelských účtů ve službě Azure Active Directory (Azure AD, která zahrnuje Office 365) pomocí technologie Windows Identity Foundation (WIF) nebo Windows Server Active Directory. Další informace najdete v tématu [možnosti ověřování účtu organizace](#orgauthoptions) dále v tomto tématu.

<a id="winauth"></a>
### <a name="windows-authentication"></a>Ověřování systému Windows

Pokud vyberete **ověřování Windows**, se nakonfigurují ukázkovou aplikaci pro účely ověření modulu ověřování Windows služby IIS. Aplikace se zobrazí doména a uživatelské ID služby Active directory nebo účet místního počítače, který se přihlásí do Windows, ale nebude obsahovat registrace uživatele nebo přihlášení uživatelského rozhraní. Tato možnost je určená pro intranetové weby.

Alternativně můžete vytvořit v síti intranet, která používá ověřování AD výběrem [On-Premises možnosti v části účty organizace](#orgauthonprem). Možnost On-Premises používá technologie Windows Identity Foundation (WIF) namísto modul ověřování Windows. Pokud chcete nastavit možnost On-Premises jsou nezbytné některé další kroky, ale technologie WIF umožňuje funkce, které nejsou k dispozici modul ověřování Windows. Například pomocí technologie WIF lze nakonfigurovat přístup k aplikacím ve službě Active Directory a dotazování dat adresáře.

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a>Možnosti ověřování účtu organizace

**Konfigurace ověřování** dialogové okno nabízí několik možností pro Azure Active Directory (Azure AD, která zahrnuje Office 365) nebo ověřování pomocí účtu systému Windows Server Active Directory (AD):

- [Cloud – jedna organizace](#orgauthsingle) (Azure AD nebo AD pomocí integrace adresáře s Azure AD)
- [Cloud – víc organizací](#orgauthmulti) (Azure AD nebo AD pomocí integrace adresáře s Azure AD)
- [On-Premises](#orgauthonprem) (AD)

Pokud chcete zkusit jednu z možností Azure AD, ale ještě nemáte účet [kliknutím sem si zaregistrovat účet služby Azure AD](https://go.microsoft.com/fwlink/?LinkId=309942).

> [!NOTE]
> Pokud vyberete jednu z možností Azure AD, váš projekt vyžaduje databázi a budete muset přihlásit k účtu globálního správce pro vašeho tenanta Azure AD. Zadejte uživatelské jméno a heslo pro účet organizace (třeba admin@contoso.onmicrosoft.com), který má oprávnění správce pro vašeho tenanta Azure AD.
> 
> **Nezadávejte přihlašovací údaje pro účet Microsoft (třeba contoso@hotmail.com) v poli přihlašovacího dialogového okna.**


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a>Cloud – jedna organizace ověřování

![Jedna organizace ověřování](creating-web-projects-in-visual-studio/_static/image24.png)

Tuto možnost zvolte, pokud chcete povolit ověřování pro uživatelské účty, které jsou definovány v jedné službě Azure AD [tenanta](https://technet.microsoft.com/library/jj573650.aspx). Například web je contoso.com to bude k dispozici zaměstnancům společnosti Contoso, kteří jsou v tenantovi contoso.onmicrosoft.com Nebudete mít ke konfiguraci Azure AD umožňuje uživatelům z jiných tenantů pro přístup k aplikaci.

#### <a name="domain"></a>Domény

Zadejte doménu služby Azure AD, kterou chcete nastavit aplikaci, například: `contoso.onmicrosoft.com`. Pokud máte [vlastní doménu](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), jako například `contoso.com` místo `contoso.onmicrosoft.com`, který Tady můžete zadat.

#### <a name="access-level"></a>Úroveň přístupu

Pokud aplikace potřebuje k dotazování nebo aktualizovat informace o adresáři pomocí rozhraní Graph API, zvolte **jednotného přihlašování, čtení dat adresáře** nebo **jednotného přihlašování, čtení a zápis dat adresáře**. Jinak klikněte na tlačítko **Single Sign-On**. Další informace najdete v tématu [úrovně přístupu aplikace](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) a [pomocí rozhraní Graph API k dotazování služby Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).

#### <a name="application-id-uri"></a>Identifikátor URI ID aplikace

Ve výchozím nastavení šablona vytvoří identifikátor ID URI aplikace můžete připojením názvu projektu k doméně Azure AD. Například, pokud je název projektu `Example` a doména je `contoso.onmicrosoft.com`, stane se identifikátor URI ID aplikace `https://contoso.onmicrosoft.com/Example`. Pokud chcete ručně zadejte identifikátor URI ID aplikace, rozbalte **další možnosti** a do textového pole zadejte identifikátor URI ID aplikace. Aplikace musí začínat identifikátor ID URI `https://`.

Ve výchozím nastavení Pokud aplikace, která je již zřízeno ve službě Azure AD má aplikace stejný identifikátor ID URI jako ten, který používá Visual Studio pro projekt, projekt připojí existující aplikace, nikoliv zřizování nového. Pokud chcete novou aplikaci, které se mají zřídit v takovém, zrušte zaškrtnutí políčka **přepsat položku aplikace, pokud se stejným ID už existuje** zaškrtávací políčko.

Pokud **přepsat** zrušení zaškrtnutí políčka a sada Visual Studio najde existující aplikace s stejný identifikátor URI ID aplikace, vytvoří nový identifikátor URI podle pořadového čísla na identifikátor URI, který se má použít. Předpokládejme například, že je název projektu `Example`, ponechte prázdné textové pole, odstraníte **přepsat** zaškrtávací políčko a tenanta Azure AD již byla aplikace s identifikátorem URI `https://contoso.onmicrosoft.com/Example`. V takovém případě se zřídí novou aplikaci s aplikací, jako je identifikátor ID URI `https://contoso.onmicrosoft.com/Example_20130619330903`.

#### <a name="provisioning-the-application-in-azure-ad"></a>Vytváření aplikace ve službě Azure AD

Aby bylo možné zřídit aplikaci ve službě Azure AD nebo připojit projekt k existující aplikaci, Visual Studio potřebuje přihlašovací údaje globálního správce pro doménu. Po kliknutí na **OK** v **konfigurace ověřování** dialogové okno, budete vyzváni k zadání uživatelského jména a hesla globálního správce pro doménu, které jste zadali. Později, po kliknutí na **vytvořit projekt** v **nový projekt ASP.NET** dialogovém okně Visual Studio zřídí aplikaci ve službě Azure AD. Všimněte si, že jako součást tohoto procesu Visual Studio vloží hodnoty tajných kódů klienta v souboru Web.config, který vyprší jeden rok po jeho vytvoření.

Informace o tom, jak vytvářet aplikace, které používají **Cloud – jedna organizace** ověřování, najdete v následujících zdrojích informací:

- [Ověřování Azure](../2012/windows-azure-authentication.md)
- [Přidání přihlašování do webové aplikace pomocí Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [Vývoj aplikací ASP.NET s použitím Azure Active Directory](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [Zabezpečení rozhraní ASP.NET Web API s využitím Azure AD a komponenty Microsoft OWIN](https://msdn.microsoft.com/magazine/dn463788.aspx)

V kurzech zatím se neaktualizovaly pro sadu Visual Studio 2013; v sadě Visual Studio 2013 je automatické některé z jaké kurzy směrovat na provést ručně.

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a>Cloud – více ověřování organizace

![Více ověřování organizace](creating-web-projects-in-visual-studio/_static/image25.png)

Tuto možnost zvolte, pokud chcete povolit ověřování pro uživatelské účty, které jsou definovány v několik služeb Azure AD [tenantů](https://technet.microsoft.com/library/jj573650.aspx). Například web je contoso.com a ho bude k dispozici zaměstnanci společnosti Contoso, kteří jsou v tenantovi contoso.onmicrosoft.com a zaměstnanci společnosti Fabrikam, kteří jsou v tenantovi fabrikam.onmicrosoft.com.

Nastavení, která jste zadali a zřizování krok aplikace jsou podobné [jednu organizaci ověřování](#orgauthsingle).

Informace o tom, jak vytvářet aplikace, které používají **Cloud – víc organizací** ověřování, najdete v následujících zdrojích informací:

- [Snadná integrace webové aplikace pomocí Azure Active Directory, ASP.NET &amp; sady Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) na blogu týmu Active Directory.
- [Vývoj webových aplikací s více Tenanty s Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) kurzu. Tento kurz se ještě neaktualizoval pro Visual Studio 2013 v sadě Visual Studio 2013 je automatické některé co tento kurz vás směruje k provést ručně.
- [Budete muset zaregistrovat pomocí vlastních více organizací ASP.NET aplikací předtím, než se můžete přihlásit](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/). Blog autorem je Vittorio Bertocci, který vysvětluje, jak řešit běžné potíže lidé dojde při vytvoření projektu, který používá organizace služby Multi-Factor authentication.

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a>On-Premises ověřování organizace

![Místní ověřování organizace](creating-web-projects-in-visual-studio/_static/image26.png)

Tuto možnost zvolte, pokud chcete povolit ověřování pro uživatelské účty, které jsou definovány v systému Windows Server Active Directory (AD), a nechcete používat Azure AD. Tato možnost slouží k vytvoření intranetový server nebo web na Internetu. Pro web na Internetu poskytují přístup ke službě AD pomocí Active Directory Federation Services (ADFS). Další informace najdete v tématu [pomocí On-Premises organizační ověřování možnost (služby AD FS) pomocí technologie ASP.NET v sadě Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).

Pro intranetový server, jako alternativu můžete zvolit [ověřování Windows](#winauth) místo tuto možnost. Pro možnost ověřování Windows není nutné zadat adresu URL dokumentu metadat. Ale ověřování Windows není nabízejí tyto možnosti pro řízení přístupu aplikace ve službě Active Directory nebo k datům adresáře v dotazu.

#### <a name="on-premises-authority"></a>Místní autorita

Zadejte adresu URL, která odkazuje na metadata dokumentů. Dokument metadat obsahuje souřadnice autority. Aplikace bude Tyhle souřadnice používat k řízení toku webové přihlášení.

#### <a name="application-id-uri"></a>Identifikátor URI ID aplikace

Zadejte jedinečný identifikátor URI, AD můžete identifikovat tuto aplikaci, nebo nechte prázdné, aby Visual Studio, vytvořte si ho.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Další kroky

Tento dokument poskytuje základní pomoc pro vytvoření nového webového projektu ASP.NET v sadě Visual Studio 2013. Další informace o používání sady Visual Studio pro vývoj webů, najdete v části [ https://www.asp.net/visual-studio/ ](../../index.md).
