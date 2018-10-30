---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/11/2012
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 458f9a6369fe97bab33d70bf31bd470b1b0e593c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836182"
---
<a name="routing-in-aspnet-web-api"></a>Routing in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.NET Web-API die HTTP-Anforderungen an den Controller weiterleitet.

> [!NOTE]
> Wenn Sie mit ASP.NET MVC vertraut sind, ist ein WebApi-Routing dem Web-API-MVC-Routing sehr ähnlich. Der Hauptunterschied besteht darin, dass die Web-API die HTTP-Methode, nicht den URI-Pfad verwendet, um die Aktion auszuwählen. Sie können auch das Routing im MVC-Stil in Web-API Projekten verwenden. In diesem Artikel werden keine Kenntnisse von ASP.NET MVC vorausgesetzt.


## <a name="routing-tables"></a>Routingtabellen

In einem ASP.NET Web-API Projekt ist ein *Controller* eine Klasse, die HTTP-Anforderungen verarbeitet. Die öffentlichen Methoden des Controllers werden *Aktionsmethoden* oder einfach *Aktionen* genannt. Wenn das Web-API-Framework eine Anforderung empfängt, leitet es diese zu einer Aktion weiter.

Um zu bestimmen, welche Aktion aufgerufen werden soll, verwendet das Framework eine *Routingtabelle*. Die Visual Studio-Projektvorlage für Web-APIs erstellt eine Standardroute:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Diese Route wird in der Datei "WebApiConfig.cs" definiert, die sich im Verzeichnis App_Start befindet:

![](routing-in-aspnet-web-api/_static/image1.png)

Weitere Informationen zur **WebApiConfig** Klasse, finden Sie unter [Konfigurieren von ASP.NET Web-API](../advanced/configuring-aspnet-web-api.md).

Falls Sie Web-API selbst hosten möchten, müssen Sie die Routingtabelle direkt auf das **HttpSelfHostConfiguration** Objekt festlegen. Weitere Informationen finden Sie unter [selbst hosten einer Web-API](../older-versions/self-host-a-web-api.md).

Jeder Eintrag in der Routingtabelle enthält eine *routenvorlage*. Die Routen-Standardvorlage für Web-API ist &quot;api / {Controller} / {Id}&quot;. In dieser Vorlage ist &quot;api&quot; ein ausgeschriebenes Pfadsegment, {Controller} und {Id} sind Platzhaltervariablen.

Wenn das Web-API-Framework eine HTTP-Anforderung empfängt, durchsucht es die URIs der Routenvorlagen in der Routingtabelle nach einer Übereinstimmung. Wenn keine Route passt, erhält der Client einen 404-Fehler. Die folgenden URIs entspricht z. B. der Standardroute:

- / api/contacts
- /API/Contacts/1
- /API/Products/gizmo1

Allerdings der folgende URI stimmt nicht überein, da es fehlen die &quot;api&quot; Segment:

- / Contacts/1

> [!NOTE]
> Der Grund für die Verwendung von "api" in der Route ist Konflikte mit ASP.NET MVC-Routings zu vermeiden. Auf diese Weise kann man &quot;/kontaktiert&quot; wechseln Sie zu einem MVC-Controller und &quot;/api/Contacts&quot; wechseln Sie zu einem Web-API-Controller. Wenn Sie diese Konvention nicht gefällt, können Sie natürlich die Standardroutingtabelle ändern.

Wenn eine übereinstimmende Route gefunden wurde, wählt Web-API-Controller und die Aktion:

- Um den Controller zu suchen, Web-API fügt &quot;Controller&quot; auf den Wert des der *{Controller}* Variable.
- Um die Aktion zu suchen, Web-API untersucht die HTTP-Methode, und sucht dann nach einer Aktion, deren Name mit diesem Namen der HTTP-Methode beginnt. Mit einer GET-Anforderung Web-API sieht für eine Aktion, die mit beginnt &quot;abrufen... &quot;, z. B. &quot;GetContact&quot; oder &quot;GetAllContacts&quot;. Diese Konvention gilt nur für abrufen, POST, PUT und DELETE-Methoden. Sie können andere HTTP-Methoden aktivieren, auf dem Controller mithilfe von Attributen. Wir sehen später ein Beispiel.
- Andere Platzhaltervariablen in der routenvorlage, z. B. *{Id},* Action-Parameter zugeordnet sind.

Sehen wir uns ein Beispiel. Nehmen wir an, dass Sie den folgenden Controller definieren:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Hier sind einige möglichen HTTP-Anforderungen zusammen mit der Aktion, die für die einzelnen aufgerufen wird:

| HTTP-Methode | URI-Pfad | Aktion | Parameter |
| --- | --- | --- | --- |
| GET | API/Produkte | GetAllProducts | *(keine)* |
| GET | API/Produkte/4 | GetProductById | 4 |
| DELETE | API/Produkte/4 | DeleteProduct | 4 |
| POST | API/Produkte | *(keine Übereinstimmung)* |  |

Beachten Sie, dass die *{Id}* -Segment des URI, falls vorhanden, zugeordnet ist die *Id* Parameter der Aktion. In diesem Beispiel ist der Controller definiert zwei GET-Methoden, mit einem *Id* Parameter und einen ohne Parameter.

Beachten Sie auch, die die POST-Anforderung fehl, da der Controller nicht definiert ist, eine &quot;Post... &quot; Methode.

## <a name="routing-variations"></a>Routing-Varianten

Im vorherigen Abschnitt beschrieben, die grundlegende Routingmechanismus von ASP.NET Web-API. Dieser Abschnitt beschreibt einige Varianten.

### <a name="http-methods"></a>HTTP-Methoden

Anstatt die Namenskonvention für HTTP-Methoden zu verwenden, können Sie explizit die HTTP-Methode für eine Aktion angeben, werden, indem die Aktionsmethode mit den **HttpGet**, **HttpPut**, **HttpPost** , oder **HttpDelete** Attribut.

Im folgenden Beispiel wird die Methode FindProduct GET-Anforderungen zugeordnet:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Um mehrere HTTP-Methoden für eine Aktion zu ermöglichen, oder zum Zulassen von HTTP-Methoden als GET, PUT, POST und DELETE, verwenden Sie die **AcceptVerbs** -Attribut, das eine Liste von HTTP-Methoden akzeptiert.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Routing durch den Namen der Aktion

Mit der Standard-routing-Vorlage verwendet die Web-API die HTTP-Methode zum Auswählen der Aktion. Allerdings können Sie auch eine Route erstellen, in dem der Name der Aktion im URI enthalten ist:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

In dieser routenvorlage die *{Action}* Parameternamen die Aktionsmethode im Controller. Verwenden Sie in diesem Format des Routings Attribute, um die zulässigen HTTP-Methoden geben. Nehmen wir beispielsweise an, dass Ihre Controller die folgende Methode verfügt:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

In diesem Fall würden eine GET-Anforderung für "api/Produkte/Details/1" die Details-Methode zugeordnet. Diese Art von routing ist vergleichbar mit ASP.NET MVC, und für eine RPC-Stil API eignet sich möglicherweise.

Sie können den Aktionsnamen überschreiben, indem die **ActionName** Attribut. Im folgenden Beispiel sind zwei Aktionen, die den zuordnen &quot;-api/Produkte/Miniaturansicht/*Id*. Eine GET unterstützt, und die andere POST unterstützt:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Nicht-Aktionen

Um zu verhindern, dass eine Methode abrufen als Aktion aufgerufen wird, verwenden Sie die **NonAction** Attribut. Dies signalisiert das Framework, dass die Methode keine Aktion, auch wenn sie andernfalls die Senderegeln übereinstimmen.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Weiterführende Themen

In diesem Thema bereitgestellten einen allgemeinen Überblick über die Weiterleitung an. Weitere Informationen finden Sie unter [Routing- und Aktionsauswahl](routing-and-action-selection.md), das beschreibt, genau wie das Framework einen URI zu einer Route übereinstimmt, wählt einen Controller und wählt dann die aufzurufende Aktion.
