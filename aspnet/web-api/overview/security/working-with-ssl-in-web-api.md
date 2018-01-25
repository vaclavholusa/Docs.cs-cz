---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: "Práce s protokolem SSL v rozhraní Web API | Microsoft Docs"
author: MikeWasson
description: "Ukazuje, jak používat protokol SSL s rozhraním ASP.NET Web API, včetně používání certifikátů SSL klienta."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="working-with-ssl-in-web-api"></a>Práce s protokolem SSL v rozhraní Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Několik společných schémat ověřování nejsou zabezpečené přes prostý HTTP. Základní ověřování a ověřování pomocí formulářů vyslat nezašifrované přihlašovací údaje. Zabezpečené, tyto schémat ověřování *musí* používat protokol SSL. Kromě toho SSL klientské certifikáty slouží k ověřování klientů.

## <a name="enabling-ssl-on-the-server"></a>Povolení protokolu SSL na serveru

Nastavení protokolu SSL ve službě IIS 7 nebo novější:

- Vytvořit nebo získat certifikát. Pro testování, můžete vytvořit certifikát podepsaný svým držitelem.
- Přidejte vazbu HTTPS.

Podrobnosti najdete v tématu [postup nastavení protokolu SSL ve službě IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Pro místní testování, můžete povolit SSL ve službě IIS Express ze sady Visual Studio. V okně Vlastnosti nastavte **povolen protokol SSL** k **True**. Poznamenejte si hodnotu **SSL URL**; pomocí této adresy URL pro testování připojení prostřednictvím protokolu HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Vynucování SSL v Kontroleru webového rozhraní API

Pokud máte HTTPS a vazbu protokolu HTTP, můžete klientům dál používat pro přístup k webu HTTP. Některé prostředky, je k dispozici prostřednictvím protokolu HTTP, zatímco jiné prostředky vyžadovat protokol SSL, mohou povolovat. V takovém případě použijte filtr akce tak, aby vyžadovala protokol SSL k chráněným prostředkům. Následující kód ukazuje filtr ověřování webového rozhraní API, který kontroluje pro protokol SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Přidejte tento filtr pro všechny akce webového rozhraní API, které vyžadují protokol SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Klientské certifikáty SSL

SSL poskytuje ověřování pomocí certifikátů infrastruktury veřejných klíčů. Na server musíte zadat certifikát, který ověřuje server, na klientovi. Je méně častých pro klienta poskytnutí certifikátu na server, ale toto je jednou z možností pro ověřování klientů. Chcete-li používat certifikáty klientů pomocí protokolu SSL, musíte způsob, jak distribuovat certifikáty podepsané držitelem pro vaše uživatele. Pro mnoho typů aplikací neměl by to být vhodné uživatelské prostředí, ale v některých prostředích (například enterprise) může být v rámci výpočetních procesů.

| Výhody | Nevýhody |
| --- | --- |
| -Certifikát pověření jsou vyšší než uživatelské jméno a heslo. -SSL poskytuje kompletní zabezpečený kanál, ověřování, integritu zpráv a šifrování zpráv. | -Je nutné získat a spravovat certifikáty infrastruktury veřejných KLÍČŮ. -Klientské platformy musí podporovat klientské certifikáty SSL. |

Můžete nakonfigurovat službu IIS tak, aby přijímal klientské certifikáty, otevřete Správce služby IIS a proveďte následující kroky:

1. Klikněte na uzel serveru ve stromovém zobrazení.
2. Dvakrát klikněte **nastavení SSL** funkce v prostředním podokně.
3. V části **klientské certifikáty**, vyberte jednu z těchto možností: 

    - **Přijměte**: Služba IIS bude přijímat certifikát z klienta, ale nevyžaduje jeden.
    - **Vyžadovat**: vyžadovat certifikát klienta. (Chcete-li tuto možnost, musíte také vybrat "Vyžadovat šifrování SSL")

V souboru ApplicationHost.config můžete také nastavit tyto možnosti:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert** příznak znamená, služba IIS bude přijímat certifikát z klienta, ale nevyžaduje jeden (odpovídá možnosti "Přijmout" ve Správci služby IIS). Pokud chcete vyžadovat certifikát, nastavte **stává** příznak. Pro testování, můžete také nastavit tyto možnosti ve službě IIS Express, v místním hostiteli applicationhost. Konfigurační soubor umístěný ve složce "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Vytvoření certifikátu klienta pro testování

Pro účely testování můžete použít [MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx) vytvořit certifikát klienta. Nejprve vytvořte certifikát testovací kořenové autority:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

MakeCert zobrazí výzvu k zadání hesla pro privátní klíč.

V dalším kroku přidáte certifikát do testu, který ukládá serveru "důvěryhodné kořenové certifikační autority", následujícím způsobem:

1. Otevřete konzolu MMC.
2. V části **soubor**, vyberte **přidat nebo odebrat modul Snap-In**.
3. Vyberte **účet počítače**.
4. Vyberte **místního počítače** a dokončete průvodce.
5. V navigačním podokně rozbalte uzel "Důvěryhodné kořenové certifikační autority".
6. Na **akce** nabídky, přejděte na příkaz **všechny úlohy**a potom klikněte na **Import** spusťte Průvodce importem certifikátu.
7. Vyhledejte soubor certifikátu, TempCA.cer.
8. Klikněte na tlačítko **otevřete**, pak klikněte na tlačítko **Další** a dokončete průvodce. (Zobrazí se výzva k znovu zadejte heslo.)

Nyní vytvoří klientský certifikát, který je podepsaný první certifikát:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Pomocí klientských certifikátů v rozhraní Web API

Na straně serveru, můžete získat certifikát klienta voláním [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) na zprávu požadavku. Metoda vrátí hodnotu null, pokud neexistuje žádný certifikát klienta. Funkce **X509Certificate2** instance. Tento objekt použijte k získání informací ze certifikát, jako je například vystavitele a předmět. Pak můžete tyto informace použít pro ověřování a autorizace.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
