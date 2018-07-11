---
title: 'Xamarin.Essentials: Śledzenie wersji'
description: Klasa VersionTracking w Xamarin.Essentials pozwala sprawdzić wersję aplikacji i numery kompilacji oraz wyświetlać dodatkowe informacje takie tak, jakby była pierwszym uruchomieniu aplikacji, nigdy nie uruchamiane lub bieżącą wersję, możesz uzyskać poprzednią kompilację informacje i więcej.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2c092d6767045f0af956c5dab74801077dadb51f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815651"
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials: Śledzenie wersji

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**VersionTracking** klasy pozwala sprawdzić wersję aplikacji i numery kompilacji oraz wyświetlać dodatkowe informacje takie tak, jakby była pierwszym uruchomieniu aplikacji, nigdy nie uruchamiane lub bieżącą wersję, możesz uzyskać poprzedni informacje o kompilacji i nie tylko.

## <a name="using-version-tracking"></a>Używanie śledzenia wersji

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Po raz pierwszy używasz **VersionTracking** klasy rozpocznie się śledzenia bieżącej wersji. Należy wywołać `Track` wcześniej tylko w aplikacji zawsze jest ładowany w celu zapewnienia jest śledzona bieżące informacje o wersji:

```csharp
VersionTracking.Track();
```

Po początkowej `Track` nosi nazwę można odczytać informacji o wersji:

```csharp

// First time ever launched application
var firstLaunch = VersionTracking.IsFirstLaunchEver;

// First time launching current version
var firstLaunchCurrent = VersionTracking.IsFirstLaunchForCurrentVersion;

// First time launching current build
var firstLaunchBuild = VersionTracking.IsFirstLaunchForCurrentBuild;

// Current app version (2.0.0)
var currentVersion = VersionTracking.CurrentVersion;

// Current build (2)
var currentBuild = VersionTracking.CurrentBuild;

// Previous app version (1.0.0)
var previousVersion = VersionTracking.PreviousVersion;

// Previous app build (1)
var previousBuild = VersionTracking.PreviousBuild;

// First version of app installed (1.0.0)
var firstVersion = VersionTracking.FirstInstalledVersion;

// First build of app installed (1)
var firstBuild = VersionTracking.FirstInstalledBuild;

// List of versions installed (1.0.0, 2.0.0)
var versionHistory = VersionTracking.VersionHistory;

// List of builds installed (1, 2)
var buildHistory = VersionTracking.BuildHistory;
```

## <a name="platform-implementation-specifics"></a>Funkcje specyficzne dla implementacji platformy

Wszystkie informacje o wersji jest przechowywany przy użyciu [preferencje](preferences.md) interfejsu API w Xamarin.Essentials i jest przechowywany przy użyciu nazwy pliku z **.xamarinessentials [YOUR-APP-pakietu-ID]**.

Odinstalowywanie aplikacji spowoduje, że _LocalSettings_i wszystkich wersji śledzi informacje do usunięcia.

## <a name="api"></a>interfejs API

- [Kod źródłowy śledzenia wersji](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [Wersja śledzenia dokumentacji interfejsu API](xref:Xamarin.Essentials.VersionTracking)
