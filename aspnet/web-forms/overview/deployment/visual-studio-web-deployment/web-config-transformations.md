---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: transformace souboru Web.config | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a526275d76618c325a6b00f33cc550f28ab0cc00
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: transformace souboru Web.config
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak automatizovat proces změny *Web.config* souboru při jeho nasazení do jiné cílové prostředí. Většina aplikací mít nastavení v *Web.config* soubor, který musí být odlišné, když je aplikace nasazená. Automatizace procesu provedení těchto změn nebude nutné udělat je ručně pokaždé, když nasadíte, který bude zdlouhavé a náchylné k chybám chybu.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformace Web.config versus parametry nasazení webu

Existují dva způsoby, jak automatizovat proces změny *Web.config* nastavení souboru: [transformace Web.config](https://msdn.microsoft.com/library/dd465326.aspx) a [Web Deploy parametry](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* transformace soubor obsahuje kód XML, který určuje, jak změnit *Web.config* souboru při nasazení. Můžete zadat různé změny pro konkrétní konfigurace sestavení a pro určité publikační profily. Výchozí konfigurace sestavení jsou ladění a vydání, a můžete vytvořit vlastní konfigurace sestavení. Profil publikování se obvykle odpovídá cílové prostředí. (Se dozvíte další informace o publikování profily v nástroji [nasazení do IIS jako testovacím prostředí](deploying-to-iis.md) kurzu.)

Parametry nasazení webové lze použít k určení různých druhů nastavení, které musí být nakonfigurované během nasazení, včetně nastavení, které se nacházejí v *Web.config* soubory. Pokud se používá k určení *Web.config* změny souborů, nasazení webu parametry jsou složitější nastavit, ale jsou užitečné v případě, že si nejste jisti, hodnota k nastavení, dokud nasazení. Například v podnikovém prostředí, může vytvořit *balíček pro nasazení* a poskytnout osobě v oddělení IT k instalaci v produkčním prostředí, a tento uživatel má být schopni zadejte připojovací řetězce nebo hesla, které nechcete znáte.

Scénář, který obsahuje tento kurz řady, víte, předem vše, co je třeba provést *Web.config* souboru, takže nemusíte používat parametry nasazení webu. Některé transformace, které se liší v závislosti na konfiguraci sestavení použít, a některé, který se liší v závislosti na profil publikování použitý budete konfigurovat.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Zadání nastavení v souboru Web.config v Azure

Pokud *Web.config* nastavení souborů, které chcete změnit jsou v `<connectionStrings>` nebo `<appSettings>` elementu, a Pokud nasazujete do webové aplikace v Azure App Service, existuje další možnost pro automatizaci změny během nasazení. Můžete zadat nastavení, které chcete projeví v Azure v **konfigurace** správy portálu stránky pro webovou aplikaci (přejděte dolů k položce **nastavení aplikace** a **připojovací řetězce**  části). Při nasazení projektu, Azure automaticky provede změny. Další informace najdete v tématu [weby systému Windows Azure: fungování řetězců aplikace a připojovacích řetězců](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Výchozí soubory transformace

V **Průzkumníku řešení**, rozbalte položku *Web.config* zobrazíte *Web.Debug.config* a *Web.Release.config* transformace souborů jsou vytvořeny ve výchozím nastavení pro dvě výchozí konfigurace sestavení.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Transformace souborů pro vlastní konfigurace sestavení můžete vytvořit tak, že kliknete pravým tlačítkem na soubor Web.config a výběr **přidat konfigurační transformaci** v místní nabídce. V tomto kurzu nemusíte k tomu a možnost nabídky je zakázána, protože jste nevytvořili žádné sestavení vlastní konfigurace.

Později vytvoříte tři další transformace soubory, jeden pro test, pracovní a provozní publikační profily. Typickým příkladem takového nastavení, které by v transformaci souboru profilu publikování zpracovat, protože závisí na cílovém prostředí je koncový bod WCF, které se liší pro testovací a produkční. Vytvoříte publikování souborů transformace profilu v dalších kurzech po vytvoření profilů publikování, které přejde s.

## <a name="disable-debug-mode"></a>Zakázat režim ladění

Příklad nastavení, která je závislá na konfiguraci sestavení místo cílové prostředí je `debug` atribut. Pro sestavení pro vydání obvykle je vhodné zakázáno ladění bez ohledu na prostředí, ve kterém provádíte nasazení. Proto ve výchozím nastavení sady Visual Studio vytvoří projekt šablony *Web.Release.config* transformace soubory s kódem, který odebere `debug` atribut z `compilation` elementu. Tady je výchozí *Web.Release.config*: kromě ukázkový kód transformace, je označeno jako komentář, obsahuje kód v `compilation` element, který odebere `debug` atribut:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"` Atribut určuje, že chcete `debug` atribut má být odebrán z `system.web/compilation` element v nasazené *Web.config* souboru. To bude provedeno pokaždé, když nasazujete sestavení pro vydání.

## <a name="limit-error-log-access-to-administrators"></a>Omezit přístup protokolu chyby pro správce

Pokud dojde k chybě při spuštění aplikace, aplikace se zobrazí na stránce Obecná chyba místo generována chybové stránky a používá [balíček Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) došlo k chybě protokolování a vytváření sestav. `customErrors` Element v aplikaci *Web.config* soubor Určuje chybové stránky:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Pokud chcete zobrazit chybovou stránku, dočasně změnit `mode` atribut `customErrors` element z "RemoteOnly" na "On" a spusťte aplikaci v sadě Visual Studio. Způsobit chybu tím, že požádá neplatná adresa URL, například *Studentsxxx.aspx*. Místo chybu generované služby IIS "prostředek nebyl nalezen" stránky, uvidíte *GenericErrorPage.aspx* stránky.

![Chybové stránky](web-config-transformations/_static/image2.png)

Pokud chcete zobrazit v protokolu chyb, všechno, co je v adrese URL nahradit po číslo portu s *elmah.axd* (například `http://localhost:51130/elmah.axd`) a stiskněte klávesu Enter:

![ELMAH stránky](web-config-transformations/_static/image3.png)

Nezapomeňte nastavit `customErrors` element zpátky do režimu "RemoteOnly" po dokončení.

Ve svém vývojovém počítači je vhodné povolit volné přístup chybovou stránku protokolu, ale v produkčním prostředí, které by bezpečnostní riziko. Pro produkční lokality chcete přidat autorizační pravidlo, které omezují přístup k protokolu chyb správcům a ujistěte se, že funguje omezení je vhodné v testovacího a přípravného také. To je proto jiné změny, která chcete implementovat pokaždé, když nasazujete sestavení pro vydání, a tudíž patří v *Web.Release.config* souboru.

Otevřete *Web.Release.config* a přidejte nový `location` element bezprostředně před uzavírací `configuration` značka, jak je vidět tady.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` Hodnotu atributu "Insert" je to způsobeno `location` elementu, který chcete přidat na stejné úrovni jako do jakékoli existující `location` elementů v *Web.config* souboru. (Už existuje jedna `location` element, který určuje autorizační pravidla pro **aktualizace kredity** stránky.)

Nyní můžete zobrazit náhled transformace, abyste měli jistotu, že programový ji správně.

V **Průzkumníku řešení**, klikněte pravým tlačítkem na *Web.Release.config* a klikněte na tlačítko **transformace Preview**.

![Náhled transformace nabídky](web-config-transformations/_static/image4.png)

Otevře stránku, které ukazuje vývoj *Web.config* souboru na levé straně a co nasazené *Web.config* soubor bude vypadat jako na pravé straně, se zvýrazněnou změny.

![Verze Preview transformace ladění](web-config-transformations/_static/image5.png)

![Verze Preview umístění transformace](web-config-transformations/_static/image6.png)

(Ve verzi preview, můžete si všimnout, některé další změny, které nebylo zapsat transformuje pro: tyto obvykle zahrnují odebrání prázdné znaky, které nemají vliv na funkčnost.)

Při testování webu po nasazení, budete taky Testovat ověřte, zda je účinné autorizační pravidlo.

> [!NOTE] 
> 
> **Poznámka k zabezpečení** nikdy zobrazit podrobnosti o chybě veřejnosti v produkční aplikace nebo uložit tyto informace na veřejném místě. Útočníci můžete použít ke zjištění chyby zabezpečení v lokalitě informace o chybě. Pokud používáte ELMAH ve vaší vlastní aplikaci, nakonfigurujte ELMAH minimalizovat bezpečnostní rizika. Doporučená konfigurace by se neměla považovat ELMAH příklad v tomto kurzu. Je příklad, který jste vybrali k ukazují, jak zpracovat aplikace musí být možné vytvářet soubory ve složce. Další informace najdete v tématu [zabezpečení koncového bodu ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Nastavení, který bude zpracovávat v publikování souborů transformace profilu

Obvyklým scénářem je tak, aby měl *Web.config* nastavení, které musí být odlišné v každém prostředí, který nasadíte do souboru. Například aplikace, která volá služby WCF, může být nutné jinému koncovému bodu v testovací a produkční prostředí. Aplikace Contoso univerzity také zahrnuje nastavení tohoto druhu. Toto nastavení určuje, na viditelné ukazatel na stránkách lokality, dozvíte se prostředí, ve kterém jsou in, například vývoj, testovací nebo produkční. Hodnota nastavení určuje, zda se aplikace připojí "(vývoj)" nebo "(testovací)" na hlavní nadpis *Site.Master* hlavní stránky:

![Prostředí indikátoru](web-config-transformations/_static/image7.png)

Indikátor prostředí je tento parametr vynechán, když aplikace běží v pracovním nebo produkčním.

Webové stránky společnosti Contoso univerzity číst hodnotu, která je nastavena v `appSettings` v *Web.config* souboru, aby bylo možné určit, jaké prostředí je aplikace spuštěna v:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Hodnota by měla být "Test" v testovacím prostředí a "Produkčního" pro pracovní a provozní.

Následující kód v souboru transformace implementovat Tato transformace:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform` Hodnota "SetAttributes" označuje, že účel Tato transformace je ke změně hodnot atributů existující elementu v atributu *Web.config* souboru. `xdt:Locator` Atribut hodnota "Match(key)" označuje, že elementu, který chcete upravit, je ten, jehož `key` atribut odpovídá `key` atribut sem. Jenom jiné atribut `add` element je `value`, a co se změnilo v nasazené *Web.config* souboru. Kód tady uvedené příčiny `value` atribut `Environment` `appSettings` element být nastavena na "Test" *Web.config* souboru, který je nasazený.

Tato transformace patří transformace soubory profil publikování, které jste dosud nevytvořili. Můžete vytvářet a aktualizovat soubory transformace, které implementují tuto změnu při vytváření profilů publikování pro prostředí testovací, přípravné nebo produkční prostředí. Můžete to udělat [nasazení pro službu IIS](deploying-to-iis.md) a [nasadit do produkčního prostředí](deploying-to-production.md) kurzy.

> [!NOTE]
> Protože toto nastavení se `<appSettings>` element, můžete mít Další alternativou k určení transformace, pokud nasazujete do webové aplikace v Azure App Service najdete v části [Web.config zadání nastavení v Azure](#watransforms) výše v Toto téma.


## <a name="setting-connection-strings"></a>Nastavení připojovacích řetězců

I když výchozí transformační soubor obsahuje příklad, který ukazuje, jak aktualizovat připojovací řetězec, ve většině případů není nutné nastavit připojovací řetězec transformace, protože můžete zadat připojovací řetězce v profilu publikování. Můžete to udělat [nasazení pro službu IIS](deploying-to-iis.md) a [nasadit do produkčního prostředí](deploying-to-production.md) kurzy.

## <a name="summary"></a>Souhrn

Jste nyní provedli tolik, jako je možné s *Web.config* transformace předtím, než vytvoříte profilů publikování a jste se seznámili s náhled co bude v nasazeném souboru Web.config.

![Verze Preview umístění transformace](web-config-transformations/_static/image8.png)

V následujícím kurzu budete postará o úkolech nasazení, které vyžadují nastavení vlastností projektu.

## <a name="more-information"></a>Další informace

Další informace o tématech, které jsou zahrnuté v tomto kurzu, najdete v části [transformace pomocí souboru Web.config, chcete-li změnit nastavení v cílovém souboru Web.config nebo app.config souboru během nasazení](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) v nasazení webového obsahu mapy pro Visual Studio a ASP.NET.

>[!div class="step-by-step"]
[Předchozí](preparing-databases.md)
[další](project-properties.md)
