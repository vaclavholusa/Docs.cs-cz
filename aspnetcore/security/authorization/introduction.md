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
# <a name="introduction"></a><span data-ttu-id="07552-103">Úvod</span><span class="sxs-lookup"><span data-stu-id="07552-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="07552-104">Autorizace odkazuje na proces, který určuje, co je uživatel moct provádět.</span><span class="sxs-lookup"><span data-stu-id="07552-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="07552-105">Administrativní uživatel může například vytvořit knihovnu dokumentů, přidat dokumenty, upravovat dokumenty a je odstranit.</span><span class="sxs-lookup"><span data-stu-id="07552-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="07552-106">Práce s knihovnou uživatele bez oprávnění správce je pouze oprávnění číst dokumenty.</span><span class="sxs-lookup"><span data-stu-id="07552-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="07552-107">Autorizace je ortogonální a nezávislé na ověřování, což je proces pro zjišťování, který je uživatel.</span><span class="sxs-lookup"><span data-stu-id="07552-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="07552-108">Ověřování může vytvořit jeden nebo více identit pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="07552-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="07552-109">Typy autorizace</span><span class="sxs-lookup"><span data-stu-id="07552-109">Authorization Types</span></span>

<span data-ttu-id="07552-110">ASP.NET Core autorizaci poskytuje jednoduchý deklarativní [role](roles.md) a [bohaté na základě zásad](policies.md) modelu.</span><span class="sxs-lookup"><span data-stu-id="07552-110">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="07552-111">Autorizace je vyjádřeno v požadavcích a obslužné rutiny vyhodnotit deklarace identity uživatele podle požadavků.</span><span class="sxs-lookup"><span data-stu-id="07552-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="07552-112">Imperativní kontroly může být založen na jednoduchý zásad nebo zásad, které vyhodnocení identitu uživatele a vlastnosti prostředku, který uživatel se pokouší o přístup.</span><span class="sxs-lookup"><span data-stu-id="07552-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="07552-113">Jmenné prostory</span><span class="sxs-lookup"><span data-stu-id="07552-113">Namespaces</span></span>

<span data-ttu-id="07552-114">Součásti ověření, včetně `AuthorizeAttribute` a `AllowAnonymousAttribute` atributy, které se nacházejí v `Microsoft.AspNetCore.Authorization` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="07552-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
