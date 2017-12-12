---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "Dynamické v. Silného typu zobrazení | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a>Dynamické v. Zobrazení se silnými typy
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

Existují tři způsoby, jak předat informace z řadiče zobrazení v architektuře ASP.NET MVC 3:

1. Jako objekt silného typu modelu.
2. Jako typ dynamické (pomocí @model dynamické)
3. Pomocí objekt ViewBag

I jste zapisují jednoduchou aplikaci MVC 3 horní Blog porovnat a kontrastu zobrazení dynamické a silného typu. Řadičem začíná se jednoduchým seznamem blogy:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

Klikněte pravým tlačítkem v metodě IndexNotStonglyTyped() a přidat zobrazení syntaxe Razor.

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

Zajistěte, aby **vytvořit zobrazení silného typu** není zaškrtnuté políčko. Výsledné zobrazení neobsahuje mnohem:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

Vzhledem k tomu, že používáme dynamický a ne zobrazení se silnými typy, není nám pomůže intellisense. Dokončený kód je zobrazena níže:

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

Teď přidáme zobrazení se silnými typy. Přidejte následující kód do kontroleru:

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


Všimněte si, že je přesně stejnou návratový View(topBlogs); volají se jako jiný silného typu zobrazení. Klikněte pravým tlačítkem uvnitř *StonglyTypedIndex()* a vyberte **přidat zobrazení**. Vyberte tuto chvíli **Blog** třída modelu a vyberte **seznamu** jako šablonu vygenerované uživatelské rozhraní.

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

Uvnitř novou šablonu zobrazení se nám získat podporu technologie intellisense.

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

Projekt c# lze stáhnout [zde](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).
