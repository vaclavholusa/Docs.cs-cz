---
uid: whitepapers/ms03-32-issue
title: Oprava chyby "Serverová aplikace není k dispozici" po použití aktualizace zabezpečení pro IE | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje opravu, která řeší problém s MS03 32 aktualizace zabezpečení pro Internet Explorer, který má vliv na aplikace ASP.NET 1.0 běžící na Wi...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: 1a289379229335a9841dec48e577c19173419891
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836720"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Oprava chyby "Serverová aplikace není k dispozici" po použití aktualizace zabezpečení pro IE
====================
> Tento dokument popisuje opravu, která řeší problém s MS03 32 aktualizace zabezpečení pro Internet Explorer, který má vliv na aplikace ASP.NET 1.0 běžící na Windows XP Professional.
> 
> Platí pro technologii ASP.NET 1.0 a Windows XP Professional.


Microsoft zjistili problém se MS03 32 aktualizace zabezpečení pro úroveň opravy zabezpečení aplikace Internet Explorer a 1.0 technologie ASP.NET a systémem Windows XP. Opravy nainstalujete ručně, nebo získáním nedávné důležité aktualizace z webu Windows Update.

Příznaky tohoto problému je, že po instalaci opravy na počítači s Windows XP, všechny požadavky na aplikace ASP.NET spuštěné na místním webovém serveru IIS 5.1 způsobit chybová zpráva s oznámením "Serveru aplikace není k dispozici". Požadavky na vzdálené webové servery jsou poškozena.

Tento problém ovlivňuje jenom instalace technologie ASP.NET 1.0 a systémem Windows XP. To nemá vliv na počítače s Windows 2000 nebo Windows Server 2003. Je také nemá vliv na počítače se systémem Windows XP s ASP.NET 1.1 nainstalovaný.

Pamatujte, že tento problém **není** chybu zabezpečení pomocí technologie ASP.NET. To **nemá** otevřete nebo povolit všechny škodlivé útoky na aplikace ASP.NET nebo serveru. Místo toho je čistě funkční chybu způsobenou patch, samotného.

Usilovně pracujeme na trvalé řešení tohoto problému. Do té doby můžete spustit následující dávkový soubor jako dočasné řešení problému. Dávkový soubor provede následující akce:

1. Zastaví stav služby IIS a ASP.NET
2. Odstraní a znovu vytvoří účet s známé dočasné heslo
3. Používá Windows `runas` příkaz ke spuštění spustitelného souboru, který vytvoří uživatelským profilem ASPNET
4. Znovu zaregistruje technologie ASP.NET. Tím se vytvoří nová náhodné heslo pro účet a použije výchozí nastavení přístupu ovládací prvek technologie ASP.NET pro něj
5. Restartování služby IIS

Dávkový soubor obsahuje pevně zakódované dočasné heslo "<strong>1pass@word</strong>" který budete vyzváni k zadání pro příkaz runas při spuštění dávkového souboru. Po dokončení příkazu Spustit jako heslo k účtu ASPNET se znovu vytvoří s silné náhodnou hodnotu. Všimněte si, že dávkový soubor může selhat, pokud pevně zakódované heslo nesplňuje požadavky na složitost hesla ve vašem prostředí. Pokud je to tento případ, můžete ji změnit na jinou hodnotu, která je vhodná pro vaše prostředí.

*> [!IMPORTANT]* Pokud jste přidali vlastní řízení přístupu nebo oprávnění databáze účtu pro účet ASPNET, bude nutné znovu vytvořit po dokončení tento dávkový soubor. Je to proto, když se znovu vytvoří účet, se zobrazí nový identifikátor zabezpečení (SID).

*> [!IMPORTANT]* Pokud používáte pracovnímu procesu ASP.NET pomocí vlastního účtu než účtu ASPNET, byste neměli spouštět tento dávkový soubor. Místo toho by měl přihlásit se interaktivně nebo pomocí příkazu Spustit jako s tímto účtem, který vytvoří profil uživatele pro tento účet.

Dávkový soubor je součástí samorozbalovací archivu níže. Jeho použití:

1. Musíte používat jako účet s oprávněními správce
2. [Stáhněte si a spusťte samorozbalovací spustitelný soubor](ms03-32-issue/_static/fixup1.exe)
3. Extrahujte obsah c:\
4. Vybrat spustit z nabídky start a zadejte `cmd.exe`
5. V příkazu Otevřít windows, zadejte `c:\fixup.cmd`.
6. Po zobrazení výzvy zadejte <strong>1pass@word</strong> jako heslo.
7. Pokud máte nastavení řízení přístupu dříve vlastní nebo oprávnění databáze účtu pro účet ASPNET, budete muset znovu použít tato nastavení.

Mnoho omluvu se za nepříjemnosti, která je příčinou. Další informace vám publikování, protože je k dispozici.

Matice níže podrobnosti platformy a verze tyto potíže týkají.

| .NET Framework | Platforma | Vliv |
| --- | --- | --- |
| Verze 1.0 | Windows 2000 Professional | Ne |
| Verze 1.0 | Windows 2000 Server | Ne |
| Verze 1.0 | Windows XP Professional | Ano |
| Verze 1.0 | Windows Server 2003 | Ne |
| Verze 1.0 | Windows XP Home s Cassini | Ne |
| Verze 1.1 | Windows 2000 Professional | Ne |
| Verze 1.1 | Windows 2000 Server | Ne |
| Verze 1.1 | Windows XP Professional | Ne |
| Verze 1.1 | Windows Server 2003 | Ne |
| Verze 1.1 | Windows XP Home s Cassini | Ne |

Dík   
 Tým ASP.NET
