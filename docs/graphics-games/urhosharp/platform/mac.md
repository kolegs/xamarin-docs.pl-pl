---
title: Obsługa UrhoSharp Mac
description: Mac określonych ustawień i funkcji do UrhoSharp.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: ed396097f55f5f4a2f8a3b718b324919d0f8daea
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/18/2018
---
# <a name="urhosharp-mac-support"></a>Obsługa UrhoSharp Mac

_Mac określonych ustawień i funkcji_

Biblioteka klas przenośnych jest Urho i zezwala na tego samego interfejsu API do użycia przez różne platformy dla logiki gier, nadal należy zainicjować Urho w sterowniku określonych platform, a w niektórych przypadkach, można wykorzystać funkcje specyficzne dla platformy .

Na stronach przyjęto założenie, że `MyGame` jest podklasą `Application` klasy.

## <a name="macos"></a>macOS

**Obsługiwane architektury:** x86/x86-64 dla 32-bitowych i 64-bitowej.

## <a name="creating-a-project"></a>Tworzenie projektu

Utwórz projekt konsoli, Urho NuGet odwołania, a następnie upewnij się, czy można zlokalizować zasoby (katalogi zawierającym katalog danych).

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Przykład

[Pełny przykład](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


