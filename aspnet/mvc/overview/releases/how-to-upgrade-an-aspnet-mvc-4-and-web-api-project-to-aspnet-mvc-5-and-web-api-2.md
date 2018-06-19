---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Postup upgradu ASP.NET MVC 4 a webového projektu rozhraní API a rozhraní Web API 2 ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 a webovém rozhraní API 2 Přepněte hostitele nových funkcí, včetně směrováním atributů, filtry ověřování a mnoho dalšího.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f61502933a5ba92896ee97cef9cff915fe23831d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874727"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Postup upgradu rozhraní ASP.NET MVC 4 a projekt webového rozhraní API a rozhraní Web API 2 ASP.NET MVC 5
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 a webovém rozhraní API 2 Přepněte hostitele nových funkcí, včetně směrováním atributů, filtry ověřování a mnoho dalšího. V tématu [ https://www.asp.net/vnext ](https://www.asp.net/core) další podrobnosti.
> 
> Tento návod vám pomohou s kroky nutné k upgradu vaší aplikace na nejnovější verzi.  
> 
> > [!NOTE]
> > Najdete v tématu [ASP.NET a nástroje Web Tools pro Visual Studio 2013 verze poznámky](../../../visual-studio/overview/2013/release-notes.md) informace o nejnovější změny z MVC 4 a webového rozhraní API na další verzi.
> 
>   
> 
> Tento článek napsal Youngjune Hongkong a Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Postup upgradu

1. Zálohování projektu. Tento názorný postup bude vyžadovat vám umožní provádět změny k souboru projektu, konfigurace balíčku a souborů web.config.
2. Pro upgrade webového rozhraní API na webovém rozhraní API 2 v souboru global.asax, změňte:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   až

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Zajistěte, aby všechny balíčky, které použít projekty jsou kompatibilní s MVC 5 a webovém rozhraní API 2. Následující tabulka uvádí MVC 4 a webového rozhraní API související s balíčky, než se musí změnit. Pokud máte balíček, který je závislý na jednom z balíčky uvedené níže, obraťte se na vydavatele získat novější verze, které jsou kompatibilní s MVC 5 a webovém rozhraní API 2. Pokud máte zdrojový kód pro tyto balíčky, by je znovu zkompiluje s nové sestavení MVC 5 a webovém rozhraní API 2.   

    | **Id balíčku** | **Stará verze** | **Nová verze** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | <o:p> </o:p> | Odebrat |
    | Microsoft.AspNet.WebPages.Administration | <o:p> </o:p> | Odebrat |
    | Microsoft-Web-Helpers | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft-weboví Pomocníci nahradilo Microsoft.AspNet.WebHelpers. Doporučujeme nejprve odeberte starý balíček a pak nainstalujte novější balíček.   
    >   
    > Neexistuje žádné křížové verze kompatibility mezi hlavní balíčků ASP.NET. Například je kompatibilní pouze s Razor 3 a ne Razor 2 MVC 5.
4. Otevřete projekt v sadě Visual Studio 2013.
5. Odeberte některé z následujících balíčků ASP.NET NuGet, které jsou nainstalovány. Odeberete tyto pomocí konzoly Správce balíčků (PMC). Otevřete pomocí PMC, vyberte **nástroje** nabídce a potom vyberte **Správce balíčků knihoven** vyberte **Konzola správce balíčků**. Projekt nemusí zahrnovat všechny z nich.

    1. `Microsoft.AspNet.WebPages.Administration`  
   Tento balíček je obvykle přidat při upgradu z MVC 3 pro MVC 4. Jej Pokud chcete odstranit, spusťte následující příkaz v pomocí PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   Tento balíček obsahuje byla přejmenované jako `Microsoft.AspNet.WebHelpers`. Jej Pokud chcete odstranit, spusťte následující příkaz v pomocí PMC:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   Tento balíček obsahuje pracovní kolem pro chyby v MVC 4, který byl vyřešen v MVC 5. Jej Pokud chcete odstranit, spusťte následující příkaz v pomocí PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Upgradujte všech balíčků ASP.NET NuGet pomocí pomocí PMC. Pomocí PMC spusťte následující příkaz:  
    `Update-Package`  
   `Update-Package` Příkaz bez žádné parametry aktualizuje všechny balíčky. Balíčky můžete aktualizovat samostatně pomocí ID argumentu. Další informace o příkazu aktualizace, spusťte `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Aktualizovat aplikaci *web.config* souboru

Ujistěte se, aby se tyto změny v aplikaci *web.config* souboru není *web.config* v soubor *zobrazení* složky.

Vyhledejte `<runtime>/<assemblyBinding>` části a proveďte následující změny:

1. V elementů s názvem atributu "System.Web.Mvc" změňte číslo verze z "4.0.0.0" na "5.0.0.0". (Dvě změny v tomto elementu.)
2. V elementů s názvem atributu &quot;System.Web.Helpers "a &quot;System.Web.WebPages&quot; změnit číslo verze z"2.0.0.0"na"3.0.0.0". Čtyři změny se projeví, dva v jednotlivých prvků.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Vyhledejte `<appSettings>` části a aktualizovat webpages:version z 2.0.0.0.0 k 3.0.0.0, jak je uvedeno níže:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Odeberte všechny úrovně důvěryhodnosti než úplné. Příklad:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Aktualizace *web.config* souborů ve složce zobrazení

Pokud vaše aplikace používá oblasti, budou také musíte aktualizovat všechny *web.config* v soubor *zobrazení* dílčí složku složky pro každou oblast.

1. Aktualizujte všechny elementy, které obsahují "System.Web.Mvc" verze "4.0.0.0" verze "5.0.0.0".  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Aktualizujte všechny elementy, které obsahují "System.Web.WebPages.Razor" verze "2.0.0.0" verze "3.0.0.0". Pokud tato část obsahuje "System.Web.WebPages", aktualizujte tyto elementů od verze "2.0.0.0" verze "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Pokud jste odebrali `Microsoft-Web-Helpers` instalace balíčku NuGet v předchozím kroku, `Microsoft.AspNet.WebHelpers` pomocí následujícího příkazu v pomocí PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Pokud vaše aplikace používá [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) metoda, přidejte následující *Web.config* souboru.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Závěrečné kroky

Sestavení a testování aplikace.

Typ projektu MVC 4 GUID odebrání souborů projektu.

1. V Průzkumníku řešení klikněte pravým tlačítkem na název projektu a potom vyberte **uvolnit projekt**.
2. Klikněte pravým tlačítkem na projekt a vyberte Upravit ProjectName.csproj.
3. Vyhledejte `ProjectTypeGuids` elementu a potom odeberte MVC 4 projekt identifikátor GUID, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Uložte a zavřete soubor otevřít projekt.
5. Klikněte pravým tlačítkem na projekt a vyberte **znovu načíst projekt**.
