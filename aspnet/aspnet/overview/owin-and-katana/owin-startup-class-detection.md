---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: "Detekce třídy pro spuštění OWIN | Microsoft Docs"
author: Praburaj
description: "Tento kurz ukazuje, jak nakonfigurovat, které třídy pro spuštění OWIN je načtena. Další informace o OWIN najdete v části Přehled Katana projektu. V tomto kurzu se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: a6ac34307b7558ad13684448f339ca74ade9e997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="owin-startup-class-detection"></a>Detekce třídy pro spuštění OWIN
====================
podle [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Tento kurz ukazuje, jak nakonfigurovat, které třídy pro spuštění OWIN je načtena. Další informace o OWIN najdete v tématu [Přehled projektu Katana](an-overview-of-project-katana.md). V tomto kurzu byla zapsána od Ricka Andersona ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan a Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Požadavky
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Detekce třídy pro spuštění OWIN

 Každá aplikace OWIN obsahuje třídu, spuštění, kde můžete určit komponenty pro kanálu aplikace. Existují různé způsoby připojíte třídě spuštění s modulem runtime, v závislosti na hostování modelu, které zvolíte (OwinHost, IIS a služby IIS Express). Třída při spuštění uvedené v tomto kurzu lze použít v každé hostitelskou aplikaci. Třída při spuštění se připojit pomocí hostování modulu runtime, který se blíží mezi tyto:  

1. **Zásady vytváření názvů**: Katana hledá třídy s názvem `Startup` v oboru názvů odpovídající název sestavení nebo obor názvů globální.
2. **Atribut OwinStartup**: Jedná se o postup Většina vývojářů bude trvat zadejte třída při spuštění. Následující atribut nastaví třída při spuštění `TestStartup` třídy v `StartupDemo` oboru názvů. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 `OwinStartup` Atribut přepsání zásady vytváření názvů. Také můžete zadat popisný název k tomuto atributu, ale pomocí popisný název vyžaduje, abyste také použít `appSetting` element v konfiguračním souboru.
3. **Element appSetting v konfiguračním souboru**: `appSetting` element přepsání `OwinStartup` atribut a zásady vytváření názvů. Může mít několik tříd spuštění (každý využívající `OwinStartup` atribut) a konfigurace, které třída při spuštění bude načten do konfiguračního souboru pomocí značek podobný následujícímu:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 Následující klíč, který explicitně určuje třídy pro spuštění a sestavení lze také použít: 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 Následující kód XML v konfiguračním souboru Určuje název třídy popisný spuštění `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 Výše uvedený kód musí být použit s následující `OwinStartup` atribut, který určuje popisný název a způsobí, že `ProductionStartup2` třídy ke spuštění.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Chcete-li zakázat zjišťování OWIN při spuštění přidejte `appSetting owin:AutomaticAppStartup` s hodnotou `"false"` v souboru web.config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Vytvoření webové aplikace ASP.NET pomocí OWIN při spuštění

1. Vytvořit prázdnou webovou aplikaci Asp.Net a pojmenujte ji **StartupDemo**. -Instalovat `Microsoft.Owin.Host.SystemWeb` pomocí Správce balíčků NuGet. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a potom **Konzola správce balíčků**. Zadejte následující příkaz:  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- Přidání třídy pro spuštění OWIN. V sadě Visual Studio 2013 klikněte pravým tlačítkem na projekt a vyberte **přidat třídu**. - v **přidat novou položku** dialogové okno zadejte *OWIN* do pole hledání a změňte název na Startup.cs, a pak klikněte na **přidat**.  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 Další čas, který chcete přidat *třídy pro spuštění Owin*, bude ve dostupné z **přidat** nabídky.  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 Alternativně můžete klikněte pravým tlačítkem na projekt a vyberte **přidat**, pak vyberte **nová položka**a pak vyberte **třídy pro spuštění Owin**.  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- Nahraďte generovaného kódu v *Startup.cs* soubor s následující:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 `app.Use` Výrazu lambda slouží k registraci komponenty zadaný middleware do kanálu OWIN. V takovém případě nastavujeme protokolování příchozích požadavků před reagovat na příchozí požadavek. `next` Parametr je delegáta ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [úloh](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) další komponentou v kanálu. `app.Run` Výrazu lambda zachytí až kanálu na příchozí požadavky a poskytuje mechanismus odpovědi.
     > [!NOTE]
     > Ve výše uvedeném kódu, budeme mít komentované `OwinStartup` atribut a My se spoléhat na konvence spuštěných třída s názvem `Startup` .-stiskněte ***F5*** ke spuštění aplikace. Dosáhl aktualizace několikrát.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
Poznámka: Na číslo zobrazené v obrázcích v tomto kurzu nebudou odpovídat na číslo se zobrazí. Milisekundu řetězec se používá k zobrazení novou odpověď, když obnovíte stránku.  
 Zobrazí se informace trasování v **výstup** okno.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Přidat další třídy spuštění

V této části přidáme jiné třídy spuštění. Můžete přidat více třídy pro spuštění OWIN do vaší aplikace. Můžete například vytvořit spuštění třídy pro vývoj, testování a produkci.

1. Vytvořte novou třídu OWIN při spuštění a pojmenujte ji `ProductionStartup`.
2. Generovaného kódu nahraďte následujícím textem:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Stisknutím klávesy F5 řízení a spusťte aplikaci. `OwinStartup` Určuje atribut třída při spuštění produkční běží.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Vytvořte další OWIN při spuštění třídu s názvem `TestStartup`.
5. Generovaného kódu nahraďte následujícím textem:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 `OwinStartup` Určuje atribut přetížení výše `TestingConfiguration` jako *popisný* název třída při spuštění.
6. Otevřete *web.config* souboru a přidejte spouštěcí klíč aplikace OWIN, který určuje popisný název třídy spuštění:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Stisknutím klávesy F5 řízení a spusťte aplikaci. Element nastavení aplikace trvá předcházejících a testovací konfigurace běží.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Odebrat *popisný* název z `OwinStartup` atribut `TestStartup` třídy.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Nahradit spouštěcí klíč aplikace OWIN v *web.config* soubor s následující:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Vrátit zpět `OwinStartup` atribut v každé třídě ve výchozím kódu atribut vytvořen sadou Visual Studio:  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 Všechny tyto klíče aplikace OWIN při spuštění způsobí, že třída produkční ke spuštění. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 Klíč poslední spuštění určuje metodu spuštění konfigurace. Následující spouštěcí klíč OWIN aplikace můžete změnit název třídy pro konfiguraci `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Pomocí Owinhost.exe

1. Nahraďte v souboru Web.config následující kód:  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 Klíč poslední služby wins, tak v tomto případě `TestStartup` je zadán.
2. Nainstalujte Owinhost z pomocí PMC: 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Přejděte do složky aplikace (složku, která obsahuje *Web.config* souboru) a na příkazový řádek a zadejte: 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 Příkazové okno se zobrazí: 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Spustit prohlížeč s adresou URL `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 OwinHost dodržení konvence spuštění uvedené výše.
5. V příkazovém okně stisknutím klávesy Enter ukončete OwinHost.
6. V `ProductionStartup` třídy, přidejte následující atribut OwinStartup, který určuje popisný název *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Do příkazového řádku a zadejte: 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 Třída při spuštění produkční je načten.  
    ![](owin-startup-class-detection/_static/image9.png)  
 Naše aplikace má více tříd, spuštění a v tomto příkladu jsme mají odložené které třída při spuštění načíst do modulu runtime.
8. Testovací následující možnosti při spuštění modulu runtime:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
