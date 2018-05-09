---
title: Migrace z technologie ASP.NET na ASP.NET Core 2.0
author: isaac2004
description: Zobrazí pokyny k migraci existujících ASP.NET MVC nebo webového rozhraní API aplikací na technologii ASP.NET 2.0 jádra.
manager: wpickett
ms.author: scaddie
ms.date: 08/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc2
ms.openlocfilehash: f8e9d3c222bdd5491e946f286bbdf71a2d26ab10
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a>Migrace z technologie ASP.NET na ASP.NET Core 2.0

Podle [Isaac Levin](https://isaaclevin.com)

Tento článek slouží jako referenční příručka pro migraci aplikace ASP.NET na technologii ASP.NET 2.0 jádra.

## <a name="prerequisites"></a>Požadavky

Nainstalujte **jeden** z následujících [stáhne .NET: Windows](https://www.microsoft.com/net/download/windows):

* .NET core SDK
* Visual Studio pro Windows
  * **Vývoj pro ASP.NET a webové** pracovního vytížení
  * **Vývoj pro různé platformy .NET core** pracovního vytížení

## <a name="target-frameworks"></a>Cílové rozhraní
Projekty ASP.NET Core 2.0 nabízí vývojářům flexibilitu cílení na .NET Core, rozhraní .NET Framework nebo obojí. V tématu [výběru mezi .NET Core a rozhraní .NET Framework pro server aplikace](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) k určení, které cílové rozhraní je nejvhodnější.

Při cílení na rozhraní .NET Framework, třeba projekty odkazovat jednotlivých balíčků NuGet.

Cílení na .NET Core můžete omezit množství explicitní balíček odkazuje, díky technologii ASP.NET 2.0 základní [metapackage](xref:fundamentals/metapackage). Nainstalujte `Microsoft.AspNetCore.All` metapackage ve vašem projektu:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

Pokud je použita metapackage, se žádné balíčky, na kterou odkazuje metapackage nasazen s aplikací. Úložišti .NET Core Runtime obsahuje tyto prostředky, a že předkompilovaných ke zlepšení výkonu. V tématu [Microsoft.AspNetCore.All metapackage pro ASP.NET Core 2.x](xref:fundamentals/metapackage) další podrobnosti.

## <a name="project-structure-differences"></a>Rozdíly struktura projektu
*.Csproj* formát souboru je jednodušší v ASP.NET Core. Některé upozorňují na důležité změny patří:
- Explicitní zahrnutí souborů není nezbytné pro mají být považovány za součást projektu. Tím se snižuje riziko ke konfliktům sloučení XML při práci s velkými týmy.
- Nejsou žádné na základě GUID odkazy na další projekty, což zlepšuje čitelnost souboru.
- Soubor můžete upravit bez uvolnění v sadě Visual Studio:

    ![Upravit CSPROJ možnost místní nabídky v Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Nahrazení souboru Global.asax
ASP.NET Core zavedl nový mechanismus pro aplikaci zavádění. Vstupní bod pro aplikace ASP.NET je *Global.asax* souboru. Úlohy, jako je konfigurace směrování a registrace filtru a oblasti jsou zpracovávány v *Global.asax* souboru.

[!code-csharp[](samples/globalasax-sample.cs)]

Tento přístup páry v odstupu aplikace a serveru, na kterém je nasazená způsobem, který brání systému implementace. Ve snaze oddělit [OWIN](http://owin.org/) byla zavedena zajistit čisticí způsob, jak používat více rozhraní společně. OWIN poskytuje kanálu přidat pouze moduly, které jsou potřeba. Hostitelské prostředí trvá [spuštění](xref:fundamentals/startup) funkce při konfiguraci služby a aplikace požadavku kanálu. `Startup` zaregistruje sadu middlewaru s aplikací. Pro každý požadavek aplikace volá všechny komponenty middlewaru se head ukazatel odkazovaného seznamu existující sadu obslužné rutiny. Jednotlivé komponenty middlewaru můžete přidat jeden nebo více obslužné rutiny na žádost o zpracování kanálu. To se provádí vrácení odkaz na obslužná rutina, která je nové head seznamu. Každý obslužná rutina zodpovídá za zapamatování a vyvolání další obslužná rutina v seznamu. Pomocí ASP.NET Core, vstupní bod aplikace je `Startup`, a už jsou závislé na *Global.asax*. Při použití OWIN v rozhraní .NET Framework, použijte jako kanál nějak takto:

[!code-csharp[](samples/webapi-owin.cs)]

Konfiguruje výchozí trasy a výchozí XmlSerialization přes Json. Podle potřeby (načítání služeb, nastavení konfigurace, statické soubory, atd.), přidejte do tohoto kanálu další Middleware.

ASP.NET Core používá podobný postup, ale není spoléhají na OWIN pro zpracování položky. Místo toho, která se provádí prostřednictvím *Program.cs* `Main` metody (podobně jako konzolové aplikace) a `Startup` je načteno prostřednictvím zde.

[!code-csharp[](samples/program.cs)]

`Startup` musí zahrnovat `Configure` metoda. V `Configure`, přidejte nezbytné middleware do kanálu. V následujícím příkladu (na základě výchozí šablony webové stránky) několik rozšiřující metody slouží ke konfiguraci kanálu s podporou pro:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Chybové stránky
* Statické soubory
* Jádro ASP.NET MVC
* Identita

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Hostitele a aplikace byla odpojené který nabízí flexibilitu přesunutí na různé platformy v budoucnu.

**Poznámka:** podrobnější odkaz na spuštění základní technologie ASP.NET a Middleware, najdete v části [spuštění v ASP.NET Core](xref:fundamentals/startup)

## <a name="storing-configurations"></a>Ukládání konfigurace
Technologie ASP.NET podporuje ukládání nastavení. Tato nastavení se používají, například pro podporu prostředí, které byly nasazené aplikace. Běžnou praxí pro uložení všech vlastních páry klíč hodnota v `<appSettings>` části *Web.config* souboru:

[!code-xml[](samples/webconfig-sample.xml)]

Aplikace, přečtěte si tato nastavení pomocí `ConfigurationManager.AppSettings` kolekce v `System.Configuration` obor názvů:

[!code-csharp[](samples/read-webconfig.cs)]

ASP.NET Core můžete ukládat konfigurační data pro aplikace v žádném souboru a je v rámci middleware zavádění načíst. Výchozí soubor použitý v rámci šablon projektu je *appSettings.JSON určený*:

[!code-json[](samples/appsettings-sample.json)]

Načítání tento soubor do instance `IConfiguration` uvnitř vaší aplikace se provádí v *Startup.cs*:

[!code-csharp[](samples/startup-builder.cs)]

Aplikace načte z `Configuration` získat nastavení:

[!code-csharp[](samples/read-appsettings.cs)]

Existují rozšíření tohoto přístupu, aby proces robustnější, například pomocí [vkládání závislostí](xref:fundamentals/dependency-injection) (DI) načíst služby s těmito hodnotami. DI přístup poskytuje sadu objektů konfigurace silného typu.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Poznámka:** podrobnější odkaz na ASP.NET Core konfigurace, najdete v části [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Vkládání nativní závislostí
Důležité cílem při sestavování velkých škálovatelné aplikace je volné párování komponent a služeb. [Vkládání závislostí](xref:fundamentals/dependency-injection) je Oblíbené technika pro dosažení tohoto cíle, a je nativní součást ASP.NET Core.

V aplikacích ASP.NET vývojáři spoléhají na knihovnu třetích stran k implementaci vkládání závislostí. Jedna taková knihovna je [Unity](https://github.com/unitycontainer/unity), zadaný ve Microsoft Patterns & postupy. 

Příklad nastavení vkládání závislostí s Unity je implementace `IDependencyResolver` který zabalí `UnityContainer`:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Vytvoření instance vaše `UnityContainer`, registraci služby a nastavte překladač závislostí `HttpConfiguration` k nové instanci `UnityResolver` pro váš kontejner:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Vložit `IProductRepository` tam, kde je potřeba:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Protože vkládání závislostí je součástí ASP.NET Core, můžete přidat služby v `ConfigureServices` metodu *Startup.cs*:

[!code-csharp[](samples/configure-services.cs)]

Úložiště můžete vložit odkudkoli, jako byla platí Unity.

**Poznámka:** pro podrobné referenční informace k vkládání závislostí v ASP.NET Core, najdete v části [vkládání závislostí v ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serving-static-files"></a>Zpracování statických souborů.
Důležitou součást vývoj webu je schopnost poskytovat statické, klientské prostředky. Nejběžnější příkladem statické soubory jsou HTML, CSS, Javascript a obrázků. Tyto soubory muset být uložena v umístění s aplikace (nebo CDN) a tak mohou být načteny požadavkem na něj odkazovat. Tento proces se změnilo v ASP.NET Core.

V technologii ASP.NET jsou statické soubory uložené v různých adresářů a v zobrazení odkazuje.

V ASP.NET Core jsou statické soubory uložené v kořenu"web" (*&lt;obsahu kořenové&gt;/wwwroot*), pokud není uvedeno jinak. Soubory jsou načteny do kanálu požadavku vyvoláním `UseStaticFiles` metoda rozšíření z `Startup.Configure`:

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**Poznámka:** Pokud cílení na rozhraní .NET Framework, nainstalujte balíček NuGet `Microsoft.AspNetCore.StaticFiles`.

Například prostředek bitové kopie v *wwwroot nebo obrázky* , jako je přístupný v prohlížeči na umístění složky `http://<app>/images/<imageFileName>`.

**Poznámka:** podrobnější odkaz na zpracování statických souborů v ASP.NET Core, najdete v části [statické soubory](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Další zdroje

* [Portování knihovny .NET Core](/dotnet/core/porting/libraries)
