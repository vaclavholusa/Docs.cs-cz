---
title: "Úvod do autorizace"
author: rick-anderson
description: "Tento dokument poskytuje základní vysvětlení autorizace a vysvětluje, jak autorizace souvisí s ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: a6bd81a4e5796c1d0a0033c2b8d5a6ba9282564c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="introduction"></a>Úvod

<a name="security-authorization-introduction"></a>

Autorizace odkazuje na proces, který určuje, co je uživatel moct provádět. Administrativní uživatel může například vytvořit knihovnu dokumentů, přidat dokumenty, upravovat dokumenty a je odstranit. Práce s knihovnou uživatele bez oprávnění správce je pouze oprávnění číst dokumenty.

Autorizace je ortogonální a nezávislé na ověřování, což je proces pro zjišťování, který je uživatel. Ověřování může vytvořit jeden nebo více identit pro aktuálního uživatele.

## <a name="authorization-types"></a>Typy autorizace

ASP.NET Core autorizaci poskytuje jednoduchý deklarativní [role](roles.md) a [bohaté na základě zásad](policies.md) modelu. Autorizace je vyjádřeno v požadavcích a obslužné rutiny vyhodnotit deklarace identity uživatele podle požadavků. Imperativní kontroly může být založen na jednoduchý zásad nebo zásad, které vyhodnocení identitu uživatele a vlastnosti prostředku, který uživatel se pokouší o přístup.

## <a name="namespaces"></a>Jmenné prostory

Součásti ověření, včetně `AuthorizeAttribute` a `AllowAnonymousAttribute` atributy, které se nacházejí v `Microsoft.AspNetCore.Authorization` oboru názvů.
