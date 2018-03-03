---
title: "UrhoSharp iOS i obsługę systemu tvOS"
description: "iOS i systemu tvOS określonych ustawień i funkcji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 9cf779b23ed830c07af0100152a44d6c3c4e317b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS i obsługę systemu tvOS

_iOS i systemu tvOS określonych ustawień i funkcji_

Biblioteka klas przenośnych jest Urho i zezwala na tego samego interfejsu API do użycia przez różne platformy dla logiki gier, nadal należy zainicjować Urho w sterowniku określonych platform, a w niektórych przypadkach, można wykorzystać funkcje specyficzne dla platformy .

Na stronach przyjęto założenie, że `MyGame` jest sublcass z `Application` klasy.

# <a name="ios-and-tvos"></a>iOS i systemu tvOS

**Obsługiwane architektury:** armv7 arm64, i386

# <a name="creating-a-project"></a>Tworzenie projektu

Utwórz projekt iOS, a następnie dodać dane do katalogu zasobów i upewnij się, że wszystkie pliki mają **BundleResource** jako **Akcja kompilacji**.

![Ustawienia projektu](ios-images/image-4.png "dodawania danych do katalogu zawierającego zasoby")

# <a name="configuring-and-launching-urho"></a>Konfigurowanie i uruchamianie Urho

Dodać za pomocą instrukcji dla `Urho` i `Urho.iOS` przestrzeni nazw, a następnie dodaj ten kod inicjowania Urho, a także do uruchamiania aplikacji:

```csharp
new MyGame().Run();
```

Zwróć uwagę, że od czasu oczekuje iOS `FinishedLaunching` do wykonania, należy kolejka wywołanie `Run()` do uruchomienia po zakończeniu metody, jest to typowe idiom:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    LaunchGame();
    return true;
}

async void LaunchGame()
{
    await Task.Yield();
    new SamplyGame().Run();
}
```

Należy wyłączyć optymalizacje PNG, ponieważ Optymalizator PNG iOS domyślne wygeneruje obrazy, które Urho nie może obecnie poprawnie używać

# <a name="custom-embedding-of-urho"></a>Niestandardowe osadzania Urho

Użytkownik może też mającej Urho przejąć ekranu całej aplikacji, a aby go użyć jako części aplikacji, można utworzyć `UrhoSurface` co jest `UIView` , który można osadzić w istniejącej aplikacji.

Jest to, co należy zrobić:

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

To będzie obsługiwać klasie Urho tak, a następnie należy wykonać:

```csharp
new MyGame().Run ();
```

