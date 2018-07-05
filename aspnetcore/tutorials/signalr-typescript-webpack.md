---
title: Verwenden von ASP.NET Core SignalR mit TypeScript und Webpack
author: ssougnez
description: In diesem Tutorial konfigurieren Sie Webpack zum Bündeln und Erstellen einer ASP.NET Core SignalR-Web-App, deren Client in TypeScript geschrieben ist.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: e4b00fa61ea0becba7d678a0b7c94d2d30f06740
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126457"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="dea65-103">Verwenden von ASP.NET Core SignalR mit TypeScript und Webpack</span><span class="sxs-lookup"><span data-stu-id="dea65-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="dea65-104">Von [Sébastien Sougnez](https://twitter.com/ssougnez) und [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="dea65-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="dea65-105">Mit [Webpack](https://webpack.js.org/) können Entwickler clientseitige Ressourcen einer Web-App bündeln und erstellen.</span><span class="sxs-lookup"><span data-stu-id="dea65-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="dea65-106">In diesem Tutorial wird veranschaulicht, wie Webpack in einer ASP.NET Core SignalR-Web-App verwendet wird, deren Client in [TypeScript](https://www.typescriptlang.org/) geschrieben ist.</span><span class="sxs-lookup"><span data-stu-id="dea65-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="dea65-107">In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="dea65-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dea65-108">Aufbauen einer einfachen ASP.NET Core SignalR-App</span><span class="sxs-lookup"><span data-stu-id="dea65-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="dea65-109">Konfigurieren des SignalR-TypeScript-Clients</span><span class="sxs-lookup"><span data-stu-id="dea65-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="dea65-110">Konfigurieren einer Buildpipeline mithilfe von Webpack</span><span class="sxs-lookup"><span data-stu-id="dea65-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="dea65-111">Konfigurieren des SignalR-Servers</span><span class="sxs-lookup"><span data-stu-id="dea65-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="dea65-112">Aktivieren der Kommunikation zwischen Client und Server</span><span class="sxs-lookup"><span data-stu-id="dea65-112">Enable communication between client and server</span></span>

<span data-ttu-id="dea65-113">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dea65-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dea65-114">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="dea65-114">Prerequisites</span></span>

<span data-ttu-id="dea65-115">Installieren Sie die folgenden Softwarekomponenten:</span><span class="sxs-lookup"><span data-stu-id="dea65-115">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dea65-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dea65-116">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="dea65-117">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="dea65-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="dea65-118">[Node.js](https://nodejs.org/) mit [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="dea65-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>
* <span data-ttu-id="dea65-119">Version 15.7 oder höher von [Visual Studio 2017](https://www.visualstudio.com/downloads/) mit der Workload für **ASP.NET und Webentwicklung**</span><span class="sxs-lookup"><span data-stu-id="dea65-119">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dea65-120">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="dea65-120">.NET Core CLI</span></span>](#tab/netcore-cli)

* [<span data-ttu-id="dea65-121">.NET Core SDK 2.1 oder höher</span><span class="sxs-lookup"><span data-stu-id="dea65-121">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="dea65-122">[Node.js](https://nodejs.org/) mit [NPM](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="dea65-122">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="dea65-123">Erstellen einer ASP.NET Core-Web-App</span><span class="sxs-lookup"><span data-stu-id="dea65-123">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dea65-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dea65-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dea65-125">Konfigurieren Sie Visual Studio, damit in der Umgebungsvariable *PATH* nach NPM gesucht wird.</span><span class="sxs-lookup"><span data-stu-id="dea65-125">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="dea65-126">Visual Studio verwendet standardmäßig die Version von NPM, die sich im Installationsverzeichnis befindet.</span><span class="sxs-lookup"><span data-stu-id="dea65-126">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="dea65-127">Führen Sie die folgenden Anweisungen in Visual Studio aus:</span><span class="sxs-lookup"><span data-stu-id="dea65-127">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="dea65-128">Navigieren Sie zu **Extras** > **Optionen** > **Projekte und Projektmappen** > **Webpaketverwaltung** > **Externe Webtools**.</span><span class="sxs-lookup"><span data-stu-id="dea65-128">Navigate to **Tools** > **Options** > **Projects and solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="dea65-129">Wählen Sie in der Liste den Eintrag *$(PATH)* aus.</span><span class="sxs-lookup"><span data-stu-id="dea65-129">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="dea65-130">Klicken Sie auf den Pfeil nach oben, um den Eintrag auf die zweite Position in der Liste zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="dea65-130">Click the up arrow to move the entry to the second position in the list.</span></span> <span data-ttu-id="dea65-131">Übrigens bezieht sich der erste Eintrag auf die lokalen Pakete des Projekts.</span><span class="sxs-lookup"><span data-stu-id="dea65-131">As an aside, the first entry refers to the project's local packages.</span></span>

    ![Visual Studio-Konfiguration](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="dea65-133">Die Konfiguration von Visual Studio ist abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="dea65-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="dea65-134">Jetzt ist es an der Zeit, das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="dea65-134">It's time to create the project.</span></span>

1. <span data-ttu-id="dea65-135">Verwenden Sie die Menüoption **Datei** > **Neu** > **Projekt**, und wählen Sie die Vorlage **ASP.NET Core-Web-App** aus.</span><span class="sxs-lookup"><span data-stu-id="dea65-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="dea65-136">Benennen Sie das Projekt *SignalRWebPack*, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="dea65-136">Name the project *SignalRWebPack*, and click the **OK** button.</span></span>
1. <span data-ttu-id="dea65-137">Wählen Sie in der Dropdownliste für das Zielframework *.NET Core* und in der Dropdownliste zur Auswahl des Frameworks *ASP.NET Core 2.1* aus.</span><span class="sxs-lookup"><span data-stu-id="dea65-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.1* from the framework selector drop-down.</span></span> <span data-ttu-id="dea65-138">Wählen Sie die **leere** Vorlage aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="dea65-138">Select the **Empty** template, and click the **OK** button.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dea65-139">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="dea65-139">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="dea65-140">Führen Sie den folgenden Befehl aus dem **integrierten Terminal** aus:</span><span class="sxs-lookup"><span data-stu-id="dea65-140">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="dea65-141">Eine leere ASP.NET Core-Web-App für .NET Core wird in einem *SignalRWebPack*-Verzeichnis erstellt.</span><span class="sxs-lookup"><span data-stu-id="dea65-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="dea65-142">Konfigurieren von Webpack und TypeScript</span><span class="sxs-lookup"><span data-stu-id="dea65-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="dea65-143">In den folgenden Schritten wird die Konvertierung von TypeScript zu JavaScript und die Bündelung clientseitiger Ressourcen konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="dea65-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="dea65-144">Führen Sie den folgenden Befehl im Projektstamm aus, um eine *package.json*-Datei zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="dea65-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="dea65-145">Fügen Sie der *package.json*-Datei die hervorgehobene Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="dea65-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="dea65-146">Durch Festlegen der Eigenschaft `private` auf `true` werden Warnungen bei der Paketinstallation im nächsten Schritt verhindert.</span><span class="sxs-lookup"><span data-stu-id="dea65-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="dea65-147">Installieren Sie die erforderlichen NPM-Pakete.</span><span class="sxs-lookup"><span data-stu-id="dea65-147">Install the required npm packages.</span></span> <span data-ttu-id="dea65-148">Führen Sie den folgenden Befehl auf der Ebene des Projektstamms aus:</span><span class="sxs-lookup"><span data-stu-id="dea65-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="dea65-149">Beachten Sie folgende Informationen:</span><span class="sxs-lookup"><span data-stu-id="dea65-149">Some command details to note:</span></span>

    * <span data-ttu-id="dea65-150">Auf das `@`-Zeichen folgt bei jedem Paketnamen eine Versionsnummer.</span><span class="sxs-lookup"><span data-stu-id="dea65-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="dea65-151">Die spezifischen Paketversionen werden von NPM installiert.</span><span class="sxs-lookup"><span data-stu-id="dea65-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="dea65-152">Die Option `-E` deaktiviert das Standardverhalten von NPM, das Bereichsoperatoren für die [semantische Versionierung](https://semver.org/) in die *package.json*-Datei schreibt.</span><span class="sxs-lookup"><span data-stu-id="dea65-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="dea65-153">Beispielsweise wird `"webpack": "4.12.0"` anstelle von `"webpack": "^4.12.0"` verwendet.</span><span class="sxs-lookup"><span data-stu-id="dea65-153">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="dea65-154">Diese Option verhindert unbeabsichtigte Upgrades auf neuere Paketversionen.</span><span class="sxs-lookup"><span data-stu-id="dea65-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="dea65-155">Ausführliche Informationen finden Sie in der offiziellen Dokumentation zu [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="dea65-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="dea65-156">Ersetzen Sie die `scripts`-Eigenschaft der Datei *package.json* durch den folgenden Codeausschnitt:</span><span class="sxs-lookup"><span data-stu-id="dea65-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="dea65-157">Im Folgenden werden die Skripts erklärt:</span><span class="sxs-lookup"><span data-stu-id="dea65-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="dea65-158">`build`: Bündelt Ihre clientseitigen Ressourcen im Entwicklungsmodus und prüft auf Änderungen an Dateien.</span><span class="sxs-lookup"><span data-stu-id="dea65-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="dea65-159">Der Dateiwatcher generiert das Bündel jedes Mal neu, wenn eine Änderung an einer Projektdatei vorgenommen wird.</span><span class="sxs-lookup"><span data-stu-id="dea65-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="dea65-160">Die Option `mode` deaktiviert Produktionsoptimierungsvorgänge wie das „Tree Shaking“ und die „Minimierung“.</span><span class="sxs-lookup"><span data-stu-id="dea65-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="dea65-161">Verwenden Sie `build` nur in der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="dea65-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="dea65-162">`release`: Bündelt clientseitige Ressourcen im Produktionsmodus.</span><span class="sxs-lookup"><span data-stu-id="dea65-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="dea65-163">`publish`: Führt das `release`-Skript aus, um die clientseitigen Ressourcen im Produktionsmodus zu bündeln.</span><span class="sxs-lookup"><span data-stu-id="dea65-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="dea65-164">Es ruft den [publish](/dotnet/core/tools/dotnet-publish)-Befehl der .NET Core-CLI auf, um die App zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="dea65-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="dea65-165">Erstellen Sie eine Datei namens *webpack.config.js* im Projektstamm mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="dea65-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="dea65-166">Die vorangehende Datei konfiguriert die Webpack-Kompilierung.</span><span class="sxs-lookup"><span data-stu-id="dea65-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="dea65-167">Zu beachtende Konfigurationsdetails:</span><span class="sxs-lookup"><span data-stu-id="dea65-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="dea65-168">Die Eigenschaft `output` überschreibt den Standardwert von *dist*.</span><span class="sxs-lookup"><span data-stu-id="dea65-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="dea65-169">Das Bündel wird stattdessen an das Verzeichnis *wwwroot* ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="dea65-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="dea65-170">Das `resolve.extensions`-Array enthält *.js*, um den JavaScript-Code des SignalR-Clients zu importieren.</span><span class="sxs-lookup"><span data-stu-id="dea65-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="dea65-171">Erstellen Sie ein neues *src*-Verzeichnis im Projektstamm.</span><span class="sxs-lookup"><span data-stu-id="dea65-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="dea65-172">Es dient als Speicherort für clientseitige Ressourcen des Projekts.</span><span class="sxs-lookup"><span data-stu-id="dea65-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="dea65-173">Erstellen Sie *src/index.html* mit dem folgenden Inhalt.</span><span class="sxs-lookup"><span data-stu-id="dea65-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="dea65-174">Der vorangehende HTML-Code definiert die Markupbausteine der Homepage.</span><span class="sxs-lookup"><span data-stu-id="dea65-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="dea65-175">Erstellen Sie ein neues *src/css*-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="dea65-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="dea65-176">Es dient als Speicherort für die *CSS*-Dateien des Projekts.</span><span class="sxs-lookup"><span data-stu-id="dea65-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="dea65-177">Erstellen Sie *src/css/main.css* mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="dea65-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="dea65-178">Die vorangehende Datei *main.css* formatiert die App.</span><span class="sxs-lookup"><span data-stu-id="dea65-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="dea65-179">Erstellen Sie *src/tsconfig.json* mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="dea65-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="dea65-180">Der vorangehende Code konfiguriert den TypeScript-Compiler, um JavaScript-Code zu erstellen, der mit [ECMAScript 5](https://wikipedia.org/wiki/ECMAScript) kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="dea65-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="dea65-181">Erstellen Sie *src/index.ts* mit dem folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="dea65-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="dea65-182">Der vorangehende TypeScript-Code ruft Verweise auf DOM-Elemente ab und fügt zwei Ereignishandler an:</span><span class="sxs-lookup"><span data-stu-id="dea65-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="dea65-183">`keyup`: Dieses Ereignis wird ausgelöst, wenn der Benutzer etwas in das Textfeld eingibt, das als `tbMessage` erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="dea65-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="dea65-184">Die Funktion `send` wird aufgerufen, wenn der Benutzer die **EINGABETASTE** drückt.</span><span class="sxs-lookup"><span data-stu-id="dea65-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="dea65-185">`click`: Dieses Ereignis wird ausgelöst, wenn der Benutzer auf die Schaltfläche **Senden** klickt.</span><span class="sxs-lookup"><span data-stu-id="dea65-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="dea65-186">Die Funktion `send` wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="dea65-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="dea65-187">Konfigurieren einer ASP.NET Core-App</span><span class="sxs-lookup"><span data-stu-id="dea65-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="dea65-188">Der in der Methode `Startup.Configure` bereitgestellte Code zeigt *Hello World!* an.</span><span class="sxs-lookup"><span data-stu-id="dea65-188">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="dea65-189">Ersetzen Sie den Aufruf der Methode `app.Run` durch Aufrufe von [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) und [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="dea65-189">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="dea65-190">Mit dem vorangehenden Code wird dem Server ermöglicht, die Datei *index.html* zu finden und bereitzustellen, unabhängig davon, ob der Benutzer die vollständige URL oder die Stamm-URL der Web-App eingibt.</span><span class="sxs-lookup"><span data-stu-id="dea65-190">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="dea65-191">Rufen Sie [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in der Methode `Startup.ConfigureServices` auf.</span><span class="sxs-lookup"><span data-stu-id="dea65-191">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="dea65-192">Mit ihr werden die SignalR-Dienste in das Projekt eingefügt.</span><span class="sxs-lookup"><span data-stu-id="dea65-192">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="dea65-193">Ordnen Sie dem `ChatHub`-Hub eine */hub*-Route zu.</span><span class="sxs-lookup"><span data-stu-id="dea65-193">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="dea65-194">Fügen Sie die folgenden Zeilen am Ende der `Startup.Configure`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="dea65-194">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="dea65-195">Erstellen Sie ein neues Verzeichnis namens *Hubs* am Projektstamm.</span><span class="sxs-lookup"><span data-stu-id="dea65-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="dea65-196">Es dient als Speicherort des SignalR-Hubs, der im nächsten Schritt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="dea65-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="dea65-197">Erstellen Sie mit dem folgenden Code den Hub *Hubs/ChatHub.cs*:</span><span class="sxs-lookup"><span data-stu-id="dea65-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="dea65-198">Fügen Sie den folgenden Code am Anfang der Datei *Startup.cs* ein, um den `ChatHub`-Verweis aufzulösen:</span><span class="sxs-lookup"><span data-stu-id="dea65-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="dea65-199">Aktivieren der Kommunikation zwischen Client und Server</span><span class="sxs-lookup"><span data-stu-id="dea65-199">Enable client and server communication</span></span>

<span data-ttu-id="dea65-200">Derzeit zeigt die App ein einfaches Formular zum Senden von Nachrichten an.</span><span class="sxs-lookup"><span data-stu-id="dea65-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="dea65-201">Es geschieht jedoch nichts, wenn Sie versuchen, es zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="dea65-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="dea65-202">Der Server lauscht einer spezifischen Route, aber er verarbeitet gesendete Nachrichten nicht.</span><span class="sxs-lookup"><span data-stu-id="dea65-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="dea65-203">Führen Sie den folgenden Befehl auf der Ebene des Projektstamms aus:</span><span class="sxs-lookup"><span data-stu-id="dea65-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="dea65-204">Der vorangehende Befehl installiert den [SignalR-TypeScript-Client](https://www.npmjs.com/package/@aspnet/signalr), mit dem der Client Nachrichten an den Server senden kann.</span><span class="sxs-lookup"><span data-stu-id="dea65-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="dea65-205">Fügen Sie den hervorgehobenen Code in die Datei *src/index.ts* ein:</span><span class="sxs-lookup"><span data-stu-id="dea65-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="dea65-206">Der vorangehende Code unterstützt das Empfangen von Nachrichten vom Server.</span><span class="sxs-lookup"><span data-stu-id="dea65-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="dea65-207">Die Klasse `HubConnectionBuilder` erstellt einen neuen Generator für die Konfiguration der Serververbindung.</span><span class="sxs-lookup"><span data-stu-id="dea65-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="dea65-208">Die Funktion `withUrl` konfiguriert die Hub-URL.</span><span class="sxs-lookup"><span data-stu-id="dea65-208">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="dea65-209">Mit SignalR wird der Austausch von Nachrichten zwischen einem Client und einem Server ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="dea65-209">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="dea65-210">Jede Nachricht verfügt über einen spezifischen Namen.</span><span class="sxs-lookup"><span data-stu-id="dea65-210">Each message has a specific name.</span></span> <span data-ttu-id="dea65-211">Sie können beispielsweise über Nachrichten mit dem Namen `messageReceived` verfügen, die die Logik zum Anzeigen neuer Nachrichten im Nachrichtenbereich ausführen.</span><span class="sxs-lookup"><span data-stu-id="dea65-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="dea65-212">Mit der `on`-Funktion kann eine spezifische Nachricht belauscht werden.</span><span class="sxs-lookup"><span data-stu-id="dea65-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="dea65-213">Sie können auf eine beliebige Anzahl von Nachrichtennamen lauschen.</span><span class="sxs-lookup"><span data-stu-id="dea65-213">You can listen to any number of message names.</span></span> <span data-ttu-id="dea65-214">Außerdem können Parameter an die Nachricht übergeben werden, z.B. der Name des Autors und der Inhalt der empfangenen Nachricht.</span><span class="sxs-lookup"><span data-stu-id="dea65-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="dea65-215">Sobald der Client eine Nachricht empfängt, wird ein neues `div`-Element mit dem Namen des Autors und dem Inhalt der Nachricht im `innerHTML`-Attribut erstellt.</span><span class="sxs-lookup"><span data-stu-id="dea65-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="dea65-216">Es wird dem Hauptelement `div` hinzugefügt, das die Nachrichten anzeigt.</span><span class="sxs-lookup"><span data-stu-id="dea65-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="dea65-217">Da der Client nun dazu in der Lage ist, Nachrichten zu empfangen, konfigurieren Sie ihn zum Senden von Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="dea65-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="dea65-218">Fügen Sie den hervorgehobenen Code in die Datei *src/index.ts* ein:</span><span class="sxs-lookup"><span data-stu-id="dea65-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="dea65-219">Für das Senden einer Nachricht über die WebSockets-Verbindung ist das Aufrufen der Methode `send` erforderlich.</span><span class="sxs-lookup"><span data-stu-id="dea65-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="dea65-220">Der erste Parameter der Methode ist der Name der Nachricht.</span><span class="sxs-lookup"><span data-stu-id="dea65-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="dea65-221">Die Nachrichtendaten befinden sich in den anderen Parametern.</span><span class="sxs-lookup"><span data-stu-id="dea65-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="dea65-222">In diesem Beispiel wird eine Nachricht, die als `newMessage` erkannt wird, an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="dea65-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="dea65-223">Die Nachricht besteht aus dem Benutzernamen und der Benutzereingabe eines Textfelds.</span><span class="sxs-lookup"><span data-stu-id="dea65-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="dea65-224">Wenn das Senden funktioniert hat, wird der Textfeldwert gelöscht.</span><span class="sxs-lookup"><span data-stu-id="dea65-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="dea65-225">Fügen Sie der `ChatHub`-Klasse die hervorgehobene Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="dea65-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="dea65-226">Der vorangehende Code überträgt die empfangenen Nachrichten an alle verbundenen Benutzer, sobald der Server sie empfängt.</span><span class="sxs-lookup"><span data-stu-id="dea65-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="dea65-227">Es ist nicht notwendig, eine generische `on`-Methode zum Empfangen aller Nachrichten zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="dea65-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="dea65-228">Eine Methode mit demselben Namen wie die Nachricht genügt.</span><span class="sxs-lookup"><span data-stu-id="dea65-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="dea65-229">In diesem Beispiel sendet der TypeScript-Client eine Nachricht, die als `newMessage` erkannt wurde.</span><span class="sxs-lookup"><span data-stu-id="dea65-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="dea65-230">Die C#-Methode `NewMessage` erwartet die vom Client gesendeten Daten.</span><span class="sxs-lookup"><span data-stu-id="dea65-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="dea65-231">Die Methode [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) wird auf [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="dea65-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="dea65-232">Die empfangenen Nachrichten werden an alle mit dem Hub verbundenen Clients gesendet.</span><span class="sxs-lookup"><span data-stu-id="dea65-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="dea65-233">Testen der App</span><span class="sxs-lookup"><span data-stu-id="dea65-233">Test the app</span></span>

<span data-ttu-id="dea65-234">Mit den folgenden Schritten können Sie überprüfen, ob die App funktioniert.</span><span class="sxs-lookup"><span data-stu-id="dea65-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dea65-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dea65-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="dea65-236">Führen Sie Webpack im *Releasemodus* aus.</span><span class="sxs-lookup"><span data-stu-id="dea65-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="dea65-237">Führen Sie mithilfe des Fensters **Paket-Manager-Konsole** den folgenden Befehl im Projektstamm aus:</span><span class="sxs-lookup"><span data-stu-id="dea65-237">Using the **Package Manager Console** window, execute the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="dea65-238">Klicken Sie auf **Debuggen** > **Starten ohne Debugging**, um die App in einem Browser zu starten, ohne den Debugger anzufügen.</span><span class="sxs-lookup"><span data-stu-id="dea65-238">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="dea65-239">Die Datei *wwwroot/index.html* wird unter `http://localhost:<port_number>` bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="dea65-239">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="dea65-240">Öffnen Sie eine weitere Browserinstanz (beliebiger Browser).</span><span class="sxs-lookup"><span data-stu-id="dea65-240">Open another browser instance (any browser).</span></span> <span data-ttu-id="dea65-241">Fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="dea65-241">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="dea65-242">Wählen Sie einen beliebigen Browser aus, geben Sie etwas im Textfeld **Nachricht** ein, und klicken Sie auf **Senden**.</span><span class="sxs-lookup"><span data-stu-id="dea65-242">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="dea65-243">Der eindeutige Benutzername und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dea65-243">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="dea65-244">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="dea65-244">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="dea65-245">Führen Sie Webpack im *Releasemodus* aus, indem Sie den folgenden Befehl im Projektstamm ausführen:</span><span class="sxs-lookup"><span data-stu-id="dea65-245">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="dea65-246">Erstellen Sie die App, und führen Sie sie aus, indem Sie den folgenden Befehl im Projektstamm ausführen:</span><span class="sxs-lookup"><span data-stu-id="dea65-246">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="dea65-247">Der Webserver beginnt mit der App und stellt sie auf „localhost“ zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="dea65-247">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="dea65-248">Öffnen Sie `http://localhost:<port_number>` im Browser.</span><span class="sxs-lookup"><span data-stu-id="dea65-248">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="dea65-249">Die Datei *wwwroot/index.html* wird bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="dea65-249">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="dea65-250">Kopieren Sie die URL aus der Adressleiste.</span><span class="sxs-lookup"><span data-stu-id="dea65-250">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="dea65-251">Öffnen Sie eine weitere Browserinstanz (beliebiger Browser).</span><span class="sxs-lookup"><span data-stu-id="dea65-251">Open another browser instance (any browser).</span></span> <span data-ttu-id="dea65-252">Fügen Sie die URL in die Adressleiste ein.</span><span class="sxs-lookup"><span data-stu-id="dea65-252">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="dea65-253">Wählen Sie einen beliebigen Browser aus, geben Sie etwas im Textfeld **Nachricht** ein, und klicken Sie auf **Senden**.</span><span class="sxs-lookup"><span data-stu-id="dea65-253">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="dea65-254">Der eindeutige Benutzername und die Nachricht werden sofort auf beiden Seiten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="dea65-254">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Die Nachricht wird in beiden Browserfenstern angezeigt](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="dea65-256">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="dea65-256">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>