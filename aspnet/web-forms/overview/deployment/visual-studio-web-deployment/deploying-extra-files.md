---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení dalších souborů | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 41bea625a53afbf7b39c03a2e8a92a03ce144289
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371817"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení dalších souborů
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak rozšíření aplikace Visual Studio publikování webu kanálu provést dodatečnou úlohu během nasazení. Úloha je kopírování dalších souborů, které nejsou ve složce projektu na cílový webový server.

Pro účely tohoto kurzu budete zkopírovat jeden další soubor: *robots.txt*. Chcete nasadit tento soubor do přípravy, ale ne do produkčního prostředí. V [nasazení do produkčního prostředí](deploying-to-production.md) výukový program, tento soubor do projektu přidat a nakonfigurovat provozní Publikovat profil chcete vyloučit. V tomto kurzu uvidíte alternativní metoda pro zpracování této situaci, která bude užitečná pro všechny soubory, které chcete nasadit, ale nechcete zahrnout do projektu.

## <a name="move-the-robotstxt-file"></a>Přesunout soubor robots.txt

Příprava pro jiný způsob zpracování *robots.txt*v této části kurzu přesunout soubor do složky, která není zahrnutá v projektu a odstraníte *robots.txt* z testovacího prostředí. Je nutné odstranit soubor z pracovní, aby mohli ověřit, že vaši novou metodu nasazení souboru na tomto prostředí pracuje správně.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *robots.txt* souboru a klikněte na tlačítko **vyjmout z projektu**.
2. Pomocí Průzkumníka souborů Windows, vytvořte novou složku ve složce řešení a pojmenujte ho *ExtraFiles*.
3. Přesunout *robots.txt* soubor *ContosoUniversity* složky projektu *ExtraFiles* složky.

    ![ExtraFiles složky](deploying-extra-files/_static/image1.png)
4. Pomocí nástrojů FTP, odstranit *robots.txt* soubor z pracovní webu.

    Jako alternativu můžete zvolit **odebrat další soubory v cílovém umístění** pod **možností publikování souboru** na **nastavení** kartu pracovní profil publikování, a znovu publikujte do přípravného prostředí.

## <a name="update-the-publish-profile-file"></a>Aktualizace souboru profilu publikování

Potřebujete jenom *robots.txt* v přípravě, takže pracovní pouze profil publikování, budete muset aktualizovat, aby ho nasadit.

1. V sadě Visual Studio, otevřete *Staging.pubxml*.
2. Na konci souboru, před uzavírací značku `</Project>` značky, přidejte následující kód:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Tento kód vytvoří novou *cílové* , která bude shromažďovat další soubory k nasazení. Cíl se skládá z jednoho nebo více úkolů, které se spustí nástroj MSBuild na základě podmínek, které zadáte.

    `Include` Atribut určuje, že složka, ve kterém chcete najít soubory je *ExtraFiles*, který je umístěn na stejné úrovni jako složky projektu. Nástroj MSBuild bude shromažďovat všechny soubory z této složky a rekurzivně z podsložky (double hvězdička určuje rekurzivní podsložek). S tímto kódem můžete umístit více souborů a soubory v podsložkách uvnitř *ExtraFiles* složka a všechny se nasadí.

    `DestinationRelativePath` Element určuje, že složek a souborů, mají být zkopírovány do kořenové složky, cílové webové stránky, ve stejné struktuře souborů a složek, protože se nenachází v *ExtraFiles* složky. Pokud chcete zkopírovat *ExtraFiles* samotnou složku, `DestinationRelativePath` hodnota bude *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Na konci souboru, před uzavírací značku `</Project>` značky, přidejte následující kód, který určuje, kdy se má spustit nový cíl.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Tento kód způsobí, že nový `CustomCollectFiles` cíl, který se spustí pokaždé, když je proveden cíl, který zkopíruje soubory do cílové složky. Je samostatný cílový pro publikování a vytvoření balíčku nasazení a nový cíl se vloží v obou cíle v případě, že jste se rozhodli nasadit s použitím balíčku pro nasazení namísto publikování.

    *.Pubxml* soubor bude vypadat jako v následujícím příkladu:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Uložte a zavřete *Staging.pubxml* souboru.

## <a name="publish-to-staging"></a>Publikovat do přípravného prostředí

Jedním kliknutím pomocí publikování nebo příkazového řádku, publikování aplikace prostřednictvím pracovního profilu.

Pokud používáte jedním kliknutím publikovat, můžete ověřit v **ve verzi Preview** okno, které *robots.txt* budou zkopírovány. Jinak používat nástroj FTP a ověřte, že *robots.txt* soubor nachází v kořenové složce webu po nasazení.

## <a name="summary"></a>Souhrn

Dokončení tohoto postupu Tato série kurzů o nasazení webové aplikace ASP.NET do poskytovatele hostitelských služeb třetích stran. Další informace o některém z témat uvedených v následujících kurzech najdete v článku [mapa obsahu nasazení ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Další informace

Pokud víte, jak pracovat s MSBuild – soubory, můžete automatizovat spoustu dalších úkolů nasazení napsáním kódu v *.pubxml* soubory (pro profil konkrétní úkoly) nebo projektu *. wpp.targets* souboru (pro úkoly, které platí pro všechny profily). Další informace o *.pubxml* a *. wpp.targets* soubory, naleznete v tématu [postupy: Úprava nastavení nasazení v souborech profil publikování (.pubxml) a. wpp.targets souboru v aplikaci Visual Studio Web Projekty](https://msdn.microsoft.com/library/ff398069). Základní informace o MSBuild kód, naleznete v tématu **anatomie soubor projektu** v [nasazení v podniku: Vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Zjistěte, jak pracovat se soubory MSBuild k provádění úloh pro vlastní scénáře, najdete v této příručce: [uvnitř the Microsoft Build Engine: použití nástroje MSBuild a Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi a William Bartholomew.

## <a name="acknowledgements"></a>Potvrzení

Chci vám chceme poděkovat následující uživatelů, kteří provedli významné příspěvky k obsahu v této sérii kurzů:

- [Alberto Poblacion, MVP &amp; MCT, Španělsko](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, vývoj pro platformu dat MVP, Spojené státy
- Nepříznivých Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itálie](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [SAYED Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Srbsko](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Předchozí](command-line-deployment.md)
> [další](troubleshooting.md)
