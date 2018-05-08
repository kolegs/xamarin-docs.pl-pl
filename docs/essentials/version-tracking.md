---
title: Wersja Xamarin.Essentials śledzenia
description: Klasa VersionTracking umożliwia sprawdzanie wersji aplikacji i numery kompilacji oraz wyświetlanie dodatkowych informacji takich jak w przypadku pierwszego czasu aplikacji nigdy uruchamiana lub przez bieżącą wersję, Pobierz poprzedniej informacji o kompilacji i inne.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ec9d62589ddfb270d5c8a5321b3bc733fc597e4b
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-version-tracking"></a>Wersja Xamarin.Essentials śledzenia

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**VersionTracking** klasa umożliwia sprawdzanie wersji aplikacji i numery kompilacji oraz wyświetlanie dodatkowych informacji takich jak w przypadku pierwszego czasu aplikacji nigdy uruchamiana, lub przez bieżącą wersję, Pobierz poprzedniej informacje o kompilacji i inne.

## <a name="using-version-tracking"></a>Przy użyciu wersji śledzenia

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Po raz pierwszy używasz **VersionTracking** klasy rozpocznie śledzenia bieżącej wersji. Należy wywołać `Track` wczesne tylko w każdym załadowaniu zapewniające informacje o wersji są śledzone aplikacji:

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

## <a name="platform-implementation-specifics"></a>Szczegóły implementacji platformy

Wszystkie informacje o wersji jest przechowywany przy użyciu [preferencje](preferences.md) interfejsu API w Xamarin.Essentials i jest przechowywany z nazwą pliku z **.xamarinessentials [YOUR-APP-pakiet-ID]**.

Odinstalowywanie aplikacji spowoduje, że _LocalSettings_i wszystkich wersji śledzenie informacji do usunięcia.

## <a name="api"></a>interfejs API

- [Kod źródłowy śledzenia wersji](https://github.com/xamarin/Essentials/tree/master/Essentials/VersionTracking)
- [Dokumentacja interfejsu API śledzenia wersji](xref:Xamarin.Essentials.VersionTracking)
