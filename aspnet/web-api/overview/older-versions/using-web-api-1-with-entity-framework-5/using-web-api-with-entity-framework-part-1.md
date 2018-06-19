---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Část 1: Přehled a vytváření projektu | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9cdff0cb0cad9adad546c8f8d46ba9b010e1079
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873323"
---
<a name="part-1-overview-and-creating-the-project"></a>Část 1: Přehled a vytváření projektu
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Představuje objekt nebo relační mapování rozhraní je rozhraní Entity Framework. Mapuje domény objekty v kódu na entity v relační databázi. Ve většině případů nemáte si dělat starosti v databázové vrstvě, protože rozhraní Entity Framework to postará za vás. Kódu umožňuje pracovat s objekty a změny jsou ukládány do databáze.

## <a name="about-the-tutorial"></a>O kurzu

V tomto kurzu vytvoříte aplikaci jednoduché úložiště. Existují dvě hlavní části k aplikaci. Normální uživatelé mohou zobrazit produkty nebo vytvořit objednávky:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Správci mohou vytvořit, odstranit nebo upravit produkty:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Dovedností, které se dozvíte

Zde je, co se dozvíte:

- Jak používat rozhraní Entity Framework s rozhraním ASP.NET Web API.
- Jak používat knockout.js vytvořit dynamické uživatelské rozhraní klienta.
- Jak používat ověřování pomocí formulářů pomocí webového rozhraní API k ověřování uživatelů.

I když v tomto kurzu je samostatný, můžete chtít nejdřív přečíst následující kurzy:

- [První rozhraní ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Vytvoření rozhraní Web API podporujícího operace CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Některé znalosti [ASP.NET MVC](../../../../mvc/index.md) je také užitečné.

## <a name="overview"></a>Přehled

Na vysoké úrovni zde je architektura aplikace:

- ASP.NET MVC vygeneruje stránky HTML pro klienta.
- Rozhraní ASP.NET Web API zpřístupní operace CRUD na data (produkty a objednávky).
- Rozhraní Entity Framework překládá používané webového rozhraní API do entity databáze modely C#.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Následující diagram znázorňuje zastoupení objektů domény v různých vrstev aplikace: vrstva databáze, v objektovém modelu a nakonec přenosový formát, který se používá k přenosu dat do klienta prostřednictvím protokolu HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Vytvoření projektu Visual Studio

Můžete vytvořit kurz projekt pomocí Visual Web Developer Express nebo si úplnou verzi sady Visual Studio.

Z **spustit** klikněte na tlačítko **nový projekt**.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**. Název projektu "ProductStore" a klikněte na tlačítko **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **Internetové aplikace** a klikněte na tlačítko **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Šablona "Internet aplikací" vytvoří aplikaci ASP.NET MVC, která podporuje ověřování pomocí formulářů. Pokud nyní aplikaci spustíte, už je některé funkce:

- Nové uživatelé mohou registrovat kliknutím na odkaz "Register" v pravém horním rohu.
- Registrovaní uživatelé se můžou přihlásit kliknutím na odkaz "Přihlášení".

Informace o členství uchovávána v databázi, která je vytvořena automaticky. Další informace o ověřování pomocí formulářů v architektuře ASP.NET MVC najdete v tématu [návod: použití ověřování pomocí formulářů v architektuře ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aktualizovat soubor CSS

Tento krok je kosmetické, ale bude stránky vykreslení jako starší snímky obrazovky.

V Průzkumníku řešení rozbalte složku obsahu a otevřete soubor s názvem Site.css. Přidejte následující stylů CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Next](using-web-api-with-entity-framework-part-2.md)
