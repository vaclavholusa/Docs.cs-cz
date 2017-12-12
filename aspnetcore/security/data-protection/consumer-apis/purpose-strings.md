---
title: "Účel řetězce"
author: rick-anderson
description: "Tento dokument podrobně popisuje, jak používá řetězce účel ochrany dat ASP.NET Core rozhraní API."
keywords: "ASP.NET Core, ochrany dat, účel řetězce"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c96ed361-c382-4980-8933-800e740cfc38
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 0d759937703d2a25604042b5e74e71155d635c1b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="purpose-strings"></a><span data-ttu-id="fa7e2-104">Účel řetězce</span><span class="sxs-lookup"><span data-stu-id="fa7e2-104">Purpose Strings</span></span>

<a name="data-protection-consumer-apis-purposes"></a>

<span data-ttu-id="fa7e2-105">Součásti, které využívají `IDataProtectionProvider` musí projít jedinečný *účely* parametru `CreateProtector` metoda.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-105">Components which consume `IDataProtectionProvider` must pass a unique *purposes* parameter to the `CreateProtector` method.</span></span> <span data-ttu-id="fa7e2-106">Účely *parametr* je vyplývajících pro zabezpečení systému ochrany dat, protože poskytuje izolaci mezi kryptografických příjemci, i v případě kořenové kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-106">The purposes *parameter* is inherent to the security of the data protection system, as it provides isolation between cryptographic consumers, even if the root cryptographic keys are the same.</span></span>

<span data-ttu-id="fa7e2-107">Pokud příjemce určuje účel, účel řetězec se používá spolu s kryptografickými klíči, které kořenové odvození kryptografických podklíče jedinečný pro tento příjemce.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-107">When a consumer specifies a purpose, the purpose string is used along with the root cryptographic keys to derive cryptographic subkeys unique to that consumer.</span></span> <span data-ttu-id="fa7e2-108">To izoluje příjemce z jiných kryptografických příjemce v aplikaci: žádné další součásti může číst jeho datové části a ho nelze číst datové části všechny ostatní součásti.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-108">This isolates the consumer from all other cryptographic consumers in the application: no other component can read its payloads, and it cannot read any other component's payloads.</span></span> <span data-ttu-id="fa7e2-109">Tato izolace také vykreslí je nemožné celý kategorie útoky na komponentu.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-109">This isolation also renders infeasible entire categories of attack against the component.</span></span>

![Příklad diagramu účel](purpose-strings/_static/purposes.png)

<span data-ttu-id="fa7e2-111">V diagramu výše `IDataProtector` instancí A a B **nelze** číst vzájemně datové části, jenom svoje vlastní.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-111">In the diagram above, `IDataProtector` instances A and B **cannot** read each other's payloads, only their own.</span></span>

<span data-ttu-id="fa7e2-112">Účel řetězec nemusí být skrytá.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-112">The purpose string doesn't have to be secret.</span></span> <span data-ttu-id="fa7e2-113">Jednoduše musí být jedinečný. v tom smyslu, že žádné další dobře behaved součást někdy zadejte stejný účel řetězec.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-113">It should simply be unique in the sense that no other well-behaved component will ever provide the same purpose string.</span></span>

>[!TIP]
> <span data-ttu-id="fa7e2-114">Použití oboru názvů a typ názvu komponenty využívání rozhraními API ochrany dat. je obvykle, stejně jako postup, který tyto informace se nikdy dojít ke konfliktu.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-114">Using the namespace and type name of the component consuming the data protection APIs is a good rule of thumb, as in practice this information will never conflict.</span></span>
>
><span data-ttu-id="fa7e2-115">Komponenta vytvořené Contoso, která je zodpovědná za minting nosné tokeny může použít Contoso.Security.BearerToken jako řetězec jeho účel.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-115">A Contoso-authored component which is responsible for minting bearer tokens might use Contoso.Security.BearerToken as its purpose string.</span></span> <span data-ttu-id="fa7e2-116">Nebo – i lépe - může použít Contoso.Security.BearerToken.v1 jako řetězec jeho účel.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-116">Or - even better - it might use Contoso.Security.BearerToken.v1 as its purpose string.</span></span> <span data-ttu-id="fa7e2-117">Připojování číslo verze umožňuje budoucí verze se má použít Contoso.Security.BearerToken.v2 jako její účel a různé verze by naprosto izolované od sebe navzájem, pokud jde o datové části přejděte.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-117">Appending the version number allows a future version to use Contoso.Security.BearerToken.v2 as its purpose, and the different versions would be completely isolated from one another as far as payloads go.</span></span>

<span data-ttu-id="fa7e2-118">Od parametru účely `CreateProtector` je pole řetězců, výše může místo toho zadáno jako `[ "Contoso.Security.BearerToken", "v1" ]`.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-118">Since the purposes parameter to `CreateProtector` is a string array, the above could have been instead specified as `[ "Contoso.Security.BearerToken", "v1" ]`.</span></span> <span data-ttu-id="fa7e2-119">To umožňuje vytvoření hierarchie pro účely a otevře možnost výskytu nekonzistentních víceklientský scénáře ochrany systémem data.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-119">This allows establishing a hierarchy of purposes and opens up the possibility of multi-tenancy scenarios with the data protection system.</span></span>

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> <span data-ttu-id="fa7e2-120">Součásti neměli povolit nedůvěryhodné uživatelský vstup jako jediný zdroj vstup pro účely řetězec.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-120">Components should not allow untrusted user input to be the sole source of input for the purposes chain.</span></span>
>
><span data-ttu-id="fa7e2-121">Představte si třeba součást Contoso.Messaging.SecureMessage, která je zodpovědná za ukládání zabezpečených zpráv.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-121">For example, consider a component Contoso.Messaging.SecureMessage which is responsible for storing secure messages.</span></span> <span data-ttu-id="fa7e2-122">Kdyby došlo k volání zabezpečené součástí zasílání zpráv `CreateProtector([ username ])`, pak uživatel se zlými úmysly může vytvořit účet s uživatelským jménem "Contoso.Security.BearerToken" pokus o získání komponentu volat `CreateProtector([ "Contoso.Security.BearerToken" ])`, což nechtěně způsobuje zabezpečené zasílání zpráv systém pro máta datových částí, které může být považována za tokeny ověřování.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-122">If the secure messaging component were to call `CreateProtector([ username ])`, then a malicious user might create an account with username "Contoso.Security.BearerToken" in an attempt to get the component to call `CreateProtector([ "Contoso.Security.BearerToken" ])`, thus inadvertently causing the secure messaging system to mint payloads that could be perceived as authentication tokens.</span></span>
>
><span data-ttu-id="fa7e2-123">Řetěz pro účely lepší pro komponentu zasílání zpráv by `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, který poskytuje správné izolace.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-123">A better purposes chain for the messaging component would be `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, which provides proper isolation.</span></span>

<span data-ttu-id="fa7e2-124">Izolace poskytované a chování `IDataProtectionProvider`, `IDataProtector`, a pro účely jsou následující:</span><span class="sxs-lookup"><span data-stu-id="fa7e2-124">The isolation provided by and behaviors of `IDataProtectionProvider`, `IDataProtector`, and purposes are as follows:</span></span>

* <span data-ttu-id="fa7e2-125">Pro danou `IDataProtectionProvider` objekt, `CreateProtector` metoda vytvoří `IDataProtector` objekt jednoznačně vázáno na obě `IDataProtectionProvider` objekt, který jej vytvořil a účely parametr, který byl předán do metody.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-125">For a given `IDataProtectionProvider` object, the `CreateProtector` method will create an `IDataProtector` object uniquely tied to both the `IDataProtectionProvider` object which created it and the purposes parameter which was passed into the method.</span></span>

* <span data-ttu-id="fa7e2-126">Účel parametr nesmí mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-126">The purpose parameter must not be null.</span></span> <span data-ttu-id="fa7e2-127">(Pokud účely je zadán jako pole, to znamená, že pole nesmí mít nulovou délku a všechny elementy pole musí obsahovat hodnotu null.) Prázdný řetězec účel technicky je povoleno, ale se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-127">(If purposes is specified as an array, this means that the array must not be of zero length and all elements of the array must be non-null.) An empty string purpose is technically allowed but is discouraged.</span></span>

* <span data-ttu-id="fa7e2-128">Dva účely argumenty jsou ekvivalentní a pouze v případě obsahují stejné řetězce (s použitím pořadí porovnávače) ve stejném pořadí.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-128">Two purposes arguments are equivalent if and only if they contain the same strings (using an ordinal comparer) in the same order.</span></span> <span data-ttu-id="fa7e2-129">Jednoúčelové argument je stejná jako odpovídající pole pro účely jeden element.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-129">A single purpose argument is equivalent to the corresponding single-element purposes array.</span></span>

* <span data-ttu-id="fa7e2-130">Dva `IDataProtector` jsou ekvivalentní objekty, pokud jsou vytvořené pomocí ekvivalentní `IDataProtectionProvider` objekty s parametry ekvivalentní účely.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-130">Two `IDataProtector` objects are equivalent if and only if they are created from equivalent `IDataProtectionProvider` objects with equivalent purposes parameters.</span></span>

* <span data-ttu-id="fa7e2-131">Pro danou `IDataProtector` objektu, volání `Unprotect(protectedData)` vrátí původní `unprotectedData` jenom v případě `protectedData := Protect(unprotectedData)` pro ekvivalentní `IDataProtector` objektu.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-131">For a given `IDataProtector` object, a call to `Unprotect(protectedData)` will return the original `unprotectedData` if and only if `protectedData := Protect(unprotectedData)` for an equivalent `IDataProtector` object.</span></span>

> [!NOTE]
> <span data-ttu-id="fa7e2-132">Jsme nejsou vzhledem k tomu tento případ, kdy některé součásti záměrně zvolí účel řetězce, který je znám v konfliktu s jinou součástí.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-132">We're not considering the case where some component intentionally chooses a purpose string which is known to conflict with another component.</span></span> <span data-ttu-id="fa7e2-133">Tato součást bude v podstatě považováno za škodlivý a tento systém není určená k poskytování záruky zabezpečení, v případě, že škodlivý kód je již spuštěna v rámci pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="fa7e2-133">Such a component would essentially be considered malicious, and this system is not intended to provide security guarantees in the event that malicious code is already running inside of the worker process.</span></span>
