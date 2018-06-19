---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Úložiště objektů Blob nestrukturovaných (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: c2c82a579feb586287c40bb82eba53c5f84afaba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872569"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Úložiště objektů Blob nestrukturovaných (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


V předchozích kapitol jsme hledá v rozdělení schémata a vysvětlení, jak aplikaci opravte ukládá Image ve službě Azure Storage Blob a další data úloh ve službě Azure SQL Database. Tato kapitola jsme hlubší, přejděte do služby objektů Blob a zobrazit, jak jsou implementované v opravte ji kód projektu.

## <a name="what-is-blob-storage"></a>Co je služba Blob storage?

Služba Azure Storage Blob poskytuje způsob, jak ukládat soubory v cloudu. Služba objektů Blob má několik výhod oproti ukládání souborů v místní systém souborů:

- Je vysoce škálovatelná. Jeden účet úložiště můžete ukládat [stovky terabajtů](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), a může mít více účtů úložiště. Některé z největších Azure zákazníkům ukládat stovky petabajty. Microsoft SkyDrive používá úložiště objektů blob.
- Je odolný. Automaticky se zálohuje všechny soubory, které ukládáte ve službě Blob.
- Poskytuje vysokou dostupnost. [SLA pro úložiště](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) lišící 99,9 % nebo 99,99 % dostupnost, v závislosti na možnosti geografická redundance, kterou zvolíte.
- Je platforma jako služba (PaaS) funkce Azure, což znamená právě ukládání a načítání souborů, platícího pouze pro skutečné množství úložiště, které používáte, a Azure automaticky postará o nastavení a všechny virtuální počítače a diskových jednotek, které jsou potřebné pro správu Služba.
- Pomocí rozhraní REST API nebo pomocí programovacího jazyka rozhraní API, mají přístup ke službě Blob. Sady SDK jsou k dispozici pro rozhraní .NET, Java, Ruby a dalších.
- Pokud ukládáte soubor ve službě Blob, můžete snadno nastavit ho veřejně dostupné přes Internet.
- Můžete zabezpečit soubory v objektu Blob služby, můžete přístup jenom Autorizovaní uživatelé, nebo můžete zadat dočasné přístupové tokeny, které dává je k dispozici někdo pouze pro omezenou dobu.

Kdykoliv umožňuje vytvářet aplikace pro Azure a chcete ukládat velké množství dat, který v místním prostředí by se dostala v souborech – jako jsou bitové kopie, videa, souborů PDF, tabulek atd. – zvažte služby objektů Blob.

## <a name="creating-a-storage-account"></a>Vytvoření účtu úložiště

Abyste mohli začít se službou objektů Blob vytvořit účet úložiště v Azure. Na portálu, klikněte na tlačítko **nový** -- **datové služby** -- **úložiště** -- **rychle vytvořit**, a potom zadejte adresu URL a umístění datového centra. Umístění dat center by měl být stejný jako webové aplikace.

![Vytvoření úložiště acct](unstructured-blob-storage/_static/image1.png)

Vyberte primární oblasti, ve které chcete obsah uložit, a pokud zvolíte [geografická replikace](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) možnost, Azure vytvoří repliky všechna vaše data v různých datových center v jiné oblasti země. Například pokud se rozhodnete datového centra západ USA, pokud se ukládají soubory, který přejde do datového centra západ USA, ale na pozadí Azure také zkopíruje jej do jednoho z jiných datových centrech US. V případě havárie v jedné oblasti země vaše data jsou stále bezpečné.

Azure nebude replikování dat napříč politické geografické hranice: Pokud je váš primární umístění v USA, vaše soubory replikovány pouze na jiné oblasti do US. Pokud je váš primární umístění Austrálie, replikují se vaše soubory pouze do jiného datového centra v Austrálii.

Samozřejmě můžete také vytvoříte účet úložiště spuštěním příkazů ze skriptu, jak jsme viděli dříve. Tady je příkaz prostředí Windows PowerShell k vytvoření účtu úložiště:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Jakmile máte účet úložiště, můžete okamžitě začít ukládání souborů v rámci služby objektů Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>V aplikaci opravte ho pomocí úložiště objektů Blob

Aplikace opravte umožňuje nahrát fotografie.

![Vytvoření úlohy, opravte ji](unstructured-blob-storage/_static/image2.png)

Když kliknete na tlačítko **vytvořit automatickou**, aplikace odešle soubor zadanou bitovou kopii a ukládá je v rámci služby objektů Blob.

### <a name="set-up-the-blob-container"></a>Nastavit kontejner objektů Blob

Chcete-li uložit soubor ve službě Blob, musíte *kontejneru* uložit v. Kontejner objektů Blob služby odpovídá složky systému souborů. Vytvoření skripty prostředí, které jsme v [automatizovat všechno kapitoly](automate-everything.md) vytvořit účet úložiště, ale uživatel není vytvořit kontejner. Proto účel `CreateAndConfigure` metodu `PhotoService` třída je vytvořit kontejner, pokud ještě neexistuje. Tato metoda je volána z `Application_Start` metoda v *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Klíč název a přístup k účtu úložiště jsou uložené v `appSettings` kolekce *Web.config* souborů a kódu v `StorageUtils.StorageAccount` metoda používá tyto hodnoty a vytvořit připojovací řetězec připojení:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync` Metoda vytvoří objekt, který představuje službu objektu Blob a objekt, který představuje kontejner (složka) ve službě Blob s názvem "Image":

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Pokud kontejner s názvem "Image" ještě neexistuje – které bude platit při prvním spuštění aplikace proti nový účet úložiště – kód vytvoří kontejner a nastaví oprávnění je zveřejnit. (Ve výchozím nastavení, nové kontejnery objektů blob jsou soukromá a jsou dostupné pouze pro uživatele, kteří mají oprávnění pro přístup k účtu úložiště.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Uložit fotografii nahrané v úložišti objektů Blob

Nahrání a uložte soubor bitové kopie aplikace používá `IPhotoService` rozhraní a implementace rozhraní v `PhotoService` třídy. *PhotoService.cs* soubor obsahuje všechny kód opravte ji aplikace, která komunikuje se službou objektů Blob.

Následující metody řadiče MVC je volána, když uživatel klikne na **vytvořit automatickou**. V tomto kódu `photoService` odkazuje na instanci systému `PhotoService` třída, a `fixittask` odkazuje na instanci systému `FixItTask` třídy entita, která ukládá data pro novou úlohu.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync` Metoda v `PhotoService` třída ukládá nahrávaný soubor v rámci služby objektů Blob a vrátí adresu URL, která odkazuje na nový objekt blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Stejně jako na `CreateAndConfigure` metoda, kód se připojí k účtu úložiště a vytvoří objekt, který představuje kontejner objektů blob "Image", s výjimkou zde předpokládá kontejneru již existuje.

Potom vytvoří jedinečný identifikátor pro bitovou kopii Chystáte se nahrát zřetězením novou hodnotu GUID s příponou souboru:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Potom kód používá objekt kontejneru objektů blob a nový jedinečný identifikátor k vytvoření objektu blob, nastaví atribut tomuto objektu, která určuje, jaký typ souboru je a pak používá objekt blob k uložení souboru v úložišti objektů blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Nakonec získá adresu URL, která odkazuje na objekt blob. Tato adresa URL bude uložena v databázi a umožňuje ve webových stránkách opravte ji zobrazit nahraný obrázek.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Tato adresa URL je jako jeden ze sloupců tabulky FixItTask uloženy v databázi.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Jenom adresu URL v databázi, a bitové kopie v úložišti objektů Blob aplikace opravte udržuje databázi malé, škálovatelné a cenově nenáročné při bitové kopie jsou uloženy, kde je úložiště levných a umožňuje zpracovávat terabajtů nebo petabajty. Jeden účet úložiště můžete ukládat stovky terabajtů fotografie opravte ji a platíte jenom se používá. Abyste mohli začít malé platící 9 centů pro první gigabajt a přidat další Image pro pennies za další GB.

### <a name="display-the-uploaded-file"></a>Zobrazení nahrávaný soubor

Aplikace opravte zobrazí souboru nahrané bitové kopie, když se zobrazí podrobnosti pro úlohu.

![Opravte s fotografií podrobnosti úlohy](unstructured-blob-storage/_static/image3.png)

Pokud chcete zobrazit bitovou kopii, všechna zobrazení MVC musí provést je zahrnout `PhotoUrl` hodnotu ve formátu HTML odesláno prohlížeči. Webový server a databáze nepoužívají cykly zobrazíte bitovou kopii, jejich slouží pouze až několik bajtů na adresu URL obrázku. V následujícím kódu Razor `Model` odkazuje na instanci systému `FixItTask` třídy entita.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Pokud si prohlédnete kód HTML stránky, která zobrazuje, zobrazí adresu URL přejdete přímo na bitovou kopii v úložišti objektů blob, něco takto:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Souhrn

Už víte, jak aplikaci opravte ukládá bitové kopie do služby objektů Blob a jenom image adresy URL v databázi SQL. Pomocí služby objektů Blob udržuje databázi SQL mnohem menší, než ho jinak by umožňuje škálování až skoro neomezený počet úloh a lze provést bez nutnosti psaní velké množství kódu.

Máte stovky terabajtů v účtu úložiště a náklady na úložiště je mnohem míň nákladné než úložiště databáze SQL, počínaje přibližně 3 centů za GB za měsíc a poplatků malé transakce. A mějte na paměti, která nejsou platícího pro maximální kapacita, ale jenom pro částku, kterou je ve skutečnosti uložit, proto je aplikace připravená škálování, ale nejsou platícího pro všechno, co víc kapacity.

V [další kapitoly](design-to-survive-failures.md) budeme mluvit o význam provádění cloudové aplikace umožňuje řádně zpracovávat chyby.

## <a name="resources"></a>Prostředky

Další informace najdete v následujících zdrojích informací:

- [Úvod do Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog podle Karel dřeva.
- [Jak používat službu Azure Blob Storage v rozhraní .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Oficiální dokumentaci na webu MicrosoftAzure.com. Stručný úvod do objektu blob úložiště příklady kódu znázorňující způsob připojení k úložišti objektů blob, vytvoření kontejnerů, nahrávání a stahování objekty BLOB atd.
- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Uvede základními koncepty a architektury zásady způsobem, velmi přístupné a zajímavé, s scénářů čerpají z prostředí Microsoft zákazníka poradní tým (CAT) s skutečným zákazníkům. Popis služby Azure Storage a objekty BLOB najdete v díl 5 počínaje 35:13.
- [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/library/dn568099.aspx). Vzor klíč osobní služby najdete v tématu.

> [!div class="step-by-step"]
> [Předchozí](data-partitioning-strategies.md)
> [další](design-to-survive-failures.md)
