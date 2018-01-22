---
title: "Verhindern von siteübergreifendem Skripting"
author: rick-anderson
description: Dieses Dokument stellt Cross-Site-Skripting (XSS) und Techniken zur Behebung dieses Sicherheitsrisiko in ASP.NET Core-app.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: af73a86aa6bcde084ecbe1a3fb5711c7da55871c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="370b0-103">Verhindern von siteübergreifendem Skripting</span><span class="sxs-lookup"><span data-stu-id="370b0-103">Preventing Cross-Site Scripting</span></span>

<span data-ttu-id="370b0-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="370b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="370b0-105">Cross-Site-Skripting (XSS) ist ein Sicherheitsrisiko Dadurch kann ein Angreifer clientseitige Skripts (normalerweise "JavaScript") in Webseiten zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="370b0-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="370b0-106">Wenn andere Benutzer betroffene Seiten geladen werden, die Angreifer Skripts ausgeführt werden, Aktivieren der Angreifer Cookies und Sitzungstoken zu stehlen ändern Sie den Inhalt der Webseite über DOM-Manipulation oder den Browser zu einer anderen Seite umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="370b0-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="370b0-107">XSS-Sicherheitsrisiken auftreten in der Regel auf, wenn eine Anwendung eine Benutzereingabe akzeptiert und ihn auf einer Seite gibt ohne überprüfen, Codierung, oder er Escapezeichen.</span><span class="sxs-lookup"><span data-stu-id="370b0-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="370b0-108">Schützen Ihre Anwendung mit XSS</span><span class="sxs-lookup"><span data-stu-id="370b0-108">Protecting your application against XSS</span></span>

<span data-ttu-id="370b0-109">AT einer grundlegenden Ebene XSS funktioniert, indem Sie dazu die Anwendung in das Einfügen von einem `<script>` Tag in der gerenderten Seite oder durch Einfügen einer `On*` Ereignis in ein Element.</span><span class="sxs-lookup"><span data-stu-id="370b0-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="370b0-110">Entwickler sollten die folgenden Schritte für die Verhinderung von, vermeiden Sie unnötigen XSS in ihrer Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="370b0-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="370b0-111">Nicht vertrauenswürdige Daten abgelegt nie die HTML-Eingaben, es sei denn, Sie führen Sie die restlichen Schritte ausführen.</span><span class="sxs-lookup"><span data-stu-id="370b0-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="370b0-112">Nicht vertrauenswürdige Daten ist alle Daten, die von einem Angreifer, HTML-Formulareingaben, Abfragezeichenfolgen, HTTP-Header, selbst Daten stammen aus einer Datenbank, da ein Angreifer möglicherweise auf Ihre Datenbank zu verletzen, auch wenn sie Ihre Anwendung verletzen können nicht gesteuert werden können.</span><span class="sxs-lookup"><span data-stu-id="370b0-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="370b0-113">Stellen Sie sicher, dass es sich um HTML-codiert ist, vor dem Platzieren von nicht vertrauenswürdiger Daten innerhalb eines HTML-Elements.</span><span class="sxs-lookup"><span data-stu-id="370b0-113">Before putting untrusted data inside an HTML element ensure it is HTML encoded.</span></span> <span data-ttu-id="370b0-114">HTML-Codierung Zeichen wie z. B. akzeptiert &lt; und ändert diese in eine sichere Form wie &amp;Lt;</span><span class="sxs-lookup"><span data-stu-id="370b0-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="370b0-115">Stellen Sie sicher, dass es sich um HTML-Attributs codiert ist, vor dem Einfügen von nicht vertrauenswürdiger Daten in einem HTML-Attribut.</span><span class="sxs-lookup"><span data-stu-id="370b0-115">Before putting untrusted data into an HTML attribute ensure it is HTML attribute encoded.</span></span> <span data-ttu-id="370b0-116">HTML-attributcodierung eine Obermenge der HTML-Codierung codiert und zusätzliche Zeichen wie z. B. "und".</span><span class="sxs-lookup"><span data-stu-id="370b0-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="370b0-117">Platzieren Sie die Daten vor dem Einfügen von nicht vertrauenswürdiger Daten in JavaScript in einem HTML-Element, dessen Inhalt Sie zur Laufzeit abrufen.</span><span class="sxs-lookup"><span data-stu-id="370b0-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="370b0-118">Wenn dies ist nicht möglich, stellen Sie sicher Daten wird JavaScript codiert.</span><span class="sxs-lookup"><span data-stu-id="370b0-118">If this is not possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="370b0-119">JavaScript-Codierung nimmt gefährliche Zeichen für JavaScript und z. B. durch ihre Hex ersetzt &lt; würde codiert werden, als `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="370b0-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="370b0-120">Stellen Sie sicher, dass sie die URL-codiert ist, vor dem Einfügen von nicht vertrauenswürdiger Daten in einer URL-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="370b0-120">Before putting untrusted data into a URL query string ensure it is URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="370b0-121">HTML-Codierung mit Razor</span><span class="sxs-lookup"><span data-stu-id="370b0-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="370b0-122">Das Razor-Modul automatisch in MVC verwendet codiert alle Ausgabe auf Variablen, stammen, es sei denn, Sie tatsächlich hart arbeiten, um zu verhindern, dass es auf diese Weise.</span><span class="sxs-lookup"><span data-stu-id="370b0-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="370b0-123">Er verwendet die HTML-Attributs Codierungsregeln jedes Mal, wenn Sie die  *@*  Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="370b0-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="370b0-124">Als HTML ist das attributcodierung eine Obermenge der HTML-Codierung Dies bedeutet, dass Sie keine betreffen, selbst mit, ob Sie HTML-Codierung oder HTML-attributcodierung verwenden soll.</span><span class="sxs-lookup"><span data-stu-id="370b0-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="370b0-125">Achten Sie darauf, nur @ in einem HTML-Kontext verwenden nicht, wenn nicht vertrauenswürdige Eingaben direkt in JavaScript einfügen möchten.</span><span class="sxs-lookup"><span data-stu-id="370b0-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="370b0-126">Tag-Hilfsmethoden werden auch Eingabe codieren, die in der Tag-Parameter verwendet.</span><span class="sxs-lookup"><span data-stu-id="370b0-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="370b0-127">Ausführen der folgenden Razor-Ansicht an.</span><span class="sxs-lookup"><span data-stu-id="370b0-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="370b0-128">Diese Sicht gibt den Inhalt der *UntrustedInput* Variable.</span><span class="sxs-lookup"><span data-stu-id="370b0-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="370b0-129">Diese Variable enthält einige Zeichen, die im XSS-Angriffen, d. h. verwendet werden &lt;, "und &gt;.</span><span class="sxs-lookup"><span data-stu-id="370b0-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="370b0-130">Untersuchen die Quelle zeigt die gerenderte Ausgabe als codiert:</span><span class="sxs-lookup"><span data-stu-id="370b0-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="370b0-131">ASP.NET Core MVC bietet eine `HtmlString` Klasse, die nicht automatisch nach Ausgabe codiert ist.</span><span class="sxs-lookup"><span data-stu-id="370b0-131">ASP.NET Core MVC provides an `HtmlString` class which is not automatically encoded upon output.</span></span> <span data-ttu-id="370b0-132">Dies sollte nie in Kombination mit nicht vertrauenswürdigen Eingabe verwendet werden, da dies ein Sicherheitsrisiko XSS verfügbar machen soll.</span><span class="sxs-lookup"><span data-stu-id="370b0-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="370b0-133">JavaScript-Codierung mithilfe von Razor</span><span class="sxs-lookup"><span data-stu-id="370b0-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="370b0-134">Möglicherweise gibt es Zeiten, die zum Einfügen eines Werts in JavaScript, die in der Ansicht verarbeitet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="370b0-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="370b0-135">Hierfür gibt es zwei Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="370b0-135">There are two ways to do this.</span></span> <span data-ttu-id="370b0-136">Die sicherste Möglichkeit, einfache Werte einfügen ist, platzieren den Wert in einem Datenattribut eines Tags und in Ihrer JavaScript-abgerufen.</span><span class="sxs-lookup"><span data-stu-id="370b0-136">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="370b0-137">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="370b0-137">For example:</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="370b0-138">Dies erzeugt die folgenden HTML-Code</span><span class="sxs-lookup"><span data-stu-id="370b0-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="370b0-139">Die, die Folgendes ausgeführt wird, rendert,</span><span class="sxs-lookup"><span data-stu-id="370b0-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="370b0-140">Sie können den JavaScript-Encoder auch direkt aufrufen,</span><span class="sxs-lookup"><span data-stu-id="370b0-140">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="370b0-141">Dies wird wie folgt im Browser gerendert;</span><span class="sxs-lookup"><span data-stu-id="370b0-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="370b0-142">Verketten Sie keine nicht vertrauenswürdige Eingaben in JavaScript in der DOM-Elemente zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="370b0-142">Do not concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="370b0-143">Verwenden Sie `createElement()` und Zuweisen von Eigenschaftswerten entsprechend z. B. `node.TextContent=`, oder verwenden Sie `element.SetAttribute()` / `element[attribute]=` andernfalls Sie setzen DOM-basierte XSS.</span><span class="sxs-lookup"><span data-stu-id="370b0-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="370b0-144">Beim Zugriff auf den Encoder in code</span><span class="sxs-lookup"><span data-stu-id="370b0-144">Accessing encoders in code</span></span>

<span data-ttu-id="370b0-145">Die HTML, JavaScript und URL-Encoder für Ihren Code auf zwei Arten verfügbar sind, können Sie sie über einfügen [Abhängigkeitsinjektion](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) oder Sie können die Standard-Encoder in enthalten die `System.Text.Encodings.Web` Namespace.</span><span class="sxs-lookup"><span data-stu-id="370b0-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="370b0-146">Wenn Sie die Standard-Encoder verwenden, und klicken Sie dann alle auf Zeichenbereiche angewendet um als sicher betrachtet werden nicht wirksam: die Standard-Encoder verwenden die sichersten mögliche Codierungsregeln.</span><span class="sxs-lookup"><span data-stu-id="370b0-146">If you use the default encoders then any  you applied to character ranges to be treated as safe will not take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="370b0-147">Die konfigurierbaren Encoder über DI verwenden Ihre Konstruktoren sollte ein *HtmlEncoder*, *JavaScriptEncoder* und *UrlEncoder* Parameter nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="370b0-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="370b0-148">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="370b0-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="370b0-149">Codierung von URL-Parameter</span><span class="sxs-lookup"><span data-stu-id="370b0-149">Encoding URL Parameters</span></span>

<span data-ttu-id="370b0-150">Wenn Sie eine URL-Abfragezeichenfolge mit nicht vertrauenswürdigen Eingabe als Wert Verwendung erstellen möchten die `UrlEncoder` um den Wert zu codieren.</span><span class="sxs-lookup"><span data-stu-id="370b0-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="370b0-151">Ein auf ein Objekt angewendeter</span><span class="sxs-lookup"><span data-stu-id="370b0-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="370b0-152">Nach der Codierung der Codierterwert enthält Variable `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="370b0-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="370b0-153">Leerzeichen, Anführungszeichen, Satzzeichen und andere unsicheren Zeichen in Prozent in ihren Hexadezimalwert codiert, z. B. wird ein Leerzeichen %20 sein.</span><span class="sxs-lookup"><span data-stu-id="370b0-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="370b0-154">Verwenden Sie nicht vertrauenswürdige Eingabe nicht als Teil eines URL-Pfads.</span><span class="sxs-lookup"><span data-stu-id="370b0-154">Do not use untrusted input as part of a URL path.</span></span> <span data-ttu-id="370b0-155">Übergeben Sie nicht vertrauenswürdige Eingaben immer als Wert einer Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="370b0-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="370b0-156">Anpassen der Encoder</span><span class="sxs-lookup"><span data-stu-id="370b0-156">Customizing the Encoders</span></span>

<span data-ttu-id="370b0-157">Standardmäßig Encoder verwenden eine Sicherungsliste begrenzt auf die grundlegenden lateinischen Unicode-Bereich, und Codieren aller Zeichen außerhalb dieses Bereichs als ihre entsprechenden Code.</span><span class="sxs-lookup"><span data-stu-id="370b0-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="370b0-158">Dieses Verhalten wirkt sich auch auf die Razor taghelpers und HtmlHelper Rendering aus, wie die Encoder zur Ausgabe von Zeichenfolgen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="370b0-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="370b0-159">Die Überlegung hinter dieser wird zum Schutz von unbekannten oder zukünftige Browser Fehler (vorherige Browser Fehler haben das Einrichten der Analyse basierend auf der Verarbeitung von nicht-englische Zeichen aktiviert).</span><span class="sxs-lookup"><span data-stu-id="370b0-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="370b0-160">Wenn Ihre Website starke Nutzung von nicht-lateinische Zeichen, z. B. Chinesisch, macht ist Kyrillisch oder andere Personen dies wahrscheinlich nicht das gewünschte Verhalten.</span><span class="sxs-lookup"><span data-stu-id="370b0-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="370b0-161">Sie können anpassen, die Listen der Encoder sichere Einbeziehung Unicode, Bereiche, die für Ihre Anwendung während des Starts geeignet ist, in `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="370b0-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="370b0-162">Z. B. mit der Konfiguration können Sie eine Razor-HtmlHelper wie;</span><span class="sxs-lookup"><span data-stu-id="370b0-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="370b0-163">Wenn Sie die Quelle der Webseite anzeigen, sehen Sie, dass er wie folgt mit der chinesischem Text codiert gerendert wurde;</span><span class="sxs-lookup"><span data-stu-id="370b0-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="370b0-164">Erweitert werden, die Zeichen behandelt als sicher vom Encoder Sie würde fügen Sie die folgende Zeile in der `ConfigureServices()` Methode in `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="370b0-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="370b0-165">In diesem Beispiel erweitert die Sicherungsliste, um die Unicode-Bereich CjkUnifiedIdeographs einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="370b0-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="370b0-166">Die gerenderte Ausgabe werden jetzt</span><span class="sxs-lookup"><span data-stu-id="370b0-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="370b0-167">Liste sicherer Bereiche werden als Unicode-codeübersichten, nicht die Sprachen angegeben.</span><span class="sxs-lookup"><span data-stu-id="370b0-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="370b0-168">Die [Unicode-Standard](http://unicode.org/) enthält eine Liste der [code Diagramme](http://www.unicode.org/charts/index.html) können Sie das Diagramm enthält die Zeichen suchen.</span><span class="sxs-lookup"><span data-stu-id="370b0-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="370b0-169">Jede Encoder, Html, JavaScript und -Url muss separat konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="370b0-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="370b0-170">Anpassen der Liste der sicheren wirkt sich nur auf Encoder über DI stammen.</span><span class="sxs-lookup"><span data-stu-id="370b0-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="370b0-171">Wenn Sie einen Encoder über direkten Zugriff auf `System.Text.Encodings.Web.*Encoder.Default` klicken Sie dann die in der Standardeinstellung lateinischen nur von Listen sicherer Adressen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="370b0-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="370b0-172">Wo sollten Codierung Take platzieren?</span><span class="sxs-lookup"><span data-stu-id="370b0-172">Where should encoding take place?</span></span>

<span data-ttu-id="370b0-173">Die allgemeinen angenommen, dass die Methode besteht darin, dass die Codierung erfolgt zum Zeitpunkt der Ausgabe und codierte Werte nie in einer Datenbank gespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="370b0-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="370b0-174">Zum Zeitpunkt der Ausgabe Codierung ermöglicht die Verwendung von Daten, z. B. von HTML in einen Zeichenfolgenwert für die Abfrage zu ändern.</span><span class="sxs-lookup"><span data-stu-id="370b0-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="370b0-175">Außerdem ermöglicht es Ihnen, Ihre Daten einfach zu suchen, ohne Werte vor dem Suchen codieren und bietet die Möglichkeit, alle Änderungen oder bei Fehlerbehebungen versucht, Encoder nutzen.</span><span class="sxs-lookup"><span data-stu-id="370b0-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="370b0-176">Überprüfung als eine XSS-Technik zum Verhindern von Datenverlusten</span><span class="sxs-lookup"><span data-stu-id="370b0-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="370b0-177">Überprüfung kann ein nützliches Tool in XSS-Angriffen eingeschränkt werden.</span><span class="sxs-lookup"><span data-stu-id="370b0-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="370b0-178">Beispielsweise wird eine einfache numerische Zeichenfolge, enthält nur die Zeichen 0-9 XSS-Angriff nicht ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="370b0-178">For example, a simple numeric string containing only the characters 0-9 will not trigger an XSS attack.</span></span> <span data-ttu-id="370b0-179">Überprüfung komplizierter Sie HTML in Benutzereingaben - akzeptieren möchten, sollten Analyse von HTML-Eingaben schwierig, wenn nicht sogar unmöglich ist.</span><span class="sxs-lookup"><span data-stu-id="370b0-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="370b0-180">MarkDown und andere Textformate wäre eine sicherere Option für umfangreiche Eingaben auf.</span><span class="sxs-lookup"><span data-stu-id="370b0-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="370b0-181">Sie sollten niemals zur Validierung von allein verlassen.</span><span class="sxs-lookup"><span data-stu-id="370b0-181">You should never rely on validation alone.</span></span> <span data-ttu-id="370b0-182">Nicht vertrauenswürdige Eingaben vor der Ausgabe immer zu codieren, unabhängig davon, was eine Überprüfung durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="370b0-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>