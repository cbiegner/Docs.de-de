---
title: Facebook externe Anmeldung Setup in ASP.NET Core
author: rick-anderson
description: Dieses Tutorial veranschaulicht die Integration von Facebook-Konto der Benutzerauthentifizierung in eine vorhandene ASP.NET Core-app.
ms.author: riande
ms.date: 08/01/2017
uid: security/authentication/facebook-logins
ms.openlocfilehash: 3ba6fe7785afa268e54e6032f1963c1867f6bb27
ms.sourcegitcommit: 74c09caec8992635825b45b7f065f871d33c077a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42634808"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Facebook externe Anmeldung Setup in ASP.NET Core

Von [Valeriy Novytskyy](https://github.com/01binary) und [Rick Anderson](https://twitter.com/RickAndMSFT)

In diesem Tutorial erfahren Sie, wie Sie Benutzern die Anmeldung mit ihren Facebook-Kontos mit einem ASP.NET Core 2.0-Beispielprojekt auf erstellt ermöglichen, sich die [Vorgängerseite](xref:security/authentication/social/index). Facebook-Authentifizierung erfordert die [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet-Paket. Zunächst erstellen eine Facebook-App-ID gemäß der [offizielle Schritte](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Erstellen einer app in Facebook

* Navigieren Sie zu der [Facebook-Entwickler-app](https://developers.facebook.com/apps/) Seite, und melden Sie sich. Wenn Sie bereits über ein Facebook-Konto haben, verwenden Sie die **für Facebook registrieren** Link auf der Anmeldeseite erstellen.

* Tippen Sie auf die **Hinzufügen einer neuen App** -Schaltfläche in der oberen rechten Ecke, um eine neue App-ID zu erstellen.

   ![Facebook Entwicklerportal in Microsoft Edge geöffnete](index/_static/FBMyApps.png)

* Füllen Sie das Formular, und tippen Sie auf die **Create App ID** Schaltfläche.

   ![Erstellen Sie ein Formular neue App-ID](index/_static/FBNewAppId.png)

* Auf der **wählen Sie ein Produkt** auf **Set Up** auf die **Facebook-Anmeldung** Karte.

   ![Setup-Produktseite](index/_static/FBProductSetup.png)

* Die **Schnellstart** der Assistent wird gestartet, mit **wählen Sie eine Plattform** als erste Seite. Der Assistent jetzt zu umgehen, indem Sie auf die **Einstellungen** Link im Menü auf der linken Seite:

   ![Skip-Schnellstart](index/_static/FBSkipQuickStart.png)

* Werden Ihnen die **Client OAuth Settings** Seite:

![Seite "Client-OAuth-Einstellungen"](index/_static/FBOAuthSetup.png)

* Geben Sie Ihre Entwicklung URI mit */signin-facebook* angefügt, die in der **gültige OAuth-Umleitungs-URIs** Feld (z. B.: `https://localhost:44320/signin-facebook`). Die Facebook-Authentifizierung konfiguriert, die später in diesem Tutorial behandelt die Anforderungen an automatisch */signin-facebook* Route zum Implementieren von OAuth-Fluss.

> [!NOTE]
> Der URI */signin-facebook* ist als der standardrückruf von der Facebook-Authentifizierungsanbieter festgelegt. Sie können den standardrückruf-URI ändern, während der Konfiguration der Facebook-Authentifizierung-Middleware über die geerbte [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) Eigenschaft der [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) -Klasse.

* Klicken Sie auf **speichern Änderungen**.

* Klicken Sie auf **Einstellungen > Basic** Link im linken Navigationsbereich. 

    Notieren Sie sich auf dieser Seite Ihre `App ID` und Ihre `App Secret`. Im nächsten Abschnitt fügen Sie beide Zertifikate in Ihrer ASP.NET Core-Anwendung:


* Bei der Bereitstellung der Website müssen Sie noch einmal die **Facebook-Anmeldung** Setupseite, und registrieren Sie einen neuen öffentlichen URI.

## <a name="store-facebook-app-id-and-app-secret"></a>Store-Facebook-App-ID und App-Geheimnis

Verknüpfen Sie die sensible Einstellungen wie etwa Facebook `App ID` und `App Secret` zu Ihrer Anwendungskonfigurationsdatei mithilfe der [Secret Manager](xref:security/app-secrets). Benennen Sie die Token für die Zwecke dieses Lernprogramms, `Authentication:Facebook:AppId` und `Authentication:Facebook:AppSecret`.

Führen Sie die folgenden Befehle zum sicheren Speichern von `App ID` und `App Secret` mit Secret Manager:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Konfigurieren der Facebook-Authentifizierung

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Hinzufügen des Facebook-Diensts in der `ConfigureServices` -Methode in der die *"Startup.cs"* Datei:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Installieren Sie die [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) Paket.

* Um dieses Paket mit Visual Studio 2017 zu installieren, mit der Maustaste, auf das Projekt, und wählen **NuGet-Pakete verwalten**.
* Führen Sie Folgendes in Ihrem Projektverzeichnis, um mit .NET Core-CLI zu installieren:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Hinzufügen die Facebook-Middleware in der `Configure` -Methode in der *"Startup.cs"* Datei:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

Finden Sie unter den [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API-Referenz für Weitere Informationen zu den Konfigurationsoptionen, die von Facebook-Authentifizierung unterstützt. Optionen für die Konfiguration können zu verwendet werden:

* Fordern Sie verschiedene Informationen über den Benutzer.
* Fügen Sie die Abfragezeichenfolge-Argumente zum Anpassen der Anmeldung hinzu.

## <a name="sign-in-with-facebook"></a>Melden Sie sich mit Facebook

Führen Sie die Anwendung, und klicken Sie auf **melden Sie sich bei**. Daraufhin wird eine Option aus, um sich mit Facebook anmelden.

![Web-Anwendung: Benutzer nicht authentifiziert.](index/_static/DoneFacebook.png)

Wenn Sie beim Klicken auf **Facebook**, Sie werden zur Authentifizierung an Facebook umgeleitet:

![Seite der Facebook-Authentifizierung](index/_static/FBLogin.png)

Facebook-Authentifizierung standardmäßige öffentliche Profil und e-Mail-Adresse anfordert:

![Seite der Facebook-Authentifizierung](index/_static/FBLoginDone.png)

Nachdem Sie Ihre Facebook-Anmeldeinformationen eingeben, werden Sie an Ihrem Standort weitergeleitet, in dem Sie Ihre e-Mail-Adresse festlegen können.

Sie sind jetzt angemeldet, sich mit Ihren Facebook-Anmeldeinformationen:

![Web-Anwendung: Benutzerauthentifizierung](index/_static/Done.png)

## <a name="troubleshooting"></a>Problembehandlung

* **ASP.NET Core 2.x nur:** Wenn Identität ist nicht konfiguriert, durch den Aufruf `services.AddIdentity` in `ConfigureServices`, Authentifizierungsversuch führt zu *ArgumentException: die Option 'SignInScheme' muss angegeben werden*. Die Projektvorlage, die in diesem Tutorial verwendete wird sichergestellt, dass dies geschehen ist.
* Wenn die Standortdatenbank nicht erstellt wurde, indem die ursprüngliche Migration anwenden, erhalten Sie *Fehler bei ein Datenbankvorgang beim Verarbeiten der Anforderung* Fehler. Tippen Sie auf **Migrationen anwenden** der Datenbank zu erstellen und aktualisieren, um den Fehler hinaus fortgesetzt.

## <a name="next-steps"></a>Nächste Schritte

* In diesem Artikel wurde gezeigt, wie Sie mit Facebook authentifizieren können. Führen Sie einen ähnlichen Ansatz für die Authentifizierung mit anderen Anbietern aufgeführt, auf die [Vorgängerseite](xref:security/authentication/social/index).

* Nachdem Sie Ihre Website in Azure-Web-app veröffentlichen, sollten Sie Zurücksetzen der `AppSecret` im Facebook-Entwicklerportal.

* Legen Sie die `Authentication:Facebook:AppId` und `Authentication:Facebook:AppSecret` Anwendungseinstellungen im Azure-Portal. Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.
