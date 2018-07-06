---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: vlastnosti projektu | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 86e4e3ef5f126a5bf8abc91c2d5ce3d14b1e1c6c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837662"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: vlastnosti projektu
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

Některé možnosti nasazení jsou nakonfigurovány ve vlastnostech projektu, které jsou uloženy v souboru projektu ( *.csproj* nebo *.vbproj* souboru). Ve většině případů jsou výchozí hodnoty z těchto nastavení, co chcete, ale můžete použít **vlastnosti projektu** uživatelského rozhraní, které jsou součástí Visual Studia pro práci s těmito nastaveními Pokud máte, tím je Změníme. V tomto kurzu zkontrolujete nastavení nasazení v **vlastnosti projektu**. Můžete také vytvořit zástupný soubor, který způsobí, že prázdnou složku pro nasazení.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Konfigurovat nastavení nasazení v okně Vlastnosti projektu

Většinu nastavení, které ovlivňují, co se stane během nasazení jsou zahrnuty v profilu publikování, jak uvidíte v následujících kurzech. Několik nastavení, které byste měli vědět, jsou umístěny v **balení/publikování** karet **vlastnosti projektu** okna. Tato nastavení jsou zadány pro každou konfiguraci sestavení – to znamená, mohou mít různá nastavení pro sestavení pro vydání, než kolik máte pro sestavení pro ladění.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projekt, vyberte **vlastnosti**a pak vyberte **balení/publikování webu**kartu.

![Kartě Balení/publikování webu](project-properties/_static/image1.png)

Pokud se zobrazí v okně, použije se výchozí zobrazuje nastavení konfigurace podle toho, která sestavení je aktuálně aktivních pro řešení. Pokud **konfigurace** pole nenaznačuje, že **aktivní (verze)** vyberte **vydání** mohla zobrazit nastavení konfigurace sestavení pro vydání. Sestavení pro vydání budete nasazovat do testovacích i produkčních prostředí.

![Výběr konfigurace sestavení pro vydání](project-properties/_static/image2.png)

S **aktivní (verze)** nebo **vydání** vybrali, se zobrazí hodnoty, které platí při nasazení pomocí konfigurace verze sestavení:

- V **položky, které chcete nasadit** pole **pouze soubory potřebné ke spuštění aplikace** zaškrtnuto. Další možnosti jsou **všechny soubory v tomto projektu** nebo **všechny soubory v této složce projektu**. Ponechte výchozí výběr, beze změny byste se vyhnout soubory zdrojového kódu, například nasazení. Toto nastavení je důvod, proč složky obsahující binární soubory systému SQL Server Compact musí být zahrnutý v projektu. Další informace o tomto nastavení najdete v tématu **Proč nejsou všechny soubory ve složce projektu nasazení?** v [technologie ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx).
- **Vyloučit generované symboly ladění** zaškrtnuto. Nesmí být ladění při použití této konfigurace sestavení.
- **Zahrnout všechny databáze, které jsou nakonfigurované v kartě Balení/publikování kódu SQL** zaškrtnuto. Určuje, zda sady Visual Studio nasadí databáze, jakož i soubory. I když zaškrtnutí políčka popisek pouze zmínky **balení/publikování kódu SQL** kartu, zrušíte zaškrtnutí tohoto políčka by také zakázat nasazení databáze, který je nakonfigurovaný v profilu publikování. Můžete se být způsobem, který později, tak zaškrtnutí políčka musí zůstat vybrané. **Balení/publikování kódu SQL** karta se používá pro starší verze databáze publikování metodu, která nebudete používat v těchto kurzech.
- **Nastavení balíčku pro nasazení webových** část se nevztahuje, protože používáte jedním kliknutím publikovat v těchto kurzech.

Změnit **konfigurace** rozevíracího seznamu pro ladění, pokud chcete zobrazit výchozí nastavení pro sestavení pro ladění. Hodnoty jsou stejné, s výjimkou **Vyloučit generované symboly ladění** je prázdná, takže můžete ladit při nasazování sestavení pro ladění.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Ujistěte se, že se nasadí Elmah složky

Jak už jste viděli v předchozím kurzu, [balíček NuGet knihovny Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) poskytuje funkce pro protokolování a vykazováním chyb. Aplikace Contoso University Elmah nakonfigurovaný k ukládání podrobnosti o chybě ve složce s názvem *Elmah*:

![Elmah složky](project-properties/_static/image3.png)

Vyloučení určitých souborů nebo složek z nasazení je běžné požadavky; Dalším příkladem může být složka, která mohou uživatelé odeslat soubory do. Nechcete, aby se soubory protokolu nebo nahrát soubory, které byly vytvořeny ve vašem vývojovém prostředí k nasazení do produkčního prostředí. A Pokud nasazujete aktualizace do produkčního prostředí, které nechcete, aby proces nasazení k odstranění souborů, které existují v produkčním prostředí. (V závislosti na tom, jak nastavit možnost nasazení, pokud soubor existuje v cílové lokalitě, ale ne zdrojové lokality, když nasazujete, Web Deploy neodstraní z cílového umístění.)

Jak už jste viděli dříve v tomto kurzu **položky, které chcete nasadit** možnost **balení/publikování webu** karty nastavená na **pouze soubory potřebné ke spuštění této aplikace**. V důsledku toho soubory protokolů, které jsou vytvořeny pomocí knihovny Elmah ve vývoji nebude nasazena, což je, co byste chtěli. (Má být nasazen, bylo by nutné zahrnutý v projektu a jejich **akce sestavení** vlastnost musel být nastaveno na **obsahu**. Další informace najdete v tématu **Proč nejsou všechny soubory ve složce projektu nasazení?** v [technologie ASP.NET webové aplikace projektu nasazení – nejčastější dotazy](https://msdn.microsoft.com/library/ee942158.aspx)). Ale Webdeploy nebude vytvořte složku v cílové lokalitě pokud existuje alespoň jeden soubor zkopírujte do něj. Proto přidáte *.txt* soubor do složky tak, aby fungoval jako zástupný symbol, takže budou zkopírovány složky.

V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *Elmah* složky, vyberte **přidat novou položku**a vytvořte textový soubor s názvem *Placeholder.txt*. Vložte následující text je: "Toto je zástupný soubor k zajištění, že se nasadí složky". a uložte soubor. To je všechno musíte udělat, pokud chcete mít jistotu, že sada Visual Studio nasadí tento soubor a složku probíhá, protože **akce sestavení** vlastnost *.txt* souborů je nastavena na **obsahu**ve výchozím nastavení.

## <a name="summary"></a>Souhrn

Teď jste dokončili všechny kroky nastavení nasazení. V dalším kurzu budete nasazení webu společnosti Contoso University do testovacího prostředí a ho vyzkoušeli.

> [!div class="step-by-step"]
> [Předchozí](web-config-transformations.md)
> [další](deploying-to-iis.md)
