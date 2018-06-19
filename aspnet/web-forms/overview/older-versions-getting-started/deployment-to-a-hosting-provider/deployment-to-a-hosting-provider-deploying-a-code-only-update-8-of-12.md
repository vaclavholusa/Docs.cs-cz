---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace Code-Only – 8 12 | Microsoft Docs'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6305a8cb87d9b00b6bb4c4f8fa6114bddc4eb89f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886911"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace Code-Only – 8 12
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Po počátečním nasazení pokračuje v práci údržbu a vývoj vašeho webu a před dlouho bude chtít nasadit aktualizace. Tento kurz vás provede procesem nasazení aktualizace do kódu aplikace. Tato aktualizace nezahrnuje změnu databáze; Zobrazí se, co se liší o nasazení změn databáze v dalším kurzu.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Změna kódu

Jako jednoduchý příklad aktualizace do vaší aplikace, přidáte do **vyučující** stránky Seznam kurzů výukové podle vybrané lektorem.

Pokud spustíte **vyučující** stránky, můžete si všimnout, že existují **vyberte** odkazy v mřížce, ale přitom nedělají nic jakoukoli jinou hodnotu než zkontrolujte šedá řádek pozadí zapnout.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Nyní přidáte kód, který běží, když **vyberte** po kliknutí na odkaz a zobrazí seznam kurzů výukové podle vybrané lektorem.

V *Instructors.aspx*, přidejte následující kód bezprostředně po **ErrorMessageLabel** `Label` ovládacího prvku:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Spuštění stránky a vyberte lektorem. Zobrazí seznam kurzů výukové podle této lektorem.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Nasazení aktualizace kódu do testovacího prostředí

Nasazení do testovacího prostředí je jednoduché spuštění jedním kliknutím znovu publikovat. Chcete-li tento proces rychlejší, můžete použít **publikování webu jedním kliknutím** panelu nástrojů.

V **zobrazení** nabídce zvolte **panely nástrojů** a pak vyberte **publikování webu jedním kliknutím**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

V **Průzkumníku**, vyberte ContosoUniversity projekt.

**publikování webu jedním kliknutím** nástrojů, vyberte **Test** profil publikování a pak klikněte na **Publikovat Web** (ikona s dvojice šipek, které ukazují doleva a doprava).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio nasadí aktualizovanou aplikaci a prohlížeč se automaticky otevře na domovskou stránku. Spuštění stránky vyučující a vyberte lektorem k ověření, že aktualizace byla úspěšně nasazena.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Za normálních okolností byste také provést testování regrese (tedy testovací zbytek lokality a ujistěte se, že nové změny nebyla rozdělit žádné existující funkce). Ale pro účely tohoto kurzu budete tento krok přeskočit a přejít k aktualizace nasadit do produkčního prostředí.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Brání opakované nasazení počáteční stav databáze do produkčního prostředí

V reálné aplikaci uživatelé komunikovat s produkční lokality po počátečním nasazení a databáze se naplní dynamická data. Proto si chcete znovu zavést databázi členství v jeho počáteční stav, který by vymažou všechna data za provozu. Vzhledem k tomu, že jsou soubory v systému SQL Server Compact databáze *aplikace\_Data* složky, budete muset předejít změnou nastavení nasazení, která soubory *aplikace\_Data* složky se nenasadí.

Otevřete **vlastnosti projektu** okna pro ContosoUniversity projekt a vyberte **balení/publikování webu** kartě. Ujistěte se, že **konfigurace** rozevíracího seznamu má buď **aktivní (verze)** nebo **verze** vybrána, vyberte **vyloučit soubory z aplikace\_Složky dat**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

V případě, že se rozhodnete nasadit sestavení ladicí verze v budoucnu, je vhodné provést stejnou změnu pro konfiguraci ladění sestavení: změnit **konfigurace** k **ladění** a pak vyberte **vyloučit soubory z aplikace\_složky dat**.

Uložte a zavřete **balení/publikování webu** kartě.

> [!NOTE] 
> 
> [!IMPORTANT]
> Ujistěte se, že nemáte **odebrat další soubory v cílovém umístění** vybraný v profilech publikovat. Pokud vyberete tuto možnost, proces nasazení databáze, které máte v aplikaci odstraní\_dat v nasazené lokality a odstraní aplikaci\_samotné složce Data.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Během aktualizace brání přístupu uživatelů k pracoviště

Změna, kterou nasazujete nyní je jednoduchá změna na jednu stránku. Ale někdy nasadit větší změny, a v takovém případě webu můžete způsobeno nestandardní chování Pokud uživatel požádá o na stránce před dokončení nasazení. Chcete-li tomu zabránit, můžete použít *aplikace\_offline.htm* souboru. Když vložíte soubor s názvem *aplikace\_offline.htm* kořenové složky vaší aplikace, služba IIS automaticky zobrazí tento soubor namísto spuštění vaší aplikace. Tak, aby se zabránilo přístup při nasazení, můžete zadat *aplikace\_offline.htm* v kořenové složce, spustí proces nasazení a potom odeberte *aplikace\_offline.htm*.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši řešení (ne z projektů) a vyberte **novou složku řešení**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Název složky *SolutionFiles*.

V této nové složky vytvořit stránku HTML s názvem *aplikace\_offline.htm*. Nahradíte existující obsah následující kód:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Můžete zkopírovat *aplikace\_offline.htm* do složky lokality pomocí připojení k serveru FTP nebo **Správce souborů** nástroj v Ovládacích panelech poskytovatele hostingu. V tomto kurzu budete používat **Správce souborů**.

Otevřete ovládací panely a vyberte **Správce souborů** stejně jako v [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu. Vyberte **contosouniversity.com** a potom **wwwroot** získat do kořenové složky vaší aplikace a pak klikněte na tlačítko **nahrát**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

V **nahrát soubor** dialogové okno, vyberte *aplikace\_offline.htm* souboru a pak klikněte na **nahrát**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Přejděte na adresu URL vašeho webu. Uvidíte, že *aplikace\_offline.htm* stránky se nyní zobrazí namísto domovské stránky.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Nyní jste připraveni k nasazení do produkčního prostředí.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Nasazení aktualizace kódu do produkčního prostředí

V **publikování webu jedním kliknutím** nástrojů, vyberte **produkční** profil publikování a pak klikněte na **Publikovat Web**.

Visual Studio nasadí aktualizovanou aplikaci a otevře prohlížeč na domovskou stránku. *Aplikace\_offline.htm* souboru se zobrazí. Než můžete otestovat a ověřit úspěšné nasazení, je třeba odstranit *aplikace\_offline.htm* souboru.

Vraťte se na **Správce souborů** aplikace v Ovládacích panelech. Vyberte **contosouniversity.com** a **wwwroot**, vyberte **aplikace\_offline.htm**a potom klikněte na **odstranit**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

V prohlížeči otevřít stránku vyučující ve veřejné síti a vyberte lektorem k ověření, že aktualizace byla úspěšně nasazena.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Nyní jste nasadili aktualizaci aplikace, která neobsahovala změnu databáze. V dalším kurzu se dozvíte, jak nasadit změnu databáze.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [další](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
