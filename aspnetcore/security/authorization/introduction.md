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
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a>Úvod

<a name="security-authorization-introduction"></a>

Autorizace odkazuje na proces, který určuje, co je uživatel moct provádět. Administrativní uživatel může například vytvořit knihovnu dokumentů, přidat dokumenty, upravovat dokumenty a je odstranit. Práce s knihovnou uživatele bez oprávnění správce je pouze oprávnění číst dokumenty.

Autorizace je ortogonální a nezávislé na ověřování, což je proces pro zjišťování, který je uživatel. Ověřování může vytvořit jeden nebo více identit pro aktuálního uživatele.

## <a name="authorization-types"></a>Typy autorizace

ASP.NET Core autorizaci poskytuje jednoduchý deklarativní [role](roles.md) a s formátováním [na základě zásad](policies.md) modelu. Autorizace je vyjádřeno v požadavcích a obslužné rutiny vyhodnotit deklarace identity uživatele podle požadavků. Imperativní kontroly může být založen na jednoduchý zásad nebo zásad, které vyhodnocení identitu uživatele a vlastnosti prostředku, který uživatel se pokouší o přístup.

## <a name="namespaces"></a>Jmenné prostory

Součásti ověření, včetně `AuthorizeAttribute` a `AllowAnonymousAttribute` atributy, se nacházejí v `Microsoft.AspNetCore.Authorization` oboru názvů.

Informace naleznete v dokumentaci na [jednoduché autorizace](xref:security/authorization/simple).
