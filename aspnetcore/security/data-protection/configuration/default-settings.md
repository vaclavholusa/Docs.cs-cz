---
title: "Správa klíčů ochrany dat a doba platnosti v ASP.NET Core"
author: rick-anderson
description: "Další informace o ochranu dat správy klíčů a doba platnosti v ASP.NET Core."
keywords: "ASP.NET Core klíče management, rozhraní DPAPI, ochrany dat, doba platnosti klíče"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 4f5409acf4d934ced828153ccfd945834d0f1718
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Správa klíčů ochrany dat a doba platnosti v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Správa klíčů

Aplikace se pokusí zjistit jeho provozní prostředí a zpracování konfigurace klíče svoje vlastní.

1. Pokud je aplikace hostovaná v [Azure Apps](https://azure.microsoft.com/services/app-service/), klíče jsou nastavené jako trvalé k *%HOME%\ASP.NET\DataProtection-Keys* složky. Tato složka je zálohovaný díky síťového úložiště a se synchronizují napříč všechny počítače, které hostují aplikaci.
   * Klíče nejsou chráněné v klidovém stavu.
   * *DataProtection klíče* složky poskytuje prstenec klíč na všechny instance aplikace v jednom nasazovací slot.
   * Samostatné nasazovací sloty, jako je například pracovní a provozní, Nesdílejte prstenec klíč. Při výměně mezi sloty nasazení, například odkládací pracovní do produkčního prostředí nebo použití A / B testování, všech aplikací pomocí funkce Ochrana dat nebude moci dešifrovat uložená data pomocí klíče prstenec uvnitř předchozí pozici. To vede k uživatelé protokolována mimo aplikaci, která používá standardní ověřování souborů cookie ASP.NET Core, protože využívá ochranu dat k ochraně jeho soubory cookie. Vyžadujete, aby okruhy klíč nezávislé na pozici,-li použít zprostředkovatel vnější prstenec klíč, například Azure Blob Storage, Azure Key Vault, úložiště SQL nebo Redis cache.

1. Pokud profil uživatele je k dispozici, klíče, jsou nastavené jako trvalé k *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* složky. Pokud operační systém Windows, klíče jsou zašifrovaná přinejmenším pomocí rozhraní DPAPI.

1. Pokud je aplikace hostované ve službě IIS, jsou trvalé klíče registru HKLM klíč speciální registru, který je ACLed pouze pro účet pracovního procesu. Klíče jsou zašifrovaná přinejmenším pomocí rozhraní DPAPI.

1. Pokud žádná z těchto podmínek shodují, klíče nejsou trvalé mimo aktuální proces. Když se proces ukončí, všechny vygenerované klíče jsou ztraceny.

Vývojář je vždy plně pod kontrolou a můžete přepsat, jak a kde jsou uložené klíče. První tři výše uvedených možností by měl poskytovat dobrý výchozí hodnoty pro většinu aplikací podobná jak ASP.NET  **\<machineKey >** automatické generování rutiny fungovaly v minulosti. Možnost konečné, záložní je jenom scénáře, který vyžaduje vývojáři zadejte [konfigurace](xref:security/data-protection/configuration/overview) předem, pokud chtějí klíče trvalost, ale tento záložní dochází pouze ve výjimečných případech.

Při hostování v kontejner Docker, klíče by měl natrvalo ve složce, kterou je svazek Docker (sdíleného svazku nebo svazku připojené hostitele, který potrvají nad rámec kontejneru životnost) nebo do externího poskytovatele, jako například [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) nebo [Redis](https://redis.io/). Externího poskytovatele je užitečný ve scénářích, webové farmy také pokud aplikace nemá přístup k sdílené síťové svazek (viz [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) informace).

> [!WARNING]
> Pokud vývojář přepsání pravidel uvedených výše a bodů systému ochrany dat na konkrétní úložiště klíčů, je zakázané automatické šifrování klíče v klidovém stavu. Může být znovu zapnout ochranu v rest [konfigurace](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Doba platnosti klíče

Klíče měly životnost-90denní ve výchozím nastavení. Když vyprší platnost klíče, aplikace se automaticky vygeneruje nový klíč a nastaví nového klíče jako aktivní klíč. Tak dlouho, dokud vyřazeno klíče zůstávají v systému, aplikace mohly dešifrovat všechna data chráněná s nimi. V tématu [Správa klíčů](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) Další informace.

## <a name="default-algorithms"></a>Výchozí algoritmy

Použitý algoritmus ochrany výchozí datové části pro utajení a HMACSHA256 je AES-256-CBC pro pravosti. Hlavní klíč 512 bitů, změnit každých 90 dní, slouží k dvě dílčí klíče používané pro tyto algoritmy na základě za datové odvozena. V tématu [podklíčů odvození](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) Další informace.

## <a name="see-also"></a>Viz také

* [Rozšíření správy klíčů](xref:security/data-protection/extensibility/key-management)
