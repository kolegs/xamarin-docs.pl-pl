---
title: Informacje o aplikacji Xamarin.Essentials
description: Klasa AppInfo udostępnia informacje na temat aplikacji.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 4591f1256dc574299bb573ef9ec5afb35af7c542
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-app-information"></a>Informacje o aplikacji Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**AppInfo** klasa udostępnia informacje na temat aplikacji.

## <a name="using-appinfo"></a>Przy użyciu AppInfo

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

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

## <a name="api"></a>interfejs API

- [Kod źródłowy AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Dokumentacja interfejsu API AppInfo](xref:Xamarin.Essentials.AppInfo)
