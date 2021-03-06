---
title: Anchor-Tag-Hilfsprogramm in ASP.NET Core
author: pkellner
description: Lernen Sie die Attribute für das ASP.NET Core-Anchor-Taghilfsprogramm kennen und erfahren Sie, welche Rolle jedes Attribut bei der Erweiterung des Verhaltens des HTML-Anchor-Tags spielt.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 6bdf71eaf38f134cb15b5950d2cae6ab67f861a4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273883"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>Anchor-Tag-Hilfsprogramm in ASP.NET Core

Von [Peter Kellner](http://peterkellner.net) und [Scott Addie](https://github.com/scottaddie)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Das [Anchor-Taghilfsprogramm](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) verbessert das HTML-Anchor-Tag (`<a ... ></a>`), indem neue Attribute hinzugefügt werden. Per Konvention erhalten Attributnamen das Präfix `asp-`. Der Attributwert `href` des gerenderten Anchor-Elements wird von den Werten der `asp-`-Attribute bestimmt.

Der folgende *SpeakerController* wird in den Beispielen in diesem Dokument verwendet:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Eine Liste der `asp-`-Attribute folgt.

## <a name="asp-controller"></a>asp-controller

Das Attribut [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) weist den Controller zu, der zum Generieren der URL verwendet wird. Im folgenden Markup werden alle Lautsprecher aufgeführt:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

Der generierte HTML-Code:

```html
<a href="/Speaker">All Speakers</a>
```

Wenn das Attribut `asp-controller` angegeben wird und `asp-action` nicht, entspricht der `asp-action`-Standardwert der Controlleraktion, die der aktuell ausgeführten Ansicht zugeordnet ist. Wenn `asp-action` im obigen Markup ausgelassen wird und das Anchor-Taghilfsprogramm in der Ansicht *Index* von *HomeController* verwendet wird (*/Home*), wird folgender HTML-Code generiert:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>asp-action

Der Attributwert [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) repräsentiert den Namen der Controlleraktion, die im generierten `href`-Attribut enthalten ist. Im folgenden Markup wird der generierte `href`-Attributwert auf die Auswertungsseite des Lautsprechers festgelegt:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Der generierte HTML-Code:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Wenn kein `asp-controller`-Attribut angegeben ist, wird der Standardcontroller zum Aufruf der Ansicht verwendet, die die aktuelle Ansicht ausführt.

Wenn der Wert des `asp-action`-Attribut `Index` lautet, wird keine Aktion an die URL angefügt, was zum Aufruf der `Index`-Standardaktion führt. Die angegebene (oder standardmäßige) Aktion muss im Controller vorhanden sein, auf den in `asp-controller` verwiesen wird.

## <a name="asp-route-value"></a>asp-route-{value}

Das Attribut [asp-route{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) aktiviert ein Platzhalterroutenpräfix. Ein beliebiger Wert im Platzhalter `{value}` wird als potenzieller Routenparameter interpretiert. Wenn keine Standardroute gefunden wird, wird dieses Routenpräfix als Anforderungsparameter und -wert an das generierte `href`-Attribut angefügt. Andernfalls wird es in der Routenvorlage ersetzt.

Betrachten Sie die folgende Controlleraktion:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Mit einer in *Startup.Configure* definierten Standardroutenvorlage:

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

Die MVC-Ansicht verwendet das von der Aktion bereitgestellte Modell wie folgt:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

Der `{id?}`-Platzhalter der Standardroute wurde abgeglichen. Der generierte HTML-Code:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Angenommen, das Routenpräfix ist (wie in der folgenden MVC-Ansicht gezeigt) nicht Teil der übereinstimmenden Routingvorlage:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

Der folgende HTML-Code wird generiert, weil `speakerid` nicht in der übereinstimmenden Route gefunden wurde:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Wenn entweder `asp-controller` oder `asp-action` nicht angegeben werden, erfolgt der gleiche Standardverarbeitungsprozess wie beim `asp-route`-Attribut.

## <a name="asp-route"></a>asp-route

Das Attribut [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) wird zum Erstellen einer URL verwendet, die direkt auf eine benannte Route verweist. Mit der Verwendung von [Routingattributen](xref:mvc/controllers/routing#attribute-routing) kann eine Route wie in `SpeakerController` gezeigt benannt und in der zugehörigen `Evaluations`-Aktion verwendet werden:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

Im folgenden Markup verweist das `asp-route`-Attribut auf die benannte Route:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Das Anchor-Taghilfsprogramm generiert eine Route direkt zu dieser Controlleraktion. Dabei wird die URL */Speaker/Evaluations* verwendet. Der generierte HTML-Code:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Wenn `asp-controller` oder `asp-action` zusätzlich zu `asp-route` angegeben sind, entspricht die generierte Route möglicherweise nicht Ihren Erwartungen. Um einen Routenkonflikt zu vermeiden, darf `asp-route` nicht mit den Attributen `asp-controller` oder `asp-action` verwendet werden.

## <a name="asp-all-route-data"></a>asp-all-route-data

Das Attribut [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) unterstützt das Erstellen eines Wörterbuchs aus Schlüssel-Wert-Paaren. Der Schlüssel ist der Parametername, der Wert ist der Parameterwert.

Im folgenden Beispiel wird ein Wörterbuch initialisiert und an eine Razor-Ansicht übergeben. Alternativ können die Daten auch mit Ihrem Modell übergeben werden.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Der oben gezeigte Code generiert den folgenden HTML-Code:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

Das `asp-all-route-data`-Wörterbuch wird vereinfacht, um eine Abfragezeichenfolge zu erstellen, die die Anforderungen der überladenen `Evaluations`-Aktion erfüllt:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Wenn ein beliebiger Schlüssel im Wörterbuch mit einem Routenparameter übereinstimmt, werden die Werte entsprechend in der Route eingesetzt. Die nicht übereinstimmenden Werte werden als Anforderungsparameter generiert.

## <a name="asp-fragment"></a>asp-fragment

Das Attribut [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) definiert ein URL-Fragment, das an die URL angefügt werden soll. Das Anchor-Taghilfsprogramm fügt das Hashzeichen (#) hinzu. Sehen Sie sich das folgende Markup an:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Der generierte HTML-Code:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Hashtags sind beim Erstellen von clientseitigen Apps nützlich. Sie können beispielsweise zum einfachen Markieren und Suchen in JavaScript verwendet werden.

## <a name="asp-area"></a>asp-area

Das Attribut [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) legt den Bereichsnamen zum Festlegen der geeigneten Route fest. Das folgende Beispiel zeigt, wie das Bereichsattribut eine Neuzuordnung von Routen bewirken kann. Das Festlegen von `asp-area` auf „Blogs“ stellt das Verzeichnis *Areas/Blogs* den Routen der zugeordneten Controller und Ansichten für dieses Anchor-Tag voran.

* **<Projektname\>**
  * **wwwroot**
  * **Bereiche**
    * **Blogs**
      * **Controller**
        * *HomeController.cs*
      * **Ansichten**
        * **Home**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *_ViewStart.cshtml*
  * **Controller**

Gemäß der obigen Verzeichnishierarchie lautet das Markup zum Verweis auf die Datei *AboutBlog.cshtml* wie folgt:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Der generierte HTML-Code:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Die Routenvorlage muss einen Verweis auf den Bereich enthalten, damit Bereiche in einer MVC-App funktionieren. Diese Vorlage wird durch den zweiten Parameter des `routes.MapRoute`-Methodenaufrufs in *Startup.Configure* repräsentiert: [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

Das Attribut [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) gibt ein Protokoll in Ihrer URL an (z.B. `https`). Zum Beispiel:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Der generierte HTML-Code:

```html
<a href="https://localhost/Home/About">About</a>
```

Der Hostname im Beispiel lautet „localhost“, aber das Anchor-Taghilfsprogramm verwendet die öffentliche Domäne der Website beim Generieren der URL.

## <a name="asp-host"></a>asp-host

Mit dem Attribut [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) wird ein Hostname in Ihrer URL angegeben. Zum Beispiel:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Der generierte HTML-Code:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>asp-page

Das Attribut [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) wird mit Razor Pages verwendet. Mit ihm wird der `href`-Attributwerts des Anchor-Tags auf eine bestimmte Seite festgelegt. Wenn Sie dem Seitennamen einen Schrägstrich „/“ voranstellen, wird die URL erstellt.

Das folgende Beispiel zeigt auf die Razor Page des Teilnehmers:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

Der generierte HTML-Code:

```html
<a href="/Attendee">All Attendees</a>
```

Das `asp-page`-Attribut und die `asp-route`-, `asp-controller`- und `asp-action`-Attribute schließen sich gegenseitig aus. Allerdings kann `asp-page` mit `asp-route-{value}` genutzt werden, um das Routing zu steuern, wie im folgenden Markup veranschaulicht wird:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Der generierte HTML-Code:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

Das Attribut [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) wird mit Razor Pages verwendet. Es dient zur Verknüpfung mit bestimmten Seitenhandlern.

Betrachten Sie den folgenden Seitenhandler:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Das Markup des Seitenmodells verweist auf den Seitenhandler `OnGetProfile`. Beachten Sie, dass das Präfix `On<Verb>` des Methodennamens für den Seitenhandler im Attributwert `asp-page-handler` ausgelassen wurde. Würde es sich um eine asynchrone Methode handeln, würde auch das Suffix `Async` ausgelassen.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Der generierte HTML-Code:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Bereiche](xref:mvc/controllers/areas)
* [Einführung in Razor Pages](xref:razor-pages/index)
