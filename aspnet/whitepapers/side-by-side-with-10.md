---
uid: whitepapers/side-by-side-with-10
title: Spuštění rozhraní .NET Framework 1.0 a 1.1 ASP.NET vedle sebe | Microsoft Docs
author: rick-anderson
description: Tento dokument White Paper popisuje postup instalace rozhraní .NET 1.0 a .NET 1.1 na počítači, což webovou aplikaci ASP.NET ke spuštění na jednu z verzí nástroje od...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573358"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Spuštění rozhraní .NET Framework 1.0 a 1.1 ASP.NET vedle sebe
====================
> Tento dokument White Paper popisuje postup instalace na počítači, což webovou aplikaci ASP.NET ke spuštění na jedné verze rozhraní .NET 1.0 a 1.1 rozhraní .NET.
> 
> Platí pro ASP.NET 1.0 a 1.1 technologie ASP.NET.


V technologii ASP.NET aplikace řečeno, aby byl spuštěn vedle sebe, pokud jsou nainstalované na stejném počítači, ale používají různé verze rozhraní .NET Framework. Následující téma popisuje konfiguraci aplikace ASP.NET pro spuštění vedle sebe a poskytuje podrobné kroky:

- [Udržovat mapování webové aplikace rozhraní .NET Framework verze 1.0 během instalace](#1)
- [Mapa webovou aplikaci na konkrétní verzi rozhraní .NET Framework](#2)
- [Najít verzi rozhraní .NET Framework, který používá webový server](#3)

Tradičně Pokud na počítači se aktualizuje součást nebo aplikace, starší verzi je odebere a nahradí novější verze. Pokud nové verze není kompatibilní s předchozí verzí, to obvykle dělí jiné aplikace, které používají součást nebo aplikace. Rozhraní .NET Framework poskytuje podporu pro spuštění vedle sebe, což umožňuje více verzí sestavení nebo aplikace bude nainstalována na stejném počítači ve stejnou dobu. Protože současně může být nainstalováno více verzí, můžete vybrat spravované aplikace, kterou verzi má použít, aniž by to ovlivnilo aplikace, které používají různé verze.

Ve výchozím nastavení při instalaci rozhraní .NET Framework verze 1.1, všechny existující aplikace ASP.NET je automaticky překonfigurovat tak, aby používali nejnovější verzi rozhraní .NET Framework. Pokud nechcete, aby vaše aplikace ASP.NET, jako výchozí rozhraní .NET Framework 1.1, klikněte na tlačítko [sem](#1) se dozvíte, jak chcete-li tomu zabránit během instalace.

Pokud váš webový server aktualizovat na rozhraní .NET Framework 1.1 a chcete jeden nebo více webových aplikací ke spuštění rozhraní .NET Framework 1.0, budete muset aktualizovat mapu skriptů Internetové informační služby (IIS). Mapování skriptu je mechanismus pro mapování .aspx příponu souboru pro konkrétní webovou aplikaci na verzi rozhraní .NET Framework. Klikněte na tlačítko [sem](#2) se dozvíte, jak k mapování webovou aplikaci na konkrétní verzi rozhraní .NET Framework.

Můžete použít správce sítě Internet informace nebo ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) k vyhledání, která verze rozhraní .NET Framework je spuštěna aplikace typu konkrétní Web. Klikněte na tlačítko [sem](#3) se dozvíte, jak najít verzi rozhraní .NET Framework, který používá webový server.

Při migraci na rozhraní .NET Framework 1.1 jeden aspekt importu je, že každou verzi rozhraní .NET Framework používá vlastního souboru Machine.config. V důsledku toho za správce webu provedla změny souboru Machine.config, musí být tyto změny migrovány do souboru rozhraní .NET Framework 1.1 Machine.config.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Správa mapování webové aplikace na rozhraní .NET Framework 1.0 během instalace

Ve výchozím nastavení se všechny existující aplikace ASP.NET mění automaticky během instalace použít novější verzi rozhraní .NET Framework. Pomocí novější verze rozhraní .NET Framework, může aplikace využít všech výhod vylepšení a novým funkcím zahrnutým v nové verzi. Ve stejnou dobu správce webu, který může být vhodné podrobnou kontrolu nad aplikace, které jsou aktualizované, můžete zabránit automatické přemapování všech existujících aplikací ASP.NET během instalace rozhraní .NET Framework.

Pokud chcete zabránit automatické přemapování celá aplikace ASP.NET na novější verzi rozhraní .NET Framework, správce webu, můžete použít možnost příkazového řádku/noaspupgrade s instalačním programem Dotnetfx.exe.

**Aby se zabránilo celkový přemapování aplikace technologie ASP.NET pro novější verze**

1. Přejděte na **spustit**.
2. Klikněte na **spustit**.
3. Typ **cmd**.
4. Click **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Z příkazového řádku zadejte následující příkaz ke spuštění instalace rozhraní .NET Framework: **Dotnetfx.exe/c: "instalace/noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Klikněte na tlačítko **Ano** v instalačním programu rozhraní Microsoft .NET Framework 1.1. Tato akce spustí proces instalace rozhraní .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Mapa webovou aplikaci na konkrétní verzi rozhraní .NET Framework

Obsahuje každou verzi rozhraní .NET Framework verze nástroje ASP.NET IIS Registration (Aspnet\_regiis.exe). Tento nástroj umožňuje správcům spustit webovou aplikaci v rámci konkrétní verzi rozhraní .NET Framework. To se označuje jako mapování webovou aplikaci pro verzi rozhraní .NET Framework. Správci musí vybrat Aspnet\_regiis.exe odpovídající verzi rozhraní .NET Framework, který bude spojený s webovou aplikací. Například správce, který chce určit, že webový server používat rozhraní .NET Framework 1.1 musí použít Aspnet\_regiis.exe která se dodává s .NET Framework 1.1.

Aspnet\_regiis.exe pro verzi 1.0 je umístěn zde:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis

Aspnet\_regiis.exe pro verzi 1,1 je umístěn zde:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis

Aspnet\_regiis.exe nabízí dvě možnosti pro skript mapování webové aplikace:

- **-s** Nastaví mapu skriptů v cestě a v jeho podřízených adresářů.
- **-sn** Nastaví mapu skriptů se pouze na cestě.

Definuje cestu webové cesta k aplikaci IIS metadata, která je definovaná ve formě W3SVC/ROOT / {WebSiteNumber} / {aplikace\_název}. Například pro webovou aplikaci označuje umístěná na výchozí web portál, je cestu metabáze W3SVC/1/ROOT nebo portálu.

![](side-by-side-with-10/_static/image4.gif)

Všimněte si, že používáte nástroj nazvaný editoru metabáze můžete také získat cestu metabáze. Tento nástroj můžete stáhnout z webu Microsoft Support na [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Spustit Aspnet\_regiis.exe -s W3SVC/1/ROOT nebo portál aktualizovat na portálu služby IIS skriptu mapy a jeho subapplication.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Spustit Aspnet\_regiis.exe -sn mapovat W3SVC/1/KOŘENOVÉ nebo portálu na portálu služby IIS skript aktualizovat, aniž by to ovlivnilo aplikací na portálu? podadresáře s.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Najít verzi rozhraní .NET Framework, který používá webové aplikace

Správce může pomocí Správce služeb sítě Internet k vyhledání, která verze rozhraní .NET Framework spouští webovou stránku. Různé verze operačního systému jinak spusťte Správce služeb sítě Internet. Pokud chcete spustit Správce služeb, postupujte podle následujících kroků.

**Chcete-li spustit Správce služeb sítě Internet**

1. Přejděte na **spustit**.
2. Klikněte na **spustit**.
3. Typ **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Ze Správce služeb Internetu, vyberte webovou aplikaci, jehož verze rozhraní .NET Framework, budete chtít vědět.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Klikněte pravým tlačítkem na webové aplikace a klikněte na **vlastnosti.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. V okně Vlastnosti vyberte **konfigurace.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Z aplikace mapování tabulky, vyberte **.aspx**a klikněte na tlačítko **upravit**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Z **spustitelný soubor** textového pole, podívejte se na verzi adresáře s posouvání. Pokud je verze adresář v.1.1.4322, aplikace je mapována na rozhraní .NET Framework 1.1. Naopak pokud je verze adresář v1.0.3705, aplikace je mapována na rozhraní .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
