---
title: Rozpoczynanie pracy z Xamarin.Essentials
description: Jednym i platform interfejsu API, który działa z dowolnym z systemem iOS, Android lub platformy uniwersalnej systemu Windows zapewnia Xamarin.Essentials aplikacji, które są dostępne z udostępnionego kodu bez względu na sposób tworzenia interfejsu użytkownika.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a42086f70eb81a761358655b3effb9f8f934c8d4
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067175"
---
# <a name="get-started-with-xamarinessentials"></a>Rozpoczynanie pracy z Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

Jednym i platform interfejsu API, który działa z dowolnym z systemem iOS, Android lub platformy uniwersalnej systemu Windows zapewnia Xamarin.Essentials aplikacji, które są dostępne z udostępnionego kodu bez względu na sposób tworzenia interfejsu użytkownika.

## <a name="platform-support"></a>Obsługa platformy

Xamarin.Essentials obsługuje następujące platformy i systemów operacyjnych:

| Platforma | Wersja |
| --- | --- |
| Android | 4.4 (interfejsu API 19) lub nowszy |
| iOS |10.0 lub nowszej |
| Platforma UWP | 10.0.16299.0 lub nowszej |

## <a name="installation"></a>Instalacja

Xamarin.Essentials jest dostępna jako pakietu NuGet, który można dodać do istniejącym lub nowym projektem za pomocą programu Visual Studio.

1. Pobierz i zainstaluj [programu Visual Studio](http://visualstudio.com) z [Visual Studio tools dla platformy Xamarin](~/cross-platform/get-started/installation/index.md).

2. Otwórz istniejący projekt lub Utwórz nowy projekt za pomocą szablonu pustą aplikację w obszarze **programu Visual Studio C#** (Android, iPhone i iPad lub i Platform). **Ważne**: Jeśli dodanie do projektu platformy UWP upewnij kompilacji 16299 lub nowszej jest ustawiona we właściwościach projektu.

3. Dodaj **Xamarin.Essentials** pakiet NuGet do każdego projektu:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    W panelu Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwy rozwiązania, a następnie wybierz **Zarządzaj pakietami NuGet**. Wyszukaj **Xamarin.Essentials** i zainstaluj pakiet w **wszystkie** projektów w tym biblioteki systemu Android, iOS, platformy uniwersalnej systemu Windows i .NET Standard.

    > [!TIP]
    > Sprawdź **Uwzględnij wersję wstępną** pole podczas [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) jest w wersji zapoznawczej.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    W panelu Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu i wybierz **Dodaj > Dodawanie pakietów NuGet...** . Wyszukaj **Xamarin.Essentials** i zainstaluj pakiet w **wszystkie** projektów w tym biblioteki systemu Android, iOS i .NET Standard.

    > [!TIP]
    > Sprawdź **Pokaż pakiety wersji wstępnej** pole podczas [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) jest w wersji zapoznawczej.

    -----

4. Dodaj odwołanie do Xamarin.Essentials w dowolnej klasy C# do odwołania z interfejsów API.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials wymaga ustawienia specyficzne dla platformy:

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials obsługuje minimalnej wersji systemu Android 4.4 odpowiadający poziom interfejsu API 19, ale Android wersji docelowej dla kompilacji musi być 8.1, odpowiadający poziom interfejsu API 27. (W programie Visual Studio, te dwie wersje ustawiane w oknie dialogowym właściwości projektu dla projektu systemu Android, na karcie manifestu systemu Android. W programie Visual Studio dla komputerów Mac są ustawione w oknie dialogowym Opcje projektu dla projektu systemu Android, na karcie aplikacji systemu Android.) 
    
    Xamarin.Essentials instaluje wersję 27.0.2 biblioteki Xamarin.Android.Support, które są wymagane. Inne biblioteki Xamarin.Android.Support, które aplikacja wymaga również powinny zostać uaktualnione do wersji 27.0.2 za pomocą Menedżera pakietów NuGet. Wszystkie biblioteki Xamarin.Android.Support używanych przez aplikację powinna być taka sama, a powinien mieć co najmniej wersji 27.0.2. Zapoznaj się [Rozwiązywanie problemów z strony](troubleshooting.md) w przypadku wystąpienia problemów Xamarin.Essentials NuGet lub zaktualizowanych NuGets w rozwiązaniu.

    W projekcie systemu Android `MainLauncher` lub dowolnej `Activity` czyli uruchomionego Xamarin.Essentials musi zostać zainicjowany w `OnCreate` metody:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Do obsługi środowiska uruchomieniowego uprawnień w systemie Android, Xamarin.Essentials musi otrzymywać żadnych `OnRequestPermissionsResult`. Dodaj następujący kod do wszystkich `Activity` klasy:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    Nie dodatkowe ustawienia wymagane.

    # <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

    Nie dodatkowe ustawienia wymagane.

    -----

6. Postępuj zgodnie z [przewodniki Xamarin.Essentials](index.md) pozwalające na kopiowanie i wklejanie wstawki kodu dla każdej funkcji.

## <a name="other-resources"></a>Inne zasoby

Firma Microsoft zaleca deweloperzy jesteś nowym użytkownikiem odwiedziny Xamarin [wprowadzenie do rozwoju Xamarin](~/cross-platform/getting-started/index.md).

Odwiedź stronę [repozytorium GitHub Xamarin.Essentials](http://github.com/xamarin/Essentials) Aby wyświetlić bieżący kod źródłowy, wkrótce dalej, próbek, uruchom i zamknij repozytorium. Społeczność są Zapraszamy!

Przeglądanie [dokumentacji interfejsu API](xref:Xamarin.Essentials) dla każdej funkcji Xamarin.Essentials.
