---
uid: whitepapers/ms03-32-issue
title: Oprava chyby, Server aplikace není k dispozici' po aktualizaci zabezpečení pro aplikaci Internet Explorer | Microsoft Docs
author: rick-anderson
description: Tento dokument popisuje opravu, která řeší problém s aktualizací zabezpečení MS03-32 pro Internet Explorer, který má vliv na aplikace ASP.NET 1.0 spuštěné na Wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: dd1a564cd347364abc3ca5ac0a9ffda448bcede8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Opravte chyby, Server aplikace není k dispozici' po aktualizaci zabezpečení pro aplikaci Internet Explorer
====================
> Tento dokument popisuje opravu, která řeší problém pro Internet Explorer, který má vliv na aplikace ASP.NET 1.0 se systémem Windows XP Professional s aktualizací zabezpečení MS03-32.
> 
> Platí pro technologii ASP.NET 1.0 a Windows XP Professional.


Microsoft zjistila problém s aktualizace zabezpečení MS03-32 pro opravy zabezpečení aplikace Internet Explorer a 1.0 ASP.NET v systému Windows XP. Tato oprava lze nainstalovat ručně nebo získání nedávné důležité aktualizace z webu Windows Update.

Tento příznak tohoto problému je, že po instalaci opravy na počítače s Windows XP, všechny požadavky na aplikace ASP.NET, které jsou spuštěné v místním webovém serveru služby IIS 5.1 vést ke chybová zpráva s oznámením "Server aplikace nedostupný". Požadavky na vzdálené webové servery jsou poškozena.

Tento problém ovlivňuje jenom instalace ASP.NET 1.0 v systému Windows XP. Ho nemá negativní vliv na počítače se systémem Windows 2000 nebo Windows Server 2003. Je také nemá negativní vliv na počítače se systémem Windows XP s ASP.NET 1.1 nainstalovaný.

Pamatujte, že tento problém **není** chyby zabezpečení s technologií ASP.NET. Ho **nemá** otevře nebo povolit všechny škodlivé útoky na aplikace ASP.NET nebo serveru. Místo toho je čistě funkční chyby způsobené oprava sám sebe.

Usilovně pracujeme na trvalé řešení tohoto problému. Do té doby můžete provést následující dávkový soubor jako alternativní řešení problému. Dávkový soubor provede následující akce:

1. Zastaví stavu služby IIS a ASP.NET
2. Odstraní a znovu vytvoří k účtu ASPNET s známé dočasné heslo
3. Používá Windows `runas` příkaz ke spuštění spustitelné soubory, které vytvoří profil uživatele aplikace ASPNET
4. Znovu zaregistruje ASP.NET. Tím se vytvoří nové náhodné heslo pro účet a použije výchozí nastavení přístupu ovládací prvek ASP.NET pro ni
5. Restartuje službu IIS

Dávkový soubor obsahuje pevně zakódované dočasné heslo z "<strong>1pass@word</strong>" což zobrazí výzva k zadání pro příkaz runas při spuštění dávkového souboru. Po dokončení příkazu Spustit jako, znovu vytvořena s hodnotou silné náhodné heslo účtu ASPNET. Všimněte si, že dávkový soubor může selhat, pokud pevně zakódované heslo nesplňuje požadavky na složitost hesla ve vašem prostředí. Pokud je to tento případ, můžete ho změnit na jinou hodnotu, která je vhodná pro vaše prostředí.

*> [!IMPORTANT]* Pokud jste přidali vlastní řízení přístupu nebo oprávnění databáze účtu pro účet ASPNET, budou muset být znovu vytvořen po dokončení tento dávkový soubor. Je to proto, že když se znovu vytvoří účet, se budou získávat nový identifikátor zabezpečení (SID).

*> [!IMPORTANT]* Pokud pracovní proces ASP.NET běží s vlastním účtem než u účtu ASPNET, by neměl spustit tento dávkový soubor. Místo toho by měla interaktivně přihlásit nebo použít příkaz runas pomocí daného účtu, která vytvoří profil uživatele pro tento účet.

Dávkový soubor je součástí samorozbalující archivu níže. Pro použití:

1. Musíte používat jako účet s oprávněním správce
2. [Stáhněte a spusťte samorozbalovací spustitelný soubor](ms03-32-issue/_static/fixup1.exe)
3. Extrahujte obsah do c:\
4. Vybrat spustit z nabídky start a zadejte `cmd.exe`
5. V systému windows otevřete příkazu, zadejte `c:\fixup.cmd`.
6. Po zobrazení výzvy zadejte <strong>1pass@word</strong> jako heslo.
7. Pokud máte nastavení řízení přístupu dříve vlastní nebo oprávnění databáze účtu pro účet ASPNET, budete muset znovu použít tato nastavení.

Mnoho omluvu za vzniklé potíže, která je příčinou. Jakmile je k dispozici jsme vám účtovat další informace.

Matice níže uvedené podrobnosti platformy a verze dopad na tento problém.

| .NET Framework | Platforma | Vliv |
| --- | --- | --- |
| Verze 1.0 | Windows 2000 Professional | Ne |
| Verze 1.0 | Windows 2000 Server | Ne |
| Verze 1.0 | Windows XP Professional | Ano |
| Verze 1.0 | Windows Server 2003 | Ne |
| Verze 1.0 | Systém Windows XP Home s Cassini | Ne |
| Verze 1.1 | Windows 2000 Professional | Ne |
| Verze 1.1 | Windows 2000 Server | Ne |
| Verze 1.1 | Windows XP Professional | Ne |
| Verze 1.1 | Windows Server 2003 | Ne |
| Verze 1.1 | Systém Windows XP Home s Cassini | Ne |

Dík   
 Tým služby ASP.NET
