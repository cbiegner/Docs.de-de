---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Erweiterte Entity Framework 6-Szenarien für eine MVC-5-Web-Anwendung (12 von 12) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio...
ms.author: riande
ms.date: 12/08/2014
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 6e3de242f7cfc584f4c3d1dfa3d1948ee4d49d66
ms.sourcegitcommit: 67a0a04ebb3b21c826e5b9600bacfc897abd6a46
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2018
ms.locfileid: "42899824"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Erweiterte Entity Framework 6-Szenarien für eine MVC-5-Web-Anwendung (12 von 12)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First "und" Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Tutorial implementiert Sie die Tabelle pro Hierarchie-Vererbung. Dieses Lernprogramm enthält werden verschiedene Themen, die nützlich zu beachten, wenn Sie die Grundlagen der Entwicklung von ASP.NET Web-Anwendungen, die Entity Framework Code First verwenden hinausgehen eingeführt. Detaillierte Anweisungen führen Sie durch den Code und mithilfe von Visual Studio für die folgenden Themen:

- [Unformatierte SQL-Abfragen ausführen](#rawsql)
- [Ausführen von Abfragen ohne nachverfolgung](#notracking)
- [Untersuchen von SQL an die Datenbank gesendet](#sql)

Im Tutorial werden verschiedene Themen eingeführt, mit der kurze Einführungen, gefolgt von Links und Ressourcen für Weitere Informationen:

- [Repository und arbeitseinheitsmuster](#repo)
- [Webdienstproxy-Klassen](#proxies)
- [Automatische änderungserkennung](#changedetection)
- [Automatische Validierung](#validation)
- [EF-Tools für Visual Studio](#tools)
- [Entity Framework-Quellcode](#source)

Das Lernprogramm enthält auch die folgenden Abschnitte:

- [Zusammenfassung](#summary)
- [Bestätigungen](#acknowledgments)
- [Ein Hinweis zu VB](#vb)
- [Allgemeine Fehler und Lösungen oder problemumgehungen für diese](#errors)

Für die meisten dieser Themen arbeiten Sie mit der Seiten, die Sie bereits erstellt. Verwendung roher SQL, um massenaktualisierungen auszuführen erstellen Sie eine neue Seite, die die Anzahl der Gutschrift in Höhe von alle Kurse in der Datenbank aktualisiert wird:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Unformatierte SQL-Abfragen ausführen

Die Entity Framework Code First-API enthält Methoden, die Ihnen ermöglichen, SQL-Befehle direkt an die Datenbank übergeben. Sie haben die folgenden Optionen:

- Verwenden der [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) -Methode für Abfragen, die Entitätstypen zurückgeben. Die zurückgegebenen Objekte muss mit dem vom erwarteten Typ der `DbSet` -Objekt, und sie werden automatisch nachverfolgt durch den Datenbankkontext, wenn Sie die Überwachung deaktivieren. (Finden Sie im Abschnitt zu den ["asnotracking"](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) Methode.)
- Verwenden der [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) -Methode für Abfragen, die Typen zurückgeben, die nicht von Entitäten. Die zurückgegebenen Daten werden nicht vom Datenbankkontext nachverfolgt, auch wenn Sie diese Methode zum Abrufen von Entitätstypen verwenden.
- Verwenden der [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) für nichtabfragebefehle.

Einer der Vorteile von Entity Framework ist die Tatsache, dass vermieden wird, den Code zu eng an eine bestimmte Methode zum Speichern von Daten zu binden. Dies geschieht, indem SQL-Abfragen und -Befehle für Sie generiert werden. Somit müssen Sie sie nicht selbst schreiben. Aber es gibt außergewöhnliche Szenarios müssen Sie bestimmte SQL-Abfragen ausführen, die Sie manuell erstellt haben, und diese Methoden ermöglichen es Ihnen, solche Ausnahmen zu behandeln.

Dies trifft immer zu, wenn Sie SQL-Befehle in einer Webanwendung ausführen. Treffen Sie deshalb Vorsichtsmaßnahmen, um Ihre Website vor Angriffen durch Einschleusung von SQL-Befehlen zu schützen. Verwenden Sie dazu parametrisierte Abfragen, um sicherzustellen, dass Zeichenfolgen, die von einer Webseite übermittelt werden, nicht als SQL-Befehle interpretiert werden können. In diesem Tutorial verwenden Sie parametrisierte Abfragen beim Integrieren von Benutzereingaben in eine Abfrage.

### <a name="calling-a-query-that-returns-entities"></a>Eine Abfrage aufrufen, gibt Entitäten zurück.

Die ["DbSet"&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) Klasse stellt eine Methode, die Sie verwenden können, zum Ausführen einer Abfrage, die eine Entität des Typs zurückgibt `TEntity`. Um festzustellen, wie Sie funktioniert ändern Sie den Code in die `Details` Methode der `Department` Controller.

In *DepartmentController.cs*in die `Details` -Methode, ersetzen Sie die `db.Departments.FindAsync` Methodenaufruf mit einer `db.Departments.SqlQuery` -Methodenaufruf, wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Wählen Sie die Registerkarte **Departments** (Abteilungen) und dann **Details** für eine der Abteilungen aus. So können Sie überprüfen, ob der neue Code korrekt funktioniert.

![Fakultätsdetails](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Aufrufen von einer Abfrage, zurückgibt andere Typen von Objekten

Sie haben zuvor ein Statistikraster für Studenten für die Infoseite erstellt, das die Anzahl der Studenten für jedes Anmeldedatum zeigt. Der Code in hierfür *"HomeController.cs"* verwendet LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Angenommen Sie, Sie möchten den Code zu schreiben, der diese Daten direkt in SQL, anstatt LINQ abruft. Zu diesem Zweck müssen Sie zum Ausführen einer Abfrage, die etwas anderes als Entitätsobjekte zurückgibt, also müssen Sie verwenden die [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) Methode.

In *"HomeController.cs"*, ersetzen Sie die LINQ-Anweisung in der `About` -Methode mit einer SQL-Anweisung wie im folgenden hervorgehobenen Code gezeigt:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Führen Sie die Seite "Info". Sie zeigt die gleichen Daten wie zuvor.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Aufrufen einer Aktualisierungsabfrage

Angenommen Sie, Sie möchten, dass Administratoren der Contoso University massenänderungen in der Datenbank, z. B. das Ändern der Anzahl der Credits für jeden Kurs ausführen können. Wenn die Universität über eine große Anzahl an Kursen verfügt, wäre es ineffizient, sie alle als Entitäten abzurufen und separat zu ändern. In diesem Abschnitt implementieren Sie eine Webseite, die dem Benutzer ermöglicht, geben Sie einen Faktor, um die Anzahl der Credits für alle Kurse zu ändern, und erstellen Sie die Änderung durch Ausführen einer SQL `UPDATE` Anweisung. Die Webseite wird wie die folgende Abbildung aussehen:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

In *CourseContoller.cs*, fügen `UpdateCourseCredits` Methoden für die `HttpGet` und `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Wenn der Controller verarbeitet eine `HttpGet` -Anforderung, die nichts zurückgegeben wird, der `ViewBag.RowsAffected` Variable und die Ansicht ein leeres Textfeld und eine Schaltfläche "Senden", wie in der vorherigen Abbildung dargestellt angezeigt.

Wenn die **Update** Schaltfläche geklickt wird, die `HttpPost` -Methode aufgerufen wird, und `multiplier` hat den Wert in das Textfeld eingegeben haben. Der Code führt dann die SQL, das Kurse aktualisiert und gibt die Anzahl der betroffenen Zeilen an die Ansicht in der `ViewBag.RowsAffected` Variable. Wenn die Ansicht, die einen Wert empfängt, es zeigt die Anzahl der Zeilen, die aktualisiert werden, anstatt das Textfeld und submit-Schaltfläche in der folgenden Abbildung dargestellt:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

In *CourseController.cs*, Maustaste den `UpdateCourseCredits` Methoden, und klicken Sie dann auf **Ansicht hinzufügen**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

In *Views\Course\UpdateCourseCredits.cshtml*, ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Führen Sie die `UpdateCourseCredits`-Methode aus, indem Sie die Registerkarte **Courses** (Kurse) auswählen, und dann „/UpdateCourseCredits“ am Ende der URL in die Adressleiste des Browsers einfügen (z.B.: `http://localhost:50205/Course/UpdateCourseCredits`). Geben Sie eine Zahl in das Textfeld ein:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Klicken Sie auf **Aktualisieren**. Die Anzahl der betroffenen Zeilen wird angezeigt:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Klicken Sie auf **Zurück zur Liste**, um die Kursliste mit der geänderte Anzahl von Credits anzuzeigen.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Weitere Informationen zu unformatierten SQL-Abfragen finden Sie unter [unformatierte SQL-Abfragen](https://msdn.microsoft.com/data/jj592907) auf MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Abfragen ohne Nachverfolgung

Wenn ein Datenbankkontext Tabellenzeilen abruft und Entitätsobjekte erstellt, die diese darstellen, verfolgen sie standardgemäß, ob die Entitäten im Arbeitsspeicher mit dem Inhalt der Datenbank synchronisiert sind. Die Daten im Arbeitsspeicher fungieren als Cache und werden verwendet, wenn Sie eine Entität aktualisieren. Diese Zwischenspeicherung ist in Webanwendungen oft überflüssig, da Kontextinstanzen in der Regel kurzlebig sind (eine neue wird bei jeder Anforderung erstellt und gelöscht), und der Kontext, der eine Entität liest, wird in der Regel gelöscht, bevor diese Entität erneut verwendet wird.

Sie können die nachverfolgung von Entitätsobjekten im Arbeitsspeicher deaktivieren, indem Sie mit der ["asnotracking"](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) Methode. Typische Szenarios, in denen Sie das möglicherweise tun wollen, umfassen Folgendes:

- Eine Abfrage ruft solche eine große Menge von Daten, die durch das Ausschalten Überwachung deutlich die Leistung steigern können.
- Sie möchten eine Entität anfügen, um diese zu aktualisieren, aber Sie zuvor die gleiche Entität für einen anderen Zweck abgerufen. Da diese Entität bereits vom Datenbankkontext nachverfolgt wird, können Sie die zu ändernde Entität nicht anfügen. Eine Möglichkeit, diese Situation ist die Verwendung der `AsNoTracking` Option mit die vorherige Abfrage.

Ein Beispiel für die Verwendung der ["asnotracking"](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) -Methode finden Sie unter [die frühere Version dieses Tutorials](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Diese Version des Lernprogramms nicht das Flag "geändert" für eine Entität Modell Binder erstellt, in der Edit-Methode festgelegt, damit es nicht benötigt `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Untersuchen von SQL an die Datenbank gesendet

Manchmal ist es hilfreich, die tatsächlichen SQL-Abfragen anzuzeigen, die an die Datenbank gesendet werden. In einem früheren Tutorial haben gesehen, wie dies im Code der Interceptor; Jetzt sehen Sie einige Möglichkeiten, dies ohne Interceptor-Code zu schreiben. Dies können Sie ausprobieren, Sie eine einfache Abfrage betrachten und sehen Sie sich, was geschieht, wenn Sie diese mittels Eager Load laden, Filtern und Sortieren von Optionen hinzufügen.

In *Controller/CourseController*, ersetzen Sie die `Index` -Methode mit den folgenden Code, um eager Loading vorübergehend zu beenden:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Jetzt legen einen Haltepunkt auf der `return` Anweisung (mit dem Cursor in dieser Zeile F9). Drücken Sie F5, um das Projekt im Debugmodus ausgeführt, und wählen die Indexseite des Kurses. Wenn der Code den Haltepunkt erreicht, untersuchen die `sql` Variable. Daraufhin wird die Abfrage, die mit SQL Server gesendet wird. Es ist eine einfache `Select` Anweisung.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Klicken Sie auf das Lupensymbol, um die Abfrage in finden Sie unter den **Text-Schnellansicht**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Jetzt fügen Sie eine Dropdown-Liste auf die Indexseite, sodass Benutzer für eine bestimmte Abteilung filtern können. Sie müssen die Kurse nach Titel sortieren, und geben Sie eager Loading für die `Department` Navigationseigenschaft.

In *CourseController.cs*, ersetzen Sie die `Index` Methode durch den folgenden Code:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Stellen Sie den Haltepunkt auf der `return` Anweisung.

Die Methode empfängt den ausgewählten Wert aus der Dropdown-Liste in der `SelectedDepartment` Parameter. Wenn nichts ausgewählt ist, wird dieser Parameter null sein.

Ein `SelectList` Auflistung mit allen Abteilungen für die Dropdown-Liste an die Ansicht übergeben wird. Die Parameter zu übergeben, um die `SelectList` Konstruktor angegeben wird, den Feldnamen der Wert, der Feldname für Text und das ausgewählte Element.

Für die `Get` Methode der `Course` Repository, die der Code gibt an, einen Filterausdruck, Sortierreihenfolge und Eager loading für die `Department` Navigationseigenschaft. Gibt zurück, der Filterausdruck immer `true` Wenn nichts in der Dropdown-Liste ausgewählt ist (d. h. `SelectedDepartment` null ist).

In *Views\Course\Index.cshtml*, unmittelbar vor dem öffnenden `table` markieren, fügen Sie den folgenden Code zum Erstellen der Dropdown Liste und eine Schaltfläche "Senden" hinzu:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Der Haltepunkt festgelegt, und führen Sie die Indexseite des Kurses. Durchlaufen Sie das erste Mal, dass der Code einen Haltepunkt trifft, damit die Seite im Browser angezeigt wird. Wählen Sie eine Abteilung aus der Dropdown-Liste, und klicken Sie auf **Filter**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Dieses Mal wird der erste Haltepunkt für die Abteilungen-Abfrage für das Dropdown-Liste sein. Zu überspringen, und zeigen die `query` Variablen das nächste Mal im Code erreicht den Breakpoint um zu sehen, was die `Course` Abfrage sieht jetzt wie. Sie sehen etwa wie folgt:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Sehen Sie, dass die Abfrage ist eine `JOIN` Abfrage, die geladen `Department` Daten zusammen mit den `Course` Daten, und es enthält eine `WHERE` Klausel.

Entfernen Sie die `var sql = courses.ToString()` Zeile.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Repository- und Arbeitseinheitsmuster

Viele Entwickler schreiben Code, um das Repository- und Arbeitseinheitsmuster als Wrapper um den Code zu implementieren, der mit dem Entity Framework arbeitet. Diese Muster sollen eine Abstraktionsebene zwischen der Datenzugriffsebene und den Geschäftslogikebene einer Anwendung erstellen. Die Implementierung dieser Muster unterstützt die Isolation Ihrer Anwendung vor Änderungen im Datenspeicher und kann automatisierte Komponententests oder eine testgesteuerte Entwicklung (Test-Driven Development, TDD) erleichtern. Schreiben von zusätzlichem Code zum Implementieren dieser Muster ist jedoch nicht immer die beste Wahl für Anwendungen, die EF, verschiedene Ursachen haben:

- Die EF-Kontextklasse isoliert Ihren Code selbst vor datenspeicherspezifischem Code.
- Die EF-Kontextklasse kann als Arbeitseinheitsklasse für Updates der Datenbank fungieren, die Sie mithilfe von EF ausführen.
- Funktionen in Entity Framework 6 erleichtern die testgesteuerte Entwicklung Implementierung ohne repositorycode zu schreiben.

Weitere Informationen zur Implementierung der Repository- und arbeitseinheitsmuster arbeitseinheitenmustern finden Sie unter [der Version von Entity Framework 5 dieser tutorialreihe](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Informationen zu Möglichkeiten zum Implementieren von TDD in Entity Framework 6 finden Sie unter den folgenden Ressourcen:

- [Wie können EF6 Antwortsimulation "dbsets" leichter](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Testen mit der ein Mockframework](https://msdn.microsoft.com/data/dn314429)
- [Tests mit Ihren eigenen Testdoubles](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Webdienstproxy-Klassen

Wenn Entity Framework erstellt Instanzen der Entität (z. B., wenn Sie eine Abfrage ausführen), wird häufig als Instanzen von einem dynamisch generierten abgeleiteten Typ, der als Proxy für die Entität fungiert. Beispielsweise finden Sie unter den folgenden zwei Bildern im Debugger. In der ersten Abbildung wird angezeigt, die die `student` Variable ist der erwartete `Student` Geben Sie sofort, nachdem Sie die Entität instanziieren. Nachdem EF zum Lesen von einer Entität "Student" aus der Datenbank verwendet wurde, sehen Sie in der zweiten Abbildung ist die Proxy-Klasse.

![Bevor Sie Proxy-Klasse](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Nach dem Proxy-Klasse](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Diese Proxyklasse überschreibt einige virtuelle Eigenschaften der Entität zum Einfügen von Hooks für Aktionen automatisch ausgeführt, wenn die Eigenschaft zugegriffen wird. Eine Funktion, der für diesen Mechanismus verwendet wird, ist lazy Loading.

In den meisten Fällen müssen Sie nicht diese Verwendung von Proxys kennen, aber es gibt jedoch Ausnahmen:

- In einigen Fällen empfiehlt es sich um zu verhindern, dass das Entity Framework-Proxyinstanzen erstellen. Beim Serialisieren von Entitäten sind z. B. Sie in der Regel die POCO-Klassen, die Webdienstproxy-Klassen. Eine Möglichkeit zur Vermeidung von Problemen der Serialisierung wird zum Serialisieren von datentransferobjekte (DTOs) anstelle von Entitätsobjekten, siehe die [mithilfe des Web-API mit Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) Tutorial. Eine weitere Möglichkeit ist [deaktivieren Sie die Proxyerstellung](https://msdn.microsoft.com/data/jj592886.aspx).
- Beim Instanziieren einer Entität Klasse mithilfe der `new` -Operator, erhalten Sie keine Proxyinstanz. Dies bedeutet, dass Sie keine Funktionen erhalten, wie z. B. verzögertes Laden und die automatische änderungsnachverfolgung. Dies ist in der Regel gut. im Allgemeinen nicht erforderlich verzögerten Laden, da Sie eine neue Entität erstellen, die nicht in der Datenbank, und die änderungsnachverfolgung, wenn Sie explizit die Entität als gekennzeichnet sind in der Regel nicht erforderlich, `Added`. Jedoch wenn lazy Loading ist erforderlich, und die änderungsnachverfolgung benötigen, können Sie erstellen neue Instanzen der Entität mit Proxys mit den [erstellen](https://msdn.microsoft.com/library/gg679504.aspx) Methode der `DbSet` Klasse.
- Sie möchten einen tatsächlichen Entitätstyp von einem Proxytyp zu erhalten. Können Sie die [GetObjectType hat](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) Methode der `ObjectContext` Klasse zum Abrufen des tatsächlichen Entitätstyp, der eine Instanz der Proxy-Typ.

Weitere Informationen finden Sie unter [arbeiten mit Proxys in](https://msdn.microsoft.com/data/JJ592886.aspx) auf MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Automatische Änderungserkennung

Entity Framework bestimmt wie eine Entität geändert wurde (und welche Updates an die Datenbank gesendet werden müssen), indem die aktuellen Werte einer Entität mit den ursprünglichen Werten verglichen werden. Die ursprünglichen Werte werden gespeichert, wenn die Entität abgefragt oder angefügt wird. Einige der Methoden, die automatisch eine Änderungserkennung durchführen, sind die folgenden:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Wenn Sie eine große Anzahl von Entitäten überwachen und Sie eine dieser Methoden oft in einer Schleife aufrufen, erhalten Sie möglicherweise erhebliche Leistungssteigerungen durch vorübergehendes Deaktivieren der automatischen änderungserkennung mithilfe der [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) Eigenschaft. Weitere Informationen finden Sie unter [automatisch erkennen von Änderungen](https://msdn.microsoft.com/data/jj556205) auf MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Automatische Validierung

Beim Aufrufen der `SaveChanges` -Methode, wird standardmäßig das Entity Framework überprüft die Daten in der alle Eigenschaften aller geänderten Entitäten vor dem Aktualisieren der Datenbank. Wenn Sie eine große Anzahl von Entitäten aktualisiert haben und Sie haben bereits überprüft die Daten dieser Arbeit ist nicht erforderlich und Sie, die das Speichern vornehmen können werden die Änderungen weniger Zeit durch vorübergehendes Deaktivieren der Validierung. Sie können hierzu die [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) Eigenschaft. Weitere Informationen finden Sie unter [Überprüfung](https://msdn.microsoft.com/data/gg193959) auf MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework Powertools

[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) ist ein Visual Studio-add-in, der verwendet wurde, erstellen Sie Modelldiagramme Daten in diesen Lernprogrammen vorgestellten. Die Tools können auch andere Funktion durchführen, wie z.B. das Generieren von Entitätsklassen auf Grundlage der Tabellen in einer vorhandenen Datenbank, damit Sie die Datenbank mit Code First verwenden können. Nachdem Sie die Tools installiert haben, können Sie einige zusätzlichen Optionen in Kontextmenüs angezeigt. Z. B. Wenn Sie mit der rechten Maustaste in der Context-Klasse **Projektmappen-Explorer**, erhalten Sie eine Option aus, um ein Diagramm generieren. Wenn Sie Code First verwenden, das Datenmodell in das Diagramm kann nicht geändert werden, aber Sie können Dinge verschieben, um ihn verständlicher zu machen.

![EF im Kontextmenü](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF-Diagramm](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Entity Framework-Quellcode

Der Quellcode für Entity Framework 6 finden Sie unter [GitHub](https://github.com/aspnet/EntityFramework6). Melden Sie Fehler, und Sie können Ihre eigenen Verbesserungen auf den Quellcode von EF mitwirken.

Obwohl der Quellcode geöffnet ist, wird Entity Framework als ein Produkt von Microsoft vollständig unterstützt. Das Microsoft Entity Framework-Team überprüft, welche Beiträge akzeptiert werden. Es testet alle Codeänderungen, um die Qualität jedes Release zu garantieren.

<a id="summary"></a>
## <a name="summary"></a>Zusammenfassung

Dies schließt diese tutorialreihe zur Verwendung von Entity Framework in einer ASP.NET MVC-Anwendung. Weitere Informationen zum Arbeiten mit Daten, die mithilfe von Entity Framework finden Sie unter den [EF-Dokumentationsseite auf MSDN](https://msdn.microsoft.com/data/ee712907) und [ASP.NET-Datenzugriff – empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

Weitere Informationen dazu, wie Sie Ihre Webanwendung bereitstellen, nachdem Sie sie erstellt haben, finden Sie unter [ASP.NET-webbereitstellung – empfohlene Ressourcen](../../../../whitepapers/aspnet-web-deployment-content-map.md) in der MSDN Library.

Weitere Informationen zu anderen Themen im Zusammenhang mit MVC, wie z. B. Authentifizierung und Autorisierung, finden Sie unter den [ASP.NET MVC – empfohlene Ressourcen](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Danksagungen

- Tom Dykstra die ursprüngliche Version des in diesem Tutorial geschrieben, Co-Autor EF 5-Aktualisierung und das Update von EF 6 geschrieben. Tom ist leitender Redakteur für Programmierung auf der Microsoft-Webplattform und Tools-Team-Inhalte.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter- [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) hat den Großteil der Arbeit aktualisieren das Tutorial für EF 5 und MVC 4 und Co-Autor des EF 6-Updates. Rick ist leitender Redakteur bei Microsoft mit Schwerpunkt auf Azure und MVC.
- [Rowan Miller](http://www.romiller.com) und anderen Mitgliedern des Entity Framework-Teams mit codereviews telefonischen und Half, viele Probleme mit Migrationen zu debuggen, die aufgetreten waren, während wir das Tutorial für EF 5 und EF 6 aktualisiert wurden.

<a id="vb"></a>
## <a name="vb"></a>VB

Wenn Sie das Tutorial für EF 4.1 ursprünglich erstellt wurde, bereitgestellt wurde c# und VB-Versionen des Projekts Abgeschlossene Downloads. Aufgrund der Zeit-Einschränkungen und andere Prioritäten haben wir nicht, die für diese Version getan. Wenn Sie beim Erstellen eines VB-Projekts mithilfe dieser Lernprogramme und wäre, die für andere Benutzer freigeben, geben Sie uns möchte.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Allgemeine Fehler und Lösungen oder problemumgehungen für diese

### <a name="cannot-createshadow-copy"></a>Kopieren kann nicht erstellt/Shadow werden

Fehlermeldung:

> Kopieren kann nicht erstellt/Shadow "&lt;Filename&gt;" Wenn die Datei bereits vorhanden ist.


Lösung

Warten Sie einige Sekunden, und aktualisieren Sie die Seite.

### <a name="update-database-not-recognized"></a>Update-Database nicht erkannt

Fehlermeldung (aus der `Update-Database` Befehl in der PMC):

> Der Begriff "Update-Database" wird nicht als Namen für ein Cmdlet, Funktion, Skriptdatei oder eines ausführbaren Programms erkannt. Überprüfen Sie die Schreibweise des Namens, oder wenn ein Pfad enthalten ist, stellen Sie sicher, dass der Pfad richtig ist, und versuchen Sie es erneut.


Lösung

Beenden Sie Visual Studio. Projekt erneut öffnen, und versuchen Sie es noch mal.

### <a name="validation-failed"></a>Fehler bei der Validierung

Fehlermeldung (aus der `Update-Database` Befehl in der PMC):

> Fehler bei der Überprüfung für eine oder mehrere Entitäten. Finden Sie unter 'EntityValidationErrors'-Eigenschaft für die weitere Details.


Lösung

Eine Ursache für dieses Problem sind Überprüfungsfehler bei den `Seed` Methode ausgeführt wird. Finden Sie unter [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Tipps zum Debuggen der `Seed` Methode.

### <a name="http-50019-error"></a>HTTP 500.19 Fehler

Fehlermeldung:

> HTTP-Fehler 500.19 - Interner Serverfehler  
> Die angeforderte Seite kann nicht zugegriffen werden, weil die zugehörigen Konfigurationsdaten für die Seite ungültig ist.


Lösung

Eine Möglichkeit, die Sie diese Fehlermeldung ist über das Vorhandensein mehrerer Kopien der Lösung, jeweils die gleiche Portnummer verwenden. In der Regel können Sie dieses Problem beheben, indem Sie alle Instanzen von Visual Studio beenden und anschließendes Neustarten das Projekt, dem Sie gerade arbeiten. Wenn dies nicht funktioniert, versuchen Sie, die Portnummer zu ändern. Klicken Sie mit der rechten Maustaste auf die Projektdatei, und klicken Sie dann auf Eigenschaften. Wählen Sie die **Web** Registerkarte aus, und ändern Sie dann die Portnummer in das **Projekt-Url** Textfeld.

### <a name="error-locating-sql-server-instance"></a>Fehler beim Bestimmen der SQL Server-Instanz

Fehlermeldung:

> Ein netzwerkbezogener oder instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server. Der Server wurde nicht gefunden oder es konnte nicht auf ihn zugegriffen werden. Stellen Sie sicher, dass der Instanzname richtig und SQL Server so konfiguriert ist, das Remoteverbindungen zulässig sind. (Anbieter: SQL-Netzwerkschnittstellen, Fehler: 26 – Fehler beim Suchen des angegebenen Servers/der angegebenen Instanz.)


Lösung

Überprüfen Sie die Verbindungszeichenfolge. Wenn Sie die Datenbank manuell gelöscht haben, ändern Sie den Namen der Datenbank in der Konstruktionszeichenfolge.

> [!div class="step-by-step"]
> [Vorherige](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
