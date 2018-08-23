---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: V toto video pixelů na Chris ukazuje, jak k uložení stavu jednoho nebo více objektů do uživatelského ovládacího prvku. Nejprve se vytvoří uživatelský ovládací prvek, který představuje abilit...
ms.author: riande
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: b15eef0af3e88f8ca333d9661c5d42a1dbc90151
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752610"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[Jak na to]: zachování stavu uživatelského ovládacího prvku během zpětné volání
[How Do I]: Persist the State of a User Control During a Postback
====================
<span data-ttu-id="108f8-104">podle [Chris pixelů na](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="108f8-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="108f8-105">V toto video pixelů na Chris ukazuje, jak k uložení stavu jednoho nebo více objektů do uživatelského ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="108f8-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="108f8-106">Nejprve se vytvoří uživatelský ovládací prvek, který představuje možnost pro uživatele k určení filtrovacích kritérií vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="108f8-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="108f8-107">Kromě toho doprovodné třídy filtru je vytvořili pro uložení informace o filtru.</span><span class="sxs-lookup"><span data-stu-id="108f8-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="108f8-108">Několik prvků uživatelského rozhraní jsou přidány do ovládací prvek filtru spolu s některé metody a vlastnosti, které chcete uložit aktuální informace o filtru ve filtru instance třídy.</span><span class="sxs-lookup"><span data-stu-id="108f8-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="108f8-109">Trvalost uživatelského ovládacího prvku v dalším kroku je implementováno pomocí RegisterRequiresControlState metodu a související metody uložit/obnovit.</span><span class="sxs-lookup"><span data-stu-id="108f8-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="108f8-110">Tyto metody ukládání instance třídy filtru a jeho dat během zpětného odeslání stránky.</span><span class="sxs-lookup"><span data-stu-id="108f8-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="108f8-111">Nakonec je diskusi o tom, jak ukládat více objektů v implementaci stavu ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="108f8-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="108f8-112">&#9654;Podívejte se na video (23 minut)</span><span class="sxs-lookup"><span data-stu-id="108f8-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
