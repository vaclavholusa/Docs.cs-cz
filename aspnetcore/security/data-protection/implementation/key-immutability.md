---
title: "Klíče neměnitelnosti a změna nastavení"
author: rick-anderson
description: "Tento dokument popisuje podrobnosti implementace Komponenta ASP.NET Core data protection klíče neměnitelnosti rozhraní API."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 425b8ba9769c2b5ac635693b045e52c110f25205
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="key-immutability-and-changing-settings"></a>Klíč neměnitelnosti a změna nastavení

Jakmile je objekt trvalý k záložnímu úložišti, její reprezentace navždy vyřešen. Nová data mohou být přidány do úložiště zálohování, ale může být mutovat nikdy stávající data. Primárním účelem toto chování je zabránit poškození dat.

Jedním z důsledků toto chování je, že jakmile klíč je zapsán do úložiště zálohování, se nedá změnit. Jeho vytvoření, aktivace a datum vypršení platnosti nesmí nikdy změnit, i když můžete odvolat pomocí `IKeyManager`. Kromě toho jeho základní algoritmické informace, jejich obsah a šifrování na rest vlastnosti jsou také neměnné.

Pokud se vývojáři změní všechna nastavení, která ovlivňuje klíče trvalost, tyto změny se projeví až po příštím klíče se vygeneruje, buď prostřednictvím explicitní volání `IKeyManager.CreateNewKey` nebo přes data protection systému vlastní [automatické klíč generování](key-management.md#data-protection-implementation-key-management) chování. Nastavení, které ovlivňují klíče trvalost jsou následující:

* [Výchozí doba života klíče](key-management.md#data-protection-implementation-key-management)

* [Šifrování klíče v mechanismus rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [Algoritmické informace obsažené v rámci klíč](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Pokud potřebujete těchto nastavení se nové starší než další klíč automatické vrácení čas, zvažte provedení explicitní volání `IKeyManager.CreateNewKey` k vytvoření nového klíče. Nezapomeňte zadat datum explicitní aktivace ({nyní + 2 dny} je obvykle umožňující čas změna rozšířit) a datum vypršení platnosti ve volání.

>[!TIP]
> Všechny aplikace dotykové ovládání úložiště by měl určovat stejným nastavením `IDataProtectionBuilder` rozšiřující metody. Vlastnosti trvalou klíč, jinak budou závislé na konkrétní aplikaci, která volá rutiny generování klíče.
