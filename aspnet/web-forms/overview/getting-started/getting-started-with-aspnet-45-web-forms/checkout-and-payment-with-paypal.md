---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: "Najdete v článku věnovaném a platbu se PayPal | Microsoft Docs"
author: Erikre
description: "Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: dd975850a3ed3e7b1746d5123572065675a88656
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="checkout-and-payment-with-paypal"></a>Najdete v článku věnovaném a platbu se PayPal
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


Tento kurz popisuje, jak upravit adresář Wingtip Toys ukázkovou aplikaci autorizace uživatelů, registrace a platebních pomocí služby PayPal. Pouze uživatelé, kteří jsou přihlášení bude mít autorizace, chcete-li zakoupit produkty. Šablona projektu webových formulářů ASP.NET 4.5 předdefinované uživatelské registrace funkce již obsahuje většinu co potřebujete. Přidáte funkci PayPal Express Checkout. V tomto kurzu budete používat vývojáře PayPal testovacím prostředí, tak se nepřenesou žádná skutečná fondů. Na konci tohoto kurzu budete testovat aplikaci tak, že vyberete produktů pro přidání do nákupního košíku, kliknutím na tlačítko ověření a přenosu dat do testování webu PayPal. Na PayPal testování webu bude potvrďte vaše informace o přesouvání a platby a pak se vraťte do místní adresář Wingtip Toys ukázkovou aplikaci pro potvrzení a dokončení nákupu.

Existuje několik procesorů zkušeného platebních třetích stran, které specialize v online nákupy, že adresa škálovatelnosti a zabezpečení. Vývojáři ASP.NET zvažte výhody použití platebních řešení třetí strany před implementací nákupního a nákup řešení.

> [!NOTE] 
> 
> Ukázkovou aplikaci adresář Wingtip Toys navržena k zobrazí konkrétní ASP.NET konceptů a funkcí, které jsou k dispozici vývojářům webů ASP.NET. Tato ukázková aplikace nebyla optimalizované pro všechny možné okolnosti týkající se škálovatelnosti a zabezpečení.


## <a name="what-youll-learn"></a>Získáte informace:

- Jak omezit přístup na určité stránky ve složce.
- Postup vytvoření známé nákupní košík z anonymní nákupní košík.
- Postup povolení protokolu SSL pro projekt.
- Postup přidání poskytovatele OAuth do projektu.
- Jak používat služby PayPal Koupit produkty, které používají služby PayPal testovacího prostředí.
- Jak si můžete zobrazit podrobnosti ze služby PayPal v **DetailsView** ovládacího prvku.
- Postup aktualizace databáze aplikace adresář Wingtip Toys s podrobnostmi získané z PayPal.

## <a name="adding-order-tracking"></a>Přidání pořadí sledování

V tomto kurzu vytvoříte dva nové třídy sledovat data z pořadí, ve kterém uživatel vytvořil. Třídy bude sledovat data týkající se informace o přesouvání, celkový počet zakoupených a potvrzení platby.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Přidání třídy modelu OrderDetail a pořadí

Dříve v této série kurz definované schéma pro kategorie produktů, a nákupní košík položky tak, že vytvoříte `Category`, `Product`, a `CartItem` třídy v *modely* složky. Nyní přidejte dva nové třídy definovat schéma pro produkt pořadí a podrobnosti o pořadí.

1. V **modely** složky, přidejte novou třídu s názvem *Order.cs*.   
 Nový soubor třídy se zobrazí v editoru.
2. Ve výchozím kódu nahraďte následujícím textem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Přidat *OrderDetail.cs* třídy k *modely* složky.
4. Ve výchozím kódu nahraďte následujícím kódem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order` a `OrderDetail` třídy obsahovat schéma můžete definovat informace o objednávce používá pro nákup a přesouvání.

Kromě toho budete muset aktualizovat třídy kontextu databáze, která spravuje tříd entit, který poskytuje přístup k datům do databáze. K tomu budete přidávat nově vytvořený pořadí a `OrderDetail` modelu třídy `ProductContext` třídy.

1. V **Průzkumníku řešení**, najít a otevřít *ProductContext.cs* souboru.
2. Přidejte zvýrazněný kód, který *ProductContext.cs* souboru, jak je uvedeno níže:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Jak je uvedeno nahoře v kurzu této série, kód v *ProductContext.cs* přidá soubor `System.Data.Entity` obor názvů tak, aby měli přístup k základním funkcím Entity Framework. Tato funkce zahrnuje schopnost dotazovat, vložit, aktualizovat a odstranit data ve spolupráci s objektů se silným typem. Výše uvedený kód `ProductContext` třída přidává přístup k rozhraní Entity Framework pro nově přidaný `Order` a `OrderDetail` třídy.

## <a name="adding-checkout-access"></a>Přidání přístupu najdete v článku věnovaném

Ukázkovou aplikaci adresář Wingtip Toys umožňuje anonymním uživatelům ke kontrole a přidat produkty nákupního košíku. Ale při anonymním uživatelům vybrat nákup produkty, které budou přidány do nákupního košíku, se musí přihlásit k webu. Jakmile jste přihlášeni, získají přístup k webové aplikace s omezeným přístupem stránek, které zpracovávají rezervaci a proces nákupní. Tyto stránky s omezeným přístupem, jsou součástí *najdete v článku věnovaném* složky aplikace.

### <a name="add-a-checkout-folder-and-pages"></a>Přidat složku najdete v článku věnovaném a stránky

Nyní vytvoříte *najdete v článku věnovaném* složku a stránky v něm, který zákazník se zobrazí během procesu checkout. Aktualizujte tyto stránek později v tomto kurzu.

1. Klikněte pravým tlačítkem na název projektu (**adresář Wingtip Toys**) v **Průzkumníku řešení** a vyberte **přidejte novou složku**. 

    ![Najdete v článku věnovaném a platbu se PayPal – nová složka](checkout-and-payment-with-paypal/_static/image1.png)
2. Název nové složky *najdete v článku věnovaném*.
3. Klikněte pravým tlačítkem myši *najdete v článku věnovaném* složku a potom vyberte **přidat**-&gt;**novou položku**. 

    ![Najdete v článku věnovaném a platbu se PayPal – nová položka](checkout-and-payment-with-paypal/_static/image2.png)
4. **Přidat novou položku** se zobrazí dialogové okno.
5. Vyberte **Visual C#**  - &gt; **webové** skupiny šablony na levé straně. Pak v prostředním podokně, vyberte **webové formuláře se stránkou předlohy**a pojmenujte ji *CheckoutStart.aspx*. 

    ![Najdete v článku věnovaném a platbu se PayPal – Přidat dialogové okno Nový položky](checkout-and-payment-with-paypal/_static/image3.png)
6. Vyberte jako předtím *Site.Master* souboru stránky předlohy.
7. Přidejte následující další stránky, které chcete *najdete v článku věnovaném* složky pomocí výše uvedených kroků:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Přidá soubor Web.config

Přidáním nové *Web.config* do souboru *najdete v článku věnovaném* složky, bude možné omezit přístup na všechny stránky obsažené ve složce.

1. Klikněte pravým tlačítkem myši *najdete v článku věnovaném* složky a vyberte **přidat**  - &gt; **novou položku**.  
 **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **webové** skupiny šablony na levé straně. Pak v prostředním podokně, vyberte **soubor webové konfigurace**, přijměte výchozí název *Web.config*a potom vyberte **přidat**.
3. Nahradit existující obsah XML *Web.config* soubor s následující:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Uložit *Web.config* souboru.

*Web.config* souboru Určuje, že všechny neznámé uživatele webové aplikace musí odepřen přístup na stránky součástí *najdete v článku věnovaném* složky. Ale pokud je uživatel zaregistrován účtu a je přihlášen, se bude známé uživatele a bude mít přístup k stránky v *najdete v článku věnovaném* složky.

Je důležité si uvědomit, že konfigurace ASP.NET odpovídá hierarchii, kde každý *Web.config* soubor aplikuje nastavení konfigurace do složky, která je v a na všechny podřízené adresáře pod ním.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Povolit protokol SSL pro projekt

 Vrstvy SSL (Secure Sockets) je protokol definované umožnit webové servery a klienty webového bezpečněji komunikovat prostřednictvím šifrování. Pokud není používá protokol SSL, data odesílaná mezi klientem a serverem je otevřená a můžou paketu sledování toku dat ve každý, kdo má fyzický přístup k síti. Kromě toho několik společných schémat ověřování nejsou zabezpečené přes prostý HTTP. Základní ověřování a ověřování pomocí formulářů vyslat nezašifrované přihlašovací údaje. Aby bylo zabezpečené, musí tyto schémat ověřování používat protokol SSL. 

1. V **Průzkumníku řešení**, klikněte na tlačítko **Northwind** projektu a potom stiskněte klávesu **F4** zobrazíte **vlastnosti** okno.
2. Změna **SSL povoleno** k `true`.
3. Kopírování **SSL URL** , kterou můžete použít později.   
 Adresa URL SSL bude `https://localhost:44300/` Pokud jste už vytvořili webů SSL (jak je znázorněno níže).   
    ![Vlastnosti projektu](checkout-and-payment-with-paypal/_static/image4.png)
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem **Northwind** projektu a klikněte na tlačítko **vlastnosti**.
5. V levé kartě klikněte na **webové**.
6. Změna **adresa Url projektu** používat **SSL URL** který jste předtím uložili.   
    ![Vlastnosti webového projektu](checkout-and-payment-with-paypal/_static/image5.png)
7. Uložit stránky stisknutím kláves **CTRL + S**.
8. Stiskněte klávesu **Ctrl + F5** ke spuštění aplikace. Visual Studio se zobrazí možnost vyhnete se upozornění týkající se SSL.
9. Klikněte na tlačítko **Ano** důvěřovat certifikátu SSL služby IIS Express a pokračujte.   
    ![Podrobnosti o certifikátu SSL služby IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Zobrazí se upozornění zabezpečení.
10. Klikněte na tlačítko **Ano** k instalaci certifikátu do vašeho místního hostitele.   
    ![Dialogové okno upozornění zabezpečení](checkout-and-payment-with-paypal/_static/image7.png)  
 Zobrazí se okno prohlížeče.

Teď můžete snadno otestujte webovou aplikaci místně pomocí protokolu SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Přidání poskytovatele OAuth 2.0

Webové formuláře ASP.NET poskytuje rozšířené možnosti pro členství a ověřování. Patří mezi ně OAuth. OAuth je otevřený protokol, který umožňuje zabezpečenou autorizaci v jednoduché a standardní metody z webových, mobilních a desktopových aplikací. Šablony webových formulářů ASP.NET používá ke zveřejnění Facebook, Twitter, Google a Microsoft jako zprostředkovatele ověřování OAuth. I když tento kurz používá jako zprostředkovatel ověřování Google pouze, můžete snadno upravit kód, který použije žádného z poskytovatelů. Kroky pro implementaci jiní poskytovatelé jsou velmi podobné kroky, které se zobrazí v tomto kurzu.

Kromě ověřování bude tento kurz také pomocí rolí k implementaci autorizace. Pouze na uživatele, přidání do `canEdit` nebude moci měnit data role (vytvořit, upravit nebo odstranit kontakty).

> [!NOTE] 
> 
> Aplikace Windows Live přijímají pouze za provozu adresu URL pro funkčního webu, tak pro testování přihlášení nemůžete použít adresu URL místního webu.


Následující postup vám umožní přidat zprostředkovatele ověřování Google.

1. Otevřete *aplikace\_Start\Startup.Auth.cs* souboru.
2. Odeberte komentář znaky z `app.UseGoogleAuthentication()` metoda tak, že metoda zobrazí jako následuje: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Přejděte na [konzole pro vývojáře Google](https://console.developers.google.com/). Musíte se také k přihlášení pomocí účtu Google developer e-mailu (gmail.com). Pokud nemáte účet Google, vyberte **vytvoření účtu** odkaz.   
 V dalším kroku se zobrazí **konzole pro vývojáře Google**.   
    ![Konzole pro vývojáře Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Klikněte **vytvoření projektu** tlačítko a zadejte název projektu a ID (můžete použít výchozí hodnoty). Potom klikněte **políčko smlouvy** a **vytvořit** tlačítko.  

    ![Google – nový projekt](checkout-and-payment-with-paypal/_static/image9.png)

 Během pár sekund se vytvoří nový projekt a prohlížeč zobrazí stránku pro nové projekty.
5. V levé kartě klikněte na **rozhraní API &amp; auth**a potom klikněte na **pověření**.
6. Klikněte **vytvořit nové ID klienta** pod **OAuth**.   
 **Vytvořit ID klienta** zobrazí se dialogové okno.   
    ![Google - vytvořit ID klienta](checkout-and-payment-with-paypal/_static/image10.png)
7. V **vytvořit ID klienta** dialogové okno, ponechte výchozí **webové aplikace** pro typ aplikace.
8. Nastavte **oprávnění zdroje JavaScript** na adresu URL SSL, který jste použili v tomto kurzu (`https://localhost:44300/` Pokud jste vytvořili další projekty SSL).   
 Tato adresa URL je zdrojem pro vaši aplikaci. Tato ukázka se pouze zadejte testovací adresu URL místního hostitele. Můžete však zadat více adres URL pro účet pro místního hostitele a provozu.
9. Nastavte **oprávnění identifikátor URI pro přesměrování** pro následující: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

 Tato hodnota je identifikátor URI tohoto ASP.NET OAuth uživatelům komunikovat se serverem google OAuth. Mějte na paměti, adresu URL SSL jste použili výše ( `https://localhost:44300/` Pokud jste vytvořili další projekty SSL).
10. Klikněte **vytvořit ID klienta** tlačítko.
11. V levé nabídce konzoly Google vývojáře, klikněte na tlačítko **souhlasu obrazovky** položky nabídky, nastavte vaše e-mailovou adresu a název produktu. Pokud jste vyplnili formulář, klikněte na tlačítko **Uložit**.
12. Klikněte **rozhraní API** položky nabídky, přejděte dolů a klikněte na **vypnout** vedle položky **rozhraní API Google +**.   
 Přijetí této možnosti povolíte rozhraní API Google +.
13. Také musíte aktualizovat **Microsoft.Owin** balíček NuGet verzi 3.0.0.   
 Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** a pak vyberte **spravovat balíčky NuGet pro řešení**.  
 Z **spravovat balíčky NuGet** oken, najít a aktualizace **Microsoft.Owin** balíček verze 3.0.0.
14. V sadě Visual Studio, aktualizovat `UseGoogleAuthentication` metodu *Startup.Auth.cs* stránky tak, že kopírování a vkládání **ID klienta** a **tajný klíč klienta** do metody. **ID klienta** a **tajný klíč klienta** hodnoty zobrazené níže jsou příklady a nebude fungovat. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Stiskněte klávesu **CTRL + F5** sestavení a spuštění aplikace. Klikněte **přihlásit** odkaz.
16. V části **přihlásit pomocí jiné služby**, klikněte na tlačítko **Google**.  
    ![Přihlásit se](checkout-and-payment-with-paypal/_static/image11.png)
17. Pokud potřebujete k zadání přihlašovacích údajů, budete přesměrováni na web google, kde bude zadejte svoje přihlašovací údaje.  
    ![Google - přihlášení](checkout-and-payment-with-paypal/_static/image12.png)
18. Po zadání přihlašovacích údajů, zobrazí se výzva k udělit oprávnění k webové aplikaci, kterou jste právě vytvořili.  
    ![Účet služby výchozí projekt](checkout-and-payment-with-paypal/_static/image13.png)
19. Klikněte na tlačítko **přijmout**. Teď můžete bude přesměrován zpět na **zaregistrovat** stránky **Northwind** aplikace, kde můžete zaregistrovat účet služby Google.  
    ![Zaregistrujte se pomocí vašeho účtu Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Máte možnost změny názvu registrace místní e-mailu, který se používá pro váš účet z Gmailu, ale chcete obecně zachovat výchozí e-mailový alias (to znamená, ten, který jste použili pro ověřování). Klikněte na tlačítko **přihlásit** jako v příkladu nahoře.

### <a name="modifying-login-functionality"></a>Úpravy funkce přihlášení

Jak jsme uvedli v řadě tento kurz mnoho funkcí registrace uživatele byl zahrnut v šabloně webových formulářů ASP.NET ve výchozím nastavení. Nyní bude upravit výchozí *Login.aspx* a *Register.aspx* stránky k volání `MigrateCart` metoda. `MigrateCart` Metoda přidruží anonymní nákupní košík nově přihlášeného uživatele. Přidružení uživatele a nákupní košík, bude moci udržovat nákupní košík uživatele mezi návštěvy adresář Wingtip Toys ukázkovou aplikaci.

1. V **Průzkumníku řešení**, najít a otevřít *účtu* složky.
2. Úprava kódu stránky s názvem *Login.aspx.cs* zahrnout kód zvýrazněných v žlutý, tak, aby se následujícím způsobem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Uložit *Login.aspx.cs* souboru.

Teď, můžete ignorovat upozornění, že je k dispozici žádné definice `MigrateCart` metoda. Budete přidávat ho trochu později v tomto kurzu.

*Login.aspx.cs* souboru kódu na pozadí podporuje metodu přihlášení. Zkontrolováním stránku Login.aspx uvidíte, že tato stránka obsahuje tlačítko "Přihlášení" který po klikněte na tlačítko aktivační události `LogIn` obslužné rutiny na modelu code-behind.

Když `Login` metodu *Login.aspx.cs* nazývá novou instanci třídy nákupní košík s názvem `usersShoppingCart` je vytvořena. ID nákupní košík (GUID) je načíst a nastavit na `cartId` proměnné. Potom, `MigrateCart` metoda je volána, předávání i `cartId` a název přihlášeného uživatele k této metodě. Při migraci nákupní košík identifikátor GUID použít k identifikaci anonymní nákupní košík se nahradí uživatelské jméno.

Kromě úpravy *Login.aspx.cs* souboru kódu na migraci nákupní košík, když se uživatel přihlásí, musíte také upravit *souboru kódu Register.aspx.cs* k migraci nákupní košík Když uživatel vytvoří nový účet a přihlásí.

1. V *účet* složku, otevřete soubor modelu code-behind s názvem *Register.aspx.cs*.
2. Upravte soubor kódu zahrnutím kód v žlutý, tak, aby se následujícím způsobem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Uložit *Register.aspx.cs* souboru. Ještě jednou ignorovat upozornění o `MigrateCart` metoda.

Všimněte si, že kód, můžete používat ve `CreateUser_Click` obslužné rutiny události je velmi podobný kódu, kterou jste použili v `LogIn` metoda. Pokud uživatel zaregistruje nebo přihlášení k webu, volání `MigrateCart` metoda budou provedeny.

## <a name="migrating-the-shopping-cart"></a>Migrace nákupní košík

Teď, když máte proces přihlášení a registrace aktualizované, můžete přidat kód pro migraci nákupní košík pomocí `MigrateCart` metoda.

1. V **Průzkumníku řešení**, Najít *logiku* složky a otevřete *ShoppingCartActions.cs* soubor třídy.
2. Přidejte kód zvýrazněných v žlutý existující kód v *ShoppingCartActions.cs* souboru, tak, aby kód *ShoppingCartActions.cs* soubor vypadat takto:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart` Metoda používá existující cartId k nalezení nákupní košík uživatele. V dalším kroku kód projde všechny nákupní košík položky a nahradí `CartId` vlastnost (zadané `CartItem` schématu) s názvem přihlášeného uživatele.

### <a name="updating-the-database-connection"></a>Aktualizace připojení k databázi

Použijete-li tento kurz pomocí **předem** adresář Wingtip Toys ukázkové aplikace, musíte znovu vytvořit výchozí databáze členství. Změnou výchozí připojovací řetězec databáze členství vytvoří se při příštím spuštění aplikace.

1. Otevřete *Web.config* souboru v kořenovém adresáři projektu.
2. Výchozí připojovací řetězec aktualizujte tak, aby se následujícím způsobem:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrace služby PayPal

PayPal je založené na webu fakturace platforma, která přijímá plateb podle online obchodníků. V tomto kurzu dále vysvětluje, jak integrovat funkce Express najdete v článku věnovaném PayPal pro do vaší aplikace. Rychlé Checkout umožňuje zákazníkům použít PayPal platit pro položky, které jste přidali k jejich nákupní košík.

### <a name="create-paylpal-test-accounts"></a>Vytvoření účtů PaylPal testu

Použít PayPal testovacím prostředí, musíte vytvořit a ověřit test vývojářského účtu. Testovací účet vývojáře použije k vytvoření kupujících testovací účet a testovací účet prodejce. Přihlašovací údaje vývojář testovací účtu také umožní adresář Wingtip Toys ukázkovou aplikaci pro přístup k testovacím prostředí PayPal.

1. V prohlížeči přejděte na vývojáře PayPal testování webu:   
    [https://Developer.PayPal.com](https://developer.paypal.com/)
2. Pokud nemáte účet služby PayPal pro vývojáře, vytvořit nový účet kliknutím na tlačítko **zaregistrovat**a následující kroky registrace. Pokud máte stávající účet vývojáře PayPal, přihlaste se kliknutím na **protokolu v**. Budete potřebovat vývojářského účtu PayPal otestovat vzorovou aplikaci adresář Wingtip Toys později v tomto kurzu.
3. Pokud jste právě zaregistrovali do služby pro vývojářského účtu PayPal, musíte ověřit vývojářského účtu PayPal s PayPal. Pomocí následujících kroků, které PayPal posílat e-mailový účet, můžete ověřit váš účet. Jakmile si ověříte vývojářského účtu PayPal, přihlaste se zpět k vývojáře PayPal testování webu.
4. Poté, co jste přihlášení k webu pro vývojáře PayPal s vývojářský účet služby PayPal, že potřebujete vytvořit účet služby PayPal kupujících testu, pokud to neuděláte již mít jeden. Chcete vytvořit účet testovací kupujících na webu služby PayPal, klikněte na tlačítko **aplikace** a pak klikněte **izolovaného prostoru účty**.   
 **Izolovaného testovací účty** stránky se zobrazí.   

    > [!NOTE] 
    > 
    > Web PayPal Developer již poskytuje obchodní testovací účet.

    ![Najdete v článku věnovaném a platbu se PayPal – izolovaného testovací účty](checkout-and-payment-with-paypal/_static/image15.png)
5. Na stránce izolovaného testovací účty, klikněte na **vytvořit účet**.
6. Na **vytvořit testovací účet** možnost kupující zkušební účet e-mail a heslo podle svého výběru.   

    > [!NOTE] 
    > 
    > Budete potřebovat kupujících e-mailové adresy a hesla k testování adresář Wingtip Toys ukázkovou aplikaci na konci tohoto kurzu.

    ![Najdete v článku věnovaném a platbu se PayPal – izolovaného testovací účty](checkout-and-payment-with-paypal/_static/image16.png)
7. Kliknutím na vytvořit testovací účet kupujících **vytvořit účet** tlačítko.  
 **Izolovaného testovací účty** zobrazí se stránka. 

    ![Najdete v článku věnovaném a platbu se PayPal – PaylPal účty](checkout-and-payment-with-paypal/_static/image17.png)
8. Na **izolovaného testovací účty** klikněte na tlačítko **zprostředkovatel** e-mailový účet.  
    **Profil** a **oznámení** zobrazí možnosti.
9. Vyberte **profil** možnost a potom klikněte na **rozhraní API pověření** zobrazíte vaše přihlašovací údaje rozhraní API pro obchodní testovací účet.
10. TEST API přihlašovací údaje zkopírujte do poznámkového bloku.

Zobrazené klasické rozhraní API testu pověření (uživatelské jméno, heslo a podpis) provádět volání rozhraní API z ukázkové aplikace adresář Wingtip Toys budete muset PayPal testovacím prostředí. V dalším kroku přidáte přihlašovací údaje.

### <a name="add-paypal-class-and-api-credentials"></a>Přidat PayPal třídy a rozhraní API pověření

Umístíte většinu kódu PayPal do jediné třídy. Tato třída obsahuje metody, používaný ke komunikaci s PayPal. Navíc přidáte přihlašovacích údajů služby PayPal pro tuto třídu.

1. V adresář Wingtip Toys ukázkovou aplikaci v sadě Visual Studio, klikněte pravým tlačítkem **logiku** složku a potom vyberte **přidat**  - &gt; **novou položku**.   
 **Přidat novou položku** se zobrazí dialogové okno.
2. V části **Visual C#** z **nainstalovaná** podokna na levé straně vyberte **kód**.
3. V prostředním podokně, vyberte **třída**. Název tato nová třída **PayPalFunctions.cs**.
4. Klikněte na tlačítko **přidat**.  
 Nový soubor třídy se zobrazí v editoru.
5. Ve výchozím kódu nahraďte následujícím kódem:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Přidejte obchodní API pověření (uživatelské jméno, heslo a podpis), zobrazené v tomto kurzu tak, aby mohla provést volání funkce PayPal testovacího prostředí.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> V této ukázkové aplikaci jednoduše přidáváte přihlašovací údaje do souboru C# (.cs). Ale v implementované řešení, měli byste zvážit šifrování přihlašovacích údajů v konfiguračním souboru.


Třída NVPAPICaller obsahuje většinu funkcí služby PayPal. Kód ve třídě, poskytuje metody, třeba změnit testu zakoupit od PayPal testovacího prostředí. Následující tři služby PayPal funkce slouží k nákupech:

- `SetExpressCheckout`funkce
- `GetExpressCheckoutDetails`funkce
- `DoExpressCheckoutPayment`funkce

`ShortcutExpressCheckout` Metoda shromažďuje testovací informace a produktu podrobnosti o nákupu z nákupní košík a volání `SetExpressCheckout` funkce PayPal. `GetCheckoutDetails` Metoda potvrdí podrobnosti o nákupu a volání `GetExpressCheckoutDetails` PayPal funkce před provedením testovací nákupu. `DoCheckoutPayment` Metoda dokončení nákupu testů z testovacího prostředí voláním `DoExpressCheckoutPayment` funkce PayPal. Zbývající kód podporuje PayPal metody a procesu, jako je například kódování řetězce, dekódování řetězců, zpracování pole a určit, přihlašovací údaje.

> [!NOTE] 
> 
> PayPal umožňuje zahrnout podrobnosti o volitelné nákupu na základě [specifikace rozhraní API služby PayPal pro](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Tím, že rozšíří kód na adresář Wingtip Toys ukázkovou aplikaci, můžete zahrnout lokalizace podrobnosti, popisy produktu, daň, služba číslo zákazníka, jakož i další volitelné pole.


Všimněte si, že vrátit a zrušit adresy URL, které jsou určené v **ShortcutExpressCheckout** metoda používat číslo portu.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Při aplikaci Visual Web Developer spuštění webového projektu pomocí protokolu SSL, běžně port 44300 se používá pro webový server. Jako v příkladu nahoře, číslo portu je 44300. Při spuštění aplikace zobrazí jiné číslo portu. Port číslo musí být správně nastavena v kódu, aby bylo možné úspěšně spouštět ukázkovou aplikaci adresář Wingtip Toys na konci tohoto kurzu. V další části tohoto kurzu vysvětluje, jak načíst číslo portu místního hostitele a aktualizace třídy PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aktualizovat číslo portu LocalHost ve třídě PayPal

Ukázkovou aplikaci adresář Wingtip Toys uzavře produkty tak, že přejdete na web služby PayPal testování a návrat k místní instanci ukázkové aplikace adresář Wingtip Toys. Aby bylo možné používat PayPal vrátit na správnou adresu URL, je třeba zadat číslo portu místně spuštění ukázkové aplikace v kódu PayPal uvedených výše.

1. Klikněte pravým tlačítkem na název projektu (**Northwind**) v **Průzkumníku řešení** a vyberte **vlastnosti**.
2. V levém sloupci, vyberte **webové** kartě.
3. Načtení číslo portu od **adresa Url projektu** pole.
4. V případě potřeby aktualizovat `returnURL` a `cancelURL` ve třídě, PayPal (`NVPAPICaller`) v *PayPalFunctions.cs* souboru číslo portu webové aplikace:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Kód, který jste přidali teď bude odpovídat očekávané port pro místní webové aplikace. PayPal bude moct vrátit na správnou adresu URL na místním počítači.

### <a name="add-the-paypal-checkout-button"></a>Přidat tlačítko PayPal ověření

Teď, když primární funkce PayPal byly přidány k ukázkové aplikaci, můžete začít přidávat značek a kódu potřebných k volání tyto funkce. Nejprve je nutné přidat tlačítko ověření, který uživatel uvidí na stránce nákupní košík.

1. Otevřete *ShoppingCart.aspx* souboru.
2. Posuňte se do spodní části souboru a najít `<!--Checkout Placeholder -->` komentář.
3. Nahraďte komentář s `ImageButton` řídit tak, aby označit až je nahrazena následujícím způsobem:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. V *ShoppingCart.aspx.cs* souboru po `UpdateBtn_Click` obslužné rutiny události u konce souboru, přidejte `CheckOutBtn_Click` obslužné rutiny události:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Také v *ShoppingCart.aspx.cs* soubor, přidejte odkaz na `CheckoutBtn`tak, aby tlačítko Nový image odkazuje následujícím způsobem:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Uložte změny do obou *ShoppingCart.aspx* souboru a *ShoppingCart.aspx.cs* souboru.
7. V nabídce vyberte **ladění**-&gt;**sestavení Northwind**.  
 Projekt bude znovu vytvořen s nově přidaný **ImageButton** ovládacího prvku.

### <a name="send-purchase-details-to-paypal"></a>Poslat podrobnosti o nákupu PayPal

Když uživatel klikne **najdete v článku věnovaném** tlačítko na stránce nákupní košík (*ShoppingCart.aspx*), budou brzy procesu nákupu. Následující kód zavolá funkci první PayPal potřebné k nákupu produkty.

1. Z *najdete v článku věnovaném* složku, otevřete soubor modelu code-behind s názvem *CheckoutStart.aspx.cs*.   
 Ujistěte se, že otevřete soubor kódu.
2. Nahraďte stávající kód s následujícími službami:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Když uživatel aplikace klepne **najdete v článku věnovaném** bude tlačítko na nákupní košík stránce v prohlížeči přejděte na *CheckoutStart.aspx* stránky. Když *CheckoutStart.aspx* stránky zatížení, `ShortcutExpressCheckout` metoda je volána. Uživatel se v tomto okamžiku přenese na testování webu PayPal. Na webu služby PayPal uživatel zadá své přihlašovací údaje služby PayPal, zkontroluje podrobnosti o nákupu, přijímá PayPal smlouvy a vrátí na adresář Wingtip Toys ukázkovou aplikaci kde `ShortcutExpressCheckout` metoda dokončí. Při `ShortcutExpressCheckout` je metoda dokončena, přesměruje uživatele *CheckoutReview.aspx* stránku zadanou v `ShortcutExpressCheckout` metoda. To umožňuje uživateli zkontrolujte podrobnosti o pořadí z v rámci ukázkové aplikace adresář Wingtip Toys.

### <a name="review-order-details"></a>Zkontrolujte podrobnosti o objednávce

Po návratu z PayPal, *CheckoutReview.aspx* stránky adresář Wingtip Toys ukázkové aplikace zobrazí podrobnosti o pořadí. Tato stránka umožňuje uživatelům zkontrolujte podrobnosti o pořadí před nákupem produkty. *CheckoutReview.aspx* stránky musí být vytvořeny následujícím způsobem:

1. V *najdete v článku věnovaném* složku, otevřete stránku s názvem *CheckoutReview.aspx*.
2. Nahraďte stávající značky s následujícími službami:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Otevřít stránku kódu s názvem *CheckoutReview.aspx.cs* a existujícího kódu nahraďte následujícím kódem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView** řízení slouží k zobrazení podrobností o pořadí, které bylo vrácené PayPal. Také ve výše uvedeném kódu uloží pořadí podrobnosti do databáze adresář Wingtip Toys jako `OrderDetail` objektu. Když uživatel klikne na **dokončení pořadí** tlačítka, bude přesměrován na *CheckoutComplete.aspx* stránky.

> [!NOTE] 
> 
> **Tip**
> 
> Ve značkách *CheckoutReview.aspx* stránky, Všimněte si, že `<ItemStyle>` značky je lze změnit styl položky v rámci **DetailsView** ovládací prvek v dolní části stránky. Pomocí zobrazení stránky v **zobrazení návrhu** (výběrem **návrhu** v levém dolním rohu Visual Studio), pak výběrem **DetailsView** řízení a výběr  **Inteligentní značky** (na ikonu šipky v horní části napravo od ovládacího prvku), nebudete moci zobrazit **Úkoly DetailsView**.
> 
> ![Najdete v článku věnovaném a platbu se PayPal – upravit pole](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Výběrem **upravit pole**, **pole** se zobrazí dialogové okno. V tomto dialogovém jednoduše můžete kontrolovat vizuálních vlastností, jako například **ItemStyle**, z **DetailsView** ovládacího prvku.
> 
> ![Najdete v článku věnovaném a platbu se PayPal – dialogové okno pole](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Dokončení nákupu

*CheckoutComplete.aspx* stránky umožňuje nákupu od společnosti PayPal. Jak je uvedeno nahoře, uživatel musí kliknout na **dokončení pořadí** tlačítko předtím, než bude aplikace přejděte do *CheckoutComplete.aspx* stránky.

1. V *najdete v článku věnovaném* složku, otevřete stránku s názvem *CheckoutComplete.aspx*.
2. Nahraďte stávající značky s následujícími službami:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Otevřít stránku kódu s názvem *CheckoutComplete.aspx.cs* a existujícího kódu nahraďte následujícím kódem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Když *CheckoutComplete.aspx* načtení stránky `DoCheckoutPayment` metoda je volána. Jak už bylo zmíněno dříve, `DoCheckoutPayment` Metoda dokončení nákupu z testovacího prostředí PayPal. Po dokončení nákupu pořadí, PayPal *CheckoutComplete.aspx* stránka zobrazuje platební transakce `ID` odběrateli.

### <a name="handle-cancel-purchase"></a>Popisovač Storno nákupu

Pokud se uživatel rozhodne zrušit nákup, budete přesměrováni na *CheckoutCancel.aspx* stránky, kde uvidí, že byla zrušena jejich pořadí.

1. Otevřete stránku s názvem *CheckoutCancel.aspx* v *najdete v článku věnovaném* složky.
2. Nahraďte stávající značky s následujícími službami:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Zpracování chyb nákupu

Zpracování chyb během procesu nákupu pomocí *CheckoutError.aspx* stránky. Kódu z *CheckoutStart.aspx* stránky, *CheckoutReview.aspx* stránky a *CheckoutComplete.aspx* stránka bude každý přesměrovat  *CheckoutError.aspx* stránky, pokud dojde k chybě.

1. Otevřete stránku s názvem *CheckoutError.aspx* v *najdete v článku věnovaném* složky.
2. Nahraďte stávající značky s následujícími službami:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx* stránky se zobrazí podrobnosti o chybě, když dojde k chybě během procesu najdete v článku věnovaném.

## <a name="running-the-application"></a>Spuštění aplikace

Spusťte aplikaci najdete v tom, jak zakoupit produkty. Všimněte si, že budete používat v PayPal testovacím prostředí. Žádný skutečný peníze se vyměňují.

1. Zajistěte, aby všechny vaše soubory jsou uloženy v sadě Visual Studio.
2. Otevřete webový prohlížeč a přejděte do [https://developer.paypal.com](https://developer.paypal.com/).
3. Přihlášení pomocí vývojářského účtu PayPal, kterou jste vytvořili dříve v tomto kurzu.  
 Pro izolovaný prostor PayPal pro vývojáře, musíte být přihlášení v [https://developer.paypal.com](https://developer.paypal.com/) k testování express checkout. To platí jenom pro společnosti PayPal izolovaného prostoru testování nechcete za provozu prostředí společnosti PayPal.
4. V sadě Visual Studio, stiskněte klávesu **F5** ke spuštění ukázkové aplikace adresář Wingtip Toys.  
 Po znovu sestaví databáze, bude v prohlížeči otevřít a zobrazit *Default.aspx* stránky.
5. Přidejte tři různé produkty do nákupního košíku výběrem kategorie produktů, jako je například "Aut" a potom kliknutím na **přidat do košíku** vedle každého produktu.  
 Nákupní košík se zobrazí na produkt, který jste vybrali.
6. Klikněte **PayPal** tlačítko checkout. 

    ![Najdete v článku věnovaném a platbu se PayPal – košíku](checkout-and-payment-with-paypal/_static/image20.png)

 Rezervování bude vyžadovat, že máte účet uživatele pro ukázkovou aplikaci adresář Wingtip Toys.
7. Klikněte **Google** odkaz na pravé straně stránky se přihlásit existující e-mailový účet gmail.com.  
 Pokud nemáte účet gmail.com, můžete vytvořit jeden pro testování v [www.gmail.com](https://www.gmail.com/). Můžete také standardní místní účet kliknutím na tlačítko "Register". 

    ![Najdete v článku věnovaném a platbu se PayPal – přihlášení](checkout-and-payment-with-paypal/_static/image21.png)
8. Přihlaste se pomocí vaší gmail účet a heslo. 

    ![Najdete v článku věnovaném a platbu se PayPal – Gmail přihlášení](checkout-and-payment-with-paypal/_static/image22.png)
9. Klikněte na tlačítko **přihlásit** tlačítko zaregistrovat adresář Wingtip Toys ukázkové aplikace uživatelské jméno účtu z gmailu. 

    ![Najdete v článku věnovaném a platbu se PayPal – registrace účtu](checkout-and-payment-with-paypal/_static/image23.png)
10. Na test webu PayPal přidat vaše **kupující** e-mailovou adresu a heslo, které jste vytvořili dříve v tomto kurzu a pak klikněte na **protokolu v** tlačítko. 

    ![Najdete v článku věnovaném a platbu se PayPal – PayPal přihlášení](checkout-and-payment-with-paypal/_static/image24.png)
11. Souhlas s PayPal zásad a klikněte na **Agree a pokračovat** tlačítko.  
 Všimněte si, že je tato stránka jenom zobrazí při prvním použití tohoto účtu PayPal. Znovu Všimněte si, že toto je účet test se vyměňují žádné skutečné peníze. 

    ![Najdete v článku věnovaném a platbu se PayPal – PayPal zásad](checkout-and-payment-with-paypal/_static/image25.png)
12. Zkontrolujte informace o objednávce na PayPal testování prostředí kontrolní stránku a klikněte na tlačítko **pokračovat**. 

    ![Najdete v článku věnovaném a platbu se PayPal – kontrola informací](checkout-and-payment-with-paypal/_static/image26.png)
13. Na *CheckoutReview.aspx* zkontrolujte velikost pořadí a zobrazit adresu generovaného přesouvání. Potom klikněte **dokončení pořadí** tlačítko. 

    ![Najdete v článku věnovaném a platbu se PayPal – Zkontrolujte pořadí](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx** se zobrazí stránka s ID platebních transakcí. 

    ![Najdete v článku věnovaném a platbu se PayPal – najdete v článku věnovaném dokončení](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Kontrola databáze

Kontrolou aktualizovaná data v databázi adresář Wingtip Toys ukázkové aplikace po spuštění aplikace zobrazí se aplikace úspěšně zaznamenávají nákupu produkty.

Si můžete prohlédnout data obsažená v *Wingtiptoys.mdf* soubor databáze pomocí **Průzkumník databáze** okno (**Průzkumníka serveru** oken v sadě Visual Studio) jako jste to udělali v tento kurz řady.

1. Zavřete okno prohlížeče, pokud je stále otevřen.
2. V sadě Visual Studio, vyberte **zobrazit všechny soubory** v horní části ikonu **Průzkumníku řešení** umožňují rozšířit **aplikace\_Data** složky.
3. Rozbalte **aplikace\_Data** složky.  
 Je nutné vybrat **zobrazit všechny soubory** ikonu složky.
4. Klikněte pravým tlačítkem myši *Wingtiptoys.mdf* souboru databáze a vyberte **otevřete**.  
    **V Průzkumníku serveru** se zobrazí.
5. Rozbalte **tabulky** složky.
6. Klikněte pravým tlačítkem myši **objednávky**tabulky a vyberte **zobrazit Data tabulky**.  
 **Objednávky** tabulka se zobrazí.
7. Zkontrolujte **PaymentTransactionID** sloupec pro potvrzení úspěšné transakce. 

    ![Najdete v článku věnovaném a platbu se PayPal – zkontrolujte databáze](checkout-and-payment-with-paypal/_static/image29.png)
8. Zavřít **objednávky** okno tabulky.
9. V Průzkumníku serveru, klikněte pravým tlačítkem myši **Rozpis objednávek** tabulky a vyberte **zobrazit Data tabulky**.
10. Zkontrolujte `OrderId` a `Username` hodnoty ve **Rozpis objednávek** tabulky. Všimněte si, že tyto hodnoty odpovídat `OrderId` a `Username` hodnoty, které jsou součástí **objednávky** tabulky.
11. Zavřít **Rozpis objednávek** okno tabulky.
12. Klikněte pravým tlačítkem na soubor databáze adresář Wingtip Toys (*Wingtiptoys.mdf*) a vyberte **ukončení připojení**.
13. Pokud se nezobrazí **Průzkumníku řešení** okně klikněte na tlačítko **Průzkumníku řešení** v dolní části **Průzkumníka serveru** okno zobrazíte **Průzkumníku řešení**  znovu.

## <a name="summary"></a>Souhrn

V tomto kurzu jste přidali pořadí a schémata podrobností pořadí sledování nákup produktů. Také integrované funkce služby PayPal na adresář Wingtip Toys ukázkovou aplikaci.

## <a name="additional-resources"></a>Další prostředky

[Přehled konfigurace technologie ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Nasazení aplikace zabezpečení rozhraní ASP.NET Web Forms členství, OAuth a databáze SQL Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Právní omezení

Tento kurz obsahuje ukázkový kód. Takové ukázkový kód je poskytován "tak, jak je" bez záruky jakéhokoli druhu. Podle toho společnost Microsoft nezaručuje, přesnost, integritu nebo kvalitu ukázkový kód. Souhlasíte s tím, že používáte ukázkový kód na vlastní nebezpečí. Za žádných okolností bude Microsoft za vám žádným způsobem žádné ukázkový kód, obsah, včetně, ale nikoli výhradně, všechny chyby nebo opomenutí v jakékoli ukázkový kód, obsah, nebo ke ztrátě nebo poškození jakéhokoli druhu vzniklých v důsledku použití jakékoli ukázkový kód. Tímto upozorněni a souhlasí s tím zbavuje odpovědnosti, uložit a uložení neškodné z a proti ztrátě všechny deklarace identity ke ztrátě, poškození nebo poškození jakéhokoli druhu včetně bez omezení, ty způsobené nebo vzniklých materiál, který účtování, Microsoft přenášet, použijte nebo spoléhají na mimo jiné včetně názory vyjádřené v něm.

>[!div class="step-by-step"]
[Předchozí](shopping-cart.md)
[další](membership-and-administration.md)
