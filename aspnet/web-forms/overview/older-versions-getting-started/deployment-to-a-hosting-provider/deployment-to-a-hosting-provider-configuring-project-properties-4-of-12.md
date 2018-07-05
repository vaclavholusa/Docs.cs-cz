---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: Konfigurace vlastností projektu – 4 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa1e55f21e74ae8da67d6d13c35a8de5693ec3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396262"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: Konfigurace vlastností projektu – 4 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Některé možnosti nasazení jsou nakonfigurovány ve vlastnostech projektu, které jsou uloženy v souboru projektu ( *.csproj* nebo *.vbproj* souboru). Ve většině případů jsou výchozí hodnoty z těchto nastavení, co chcete, ale můžete použít **vlastnosti projektu** uživatelského rozhraní, které jsou součástí Visual Studia pro práci s těmito nastaveními Pokud máte, tím je Změníme. V tomto kurzu zkontrolujete nastavení nasazení v **vlastnosti projektu**. Můžete také vytvořit zástupný soubor, který způsobí, že prázdnou složku pro nasazení.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Konfigurace nastavení nasazení v okně Vlastnosti projektu

Většinu nastavení, které ovlivňují, co se stane během nasazení jsou zahrnuty v profilu publikování, jak uvidíte v následujících kurzech. Několik nastavení, které byste měli vědět, jsou umístěny v **balení/publikování** karet **vlastnosti projektu** okna. Tato nastavení jsou zadány pro každou konfiguraci sestavení – to znamená, mohou mít různá nastavení pro sestavení pro vydání, než kolik máte pro sestavení pro ladění.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projekt, vyberte **vlastnosti**a pak vyberte **balení/publikování webu**kartu.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Pokud se zobrazí v okně, použije se výchozí zobrazuje nastavení konfigurace podle toho, která sestavení je aktuálně aktivních pro řešení. Pokud **konfigurace** pole nenaznačuje, že **aktivní (verze)** vyberte **vydání** mohla zobrazit nastavení konfigurace sestavení pro vydání. Sestavení pro vydání budete nasazovat do testovacích i produkčních prostředí.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

S **aktivní (verze)** nebo **vydání** vybrali, se zobrazí hodnoty, které platí při nasazení pomocí konfigurace verze sestavení:

- V **položky, které chcete nasadit** pole **pouze soubory potřebné ke spuštění aplikace** zaškrtnuto. Další možnosti jsou **všechny soubory v tomto projektu** nebo **všechny soubory v této složce projektu**. Ponechte výchozí výběr, beze změny byste se vyhnout soubory zdrojového kódu, například nasazení. Toto nastavení je důvod, proč složky obsahující binární soubory systému SQL Server Compact musí být zahrnutý v projektu. Další informace o tomto nastavení najdete v tématu **Proč nejsou všechny soubory ve složce projektu nasazení?** v [technologie ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx).
- **Vyloučit generované symboly ladění** zaškrtnuto. Nesmí být ladění při použití této konfigurace sestavení.
- **Vyloučit soubory z aplikace\_složka dat** není vybraná. SQL Server Compact souboru pro databázi členství v této složce a máte k jejímu nasazení. Když nasazujete aktualizace, které nejsou zahrnuté změny databáze, bude toto políčko zaškrtněte.
- **Předkompilace tuto aplikaci před publikováním** není vybraná. Ve většině případů není nutné předkompilovat projekty webových aplikací. Další informace o této možnosti najdete v tématu [kartě Balení/publikování webových, vlastnosti projektu](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) a [Advanced předkompilovat dialogové okno nastavení](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Zahrnout všechny databáze, které jsou nakonfigurované v kartě Balení/publikování kódu SQL** je vybrána, ale tato možnost nemá žádný vliv nyní vzhledem k tomu, že se konfigurace **balení/publikování kódu SQL** kartu. Dané karty je pro metodu nasazení starší verzi databáze, která používá jedinou možností pro nasazení databáze SQL serveru. Budete používat **balení/publikování kódu SQL** kartu [migrace na SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) kurzu.
- **Nastavení balíčku pro nasazení webových** část se nevztahuje, protože používáte jedním kliknutím publikovat v těchto kurzech.

Změnit **konfigurace** rozevíracího seznamu pro ladění, pokud chcete zobrazit výchozí nastavení pro sestavení pro ladění. Hodnoty jsou stejné, s výjimkou **Vyloučit generované symboly ladění** je prázdná, takže můžete ladit při nasazování sestavení pro ladění.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Provádění jistě, že se nasadí Elmah složky

Jak už jste viděli v předchozím kurzu, [balíček NuGet knihovny Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) poskytuje funkce pro protokolování a vykazováním chyb. Aplikace Contoso University Elmah nakonfigurovaný k ukládání podrobnosti o chybě ve složce s názvem *Elmah*:

![Elmah složky](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Vyloučení určitých souborů nebo složek z nasazení je běžné požadavky; Dalším příkladem může být složka, která mohou uživatelé odeslat soubory do. Nechcete, aby se soubory protokolu nebo nahrát soubory, které byly vytvořeny ve vašem vývojovém prostředí k nasazení do produkčního prostředí. A Pokud nasazujete aktualizace do produkčního prostředí, které nechcete, aby proces nasazení k odstranění souborů, které existují v produkčním prostředí. (V závislosti na tom, jak nastavit možnost nasazení, pokud soubor existuje v cílové lokalitě, ale ne zdrojové lokality, když nasazujete, Web Deploy neodstraní z cílového umístění.)

Jak už jste viděli dříve v tomto kurzu **položky, které chcete nasadit** možnost **balení/publikování webu** karty nastavená na **pouze soubory potřebné ke spuštění této aplikace**. V důsledku toho soubory protokolů, které jsou vytvořeny pomocí knihovny Elmah ve vývoji nebude nasazena, což je, co byste chtěli. (Má být nasazen, bylo by nutné zahrnutý v projektu a jejich **akce sestavení** vlastnost musel být nastaveno na **obsahu**. Další informace najdete v tématu **Proč nejsou všechny soubory ve složce projektu nasazení?** v [technologie ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx)). Ale Webdeploy nebude vytvořte složku v cílové lokalitě pokud existuje alespoň jeden soubor zkopírujte do něj. Proto přidáte *.txt* soubor do složky tak, aby fungoval jako zástupný symbol, takže budou zkopírovány složky.

V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *Elmah* složky, vyberte **přidat novou položku**a vytvořte soubor s názvem *Placeholder.txt*. Vložte následující text je: "Toto je zástupný soubor k zajištění, že se nasadí složky". a uložte soubor. To je všechno musíte udělat, pokud chcete mít jistotu, že sada Visual Studio nasadí tento soubor a složku probíhá, protože **akce sestavení** vlastnost *.txt* souborů je nastavena na **obsahu**ve výchozím nastavení.

Teď jste dokončili všechny kroky nastavení nasazení. V dalším kurzu budete nasazení webu společnosti Contoso University do testovacího prostředí a ho vyzkoušeli.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [další](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
