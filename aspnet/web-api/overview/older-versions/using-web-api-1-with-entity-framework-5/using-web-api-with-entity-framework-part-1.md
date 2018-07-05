---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Část 1: Přehled a vytvoření projektu | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f0616383fce2e92f7d1a0b63bf840208f7327bf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394058"
---
<a name="part-1-overview-and-creating-the-project"></a>Část 1: Přehled a vytvoření projektu
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework je objektově/relační mapování rozhraní. Mapuje se na entity v relační databázi objektů domény ve vašem kódu. Ve většině případů není nutné se starat o databázové vrstvě, protože rozhraní Entity Framework za vás postará o ho. Váš kód provádí úpravy objektů a změny se ukládají do databáze.

## <a name="about-the-tutorial"></a>Informace o kurzu

V tomto kurzu vytvoříte jednoduchou úložiště aplikací. Existují dvě hlavní části aplikace. Normální uživatelé mohou zobrazit produkty a vytvoření objednávky:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Správci mohou vytvořit, odstranit nebo upravit produkty:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Dovednosti, které se dozvíte

Zde je, co se dozvíte:

- Jak používat rozhraní Entity Framework s webovým rozhraním API technologie ASP.NET.
- Jak používat rozhraní knockout.js k vytvoření dynamického uživatelského rozhraní klienta.
- Jak používat ověřování pomocí formulářů s webovým rozhraním API pro ověřování uživatelů.

I když v tomto kurzu je samostatný, můžete chtít nejdřív přečíst následující kurzy:

- [Vaše první rozhraní ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Vytvoření webového rozhraní API, která podporuje operace CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Základní znalost [ASP.NET MVC](../../../../mvc/index.md) je také užitečné.

## <a name="overview"></a>Přehled

Na vysoké úrovni je tady Architektura aplikace:

- ASP.NET MVC vygeneruje stránky HTML klienta.
- Rozhraní ASP.NET Web API zveřejňuje operace CRUD s daty (výrobky a objednávky).
- Entity Framework překládá modely C# používá webového rozhraní API do entity databáze.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Následující diagram znázorňuje, jak jsou reprezentovány objekty domény v různých vrstvách aplikace: databázové vrstvě, objektový model a nakonec přenosový formát, který se používá k přenosu dat do klienta prostřednictvím protokolu HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Vytvoření projektu sady Visual Studio

Můžete vytvořit projekt kurz pomocí Visual Web Developer Express nebo plná verze Visual Studio.

Z **Start** klikněte na **nový projekt**.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **webové**. V seznamu šablon projektu vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt "ProductStore" a klikněte na tlačítko **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

V **nového projektu ASP.NET MVC 4** dialogového okna, vyberte **internetovou aplikaci** a klikněte na tlačítko **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

"Internetové aplikace" šablona vytvoří aplikaci ASP.NET MVC, která podporuje ověřování pomocí formulářů. Pokud nyní aplikaci spustíte, už má některé funkce:

- Noví uživatelé můžete zaregistrovat klepnutím na odkaz "Register" v pravém horním rohu.
- Registrovaní uživatelé přihlásit po klepnutí na odkaz "Přihlásit".

Informace o členství se ukládají v databázi, která se vytvoří automaticky. Další informace o ověřování pomocí formulářů v ASP.NET MVC naleznete v tématu [návod: použití ověřování pomocí formulářů v ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aktualizovat soubor CSS

Tento krok je kosmetické, ale budou stránky se pak takto předchozí snímky obrazovky.

V Průzkumníku řešení rozbalte složku obsahu a otevřete soubor s názvem Site.css. Přidejte následující stylů CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Next](using-web-api-with-entity-framework-part-2.md)
