---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: "Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webové formuláře – část 8 | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET používající rozhraní Entity Framework. Vzorová aplikace je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 323ee44f43f6d4081bd9ba50791755696bc9128f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Začínáme s databáze Entity Framework 4.0 nejprve a 4 webových formulářů ASP.NET - část 8
====================
podle [tní Dykstra](https://github.com/tdykstra)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0. Informace o kurzu řady najdete v tématu [první kurz v řadě](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Pomocí funkce Dynamická Data můžete naformátovat a ověřování dat

V tomto kurzu předchozí implementována uložené procedury. Tento kurz vám ukáže, jak funkce Dynamická Data můžou poskytovat následující výhody:

- Pole jsou automaticky formátovaná pro zobrazení na základě jejich datového typu.
- Pole se ověří automaticky na základě jejich datového typu.
- Můžete přidat metadata do datového modelu k přizpůsobení chování formátování a ověřování. Když to uděláte, můžete přidat pravidla formátování a ověřování na jenom jednom místě a jste automaticky použít všude, kde je přístup pole použití ovládacích prvků Dynamická Data.

Pokud chcete zobrazit, jak to funguje, změníte ovládací prvky můžete použít k zobrazení a úprava polí v existující *Students.aspx* stránky a vy budete přidat formátování a ověřování metadata do pole název a datum `Student` typ entity.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>Pomocí DynamicField a DynamicControl ovládací prvky

Otevřete *Students.aspx* stránky a v `StudentsGridView` nahradit ovládacího prvku **název** a **datum registrace** `TemplateField` prvky s následující kód:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

Tento kód používá `DynamicControl` řídí místě `TextBox` a `Label` šablony pole pro název ovládacích prvků v student a používá `DynamicField` ovládací prvek pro datum registrace. Nejsou zadány žádné řetězce formátu.

Přidat `ValidationSummary` řídit po `StudentsGridView` ovládacího prvku.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

V `SearchGridView` nahraďte kód pro ovládací prvek **název** a **datum registrace** v sadě sloupců, jako je `StudentsGridView` řídit, s výjimkou vynechejte `EditItemTemplate` element. `Columns` Element `SearchGridView` řízení nyní obsahuje následující kód:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

Otevřete *Students.aspx.cs* a přidejte následující `using` příkaz:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

Přidání obslužné rutiny pro stránky `Init` událostí:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

Tento kód určuje, zda Dynamická Data vám poskytne formátování a ověřování v tyto ovládací prvky vázané na data pro pole `Student` entity. Pokud se zobrazí chybová zpráva jako v následujícím příkladu, při spuštění stránky, obvykle znamená jste zapomněli k volání `EnableDynamicData` metoda v `Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

Spuštění stránky.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

V **datum registrace** sloupce, čas se zobrazí spolu s datum, protože je vlastnost typu `DateTime`. Později, budete opravte.

Teď Všimněte si, že dynamická Data automaticky poskytuje základní data ověření. Klikněte například na **upravit**, zrušte zaškrtnutí políčka datum, klikněte na **aktualizace**, a zjistíte, že dynamická Data automaticky provede to povinné pole vzhledem k tomu, že hodnota není null v datovém modelu. Na stránce se zobrazuje hvězdička po pole a chybovou zprávu ve `ValidationSummary` ovládacího prvku:

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

Může vynechat `ValidationSummary` řídit, protože také můžete podržet ukazatel myši nad hvězdičky zobrazíte chybovou zprávu:

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

Dynamická Data budou také ověřit data zadaná v **datum registrace** pole je platné datum:

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

Jak vidíte, toto je obecnou chybovou zprávu. V další části se zobrazí postup přizpůsobení formátování pravidel a také ověřování a zprávy.

## <a name="adding-metadata-to-the-data-model"></a>Přidání metadat do datového modelu

Obvykle který chcete přizpůsobit funkce poskytované službou Dynamická Data. Například můžete změnit způsob zobrazení dat a obsah chybové zprávy. Je obvykle také přizpůsobit pravidla ověření dat více funkcí než Dynamická Data poskytují automaticky podle datové typy. K tomu, vytvoření částečné třídy, které odpovídají typy entit.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projekt, vyberte **přidat odkaz na**a přidejte odkaz na `System.ComponentModel.DataAnnotations`.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

V *DAL* složku, vytvořte nový soubor třídy a pojmenujte ji *Student.cs*a nahraďte kód šablony v ní následující kód.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

Tento kód vytvoří pro konkrétní třídu `Student` entity. `MetadataType` Použít pro tuto konkrétní třídu atribut určuje třídu, která používáte k určení metadat. Třídy metadat může mít libovolný název, ale pomocí název entity plus "Metadata" je běžnou praxi.

Atributy použité na vlastnosti ve třídě metadat zadejte formátování, ověření, pravidla a chybové zprávy. Atributy zobrazeny zde bude mít následující výsledky:

- `EnrollmentDate`Zobrazí datum (bez čas).
- Obě název pole musí být 25 znaků nebo méně v délku a vlastní chybové zprávy je k dispozici.
- Jsou požadovány obě pole název a vlastní chybové zprávy je k dispozici.

Spustit *Students.aspx* stránku znovu a zjistíte, že data se nyní zobrazují bez časy:

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

Upravte řádek a zkuste vymazání hodnot v polích pro název. Hvězdičky označující pole chyb se zobrazí, jakmile necháte pole, před kliknutím na **aktualizace**. Když kliknete na tlačítko **aktualizace**, na stránce se zobrazí text chybové zprávy, jste zadali.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

Zkuste zadat názvy, které jsou delší než 25 znaků, klikněte na tlačítko **aktualizace**, a na stránce se zobrazí text chybové zprávy, jste zadali.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

Teď, když jste nastavili tato pravidla formátování a ověřování v metadatech datového modelu, pravidla se automaticky použijí na každé stránce, která zobrazuje nebo umožňuje změny těchto polí, dokud bude používat `DynamicControl` nebo `DynamicField` ovládací prvky. Tím se snižuje množství redundantní kód, který máte k zápisu, který provede programování a testování jednodušší, a zajišťuje, že jsou konzistentní v rámci aplikace formátování data a ověření.

## <a name="more-information"></a>Další informace

Tím končí tato série kurzů na Začínáme s platformou Entity Framework. Další zdroje můžete Naučte se používat rozhraní Entity Framework, pokračujte [z prvního kurzu další kurz řady Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) nebo najdete na následujících stránkách:

- [Rozhraní Entity Framework – nejčastější dotazy](http://www.ef-faq.org/introduction.html)
- [Blog týmu Entity Framework](https://blogs.msdn.com/b/adonet/)
- [Rozhraní Entity Framework v knihovně MSDN](https://msdn.microsoft.com/library/bb399572.aspx)
- [Rozhraní Entity Framework v Centru pro vývojáře MSDN dat](https://msdn.microsoft.com/data/ef.aspx)
- [Přehled ovládacího prvku EntityDataSource webového serveru v knihovně MSDN](https://msdn.microsoft.com/library/cc488502.aspx)
- [Ovládací prvek EntityDataSource referenční dokumentace rozhraní API v knihovně MSDN](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [Fóra Entity Framework na webu MSDN](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Blog Julie Lerman](http://thedatafarm.com/blog/)

>[!div class="step-by-step"]
[Předchozí](the-entity-framework-and-aspnet-getting-started-part-7.md)
