---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Volání webového rozhraní API z Windows Phone 8 aplikace (C#) | Dokumentace Microsoftu
author: rmcmurray
description: Vytvoření kompletní scénář začátku do konce skládající se z aplikace ASP.NET Web API, která obsahuje katalog knih, které mají aplikace Windows Phone 8.
ms.author: aspnetcontent
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 6b7a833818424cbf3a3bf9e1e14e5b2864742c38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805039"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Volání webového rozhraní API z aplikace pro Windows Phone 8 (C#)
====================
podle [Robert McMurray](https://github.com/rmcmurray)

V tomto kurzu se dozvíte, jak vytvořit kompletní scénář začátku do konce skládající se z aplikace ASP.NET Web API, která obsahuje katalog knih, které mají aplikace Windows Phone 8.

### <a name="overview"></a>Přehled

RESTful služeb, jako je ASP.NET Web API zjednodušit vytváření aplikací založených na protokolu HTTP pro vývojáře tím, že poskytuje abstrakci Architektura aplikace na straně serveru a na straně klienta. Místo vytváření vlastnickým protokolem založené na soket pro komunikaci, webové rozhraní API vývojářům jednoduše je potřeba publikovat metody požadavku HTTP pro svou aplikaci (například: GET, POST, PUT, DELETE), a vývojáři klientských aplikací můžou potřebují jenom využívat metody HTTP, které jsou nezbytné pro svou aplikaci.

V tomto kurzu začátku do konce se dozvíte, jak pomocí webového rozhraní API k vytvoření následující projekty:

- V [první části tohoto kurzu](#STEP1), vytvoříte aplikaci rozhraní ASP.NET Web API, která podporuje všechny operace vytvoření, čtení, aktualizace a odstranění (CRUD) ke správě adresáře katalogu. Tato aplikace bude používat [ukázkový soubor XML (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) z webu MSDN.
- V [druhé části tohoto kurzu](#STEP2), vytvoříte interaktivní aplikace Windows Phone 8, která načte data z aplikace webového rozhraní API.

#### <a name="prerequisites"></a>Požadavky

- Visual Studio 2013 with nainstalovánu sadu Windows Phone 8 SDK
- Windows 8 nebo novější systém 64-bit s nainstalovanou technologii Hyper-V
- Seznam dalších požadavcích najdete v tématu *požadavky na systém* části na [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) stránce pro stažení.

> [!NOTE]
> Pokud se chystáte otestovat připojení mezi webovým rozhraním API a projekty pro Windows Phone 8 v místním systému, budete muset postupujte podle pokynů *[připojení k rozhraní API webové aplikace v místním emulátorem Windows Phone 8 Počítač](https://go.microsoft.com/fwlink/?LinkId=324014)* článku nastavte testovací prostředí.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Krok 1: Vytvoření webového rozhraní API knihkupectví projektu

Prvním krokem tohoto kurzu začátku do konce, je vytvořit projekt webového rozhraní API, která podporuje všechny operace CRUD; Všimněte si, že přidáte projekt aplikace Windows Phone k tomuto řešení v [kroku 2](#STEP2) tohoto kurzu.

1. Otevřít **Visual Studio 2013**.
2. Klikněte na tlačítko **souboru**, pak **nové**a potom **projektu**.
3. Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **nainstalováno**, pak **šablony**, pak **Visual C#** a pak **Webové**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Klikněte na obrázek rozbalení                                                                |


4. Zvýrazněte **webová aplikace ASP.NET**, zadejte **knihkupectví** pro název projektu a pak klikněte na tlačítko **OK**.
5. Když **nový projekt ASP.NET** dialogové okno se zobrazí, vyberte **webového rozhraní API** šablonu a pak klikněte na tlačítko **OK**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Klikněte na obrázek rozbalení                                                                |


6. Při otevření projektu webového rozhraní API, odeberte z projektu vzorku kontroleru:

    1. Rozbalte **řadiče** složku v Průzkumníku řešení.
    2. Klikněte pravým tlačítkem myši **ValuesController.cs** souboru a pak klikněte na tlačítko **odstranit**.
    3. Klikněte na tlačítko **OK** po zobrazení výzvy potvrďte odstranění.
7. Přidání datového souboru XML do projektu webového rozhraní API. Tento soubor obsahuje obsah knihkupectví katalogu:

   1. Klikněte pravým tlačítkem **aplikace\_Data** složku v Průzkumníku řešení klikněte **přidat**a potom klikněte na **nová položka**.
   2. Když **přidat novou položku** dialogové okno se zobrazí, vyberte **soubor XML** šablony.
   3. Název souboru **Books.xml**a potom klikněte na tlačítko **přidat**.
   4. Když **Books.xml** otevření souboru, nahraďte kód v souboru XML ze vzorku **books.xml** soubor na webové stránce MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Uložte a zavřete soubor XML.

8. Přidání knihkupectví modelu do projektu webového rozhraní API. Tento model obsahuje logiku pro aplikace knihkupectví vytvoření, čtení, aktualizace a odstranění (CRUD):

   1. Klikněte pravým tlačítkem myši **modely** složku v Průzkumníku řešení klikněte **přidat**a potom klikněte na tlačítko **třídy**.
   2. Když **přidat novou položku** dialogové okno se zobrazí, zadejte název souboru třídy **BookDetails.cs**a potom klikněte na tlačítko **přidat**.
   3. Když **BookDetails.cs** otevření souboru, nahraďte kód v souboru následujícím kódem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Uložte a zavřete **BookDetails.cs** souboru.

9. Přidáte kontroler knihkupectví do projektu webového rozhraní API:

   1. Klikněte pravým tlačítkem myši **řadiče** složku v Průzkumníku řešení klikněte **přidat**a potom klikněte na **řadič**.
   2. Když **přidat vygenerované uživatelské rozhraní** se zobrazí dialogové okno, zvýrazněte **kontroler rozhraní Web API 2 – prázdný**a potom klikněte na tlačítko **přidat**.
   3. Když **přidat kontroler** dialogové okno se zobrazí, zadejte název kontroleru **BooksController**a potom klikněte na tlačítko **přidat**.
   4. Když **BooksController.cs** otevření souboru, nahraďte kód v souboru následujícím kódem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Uložte a zavřete **BooksController.cs** souboru.

10. Sestavení aplikace webového rozhraní API ke kontrole chyb.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Krok 2: Přidání projektu Windows Phone 8 knihkupectví katalogu

Dalším krokem tohoto scénáře začátku do konce je vytvoření katalogu aplikací pro Windows Phone 8. Tato aplikace bude používat *aplikace Windows Phone datové vazby* šablonu pro výchozí uživatelské rozhraní a bude používat aplikace webového rozhraní API, kterou jste vytvořili v [kroku 1](#STEP1) tohoto kurzu jako zdroj dat.

1. Klikněte pravým tlačítkem myši **knihkupectví** řešení v v Průzkumníku řešení klikněte **přidat**a potom **nový projekt**.
2. Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **nainstalováno**, pak **Visual C#** a potom **Windows Phone**.
3. Zvýrazněte **aplikace Windows Phone datové vazby**, zadejte **BookCatalog** pro název a pak klikněte na tlačítko **OK**.
4. Přidejte balíček Json.NET NuGet **BookCatalog** projektu:

    1. Klikněte pravým tlačítkem na **odkazy** pro **BookCatalog** projektu v Průzkumníku řešení a potom klikněte na tlačítko **spravovat balíčky NuGet**.
    2. Když **spravovat balíčky NuGet** se zobrazí dialogové okno, rozbalte **Online** části a zvýraznit **nuget.org**.
    3. Zadejte **Json.NET** do vyhledávacího pole a klikněte na ikonu hledání.
    4. Zvýrazněte **Json.NET** ve výsledcích hledání a pak klikněte na tlačítko **nainstalovat**.
    5. Po dokončení instalace, klikněte na tlačítko **Zavřít**.
5. Přidat **BookDetails** model k **BookCatalog** projektu; obsahuje obecný model knihkupectví třídy:

   1. Klikněte pravým tlačítkem myši **BookCatalog** projektu v Průzkumníku řešení a potom klikněte na **přidat**a potom klikněte na tlačítko **novou složku**.
   2. Název nové složky **modely**.
   3. Klikněte pravým tlačítkem myši **modely** složku v Průzkumníku řešení klikněte **přidat**a potom klikněte na tlačítko **třídy**.
   4. Když **přidat novou položku** dialogové okno se zobrazí, zadejte název souboru třídy **BookDetails.cs**a potom klikněte na tlačítko **přidat**.
   5. Když **BookDetails.cs** otevření souboru, nahraďte kód v souboru následujícím kódem: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Uložte a zavřete **BookDetails.cs** souboru.

6. Aktualizace **MainViewModel.cs** třídy, aby obsahoval funkce pro komunikaci s aplikací knihkupectví webového rozhraní API:

   1. Rozbalte **modely ViewModels** složku v Průzkumníku řešení a poté dvojitým kliknutím **MainViewModel.cs** souboru.
   2. Když **MainViewModel.cs** otevření souboru nahraďte kód v souboru následujícím kódem, mějte na paměti, že budete muset aktualizovat hodnotu `apiUrl` konstantní pomocí příkazu skutečnou adresu URL webového rozhraní API: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Uložte a zavřete **MainViewModel.cs** souboru.

7. Aktualizace **MainPage.xaml** soubor upravit název aplikace:

   1. Dvakrát klikněte **MainPage.xaml** souboru v Průzkumníku řešení.
   2. Když **MainPage.xaml** soubor otevřít, vyhledejte následující řádky kódu: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Nahraďte tyto řádky s následujícími možnostmi: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Uložte a zavřete **MainPage.xaml** souboru.

8. Aktualizace **DetailsPage.xaml** soubor pro přizpůsobení zobrazených položek:

   1. Dvakrát klikněte **DetailsPage.xaml** souboru v Průzkumníku řešení.
   2. Když **DetailsPage.xaml** soubor otevřít, vyhledejte následující řádky kódu: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Nahraďte tyto řádky s následujícími možnostmi: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Uložte a zavřete **DetailsPage.xaml** souboru.

9. Vytvoření aplikace Windows Phone ke kontrole chyb.

### <a name="step-3-testing-the-end-to-end-solution"></a>Krok 3: Testování – ucelené řešení

Jak je uvedeno v *požadavky* projekty části tohoto kurzu, a to při testování připojení mezi webovým rozhraním API a Windows Phone 8 v místním systému, budete muset postupovat podle pokynů *[ Připojení k rozhraní API webové aplikace v místním počítači emulátorem Windows Phone 8](https://go.microsoft.com/fwlink/?LinkId=324014)* článku nastavte testovací prostředí.

Jakmile budete mít testování prostředí nakonfigurované, je potřeba nastavit jako spouštěný projekt aplikace Windows Phone. Uděláte to tak, zvýrazněte **BookCatalog** aplikace v Průzkumníku řešení a pak klikněte na tlačítko **nastavit jako spouštěný projekt**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Klikněte na obrázek rozbalení |

Pokud stisknete klávesu F5, Visual Studio se spustí i Windows Phone Emulator, který se zobrazí &quot;počkejte&quot; zpráva, zatímco data aplikací se načte z vašeho webového rozhraní API:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Klikněte na obrázek rozbalení |

Pokud všechno proběhne úspěšně, měli byste vidět katalogu zobrazí:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Klikněte na obrázek rozbalení |

Pokud klepnete na libovolný název knihy, aplikace se zobrazí popis knihy:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Klikněte na obrázek rozbalení |

Pokud aplikace nemůže komunikovat s webové rozhraní API, zobrazí se chybová zpráva:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Klikněte na obrázek rozbalení |

Pokud klepnete na chybovou zprávu, zobrazí se další podrobnosti o chybě:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Klikněte na obrázek rozbalení                                                                 |

