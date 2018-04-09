---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Část 4: Přidání zobrazení správce | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-4-adding-an-admin-view"></a>Část 4: Přidání zobrazení správce
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Přidání zobrazení správce

Nyní jsme budete zapnout na straně klienta a přidání stránky, který můžete využívat data ze správce řadiče. Stránce umožní uživatelům vytvořit, upravit nebo odstranit produkty, pomocí zasílání požadavků AJAX do kontroleru.

V Průzkumníku řešení rozbalte složku řadiče a otevřete soubor s názvem HomeController.cs. Tento soubor obsahuje kontroler MVC. Přidejte metodu s názvem `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl** metoda vytvoří identifikátor URI webové rozhraní API, a to uložíme v kontejner zobrazení pro pozdější.

V dalším kroku umístěte kurzor text v rámci `Admin` metody akce, poté klikněte pravým tlačítkem a vyberte **přidat zobrazení**. Tím se otevře **přidat zobrazení** dialogové okno.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

V **přidat zobrazení** dialogové okno, název zobrazení "Admin". Zaškrtněte políčko s názvem bez přípony **vytvořit zobrazení silného typu**. V části **třídy modelu**, vyberte "Produkt (ProductStore.Models)". Všechny ostatní možnosti ponechte jako výchozí hodnoty.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Kliknutím na tlačítko **přidat** přidá soubor s názvem Admin.cshtml pod Domů zobrazení. Umožňuje otevřít tento soubor a přidejte následující kód HTML. Tento HTML definuje strukturu stránce, ale žádné funkce ještě drátové nahoru.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Vytvoření odkazu na stránku Správce

V Průzkumníku řešení rozbalte složku, zobrazení a poté rozbalte složku sdílené. Otevřete soubor s názvem \_Layout.cshtml. Vyhledejte **ul** element s id = "nabídka" a odkaz akce pro zobrazení správce:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> V ukázkovém projektu byly provedeny několik dalších kosmetická změny, jako je například nahraďte řetězec "Zde bude vaše logo". Tyto neovlivňují funkci aplikace. Můžete stáhnout na projekt a porovnání souborů.


Spusťte aplikaci a klikněte na odkaz "Admin", který se zobrazí v horní části na domovskou stránku. Stránka Správce by měl vypadat asi takto:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Pravé teď stránce nic se neděje. V další části použijeme Knockout.js vytvářet dynamické uživatelské rozhraní.

## <a name="add-authorization"></a>Přidání ověřování

Stránky pro správu je nyní k dispozici všem uživatelům, kteří navštěvují webu. Umožňuje změnit to omezit oprávnění na správce.

Začněte přidáním role "Správce" a uživatel s oprávněním správce. V Průzkumníku řešení rozbalte složku filtry a otevřete soubor s názvem InitializeSimpleMembershipAttribute.cs. Vyhledejte `SimpleMembershipInitializer` konstruktor. Po zavolání **WebSecurity.InitializeDatabaseConnection**, přidejte následující kód:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Toto je quick-and-dirty způsob, jak přidat roli "Správce" a vytvořte uživatele pro roli.

V Průzkumníku řešení rozbalte složku řadiče a otevřete soubor HomeController.cs. Přidat **Authorize** atribut `Admin` metoda.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Otevřete soubor AdminController.cs a přidejte **Authorize** atribut celý `AdminController` třídy.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC a webového rozhraní API, jak definovat **Autorizovat** atributy v různých oborech názvů. Aplikace MVC používá **System.Web.Mvc.AuthorizeAttribute**, zatímco webového rozhraní API používá **System.Web.Http.AuthorizeAttribute**.


Pouze správci mohou nyní zobrazit stránky pro správu. Navíc při odeslání požadavku HTTP k řadiči pro správu, žádost musí obsahovat soubor cookie ověřování. Pokud ne, server odešle odpověď HTTP 401 (Neautorizováno). Můžete to vidět v aplikaci Fiddler odesíláním žádosti GET za účelem `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-3.md)
> [další](using-web-api-with-entity-framework-part-5.md)
