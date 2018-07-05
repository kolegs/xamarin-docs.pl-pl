---
title: 'Xamarin.Essentials: wibracje'
description: W tym dokumencie opisano klasy wibracje w Xamarin.Essentials, która pozwala na uruchamianie i zatrzymywanie funkcji vibrate odpowiednią ilość czasu.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1de464d289bc684015e5fb8489683e3134535b70
ms.sourcegitcommit: cb69bdb469db0b3118e365d71114091c6febb027
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37406774"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: wibracje

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Wibracje** klasa umożliwia uruchamianie i zatrzymywanie funkcji vibrate odpowiednią ilość czasu.

## <a name="getting-started"></a>Wprowadzenie

Aby uzyskać dostęp do **wibracje** następujące ustawienia określone platformy jest wymagane.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Uprawnienie Vibrate jest wymagany i musi być skonfigurowany w projekcie dla systemu Android. Mogą to być dodawane w następujący sposób:

Otwórz **AssemblyInfo.cs** plik **właściwości** folderze i Dodaj:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

LUB zaktualizuj Manifest systemu Android:

Otwórz **AndroidManifest.xml** plik **właściwości** folderze i Dodaj następujący kod wewnątrz **manifestu** węzła.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Lub kliknij prawym przyciskiem myszy nad projektem Anroid i otwórz właściwości projektu. W obszarze **manifestu systemu Android** znaleźć **wymagane uprawnienia:** obszaru i wyboru **VIBRATE** uprawnień. Spowoduje to automatyczne zaktualizowanie **AndroidManifest.xml** pliku.

# <a name="iostabios"></a>[iOS](#tab/ios)

Żadna dodatkowa konfiguracja wymagana.

# <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

Żadna dodatkowa konfiguracja wymagana.

-----

## <a name="using-vibration"></a>Za pomocą wibracje

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Funkcje wibracje można żądać określoną ilość czasu, lub wartość domyślna 500 milisekund.

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

Można ich żądać anulowania wibracje urządzenia za pomocą `Cancel` metody:

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

## <a name="platform-differences"></a>Różnice dotyczące platform

| Platforma | Różnica |
| --- | --- |
| iOS | Wibruje tylko wtedy, gdy urządzenie jest ustawiona na "Vibrate na pierścień". |
| iOS | Zawsze wibruje dla 500 milisekund. |
| iOS | Nie można anulować wibracje. |

## <a name="api"></a>interfejs API

- [Wibracje kodu źródłowego](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Dokumentacja interfejsu API wibracje](xref:Xamarin.Essentials.Vibration)
