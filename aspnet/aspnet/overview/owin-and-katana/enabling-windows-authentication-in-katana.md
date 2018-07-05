---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Povolení ověřování Windows v sadě Katana | Dokumentace Microsoftu
author: MikeWasson
description: 'Tento článek ukazuje, jak povolit ověřování Windows v sadě Katana. Popisuje dva scénáře: pomocí služby IIS pro hostování Katana a pomocí HttpListener k samoobslužnému hostování Kat...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: e7fbe6af323ecdc09b4d79073f506c5ee056f30f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391612"
---
<a name="enabling-windows-authentication-in-katana"></a>Povolení ověřování Windows v sadě Katana
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> Tento článek ukazuje, jak povolit ověřování Windows v sadě Katana. Popisuje dva scénáře: použití služby IIS pro hostování Katana a pomocí HttpListener Katana samoobslužné hostování ve vlastním procesu. Děkujeme, že Jiří Dorrans, David Matson a Chris Ross revize v tomto článku.


Katana je implementace společnosti Microsoft [OWIN](http://owin.org/), Open Web Interface pro .NET. Můžete si přečíst Úvod do OWIN a Katana [tady](an-overview-of-project-katana.md). Architektura OWIN má několik vrstev:

- Hostitel: Spravuje proces, ve kterém se spouští kanál OWIN.
- Server: Otevře soket sítě a čeká na požadavky.
- Middleware: Zpracuje požadavek HTTP a odpovědí.

Katana v současné době poskytuje dva servery, které podporují integrované ověřování Windows:

- **Microsoft.Owin.Host.SystemWeb**. Používá službu IIS pomocí kanálu ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Používá [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Tento server je aktuálně výchozí možnost, při hostování na vlastním Katana.

> [!NOTE]
> Katana neposkytuje aktuálně OWIN middleware pro ověřování Windows, protože tato funkce je k dispozici na serverech.


## <a name="windows-authentication-in-iis"></a>Ověřování Windows ve službě IIS

Pomocí Microsoft.Owin.Host.SystemWeb, můžete jednoduše povolit ověřování Windows služby IIS.

Začněme tím, že vytvoříte novou aplikaci ASP.NET, pomocí šablony projektu "Prázdná webová aplikace ASP.NET".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Dále přidejte balíčky NuGet. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Nyní přidejte třídu pojmenovanou `Startup` následujícím kódem:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

To je všechno, co potřebujete k vytvoření aplikace "Hello world" pro OWIN ve službě IIS. Stisknutím klávesy F5 pro ladění aplikace. Měli byste vidět "Hello World!" v okně prohlížeče.

![](enabling-windows-authentication-in-katana/_static/image2.png)

V dalším kroku vám poskytujeme ověřování Windows služby IIS Express. Z **zobrazení** nabídce vyberte možnost **vlastnosti**. Klikněte na název projektu v Průzkumníku řešení Chcete-li zobrazit vlastnosti projektu.

V **vlastnosti** okno, nastavte **anonymní ověřování** k **zakázané** a nastavte **ověřování Windows** k  **Povolené**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Při spuštění aplikace ze sady Visual Studio, služby IIS Express bude vyžadovat přihlašovací údaje uživatele Windows. Můžete to zobrazit pomocí [Fiddler](http://fiddler2.com/home) nebo jiného nástroje ladění HTTP. Tady je příklad odpovědi HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Hlavičky WWW-Authenticate tato odpověď značí, že server podporuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protokol, který používá protokol Kerberos nebo NTLM.

Později, když nasazujete aplikace na server, využijte [tyto kroky](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) povolení ověřování Windows služby IIS na tomto serveru.

## <a name="windows-authentication-in-httplistener"></a>Ověřování Windows v HttpListener

Pokud používáte Microsoft.Owin.Host.HttpListener k samoobslužnému hostování Katana, můžete povolit ověřování Windows přímo na **HttpListener** instance.

Nejprve vytvořte novou konzolovou aplikaci. Dále přidejte balíčky NuGet. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Nyní přidejte třídu pojmenovanou `Startup` následujícím kódem:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Tato třída implementuje stejné příkladu "Hello world" z před, ale také nastaví ověřování Windows jako schéma ověřování.

Uvnitř `Main` funkci, spuštění kanálu OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Odeslat požadavek ve Fiddleru potvrďte, že aplikace používá ověřování Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Související témata

[Přehled projektu Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Principy ověřování pomocí formulářů OWIN v MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
