---
title: Rozpoczynanie pracy z usługą Xamarin.Essentials
description: Xamarin.Essentials udostępnia interfejs API jednego dla wielu platform, która współdziała z dowolnym z systemem iOS, Android i platformy uniwersalnej systemu Windows aplikacji, które są dostępne z udostępnionego kodu, niezależnie od tego, w jaki sposób jest tworzony interfejs użytkownika.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e371a6125d223d354b75ce7e09dcc28efb3dffa
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855237"
---
# <a name="get-started-with-xamarinessentials"></a>Rozpoczynanie pracy z usługą Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

Xamarin.Essentials udostępnia interfejs API jednego dla wielu platform, która współdziała z dowolnym z systemem iOS, Android i platformy uniwersalnej systemu Windows aplikacji, które są dostępne z udostępnionego kodu, niezależnie od tego, w jaki sposób jest tworzony interfejs użytkownika.

## <a name="platform-support"></a>Obsługa platformy

Xamarin.Essentials obsługuje następujące platformy i systemy operacyjne:

| Platforma | Wersja |
| --- | --- |
| Android | (Interfejs API 19) w wersji 4.4 lub nowszy |
| iOS |10.0 lub nowszy |
| Platforma UWP | 10.0.16299.0 lub nowszej |

## <a name="installation"></a>Instalacja

Xamarin.Essentials jest dostępna jako pakiet NuGet, który można dodać do dowolnego istniejącego lub nowego projektu przy użyciu programu Visual Studio.

1. Pobierz i zainstaluj [programu Visual Studio](http://visualstudio.com) z [Visual Studio tools for Xamarin](~/cross-platform/get-started/installation/index.md).

2. Otwórz istniejący projekt lub Utwórz nowy projekt za pomocą szablonu pustej aplikacji, w obszarze **programu Visual Studio C#** (Android, iPhone i iPad lub dla wielu Platform). **Ważne**: Jeśli dodanie do projektu platformy uniwersalnej systemu Windows upewnij się, kompilacja 16299 lub nowszej jest ustawiona we właściwościach projektu.

3. Dodaj **Xamarin.Essentials** do każdego projektu pakiet NuGet:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    W panelu Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę rozwiązania, a następnie wybierz **Zarządzaj pakietami NuGet**. Wyszukaj **Xamarin.Essentials** i zainstaluj pakiet do **wszystkich** projektów, w tym systemów Android, iOS, platformy uniwersalnej systemu Windows i .NET Standard bibliotek.

    > [!TIP]
    > Sprawdź **Uwzględnij wersję wstępną** pola podczas [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) jest w wersji zapoznawczej.

    # <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    W panelu Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz **Dodaj > Dodaj pakiety NuGet...** . Wyszukaj **Xamarin.Essentials** i zainstaluj pakiet do **wszystkich** projektów, w tym systemów Android, iOS i .NET Standard bibliotek.

    > [!TIP]
    > Sprawdź **Pokaż pakiety w wersji wstępnej** pola podczas [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) jest w wersji zapoznawczej.

    -----

4. Dodaj odwołanie do Xamarin.Essentials w dowolnej klasy języka C# można odwoływać się do interfejsów API.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials wymaga konfiguracji specyficznej dla platformy:

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials obsługuje minimalnej wersji systemu Android 4.4, odpowiadający poziom interfejsu API 19, ale docelowa wersja systemu Android do kompilowania musi być 8.1, odpowiadający poziom 27 interfejsu API. (W programie Visual Studio, te dwie wersje ustawiane w oknie dialogowym właściwości projektu dla projektu systemu Android, w karcie Manifest systemu Android. W programie Visual Studio dla komputerów Mac są one ustawione w oknie dialogowym Opcje projektu dla projektu systemu Android, na karcie aplikacji dla systemu Android). 
    
    Xamarin.Essentials powoduje zainstalowanie wersji 27.0.2.1 Xamarin.Android.Support bibliotek, które są wymagane. Inne biblioteki Xamarin.Android.Support, których aplikacja wymaga również powinien zostać zaktualizowany do wersji 27.0.2.1 przy użyciu Menedżera pakietów NuGet. Wszystkie biblioteki Xamarin.Android.Support używanych przez aplikację powinna być taka sama, a powinien wynosić co najmniej wersji 27.0.2.1. Zapoznaj się [strona rozwiązywania problemów](troubleshooting.md) Jeśli masz problemy z Xamarin.Essentials NuGet Dodawanie lub aktualizowanie rozszerzeń Nuget w swoim rozwiązaniu.

    W projekcie dla systemu Android `MainLauncher` lub dowolnego `Activity` oznacza to wprowadzona na rynek Xamarin.Essentials muszą być zainicjowane w `OnCreate` metody:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Do obsługi środowiska uruchomieniowego uprawnienia w systemie Android, Xamarin.Essentials musi otrzymać bit dowolne `OnRequestPermissionsResult`. Dodaj następujący kod do wszystkich `Activity` klasy:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    Żadna dodatkowa konfiguracja wymagana.

    # <a name="uwptabuwp"></a>[PLATFORMY UNIWERSALNEJ SYSTEMU WINDOWS](#tab/uwp)

    Żadna dodatkowa konfiguracja wymagana.

    -----

6. Postępuj zgodnie z [przewodniki Xamarin.Essentials](index.md) umożliwiających użytkownikowi kopiowanie i wklejanie fragmentów kodu dla każdej funkcji.

## <a name="other-resources"></a>Inne zasoby

Firma Microsoft zaleca deweloperów jesteś nowym użytkownikiem platformy Xamarin, odwiedź stronę [wprowadzenie do programowania Xamarin](~/cross-platform/getting-started/index.md).

Odwiedź stronę [repozytorium serwisu GitHub Xamarin.Essentials](http://github.com/xamarin/Essentials) Aby wyświetlić bieżący kod źródłowy, co będzie dalej, próbek, uruchom i zamknij repozytorium. Kod wniesiony przez społeczność są Zapraszamy!

Przeglądanie [dokumentacji interfejsu API](xref:Xamarin.Essentials) dla każdej funkcji Xamarin.Essentials.
