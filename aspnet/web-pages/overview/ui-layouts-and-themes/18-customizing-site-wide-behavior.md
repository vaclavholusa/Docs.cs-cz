---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: "Přizpůsobení chování na webu pro ASP.NET Web Pages lokalit (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tato kapitola vysvětluje, jak nakonfigurovat nastavení pro celý web nebo celou složku, nikoli jen na stránce."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: b1caa26a23517bd976addfefac89375ae965eb91
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Přizpůsobení chování na webu pro ASP.NET – webové stránky (Razor) servery
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak nakonfigurovat nastavení na straně serveru pro stránky na webu technologie ASP.NET Web Pages (Razor).
> 
> Získáte informace:
> 
> - Jak spustit kód který vám umožní sadu hodnot (globální hodnot nebo nastavení pomocníka) pro všechny stránky v lokalitě.
> - Jak spustit kód, který umožňuje nastavit hodnoty pro všechny stránky ve složce.
> - Postup spuštění kódu před a po každém načte.
> - Postup odeslání chyb při centrální chybovou stránku.
> - Postup přidání ověřování pro všechny stránky ve složce.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - Služba WebMatrix 3
> - Knihovnu ASP.NET Web Helpers (balíček NuGet)
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 3 a Visual Studio 2013 (nebo Visual Studio Express 2013 pro Web), s výjimkou je nemohou používat knihovnu ASP.NET Web Helpers.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Přidání kódu spuštění webové stránky pro webové stránky ASP.NET

Pro většinu kód, který zapisuje na webových stránkách ASP.NET jednotlivé stránky může obsahovat všechny kód, který je požadovaný pro tuto stránku. Například pokud stránka odešle e-mailovou zprávu, je možné uvést všechny kód pro tuto operaci na jedné stránce. To může zahrnovat kód pro inicializaci nastavení pro odesílání e-mailu (který je pro SMTP server) a odesílání e-mailové zprávy.

Ale v některých případech můžete chtít spouštět nějaký kód před spuštěním libovolné stránky na webu. To je užitečné pro nastavení hodnot, které lze použít kdekoli v síti (označované jako *globální hodnoty*.) Například některé pomocné rutiny vyžadují, abyste zadejte hodnoty jako nastavení e-mailu nebo klíče účtu. Může být užitečný zachovat tato nastavení v globální hodnoty.

To provedete tak, že vytvoříte stránku s názvem  *\_AppStart.cshtml* v kořenovém adresáři serveru. Pokud tuto stránku existuje, spustí se při prvním požadavku na libovolnou stránku v lokalitě. Proto je vhodná k spuštění kódu nastavit globální hodnoty. (Protože  *\_AppStart.cshtml* má předponu podtržítko, ASP.NET nepošle stránku v prohlížeči i, pokud uživatelé požadují ji přímo.)

Následující obrázek ukazuje, jak  *\_AppStart.cshtml* stránky funguje. Když přijde žádost pro stránku, a pokud se jedná o první požadavek pro všechny stránky v lokalitě, ASP.NET první kontroly jestli  *\_AppStart.cshtml* stránka existuje. Pokud ano, některý kód na  *\_AppStart.cshtml* stránka běží, a poté spustí k požadované stránce.

![[Obrázek]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Globální hodnoty nastavení pro svůj web

1. V kořenové složce webu služby WebMatrix, vytvořte soubor s názvem  *\_AppStart.cshtml*. Soubor musí být v kořenovém adresáři serveru.
2. Nahradí existující obsah s následujícími službami: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Tento kód ukládá hodnotu v `AppState` slovník, který je automaticky dostupný pro všechny stránky v lokalitě. Všimněte si, že  *\_AppStart.cshtml* soubor nemá žádné značky v ní. Spustí kód a pak přesměruje na stránku, která původně požádal stránky.

    > [!NOTE]
    > Buďte opatrní při chápat kódu  *\_AppStart.cshtml* souboru. Pokud dojde k chybám v kódu v  *\_AppStart.cshtml* souboru na webu se nespustí.
3. V kořenové složce vytvořte novou stránku s názvem *AppName.cshtml*.
4. Výchozí značek a kódu nahraďte následujícím textem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Tento kód extrahuje hodnotu z `AppState` objekt, který nastavíte v  *\_AppStart.cshtml* stránky.
5. Spustit *AppName.cshtml* stránku v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.) Na stránce zobrazuje globální hodnota. 

    ![[Obrázek]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Nastavení hodnot pro pomocné rutiny

Vhodné využít pro  *\_AppStart.cshtml* soubor je nastavit hodnoty pro pomocné rutiny, které používáte ve vaší lokalitě a které je nutné inicializovat. Typickým příkladem jsou nastavení e-mailu pro `WebMail` pomocné rutiny a privátní a veřejné klíče pro `ReCaptcha` pomocné rutiny. V takových případech můžete nastavit hodnoty jednou  *\_AppStart.cshtml* a pak je již nastavena pro všechny stránky ve vaší lokalitě.

Tento postup ukazuje, jak nastavit `WebMail` nastavení globálně. (Další informace o používání `WebMail` pomocné rutiny, najdete v části [přidání e-mailu na web ASP.NET Web Pages](../getting-started/11-adding-email-to-your-web-site.md).)

1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste to ještě už přidali.
2. Pokud ještě nemáte  *\_AppStart.cshtml* souboru, v kořenové složce webu, vytvořte soubor s názvem  *\_AppStart.cshtml*.
3. Přidejte následující `WebMail` nastavení  *\_AppStart.cshtml* souboru: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Upravit následující související nastavení v kódu e-mailu:

    - Nastavit `your-SMTP-host` na název serveru SMTP, který máte přístup.
    - Nastavit `your-user-name-here` na uživatelské jméno pro svůj účet serveru SMTP.
    - Nastavit `your-account-password` na heslo pro svůj účet serveru SMTP.
    - Nastavit `your-email-address-here` vlastní e-mailovou adresu. Toto je e-mailovou adresu, které je zpráva odeslána z. (Někteří poskytovatelé e-mailu Nenechte si můžete nastavit jinou `From` adres a bude používat vaše uživatelské jméno jako `From` adresu.)

    Další informace o nastavení SMTP, najdete v části [konfigurace nastavení e-mailu](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) v článku [odesílání e-mailu z webových stránek ASP.NET (Razor) stránky](https://go.microsoft.com/fwlink/?LinkID=202899) a [problémy s odesílání e-mailu](https://go.microsoft.com/fwlink/?LinkId=253001#email)v [ASP.NET Web Pages Průvodce řešením potíží (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).
- Uložit  *\_AppStart.cshtml* souboru a zavřete ho.
- V kořenové složce webu, vytvořte novou stránku s názvem *TestEmail.cshtml*.
- Nahradí existující obsah s následujícími službami: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
- Spustit *TestEmail.cshtml* stránku v prohlížeči.
- Vyplňte pole odeslat sami e-mailovou zprávu a pak klikněte na **odeslat**.
- Zkontrolujte e-mailu a ujistěte se, že jste, že jste podmínky zprávy.

Důležitou součástí v tomto příkladu je, že nastavení, která obvykle neměnit – jako název serveru SMTP a přihlašovacích údajů e-mailu – jsou nastavené  *\_AppStart.cshtml* souboru. Tímto způsobem, nemusíte je znovu nastavit v každé stránce, kde odeslání e-mailu. (I když Pokud z nějakého důvodu potřebujete změnit tato nastavení, můžete nastavit je samostatně na stránce.) Na stránce můžete nastavit pouze hodnoty, které obvykle změnit pokaždé, když, jako je příjemce a tělo e-mailové zprávy.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Spuštěním kódu před a po souborů ve složce

Stejně jako můžete použít  *\_AppStart.cshtml* napsat kód, před spuštěním stránky v lokalitě, můžete napsat kód, který běží před (a po) všechny stránky v určité složce spustit. To je užitečné pro takové věci, jako nastavení stejné stránce rozložení pro všechny stránky ve složce, nebo pro kontrolu, uživatel je přihlášen před spuštěním na stránce ve složce.

Pro stránky na konkrétní složky, můžete vytvořit kód v souboru s názvem  *\_PageStart.cshtml*. Následující obrázek ukazuje, jak  *\_PageStart.cshtml* stránky funguje. Když přijde žádost pro stránku, ASP.NET nejdřív zkontroluje  *\_AppStart.cshtml* stránky a který je spuštěn. Pak ASP.NET zkontroluje, jestli je  *\_PageStart.cshtml* stránky, a pokud ano, který spouští. Pak spustí k požadované stránce.

Uvnitř  *\_PageStart.cshtml* stránky, můžete určit, kam během zpracování, které chcete k požadované stránce spuštění zahrnutím `RunPage` metoda. To vám umožní spustit kód před spuštěním k požadované stránce a potom znovu za ním. Pokud nechcete zahrnout `RunPage`, kód v  *\_PageStart.cshtml* běží a potom k požadované stránce automaticky spustí.

![[Obrázek]](18-customizing-site-wide-behavior/_static/image3.jpg)

Technologie ASP.NET umožňuje vytvořit hierarchii  *\_PageStart.cshtml* soubory. Můžete vložit  *\_PageStart.cshtml* soubor v kořenovém adresáři serveru a všechny podsložky. Při požadavku na stránku  *\_PageStart.cshtml* souboru na nejvyšší úrovni (který je nejblíže kořenovému adresáři webu) spustí, za nímž následuje  *\_PageStart.cshtml* souboru v dalším podsložku, a tak dále dolů strukturu podsložky, dokud požadavek dosáhne složku, která obsahuje požadovanou stránku. Po všech příslušných  *\_PageStart.cshtml* spustili soubory, spustí se požadovaná stránka.

Například můžete mít následující kombinace  *\_PageStart.cshtml* soubory a *Default.cshtml* souboru:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Při spuštění */myfolder/default.cshtml*, zobrazí se následující:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Spuštění kódu inicializace pro všechny stránky ve složce

Vhodné využít pro  *\_PageStart.cshtml* souborů je k chybě při inicializaci stránce rozložení pro všechny soubory v jedné složce.

1. V kořenové složce vytvořte novou složku s názvem *InitPages*.
2. V *InitPages* složku vašeho webu, vytvořte soubor s názvem  *\_PageStart.cshtml* a výchozí značek a kódu nahraďte následujícím kódem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. V kořenovém adresáři webu vytvořte složku s názvem *sdílené*.
4. V *sdílené* složky, vytvořte soubor s názvem  *\_Layout1.cshtml* a výchozí značek a kódu nahraďte následujícím kódem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. V *InitPages* složky, vytvořte soubor s názvem *Content1.cshtml* a nahradí existující obsah následujícím kódem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. V *InitPages* složce vytvořte jiný soubor s názvem *Content2.cshtml* a výchozí značka nahraďte následujícím kódem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Spustit *Content1.cshtml* v prohlížeči. 

    ![[Obrázek]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Při *Content1.cshtml* stránka běží,  *\_PageStart.cshtml* souboru sady `Layout` a také nastaví `PageData["MyBackground"]` na barvu. V *Content1.cshtml*, se použijí rozložení a barvy.
8. Zobrazení *Content2.cshtml* v prohlížeči. 

    Rozložení je stejné, protože obě stránky použít stejnou rozložení stránky a barvu jako v inicializovat  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Pomocí \_PageStart.cshtml ke zpracování chyb

Jiné dobré použít pro  *\_PageStart.cshtml* souboru je vytvoření způsob, jak zpracovávat programovací chyby (výjimky), které můžou nastat v některém *.cshtml* stránku ve složce. Tento příklad ukazuje jeden způsob, jak to udělat.

1. V kořenové složce vytvořte složku s názvem *InitCatch*.
2. V *InitCatch* složku vašeho webu, vytvořte soubor s názvem  *\_PageStart.cshtml* a existující značek a kódu nahraďte následujícím kódem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    V tomto kódu, můžete se pokusit spustit k požadované stránce explicitně voláním `RunPage` metodu `try` bloku. Pokud dojde k chybám programování v požadované stránce, kód uvnitř `catch` blokovat spuštění. V tomto případě kód provede přesměrování na stránku (*Error.cshtml*) a předá název souboru, který jako část adresy URL došlo k chybě. (Vytvoříte stránky za chvíli.)
3. V *InitCatch* složku vašeho webu, vytvořte soubor s názvem *Exception.cshtml* a existující značek a kódu nahraďte následujícím kódem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Pro účely tohoto příkladu jaké úlohy na této stránce úmyslně vytváří chybu tak, že zkusíte otevřít databázový soubor, který neexistuje.
4. V kořenové složce vytvořte soubor s názvem *Error.cshtml* a existující značek a kódu nahraďte následujícím kódem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Na této stránce výraz `@Request["source"]` získá hodnotu z adresy URL a zobrazí se.
5. Na panelu nástrojů klikněte na tlačítko **Uložit**.
6. Spustit *Exception.cshtml* v prohlížeči. 

    ![[Obrázek]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Vzhledem k tomu, že dojde k chybě v *Exception.cshtml*,  *\_PageStart.cshtml* k přesměrování stránky *Error.cshtml* souboru, který zobrazí zprávu.

    Další informace o výjimkách najdete v tématu [Úvod k technologii ASP.NET Web Pages programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Pomocí \_PageStart.cshtml omezit přístup ke složce

Můžete také  *\_PageStart.cshtml* souboru omezit přístup na všechny soubory ve složce.

1. Ve službě WebMatrix, vytvořte nový web pomocí **šablonu z webu** možnost.
2. Vyberte z dostupných šablon, **Starter Site**.
3. V kořenové složce vytvořte složku s názvem *AuthenticatedContent*.
4. V *AuthenticatedContent* složky, vytvořte soubor s názvem  *\_PageStart.cshtml* a existující značek a kódu nahraďte následujícím kódem: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Kód spustí tím, že všechny soubory ve složce mezipaměti. (To je nutný v případech, například veřejné počítače, kde nechcete, aby jeden uživatel stránky uložené v mezipaměti k dispozici pro další uživatele.) V dalším kroku kód určuje, zda uživatel přihlásí na web před zobrazením kterákoli ze stránek ve složce. Pokud se uživatel není přihlášen, kód přesměruje na přihlašovací stránku. Přihlašovací stránky může vrátit uživatele na stránku, která původně požádal Pokud zahrnete hodnotu řetězce dotazu s názvem `ReturnUrl`.
5. Vytvořit novou stránku v *AuthenticatedContent* složku s názvem *Page.cshtml*.
6. Nahraďte kód výchozí s následujícími službami:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Spustit *Page.cshtml* v prohlížeči. Kód vás přesměruje na přihlašovací stránku. Je nutné zaregistrovat před přihlášení. Po zaregistrované a přihlášení, můžete přejít na stránku a zobrazte její obsah.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky

[Úvod do programování pomocí syntaxe Razor rozhraní ASP.NET Web Pages.](https://go.microsoft.com/fwlink/?LinkID=251587)
