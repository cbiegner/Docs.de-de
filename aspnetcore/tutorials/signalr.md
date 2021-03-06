---
title: 'Tutorial: Erste Schritte mit SignalR unter ASP.NET Core'
author: tdykstra
description: In diesem Tutorial erstellen Sie eine Chat-App, die SignalR für ASP.NET Core verwendet.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6f93d6dc664f68425ef0fa0d02f9011e4875bc33
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402132"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Tutorial: Erste Schritte mit SignalR unter ASP.NET Core

In diesem Tutorial werden die Grundlagen zur Erstellung einer Echtzeit-App mit SignalR beschrieben. Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen einer Web-App, die SignalR für ASP.NET Core verwendet
> * Erstellen eines SignalR-Hubs auf dem Server
> * Herstellen einer Verbindung zwischen JavaScript-Clients und dem SignalR-Hub
> * Verwenden des Hubs zum Senden von Nachrichten von jedem Client an alle verbundenen Clients

Am Ende verfügen Sie über eine funktionierende Chat-App:

![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Erforderliche Komponenten

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Version 15.8 von Visual Studio 2017 oder höher](https://www.visualstudio.com/downloads/) mit der Workload **ASP.NET und Webentwicklung**
* [.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all)
* [C# für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* [Visual Studio für Mac Version 7.5.4 oder höher](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.1 oder höher](https://www.microsoft.com/net/download/all) (in der Visual Studio-Installation enthalten)

---

## <a name="create-the-project"></a>Erstellen eines Projekts

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Klicken Sie im Menü auf **Datei > Neues Projekt**.

* Wählen Sie im Dialogfeld **Neues Projekt** **Installiert > Visual C# > Web > ASP.NET Core-Webanwendung** aus. Geben Sie als Projektname *SignalRChat* ein.

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* Klicken Sie auf **Webanwendung**, um ein Projekt zu erstellen, das Razor Pages verwendet.

* Wählen Sie ein Zielframework von **.NET Core** aus, und klicken Sie anschließend auf **ASP.NET Core 2.1** und **OK**.

  ![Dialogfeld „Neues Projekt“ in Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Öffnen Sie einen Ordner, den Sie für ein neues Projekt verwenden können.

* Führen Sie über das **integrierte Terminal** den folgenden Befehl aus:

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* Klicken Sie im Menü auf **Datei > Neue Projektmappe**.

* Klicken Sie auf **.NET Core > App > ASP.NET Core-Web-App** (Wählen Sie nicht **ASP.NET Core-Web-App (MVC)** aus).

* Klicken Sie auf **Weiter**.

* Nennen Sie das Projekt *SignalRChat*, und wählen Sie dann **Erstellen** aus.

---

## <a name="add-the-signalr-client-library"></a>Hinzufügen der SignalR-Clientbibliothek

Die SignalR-Serverbibliothek ist im [Metapaket Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten. Die JavaScript-Clientbibliothek ist nicht automatisch im Projekt enthalten. In diesem Tutorial verwenden Sie den [Bibliotheks-Manager (LibMan)](xref:client-side/libman/index), um die Clientbibliothek von *unpkg* abzurufen. [unpkg](https://unpkg.com/#/) ist ein [CDN](https://wikipedia.org/wiki/Content_delivery_network) (Content Delivery Network), mit dem Sie alles bereitstellen können, was im [npm (Node.js-Paket-Manager)](https://www.npmjs.com/get-npm) zu finden ist.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Client-Side Library** (Clientseitige Bibliothek) aus.

* Wählen Sie **unpkg** im Dialogfeld **Add Client-Side Library** (Clientseitige Bibliothek hinzufügen) als **Anbieter** aus. 

* Geben Sie _@aspnet/signalr@1_ für die **Bibliothek** ein, und wählen Sie die neuste Version aus, die keine Vorschauversion ist.

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Bibliothek](signalr/_static/libman1.png)

* Klicken Sie auf **Choose specific files** (Spezifische Dateien auswählen), erweitern Sie den Ordner *dist/browser*, und wählen Sie *signalr.js* und *signalr.min.js* aus.

* Legen Sie *wwwroot/lib/signalr/* als **Zielspeicherort** fest, und klicken Sie auf **Installieren**.

  ![Dialogfeld „Clientseitige Bibliothek hinzufügen“: Auswählen der Dateien und des Zielspeicherorts](signalr/_static/libman2.png)

  Der Ordner *wwwroot/lib/signalr/* wird von [LibMan](xref:client-side/libman/index) erstellt, und die ausgewählten Dateien werden in ihn hineinkopiert.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Führen Sie den folgenden Befehl über das **integrierte Terminal** aus, um LibMan zu installieren.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Navigieren Sie zum Projektordner (der die Datei *SignalRChat.csproj* enthält).

* Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen. Es kann einige Sekunden dauern, bis die Ausgabe angezeigt wird.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Die Parameter legen folgende Optionen fest:
  * Die Verwendung des Anbieters „unpkg“.
  * Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.
  * Das Kopieren von ausschließlich den angegebenen Dateien.

  Die Ausgabe sieht wie folgt aus:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* Führen Sie den folgenden Befehl über das **Terminal** aus, um LibMan zu installieren.

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* Navigieren Sie zum Projektordner (der die Datei *SignalRChat.csproj* enthält).

* Führen Sie den folgenden Befehl aus, um die SignalR-Clientbibliothek mit LibMan abzurufen.

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  Die Parameter legen folgende Optionen fest:
  * Die Verwendung des Anbieters „unpkg“.
  * Das Kopieren der Dateien in den Zielordner *wwwroot/lib/signalr*.
  * Das Kopieren von ausschließlich den angegebenen Dateien.

  Die Ausgabe sieht wie folgt aus:

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-the-signalr-hub"></a>Erstellen des SignalR-Hubs

Ein [Hub](xref:signalr/hubs) ist eine Klasse, die als grundlegende Pipeline verwendet wird, mit der Client/Server-Kommunikation verarbeitet wird.

* Erstellen Sie im SignalRChat-Projektordner einen *Hubs*-Ordner.

* Erstellen Sie im *Hubs*-Ordner eine *ChatHub.cs*-Datei mit folgendem Code:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  Die `ChatHub`-Klasse erbt von der [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub)-Klasse von SignalR. Die `Hub`-Klasse verwaltet Verbindungen, Gruppen und Messaging.

  Die `SendMessage`-Methode kann von jedem verbundenen Client aufgerufen werden. Die empfangenen Nachrichten werden an alle Clients gesendet. SignalR-Code ist asynchron, damit die maximale Skalierbarkeit gewährleistet werden kann.

## <a name="configure-the-project-to-use-signalr"></a>Konfigurieren des Projekts zur Verwendung von SignalR

Der SignalR-Server muss zunächst konfiguriert werden, um Anforderungen an SignalR zu senden.

* Fügen Sie der *Startup.cs*-Datei den folgenden hervorgehobenen Code zu.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Durch diese Änderungen wird SignalR zum [Dependency Injection](xref:fundamentals/dependency-injection)-System sowie der [Middlewarepipeline](xref:fundamentals/middleware/index) hinzugefügt.

## <a name="create-the-signalr-client-code"></a>Erstellen des SignalR-Clientcodes

* Ersetzen Sie den Inhalt in *Pages\Index.cshtml* durch den folgenden Code:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Der vorangehende Code:

  * erstellt Textfelder für den Namen und den Nachrichtentext sowie eine Senden-Schaltfläche
  * erstellt eine Liste mit `id="messagesList"` zum Anzeigen von Nachrichten, die vom SignalR-Hub empfangen werden
  * schließt Skriptverweise sowie den *chat.js*-Anwendungscode, den Sie im folgenden Schritt erstellen, in SignalR ein

* Erstellen Sie im *wwwroot/js*-Ordner eine *chat.js*-Datei mit folgendem Code:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Der vorangehende Code:

  * erstellt und startet eine Verbindung
  * fügt einen Handler zur Senden-Schaltfläche hinzu, der Nachrichten an den Hub sendet
  * fügt einen Handler zum Verbindungsobjekt hinzu, der Nachrichten vom Hub empfängt und diese der Liste hinzufügt

## <a name="run-the-app"></a>Ausführen der App

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Drücken Sie **STRG+F5**, um die App ohne Debugging zu starten.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* Wählen Sie im Menü **Ausführen > Starten ohne Debuggen** aus.

---

* Kopieren Sie die URL aus der Adressleiste, öffnen Sie eine neue Browserinstanz oder Registerkarte, und fügen Sie die URL in die Adressleiste ein.

* Geben Sie in einem der beiden Browser einen Namen und eine Nachricht ein, und klicken sie auf **Send** (Senden).

  Der Name und die Nachricht werden sofort auf beiden Seiten angezeigt.

  ![SignalR-Beispiel-App](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Wenn die App nicht funktioniert, öffnen Sie die Browser-Entwicklungstools (F12), und wechseln Sie zur Konsole. Möglicherweise werden Fehler in Bezug auf Ihren HTML- und JavaScript-Code angezeigt. Nehmen wir an, dass Sie z.B. *signalr.js* in einen anderen Ordner als vorgeschrieben platziert haben. In diesem Fall funktioniert der Verweis auf diese Datei nicht, und Ihnen wird der Fehler 404 in der Konsole angezeigt.
> ![Fehler „signalr.js nicht gefunden“](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie möchten, dass Clients eine Verbindung mit einer SignalR-App von unterschiedlichen Domänen herstellt, müssen Sie die Ressourcenfreigabe zwischen verschiedenen Ursprüngen aktivieren (Cross-Origin Resource Sharing, CORS). Weiter Informationen finden Sie unter [Cross-origin resource sharing (Ressourcenfreigabe zwischen verschiedenen Ursprüngen)](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

Weitere Informationen zu SignalR, Hubs sowie JavaScript-Clients finden Sie in folgenden Artikeln:

* [Introduction to SignalR for ASP.NET Core (Einführung in SignalR für ASP.NET Core)](xref:signalr/introduction)
* [Verwenden von Hubs in SignalR für ASP.NET Core](xref:signalr/hubs)
* [ASP.NET Core SignalR JavaScript client (JavaScript-Client in SignalR für ASP.NET Core)](xref:signalr/javascript-client)
