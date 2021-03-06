---
title: DevOps s využitím ASP.NET Core a Azure | Nástroje a soubory ke stažení
author: CamSoper
description: Průvodce, který poskytuje pokyny k začátku do konce na vytváření procesních toků pro DevOps pro aplikace ASP.NET Core hostované v Azure.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 573e257e6fc7614010a8749ff439f16011c2c10a
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089378"
---
# <a name="tools-and-downloads"></a>Nástroje a soubory ke stažení

Azure nabízí několik rozhraní pro zřizování a správu prostředků, jako [webu Azure portal](https://portal.azure.com), [rozhraní příkazového řádku Azure](/cli/azure/), [prostředí Azure PowerShell](/powershell/azure/overview), [cloudu Azure Prostředí](https://shell.azure.com/bash)a sady Visual Studio. Tato příručka používá minimalist přístup a používá Azure Cloud Shell pokaždé, když je možné snížit kroky. Na webu Azure portal musí být však použít pro některé části.

## <a name="prerequisites"></a>Požadavky

Vyžadují se následující předplatná:

* Azure &mdash; Pokud nemáte účet, [získat bezplatnou zkušební verzi](https://azure.microsoft.com/free/).
* Služby Azure DevOps &mdash; vaše předplatné Azure DevOps a organizace se vytvoří v kapitole 4.
* GitHub &mdash; Pokud nemáte účet, [ZDARMA zaregistrovat](https://github.com/join).

Tyto nástroje jsou požadovány:

* [Git](https://git-scm.com/downloads) &mdash; základní principy Git se doporučuje pro tohoto průvodce. Zkontrolujte [dokumentace pro Git](https://git-scm.com/doc), konkrétně [vzdálené úložiště git](https://git-scm.com/docs/git-remote) a [git push](https://git-scm.com/docs/git-push).
* [Sada .NET core SDK](https://www.microsoft.com/net/download/) &mdash; verze 2.1.300 nebo novější je potřebné k sestavení a spuštění ukázkové aplikace. Pokud je nainstalována aplikace Visual Studio s **vývoj pro různé platformy .NET Core** již je nainstalovaná úloha, sada .NET Core SDK.

    Ověření instalace .NET Core SDK. Otevřete příkazové okno a spusťte následující příkaz:

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Doporučené nástroje (jenom Windows)

* [Visual Studio](https://www.visualstudio.com/)uživatele robustní nástroje Azure poskytuje grafické uživatelské rozhraní pro většinu funkcí popsaných v této příručce. Bude fungovat jakékoli edici sady Visual Studio, včetně bezplatná edice Visual Studio Community. V kurzech se zapisují do ukazují vývoje, nasazení a DevOps s i bez sady Visual Studio.

  Potvrďte, že sada Visual Studio obsahuje následující [úlohy](/visualstudio/install/modify-visual-studio) nainstalovaný:

  * Vývoj pro ASP.NET a web
  * Vývoj pro Azure
  * Vývoj pro různé platformy .NET core
