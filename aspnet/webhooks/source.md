---
uid: webhooks/source
title: ASP.NET – Webhooky zdrojový kód a balíčky NuGet | Dokumentace Microsoftu
author: rick-anderson
description: Odkazy na zdrojový kód ASP.NET – Webhooky a balíčky NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ff716b476f7dc69b6071d3febd5b5871e4f02689
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752639"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET – Webhooky zdrojový kód a balíčky NuGet

Microsoft ASP.NET WebHooks je součástí Microsoft ASP.NET řadu modulů a je hostovaný jako [Open Source projekt na Githubu](https://github.com/aspnet/WebHooks). To znamená, že jsme přijímat příspěvky, ale podívejte se prosím na [příspěvek pokyny](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) před odesláním žádosti o přijetí změn.

Tuto dokumentaci online, které právě čtete nyní také hostována jako [Open Source na Githubu](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) a také přijímá příspěvky.

## <a name="nuget-packages"></a>Balíčky NuGet

[Balíčky NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) jsou rozdělené do tří částí:

* [Běžné](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): common balíček, který je sdílen mezi odesílateli a příjemci.

* [Odesílatel](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): sadu balíčků, které podporuje odesílání vlastních Webhooků do jiné. Funkce pro odesílání Webhooky jsou popsány podrobněji [odesílání Webhooky](sending/index.md).

* [Příjemci](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): sadu balíčků, podpora Webhooků příjem od ostatních. Funkce pro příjem Webhooky jsou popsány podrobněji [přijímá Webhooky](receiving/index.md).
