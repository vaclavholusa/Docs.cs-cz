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
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="13b40-103">Úvod do autorizace v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13b40-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="13b40-104">Autorizace odkazuje na proces, který určuje, co je uživatel moct provádět.</span><span class="sxs-lookup"><span data-stu-id="13b40-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="13b40-105">Administrativní uživatel může například vytvořit knihovnu dokumentů, přidat dokumenty, upravovat dokumenty a je odstranit.</span><span class="sxs-lookup"><span data-stu-id="13b40-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="13b40-106">Práce s knihovnou uživatele bez oprávnění správce je pouze oprávnění číst dokumenty.</span><span class="sxs-lookup"><span data-stu-id="13b40-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="13b40-107">Autorizace je ortogonální a nezávislé na ověřování.</span><span class="sxs-lookup"><span data-stu-id="13b40-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="13b40-108">Autorizace však vyžaduje ověřovací mechanismus.</span><span class="sxs-lookup"><span data-stu-id="13b40-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="13b40-109">Ověřování je proces zjišťování, který je uživatel.</span><span class="sxs-lookup"><span data-stu-id="13b40-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="13b40-110">Ověřování může vytvořit jeden nebo více identit pro aktuálního uživatele.</span><span class="sxs-lookup"><span data-stu-id="13b40-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="13b40-111">Typy autorizace</span><span class="sxs-lookup"><span data-stu-id="13b40-111">Authorization types</span></span>

<span data-ttu-id="13b40-112">ASP.NET Core autorizaci poskytuje jednoduchý deklarativní [role](xref:security/authorization/roles) a s formátováním [na základě zásad](xref:security/authorization/policies) modelu.</span><span class="sxs-lookup"><span data-stu-id="13b40-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="13b40-113">Autorizace je vyjádřeno v požadavcích a obslužné rutiny vyhodnotit deklarace identity uživatele podle požadavků.</span><span class="sxs-lookup"><span data-stu-id="13b40-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="13b40-114">Imperativní kontroly může být založen na jednoduchý zásad nebo zásad, které vyhodnocení identitu uživatele a vlastnosti prostředku, který uživatel se pokouší o přístup.</span><span class="sxs-lookup"><span data-stu-id="13b40-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="13b40-115">Jmenné prostory</span><span class="sxs-lookup"><span data-stu-id="13b40-115">Namespaces</span></span>

<span data-ttu-id="13b40-116">Součásti ověření, včetně `AuthorizeAttribute` a `AllowAnonymousAttribute` atributy, se nacházejí v `Microsoft.AspNetCore.Authorization` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="13b40-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="13b40-117">Informace naleznete v dokumentaci na [jednoduché autorizace](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="13b40-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
