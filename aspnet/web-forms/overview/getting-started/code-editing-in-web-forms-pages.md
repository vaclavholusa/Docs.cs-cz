---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Úprava kódu webových formulářů ASP.NET v sadě Visual Studio 2013 | Dokumentace Microsoftu
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 670f81ca1ef9923575cb2fee1747f06f426963d8
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391216"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Kód úpravy webových formulářů ASP.NET v sadě Visual Studio 2013
====================
podle [Erik Reitan](https://github.com/Erikre)

V mnoha stránkách webový formulář ASP.NET napíšete kód v jazyce Visual Basic, C# nebo jiném jazyce. Editor kódu v sadě Visual Studio vám umožňují psát kód rychle a pomáhá předejít chybám. Kromě toho nabízí editor způsoby, jak vytvořit opakovaně použitelný kód snížit množství práce, kterou je třeba provést.

Tento návod ukazuje různé funkce editoru kódu sady Visual Studio.

V tomto návodu se dozvíte, jak:

- Opravte chyby kódování vložené.
- Refaktorovat a přejmenovat kódu.
- Přejmenujte proměnných a objektech.
- Vložte fragmenty kódu.

## <a name="prerequisites"></a>Požadavky


K dokončení tohoto návodu budete potřebovat:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Rozhraní .NET Framework se instaluje automaticky. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 for Web bude často se označuje jako Visual Studio v celé této sérii kurzů.  
    >   
    > Pokud používáte Visual Studio, Tento názorný průvodce předpokládá, že jste vybrali **vývoj pro Web** kolekce nastavení při prvním spuštění sady Visual Studio. Další informace najdete v tématu [postupy: Výběr nastavení prostředí vývoje webu](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Vytvoření projektu webové aplikace a na stránce

<a id="sectionToggle0"></a>

V této části tohoto návodu vytvoříte projekt webové aplikace a přidejte novou stránku do ní.

### <a name="to-create-a-web-application-project"></a>Chcete-li vytvořit projekt webové aplikace

1. Otevřete Microsoft Visual Studio.
2. Na **souboru** nabídce vyberte možnost **nový projekt**.  
    ![Nabídka Soubor](code-editing-in-web-forms-pages/_static/image1.png)

    **Nový projekt** zobrazí se dialogové okno.
3. Vyberte **šablony**  - &gt; **Visual C#**  - &gt; **webové** šablony skupiny na levé straně.
4. Zvolte **webová aplikace ASP.NET** šablon v prostředním sloupci.
5. Pojmenujte svůj projekt ***BasicWebApp*** a klikněte na tlačítko **OK** tlačítko.   
![Dialogové okno Nový projekt](code-editing-in-web-forms-pages/_static/image2.png)
6. V dalším kroku vyberte **webových formulářů** šablony a kliknutím **OK** tlačítko pro vytvoření projektu.  
![Dialogové okno Nový projekt ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio vytvoří nový projekt, který obsahuje předem připravených funkce na základě šablony webových formulářů.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Vytváří se nové technologie ASP.NET webové stránky s formuláři


Při vytváření nové aplikace webových formulářů pomocí **webová aplikace ASP.NET** šablony projektu, Visual Studio přidá stránky ASP.NET (webové formuláře – stránka) s názvem *Default.aspx*, stejně jako několik dalších souborů a složky. Můžete použít *Default.aspx* stránku jako domovské stránky pro webové aplikace. Ale v tomto návodu vytvoříte a pracovat s novou stránku.

### <a name="to-add-a-page-to-the-web-application"></a>Přidání stránky do webové aplikace


1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na název webové aplikace (v tomto kurzu je název aplikace **BasicWebSite**) a potom klikněte na tlačítko **přidat**  - &gt; **Nová položka**.   
**Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **webové** šablony skupiny na levé straně. Vyberte **webový formulář** uprostřed seznamu a pojmenujte ho *FirstWebPage.aspx*.   
    ![Přidat novou položku – dialogové okno](code-editing-in-web-forms-pages/_static/image4.png)
3. Klikněte na tlačítko **přidat** přidat na stránku webové formuláře do projektu.  
 Visual Studio vytvoří novou stránku a otevře jej.
4. Dále nastavte tuto novou stránku jako výchozí úvodní stránka. V **Průzkumníka řešení**, klikněte pravým tlačítkem na novou stránku s názvem *FirstWebPage.aspx* a vyberte **nastavit jako úvodní stránku**. Při příštím spuštění této aplikace pro testování náš postup, se automaticky zobrazí tuto novou stránku v prohlížeči.


## <a name="correcting-inline-coding-errors"></a>Oprava chyby kódování vložené


Editor kódu v sadě Visual Studio vám umožní zabránit chybám při psaní kódu, a pokud jste chybu, editor kódu umožňuje k opravě chyby. V této části tohoto návodu budete psát jediného řádku kódu, které ilustrují funkce opravy chyb v editoru.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Chcete-li opravit chyby jednoduché psaní kódu v sadě Visual Studio


1. V **návrhu** dvakrát klikněte na prázdnou stránku vytvořit obslužnou rutinu události pro **zatížení** události pro danou stránku.   
   Obslužná rutina události používáte pouze jako bod kódu.
2. Uvnitř obslužné rutiny, zadejte následující řádek, který obsahuje chybu a stisknutím klávesy **ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Když stisknete klávesu **ENTER**, editor kódu umístí červenou a zelenou vlnovkou (obvykle volání &quot;podtržení&quot; řádky) v rámci oblasti kódu, které mají problémy. Zelená podtržení zobrazuje varování. Červené podtržení označuje chybu, která je potřeba opravit. 

    Podržte ukazatel myši nad `myStr` zobrazíte popisek, který vás informuje o upozornění. Také podržte ukazatel myši nad červenou vlnovkou, chcete-li zobrazit chybová zpráva.

    Následující obrázek ukazuje kód podtržení.

    ![Úvodní text v návrhovém zobrazení](code-editing-in-web-forms-pages/_static/image5.png "uvítací text v návrhovém zobrazení")  
   Chyba je opravit přidáním středníkem `;` na konec řádku. Upozornění jednoduše vás upozorní, že jste nepoužili `myStr` ještě proměnné.  

    > [!NOTE] 
    > 
    > Zobrazení aktuálního kódu formátování nastavení v sadě Visual Studio tak, že vyberete **nástroje**  - &gt; **možnosti**  - &gt; **písma a Barvy**.


## <a name="refactoring-and-renaming"></a>Refaktoring a přejmenování

Refaktoring je metodologie softwaru, která zahrnuje restrukturalizaci váš kód, aby bylo snazší porozumět a chcete zachovat, při zachování její funkčnost. Jednoduchým příkladem může být, že napíšete kód v obslužné rutině události k získání dat z databáze. Při vývoji vaší stránce, zjistíte, že potřebujete přístup k datům z několika různých obslužných rutin. Proto se Refaktorovat kód na stránce tak, že vytvoření metody přístupu k datům stránky a vložením volání metody v obslužných rutinách.

Editor kódu obsahuje nástroje, které můžete provádět různé úlohy refaktoringu. V tomto návodu budete pracovat se dvěma refaktoringu technikami: přejmenováním proměnné a extrahování metod. Další možnosti refaktorování zahrnují zapouzdření polí, zvyšuje se úroveň lokálních proměnných na parametry metod a Správa parametrů metody. K dispozici tyto možnosti refaktorování, závisí na umístění v kódu.

### <a name="refactoring-code"></a>Refaktoring kódu

Je běžným scénářem refaktoringu vytvoření (extract) metodu z kódu, který se nachází uvnitř jiného člena, jako je například metody. To snižuje velikost původního člena a díky opakovaně použitelného kódu byl extrahován.

V této části Průvodce jednoduchého kódu a pak z něj extrahovat metodu. Refaktoring je podporována pro jazyk C#, takže vytvoříte stránky, která používá C# jako programovací jazyk.

### <a name="to-extract-a-method-in-a-c-page"></a>Extrahovat metodu na stránce jazyka C#

1. Přepnout na **návrhu** zobrazení.
2. V **nástrojů**, z **standardní** kartu tak, že přetáhnete [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku na stránku.
3. Dvakrát klikněte **tlačítko** ovládacího prvku k vytvoření obslužné rutiny pro jeho [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) události a poté přidejte následující zvýrazněný kód:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Kód vytvoří **ArrayList** objektu, používá smyčku k načtení hodnoty a pak používá další smyčky k zobrazení obsahu **ArrayList** objektu.
4. Stisknutím klávesy **CTRL + F5** spuštění stránky a pak klikněte na tlačítko **tlačítko** abyste měli jistotu, že se zobrazí následující výstup:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Vraťte se do editoru kódu a pak vyberte následující řádky v obslužné rutině události.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Klikněte pravým tlačítkem na výběr, klikněte na tlačítko **Refaktorovat**a klikněte na tlačítko **extrahovat metodu**. 

    **Extrahovat metodu** zobrazí se dialogové okno.
7. V **nový název metody** zadejte **DisplayArray**a potom klikněte na tlačítko **OK**. 

    Editor kódu vytvoří novou metodu s názvem `DisplayArray`a vloží do nové metody ve volání **klikněte na tlačítko** obslužnou rutinu, ve kterém bylo původně určené smyčky.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Stisknutím klávesy **CTRL + F5** stránce spustit znovu, a klikněte na tlačítko **tlačítko**.

    Na stránce funguje stejně jako předtím. `DisplayArray` Metoda může být nyní volání z libovolného místa ve třídě stránky.

## <a name="renaming-variables"></a>Přejmenováním proměnné

Při práci s proměnných a také objekty, můžete chtít poté, co je již odkazováno v kódu je přejmenovat. Přejmenování proměnných a objektech však může způsobit kód pro přerušení promeškali přejmenování jeden z odkazů. Proto vám pomůže refaktoring přejmenování provést.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Chcete-li použít refaktoring pro přejmenování proměnné


1. V **klikněte na tlačítko** obslužná rutina události, vyhledejte následující řádek:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Klikněte pravým tlačítkem na název proměnné `alist`, zvolte **Refaktorovat**a klikněte na tlačítko **přejmenovat**.

    **Přejmenovat** zobrazí se dialogové okno.
3. V **nový název** zadejte **ArrayList1** a ujistěte se, že **náhled změn odkazu** vybral zaškrtávací políčko. Pak klikněte na tlačítko **OK**.

    **Náhled změn** dialogové okno se zobrazí a zobrazí strom, který obsahuje všechny odkazy na proměnné, kterou chcete přejmenovat.
4. Klikněte na tlačítko **použít** zavřete **náhled změn** dialogové okno.

    Proměnné, které se odkazují na instance, kterou jste vybrali, budou přejmenovány. Mějte na paměti, ale který proměnnou `alist` není přejmenována na následujícím řádku.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Proměnná `alist` v tomto řádku není přejmenovat, protože nepředstavuje stejnou hodnotu jako proměnnou `alist` přejmenovaný. Proměnná `alist` v `DisplayArray` deklarace je lokální proměnná pro tuto metodu. To ukazuje, že pomocí refaktoring přejmenování proměnné se liší od jednoduše provedením akce najít a nahradit v editoru; Refaktoring přejmenování proměnné se znalostí sémantiku proměnné, která funguje s.


## <a name="inserting-snippets"></a>Vkládání fragmentů kódu

Vzhledem k tomu, že existují mnoho úkolů kódování, které vývojáři webové formuláře se často potřeba provést, editor kódu poskytuje knihovnu fragmenty kódu nebo bloky předepsaného kódu. Tyto fragmenty kódu můžete vložit do stránky.

Každý jazyk, který použijete v sadě Visual Studio má mírné rozdíly tak, jak vložit fragmenty kódu. Informace o vkládání fragmentů kódu, naleznete v tématu [fragmenty kódu technologie IntelliSense jazyka Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx). Informace o vkládání fragmentů kódu v jazyce Visual C# najdete v tématu [fragmenty kódu Visual C#](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Další kroky

Tento návod znázorňuje základní funkce editoru kódu sady Visual Studio 2010 pro opravu chyb v kódu, refaktoring kódu, přejmenováním proměnné a vkládání fragmentů kódu do vašeho kódu. Další funkce v editoru můžete vytvořit vývoj aplikací rychlý a snadný. Například můžete chtít:

- Další informace o funkcích technologie IntelliSense, jako je například úprava možnosti technologie IntelliSense, Správa fragmentů kódu a fragmenty kódu online hledání. Další informace najdete v tématu [pomocí technologie IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Zjistěte, jak vytvořit své vlastní fragmenty kódu. Další informace najdete v tématu [vytváření a používání fragmenty kódu technologie IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Další informace o funkcích specifické pro jazyk Visual Basic fragmenty kódu technologie IntelliSense, jako je například přizpůsobení fragmenty kódu a řešení potíží. Další informace najdete v tématu [fragmenty kódu technologie IntelliSense jazyka Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Další informace o jazyce C#-konkrétní funkce technologie IntelliSense, jako je Refaktoring a fragmenty kódu. Další informace najdete v tématu [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
