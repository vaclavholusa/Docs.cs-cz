---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Přidání Kontroleru (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b38f3c4051af426e471568b8a25a471986f07209
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576224"
---
<a name="adding-a-controller-vb"></a>Přidání Kontroleru (VB)
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu. [Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/adding-a-controller.md) tohoto kurzu.


MVC jsou zahrnovaného *model-view-controller*. MVC je vzor pro vývoj aplikací tak, aby každá část má samostatné odpovědnosti:

- Model: Data pro vaši aplikaci.
- Zobrazení: Soubory šablon, které vaše aplikace bude používat k dynamickému generování odpovědi HTML.
- Řadiče: Třídy, které zpracovávají příchozí žádosti adresy URL pro aplikaci, načíst datový model a zadejte zobrazit šablony, které vykreslují odpověď klientovi.

Společnost Microsoft a budete moct pokrývající všechny tyto koncepty v tomto kurzu ukazují, jak se dají použít k sestavení aplikace.

Vytvořit nový kontroler kliknutím pravým tlačítkem myši *řadiče* složky v **Průzkumníka řešení** a následným výběrem **přidat kontroler**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Pojmenujte nový kontroler &quot;HelloWorldController&quot; a klikněte na tlačítko **přidat**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Všimněte si, že v **Průzkumníka řešení** na pravé straně s názvem, že je pro vás vytvořil nový soubor *HelloWorldController.cs* a zda je soubor otevřen v integrovaném vývojovém prostředí.

Uvnitř nové `public class HelloWorldController` blokovat, vytvořte dva nové metody, které vypadají jako v následujícím kódu. Vrátí řetězec HTML přímo z kontroleru jako příklad.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Kontrolér jmenuje `HelloWorldController` a názvem nové metody `Index`. Spusťte aplikaci (stisknutím klávesy F5 nebo Ctrl + F5). Po zahájení prohlížeči připojte &quot;HelloWorld&quot; na cestu v panelu Adresa. (V tomto počítači má `http://localhost:43246/HelloWorld`) prohlížeče bude vypadat jako na následujícím snímku obrazovky. Ve výše uvedené metody kód vrátil řetězec přímo. Řekli jsme systém právě vrátí kód HTML a udělal!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC vyvolá jiný kontroler třídy (a různé metody akcí v nich) v závislosti na příchozí adrese URL. Logika výchozí mapování používá ASP.NET MVC používá řídit, jaký kód je vyvolána formát takto:

`/[Controller]/[ActionName]/[Parameters]`

První část adresy URL určuje třída kontroleru k provedení. Takže */HelloWorld* mapuje `HelloWorldController` třídy. Druhá část adresy URL určí metodu akce v třídě ke spuštění. Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třídy ke spuštění. Všimněte si, že jsme museli navštívit */HelloWorld* výše a `Index` metoda byla použita ve výchozím nastavení. Důvodem je, že metodu s názvem `Index` představuje výchozí metodu, která bude volána na řadiči, pokud není explicitně zadaná.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec &quot;Toto je metoda úvodní akce... &quot;. Výchozí mapování MVC `/[Controller]/[ActionName]/[Parameters]`. Pro tuto adresu URL kontroleru je `HelloWorld` a `Welcome` je metoda. Jsme nepoužili `[Parameters]` část adresy URL ještě.

![](adding-a-controller/_static/image6.png)

Pojďme mírně upravte příklad tak, aby se dají předat některé informace o parametrech v z adresy URL do kontroleru (například */HelloWorld/uvítací? název = Scott&amp;numtimes = 4*). Změna vašeho `Welcome` tak, aby zahrnoval dva parametry, jak je znázorněno níže. Všimněte si, že jsme použili nepovinný parametr funkce jazyka Visual Basic s údajem, `numTimes` parametr by ve výchozím nastavení 1-li pro tento parametr není předána žádná hodnota.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Spusťte aplikaci a přejděte do `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Můžete vyzkoušet různé hodnoty pro `name` a `numtimes`. Systém automaticky mapují pojmenované parametry v řetězci dotazu do adresního řádku parametrům ve své metodě.

![](adding-a-controller/_static/image7.png)

V obou těchto příkladech kontroleru způsobem VC část MVC – to je práce zobrazení a kontroler. Kontroler přímo vrací HTML. Obvykle bychom řadiče vrácení HTML přímo, protože, který se stane velmi náročný kód. Místo toho obvykle použijeme soubor šablony samostatným zobrazením ke generování odpovědi HTML. Podívejme se na tom, jak to můžeme udělat.

> [!div class="step-by-step"]
> [Předchozí](intro-to-aspnet-mvc-3.md)
> [další](adding-a-view.md)
