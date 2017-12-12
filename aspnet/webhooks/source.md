---
uid: webhooks/source
title: "ASP.NET Webhooky zdrojového kódu a balíčky NuGet | Microsoft Docs"
author: rick-anderson
description: "Odkazy na ASP.NET Webhooky zdrojového kódu a balíčky NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET Webhooky zdrojového kódu a balíčky NuGet

Microsoft ASP.NET WebHooks je součástí rodiny Microsoft ASP.NET modulů a je hostovaná jako [otevřený zdroj projektu na Githubu](https://github.com/aspnet/WebHooks). To znamená, že jsme přijímat příspěvky, ale podívejte se prosím na [příspěvku pokyny](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) před odesláním žádost o přijetí změn.

Tato dokumentace online, které právě čtete teď se hostují taky jako [Open Source na Githubu](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) a také přijímá příspěvky.

## <a name="nuget-packages"></a>Balíčky Nuget

Microsoft ASP.NET WebHooks je také k dispozici, protože balíčky Nuget, které znamená, že budete mít v sadě Visual Studio vyberte příznak Preview, chcete-li zobrazit jejich náhled.

[Balíčky Nuget](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) jsou devided do tří částí:

* [Běžné](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): běžné balíčku, jež jsou sdílena mezi odesílateli a příjemci.

* [Odesílatel](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Sada balíčky podpora odeslání vlastní Webhooky ostatním. Funkce pro odesílání Webhooky je podrobně popsaná v další v [odesílání Webhooky](sending/index.md).

* [Příjemci](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Sada balíčky podpora přijímání Webhooky od ostatních uživatelů. Funkce pro příjem Webhooky jsou popsány podrobněji v [přijetí Webhooky](receiving/index.md).
