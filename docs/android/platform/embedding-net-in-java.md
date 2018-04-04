---
title: Osadzanie .NET w języku Java
description: Jak używać biblioteki Xamarin .NET w opartych na języku Java natywnego projekt systemu Android
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2018
ms.openlocfilehash: 0ca855975db4b20fe80e98e2e818b8fde27f558c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="embedding-net-in-java"></a>Osadzanie .NET w języku Java

W niektórych przypadkach możesz dodać do istniejącego projektu systemu Android native biblioteki Xamarin .NET. Aby to zrobić, można użyć [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/) narzędzia, aby włączyć biblioteki .NET do natywnej biblioteki, który można włączyć do natywnych aplikacji systemu Android opartych na języku Java.

 
## <a name="requirements"></a>Wymagania

Aby użyć Embeddinator 4000 z językiem Java w systemie Android, potrzebne następujące elementy:

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/preview/index.html) lub nowszym należy zainstalować.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) lub nowszym należy zainstalować.

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) lub nowszym należy zainstalować.

-   **Mono** &ndash; [Mono 5.0](http://www.mono-project.com/download/) lub nowszym należy zainstalować.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio umożliwia edytowanie i kompilowania kodu C#.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio for Mac można użyć do edycji i kompilowania kodu C#.

-----

 
## <a name="using-the-embeddinator-4000"></a>Przy użyciu Embeddinator 4000

Aby korzystać z biblioteki .NET w macierzystym projekt systemu Android, należy użyć następujących czynności:

1.  Tworzenie projektu biblioteki systemu Android C#.

2.  Zainstaluj Embeddinator-4000 za pośrednictwem pakietu NuGet.

3.  Uruchom Embeddinator w zestawie biblioteki systemu Android.

4.  Użyj wygenerowanego pliku AAR w projekcie języka Java w programie Android Studio.

Te kroki opisano szczegółowo w [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/getting-started-java-android.html) dokumentacji.
