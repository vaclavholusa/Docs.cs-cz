---
uid: whitepapers/side-by-side-with-10
title: Technologie ASP.NET na straně by-Side Execution rozhraní .NET Framework 1.0 a 1.1 | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument White Paper popisuje, jak nainstalovat na váš počítač, umožní webové aplikaci ASP.NET spustit v jedné verze od .NET 1.0 a 1.1 rozhraní .NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1b8bcebd59a900e54c759509fd4fc5ad1f4f8576
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391157"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Technologie ASP.NET na straně by-Side Execution rozhraní .NET Framework 1.0 a 1.1
====================
> Tento dokument White Paper popisuje postup instalace rozhraní .NET 1.0 a 1.1 rozhraní .NET na svém počítači umožní webové aplikaci ASP.NET spustit v jedné verze rozhraní framework.
> 
> Platí pro technologii ASP.NET 1.0 a ASP.NET 1.1.


V technologii ASP.NET aplikace se říká, že souběžné spuštění, když jsou nainstalovány na stejném počítači, ale používají různé verze rozhraní .NET Framework. Následující téma popisuje postup konfigurace aplikací technologie ASP.NET pro spuštění vedle sebe a obsahuje podrobný postup:

- [Udržovat mapování vaší webové aplikace .NET Framework verze 1.0 během instalace](#1)
- [Mapa webovou aplikaci na konkrétní verzi rozhraní .NET Framework](#2)
- [Najít verzi rozhraní .NET Framework, která používá webový server](#3)

Tradičně Když komponenta nebo aplikace se aktualizuje na počítači, starší verze je odebrat a nahradí novější verze. Pokud nová verze není kompatibilní s předchozí verzí, to obvykle dělí jiné aplikace, které používají součást nebo aplikace. Rozhraní .NET Framework poskytuje podporu pro spuštění vedle sebe, což umožňuje více verzí sestavení nebo aplikace bude nainstalována do stejného počítače ve stejnou dobu. Protože najednou může být nainstalováno více verzí, můžete vybrat spravované aplikace která verze se má použít, aniž by to ovlivnilo aplikace, které používají jinou verzi.

Ve výchozím nastavení během instalace rozhraní .NET Framework verze 1.1, všechny stávající aplikace ASP.NET jsou automaticky překonfigurovat tak, aby používali nejnovější verzi rozhraní .NET Framework. Pokud nechcete, aby vaše aplikace ASP.NET na rozhraní .NET Framework 1.1 ve výchozím nastavení, klikněte na tlačítko [tady](#1) se naučíte, pokud tomu chcete zabránit během instalace.

Pokud aktualizovat váš webový server na rozhraní .NET Framework 1.1 a má jednu nebo více webových aplikací ke spuštění rozhraní .NET Framework 1.0, budete muset aktualizovat mapu skriptů se Internetové informační služby (IIS). Mapování skriptů je mechanismus pro mapování přípona souboru .aspx pro konkrétní webové aplikace na verzi rozhraní .NET Framework. Klikněte na tlačítko [tady](#2) se dozvíte, jak namapovat webovou aplikaci na konkrétní verzi rozhraní .NET Framework.

Můžete použít správce sítě Internet informace nebo ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) k vyhledání, která verze rozhraní .NET Framework běží konkrétní webové aplikace. Klikněte na tlačítko [tady](#3) se naučíte, jak najít verzi rozhraní .NET Framework, která používá webový server.

Jedním z faktorů import při migraci na rozhraní .NET Framework 1.1 je, že každou verzi rozhraní .NET Framework používá svůj vlastní soubor Machine.config. V důsledku toho Web správce provedl změny souboru Machine.config, je nutné tyto změny migrovat do souboru Machine.config 1.1 rozhraní .NET Framework.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Správa mapování vaší webové aplikace na rozhraní .NET Framework 1.0 během instalace

Ve výchozím nastavení jsou automaticky během instalace používat novější verzi rozhraní .NET Framework překonfigurovat všech existujících aplikací ASP.NET. Používá novější verzi rozhraní .NET Framework, aplikace můžete plně využít vylepšení a nových funkcích v nové verzi. Ve stejnou dobu správce webu, který může být vhodné podrobnou kontrolu nad aplikace, které se aktualizují, můžete zabránit automatické přemapování všech existujících aplikací ASP.NET během instalace rozhraní .NET Framework.

Pokud chcete zabránit automatickému přemapování celé aplikace ASP.NET na novější verzi rozhraní .NET Framework, můžete použít správce webu možnost příkazového řádku/noaspupgrade s instalačním programem Dotnetfx.exe.

**Aby se zabránilo celkový přemapování aplikace ASP.NET na novější verzi**

1. Přejděte na **Start**.
2. Klikněte na **spustit**.
3. Typ **cmd**.
4. Klikněte na tlačítko **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Z příkazového řádku zadejte následující řádek, který spustí instalaci rozhraní .NET Framework: **Dotnetfx.exe sady "instalace/noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Klikněte na tlačítko **Ano** při instalaci rozhraní Microsoft .NET Framework 1.1. Tím se spustí proces instalace rozhraní .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Mapa webovou aplikaci na konkrétní verzi rozhraní .NET Framework

Zahrnuje každá verze rozhraní .NET Framework verze nástroje ASP.NET IIS Registration Tool (Aspnet\_regiis.exe). Tento nástroj umožňuje správcům určit spuštění webové aplikace v rámci konkrétní verzi rozhraní .NET Framework. To se označuje jako mapování webové aplikace na verzi rozhraní .NET Framework. Správci musí vybrat Aspnet\_regiis.exe, která odpovídá verzi rozhraní .NET Framework, která bude spojená s webovou aplikací. Například správce, který chce určit, že webový server používat rozhraní .NET Framework 1.1 musí použít Aspnet\_regiis.exe, který je součástí rozhraní .NET Framework 1.1.

Aspnet\_regiis.exe pro verzi 1.0 se nachází na:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis

Aspnet\_regiis.exe pro verzi 1,1 se nachází na:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis

Aspnet\_regiis.exe poskytuje dvě možnosti pro skript mapování webové aplikace:

- **-s** Nastaví mapu skriptů se v cestě a v jeho podřízených adresáře.
- **-sn** Nastaví mapu skriptů se pouze na cestě.

Definuje cestu webové aplikace služby IIS metadat cesty, která je definována v podobě W3SVC/ROOT / {WebSiteNumber} / {aplikace\_Name}. Cesta metabáze je pro webové aplikace volá Portal nachází v rámci výchozí webový server, W3SVC/1/ROOT/portál.

![](side-by-side-with-10/_static/image4.gif)

Všimněte si, že nástroj zvaný editoru metabáze můžete také použít k získání cesty metabáze. Tento nástroj si můžete stáhnout z webu Microsoft Support na [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Spustit Aspnet\_mapy a jeho subapplication skriptu regiis.exe -s W3SVC/1/ROOT/portál aktualizovat na portálu služby IIS.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Spustit Aspnet\_regiis.exe -sn namapovat W3SVC/1/ROOT/portál aktualizovat portál skriptů služby IIS, aniž by to ovlivnilo aplikací na portálu? s podadresářů.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Najít verzi rozhraní .NET Framework, která používá webovou aplikaci

Správce může pomocí Správce služeb sítě Internet k vyhledání, která verze rozhraní .NET Framework běží webový server. Různé verze operačního systému jinak spusťte Správce služeb sítě Internet. Ke spuštění portálu service manager, postupujte podle kroků uvedených níže.

**Chcete-li spustit Správce služeb sítě Internet**

1. Přejděte na **Start**.
2. Klikněte na **spustit**.
3. Typ **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Ze Správce služeb Internetu vyberte webovou aplikaci, jejíž verze rozhraní .NET Framework, budete chtít vědět.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Klikněte pravým tlačítkem na webovou aplikaci a klikněte na **vlastnosti.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. V okně Vlastnosti vyberte **konfigurace.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. V tabulce mapování aplikace vyberte **.aspx**a klikněte na tlačítko **upravit**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Z **spustitelný soubor** textového pole, najdete v adresáři verze posunutím. Pokud adresář verze je v.1.1.4322, aplikace se mapuje na rozhraní .NET Framework 1.1. Naopak pokud adresář verze je v1.0.3705, aplikace se mapuje na rozhraní .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
