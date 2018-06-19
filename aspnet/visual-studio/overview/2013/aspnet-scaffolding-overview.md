---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Generování uživatelského rozhraní ASP.NET v sadě Visual Studio 2013 | Microsoft Docs
author: tfitzmac
description: Generování uživatelského rozhraní ASP.NET je nová funkce, která je zahrnutá v sadě Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566323"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Generování uživatelského rozhraní ASP.NET v sadě Visual Studio 2013
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Generování uživatelského rozhraní ASP.NET je nová funkce, která je zahrnutá v sadě Visual Studio 2013.


## <a name="overview"></a>Přehled

Generování uživatelského rozhraní ASP.NET je architektura generování kódu pro webové aplikace ASP.NET. Visual Studio 2013 zahrnuje generátory předem nainstalovaná kódu pro projekty MVC a webového rozhraní API. Pokud chcete rychle přidáte kód, který komunikuje s datové modely přidáte do projektu generování uživatelského rozhraní. Pomocí generování uživatelského rozhraní, můžete snížit množství času k vývoji operace standardní dat ve vašem projektu.

Ve výchozím nastavení Visual Studio 2013 nepodporuje generování kódu pro projekt webové formuláře, ale můžete generování uživatelského rozhraní s webovými formuláři přidávání k projektu závislosti MVC nebo instalaci rozšíření. Níže jsou uvedeny obou přístupů.

Visual Studio 2013 Update 2 (aktuálně RC) poskytuje možnost rozšířit ASP.NET generování uživatelského rozhraní pro splnění požadavků váš scénář. Pomocí této funkce můžete vytvořit šablonu přizpůsobené generování uživatelského rozhraní a přidejte ho do dialogového okna Přidat vygenerované uživatelské rozhraní nového. V rámci vlastní šablony zadejte kód, který se vygeneruje, když přidání vygenerované položky. Další informace najdete v tématu [vytváření vlastní Scaffolder pro sadu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Požadavky

Pokud chcete používat ASP.NET generování uživatelského rozhraní, musíte mít:

- Microsoft Visual Studio 2013
- Webové nástroje pro vývojáře (součást instalace Visual Studio 2013 výchozí)
- Webové rozhraní ASP.NET a nástroje 2013 (součást instalace Visual Studio 2013 výchozí)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Přidání vygenerované položky MVC nebo webového rozhraní API

Chcete-li přidat vygenerované uživatelské rozhraní, klikněte pravým tlačítkem na projekt nebo složku v rámci projektu a vyberte **přidat** – **novou vygenerovanou položku**, jak je znázorněno na následujícím obrázku.

![Přidat vygenerované uživatelské rozhraní položky](aspnet-scaffolding-overview/_static/image1.png)

Z **přidat vygenerované uživatelské rozhraní** okně vyberte typ vygenerované uživatelské rozhraní pro přidání.

![Vyberte typ vygenerované uživatelské rozhraní](aspnet-scaffolding-overview/_static/image2.png)

**Přidat kontroler** okno vám dává možnost vybrat možnosti generování řadič, včetně toho, jestli chcete používat nové funkce asynchronní z Entity Framework 6.

![Přidání kontroleru](aspnet-scaffolding-overview/_static/image3.png)

Pro váš scénář se vytvoří příslušné třídy a stránky. Například následující obrázek ukazuje MVC jsou řadič MVC a zobrazení, které byly vytvořeny pomocí generování uživatelského rozhraní pro třídu modelu s názvem filmy.

![Vytvořené soubory](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Přidání vygenerované položky do webových formulářů

Pokud chcete přidat generování uživatelského rozhraní, která generuje kód webových formulářů, musíte nainstalovat rozšíření pro Visual Studio nebo přidat závislosti MVC. Níže jsou uvedeny oba přístupy, ale potřebujete proveďte jednu z těchto přístupů.

### <a name="web-forms-scaffolding-extension"></a>Webové formuláře generování uživatelského rozhraní rozšíření

Můžete nainstalovat rozšíření sady Visual Studio, které vám umožní pomocí generování uživatelského rozhraní s projektem webových formulářů. V sadě Visual Studio, vyberte **nástroje** a potom **rozšíření a aktualizace**. Z toto dialogové okno hledání Galerii Visual Studio pro **Web Forms generování uživatelského rozhraní**.

![Instalace webové formuláře generování uživatelského rozhraní](aspnet-scaffolding-overview/_static/image5.png)

Další informace najdete v tématu [Web Forms generování uživatelského rozhraní](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Závislosti MVC

Chcete-li přidat závislosti MVC, vyberte **přidat** - **novou vygenerovanou položku**. V okně Přidat vygenerované uživatelské rozhraní, vyberte **závislosti MVC**, jak je uvedeno níže.

![Přidat závislosti MVC](aspnet-scaffolding-overview/_static/image6.png)

Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné. Pokud vyberete minimální, jenom balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do projektu. Pokud vyberete možnost Úplná minimální závislosti přidají, a také požadované soubory obsahu pro projekt MVC. Chcete-li snadno použít generování uživatelského rozhraní, vyberte úplné závislosti.

![Vyberte úplný závislosti](aspnet-scaffolding-overview/_static/image7.png)

Po přidání závislostí, zobrazí se **readme.txt** souboru. Pečlivě postupujte podle pokynů v tomto souboru a ujistěte se, že váš projekt funguje správně.

Pokud jste dokončili kroky v souboru readme.txt, můžete přidat nové vygenerované položky, jak je uvedeno v předchozí části o MVC a webového rozhraní API. Automaticky generované zobrazení a řadič bude fungovat správně v rámci projektu.

## <a name="tutorials"></a>Kurzy

Vytvoření vlastní scaffolder naleznete v tématu [vytváření vlastní Scaffolder pro sadu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Pokud chcete přizpůsobit generované soubory, najdete v části [postup přizpůsobení generované soubory z tohoto dialogového okna novou vygenerovanou položku](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Příklad pomocí generování uživatelského rozhraní s **Database First vývoj**, najdete v části [EF Database First s architekturou ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Příklad pomocí generování uživatelského rozhraní v **MVC** projektu najdete v tématu [Začínáme s ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Příklad pomocí generování uživatelského rozhraní v **webového rozhraní API** projektu najdete v tématu [vytvořit rozhraní REST API s atribut směrování ve webovém rozhraní API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
