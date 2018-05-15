---
title: Wibrację Xamarin.Essentials
description: Klasa wibrację umożliwia uruchamianie i zatrzymywanie funkcji vibrate odpowiednią ilość czasu.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 68b064d9824a82ea733c7c8bef0c2d43f0a04283
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-vibration"></a>Wibrację Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Wibrację** klasa umożliwia uruchamianie i zatrzymywanie funkcji vibrate odpowiednią ilość czasu.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **wibrację** następujące ustawienia określonych platform jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Uprawnienie Vibrate jest wymagane i muszą być skonfigurowane w projekt systemu Android. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plików w obszarze **właściwości** folderu i dodać:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

LUB zaktualizować manifestu systemu Android:

Otwórz **AndroidManifest.xml** plików w obszarze **właściwości** folderu i dodaj następującą wewnątrz **manifestu** węzła.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Kliknij prawym przyciskiem myszy projekt Anroid i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszarów i wyboru **VIBRATE** uprawnienia. Ta operacja spowoduje automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

# <a name="iostabios"></a>[iOS](#tab/ios)

Nie dodatkowe ustawienia wymagane.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Nie dodatkowe ustawienia wymagane.

-----

## <a name="using-vibration"></a>Przy użyciu wibrację

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Funkcje wibrację może zażądać dla zestawu ilość czasu lub wartość domyślną 500 milisekund.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

Anulowanie wibrację urządzenia może być wysłane z `Cancel` metody:

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>Różnice dotyczące platformy

| Platforma | Różnica |
| --- | --- |
| iOS | Zawsze wibruje do 500 milisekund. |
| iOS | Nie można anulować wibrację. |

## <a name="api"></a>interfejs API

- [Kod źródłowy wibrację](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Dokumentacja interfejsu API wibrację](xref:Xamarin.Essentials.Vibration)
