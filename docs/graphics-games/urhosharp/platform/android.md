---
title: "Obsługa systemu UrhoSharp Android"
description: "Dla systemu android określonych ustawień i funkcji"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 5100fd4ac573021e088a88446f5f6559d49c4972
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="urhosharp-android-support"></a>Obsługa systemu UrhoSharp Android

_Dla systemu android określonych ustawień i funkcji_

Biblioteka klas przenośnych jest Urho i zezwala na tego samego interfejsu API do użycia przez różne platformy dla logiki gier, nadal należy zainicjować Urho w sterowniku określonych platform, a w niektórych przypadkach, można wykorzystać funkcje specyficzne dla platformy .

Na stronach przyjęto założenie, że `MyGame` jest podklasą `Application` klasy.

# <a name="architectures"></a>Architektury

**Obsługiwane architektury**: x86, armeabi, armeabi v7a

# <a name="create-a-project"></a>Tworzenie projektu

Utwórz projekt systemu Android i Dodaj pakiet UrhoSharp NuGet.

Dodaj dane zawierające zasobów do **zasoby** katalogu i upewnij się, że wszystkie pliki mają **AndroidAsset** jako **Akcja kompilacji**.

![Ustawienia projektu](android-images/image-3.png "Dodaj danych zawierającego zasoby do katalogu zawierającego zasoby")

# <a name="configure-and-launching-urho"></a>Konfigurowanie i uruchamianie Urho

Dodać za pomocą instrukcji dla `Urho` i `Urho.Android` przestrzeni nazw, a następnie dodaj ten kod inicjowania Urho, a także do uruchamiania aplikacji.

Najprostszym sposobem uruchomienia gry, zgodnie z implementacją w klasie MyGame jest do wywołania

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

Spowoduje to otwarcie działania fullscreen z gry jako zawartości.

# <a name="custom-embedding-of-urho"></a>Niestandardowe osadzania Urho

Użytkownik może również mającej Urho przejąć ekranu całej aplikacji, a aby go użyć jako części aplikacji, można utworzyć `SurfaceView` za pomocą:

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

Należy również przekazywania kilka zdarzeń przez działanie do UrhoSharp, np.:

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

Wykonaj te same: `OnResume`, `OnPause`, `OnLowMemory`, `OnDestroy`, `DispatchKeyEvent` i `OnWindowFocusChanged`.

Oznacza to typowe działanie, które uruchamia gry:

```csharp
[Activity(Label = "MyUrhoApp", MainLauncher = true,
    Icon = "@drawable/icon", Theme = "@android:style/Theme.NoTitleBar.Fullscreen",
    ConfigurationChanges = ConfigChanges.KeyboardHidden | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Portrait)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        var mLayout = new AbsoluteLayout(this);
        var surface = UrhoSurface.CreateSurface<MyUrhoApp>(this);
        mLayout.AddView(surface);
        SetContentView(mLayout);
    }

    protected override void OnResume()
    {
        UrhoSurface.OnResume();
        base.OnResume();
    }

    protected override void OnPause()
    {
        UrhoSurface.OnPause();
        base.OnPause();
    }

    public override void OnLowMemory()
    {
        UrhoSurface.OnLowMemory();
        base.OnLowMemory();
    }

    protected override void OnDestroy()
    {
        UrhoSurface.OnDestroy();
        base.OnDestroy();
    }

    public override bool DispatchKeyEvent(KeyEvent e)
    {
        if (!UrhoSurface.DispatchKeyEvent(e))
            return false;
        return base.DispatchKeyEvent(e);
    }

    public override void OnWindowFocusChanged(bool hasFocus)
    {
        UrhoSurface.OnWindowFocusChanged(hasFocus);
        base.OnWindowFocusChanged(hasFocus);
    }
}
```

