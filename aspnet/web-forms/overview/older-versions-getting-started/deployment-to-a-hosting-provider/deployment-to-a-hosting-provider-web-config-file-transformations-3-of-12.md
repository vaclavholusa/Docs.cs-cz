---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: transformace souboru Web.Config - 3 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: a5afcf4d05d6fa51ad70f7e5bdfafd20002f188a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841755"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: transformace souboru Web.Config - 3 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak automatizovat proces změny *Web.config* souboru, když ji nasadíte do jiné cílové prostředí. Většina aplikací má nastavení *Web.config* soubor, který musí být odlišné, když je aplikace nasazená. Automatizace procesu provedení těchto změn je udržuje od nutnosti je ručně pokaždé, když nasadíte, které by být zdlouhavé a náchylné k chybám.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformace souboru Web.config a webové nasazení parametry

Existují dva způsoby, jak automatizovat proces změny *Web.config* nastavení souboru: [transformace Web.config](https://msdn.microsoft.com/library/dd465326.aspx) a [Webdeploy parametry](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* transformační soubor obsahuje kód XML, který určuje, jak změnit *Web.config* souboru po jeho nasazení. Můžete zadat jiné změny pro konkrétní konfiguraci sestavení a pro konkrétní profily publikování. Výchozí konfigurace sestavení jsou ladění a vydání a můžete vytvořit vlastní konfigurace sestavení. Profil publikování se obvykle odpovídá cílové prostředí. (Se dozvíte více o publikační profily v [nasazení do služby IIS jako testovacího prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurz.)

Parametry pro nasazení webu je možné určit různé druhy nastavení, která se musí nakonfigurovat během nasazení, včetně nastavení, které se nacházejí v *Web.config* soubory. Pokud se použije k určení *Web.config* změny souborů, jsou mnohem složitější a nastavit parametry nástroj nasazení webu, ale jsou užitečné, pokud si nejste jisti hodnota k nastavení, dokud nasadíte. Například v podnikovém prostředí, může vytvořit *balíček pro nasazení* dát lidem v oddělení IT k instalaci v produkčním prostředí a musí být možné zadávat připojovací řetězce a hesla, které nechcete tuto osobu vědět.

Pro scénáře, který tento kurz se zabývá, víte, vše, co musíme udělat, aby *Web.config* souboru, takže není potřeba používat parametry nasazení webu. Některé transformace, která se liší v závislosti na konfiguraci sestavení používá, a některé, které se liší v závislosti na profil publikování použitý budete konfigurovat.

## <a name="creating-transformation-files-for-publish-profiles"></a>Vytváření souborů transformace pro profily publikování

V **Průzkumníku řešení**, rozbalte *Web.config* zobrazíte *Web.Debug.config* a *Web.Release.config* soubory transformace Vytvoří se standardně pro dvě výchozí konfigurace sestavení.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Můžete vytvořit soubory transformace pro vlastní konfigurace sestavení tak, že kliknete pravým tlačítkem soubor Web.config a zvolíte **přidat konfigurační transformaci** v místní nabídce, ale pro účely tohoto kurzu není nutné to udělat.

Potřebujete dva další soubory transformace pro konfiguraci změny, které se vztahují na cíl nasazení, nikoli na konfiguraci sestavení. Typickým příkladem tento typ nastavení je koncový bod WCF, které se liší pro testovací a produkční. V dalších kurzech vytvoříte publikační profily s názvem testovacím i produkčním prostředí, takže je třeba *Web.Test.config* souboru a *Web.Production.config* souboru.

Transformační soubory, které jsou svázány se publikační profily musí být vytvořeny ručně. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a vyberte **otevřít složku v Průzkumníku Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

V **Windows Explorer**, vyberte *Web.Release.config* soubor, zkopírujte soubor a vložte dvě kopie. Přejmenovat tyto kopie *Web.Production.config* a *Web.Test.config*, pak zavřete **Windows Explorer**.

V **Průzkumníka řešení**, klikněte na tlačítko **aktualizovat** zobrazíte nové soubory.

Vyberte nové soubory, klikněte pravým tlačítkem a pak klikněte na tlačítko **zahrnout do projektu** v místní nabídce.

![Včetně testovací a produkční konfigurační soubory v projektu](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Pokud chcete zakázat tyto soubory nasazení, vyberte je v **Průzkumníka řešení**a pak v **vlastnosti** okno Změnit **akce sestavení** vlastnost z **Obsahu** k **žádný**. (Soubory transformace, které jsou založené na konfiguraci sestavení automaticky bránit v nasazení.)

Nyní jste připraveni k zadání *Web.config* transformace do *Web.config* soubory transformace.

## <a name="limiting-error-log-access-to-administrators"></a>Omezení přístupu k protokolu chyb pro správce

Pokud dojde k chybě, když je aplikace spuštěna, aplikace zobrazí obecná chybová stránka místo generována chybová stránka a používá [balíček NuGet knihovny Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) chyby protokolování a vytváření sestav. `customErrors` Prvek *Web.config* soubor Určuje chybové stránky:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Pokud chcete zobrazit chybovou stránku, dočasně změnit `mode` atribut `customErrors` element z "RemoteOnly" na "Na" a spuštění aplikace ze sady Visual Studio. Způsobit chybu, můžete si vyžádat neplatnou adresu URL, jako například *Studentsxxx.aspx*. Místo chybová stránka služby IIS vygeneruje "stránka nebyla nalezena", zobrazí *GenericErrorPage.aspx* stránky.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Chcete-li naleznete v protokolu chyb, všechno v adrese URL nahradit za číslo portu se *elmah.axd* (například v na snímku obrazovky `http://localhost:51130/elmah.axd`) a stiskněte klávesu Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Nezapomeňte nastavit `customErrors` element zpátky do režimu "RemoteOnly" až budete hotovi.

Na počítači pro vývoj je vhodné povolit bezplatný přístup na chybovou stránku protokolu, ale v produkčním prostředí, která bude představovat bezpečnostní riziko. Pro produkční lokality můžete přidat autorizační pravidlo, která omezuje přístup protokolu chyba jen správcům nakonfigurováním transformace *Web.Production.config* souboru.

Otevřít *Web.Production.config* a přidat nový `location` ihned za úvodní prvek `configuration` označit, jak je znázorněno zde. (Ujistěte se, že můžete přidat pouze `location` elementu a není okolního značek, která je znázorněna zde jenom k poskytnutí některých kontextu.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` Hodnotu atributu "Vložit" způsobí, že to `location` element na stejné úrovni jako přidat do jakékoli existující `location` prvků v *Web.config* souboru. (Již existuje jedna `location` prvek, který určuje autorizační pravidla pro **aktualizace kredity** stránky.) Při testování v produkčním po nasazení budete testovat ověřte, že toto autorizační pravidlo platí.

Není nutné k omezení přístupu protokolu chyby v testovacím prostředí, takže není nutné přidat tento kód *Web.Test.config* souboru.

> [!NOTE] 
> 
> **Poznámka k zabezpečení** nikdy zobrazit podrobnosti o chybě veřejně v produkční aplikace, nebo ukládání těchto informací na veřejném místě. Útočníci slouží ke zjišťování ohrožení zabezpečení v lokalitě informace o chybě. Pokud používáte ELMAH ve své aplikaci, je potřeba prozkoumat způsoby, ve kterém by šlo ELMAH minimalizovat rizika zabezpečení. Doporučená konfigurace by neměly být zahrnuté v ELMAH příkladu v tomto kurzu. To je příklad, který jste vybrali, aby bylo možné ukazují, jak zpracovat složku, musí být schopen vytvořit soubory v aplikaci.


## <a name="setting-an-environment-indicator"></a>Nastavení prostředí indikátor

Běžný scénář, kdy je, aby *Web.config* soubor nastavení, která musí být v každém prostředí, který nasadíte do jiné. Například aplikace, která volá službu WCF může potřebovat jiný koncový bod v testovacím a produkčním prostředí. Aplikace Contoso University zahrnuje také nastavení tohoto druhu. Toto nastavení řídí viditelné ukazatele na stránkách webu, který říká prostředí, ve kterém pracujete, jako je například vývojové, testovací nebo produkční prostředí. Hodnota nastavení určuje, zda se aplikace připojí "(vývoj)" nebo "(testovací)" na hlavní nadpis *Site.Master* hlavní stránky:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Indikátor prostředí je vynecháno, pokud je aplikace spuštěna v produkčním prostředí.

Webové stránky společnosti Contoso University přečíst hodnotu, která je nastavena v `appSettings` v *Web.config* souboru, aby bylo možné určit, jaké prostředí je aplikace spuštěna v:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Hodnota musí být "Test" v testovacím prostředí a "Produkční" v provozním prostředí.

Otevřít *Web.Production.config* a přidat `appSettings` element bezprostředně před otevírací značce `location` elementu, které jste přidali dříve:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform` Hodnotu "SetAttributes" označuje, že účel této transformace je chcete-li změnit hodnoty atributů z existujícího prvku v atributu *Web.config* souboru. `xdt:Locator` Atribut hodnotu "Match(key)" označuje, že elementu, který chcete upravit, jehož `key` atribut shody `key` tady zadán atribut. Jenom ostatní atribut `add` element je `value`, a to je, co se změnilo v nasazených *Web.config* souboru. Tento kód způsobí, že `value` atribut `Environment` `appSettings` element být nastavena na "Produkční" *Web.config* soubor, který se nasadí do produkčního prostředí.

Dále použijte stejnou změnu pro *Web.Test.config* souboru, s výjimkou sadu `value` na "Test" místo "Produkční". Jakmile budete hotovi, `appSettings` tématu *Web.Test.config* bude vypadat jako v následujícím příkladu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Zakázat režim ladění

Pro sestavení pro vydání nechcete, aby povoleno ladění bez ohledu na prostředí, ve kterém nasazujete. Ve výchozím nastavení *Web.Release.config* transformační soubor se automaticky vytvoří s kódem, který odebere `debug` atribut z `compilation` element:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform` Atribut způsobí, že `debug` atribut vynechat z nasazených *Web.config* souboru pokaždé, když nasadíte sestavení pro vydání.

Tento stejný transformace je v provozním i testovacím prostředí soubory transformace vzhledem k tomu, že jste vytvořili zkopírováním verze souboru transformace. Nepotřebujete ho existuje duplicitní soubory s každou z těchto proto si otevřete, odeberte **kompilace** prvek a soubor uložte a zavřete každou.

## <a name="setting-connection-strings"></a>Nastavení připojovacích řetězců

Ve většině případů nepotřebujete nastavit připojovací řetězec transformací, protože připojovací řetězce můžete zadat v profilu publikování. Ale dojde k výjimce při nasazení databáze SQL Server Compact a použití migrace Entity Framework Code First aktualizace databáze na cílovém serveru. V tomto scénáři budete muset zadat další připojovací řetězec, který se použije pro aktualizaci schématu databáze na serveru. Chcete-li nastavit tuto transformaci, přidejte **&lt;connectionStrings&gt;** ihned za úvodní prvek **&lt;konfigurace&gt;** značky v obou *Web.Test.config* a *Web.Production.config* transformační soubory:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform` Atribut určuje, že tento připojovací řetězec se přidají do *connectionStrings* element v nasazených *Web.config* souboru. (Procesu publikování tento další připojovací řetězec automaticky vytvoří za vás, pokud neexistuje, ale ve výchozím nastavení **providerName** získá atribut nastaven na `System.Data.SqlClient`, což nefunguje pro SQL Server Compact. Ručním přidáním připojovací řetězec, můžete zabránit proces nasazení vytváření element připojovacího řetězce s názvem chybě zprostředkovatele.)

Nyní všechny zadáte *Web.config* transformace, které potřebujete pro nasazení aplikace Contoso University k testování a produkce. V následujícím kurzu se bude starat o úlohy nastavení nasazení, které vyžadují nastavení vlastností projektu.

## <a name="more-information"></a>Další informace

Další informace o tématech, které jsou popsané v tomto kurzu, najdete v článku scénář transformace souboru Web.config v [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [další](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
