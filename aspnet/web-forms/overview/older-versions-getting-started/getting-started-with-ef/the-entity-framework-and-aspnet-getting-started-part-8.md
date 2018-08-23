---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů – 8. část | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Frameworku. Ukázková aplikace je...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 249677c9e0eca248710bf730e57a7aeca5a9b5ce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754303"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Začínáme s Entity Framework 4.0 Database First a 4 webových formulářů ASP.NET – 8. část
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Funkce dynamických dat pro formátování a ověřování dat

V předchozím kurzu jste implementovali uložené procedury. Tomto kurzu se dozvíte, jak mohou funkci Dynamická Data poskytuje následující výhody:

- Pole jsou automaticky formátovat k zobrazení na základě jeho datového typu.
- Pole se automaticky ověří na základě jeho datového typu.
- Přidat metadata do datového modelu můžete přizpůsobit chování formátování i ověřování. Když to uděláte, můžete přidat pravidla formátování a validace na jenom jednom místě a jste automaticky použít všude, kde máte přístup k poli pomocí dynamické databázové ovládací prvky.

Pokud chcete zobrazit, jak to funguje, změníte ovládací prvky, které použijete k zobrazení a úprava polí v existujícím *Students.aspx* stránky a přidejte formátování i ověřování metadat na pole name a date `Student` typu entity.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Použití objektu DynamicControl ovládací prvky a DynamicField

Otevřít *Students.aspx* stránku a `StudentsGridView` nahraďte ovládací prvek **název** a **datum registrace** `TemplateField` prvky s následující kód:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Tento kód používá `DynamicControl` řídí místo `TextBox` a `Label` ovládacích prvků v studenta šablony pole pro název serveru a používá `DynamicField` ovládací prvek pro datum registrace. Nejsou zadány žádné řetězce formátu.

Přidat `ValidationSummary` řídit po `StudentsGridView` ovládacího prvku.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

V `SearchGridView` nahradit kód pro ovládací prvek **název** a **datum registrace** sloupců, jako jste to udělali v `StudentsGridView` řídit, s výjimkou vynechat, nechte `EditItemTemplate` elementu. `Columns` Elementu `SearchGridView` ovládací prvek nyní obsahuje následující kód:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Otevřít *Students.aspx.cs* a přidejte následující `using` – příkaz:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Přidání obslužné rutiny pro stránky `Init` události:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Tento kód určuje, jestli Dynamická Data poskytne formátování i ověřování v těchto ovládacích prvků vázaných na data pro pole `Student` entity. Pokud se zobrazí chybová zpráva jako v následujícím příkladu, při spuštění stránky, obvykle to znamená, jste zapomněli volat `EnableDynamicData` metoda ve `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Spuštění stránky.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

V **datum registrace** sloupce, čas se zobrazí spolu s datem, protože je vlastnost typu `DateTime`. Opravíte to později.

Nyní Všimněte si, dynamická Data automaticky poskytuje základní data ověření. Klikněte například na **upravit**, zrušte zaškrtnutí pole data, klikněte na tlačítko **aktualizace**, a uvidíte, že dynamických dat automaticky provede to povinné pole. protože hodnota není s možnou hodnotou Null v datovém modelu. Na stránce se zobrazí hvězdička po pole a chybovou zprávu ve `ValidationSummary` ovládacího prvku:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Může vynechat `ValidationSummary` ovládací prvek, protože můžete také podržíte ukazatel myši nad na hvězdičku chybová zpráva:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dynamická Data také ověří data zadaná v **datum registrace** pole je platné datum:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Jak vidíte, toto je obecné chybové zprávy. V další části uvidíte, jak přizpůsobit zprávy stejně jako ověřování a formátování pravidla.

## <a name="adding-metadata-to-the-data-model"></a>Přidání metadat do datového modelu

Obvykle budete chtít přizpůsobit funkce poskytované službou Dynamická Data. Například můžete změnit způsob zobrazení dat a obsahu chybové zprávy. Obvykle také přizpůsobíte data ověřovacích pravidel poskytnout více funkcí, než jaké poskytuje dynamických dat automaticky na základě typu dat. K tomuto účelu vytvoříte částečné třídy, které odpovídají typy entit.

V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projekt, vyberte **přidat odkaz**a přidejte odkaz na `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

V *DAL* složku, vytvořte nový soubor třídy, pojmenujte ho *Student.cs*a nahraďte kód šablony do něj následující kód.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Tento kód vytvoří částečnou třídu pro `Student` entity. `MetadataType` Použije pro tuto částečnou třídu Určuje třídu, která používáte k určení metadat. Metadata třída může mít libovolný název, ale pomocí "Metadat" a název entity je běžnou praxí.

Atributy použité na vlastnosti ve třídě metadat určit formátování, ověření, pravidla a chybové zprávy. Atributy uvedené v tomto poli mít následující výsledky:

- `EnrollmentDate` Zobrazí se jako datum (bez času).
- Musí být obě pole Název 25 znaků nebo méně, a vlastní chybové zprávy je k dispozici.
- Jsou požadovány obě pole název a vlastní chybové zprávy je k dispozici.

Spustit *Students.aspx* stránku znovu nezobrazovat a uvidíte, že data se teď zobrazují bez časy:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Upravit řádek a zkuste vymažte hodnoty v poli název. Hvězdičky označující chyby pole se zobrazí, co nejdříve nechte pole, před kliknutím na **aktualizace**. Po kliknutí na **aktualizace**, na stránce se zobrazí zadaný text chybové zprávy.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Zkuste zadat názvy, které jsou maximálně 25 znaků, klikněte na tlačítko **aktualizace**, a na stránce se zobrazí zadaný text chybové zprávy.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Teď, když jste nastavili tato pravidla formátování i ověřování v metadatech datového modelu, pravidla se automaticky použijí na každou stránku, která zobrazí nebo umožňuje, aby změny těchto polí, tak dlouho, dokud používáte `DynamicControl` nebo `DynamicField` ovládacích prvků. To snižuje množství redundantní kód, který musíte napsat, takže programování a testování jednodušší a zajišťuje, že data formátování i ověřování jsou konzistentní vzhledem k aplikacím v celé aplikaci.

## <a name="more-information"></a>Další informace

Tím končí této sérii kurzů na Začínáme s Entity Framework. Další zdroje vám pomůžou získat informace tom, jak používat rozhraní Entity Framework, pokračujte [z prvního kurzu další Entity Framework sérii](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) nebo naleznete na následujících stránkách:

- [Entity Framework – nejčastější dotazy](http://www.ef-faq.org/introduction.html)
- [Blog týmu Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Entity Framework v knihovně MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework v datovém centru pro vývojáře MSDN](https://msdn.microsoft.com/data/ef.aspx)
- [Přehled ovládacího prvku EntityDataSource webového serveru v knihovně MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Ovládací prvek EntityDataSource reference k rozhraní API v knihovně MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Entity Framework fóra na webu MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie Lerman blogu](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [Předchozí](the-entity-framework-and-aspnet-getting-started-part-7.md)
