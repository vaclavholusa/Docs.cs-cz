# <a name="aspnet-core-authorization-sample"></a>Ukázka autorizace ASP.NET Core

Tento příklad ukazuje použití stránky Razor autorizace konvencemi. Tento příklad znázorňuje funkce popsané v [stránky Razor autorizace konvence](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) tématu.

Autorizace uživatelů v této ukázce používá funkce popsané v souboru cookie ověřování [souboru cookie ověřování bez ASP.NET Core Identity](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) tématu. Informace o používání ASP.NET Core Identity, najdete v tématech v [ověřování](https://docs.microsoft.com/aspnet/core/security/authentication/index) části dokumentace.

Při spuštění ukázky, použijte e-mailovou adresu **maria.rodriguez@contoso.com** k ověření uživatele.

## <a name="examples-in-this-sample"></a>Příklady v této ukázce

| Funkce | Popis |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Přidá [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na stránku se zadanou cestou. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Přidá [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na všechny stránky ve složce se zadanou cestou. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Přidá [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na stránku se zadanou cestou. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Přidá [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na všechny stránky ve složce se zadanou cestou. |
