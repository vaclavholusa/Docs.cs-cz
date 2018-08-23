---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Nestrukturované úložiště objektů Blob (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 9ca599e023ac9b1cf8f00389048c8da6ef0e364f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755279"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Nestrukturované úložiště objektů Blob (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


V předchozích kapitol jsme zvažovali schémata dělení a vysvětlení způsobu, jakým aplikace Fix It ukládá obrázky ve službě Azure Storage Blob a další data úlohy ve službě Azure SQL Database. V této kapitole jsme podrobněji služby Blob service a ukazují, jak je implementován v projektu kódu Fix It.

## <a name="what-is-blob-storage"></a>Co je služba Blob storage?

Služba Azure Storage Blob poskytuje způsob, jak ukládat soubory v cloudu. Blob service má několik výhod oproti ukládání souborů v systému souborů místní sítě:

- Je vysoce škálovatelná. Jeden účet úložiště můžete ukládat [stovky terabajtů](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), a může mít více účtů úložiště. Některé z největších zákazníkům Azure ukládat stovky petabajtů. Aplikace Microsoft SkyDrive využívá úložiště objektů blob.
- Je trvalý. Každý soubor, které ukládáte ve službě Blob service je automaticky zálohována.
- Poskytuje vysokou dostupnost. [Smlouva SLA pro Storage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) příslibů 99,99 % minimálně 99,9 % dostupnost, v závislosti na zvolené možnosti redundance geograficky zvolíte.
- Je součástí Azure, což znamená, že právě ukládání a načítání souborů, platíte pouze za skutečný objem úložiště, které používáte, platforma jako služba (PaaS) a Azure automaticky zajistí nastavovat a spravovat všechny virtuální počítače a požadované pro diskové jednotky Služba.
- Blob service můžete přistupovat pomocí rozhraní REST API nebo pomocí rozhraní API pro programovací jazyk. Sady SDK jsou dostupné pro .NET, Java, Ruby a dalších.
- Při ukládání souboru ve službě Blob service můžete snadno si je veřejně dostupný přes Internet.
- Můžete zabezpečit soubory v objektu Blob service, můžete k přístup jenom Autorizovaní uživatelé, nebo můžete poskytnout dočasný přístup tokeny, které zpřístupní jejich někdo pouze pro omezenou dobu.

Kdykoli už vytváříte aplikace pro Azure a chcete se ukládá velké množství dat, která v místním prostředí přejde v souborech – jako jsou obrázky, videa, soubory PDF, tabulky a podobně – zvažte službu Blob service.

## <a name="creating-a-storage-account"></a>Vytváří se účet úložiště

Začínáme se službou Blob service můžete vytvořit účet úložiště v Azure. Na portálu klikněte na tlačítko **nový** -- **datové služby** -- **úložiště** -- **rychlé vytvoření**, a potom zadejte adresu URL a umístěním datového centra. Umístěním datového centra by měl být stejný jako webové aplikace.

![Vytvořit úložiště acct](unstructured-blob-storage/_static/image1.png)

Vyberte primární oblasti, ve které chcete obsah uložit, a pokud se rozhodnete [geografickou replikaci](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) možnost, Azure vytvoří repliky svých dat v různých datových center v jiné oblasti země. Například pokud se rozhodnete datové centrum západ USA, při ukládání souboru, který přejde na datové centrum západ USA, ale na pozadí Azure také zkopíruje jej do jednoho z jiných datových centrech USA. V případě havárie v jedné oblasti země vaše data jsou stále bezpečné.

Azure nebudou data replikovat napříč několika geograficky politické hranice: Pokud je vaše primární umístění v USA, vaše soubory jsou pouze replikovat do jiné oblasti v USA, Pokud je vaše primární umístění Austrálie, soubory se replikují jenom do jiného datového centra v Austrálii.

Samozřejmě můžete také vytvoříte účet úložiště spuštěním příkazů ze skriptu, jak jsme viděli dříve. Tady je příkaz prostředí Windows PowerShell k vytvoření účtu úložiště:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Jakmile budete mít účet úložiště, můžete okamžitě začít ukládání souborů ve službě Blob service.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Používání úložiště objektů Blob v aplikace Fix It

Aplikace Fix It umožňuje nahrát fotografie.

![Vytvořte úlohu Fix It](unstructured-blob-storage/_static/image2.png)

Po kliknutí na **vytvořit automatickou**, aplikace nahraje zadaný soubor s obrázkem a ukládá ho do služby Blob service.

### <a name="set-up-the-blob-container"></a>Nastavení kontejneru objektů Blob

Za účelem uložení souboru ve službě Blob service je nutné *kontejneru* a uložit v. Kontejner objektů Blob služby odpovídá složku systému souborů. Skriptů pro vytvoření prostředí, které jsme se zaměřili [automatizovat všechno kapitoly](automate-everything.md) vytvořit účet úložiště, ale nechcete vytvářet kontejner. Takže účel `CreateAndConfigure` metodu `PhotoService` třídy, je vytvořit kontejner, pokud ještě neexistuje. Tato metoda je volána z `Application_Start` metoda *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Klíč název a přístup k účtu úložiště se ukládají v `appSettings` kolekce *Web.config* souborů a kódu v `StorageUtils.StorageAccount` metoda používá k sestavení připojovací řetězec a připojení těchto hodnot:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync` Metoda pak vytvoří objekt, který představuje službu Blob service a objekt, který představuje kontejner (složka) s názvem "Image" ve službě Blob service:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Pokud kontejner s názvem "Image" ještě neexistuje – které budou platit při prvním spuštění aplikace proti nový účet úložiště – kód vytvoří kontejner a nastaví oprávnění, zveřejněte ji. (Ve výchozím nastavení, nové kontejnery objektů blob jsou privátní a jsou přístupné pouze uživatelům, kteří mají oprávnění pro přístup k účtu úložiště.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Store nahrané fotky v úložišti objektů Blob

K nahrání a uložte soubor obrázku, tato aplikace používá `IPhotoService` rozhraní a implementaci rozhraní v `PhotoService` třídy. *PhotoService.cs* soubor obsahuje všechny kódu v Fix It aplikaci, která komunikuje se službou Blob service.

Následující metoda kontroleru MVC se volá, když uživatel klikne **vytvořit automatickou**. V tomto kódu `photoService` odkazuje na instanci `PhotoService` třídy, a `fixittask` odkazuje na instanci `FixItTask` třídu entity, která ukládá data pro nový úkol.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync` Metodu `PhotoService` třídy ukládá uloženého souboru ve službě Blob service a vrátí adresu URL, která odkazuje na nový objekt blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Stejně jako `CreateAndConfigure` metoda, kód se připojí k účtu úložiště a vytvoří objekt, který představuje kontejner objektů blob "Image", s výjimkou zde předpokládá kontejner již existuje.

Potom vytvoří jedinečný identifikátor pro image chystáte odeslat, zřetězením novou hodnotu GUID s příponou souboru:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Potom kód používá objekt kontejneru objektů blob a nový jedinečný identifikátor pro vytvoření objektu blob, nastaví atribut k tomuto objektu označující druh souboru je a potom použije objekt blob pro uložení souboru v úložišti objektů blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Nakonec získá adresu URL, která odkazuje na objekt blob. Tato adresa URL se uloží v databázi a je možné na webových stránkách Fix It k zobrazení této odeslané image.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Tato adresa URL je uloženo v databázi jako jeden ze sloupců tabulky FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Pouze adresu URL v databázi a obrázků v úložišti objektů Blob aplikace Fix It udržuje databázi malé, škálovatelné a cenově dostupné, zatímco Image se ukládají, pokud úložiště je levná a schopnou zvládat terabajtů nebo petabajtů. Jeden účet úložiště můžete ukládat stovky terabajtů Fix It fotografie a platíte jenom za využité. Abyste mohli začít malé platební 9 centů pro první GB a přidat další Image pro pennies každý další GB.

### <a name="display-the-uploaded-file"></a>Zobrazit nahraného souboru

Aplikace Fix It zobrazí soubor nahraný obrázek při zobrazení podrobností úlohy.

![Opravte podrobnosti úlohy s fotografií](unstructured-blob-storage/_static/image3.png)

A zobrazte obrázek, všechna zobrazení MVC musí provést je zahrnout `PhotoUrl` hodnotu ve formátu HTML odesláno prohlížeči. Webový server a databázi nepoužívají cykly a zobrazte obrázek, pouze obsluhují až několik bajtů na adresu URL obrázku. V následujícím kódu Razor `Model` odkazuje na instanci `FixItTask` třída entity.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Když se podíváte na kód HTML stránky, která se zobrazí, se zobrazí adresa URL odkazuje přímo na image v úložišti objektů blob, podobný následujícímu:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Souhrn

Už víte, jak aplikace Fix It ukládá obrázky do služby Blob service a pouze adresy URL obrázků ve službě SQL database. Použití služby Blob service uchovává SQL database mnohem menší, než ho jinak by umožňuje škálování až na téměř neomezené množství úloh a lze provést, aniž byste museli psát hodně kódu.

Stovky terabajtů může mít v účtu úložiště a náklady na úložiště jsou mnohem levnější než úložiště databáze SQL, počínaje přibližně 3 několik centů za GB měsíčně plus transakce malý poplatek. A pamatujte, že nejsou platit pro maximální kapacitu, ale pouze pro částku, kterou ve skutečnosti uložit, aby vaše aplikace je připravená pro škálování, ale nejsou platíte za všechno, dodatečnou kapacitu.

V [další kapitolu](design-to-survive-failures.md) budeme mluvit o důležitosti vytváření cloudové aplikace zvládne elegantně zpracovat selhání.

## <a name="resources"></a>Prostředky

Další informace naleznete na následujících odkazech:

- [Úvod do Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog o Mike dřeva.
- [Použití služby Azure Blob Storage v rozhraní .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Oficiální dokumentaci na webu MicrosoftAzure.com. Stručný úvod do objektu blob úložiště, za nímž následuje příklady kódu znázorňující způsob připojení k úložišti objektů blob, vytvoření kontejnerů, nahrávání a stahování objektů BLOB a tak dál.
- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Marc Mercuri a Mark Simms. Nabídne základními koncepty a Principy architektury tak vysoce dostupné a zajímavé příběhy z prostředí Microsoft zákazníka poradní tým (CAT) se skutečným zákazníkům. Informace o službě Azure Storage a objekty BLOB naleznete v tématu epizodě 5 počínaje 35:13.
- [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/dn568099.aspx). Model osobního klíče najdete v tématu.

> [!div class="step-by-step"]
> [Předchozí](data-partitioning-strategies.md)
> [další](design-to-survive-failures.md)
