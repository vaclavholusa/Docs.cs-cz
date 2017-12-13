---
title: "Úvod do autorizace"
author: rick-anderson
description: "Tento dokument poskytuje základní vysvětlení autorizace a vysvětluje, jak autorizace souvisí s ASP.NET Core."
keywords: ASP.NET Core, autorizace
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 192cc494c2378f77207a4b1c17b0b0a73ca642ed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction"></a><span data-ttu-id="f835f-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="f835f-104">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="f835f-105">Autorizace odkazuje na proces, který určuje, co je uživatel moct provádět.</span><span class="sxs-lookup"><span data-stu-id="f835f-105">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="f835f-106">Administrativní uživatel může například vytvořit knihovnu dokumentů, přidat dokumenty, upravovat dokumenty a je odstranit.</span><span class="sxs-lookup"><span data-stu-id="f835f-106">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="f835f-107">Práce s knihovnou uživatele bez oprávnění správce je pouze oprávnění číst dokumenty.</span><span class="sxs-lookup"><span data-stu-id="f835f-107">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="f835f-108">Autorizace je ortogonální a nezávislé na ověřování, což je proces pro zjišťování, který je uživatel.</span><span class="sxs-lookup"><span data-stu-id="f835f-108">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="f835f-109">Ověřování může vytvořit jeden nebo více identit pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="f835f-109">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="f835f-110">Typy autorizace</span><span class="sxs-lookup"><span data-stu-id="f835f-110">Authorization Types</span></span>

<span data-ttu-id="f835f-111">ASP.NET Core autorizaci poskytuje jednoduchý deklarativní [role](roles.md) a [bohaté na základě zásad](policies.md) modelu.</span><span class="sxs-lookup"><span data-stu-id="f835f-111">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="f835f-112">Autorizace je vyjádřeno v požadavcích a obslužné rutiny vyhodnotit deklarace identity uživatele podle požadavků.</span><span class="sxs-lookup"><span data-stu-id="f835f-112">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="f835f-113">Imperativní kontroly může být založen na jednoduchý zásad nebo zásad, které vyhodnocení identitu uživatele a vlastnosti prostředku, který uživatel se pokouší o přístup.</span><span class="sxs-lookup"><span data-stu-id="f835f-113">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="f835f-114">Obory názvů</span><span class="sxs-lookup"><span data-stu-id="f835f-114">Namespaces</span></span>

<span data-ttu-id="f835f-115">Součásti ověření, včetně `AuthorizeAttribute` a `AllowAnonymousAttribute` atributy, které se nacházejí v `Microsoft.AspNetCore.Authorization` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="f835f-115">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
