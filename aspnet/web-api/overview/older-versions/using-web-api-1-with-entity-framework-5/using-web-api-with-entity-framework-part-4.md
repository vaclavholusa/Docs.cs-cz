---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: '4. část: Přidání zobrazení pro správce | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 8d9d14e5228606a81482eb08086a114ad5145e44
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828809"
---
<a name="part-4-adding-an-admin-view"></a>4. část: Přidání zobrazení pro správce
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Přidání zobrazení správce

Teď vytvoříme zapnout na straně klienta a přidáme stránku, která využívají data z kontroleru pro správce. Na stránce vám umožní uživatelům vytvořit, upravit nebo odstranit produktů, pomocí zasílání požadavků AJAX na kontroleru.

V Průzkumníku řešení rozbalte složku řadiče a otevřete soubor s názvem HomeController.cs. Tento soubor obsahuje kontroler MVC. Přidat metodu s názvem `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl** metoda vytvoří identifikátor URI pro webové rozhraní API, a to uchovává v kontejneru a zobrazení pro pozdější.

V dalším kroku umístění kurzoru text v rámci `Admin` metodě akce, pak klikněte pravým tlačítkem a vyberte **přidat zobrazení**. Tím se otevře **přidat zobrazení** dialogového okna.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

V **přidat zobrazení** dialogového okna, název zobrazení "Admin". Zaškrtněte políčko s popiskem **vytvoření zobrazení se silnými typy**. V části **třídy modelu**, vyberte "Produktu (ProductStore.Models)". Všechny další možnosti ponechte jejich výchozí hodnoty.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Kliknutím na **přidat** přidá soubor s názvem Admin.cshtml v rámci zobrazení Domů. Otevřete tento soubor a přidejte následující kód HTML. Tento kód HTML definuje strukturu stránky, ale žádné funkce je připraveno, ještě.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Vytvořit odkaz na stránku pro správu

V Průzkumníku řešení rozbalte složku zobrazení a poté rozbalte složku Shared. Otevřete soubor s názvem \_Layout.cshtml. Vyhledejte **ul** element s id = "nabídka" a odkaz akce pro zobrazení správce:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> V projektu vzorku můžu provedli několik dalších tyto kosmetické změny, jako je například nahradit řetězec "Zde bude vaše logo". Tyto nemají vliv na funkce aplikace. Je možné projekt stáhnout a porovnat soubory.


Spusťte aplikaci a klikněte na odkaz "Admin", který se zobrazí v horní části domovské stránky. Na stránce správy by měl vypadat nějak takto:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Pravé teď na stránce nic nedělá. V další části používáme rozhraní Knockout.js k vytvoření dynamického uživatelského rozhraní.

## <a name="add-authorization"></a>Přidat autorizaci

Na stránce správy je nyní k dispozici všem uživatelům následujícím webu. Změňme ji omezit oprávnění správcům.

Začněte přidáním role "Správce" a uživatel s oprávněním správce. V Průzkumníku řešení rozbalte složku filtry a otevřete soubor s názvem InitializeSimpleMembershipAttribute.cs. Vyhledejte `SimpleMembershipInitializer` konstruktoru. Po volání **WebSecurity.InitializeDatabaseConnection**, přidejte následující kód:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Toto je quick-and-dirty způsob, jak přidat roli "Správce" a vytvořte uživatele pro roli.

V Průzkumníku řešení rozbalte složku řadiče a otevření souboru HomeController.cs. Přidat **Authorize** atribut `Admin` metody.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Otevřete soubor AdminController.cs a přidejte **Authorize** atribut pro celou `AdminController` třídy.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC a webového rozhraní API, jak definovat **Authorize** atributy v různých oborech názvů. Aplikace MVC používá **System.Web.Mvc.AuthorizeAttribute**, zatímco webového rozhraní API používá **System.Web.Http.AuthorizeAttribute**.


Pouze správci teď mohou zobrazit stránky pro správu. Navíc pokud odešlete požadavek HTTP kontroleru pro správce, žádost musí obsahovat soubor cookie ověřování. Pokud ne, server odešle odpověď HTTP 401 (Neautorizováno). Zobrazí se to ve Fiddleru odesláním požadavek GET na `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-3.md)
> [další](using-web-api-with-entity-framework-part-5.md)
