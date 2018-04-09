---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programování ASP.NET Web Pages (Razor) pomocí sady Visual Studio | Microsoft Docs
author: tfitzmac
description: Tento dodatek vysvětluje, jak můžete pomocí sady Visual Studio 2010 nebo Visual Web Developer 2010 Express programu rozhraní ASP.NET Web Pages se syntaxí Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programování webových stránek ASP.NET (Razor) pomocí sady Visual Studio
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak používat Visual Studio nebo Visual Web Developer Express programu weby technologie ASP.NET Web Pages (Razor).
> 
> Co se dozvíte
> 
> - Co potřebujete k instalaci pro práci s webové stránky ASP.NET ve vaší verzi sady Visual Studio (Pokud se nic).
> - Jak přidat podporu pro ASP.NET Web Pages Visual Web Developer 2010 Express.
> - Jak používat funkce v sadě Visual Studio pro práci s stránky ASP.NET Razor, včetně technologie IntelliSense a ladicí program.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - Služba WebMatrix 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 a službě WebMatrix 2.


Můžete naprogramovat ASP.NET Web pages se syntaxí Razor pomocí služby WebMatrix nebo mnoha jiných editory kódu. Můžete také použít Microsoft Visual Studio, která je plně funkční integrované vývojové prostředí (IDE), které poskytuje výkonnou sadu nástrojů pro vytváření mnoho typů aplikací (nikoli pouze weby). Chcete-li pracovat s stránky ASP.NET Razor, můžete buď pomocí jedné z úplné vydání sady Visual Studio nebo bezplatnou [Visual Studio Express pro Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express) edition.

Jsou dva zvláště užitečné funkce, které Visual Studio poskytuje pro programování s ASP.NET Razor webové stránky:

- *IntelliSense*. Funkci IntelliSense, která je součástí sady Visual Studio je komplexnější než IntelliSense ve službě WebMatrix.
- *Ladicí program*. Ladicí program umožňuje odstraňovat kódu po zastavení program je spuštěn, prozkoumání proměnné a procházení řádky kódu.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Pomocí sady Visual Studio s různými verzemi nástroje ASP.NET – webové stránky

Visual Studio 2012 a Visual Studio 2013 zahrnují podporu pro ASP.NET Web Pages. (Balíčky, které jsou požadovány pro podporu rozhraní ASP.NET Web Pages jsou nainstalovány při instalaci sady Visual Studio.)

Visual Studio 2010 nezahrnuje podpora ve výchozím nastavení pro rozhraní ASP.NET Web Pages. Použít rozhraní ASP.NET Web Pages pomocí sady Visual Studio 2010, je nutné nainstalovat balíček ASP.NET MVC. Získat ASP.NET Web Pages 2, nainstalujte technologii ASP.NET MVC 4.

Následující tabulka shrnuje podporu pro ASP.NET Web Pages v různých verzích sady Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **Rozhraní ASP.NET Web Pages 2** | Instalace technologie ASP.NET MVC 4 | (Zahrnout) | (Zahrnout) |
| **Rozhraní ASP.NET Web Pages 3** |  | Aktualizace pro rozhraní ASP.NET Web Pages 3 až NuGet | (Zahrnout) |

Chcete-li pracovat s Visual Studio 2010, přečtěte si téma [instalaci podporu pro webové stránky ASP.NET v sadě Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Spuštění sady Visual Studio ze služby WebMatrix

Pokud jste spustili projektu ve službě WebMatrix a chcete přepnout do sady Visual Studio, služba WebMatrix poskytuje tlačítko snadno otevřete projekt v sadě Visual Studio. Musíte mít nainstalovanou sadu Visual Studio v počítači pro toto tlačítko Povolit. Následující obrázek ukazuje tlačítko ve službě WebMatrix.

![Spusťte sadu Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Když kliknete na tlačítko, otevření projektu v sadě Visual Studio. Můžete přepnout přepínat mezi WebMatrix a Visual Studio bez problémů. Pokud soubory změnily v jiném prostředí a musí být znovu k získání nejnovějších změn, budete upozorněni.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Vytváření webu Razor rozhraní ASP.NET v sadě Visual Studio

Vytvoření webu k syntaxe Razor rozhraní ASP.NET v sadě Visual Studio:

1. Spusťte Visual Studio nebo Visual Web Developer.
2. V **soubor** nabídky, klikněte na tlačítko **nový web**.

    ![Vytvořit nový web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. V **nový web** dialogovém okně vyberte jazyk, který chcete použít (Visual C# nebo Visual Basic).
4. Vyberte **webové stránky ASP.NET (Razor)** šablony.

    ![lokality Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Click **OK**.

Nový projekt existuje a je naplněna některé výchozí webové stránky můžete začít pracovat.

### <a name="using-intellisense"></a>Používání atributu IntelliSense

Teď, když jste vytvořili lokalitu, uvidíte, jak funguje technologie IntelliSense v sadě Visual Studio.

1. Na webu, kterou jste právě vytvořili, otevřete *Default.cshtml* stránky.
2. Po `<h3>` značky na stránce zadejte `@ServerInfo.` (včetně tečky). Všimněte si, jak technologie IntelliSense zobrazí dostupné metody pro `ServerInfo` pomocné rutiny v rozevíracím seznamu. 

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Vyberte `GetHtml` metoda ze seznamu a potom stiskněte klávesu Enter. IntelliSense automaticky vyplní metodu. (Jako u jakékoli metody v jazyce C#, je nutné přidat `()` znaků po metodě.)  
   Dokončený kód `GetHtml` metoda vypadá jako v následujícím příkladu:  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Stisknutím kombinace kláves Ctrl + F5 a spusťte stránky. Je to, co bude stránka vypadat, když se zobrazí v prohlížeči: 

    ![výchozí stránku v prohlížeči](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Zavřete prohlížeč.

### <a name="using-the-debugger"></a>Používání ladicího programu

1. V horní části *Default.cshtml* stránka po řádek, který začíná `Page.Title`, přidejte následující řádek kódu: 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Šedé okraji editoru nalevo od kódu, klikněte na tlačítko Další tento nový řádek. Chcete-li přidat *zarážek*. Zarážka je značku, která říká službě ladicí program na zastavení od tohoto okamžiku spuštění programu, abyste viděli, co se děje.

    ![Sada zarážek](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Odeberte volání `ServerInfo.GetHtml` metoda a přidejte volání `@myTime` proměnné na příslušné místo. Toto volání se zobrazí aktuální čas hodnotu, která vrátí nový řádek kódu.
4. Stisknutím klávesy F5 spusťte stránku v ladicím programu. Stránce zastaví na zarážce, kterou nastavíte. Následující obrázek znázorňuje, co bude stránka vypadat v editoru se zarážkou (v žlutý). 

    ![Breakpoint – ladění](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Na panelu nástrojů ladění, klikněte **Krokovat s vnořením** tlačítko (nebo stiskněte klávesu F11) ke spuštění na další řádek kódu. Pokaždé, když na toto tlačítko přechodu provádění na dalším řádku kódu.

    ![Krokovat s vnořením tlačítko](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Zkontrolujte hodnotu `myTime` proměnné tak, že podržíte ukazatel myši nad ním nebo zkontrolováním hodnoty zobrazené na **místní hodnoty –** a **zásobníkem volání** systému windows. Visual Studio zobrazí hodnotu proměnné.

    ![Hodnota času zobrazení](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Po dokončení zkoumání proměnnou a procházení kód, stiskněte klávesu F5, aby kontinuálně běžely stránce bez zastavení na každém řádku. Pokud jste dokončili procházení všech kód, prohlížeč zobrazí stránku.

Další informace o ladicího programu a o tom, jak ladění kódu v sadě Visual Studio najdete v tématu [návod: ladění webové stránky v aplikaci Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Pomocí syntaxe Razor rozhraní ASP.NET MVC projektů pomocí sady Visual Studio

Také se používá hojně používá syntaxi Razor, v projekty ASP.NET MVC. MVC je výkonný, na základě vzory způsob, jak vytvářet dynamické weby. Pokud váš web ASP.NET Web Pages bude obtížné, můžete převod na aplikace ASP.NET MVC. Příklad vytvoření aplikace MVC, naleznete v části [Začínáme s ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalace podpory pro webové stránky ASP.NET v sadě Visual Studio 2010

V této části ukazuje, jak nainstalovat Visual Web Developer Express 2010 a nástroje webové stránky ASP.NET pro Visual Studio.

1. Pokud ještě nemáte služby instalace webové platformy, můžete ji stáhněte z následující adresy URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Spuštění instalačního programu webové platformy.
3. Klikněte **produkty** kartě.

    ![Karta produkty WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Vyhledejte **ASP.NET MVC 4** (pro ASP.NET Web Pages 2) a potom klikněte na **přidat**. Zahrnout tyto produkty Visual Studio tools pro vytváření webů ASP.NET Razor.

    ![Možnosti instalace WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Klikněte na tlačítko **nainstalovat** k dokončení instalace.
