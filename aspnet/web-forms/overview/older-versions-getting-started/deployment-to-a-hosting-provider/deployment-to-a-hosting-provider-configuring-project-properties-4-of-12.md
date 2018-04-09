---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: Konfigurace vlastností projektu - 4 12 | Microsoft Docs'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: b8352c1832ffc79db93b6324dd673afaff6b0d74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: Konfigurace vlastností projektu - 4 12
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Některé možnosti nasazení jsou nakonfigurované ve vlastnostech projektu, které jsou uložené v souboru projektu ( *.csproj* nebo *.vbproj* souboru). Ve většině případů výchozí hodnoty těchto nastavení jsou, co chcete použít, ale můžete použít **vlastnosti projektu** uživatelského rozhraní, které jsou součástí sady Visual Studio pro práci s těmito nastaveními, pokud budete muset změnit. V tomto kurzu můžete zkontrolovat nastavení nasazení v **vlastnosti projektu**. Můžete také vytvořit zástupný soubor, který vede k prázdné složce pro nasazení.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Konfigurace nastavení nasazení v okně Vlastnosti projektu

Většina nastavení, které ovlivňuje, co se stane, že při nasazení jsou součástí profil publikování, jak uvidíte v následujících kurzech. Několik nastavení, které byste měli vědět, jsou umístěné v **nasadit** kartách **vlastnosti projektu** okno. Tato nastavení jsou určena pro každou konfiguraci sestavení – to znamená, mohou mít různá nastavení pro sestavení pro vydání, než kolik máte sestavení pro ladění.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projekt, vyberte **vlastnosti**a pak vyberte **balení/publikování webu**kartě.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Když se zobrazí okno, bude výchozí s nastavením pro kteroukoli konfigurace sestavení je aktuálně aktivní řešení. Pokud **konfigurace** pole neindikuje **aktivní (verze)**, vyberte **verze** aby bylo možné zobrazit nastavení pro konfiguraci sestavení verze. Nasadíte verze sestavení pro testovací a produkční prostředí.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

S **aktivní (verze)** nebo **verze** vybrali, zobrazí hodnoty, které jsou platné, když nasadíte pomocí konfigurace sestavení verze:

- V **položky k nasazení** pole **pouze soubory potřebné ke spuštění aplikace** je vybrána. Ostatní možnosti patří **všechny soubory v tomto projektu** nebo **všechny soubory v této složce projektu**. Ponechat výchozí výběr beze změny vyhnete soubory zdrojového kódu, například nasazení. Toto nastavení je důvod, proč složky, které obsahují systém SQL Server Compact binární soubory musí být zahrnutý v projektu. Další informace o tomto nastavení najdete v tématu **proč si všechny soubory ve složce projektu nasadí?** v [ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx).
- **Vyloučit generované symboly ladění** je vybrána. Při použití této konfigurace sestavení, nebude možné ladění.
- **Vyloučit soubory z aplikace\_složky dat** není vybraná. Systém SQL Server Compact souboru pro databázi členství v této složce a zda máte nasazení. Když nasazujete aktualizace, které neobsahují změny v databázi, vyberte toto políčko.
- **Předkompilovat tuto aplikaci před publikováním** není vybraná. Ve většině případů není třeba předkompilovat projekty webových aplikací. Další informace o této možnosti najdete v tématu [kartě Balení/publikování Web, vlastnosti projektu](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) a [Advanced předkompilovat Dialog s nastavením](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Zahrnout všechny databáze nakonfigurované na kartě Balení/publikování kódu SQL** je vybrána, ale tuto možnost se neprojeví nyní, protože nejsou konfigurace **balení/publikování kódu SQL** kartě. Že karta je pro metodu nasazení starší verze databáze, která používá jako jediná možnost pro nasazení databáze systému SQL Server. Budete používat **balení/publikování kódu SQL** ve [migrace do systému SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) kurzu.
- **Nastavení balíčku pro nasazení webových** oddíl nelze použít, protože používáte jedním kliknutím publikovat v těchto kurzech.

Změna **konfigurace** rozevíracího seznamu pro ladění zobrazíte výchozí nastavení pro ladění. Hodnoty jsou stejné, s výjimkou **Vynechat generované symboly ladění** je prázdná, takže můžete ladit, když nasazujete sestavení ladicí verze.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Provedení zda, získá nasazená složce Elmah

Jak už jste viděli v tomto kurzu předchozí [balíček Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) poskytuje funkce pro chybu protokolování a vytváření sestav. V aplikaci Contoso univerzity Elmah nakonfigurován, aby podrobnosti o chybě uložit do složky s názvem *Elmah*:

![Elmah složky](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

S výjimkou určité soubory nebo složky z nasazení je běžné požadavek; Dalším příkladem může být do složky, které mohou uživatelé odeslat soubory do. Nechcete, aby se soubory protokolu nebo nahrát soubory, které byly vytvořeny ve vašem vývojovém prostředí pro nasazení do produkčního prostředí. A Pokud nasazujete aktualizace do produkčního prostředí nechcete, aby proces nasazení k odstranění souborů, které existují v produkčním prostředí. (V závislosti na tom, jak nastavit možnost nasazení, pokud soubor existuje v cílové lokalitě, ale není zdrojové lokality při nasazení, Web Deploy neodstraní z cílového.)

Jak už jste viděli dříve v tomto kurzu **položky k nasazení** možnost **balení/publikování webu** karta je nastaven na **pouze soubory potřebné ke spuštění této aplikace**. Výsledkem je soubory protokolů, které jsou vytvořené pomocí Elmah vývojem nebude nasazen, který se má stát. (Chcete-li být nasazen, by musel být zahrnutý v projektu a jejich **akce sestavení** vlastnost musel být nastavena na **obsahu**. Další informace najdete v tématu **proč si všechny soubory ve složce projektu nasadí?** v [ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx)). Ale Web Deploy nebude vytvořte složku v cílové lokalitě pokud existuje alespoň jeden soubor zkopírujte do něj. Proto přidáte *.txt* soubor do složky tak, aby fungoval jako zástupný znak, aby se zkopírují na složku.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Elmah* složky, vyberte **přidat novou položku**a vytvořte soubor s názvem *Placeholder.txt*. Zadejte následující text v něm: "Toto je soubor zástupný symbol zajistit, že složce nasadí." A uložte soubor. To je všechno, budete muset udělat, pokud chcete mít jistotu, že Visual Studio nasadí tento soubor a složka je v, protože **akce sestavení** vlastnost *.txt* soubory nastavena na **obsahu**ve výchozím nastavení.

Teď jste dokončili všechny úlohy, nastavení nasazení. V dalším kurzu budete nasazení webu společnosti Contoso univerzity do testovacího prostředí a otestovat ji.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [další](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
