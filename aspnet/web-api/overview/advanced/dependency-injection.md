---
uid: web-api/overview/advanced/dependency-injection
title: "Vkládání závislostí v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: "Tento kurz ukazuje, jak se do vašeho řadiče rozhraní ASP.NET Web API vložit závislosti. Použité v kurzu Unity aplikace bloku 2 rozhraní API webové verze softwaru..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: b4cf39c59ed257b0014dbdbecef3eb7bc48f410d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Vkládání závislostí v rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Tento kurz ukazuje, jak se do vašeho řadiče rozhraní ASP.NET Web API vložit závislosti.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2
> - [Blok aplikace Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (verze 5 také funguje)


## <a name="what-is-dependency-injection"></a>Co je vkládání závislostí?

A *závislostí* je libovolný objekt, který vyžaduje jiný objekt. Například je běžné k definování [úložiště](http://martinfowler.com/eaaCatalog/repository.html) přístup k datům, která zpracovává. Umožňuje znázornění s příklad. Nejprve budeme definovat modelu domény:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Tady je jednoduché úložiště třídu, která ukládá položky v databázi pomocí Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Nyní nastavíme kontroleru webového rozhraní API, který podporuje požadavky GET pro `Product` entity. (I mě ponechat POST a další metody pro jednoduchost). Tady je první pokus o:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Všimněte si, že třídy kontroleru závisí na `ProductRepository`, a jsme jsou umožníte řadičem vytvořit `ProductRepository` instance. Je však vhodné pevný kód závislost tímto způsobem z několika důvodů.

- Pokud chcete nahradit `ProductRepository` s jinou implementaci, budete také muset upravit třídy kontroleru.
- Pokud `ProductRepository` má závislosti, je nutné nakonfigurovat tyto uvnitř kontroleru. Pro velké projekt s více řadiči bude konfigurace kódu rozmístěny na projektu.
- Je obtížné testů jednotek, protože kontroleru je pevně zakódovaná k dotazování databáze. Testování částí měli byste použít model nebo se zakázaným inzerováním úložiště, které není možné pomocí currect návrhu.

Budeme moci reagovat na tyto problémy podle *vložení* úložiště do kontroleru. Nejprve Refaktorovat `ProductRepository` třída do rozhraní:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Zadejte `IProductRepository` jako parametr konstruktoru:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Tento příklad používá [konstruktor vkládání](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Můžete také použít *setter vkládání*, kde můžete nastavit závislost prostřednictvím setter metody nebo vlastnosti.

Ale nyní došlo k potížím, protože aplikace nemá vytvoření řadiče přímo. Webové rozhraní API vytvoří řadiče, když ho přesměruje požadavek a webového rozhraní API neví nic `IProductRepository`. Toto je, kde odeslán překladač závislostí webového rozhraní API.

## <a name="the-web-api-dependency-resolver"></a>Překladač závislostí webového rozhraní API

Definuje webové rozhraní API **IDependencyResolver** rozhraní pro řešení závislostí. Tady je definici rozhraní:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope** rozhraní má dvě metody:

- **Metody GetService** vytvoří jednu instanci typu.
- **Metodu GetServices** vytvoří kolekci objektů zadaného typu.

**IDependencyResolver** metoda dědí **IDependencyScope** a přidá **BeginScope** metoda. O oborech I mluvit později v tomto kurzu.

Když webového rozhraní API vytvoří instanci řadiče, první volání **IDependencyResolver.GetService**a předejte typ kontroleru. Toto rozšíření háku můžete použít k vytvoření kontroleru, řešení všechny závislosti. Pokud **metody GetService** vrátí hodnotu null, vypadá webového rozhraní API pro konstruktor bez parametrů na třídy kontroleru.

## <a name="dependency-resolution-with-the-unity-container"></a>Zjištění závislosti, s kontejnerem Unity

I když můžete napsat úplná **IDependencyResolver** implementace od začátku, rozhraní je skutečně navržena tak, aby fungoval jako most mezi webového rozhraní API a existující IoC kontejnerů.

Kontejner IoC je softwarová součást, která je zodpovědná za správu závislosti. Zaregistrujte typy s kontejner a poté použít k vytvoření objektů kontejneru. Kontejner se automaticky hodnoty se vztahy závislostí. Mnoho IoC kontejnery umožňují řídit třeba životní cyklus objektů a oboru.

> [!NOTE]
> "IoC" znamená "inverzi řízení", což je obecný vzor, kde rozhraní volání do kódu aplikace. Kontejner IoC vytvoří vašich objektů, které "Invertuje" obvyklý tok řízení.


V tomto kurzu použijeme [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) z Microsoft Patterns &amp; postupy. (Zahrnout další oblíbené knihovny [rošády Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), a [StructureMap ](http://docs.structuremap.net/).) Správce balíčků NuGet můžete použít k instalaci Unity. Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Zde je implementací **IDependencyResolver** který zabalí kontejner Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Pokud **metody GetService** metoda nelze přeložit typ, by měla vrátit **null**. Pokud **metodu GetServices** metoda nelze přeložit typ, měla by vrátit objekt prázdnou kolekci. Nemáte generování výjimek pro neznámé typy.


## <a name="configuring-the-dependency-resolver"></a>Konfigurace překladač závislostí

Nastavit překladač závislostí **překladače závislostí** vlastnost globální **HttpConfiguration** objektu.

Následující kód registruje `IProductRepository` rozhraní s Unity a poté vytvoří `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Závislost oboru a dobu života řadiče

Řadiče se vytvářejí na základě požadavku. Ke správě životnosti objektu **IDependencyResolver** používá koncept *oboru*.

Překladač závislostí, který je připojen k **HttpConfiguration** objekt má globální obor. Webové rozhraní API, vytvoří řadič, zavolá **BeginScope**. Tato metoda vrátí **IDependencyScope** představující podřízeném oboru.

Pak zavolá rozhraní Web API **metody GetService** v podřízeném oboru vytvoření kontroleru. Po dokončení žádosti o volání webového rozhraní API **Dispose** v podřízeném oboru. Použití **Dispose** metodu dispose závislostí kontroleru.

Jak implementovat **BeginScope** závisí na kontejner IoC. Pro Unity odpovídá oboru kontejneru podřízené:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Většina kontejnery IoC mají podobné ekvivalenty.
