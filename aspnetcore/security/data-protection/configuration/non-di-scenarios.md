---
title: "Scénáře využívající bez DI ochrany dat v ASP.NET Core"
author: rick-anderson
description: "Zjistěte, jak pro podporu scénáře ochrany dat, kde nemůžete nebo nechcete použít službu poskytované vkládání závislostí."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: eccf914d20e04adbb113f17e262766ed2dd1a554
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Scénáře využívající bez DI ochrany dat v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Ochrana dat ASP.NET Core systému je obvykle [přidat do kontejneru služby](xref:security/data-protection/consumer-apis/overview) a spotřebovávají závislé součásti pomocí vkládání závislostí (DI). Ale existují případy, kdy to není v rámci výpočetních procesů nebo požadovaným způsobem, zejména v případě, že import systému do stávající aplikace.

Pro podporu těchto scénářů [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) balíček poskytuje na konkrétní typ. [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), který nabízí jednoduchý způsob, jak používat ochranu dat bez spoléhání na DI. `DataProtectionProvider` Zadejte implementuje [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Vytváření `DataProtectionProvider` vyžaduje pouze, poskytování [DirectoryInfo](/dotnet/api/system.io.directoryinfo) instance k označení, které se má uložit šifrovací klíče poskytovatele, jak je vidět v následující ukázce kódu:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

Ve výchozím nastavení `DataProtectionProvider` konkrétní typ nepodporuje šifrování nezpracovaná materiál klíče je před uložením do systému souborů. To je podpora scénáře, kde vývojáře odkazuje na sdílené síťové složky a ochrana dat systému nelze odvodit automaticky mechanismus šifrování pomocí klíče příslušné na rest.

Kromě toho `DataProtectionProvider` konkrétní typ není [izolovat aplikace](xref:security/data-protection/configuration/overview#per-application-isolation) ve výchozím nastavení. Všechny aplikace pomocí stejného klíče adresáře můžete sdílet stejně dlouho jako datové části jejich [účel parametry](xref:security/data-protection/consumer-apis/purpose-strings) shodovat.

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) konstruktor přijímá volitelné konfiguraci zpětného volání, které je možné upravit chování systému. Následující ukázka ukazuje obnovení izolace s explicitní volání [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). Ukázka také ukazuje, konfigurace systému k automatickému šifrování trvalou klíče pomocí rozhraní Windows DPAPI. Pokud adresář odkazuje na sdílenou jednotku UNC, můžete distribuovat certifikát sdílené mezi všechny relevantní počítače a konfigurace systému pro použití šifrování založené na certifikátech s volání [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Instance `DataProtectionProvider` konkrétní typ jsou nákladné. Pokud aplikace udržuje více instancí tohoto typu a pokud se používá stejný adresář úložiště klíčů, může způsobit snížení výkonu aplikace. Pokud použijete `DataProtectionProvider` typu, doporučujeme po vytvoření tohoto typu a opakovaně ji co nejvíc používat. `DataProtectionProvider` Typu a všechny [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) vytvořit z něj instancí jsou bezpečné pro přístup z více vláken pro více volající.
