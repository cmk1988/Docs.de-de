---
title: Erzwingen von HTTPS in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie HTTPS/TLS in einer ASP.NET Core-Web-app benötigen.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325600"
---
# <a name="enforce-https-in-aspnet-core"></a>Erzwingen von HTTPS in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Dokument zeigt, wie Sie:

* HTTPS für alle Anforderungen erforderlich.
* Alle HTTP-Anforderungen auf HTTPS umleiten.

Keine API kann verhindern, dass einen Client sensible Daten bei der ersten Anforderung senden.

> [!WARNING]
> Führen Sie **nicht** verwenden [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) für Web-APIs, die vertraulichen Informationen zu erhalten. `RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter. API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch. Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden. Web-APIs sollten daher entweder:
>
> * nicht auf HTTP lauschen oder
> * die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.

<a name="require"></a>

## <a name="require-https"></a>Anforderung von HTTPS

::: moniker range=">= aspnetcore-2.1"

Es empfiehlt sich alle Produktion ASP.NET Core Web-apps-Aufruf:

* Die HTTPS-Umleitung-Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) auf alle HTTP-Anfragen an HTTPS umzuleiten.
* [UseHsts](#hsts), HTTP Strict Transport Security-Protokoll (HSTS).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Der oben markierte Code:

* Verwendet die standardmäßige [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Verwendet die standardmäßige [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), es sei denn, durch Überschreiben der `ASPNETCORE_HTTPS_PORT` Umgebungsvariable oder [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Es wird empfohlen statt temporäre umleitungen dauerhafte umleitungen, wie die Zwischenspeicherung von Ressourcenlinks instabil Verhalten in entwicklungsumgebungen führen kann. Es wird empfohlen, [HSTS](#hsts) , um Clients zu signalisieren, die Ressource nur sichere Anforderungen an die app (nur in der Produktion) gesendet werden sollen.

> [!WARNING]
> Ein Port muss für die Middleware verfügbar sein, an HTTPS umgeleitet wird. Wenn kein Port verfügbar ist, tritt kein Umleitung auf HTTPS. Der HTTPS-Port kann mit einer der folgenden Methoden angegeben werden:
>
> * Legen Sie `HttpsRedirectionOptions.HttpsPort`.
> * Festlegen der Umgebungsvariable `ASPNETCORE_HTTPS_PORT`.
> * Legen Sie bei der Entwicklung eine HTTPS-URL in *"launchsettings.JSON"*.
> * Konfigurieren Sie einen HTTPS-URL-Endpunkt für [Kestrel](xref:fundamentals/servers/kestrel) oder [HTTP.sys](xref:fundamentals/servers/httpsys).
>
> Wenn Kestrel oder HTTP.sys als öffentlich zugängliche Edgeserver verwendet wird, muss Kestrel oder HTTP.sys zum Lauschen an beide konfiguriert werden:
>
> * Die, in dem der Client umgeleitet wird, sicherer Port (in der Regel Port 443 in der Produktion und 5001 in der Entwicklung).
> * Der unsichere Port (üblicherweise 80 in der Produktion) und 5000 in der Entwicklung.
>
> Der unsichere Port muss vom Client in der Reihenfolge für die app eine unsichere Anforderung erhalten, und leiten Sie ihn mit dem sicheren Port zugegriffen werden.
>
> Jeder Firewall zwischen dem Client und Server müssen auch die Ports für den Datenverkehr zu öffnen.
>
> Weitere Informationen finden Sie unter [Kestrel-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) oder <xref:fundamentals/servers/httpsys>.

Die folgenden hervorgehobenen Code ruft [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) middlewareoptionen konfigurieren:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Aufrufen von `AddHttpsRedirection` ist nur erforderlich, ändern Sie die Werte der `HttpsPort` oder `RedirectStatusCode`.

Der oben markierte Code:

* Legt [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) zu [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), dies ist der Standardwert. Verwenden Sie die Felder von der [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) -Klasse für Zuweisungen zu `RedirectStatusCode`.
* Legt den HTTPS-Port in 5001 fest. Der Standardwert ist 443.

Die folgenden Mechanismen legen Sie den Port automatisch:

* Die Middleware kann ermitteln, die Ports über [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) wenn Folgendes zutrifft:

  * Kestrel oder HTTP.sys direkt mit HTTPS-Endpunkte verwendet wird (gilt auch für die app mit Visual Studio Code-Debugger ausgeführt wird).
  * Nur **eine HTTPS-Port** wird von der app verwendet.

* Visual Studio wird verwendet:

  * IIS Express ist HTTPS-tauglich.
  * *"launchsettings.JSON"* legt die `sslPort` für IIS Express.

> [!NOTE]
> Wenn eine app ausgeführt wird, hinter einem Reverseproxy (z. B. IIS, IIS Express) `IServerAddressesFeature` ist nicht verfügbar. Der Port muss manuell konfiguriert werden. Wenn der Port nicht festgelegt ist, werden nicht die Anforderungen umgeleitet.

Der Port kann konfiguriert werden, durch Festlegen der [Https_port Webhost-Konfigurationseinstellung](xref:fundamentals/host/web-host#https-port):

**Schlüssel**: Https_port  
**Typ:** *Zeichenfolge*  
**Standardmäßige**: ein Standardwert ist nicht festgelegt.  
**Festlegen mit:** `UseSetting`  
**Umgebungsvariable**: `<PREFIX_>HTTPS_PORT` (das Präfix ist `ASPNETCORE_` bei Verwendung der Web-Host.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> Der Port kann indirekt konfiguriert werden, durch Festlegen der URL durch die `ASPNETCORE_URLS` -Umgebungsvariablen angegeben. Die Umgebungsvariable konfiguriert den Server, und klicken Sie dann die Middleware indirekt über den HTTPS-Port ermittelt `IServerAddressesFeature`.

Wenn kein Port festgelegt ist:

* Anforderungen werden nicht umgeleitet.
* Die Middleware protokolliert die Warnung "Fehler bei den Https-Port für die Umleitung zu bestimmen."

> [!NOTE]
> Eine Alternative zur Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) ist die Verwendung von URL-Umschreibenden Middleware (`AddRedirectToHttps`). `AddRedirectToHttps` können den Statuscode und den Port auch festlegen, wenn die Umleitung ausgeführt wird. Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).
>
> Ohne die Notwendigkeit von zusätzlichen umleitungs-Regeln an HTTPS umleiten möchten, empfehlen wir Ihnen unter Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) in diesem Thema beschrieben.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

Die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um die HTTPS erforderlich ist. `[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können. Um das Attribut global anzuwenden, fügen Sie den folgenden Code zur `ConfigureServices` in `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Der oben markierte Code ist erforderlich, alle Anforderungen verwenden `HTTPS`daher HTTP-Anforderungen werden ignoriert. Der folgende hervorgehobene Code leitet alle HTTP-Anfragen an HTTPS um:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting). Die Middleware kann außerdem die app, die den Statuscode oder den Statuscode sowie den Port festgelegt, wenn die Umleitung ausgeführt wird.

Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode. Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer. denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP Strict Transport Security-Protokoll (HSTS)

Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine optionale sicherheitserweiterung, die von einer Web-app durch die Verwendung eines Antwortheaders angegeben wird. Wenn eine [Browser mit Unterstützung von HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) diesen Header empfängt:

* Der Browser speichert die Konfiguration für die Domäne, die verhindert, dass eine Kommunikation über HTTP senden. Der Browser wird die gesamte Kommunikation über HTTPS erzwungen.
* Der Browser wird verhindert, dass der Benutzer mithilfe von Zertifikaten für nicht vertrauenswürdig oder ungültig. Der Browser deaktiviert eingabeaufforderungen, die einen solches Zertifikat vorübergehend vertrauen Benutzer zu ermöglichen.

Da HSTS vom Client erzwungen wird, hat es einige Einschränkungen:

* Der Client muss HSTS unterstützen.
* HSTS muss mindestens eine erfolgreiche HTTPS-Anforderung zu, um die Richtlinie HSTS herzustellen.
* Die Anwendung muss jede HTTP-Anforderung überprüfen und umleiten oder die HTTP-Anforderung ablehnen.

ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode. Der folgende code ruft `UseHsts` bei der app nicht im [Entwicklungsmodus](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` ist nicht in der Entwicklung empfohlen, da die Einstellungen HSTS hohem Maße zwischenspeicherbar sind von Browsern. In der Standardeinstellung `UseHsts` schließt die lokalen Loopback-Adresse.

Für produktionsumgebungen HTTPS zum ersten Mal implementieren wird den Anfangswert von HSTS auf einen kleinen Wert fest. Legen Sie den Wert von Stunden keine mehr als ein Tag für den Fall, dass Sie die HTTP-HTTPS-Infrastruktur wiederherstellen müssen. Nachdem Sie sich die Nachhaltigkeit der HTTPS-Konfiguration sicher sind, erhöhen Sie den HSTS-Max-Age-Wert; häufig verwendeter Wert ist ein Jahr. 

Der folgende Code

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Legt den preload Parameter des Strict-Transport-Security-Headers. Preload ist nicht Teil der [RFC HSTS Spezifikation](https://tools.ietf.org/html/rfc6797), aber vom Webbrowser zum Vorabladen von HSTS Websites Neuinstallation unterstützt wird. Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).
* Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen der Host die HSTS-Richtlinie gilt.
* Explizit festlegt den Max-Age-Parameter, der den Strict-Transport-Security-Header auf 60 Tage. Wenn nicht festgelegt, der Standardwert ist 30 Tage. Finden Sie unter den [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.
* Fügt `example.com` zur Liste der Hosts zu schließen.

`UseHsts` Schließt die folgenden Loopback-Hosts an:

* `localhost` : Die IPv4-Loopback-Adresse.
* `127.0.0.1` : Die IPv4-Loopback-Adresse.
* `[::1]` : Die IPv6-Loopback-Adresse.

Im vorherige Beispiel veranschaulicht das weitere Hosts hinzufügen möchten.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Deaktivieren von HTTPS/HSTS bei projekterstellung

In einigen Back-End-Dienst-Szenarien, in denen verbindungssicherheit am Rand des Netzwerks öffentlich zugänglichen behandelt wird, ist nicht Konfigurieren der Sicherheit der Serververbindung über jeden Knoten erforderlich. Web-apps, die aus den Vorlagen in Visual Studio oder über generiert die [Dotnet neue](/dotnet/core/tools/dotnet-new) Befehls aktivieren [HTTPS-Umleitung](#require) und [HSTS](#hsts). Für Bereitstellungen, die diese Szenarien erfordern nicht, Sie können HTTPS/HSTS deaktivieren, wenn die app aus der Vorlage erstellt wird.

Zum Deaktivieren von HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli) 

Verwenden Sie die `--no-https`-Option. Beispiel:

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>So richten Sie ein entwicklerzertifikat für Docker

Finden Sie unter [GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Zusätzliche Informationen

* [OWASP HSTS-Browserunterstützung](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
