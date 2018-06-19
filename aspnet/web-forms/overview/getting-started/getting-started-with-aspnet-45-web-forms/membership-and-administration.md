---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Členství a správu | Microsoft Docs
author: Erikre
description: Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 166bc642ea2083f455be0648e424f0b0ae3b082c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
ms.locfileid: "30889628"
---
<a name="membership-and-administration"></a>Členství a Správa
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


V tomto kurzu se dozvíte, jak aktualizovat adresář Wingtip Toys ukázkové aplikace a přidat vlastní role pomocí ASP.NET Identity. Je také ukazuje, jak implementovat stránku správy, ze kterého uživatel s vlastní roli můžete přidávat a odebírat produkty z webu.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) systém členství použít k vytvoření webové aplikace ASP.NET a je k dispozici v technologii ASP.NET 4.5. ASP.NET Identity se používá v šabloně projektů Visual Studio 2013 webových formulářů, jakož i šablon pro [ASP.NET MVC](../../../../mvc/index.md), [rozhraní ASP.NET Web API](../../../../web-api/index.md), a [jedné stránky aplikace ASP.NET](../../../../single-page-application/index.md). Můžete také konkrétně nainstalovat ASP.NET Identity systému, když začínáte s prázdnou webovou aplikaci pomocí nástroje NuGet. Však použít tento kurz řady **webových formulářů**projecttemplate, která zahrnuje systém identit technologie ASP.NET. ASP.NET Identity lze snadno integrovat data profilu uživatelská data aplikací. ASP.NET Identity navíc umožňuje vyberte model trvalosti pro profily uživatelů ve vaší aplikaci. Data můžete uložit do databáze systému SQL Server nebo jiného úložiště dat, včetně *NoSQL* úložiště dat, jako jsou tabulky úložiště služby Windows Azure.

V tomto kurzu vychází předchozí kurzu s názvem "Najdete v článku věnovaném a platebních s PayPal" v řadě kurz adresář Wingtip Toys.

## <a name="what-youll-learn"></a>Získáte informace:

- Jak přidat vlastní roli a uživatele k aplikaci pomocí kódu.
- Jak omezit přístup pro správu složku a stránky.
- Jak poskytnout navigace pro uživatele, který patří do vlastní role.
- Postup použití vazby modelu k naplnění [rozevírací seznam](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) ovládacího prvku pomocí kategorie produktů.
- Postup nahrání souboru do webové aplikace pomocí [odesílání souborů při odpovědích](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) ovládacího prvku.
- Jak používat ovládací prvky pro ověřování k provedení ověření vstupu.
- Jak přidávat a odebírat produkty z aplikace.

## <a name="these-features-are-included-in-the-tutorial"></a>Tyto funkce jsou zahrnuté v tomto kurzu:

- ASP.NET Identity
- Konfigurace a autorizace
- Vazby modelu
- Ověření nerušivého

Webové formuláře ASP.NET poskytuje možnosti členství. Pomocí výchozí šablony, máte členství integrované funkce, které můžete okamžitě použít, když je aplikace spuštěná. V tomto kurzu se dozvíte, jak používat ASP.NET Identity k přidání vlastní role a přiřazení uživatele k dané roli. Se dozvíte, jak omezit přístup ke složce pro správu. Na stránce přidáte do složky správy, která umožňuje uživateli s vlastní roli, přidávat a odebírat produkty a poté, co byl přidán náhled produktu.

## <a name="adding-a-custom-role"></a>Přidání vlastní Role

Pomocí ASP.NET Identity, můžete přidat vlastní role a přiřaďte uživatele do této role pomocí kódu.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na *logiku* složky a vytvořte novou třídu.
2. Pojmenujte novou třídu *RoleActions.cs*.
3. Upravte kód tak, aby se následujícím způsobem:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. V **Průzkumníku řešení**, otevřete *Global.asax.cs* souboru.
5. Změnit *Global.asax.cs* souboru tak, že přidáte kód zvýrazněných v žlutý tak, aby se následujícím způsobem:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Všimněte si, že `AddUserAndRole` je podtržený červeně. Dvakrát klikněte na kód AddUserAndRole.  
   Bude podtržené písmeno "A" na začátku zvýrazněná metoda.
7. Pozastavte ukazatel myši nad písmenem "A" a klikněte na uživatelské rozhraní, které vám umožní generovat metoda zástupnou proceduru pro `AddUserAndRole` metoda. 

    ![Členství a Advministration - generovat Stub – metoda](membership-and-administration/_static/image1.png)
8. Klikněte na možnost s názvem:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Otevřete *RoleActions.cs* souboru z *logiku* složky.  
   `AddUserAndRole` Metoda byla přidána do souboru třídy.
10. Změnit *RoleActions.cs* souboru odebráním `NotImplementedeException` a přidávání kódu zvýrazněných v žlutý, tak, aby se následujícím způsobem:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Výše uvedený kód nejprve vytvoří kontext databáze pro databázi členství. Databáze členství je také uloženo jako *.mdf* v soubor *aplikace\_Data* složky. Bude moct zobrazit tuto databázi, jakmile prvního uživatel přihlášeného k této webové aplikace. 

> [!NOTE] 
> 
> Pokud chcete k ukládání dat členství spolu s daty produktu, můžete zvážit použití stejné **DbContext** jste použili k uložení dat produktu ve výše uvedeném kódu.


 *Interní* – klíčové slovo je – modifikátor přístupu pro typy (například třídy) a členy typu (například metody nebo vlastnosti). Vnitřní typy nebo členové jsou přístupné pouze v rámci soubory obsažené ve stejném sestavení *(.dll* souboru). Při vytváření aplikace, soubor sestavení *(.dll*) je vytvořen, obsahuje kód, který se spustí při spuštění aplikace. 

A `RoleStore` objekt, který poskytuje správu rolí, je na základě vytvořit kontext databáze.

> [!NOTE] 
> 
> Všimněte si, že pokud `RoleStore` je vytvořen objekt používá obecný `IdentityRole` typu. To znamená, že `RoleStore` je povoleno pouze tak, aby obsahovala `IdentityRole` objekty. Také pomocí obecných typů prostředků v paměti jsou zpracovávány lépe.


Dále `RoleManager` objektu, se vytvářejí na základě `RoleStore` objekt, který jste právě vytvořili. `RoleManager` rozhraní API, která umožňuje automaticky uloží změny související s rolemi zpřístupňuje objektu `RoleStore`. `RoleManager` Je povoleno pouze tak, aby obsahovala `IdentityRole` objekty vzhledem k tomu, že kód používá `<IdentityRole>` obecného typu.

Volání `RoleExists` metoda k určení, zda je přítomen v databázi členství role "hodnoty canEdit". Pokud není, můžete vytvořit role.

Vytváření `UserManager` objekt se zdá být složitější než `RoleManager` řídit, ale je téměř stejný. Toto pravidlo je zakódovaný jenom na jeden řádek, nikoli několik. Zde je vytvoření parametr, který jste předali instance jako nový objekt součástí v závorkách.

Dále vytvořte uživatele "canEditUser" tak, že vytvoříte novou `ApplicationUser` objektu. Pak, pokud úspěšně vytvořit uživateli, přidejte uživatele do nové role.

> [!NOTE] 
> 
> Zpracování chyb bude aktualizován v průběhu z kurzu této série kurz "Zpracování chyb ASP.NET".


Při dalším spuštění aplikace s názvem "canEditUser" uživatele budou přidáni jako roli s názvem "hodnoty canEdit" aplikace. Později v tomto kurzu se přihlásit jako uživatel "canEditUser" zobrazíte další možnosti, které budou přidány během tohoto kurzu. Rozhraní API podrobnosti o ASP.NET Identity najdete v tématu [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Další podrobnosti o inicializaci systému ASP.NET Identity naleznete [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Omezení přístupu na stránku správy

Ukázkovou aplikaci adresář Wingtip Toys umožňuje anonymní uživatelé i přihlášeného uživatele k zobrazení a zakoupit produkty. Přihlášený uživatel s rolí vlastní hodnoty "canEdit" však může přistupovat k omezenou stránku, aby bylo možné přidávat a odebírat produkty.

#### <a name="add-an-administration-folder-and-page"></a>Přidat složku správu a stránky

V dalším kroku vytvoříte složku s názvem *správce* pro uživatele "canEditUser" patřícího k roli vlastní adresář Wingtip Toys ukázkové aplikace.

1. Klikněte pravým tlačítkem na název projektu (**adresář Wingtip Toys**) v **Průzkumníku řešení** a vyberte **přidat**  - &gt; **novou složku**.
2. Název nové složky *správce*.
3. Klikněte pravým tlačítkem myši *správce* složku a potom vyberte **přidat**  - &gt; **novou položku**.   
   **Přidat novou položku** se zobrazí dialogové okno.
4. Vyberte <strong>Visual C#</strong> - &gt; <strong>webové</strong> skupiny šablony na levé straně. Vyberte ze seznamu střední <strong>webové formuláře se stránkou předlohy</strong>, pojmenujte ji <em>AdminPage.aspx</em><strong>,</strong> a pak vyberte <strong>přidat</strong>.
5. Vyberte *Site.Master* souboru jako stránky předlohy a potom vyberte **OK**.

#### <a name="add-a-webconfig-file"></a>Přidá soubor Web.config

Přidáním *Web.config* do souboru *správce* složky, můžete omezit přístup na stránku obsažené ve složce.

1. Klikněte pravým tlačítkem myši *správce* složky a vyberte **přidat**  - &gt; **novou položku**.  
   **Přidat novou položku** se zobrazí dialogové okno.
2. V seznamu webové šablony Visual C# vyberte <strong>soubor webové konfigurace</strong>ze seznamu střední přijměte výchozí název <em>Web.config</em><strong>,</strong> a pak vyberte <strong>Přidat</strong>.
3. Nahradit existující obsah XML *Web.config* soubor s následující:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Uložit *Web.config* souboru. *Web.config* souboru Určuje, že pouze uživatel patřící do role "hodnoty canEdit" aplikace můžete přístup ke stránce součástí *správce* složky.

### <a name="including-custom-role-navigation"></a>Včetně navigační vlastní Role

Pokud chcete povolit uživatelům roli vlastní hodnoty "canEdit" přejděte do části Správa aplikace, musíte přidat odkaz *Site.Master* stránky. Uživatelé, kteří patří k roli "hodnoty canEdit" se moci zobrazit pouze **správce** propojit a přístup v části Správa.

1. V Průzkumníku řešení, najít a otevřít *Site.Master* stránky.
2. Pro vytvoření odkazu pro uživatele "hodnoty canEdit" role, přidejte značku zvýrazněných v žlutý následující neuspořádaný seznam `<ul>` element tak, aby v seznamu se zobrazí jako následuje:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Otevřete *Site.Master.cs* souboru. Ujistěte se, **správce** viditelné pouze pro uživatele "canEditUser" tak, že přidáte kód zvýrazněných v žlutý na odkaz `Page_Load` obslužné rutiny. `Page_Load` Obslužná rutina zobrazí následujícím způsobem:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Když se stránka načte, kód zkontroluje, zda přihlášený uživatel má roli "hodnoty canEdit". Pokud uživatel patří do role "hodnoty canEdit", obsahující odkaz na element span *AdminPage.aspx* stránce (a v důsledku toho odkaz uvnitř značky span) jsou dostupná.

### <a name="enabling-product-administration"></a>Povolení správy produktu

Zatím jste vytvořili roli "hodnoty canEdit" a přidali uživatele s "canEditUser", na složku správu a stránku pro správu. Nastavili jste přístupová práva pro správu složku a stránky a přidali navigační odkaz pro uživatele "hodnoty canEdit" role do aplikace. Dále přidáte kód k *AdminPage.aspx* stránky a chcete-li kód *AdminPage.aspx.cs* souboru kódu na pozadí, které umožní uživatelům přidávat a odebírat produkty s rolí "hodnoty canEdit".

1. V **Průzkumníku řešení**, otevřete *AdminPage.aspx* souboru z *správce* složky.
2. Nahraďte stávající značky s následujícími službami:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Dále otevřete *AdminPage.aspx.cs* souboru kódu kliknutím pravým tlačítkem myši *AdminPage.aspx* a kliknutím na **kód zobrazení**.
4. Nahraďte stávající kód v *AdminPage.aspx.cs* souboru kódu s následujícím kódem:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

V kódu, který jste zadali pro *AdminPage.aspx.cs* souboru kódu na pozadí, třídu s názvem `AddProducts` ve skutečnosti zajišťuje zpracování přidávání produkty do databáze. Tato třída, dosud neexistuje, můžete ji nyní vytvoří.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *logiku* složku a potom vyberte **přidat**  - &gt; **novou položku**.   
   **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **kód** skupiny šablony na levé straně. Pak vyberte **třída**ze středu seznamu a pojmenujte ji *AddProducts.cs*.   
   Zobrazí se nový soubor třídy.
3. Nahraďte stávající kód s následujícími službami:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx* stránky umožňuje uživatele patřícího k roli "hodnoty canEdit" přidávat a odebírat produkty. Při přidání nového produktu, podrobnosti o produktu jsou ověřit a poté zadat do databáze. Nového produktu je okamžitě k dispozici všem uživatelům webové aplikace.

#### <a name="unobtrusive-validation"></a>Ověření nerušivého

Podrobnosti o produktu, které poskytuje uživatele na *AdminPage.aspx* stránky se ověřují pomocí ovládacích prvků pro ověřování (`RequiredFieldValidator` a `RegularExpressionValidator`). Tyto ovládací prvky automaticky používat nerušivý ověření. Ověření nerušivého umožňuje ověření ovládacích prvků pomocí jazyka JavaScript pro logiku ověřování na straně klienta, což znamená, že stránce nevyžaduje zpráv se serverem, který má být ověřen. Ve výchozím nastavení, je součástí nerušivý ověření *Web.config* soubor podle nastavení konfigurace se následující:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Regulární výrazy

Cena produktu na *AdminPage.aspx* stránky se ověří pomocí **RegularExpressionValidator** ovládacího prvku. Tento ovládací prvek ověřuje, zda hodnota přidruženého vstupního ovládacího prvku (textového pole "AddProductPrice") odpovídá vzorku určeného regulární výraz. Regulární výraz je zápis porovnávání, která umožňuje rychle najít a porovnat vzory zvláštní znak. **RegularExpressionValidator** ovládací prvek obsahuje vlastnost s názvem `ValidationExpression` který obsahuje regulární výraz používaný k ověření vstupu ceny, jak je uvedeno níže:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Ovládací prvek odesílání souborů při odpovědích

Kromě ovládací prvky pro vstup a ověření, jste přidali **odesílání souborů při odpovědích** řídit k *AdminPage.aspx* stránky. Tento ovládací prvek poskytuje možnost odesílat soubory. V takovém případě jsou povolení jenom soubory bitové kopie k odeslání. V souboru kódu na pozadí (*AdminPage.aspx.cs*), když `AddProductButton` po kliknutí na, zkontroluje kód `HasFile` vlastnost **odesílání souborů při odpovědích** ovládacího prvku. Pokud ovládací prvek má soubor a pokud je povolený typ souboru (založené na příponě souboru), je uloženy bitovou kopii *bitové kopie* složky a *bitové kopie nebo palec* složky aplikace.

#### <a name="model-binding"></a>Vazby modelu

Dříve v této série kurz používá vazby modelu k naplnění **ListView** řízení, **FormsView** řízení, **GridView** řízení a  **DetailView** ovládacího prvku. V tomto kurzu použijete k vyplnění vazby modelu **rozevírací seznam** ovládacího prvku seznam kategorií produktů.

Kód, který jste přidali do *AdminPage.aspx* soubor obsahuje **rozevírací seznam** ovládací prvek s názvem `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Použít modelovou vazbu k naplnění to **rozevírací seznam** nastavením `ItemType` atribut a `SelectMethod` atribut. `ItemType` Atribut určuje, že budete používat `WingtipToys.Models.Category` zadejte při naplňování ovládacího prvku. Tento typ na začátku tohoto kurzu řady definované vytváření `Category` – třída (zobrazené dole). `Category` Třída je v *modely* složky uvnitř *Category.cs* souboru.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod` Atribut **rozevírací seznam** řízení Určuje, že budete používat `GetCategories` – metoda (zobrazené dole) který je součástí souboru kódu na pozadí (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Tato metoda určuje, že `IQueryable` rozhraní se používá k vyhodnocení dotazy na data `Category` typu. Vrácená hodnota se používá k naplnění **rozevírací seznam** v kódu stránky (*AdminPage.aspx*).

Text zobrazovaný pro každou položku v seznamu se určuje nastavením `DataTextField` atribut. `DataTextField` Atribut používá `CategoryName` z `Category` – třída (viz výše) pro každou kategorii v zobrazení **rozevírací seznam** ovládacího prvku. Skutečnou hodnotu, která se předá, když je položka vybrána v **rozevírací seznam** ovládací prvek je založen na `DataValueField` atribut. `DataValueField` Je nastavena na hodnotu `CategoryID` také definovat v `Category` – třída (viz výše).

### <a name="how-the-application-will-work"></a>Fungování aplikace

Když uživatel patřící do role "hodnoty canEdit" přejde na stránku poprvé, `DropDownAddCategory` **rozevírací seznam** ovládací prvek je vyplněný, jak je popsáno výše. `DropDownRemoveProduct` **Rozevírací seznam** řízení se také zobrazí v produkty, které používají stejný přístup. Vybere typ kategorie uživatele patřícího k roli "hodnoty canEdit" a přidá podrobnosti o produktu (**název**, **popis**, **cena**, a **soubor bitové kopie**). Když uživatel patřící do role "hodnoty canEdit" klikne **přidat produkt** tlačítko `AddProductButton_Click` obslužné rutiny události se aktivuje. `AddProductButton_Click` Obslužné rutiny události nachází v souboru kódu na pozadí (*AdminPage.aspx.cs*) zkontroluje, bitové kopie souboru a ujistěte se, odpovídá povoleného typu *(.gif*, *.png*, *.jpeg*, nebo *.jpg*). Pak je soubor obrázku uložen do složky, adresář Wingtip Toys ukázkové aplikace. V dalším kroku nového produktu se přidá do databáze. K provedení přidání nového produktu, novou instanci třídy `AddProducts` třída se vytvoří a s názvem produkty. `AddProducts` Třída má metodu s názvem `AddProduct`, a objekt produkty volá tuto metodu za účelem přidání produktů do databáze.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Pokud kód úspěšně přidá nového produktu do databáze, je znovu načíst stránku s hodnotou řetězce dotazu `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Když znovu načte stránky, řetězec dotazu je součástí adresy URL. Opětovným načtením stránky, okamžitě uživatele patřícího k roli "hodnoty canEdit" uvidí aktualizace v **rozevírací seznam** na ovládací prvky *AdminPage.aspx* stránky. Navíc zahrnutím řetězec dotazu s adresou URL stránky zobrazit zpráva o úspěšném provedení pro uživatele patřícího k roli "hodnoty canEdit".

Když *AdminPage.aspx* stránky zavážky, `Page_Load` událost je volána.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load` Obslužné rutiny události kontroluje hodnotu řetězce dotazu a určuje, zda se zobrazit zpráva o úspěšném provedení.

## <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit nyní aplikaci zobrazíte přidání, odstranění a aktualizace položek v nákupní košík. Celkový počet nákupního košíku se projeví celkové náklady na všechny položky v nákupní košík.

1. V Průzkumníku řešení, stiskněte klávesu **F5** ke spuštění ukázkové aplikace adresář Wingtip Toys.  
   Otevře se prohlížeč a ukazuje *Default.aspx* stránky.
2. Klikněte **přihlásit** odkaz v horní části stránky. 

    ![Členství a správu - protokolu v odkazu](membership-and-administration/_static/image2.png)

   *Login.aspx* zobrazí se stránka.
3. Použijte následující uživatelské jméno a heslo:  
   Uživatelské jméno: canEditUser@wingtiptoys.com  
   Heslo: Pa$ $word1 

    ![Na stránce protokol členství a správu-](membership-and-administration/_static/image3.png)
4. Klikněte **přihlásit** tlačítko v dolní části stránky.
5. V horní části na další stránku, vyberte **správce** odkazu přejděte na *AdminPage.aspx* stránky. 

    ![Členství a správu – Správce odkaz](membership-and-administration/_static/image4.png)
6. Chcete-li otestovat ověření vstupu, klikněte na tlačítko **přidat produktu** tlačítko bez přidání jakékoli podrobnosti o produktu. 

    ![Členství a správu - stránky pro správu](membership-and-administration/_static/image5.png)

   Všimněte si, že jsou zobrazené zprávy povinné pole.
7. Přidat podrobnosti o novém produktu a klikněte **přidat produktu** tlačítko. 

    ![Členství a správu – přidání produktu](membership-and-administration/_static/image6.png)
8. Vyberte **produkty** v horním navigačním panelu nabídce zobrazení nového produktu jste přidali. 

    ![Členství a správu – zobrazení nového produktu](membership-and-administration/_static/image7.png)
9. Klikněte **správce** odkaz na návrat na stránku správy.
10. V **odebrat produkt** části stránky, vyberte nového produktu, které jste přidali v **DropDownListBox**.
11. Klikněte **odebrat produkt** tlačítko odebrání nového produktu z aplikace. 

    ![Členství a správu - odebrat produktu](membership-and-administration/_static/image8.png)
12. Vyberte **produkty** v horním navigačním panelu nabídce potvrďte, že produkt byl odebrán.
13. Klikněte na tlačítko **Odhlásit** existovat režim pro správu.   
    Všimněte si, že se již nenachází v horním navigačním podokně **správce** položku nabídky.

## <a name="summary"></a>Souhrn

V tomto kurzu přidat vlastní roli a uživatele patřícího k roli vlastní omezený přístup pro správu složku a stránky a poskytuje navigace pro uživatele patřícího k roli vlastní. Používá vazby modelu k naplnění **rozevírací seznam** ovládacího prvku s daty. Můžete implementovat **odesílání souborů při odpovědích** řízení a ovládací prvky pro ověřování. Také jste zjistili, jak přidávat a odebírat produkty z databáze. V dalším kurzu dozvíte, jak implementovat směrování ASP.NET.

## <a name="additional-resources"></a>Další prostředky

[Web.config – autorizace – Element](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Nasazení aplikace zabezpečené rozhraní ASP.NET Web Forms s členství, OAuth a SQL Database k webovému serveru Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Předchozí](checkout-and-payment-with-paypal.md)
> [další](url-routing.md)
