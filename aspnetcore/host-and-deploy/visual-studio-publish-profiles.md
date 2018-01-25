---
title: "Visual Studio publikační profily pro nasazení aplikace ASP.NET Core"
author: rick-anderson
description: "Zjistit, jak vytvořit publikační profily pro aplikace ASP.NET Core v sadě Visual Studio."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio publikační profily pro nasazení aplikace ASP.NET Core

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
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

`Sdk` Atribut `<Project>` elementu (v první řádek) výše uvedený kód provede následující akce:

* Importuje soubor vlastnosti z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* na začátku.
* Importuje soubor cíle z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* na konci.

Výchozím umístěním pro `MSBuildSDKsPath` (s Visual Studio 2017 Enterprise) je *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* složky.

`Microsoft.NET.Sdk.Web`závisí na:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Což způsobí, že následující vlastnosti a cílů určených k importu:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Publikujte cíle import právo sada cíle založené na metodě publikování použít.

Při načtení projektu nástroje MSBuild nebo Visual Studio jsou prováděny následující akce vysoké úrovně:

* Sestavení projektu
* Výpočetní soubory k publikování
* Publikovat soubory do cílového umístění

### <a name="compute-project-items"></a>Výpočetní položky projektu

Při načtení projektu, se vypočítávají položky projektu (soubory). `item type` Atribut určuje způsob zpracování souboru. Ve výchozím nastavení *.cs* souborů jsou součástí `Compile` seznamu položek. Soubory `Compile` kompilované seznamu položek.

`Content` Seznamu položek obsahuje soubory, které jsou publikovány kromě výstupy sestavení. Ve výchozím nastavení, soubory odpovídající vzoru `wwwroot/**` jsou součástí `Content` položky. [Wwwroot /\* \* je vzor expanze názvů](https://gruntjs.com/configuring-tasks#globbing-patterns) určující všechny soubory v *wwwroot* složky **a** podsložky. Pokud chcete explicitně přidat do seznamu publikovat soubor, přidejte přímo v souboru *.csproj* souboru, jak je znázorněno v [soubory včetně](#including-files).

Při výběru **publikovat** tlačítko v sadě Visual Studio nebo při publikování z příkazového řádku:

* Nebude vlastnosti se vypočítávají (soubory, které jsou potřebné k vytvoření).
* Visual Studio pouze: obnovení balíčků NuGet. (Obnovit, musí být explicitní uživatelem v rozhraní příkazového řádku).
* Sestavení projektu.
* Publikování položky se vypočítávají (soubory, které jsou potřebné k publikování).
* Po publikování projektu. (Počítaný soubory se zkopírují do cílového umístění publikovat).

Když projekt ASP.NET Core odkazuje `Microsoft.NET.Sdk.Web` v souboru projektu, *app_offline.htm* soubor je umístěn v kořenovém adresáři webové aplikace. Když se soubor nachází, modul ASP.NET Core řádně ukončí aplikaci a slouží *app_offline.htm* souboru během nasazení. Další informace najdete v tématu [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module#appofflinehtm).

## <a name="basic-command-line-publishing"></a>Základní publikování příkazového řádku

Příkazového řádku publikování nevyžaduje Visual Studio a funguje na všechny platformy .NET Core podporována. V níže, ukázky `dotnet publish` příkaz spuštěn z adresáře projektu (obsahující *.csproj* souboru). Pokud není ve složce projektu explicitně předat v cestě k souboru projektu. Příklad:

```console
dotnet publish c:/webs/web1
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

---

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

`dotnet publish` Příkaz volá MSBuild, které vyvolá `Publish` cíl. Žádné parametry předaný `dotnet publish` jsou předávány MSBuild. `-c` Parametr se mapuje `Configuration` vlastnosti MSBuild. `-o` Parametr mapuje `OutputPath`.

Vlastnosti nástroje MSBuild lze předat některou z následujících formátů:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Publikuje následující příkaz `Release` sestavení do sdílené síťové složky:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Sdílené síťové složce je zadán s lomítkem (*//r8/*) a funguje na všech platformách .NET Core podporována.

Potvrďte, že není spuštěn publikované aplikace pro nasazení. Soubory *publikování* složky jsou zamčené, když aplikace běží. Nasazení nemůže proběhnout, protože uzamčen, že soubory nelze kopírovat.

## <a name="publish-profiles"></a>Publikační profily

Tato část používá k vytvoření profilů publikování Visual Studio 2017 a vyšší. Po vytvoření publikování ze sady Visual Studio nebo pomocí příkazového řádku je k dispozici.

Publikování profily může zjednodušit proces publikování. Několik publikačních profilů může existovat. Pokud chcete vytvořit profil publikování v sadě Visual Studio, klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **publikovat**. Případně vyberte možnost **publikovat \<název projektu >** nabídce sestavení. **Publikovat** kapacity stránky aplikace se zobrazí. Pokud projekt neobsahuje profil publikování, zobrazí se následující stránka:

![Vybrat kartu publikovat aplikace kapacity stránky zobrazující Azure, služby IIS, FTB, složku s Azure. Také ukazuje vytvořit nový a vyberte Exiting přepínací tlačítka](visual-studio-publish-profiles/_static/az.png)

Při **složky** je vybrána, **publikovat** tlačítko vytvoří složku profil publikování a publikuje.

![** Karta publikovat ** aplikace kapacity stránky zobrazující Azure, služby IIS, FTB, složky](visual-studio-publish-profiles/_static/pub1.png)

Po vytvoření profilu publikování **publikovat** změny a vyberte **vytvořit nový profil** k vytvoření nového profilu.

![** Karta publikovat ** aplikace kapacity stránky zobrazující FolderProfile-vytvořili v posledním kroku a vytvořit nový profil odkaz](visual-studio-publish-profiles/_static/create_new.png)

Průvodce Publikovat podporuje následující cíle publikování:

* Microsoft Azure App Service
* Služba IIS, FTP atd., (pro všechny webové servery)
* Folder
* Importujte profil (umožňuje import profilu).
* Virtuální počítače Microsoft Azure

V tématu [jaké možnosti publikování je pro mě nejlepší?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) Další informace.

Při vytváření profilu publikování pomocí sady Visual Studio *vlastnosti/PublishProfiles/\<název publikovat > .pubxml* k vytvoření souboru MSBuild. To *.pubxml* soubor je soubor MSBuild a obsahuje konfigurační nastavení publikování. Tento soubor můžete změnit na přizpůsobení sestavení a publikování procesu. Proces publikování je pro čtení tohoto souboru. `<LastUsedBuildConfiguration>`je speciální, protože je globální vlastnost a by neměl být ve všech souborů, které je v sestavení naimportována. V tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Další informace.
*.Pubxml* souboru by neměla být zaškrtnuta do správy zdrojového kódu, protože závisí na *.uživatel* souboru. *.Uživatel* soubor by měl zkontrolovat nikdy do správy zdrojového kódu, protože ho mohou obsahovat citlivé údaje a jeho je platná pouze pro jednoho uživatele a počítače.

Se šifrují citlivých informací (jako je publikování heslo) na uživatele nebo počítač úrovně a uložené v *vlastnosti/PublishProfiles/\<název publikovat >. pubxml.user* souboru. Protože tento soubor může obsahovat citlivé informace, by měl **není** se změnami do správy zdrojového kódu.

Přehled o tom, jak publikovat webovou aplikaci na ASP.NET Core najdete [hostitele a nasazení](index.md). [Hostování a nasadit](index.md) je opensourcový projekt v https://github.com/aspnet/websdk.

`dotnet publish`můžete použít složku, MSDeploy, a [KUDU](https://github.com/projectkudu/kudu/wiki) publikační profily:
 
Složka (funguje napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

MSDeploy (aktuálně toto funguje pouze v systému windows, protože není MSDeploy napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

Balíček MSDeploy (aktuálně toto funguje pouze v systému windows, protože není MSDeploy napříč platformami):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

V předcházející ukázky **nemáte** předat `deployonbuild` k `dotnet publish`.

Další informace najdete v tématu [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

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

Spuštěním následujícího příkazu Zpráva prolétne si obsah publikovat a publikujete ho v Azure pomocí rozhraní API modulu KUDU:

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Při použití profil publikování, nastavte následující vlastnosti nástroje MSBuild:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Při publikování k profilu s názvem *FolderProfile*, můžete provést některou z níže uvedených příkazů:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Při vyvolání `dotnet build`, zavolá `msbuild` spustit sestavení a publikování procesu. Volání metody `dotnet build` nebo `msbuild` je v podstatě ekvivalentní při předávání ve složce profilu. Při volání metody MSBuild přímo v systému Windows, je použít rozhraní .NET Framework verze nástroje MSBuild. MSDeploy je aktuálně omezená na počítačích s Windows pro publikování. Volání metody `dotnet build` na do jiné složky profil vyvolá MSBuild a MSBuild používá MSDeploy v jiné složce profily. Volání metody `dotnet build` na jiný složky profilu vyvolá MSBuild (pomocí MSDeploy) a má za následek selhání (i když běžících na platformě Windows). Pokud chcete publikovat pomocí profilu pro jiné složky, přímo voláním funkce MSBuild.

Profil publikování se následující složce byl vytvořen pomocí sady Visual Studio a publikuje do sdílené síťové složky:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

Poznámka: `<LastUsedBuildConfiguration>` je nastaven na `Release`. Při publikování ze sady Visual Studio `<LastUsedBuildConfiguration>` je nastavena hodnota vlastnosti konfigurace, pomocí hodnoty, když se spustí proces publikování. `<LastUsedBuildConfiguration>` Vlastnosti konfigurace je speciální a by nemělo být přepsáno v importovaného souboru MSBuild. Tato vlastnost může být přepsána z příkazového řádku. Příklad:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Pomocí nástroje MSBuild:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publikovat do koncového bodu MSDeploy z příkazového řádku

Jak už jsme zmínili, publikování můžete udělat pomocí `dotnet publish` nebo `msbuild` příkaz. `dotnet publish`běží v kontextu .NET Core. `msbuild` Příkaz vyžaduje rozhraní .NET framework a proto je omezený na prostředí Windows.

Nejjednodušší způsob, jak publikovat s MSDeploy je nejdřív vytvořte profil publikování ve Visual Studio 2017 a použít profil z příkazového řádku.

V následující ukázce je vytvoření webové aplikace ASP.NET Core (pomocí `dotnet new mvc`), a přidají se profilem publikování Azure pomocí sady Visual Studio.

Spustit `msbuild` z **příkazový řádek vývojáře pro VS 2017**. Příkazový řádek vývojáře má správný *msbuild.exe* v cestě sadou některé nástroje MSBuild proměnné.

MSBuild používá následující syntaxe:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Získat `Password` z  *\<publikovat name >. PublishSettings* souboru. Stažení *. PublishSettings* soubor z buď:

* Průzkumník řešení: Klikněte pravým tlačítkem na webovou aplikaci a vyberte **stažení profilu publikování**.
* Azure Management Portal: Vyberte **profilu publikování Get** v okně webové aplikace.

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
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>`neodstraní *přeskočit* cíle z webu nasazení. `<Content>`cílové soubory a složky, se odstraní z webu nasazení. Předpokládejme například, že nasazené webové aplikace měla následující soubory:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Pokud následující `<MsDeploySkipRules>` těchto souborů nemohli odstranit na webu nasazení, přidání značka.

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

`<MsDeploySkipRules>` Brání značek výše uvedeném *přeskočen* soubory nebudou depoyed ale neodstraní těchto souborů se poté, co máte nasazené.

Následující `<Content>` značek odstraní cílové soubory v lokalitě nasazení:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Pomocí příkazového řádku nasazení s `<Content>` značek výše má za následek výstup podobný následujícímu:

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

Zahrnuje následující kód *bitové kopie* složky mimo adresář projektu, který má *wwwroot nebo obrázky* složku publikování webu:

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Značky lze přidat do *.csproj* soubor nebo profil publikování. Pokud je přidán do *.csproj* souboru, má součástí každý profil publikování v projektu.

Následující zvýrazněnou kód ukazuje, jak na:

* Kopírování souboru z mimo projekt do *wwwroot* složky.
* Vyloučit *wwwroot\Content* složky.
* Exclude *Views\Home\About2.cshtml*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Najdete v článku [WebSDK Readme](https://github.com/aspnet/websdk) pro další ukázky nasazení.

### <a name="run-a-target-before-or-after-publishing"></a>Spustit cíl před nebo po publikování

Integrované `BeforePublish` a `AfterPublish` cíle můžete použít ke spuštění cíl před nebo po cíl publikování. Následující kód můžete přidat do profilu publikování k protokolování zpráv bude výstup konzoly, před a po publikování:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Služby modulu Kudu

Chcete-li zobrazit soubory v Azure aplikace Service nasazení aplikace web, použijte [Kudu služby](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Připojení `scm` tokenu k názvu webové aplikace. Příklad:

| Adresa URL                                    | Výsledek      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | Web App     |
| `http://mysite.scm.azurewebsites.net/` | Adresářové kudu |

Vyberte [ladění konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console) položku nabídky Zobrazit nebo upravit nebo odstranit nebo přidat soubory.

## <a name="additional-resources"></a>Další zdroje

* [Nasazení webové](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) zjednodušuje nasazení webové aplikace a weby pro servery služby IIS.
* [https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): soubor problémy a žádosti o funkce pro nasazení.
