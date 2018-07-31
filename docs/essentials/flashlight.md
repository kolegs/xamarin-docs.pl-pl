---
title: 'Xamarin.Essentials: latarki'
description: W tym dokumencie opisano klasy latarki w Xamarin.Essentials, który umożliwia włączanie i wyłączanie aparatu urządzenia flash, aby przekształcić ją w latarki.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8c471f64c14a2e41693c450e02f89e7ac845d060
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353363"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: latarki

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Latarki** klasa ma możliwość Włączanie lub wyłączanie aparatu urządzenia flash, aby przekształcić ją w latarki.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **latarki** następujące ustawienia określone platformy jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Uprawnienia latarki i aparat są wymagane i muszą być skonfigurowane w projekt systemu Android. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plik **właściwości** folderze i Dodaj:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

LUB zaktualizuj Manifest systemu Android:

Otwórz **AndroidManifest.xml** plik **właściwości** folderze i Dodaj następujący kod wewnątrz **manifestu** węzła.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Lub kliknij prawym przyciskiem myszy projekt Android i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszaru i wyboru **LATARKI** i **aparatu** uprawnienia. Spowoduje to automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

Przez dodanie tych uprawnień [sklepu Google Play automatycznie odfiltruje urządzeń](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) bez określonego sprzętu. Obejścia tego problemu można uzyskać przez dodanie poniższego do pliku AssemblyInfo.cs w projekcie dla systemu Android:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Żadna dodatkowa konfiguracja wymagana.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Żadna dodatkowa konfiguracja wymagana.

-----

## <a name="using-flashlight"></a>Za pomocą latarki

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Latarki można włączać i wyłączać za pośrednictwem `TurnOnAsync` i `TurnOffAsync` metody:

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

### <a name="androidtabandroid"></a>[Android](#tab/android)

Klasa latarki została zoptymalizowana na podstawie systemu operacyjnego urządzenia.

#### <a name="api-level-23-and-higher"></a>Poziom interfejsu API 23 lub nowszy

W nowszych poziomy interfejsu API [tryb latarka](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) będzie służyć do włączyć lub wyłączyć jednostki flash urządzenia.

#### <a name="api-level-22-and-lower"></a>Poziom 22 interfejsu API i niższy

Włączanie lub wyłączanie tekstury do powierzchni aparatu zostanie utworzona `FlashMode` jednostki aparatu. 

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) umożliwia włączanie i wyłączanie latarka i tryb Flash urządzenia.

### <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

[Lampa](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) służy do wykrywania pierwszy lamp na odwrocie podkładki urządzenie, aby włączyć lub wyłączyć.

-----

## <a name="api"></a>interfejs API

- [Kod źródłowy latarki](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Dokumentacja interfejsu API latarki](xref:Xamarin.Essentials.Flashlight)
