---
title: Sprawdzanie orientacji urządzenia
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms DependencyService klasy dostępu do orientacji urządzenia z udostępnionego kodu.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 620404a217b2e8a31192ae6613dcec023ac366cd
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995644"
---
# <a name="checking-device-orientation"></a>Sprawdzanie orientacji urządzenia

Ten artykuł przeprowadzi przy użyciu [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) do sprawdzenia orientacji urządzenia z udostępnionego kodu przy użyciu macierzystych interfejsów API na każdej platformie. Ten przewodnik jest oparty na istniejących `DeviceOrientation` wtyczki według Ali Özgür. Zobacz [repozytorium GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) Aby uzyskać więcej informacji.

- **[Tworzenie interfejsu](#Creating_the_Interface)**  &ndash; zrozumieć, jak interfejs jest tworzony w współużytkowanym kodem.
- **[iOS implementacji](#iOS_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu iOS.
- **[Implementacja systemu android](#Android_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Android.
- **[Implementacja platformy uniwersalnej systemu Windows](#WindowsImplementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym do uniwersalnej platformy Windows (UWP).
- **[Implementacja w kodzie udostępnionego](#Implementing_in_Shared_Code)**  &ndash; Dowiedz się, jak używać `DependencyService` do wywołania w natywnych implementacji ze współużytkowanym kodem.

Aplikacji przy użyciu `DependencyService` mają następującą strukturę:

![](device-orientation-images/orientation-diagram.png "DependencyService struktury aplikacji")

> [!NOTE]
> Istnieje możliwość wykryć, czy urządzenie jest w orientacji pionowej lub poziomej w udostępnionego kodu źródłowego, jak pokazano w [Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation urządzenia). Metody opisanej w tym artykule używa natywne funkcje, aby uzyskać więcej informacji na temat orientacji, w tym, czy urządzenie jest odwrócony.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Najpierw Utwórz interfejs w kodzie udostępnionego, który określa funkcje, które planujesz wdrożyć. W tym przykładzie interfejs zawiera jedną metodę:

```csharp
namespace DependencyServiceSample.Abstractions
{
    public enum DeviceOrientations
    {
        Undefined,
        Landscape,
        Portrait
    }

    public interface IDeviceOrientation
    {
        DeviceOrientations GetOrientation();
    }
}
```

Kodowania dla tego interfejsu w kod udostępniony umożliwi dostęp do orientacji urządzenia interfejsów API na każdej platformie aplikacji platformy Xamarin.Forms.

> [!NOTE]
> Klasy implementującej interfejs musi mieć konstruktora bez parametrów, aby pracować z `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementacja systemu iOS

Interfejs musi zostać wdrożona w każdego projektu specyficznego dla platformy aplikacji. Należy pamiętać, że klasa ma konstruktora bez parametrów, aby `DependencyService` można tworzyć nowych wystąpień:

```csharp
using UIKit;
using Foundation;

namespace DependencyServiceSample.iOS
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation(){ }

        public DeviceOrientations GetOrientation()
        {
            var currentOrientation = UIApplication.SharedApplication.StatusBarOrientation;
            bool isPortrait = currentOrientation == UIInterfaceOrientation.Portrait
                || currentOrientation == UIInterfaceOrientation.PortraitUpsideDown;

            return isPortrait ? DeviceOrientations.Portrait: DeviceOrientations.Landscape;
        }
    }
}
```

Na koniec należy dodać to `[assembly]` atrybut powyżej klasy (i na zewnątrz wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Ten atrybut rejestruje klasę jako implementacja `IDeviceOrientation` interfejsu, co oznacza, że `DependencyService.Get<IDeviceOrientation>` może służyć w kodzie udostępnionej utworzyć jej wystąpienie.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementacja systemu android

Poniższy kod implementuje `IDeviceOrientation` w systemie Android:

```csharp
using DependencyServiceSample.Droid;
using Android.Hardware;

namespace DependencyServiceSample.Droid
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public static void Init() { }

        public DeviceOrientations GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            var rotation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = rotation == SurfaceOrientation.Rotation90 || rotation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientations.Landscape : DeviceOrientations.Portrait;
        }
    }
}
```

Dodaj tę `[assembly]` atrybut powyżej klasy (i na zewnątrz wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Ten atrybut rejestruje klasę jako implementacja `IDeviceOrientaiton` interfejsu, co oznacza, że `DependencyService.Get<IDeviceOrientation>` mogą być używane w udostępnionego kodu można utworzyć jej wystąpienie.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Universal Windows Platform implementacji

Poniższy kod implementuje `IDeviceOrientation` interfejsu na platformie Universal Windows:

```csharp
namespace DependencyServiceSample.WindowsPhone
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public DeviceOrientations GetOrientation()
        {
            var orientation = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            if (orientation == Windows.UI.ViewManagement.ApplicationViewOrientation.Landscape) {
                return DeviceOrientations.Landscape;
            }
            else {
                return DeviceOrientations.Portrait;
            }
        }
    }
}
```

Dodaj `[assembly]` atrybut powyżej klasy (i na zewnątrz wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Ten atrybut rejestruje klasę jako implementacja `DeviceOrientationImplementation` interfejsu, co oznacza, że `DependencyService.Get<IDeviceOrientation>` mogą być używane w udostępnionego kodu można utworzyć jej wystąpienie.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Wdrażanie w udostępnionego kodu

Teraz możemy napisać i przetestować kod udostępniony, który uzyskuje dostęp do `IDeviceOrientation` interfejsu. Ta strona prostego zawiera przycisk, który aktualizuje swój własny tekst, oparty na orientacji urządzenia. Używa ona `DependencyService` wystąpienia `IDeviceOrientation` interfejsu &ndash; w czasie wykonywania tego wystąpienia będzie to implementacja specyficzne dla platformy, które ma pełny dostęp do natywnego zestawu SDK:

```csharp
public MainPage ()
{
    var orient = new Button {
        Text = "Get Orientation",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    orient.Clicked += (sender, e) => {
       var orientation = DependencyService.Get<IDeviceOrientation>().GetOrientation();
       switch(orientation){
           case DeviceOrientations.Undefined:
                orient.Text = "Undefined";
                break;
           case DeviceOrientations.Landscape:
                orient.Text = "Landscape";
                break;
           case DeviceOrientations.Portrait:
                orient.Text = "Portrait";
                break;
       }
    };
    Content = orient;
}
```

Uruchomienie takiej aplikacji na iOS, Android lub platformy Windows i naciśnięcie przycisku powoduje, że w tekst przycisku aktualizowania orientacji urządzenia.

![](device-orientation-images/orientation.png "Przykładowe orientacji urządzenia")


## <a name="related-links"></a>Linki pokrewne

- [Za pomocą DependencyService (przykład)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (przykład)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
