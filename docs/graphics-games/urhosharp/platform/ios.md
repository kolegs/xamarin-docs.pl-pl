---
title: UrhoSharp iOS i obsługę systemu tvOS
description: W tym dokumencie omówiono iOS oraz systemu tvOS obsługę UrhoSharp. Przedstawiono sposób tworzenia projektu, konfigurowanie i uruchamianie Urho i wykonywanie niestandardowych osadzania z Urho.
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 7e8975b6885f6c902634e05aafca0b8ee60a981c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783978"
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS i obsługę systemu tvOS

Biblioteka klas przenośnych jest Urho i zezwala na tego samego interfejsu API do użycia przez różne platformy dla logiki gier, nadal należy zainicjować Urho w sterowniku określonych platform, a w niektórych przypadkach, można wykorzystać funkcje specyficzne dla platformy .

Na stronach przyjęto założenie, że `MyGame` jest sublcass z `Application` klasy.

## <a name="ios-and-tvos"></a>iOS i systemu tvOS

**Obsługiwane architektury:** armv7 arm64, i386

## <a name="creating-a-project"></a>Tworzenie projektu

Utwórz projekt iOS, a następnie dodać dane do katalogu zasobów i upewnij się, że wszystkie pliki mają **BundleResource** jako **Akcja kompilacji**.

![Ustawienia projektu](ios-images/image-4.png "dodawania danych do katalogu zawierającego zasoby")

## <a name="configuring-and-launching-urho"></a>Konfigurowanie i uruchamianie Urho

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

## <a name="custom-embedding-of-urho"></a>Niestandardowe osadzania Urho

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

