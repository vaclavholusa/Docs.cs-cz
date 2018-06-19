---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Použití rozhraní Web API s webovými formuláři ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
ms.locfileid: "26575854"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Použití rozhraní Web API s webovými formuláři ASP.NET
====================
podle [Wasson Jan](https://github.com/MikeWasson)

I když ASP.NET MVC je součástí balíčku webového rozhraní API ASP.NET, je snadné přidání webového rozhraní API do tradiční aplikace webových formulářů ASP.NET. Tento kurz vás provede kroky.

## <a name="overview"></a>Přehled

Použití webového rozhraní API v aplikaci webových formulářů, existují dva hlavní kroky:

- Přidání kontroleru webového rozhraní API, která je odvozena z **objektu ApiController** třídy.
- Přidat tabulku směrování, aby **aplikace\_spustit** metoda.

## <a name="create-a-web-forms-project"></a>Vytvoření projektu webové formuláře

Spuštění sady Visual Studio a vyberte **nový projekt** z **spustit** stránky. Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **aplikaci webových formulářů ASP.NET**. Zadejte název projektu a klikněte na tlačítko **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Vytvoření modelu a řadiče

Tento kurz používá stejný model a řadič třídy, jako [Začínáme](tutorial-your-first-web-api.md) kurzu.

Nejprve přidejte třídu modelu. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat třídu**. Název třídy produktu a přidejte následující implementace:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Dál přidejte kontroleru webového rozhraní API se projektu, *řadič* je objekt, který zpracovává požadavky HTTP pro webové rozhraní API.

V **Průzkumníku**, klikněte pravým tlačítkem na projekt. Vyberte **přidat novou položku**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

V části **nainstalovaných šablonách**, rozbalte položku **Visual C#** a vyberte **webové**. Zvolte ze seznamu šablon **třída Kontroleru rozhraní Web API**. Název kontroleru "ProductsController" a klikněte na tlačítko **přidat**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**Přidat novou položku** Průvodce vytvoří soubor s názvem ProductsController.cs. Odstranit metody, které průvodce součástí a přidejte následující metody:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Další informace o kódu v tomto kontroleru, najdete v článku [Začínáme](tutorial-your-first-web-api.md) kurzu.

## <a name="add-routing-information"></a>Přidání informací o směrování

Potom přidáme trasu identifikátor URI, že identifikátory URI ve tvaru &quot;/api/produkty/&quot; jsou směrovány do kontroleru.

V **Průzkumníku**, poklikejte na soubor Global.asax k otevření souboru kódu Global.asax.cs. Přidejte následující **pomocí** příkaz.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Pak přidejte následující kód, který **aplikace\_spustit** metoda:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Další informace o směrovacích tabulek najdete v tématu [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Přidat AJAX na straně klienta

To je všechno je potřeba vytvořit webové rozhraní API, které mohou klienti získat přístup. Nyní Pojďme přidat stránku HTML, který používá jQuery pro volání rozhraní API.

Zajistěte, aby vaše stránky předlohy (například *Site.Master*) zahrnuje `ContentPlaceHolder` s `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Otevřete soubor Default.aspx. Jak je znázorněno, nahraďte často používaný text, který je v části hlavní obsahu:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

V dalším kroku přidejte odkaz na zdrojový soubor jQuery v `HeaderContent` části:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Poznámka: Můžete snadno přidat odkaz na skript přetahování a vkládání souborů z **Průzkumníku řešení** do okna editoru kódu.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Pod značky script jQuery přidejte následující blok skriptu:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Po načtení dokumentu tento skript je požadavek AJAX &quot;rozhraní api nebo produkty&quot;. Požadavek vrací seznam produktů ve formátu JSON. Skript přidá do tabulky HTML informace o produktu.

Při spuštění aplikace by měl vypadat přibližně takto:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
