---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Přidávání řadiče (C#) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Serivice aktualizací Service Pack 1, které i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868292"
---
<a name="adding-a-controller-c"></a>Přidávání řadiče (C#)
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu. [Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


Zastupuje rozhraní MVC *model-view-controller*. MVC je vzor pro vývoj aplikací, které jsou dobře navrženou a snadnou údržbou. Aplikace založené na MVC obsahují:

- Řadiče: Třídy, které zpracovat příchozí požadavky na aplikaci načíst datový model a pak zadejte zobrazit šablony, které vrátí odpověď klientovi.
- Modely: Třídy, které představují data, aplikace a které používají logiku ověření vynutit obchodní pravidla pro tato data.
- Zobrazení: Soubory šablony, které vaše aplikace používá k dynamickému generování odpovědi HTML.

Jsme budete být pokrývajících tyto koncepty tento kurz řady a ukazují, jak je používat k sestavení aplikace.

Začněme vytvořením třídy kontroleru. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složku a potom vyberte **přidat kontroler**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Pojmenujte nový kontroler "HelloWorldController". Ponechte výchozí šablony jako **prázdný kontroler** a klikněte na tlačítko **přidat**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Všimněte si v **Průzkumníku řešení** aby nový soubor byla vytvořená s názvem *HelloWorldController.cs*. Soubor je otevřen v prostředí IDE.

![](adding-a-controller/_static/image5.png)

Uvnitř `public class HelloWorldController` blokovat, vytvořte dvě metody, které vypadají podobně jako následující kód. Vrátí řetězec HTML jako příklad kontroleru.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Název vašeho řadiče `HelloWorldController` a první metoda výše jmenuje `Index`. Budeme ji volat z prohlížeče. Spusťte aplikaci (stiskněte F5 nebo Ctrl + F5). V prohlížeči připojí k cestě v panelu Adresa "HelloWorld". (Například na obrázku níže, jeho `http://localhost:43246/HelloWorld.`) stránku v prohlížeči bude vypadat jako na následujícím snímku obrazovky. V metodě výše uvedený kód vrátil řetězec přímo. Je systém právě vrátí kód HTML v aplikaci, a to!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC volá jiné řadiče třídy (a různé metody akcí v nich), v závislosti na adresy URL příchozích. Logika výchozí mapování používá ASP.NET MVC používá k určení jaký kód k vyvolání tento formát:

`/[Controller]/[ActionName]/[Parameters]`

První část adresy URL určuje třídy kontroleru provést. Proto */HelloWorld* se mapuje `HelloWorldController` třídy. Druhá část adresy URL určuje metody akce v třídě provést. Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třída spustí. Všimněte si, že jsme museli vyhledejte */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení. Důvodem je, že metodu s názvem `Index` je výchozí metodou, která bude volána na řadiči, pokud není explicitně určen.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec "Toto je metoda úvodní akce...". Výchozí mapování MVC je `/[Controller]/[ActionName]/[Parameters]`. Pro tuto adresu URL, že je řadič `HelloWorld` a `Welcome` je metoda akce. Nebyly použity `[Parameters]` součástí ještě adresu URL.

![](adding-a-controller/_static/image7.png)

Pojďme upravit v příkladu mírně tak, aby můžete předat některé informace o parametrech z adresy URL do kontroleru (například */HelloWorld/Vítá? name = Scott&amp;numtimes = 4*). Změna vaší `Welcome` tak, aby zahrnoval dva parametry, jak je uvedeno níže. Všimněte si, že kód používá volitelný parametr funkcí jazyka C# k označení, že `numTimes` parametr by měl výchozí na 1, pokud pro tento parametr není předána žádná hodnota.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Můžete použít různé hodnoty pro `name` a `numtimes` v adrese URL. Systém automaticky mapuje pojmenované parametry z řetězce dotazu v panelu Adresa parametry ve své metodě.

![](adding-a-controller/_static/image8.png)

V obou těchto příkladech řadičem provádění část "VC" MVC – to znamená, zobrazení a kontroler práci. Řadičem přímo vrací HTML. Normálně nechcete, aby řadiče vrácení HTML přímo, vzhledem k tomu, který se stane velmi náročná kódu. Místo toho obvykle použijeme oddělená zobrazení souboru šablony ke generování odpovědi HTML. Podíváme se na tom, jak jsme to lze provést další.

> [!div class="step-by-step"]
> [Předchozí](intro-to-aspnet-mvc-3.md)
> [další](adding-a-view.md)
