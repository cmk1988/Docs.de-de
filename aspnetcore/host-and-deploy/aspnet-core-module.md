---
title: Konfigurationsreferenz für das ASP.NET Core-Modul
author: guardrex
description: Erfahren Sie, wie Sie das ASP.NET Core-Modul so konfigurieren, dass es ASP.NET Core-Apps hosten kann.
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: 0ae19b26bc86c9da7a61f3117aaae1844115593a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913280"
---
# <a name="aspnet-core-module-configuration-reference"></a>Konfigurationsreferenz für das ASP.NET Core-Modul

Von [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT) und [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Diese Dokumentation bietet Anweisungen dazu, wie Sie das ASP.NET Core-Modul so konfigurieren, dass es ASP.NET Core-Apps hosten kann. Eine Einführung in das ASP.NET Core-Modul und Installationsanweisungen finden Sie unter [ASP.NET Core Module overview (ASP.NET Core-Modulübersicht)](xref:fundamentals/servers/aspnet-core-module).

::: moniker range=">= aspnetcore-2.2"

## <a name="hosting-model"></a>Hostmodell

Für Apps, die in .NET Core 2.2 oder höher ausgeführt werden, unterstützt das Modul ein In-Process-Hostingmodell zur Verbesserung der Leistung, gemessen an Reverse-Proxy-Hosting (Out-of-Process). Weitere Informationen finden Sie unter <xref:fundamentals/servers/aspnet-core-module#aspnet-core-module-description>.

In-Process-Hosting ist eine wählbare Option für vorhandene Apps, die [Dotnet neu](/dotnet/core/tools/dotnet-new)-Vorlagen sehen das In-Process-Hostingmodell aber als Standard für alle IIS- und IIS Express-Szenarien vor.

Um eine App für In-Process-Hosting zu konfigurieren, fügen Sie der Projektdatei der App die Eigenschaft `<AspNetCoreModuleHostingModel>` mit dem Wert `inprocess` hinzu (Out-of-Process-Hosting wird mit `outofprocess` festgelegt):

```xml
<PropertyGroup>
  <AspNetCoreModuleHostingModel>inprocess</AspNetCoreModuleHostingModel>
</PropertyGroup>
```

Die folgenden Merkmale treffen für In-Process-Hosting zu:

* Der [Kestrel-Server](xref:fundamentals/servers/kestrel) wird nicht verwendet. Eine benutzerdefinierte Implementierung von <xref:Microsoft.AspNetCore.Hosting.Server.IServer>, `IISHttpServer`, wird als Server der App verwendet.

* Das [RequestTimeout-Attribut](#attributes-of-the-aspnetcore-element) trifft auf In-Process-Hosting nicht zu.

* Das gemeinsame Verwenden eines Anwendungspools durch mehrere Apps wird nicht unterstützt. Verwenden Sie einen Anwendungspool pro App.

* Wenn [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) verwendet oder eine [app_offline.htm-Datei manuell in der Bereitstellung platziert wird](xref:host-and-deploy/iis/index#locked-deployment-files), wird die App möglicherweise nicht sofort beendet, wenn eine offene Verbindung vorhanden ist. Beispielsweise kann eine Websocketverbindung eine Verzögerung beim Herunterfahren der App bewirken.

* Die Architektur (Bitbreite) der App und der installierten Runtime (x64 oder x86) müssen mit der Architektur des Anwendungspools übereinstimmen.

* Wenn der Host der App manuell mit `WebHostBuilder` eingerichtet wird (auf die Verwendung von [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) verzichtet wird) und die App jemals direkt auf dem Kestrel-Server ausgeführt wird (selbstgehostet), rufen Sie `UseKestrel` auf, bevor Sie `UseIISIntegration` aufrufen. Bei umgekehrter Reihenfolge tritt beim Starten des Hosts ein Fehler auf.

### <a name="hosting-model-changes"></a>Änderungen am Hostmodell

Wenn die Einstellung `hostingModel` in der Datei *web.config* geändert wird (im Abschnitt [Konfiguration mit web.config](#configuration-with-webconfig) erläutert), verwendet das Modul den Arbeitsprozess von IIS erneut.

Bei IIS Express verwendet das Modul den Arbeitsprozess nicht erneut, sondern löst stattdessen ein ordnungsgemäßes Herunterfahren des aktuellen IIS Express-Prozesses aus. Mit der nächsten Anforderung an die App wird ein neuer IIS Express-Prozess erzeugt.

### <a name="process-name"></a>Prozessname

`Process.GetCurrentProcess().ProcessName` meldet entweder `w3wp` (In-Process) oder `dotnet` (Out-of-Process).

::: moniker-end

## <a name="configuration-with-webconfig"></a>Konfiguration mit der Datei „web.config“

Das ASP.NET Core-Modul wurde mit dem `aspNetCore`-Abschnitt des `system.webServer`-Knotens in der Datei *web.config* der Site konfiguriert.

Die folgende *web.config*-Datei wird für eine [Framework-abhängige Bereitstellung](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) veröffentlicht und konfiguriert für da sASP.NET Core-Modul die Handhabung von Siteanforderungen:

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Die folgende *web.config*-Datei wird für eine [eigenständige Bereitstellung](/dotnet/articles/core/deploying/#self-contained-deployments-scd) veröffentlicht:

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" 
                hostingModel="inprocess" />
  </system.webServer>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

Wenn eine App für [Azure App Service](https://azure.microsoft.com/services/app-service/) bereitgestellt wird, wird der Pfad `stdoutLogFile` auf `\\?\%home%\LogFiles\stdout` gesetzt. Der Pfad speichert stdout-Protokolle zum Ordner *LogFiles*, einem Speicherort, der automatisch vom Dienst erstellt wird.

Einen wichtigen Hinweis zur Konfiguration von *web.config*-Dateien in untergeordneten Apps finden Sie unter [Konfiguration von untergeordneten Anwendungen](xref:host-and-deploy/iis/index#sub-application-configuration).

### <a name="attributes-of-the-aspnetcore-element"></a>Attribute des aspNetCore-Elements

::: moniker range=">= aspnetcore-2.2"

| Attribut | Beschreibung  | Standard |
| --------- | ----------- | :-----: |
| `arguments` | <p>Optionales Zeichenfolgeattribut.</p><p>Argumente zur ausführbaren Datei, die in **processPath** angegeben wurde.</p>| |
| `disableStartUpErrorPage` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, wird die Seite **502.5: Prozessfehler** unterdrückt, und die in der Datei *web.config* konfigurierte Seite für den Statuscode 502 hat Vorrang.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, wird das Token an den untergeordneten Prozess weitergeleitet, der an %ASPNETCORE_PORT% als Header 'MS-ASPNETCORE-WINAUTHTOKEN' pro Anforderung lauscht. Es liegt in der Verantwortung des entsprechenden Prozesses, CloseHandle auf dem Token pro Anforderung aufzurufen.</p> | `true` |
| `hostingModel` | <p>Optionales Zeichenfolgeattribut.</p><p>Gibt das Hostingmodell als In-Process (`inprocess`) oder Out-of-Process (`outofprocess`) an.</p> | `outofprocess` |
| `processesPerApplication` | <p>Optionales Ganzzahlattribut.</p><p>Gibt die Anzahl der Instanzen des Prozesses aus der Einstellung **processPath** an, die aus der App gestartet werden können.</p><p>&dagger;Für In-Process-Hosting ist dieser Wert auf `1` beschränkt.</p> | Standardwert: `1`<br>Min.: `1`<br>Max.: `100`&dagger; |
| `processPath` | <p>Erforderliches Zeichenfolgenattribut.</p><p>Pfad zur ausführbaren Datei, die einen Prozess startet, der auf HTTP-Anforderungen lauscht. Relative Pfade werden unterstützt. Wenn der Pfad mit `.` beginnt, gilt er als relativ zum Stammverzeichnis.</p> | |
| `rapidFailsPerMinute` | <p>Optionales Ganzzahlattribut.</p><p>Gibt an, wie viele Abstürze des in **ProcessPath** angegebenen Prozesses pro Minute zulässig sind. Wenn dieses Limit überschritten wird, beendet das Modul das Starten des Prozesses für den Rest der Minute.</p><p>Bei In-Process-Hosting nicht unterstützt.</p> | Standardwert: `10`<br>Min.: `0`<br>Max.: `100` |
| `requestTimeout` | <p>Optionales TimeSpan-Attribut.</p><p>Gibt an, wie lange das ASP.NET Core-Modul auf eine Antwort des Prozesses wartet, der auf „% ASPNETCORE_PORT“ lauscht.</p><p>In den Versionen des ASP.NET Core-Moduls, die mit Version ASP.NET Core 2.1 oder später ausgeliefert wurden, wird `requestTimeout` in Stunden, Minuten und Sekunden angegeben.</p><p>Trifft auf In-Process-Hosting nicht zu. Bei In-Process-Hosting wartet das Modul darauf, dass die App die Anforderung verarbeitet.</p> | Standardwert: `00:02:00`<br>Min.: `00:00:00`<br>Max.: `360:00:00` |
| `shutdownTimeLimit` | <p>Optionales Ganzzahlattribut.</p><p>Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei ordnungsgemäß beendet wird, wenn die Datei *app_offline.htm* erkannt wird.</p> | Standardwert: `10`<br>Min.: `0`<br>Max.: `600` |
| `startupTimeLimit` | <p>Optionales Ganzzahlattribut.</p><p>Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei einen Prozess startet, der an dem Port lauscht. Wenn dieses Zeitlimit überschritten wird, beendet das Modul den Prozess. Das Modul versucht, den Prozess neu zu starten, wenn es eine neue Anforderung erhält, und versucht weiterhin, den Prozess bei nachfolgenden eingehenden Anforderungen neu zu starten, es sei denn, die App kann **RapidFailsPerMinute**-Anzahl in der letzten fortlaufenden Minute nicht starten.</p><p>Der Wert 0 (null) wird **nicht** als unendliches Timeout angesehen.</p> | Standardwert: `120`<br>Min.: `0`<br>Max.: `3600` |
| `stdoutLogEnabled` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, werden **stdout** und **stderr** für den Prozess, der in **processPath** angegeben wurde, zu der Datei weitergeleitet, die in **stdoutLogFile** angegeben wurde.</p> | `false` |
| `stdoutLogFile` | <p>Optionales Zeichenfolgeattribut.</p><p>Gibt den relativen oder absoluten Pfad an, für den **stdout** und **stderr** aus dem in **ProcessPath** angegebenen Prozess protokolliert wurden. Relative Pfade sind relativ zum Stamm der Site. Jeder mit `.` beginnende Pfad ist zum Stammverzeichnis relativ, und alle anderen Pfade werden als absolute Pfade behandelt. Alle im Pfad angegebenen Ordner müssen vorhanden sein, damit das Modul die Protokolldatei erstellt. Mithilfe von Unterstrichtrennzeichen werden ein Zeitstempel, eine Prozess-ID und eine Dateierweiterung (*.log*) dem letzten Segment des Pfads **stdoutlogfile** hinzugefügt. Wenn `.\logs\stdout` als Wert angegeben wird, wird ein stdout-Beispielprotokoll als *stdout_20180205194132_1934.log* im Ordner *logs* gespeichert, sofern es am 2.5.2018 um 19:41:32 mit Prozess-ID 1934 gespeichert wurde.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Attribut | Beschreibung  | Standard |
| --------- | ----------- | :-----: |
| `arguments` | <p>Optionales Zeichenfolgeattribut.</p><p>Argumente zur ausführbaren Datei, die in **processPath** angegeben wurde.</p>| |
| `disableStartUpErrorPage` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, wird die Seite **502.5: Prozessfehler** unterdrückt, und die in der Datei *web.config* konfigurierte Seite für den Statuscode 502 hat Vorrang.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, wird das Token an den untergeordneten Prozess weitergeleitet, der an %ASPNETCORE_PORT% als Header 'MS-ASPNETCORE-WINAUTHTOKEN' pro Anforderung lauscht. Es liegt in der Verantwortung des entsprechenden Prozesses, CloseHandle auf dem Token pro Anforderung aufzurufen.</p> | `true` |
| `processesPerApplication` | <p>Optionales Ganzzahlattribut.</p><p>Gibt die Anzahl der Instanzen des Prozesses aus der Einstellung **processPath** an, die aus der App gestartet werden können.</p> | Standardwert: `1`<br>Min.: `1`<br>Max.: `100` |
| `processPath` | <p>Erforderliches Zeichenfolgenattribut.</p><p>Pfad zur ausführbaren Datei, die einen Prozess startet, der auf HTTP-Anforderungen lauscht. Relative Pfade werden unterstützt. Wenn der Pfad mit `.` beginnt, gilt er als relativ zum Stammverzeichnis.</p> | |
| `rapidFailsPerMinute` | <p>Optionales Ganzzahlattribut.</p><p>Gibt an, wie viele Abstürze des in **ProcessPath** angegebenen Prozesses pro Minute zulässig sind. Wenn dieses Limit überschritten wird, beendet das Modul das Starten des Prozesses für den Rest der Minute.</p> | Standardwert: `10`<br>Min.: `0`<br>Max.: `100` |
| `requestTimeout` | <p>Optionales TimeSpan-Attribut.</p><p>Gibt an, wie lange das ASP.NET Core-Modul auf eine Antwort des Prozesses wartet, der auf „% ASPNETCORE_PORT“ lauscht.</p><p>In den Versionen des ASP.NET Core-Moduls, die mit Version ASP.NET Core 2.1 oder später ausgeliefert wurden, wird `requestTimeout` in Stunden, Minuten und Sekunden angegeben.</p> | Standardwert: `00:02:00`<br>Min.: `00:00:00`<br>Max.: `360:00:00` |
| `shutdownTimeLimit` | <p>Optionales Ganzzahlattribut.</p><p>Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei ordnungsgemäß beendet wird, wenn die Datei *app_offline.htm* erkannt wird.</p> | Standardwert: `10`<br>Min.: `0`<br>Max.: `600` |
| `startupTimeLimit` | <p>Optionales Ganzzahlattribut.</p><p>Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei einen Prozess startet, der an dem Port lauscht. Wenn dieses Zeitlimit überschritten wird, beendet das Modul den Prozess. Das Modul versucht, den Prozess neu zu starten, wenn es eine neue Anforderung erhält, und versucht weiterhin, den Prozess bei nachfolgenden eingehenden Anforderungen neu zu starten, es sei denn, die App kann **RapidFailsPerMinute**-Anzahl in der letzten fortlaufenden Minute nicht starten.</p><p>Der Wert 0 (null) wird **nicht** als unendliches Timeout angesehen.</p> | Standardwert: `120`<br>Min.: `0`<br>Max.: `3600` |
| `stdoutLogEnabled` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, werden **stdout** und **stderr** für den Prozess, der in **processPath** angegeben wurde, zu der Datei weitergeleitet, die in **stdoutLogFile** angegeben wurde.</p> | `false` |
| `stdoutLogFile` | <p>Optionales Zeichenfolgeattribut.</p><p>Gibt den relativen oder absoluten Pfad an, für den **stdout** und **stderr** aus dem in **ProcessPath** angegebenen Prozess protokolliert wurden. Relative Pfade sind relativ zum Stamm der Site. Jeder mit `.` beginnende Pfad ist zum Stammverzeichnis relativ, und alle anderen Pfade werden als absolute Pfade behandelt. Alle im Pfad angegebenen Ordner müssen vorhanden sein, damit das Modul die Protokolldatei erstellt. Mithilfe von Unterstrichtrennzeichen werden ein Zeitstempel, eine Prozess-ID und eine Dateierweiterung (*.log*) dem letzten Segment des Pfads **stdoutlogfile** hinzugefügt. Wenn `.\logs\stdout` als Wert angegeben wird, wird ein stdout-Beispielprotokoll als *stdout_20180205194132_1934.log* im Ordner *logs* gespeichert, sofern es am 2.5.2018 um 19:41:32 mit Prozess-ID 1934 gespeichert wurde.</p> | `aspnetcore-stdout` |

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

| Attribut | Beschreibung  | Standard |
| --------- | ----------- | :-----: |
| `arguments` | <p>Optionales Zeichenfolgeattribut.</p><p>Argumente zur ausführbaren Datei, die in **processPath** angegeben wurde.</p>| |
| `disableStartUpErrorPage` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, wird die Seite **502.5: Prozessfehler** unterdrückt, und die in der Datei *web.config* konfigurierte Seite für den Statuscode 502 hat Vorrang.</p> | `false` |
| `forwardWindowsAuthToken` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, wird das Token an den untergeordneten Prozess weitergeleitet, der an %ASPNETCORE_PORT% als Header 'MS-ASPNETCORE-WINAUTHTOKEN' pro Anforderung lauscht. Es liegt in der Verantwortung des entsprechenden Prozesses, CloseHandle auf dem Token pro Anforderung aufzurufen.</p> | `true` |
| `processesPerApplication` | <p>Optionales Ganzzahlattribut.</p><p>Gibt die Anzahl der Instanzen des Prozesses aus der Einstellung **processPath** an, die aus der App gestartet werden können.</p> | Standardwert: `1`<br>Min.: `1`<br>Max.: `100` |
| `processPath` | <p>Erforderliches Zeichenfolgenattribut.</p><p>Pfad zur ausführbaren Datei, die einen Prozess startet, der auf HTTP-Anforderungen lauscht. Relative Pfade werden unterstützt. Wenn der Pfad mit `.` beginnt, gilt er als relativ zum Stammverzeichnis.</p> | |
| `rapidFailsPerMinute` | <p>Optionales Ganzzahlattribut.</p><p>Gibt an, wie viele Abstürze des in **ProcessPath** angegebenen Prozesses pro Minute zulässig sind. Wenn dieses Limit überschritten wird, beendet das Modul das Starten des Prozesses für den Rest der Minute.</p> | Standardwert: `10`<br>Min.: `0`<br>Max.: `100` |
| `requestTimeout` | <p>Optionales TimeSpan-Attribut.</p><p>Gibt an, wie lange das ASP.NET Core-Modul auf eine Antwort des Prozesses wartet, der auf „% ASPNETCORE_PORT“ lauscht.</p><p>In den Versionen des ASP.NET Core-Moduls, die im Lieferumfang der Version ASP.NET Core 2.0 oder früher enthalten waren, darf der `requestTimeout` nur in ganzen Minuten angegeben werden. Andernfalls wird standardmäßig der Wert 2 Minuten angenommen.</p> | Standardwert: `00:02:00`<br>Min.: `00:00:00`<br>Max.: `360:00:00` |
| `shutdownTimeLimit` | <p>Optionales Ganzzahlattribut.</p><p>Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei ordnungsgemäß beendet wird, wenn die Datei *app_offline.htm* erkannt wird.</p> | Standardwert: `10`<br>Min.: `0`<br>Max.: `600` |
| `startupTimeLimit` | <p>Optionales Ganzzahlattribut.</p><p>Gibt in Sekunden an, wie lange das Modul darauf wartet, dass die ausführbare Datei einen Prozess startet, der an dem Port lauscht. Wenn dieses Zeitlimit überschritten wird, beendet das Modul den Prozess. Das Modul versucht, den Prozess neu zu starten, wenn es eine neue Anforderung erhält, und versucht weiterhin, den Prozess bei nachfolgenden eingehenden Anforderungen neu zu starten, es sei denn, die App kann **RapidFailsPerMinute**-Anzahl in der letzten fortlaufenden Minute nicht starten.</p> | Standardwert: `120`<br>Min.: `0`<br>Max.: `3600` |
| `stdoutLogEnabled` | <p>Optionales boolesches Attribut.</p><p>Wenn TRUE, werden **stdout** und **stderr** für den Prozess, der in **processPath** angegeben wurde, zu der Datei weitergeleitet, die in **stdoutLogFile** angegeben wurde.</p> | `false` |
| `stdoutLogFile` | <p>Optionales Zeichenfolgeattribut.</p><p>Gibt den relativen oder absoluten Pfad an, für den **stdout** und **stderr** aus dem in **ProcessPath** angegebenen Prozess protokolliert wurden. Relative Pfade sind relativ zum Stamm der Site. Jeder mit `.` beginnende Pfad ist zum Stammverzeichnis relativ, und alle anderen Pfade werden als absolute Pfade behandelt. Alle im Pfad angegebenen Ordner müssen vorhanden sein, damit das Modul die Protokolldatei erstellt. Mithilfe von Unterstrichtrennzeichen werden ein Zeitstempel, eine Prozess-ID und eine Dateierweiterung (*.log*) dem letzten Segment des Pfads **stdoutlogfile** hinzugefügt. Wenn `.\logs\stdout` als Wert angegeben wird, wird ein stdout-Beispielprotokoll als *stdout_20180205194132_1934.log* im Ordner *logs* gespeichert, sofern es am 2.5.2018 um 19:41:32 mit Prozess-ID 1934 gespeichert wurde.</p> | `aspnetcore-stdout` |

::: moniker-end

### <a name="setting-environment-variables"></a>Festlegen von Umgebungsvariablen

Umgebungsvariablen können für den Prozess im Attribut `processPath` angegeben werden. Geben Sie eine Umgebungsvariable mit dem untergeordneten Element `environmentVariable` eines `environmentVariables`-Auflistungselements an. In diesem Abschnitt festgelegte Umgebungsvariablen haben Vorrang vor Systemumgebungsvariablen.

Im folgenden Beispiel werden zwei Umgebungsvariablen festgelegt. `ASPNETCORE_ENVIRONMENT` konfiguriert die Umgebung der App als `Development`. Ein Entwickler setzt diesen Wert möglicherweise vorübergehend in der Datei *web.config*, um das Laden der [Seite mit Ausnahmen für Entwickler](xref:fundamentals/error-handling) beim Debugging einer App-Ausnahme zu erzwingen. `CONFIG_DIR` ist ein Beispiel für eine benutzerdefinierte Umgebungsvariable, wobei der Entwickler Code geschrieben hat, der den Wert beim Start liest, um einen Pfad zum Laden der Konfigurationsdatei der App zu bilden.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

> [!WARNING]
> Legen Sie die Umgebungsvariable `ASPNETCORE_ENVIRONMENT` nur auf Staging- und Testservern auf `Development` fest, auf die nicht vertrauenswürdige Netzwerke, z.B. das Internet, nicht zugreifen können.

## <a name="appofflinehtm"></a>app_offline.htm

Wenn eine Datei mit dem Namen *app_offline.htm* im Stammverzeichnis einer App erkannt wird, versucht das ASP.NET Core-Modul, die App ordnungsgemäß zu beenden und die Verarbeitung eingehender Anforderungen zu stoppen. Wenn die App nach der Anzahl von Sekunden, die in `shutdownTimeLimit` definiert wurden, noch ausgeführt wird, beendet das ASP.NET Core-Modul den laufenden Prozess.

Wenn die Datei *app_offline.htm* vorhanden ist, reagiert das ASP.NET Core-Modul auf Anforderungen, indem es den Inhalt der *app_offline.htm*-Datei zurücksendet. Wenn die Datei *app_offline.htm* entfernt wurde, wird die App von der nächsten Anforderung gestartet.

::: moniker range=">= aspnetcore-2.2"

Beim Verwenden des Out-of-Process-Hostingmodells wird die App möglicherweise nicht sofort heruntergefahren, wenn eine offene Verbindung besteht. Beispielsweise kann eine Websocketverbindung eine Verzögerung beim Herunterfahren der App bewirken.

::: moniker-end

## <a name="start-up-error-page"></a>Startfehler-Seite

::: moniker range=">= aspnetcore-2.2"

*Gilt nur für Out-of-Process-Hosting.*

::: moniker-end

Wenn das ASP.NET Core-Modul den Back-Endprozess nicht starten kann oder der Back-Endprozess gestartet wird, aber nicht am konfigurierten Prozess lauscht, wird die Statuscodeseite *502.5: Prozessfehler* angezeigt. Um diese Seite zu unterdrücken und zur IIS-502-Sandardstatuscodeseite zurückzukehren, verwenden Sie das Attribut `disableStartUpErrorPage`. Weitere Informationen zum Konfigurieren von benutzerdefinierten Fehlermeldungen finden Sie unter [HTTP-Fehler `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).

![Statuscodeseite „502.5: Prozessfehler“](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a>Protokollerstellung und Weiterleitung

Das ASP.NET Core-Modul leitet die Konsolenausgabe „stdout“ und „stderr“ auf den Datenträger weiter, wenn die Attribute `stdoutLogEnabled` und `stdoutLogFile` des `aspNetCore`-Elements festgelegt werden. Alle im `stdoutLogFile`-Pfad angegebenen Ordner müssen vorhanden sein, damit das Modul die Protokolldatei erstellt. Der App-Pool muss über Schreibzugriff auf den Speicherort verfügen, an dem die Protokolle geschrieben werden (verwenden Sie `IIS AppPool\<app_pool_name>`, um die Schreibberechtigung bereitzustellen).

Protokolle werden nur dann rotiert, wenn die Prozesswiederverwendung/der Prozessneustart stattfindet. Der Hoster ist für die Begrenzung des Speicherplatzes zuständig, den die Protokolle nutzen.

Die Verwendung des „stdout“-Protokolls wird zur Problembehandlung bei Startproblemen der App empfohlen. Verwenden Sie das Protokoll „stdout“ nicht zu allgemeinen App-Protokollierungszwecken. Verwenden Sie für die routinemäßige Protokollierung in einer ASP.NET Core-App eine Protokollierungsbibliothek, die die Protokolldateigröße beschränkt und die Protokolle rotiert. Weitere Informationen finden Sie im Artikel zur [Protokollierung von Drittanbietern](xref:fundamentals/logging/index#third-party-logging-providers).

Ein Zeitstempel und eine Dateierweiterung werden automatisch hinzugefügt, wenn die Protokolldatei erstellt wird. Ein Protokolldateiname wird erstellt, indem der Zeitstempel, die Prozess-ID und die Dateierweiterung (*.log*) an das letzte Segment des `stdoutLogFile`-Pfads (in der Regel *stdout*), getrennt durch Unterstriche, angehängt werden. Wenn der `stdoutLogFile`-Pfad mit *stdout* endet, hat ein Protokoll für eine App mit der PID 1934, erstellt am 2.5.2018 um 19:42:32, den Dateinamen *stdout_20180205194132_1934.log*.

Das folgende `aspNetCore`-Beispielelement konfiguriert die stdout-Protokollierung für eine in Azure App Service gehostete App. Ein lokaler Pfad oder ein Netzwerkfreigabepfad ist für die lokale Protokollierung zulässig. Vergewissern Sie sich, dass die Identität des AppPool-Benutzers über die Berechtigung zum Schreiben in den angegebenen Pfad verfügt.

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

::: moniker-end

[Konfiguration mit der Datei „web.config“](#configuration-with-webconfig) enthält ein Beispiel für das `aspNetCore`-Element in der Datei *web.config*.

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a>Die Proxykonfiguration verwendet das HTTP-Protokoll und ein Paarbildungstoken

::: moniker range=">= aspnetcore-2.2"

*Gilt nur für Out-of-Process-Hosting.*

::: moniker-end

Der Proxy, der zwischen dem ASP.NET Core-Modul und Kestrel erstellt wurde, verwendet das HTTP-Protokoll. Die Verwendung von HTTP ist eine Leistungsoptimierung, bei der der Datenverkehr zwischen dem Modul und Kestrel in einer Loopbackadresse außerhalb der Netzwerkschnittstelle stattfindet. Es gibt kein Risiko für Lauschangriffe auf den Datenverkehr zwischen dem Modul und Kestrel von einem Speicherort außerhalb des Servers.

Ein Paarbildungstoken wird verwendet, um sicherzustellen, dass die von Kestrel empfangenen Anfragen von IIS über einen Proxy gesendet wurden und nicht von einer anderen Quelle stammen. Das Paarbildungstoken wurde durch das Modul erstellt und in einer Umgebungsvariablen (`ASPNETCORE_TOKEN`) festgelegt. Das Paarbildungstoken ist auch bei jeder Proxyanforderung in einem Header (`MSAspNetCoreToken`) festgelegt. IIS-Middleware überprüft jede erhaltene Anforderung, um sicherzustellen, dass der Headerwert des Paarbildungstokens dem Wert der Umgebungsvariablen entspricht. Wenn die Tokenwerte nicht übereinstimmen, wird die Anforderung protokolliert und abgelehnt. Es kann nicht von einem Speicherort außerhalb des Servers auf die Umgebungsvariablen des Paarbildungstokens und den Datenverkehr zwischen dem Modul und Kestrel zugegriffen werden. Wenn ein Angreifer den Wert des Paarbildungstokens nicht kennt, kann er keine Anforderungen einreichen, die die IIS-Middleware-Prüfung umgehen.

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a>ASP.NET Core-Modul mit einer IIS-Freigabekonfiguration

Das Installationsprogramm des ASP.NET Core-Moduls wird mit den Berechtigungen des **SYSTEM**-Kontos ausgeführt. Da das lokale Systemkonto keine Berechtigung zum Ändern für den von der IIS-Freigabekonfiguration verwendeten Freigabepfad hat, stößt der Installer beim Versuch, die Moduleinstellungen in *applicationHost.config* auf der Freigabe zu konfigurieren, auf einen „Zugriff verweigert“-Fehler. Wenn Sie eine IIS-Freigabekonfiguration verwenden, gehen Sie folgendermaßen vor:

1. Deaktivieren Sie die IIS-Freigabekonfiguration.
1. Führen Sie den Installer aus.
1. Exportieren Sie die aktualisierte Datei *applicationHost.config* auf die Freigabe.
1. Aktivieren Sie die IIS-Freigabekonfiguration erneut.

## <a name="module-version-and-hosting-bundle-installer-logs"></a>Version des Moduls und Installerprotokolle des Hostingpakets

So ermitteln Sie die Version des installierten ASP.NET Core-Moduls:

1. Navigieren Sie auf dem Hostsystem zu *%windir%\System32\inetsrv*.
1. Suchen Sie die Datei *aspnetcore.dll*.
1. Klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie im Dropdownmenü die Option **Eigenschaften** aus.
1. Wählen Sie die Registerkarte **Details** aus. Die **Dateiversion** und die **Produktversion** stellen die installierte Version des Moduls dar.

Die Installer-Protokolle des Hostingpakets für das Modul finden Sie unter *C:\\Benutzer\\%UserName%\\AppData\\Local\\Temp*. Die Datei heißt *dd_DotNetCoreWinSvrHosting__\<Zeitstempel>_000_AspNetCoreModule_x64.log*.

## <a name="module-schema-and-configuration-file-locations"></a>Dateispeicherorte für Modul, Schema und Konfiguration

### <a name="module"></a>Modul

**IIS (x86/amd64):**

   * %windir%\System32\inetsrv\aspnetcore.dll

   * %windir%\SysWOW64\inetsrv\aspnetcore.dll

**IIS Express (x86/amd64):**

   * %ProgramFiles%\IIS Express\aspnetcore.dll

   * %ProgramFiles(x86)%\IIS Express\aspnetcore.dll

### <a name="schema"></a>Schema

**IIS**

   * %windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml

**IIS Express**

   * %ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml

### <a name="configuration"></a>Konfiguration

**IIS**

   * %windir%\System32\inetsrv\config\applicationHost.config

**IIS Express**

   * .vs\config\applicationHost.config

Den Speicherort dieser Dateien finden Sie, indem Sie *aspnetcore.dll* in der Datei *applicationHost.config* suchen. Für IIS Express ist die Datei *applicationHost.config* jedoch nicht standardmäßig vorhanden. Die Datei wird unter  *\<application_root >\\VS\\config* beim Starten eines Web-App-Projekts in der Visual Studio-Projektmappe erstellt.
