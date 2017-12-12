---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: "Práce s obrázky v stránku ASP.NET Web Pages (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tato kapitola ukazuje, jak přidat, zobrazit a pracovat s obrázky (Změna velikosti, převrácení a přidejte vodoznaky) ve vašem webu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 74838dd364a43f3f4c966c1417d0f0b2cc242f28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Práce s obrázky v Web Pages (Razor) technologie ASP.NET
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek ukazuje, jak přidat, zobrazit a pracovat s obrázky (Změna velikosti, převrácení a přidejte vodoznaky) na webu technologie ASP.NET Web Pages (Razor).
> 
> Získáte informace:
> 
> - Jak přidat bitovou kopii na stránku dynamicky.
> - Jak uživatelům odeslat bitovou kopii.
> - Jak změnit velikost obrázku.
> - Jak překlopit nebo otočit bitovou kopii.
> - Postup přidání vodoznak pro bitovou kopii.
> - Jak image použít jako vodoznak.
> 
> Jsou to ASP.NET programování v článku nové funkce:
> 
> - `WebImage` Pomocné rutiny.
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


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Přidání obrázku dynamicky na webovou stránku

Bitové kopie můžete přidat na váš web a na jednotlivé stránky, když vyvíjíte web. Můžete taky umožnit uživatelům nahrajte Image, které mohou být užitečné pro úlohy, jako když necháte je přidat profilové fotky.

Pokud bitová kopie je již k dispozici na webu a chcete zobrazit na stránce, můžete použít HTML `<img>` element takto:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

V některých případech ale musíte být schopni zobrazit obrázky dynamicky &#8212; To znamená, že nevíte, jaké bitovou kopii k zobrazení až stránce běží.

Postup v této části ukazuje, jak zobrazit bitovou kopii za chodu, kde uživatelé zadat název souboru bitové kopie ze seznamu názvů bitové kopie. Výběrem název bitovou kopii z rozevíracího seznamu a při odeslání stránky, se zobrazí obrázek, který se vybrali.

![[Obrázek] ] (9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. Ve službě WebMatrix vytvoření nového webu.
2. Přidat novou stránku s názvem *DynamicImage.cshtml*.
3. V kořenové složce webové stránky, přidejte novou složku a pojmenujte ji *bitové kopie*.
4. Přidat čtyři image *bitové kopie* složku, kterou jste právě vytvořili. (Všechny Image, máte užitečný bude provést, ale měli vešel na stránku.) Přejmenujte bitové kopie *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, a *Photo4.jpg*. (Nebudete používat *Photo4.jpg* v tomto postupu, ale budete používat ji později v článku.)
5. Ověřte, že čtyři obrázky nejsou označeny jako jen pro čtení.
6. Nahradí existující obsah na stránce s následujícími službami:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Tělo stránky má rozevíracího seznamu ( `<select>` element) s názvem `photoChoice`. V seznamu má tři možnosti a `value` atribut jednotlivých možností seznamu má název jednoho z bitové kopie, které jste zadali v *bitové kopie* složky. V podstatě seznamu umožňuje uživateli vybrat popisný název jako &quot;fotografií 1&quot;, a poté předá *.jpg* název souboru při odeslání stránky.

    V kódu můžete získat výběr uživatele (jinými slovy, název souboru obrázku) ze seznamu načtením `Request["photoChoice"]`. Nejprve zobrazí, pokud existuje jiný výběr vůbec. Pokud existuje, můžete vytvořit cestu pro bitovou kopii, která se skládá z názvu složky pro obrázky a název souboru obrázku uživatele. (Pokud jste se pokusili vytvořit cestu, ale došlo ustanovení v `Request["photoChoice"]`, by dojde k chybě.) Výsledkem je relativní cesta takto:

    *bitové kopie nebo Photo1.jpg*

    Cesta je uložena v proměnné s názvem `imagePath` , kterou budete potřebovat později na stránce.

    V těle, je také `<img>` element, který se používá k zobrazení bitovou kopii, která zachyceny uživatele. `src` Atribut není nastaven na název souboru nebo adresa URL, jako by se zobrazit statický prvek. Místo toho je nastaven na hodnotu `@imagePath`, což znamená, že získá svou hodnotu z cesty k nastavení v kódu.

    Při prvním spuštění stránky, ale neexistuje žádný obrázek, který chcete zobrazit, protože uživatel není vybrána nic. To by za normálních okolností znamenají, že `src` atribut by být prázdný a zobrazí se bitovou kopii jako červený &quot;x&quot; (nebo ať prohlížeče vykreslí, když nemůže najít bitovou kopii). Chcete-li tomu zabránit, můžete zadat `<img>` element v `if` blok, který testuje, zda `imagePath` proměnná nic se nachází. Pokud se uživatel s výběrem `imagePath` obsahuje cestu. Pokud uživatel nebyl vyberte bitovou kopii nebo pokud je to první stránka se zobrazí, `<img>` není i vykresluje element.
7. Uložte soubor a spusťte stránku v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.)
8. Vyberte bitovou kopii z rozevíracího seznamu a pak klikněte na tlačítko **Ukázkový obrázek**. Ujistěte se, najdete v části různých bitových kopií pro různé možnosti.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Nahrávání obrázku

Předchozí příklad ukázal, jak zobrazit dynamicky bitovou kopii, ale to šlo jenom s obrázky, které již byly na vašem webu. Tento postup ukazuje, jak umožnit uživatelům odeslat bitovou kopii, která se následně zobrazí na stránce. V technologii ASP.NET, můžete upravit obrázků pomocí `WebImage` pomocné rutiny, která má metody, která umožňují vytvořit, upravit a uložit bitové kopie. `WebImage` Pomocná podporuje všechny běžné webové image typy souborů, včetně *.jpg*, *.png*, a *.bmp*. V tomto článku budete používat *.jpg* bitové kopie, ale můžete použít jakýkoli z typů bitové kopie.

![[Obrázek] ] (9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Přidat novou stránku a pojmenujte ji *UploadImage.cshtml*.
2. Nahradí existující obsah na stránce s následujícími službami: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Tělo text má `<input type="file">` element, který umožňuje uživateli vybrat soubor k odeslání. Kliknutím na **odeslání**, odeslání souboru je vydat společně s formuláři.

    Nahraný obrázek získáte pomocí `WebImage` pomocné rutiny, která má nejrůznějším užitečné metody pro práci s obrázky. Konkrétně používat `WebImage.GetImageFromRequest` získat nahraný obrázek (pokud existuje) a uložte ho do proměnné s názvem `photo`.

    Hodně práce v tomto příkladu zahrnuje získání a nastavení názvy souborů a cestu. Problém je, že chcete získat název (a pouze název) bitovou kopii, která uživatel nahrál a poté vytvořit novou cestu, pro které se chystáte uložte bitovou kopii. Protože uživatelé může potenciálně nahrát více bitových kopií, které mají stejný název, použijete k vytvoření jedinečné názvy a ujistěte se, že uživatelé nejsou přepíše existující obrázky bit navíc kódu.

    Pokud image skutečně odeslal (test `if (photo != null)`), můžete získat název bitové kopie z obrázku `FileName` vlastnost. Když uživatel odešle obrázek, `FileName` obsahuje původní název uživatele, který obsahuje cestu z počítače uživatele. Ho může vypadat například takto:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Všechny tyto informace cestu, ale nechcete &#8212; Chcete se jejich název (*SamplePhoto1.jpg*). Můžete pruhu se právě souboru z cesty pomocí `Path.GetFileName` metoda takto:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Pak vytvořte nový název jedinečný soubor přidáním identifikátor GUID na původní název. (Další informace o identifikátory GUID najdete v tématu [o identifikátory GUID](#SB_AboutGUIDs) dále v tomto článku.) Pak vytvoříte úplnou cestu, která můžete použít pro uložení obrázku. Uložení cesta se skládá z nový název souboru, složce (imagí) a aktuální umístění webu.

    > [!NOTE]
    > Aby váš kód pro uložení souborů v *bitové kopie* složky, aplikace musí pro čtení a zápis oprávnění pro tuto složku. Ve svém vývojovém počítači nejedná obvykle problém. Při publikování webu na webový server poskytovatele hostingu, může však potřeba explicitně nastavit tato oprávnění. Pokud jste spuštění tohoto kódu na serveru pro poskytovatele hostingu a dojde k chybám, ověřte u poskytovatele hostingu a zjistěte, jak nastavit tato oprávnění.

    Nakonec předáte Uložit cestu k `Save` metodu `WebImage` pomocné rutiny. Slouží k uložení nahraný obrázek pod jejím novým názvem. Uložení metoda vypadá takto: `photo.Save(@"~\" + imagePath)`. Úplná cesta je připojeno k `@"~\"`, což je aktuální umístění webu. (Informace o `~` operátor, najdete v části [Úvod do ASP.NET webové programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Jako v předchozím příkladu obsahuje text stránky `<img>` elementu, který chcete zobrazit obrázek. Pokud `imagePath` byla nastavena, `<img>` prvek je vykreslovaný a jeho `src` je nastavena na hodnotu `imagePath` hodnotu.
3. Spusťte stránku v prohlížeči.
4. Odeslat bitovou kopii a ujistěte se, že se zobrazí na stránce.
5. Ve vaší lokalitě, otevřete *bitové kopie* složky. Uvidíte, že nový soubor byl přidán jejichž název souboru vypadá přibližně takto:: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Toto je obrázek, který jste nahráli s identifikátorem GUID předponu názvu. (Svůj vlastní soubor bude mít jiný identifikátor GUID a pravděpodobně pojmenovaný něco jiného než *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>O identifikátory GUID
> 
> Identifikátor GUID (globálně jedinečné ID) je identifikátor, který je obvykle vykreslen v tento formát: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Číslice a písmena (z A-F) se liší pro každý identifikátor GUID, ale všechny se řídí vzor používání skupin 8-4-4-4-12 znaků. (Identifikátor GUID je technicky počet bajtů / 12816bitové.) Pokud budete potřebovat identifikátor GUID, můžete volat specializované kód, který vytvoří identifikátor GUID pro vás. Cílem identifikátory GUID je, že mezi značné velikost číslo (3,4 × 10<sup>38</sup>) a algoritmus pro generování ho výsledné číslo prakticky záruku, že se jeden typ. Identifikátory GUID proto jsou vhodný způsob, jak generovat názvy pro věcí, když musí zaručit, že nebudete používat stejný název dvakrát. Nevýhodou, je samozřejmě, identifikátory GUID nejsou zvlášť uživatelsky přívětivý, takže se zpravidla používá, pokud název se používá pouze v kódu.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Změna velikosti obrázku

Pokud váš web přijímá Image od uživatele, můžete změnit velikost bitové kopie, před zobrazení nebo je uložíte. Můžete znovu použít `WebImage` Pomocník pro to.

Tento postup ukazuje, jak změnit velikost image nahrané vytvořit miniaturu a poté uložte miniaturu a původní bitové kopie na webu. Můžete zobrazit na stránce miniaturu a používat hypertextový odkaz přesměrovat uživatele na bitovou kopii plné velikosti.

![[Obrázek] ] (9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Přidat novou stránku s názvem *Thumbnail.cshtml*.
2. V *bitové kopie* složce vytvořit podsložku s názvem *palec*.
3. Nahradí existující obsah na stránce s následujícími službami: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Tento kód je podobný kódu z předchozího příkladu. Rozdílem je, že tento kód uloží obrázek dvakrát, jednou za normálních okolností a jednou po vytvoření miniatur kopírování bitové kopie. Nejdřív získat nahrané bitové kopie a uložit ho v *bitové kopie* složky. Pak vytvoříte novou cestu pro miniaturu. Chcete-li vytvořit ve skutečnosti miniaturu, zavolejte `WebImage` pomocné rutiny pro `Resize` metodu pro vytvoření bitové kopie 60-podle 60 pixelů. Příklad ukazuje, jak zachová poměr stran a jak můžete zabránit bitovou kopii se zvětšení (v případě, že nová velikost by skutečně byl obraz větší). Obrázek se změněnou velikostí pak je uložena v *palec* podsložky.

    Na konci značky, můžete použít stejné `<img>` element s dynamická `src` atribut, který jste viděli v předchozích příkladech podmíněně zobrazíte bitovou kopii. V takovém případě je zobrazit miniaturu. Můžete také použít `<a>` elementu, který chcete vytvořit hypertextový odkaz na velkých verze bitové kopie. Stejně jako u `src` atribut `<img>` , je nastavena `href` atribut `<a>` element dynamicky bez ohledu `imagePath`. Abyste měli jistotu, že cesta může fungovat jako adresu URL, předáte `imagePath` k `Html.AttributeEncode` metody, která převádí vyhrazené znaky v cestě k znaky, které jsou v pořádku v adrese URL.
4. Spusťte stránku v prohlížeči.
5. Odeslat fotografii a ověřte, zda je zobrazen miniaturu.
6. Klikněte na miniaturu zobrazíte obrázek v plné velikosti.
7. V *bitové kopie* a *bitové kopie nebo palec*, Všimněte si, že byly přidány nové soubory.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Otáčení a překlopení obrázku

`WebImage` Pomocník umožňuje také překlopit a otáčení obrázků. Tento postup ukazuje, jak získat bitovou kopii ze serveru, překlopit bitovou kopii obráceně (svisle), uložit jej a následně na stránce se zobrazí přetočený obrázek. V tomto příkladu právě používáte soubor už máte na serveru (*Photo2.jpg*). V reálné aplikaci byste pravděpodobně překlopit image s názvem získáte dynamicky, stejně jako v předchozích příkladech.

![[Obrázek] ] (9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Přidat novou stránku s názvem *FlipImage.cshtml*.
2. Nahradí existující obsah na stránce s následujícími službami: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Kód používá `WebImage` pomocná rutina pro získání bitovou kopii ze serveru. Vytvoření cesty na bitovou kopii pomocí stejného postupu, který jste použili v předchozích příkladech pro ukládání bitových kopií, a cestu předat při vytváření k pomocí bitové kopie `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Pokud je nalezen bitovou kopii, vytvoříte novou cestu a název, stejně jako v předchozích příkladech. Chcete-li překlopit bitovou kopii, zavolejte `FlipVertical` metoda a potom uložte bitovou kopii znovu.

    Obrázek se znovu zobrazí na stránce pomocí `<img>` element s `src` atribut nastaven na `imagePath`.
3. Spusťte stránku v prohlížeči. Obrázek pro *Photo2.jpg* se zobrazí obráceně.
4. Aktualizujte stránku nebo žádosti o stránku znovu a ověřte, že bitová kopie je přetočený pravé straně se znovu.

Otočit bitovou kopii, můžete použít stejný kód, vyjma toho, že místo volání `FlipVertical` nebo `FlipHorizontal`, zavoláte `RotateLeft` nebo `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Přidání vodoznak do Image

Když přidáváte bitových kopií na váš web, můžete chtít přidat vodoznak na bitovou kopii, než ji uložit nebo zobrazit na stránce. Lidé často používají vodoznaky chcete přidat informace o autorských právech pro bitovou kopii nebo inzerovat jejich název firmy.

![[Obrázek] ] (9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Přidat novou stránku s názvem *Watermark.cshtml*.
2. Nahradí existující obsah na stránce s následujícími službami: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Tento kód je jako kód *FlipImage.cshtml* stránky z dříve (i když se v tuto chvíli se používá *Photo3.jpg* souboru). Chcete-li přidat vodoznak, zavolejte `WebImage` pomocné rutiny pro `AddTextWatermark` metoda před uložením bitovou kopii. Ve volání `AddTextWatermark`, předáte text &quot;Moje vodoznak&quot;, nastavit barvu písma na žlutou a rodiny písem na Arial. (I když není zde zobrazený `WebImage` Pomocník také umožňuje určit krytí, rodinu písem a velikost písma a pozice vodoznakového textu.) Při ukládání obrázku nesmí být jen pro čtení.

    Protože jste viděli, bitovou kopii se zobrazí na stránce pomocí `<img>` element s atributem src nastaven na hodnotu `@imagePath`.
3. Spusťte stránku v prohlížeči. Všimněte si text "Moje vodoznak" v horním pravém dolním rohu bitovou kopii.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Pomocí bitovou kopii jako vodoznak

Místo použití text pro vodoznak, můžete použít jiné image. Bitové kopie, jako je logo společnosti osoby někdy použít jako vodoznak, nebo použít vodoznak místo textu pro informace o autorských právech.

![[Obrázek] ] (9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Přidat novou stránku s názvem *ImageWatermark.cshtml*.
2. Přidejte bitovou kopii *bitové kopie* složky, můžete použít jako logo a přejmenovat *MyCompanyLogo.jpg*. Tato bitová kopie by měl být obrázek, který se zobrazí jasně když je nastavená na 80 pixelů a 20 pixelů.
3. Nahradí existující obsah na stránce s následujícími službami: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    To je další variantou kód v předchozích příkladech. V takovém případě volání `AddImageWatermark` do cílové image přidat vodoznaku (*Photo3.jpg*) před uložením bitovou kopii. Při volání `AddImageWatermark`, nastavte jeho šířku na 80 pixelů a výšky na 20 pixelů. *MyCompanyLogo.jpg* image je vodorovného zarovnání v centru a svisle zarovnán v dolní části bitovou kopii cíl. Krytí nastavena na 100 % a odsazení je nastaveno na 10 pixelů. Pokud vodoznaku je větší, než cílový image, není provedena žádná akce. Pokud vodoznaku je větší, než cílový bitové kopie a nastavení odsazení vodoznak bitové kopie na nulu, vodoznak se ignoruje.

    Jak před, můžete zobrazit pomocí bitové kopie `<img>` elementu a dynamický `src` atribut.
4. Spusťte stránku v prohlížeči. Všimněte si, že vodoznaku se zobrazí v dolní části hlavní bitové kopie.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


[Práce se soubory v stránky webu technologie ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202896)

[Úvod do programování pomocí syntaxe Razor rozhraní ASP.NET Web Pages.](https://go.microsoft.com/fwlink/?LinkID=251587)
