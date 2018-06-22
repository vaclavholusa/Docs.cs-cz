---
title: Úvod do autorizace v ASP.NET Core
author: rick-anderson
description: Přečtěte si základní informace o ověřování a autorizace práce v aplikacích ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276864"
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
