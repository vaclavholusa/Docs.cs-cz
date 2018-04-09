---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Volání webového rozhraní API z Windows Phone 8 aplikace (C#) | Microsoft Docs
author: rmcmurray
description: Vytvořte úplného začátku do konce scénáře skládající se z aplikace ASP.NET Web API, která nabízí katalog Books do aplikace Windows Phone 8.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Volání webového rozhraní API z aplikace pro Windows Phone 8 (C#)
====================
podle [Roberta Mcmurrayho](https://github.com/rmcmurray)

V tomto kurzu se dozvíte, jak vytvořit úplného začátku do konce scénáře skládající se z aplikace ASP.NET Web API, která nabízí katalog Books do aplikace Windows Phone 8.

### <a name="overview"></a>Přehled

RESTful služby, jako je ASP.NET Web API zjednodušit vytváření aplikací založené na protokolu HTTP pro vývojáře, protože poskytuje abstrakci Architektura aplikace na straně serveru a na straně klienta. Místo vytváření vlastní protokol založený na soket pro komunikaci, vývojáři webového rozhraní API jednoduše je potřeba publikovat nezbytných metod HTTP pro svou aplikaci (například: GET, POST, PUT, DELETE), a vývojáři aplikace klienta stačí využívat metody HTTP, které jsou nezbytné pro svou aplikaci.

V tomto kurzu začátku do konce se dozvíte, jak vytvořit následující projekty pomocí webového rozhraní API:

- V [první části tohoto kurzu](#STEP1), vytvoříte aplikaci ASP.NET Web API, která podporuje všechny operace vytvoření, čtení, aktualizace a odstranění (CRUD) ke správě adresáře katalogu. Tato aplikace bude používat [ukázkový soubor XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) z webu MSDN.
- V [druhé části tohoto kurzu](#STEP2), vytvoříte interaktivní aplikace Windows Phone 8, která načte data z vaší aplikace webového rozhraní API.

#### <a name="prerequisites"></a>Požadavky

- Visual Studio 2013 s Windows Phone 8 SDK nainstalován
- Windows 8 nebo novější na 64bitovém systému s technologií Hyper-V nainstalována
- Seznam Další požadavky naleznete v tématu *požadavky na systém* části na [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) stránce pro stažení.

> [!NOTE]
> Pokud chcete k testování připojení mezi webového rozhraní API a Windows Phone 8 projekty v lokálním systému, budete muset postupujte podle pokynů *[připojení k webové rozhraní API aplikace v místním emulátoru Windows Phone 8 Počítač](https://go.microsoft.com/fwlink/?LinkId=324014)* článku nastavte svého testovacího prostředí.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Krok 1: Vytvoření webového projektu pro knihkupectví rozhraní API

Prvním krokem v tomto kurzu začátku do konce je vytvoření projektu webového rozhraní API, která podporuje všechny operace CRUD; Všimněte si, že přidáte projekt aplikace Windows Phone k tomuto řešení v [kroku 2](#STEP2) tohoto kurzu.

1. Otevřete **Visual Studio 2013**.
2. Klikněte na tlačítko **soubor**, pak **nové**a potom **projektu**.
3. Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **nainstalovaná**, pak **šablony**, pak **Visual C#**a potom **Webové**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Kliknutím na obrázek rozbalení                                                                |


4. Zvýrazněte **webové aplikace ASP.NET**, zadejte **knihkupectví** název projektu a pak klikněte na tlačítko **OK**.
5. Když **nový projekt ASP.NET** se zobrazí dialogové okno, vyberte **webového rozhraní API** šablony a pak klikněte na **OK**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Kliknutím na obrázek rozbalení                                                                |


6. Když se otevře projekt webového rozhraní API, odeberte z projektu řadič ukázka:

    1. Rozbalte **řadiče** složky v Průzkumníku řešení.
    2. Klikněte pravým tlačítkem myši **ValuesController.cs** souboru a potom klikněte na **odstranit**.
    3. Klikněte na tlačítko **OK** po zobrazení výzvy potvrďte odstranění.
7. Přidání dat XML do projektu webového rozhraní API. Tento soubor obsahuje obsah katalogu pro knihkupectví:

   1. Klikněte pravým tlačítkem myši **aplikace\_Data** složky v Průzkumníku řešení klikněte **přidat**a potom klikněte na **novou položku**.
   2. Když **přidat novou položku** se zobrazí dialogové okno, zvýrazněte **souboru XML** šablony.
   3. Název souboru **Books.xml**a potom klikněte na **přidat**.
   4. Když **Books.xml** otevření souboru, nahraďte kód v souboru XML z ukázky **books.xml** soubor na webu MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Uložte a zavřete soubor XML.

8. Přidání modelu pro knihkupectví do projektu webového rozhraní API. Tento model obsahuje logiku pro knihkupectví aplikaci vytvořit, číst, aktualizace nebo odstranění (CRUD):

   1. Klikněte pravým tlačítkem myši **modely** složky v Průzkumníku řešení klikněte **přidat**a potom klikněte na **třída**.
   2. Když **přidat novou položku** se zobrazí dialogové okno, zadejte název souboru třída **BookDetails.cs**a potom klikněte na **přidat**.
   3. Když **BookDetails.cs** otevření souboru, nahraďte kód v souboru následujícím kódem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Uložte a zavřete **BookDetails.cs** souboru.

9. Řadič pro knihkupectví přidejte do projektu webového rozhraní API:

   1. Klikněte pravým tlačítkem myši **řadiče** složky v Průzkumníku řešení klikněte **přidat**a potom klikněte na **řadič**.
   2. Když **přidat vygenerované uživatelské rozhraní** se zobrazí dialogové okno, zvýrazněte **webové rozhraní API 2 řadiče - prázdný**a pak klikněte na **přidat**.
   3. Když **přidat kontroler** se zobrazí dialogové okno, názvu kontroleru **BooksController**a potom klikněte na **přidat**.
   4. Když **BooksController.cs** otevření souboru, nahraďte kód v souboru následujícím kódem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Uložte a zavřete **BooksController.cs** souboru.

10. Sestavení aplikace webového rozhraní API ke kontrole chyb.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Krok 2: Přidání projektu Windows Phone 8 knihkupectví katalogu

Dalším krokem tohoto začátku do konce scénáře je vytvoření katalogu aplikací pro Windows Phone 8. Tato aplikace bude používat *Windows Phone Vycentrovat aplikace* šablonu pro výchozí uživatelské rozhraní a bude používat aplikace webového rozhraní API, který jste vytvořili v [kroku 1](#STEP1) tohoto kurzu jako zdroj dat.

1. Klikněte pravým tlačítkem myši **knihkupectví** řešení v v Průzkumníku řešení klikněte **přidat**a potom **nový projekt**.
2. Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **nainstalovaná**, pak **Visual C#**a potom **Windows Phone**.
3. Zvýrazněte **Windows Phone Vycentrovat aplikace**, zadejte **BookCatalog** pro název a pak klikněte na tlačítko **OK**.
4. Přidejte balíček Json.NET NuGet **BookCatalog** projektu:

    1. Klikněte pravým tlačítkem na **odkazy** pro **BookCatalog** projektu v Průzkumníku řešení a potom klikněte na **spravovat balíčky NuGet**.
    2. Když **spravovat balíčky NuGet** se zobrazí dialogové okno, rozbalte položku **Online** části a zvýraznit **nuget.org**.
    3. Zadejte **Json.NET** do vyhledávání pole a klikněte na ikonu hledání.
    4. Zvýrazněte **Json.NET** ve výsledcích hledání a pak klikněte na tlačítko **nainstalovat**.
    5. Po dokončení instalace, klikněte na tlačítko **Zavřít**.
5. Přidat **BookDetails** modelu k **BookCatalog** projektu; obsahuje model obecné třídy pro knihkupectví:

   1. Klikněte pravým tlačítkem myši **BookCatalog** projektu v Průzkumníku řešení a potom klikněte na **přidat**a potom klikněte na **novou složku**.
   2. Název nové složky **modely**.
   3. Klikněte pravým tlačítkem myši **modely** složky v Průzkumníku řešení klikněte **přidat**a potom klikněte na **třída**.
   4. Když **přidat novou položku** se zobrazí dialogové okno, zadejte název souboru třída **BookDetails.cs**a potom klikněte na **přidat**.
   5. Když **BookDetails.cs** otevření souboru, nahraďte kód v souboru následujícím kódem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Uložte a zavřete **BookDetails.cs** souboru.

6. Aktualizace **MainViewModel.cs** třída zahrnout funkce pro komunikaci s aplikací pro knihkupectví webového rozhraní API:

   1. Rozbalte **ViewModels** složky v Průzkumníku řešení a pak dvojitým kliknutím **MainViewModel.cs** souboru.
   2. Když **MainViewModel.cs** otevření souboru, nahraďte kód v souboru následující; Všimněte si, že budete muset aktualizovat hodnotu `apiUrl` konstantní s skutečná adresa URL webové rozhraní API: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Uložte a zavřete **MainViewModel.cs** souboru.

7. Aktualizace **MainPage.xaml** soubor pro přizpůsobení název aplikace:

   1. Dvakrát klikněte **MainPage.xaml** soubor v Průzkumníku řešení.
   2. Když **MainPage.xaml** otevření souboru, vyhledejte následující řádky kódu: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Tyto řádky nahraďte následujícím textem: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Uložte a zavřete **MainPage.xaml** souboru.

8. Aktualizace **DetailsPage.xaml** soubor pro přizpůsobení zobrazených položek:

   1. Dvakrát klikněte **DetailsPage.xaml** soubor v Průzkumníku řešení.
   2. Když **DetailsPage.xaml** otevření souboru, vyhledejte následující řádky kódu: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Tyto řádky nahraďte následujícím textem: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Uložte a zavřete **DetailsPage.xaml** souboru.

9. Sestavení aplikace Windows Phone ke kontrole chyb.

### <a name="step-3-testing-the-end-to-end-solution"></a>Krok 3: Testování-komplexní řešení

Jak je uvedeno v *požadavky* části tohoto kurzu, a to při testování připojení mezi webového rozhraní API a Windows Phone 8 projekty v lokálním systému, budete muset postupujte podle pokynů *[ Připojení k webové rozhraní API aplikace v místním počítači emulátoru Windows Phone 8](https://go.microsoft.com/fwlink/?LinkId=324014)* článku nastavte svého testovacího prostředí.

Jakmile máte nakonfigurované testovacího prostředí, musíte nastavit jako spouštěný projekt aplikace Windows Phone. Chcete-li tak učinit, zvýrazněte **BookCatalog** aplikace v Průzkumníku řešení a pak klikněte na tlačítko **nastavit jako spouštěný projekt**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Kliknutím na obrázek rozbalení |

Po stisknutí klávesy F5, Visual Studio se spustí i Windows Phone emulátoru, která se zobrazí &quot;počkejte&quot; zprávy z vaší webové rozhraní API je načítání dat aplikací:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Kliknutím na obrázek rozbalení |

Pokud všechno proběhne úspěšně, měli byste vidět katalogu zobrazí:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Kliknutím na obrázek rozbalení |

Pokud klepnete na libovolný název knihy, aplikace se zobrazí popis adresáře:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Kliknutím na obrázek rozbalení |

Pokud aplikace nemůže komunikovat s vašeho webového rozhraní API, zobrazí se chybová zpráva:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Kliknutím na obrázek rozbalení |

Pokud klepnete na chybovou zprávu, zobrazí se další podrobnosti o této chybě:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Kliknutím na obrázek rozbalení                                                                 |

