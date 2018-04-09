---
title: Rozšíření správy klíčů v základní technologie ASP.NET
author: rick-anderson
description: Další informace o rozšíření ochrany dat ASP.NET Core správy klíčů.
manager: wpickett
ms.author: riande
ms.date: 11/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: e3042b371cf7be8fa0218c1906042d2810b180e3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Rozšíření správy klíčů v základní technologie ASP.NET

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Pro čtení [Správa klíčů](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) části před čtením této části, jak vysvětluje některé základní koncepty za tato rozhraní API.

>[!WARNING]
> Typy, které implementují některá z následujících rozhraní by měly být vláken pro více volající.

## <a name="key"></a>Key

`IKey` Rozhraní je základní reprezentace klíče v cryptosystem. Termín používá se zde v abstraktní smysl, ne v tom smyslu literálu "kryptografické klíče materiálu". Klíč má následující vlastnosti:

* Aktivace, vytvoření a datum vypršení platnosti

* Stav odvolání

* Identifikátor klíče (identifikátor GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Kromě toho `IKey` zpřístupní `CreateEncryptor` metodu, která slouží k vytvoření [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance vázaný na tento klíč.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Kromě toho `IKey` zpřístupní `CreateEncryptorInstance` metodu, která slouží k vytvoření [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance vázaný na tento klíč.

---

> [!NOTE]
> Neexistuje žádné rozhraní API pro načtení nezpracovaná kryptografických materiálu z `IKey` instance.

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` Rozhraní představuje objekt zodpovědný za obecné úložiště klíčů, načítání a zpracování. Poskytuje tři základní operace:

* Vytvořte nový klíč a zachovat do úložiště.

* Získáte všechny klíče z úložiště.

* Odvolání jeden či více klíčů a zachovat informace o odvolání certifikátů do úložiště.

>[!WARNING]
> Zápis `IKeyManager` je velmi pokročilé úlohy a Většina vývojářů neměli. Místo toho Většina vývojářů by měl využít výhod zařízení, které nabízí [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) třídy.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` Typ je v poli konkrétní implementace `IKeyManager`. Poskytuje několik užitečné zařízení, včetně klíče úschově a šifrovací klíče v klidovém stavu. Klíče v tomto systému jsou reprezentovány jako elementy XML (konkrétně [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` závisí na několik součástí vyřízení její úkoly:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`, který určuje algoritmy používané nových klíčů.

* `IXmlRepository`, které ovládací prvky, kde jsou klíče uchovávané v úložišti.

* `IXmlEncryptor` [Nepovinné], což umožňuje šifrování klíče v klidovém stavu.

* `IKeyEscrowSink` [Nepovinné], který poskytuje služby klíče úschově.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

* `IXmlRepository`, které ovládací prvky, kde jsou klíče uchovávané v úložišti.

* `IXmlEncryptor` [Nepovinné], což umožňuje šifrování klíče v klidovém stavu.

* `IKeyEscrowSink` [Nepovinné], který poskytuje služby klíče úschově.

---

Níže jsou diagramy vysoké, které označují, jak jsou tyto součásti drátové společně v rámci `XmlKeyManager`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

   ![Vytvoření klíče](key-management/_static/keycreation2.png)

   *Vytvoření klíče nebo CreateNewKey*

Při provádění `CreateNewKey`, `AlgorithmConfiguration` komponenta se používá k vytvoření jedinečný `IAuthenticatedEncryptorDescriptor`, který je pak serializovanou jako XML. Pokud se klíče úschově podřízený nachází, nezpracovaná XML (nezašifrované) zajišťuje jímky pro dlouhodobé uložení. Nezašifrované XML je spusťte `IXmlEncryptor` (v případě potřeby) ke generování šifrovaných dokumentu XML. Tato zašifrovaného dokumentu je trvalé do dlouhodobého úložiště prostřednictvím `IXmlRepository`. (Pokud žádné `IXmlEncryptor` je nakonfigurován, nezašifrované dokumentu je uchován v `IXmlRepository`.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

   ![Vytvoření klíče](key-management/_static/keycreation1.png)

   *Vytvoření klíče nebo CreateNewKey*

Při provádění `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` komponenta se používá k vytvoření jedinečný `IAuthenticatedEncryptorDescriptor`, který je pak serializovanou jako XML. Pokud se klíče úschově podřízený nachází, nezpracovaná XML (nezašifrované) zajišťuje jímky pro dlouhodobé uložení. Nezašifrované XML je spusťte `IXmlEncryptor` (v případě potřeby) ke generování šifrovaných dokumentu XML. Tato zašifrovaného dokumentu je trvalé do dlouhodobého úložiště prostřednictvím `IXmlRepository`. (Pokud žádné `IXmlEncryptor` je nakonfigurován, nezašifrované dokumentu je uchován v `IXmlRepository`.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

   ![Načítání klíče](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

   ![Načítání klíče](key-management/_static/keyretrieval1.png)

---

   *Načítání klíče nebo GetAllKeys*

Při provádění `GetAllKeys`, XML dokumenty představující klíče a odvolání se načítají z základní `IXmlRepository`. Pokud jsou tyto dokumenty zašifrované, systém bude automaticky jejich dešifrování. `XmlKeyManager` vytvoří odpovídající `IAuthenticatedEncryptorDescriptorDeserializer` instance k deserializaci dokumenty zpět do `IAuthenticatedEncryptorDescriptor` instance, které jsou pak uzavřen do individuální `IKey` instance. Tato kolekce `IKey` instance je vrácen volajícímu.

Další informace o konkrétní elementy XML lze nalézt v [úložiště klíčů formátovat dokument](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` Rozhraní představuje typ, který můžete zachovat XML tak, aby a načtení XML ze záložnímu úložišti. Poskytuje dvě rozhraní API:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (XElement element, friendlyName řetězec)

Implementace `IXmlRepository` nemusíte analyzovat soubor XML prošla je. Jejich považovat za neprůhledné dokumentů XML a nechat vyšší vrstvy starat o generování a analýza dokumenty.

Existují dva předdefinované konkrétní typy, které implementují třídu `IXmlRepository`: `FileSystemXmlRepository` a `RegistryXmlRepository`. Najdete v článku [dokumentu zprostředkovatele úložiště klíčů](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers) Další informace. Vlastní registrace `IXmlRepository` může být vhodným způsobem použít záložní úložiště, například Azure Blob Storage.

Chcete-li změnit výchozí úložiště celou aplikaci, zaregistrovat vlastní `IXmlRepository` instance:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor` Rozhraní představuje typ, který můžete šifrovat element XML ve formátu prostého textu. Zpřístupňuje jediného rozhraní API:

* Šifrování (XElement plaintextElement): EncryptedXmlInfo

Pokud serializovaný seznam `IAuthenticatedEncryptorDescriptor` obsahuje všechny elementy označena jako "vyžaduje šifrování", pak `XmlKeyManager` tyto prvky se spustí pomocí konfigurovaného `IXmlEncryptor`na `Encrypt` metoda ale zachová enciphered element místo ve formátu prostého textu elementu na `IXmlRepository`. Výstup `Encrypt` je metoda `EncryptedXmlInfo` objektu. Tento objekt je obálky, který obsahuje výsledné enciphered `XElement` a typ, který představuje `IXmlDecryptor` které můžete používat k dešifrování odpovídající element.

Existují čtyři integrované konkrétní typy, které implementují třídu `IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

Najdete v článku [klíče šifrování v dokumentu rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) Další informace.

Chcete-li změnit výchozí klíč šifrování na rest mechanismus celou aplikaci, zaregistrovat vlastní `IXmlEncryptor` instance:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor` Rozhraní představuje typ, který umí k dešifrování `XElement` , byl enciphered prostřednictvím `IXmlEncryptor`. Zpřístupňuje jediného rozhraní API:

* Dešifrování (XElement encryptedElement): XElement

`Decrypt` Metoda vrátí zpět provádí šifrování `IXmlEncryptor.Encrypt`. Obecně platí, každý konkrétní `IXmlEncryptor` implementace bude mít odpovídající konkrétní `IXmlDecryptor` implementace.

Typy, které implementují rozhraní `IXmlDecryptor` musí mít jednu z následujících dvou veřejné konstruktory:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider` Předaný konstruktoru může mít hodnotu null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` Rozhraní představuje typ, který může provádět úschově citlivé informace. Odvolat serializované popisovače můžou obsahovat citlivé informace (například kryptografické materiálů), a to je co vedla k uvedení [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) zadejte na prvním místě. Ale dojít kvůli nehodám a klíč okruhy mohl být odstraněn, nebo dojde k poškození.

Poskytuje rozhraní úschově šrafování nouzový řídicí povolení přístupu k nezpracované serializovaných XML předtím, než je transformovat žádné nakonfigurované [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). Rozhraní zpřístupní jediného rozhraní API:

* Úložiště (identifikátor Guid keyId, XElement element)

Je až `IKeyEscrowSink` implementace zpracování zadaný element zabezpečeným způsobem konzistentní s firemní zásady. Jednou z možných implementací může být pro sink úschově k zašifrování elementu XML pomocí známých podnikové certifikátu X.509, kde má byla uloží privátní klíč certifikátu; `CertificateXmlEncryptor` typ s tím pomůže. `IKeyEscrowSink` Implementace zodpovídá taky za uložením zadaný element správně.

Ve výchozím nastavení je aktivován žádný mechanismus úschově, když správci serveru můžete [nakonfigurujte tato globální](xref:security/data-protection/configuration/machine-wide-policy). Můžete také nastavit prostřednictvím kódu programu prostřednictvím `IDataProtectionBuilder.AddKeyEscrowSink` metoda, jak je znázorněno v následující ukázka. `AddKeyEscrowSink` Zrcadlení přetížení metody `IServiceCollection.AddSingleton` a `IServiceCollection.AddInstance` přetížení, jako `IKeyEscrowSink` instance by měla být jednotlivé prvky. Pokud je to více `IKeyEscrowSink` instance jsou zaregistrovány, každé z nich bude volána při generování klíče, klíče může být uloží je na více mechanismů současně.

Neexistuje žádné rozhraní API pro čtení materiálu z `IKeyEscrowSink` instance. To je konzistentní s teoreticky návrhu mechanismu úschově: Tato má zpřístupněte materiál klíče pro důvěryhodnou autoritou, a vzhledem k tomu, že aplikace není samotné důvěryhodnou autoritou, by neměl mít přístup k vlastní escrowed materiálů.

Následující vzorový kód ukazuje vytvoření a registrace `IKeyEscrowSink` kde klíče se uloží tak, že je můžete obnovit pouze členové "CONTOSODomain Admins".

> [!NOTE]
> Chcete-li tuto ukázku spustit, musíte být v doméně systému Windows 8 / Windows Server 2012 počítače a řadič domény musí být Windows Server 2012 nebo novější.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
