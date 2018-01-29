---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: "Zwischenspeichern von Daten in einer ASP.NET Web-Seiten (Razor) Standort für eine bessere Leistung | Microsoft Docs"
author: tfitzmac
description: "Sie können beschleunigte Websites durch Speichern – d. h. dass Cache - Daten, die normalerweise würde sehr lange dauern abrufen oder Verarbeiten der Ergebnisse einer..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/14/2014
ms.topic: article
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 742409219bd3b05f8ddf2c0d5034919fc9bf1d26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a><span data-ttu-id="6cc34-103">Zwischenspeichern von Daten an einem Standort der ASP.NET Web Pages (Razor) für eine bessere Leistung</span><span class="sxs-lookup"><span data-stu-id="6cc34-103">Caching Data in an ASP.NET Web Pages (Razor) Site for Better Performance</span></span>
====================
<span data-ttu-id="6cc34-104">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6cc34-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6cc34-105">In diesem Artikel wird erläutert, wie eine Hilfsmethode zum Cacheinformationen für eine bessere Leistung in einer Website für ASP.NET Web Pages (Razor) verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6cc34-105">This article explains how to use a helper to cache information for faster performance in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="6cc34-106">Sie können Ihrer Website beschleunigen, indem Sie ihm Store &#8212; d. h. Cache &#8212; die Ergebnisse, die normalerweise in Anspruch nehmen würde sehr viel Zeit zum Abrufen oder verarbeiten und die Daten werden nicht häufig geändert.</span><span class="sxs-lookup"><span data-stu-id="6cc34-106">You can speed up your website by having it store &#8212; that is, cache &#8212; the results of data that ordinarily would take considerable time to retrieve or process and that does not change often.</span></span>
> 
> <span data-ttu-id="6cc34-107">**Lernen Sie:**</span><span class="sxs-lookup"><span data-stu-id="6cc34-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="6cc34-108">Wie Zwischenspeicherung verwenden, um die Reaktionsfähigkeit Ihrer Website zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="6cc34-108">How to use caching to improve the responsiveness of your website.</span></span>
> 
> <span data-ttu-id="6cc34-109">Dies sind die Funktionen von ASP.NET im Artikel:</span><span class="sxs-lookup"><span data-stu-id="6cc34-109">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="6cc34-110">Die `WebCache` Helper.</span><span class="sxs-lookup"><span data-stu-id="6cc34-110">The `WebCache` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6cc34-111">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="6cc34-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6cc34-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="6cc34-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="6cc34-113">Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="6cc34-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="6cc34-114">Jedes Mal, wenn ein Benutzer eine Seite über Ihren Standort anfordert, muss der Webserver einige arbeiten zu erledigen, um die Anforderung zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="6cc34-114">Every time someone requests a page from your site, the web server has to do some work in order to fulfill the request.</span></span> <span data-ttu-id="6cc34-115">Der Server u. für einige der Seiten um Aufgaben auszuführen, die (vergleichsweise), wie z. B. das Abrufen von Daten aus einer Datenbank dauert.</span><span class="sxs-lookup"><span data-stu-id="6cc34-115">For some of your pages, the server might have to perform tasks that take a (comparatively) long time, such as retrieving data from a database.</span></span> <span data-ttu-id="6cc34-116">Auch wenn diese Aufgaben in absoluten Ausdrücken lang erfordern, wenn Ihre Website viel Datenverkehr auftritt, kann eine ganze Reihe von einzelnen Anforderungen, die dazu führen, der Webserver zum Ausführen der Aufgabe langsamen oder komplizierten dass zu viel Arbeit summieren.</span><span class="sxs-lookup"><span data-stu-id="6cc34-116">Even if these tasks don't take long in absolute terms, if your site experiences a lot of traffic, a whole series of individual requests that cause the web server to perform the complicated or slow task can add up to a lot of work.</span></span> <span data-ttu-id="6cc34-117">Dies kann letztendlich die Leistung des Standorts auswirken.</span><span class="sxs-lookup"><span data-stu-id="6cc34-117">This can ultimately affect the performance of the site.</span></span>

<span data-ttu-id="6cc34-118">Eine Möglichkeit zum Verbessern der Leistung Ihrer Website unter Umständen wie folgt ist zum Zwischenspeichern von Daten.</span><span class="sxs-lookup"><span data-stu-id="6cc34-118">One way to improve the performance of your website in circumstances like this is to cache data.</span></span> <span data-ttu-id="6cc34-119">Wenn Ihre Website wiederholte Anforderungen für die gleichen Informationen ruft müssen die Informationen nicht für jede Person geändert werden und es nicht Zeit ist beachtet wird, anstatt erneut abrufen oder Neuberechnen, können die Daten einmal abrufen und speichern Sie die Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="6cc34-119">If your site gets repeated requests for the same information, and the information does not need to be modified for each person, and it's not time sensitive, instead of re-fetching or recalculating it, you can fetch the data once and then store the results.</span></span> <span data-ttu-id="6cc34-120">Das nächste Mal, das eine Anforderung für diesen eingeht, herunterladen Informationen Sie einfach aus dem Cache.</span><span class="sxs-lookup"><span data-stu-id="6cc34-120">The next time a request comes in for that information, you just get it out of the cache.</span></span>

<span data-ttu-id="6cc34-121">Im Allgemeinen Zwischenspeichern Sie Informationen, die nicht häufig ändern.</span><span class="sxs-lookup"><span data-stu-id="6cc34-121">In general, you cache information that doesn't change frequently.</span></span> <span data-ttu-id="6cc34-122">Wenn Sie Informationen im Cache ablegen, wird es im Arbeitsspeicher auf dem Webserver gespeichert.</span><span class="sxs-lookup"><span data-stu-id="6cc34-122">When you put information in the cache, it's stored in memory on the web server.</span></span> <span data-ttu-id="6cc34-123">Sie können angeben, wie lange es, von Sekunden auf Tage zwischengespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="6cc34-123">You can specify how long it should be cached, from seconds to days.</span></span> <span data-ttu-id="6cc34-124">Ablauf des Zeitraums der Zwischenspeicherung wird die Informationen automatisch aus dem Cache entfernt.</span><span class="sxs-lookup"><span data-stu-id="6cc34-124">When the caching period expires, the information is automatically removed from the cache.</span></span>

> [!NOTE]
> <span data-ttu-id="6cc34-125">Einträge im Cache möglicherweise Gründen außer entfernt, die abgelaufen sind.</span><span class="sxs-lookup"><span data-stu-id="6cc34-125">Entries in the cache might be removed for reasons other than that they've expired.</span></span> <span data-ttu-id="6cc34-126">Beispielsweise der Webserver möglicherweise vorübergehend viel Arbeitsspeicher, und eine Möglichkeit, die es Arbeitsspeicher freigeben kann, ist durch das Auslösen von Einträgen aus dem Cache.</span><span class="sxs-lookup"><span data-stu-id="6cc34-126">For example, the web server might temporarily run low on memory, and one way it can reclaim memory is by throwing entries out of the cache.</span></span> <span data-ttu-id="6cc34-127">Wie Sie sehen, auch wenn Sie die Informationen in den Cache gesteckt haben, müssen Sie sicherstellen, dass sie immer noch vorhanden ist, bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="6cc34-127">As you'll see, even if you've put information into the cache, you have to check to be sure it's still there when you need it.</span></span>


<span data-ttu-id="6cc34-128">Angenommen Sie, Ihre Website eine Seite ist, die die aktuelle Temperatur und Wettervorhersage anzeigt.</span><span class="sxs-lookup"><span data-stu-id="6cc34-128">Imagine your website has a page that displays the current temperature and weather forecast.</span></span> <span data-ttu-id="6cc34-129">Um diese Art von Informationen zu erhalten, können Sie eine Anforderung an einen externen Dienst senden.</span><span class="sxs-lookup"><span data-stu-id="6cc34-129">To get this type of information, you might send a request to an external service.</span></span> <span data-ttu-id="6cc34-130">Da diese Informationen nicht viel (innerhalb eines zweistündigen Zeitraums, z. B.) ändern und externe Aufrufe Zeit und Bandbreite erfordern, ist es ein guter Kandidat für das caching.</span><span class="sxs-lookup"><span data-stu-id="6cc34-130">Since this information doesn't change much (within a two-hour time period, for example) and since external calls require time and bandwidth, it's a good candidate for caching.</span></span>

## <a name="adding-caching-to-a-page"></a><span data-ttu-id="6cc34-131">Hinzufügen von Caching zu einer Seite</span><span class="sxs-lookup"><span data-stu-id="6cc34-131">Adding Caching to a Page</span></span>

<span data-ttu-id="6cc34-132">ASP.NET umfasst einen `WebCache` -Hilfsprogramm, das erleichtert das caching auf Ihrer Website hinzufügen und den Cache Daten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="6cc34-132">ASP.NET includes a `WebCache` helper that makes it easy to add caching to your site and add data to the cache.</span></span> <span data-ttu-id="6cc34-133">In diesem Verfahren erstellen Sie eine Seite, die die aktuelle Zeit zwischenspeichert.</span><span class="sxs-lookup"><span data-stu-id="6cc34-133">In this procedure, you'll create a page that caches the current time.</span></span> <span data-ttu-id="6cc34-134">Dies ist ein praktisches Beispiel nicht, da die aktuelle Uhrzeit etwas, das häufig ändert, und, ist, die darüber hinaus ist nicht komplex zum Berechnen.</span><span class="sxs-lookup"><span data-stu-id="6cc34-134">This isn't a real-world example, since the current time is something that does change often, and that moreover isn't complex to calculate.</span></span> <span data-ttu-id="6cc34-135">Es ist jedoch eine gute Möglichkeit zum Zwischenspeichern in Aktion zu veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="6cc34-135">However, it's a good way to illustrate caching in action.</span></span>

1. <span data-ttu-id="6cc34-136">Fügen Sie eine neue Seite mit dem Namen *WebCache.cshtml* auf der Website.</span><span class="sxs-lookup"><span data-stu-id="6cc34-136">Add a new page named *WebCache.cshtml* to the website.</span></span>
2. <span data-ttu-id="6cc34-137">Der folgende Code und Markup der Seite hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="6cc34-137">Add the following code and markup to the page:</span></span>

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    <span data-ttu-id="6cc34-138">Wenn Sie Daten zwischenzuspeichern, Sie ihn im Cache abgelegt unter Verwendung eines Namens Dies ist eindeutig innerhalb der Website.</span><span class="sxs-lookup"><span data-stu-id="6cc34-138">When you cache data, you put it into the cache using a name this is unique across the website.</span></span> <span data-ttu-id="6cc34-139">In diesem Fall verwenden Sie einen Cacheeintrag, der mit dem Namen `CachedTime`.</span><span class="sxs-lookup"><span data-stu-id="6cc34-139">In this case, you'll use a cache entry named `CachedTime`.</span></span> <span data-ttu-id="6cc34-140">Dies ist die `cacheItemKey` im Codebeispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6cc34-140">This is the `cacheItemKey` shown in the code example.</span></span>

    <span data-ttu-id="6cc34-141">Der Code zunächst liest die `CachedTime` des Cacheeintrags.</span><span class="sxs-lookup"><span data-stu-id="6cc34-141">The code first reads the `CachedTime` cache entry.</span></span> <span data-ttu-id="6cc34-142">Wenn ein Wert zurückgegeben wird (d. h., wenn der Cacheeintrag nicht null ist), legt der Code einfach den Wert der Variablen Zeit auf das Zwischenspeichern von Daten fest.</span><span class="sxs-lookup"><span data-stu-id="6cc34-142">If a value is returned (that is, if the cache entry isn't null), the code just sets the value of the time variable to the cache data.</span></span>

    <span data-ttu-id="6cc34-143">Jedoch, wenn der Cacheeintrag nicht vorhanden ist (d. h. er null ist), der Code legt das Time-Werten, dem Cache hinzugefügt und einem Ablaufwert auf eine Minute.</span><span class="sxs-lookup"><span data-stu-id="6cc34-143">However, if the cache entry doesn't exist (that is, it's null), the code sets the time value, adds it to the cache, and sets an expiration value to one minute.</span></span> <span data-ttu-id="6cc34-144">Nach einer Minute wird der Cacheeintrag verworfen.</span><span class="sxs-lookup"><span data-stu-id="6cc34-144">After one minute, the cache entry is discarded.</span></span> <span data-ttu-id="6cc34-145">(Der Standardwert für die Ablaufzeit für ein Element im Cache ist 20 Minuten.) Der Befehl `WebCache.Set(cacheItemKey, time, 1, false)` wird gezeigt, wie der Cache den aktuellen Zeitwert hinzu, und legen Sie dessen Ablauf auf 1 Minute.</span><span class="sxs-lookup"><span data-stu-id="6cc34-145">(The default expiration value for an item in the cache is 20 minutes.) The command `WebCache.Set(cacheItemKey, time, 1, false)` shows how to add the current time value to the cache and set its expiration to 1 minute.</span></span> <span data-ttu-id="6cc34-146">Festlegen der *SlidingExpiration* Parameter `false` bedeutet, dass die Ablaufzeit wird nicht jedes Mal, die sie angefordert wird erneuert.</span><span class="sxs-lookup"><span data-stu-id="6cc34-146">Setting the *slidingExpiration* parameter to `false` means the expiration time is not renewed each time it is requested.</span></span> <span data-ttu-id="6cc34-147">Es läuft genau 1 Minute, nachdem sie ursprünglich mit dem Cache hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="6cc34-147">It will expire exactly 1 minute after it was originally added to the cache.</span></span> <span data-ttu-id="6cc34-148">Wenn Sie diesen Wert, um festlegen `true` die Ablaufzeit von einer Minute wird jedes Mal zurückgesetzt, der Wert aus dem Cache angefordert wird.</span><span class="sxs-lookup"><span data-stu-id="6cc34-148">If you set this value to `true` the 1 minute expiration time is reset each time the value is requested from the cache.</span></span>

    <span data-ttu-id="6cc34-149">Dieser Code veranschaulicht das Muster, das Sie beim Zwischenspeichern von Daten immer verwenden soll.</span><span class="sxs-lookup"><span data-stu-id="6cc34-149">This code illustrates the pattern you should always use when you cache data.</span></span> <span data-ttu-id="6cc34-150">Bevor Sie ein Element aus dem Cache erhalten, überprüfen Sie zunächst immer, ob die `WebCache.Get` Methode hat null zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6cc34-150">Before you get something out of the cache, always check first whether the `WebCache.Get` method has returned null.</span></span> <span data-ttu-id="6cc34-151">Denken Sie daran, dass der Cacheeintrag möglicherweise abgelaufen oder möglicherweise einem anderen Grund entfernt wurde, ein bestimmten Eintrag garantiert nie im Cache befinden.</span><span class="sxs-lookup"><span data-stu-id="6cc34-151">Remember that the cache entry might have expired or might have been removed for some other reason, so any given entry is never guaranteed to be in the cache.</span></span>
3. <span data-ttu-id="6cc34-152">Führen Sie *WebCache.cshtml* in einem Browser.</span><span class="sxs-lookup"><span data-stu-id="6cc34-152">Run *WebCache.cshtml* in a browser.</span></span> <span data-ttu-id="6cc34-153">(Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Beim ersten Anforderung von der Seite die Zeitdaten befindet sich nicht im Cache und der Code muss in der Time-Werten in den Cache hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6cc34-153">(Make sure the page is selected in the **Files** workspace before you run it.) The first time you request the page, the time data isn't in the cache, and the code has to add the time value to the cache.</span></span>

    ![Cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. <span data-ttu-id="6cc34-155">Aktualisieren Sie *WebCache.cshtml* im Browser.</span><span class="sxs-lookup"><span data-stu-id="6cc34-155">Refresh *WebCache.cshtml* in the browser.</span></span> <span data-ttu-id="6cc34-156">Diesmal ist die Zeitdaten im Cache.</span><span class="sxs-lookup"><span data-stu-id="6cc34-156">This time, the time data is in the cache.</span></span> <span data-ttu-id="6cc34-157">Beachten Sie, dass die Zeit seit der letzten Ausführung geändert hat, Sie auf die Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6cc34-157">Notice that the time hasn't changed since the last time you viewed the page.</span></span>

    ![Cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. <span data-ttu-id="6cc34-159">Warten Sie einen Moment der Zwischenspeicher geleert werden, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="6cc34-159">Wait one minute for the cache to be emptied, and then refresh the page.</span></span> <span data-ttu-id="6cc34-160">Die Seite gibt erneut an, dass die Uhrzeitdaten wurde nicht im Cache gefunden, und die aktualisierte Zeit zum Cache hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="6cc34-160">The page again indicates that the time data wasn't found in the cache, and the updated time is added to the cache.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="6cc34-161">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="6cc34-161">Additional Resources</span></span>


- [<span data-ttu-id="6cc34-162">Anzeigen von Daten in einem Diagramm</span><span class="sxs-lookup"><span data-stu-id="6cc34-162">Displaying Data in a Chart</span></span>](https://go.microsoft.com/fwlink/?LinkId=202895)
- <span data-ttu-id="6cc34-163">[WebCache-API-Referenz](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="6cc34-163">[WebCache API reference](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)</span></span>