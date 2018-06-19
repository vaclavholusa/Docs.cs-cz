---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: transformace souboru Web.Config - 3 12 | Microsoft Docs'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 86eb74ca35e8804978127412e2276eeee9d615dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888133"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: transformace souboru Web.Config - 3 12
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak automatizovat proces změny *Web.config* souboru při jeho nasazení do jiné cílové prostředí. Většina aplikací mít nastavení v *Web.config* soubor, který musí být odlišné, když je aplikace nasazená. Automatizace procesu provedení těchto změn nebude nutné udělat je ručně pokaždé, když nasadíte, který bude zdlouhavé a náchylné k chybám chybu.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformace Web.config versus webové nasazení parametry

Existují dva způsoby, jak automatizovat proces změny *Web.config* nastavení souboru: [transformace Web.config](https://msdn.microsoft.com/library/dd465326.aspx) a [Web Deploy parametry](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* transformace soubor obsahuje kód XML, který určuje, jak změnit *Web.config* souboru při nasazení. Můžete zadat různé změny pro konkrétní konfigurace sestavení a pro určité publikační profily. Výchozí konfigurace sestavení jsou ladění a vydání, a můžete vytvořit vlastní konfigurace sestavení. Profil publikování se obvykle odpovídá cílové prostředí. (Se dozvíte další informace o publikování profily v nástroji [nasazení do IIS jako testovacím prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu.)

Parametry nasazení webové lze použít k určení různých druhů nastavení, které musí být nakonfigurované během nasazení, včetně nastavení, které se nacházejí v *Web.config* soubory. Pokud se používá k určení *Web.config* změny souborů, nasazení webu parametry jsou složitější nastavit, ale jsou užitečné v případě, že si nejste jisti, hodnota k nastavení, dokud nasazení. Například v podnikovém prostředí, může vytvořit *balíček pro nasazení* a poskytnout osobě v oddělení IT k instalaci v produkčním prostředí, a tento uživatel má být schopni zadejte připojovací řetězce nebo hesla, které nechcete znáte.

Scénář, který tento kurz se zaměřuje, víte, vše, co je třeba provést *Web.config* souboru, takže nemusíte používat parametry nasazení webu. Některé transformace, které se liší v závislosti na konfiguraci sestavení použít, a některé, který se liší v závislosti na profil publikování použitý budete konfigurovat.

## <a name="creating-transformation-files-for-publish-profiles"></a>Vytváření souborů transformace profilů publikování

V **Průzkumníku řešení**, rozbalte položku *Web.config* zobrazíte *Web.Debug.config* a *Web.Release.config* transformace souborů jsou vytvořeny ve výchozím nastavení pro dvě výchozí konfigurace sestavení.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Transformace souborů pro vlastní konfigurace sestavení můžete vytvořit tak, že kliknete pravým tlačítkem na soubor Web.config a výběr **přidat konfigurační transformaci** v místní nabídce, ale pro účely tohoto kurzu nemusíte to udělat.

Dva další soubory transformace, potřebujete pro konfiguraci změny, které se vztahují k nasazení cílové místo na konfiguraci sestavení. Typickým příkladem tohoto druhu, nastavení je koncový bod WCF, které se liší pro testovací a produkční. V dalších kurzech vytvoříte publikační profily s názvem testovací a produkční, takže je nutné *Web.Test.config* souboru a *Web.Production.config* souboru.

Transformace souborů, které jsou svázané s publikační profily musí být vytvořeny ručně. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a vyberte **otevřít složku v Průzkumníku Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

V **Průzkumníka Windows**, vyberte *Web.Release.config* souboru, soubor zkopírujte a vložte dvě kopie. Přejmenujte tyto kopie *Web.Production.config* a *Web.Test.config*, pak zavřete **Průzkumníka Windows**.

V **Průzkumníku řešení**, klikněte na tlačítko **aktualizovat** zobrazíte nové soubory.

Vyberte nové soubory, klikněte pravým tlačítkem a pak klikněte na tlačítko **zahrnout do projektu** v místní nabídce.

![Včetně testovací a produkční konfigurační soubory v projektu](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Abyste zabránili nasazuje tyto soubory, vyberte je v **Průzkumníku řešení**a potom v **vlastnosti** okno Změnit **akce sestavení** vlastnost z **Obsahu** k **žádné**. (Transformace souborů, které jsou založeny na konfigurace sestavení jsou automaticky zabránila nasazení.)

Nyní jste připraveni k zadání *Web.config* transformace do *Web.config* transformace souborů.

## <a name="limiting-error-log-access-to-administrators"></a>Omezení přístupu k protokolu chyb pro správce

Pokud dojde k chybě při spuštění aplikace, aplikace se zobrazí na stránce Obecná chyba místo generována chybové stránky a používá [balíček Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) došlo k chybě protokolování a vytváření sestav. `customErrors` Element v *Web.config* soubor Určuje chybové stránky:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Pokud chcete zobrazit chybovou stránku, dočasně změnit `mode` atribut `customErrors` element z "RemoteOnly" na "On" a spusťte aplikaci v sadě Visual Studio. Způsobit chybu tím, že požádá neplatná adresa URL, například *Studentsxxx.aspx*. Místo generované služby IIS "stránka nebyla nalezena" chybovou stránku, uvidíte *GenericErrorPage.aspx* stránky.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Pokud chcete zobrazit v protokolu chyb, všechno, co je v adrese URL nahradit po číslo portu s *elmah.axd* (například v snímek obrazovky `http://localhost:51130/elmah.axd`) a stiskněte klávesu Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Nezapomeňte nastavit `customErrors` element zpátky do režimu "RemoteOnly" po dokončení.

Ve svém vývojovém počítači je vhodné povolit volné přístup chybovou stránku protokolu, ale v produkčním prostředí, které by bezpečnostní riziko. Pro produkční lokality, můžete přidat autorizační pravidlo, které omezují přístup protokolu chyby jenom správcům nakonfigurováním transformace v *Web.Production.config* souboru.

Otevřete *Web.Production.config* a přidejte nový `location` element ihned po otevření `configuration` značka, jak je vidět tady. (Ujistěte se, že přidáte pouze `location` elementu a není okolního značku, která se zde zobrazí pouze k zajištění některé kontextu.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` Hodnotu atributu "Insert" je to způsobeno `location` elementu, který chcete přidat na stejné úrovni jako do jakékoli existující `location` elementů v *Web.config* souboru. (Už existuje jedna `location` element, který určuje autorizační pravidla pro **aktualizace kredity** stránky.) Při testování v produkčním po nasazení, budete testovat ověřte, zda je toto autorizační pravidlo efektivní.

Nemáte omezit přístup protokolu chyb v testovacím prostředí, takže není nutné přidat tento kód, aby *Web.Test.config* souboru.

> [!NOTE] 
> 
> **Poznámka k zabezpečení** nikdy zobrazit podrobnosti o chybě veřejnosti v produkční aplikace nebo uložit tyto informace na veřejném místě. Útočníci můžete použít ke zjištění chyby zabezpečení v lokalitě informace o chybě. Pokud používáte ELMAH ve vaší vlastní aplikaci, je nutné prozkoumat způsoby, ve které je možné nakonfigurovat ELMAH na minimalizovat bezpečnostní rizika. Doporučená konfigurace by se neměla považovat ELMAH příklad v tomto kurzu. Je příklad, který jste vybrali k ukazují, jak zpracovat aplikace musí být možné vytvářet soubory ve složce.


## <a name="setting-an-environment-indicator"></a>Nastavení prostředí slouží jako ukazatel

Obvyklým scénářem je tak, aby měl *Web.config* nastavení, které musí být odlišné v každém prostředí, který nasadíte do souboru. Například aplikace, která volá služby WCF, může být nutné jinému koncovému bodu v testovací a produkční prostředí. Aplikace Contoso univerzity také zahrnuje nastavení tohoto druhu. Toto nastavení určuje, na viditelné ukazatel na stránkách lokality, dozvíte se prostředí, ve kterém jsou in, například vývoj, testovací nebo produkční. Hodnota nastavení určuje, zda se aplikace připojí "(vývoj)" nebo "(testovací)" na hlavní nadpis *Site.Master* hlavní stránky:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Indikátor prostředí je tento parametr vynechán, když je aplikace spuštěna v produkčním prostředí.

Webové stránky společnosti Contoso univerzity číst hodnotu, která je nastavena v `appSettings` v *Web.config* souboru, aby bylo možné určit, jaké prostředí je aplikace spuštěna v:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Hodnota musí být "Test" v testovacím prostředí a "Produkčnímu" v provozním prostředí.

Otevřete *Web.Production.config* a přidejte `appSettings` element bezprostředně před otevírací značce `location` elementu, které jste přidali dříve:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform` Hodnota "SetAttributes" označuje, že účel Tato transformace je ke změně hodnot atributů existující elementu v atributu *Web.config* souboru. `xdt:Locator` Atribut hodnota "Match(key)" označuje, že elementu, který chcete upravit, je ten, jehož `key` atribut odpovídá `key` atribut sem. Jenom jiné atribut `add` element je `value`, a co se změnilo v nasazené *Web.config* souboru. Tento kód způsobí, že `value` atribut `Environment` `appSettings` element být nastavena na "Produkčnímu" *Web.config* souboru, který je nasazený do produkčního prostředí.

Dále platí stejné změnu *Web.Test.config* souboru, s výjimkou sady `value` k "Test" místo "Produkčnímu". Až skončíte, `appSettings` kapitoly *Web.Test.config* bude vypadat jako v následujícím příkladu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Zakázat režim ladění

Pro sestavení pro vydání nechcete, aby povoleným laděním bez ohledu na prostředí, ve kterém provádíte nasazení. Ve výchozím nastavení *Web.Release.config* transformační soubor je automaticky vytvořen s kódem, který odebere `debug` atribut z `compilation` element:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform` Atribut příčiny `debug` atribut vynechává z nasazené *Web.config* souboru vždy, když nasazujete sestavení pro vydání.

Tento stejný transformaci je v testovací a produkční transformace souborů vzhledem k tomu, že jste vytvořili pomocí kopírování transformační soubor verzi. Nepotřebujete ho existuje duplicitní soubory s každou z nich proto si otevřete, odeberte **kompilace** elementu a uložte a zavřete každý soubor.

## <a name="setting-connection-strings"></a>Nastavení připojovacích řetězců

Ve většině případů není nutné nastavit připojovací řetězec transformace, protože můžete zadat připojovací řetězce v profilu publikování. Ale pokud nasazujete databázi systému SQL Server Compact, dojde k výjimce a používáte migrace Code First Entity Framework aktualizace databáze na cílovém serveru. V tomto scénáři budete muset určit další připojovací řetězec, který se použije pro aktualizaci schématu databáze na serveru. Pokud chcete nastavit Tato transformace, přidejte **&lt;connectionStrings&gt;** element ihned po otevření **&lt;konfigurace&gt;** značky v obou *Web.Test.config* a *Web.Production.config* transformace souborů:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform` Atribut určuje, že tento připojovací řetězec bude přidán do *connectionStrings* element v nasazené *Web.config* souboru. (Proces publikování tento další připojovací řetězec automaticky vytvoří za vás, pokud neexistuje, ale ve výchozím nastavení **providerName** získá atribut nastaven na `System.Data.SqlClient`, který není pro systém SQL Server Compact nefunguje. Ručním přidáním připojovací řetězec, můžete zabránit procesu nasazení vytváření element řetězce připojení s názvem chybě zprostředkovatele.)

Jste nyní zadali všechny *Web.config* transformace, které potřebujete pro nasazení aplikace Contoso univerzity pro testovací a produkční. V následujícím kurzu budete postará o úkolech nasazení, které vyžadují nastavení vlastností projektu.

## <a name="more-information"></a>Další informace

Další informace o tématech, které jsou zahrnuté v tomto kurzu, najdete v části scénář transformaci Web.config v [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [další](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
