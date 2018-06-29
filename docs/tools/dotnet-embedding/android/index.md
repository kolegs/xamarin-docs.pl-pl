---
title: .NET osadzanie w systemie Android
ms.prod: xamarin
ms.assetid: EB2F967A-6D95-4448-994B-6D5C7BFAC2C7
author: topgenorth
ms.author: toopge
ms.date: 06/15/2018
ms.openlocfilehash: e90d1e6258d4cfd9c918c566c9e18c358ee7668a
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067183"
---
# <a name="net-embedding-on-android"></a>.NET osadzanie w systemie Android

W niektórych przypadkach możesz dodać do istniejącego projektu systemu Android native biblioteki Xamarin .NET. Aby to zrobić, można użyć [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/) narzędzia, aby włączyć biblioteki .NET do natywnej biblioteki, który można włączyć do natywnych aplikacji systemu Android opartych na języku Java.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="xamarinandroid-requirements"></a>Wymagania dotyczące platformy Xamarin.Android

Dla platformy Xamarin.Android do pracy z osadzanie .NET potrzebne są następujące elementy:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) lub nowszym należy zainstalować.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) lub nowszym należy zainstalować.

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowszym należy zainstalować.


## <a name="using-embeddinator-4000"></a>Przy użyciu Embeddinator 4000

Aby korzystać z biblioteki .NET w macierzystym projekt systemu Android, wykonaj następujące kroki:

1.  Tworzenie projektu biblioteki systemu Android C#.

2.  Zainstaluj [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Zlokalizuj **Embeddinator 4000.exe** i dodaj go do Twojego **ścieżki**. Na przykład:

    ```cmd
    set PATH=%PATH%;C:\Users\USERNAME\.nuget\packages\embeddinator-4000\0.4.0\tools
    ```

4.  Uruchom Embeddinator 4000 w zestawie biblioteki. Na przykład:

    ```cmd
    Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Użyj wygenerowanego pliku AAR w projekcie języka Java w programie Android Studio.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="xamarinandroid-requirements"></a>Wymagania dotyczące platformy Xamarin.Android

Dla platformy Xamarin.Android do pracy z osadzanie .NET potrzebne są następujące elementy:

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://visualstudio.microsoft.com/xamarin/) lub nowszym należy zainstalować.

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/) lub nowszym należy zainstalować.

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowszym należy zainstalować.

-   **Mono** &ndash; [Mono 5.0](http://www.mono-project.com/download/) lub nowszym należy zainstalować (mono jest instalowana z programem Visual Studio dla komputerów Mac).


## <a name="using-embeddinator-4000"></a>Przy użyciu Embeddinator 4000

Aby korzystać z biblioteki .NET w macierzystym projekt systemu Android, wykonaj następujące kroki:

1.  Tworzenie projektu biblioteki systemu Android C#.

2.  Zainstaluj [Embeddinator 4000](https://www.nuget.org/packages/Embeddinator-4000/).

3.  Zlokalizuj **Embeddinator 4000.exe** i Dodaj **mono** do ścieżki. Na przykład:

    ```bash
    export TOOLS=~/.nuget/packages/embeddinator-4000/0.4.0/tools
    export PATH=$PATH:/Library/Frameworks/Mono.framework/Commands
    ```

4.  Uruchom Embeddinator 4000 w zestawie biblioteki. Na przykład:

    ```bash
    mono $TOOLS/Embeddinator-4000.exe -gen=Java -out=foo Xamarin.Foo.dll
    ```

5.  Użyj wygenerowanego pliku AAR w projekcie języka Java w programie Android Studio.

-----

Opcje użycia i wiersza polecenia zostały opisane w [Embeddinator 4000](https://github.com/mono/Embeddinator-4000/blob/master/Usage.md#java--c) dokumentacji.


## <a name="callbacks"></a>Wywołania zwrotne

Dowiedz się więcej o [wykonywanie wywołań między C# i Java](callbacks.md).

## <a name="samples"></a>Przykłady

* [Pogody przykładowej aplikacji](https://github.com/jamesmontemagno/embeddinator-weather)
