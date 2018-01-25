---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení dalších souborů | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 46e18ba81c3db8bb04c5cb997bcc1607e4e38bae
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení dalších souborů
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak rozšířit publikování webů sady Visual Studio kanálu, který má provést dodatečnou úlohu během nasazení. Tato úloha je zkopírovat další soubory, které nejsou ve složce projektu k cílovému webu.

Pro účely tohoto kurzu budete zkopírovat jeden další soubor: *robots.txt*. Chcete nasadit tento soubor do přípravy, ale ne do produkčního prostředí. V [nasazení do produkčního prostředí](deploying-to-production.md) kurz, přidání tohoto souboru do projektu a nakonfigurování provozní publikování profilu, které chcete vyloučit. V tomto kurzu se zobrazí alternativní metoda pro zpracování této situaci, ten, který bude užitečná pro všechny soubory, které chcete nasadit, ale nechcete, aby pro zahrnutí do projektu.

## <a name="move-the-robotstxt-file"></a>Přesuňte soubor robots.txt

Příprava na jinou metodu zpracování *robots.txt*, v této části kurzu přesunout soubor do složky, který není zahrnutý v projektu a odstraníte *robots.txt* z testovacího prostředí. Je nezbytné k odstranění souboru z pracovní, aby mohli ověřit, zda vaše nové metody nasazení soubor v tomto prostředí pracuje správně.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *robots.txt* souboru a klikněte na tlačítko **vyloučit z projektu**.
2. Pomocí Průzkumníka souborů Windows, vytvořte novou složku ve složce řešení a pojmenujte ji *ExtraFiles*.
3. Přesunout *robots.txt* souboru z *ContosoUniversity* složky projektu na *ExtraFiles* složky.

    ![ExtraFiles složky](deploying-extra-files/_static/image1.png)
4. Pomocí nástroje vaší FTP odstranit *robots.txt* soubor z pracovních webu.

    Alternativně můžete vybrat **odebrat další soubory v cílovém umístění** pod **možností publikování souboru** na **nastavení** kartě pracovní profil publikování, a znovu publikujte do přípravy.

## <a name="update-the-publish-profile-file"></a>Aktualizace souboru profilu publikování

Potřebujete jenom *robots.txt* v pracovním, tak je pracovní pouze profil publikování, je potřeba aktualizovat, aby bylo možné ho nasadit.

1. V sadě Visual Studio otevřete *Staging.pubxml*.
2. Na konci souboru, před uzavírací `</Project>` značky, přidejte následující kód:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Tento kód vytvoří novou *cíl* , bude shromažďovat další soubory k nasazení. Cíl se skládá z jednoho nebo více úloh, které budou spuštěny MSBuild na základě podmínek, které zadáte.

    `Include` Atribut určuje, že složka, ve kterém chcete najít soubory je *ExtraFiles*, který je umístěn na stejné úrovni jako složce projektu. MSBuild bude shromažďovat všechny soubory z této složky a rekurzivně ze všech podsložek (dvojitý znak hvězdičky určuje rekurzivní podsložek). S tímto kódem by mohlo více souborů a souborů v podsložkách uvnitř *ExtraFiles* složky a všech bude nasazeno.

    `DestinationRelativePath` Element určuje, že složek a souborů, by měl být zkopírovány do kořenové složky webu cílové ve stejné struktuře souborů a složek, protože se nenachází v *ExtraFiles* složky. Pokud chcete zkopírovat *ExtraFiles* samotné, složce `DestinationRelativePath` hodnotu *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Na konci souboru, před uzavírací `</Project>` značky, přidejte následující kód, který určuje, kdy k provedení nového cíle.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Tento kód způsobí, že nové `CustomCollectFiles` cíle provést vždy, když se spustí cíl, který zkopíruje soubory do cílové složky. Je samostatný cíle pro publikování a vytvoření balíčku nasazení a v případě, že se rozhodnete nasadit místo publikování pomocí balíčku pro nasazení, je nová cílová vložit v obou cíle.

    *.Pubxml* soubor bude vypadat jako v následujícím příkladu:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Uložte a zavřete *Staging.pubxml* souboru.

## <a name="publish-to-staging"></a>Publikování do přípravy

Použití jedním kliknutím publikovat nebo příkazového řádku, publikování aplikace pomocí pracovní profil.

Pokud používáte jedním kliknutím publikovat, můžete ověřit v **Preview** okno, *robots.txt* budou zkopírovány. Jinak používat vaše nástroje FTP a ověřte, zda *robots.txt* soubor nachází v kořenové složce webové stránky po nasazení.

## <a name="summary"></a>Souhrn

Dokončení této série kurzů k nasazení do hostujícího zprostředkovatele třetí strany webovou aplikaci ASP.NET. Další informace o všech témat zahrnutých v těchto kurzech najdete v tématu [mapa obsahu nasazení ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Další informace

Pokud umíte pracovat se soubory nástroje MSBuild, můžete automatizovat celou řadu dalších úloh nasazení podle psaní kódu v *.pubxml* soubory (pro úlohy specifické pro profil) nebo projektu *. wpp.targets* souboru (pro úkoly, které platí pro všechny profily). Další informace o *.pubxml* a *. wpp.targets* soubory, najdete v části [postup: Upravit nastavení nasazení v souborech profil publikování (.pubxml) a. wpp.targets souboru v aplikaci Visual Studio Web Projekty](https://msdn.microsoft.com/library/ff398069). Základní informace o MSBuild kód, najdete v části **The anatomie soubor projektu** v [Enterprise nasazení řady: Principy souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Naučte se pracovat se soubory nástroje MSBuild úkoly pro vaše vlastní scénáře, najdete v článku tato kniha: [uvnitř Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi a William Bartholomew.

## <a name="acknowledgements"></a>Potvrzení

Děkujeme, že následující osob, které významně přispěli k obsahu tento kurz řady chci:

- [Alberto Poblacion, MVP &amp; MCT, Španělsko](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, vývoj platformy Data MVP, Spojené státy
- Harsh Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Jan Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itálie](http://www.iamraf.net/)
- [Rick Anderson Microsoft](https://blogs.msdn.com/b/rickandy/)
- [SAYED Hashimi společnost Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[Předchozí](command-line-deployment.md)
[další](troubleshooting.md)
