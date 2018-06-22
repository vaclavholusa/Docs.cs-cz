---
title: Klíč nastavení neměnitelnosti a klíč v ASP.NET Core
author: rick-anderson
description: Další podrobnosti implementace klíče neměnitelnosti ochranu dat ASP.NET jádra rozhraní API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 45460e0bdf6ad0a978f984d55b64f562c13fb575
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279451"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Klíč nastavení neměnitelnosti a klíč v ASP.NET Core

Jakmile je objekt trvalý k záložnímu úložišti, její reprezentace navždy vyřešen. Nová data mohou být přidány do úložiště zálohování, ale může být mutovat nikdy stávající data. Primárním účelem toto chování je zabránit poškození dat.

Jedním z důsledků toto chování je, že jakmile klíč je zapsán do úložiště zálohování, se nedá změnit. Jeho vytvoření, aktivace a datum vypršení platnosti nesmí nikdy změnit, i když můžete odvolat pomocí `IKeyManager`. Kromě toho jeho základní algoritmické informace, jejich obsah a šifrování na rest vlastnosti jsou také neměnné.

Pokud se vývojáři změní všechna nastavení, která ovlivňuje klíče trvalost, tyto změny se projeví až po příštím klíče se vygeneruje, buď prostřednictvím explicitní volání `IKeyManager.CreateNewKey` nebo přes data protection systému vlastní [automatické klíč generování](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) chování. Nastavení, které ovlivňují klíče trvalost jsou následující:

* [Výchozí doba života klíče](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Šifrování klíče v mechanismus rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [Algoritmické informace obsažené v rámci klíč](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Pokud potřebujete těchto nastavení se nové starší než další klíč automatické vrácení čas, zvažte provedení explicitní volání `IKeyManager.CreateNewKey` k vytvoření nového klíče. Nezapomeňte zadat datum explicitní aktivace ({nyní + 2 dny} je obvykle umožňující čas změna rozšířit) a datum vypršení platnosti ve volání.

>[!TIP]
> Všechny aplikace dotykové ovládání úložiště by měl určovat stejným nastavením `IDataProtectionBuilder` rozšiřující metody. Vlastnosti trvalou klíč, jinak budou závislé na konkrétní aplikaci, která volá rutiny generování klíče.
