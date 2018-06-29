---
title: 'Xamarin.Essentials: Informacje o aplikacji'
description: W tym dokumencie opisano klasy AppInfo w Xamarin.Essentials, który zawiera informacje o aplikacji. Na przykład przedstawia nazwę aplikacji i wersji.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080277"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: Informacje o aplikacji

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**AppInfo** klasa udostępnia informacje na temat aplikacji.

## <a name="using-appinfo"></a>Przy użyciu AppInfo

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Uzyskiwanie informacji o aplikacji:

Następujące informacje są dostępne za pośrednictwem interfejsu API:

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="displaying-application-settings"></a>Wyświetlanie ustawień aplikacji

**AppInfo** klasy może być także wyświetlane na stronie Ustawienia obsługiwane przez system operacyjny dla aplikacji:

```csharp
// Display settings page
AppInfo.OpenSettings();
```

Ta strona Ustawienia umożliwia użytkownikowi zmienianie uprawnień aplikacji oraz wykonywać inne zadania specyficzne dla platformy.

## <a name="api"></a>interfejs API

- [Kod źródłowy AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Dokumentacja interfejsu API AppInfo](xref:Xamarin.Essentials.AppInfo)
