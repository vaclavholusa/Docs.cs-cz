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
ms.openlocfilehash: 8c631900c8c5ab6097e0cb9fd4a71abbcba1c88b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="56df5-103">Práce s protokolem SSL v rozhraní Web API</span><span class="sxs-lookup"><span data-stu-id="56df5-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="56df5-104">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="56df5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="56df5-105">Několik společných schémat ověřování nejsou zabezpečené přes prostý HTTP.</span><span class="sxs-lookup"><span data-stu-id="56df5-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="56df5-106">Základní ověřování a ověřování pomocí formulářů vyslat nezašifrované přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="56df5-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="56df5-107">Zabezpečené, tyto schémat ověřování *musí* používat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="56df5-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="56df5-108">Kromě toho SSL klientské certifikáty slouží k ověřování klientů.</span><span class="sxs-lookup"><span data-stu-id="56df5-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="56df5-109">Povolení protokolu SSL na serveru</span><span class="sxs-lookup"><span data-stu-id="56df5-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="56df5-110">Nastavení protokolu SSL ve službě IIS 7 nebo novější:</span><span class="sxs-lookup"><span data-stu-id="56df5-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="56df5-111">Vytvořit nebo získat certifikát.</span><span class="sxs-lookup"><span data-stu-id="56df5-111">Create or get a certificate.</span></span> <span data-ttu-id="56df5-112">Pro testování, můžete vytvořit certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="56df5-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="56df5-113">Přidejte vazbu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56df5-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="56df5-114">Podrobnosti najdete v tématu [postup nastavení protokolu SSL ve službě IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="56df5-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="56df5-115">Pro místní testování, můžete povolit SSL ve službě IIS Express ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56df5-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="56df5-116">V okně Vlastnosti nastavte **povolen protokol SSL** k **True**.</span><span class="sxs-lookup"><span data-stu-id="56df5-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="56df5-117">Poznamenejte si hodnotu **SSL URL**; pomocí této adresy URL pro testování připojení prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="56df5-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="56df5-118">Vynucování SSL v Kontroleru webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="56df5-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="56df5-119">Pokud máte HTTPS a vazbu protokolu HTTP, můžete klientům dál používat pro přístup k webu HTTP.</span><span class="sxs-lookup"><span data-stu-id="56df5-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="56df5-120">Některé prostředky, je k dispozici prostřednictvím protokolu HTTP, zatímco jiné prostředky vyžadovat protokol SSL, mohou povolovat.</span><span class="sxs-lookup"><span data-stu-id="56df5-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="56df5-121">V takovém případě použijte filtr akce tak, aby vyžadovala protokol SSL k chráněným prostředkům.</span><span class="sxs-lookup"><span data-stu-id="56df5-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="56df5-122">Následující kód ukazuje filtr ověřování webového rozhraní API, který kontroluje pro protokol SSL:</span><span class="sxs-lookup"><span data-stu-id="56df5-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="56df5-123">Přidejte tento filtr pro všechny akce webového rozhraní API, které vyžadují protokol SSL:</span><span class="sxs-lookup"><span data-stu-id="56df5-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="56df5-124">Klientské certifikáty SSL</span><span class="sxs-lookup"><span data-stu-id="56df5-124">SSL Client Certificates</span></span>

<span data-ttu-id="56df5-125">SSL poskytuje ověřování pomocí certifikátů infrastruktury veřejných klíčů.</span><span class="sxs-lookup"><span data-stu-id="56df5-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="56df5-126">Na server musíte zadat certifikát, který ověřuje server, na klientovi.</span><span class="sxs-lookup"><span data-stu-id="56df5-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="56df5-127">Je méně častých pro klienta poskytnutí certifikátu na server, ale toto je jednou z možností pro ověřování klientů.</span><span class="sxs-lookup"><span data-stu-id="56df5-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="56df5-128">Chcete-li používat certifikáty klientů pomocí protokolu SSL, musíte způsob, jak distribuovat certifikáty podepsané držitelem pro vaše uživatele.</span><span class="sxs-lookup"><span data-stu-id="56df5-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="56df5-129">Pro mnoho typů aplikací neměl by to být vhodné uživatelské prostředí, ale v některých prostředích (například enterprise) může být v rámci výpočetních procesů.</span><span class="sxs-lookup"><span data-stu-id="56df5-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="56df5-130">Výhody</span><span class="sxs-lookup"><span data-stu-id="56df5-130">Advantages</span></span> | <span data-ttu-id="56df5-131">Nevýhody</span><span class="sxs-lookup"><span data-stu-id="56df5-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="56df5-132">-Certifikát pověření jsou vyšší než uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="56df5-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="56df5-133">-SSL poskytuje kompletní zabezpečený kanál, ověřování, integritu zpráv a šifrování zpráv.</span><span class="sxs-lookup"><span data-stu-id="56df5-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="56df5-134">-Je nutné získat a spravovat certifikáty infrastruktury veřejných KLÍČŮ.</span><span class="sxs-lookup"><span data-stu-id="56df5-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="56df5-135">-Klientské platformy musí podporovat klientské certifikáty SSL.</span><span class="sxs-lookup"><span data-stu-id="56df5-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="56df5-136">Můžete nakonfigurovat službu IIS tak, aby přijímal klientské certifikáty, otevřete Správce služby IIS a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="56df5-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="56df5-137">Klikněte na uzel serveru ve stromovém zobrazení.</span><span class="sxs-lookup"><span data-stu-id="56df5-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="56df5-138">Dvakrát klikněte **nastavení SSL** funkce v prostředním podokně.</span><span class="sxs-lookup"><span data-stu-id="56df5-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="56df5-139">V části **klientské certifikáty**, vyberte jednu z těchto možností:</span><span class="sxs-lookup"><span data-stu-id="56df5-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="56df5-140">**Přijměte**: Služba IIS bude přijímat certifikát z klienta, ale nevyžaduje jeden.</span><span class="sxs-lookup"><span data-stu-id="56df5-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="56df5-141">**Vyžadovat**: vyžadovat certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="56df5-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="56df5-142">(Chcete-li tuto možnost, musíte také vybrat "Vyžadovat šifrování SSL")</span><span class="sxs-lookup"><span data-stu-id="56df5-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="56df5-143">V souboru ApplicationHost.config můžete také nastavit tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="56df5-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="56df5-144">**SslNegotiateCert** příznak znamená, služba IIS bude přijímat certifikát z klienta, ale nevyžaduje jeden (odpovídá možnosti "Přijmout" ve Správci služby IIS).</span><span class="sxs-lookup"><span data-stu-id="56df5-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="56df5-145">Pokud chcete vyžadovat certifikát, nastavte **stává** příznak.</span><span class="sxs-lookup"><span data-stu-id="56df5-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="56df5-146">Pro testování, můžete také nastavit tyto možnosti ve službě IIS Express, v místním hostiteli applicationhost. Konfigurační soubor umístěný ve složce "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="56df5-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="56df5-147">Vytvoření certifikátu klienta pro testování</span><span class="sxs-lookup"><span data-stu-id="56df5-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="56df5-148">Pro účely testování můžete použít [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) vytvořit certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="56df5-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="56df5-149">Nejprve vytvořte certifikát testovací kořenové autority:</span><span class="sxs-lookup"><span data-stu-id="56df5-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="56df5-150">MakeCert zobrazí výzvu k zadání hesla pro privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="56df5-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="56df5-151">V dalším kroku přidáte certifikát do testu, který ukládá serveru "důvěryhodné kořenové certifikační autority", následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="56df5-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="56df5-152">Otevřete konzolu MMC.</span><span class="sxs-lookup"><span data-stu-id="56df5-152">Open MMC.</span></span>
2. <span data-ttu-id="56df5-153">V části **soubor**, vyberte **přidat nebo odebrat modul Snap-In**.</span><span class="sxs-lookup"><span data-stu-id="56df5-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="56df5-154">Vyberte **účet počítače**.</span><span class="sxs-lookup"><span data-stu-id="56df5-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="56df5-155">Vyberte **místního počítače** a dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="56df5-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="56df5-156">V navigačním podokně rozbalte uzel "Důvěryhodné kořenové certifikační autority".</span><span class="sxs-lookup"><span data-stu-id="56df5-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="56df5-157">Na **akce** nabídky, přejděte na příkaz **všechny úlohy**a potom klikněte na **Import** spusťte Průvodce importem certifikátu.</span><span class="sxs-lookup"><span data-stu-id="56df5-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="56df5-158">Vyhledejte soubor certifikátu, TempCA.cer.</span><span class="sxs-lookup"><span data-stu-id="56df5-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="56df5-159">Klikněte na tlačítko **otevřete**, pak klikněte na tlačítko **Další** a dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="56df5-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="56df5-160">(Zobrazí se výzva k znovu zadejte heslo.)</span><span class="sxs-lookup"><span data-stu-id="56df5-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="56df5-161">Nyní vytvoří klientský certifikát, který je podepsaný první certifikát:</span><span class="sxs-lookup"><span data-stu-id="56df5-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="56df5-162">Pomocí klientských certifikátů v rozhraní Web API</span><span class="sxs-lookup"><span data-stu-id="56df5-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="56df5-163">Na straně serveru, můžete získat certifikát klienta voláním [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) na zprávu požadavku.</span><span class="sxs-lookup"><span data-stu-id="56df5-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="56df5-164">Metoda vrátí hodnotu null, pokud neexistuje žádný certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="56df5-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="56df5-165">Funkce **X509Certificate2** instance.</span><span class="sxs-lookup"><span data-stu-id="56df5-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="56df5-166">Tento objekt použijte k získání informací ze certifikát, jako je například vystavitele a předmět.</span><span class="sxs-lookup"><span data-stu-id="56df5-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="56df5-167">Pak můžete tyto informace použít pro ověřování a autorizace.</span><span class="sxs-lookup"><span data-stu-id="56df5-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
