---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: "Uživatelské rozhraní a navigace | Microsoft Docs"
author: Erikre
description: "Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 7f1d8a1a473820a7c8da4c8086904cc41c86fd2a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="ui-and-navigation"></a>Uživatelské rozhraní a navigace
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


V tomto kurzu budete upravovat rozhraní výchozí webové aplikace pro podporu funkcí aplikace front úložiště adresář Wingtip Toys. Navíc budete přidávat jednoduché a data vázaný navigace. V tomto kurzu vychází předchozí kurzu "Vytvoření Data Access Layer" a je součástí série kurz adresář Wingtip Toys.

## <a name="what-youll-learn"></a>Získáte informace:

- Postup změny uživatelského rozhraní pro podporu funkcí aplikace front úložiště adresář Wingtip Toys.
- Postup konfigurace element HTML5 zahrnout navigaci na stránce.
- Postup vytvoření ovládacího prvku datové přejděte na konkrétní produkt data.
- Postupy: zobrazení dat z databáze vytvořené pomocí platformy Entity Framework Code First.

Webové formuláře ASP.NET umožňují vytvářet dynamické obsahu pro webovou aplikaci. Každou webovou stránku ASP.NET je vytvořen v podobně jako při statické HTML webové stránky (stránka, která nezahrnuje zpracování na serveru), ale webová stránka ASP.NET obsahuje dodatečné prvky, které ASP.NET rozpozná a procesy při spuštění stránky generují kód HTML.

Statické stránky HTML (*.htm* nebo *.html* soubor), server splňuje `Web` žádosti o čtení souboru a jeho jako odesláním-je do prohlížeče. Naopak když někdo požadavky webovou stránku ASP.NET (*.aspx* soubor), stránka se spustí jako aplikace na webovém serveru. Při spuštěné stránce může provádět všechny úlohy, které vyžaduje váš Web z umístění, včetně výpočet hodnot, čtení nebo zápis informace o databázi nebo volání jiných programů. Jako výstup stránka dynamicky vytváří značky (jako jsou elementy ve formátu HTML) a odešle tato dynamického výstupu do prohlížeče.

## <a name="modifying-the-ui"></a>Úprava uživatelského rozhraní

Tento kurz řady budete pokračovat změnou *Default.aspx* stránky. Slouží k úpravě uživatelského rozhraní, která je už vytvořené pomocí výchozí šablony použité k vytvoření aplikace. Typ změny, které uděláte jsou typické při vytváření všech aplikací webových formulářů. Uděláte to tak, že změna názvu, nahraďte obsah a odebrání nepotřebných výchozí obsah.

1. Otevřete nebo přepněte *Default.aspx* stránky.
2. Pokud stránky se zobrazí v **návrhu** zobrazení, přepněte do **zdroj** zobrazení.
3. V horní části stránky na `@Page` direktivy, změny `Title` atribut "Vítejte", jak je znázorněno zvýrazněných v žlutý níže. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Také na *Default.aspx* stránky, nahraďte všechna výchozí obsahu obsažené v `<asp:Content>` označovat tak, aby se zobrazí jako kód níže. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Uložit *Default.aspx* stránky tak, že vyberete **uložit Default.aspx** z **souboru** nabídky.

 Výsledná *Default.aspx* stránka bude vypadat takto: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

V příkladu jste nastavili `Title` atribut `@Page` – direktiva. Když se zobrazí kód HTML v prohlížeči, serverový kód `<%: Page.Title %>` přeloží na obsah obsažené v `Title` atribut.

Příklad stránky obsahuje základní prvky, které tvoří webovou stránku ASP.NET. Tato stránka obsahuje statický text jako můžete mít na stránce HTML, společně s prvky, které jsou specifické pro technologii ASP.NET. Součástí obsahu *Default.aspx* integrovat stránky obsahu stránky předlohy, čímž se vysvětluje dále v tomto kurzu.

### <a name="page-directive"></a>@Page– Direktiva

Webové formuláře ASP.NET obvykle obsahují direktivy, které vám umožní zadat stránku vlastností a informace o konfiguraci pro stránku. Direktivy jsou použitý technologií ASP.NET jako pokyny, jak proces stránce, ale nejsou vykresleno jako součást kód, který je odesláno prohlížeči.

Je nejčastěji používanou – direktiva `@Page` – direktiva, což vám umožní určit mnoho možností konfigurace pro stránku, včetně následujících:

1. Server programovací jazyk pro kód na stránce, například C#.
2. Jestli stránka je stránka s kódem serveru přímo na stránce, která se nazývá jednoho souboru stránky, nebo zda je na stránce s kódem v samostatném souboru třídy, kterému se říká kódu stránky.
3. Jestli má přidruženou stránku předlohy stránky a proto by měla být považovány za stránku obsahu.
4. Možnosti ladění a trasování.

Pokud nepoužijete `@Page` na stránce nebo pokud direktiva neobsahuje konkrétní nastavení, bude nastavení zděděno z *Web.config* konfigurační soubor nebo z *Machine.config* konfigurační soubor. *Machine.config* soubor poskytuje další konfiguraci nastavení pro všechny aplikace spuštěné na počítači.

> [!NOTE] 
> 
> *Machine.config* také obsahuje podrobné informace o nastavení všechny možné konfigurace.


### <a name="web-server-controls"></a>Ovládací prvky webového serveru

Ve většině aplikací webových formulářů ASP.NET přidáte ovládací prvky, které umožňují uživatelům interakci s stránky, jako jsou tlačítka, textová pole, seznamy a tak dále. Tyto ovládací prvky webového serveru jsou podobné tlačítka HTML a elementy vstupu. Nicméně jsou zpracovávány na serveru, budete moci použít serverový kód pro nastavení jejich vlastností. Tyto ovládací prvky také vyvolávání událostí, které může zpracovat v serverovém kódu.

Ovládací prvky serveru použít speciální syntaxi, kterou ASP.NET rozpozná při spuštění stránky. Název značky pro serverových ovládacích prvků ASP.NET začíná `asp:` předponu. To umožňuje technologii ASP.NET rozpoznat a zpracování těchto ovládacích prvků serveru. Předpona, která mohou lišit, pokud ovládací prvek není součástí rozhraní .NET Framework. Kromě `asp:` předpona, serverových ovládacích prvků ASP.NET také zahrnovat `runat="server"` atribut a `ID` můžete odkazovat na ovládací prvek v serverovém kódu.

Při spuštění stránky ASP.NET identifikuje ovládací prvky serveru a spouští kód, který je přidružen kontrolní mechanismy. Mnoho ovládacích prvků vykresluje jazyk HTML nebo jiných značek na stránku, jakmile se zobrazí v prohlížeči.

### <a name="server-code"></a>Serverový kód

Většina aplikací webových formulářů ASP.NET obsahovat kód, který běží na serveru při zpracování stránky. Jak je uvedeno nahoře, serverový kód slouží k provádění různých akcí, jako je například přidávání dat do ovládacího prvku ListView. ASP.NET podporuje mnoho jazyků pro spuštění na serveru, včetně C#, Visual Basic, J# a dalších.

Technologie ASP.NET podporuje dva modely pro psaní kódu serveru pro webovou stránku. V modelu s jedním souborem kód pro stránky je v elementu skript, kde počáteční značka obsahuje `runat="server"` atribut. Alternativně můžete vytvořit kód pro stránku v samostatném souboru třídy, což se označuje jako model kódu. V takovém případě stránky webových formulářů ASP.NET obecně neobsahuje žádný kód serveru. Místo toho `@Page` – direktiva zahrnují informace, které odkazuje *.aspx* stránku s jeho přidružené kódu souboru.

`CodeBehind` Atributů, které jsou součástí `@Page` – direktiva Určuje název souboru samostatné třídy a `Inherits` atribut určuje název třídy v souboru kódu na pozadí, která odpovídá na stránku.

### <a name="updating-the-master-page"></a>Aktualizace stránky předlohy

V webových formulářů ASP.NET hlavní stránky umožňují vytvořit konzistentního rozložení pro stránky v aplikaci. Jedna stránka předlohy definuje vzhled a chování a standardní chování, které chcete použít pro všechny stránky (nebo skupinu stránek) ve vaší aplikaci. Potom můžete vytvořit jednotlivé stránky obsahu, které obsahují obsah, který chcete zobrazit, jak je popsáno výše. Pokud uživatelé požadují stránky obsahu, budou ASP.NET sloučí se stránkou předlohy k vytvoření výstupu, který kombinuje rozložení stránky předlohy s obsahem ze stránky obsahu.

Nová lokalita musí jeden logo, které se zobrazí na každé stránce. Chcete-li přidat tento logo, můžete upravit HTML na hlavní stránce.

1. V **Průzkumníku řešení**, najít a otevřít **Site.Master** stránky.
2. Pokud je stránce v **návrhu** zobrazení, přepněte do **zdroj** zobrazení.
3. Aktualizovat hlavní stránky pomocí **úprava nebo přidání** kód zvýrazněných v žlutý: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Tento HTML se zobrazí obrázek s názvem *logo.jpg* z *bitové kopie* složce webové aplikace, které přidáte později. Po zobrazení stránky, která používá hlavní stránce v prohlížeči se zobrazí logo. Pokud uživatel klikne na logo, uživatel bude přejděte zpět *Default.aspx* stránky. Značka HTML anchor `<a>` zabalí ovládací prvek bitové kopie serveru a umožňuje obrázek, který má být zahrnut jako součást odkazu. `href` Atributů pro značku ukotvení Určuje kořenový "`~/`" webu jako odkaz umístění. Ve výchozím nastavení *Default.aspx* stránky se zobrazí, když uživatel přejde do kořenového adresáře webu. **Image** `<asp:Image>` ovládací prvek serveru zahrnuje přidání vlastnosti, jako například `BorderStyle`, vykreslí jako kód HTML, kdy se zobrazí v prohlížeči.

### <a name="master-pages"></a>Stránky předlohy

Hlavní stránka je soubor s příponou .master (například *Site.Master*) s předdefinované rozložení, které mohou zahrnovat statický text, HTML elementy a ovládací prvky serveru. Hlavní stránka je označena speciální `@Master` direktiva, která nahrazuje `@Page` direktiva, která se používá pro běžném provozu *.aspx* stránky.

Kromě `@Master` direktivy, stránky předlohy také obsahuje všechny elementy HTML nejvyšší úrovně pro stránku, jako například `html`, `head`, a `form`. Na hlavní stránce jste přidali výše, můžete použít například HTML `table` rozložení, `img` element pro logo společnosti, statický text a ovládací prvky serveru pro zpracování běžné členství pro svůj web. V rámci hlavní stránky můžete použít libovolný kód HTML a všechny prvky technologie ASP.NET.

Kromě statický text a ovládací prvky, které se zobrazí na všechny stránky, stránky předlohy také zahrnuje jednu nebo více **ContentPlaceHolder** ovládací prvky. Tyto ovládací prvky zástupný symbol definovat oblastech, kde se objeví nahraditelný obsah. Pak nahraditelný obsah je definována v stránky obsahu, jako například *Default.aspx*pomocí **obsah** prvku serveru.

#### <a name="adding-image-files"></a>Přidání souborů bitové kopie

Obrázek loga, který se odkazuje výše, společně s všechny produktu bitové kopie, musíte přidat do webové aplikace tak, že je můžete vidět, když na projekt se zobrazí v prohlížeči.

#### <a name="download-from-msdn-samples-site"></a>Stáhněte z webu MSDN ukázky:

[Začínáme s 4.5 webových formulářů ASP.NET a Visual Studio 2013 – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

Stahování obsahuje prostředky v *Northwind prostředky* složky, které se používají k vytvoření ukázkové aplikace.

1. Pokud jste tak již neučinili, stáhněte si komprimovaný ukázkové soubory pomocí výše uvedený odkaz z webu MSDN ukázky.
2. Po stažení, otevřete soubor .zip a zkopírujte obsah do místní složky v počítači.
3. Najít a otevřít *Northwind prostředky* složky.
4. Pomocí přetahování a vkládání, zkopírujte *katalogu* složky z místní složky do kořenového adresáře projektu webové aplikace v **Průzkumníku řešení** sady Visual Studio. 

    ![Uživatelské rozhraní a navigace - kopírovat soubory](ui_and_navigation/_static/image1.png)
5. Dál vytvořte novou složku s názvem *bitové kopie* kliknutím pravým tlačítkem myši **Northwind** projektu v **Průzkumníku řešení** a výběrem **přidat**  - &gt; **Novou složku**.
6. Kopírování *logo.jpg* souboru z *Northwind prostředky* složky v **Průzkumníka souborů** k *bitové kopie* složce webové aplikace v projektu **Průzkumníku řešení** sady Visual Studio.
7. Klikněte **zobrazit všechny soubory** možnost v horní části **Průzkumníku řešení** aktualizovat seznam souborů, pokud se nezobrazí nové soubory.  
  
    **Průzkumník řešení** nyní zobrazuje soubory aktualizovaný projekt. 

    ![Uživatelské rozhraní a navigace – Průzkumník řešení](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Přidání stránky

Před přidáním navigace k webové aplikaci, budete nejprve přidejte dva nové stránky, které budete přejděte do. Později v kurzu této série budete zobrazit produkty a podrobnosti o produktu na tyto nové stránky.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **Northwind**, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**.   
 **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **webové** skupiny šablony na levé straně. Pak vyberte **webové formuláře se stránkou předlohy** ze středu seznamu a pojmenujte ji *ProductList.aspx*. 

    ![Uživatelského rozhraní a navigace – přidat novou položku – dialogové okno](ui_and_navigation/_static/image3.png)
3. Vyberte **Site.Master** připojit stránky předlohy pro nově vytvořený *.aspx* stránky. 

    ![Uživatelské rozhraní a navigace – vyberte stránku předlohy](ui_and_navigation/_static/image4.png)
4. Přidání další stránky s názvem *ProductDetails.aspx* pomocí následujících kroků stejné.

### <a name="updating-bootstrap"></a>Aktualizace Bootstrap

Šablony projektů Visual Studio 2013 pomocí [Bootstrap](http://getbootstrap.com/), rozložení a motivů rozhraní vytvořené Twitter. K poskytování přizpůsobivý návrh, což znamená, že rozložení se může dynamicky přizpůsobit velikost okna jiný prohlížeč používá Bootstrap CSS3. Funkce motivů Bootstrap je také můžete snadno ovlivňuje změnu aplikace vzhled a chování. Ve výchozím nastavení zahrnuje šablony webové aplikace ASP.NET v sadě Visual Studio 2013 Bootstrap jako balíčku NuGet.

V tomto kurzu se změní vzhled a chování aplikace adresář Wingtip Toys nahrazením soubory Bootstrap šablon stylů CSS.

1. V **Průzkumníku řešení**, otevřete *obsahu* složky.
2. Klikněte pravým tlačítkem myši *bootstrap.css* souborů a přejmenujte ji na *bootstrap original.css*.
3. Přejmenujte *bootstrap.min.css* k *bootstrap original.min.css*.
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *obsahu* složky a vyberte **otevřít složku v Průzkumníku souborů**.  
 Zobrazí se Průzkumníku souborů. Stažené bootstrap soubory šablon stylů CSS uloží do tohoto umístění.
5. V prohlížeči přejděte na [http://Bootswatch.com](http://bootswatch.com/).
6. Přejděte okna prohlížeče, dokud neuvidíte Cerulean motivu. 

    ![Uživatelské rozhraní a navigace - Cerulean motiv](ui_and_navigation/_static/image5.png)
7. Stáhněte si oba *bootstrap.css* souboru a *bootstrap.min.css* do souboru *obsahu* složky. Použijte cestu ke složce obsahu, který se zobrazí v **Průzkumníka souborů** okno, které jste dříve otevřen.
8. V **Visual Studio** v horní části **Průzkumníku řešení**, vyberte **zobrazit všechny soubory** možnosti zobrazíte nové soubory ve složce obsahu. 

    ![Uživatelské rozhraní a navigace – Průzkumník řešení](ui_and_navigation/_static/image6.png)

 Zobrazí se dva nové soubory šablon stylů CSS v **obsahu** složka, ale Všimněte si, že na ikonu vedle názvu každého souboru je zobrazena šedě. To znamená, že soubor nebyl ještě přidán do projektu.
9. Klikněte pravým tlačítkem myši *bootstrap.css* a *bootstrap.min.css* soubory a vyberte **zahrnout do projektu**.   
 Při spuštění aplikace adresář Wingtip Toys později v tomto kurzu, zobrazí se nové uživatelské rozhraní.

> [!NOTE] 
> 
> Šablony webové aplikace ASP.NET používá *Bundle.config* soubor v kořenovém adresáři projektu k uložení cesta k souborům Bootstrap šablon stylů CSS.


### <a name="modifying-the-default-navigation"></a>Úprava výchozí navigace

Výchozí navigaci na každé stránce v aplikaci můžete změnit úpravou element navigace neuspořádaný seznam, který je v *Site.Master* stránky.

1. V **Průzkumníku řešení**, vyhledejte a otevřete *Site.Master* stránky.
2. Přidáte odkaz na další navigační zvýrazněných v žlutý k neuspořádaný seznam vidíte níže:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Jak můžete vidět v kódu HTML výše, můžete upravit každou položku řádku `<li>` obsahující značku ukotvení `<a>` s odkazem `href` atribut. Každý `href` odkazuje na stránku ve webové aplikaci. V prohlížeči, když uživatel klikne na jeden z těchto odkazů (například **produkty**), bude přejděte na stránku obsažené v `href` (například **ProductList.aspx**). Na konci tohoto kurzu budete spouštět aplikace.

> [!NOTE] 
> 
> Tilda (`~`) znak se používá k určení, který `href` cesta začíná v kořenovém adresáři projektu.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Přidání ovládacího prvku Data k zobrazení dat navigace

V dalším kroku přidáte ovládacího prvku zobrazíte všechny kategorie z databáze. Každá kategorie bude fungovat jako odkaz *ProductList.aspx* stránky. Když uživatel klikne na odkaz kategorie v prohlížeči, budou se přejít na stránku produktů a zobrazit pouze produkty, které jsou spojené s vybranou kategorii.

Budete používat **ListView** ovládacího prvku pro zobrazení všech kategorií obsaženým v databázi. Chcete-li přidat **ListView** řízení k hlavní stránce:

1. V *Site.Master* stránky, přidejte následující zvýrazněnou `<div>` element **po** `<div>` element obsahující `id="TitleContent"` které jste přidali dříve:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Tento kód se zobrazí všechny kategorie z databáze. **ListView** ovládací prvek zobrazí každý název kategorie jako text odkazu a zahrnuje odkaz *ProductList.aspx* stránka s obsahující hodnotu řetězce dotazu `ID` kategorie. Nastavením `ItemType` vlastnost **ListView** řídit, vazby dat výraz `Item` je k dispozici v rámci `ItemTemplate` uzel a řízení se stane silného typu. Můžete vybrat podrobnosti `Item` pomocí IntelliSense, například určení `CategoryName`. Tento kód se nachází v kontejneru `<%#: %>` který označuje výraz datové vazby. Přidáním (:) na konec `<%#` předpona, bude výsledkem výrazu vazby dat je kódovaný jazykem HTML. Pokud je výsledek kódovaný jazykem HTML, vaše aplikace je líp chráněný před webů skriptu HTML vkládání útokům a vkládání (XSS).

> [!NOTE] 
> 
> **Tip**
> 
> Když přidáte kód tak, že zadáte během vývoje, můžete byste si být jisti, že platný člen objektu je najít, protože silného typu ovládací prvky datových zobrazují Dostupní členové založené na technologii IntelliSense. IntelliSense nabízí kódu vhodný kontext při psaní kódu, třeba vlastnosti, metod a objekty.


V dalším kroku budete implementovat `GetCategories` metoda načíst data.

### <a name="linking-the-data-control-to-the-database"></a>Ovládací prvek pro datové připojení k databázi

Než budete moci zobrazit dat v ovládacím prvku data, budete muset propojit ovládací prvek dat do databáze. Chcete-li na odkaz, můžete upravit kód za *Site.Master.cs* souboru.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Site.Master* a pak klikněte na tlačítko **kód zobrazení**. *Site.Master.cs* otevření souboru v editoru.
2. V blízkosti začátku *Site.Master.cs* soubor, přidejte dva další obory názvů tak, aby všechny zahrnuté obory názvů měly vypadat následovně:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Přidat zvýrazněné `GetCategories` metoda po `Page_Load` obslužné rutiny události následujícím způsobem:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Výše uvedený kód se spustí při načtení všechny stránky, která používá hlavní stránce v prohlížeči. `ListView` Ovládací prvek (s názvem "categoryList"), který jste přidali dříve v tomto kurzu použije k výběru dat vazby modelu. Ve značkách `ListView` řízení nastavíte ovládacího prvku `SelectMethod` vlastnost, která má `GetCategories` metody uvedené výše. `ListView` Řízení volání `GetCategories` metoda v příslušnou dobu v stránky životní cyklus a automaticky váže vrácená data. Další informace o vytvoření vazby dat v dalším kurzu se dozvíte.

### <a name="running-the-application-and-creating-the-database"></a>Spuštění aplikace a vytvoření databáze

Dříve v této série kurzu jste vytvořili třídu inicializátoru (s názvem "ProductDatabaseInitializer") a zadat této třídy v *global.asax.cs* souboru. Rozhraní Entity Framework vygeneruje databáze, když aplikace běží poprvé, protože `Application_Start` metody, které jsou součástí *global.asax.cs* soubor bude volat třída inicializátoru. Třída inicializátoru použije třídy modelu (`Category` a `Product`) jste přidali dříve v této série kurz k vytvoření databáze.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *Default.aspx* a vyberte **nastavit jako úvodní stránku**.
2. Ve Visual Studiu stisknutím klávesy **F5**.   
 Bude trvat nějakou dobu nastavit všechno během této nejprve spustit.   
    ![Uživatelské rozhraní a navigace - okna prohlížeče](ui_and_navigation/_static/image7.png)  
 Při spuštění aplikace, aplikace bude zkompilován a databáze s názvem *wingtiptoys.mdf* budou vytvořeny v *aplikace\_Data* složky. V prohlížeči se zobrazí kategorie navigační nabídky. Tato nabídka vygenerovalo načítání kategorií z databáze. V dalším kurzu budete implementovat navigaci.
3. Zavřete prohlížeč zastavit běžící aplikaci.

### <a name="reviewing-the-database"></a>Kontrola databáze

Otevřete *Web.config* souboru a podívejte se na část řetězce připojení. Uvidíte, že `AttachDbFilename` hodnota v připojovacím řetězci odkazuje na `DataDirectory` pro projekt webové aplikace. Hodnota `|DataDirectory|` je rezervovaná hodnota, která představuje *aplikace\_Data* složky v projektu. Tato složka nachází, kde je umístěna databáze, který byl vytvořen z třídy entity.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Pokud *aplikace\_Data* složka není viditelný nebo pokud je prázdná, vyberte **aktualizovat** ikonu a potom **zobrazit všechny soubory** ikona v horní části **Průzkumníku řešení** okno. Rozšiřování šířku **Průzkumníku řešení** windows může být nutné zobrazit všechny dostupných ikon.


Teď si můžete prohlédnout data obsažená v *wingtiptoys.mdf* soubor databáze pomocí **Průzkumníka serveru** okno.

1. Rozbalte *aplikace\_Data* složky. Pokud *aplikace\_Data* složka není zobrazena, přečtěte si poznámku výše.
2. Pokud *wingtiptoys.mdf* soubor databáze není viditelná, vyberte **aktualizovat** ikonu a potom **zobrazit všechny soubory** ikona v horní části **Průzkumníku řešení**  okno.
3. Klikněte pravým tlačítkem myši *wingtiptoys.mdf* souboru databáze a vyberte **otevřete**.  
    **V Průzkumníku serveru** se zobrazí. 

    ![Uživatelské rozhraní a navigace - Průzkumníka serveru](ui_and_navigation/_static/image8.png)
4. Rozbalte *tabulky* složky.
5. Klikněte pravým tlačítkem myši **produkty**tabulky a vyberte **zobrazit Data tabulky**.  
 **Produkty** tabulka se zobrazí. 

    ![Uživatelské rozhraní a navigace – tabulky produktů](ui_and_navigation/_static/image9.png)
6. Toto zobrazení umožňuje zobrazit a upravit data v **produkty** tabulky ručně.
7. Zavřít **produkty** okno tabulky.
8. V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **produkty** tabulky znovu a vyberte **otevřete definici tabulky**.  
 Návrh dat pro **produkty** tabulka se zobrazí. 

    ![Uživatelské rozhraní a navigace – produkty návrhu](ui_and_navigation/_static/image10.png)
9. V **T-SQL** kartě uvidíte příkaz SQL DDL, která byla použita k vytvoření tabulky. Můžete také použít rozhraní v **návrhu** kartě k úpravě schématu.
10. V **Průzkumníka serveru**, klikněte pravým tlačítkem na **Northwind** databáze a vyberte **ukončení připojení**.   
 Podle odpojování databáze ze sady Visual Studio, bude moct změnit později z této série kurz schématu databáze.
11. Vraťte se do **Průzkumníku řešení**výběrem **Průzkumníku řešení** karta v dolní části **Průzkumníka serveru** okno.

## <a name="summary"></a>Souhrn

V tomto kurzu řady přidáte některé základní uživatelské rozhraní, grafické, stránky a navigace. Kromě toho jste spustili webové aplikace, který vytvoří databázi z datových tříd, které jste přidali v předchozím kurzu. Také si obsah *produkty* tabulky databáze zobrazením databázi přímo. V dalším kurzu budete zobrazit datové položky a informace z databáze.

## <a name="additional-resources"></a>Další prostředky

[Úvod do programování webových stránek ASP.NET](https://msdn.microsoft.com/library/ms178125.aspx)   
[Přehled ovládacích prvků webového serveru technologie ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Kurz šablon stylů CSS](http://www.w3schools.com/css/default.asp)

>[!div class="step-by-step"]
[Předchozí](create_the_data_access_layer.md)
[další](display_data_items_and_details.md)
