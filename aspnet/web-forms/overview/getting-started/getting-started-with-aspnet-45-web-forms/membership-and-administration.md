---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Členství a Správa | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 23d08d5a05a8321fbc794e2c9b54cc39c9b5baf6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754269"
---
<a name="membership-and-administration"></a>Členství a Správa
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


V tomto kurzu se dozvíte, jak aktualizovat ukázkové aplikace Wingtip Toys přidat vlastní roli a používat ASP.NET Identity. Je také ukazuje, jak implementovat stránku pro správu ze kterého uživatel s vlastní role můžete přidávat a odebírat produkty z webu.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) je systém členství použít k sestavení webové aplikace ASP.NET a je k dispozici v technologii ASP.NET 4.5. ASP.NET Identity se používá v šabloně projektu webových formulářů sady Visual Studio 2013, stejně jako šablony pro [ASP.NET MVC](../../../../mvc/index.md), [rozhraní ASP.NET Web API](../../../../web-api/index.md), a [jednostránkové aplikace ASP.NET](../../../../single-page-application/index.md). Můžete nainstalovat také konkrétně systém ASP.NET Identity, když začínáte s prázdnou webovou aplikaci pomocí nástroje NuGet. Ale použít v této sérii kurzů **webových formulářů**projecttemplate, která zahrnuje systém identit technologie ASP.NET. ASP.NET Identity snadno integrovat data profilu uživatelská data aplikací. ASP.NET Identity navíc umožňuje zvolte model trvalosti pro profily uživatelů ve vaší aplikaci. Data můžete ukládat v databázi serveru SQL Server nebo jiného úložiště dat, včetně *NoSQL* úložiště dat, jako jsou tabulky v úložišti Windows Azure.

V tomto kurzu se navazuje na předchozí kurz o službě s názvem "Pokladna a platba s PayPal" v této sérii kurzů na adresář Wingtip Toys.

## <a name="what-youll-learn"></a>Co se dozvíte:

- Jak přidat vlastní roli a uživatele k aplikaci pomocí kódu.
- Jak omezit přístup do složky správy a stránky.
- Jak poskytnout navigace pro uživatele, který patří do vlastní roli.
- Jak naplnit pomocí vazby modelu [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) ovládací prvek s kategorií produktů.
- Postup nahrání souboru do webové aplikace pomocí [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) ovládacího prvku.
- Postup použití ověřovacích ovládacích prvků k provedení ověření vstupu.
- Jak přidávat a odebírat produkty z aplikace.

## <a name="these-features-are-included-in-the-tutorial"></a>Tyto funkce jsou zahrnuté v tomto kurzu:

- ASP.NET Identity
- Konfigurace a ověření
- Vazby modelu
- Nerušivý ověření

Webové formuláře ASP.NET poskytuje možnosti členství. Pomocí výchozí šablony mají členství integrované funkce, které můžete okamžitě použít při spuštění aplikace. V tomto kurzu se dozvíte, jak přidat vlastní roli a přiřadit k této roli uživatele pomocí ASP.NET Identity. Se dozvíte, jak omezit přístup ke složce pro správu. Přidejte stránky ke složce pro správu, který umožňuje uživateli s vlastní roli přidávat a odebírat produkty a poté, co se přidal ve verzi preview produktu.

## <a name="adding-a-custom-role"></a>Přidání vlastní roli

ASP.NET Identity můžete přidat vlastní roli a přiřadit uživatele do této role pomocí kódu.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na *logiky* složky a vytvořte novou třídu.
2. Pojmenujte novou třídu *RoleActions.cs*.
3. Upravte kód tak, aby vypadal takto:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. V **Průzkumníka řešení**, otevřete *Global.asax.cs* souboru.
5. Upravit *Global.asax.cs* soubor přidáním kódu zvýrazněné žlutou barvou tak, aby vypadal takto:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Všimněte si, že `AddUserAndRole` je podtrženo červenou barvou. Dvakrát klikněte na panel AddUserAndRole kódu.  
   Bude podtržené písmeno "A" na začátku metody zvýrazněné.
7. Najeďte myší na písmeno "A" a klikněte na uživatelské rozhraní, která umožňuje generovat pahýl metody pro `AddUserAndRole` metody. 

    ![Členství a Advministration - generovat Pahýl metody](membership-and-administration/_static/image1.png)
8. Klikněte na možnost s názvem:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Otevřít *RoleActions.cs* soubor *logiky* složky.  
   `AddUserAndRole` Metoda byla přidána do souboru třídy.
10. Upravit *RoleActions.cs* souboru odebráním `NotImplementedeException` a přidání kódu zvýrazněné žlutou barvou, tak, aby vypadal takto:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Výše uvedený kód nejprve vytvoří kontext databáze pro databázi členství. Databáze členství je také uložena jako *.mdf* soubor *aplikace\_Data* složky. Bude moct zobrazit tuto databázi po první uživatel přihlášen na tuto webovou aplikaci. 

> [!NOTE] 
> 
> Pokud chcete k ukládání dat o členství spolu s daty o produktu, můžete zvážit použití stejné **DbContext** , který jste použili k uložení dat produktu ve výše uvedeném kódu.


 *Interní* – klíčové slovo je modifikátor přístupu pro typy (například třídy) a členy typů (například metody nebo vlastnosti). Vnitřní typy nebo členy jsou přístupné jenom v souborech, které jsou obsaženy ve stejném sestavení *(.dll* souboru). Když vytváříte aplikaci, soubor sestavení *(.dll*) se vytvoří, která obsahuje kód, který se spustí při spuštění aplikace. 

A `RoleStore` objektu, který poskytuje správu rolí, je založeno na kontext databáze.

> [!NOTE] 
> 
> Všimněte si, že `RoleStore` je vytvořen objekt používá obecný `IdentityRole` typu. To znamená, že `RoleStore` je povolen pouze tak, aby obsahovala `IdentityRole` objekty. Také pomocí obecných typů prostředků v paměti jsou zpracovány lépe.


Dále `RoleManager` objektu, je vytvořena na základě `RoleStore` objekt, který jste právě vytvořili. `RoleManager` rozhraní API, které lze automaticky uloží změny související s rolemi zpřístupňuje objektu `RoleStore`. `RoleManager` Je povolen pouze tak, aby obsahovala `IdentityRole` objekty, protože tento kód použije `<IdentityRole>` obecného typu.

Volání `RoleExists` metodou ke zjištění, zda je k dispozici v databázi členství role "hodnoty canEdit". Pokud není, vytvoříte roli.

Vytváří `UserManager` objektu se zdá být složitější než `RoleManager` řízení, ale je téměř stejný. Je právě kódován na jednom řádku, nikoli několik. Tady je vytvoření parametru, který se předá instance jako nový objekt součástí v závorkách.

Dále vytvoříte uživatele "canEditUser" tak, že vytvoříte nový `ApplicationUser` objektu. Potom, pokud úspěšně vytvoříte uživatele, je přidat do nové role uživatele.

> [!NOTE] 
> 
> Zpracování chyb bude aktualizován v průběhu kurzu "Zpracování chyb technologie ASP.NET" dále v této sérii kurzů.


Při příštím spuštění aplikace uživatel s názvem "canEditUser" se přidá jako roli s názvem "hodnoty canEdit" aplikace. Později v tomto kurzu se přihlásíte jako uživatele "canEditUser" a zobrazte další možnosti, které budou přidány během tohoto kurzu. Rozhraní API podrobnosti o ASP.NET Identity najdete v tématu [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Další podrobnosti o inicializaci systému ASP.NET Identity najdete v tématu [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Omezení přístupu na stránku pro správu

Ukázkové aplikace Wingtip Toys umožňuje anonymním uživatelům i přihlášeným uživatelům zobrazení a nákup produktů. Přihlášeného uživatele, který má vlastní "hodnoty canEdit" roli však můžete přistupovat k stránku s omezeným přístupem, aby bylo možné přidávat a odebírat produkty.

#### <a name="add-an-administration-folder-and-page"></a>Přidejte složku správu a stránky

V dalším kroku se vytvoří složku s názvem *správce* "canEditUser" uživatele patřícího k roli vlastní Wingtip Toys ukázkovou aplikaci.

1. Klikněte pravým tlačítkem na název projektu (**Wingtip Toys**) v **Průzkumníka řešení** a vyberte **přidat**  - &gt; **novou složku**.
2. Název nové složky *správce*.
3. Klikněte pravým tlačítkem myši *správce* složku a pak vyberte **přidat**  - &gt; **nová položka**.   
   **Přidat novou položku** se zobrazí dialogové okno.
4. Vyberte <strong>Visual C#</strong> - &gt; <strong>webové</strong> šablony skupiny na levé straně. Vyberte ze seznamu střední <strong>webové formuláře se stránkou předlohy</strong>, pojmenujte ho <em>AdminPage.aspx</em><strong>,</strong> a pak vyberte <strong>přidat</strong>.
5. Vyberte *Site.Master* soubor jako hlavní stránky a pak zvolte **OK**.

#### <a name="add-a-webconfig-file"></a>Přidání souboru Web.config

Přidáním *Web.config* do souboru *správce* složky, můžete omezit přístup na stránku obsažené ve složce.

1. Klikněte pravým tlačítkem myši *správce* a pak zvolte položku **přidat**  - &gt; **nová položka**.  
   **Přidat novou položku** se zobrazí dialogové okno.
2. V seznamu webové šablony Visual C#, vyberte <strong>souboru konfigurace webu</strong>ze seznamu střední přijměte výchozí název <em>Web.config</em><strong>,</strong> a pak vyberte <strong>Přidat</strong>.
3. Nahradit existující obsah v XML *Web.config* souboru následujícím kódem:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Uložit *Web.config* souboru. *Web.config* souboru Určuje, že pouze uživatele patřícího k roli "hodnoty canEdit" aplikace můžou přistupovat k stránce obsažené v *správce* složky.

### <a name="including-custom-role-navigation"></a>Včetně navigace vlastní roli

Povolit uživateli roli vlastní "hodnoty canEdit" přejděte do části Správa aplikace, je nutné přidat odkaz *Site.Master* stránky. Uživatelé, kteří patří do role "hodnoty canEdit" budou moci zobrazit jenom **správce** propojit a přístup k bodu správy.

1. V Průzkumníku řešení vyberte a otevřete *Site.Master* stránky.
2. Pro vytvoření odkazu na uživatelské roli "hodnoty canEdit", přidání značek zvýrazněné žlutou barvou následující Neseřazený seznam `<ul>` element tak, aby v seznamu se zobrazí jako následující:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Otevřít *Site.Master.cs* souboru. Ujistěte se, **správce** viditelné pouze pro uživatele "canEditUser" tak, že přidáte kód zvýrazněné žlutou barvou na odkaz `Page_Load` obslužné rutiny. `Page_Load` Obslužnou rutinu se zobrazí takto:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Když se stránka načte, kód kontroluje, zda má uživatel přihlášený role "hodnoty canEdit". Pokud uživatel patří do role "hodnoty canEdit", obsahující odkaz na element span *AdminPage.aspx* stránce (a v důsledku propojení uvnitř značky span) je nastavena jako viditelná.

### <a name="enabling-product-administration"></a>Povolení správy produktu

Zatím jste vytvořili roli "hodnoty canEdit" a přidat uživatele s "canEditUser" složku správu a stránku pro správu. Nastavení oprávnění pro složku správu a stránku a přidali navigační odkaz pro uživatele "hodnoty canEdit" role do aplikace. V dalším kroku přidáte kód k *AdminPage.aspx* stránce a kódu *AdminPage.aspx.cs* soubor kódu na pozadí, která vám umožní uživatelům přidávat a odebírat produkty s rolí "hodnoty canEdit".

1. V **Průzkumníka řešení**, otevřete *AdminPage.aspx* soubor *správce* složky.
2. Nahraďte existující kód následujícím kódem:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Dále otevřete *AdminPage.aspx.cs* použití modelu code-behind souboru kliknutím pravým tlačítkem myši *AdminPage.aspx* a kliknete na **zobrazit kód**.
4. Nahraďte existující kód ve třídě *AdminPage.aspx.cs* použití modelu code-behind souboru následujícím kódem:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

V kódu, který jste zadali pro *AdminPage.aspx.cs* soubor kódu na pozadí, třídu s názvem `AddProducts` ve skutečnosti zajišťuje zpracování přidat produkty k databázi. Tato třída ještě neexistuje, proto ji bude nyní vytvořit.

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na *logiky* složku a pak vyberte **přidat**  - &gt; **nová položka**.   
   **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **kód** šablony skupiny na levé straně. Vyberte **třídy**uprostřed seznamu a pojmenujte ho *AddProducts.cs*.   
   Zobrazí se nový soubor třídy.
3. Nahraďte stávající kód následujícím kódem:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx* stránka umožní uživatele patřícího k roli "hodnoty canEdit" přidávat a odebírat produkty. Při přidání nového produktu, podrobnosti o produktu se ověří a potom zadán do databáze. Nový produkt je ihned k dispozici pro všechny uživatele webové aplikace.

#### <a name="unobtrusive-validation"></a>Nerušivý ověření

Informace o produktech, které uživatel zadá na *AdminPage.aspx* stránky se ověřují pomocí validačních ovládacích prvků (`RequiredFieldValidator` a `RegularExpressionValidator`). Tyto ovládací prvky automaticky pomocí nerušivého ověření. Umožňuje nerušivý ověření validačních ovládacích prvků pomocí jazyka JavaScript pro logiku ověřování na straně klienta, což znamená, že na stránce nevyžaduje postoupí do serveru, který má být ověřen. Ve výchozím nastavení, je součástí nerušivý ověření *Web.config* soubor založený na následující nastavení:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Regulární výrazy

Ceny produktu na *AdminPage.aspx* stránky se ověří pomocí **RegularExpressionValidator** ovládacího prvku. Tento ovládací prvek ověřuje, zda je hodnota z přidruženého vstupního ovládacího prvku ("AddProductPrice" textového pole) odpovídá vzoru regulárního výrazu určeném. Regulární výraz je zápis porovnávání se vzorem, který umožňuje rychle najít a porovnat konkrétních znakových vzorů. **RegularExpressionValidator** ovládací prvek obsahuje vlastnost s názvem `ValidationExpression` , který obsahuje regulární výraz používaný k ověření vstupu ceny, jak je znázorněno níže:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Ovládací prvek fileUpload

Kromě ovládací prvky vstupu a ověření jste přidali **FileUpload** ovládací prvek *AdminPage.aspx* stránky. Tento ovládací prvek umožňuje nahrát soubory. V takovém případě můžete povolují pouze soubory bitové kopie k odeslání. V souboru kódu na pozadí (*AdminPage.aspx.cs*), když `AddProductButton` dojde ke kliknutí na, kontroly kódu `HasFile` vlastnost **FileUpload** ovládacího prvku. Pokud je ovládací prvek souboru a pokud může typ souboru (podle přípony souboru), na obrázku se uloží do *image* složky a *Image/Thumbs* složky aplikace.

#### <a name="model-binding"></a>Vazby modelu

Dříve v této sérii kurzů jste použili vazby modelu a naplnit **ListView** ovládací prvek, **FormsView** ovládací prvek, **GridView** ovládací prvek a  **DetailView** ovládacího prvku. V tomto kurzu použijete k vyplnění vazby modelu **DropDownList** ovládacího prvku seznam kategorií produktů.

Kód, který jste přidali do *AdminPage.aspx* obsahuje soubor **DropDownList** ovládací prvek s názvem `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Vazby modelu použijete k vyplnění to **DropDownList** nastavením `ItemType` atribut a `SelectMethod` atribut. `ItemType` Atribut určuje, že používáte `WingtipToys.Models.Category` zadejte při naplňování ovládacího prvku. Definice tohoto typu na začátku této sérii kurzů tak, že vytvoříte `Category` třídy (viz dole). `Category` Hodina může začít *modely* složky uvnitř *Category.cs* souboru.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod` Atribut **DropDownList** ovládacího prvku určuje, že použijete `GetCategories` – metoda (viz dole), který je zahrnut do souboru kódu na pozadí (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Tato metoda určuje, že `IQueryable` rozhraní se používá k vyhodnocení dotazu na `Category` typu. Vrácená hodnota se používá k naplnění **DropDownList** v kódu stránky (*AdminPage.aspx*).

Text zobrazený pro každou položku v seznamu se určuje nastavením `DataTextField` atribut. `DataTextField` Atribut používá `CategoryName` z `Category` třídy (popsaný výš) pro každou kategorii v zobrazení **DropDownList** ovládacího prvku. Skutečná hodnota, která je předána, když je položka vybrána v **DropDownList** ovládací prvek je založen na `DataValueField` atribut. `DataValueField` Atribut je nastaven na `CategoryID` definovat v `Category` třídy (viz výše).

### <a name="how-the-application-will-work"></a>Jak bude aplikace fungovat.

Když uživatele patřícího k roli "hodnoty canEdit" odkazuje na stránku pro první spuštění `DropDownAddCategory` **DropDownList** naplnění ovládacího prvku, jak je popsáno výše. `DropDownRemoveProduct` **DropDownList** ovládací prvek se taky vyplní produkty, které používají stejným způsobem. Vybere typ kategorie uživatele patřícího k roli "hodnoty canEdit" a přidá podrobnosti o produktu (**název**, **popis**, **cena**, a **soubor bitové kopie**). Když uživatele patřícího k roli "hodnoty canEdit" klikne **přidat produkt** tlačítko, `AddProductButton_Click` obslužná rutina události se aktivuje. `AddProductButton_Click` Obslužná rutina události nachází v souboru kódu na pozadí (*AdminPage.aspx.cs*) kontroluje soubor obrázku se ujistěte, že odpovídá typů souborů povolených *(.gif*, *.png*, *.jpeg*, nebo *.jpg*). Potom soubor bitové kopie se uloží do složky ukázkové aplikace Wingtip Toys. V dalším kroku se přidá nového produktu do databáze. K provedení přidání nového produktu, novou instanci třídy `AddProducts` třídy je vytvořen a s názvem produktů. `AddProducts` Třída obsahuje metodu s názvem `AddProduct`, a objekt produkty volá tuto metodu za účelem přidání produktů do databáze.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Pokud kód úspěšně přidá nového produktu do databáze, na stránce opětovném načtení nástroje s hodnotu řetězce dotazu `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Když znovu načte stránku, řetězec dotazu součástí adresy URL. Znovu načíst tuto stránku, uživatele patřícího k roli "hodnoty canEdit" můžete okamžitě zjistit aktualizace v **DropDownList** ovládací prvky na *AdminPage.aspx* stránky. Také pokud uvedete řetězec dotazu s adresou URL, na stránce lze zobrazit zprávu o úspěšném dokončení uživatele patřícího k roli "hodnoty canEdit".

Když *AdminPage.aspx* stránce opětovné načtení, `Page_Load` události je volána.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load` Obslužná rutina události zkontroluje hodnotu řetězce dotazu a určuje, jestli se má zobrazit zpráva o úspěchu.

## <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit nyní aplikaci chcete zobrazit, jak můžete přidat, odstranit a aktualizovat položky v nákupním košíku. Nákupní košík celkem bude odrážet celkové náklady na všechny položky v nákupním košíku.

1. V Průzkumníku řešení, stiskněte klávesu **F5** ke spuštění ukázkové aplikace Wingtip Toys.  
   V prohlížeči se otevře a zobrazí *Default.aspx* stránky.
2. Klikněte na tlačítko **přihlášení** odkazu v horní části stránky. 

    ![Členství a správa – Přihlaste se odkaz](membership-and-administration/_static/image2.png)

   *Login.aspx* zobrazí se stránka.
3. Použijte následující uživatelské jméno a heslo:  
   Uživatelské jméno: canEditUser@wingtiptoys.com  
   Heslo: Pa$ $word1 

    ![Členství a správa – přihlašovací stránkou](membership-and-administration/_static/image3.png)
4. Klikněte na tlačítko **přihlášení** tlačítko v dolní části stránky.
5. V horní části stránky na další stránku, vyberte **správce** odkaz přejděte *AdminPage.aspx* stránky. 

    ![Členství a správa – správce odkazů](membership-and-administration/_static/image4.png)
6. Chcete-li otestovat ověření vstupu, klikněte na tlačítko **přidat produkt** tlačítko bez nutnosti přidávat žádné podrobnosti o produktu. 

    ![Členství a správa – stránky pro správu](membership-and-administration/_static/image5.png)

   Všimněte si, že se zobrazují zprávy povinné pole.
7. Přidejte podrobnosti o nového produktu a klikněte **přidat produkt** tlačítko. 

    ![Členství a správa – přidání produktu](membership-and-administration/_static/image6.png)
8. Vyberte **produkty** horní navigační nabídce zobrazíte nového produktu jste přidali. 

    ![Členství a správa – zobrazit nový produkt](membership-and-administration/_static/image7.png)
9. Klikněte na tlačítko **správce** odkaz se vraťte na stránku pro správu.
10. V **odebrat produkt** části stránky vyberte nového produktu, který jste přidali v kroku **DropDownListBox**.
11. Klikněte na tlačítko **odebrat produkt** tlačítko k odstranění nového produktu z aplikace. 

    ![Členství a správa – odebrat produkt](membership-and-administration/_static/image8.png)
12. Vyberte **produkty** z horní navigační nabídce potvrďte, že produkt byl odebrán.
13. Klikněte na tlačítko **Odhlásit** existovat režimu správy.   
    Všimněte si, že už nebude zobrazovat v horním navigačním podokně **správce** položky nabídky.

## <a name="summary"></a>Souhrn

V tomto kurzu přidat vlastní roli a uživatele patřícího k roli vlastní omezený přístup ke stránce a složku správy a navigace pro uživatele patřícího k vlastní roli k dispozici. Jste použili k naplnění vazby modelu **DropDownList** ovládacího prvku s daty. Můžete implementovat **FileUpload** ovládacího prvku a validačních ovládacích prvků. Také jste se naučili, jak přidávat a odebírat produkty z databáze. V dalším kurzu se dozvíte, jak implementovat směrování ASP.NET.

## <a name="additional-resources"></a>Další prostředky

[Web.config – autorizace – Element](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Nasazení aplikace zabezpečené rozhraní ASP.NET Web Forms pomocí Membership, OAuth a SQL Database na web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Předchozí](checkout-and-payment-with-paypal.md)
> [další](url-routing.md)
