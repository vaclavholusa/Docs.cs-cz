---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Přidání Kontroleru (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Serivice aktualizací Service Pack 1, které i...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b31cf3bdf18c144d2735973119367b01de0353fe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753672"
---
<a name="adding-a-controller-c"></a>Přidání Kontroleru (C#)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka C# je k dispozici v tomto tématu. [Stáhněte si verzi C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


MVC jsou zahrnovaného *model-view-controller*. MVC je vzor pro vývoj aplikací, které jsou dobře navrženou a snadnou údržbou. Aplikace využívající architekturu MVC obsahují:

- Řadiče: Třídy, které zpracovávají příchozí požadavky na aplikace, načíst datový model a pak zadejte zobrazit šablony, které vrací odpověď klientovi.
- Modely: Třídy, které představují data aplikace a logiku ověřování, který slouží k vynucení obchodní pravidla pro tato data.
- Zobrazení: Soubory šablon, které vaše aplikace používá k dynamickému generování odpovědi HTML.

Společnost Microsoft a budete moct pokrývající všechny tyto koncepty v této řadě kurzů ukazují, jak se dají použít k sestavení aplikace.

Začněme tím, že vytvoříte třídu kontroleru. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *řadiče* složku a pak vyberte **přidat kontroler**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Pojmenujte nový kontroler "HelloWorldController". Ponechte výchozí šablony jako **prázdný kontroler** a klikněte na tlačítko **přidat**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Všimněte si, že v **Průzkumníka řešení** zda nový soubor byl vytvořen pojmenovaný *HelloWorldController.cs*. Soubor je otevřen v integrovaném vývojovém prostředí.

![](adding-a-controller/_static/image5.png)

Uvnitř `public class HelloWorldController` blokovat, vytvořte dvě metody, které vypadají jako v následujícím kódu. Vrátí řetězec HTML jako příklad kontroleru.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Kontrolér jmenuje `HelloWorldController` a se jmenuje první výše uvedené metody `Index`. Pojďme ho vyvolat z prohlížeče. Spusťte aplikaci (stisknutím klávesy F5 nebo Ctrl + F5). V prohlížeči připojí k cestě do adresního řádku "HelloWorld". (Například na obrázku níže, jeho `http://localhost:43246/HelloWorld.`) na stránce v prohlížeči bude vypadat jako na následujícím snímku obrazovky. Ve výše uvedené metody kód vrátil řetězec přímo. Jste uvedli jako systém právě vrátí kód HTML a udělal!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC vyvolá jiný kontroler třídy (a různé metody akcí v nich) v závislosti na příchozí adrese URL. Logika výchozí mapování používá ASP.NET MVC používá k určení jaký kód, který má být vyvolán formátu tímto způsobem:

`/[Controller]/[ActionName]/[Parameters]`

První část adresy URL určuje třída kontroleru k provedení. Takže */HelloWorld* mapuje `HelloWorldController` třídy. Druhá část adresy URL určí metodu akce v třídě ke spuštění. Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třídy ke spuštění. Všimněte si, že jsme museli procházet */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení. Důvodem je, že metodu s názvem `Index` představuje výchozí metodu, která bude volána na řadiči, pokud není explicitně zadaná.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec "Toto je metoda úvodní akce...". Výchozí mapování MVC `/[Controller]/[ActionName]/[Parameters]`. Pro tuto adresu URL kontroleru je `HelloWorld` a `Welcome` je metoda akce. Nebyly použity `[Parameters]` část adresy URL ještě.

![](adding-a-controller/_static/image7.png)

Pojďme mírně upravte příklad tak, aby některé informace o parametrech z adresy URL můžete předat do kontroleru (například */HelloWorld/uvítací? název = Scott&amp;numtimes = 4*). Změna vašeho `Welcome` tak, aby zahrnoval dva parametry, jak je znázorněno níže. Všimněte si, že kód používá volitelný parametr funkce jazyka C# k označení, že `numTimes` parametr by ve výchozím nastavení 1-li pro tento parametr není předána žádná hodnota.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Můžete vyzkoušet různé hodnoty pro `name` a `numtimes` v adrese URL. Systém automaticky mapují pojmenované parametry z řetězce dotazu do adresního řádku parametrům ve své metodě.

![](adding-a-controller/_static/image8.png)

V obou těchto příkladech kontroleru způsobem "VC" část MVC, tedy zobrazení a kontroler práce. Kontroler přímo vrací HTML. Obvykle nechcete řadiče vrácení HTML přímo, protože, který se stane velmi náročný kód. Místo toho obvykle použijeme soubor šablony samostatným zobrazením ke generování odpovědi HTML. Pojďme se podívat na tom, jak to můžeme udělat.

> [!div class="step-by-step"]
> [Předchozí](intro-to-aspnet-mvc-3.md)
> [další](adding-a-view.md)
