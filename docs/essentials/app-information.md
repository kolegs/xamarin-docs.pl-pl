---
title: 'Xamarin.Essentials: Informacje o aplikacji'
description: W tym dokumencie opisano klasy AppInfo w Xamarin.Essentials, który zawiera informacje o aplikacji. Na przykład przedstawia nazwę aplikacji i wersji.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 25765fbbcbd2475bfcbc72ca5c4feefa8ef19d08
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782108"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: Informacje o aplikacji

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
