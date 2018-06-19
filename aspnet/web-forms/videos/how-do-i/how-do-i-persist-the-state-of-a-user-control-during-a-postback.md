---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[Jak na to]: zachování stavu uživatelského ovládacího prvku během zpětné volání | Microsoft Docs'
author: rick-anderson
description: V této video PEL Jan ukazuje, jak se zachovat stav jednoho nebo více objektů v uživatelského ovládacího prvku. Nejprve je vytvořen uživatelský ovládací prvek, který představuje abilit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572095"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Jak na to]: zachování stavu uživatelského ovládacího prvku během zpětné volání
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="7dd3b-105">podle [PEL Jan](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="7dd3b-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="7dd3b-106">V této video PEL Jan ukazuje, jak se zachovat stav jednoho nebo více objektů v uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="7dd3b-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="7dd3b-107">Nejprve je vytvořen uživatelský ovládací prvek, který představuje možnost pro uživatele k zadání filtru kritéria hledání.</span><span class="sxs-lookup"><span data-stu-id="7dd3b-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="7dd3b-108">Kromě toho se vytvoří doprovodné třídy filtru uložit informace o filtru.</span><span class="sxs-lookup"><span data-stu-id="7dd3b-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="7dd3b-109">Několik prvky uživatelského rozhraní se přidají do ovládacího prvku filtru společně s některé metody a vlastnosti, které chcete uložit aktuální informace o filtru v instanci třídy filtru.</span><span class="sxs-lookup"><span data-stu-id="7dd3b-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="7dd3b-110">Trvalost ovládacích uživatele v dalším kroku je implementovaná pomocí RegisterRequiresControlState metoda a související metody uložit/obnovit.</span><span class="sxs-lookup"><span data-stu-id="7dd3b-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="7dd3b-111">Tyto metody uložit instanci třídy filtru a jeho data během postback stránky.</span><span class="sxs-lookup"><span data-stu-id="7dd3b-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="7dd3b-112">Nakonec je diskuzi o tom, jak ukládat více objektů v implementaci stavu ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="7dd3b-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="7dd3b-113">&#9654; Podívejte se na video (23 minuty)</span><span class="sxs-lookup"><span data-stu-id="7dd3b-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
