---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace Code-Only – 8 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 39075d2e979076f88200ea595c92f95f38f35911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374010"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace Code-Only – 8 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Po počátečním nasazení pokračuje v práci údržbu a vývoj webu, a před dlouho budete chtít nasazení aktualizace. Tento kurz vás provede procesem nasazení aktualizace kódu aplikace. Tato aktualizace nezahrnuje Změna databáze; uvidíte, co se liší o nasazení změn databází v dalším kurzu.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Změna kódu

Jako jednoduchý příklad příkazu update pro vaši aplikaci, přidáte k **Instruktoři** stránce Seznam kurzy vedené vybrané instruktorem.

Při spuštění **Instruktoři** stránky, můžete si všimnout, že jsou **vyberte** odkazy v mřížce, ale jejich nic nedělají než zkontrolujte šedá zapnout pozadí řádku.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Teď přidejte kód, který spouští, když **vyberte** dojde ke kliknutí na odkaz a zobrazí seznam kurzy vedené vybrané instruktorem.

V *Instructors.aspx*, přidejte následující kód bezprostředně po **ErrorMessageLabel** `Label` ovládacího prvku:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Spustit na stránku a vybrat instruktorem. Zobrazí seznam kurzy vedené této instruktorem.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Nasazení aktualizace kódu do testovacího prostředí

Nasazení do testovacího prostředí je znovu publikovat jednoduché spuštění jedním kliknutím. Chcete-li tento proces urychlit, můžete použít **publikování webu jedním kliknutím** nástrojů.

V **zobrazení** nabídce zvolte **panely nástrojů** a pak vyberte **publikování webu jedním kliknutím**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

V **Průzkumníka řešení**, vyberte projekt ContosoUniversity.

**publikování webu jedním kliknutím** nástrojů, zvolte **testovací** profil publikování a pak klikněte na tlačítko **Publikovat Web** (na ikonu šipky směřující vlevo a vpravo).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio nasadí aktualizované aplikace a prohlížeč se automaticky otevře na domovskou stránku. Spustit školitelů stránky a vyberte k ověření, že se aktualizace úspěšně nasadilo instruktorem.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Běžným způsobem provést také regresního testování (to znamená, že test zbytku lokality, abyste měli jistotu, že tato nová změna neměli přerušit všechny existující funkce). Ale pro účely tohoto kurzu budete tento krok přeskočit a přejít k nasazení aktualizací do produkčního prostředí.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Brání počáteční stav databáze opakované nasazení do produkčního prostředí

V reálné aplikaci uživatelé komunikují s svoje produkční lokality po počátečním nasazení a databází se vyplní s dynamickými daty. Proto nechcete znovu nasadit databázi členství v tomto počátečním stavu, který by vymazat všechny dynamická data. Vzhledem k tomu, že jsou soubory v systému SQL Server Compact databáze *aplikace\_Data* složku, je nutné zabránit to změnou nastavení nasazení, které soubory v *aplikace\_Data* složky nenasadí.

Otevřít **vlastnosti projektu** okně ContosoUniversity projektu a vyberte **balení/publikování webu** kartu. Ujistěte se, že **konfigurace** rozevíracího seznamu se buď **aktivní (verze)** nebo **vydání** vybraná, vyberte **vyloučit soubory z aplikace\_Složky dat**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

V případě, že se rozhodnete nasazení ladicího buildu v budoucnu, je vhodné provést stejnou změnu pro konfiguraci sestavení ladění: Změna **konfigurace** k **ladění** a pak vyberte **vyloučit soubory z aplikace\_složka dat**.

Uložte a zavřete **balení/publikování webu** kartu.

> [!NOTE] 
> 
> [!IMPORTANT]
> Ujistěte se, že není nutné **odebrat další soubory v cílovém umístění** vybrané v profilech publikování. Pokud vyberete tuto možnost, procesu nasazení se odstraní databáze, které máte v aplikaci\_Data v nasazené lokality který se odstraní aplikaci\_samotné složce Data.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Zabránění přístupu uživatelů k produkční lokality během aktualizace

Změna, kterou nasazujete nyní je jednoduchou změnu na jednu stránku. Ale v některých případech můžete nasadit větší změny a v takovém případě web se může chovat podivné Pokud uživatel požádá o stránku předtím, než se nasazení dokončí. Chcete-li tomu zabránit, můžete použít *aplikace\_offline.htm* souboru. Zadaný soubor s názvem *aplikace\_offline.htm* v kořenové složce vaší aplikace, služba IIS automaticky zobrazí tento soubor namísto spuštění vaší aplikace. Takže pokud chcete zabránit přístupu během nasazování, kam si ukládáte *aplikace\_offline.htm* v kořenové složce spustit proces nasazení a pak odeberte *aplikace\_offline.htm*.

V **Průzkumníka řešení**klikněte pravým tlačítkem řešení (ne z projektů) a vyberte **novou složku řešení**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Název složky *SolutionFiles*.

V nové složce vytvořte stránku HTML s názvem *aplikace\_offline.htm*. Nahraďte existující obsah následujícím kódem:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Můžete zkopírovat *aplikace\_offline.htm* souboru k webu pomocí připojení k serveru FTP nebo **Správce souborů** nástroj v Ovládacích panelech poskytovatele hostingu. Pro účely tohoto kurzu budete používat **Správce souborů**.

Otevřete ovládací panely a vyberte **Správce souborů** jako jste to udělali [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu. Vyberte **contosouniversity.com** a potom **wwwroot** získat do kořenové složky vaší aplikace, a potom klikněte na **nahrát**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

V **nahrát soubor** dialogové okno, vyberte *aplikace\_offline.htm* souboru a pak klikněte na tlačítko **nahrát**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Přejděte na adresu URL vašeho webu. Uvidíte, že *aplikace\_offline.htm* stránky se nyní zobrazí místo domovské stránky.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Nyní jste připraveni k nasazení do produkčního prostředí.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Nasazení aktualizace kódu do produkčního prostředí

V **publikování webu jedním kliknutím** nástrojů, zvolte **produkční** profil publikování a pak klikněte na tlačítko **Publikovat Web**.

Visual Studio nasadí aktualizované aplikace a otevře prohlížeč na domovské stránce webu. *Aplikace\_offline.htm* soubor se zobrazí. Předtím, než můžete otestovat a ověřit úspěšné nasazení, je nutné odebrat *aplikace\_offline.htm* souboru.

Vraťte se **Správce souborů** aplikace v Ovládacích panelech. Vyberte **contosouniversity.com** a **wwwroot**vyberte **aplikace\_offline.htm**a potom klikněte na tlačítko **odstranit**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

V prohlížeči otevřete stránku školitelů v na veřejném webu a vyberte instruktorem k ověření, že se aktualizace úspěšně nasadilo.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Nyní jste nasadili aktualizaci aplikace, která neobsahovala Změna databáze. V dalším kurzu se dozvíte, jak nasadit databázi změnit.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [další](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
