---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Pokladna a platba přes PayPal | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: b59a395e255823a732aef1b899612063e09b2424
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753219"
---
<a name="checkout-and-payment-with-paypal"></a>Pokladna a platba přes PayPal
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


Tento kurz popisuje, jak upravit ukázkové aplikace Wingtip Toys autorizace uživatelů, registrace a platba pomocí PayPal. Jenom uživatelé, kteří jsou přihlášení bude mít autorizaci k nákupu produktů. Šablona projektu webových formulářů ASP.NET 4.5 předdefinované uživatelské registraci funkce již obsahuje velkou část, co potřebujete. Přidejte funkcemi při placení Express PayPal. V tomto kurzu budete používat PayPal pro vývojáře, testovací prostředí, takže se nepřenesou žádná skutečná fondů. Na konci tohoto kurzu budete testovat aplikaci tak, že vyberete produktů pro přidání do nákupního košíku, kliknutím na tlačítko Pokladna a přenosu dat do testování webu PayPal. Na webové stránce testování PayPal bude potvrzení dopravě a platebních údajů a pak se vraťte do místní ukázkové aplikace Wingtip Toys pro potvrzení a dokončení nákupu.

Existuje několik procesorů zkušení plateb, kteří se specializují v online nakupování tuto adresu škálovatelnost a zabezpečení. Vývoj v ASP.NET zvažte výhody využití platebních řešení třetí strany před implementací nákupního a zakoupení řešení.

> [!NOTE] 
> 
> Ukázkové aplikace Wingtip Toys je navržená k zobrazí konkrétní technologie ASP.NET konceptů a funkcí, které jsou k dispozici pro webové vývojáře využívající technologii ASP.NET. Tato ukázková aplikace nebyl optimalizován pro všechny možné okolnosti, škálovatelnost a zabezpečení.


## <a name="what-youll-learn"></a>Co se dozvíte:

- Jak omezit přístup na konkrétní stránky ve složce.
- Jak vytvořit známé nákupního košíku z anonymního nákupního košíku.
- Postup povolení SSL pro projekt.
- Jak do projektu přidat poskytovatele OAuth.
- Jak používat PayPal zakoupit produkty prostřednictvím služby PayPal testovací prostředí.
- Jak zobrazit podrobnosti o PayPal v **DetailsView** ovládacího prvku.
- Postup aktualizace databáze aplikace Wingtip Toys s podrobnostmi o získaných PayPal.

## <a name="adding-order-tracking"></a>Přidání sledování pořadí

V tomto kurzu vytvoříte dva nové třídy ke sledování dat v pořadí, ve kterém uživatel vytvořil. Třídy bude sledovat data týkající se informace o odeslání, celkem nákupu a potvrzení platby.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Přidání třídy modelu OrderDetail a pořadí

Dříve v této řadě kurzů definice schématu pro kategorii, které produkty, a tím, že vytvoříte položky nákupního košíku `Category`, `Product`, a `CartItem` tříd v *modely* složky. Nyní přidejte dva nové třídy pro definování schématu pro objednávky produktů a podrobnosti objednávky.

1. V **modely** složky, přidejte novou třídu s názvem *Order.cs*.   
   Nový soubor třídy se zobrazí v editoru.
2. Nahraďte kód následujícím:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Přidat *OrderDetail.cs* třídu *modely* složky.
4. Nahraďte kód následujícím kódem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order` a `OrderDetail` třídy obsahuje schéma pro definování pořadí informace použité k nákupu a příjemce.

Kromě toho je potřeba aktualizovat třídy kontextu databáze, která spravuje tříd entit, který poskytuje přístup k datům v databázi. K tomuto účelu přidá nově vytvořeného pořadí a `OrderDetail` model třídy, které `ProductContext` třídy.

1. V **Průzkumníka řešení**, najít a otevřít *ProductContext.cs* souboru.
2. Přidejte zvýrazněný kód, který *ProductContext.cs* sdílené, jak je znázorněno níže:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Jak už bylo zmíněno dříve v této řadě kurzů, kód v *ProductContext.cs* přidá soubor `System.Data.Entity` obor názvů, abyste měli přístup ke všem funkcím základní rozhraní Entity Framework. Tato funkce zahrnuje možnost pro dotazování, vložení, aktualizace a odstranění dat při práci s objektů se silným typem. Ve výše uvedeném kódu `ProductContext` třídy přidá Entity Framework přístup do nově přidaný `Order` a `OrderDetail` třídy.

## <a name="adding-checkout-access"></a>Přidání ověření přístupu

Ukázkové aplikace Wingtip Toys umožňuje anonymním uživatelům ke kontrole a přidejte produkty do nákupního košíku. Ale při anonymním uživatelům vybrat nákup produktů, které se budou přidávat do nákupního košíku, se musí přihlásit k webu. Jakmile jste přihlášeni, bude moct stránky s omezeným přístupem webové aplikace, které zpracovávají rezervace a proces nákupu. Tyto stránky s omezeným přístupem, které jsou obsaženy v *Checkout* složky aplikace.

### <a name="add-a-checkout-folder-and-pages"></a>Přidat složku rezervovat a stránky

Teď vytvoříte *Checkout* složku a stránky, které se zobrazí zákazník během procesu registrace. Později v tomto kurzu budete aktualizovat tyto stránky.

1. Klikněte pravým tlačítkem na název projektu (**Wingtip Toys**) v **Průzkumníka řešení** a vyberte **přidat novou složku**. 

    ![Pokladna a platba přes PayPal – nová složka](checkout-and-payment-with-paypal/_static/image1.png)
2. Název nové složky *Checkout*.
3. Klikněte pravým tlačítkem myši *Checkout* složku a pak vyberte **přidat**-&gt;**nová položka**. 

    ![Pokladna a platba přes PayPal – nová položka](checkout-and-payment-with-paypal/_static/image2.png)
4. **Přidat novou položku** se zobrazí dialogové okno.
5. Vyberte **Visual C#**  - &gt; **webové** šablony skupiny na levé straně. Potom v prostředním podokně vyberte **webové formuláře se stránkou předlohy**a pojmenujte ho *CheckoutStart.aspx*. 

    ![Pokladna a platba přes PayPal – přidání nové položky dialogového okna](checkout-and-payment-with-paypal/_static/image3.png)
6. Stejně jako předtím, vyberte *Site.Master* souboru stránky předlohy.
7. Přidejte následující další stránky, které *Checkout* složky pomocí stejného postupu výše:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Přidání souboru Web.config

Přidáním nového *Web.config* do souboru *Checkout* složky, budete moct omezit přístup na všechny stránky, které jsou obsaženy ve složce.

1. Klikněte pravým tlačítkem myši *Checkout* a pak zvolte položku **přidat**  - &gt; **nová položka**.  
   **Přidat novou položku** se zobrazí dialogové okno.
2. Vyberte **Visual C#**  - &gt; **webové** šablony skupiny na levé straně. Potom v prostředním podokně vyberte **souboru konfigurace webu**, přijměte výchozí název *Web.config*a pak vyberte **přidat**.
3. Nahradit existující obsah v XML *Web.config* souboru následujícím kódem:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Uložit *Web.config* souboru.

*Web.config* soubor Určuje, že všichni uživatelé Neznámý webové aplikace musí odepřen přístup k stránky součástí *Checkout* složky. Však pokud uživatel nemá zaregistrovaný účet a je přihlášen, bude uživatel známé a bude mít přístup ke stránkám *Checkout* složky.

Je důležité si uvědomit, že konfigurace technologie ASP.NET následující hierarchie, kde každý *Web.config* souboru aplikuje nastavení konfigurace ve složce, která je v a na všechny podřízené adresáře pod ní.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Povolit protokol SSL v projektu

 Vrstva SSL (Secure Sockets) je protokol definované umožňující webové servery a webovým klientům bezpečněji komunikovat pomocí šifrování. Když se používá protokol SSL, je otevřená pro paketů pro analýzu sítě všichni mají fyzický přístup k síti data odesílaná mezi klientem a serverem. Kromě toho několik společných schémat ověřování nejsou zabezpečené přes standardní protokol HTTP. Konkrétně se základní ověřování a ověřování pomocí formulářů odesílat nezašifrované přihlašovací údaje. Aby bylo zabezpečené, musí tato schémata ověřování používat protokol SSL. 

1. V **Průzkumníka řešení**, klikněte na tlačítko **Northwind** projektu a potom stiskněte klávesu **F4** zobrazíte **vlastnosti** okna.
2. Změna **protokol SSL povolený** k `true`.
3. Kopírovat **adresa URL protokolu SSL** abyste ji mohli použít později.   
 Adresa URL protokolu SSL budou `https://localhost:44300/` Pokud jste předtím vytvořili SSL webové stránky (jak je vidět níže).   
    ![Vlastnosti projektu](checkout-and-payment-with-paypal/_static/image4.png)
4. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **Northwind** projektu a klikněte na tlačítko **vlastnosti**.
5. Na levé kartě klikněte **webové**.
6. Změnit **adresa Url projektu** používat **adresa URL protokolu SSL** , který jste předtím uložili.   
    ![Vlastnosti webového projektu](checkout-and-payment-with-paypal/_static/image5.png)
7. Uložit na stránku stisknutím kombinace kláves **CTRL + S**.
8. Stisknutím klávesy **Ctrl + F5** ke spuštění aplikace. Visual Studio se zobrazí možnost umožňují zabránit zobrazování upozornění protokolu SSL.
9. Klikněte na tlačítko **Ano** důvěřovat certifikátu SSL služby IIS Express a pokračujte.   
    ![Podrobnosti o certifikátu SSL služby IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Zobrazí se upozornění zabezpečení.
10. Klikněte na tlačítko **Ano** k instalaci certifikátu do vašeho místního hostitele.   
    ![Dialogové okno upozornění zabezpečení](checkout-and-payment-with-paypal/_static/image7.png)  
 Zobrazí se okno prohlížeče.

Teď můžete snadno testujte webové aplikace místně s použitím SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Přidání poskytovatele OAuth 2.0

Webové formuláře ASP.NET poskytuje rozšířené možnosti pro členství a ověřování. Patří mezi ně OAuth. OAuth je otevřený protokol, který umožňuje zabezpečené autorizace v jednoduché a standardní metody z webových, mobilních a desktopových aplikací. Šablony webových formulářů ASP.NET používá k vystavení Facebook, Twitter, Google a Microsoft jako zprostředkovatele ověřování OAuth. I když v tomto kurzu používá jako zprostředkovatel ověřování Google pouze, můžete snadno upravit kód, který použije některý z zprostředkovatele. Postup implementace jiných poskytovatelů služeb jsou velmi podobné kroky, které se zobrazí v tomto kurzu.

Kromě ověřování kurzu také použije role pro implementaci autorizace. Jenom uživatelé, je přidat do `canEdit` role budou moci měnit data (vytvoření, úprava nebo odstranění kontaktů).

> [!NOTE] 
> 
> Aplikace Windows Live přijímat pouze za provozu adresu URL pro webovou stránku, funkční, tak adresy URL místního webu nelze použít pro testování přihlášení.


Následující postup vám umožní přidat zprostředkovatele ověřování Google.

1. Otevřít *aplikace\_Start\Startup.Auth.cs* souboru.
2. Odebere znaky komentáře z `app.UseGoogleAuthentication()` metodu tak, aby metoda se zobrazí jako následující: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Přejděte [konzole pro vývojáře Google](https://console.developers.google.com/). Je také potřeba přihlášení pomocí účtu Google developer e-mailu (gmail.com). Pokud nemáte účet Google, vyberte **vytvořit účet** odkaz.   
   V dalším kroku se zobrazí **konzole pro vývojáře Google**.   
    ![Konzole pro vývojáře Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Klikněte na tlačítko **vytvořit projekt** tlačítko a zadejte název projektu a ID (můžete použít výchozí hodnoty). Klikněte **zaškrtávací políčko smlouvy** a **vytvořit** tlačítko.  

    ![Google - nový projekt](checkout-and-payment-with-paypal/_static/image9.png)

   Během několika sekund se vytvoří nový projekt a prohlížeči se zobrazí nová stránka projekty.
5. Na levé kartě klikněte **rozhraní API &amp; auth**a potom klikněte na tlačítko **přihlašovací údaje**.
6. Klikněte na tlačítko **vytvořit nové ID klienta** pod **OAuth**.   
   **Vytvořit ID klienta** zobrazí se dialogové okno.   
    ![Google - vytvořit ID klienta](checkout-and-payment-with-paypal/_static/image10.png)
7. V **vytvořit ID klienta** dialogového okna, ponechte výchozí **webovou aplikaci** pro typ aplikace.
8. Nastavte **oprávnění zdrojů JavaScript** odeslané na adresu SSL, který jste použili dříve v tomto kurzu (`https://localhost:44300/` Pokud jste vytvořili ostatní projekty SSL).   
   Tato adresa URL je zdrojem pro vaši aplikaci. V tomto příkladu zadáte pouze testovací adresa URL místního hostitele. Ale můžete zadat více adres URL, aby se zohlednily localhost a produkčním prostředí.
9. Nastavte **oprávnění identifikátor URI pro přesměrování** takto: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Tato hodnota je identifikátor URI tohoto ASP.NET OAuth uživatele ke komunikaci se serverem služby google OAuth. Mějte na paměti adresa URL protokolu SSL, které jste použili výše ( `https://localhost:44300/` Pokud jste vytvořili ostatní projekty SSL).
10. Klikněte na tlačítko **vytvořit ID klienta** tlačítko.
11. V nabídce vlevo v konzole pro vývojáře Google, klikněte na tlačítko **obrazovkami pro vyjádření souhlasu** položku nabídky, nastavte vaše e-mailovou adresu a název produktu. Po vyplnění formuláře klikněte na tlačítko **Uložit**.
12. Klikněte na tlačítko **rozhraní API** položku nabídky, přejděte dolů a klikněte **vypnout** vedle **rozhraní API Google +**.   
    Přijetí tato možnost vám umožní rozhraní API Google +.
13. Také musíte aktualizovat **Microsoft.Owin** balíček NuGet do verze 3.0.0.   
    Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** a pak vyberte **spravovat balíčky NuGet pro řešení**.  
    Z **spravovat balíčky NuGet** oken, najít a aktualizovat **Microsoft.Owin** balíček verze 3.0.0.
14. V sadě Visual Studio, aktualizovat `UseGoogleAuthentication` metodu *Startup.Auth.cs* stránky zkopírováním a vložením **ID klienta** a **tajný kód klienta** do metody. **ID klienta** a **tajný kód klienta** hodnoty zobrazené níže jsou uvedeny ukázky a nebude fungovat. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Stisknutím klávesy **CTRL + F5** sestavíte a spustíte aplikaci. Klikněte na tlačítko **přihlášení** odkaz.
16. V části **přihlášení pomocí jiné služby**, klikněte na tlačítko **Google**.  
    ![Přihlásit se](checkout-and-payment-with-paypal/_static/image11.png)
17. Pokud budete muset zadat svoje přihlašovací údaje, budete přesměrováni na webu google, kam zadáte svoje přihlašovací údaje.  
    ![Google - přihlášení](checkout-and-payment-with-paypal/_static/image12.png)
18. Po zadání svých přihlašovacích údajů se výzva k udělení oprávnění k webové aplikaci, kterou jste právě vytvořili.  
    ![Výchozí účet služby v projektu](checkout-and-payment-with-paypal/_static/image13.png)
19. Klikněte na tlačítko **přijmout**. Budete teď přesměrováni zpět **zaregistrovat** stránce **Northwind** aplikace, ve kterém můžete zaregistrovat účet služby Google.  
    ![Registrace s vaším účtem Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Máte možnost změnit název registrace místní e-mailu použitý pro váš účet Gmailu, ale obecně je žádoucí uchovat výchozí e-mailový alias (to znamená, ten, který jste použili pro ověřování). Klikněte na tlačítko **přihlášení** jak je znázorněno výše.

### <a name="modifying-login-functionality"></a>Funkce změny přihlašování

Jak už jsme zmínili v této sérii kurzů většina funkcí registraci uživatelů, zahrnuli do šablony webových formulářů ASP.NET, ve výchozím nastavení. Teď změníte výchozí *Login.aspx* a *Register.aspx* stránky k volání `MigrateCart` metody. `MigrateCart` Metoda přidruží anonymní nákupního košíku nově přihlášeného uživatele. Přidružení uživatele a nákupní košík, bude moci udržovat nákupního košíku uživatele mezi návštěvy ukázkové aplikace Wingtip Toys.

1. V **Průzkumníka řešení**, najít a otevřít *účet* složky.
2. Upravte stránku použití modelu code-behind s názvem *Login.aspx.cs* zahrnout kód zvýrazněné žlutou barvou, tak, aby vypadal takto:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Uložit *Login.aspx.cs* souboru.

Prozatím můžete ignorovat varování, že neexistuje žádná definice pro `MigrateCart` metody. Budete přidávat je o něco později v tomto kurzu.

*Login.aspx.cs* soubor kódu na pozadí podporuje metodu přihlášení. Že se podíváte na stránku Login.aspx, uvidíte, že tato stránka obsahuje tlačítko "Přihlásit", že klikněte na aktivační události `LogIn` obslužné rutiny na modelu code-behind.

Když `Login` metodu na *Login.aspx.cs* nazývá novou instanci třídy nákupní košík s názvem `usersShoppingCart` se vytvoří. ID (GUID) nákupní košík je načten a nastavit na `cartId` proměnné. Pak, bude `MigrateCart` metoda je volána, předávání i `cartId` a jméno přihlášeného uživatele do této metody. Když migrujete nákupního košíku, GUID slouží k identifikaci anonymní nákupního košíku se nahradí uživatelské jméno.

Spolu s prováděním změn *Login.aspx.cs* použití modelu code-behind souboru migrace nákupního košíku, když se uživatel přihlašuje, je také nutné upravit *soubor kódu na pozadí Register.aspx.cs* migrace nákupního košíku Když uživatel vytvoří nový účet a přihlášení.

1. V *účet* složku, otevřete soubor kódu na pozadí s názvem *Register.aspx.cs*.
2. Upravte soubor použití modelu code-behind žlutou barvou, včetně kód tak, aby vypadal takto:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Uložit *Register.aspx.cs* souboru. Zase o ignorovat upozornění `MigrateCart` metody.

Všimněte si, že kód, můžete použít v `CreateUser_Click` obslužná rutina události je velmi podobný kód je použitý v `LogIn` metody. Když uživatel zaregistruje nebo přihlášení k serveru, volání `MigrateCart` metoda pak bude možné.

## <a name="migrating-the-shopping-cart"></a>Migrace nákupního košíku

Teď, když máte přihlášení a registraci proces aktualizace, můžete přidat kód pro migrace nákupního košíku pomocí `MigrateCart` metody.

1. V **Průzkumníku řešení**, vyhledejte *logiky* složky a otevřete *ShoppingCartActions.cs* souboru třídy.
2. Přidejte kód zvýrazněné žlutou barvou pro existující kód ve třídě *ShoppingCartActions.cs* souboru, tak, aby kód v *ShoppingCartActions.cs* souboru se zobrazí takto:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart` Metoda používá existující cartId k nalezení nákupního košíku uživatele. V dalším kroku kód projde všechny nákupního košíku položky a nahradí `CartId` vlastnosti (podle `CartItem` schématu) s názvem přihlášeného uživatele.

### <a name="updating-the-database-connection"></a>Aktualizuje se připojení k databázi

Pokud se po tomto kurzu pomocí **předem připravených** Wingtip Toys ukázkovou aplikaci, musíte znovu vytvořit výchozí databáze členství. Změnou výchozí připojovací řetězec databáze členství vytvoří se při příštím spuštění aplikace.

1. Otevřít *Web.config* souboru v kořenovém adresáři projektu.
2. Výchozí připojovací řetězec aktualizujte tak, aby vypadal takto:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrace služby PayPal

PayPal je webové fakturační platforma, která přijímá platby si obchodníci můžou online. V tomto kurzu potom vysvětluje, jak integrace funkcemi při placení Express PayPal pro do vaší aplikace. Expresní registrace umožňuje vašim zákazníkům využít PayPal k platbám za položky, které jste přidali do svého nákupního košíku.

### <a name="create-paylpal-test-accounts"></a>Vytvoření účtů PaylPal testu

Pokud chcete používat PayPal, testovací prostředí, musíte vytvořit a ověřte testovací účet pro vývojáře. Testovací účet pro vývojáře použije k vytvoření kupující prodejce testu a testovacího účtu. Pověření účtu pro vývojáře testu také umožní ukázkové aplikace Wingtip Toys pro přístup k testovacím prostředí PayPal.

1. V prohlížeči přejděte na PayPal vývojářské testování webu:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Pokud nemáte účet pro vývojáře PayPal, vytvořit nový účet kliknutím **zaregistrovat**a registraci kroky. Pokud máte existující účet pro vývojáře PayPal, přihlásit po klepnutí na **přihlásit**. Váš účet vývojáře PayPal pro testování ukázkové aplikace Wingtip Toys později v tomto kurzu budete potřebovat.
3. Pokud jste si právě zaregistrovali službu pro váš účet vývojáře PayPal, budete muset ověřit váš účet vývojáře PayPal přes PayPal. Pomocí následujících kroků, které PayPal odesílat e-mailový účet můžete ověřit váš účet. Po ověření vašeho účtu vývojáře webu PayPal, přihlaste se zpět PayPal vývojářské testování webu.
4. Poté, co jste se přihlásili k webu pro vývojáře PayPal pomocí účtu PayPal pro vývojáře, že které potřebujete k vytvoření účtu PayPal kupujících test Pokud to neuděláte, již existuje. Chcete vytvořit účet kupujících test na webu PayPal, klikněte na tlačítko **aplikací** kartu a potom klikněte na tlačítko **izolovaného prostoru účty**.   
 **Izolovaného prostoru testovací účty** stránka je zobrazena.   

    > [!NOTE] 
    > 
    > Web PayPal Developer již poskytuje obchodní zkušební účet.

    ![Pokladna a platba přes PayPal – izolovaného prostoru testovací účty](checkout-and-payment-with-paypal/_static/image15.png)
5. Na stránce účty testovací izolovaného prostoru, klikněte na tlačítko **vytvořit účet**.
6. Na **vytvořit zkušební účet** stránky zvolte kupující testovacího účtu e-mailu a hesla podle svého výběru.   

    > [!NOTE] 
    > 
    > Budete potřebovat kupujících e-mailové adresy a hesla k testování ukázkové aplikace Wingtip Toys na konci tohoto kurzu.

    ![Pokladna a platba přes PayPal – izolovaného prostoru testovací účty](checkout-and-payment-with-paypal/_static/image16.png)
7. Vytvořit testovací účet kupujících kliknutím **vytvořit účet** tlačítko.  
 **Izolovaného prostoru testovací účty** zobrazí se stránka. 

    ![Pokladna a platba přes PayPal – PaylPal účty](checkout-and-payment-with-paypal/_static/image17.png)
8. Na **izolovaného prostoru testovací účty** stránky, klikněte na tlačítko **facilitátora** e-mailový účet.  
    **Profil** a **oznámení** možnosti se zobrazí.
9. Vyberte **profilu** možnost a potom klikněte na **API pověření** Chcete-li zobrazit svoje přihlašovací údaje rozhraní API pro obchodní zkušební účet.
10. Kopírování přihlašovacích údajů TEST rozhraní API do poznámkového bloku.

Zobrazené klasického rozhraní API testu pověření (uživatelské jméno, heslo a podpis), k volání rozhraní API z ukázkové aplikace Wingtip Toys budete muset PayPal prostředí pro testování. V dalším kroku přidejte přihlašovací údaje.

### <a name="add-paypal-class-and-api-credentials"></a>Přidat PayPal třídy a rozhraní API pověření

Umístíte většinou PayPal kódu do jedné třídy. Tato třída obsahuje metody slouží ke komunikaci s PayPal. Navíc se do této třídy přidat svoje přihlašovací údaje služby PayPal.

1. V ukázkové aplikaci Wingtip Toys v sadě Visual Studio, klikněte pravým tlačítkem myši **logiky** složku a pak vyberte **přidat**  - &gt; **nová položka**.   
   **Přidat novou položku** se zobrazí dialogové okno.
2. V části **Visual C#** z **nainstalováno** podokna na levé straně vyberte **kód**.
3. V prostředním podokně vyberte **třídy**. Název této nové třídy **PayPalFunctions.cs**.
4. Klikněte na tlačítko **přidat**.  
   Nový soubor třídy se zobrazí v editoru.
5. Nahraďte kód následujícím kódem:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Přidáte přihlašovací údaje obchodníka. rozhraní API (uživatelské jméno, heslo a podpis), které jste dříve v tomto kurzu zobrazí tak, aby mohl provádět volání funkce do testovacího prostředí PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> V této ukázkové aplikaci jednoduše přidáváte přihlašovací údaje do souboru C# (cs). Nicméně v implementované řešení, je vhodné zvážit šifrování svoje přihlašovací údaje v konfiguračním souboru.


Třída NVPAPICaller obsahuje většinu funkčnosti PayPal. Kód ve třídě poskytuje metody, které jsou potřebné k tomu test nakupovat PayPal testovací prostředí. Následující tři služby PayPal funkce se používají k zajištění nákup:

- `SetExpressCheckout` – funkce
- `GetExpressCheckoutDetails` – funkce
- `DoExpressCheckoutPayment` – funkce

`ShortcutExpressCheckout` Metoda shromažďuje Podrobnosti testu nákupní informace a produkt z nákupního košíku a volání `SetExpressCheckout` PayPal funkce. `GetCheckoutDetails` Metoda potvrdí podrobnosti o nákupu a volání `GetExpressCheckoutDetails` PayPal funkce před provedením nákupu testu. `DoCheckoutPayment` Dokončení nákupu testů z testovacího prostředí pomocí volání metody `DoExpressCheckoutPayment` PayPal funkce. Zbývající kód podporuje PayPal metody a proces, jako je například kódování řetězce, dekódování řetězců, zpracování polí a určení přihlašovacích údajů.

> [!NOTE] 
> 
> PayPal umožňuje zahrnout podrobnosti o volitelné nákupu na základě [specifikace rozhraní API služby PayPal pro](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Rozšířením kód v ukázkové aplikaci Wingtip Toys může obsahovat podrobnosti o lokalizaci, popisů produktů, daň, zákaznické číslo služby, jakož i mnoho dalších volitelná pole.


Všimněte si, že vrátit a zrušit adresy URL, které jsou určené v **ShortcutExpressCheckout** metoda používat číslo portu.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Když Visual Web Developer spuštěna webového projektu pomocí protokolu SSL, běžně port 44300 je používán pro webový server. Jak uvádíme výš, je číslo portu 44300. Při spuštění aplikace zobrazí jiné číslo portu. Port číslo musí být správně nastavena v kódu, abyste mohli úspěšně spuštění ukázkové aplikace Wingtip Toys na konci tohoto kurzu. Další části tohoto kurzu vysvětluje, jak načíst číslo portu místního hostitele a aktualizovat třídu PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aktualizovat číslo portu LocalHost ve třídě PayPal

Ukázkové aplikace Wingtip Toys nákup produktů přechodem k testování webu PayPal a vrácení k místní instanci ukázkové aplikace Wingtip Toys. Pokud chcete mít PayPal, vraťte se do správné adresy URL, budete muset zadat číslo portu místně spuštěných ukázkovou aplikaci v kódu PayPal uvedených výše.

1. Klikněte pravým tlačítkem na název projektu (**Northwind**) v **Průzkumníka řešení** a vyberte **vlastnosti**.
2. V levém sloupci, vyberte **webové** kartu.
3. Číslo portu od načtení **adresa Url projektu** pole.
4. V případě potřeby aktualizovat `returnURL` a `cancelURL` ve třídě PayPal (`NVPAPICaller`) v *PayPalFunctions.cs* souboru číslo portu webové aplikace:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Kód, který jste přidali teď bude odpovídat očekávané port pro místní webovou aplikaci. PayPal, budete moct vrátit správnou adresu URL na místním počítači.

### <a name="add-the-paypal-checkout-button"></a>Přidejte tlačítko Pokladna PayPal

Teď, když primární PayPal funkce byly přidány k ukázkové aplikaci, můžete začít, přidání značek a kódu potřebných k volání těchto funkcí. Nejprve je nutné přidat rezervaci tlačítko, které uživateli se zobrazí na stránce nákupní košík.

1. Otevřít *ShoppingCart.aspx* souboru.
2. Posunout na konec souboru a vyhledejte `<!--Checkout Placeholder -->` komentář.
3. Nahraďte komentář `ImageButton` tak, aby se značkou nahoru nahradí následujícím způsobem:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. V *ShoppingCart.aspx.cs* souboru po `UpdateBtn_Click` obslužné rutiny události na konci souboru, přidejte `CheckOutBtn_Click` obslužné rutiny události:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Také v *ShoppingCart.aspx.cs* přidejte odkaz na `CheckoutBtn`, takže tlačítko Nový obrázek odkazuje následujícím způsobem:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Uložte změny do obou *ShoppingCart.aspx* souboru a *ShoppingCart.aspx.cs* souboru.
7. V nabídce vyberte **ladění**-&gt;**sestavení Northwind**.  
   Projekt bude znovu vytvořen s nově přidaný **ImageButton** ovládacího prvku.

### <a name="send-purchase-details-to-paypal"></a>Odeslat podrobnosti o nákupu PayPal

Pokud uživatel klikne **Checkout** tlačítko na stránce nákupní košík (*ShoppingCart.aspx*), se zobrazí za přibližně procesu nákupu. Následující kód volá funkci první PayPal potřebné k nákupu produktů.

1. Z *Checkout* složku, otevřete soubor kódu na pozadí s názvem *CheckoutStart.aspx.cs*.   
   Ujistěte se, že otevřete soubor kódu na pozadí.
2. Nahraďte stávající kód následujícím kódem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Pokud uživatel aplikaci klikne **Checkout** tlačítko na nákupního košíku, v prohlížeči přejdete na *CheckoutStart.aspx* stránky. Když *CheckoutStart.aspx* stránce zatížením, `ShortcutExpressCheckout` metoda je volána. Uživatel v tomto okamžiku se přenesou do testování webu PayPal. Na webu PayPal, uživatel zadá své přihlašovací údaje služby PayPal, kontroly podrobnosti o nákupu, přijímá PayPal smlouvu a vrátí do ukázkové aplikace Wingtip Toys kde `ShortcutExpressCheckout` dokončení metody. Při `ShortcutExpressCheckout` metody je dokončena, přesměruje uživatele *CheckoutReview.aspx* stránku zadanou v `ShortcutExpressCheckout` metoda. To umožňuje uživateli chcete podívat na podrobnosti objednávky z v rámci ukázkové aplikace Wingtip Toys.

### <a name="review-order-details"></a>Zkontrolujte podrobnosti objednávky

Po návratu z PayPal, *CheckoutReview.aspx* zobrazí se stránka ukázkové aplikace Wingtip Toys podrobnostmi o objednávce. Tato stránka umožňuje uživateli chcete podívat na podrobnosti objednávky před nákupem produktů. *CheckoutReview.aspx* stránky musí být vytvořeny následujícím způsobem:

1. V *Checkout* složku, otevřete stránku s názvem *CheckoutReview.aspx*.
2. Nahraďte existující kód následujícím kódem:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Otevřete stránku použití modelu code-behind s názvem *CheckoutReview.aspx.cs* a nahraďte existující kód následujícím kódem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView** ovládacího prvku se používá k zobrazení Podrobnosti objednávky, které byly vráceny z PayPal. Navíc výše uvedený kód uloží podrobnosti objednávky do databáze na adresář Wingtip Toys jako `OrderDetail` objektu. Když uživatel klikne na **dokončení pořadí** tlačítko, bude přesměrován na *CheckoutComplete.aspx* stránky.

> [!NOTE] 
> 
> **Tip**
> 
> Ve značkách *CheckoutReview.aspx* stránce, Všimněte si, že `<ItemStyle>` značka se používá ke změně stylu položek v rámci **DetailsView** ovládacího prvku v dolní části stránky. Pomocí zobrazení stránky v **návrhové zobrazení** (tak, že vyberete **návrhu** v levém dolním rohu sady Visual Studio), pak vyberete **DetailsView** řídit a vyberete  **Inteligentní značka** (ikonu šipky v horní části právo na ovládací prvek), budete moct zobrazit **Úkoly DetailsView**.
> 
> ![Pokladna a platba přes PayPal – upravit pole](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Výběrem **upravit pole**, **pole** se zobrazí dialogové okno. V tomto dialogovém jednoduše můžete kontrolovat vizuální vlastnosti, jako například **ItemStyle**, nástroje **DetailsView** ovládacího prvku.
> 
> ![Pokladna a platba přes PayPal – dialogové okno pole](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Dokončete nákup

*CheckoutComplete.aspx* stránky vytvoří nákupu z PayPal. Jak je uvedeno výše, uživatel musí kliknout na **dokončení pořadí** tlačítko předtím, než aplikaci přejdete na *CheckoutComplete.aspx* stránky.

1. V *Checkout* složku, otevřete stránku s názvem *CheckoutComplete.aspx*.
2. Nahraďte existující kód následujícím kódem:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Otevřete stránku použití modelu code-behind s názvem *CheckoutComplete.aspx.cs* a nahraďte existující kód následujícím kódem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Když *CheckoutComplete.aspx* načtení stránky `DoCheckoutPayment` metoda je volána. Jak bylo zmíněno výše, `DoCheckoutPayment` nákupu z testovacího prostředí PayPal dokončení metody. Po dokončení PayPal nákupní pořadí, *CheckoutComplete.aspx* stránka zobrazuje platební transakce `ID` odběrateli.

### <a name="handle-cancel-purchase"></a>Popisovač zrušit nákup

Pokud se uživatel rozhodne zrušit nákup, budete přesměrováni na *CheckoutCancel.aspx* stránku, kde se zobrazí, že jejich pořadí se zrušila.

1. Otevřete stránku s názvem *CheckoutCancel.aspx* v *Checkout* složky.
2. Nahraďte existující kód následujícím kódem:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Zpracování chyb, které nákupu

Chyby během procesu nákupu bude zpracován adresou *CheckoutError.aspx* stránky. Kódu z *CheckoutStart.aspx* stránky, *CheckoutReview.aspx* stránky a *CheckoutComplete.aspx* stránka bude každý přesměrovat  *CheckoutError.aspx* stránky, pokud dojde k chybě.

1. Otevřete stránku s názvem *CheckoutError.aspx* v *Checkout* složky.
2. Nahraďte existující kód následujícím kódem:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx* zobrazí se stránka s podrobnostmi o chybě, když během procesu registrace dojde k chybě.

## <a name="running-the-application"></a>Spuštění aplikace

Spuštění aplikace se dozvíte, jak zakoupit produktů. Všimněte si, že budete používat v PayPal prostředí pro testování. Žádné částky se vyměňují.

1. Ujistěte se, že všechno, co vaše soubory jsou uloženy v sadě Visual Studio.
2. Otevřete webový prohlížeč a přejděte do [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Přihlášení pomocí vývojářského účtu PayPal, který jste vytvořili dříve v tomto kurzu.  
   Izolovaný prostor pro vývojáře společnosti PayPal, musíte být přihlášeni na [ https://developer.paypal.com ](https://developer.paypal.com/) otestovat express checkout. To platí pouze pro testování, aby PayPal v živém prostředí sandboxu společnosti PayPal.
4. V sadě Visual Studio, stiskněte klávesu **F5** ke spuštění ukázkové aplikace Wingtip Toys.  
   Až znovu sestaví databázi, v prohlížeči se otevře a zobrazí *Default.aspx* stránky.
5. Přidat výběrem kategorie produktů, jako je například cars (auta) a potom kliknutím na tři různé produkty do nákupního košíku **přidat do košíku** vedle každého produktu.  
   Nákupního košíku se zobrazí na produkt, který jste vybrali.
6. Klikněte na tlačítko **PayPal** tlačítko rezervovat. 

    ![Pokladna a platba přes PayPal – košíku](checkout-and-payment-with-paypal/_static/image20.png)

   Rezervuje se bude vyžadovat, že máte účet uživatele pro ukázkovou aplikaci Wingtip Toys.
7. Klikněte na tlačítko **Google** odkaz na pravé straně stránky k přihlášení pomocí existujícího účtu gmail.com e-mailu.  
   Pokud nemáte účet gmail.com, můžete vytvořit jednu pro testovací účely v [www.gmail.com](https://www.gmail.com/). Můžete také použít standardní místní účet kliknutím na "Zaregistrovat". 

    ![Pokladna a platba přes PayPal – přihlášení](checkout-and-payment-with-paypal/_static/image21.png)
8. Přihlaste se pomocí účtu gmail a heslo. 

    ![Pokladna a platba přes PayPal – Gmail přihlášení](checkout-and-payment-with-paypal/_static/image22.png)
9. Klikněte na tlačítko **přihlášení** tlačítko pro registraci účtu gmailu se svým uživatelským jménem Wingtip Toys ukázkové aplikace. 

    ![Pokladna a platba přes PayPal – zaregistrovat účet](checkout-and-payment-with-paypal/_static/image23.png)
10. Na testovací lokalitě PayPal, přidejte vaše **kupujících** e-mailová adresa a heslo, které jste vytvořili dříve v tomto kurzu a pak klikněte na tlačítko **přihlásit** tlačítko. 

    ![Pokladna a platba přes PayPal – PayPal přihlášení](checkout-and-payment-with-paypal/_static/image24.png)
11. Přijměte zásady PayPal a klikněte na tlačítko **Agree a pokračovat** tlačítko.  
    Všimněte si, že tato stránka je pouze zobrazí při prvním použití tohoto účtu PayPal. Opět si všimněte, že toto je testovací účet, se vyměňují žádné skutečné peníze. 

    ![Pokladna a platba přes PayPal – zásady PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Zkontrolovat informace o pořadí na PayPal, testovací prostředí kontrolní stránku a klikněte na tlačítko **pokračovat**. 

    ![Pokladna a platba přes PayPal – kontrola informací o](checkout-and-payment-with-paypal/_static/image26.png)
13. Na *CheckoutReview.aspx* stránce zkontrolujte částka objednávky a zobrazit vygenerované dodací adresu. Klikněte **dokončení pořadí** tlačítko. 

    ![Pokladna a platba přes PayPal – pořadí revize](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx** zobrazí se stránka s ID platební transakce. 

    ![Pokladna a platba přes PayPal – dokončení registrace](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Kontrola databáze

Kontrolou aktualizovaná data v ukázkové databázi aplikace Wingtip Toys po spuštění aplikace, uvidíte, že aplikace úspěšně nahrál nákup produktů.

Si můžete prohlédnout data obsažená v *Wingtiptoys.mdf* databázový soubor s použitím **Průzkumník databáze** okno (**Průzkumníka serveru** okna v sadě Visual Studio) stejně jako dříve v této sérii kurzů.

1. Zavřete okno prohlížeče, pokud je stále otevřen.
2. V sadě Visual Studio, vyberte **zobrazit všechny soubory** ikonu v horní části **Průzkumníka řešení** a umožňuje tak rozšířit **aplikace\_Data** složky.
3. Rozbalte **aplikace\_Data** složky.  
 Budete muset vybrat **zobrazit všechny soubory** ikonu složky.
4. Klikněte pravým tlačítkem myši *Wingtiptoys.mdf* databázový soubor a vyberte **otevřít**.  
    **Průzkumník serveru** se zobrazí.
5. Rozbalte **tabulky** složky.
6. Klikněte pravým tlačítkem myši **objednávky**tabulce a vybrat **zobrazit Data tabulky**.  
 **Objednávky** tabulky se zobrazí.
7. Zkontrolujte **PaymentTransactionID** sloupci úspěšných transakcí. 

    ![Pokladna a platba přes PayPal – Kontrola databáze](checkout-and-payment-with-paypal/_static/image29.png)
8. Zavřít **objednávky** okno tabulky.
9. V Průzkumníku serveru klikněte pravým tlačítkem myši **OrderDetails** tabulce a vybrat **zobrazit Data tabulky**.
10. Zkontrolujte `OrderId` a `Username` hodnoty v **OrderDetails** tabulky. Všimněte si, že tyto hodnoty odpovídat `OrderId` a `Username` hodnoty, které jsou součástí **objednávky** tabulky.
11. Zavřít **OrderDetails** okno tabulky.
12. Klikněte pravým tlačítkem na soubor databáze na adresář Wingtip Toys (*Wingtiptoys.mdf*) a vyberte **ukončení připojení**.
13. Pokud se nezobrazí **Průzkumníku řešení** okna, klikněte na tlačítko **Průzkumníku řešení** v dolní části **Průzkumníka serveru** okno k zobrazení **Průzkumníka řešení**  znovu.

## <a name="summary"></a>Souhrn

V tomto kurzu jste přidali, pořadí a pořadí podrobností schémata sledování nákup produktů. Také integrované funkce PayPal do ukázkové aplikace Wingtip Toys.

## <a name="additional-resources"></a>Další prostředky

[Přehled konfigurace technologie ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Nasaďte aplikaci zabezpečení rozhraní ASP.NET Web Forms s nástroji Membership, OAuth a SQL Database do služby Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Právní omezení

Tento kurz obsahuje ukázkový kód. Takové ukázkový kód poskytuje "tak jak jsou" bez záruky jakéhokoli druhu. Proto Microsoft nezaručuje přesnost, integritu nebo kvalita ukázkového kódu. Vyjadřujete souhlas s pomocí ukázkového kódu na vaše vlastní nebezpečí. Za žádných okolností bude Microsoft za vás nijak případný ukázkový kód, obsah, včetně, ale nikoli výhradně, žádné chyby nebo opomenutí případný ukázkový kód, obsah, nebo ke ztrátě nebo poškození jakéhokoli druhu vzniklé v důsledku použití případný ukázkový kód. Tímto se zobrazí oznámení a souhlasí s tím, že odškodníte, uložit a podržte chránit a všechny ztráty, deklarace identity riziko ztráty, škody nebo poškození jakéhokoli druh včetně bez omezení, jsou způsobené nebo vyplývající z materiál, který pošlete, Microsoft přenos, použít nebo využívají včetně, ale nikoli výhradně, názory vyjádřené v něm.

> [!div class="step-by-step"]
> [Předchozí](shopping-cart.md)
> [další](membership-and-administration.md)
