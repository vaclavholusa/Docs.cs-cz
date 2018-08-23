---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: transformace souboru Web.config | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 58f3daac5e9fbe6d327f19b6ae78ea17a26cd7e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753269"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: transformace souboru Web.config
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak automatizovat proces změny *Web.config* souboru, když ji nasadíte do jiné cílové prostředí. Většina aplikací má nastavení *Web.config* soubor, který musí být odlišné, když je aplikace nasazená. Automatizace procesu provedení těchto změn je udržuje od nutnosti je ručně pokaždé, když nasadíte, které by být zdlouhavé a náchylné k chybám.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformace souboru Web.config a parametry nasazení webu

Existují dva způsoby, jak automatizovat proces změny *Web.config* nastavení souboru: [transformace Web.config](https://msdn.microsoft.com/library/dd465326.aspx) a [Webdeploy parametry](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* transformační soubor obsahuje kód XML, který určuje, jak změnit *Web.config* souboru po jeho nasazení. Můžete zadat jiné změny pro konkrétní konfiguraci sestavení a pro konkrétní profily publikování. Výchozí konfigurace sestavení jsou ladění a vydání a můžete vytvořit vlastní konfigurace sestavení. Profil publikování se obvykle odpovídá cílové prostředí. (Se dozvíte více o publikační profily v [nasazení do služby IIS jako testovacího prostředí](deploying-to-iis.md) kurz.)

Parametry pro nasazení webu je možné určit různé druhy nastavení, která se musí nakonfigurovat během nasazení, včetně nastavení, které se nacházejí v *Web.config* soubory. Pokud se použije k určení *Web.config* změny souborů, jsou mnohem složitější a nastavit parametry nástroj nasazení webu, ale jsou užitečné, pokud si nejste jisti hodnota k nastavení, dokud nasadíte. Například v podnikovém prostředí, může vytvořit *balíček pro nasazení* dát lidem v oddělení IT k instalaci v produkčním prostředí a musí být možné zadávat připojovací řetězce a hesla, které nechcete tuto osobu vědět.

Pro scénáře, který se popisuje v této sérii kurzů, můžete předem vědět vše, co musíme udělat, aby *Web.config* souboru, takže není potřeba používat parametry nasazení webu. Některé transformace, která se liší v závislosti na konfiguraci sestavení používá, a některé, které se liší v závislosti na profil publikování použitý budete konfigurovat.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Zadání nastavení v souboru Web.config v Azure

Pokud *Web.config* soubor nastavení, které chcete změnit `<connectionStrings>` nebo `<appSettings>` elementu, a pokud provádíte nasazení do služby Web Apps ve službě Azure App Service, existuje další možnost pro automatizaci změny během nasazení. Můžete zadat nastavení, které mají platit v Azure in **konfigurovat** kartu stránce portálu pro správu pro vaši webovou aplikaci (přejděte dolů k položce **nastavení aplikace** a **připojovacích řetězců**  oddíly). Když nasadíte projekt, Azure automaticky tyto změny implementuje. Další informace najdete v tématu [weby Windows Azure: fungování řetězců aplikace a připojovacích řetězců](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Výchozí soubory transformace

V **Průzkumníku řešení**, rozbalte *Web.config* zobrazíte *Web.Debug.config* a *Web.Release.config* soubory transformace Vytvoří se standardně pro dvě výchozí konfigurace sestavení.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Můžete vytvořit soubory transformace pro vlastní konfigurace sestavení tak, že kliknete pravým tlačítkem soubor Web.config a zvolíte **přidat konfigurační transformaci** v místní nabídce. Pro účely tohoto kurzu není nutné to udělat, a možnost nabídky je zakázán, protože jste ještě nevytvořili žádné konfigurace vlastního sestavení.

Později vytvoříte tři další transformační soubory, jeden pro testování, Fázování a produkce publikační profily. Typickým příkladem takového nastavení, které by v souboru transformace profilu publikování zpracovat, protože závisí na cílovém prostředí je koncového bodu WCF, které se liší pro testovací a produkční. Vytvoříte publikovat soubory transformace profilu v dalších kurzech po vytvoření profilů publikování, odpovídající s.

## <a name="disable-debug-mode"></a>Zakázat režim ladění

Příklad nastavení, které závisí na konfiguraci sestavení místo cílové prostředí je `debug` atribut. Pro sestavení pro vydání obvykle je vhodné zakázáno ladění bez ohledu na prostředí, ve kterém nasazujete. Proto ve výchozím nastavení sady Visual Studio vytvořit šablony projektů *Web.Release.config* transformační soubory s kódem, který odebere `debug` atribut z `compilation` elementu. Tady je výchozí *Web.Release.config*: kromě ukázkový kód transformace, která je zakomentovaný, obsahuje kód v `compilation` element, který odebere `debug` atribut:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"` Určuje atribut, který má `debug` atribut má být odebrán z `system.web/compilation` element v nasazených *Web.config* souboru. To se provede při každém nasazení sestavení pro vydání.

## <a name="limit-error-log-access-to-administrators"></a>Omezit přístup k protokolu chyb správci

Pokud dojde k chybě, když je aplikace spuštěna, aplikace zobrazí obecná chybová stránka místo generována chybová stránka a používá [balíček NuGet knihovny Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) chyby protokolování a vytváření sestav. `customErrors` v aplikaci prvku *Web.config* soubor Určuje chybové stránky:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Pokud chcete zobrazit chybovou stránku, dočasně změnit `mode` atribut `customErrors` element z "RemoteOnly" na "Na" a spuštění aplikace ze sady Visual Studio. Způsobit chybu, můžete si vyžádat neplatnou adresu URL, jako například *Studentsxxx.aspx*. Namísto chyby generované služby IIS "prostředek nebyl nalezen" stránce se zobrazí *GenericErrorPage.aspx* stránky.

![Chybová stránka](web-config-transformations/_static/image2.png)

Chcete-li naleznete v protokolu chyb, všechno v adrese URL nahradit za číslo portu se *elmah.axd* (například `http://localhost:51130/elmah.axd`) a stiskněte klávesu Enter:

![ELMAH stránky](web-config-transformations/_static/image3.png)

Nezapomeňte nastavit `customErrors` element zpátky do režimu "RemoteOnly" až budete hotovi.

Na počítači pro vývoj je vhodné povolit bezplatný přístup na chybovou stránku protokolu, ale v produkčním prostředí, která bude představovat bezpečnostní riziko. Pro produkční lokality, chcete přidat autorizační pravidlo, která omezuje přístup k protokolu chyb pro správce a ujistěte se, že funguje omezení je vhodné do testovacího a přípravného také. Proto to je jiná změna, kterou chcete implementovat pokaždé, když nasadíte sestavení pro vydání, a proto patří *Web.Release.config* souboru.

Otevřít *Web.Release.config* a přidat nový `location` element bezprostředně před uzavírací `configuration` označit, jak je znázorněno zde.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` Hodnotu atributu "Vložit" způsobí, že to `location` element na stejné úrovni jako přidat do jakékoli existující `location` prvků v *Web.config* souboru. (Již existuje jedna `location` prvek, který určuje autorizační pravidla pro **aktualizace kredity** stránky.)

Nyní můžete zobrazit náhled transformace, abyste měli jistotu, že programový ji správně.

V **Průzkumníka řešení**, klikněte pravým tlačítkem na *Web.Release.config* a klikněte na tlačítko **náhled transformace**.

![Náhled transformace nabídky](web-config-transformations/_static/image4.png)

Otevře se stránka, které zobrazují vývoj *Web.config* souboru na levé straně a co nasazených *Web.config* soubor bude vypadat jako na pravé straně, se zvýrazněným změnami.

![Náhled transformace ladění](web-config-transformations/_static/image5.png)

![Náhled transformace umístění](web-config-transformations/_static/image6.png)

(Ve verzi preview, můžete si všimnout některé další změny, které jste nenapsali transformuje pro: tyto obvykle zahrnují odebrání prázdný znak, který nemá vliv na funkčnost.)

Při testování webu po nasazení budete také testovat k ověření, že toto autorizační pravidlo platí.

> [!NOTE] 
> 
> **Poznámka k zabezpečení** nikdy zobrazit podrobnosti o chybě veřejně v produkční aplikace, nebo ukládání těchto informací na veřejném místě. Útočníci slouží ke zjišťování ohrožení zabezpečení v lokalitě informace o chybě. Pokud používáte ELMAH ve své aplikaci, nakonfigurujte ELMAH minimalizovat rizika zabezpečení. Doporučená konfigurace by neměly být zahrnuté v ELMAH příkladu v tomto kurzu. To je příklad, který jste vybrali, aby bylo možné ukazují, jak zpracovat složku, musí být schopen vytvořit soubory v aplikaci. Další informace najdete v tématu [zabezpečení koncového bodu ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Nastavení, která bude zpracovávat v soubory transformace profil publikování

Běžný scénář, kdy je, aby *Web.config* soubor nastavení, která musí být v každém prostředí, který nasadíte do jiné. Například aplikace, která volá službu WCF může potřebovat jiný koncový bod v testovacím a produkčním prostředí. Aplikace Contoso University zahrnuje také nastavení tohoto druhu. Toto nastavení řídí viditelné ukazatele na stránkách webu, který říká prostředí, ve kterém pracujete, jako je například vývojové, testovací nebo produkční prostředí. Hodnota nastavení určuje, zda se aplikace připojí "(vývoj)" nebo "(testovací)" na hlavní nadpis *Site.Master* hlavní stránky:

![Indikátor prostředí](web-config-transformations/_static/image7.png)

Indikátor prostředí je vynecháno, pokud je aplikace spuštěna v přípravném nebo produkčním prostředí.

Webové stránky společnosti Contoso University přečíst hodnotu, která je nastavena v `appSettings` v *Web.config* souboru, aby bylo možné určit, jaké prostředí je aplikace spuštěna v:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Hodnota by měla být "Test" v testovacím prostředí a "Produkční" pro přípravným a produkčním prostředím.

Následující kód do souboru transformace implementuje Tato transformace:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform` Hodnotu "SetAttributes" označuje, že účel této transformace je chcete-li změnit hodnoty atributů z existujícího prvku v atributu *Web.config* souboru. `xdt:Locator` Atribut hodnotu "Match(key)" označuje, že elementu, který chcete upravit, jehož `key` atribut shody `key` tady zadán atribut. Jenom ostatní atribut `add` element je `value`, a to je, co se změnilo v nasazených *Web.config* souboru. Kód zobrazený zde způsobí, že `value` atribut `Environment` `appSettings` element být nastavena na "Test" *Web.config* soubor, který je nasazen.

Tato transformace patří transformace soubory profilu publikování, které jste ještě nevytvořili. Můžete vytvářet a aktualizovat soubory transformace, které implementují tato změna při vytváření profilů publikování pro test, testovací a produkční prostředí. Můžete to udělat [nasazení do služby IIS](deploying-to-iis.md) a [nasazení do produkčního prostředí](deploying-to-production.md) kurzy.

> [!NOTE]
> Protože toto nastavení se `<appSettings>` element, můžete mít Další alternativou k určení transformace při nasazování do služby Web Apps v Azure App Service naleznete v tématu [Web.config zadání nastavení v Azure](#watransforms) výše v v tomto tématu.


## <a name="setting-connection-strings"></a>Nastavení připojovacích řetězců

I když výchozí transformační soubor obsahuje příklad, který ukazuje, jak aktualizovat připojovací řetězec, ve většině případů nepotřebujete nastavit připojovací řetězec transformací, protože připojovací řetězce můžete zadat v profilu publikování. Můžete to udělat [nasazení do služby IIS](deploying-to-iis.md) a [nasazení do produkčního prostředí](deploying-to-production.md) kurzy.

## <a name="summary"></a>Souhrn

Nyní jste provedli tolik, je možné s *Web.config* transformace předtím, než vytvoříte profily publikování a už víte, co se bude v nasazeném souboru Web.config ve verzi preview.

![Náhled transformace umístění](web-config-transformations/_static/image8.png)

V následujícím kurzu se bude starat o úlohy nastavení nasazení, které vyžadují nastavení vlastností projektu.

## <a name="more-information"></a>Další informace

Další informace o tématech, které jsou popsané v tomto kurzu, najdete v části [transformace Web.config pomocí změnit nastavení v cílovém souboru Web.config nebo app.config souboru během nasazování](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) v obsahu mapy webové nasazení pro Visual Studio a ASP.NET.

> [!div class="step-by-step"]
> [Předchozí](preparing-databases.md)
> [další](project-properties.md)
