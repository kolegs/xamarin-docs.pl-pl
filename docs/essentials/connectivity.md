---
title: 'Xamarin.Essentials: łączność'
description: Klasa łączność w Xamarin.Essentials umożliwia monitorowanie zmian w warunkach sieciowych urządzenia, należy sprawdzić bieżące dostępu do sieci i jak jest aktualnie połączony.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 54c165e15e725caaecb1573b74cfe295170db141
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782869"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: łączność

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Łączności** klasa umożliwia monitorowanie zmian w warunkach sieciowych urządzenia, sprawdź bieżące dostępu do sieci i jak jest aktualnie połączony.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **łączności** następujące ustawienia określonych platform jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` Uprawnienie jest wymagane i muszą być skonfigurowane w projekt systemu Android. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plików w obszarze **właściwości** folderu i dodać:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

LUB zaktualizować manifestu systemu Android:

Otwórz **AndroidManifest.xml** plików w obszarze **właściwości** folderu i dodaj następującą wewnątrz **manifestu** węzła.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Kliknij prawym przyciskiem myszy projekt Anroid i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszarów i wyboru **stan dostępu do sieci** uprawnienia. Ta operacja spowoduje automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

# <a name="iostabios"></a>[iOS](#tab/ios)

Nie dodatkowe ustawienia wymagane.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Nie dodatkowe ustawienia wymagane.

-----

## <a name="using-connectivity"></a>Przy użyciu połączenia

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Sprawdź bieżący dostępu do sieci:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Dostęp sieciowy](xref:Xamarin.Essentials.NetworkAccess) znajduje się na następujące kategorie:

* **Internet** — lokalnych i dostępem do Internetu.
* **ConstrainedInternet** — ograniczony dostęp do Internetu. Określa wewnętrzne łączności portalu, gdzie lokalnego dostępu do portalu sieci web jest dostępny, ale wymaga dostępu do Internetu, że określone poświadczenia są udostępniane za pośrednictwem portalu.
* **Lokalne** — lokalny wyłącznie dostęp do sieci.
* **Brak** — brak łączności jest dostępna.
* **Nieznany** — nie można ustalić łączności z Internetem.

Można sprawdzić, jaki typ [profilu połączenia](xref:Xamarin.Essentials.ConnectionProfile) aktywnie używa urządzenia:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Jeśli profil połączenia lub sieci dostępu zmiany może odbierać zdarzenia po wyzwoleniu:

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>Ograniczenia

Należy pamiętać, że możliwe jest ważne jest, który `Internet` został zgłoszony przez `NetworkAccess` , ale nie ma pełny dostęp do sieci web. Z powodu działania łączności na każdej platformie go tylko gwarantuje, że połączenie jest dostępne. Na przykład urządzenie może być połączone z siecią Wi-Fi, ale router jest odłączony od Internetu. W takim przypadku należy podać internetowych, ale nie jest aktywne połączenie.

## <a name="api"></a>interfejs API

* [Kod źródłowy łączności](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Dokumentacja interfejsu API łączności](xref:Xamarin.Essentials.Connectivity)
