---
title: Jak usunąć błąd pathtoolongexception —?
description: W tym artykule opisano sposób rozwiązywania pathtoolongexception —, które mogą wystąpić podczas kompilowania aplikacji.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: f50ca3e738cb781f9c80e83f58f2e0fa1fa8e113
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Jak usunąć błąd pathtoolongexception —?

## <a name="cause"></a>Przyczyna

Nazwy ścieżek wygenerowany w projekcie platformy Xamarin.Android może być dość długo.
Na przykład ścieżka podobne do poniższych zostanie wygenerowany podczas kompilacji:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

W systemie Windows (gdzie maksymalna długość ścieżki jest [260 znaków](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), **pathtoolongexception —** może być utworzone podczas kompilowania projektu, jeśli wygenerowanego ścieżki przekracza maksymalną długość. 

## <a name="fix"></a>Poprawka

Począwszy od platformy Xamarin.Android 8.0, `UseShortFileNames` obejścia tego błędu można ustawić właściwości programu MSBuild. Jeśli ta właściwość jest skonfigurowana `True` (wartość domyślna to `False`), procesu kompilacji używa krótszej nazwy ścieżek, aby zmniejszyć prawdopodobieństwo produkujących **pathtoolongexception —**.
Na przykład, jeśli `UseShortFileNames` ma ustawioną wartość `True`, powyższej ścieżce jest obcinana do ścieżki, która jest podobny do następującego:

**C:\\niektórych\\katalogu\\rozwiązania\\projektu\\obj\\debugowania\\lp\\1\\jl\\zasobów**

Aby ustawić tę właściwość, należy dodać następujące właściwości programu MSBuild w projekcie **.csproj** pliku:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Aby uzyskać więcej informacji na temat ustawienie właściwości kompilacji, zobacz [procesu kompilacji](~/android/deploy-test/building-apps/build-process.md).
