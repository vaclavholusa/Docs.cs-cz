---
title: "Vytvoření profilů publikování pro Visual Studio a nástroje MSBuild"
author: rick-anderson
description: "Popisuje publikování na webu v sadě Visual Studio."
keywords: "Web ASP.NET Core, publikování, publikování, msbuild, nasazení webu, publikovat dotnet, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>Vytvoření publikační profily pro Visual Studio a nástroje MSBuild, jak nasadit aplikace ASP.NET Core

Podle [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Tento článek se týká použití Visual Studio 2017 k vytvoření publikační profily. Profily publikování, vytvořené pomocí sady Visual Studio můžete spustit z nástroje MSBuild a Visual Studio 2017. Článek obsahuje podrobné informace o procesu publikování. V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny k publikování do Azure.

Následující *.csproj* soubor byl vytvořen pomocí příkazu `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

`Sdk` Atribut `<Project>` elementu (v první řádek) výše uvedený kód provede následující akce:

* Importy `props` souboru z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na začátku.
* Importuje soubor cíle z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na konci.

Výchozím umístěním pro `MSBuildSDKsPath` (s Visual Studio 2017 Enterprise) je *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* složky.

`Microsoft.NET.Sdk.Web`závisí na:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Což způsobí, že následující `props` a `targets` určených k importu:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Publikujte cíle bude znovu importovat správnou sadu cílů založené na metodě publikování použít.

Při načtení projektu nástroje MSBuild nebo Visual Studio jsou prováděny následující akce vysoké úrovně:

* Sestavení projektu
* Výpočetní soubory k publikování
* Publikovat soubory do cílového umístění

### <a name="compute-project-items"></a>Výpočetní položky projektu

Při načtení projektu, se vypočítávají položky projektu (soubory). `item type` Atribut určuje způsob zpracování souboru. Ve výchozím nastavení *.cs* souborů jsou součástí `Compile` seznamu položek. Soubory `Compile` kompilované seznamu položek.

`Content` Seznamu položek obsahuje soubory, které budou publikovány kromě výstupy sestavení. Ve výchozím nastavení, soubory odpovídající vzor wwwroot / ** budou zahrnuty do `Content` položky. [Wwwroot / ** je vzor expanze názvů](https://gruntjs.com/configuring-tasks#globbing-patterns) určující všechny soubory v *wwwroot* složky **a** podsložky. Pokud chcete explicitně přidat do seznamu publikovat soubor, přidejte přímo v souboru *.csproj* souboru, jak je znázorněno v [soubory včetně](#including-files).

Když vyberete **publikovat** tlačítko v sadě Visual Studio nebo při publikování z příkazového řádku:

- Nebude vlastnosti se vypočítávají (soubory, které jsou potřebné k vytvoření).
- Visual Studio pouze: obnovení balíčků NuGet.  (Obnovit, musí být explicitní uživatelem v rozhraní příkazového řádku).
- Sestavení projektu.
- Publikování položky se vypočítávají (soubory, které jsou potřebné k publikování).
- Po publikování projektu. (Počítaný soubory se zkopírují do cílového umístění publikovat).

## <a name="basic-command-line-publishing"></a>Publikování základní příkazového řádku

Tato část funguje na všech platformách podporované .NET Core a nevyžaduje Visual Studio. V níže, ukázky `dotnet publish` příkaz spuštěn z adresáře projektu (obsahující *.csproj* souboru). Pokud si nejste ve složce projektu, které lze předat explicitně v cestě k souboru projektu. Příklad:

```console
dotnet publish  c:/webs/web1
```

Spusťte následující příkazy k vytvoření a publikování webové aplikace:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

`dotnet publish` Vytváří výstup podobný následujícímu:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Výchozí nastavení publikování složky je `bin\$(Configuration)\netcoreapp<version>\publish`. Výchozí hodnota pro `$(Configuration)` je ladění. V ukázce výše `<TargetFramework>` je `netcoreapp2.0`.

`dotnet publish -h`Zobrazí nápovědu informace pro publikování.

Určuje následující příkaz `Release` sestavení a publikování adresáře:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

`dotnet publish` Příkaz volání `MSBuild` který vyvolá `Publish` cíl. Žádné parametry předaný `dotnet publish` jsou předávány `MSBuild`. `-c` Parametr se mapuje `Configuration` vlastnosti MSBuild. `-o` Parametr mapuje `OutputPath`.

Můžete předat vlastnosti nástroje MSBuild některou z následujících formátů:

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

Publikuje následující příkaz `Release` sestavení do sdílené síťové složky:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Sdílené síťové složce je zadán s lomítkem (*//r8/*) a funguje na všech platformách .NET Core podporována.

Potvrďte, že není spuštěn publikované aplikace pro nasazení. Soubory *publikování* složky jsou zamčené, když aplikace běží. Nasazení nemůže proběhnout, protože uzamčen, že soubory nelze kopírovat.

## <a name="publish-profiles"></a>Publikační profily

Tato část používá k vytvoření profilů publikování Visual Studio 2017 a vyšší. Po vytvoření můžete publikovat ze sady Visual Studio nebo pomocí příkazového řádku.

Publikování profily může zjednodušit proces publikování. Můžete mít více publikační profily. Pokud chcete vytvořit profil publikování v sadě Visual Studio, klikněte pravým tlačítkem na projekt v prozkoumat řešení a vyberte **publikovat**. Alternativně můžete vybrat **publikovat \<název projektu >** nabídce sestavení. **Publikovat** kapacity stránky aplikace se zobrazí. Pokud projekt neobsahuje profil publikování, zobrazí se následující stránka:

![** Publikovat ** kartu aplikace kapacity stránky zobrazující Azure, služby IIS, FTB, vybrána složka s Azure. Také ukazuje vytvořit nový a vyberte Exiting přepínací tlačítka](web-publishing-vs/_static/az.png)

Při **složky** je vybrána, **publikovat** tlačítko vytvoří složku profil publikování a publikuje.

![** Karta publikovat ** aplikace kapacity stránky zobrazující Azure, služby IIS, FTB, složky](web-publishing-vs/_static/pub1.png)

Po vytvoření profilu publikování **publikovat** kartě změny a vyberte **vytvořit nový profil** k vytvoření nového profilu.

![** Karta publikovat ** aplikace kapacity stránky zobrazující FolderProfile-vytvořili v posledním kroku a vytvořit nový profil odkaz](web-publishing-vs/_static/create_new.png)

Průvodce Publikovat podporuje následující cíle publikování:

- Microsoft Azure App Service
- Služba IIS, FTP atd., (pro všechny webové servery)
- Folder
- Import profilu (umožňuje importovat profil).
- Virtuální počítače Microsoft Azure

V tématu [jaké možnosti publikování je pro mě nejlepší?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) Další informace.

Když vytvoříte profil publikování pomocí sady Visual Studio *vlastnosti/PublishProfiles/\<název publikovat > .pubxml* k vytvoření souboru MSBuild. To *.pubxml* soubor je soubor MSBuild a obsahuje konfigurační nastavení publikování. Tento soubor pro přizpůsobení sestavení a publikování procesu, můžete změnit. Proces publikování je pro čtení tohoto souboru. `<LastUsedBuildConfiguration>`je speciální, protože je globální vlastnost a by neměl být ve všech souborů, které je v sestavení naimportována. V tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Další informace.
*.Pubxml* souboru by neměla být zaškrtnuta do správy zdrojového kódu, protože závisí na *.uživatel* souboru. *.Uživatel* soubor by měl zkontrolovat nikdy do správy zdrojového kódu, protože ho mohou obsahovat citlivé údaje a jeho je platná pouze pro jednoho uživatele a počítače.

Se šifrují citlivých informací (jako je publikování heslo) na uživatele nebo počítač úrovně a uložené v *vlastnosti/PublishProfiles/\<název publikovat >. pubxml.user* souboru. Protože tento soubor může obsahovat citlivé informace, by měl **není** se změnami do správy zdrojového kódu.

Přehled o tom, jak publikovat webovou aplikaci na ASP.NET Core najdete [publikování a nasazení](index.md). [Publikování a nasazení](index.md) je opensourcový projekt v https://github.com/aspnet/websdk.

 `dotnet publish`můžete použít složku, Msdeploy, a [KUDU](https://github.com/projectkudu/kudu/wiki) publikační profily:
 
Složku (funguje napříč platformami)`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

MSDeploy (aktuálně toto funguje pouze v systému windows, protože msdeploy není napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

Balíček MSDeploy (aktuálně toto funguje pouze v systému windows, protože msdeploy není napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

V předcházející ukázky **nemáte** předat `deployonbuild` k `dotnet publish`.

Další informace najdete v tématu [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)

`dotnet publish`podporuje rozhraní API modulu KUDU pro publikování do služby Azure z libovolné platformě. Visual Studio publikovat nepodporuje podporu rozhraní API modulu KUDU, ale podporuje websdk pro křížové plat publikování v Azure.

Přidat profil publikování do *vlastnosti nebo PublishProfiles* složku s následujícím obsahem:

```xml
<Project>
<PropertyGroup>
                <PublishProtocol>Kudu</PublishProtocol>
                <PublishSiteName>nodewebapp</PublishSiteName>
                <UserName>username</UserName>
                <Password>password</Password>
</PropertyGroup>
</Project>
```

Spuštěním následujícího příkazu zip si obsah publikovat a publikujete ho v Azure pomocí rozhraní API modulu KUDU.

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Při použití profil publikování, nastavte následující vlastnosti nástroje MSBuild:

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

Například při publikování k profilu s názvem *FolderProfile* můžete provést některou z níže uvedených příkazů.

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Při vyvolání `dotnet build` bude volat `msbuild` spustit sestavení a publikování procesu. Volání metody `dotnet build` nebo `msbuild` je v podstatě ekvivalentní při předání ve složce profilu. Při volání metody MSBuild přímo v systému Windows můžete získat verzi rozhraní .NET Framework nástroje MSBuild.  MSDeploy je aktuálně omezená na počítačích s Windows pro publikování. Volání metody `dotnet build` na do jiné složky profil vyvolá MSBuild a MSBuild používá MSDeploy v jiné složce profily. Volání metody `dotnet build` na jiný složky profilu vyvolá MSBuild (pomocí MSDeploy) a má za následek selhání (i když běžících na platformě Windows). Pokud chcete publikovat pomocí profilu pro jiné složky, přímo voláním funkce MSBuild.

Profil publikování se následující složce byl vytvořen pomocí sady Visual Studio a publikuje do sdílené síťové složky:

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

Poznámka: `<LastUsedBuildConfiguration>` je nastaven na `Release`.  Při publikování ze sady Visual Studio `<LastUsedBuildConfiguration>` je nastavena hodnota vlastnosti konfigurace, pomocí hodnoty, když se spustí proces publikování. `<LastUsedBuildConfiguration>` Vlastnosti konfigurace je speciální a by nemělo být přepsáno v importovaného souboru MSBuild. Tato vlastnost z příkazového řádku můžete přepsat. Příklad:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Pomocí nástroje MSBuild:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publikovat do koncového bodu MSDeploy z příkazového řádku

Jak už jsme zmínili, které můžete publikovat pomocí `dotnet publish` nebo `msbuild` příkaz. `dotnet publish`běží v kontextu .NET Core. `msbuild`vyžaduje rozhraní .NET framework a proto je omezený na prostředí Windows.

Nejjednodušší způsob, jak publikovat s MSDeploy je nejdřív vytvořte profil publikování ve Visual Studio 2017 a použít profil z příkazového řádku.

V následující ukázce je vytvoření webové aplikace ASP.NET Core (pomocí `dotnet new mvc`) a přidají se profilem publikování Azure pomocí sady Visual Studio.

Při spuštění `msbuild` z **příkazový řádek vývojáře pro VS 2017**. Příkazový řádek vývojáře budou mít správný *msbuild.exe* v jeho cestu a nastavte některé nástroje MSBuild proměnné.

MSBuild používá následující syntaxe:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Můžete získat `Password` z  *\<publikovat name >. PublishSettings* souboru. Si můžete stáhnout *. PublishSettings* ze souboru:

- Průzkumník řešení: klikněte pravým tlačítkem na webovou aplikaci a vyberte **stažení profilu publikování**.
- Azure Management Portal: Vyberte **profilu publikování Get** v okně webové aplikace.

`Username`můžete najít v profilu publikování.

Profil publikování se následující ukázka používá "Web11112 – nasazení webu":

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Vyloučení souborů

Při publikování webové aplikace ASP.NET Core, sestavení artefaktů a obsah *wwwroot* složky jsou zahrnuty. `msbuild`podporuje [expanze názvů vzory](https://gruntjs.com/configuring-tasks#globbing-patterns). Například následující `<Content>` element značek vyloučí všechny text (*.txt*) souborů z *wwwroot a obsah* složce a jejích podsložkách.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Výše uvedený kód lze přidat do profilu publikování nebo *.csproj* souboru. Když je přidán do *.csproj* , pravidlo se přidá soubor všechny profily publikování v projektu.

Následující `<MsDeploySkipRules>` element značek vyloučeny všechny soubory z *wwwroot a obsah* složky:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>`neodstraní *přeskočit* cíle z webu nasazení. `<Content>`cílové soubory a složky, se odstraní z webu nasazení. Předpokládejme například, že měl nasadíte webovou aplikaci s následující soubory:

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

Pokud jste přidali následující `<MsDeploySkipRules>` značek, těchto souborů nemohli odstranit na webu nasazení.

``` xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` Brání značek výše uvedeném *přeskočen* soubory nebudou depoyed, ale bude není odstranit ty soubory, jakmile jsou nasazeny.

Následující `<Content>` značek by odstranit cílové soubory v lokalitě nasazení:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Pomocí příkazového řádku nasazení s `<Content>` výše uvedený kód by způsobilo výstup podobný následujícímu:

``` console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="including-files"></a>Zahrnutí souborů

Následující kód slouží k patří *bitové kopie* složky mimo adresář projektu, který má *wwwroot nebo obrázky* složku publikování webu.

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Značky lze přidat do *.csproj* soubor nebo profil publikování. Pokud je přidán do *.csproj* souboru, bude součástí každý profil publikování v projektu.

Následující zvýrazněnou kód ukazuje, jak na:

* Kopírování souboru z mimo projekt do *wwwroot* složky.
* Vyloučit *wwwroot\Content* složky.
* Vyloučit *Views\Home\About2.cshtml*.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

Najdete v článku [WebSDK Readme](https://github.com/aspnet/websdk) pro další ukázky nasazení.

### <a name="run-a-target-before-or-after-publishing"></a>Spustit cíl před nebo po publikování

Builtin `BeforePublish` a `AfterPublish` cíle můžete použít ke spuštění cíl před nebo po cíl publikování. Následující kód můžete přidat do profilu publikování k protokolování zpráv bude výstup konzoly, před a po publikování:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Služby modulu Kudu

Chcete-li zobrazit soubory na webové aplikace Azure, použijte [kudu služby](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Připojení `scm` tokenu název nebo webové aplikace. Příklad:

| Adresa URL               | Výsledek|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Webové aplikace |
| `http://mysite.scm.azurewebsites.net/` | Adresářové kudu |

Vyberte [ladění konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console) položku nabídky Zobrazit nebo upravit nebo odstranit nebo přidat soubory.

## <a name="additional-resources"></a>Další zdroje

- [Nasazení webové](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) zjednodušuje nasazení webové aplikace a weby pro servery služby IIS.

- [https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): soubor problémy a žádosti o funkce pro nasazení.
