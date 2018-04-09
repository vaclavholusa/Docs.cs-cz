---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: vlastnosti projektu | Microsoft Docs'
author: tdykstra
description: Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: fba3f089bf1693eec873b08b4bc50e3accba06ee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: vlastnosti projektu
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

Některé možnosti nasazení jsou nakonfigurované ve vlastnostech projektu, které jsou uložené v souboru projektu ( *.csproj* nebo *.vbproj* souboru). Ve většině případů výchozí hodnoty těchto nastavení jsou, co chcete použít, ale můžete použít **vlastnosti projektu** uživatelského rozhraní, které jsou součástí sady Visual Studio pro práci s těmito nastaveními, pokud budete muset změnit. V tomto kurzu můžete zkontrolovat nastavení nasazení v **vlastnosti projektu**. Můžete také vytvořit zástupný soubor, který vede k prázdné složce pro nasazení.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Konfigurace nastavení nasazení v okně Vlastnosti projektu

Většina nastavení, které ovlivňuje, co se stane, že při nasazení jsou součástí profil publikování, jak uvidíte v následujících kurzech. Několik nastavení, které byste měli vědět, jsou umístěné v **nasadit** kartách **vlastnosti projektu** okno. Tato nastavení jsou určena pro každou konfiguraci sestavení – to znamená, mohou mít různá nastavení pro sestavení pro vydání, než kolik máte sestavení pro ladění.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projekt, vyberte **vlastnosti**a pak vyberte **balení/publikování webu**kartě.

![Kartě Balení/publikování webu](project-properties/_static/image1.png)

Když se zobrazí okno, bude výchozí s nastavením pro kteroukoli konfigurace sestavení je aktuálně aktivní řešení. Pokud **konfigurace** pole neindikuje **aktivní (verze)**, vyberte **verze** aby bylo možné zobrazit nastavení pro konfiguraci sestavení verze. Nasadíte verze sestavení pro testovací a produkční prostředí.

![Výběr konfigurace verze sestavení](project-properties/_static/image2.png)

S **aktivní (verze)** nebo **verze** vybrali, zobrazí hodnoty, které jsou platné, když nasadíte pomocí konfigurace sestavení verze:

- V **položky k nasazení** pole **pouze soubory potřebné ke spuštění aplikace** je vybrána. Ostatní možnosti patří **všechny soubory v tomto projektu** nebo **všechny soubory v této složce projektu**. Ponechat výchozí výběr beze změny vyhnete soubory zdrojového kódu, například nasazení. Toto nastavení je důvod, proč složky, které obsahují systém SQL Server Compact binární soubory musí být zahrnutý v projektu. Další informace o tomto nastavení najdete v tématu **proč si všechny soubory ve složce projektu nasadí?** v [ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx).
- **Vyloučit generované symboly ladění** je vybrána. Při použití této konfigurace sestavení, nebude možné ladění.
- **Zahrnout všechny databáze nakonfigurované na kartě Balení/publikování kódu SQL** je vybrána. Určuje, zda bude databáze a také soubory nasazení sady Visual Studio. I když políčko popisku pouze zmínkami **balení/publikování kódu SQL** kartě, zrušíte zaškrtnutí tohoto políčka by také zakázat nasazení databáze, který je nakonfigurovaný v profilu publikování. Můžete se to, který později, tak pole musí zůstat vybrané. **Balení/publikování kódu SQL** karta slouží k publikování metoda, která nebudete používat v těchto kurzech starší verze databáze.
- **Nastavení balíčku pro nasazení webových** oddíl nelze použít, protože používáte jedním kliknutím publikovat v těchto kurzech.

Změna **konfigurace** rozevíracího seznamu pro ladění zobrazíte výchozí nastavení pro ladění. Hodnoty jsou stejné, s výjimkou **Vynechat generované symboly ladění** je prázdná, takže můžete ladit, když nasazujete sestavení ladicí verze.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Získá nasazení složce Elmah

Jak už jste viděli v tomto kurzu předchozí [balíček Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) poskytuje funkce pro chybu protokolování a vytváření sestav. V aplikaci Contoso univerzity Elmah nakonfigurován, aby podrobnosti o chybě uložit do složky s názvem *Elmah*:

![Elmah složky](project-properties/_static/image3.png)

S výjimkou určité soubory nebo složky z nasazení je běžné požadavek; Dalším příkladem může být do složky, které mohou uživatelé odeslat soubory do. Nechcete, aby se soubory protokolu nebo nahrát soubory, které byly vytvořeny ve vašem vývojovém prostředí pro nasazení do produkčního prostředí. A Pokud nasazujete aktualizace do produkčního prostředí nechcete, aby proces nasazení k odstranění souborů, které existují v produkčním prostředí. (V závislosti na tom, jak nastavit možnost nasazení, pokud soubor existuje v cílové lokalitě, ale není zdrojové lokality při nasazení, Web Deploy neodstraní z cílového.)

Jak už jste viděli dříve v tomto kurzu **položky k nasazení** možnost **balení/publikování webu** karta je nastaven na **pouze soubory potřebné ke spuštění této aplikace**. Výsledkem je soubory protokolů, které jsou vytvořené pomocí Elmah vývojem nebude nasazen, který se má stát. (Chcete-li být nasazen, by musel být zahrnutý v projektu a jejich **akce sestavení** vlastnost musel být nastavena na **obsahu**. Další informace najdete v tématu **proč si všechny soubory ve složce projektu nasadí?** v [ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx)). Ale Web Deploy nebude vytvořte složku v cílové lokalitě pokud existuje alespoň jeden soubor zkopírujte do něj. Proto přidáte *.txt* soubor do složky tak, aby fungoval jako zástupný znak, aby se zkopírují na složku.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Elmah* složky, vyberte **přidat novou položku**a vytvořte textový soubor s názvem *Placeholder.txt*. Zadejte následující text v něm: "Toto je soubor zástupný symbol zajistit, že složce nasadí." A uložte soubor. To je všechno, budete muset udělat, pokud chcete mít jistotu, že Visual Studio nasadí tento soubor a složka je v, protože **akce sestavení** vlastnost *.txt* soubory nastavena na **obsahu**ve výchozím nastavení.

## <a name="summary"></a>Souhrn

Teď jste dokončili všechny úlohy, nastavení nasazení. V dalším kurzu budete nasazení webu společnosti Contoso univerzity do testovacího prostředí a otestovat ji.

> [!div class="step-by-step"]
> [Předchozí](web-config-transformations.md)
> [další](deploying-to-iis.md)
