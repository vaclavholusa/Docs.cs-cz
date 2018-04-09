---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: ASP.NET – webové formuláře v sadě Visual Studio 2013 úpravy kódu | Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Kód úpravy webových formulářů ASP.NET v sadě Visual Studio 2013
====================
Podle [Erik Reitan](https://github.com/Erikre)

V mnoha stránky webového formuláře ASP.NET můžete napsat kód v jazyce Visual Basic, C# nebo jiném jazyce. Editor kódu v sadě Visual Studio můžete psát kód rychle a vyhnout se chybám. Kromě toho nabízí editor způsoby, jak vytvořit opakovaně použitelný kód snížit množství práce, kterou je potřeba udělat.

Tento návod ukazuje různé funkce editoru kódu aplikace Visual Studio.

Během tohoto návodu se dozvíte, jak:

- Opravte chyby kódování vložené.
- Refaktorovat a přejmenovat kód.
- Přejmenujte proměnné a objekty.
- Vložte fragmenty kódu.

## <a name="prerequisites"></a>Požadavky


K dokončení tohoto návodu, budete potřebovat:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [sady Microsoft Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Rozhraní .NET Framework se instaluje automaticky. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 a Microsoft Visual Studio Express 2013 pro Web se často se označuje jako Visual Studio v rámci tohoto kurzu řady.  
    >   
    > Pokud používáte Visual Studio, tento návod předpokládá, že jste vybrali **vývoj webů** kolekce nastavení při prvním spuštění sady Visual Studio. Další informace najdete v tématu [postupy: nastavení prostředí vyberte vývoj webové](https://msdn.microsoft.com/library/ff521558.aspx).

  Úvod do sady Visual Studio a ASP.NET, najdete v části [vytvoření základní stránky webových formulářů ASP.NET 4.5 ve Visual Studiu 2013](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Vytvoření projektu webové aplikace a stránky

<a id="sectionToggle0"></a>

V této části průvodce vytvoříte projekt webové aplikace a do ní přidejte nová stránka.

### <a name="to-create-a-web-application-project"></a>Chcete-li vytvořit projekt webové aplikace

1. Open Microsoft Visual Studio.
2. Na **soubor** nabídce vyberte možnost **nový projekt**.  
    ![Nabídka Soubor](code-editing-in-web-forms-pages/_static/image1.png)

    **Nový projekt** zobrazí se dialogové okno.
3. Vyberte **šablony**  - &gt; **Visual C#**  - &gt; **webové** skupiny šablony na levé straně.
4. Vyberte **webové aplikace ASP.NET** šablony v prostředním sloupci.
5. Název projektu ***BasicWebApp*** a klikněte na **OK** tlačítko.   
![Dialogové okno Nový projekt](code-editing-in-web-forms-pages/_static/image2.png)
6. Potom vyberte **webových formulářů** šablonu a klikněte **OK** tlačítko pro vytvoření projektu.  
![Dialogové okno Nový projekt ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio vytvoří nový projekt, který zahrnuje předkompilované funkce na základě šablony webových formulářů.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Vytvoření nové stránky webového formuláře ASP.NET


Při vytváření nové aplikace webových formulářů pomocí **webové aplikace ASP.NET** šablona projektu sady Visual Studio přidá stránku ASP.NET (stránky webových formulářů) s názvem *Default.aspx*, stejně jako několik dalších souborů a složky. Můžete použít *Default.aspx* stránky jako domovské stránky pro webovou aplikaci. Ale v tomto návodu vytvoříte a pracovat s novou stránku.

### <a name="to-add-a-page-to-the-web-application"></a>Přidání stránky do webové aplikace


1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název webové aplikace (v tomto kurzu, název aplikace je **BasicWebSite**) a potom klikněte na **přidat**  - &gt; **Novou položku**.   
**Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **webové** skupiny šablony na levé straně. Pak vyberte **webového formuláře** ze středu seznamu a pojmenujte ji *FirstWebPage.aspx*.   
    ![Přidat novou položku – dialogové okno](code-editing-in-web-forms-pages/_static/image4.png)
3. Klikněte na tlačítko **přidat** přidání stránky webových formulářů do projektu.  
 Visual Studio vytvoří novou stránku a otevře ji.
4. Dále nastavte tuto novou stránku jako výchozí spuštění stránky. V **Průzkumníku řešení**, klikněte pravým tlačítkem na novou stránku s názvem *FirstWebPage.aspx* a vyberte **nastavit jako úvodní stránku**. Při příštím spuštění této aplikace k otestování naší průběh, se automaticky zobrazí tuto novou stránku v prohlížeči.


## <a name="correcting-inline-coding-errors"></a>Odstranění vložené chyby kódování


Editor kódu v sadě Visual Studio pomáhá zabránit chybám, jak napsat kód, a pokud jste provedli chybu, editoru kódu umožňuje opravte chybu. V této části návodu budete psát řádek kódu ilustrující funkce korekce chyb v editoru.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Chcete-li opravit jednoduché kódování chyby v sadě Visual Studio


1. V **návrhu** klikněte dvakrát na prázdnou stránku vytvořit obslužnou rutinu události pro **zatížení** událostí pro stránku.   
   Obslužné rutiny události používají pouze jako místo napsat kód.
2. Uvnitř obslužné rutiny, zadejte následující řádek obsahující chyby a stiskněte klávesu **ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Po stisknutí klávesy **ENTER**, editoru kódu umístí podtržení zelenou a červený (běžně volání &quot;vlnovkou&quot; řádky) v oblasti kódu, které mají problémy. Zelená podtržení zobrazuje varování. Červené podtržení označuje chybu, která je nutné opravit. 

    Podržte ukazatel myši nad `myStr` zobrazíte popis toho, o upozornění. Navíc podržte ukazatel myši nad červené podtržení zobrazíte chybovou zprávu.

    Následující obrázek znázorňuje kód s podtržení.

    ![Úvodní text v zobrazení návrhu](code-editing-in-web-forms-pages/_static/image5.png "uvítací text v zobrazení návrhu")  
   Chyba musí být vyřešeny přidáním středníkem `;` na konec řádku. Upozornění jednoduše vás upozorní, že jste nepoužili `myStr` ještě proměnné.  

    > [!NOTE] 
    > 
    > Zobrazit aktuální kódu formátování nastavení v sadě Visual Studio výběrem **nástroje**  - &gt; **možnosti**  - &gt; **písem a Barvy**.


## <a name="refactoring-and-renaming"></a>Refaktoring a přejmenování

Refaktoring je softwarová metodologie, která zahrnuje restrukturalizaci kódu aby bylo snazší, abyste pochopili a pokud chcete zachovat, při zachování jeho funkcí. Jednoduchým příkladem může být psaní kódu v obslužné rutiny události se získat data z databáze. Když budete vyvíjet stránku, zjistíte, že potřebujete přístup k datům z několika různých obslužných rutin. Proto Refaktorovat kódu stránky vytvořením metoda přístupu k datům na stránce a vkládání volání do metody do obslužné rutiny.

Editor kódu obsahuje nástroje, které můžete provádět různé úlohy refaktoringu. V tomto návodu budete pracovat se dvěma technikami refaktoringu: Přejmenování proměnné a extrahování metody. Další možnosti refaktoringu zahrnují zapouzdření polí, převod místních proměnných na parametry metody a správě parametry metody. Dostupnost těchto možností refaktoringu závisí na umístění v kódu.

### <a name="refactoring-code"></a>Refaktoring kódu

Běžný scénář refaktoringu je vytvoření (extrakce) z kód, který je uvnitř jiného člena, jako je například metoda metodu. To snižuje velikost původní člena a umožňuje opakovaně použitelný kód extrahované.

V této části Průvodce napsat jednoduchý kód a potom z něj extrahujte metodu. Refaktoring je podporována pro jazyk C#, takže vytvoříte stránky, která používá c jako jeho programovací jazyk.

### <a name="to-extract-a-method-in-a-c-page"></a>Extrahování metody stránce C#

1. Přepnout na **návrhu** zobrazení.
2. V **sada nástrojů**, z **standardní** kartě, přetáhněte ji [tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ovládacího prvku na stránku.
3. Dvakrát klikněte na **tlačítko** řízení vytvořit obslužnou rutinu události pro jeho [klikněte na tlačítko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) události a poté přidejte následující zvýrazněný kód:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Kód vytvoří **ArrayList** používá smyčku do něj hodnoty objektu a potom pomocí další smyčky zobrazí obsah **ArrayList** objektu.
4. Stiskněte klávesu **CTRL + F5** spuštění stránky, a pak klikněte na **tlačítko** a ujistěte se, že se zobrazí následující výstup:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Vrátit do editoru kódu a pak vyberte následující řádky v obslužné rutině.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Klikněte pravým tlačítkem na výběr, klikněte na tlačítko **Refaktorovat**a potom zvolte **extrahovat metodu**. 

    **Extrahovat metodu** zobrazí se dialogové okno.
7. V **nový název metody** zadejte **DisplayArray**a potom klikněte na **OK**. 

    Editor kódu vytvoří novou metodu s názvem `DisplayArray`a vloží volání nové metody v **klikněte na tlačítko** obslužné rutiny, kde byla původně smyčky.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Stiskněte klávesu **CTRL + F5** stránku znovu spustíte, a klikněte na **tlačítko**.

    Stránce funguje stejně jako předtím. `DisplayArray` Metoda může být nyní volání z libovolného místa v třídy stránky.

## <a name="renaming-variables"></a>Přejmenování proměnné

Při práci s proměnné a také objekty, můžete je přejmenovat, až se už neodkazuje v kódu. Přejmenování proměnné a objekty může způsobit kód pro přerušení, pokud zapomenete přejmenovat jeden z odkazů. Proto můžete refaktoring pro provedení přejmenování.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Chcete-li použít refaktoring pro přejmenování proměnné


1. V **klikněte na tlačítko** obslužné rutiny události, vyhledejte následující řádek:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Klikněte pravým tlačítkem na název proměnné `alist`, zvolte **Refaktorovat**a potom zvolte **přejmenovat**.

    **Přejmenovat** zobrazí se dialogové okno.
3. V **nový název** zadejte **ArrayList1** a zajistěte, aby **zobrazení náhledu změn odkaz** vybral zaškrtávací políčko. Pak klikněte na tlačítko **OK**.

    **Zobrazení náhledu změn** se zobrazí dialogové okno a zobrazí stromové struktury, která obsahuje všechny odkazy na proměnné, kterou chcete přejmenovat.
4. Klikněte na tlačítko **použít** zavřete **zobrazení náhledu změn** dialogové okno.

    Přejmenování proměnné, které odkazují na instanci, kterou jste vybrali. Upozorňujeme však, který proměnnou `alist` není přejmenována na následujícím řádku.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Proměnná `alist` v tomto řádku není přejmenovat, protože nepředstavuje stejnou hodnotu jako proměnnou `alist` přejmenovaný. Proměnná `alist` v `DisplayArray` deklarace je místní proměnné pro tuto metodu. To ukazuje, že pomocí refaktoring pro přejmenování proměnné se liší od jednoduchého provedení akce hledání a nahrazování v editoru; refaktoring přejmenuje proměnné s znalostmi sémantika proměnné, která je práce.


## <a name="inserting-snippets"></a>Vkládání fragmentů kódu

Protože nejsou k dispozici mnoho kódování úlohy, které vývojáři webové formuláře se často potřeba udělat, editoru kódu poskytuje knihovnu fragmentů kódu nebo bloků předepsaného kódu. Tyto fragmenty kódu můžete vložit do vaší stránky.

Pro každý jazyk, který používáte v sadě Visual Studio existuje drobné rozdíly ve způsobu, jakým Vložit fragmenty kódu. Informace o vkládání fragmentů najdete v tématu [jazyka Visual Basic IntelliSense – fragmenty kódu](https://msdn.microsoft.com/library/18yz4be4.aspx). Informace o vkládání fragmentů kódu v jazyce Visual C# najdete v tématu [fragmenty kódu Visual C#](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Další kroky

Tento návod znázorňuje základní funkce editoru kódu Visual Studio 2010 pro opravu chyb v kódu, refaktoring kódu, přejmenování proměnné a vkládání fragmentů kódu do vašeho kódu. Vývoj aplikací rychlý a snadný můžete nastavit další funkce v editoru. Například můžete chtít:

- Další informace o funkcích technologie IntelliSense, jako je například úprava možnosti IntelliSense, správě fragmentů kódu a vyhledávání fragmentů kódu online. Další informace najdete v tématu [pomocí IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Naučte se vytvářet vlastní fragmenty kódu. Další informace najdete v tématu [vytváření a používání IntelliSense – fragmenty kódu](https://msdn.microsoft.com/library/ms165392.aspx)
- Další informace o funkcích specifické pro Visual Basic IntelliSense – fragmenty kódu, například přizpůsobení fragmentů kódu a řešení potíží. Další informace najdete v tématu [jazyka Visual Basic IntelliSense – fragmenty kódu](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Další informace o jazyka C# – konkrétní funkce technologie IntelliSense, jako je například refaktoring a fragmenty kódu. Další informace najdete v tématu [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
