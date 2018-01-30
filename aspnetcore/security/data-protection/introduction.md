---
title: "Úvod do ochrany dat"
author: rick-anderson
description: "Tento dokument zavádí koncepci ochrany dat a popisuje zásady přidružené rozhraní API ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/introduction
ms.openlocfilehash: acd38679390b92705703111b72816f1a5d3ba848
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-data-protection"></a>Úvod do ochrany dat

Webové aplikace často potřebují k ukládání dat citlivé na zabezpečení. Windows poskytuje rozhraní DPAPI pro aplikací klasické pracovní plochy, ale to není vhodná pro webové aplikace. Zásobník ochrany dat ASP.NET Core poskytují jednoduchý a snadno použitelný API kryptografických vývojář může použít k ochraně dat, včetně správy klíčů a otočení.

Zásobník ochrany dat ASP.NET Core slouží k sloužit jako dlouhodobé náhradou <machineKey> element technologie ASP.NET 1.x - 4.x. Byla navržená tak, aby vyřešit řadu nedostatků starý kryptografický zásobníku při současném poskytování out-of-the-box řešení pro většinu případů použití, které moderní aplikace jsou setkat.

## <a name="problem-statement"></a>Stanovení problému

Příkaz celkový problém, můžete stručně uvedená v jedné větě: je nutné zachovat důvěryhodné informace pro pozdější načtení, ale I nedůvěřujete mechanismus trvalosti. V podmínkách webové to může být zapsána jako "Potřebuji odezvy důvěryhodného stavu prostřednictvím nedůvěryhodného klienta."

Kanonický příklad tohoto objektu je soubor cookie pro ověřování nebo nosiče tokenu. Generuje server "Jsem Groot a mít oprávnění xyz" token a předá ho do klienta. Někdy v budoucnu nevypnete Klient nabídne tento token zpět na server, ale server potřebuje nějaký druh záruku, že klient nebyl forged token. Proto první požadavek: pravosti (také známa jako integritu, zfalšování kontroly pravopisu systému).

Vzhledem k tomu, že trvalého stavu je důvěryhodný pro server, Očekáváme, že tento stav může obsahovat informace, které jsou specifické pro provozní prostředí. To může být ve tvaru cestu k souboru, oprávnění, popisovač nebo nepřímý odkaz na jiné a některé další část dat specifickou pro server. Tyto informace by neměl být poskytnuty obecně nedůvěryhodného klienta. Proto požadavek na druhý: utajení.

Nakonec od moderní aplikace jsou komponentizované, co jste viděli je, budou jednotlivé komponenty chcete využít tento systém bez ohledu na ostatní součásti v systému. Například pokud součást tokenu nosiče používá tento zásobníku, ho pracovat bez rušení mechanismus anti-proti útokům CSRF, která by mohla využívat se stejným zásobníkem. Proto požadavek na konečné: izolace.

Můžeme poskytnout další omezení chcete-li zúžit rozsah naše požadavky. Předpokládáme, že jsou všechny služby, které pracují v rámci cryptosystem stejně důvěryhodné a že data nemusí být generována nebo využívat mimo služby v našem přímou kontrolu. Kromě toho je nutné, operace jsou tak rychlý jako možné vzhledem k tomu, že každý požadavek pro webovou službu projít cryptosystem jeden či více krát. Díky tomu symetrické šifrování ideální pro náš scénář a jsme slevy asymetrické šifrování, dokud například čas, který je potřeba.

## <a name="design-philosophy"></a>Filosofie návrhu tříd

Spuštění tím, že určíte problémy s existující zásobníku. Jakmile jsme měli který, jsme názory povahu existující řešení a dospělo k závěru, že žádné existující řešení poměrně měl možnostem, které jsme žádá o. Potom jsme analyzovány řešení založené na několik hlavních zásad.

* Systém by měl nabízejí zjednodušení konfigurace. V ideálním případě systém by bez nutnosti konfigurace a vývojáři mohou dosáhl základů systémem. V situacích, kdy je potřeba nakonfigurovat konkrétní aspekt (například klíče úložiště) vývojáři by měla přihlédnout k provedení těchto konkrétní konfigurace jednoduché.

* Nabízí jednoduché rozhraní API určených. Rozhraní API by měl být snadno použitelné správně a obtížně použitelný nesprávně.

* Vývojáři by neměl další zásady správy klíčů. Systém by měla řídit výběr algoritmus a životnosti klíče jménem pro vývojáře. V ideálním případě by měl vývojář nikdy i mít přístup k nezpracované materiál klíče.

* Klíčů by měly být chráněné v klidovém stavu, pokud je to možné. Systém by měl zjistit mechanismus ochrany odpovídající výchozí a použít automaticky.

S těmito zásadami pamatovat vyvinuli jednoduchý, [snadno použitelný](using-data-protection.md) data protection zásobníku.

Ochrana dat ASP.NET Core rozhraní API nejsou primárně určený pro neomezené trvalost důvěrné datové části. Další technologie, jako [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) a [Azure Rights Management](https://docs.microsoft.com/rights-management/) jsou vhodnější ve scénáři neomezené úložiště, a mají možnosti odpovídajícím způsobem silné správy klíčů. Ale nutné dodat, není nic zakazují vývojář pomocí funkce Ochrana dat ASP.NET Core rozhraní API pro dlouhodobou ochranu důvěrných údajů.

## <a name="audience"></a>Cílová skupina

Systém ochrany dat je rozdělené do pěti hlavní balíčky. Různé aspekty tato rozhraní API cíle tři hlavní cílové skupiny;

1. [Příjemce rozhraní API přehled](consumer-apis/overview.md) cílové aplikace a framework vývojáři.

   "Nechci Další informace o tom, jak funguje v zásobníku nebo o tom, jak je nakonfigurovaný. Jednoduše chcete provádět některé operace v jako jednoduchý takovým způsobem, který nejdříve s velkou pravděpodobností úspěšně pomocí rozhraní API."

2. [Rozhraní API pro konfiguraci](configuration/overview.md) cílové aplikace vývojáři a správci systému.

   "Potřebuji systém ochrany dat říct, že Moje prostředí vyžaduje nastavení nebo jiné než výchozí cesty."

3. Rozšiřitelnost vývojáři cílové rozhraní API starosti implementace vlastních zásad. Využití těchto rozhraní API by omezena na velmi zřídka a došlo, vývojáři vědět zabezpečení.

   "Potřebuji nahradit komponentu celý v rámci systému, protože chování skutečně jedinečné požadavky. Chci se další uncommonly používaných částí plochy rozhraní API k sestavení modulu plug-in, který splňuje mé požadavky."

## <a name="package-layout"></a>Balíček rozložení

Zásobník ochrany dat se skládá z pěti balíčky.

* Microsoft.AspNetCore.DataProtection.Abstractions obsahuje základní rozhraní IDataProtectionProvider a IDataProtector. Obsahuje taky užitečné rozšiřující metody, které mohou pomoci práci s těmito typy (například přetížení IDataProtector.Protect). Najdete v části příjemce rozhraní pro další informace. Pokud někdo jiný zodpovídá za vytvoření instance systému ochrany dat a jednoduše spotřebovávají rozhraní API, budete chtít odkaz Microsoft.AspNetCore.DataProtection.Abstractions.

* Microsoft.AspNetCore.DataProtection obsahuje základní implementace systému ochrany dat, včetně základních kryptografických operací, správu klíčů, konfigurace a rozšíření. Pokud jste zodpovědný za vytváření instancí systému ochrany dat (například přidáním do IServiceCollection) nebo změnou nebo rozšíření své chování, budete chtít odkaz Microsoft.AspNetCore.DataProtection.

* Microsoft.AspNetCore.DataProtection.Extensions obsahuje další rozhraní API, které vývojáři mohou být užitečné, ale které nepatří do základní balíček. Například tento balíček obsahuje jednoduché "doložit systému odkazující na adresář konkrétní úložiště klíčů se žádné nastavení vkládání závislostí" rozhraní API (Další informace o). Také obsahuje rozšiřující metody pro omezení délky trvání chráněných datových částí (Další informace o).

* Microsoft.AspNetCore.DataProtection.SystemWeb lze nainstalovat do stávající aplikaci ASP.NET 4.x přesměrování jeho <machineKey> na místo toho použijte novým zásobníkem ochrany dat operací. V tématu [kompatibility](compatibility/replacing-machinekey.md#compatibility-replacing-machinekey) Další informace.

* Microsoft.AspNetCore.Cryptography.KeyDerivation poskytuje implementaci pro hashování rutiny hesel PBKDF2 a mohou být využívána systémy, které je třeba zpracovat uživatelská hesla bezpečně. V tématu [Hashování hesel](consumer-apis/password-hashing.md) Další informace.
