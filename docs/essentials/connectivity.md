---
title: 'Xamarin.Essentials: łączność'
description: Klasa łączności w Xamarin.Essentials umożliwia monitorowanie zmian w warunkach sieciowych urządzenia, należy sprawdzić bieżące dostępu do sieci i jak jest obecnie połączony.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 96b4ee0487034c651bec1dfb168fed7567b63c96
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353701"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: łączność

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Łączności** klasy umożliwia monitorowanie zmian w warunkach sieciowych urządzenia, sprawdź bieżący dostęp do sieci i jak jest obecnie połączony.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **łączności** następujące ustawienia określone platformy jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` Uprawnienie jest wymagane i musi być skonfigurowany w projekcie dla systemu Android. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plik **właściwości** folderze i Dodaj:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

LUB zaktualizuj Manifest systemu Android:

Otwórz **AndroidManifest.xml** plik **właściwości** folderze i Dodaj następujący kod wewnątrz **manifestu** węzła.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Lub kliknij prawym przyciskiem myszy projekt Android i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszaru i wyboru **stanu sieci dostępu** uprawnień. Spowoduje to automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

# <a name="iostabios"></a>[iOS](#tab/ios)

Żadna dodatkowa konfiguracja wymagana.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Żadna dodatkowa konfiguracja wymagana.

-----

## <a name="using-connectivity"></a>Przy użyciu łączności

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Sprawdź bieżący dostęp sieciowy:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Dostęp sieciowy](xref:Xamarin.Essentials.NetworkAccess) znajduje się na następujące kategorie:

* **Internet** — lokalnych i dostępem do Internetu.
* **ConstrainedInternet** — ograniczony dostęp do Internetu. Wskazuje wewnętrzne łączności portalu, gdzie znajduje się lokalny dostęp do portalu sieci web, ale wymaga dostępu do Internetu, czy określone poświadczenia są udostępniane za pośrednictwem portalu.
* **Lokalne** — lokalne tylko dostęp do sieci.
* **Brak** — brak łączności jest dostępna.
* **Nieznany** — nie można określić połączenie z Internetem.

Można sprawdzić, jaki typ [profilu połączenia](xref:Xamarin.Essentials.ConnectionProfile) aktywnie używa urządzenia:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Zawsze, gdy profil połączenia lub sieci uzyskiwać dostęp do zmiany może odbierać zdarzenia po wyzwoleniu:

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

Ważne jest, aby pamiętać, że jest to możliwe, `Internet` zgłoszonych przez `NetworkAccess` , ale nie jest pełny dostęp w sieci Web. Z powodu działania połączenia na poszczególnych platformach go tylko gwarantuje, że połączenie jest dostępne. Na przykład urządzenie może być połączone z siecią Wi-Fi, ale router jest odłączony od Internetu. W tym wystąpieniu internetowe mogą być zgłaszane, ale nie jest aktywne połączenie.

## <a name="api"></a>interfejs API

* [Kod źródłowy połączenia](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Dokumentacja interfejsu API połączenia](xref:Xamarin.Essentials.Connectivity)
