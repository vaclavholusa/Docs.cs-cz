---
title: Úvod do autorizace v ASP.NET Core
author: rick-anderson
description: Přečtěte si základní informace o ověřování a autorizace práce v aplikacích ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Úvod do autorizace v ASP.NET Core

<a name="security-authorization-introduction"></a>

Autorizace odkazuje na proces, který určuje, co je uživatel moct provádět. Administrativní uživatel může například vytvořit knihovnu dokumentů, přidat dokumenty, upravovat dokumenty a je odstranit. Práce s knihovnou uživatele bez oprávnění správce je pouze oprávnění číst dokumenty.

Autorizace je ortogonální a nezávislé na ověřování. Autorizace však vyžaduje ověřovací mechanismus. Ověřování je proces zjišťování, který je uživatel. Ověřování může vytvořit jeden nebo více identit pro aktuálního uživatele.

## <a name="authorization-types"></a>Typy autorizace

ASP.NET Core autorizaci poskytuje jednoduchý deklarativní [role](xref:security/authorization/roles) a s formátováním [na základě zásad](xref:security/authorization/policies) modelu. Autorizace je vyjádřeno v požadavcích a obslužné rutiny vyhodnotit deklarace identity uživatele podle požadavků. Imperativní kontroly může být založen na jednoduchý zásad nebo zásad, které vyhodnocení identitu uživatele a vlastnosti prostředku, který uživatel se pokouší o přístup.

## <a name="namespaces"></a>Jmenné prostory

Součásti ověření, včetně `AuthorizeAttribute` a `AllowAnonymousAttribute` atributy, se nacházejí v `Microsoft.AspNetCore.Authorization` oboru názvů.

Informace naleznete v dokumentaci na [jednoduché autorizace](xref:security/authorization/simple).
