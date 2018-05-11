---
title: Latarki Xamarin.Essentials
description: Klasa latarki ma możliwość włączyć lub wyłączyć aparat urządzenia flash, aby przekształcić z latarki.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f187fa404df09e387ed870f524239d3baabfdd3f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-flashlight"></a>Latarki Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Latarki** klasa ma możliwość włączyć lub wyłączyć aparat urządzenia flash, aby przekształcić z latarki.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **latarki** następujące ustawienia określonych platform jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Latarki i aparatu uprawnienia są wymagane i muszą być skonfigurowane w projekt systemu Android. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plików w obszarze **właściwości** folderu i dodać:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

LUB zaktualizować manifestu systemu Android:

Otwórz **AndroidManifest.xml** plików w obszarze **właściwości** folderu i dodaj następującą wewnątrz **manifestu** węzła.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Kliknij prawym przyciskiem myszy projekt Anroid i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszarów i wyboru **LATARKI** i **aparatu** uprawnienia. Ta operacja spowoduje automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

Przez dodanie tych uprawnień [Google Play automatycznie odfiltruje urządzeń](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) bez określonego sprzętu. Obejścia tego problemu można uzyskać przez dodanie poniższego do pliku AssemblyInfo.cs w projekcie systemu Android:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Nie dodatkowe ustawienia wymagane.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Nie dodatkowe ustawienia wymagane.

-----

## <a name="using-flashlight"></a>Przy użyciu latarki

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Latarki można włączyć lub wyłączyć za pomocą `TurnOnAsync` i `TurnOffAsync` metod:

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

## <a name="platform-implementation-specifics"></a>Szczegóły implementacji platformy

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

Klasa latarki została optmized opartą na systemie operacyjnym urządzenia.

#### <a name="api-level-23-and-higher"></a>Interfejs API na poziomie 23 i nowsze

Na nowsze poziomy interfejsu API [tryb latarka](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) umożliwia włączanie i wyłączanie flash jednostkę urządzenie.

#### <a name="api-level-22-and-lower"></a>Interfejs API na poziomie 22 i małe

Powierzchni aparatu jest tworzony, aby włączyć lub wyłączyć `FlashMode` jednostki aparatu. 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) umożliwia włączanie i wyłączanie latarka i Flash tryb urządzenia.

### <a name="uwptabuwp-specifics"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp-specifics)

[Światła](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) jest używana do wykrywania pierwszy światła tyłu urządzenie, aby włączyć lub wyłączyć.

-----

## <a name="api"></a>interfejs API

- [Kod źródłowy latarki](https://github.com/xamarin/Essentials/tree/master/Essentials/Flashlight)
- [Dokumentacja interfejsu API latarki](xref:Xamarin.Essentials.Flashlight)
