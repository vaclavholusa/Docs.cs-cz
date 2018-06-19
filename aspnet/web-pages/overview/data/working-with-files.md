---
uid: web-pages/overview/data/working-with-files
title: Práce se soubory v stránku ASP.NET Web Pages (Razor) | Microsoft Docs
author: tfitzmac
description: Tato kapitola vysvětluje, jak číst, zapisovat, připojení, odstranění a nahrání souborů.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 0f119f8fb4873e55292203f21a2efd8f26793ae4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
ms.locfileid: "28040220"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Práce se soubory v Web Pages (Razor) technologie ASP.NET
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak číst, zapisovat, připojení, odstranění a odeslat soubory webu technologie ASP.NET Web Pages (Razor).
> 
> > [!NOTE]
> > Pokud chcete nahrát bitové kopie a s nimi manipulovat (například překlopit nebo změnit jejich velikost), najdete v části [práce s obrázky v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Získáte informace:** 
> 
> - Postup vytvoření textového souboru a zápisu dat do ní.
> - Postup připojení dat k existující soubor.
> - Jak ke čtení souboru a zobrazení z něj.
> - Postup odstranění souborů z webu.
> - Jak uživatelům nahrát soubor nebo více souborů.
> 
> Jsou to ASP.NET programování v článku nové funkce:
> 
> - `File` Objekt, který poskytuje způsob, jak spravovat soubory.
> - `FileUpload` Pomocné rutiny.
> - `Path` Objekt, který poskytuje metody, které umožňují pracovat s názvy a cesta k souboru.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - Služba WebMatrix 2
>   
> 
> V tomto kurzu taky spolupracuje se službou WebMatrix 3.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Vytvoření textového souboru a zápisu dat do ní

Kromě použití databáze ve vašem webu, může pracovat se soubory. Můžete například použít textových souborů jako jednoduchý způsob, jak ukládat data pro tuto lokalitu. (Někdy označuje jako textový soubor, který se používá k ukládání dat *plochý soubor*.) Textové soubory mohou být v různých formátech, třeba *.txt*, *.xml*, nebo *.csv* (hodnoty oddělené čárkami).

Pokud chcete k ukládání dat do textového souboru, můžete použít `File.WriteAllText` metoda zadat soubor k vytvoření a data, která mají do něj zapisovat. V tomto postupu vytvoříte stránky, která obsahuje jednoduchý formulář s třemi `input` elementy (křestní jméno, příjmení a e-mailovou adresu) a **odeslání** tlačítko. Když uživatel formulář odešle, budete ukládat vstup uživatele do textového souboru.

1. Vytvořte novou složku s názvem *aplikace\_Data*, pokud již neexistuje.
2. V kořenovém adresáři vašeho webu, vytvořte nový soubor s názvem *UserData.cshtml*.
3. Nahradí existující obsah s následujícími službami: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Značka jazyka HTML vytvoří se tři textová pole formuláře. V kódu, můžete použít `IsPost` vlastnosti k určení, zda byla odeslána stránce před zahájením zpracování.

    První úlohou je k získání vstupu uživatele a přiřaďte ho k proměnné. Kód pak zřetězí hodnoty proměnných samostatné do jednoho řetězce oddělených čárkou, který je pak uloženy v jiné proměnné. Všimněte si, že čárkami oddělovače je řetězec obsažené v uvozovkách (","), protože oznámena vkládáte čárkami do dlouhý řetězec, který vytváříte. Na konci data, která jste řetězení společně, přidáte `Environment.NewLine`. Tento postup přidá konec řádku (znak nového řádku). Co vytváříte s všechny tento zřetězení je řetězec, který vypadá takto:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (S konec řádku neviditelná na konci.)

    Potom můžete vytvářet proměnné (`dataFile`) obsahuje umístění a název souboru pro uložení dat v. Nastavení umístění vyžaduje některé speciální zacházení. V seznamu webové servery chybný postupem v kódu odkazovat na absolutní cesty, jako je *C:\Folder\File.txt* souborů na webovém serveru. Pokud se přesune na web, bude nesprávný absolutní cesta. Kromě toho pro hostované lokalitu (na rozdíl od svého počítače) obvykle není i víte, co je správná cesta při psaní kódu.

    Ale někdy (jako je nyní k zápisu do souboru) nutné úplnou cestu. Řešení, je použít `MapPath` metodu `Server` objektu. Úplná cesta vrátí na váš web. Získání cesty pro kořenové složky webu, můžete uživatele `~` – operátor (na represen webu je virtuální kořenové) k `MapPath`. (Můžete také předat název podsložky, jako je *~/App\_dat nebo*, získání cesty pro tuto podsložku.) Potom můžete řetězení Další informace na jakémkoli metoda vrátí k vytvoření úplnou cestu. V tomto příkladu můžete přidat název souboru. (Další informace o tom, jak pracovat s cest k souborům a složkám v [Úvod k technologii ASP.NET Web Pages programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Soubor je uložen v *aplikace\_Data* složky. Tato složka je speciální složky v technologii ASP.NET, která se používá k ukládání datových souborů, jak je popsáno v [Úvod k práci s databází v lokalitách webové stránky ASP.NET](https://go.microsoft.com/fwlink/?LinkId=195209).

    `WriteAllText` Metodu `File` objekt zapíše data do souboru. Tato metoda přebírá dva parametry: název (s cestou) k zápisu do souboru a skutečná data k zápisu. Všimněte si, že název prvního parametru má `@` znak jako předponu. To informuje technologii ASP.NET, že zadáváte literálu typu verbatim řetězec a že znaky, například "/" zvláštním způsobem se nemohou interpretovat. (Další informace najdete v tématu [Úvod do ASP.NET webové programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Aby váš kód pro uložení souborů v *aplikace\_Data* složky, aplikace musí pro čtení a zápis oprávnění pro tuto složku. Ve svém vývojovém počítači nejedná obvykle problém. Při publikování webu na webový server poskytovatele hostingu, může však potřeba explicitně nastavit tato oprávnění. Pokud jste spuštění tohoto kódu na serveru pro poskytovatele hostingu a dojde k chybám, ověřte u poskytovatele hostingu a zjistěte, jak nastavit tato oprávnění.

- Spusťte stránku v prohlížeči. 

    ![](working-with-files/_static/image1.jpg)
- Zadejte hodnoty do polí a pak klikněte na **odeslání**.
- Zavřete prohlížeč.
- Vraťte se na projekt a aktualizujte zobrazení.
- Otevřete *data.txt* souboru. Data, která jste odeslali ve formuláři se v souboru. 

    ![[Obrázek]](working-with-files/_static/image2.jpg)
- Zavřít *data.txt* souboru.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Připojení dat k existující soubor

V předchozím příkladu jste použili `WriteAllText` k vytvoření textového souboru, který má potom pouze jeden prvek dat v něm. Pokud znovu volat metodu a předejte ji stejný název souboru, se úplně přepíše existující soubor. Ale po vytvoření souboru často chcete přidat nová data na konec souboru. Můžete to, že pomocí `AppendAllText` metodu `File` objektu.

1. Na webu, vytvořit kopii *UserData.cshtml* souboru a název kopie *UserDataMultiple.cshtml*.
2. Nahraďte kód blok před otevření `<!DOCTYPE html>` značky s následující blok kódu: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Tento kód obsahuje jednu změnu z předchozího příkladu. Místo použití `WriteAllText`, používá `the AppendAllText` metoda. Tyto metody jsou podobné, s tím rozdílem, že `AppendAllText` data přidá na konec souboru. Stejně jako u `WriteAllText`, `AppendAllText` vytvoří soubor, pokud ještě neexistuje.
3. Spusťte stránku v prohlížeči.
4. Zadejte hodnoty pro pole a pak klikněte na **odeslání**.
5. Přidejte další data a formulář znovu odešlete.
6. Vrátit do projektu, klikněte pravým tlačítkem na složku projekt a pak klikněte na tlačítko **aktualizovat**.
7. Otevřete *data.txt* souboru. Nyní obsahuje nová data, kterou jste právě zadali. 

    ![[Obrázek]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Čtení a zobrazení dat ze souboru

I v případě nepotřebujete k zápisu dat do textového souboru, budete pravděpodobně někdy potřebovat číst data z jednoho. K tomuto účelu můžete znovu použít `File` objektu. Můžete použít `File` objektu určeného ke čtení jednotlivě na každém řádku (oddělené symbolem konce řádků) nebo pro čtení jednotlivé položky bez ohledu na to, jak jste oddělené.

Tento postup ukazuje, jak ke čtení a zobrazení data, která jste vytvořili v předchozím příkladu.

1. V kořenovém adresáři vašeho webu, vytvořte nový soubor s názvem *DisplayData.cshtml*.
2. Nahradí existující obsah s následujícími službami: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Spustí kód čtením souboru, který jste vytvořili v předchozím příkladu do proměnné s názvem `userData`, pomocí volání této metody:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Kód k tomu je uvnitř `if` příkaz. Pokud chcete přečíst soubor, je vhodné použít `File.Exists` metoda nejprve určit, zda je soubor k dispozici. Kód také zkontroluje, zda je soubor prázdný.

    Tělo stránky obsahuje dva `foreach` cyklu, jeden vnořený do druhého. Vnější `foreach` smyčky získá jeden řádek v čase z datového souboru. V takovém případě jsou definovány řádky konce řádků v souboru &#8212; každá položka dat je na samostatném řádku. Vytvoří novou položku vnější smyčky (`<li>` element) uvnitř uspořádaného seznamu (`<ol>` element).

    Vnitřní smyčky rozdělí každý řádek dat na položky (pole) pomocí čárky jako oddělovač. (Založená na předchozí příklad, to znamená, že každý řádek obsahuje tři pole &#8212; křestní jméno, příjmení a e-mailovou adresu, oddělených čárkami.) Vnitřní smyčky také vytvoří `<ul>` položky seznamu a zobrazí jeden seznam pro každé pole v řádku data.

    Kód ukazuje, jak používat dva typy dat, pole a `char` datového typu. Pole není nutná, protože `File.ReadAllLines` metoda vrátí data jako pole. `char` Datový typ je požadovaná, protože `Split` metoda vrátí `array` v které každý element je typu `char`. (Informace o polích najdete v tématu [Úvod do ASP.NET webové programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Spusťte stránku v prohlížeči. Zobrazí se data, která jste zadali pro v předchozích příkladech. 

    ![[Obrázek]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Zobrazení dat ze souboru aplikace Microsoft Excel oddělený čárkami**
> 
> Aplikace Microsoft Excel můžete uložit data obsažená v tabulce jako soubor s položkami oddělenými čárkou (*.csv* souboru). Když to uděláte, soubor je uložen v prostém textu, není ve formátu aplikace Excel. Každý řádek v tabulce jsou oddělené oddělovačem konec řádku v textovém souboru, a každá položka dat je oddělit čárkou. Kód v předchozím příkladu můžete použít ke čtení souboru aplikace Excel s položkami oddělenými čárkou jenom změnou názvu datového souboru ve svém kódu.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Odstraňování souborů

Chcete-li odstranit soubory z vašeho webu, můžete použít `File.Delete` metoda. Tento postup ukazuje, jak umožnit uživatelům odstranit bitovou kopii (*.jpg* souboru) od *bitové kopie* složku, v případě, že zná název souboru.

> [!NOTE] 
> 
> **Důležité** v produkční web, obvykle omezíte kdo má povoleno provádět změny v datech. Informace o tom, jak nastavit členství a o způsobech autorizaci uživatelů k provádění úloh v lokalitě najdete v tématu [členství na web webových stránek ASP.NET a přidání zabezpečení](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Na webu, vytvořte podsložku s názvem *bitové kopie*.
2. Kopírování jeden nebo více *.jpg* souborů do *bitové kopie* složky.
3. V kořenovém adresáři webu, vytvořte nový soubor s názvem *FileDelete.cshtml*.
4. Nahradí existující obsah s následujícími službami: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Tato stránka obsahuje formulář, kde mohou uživatelé zadat název souboru bitové kopie. Jejich nezadávejte *.jpg* příponu názvu souboru; tak, že omezíte název souboru, jako to, budete nápovědy zabraňuje uživatelům v odstranění libovolné soubory na svém webu.

    Kód načte název souboru, které uživatel zadá a potom vytvoří kompletní cesta. Pokud chcete vytvořit cestu, kód používá aktuální cestě k webu (jak ho vrátila `Server.MapPath` metoda), *bitové kopie* název složky, název, který má zadaný uživatel a ".jpg" jako řetězcový literál.

    K odstranění tohoto souboru, volání kódu `File.Delete` metoda, předání úplnou cestu, která právě vytvořený. Na konci značky kód zobrazí zprávu s potvrzením, že soubor byl odstraněn.
5. Spusťte stránku v prohlížeči. 

    ![[Obrázek]](working-with-files/_static/image5.jpg)
6. Zadejte název souboru, který se odstranit a pak klikněte na **odeslání**. Pokud soubor byl odstraněn, název souboru se zobrazí v dolní části stránky.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Povolení uživatelům nahrávání souboru

`FileUpload` Pomocník umožňuje uživatelům odeslat soubory na váš web. Následující postup ukazuje, jak umožnit uživatelům nahrát jeden soubor.

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste ji nepřidali dříve.
2. V *aplikace\_Data* složky, vytvořte novou složku a pojmenujte ji *UploadedFiles*.
3. V kořenovém adresáři vytvořte nový soubor s názvem *FileUpload.cshtml*.
4. Nahradí existující obsah na stránce s následujícími službami: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    Část textu stránky používá `FileUpload` pomocné rutiny pro vytvoření nahrávání pole a tlačítka, který jste pravděpodobně obeznámeni s:

    ![[Obrázek]](working-with-files/_static/image6.jpg)

    Vlastnosti, které jste vytvořili `FileUpload` pomocníka zadejte má jediné pole pro soubor k odeslání a že chcete tlačítko Odeslat ke čtení **nahrát**. (Další polí přidáte později v článku.)

    Když uživatel klikne na **nahrát**, kód v horní části stránky získá souboru a jeho uložení. `Request` Také obsahuje objekt, který běžně slouží k získání hodnoty z pole formuláře `Files` pole, která obsahuje soubor (nebo soubory), byly odeslány. Můžete získat jednotlivé soubory z konkrétní pozici v poli &#8212; například získat první nahrávaný soubor, získat `Request.Files[0]`, abyste získali druhý soubor, získáte `Request.Files[1]`a tak dále. (Nezapomeňte, že při programování, počítání obvykle začíná od nuly.)

    Při načítání nahrávaný soubor jste ji vložili do proměnné (zde `uploadedFile`) tak, že je můžete používat. K určení názvu uloženého souboru, právě získáte jeho `FileName` vlastnost. Ale když uživatel odešle soubor, `FileName` obsahuje název původního uživatele, který zahrnuje celou cestu. Ho může vypadat například takto:

    *C:\Users\Public\Sample.txt*

    Nechcete tyto cesty informace, ale protože se jedná o cestu v počítači uživatele, ne pro váš server. Chcete se jejich název (*ukázka.txt*). Můžete pruhu se právě souboru z cesty pomocí `Path.GetFileName` metoda takto:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    `Path` Objektu je nástroj, který má několik metod, jako to, který můžete použít k pruhu cesty, kombinovat cesty a tak dále.

    Jakmile jste, že jste podmínky název nahrávaný soubor, můžete vytvořit novou cestu, pro které chcete uložit nahrávaný soubor ve vašem webu. V takovém případě můžete kombinovat `Server.MapPath`, názvy složek (*aplikace\_dat nebo UploadedFiles*) a název souboru nově odřapíkovaného vytvořit novou cestu. Potom můžete volat nahrávaný soubor `SaveAs` metoda ve skutečnosti se soubor uložit.
5. Spusťte stránku v prohlížeči. 

    ![[Obrázek]](working-with-files/_static/image7.jpg)
6. Klikněte na tlačítko **Procházet** a potom vyberte soubor k odeslání. 

    ![[Obrázek]](working-with-files/_static/image8.jpg)

    Do textového pole **Procházet** tlačítko bude obsahovat umístění a cesta k souboru.

    ![[Obrázek]](working-with-files/_static/image9.jpg)
7. Klikněte na tlačítko **nahrát**.
8. Na webu, klikněte pravým tlačítkem na složku projekt a pak klikněte na **aktualizovat**.
9. Otevřete *UploadedFiles* složky. Soubor, který jste nahráli je ve složce. 

    ![[Obrázek]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Když necháte uživatelé odesílat více soubory

V předchozím příkladu umožníte uživatelům odeslat jeden soubor. Ale můžete použít `FileUpload` pomocná nahrát více než jeden soubor současně. To je užitečné pro scénáře, jako je odesílání fotografie, kde je zdlouhavé odesílání jeden soubor současně. (Další informace o nahrávání fotografie v [práce s obrázky v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).) Tento příklad ukazuje, jak uživatelům nahrát dva současně, i když můžete použít stejný postup k nahrání více než.

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste tak ještě neučinili.
2. Vytvořit novou stránku s názvem *FileUploadMultiple.cshtml*.
3. Nahradí existující obsah na stránce s následujícími službami:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    V tomto příkladu `FileUpload` pomocné rutiny v těle stránky je ve výchozím nastavení nakonfigurované tak, aby uživatelé nahrát dva soubory. Protože `allowMoreFilesToBeAdded` je nastaven na `true`, pomocné rutiny vykreslí odkaz, který umožňuje uživateli přidat další nahrávání polí:

    ![[Obrázek]](working-with-files/_static/image11.jpg)

    Zpracovat soubory, které uživatel odešle, kód používá stejný základní postup, který jste použili v předchozím příkladu &#8212; získat soubor z `Request.Files` a pak ho uložte. (Včetně různých věcí musíte udělat správný název souboru a cesty.) Inovace této doby je, že uživatel může být odesílání několik souborů a neznáte mnoho. Pokud chcete zjistit, můžete získat `Request.Files.Count`.

    S tímto číslem v dolním, které můžete projít `Request.Files`, zase načíst každý soubor a uložte ho. Pokud chcete cykly pouze známé počet opakování v kolekci, můžete použít `for` smyčky, například takto:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    Proměnná `i` je pouze dočasné čítač, který přejde od nuly do jakoukoli horní limit nastavíte. V takovém případě horní limit je počet souborů. Ale protože čítač začíná od nuly, jako je typický pro počítání scénáře technologie ASP.NET, horní limit je ve skutečnosti jeden menší než počet souborů. (Pokud jsou tři soubory, počet je nula. 2.)

    `uploadedCount` Proměnná celkový počet zpracovaných položek všechny soubory, které se úspěšně nahrála a uložit. Tento kód účty pro případ, že soubor očekávané nemusí mít k odeslání.
4. Spusťte stránku v prohlížeči. Prohlížeč zobrazí na stránce a jeho dvě nahrávání polí.
5. Vyberte dva soubory nahrát.
6. Klikněte na tlačítko **přidejte další soubor**. Na stránce zobrazuje nové pole nahrávání. 

    ![[Obrázek]](working-with-files/_static/image12.jpg)
7. Klikněte na tlačítko **nahrát**.
8. Na webu, klikněte pravým tlačítkem na složku projekt a pak klikněte na **aktualizovat**.
9. Otevřete *UploadedFiles* se úspěšně nahrál soubory.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


[Práce s obrázky v stránky webu technologie ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202897)

[Export do souboru CSV](https://msdn.microsoft.com/library/ms155919.aspx)
