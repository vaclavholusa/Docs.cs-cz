---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Povolení ověřování systému Windows v Katana | Microsoft Docs"
author: MikeWasson
description: "Tento článek ukazuje, jak povolit ověřování systému Windows v Katana. Pokrývá dva scénáře: pomocí služby IIS na hostitele Katana a pomocí HttpListener hostování na vlastním Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: cc23a053fb1ba60ea84eca59e99f0e375fefc4cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="enabling-windows-authentication-in-katana"></a>Povolení ověřování systému Windows v Katana
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Tento článek ukazuje, jak povolit ověřování systému Windows v Katana. Pokrývá dva scénáře: pomocí služby IIS na hostitele Katana a pomocí HttpListener hostování na vlastním Katana ve vlastní proces. Díky Jiří Dorrans, David Matson a Ross Jan kontroly v tomto článku.


Implementace společnosti Microsoft je Katana [OWIN](http://owin.org/), Open Web Interface pro .NET. Můžete si přečíst Úvod do OWIN a Katana [zde](an-overview-of-project-katana.md). Architektura OWIN má několik vrstev:

- Hostitele: Spravuje proces, ve kterém se spouští kanálu OWIN.
- Server: Otevře soketu sítě a naslouchá požadavkům.
- Middleware: Zpracuje požadavek HTTP a odpovědí.

Katana aktuálně poskytuje dva servery, které podporují integrované ověřování systému Windows:

- **Microsoft.Owin.Host.SystemWeb**. Pomocí služby IIS s kanálu ASP.NET.
- **Microsoft.Owin.Host.HttpListener**. Používá [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx). Tento server je aktuálně výchozí možností při vlastním hostování Katana.

> [!NOTE]
> Katana neposkytuje aktuálně OWIN middleware pro ověřování systému Windows, protože tato funkce je již k dispozici na serverech.


## <a name="windows-authentication-in-iis"></a>Ověřování systému Windows ve službě IIS

Pomocí Microsoft.Owin.Host.SystemWeb, můžete jednoduše povolit ověřování systému Windows ve službě IIS.

Začněme tím, že vytvoříte novou aplikaci ASP.NET pomocí šablony projektu "Prázdný webové aplikace ASP.NET".

![](enabling-windows-authentication-in-katana/_static/image1.png)

V dalším kroku přidání balíčků NuGet. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Nyní přidejte třídu s názvem `Startup` následujícím kódem:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

To je všechno je potřeba vytvořit aplikaci "Hello, world" pro OWIN, spuštěná ve službě IIS. Stisknutím klávesy F5 ladění aplikace. Měli byste vidět "Hello, World!" v okně prohlížeče.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Dál jsme budete povolte ověřování systému Windows ve službě IIS Express. Z **zobrazení** nabídce vyberte možnost **vlastnosti**. Klikněte na název projektu v Průzkumníku řešení Chcete-li zobrazit vlastnosti projektu.

V **vlastnosti** nastavte **anonymní ověřování** k **zakázané** a nastavte **ověřování systému Windows** k  **Povolit**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Při spuštění aplikace ze sady Visual Studio, IIS Express bude vyžadovat přihlašovací údaje uživatele systému Windows. To můžete zobrazit pomocí [Fiddler](http://fiddler2.com/home) nebo jiné HTTP nástroje ladění. Tady je příklad odpověď HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

V této odpovědi hlavičky WWW-Authenticate znamenat, že server podporuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protokol, který používá protokol Kerberos nebo NTLM.

Později, když nasazujete aplikace na server, použijte [tyto kroky](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) povolení ověřování systému Windows ve službě IIS na příslušném serveru.

## <a name="windows-authentication-in-httplistener"></a>Ověřování systému Windows v HttpListener

Pokud používáte Microsoft.Owin.Host.HttpListener k hostování na vlastním Katana, můžete povolit ověřování systému Windows přímo na **HttpListener** instance.

Nejprve vytvořte novou konzolovou aplikaci. V dalším kroku přidání balíčků NuGet. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Nyní přidejte třídu s názvem `Startup` následujícím kódem:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Tato třída implementuje stejné "Hello, world" příklad z před, ale také k nastavení ověřování systému Windows jako schéma ověřování.

Uvnitř `Main` fungovat, spuštění kanálu OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

V aplikaci Fiddler k potvrzení, že aplikace používá ověřování systému Windows můžete odeslat žádost:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Související témata

[Přehled Katana projektu](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[Seznámení s OWIN ověřování pomocí formulářů v MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
