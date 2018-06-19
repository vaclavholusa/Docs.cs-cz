---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Vytváření kapitoly soubory ke stažení pro EF 5 MVC 4 kurzy | Microsoft Docs
author: Rick-Anderson
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: bda7effabd715e4658d2472e1f0a66d7bba18cab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878513"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Vytváření kapitoly soubory ke stažení pro EF 5 MVC 4 kurzy
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Vytváření kapitoly stahování

1. Stáhněte a rozbalte soubor zip ukázkový projekt. V rozbalené stažení balíčku najdete další zip soubory, jeden pro dokončení každé kapitoly.
2. Klikněte pravým tlačítkem na soubor zip požadované, klikněte na tlačítko **vlastnosti**a klikněte na **Odblokovat** tlačítko.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Rozbalte soubor.
4. Dvakrát klikněte *CUx.sln* soubor spusťte sadu Visual Studio.
5. Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak **Konzola správce balíčků**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. V balíček správce konzoly (pomocí PMC), klikněte na tlačítko **obnovení**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Ukončete aplikaci Visual Studio.
8. Restartujte Visual Studio, otevřete soubor řešení uzavřený v předchozích krocích.
9. V balíček správce konzoly (pomocí PMC), zadejte `Update-Database` příkaz:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Pokud dojde k následující chybě:  
    >   
    >  *Termín 'Update-Database' nebyl rozpoznán jako název rutiny, funkce, soubor skriptu nebo spustitelného programu. Zkontrolujte, zda název, nebo pokud byl zahrnut cestu, ověřte, zda je cesta správná a zkuste to znovu.*  
    > Ukončete a restartujte Visual Studio.

    Spustí každý migrace a potom se spustí metodu počáteční hodnoty. Teď můžete spustit aplikace.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Předchozí](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
