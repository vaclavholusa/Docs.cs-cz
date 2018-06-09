---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Přidávání řadiče (VB) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 9a433083c31c7929f7599e52800c887f301d7727
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30870606"
---
<a name="adding-a-controller-vb"></a>Přidávání řadiče (VB)
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se VB.NET zdrojový kód je k dispozici v tomto tématu. [Stáhnout verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přepnout [C# verze](../cs/adding-a-controller.md) tohoto kurzu.


Zastupuje rozhraní MVC *model-view-controller*. MVC je vzor pro vývoj aplikací tak, aby měl každý část samostatné odpovědnost:

- Model: Data pro vaši aplikaci.
- Zobrazení: Soubory šablon vaše aplikace bude používat k dynamickému generování odpovědi HTML.
- Řadiče: Třídy, které zpracovávat příchozí požadavky na adresu URL pro aplikaci, načíst datový model a pak zadejte zobrazit šablony, které vykreslení odpovědi klientovi.

Budete se o tyto koncepty v tomto kurzu jsme ukazují, jak je používat k sestavení aplikace.

Vytvořit nový řadič kliknutím pravým tlačítkem myši *řadiče* složky v **Průzkumníku řešení** a potom výběrem **přidat kontroler**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Pojmenujte nový kontroler &quot;HelloWorldController&quot; a klikněte na tlačítko **přidat**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Všimněte si v **Průzkumníku řešení** na pravé straně s názvem, aby byl pro vás vytvořen nový soubor *HelloWorldController.cs* a zda je soubor otevřete v prostředí IDE.

Uvnitř nové `public class HelloWorldController` blokovat, vytvořte dvě nové metody, které vypadat podobně jako následující kód. Vrátí řetězec HTML přímo z řadiče jako příklad.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Název vašeho řadiče `HelloWorldController` a nová metoda jmenuje `Index`. Spusťte aplikaci (stiskněte F5 nebo Ctrl + F5). Jakmile je zahájen prohlížeč, připojit &quot;HelloWorld&quot; na cestu v panelu Adresa. (V mém počítači má `http://localhost:43246/HelloWorld`) prohlížeče bude vypadat podobně jako na následující snímek obrazovky. V metodě výše uvedený kód vrátil řetězec přímo. Jsme vás vyzval systém právě vrátí kód HTML, a to!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC volá jiné řadiče třídy (a různé metody akcí v nich), v závislosti na adresy URL příchozích. Logika výchozí mapování používá ASP.NET MVC používá k řízení, jaký kód je volána tento formát:

`/[Controller]/[ActionName]/[Parameters]`

První část adresy URL určuje třídy kontroleru provést. Proto */HelloWorld* se mapuje `HelloWorldController` třídy. Druhá část adresy URL určuje metody akce v třídě provést. Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třída spustí. Všimněte si, že jsme museli navštívit */HelloWorld* výše a `Index` metoda byla použita ve výchozím nastavení. Důvodem je, že metodu s názvem `Index` je výchozí metodou, která bude volána na řadiči, pokud není explicitně určen.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec &quot;metodu úvodní akce... &quot;. Výchozí mapování MVC je `/[Controller]/[ActionName]/[Parameters]`. Pro tuto adresu URL, že je řadič `HelloWorld` a `Welcome` je metoda. Jsme nepoužili `[Parameters]` součástí ještě adresu URL.

![](adding-a-controller/_static/image6.png)

Pojďme upravit v příkladu mírně tak, aby řadič můžete předat některé informace o parametrech v z adresy URL (například */HelloWorld/Vítá? název = Scott&amp;numtimes = 4*). Změna vaší `Welcome` tak, aby zahrnoval dva parametry, jak je uvedeno níže. Všimněte si, že jsme k označení, že jste použili funkci VB volitelný parametr `numTimes` parametr by měl výchozí na 1, pokud pro tento parametr není předána žádná hodnota.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Spusťte aplikaci a přejděte do `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Můžete použít různé hodnoty pro `name` a `numtimes`. Systém automaticky mapuje pojmenované parametry z řetězec vašeho dotazu na panelu Adresa parametry ve své metodě.

![](adding-a-controller/_static/image7.png)

V obou těchto příkladech řadičem provádění VC část MVC – se pracovní zobrazení a kontroler. Řadičem přímo vrací HTML. Normálně Neradi bychom řadiče vrácení HTML přímo, vzhledem k tomu, který se stane velmi náročná kódu. Místo toho obvykle použijeme oddělená zobrazení souboru šablony ke generování odpovědi HTML. Podíváme, jak jsme to můžete provést.

> [!div class="step-by-step"]
> [Předchozí](intro-to-aspnet-mvc-3.md)
> [další](adding-a-view.md)
