---
title: Informacje o aplikacji Xamarin.Essentials
description: Klasa AppInfo udostępnia informacje na temat aplikacji.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 32e3eb8fab719540e4c9ffec4e57f5510c10e3f5
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2018
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

- [Kod źródłowy AppInfo](https://github.com/xamarin/Essentials/tree/master/Essentials/AppInfo)
- [Dokumentacja interfejsu API AppInfo](xref:Xamarin.Essentials.AppInfo)
