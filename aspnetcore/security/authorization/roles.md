---
title: Rollenbasierte Autorisierung in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie ASP.NET Core-Controller und Aktion Zugriff einschränken, indem Sie die Rollen an das Authorize-Attribut übergeben.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/roles
ms.openlocfilehash: 59753b90d3196b0bc16d4963f45b995f5108bc8b
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356674"
---
# <a name="role-based-authorization-in-aspnet-core"></a>Rollenbasierte Autorisierung in ASP.NET Core

<a name="security-authorization-role-based"></a>

Wenn eine Identität erstellt wird möglicherweise eine oder mehrere Rollen angehören. Tracy kann z. B. den Administrator- und Rollen gehören, obwohl Scott nur für die Benutzerrolle angehören kann. Wie diese Rollen erstellt und verwaltet werden, hängt von der Sicherungsspeicher der Autorisierung ab. Rollen werden verfügbar gemacht, für den Entwickler über die ["IsInRole"](/dotnet/api/system.security.principal.genericprincipal.isinrole) Methode für die ["ClaimsPrincipal"](/dotnet/api/system.security.claims.claimsprincipal) Klasse.

::: moniker range=">= aspnetcore-2.0"

> [!IMPORTANT]
> Die Informationen in diesem Artikel können **nicht** auf Razor-Seiten angewendet werden. Razor-Seiten unterstützt [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter) und [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter). Weitere Informationen finden Sie unter [Filtermethoden für Razor-Seiten](xref:razor-pages/filter).

::: moniker-end

## <a name="adding-role-checks"></a>Hinzufügen von rollenüberprüfungen

Rollenbasierte autorisierungsprüfungen sind deklarativ&mdash;Entwickler bettet diese in ihren Code für einen Controller oder einer Aktion innerhalb eines Controllers angeben von Rollen, die der aktuelle Benutzer ein Mitglied sein muss, um die angeforderte Ressource zuzugreifen.

Z. B. der folgende Code, schränkt den Zugriff auf alle Aktionen auf der `AdministrationController` für Benutzer sind, die ein Mitglied der `Administrator` Rolle:

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Sie können mehrere Rollen als durch Trennzeichen getrennte Liste angeben:

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Dieser Controller nur zugegriffen werden von Benutzern, die Mitglieder sind von der `HRManager` Rolle oder die `Finance` Rolle.

Wenn Sie mehrere Attribute anwenden, und klicken Sie dann ein Zugriff auf Benutzer ein Mitglied aller angegebenen Rollen sein muss; Das folgende Beispiel erfordert, dass ein Benutzer muss ein Mitglied sowohl der `PowerUser` und `ControlPanelUser` Rolle.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Sie können den Zugriff weiter einzuschränken, durch zusätzliche Rolle Authorization-Attribute auf der Aktionsebene anwenden:

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

In den vorherigen Code Snippet Membern der der `Administrator` Rolle oder die `PowerUser` Rolle kann auf den Controller zugreifen und die `SetTime` Aktion, aber nur Mitglieder der der `Administrator` Rolle zugreifen kann die `ShutDown` Aktion.

Sie können auch einen Controller Sperren jedoch anonyme, nicht authentifizierte Zugriff auf einzelne Aktionen zulassen.

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a>Überprüfung der richtlinienbasierten-Funktion

Anforderungen an die Rolle können auch mithilfe der neuen Richtlinie-Syntax wird registriert, in denen ein Entwickler eine Richtlinie beim Start als Teil der Dienstkonfiguration für die Autorisierung ausgedrückt werden. Dieser Fehler tritt normalerweise in `ConfigureServices()` in Ihre *"Startup.cs"* Datei.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

Richtlinien werden angewendet, mit der `Policy` Eigenschaft für die `AuthorizeAttribute` Attribut:

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Wenn Sie eine Anforderung mehrere zulässige Rollen angeben möchten, und klicken Sie dann als Parameter zum Angeben der `RequireRole` Methode:

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

In diesem Beispiel autorisiert Benutzer Zugehörigkeit zu der `Administrator`, `PowerUser` oder `BackupAdministrator` Rollen.
