
<span data-ttu-id="b5272-101">Wir behandeln [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) im nächsten Tutorial.</span><span class="sxs-lookup"><span data-stu-id="b5272-101">We'll cover [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) in the next tutorial.</span></span> <span data-ttu-id="b5272-102">Das [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx)-Attribut gibt an, was für den Namen eines Felds angezeigt werden soll (in diesem Fall „Release Date“ anstatt „ReleaseDate“).</span><span class="sxs-lookup"><span data-stu-id="b5272-102">The [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) attribute specifies what to display for the name of a field (in this case "Release Date" instead of "ReleaseDate").</span></span> <span data-ttu-id="b5272-103">Das [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)-Attribut gibt den Typ der Daten (Date) an, damit die im Feld gespeicherten Uhrzeitinformationen nicht angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b5272-103">The [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) attribute specifies the type of the data (Date), so the time information stored in the field is not displayed.</span></span>

<span data-ttu-id="b5272-104">Navigieren Sie zum `Movies`-Controller, und halten Sie den Mauszeiger über einen **Bearbeiten**-Link, um die Ziel-URL zu sehen.</span><span class="sxs-lookup"><span data-stu-id="b5272-104">Browse to the `Movies` controller and hold the mouse pointer over an **Edit** link to see the target URL.</span></span>

![Browserfenster mit Maus über dem Link „Bearbeiten“ und der Link-URL „http://localhost:1234/Movies/Edit/5“](../../tutorials/first-mvc-app/controller-methods-views/_static/edit7.png)

<span data-ttu-id="b5272-106">Die Links **Bearbeiten**, **Details** und **Löschen** werden mithilfe des MVC Core-Hilfsprogramms für Ankertags in der Datei *Views/Movies/Index.cshtml* generiert.</span><span class="sxs-lookup"><span data-stu-id="b5272-106">The **Edit**, **Details**, and **Delete** links are generated by the MVC Core Anchor Tag Helper in the *Views/Movies/Index.cshtml* file.</span></span>

<span data-ttu-id="b5272-107">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]</span><span class="sxs-lookup"><span data-stu-id="b5272-107">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1-3&range=46-50)]</span></span>

<span data-ttu-id="b5272-108">Durch [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) kann auch serverseitiger Code HTML-Elemente in Razor-Dateien erstellen und rendern.</span><span class="sxs-lookup"><span data-stu-id="b5272-108">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="b5272-109">Im obigen Code generiert `AnchorTagHelper` dynamisch den HTML-`href`-Attributwert aus der Controlleraktionsmethode und der Routen-ID. Verwenden Sie in Ihrem bevorzugten Browser **Quelltext anzeigen** oder die Entwicklertools, um das generierte Markup zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="b5272-109">In the code above, the `AnchorTagHelper` dynamically generates the HTML `href` attribute value from the controller action method and route id. You use **View Source** from your favorite browser or use the developer tools to examine the generated markup.</span></span> <span data-ttu-id="b5272-110">Ein Teil des generierten HTML-Codes wird unten gezeigt:</span><span class="sxs-lookup"><span data-stu-id="b5272-110">A portion of the generated HTML is shown below:</span></span>

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

<span data-ttu-id="b5272-111">Erinnern Sie sich an das Format für das [Routing](xref:mvc/controllers/routing) in der Datei *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b5272-111">Recall the format for [routing](xref:mvc/controllers/routing) set in the *Startup.cs* file:</span></span>

<span data-ttu-id="b5272-112">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="b5272-112">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]</span></span>

<span data-ttu-id="b5272-113">ASP.NET Core übersetzt `http://localhost:1234/Movies/Edit/4` in eine Anforderung an die `Edit`-Aktionsmethode des `Movies`-Controllers mit dem `Id`-Parameter 4.</span><span class="sxs-lookup"><span data-stu-id="b5272-113">ASP.NET Core translates `http://localhost:1234/Movies/Edit/4` into a request to the `Edit` action method of the `Movies` controller with the parameter `Id` of 4.</span></span> <span data-ttu-id="b5272-114">(Controllermethoden werden auch als Aktionsmethoden bezeichnet.)</span><span class="sxs-lookup"><span data-stu-id="b5272-114">(Controller methods are also known as action methods.)</span></span>

<span data-ttu-id="b5272-115">[Taghilfsprogramme](xref:mvc/views/tag-helpers/intro) sind eines der am häufigsten verwendeten neuen Features in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5272-115">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are one of the most popular new features in ASP.NET Core.</span></span> <span data-ttu-id="b5272-116">Weitere Informationen finden Sie unter [Zusätzliche Ressourcen](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="b5272-116">See [Additional resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="b5272-117">Öffnen Sie den `Movies`-Controller, und untersuchen Sie die beiden `Edit`-Aktionsmethoden.</span><span class="sxs-lookup"><span data-stu-id="b5272-117">Open the `Movies` controller and examine the two `Edit` action methods.</span></span> <span data-ttu-id="b5272-118">Der folgende Code zeigt die `HTTP GET Edit`-Methode, die den Film abruft und das Bearbeitungsformular ausfüllt, das von der Razor-Datei *Edit.cshtml* generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="b5272-118">The following code shows the `HTTP GET Edit` method, which fetches the movie and populates the edit form generated by the *Edit.cshtml* Razor file.</span></span>

<span data-ttu-id="b5272-119">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]</span><span class="sxs-lookup"><span data-stu-id="b5272-119">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]</span></span>

<span data-ttu-id="b5272-120">Der folgende Code zeigt die `HTTP POST Edit`-Methode, die die bereitgestellten Filmwerte verarbeitet:</span><span class="sxs-lookup"><span data-stu-id="b5272-120">The following code shows the `HTTP POST Edit` method, which processes the posted movie values:</span></span>

<span data-ttu-id="b5272-121">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]</span><span class="sxs-lookup"><span data-stu-id="b5272-121">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]</span></span>

<span data-ttu-id="b5272-122">Das `[Bind]`-Attribut ist eine Möglichkeit zum Schutz vor [zu vielen Angaben](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost).</span><span class="sxs-lookup"><span data-stu-id="b5272-122">The `[Bind]` attribute is one way to protect against [over-posting](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost).</span></span> <span data-ttu-id="b5272-123">Sie sollten nur Eigenschaften in das `[Bind]`-Attribut aufnehmen, die Sie ändern möchten.</span><span class="sxs-lookup"><span data-stu-id="b5272-123">You should only include properties in the `[Bind]` attribute that you want to change.</span></span> <span data-ttu-id="b5272-124">Weitere Informationen finden Sie unter [Protect your controller from over-posting](http://www.asp.net/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) (Schützen Ihres Controllers vor zu vielen Angaben).</span><span class="sxs-lookup"><span data-stu-id="b5272-124">See [Protect your controller from over-posting](http://www.asp.net/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) for more information.</span></span> <span data-ttu-id="b5272-125">[ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) bietet eine alternative Methode, um zu viele Angaben zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="b5272-125">[ViewModels](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) provide an alternative approach to prevent over-posting.</span></span>

<span data-ttu-id="b5272-126">Beachten Sie, dass der zweiten `Edit`-Aktionsmethode das `[HttpPost]`-Attribut vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="b5272-126">Notice the second `Edit` action method is preceded by the `[HttpPost]` attribute.</span></span>

<span data-ttu-id="b5272-127">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="b5272-127">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2&highlight=4)]</span></span>

<span data-ttu-id="b5272-128">Das `HttpPost`-Attribut gibt an, dass diese `Edit`-Methode *nur* für `POST`-Anforderungen aufgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="b5272-128">The `HttpPost` attribute specifies that this `Edit` method can be invoked *only* for `POST` requests.</span></span> <span data-ttu-id="b5272-129">Sie könnten das `[HttpGet]`-Attribut auf die erste Bearbeitungsmethode anwenden, aber dies ist nicht erforderlich, da `[HttpGet]` der Standardwert ist.</span><span class="sxs-lookup"><span data-stu-id="b5272-129">You could apply the `[HttpGet]` attribute to the first edit method, but that's not necessary because `[HttpGet]` is the default.</span></span>

<span data-ttu-id="b5272-130">Das `ValidateAntiForgeryToken`-Attribut wird verwendet, um [die Fälschung einer Anforderung zu verhindern](xref:security/anti-request-forgery). Es wird einem Fälschungssicherheitstoken zugeordnet, das in der Datei für die Bearbeitungsansicht (*Views/Movies/Edit.cshtml*) generiert wird.</span><span class="sxs-lookup"><span data-stu-id="b5272-130">The `ValidateAntiForgeryToken` attribute is used to [prevent forgery of a request](xref:security/anti-request-forgery) and is paired up with an anti-forgery token generated in the edit view file (*Views/Movies/Edit.cshtml*).</span></span> <span data-ttu-id="b5272-131">Die Datei für die Bearbeitungsansicht generiert das Fälschungssicherheitstoken mit dem [Hilfsprogramm für Formulartags](xref:mvc/views/working-with-forms).</span><span class="sxs-lookup"><span data-stu-id="b5272-131">The edit view file generates the anti-forgery token with the [Form Tag Helper](xref:mvc/views/working-with-forms).</span></span>

<span data-ttu-id="b5272-132">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]</span><span class="sxs-lookup"><span data-stu-id="b5272-132">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/Edit.cshtml?range=9)]</span></span>

<span data-ttu-id="b5272-133">Das [Hilfsprogramm für Formulartags](xref:mvc/views/working-with-forms) generiert ein ausgeblendetes Fälschungssicherheitstoken, das mit dem von `[ValidateAntiForgeryToken]` generierten Fälschungssicherheitstoken in der `Edit`-Methode des Movies-Controllers übereinstimmen muss.</span><span class="sxs-lookup"><span data-stu-id="b5272-133">The [Form Tag Helper](xref:mvc/views/working-with-forms) generates a hidden anti-forgery token that must match the `[ValidateAntiForgeryToken]` generated anti-forgery token in the `Edit` method of the Movies controller.</span></span> <span data-ttu-id="b5272-134">Weitere Informationen finden Sie unter [Antianforderungsfälschung](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="b5272-134">For more information, see [Anti-Request Forgery](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="b5272-135">Die `HttpGet Edit`-Methode verwendet den `ID`-Parameter eines Films, sucht den Film mit der `SingleOrDefaultAsync`-Methode von Entity Framework, und gibt den ausgewählten Film an die Bearbeitungsansicht zurück.</span><span class="sxs-lookup"><span data-stu-id="b5272-135">The `HttpGet Edit` method takes the movie `ID` parameter, looks up the movie using the Entity Framework `SingleOrDefaultAsync` method, and returns the selected movie to the Edit view.</span></span> <span data-ttu-id="b5272-136">Wenn ein Film nicht gefunden werden kann, wird `NotFound` (HTTP 404) zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b5272-136">If a movie cannot be found, `NotFound` (HTTP 404) is returned.</span></span>

<span data-ttu-id="b5272-137">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]</span><span class="sxs-lookup"><span data-stu-id="b5272-137">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit1)]</span></span>

<span data-ttu-id="b5272-138">Als das Gerüstsystem die Bearbeitungsansicht erstellt hat, wurde die `Movie`-Klasse überprüft und Code zum Rendern der `<label>`- und `<input>`-Elemente für jede Eigenschaft der Klasse erstellt.</span><span class="sxs-lookup"><span data-stu-id="b5272-138">When the scaffolding system created the Edit view, it examined the `Movie` class and created code to render `<label>` and `<input>` elements for each property of the class.</span></span> <span data-ttu-id="b5272-139">Das folgende Beispiel zeigt die Bearbeitungsansicht, die vom Visual Studio-Gerüstsystem generiert wurde:</span><span class="sxs-lookup"><span data-stu-id="b5272-139">The following example shows the Edit view that was generated by the Visual Studio scaffolding system:</span></span>

<span data-ttu-id="b5272-140">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="b5272-140">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/EditCopy.cshtml?highlight=1)]</span></span>

<span data-ttu-id="b5272-141">Beachten Sie, dass die Ansichtsvorlage eine `@model MvcMovie.Models.Movie`-Anweisung am Anfang der Datei aufweist.</span><span class="sxs-lookup"><span data-stu-id="b5272-141">Notice how the view template has a `@model MvcMovie.Models.Movie` statement at the top of the file.</span></span> <span data-ttu-id="b5272-142">`@model MvcMovie.Models.Movie` gibt an, dass die Ansicht erwartet, dass das Modell für die Ansichtsvorlage den Typ `Movie` hat.</span><span class="sxs-lookup"><span data-stu-id="b5272-142">`@model MvcMovie.Models.Movie` specifies that the view expects the model for the view template to be of type `Movie`.</span></span>

<span data-ttu-id="b5272-143">Der Gerüstcode verwendet mehrere Methoden von Taghilfsprogrammen, um das HTML-Markup zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="b5272-143">The scaffolded code uses several Tag Helper methods to streamline the HTML markup.</span></span> <span data-ttu-id="b5272-144">Das [Hilfsprogramm für Bezeichnungstags](xref:mvc/views/working-with-forms) zeigt den Namen des Felds an („Title“, „ReleaseDate“, „Genre“ oder „Price“).</span><span class="sxs-lookup"><span data-stu-id="b5272-144">The - [Label Tag Helper](xref:mvc/views/working-with-forms) displays the name of the field ("Title", "ReleaseDate", "Genre", or "Price").</span></span> <span data-ttu-id="b5272-145">Das [Hilfsprogramm für Eingabetags](xref:mvc/views/working-with-forms) rendert ein HTML-`<input>`-Element.</span><span class="sxs-lookup"><span data-stu-id="b5272-145">The [Input Tag Helper](xref:mvc/views/working-with-forms) renders an HTML `<input>` element.</span></span> <span data-ttu-id="b5272-146">Das [Hilfsprogramm für Validierungstags](xref:mvc/views/working-with-forms) zeigt Validierungsmeldungen an, die dieser Eigenschaft zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="b5272-146">The [Validation Tag Helper](xref:mvc/views/working-with-forms) displays any validation messages associated with that property.</span></span>

<span data-ttu-id="b5272-147">Führen Sie die Anwendung aus, und navigieren Sie zur `/Movies`-URL.</span><span class="sxs-lookup"><span data-stu-id="b5272-147">Run the application and navigate to the `/Movies` URL.</span></span> <span data-ttu-id="b5272-148">Klicken Sie auf einen Link **Bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="b5272-148">Click an **Edit** link.</span></span> <span data-ttu-id="b5272-149">Zeigen Sie im Browser den Quelltext für die Seite an.</span><span class="sxs-lookup"><span data-stu-id="b5272-149">In the browser, view the source for the page.</span></span> <span data-ttu-id="b5272-150">Der generierte HTML-Code für das `<form>`-Element wird unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b5272-150">The generated HTML for the `<form>` element is shown below.</span></span>

<span data-ttu-id="b5272-151">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]</span><span class="sxs-lookup"><span data-stu-id="b5272-151">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/edit_view_source.html?highlight=1,6,10,17,24,28)]</span></span>

<span data-ttu-id="b5272-152">Die `<input>`-Elemente sind in einem `HTML <form>`-Element enthalten, dessen `action`-Attribut so festgelegt ist, dass Beiträge an die URL `/Movies/Edit/id` gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="b5272-152">The `<input>` elements are in an `HTML <form>` element whose `action` attribute is set to post to the `/Movies/Edit/id` URL.</span></span> <span data-ttu-id="b5272-153">Die Formulardaten werden an den Server gesendet, wenn auf die Schaltfläche `Save` geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="b5272-153">The form data will be posted to the server when the `Save` button is clicked.</span></span> <span data-ttu-id="b5272-154">Die letzte Zeile vor dem schließenden `</form>`-Element zeigt das ausgeblendete [XSRF](xref:security/anti-request-forgery)-Token, das durch das [Hilfsprogramm für Formulartags](xref:mvc/views/working-with-forms) generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="b5272-154">The last line before the closing `</form>` element shows the hidden [XSRF](xref:security/anti-request-forgery) token generated by the [Form Tag Helper](xref:mvc/views/working-with-forms).</span></span>

## <a name="processing-the-post-request"></a><span data-ttu-id="b5272-155">Verarbeiten der POST-Anforderung</span><span class="sxs-lookup"><span data-stu-id="b5272-155">Processing the POST Request</span></span>

<span data-ttu-id="b5272-156">Die folgende Liste zeigt die `[HttpPost]`-Version der `Edit`-Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="b5272-156">The following listing shows the `[HttpPost]` version of the `Edit` action method.</span></span>

<span data-ttu-id="b5272-157">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]</span><span class="sxs-lookup"><span data-stu-id="b5272-157">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_edit2)]</span></span>

<span data-ttu-id="b5272-158">Das `[ValidateAntiForgeryToken]`-Attribut überprüft das ausgeblendete [XSRF](xref:security/anti-request-forgery)-Token, das vom Generator für Fälschungssicherheitstoken im [Hilfsprogramm für Formulartags](xref:mvc/views/working-with-forms) generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="b5272-158">The `[ValidateAntiForgeryToken]` attribute validates the hidden [XSRF](xref:security/anti-request-forgery) token generated by the anti-forgery token generator in the [Form Tag Helper](xref:mvc/views/working-with-forms)</span></span>

<span data-ttu-id="b5272-159">Das [Modellbindungssystem](xref:mvc/models/model-binding) verwendet die übermittelten Formularwerte und erstellt ein `Movie`-Objekt, das als `movie`-Parameter übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="b5272-159">The [model binding](xref:mvc/models/model-binding) system takes the posted form values and creates a `Movie` object that's passed as the `movie` parameter.</span></span> <span data-ttu-id="b5272-160">Die `ModelState.IsValid`-Methode stellt sicher, dass die im Formular übermittelten Daten verwendet werden können, um ein `Movie`-Objekt zu ändern (bearbeiten oder aktualisieren).</span><span class="sxs-lookup"><span data-stu-id="b5272-160">The `ModelState.IsValid` method verifies that the data submitted in the form can be used to modify (edit or update) a `Movie` object.</span></span> <span data-ttu-id="b5272-161">Wenn die Daten gültig sind, werden sie gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b5272-161">If the data is valid it's saved.</span></span> <span data-ttu-id="b5272-162">Die aktualisierten (bearbeiteten) Filmdaten werden in der Datenbank gespeichert, indem die `SaveChangesAsync`-Methode des Datenbankkontexts aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="b5272-162">The updated (edited) movie data is saved to the database by calling the `SaveChangesAsync` method of database context.</span></span> <span data-ttu-id="b5272-163">Nach dem Speichern der Daten leitet der Code den Benutzer an die `Index`-Aktionsmethode der `MoviesController`-Klasse weiter, die die Filmsammlung einschließlich der gerade vorgenommenen Änderungen anzeigt.</span><span class="sxs-lookup"><span data-stu-id="b5272-163">After saving the data, the code redirects the user to the `Index` action method of the `MoviesController` class, which displays the movie collection, including the changes just made.</span></span>

<span data-ttu-id="b5272-164">Bevor das Formular an den Server gesendet wird, überprüft die clientseitige Validierung alle Validierungsregeln der Felder.</span><span class="sxs-lookup"><span data-stu-id="b5272-164">Before the form is posted to the server, client side validation checks any validation rules on the fields.</span></span> <span data-ttu-id="b5272-165">Sind Validierungsfehler vorhanden, wird eine Fehlermeldung angezeigt, und das Formular wird nicht gesendet.</span><span class="sxs-lookup"><span data-stu-id="b5272-165">If there are any validation errors, an error message is displayed and the form is not posted.</span></span> <span data-ttu-id="b5272-166">Wenn JavaScript deaktiviert ist, erfolgt keine clientseitige Validierung. Der Server erkennt jedoch, dass die bereitgestellten Werte nicht gültig sind, und die Formularwerte werden wieder mit Fehlermeldungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b5272-166">If JavaScript is disabled, you won't have client side validation but the server will detect the posted values that are not valid, and the form values will be redisplayed with error messages.</span></span> <span data-ttu-id="b5272-167">Später in diesem Tutorial untersuchen wir die [Modellvalidierung](xref:mvc/models/validation) im Detail.</span><span class="sxs-lookup"><span data-stu-id="b5272-167">Later in the tutorial we examine [Model Validation](xref:mvc/models/validation) in more detail.</span></span> <span data-ttu-id="b5272-168">Das [Hilfsprogramm für Validierungstags](xref:mvc/views/working-with-forms) in der Ansichtsvorlage *Views/Movies/Edit.cshtml* übernimmt die Anzeige der entsprechenden Fehlermeldungen.</span><span class="sxs-lookup"><span data-stu-id="b5272-168">The [Validation Tag Helper](xref:mvc/views/working-with-forms) in the *Views/Movies/Edit.cshtml* view template takes care of displaying appropriate error messages.</span></span>

![Ansicht bearbeiten: Eine Ausnahme für einen falschen Wert für den Preis von „abc“ gibt an, dass das Feld für den Preis eine Zahl sein muss.](../../tutorials/first-mvc-app/controller-methods-views/_static/val.png)

<span data-ttu-id="b5272-171">Alle `HttpGet`-Methoden im Movie-Controller folgen einem ähnlichen Muster.</span><span class="sxs-lookup"><span data-stu-id="b5272-171">All the `HttpGet` methods in the movie controller follow a similar pattern.</span></span> <span data-ttu-id="b5272-172">Sie erhalten ein Movie-Objekt (oder eine Liste von Objekten bei `Index`), und übergeben das Objekt (Modell) an die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="b5272-172">They get a movie object (or list of objects, in the case of `Index`), and pass the object (model) to the view.</span></span> <span data-ttu-id="b5272-173">Die `Create`-Methode übergibt ein leeres Movie-Objekt an die Ansicht `Create`.</span><span class="sxs-lookup"><span data-stu-id="b5272-173">The `Create` method passes an empty movie object to the `Create` view.</span></span> <span data-ttu-id="b5272-174">Alle Methoden, die Daten erstellen, bearbeiten, löschen oder in beliebiger Weise ändern, nutzen dazu die `[HttpPost]`-Überladung der Methode.</span><span class="sxs-lookup"><span data-stu-id="b5272-174">All the methods that create, edit, delete, or otherwise modify data do so in the `[HttpPost]` overload of the method.</span></span> <span data-ttu-id="b5272-175">Das Ändern von Daten in einer `HTTP GET`-Methode ist ein Sicherheitsrisiko.</span><span class="sxs-lookup"><span data-stu-id="b5272-175">Modifying data in an `HTTP GET` method is a security risk.</span></span> <span data-ttu-id="b5272-176">Das Ändern von Daten in einer `HTTP GET`-Methode verstößt auch gegen bewährte HTTP-Methoden und das architektonische [REST](http://rest.elkstein.org/)-Muster, das angibt, dass die GET-Anforderungen nicht den Status Ihrer Anwendung ändern sollten.</span><span class="sxs-lookup"><span data-stu-id="b5272-176">Modifying data in an `HTTP GET` method also violates HTTP best practices and the architectural [REST](http://rest.elkstein.org/) pattern, which specifies that GET requests should not change the state of your application.</span></span> <span data-ttu-id="b5272-177">Die Durchführung eines GET-Vorgangs sollte also eine sichere Operation sein, die keine Nebenwirkungen hat und die permanenten Daten nicht ändert.</span><span class="sxs-lookup"><span data-stu-id="b5272-177">In other words, performing a GET operation should be a safe operation that has no side effects and doesn't modify your persisted data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5272-178">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b5272-178">Additional resources</span></span>

* [<span data-ttu-id="b5272-179">Globalisierung und Lokalisierung</span><span class="sxs-lookup"><span data-stu-id="b5272-179">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="b5272-180">Einführung in Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="b5272-180">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="b5272-181">Erstellen von Taghilfsprogrammen</span><span class="sxs-lookup"><span data-stu-id="b5272-181">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="b5272-182">Antianforderungsfälschung</span><span class="sxs-lookup"><span data-stu-id="b5272-182">Anti-Request Forgery</span></span>](xref:security/anti-request-forgery)
* <span data-ttu-id="b5272-183">Schützen Sie Ihre Domänencontroller vor [zu vielen Angaben](http://www.asp.net/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost)</span><span class="sxs-lookup"><span data-stu-id="b5272-183">Protect your controller from [over-posting](http://www.asp.net/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost)</span></span>
* [<span data-ttu-id="b5272-184">ViewModels</span><span class="sxs-lookup"><span data-stu-id="b5272-184">ViewModels</span></span>](http://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
* [<span data-ttu-id="b5272-185">Hilfsprogramm für Formulartags</span><span class="sxs-lookup"><span data-stu-id="b5272-185">Form Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="b5272-186">Hilfsprogramm für Eingabetags</span><span class="sxs-lookup"><span data-stu-id="b5272-186">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="b5272-187">Hilfsprogramm für Bezeichnungstags</span><span class="sxs-lookup"><span data-stu-id="b5272-187">Label Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="b5272-188">Hilfsprogramm für Auswahltags</span><span class="sxs-lookup"><span data-stu-id="b5272-188">Select Tag Helper</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="b5272-189">Hilfsprogramm für Validierungstags</span><span class="sxs-lookup"><span data-stu-id="b5272-189">Validation Tag Helper</span></span>](xref:mvc/views/working-with-forms)