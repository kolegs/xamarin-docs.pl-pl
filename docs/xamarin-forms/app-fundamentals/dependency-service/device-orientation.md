---
title: Sprawdzanie, czy orientacji urządzenia
description: Umożliwia DependencyService orientacji urządzenia dostępu z udostępnionego kodu
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: b8392dad578f94380e90da24cbf44120d38f754d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="checking-device-orientation"></a>Sprawdzanie, czy orientacji urządzenia

Ten artykuł przeprowadzi Cię do używania [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) do sprawdzenia orientacji urządzenia z udostępnionego kodu przy użyciu macierzystych interfejsów API na każdej z platform. Ten przewodnik jest oparty na istniejące `DeviceOrientation` wtyczki według Ali Özgür. Zobacz [repozytorium GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) Aby uzyskać więcej informacji.

- **[Tworzenie interfejsu](#Creating_the_Interface)**  &ndash; zrozumieć, jak interfejsu jest tworzony w kodzie udostępnionego.
- **[iOS implementacji](#iOS_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu iOS.
- **[Implementacja systemu android](#Android_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Android.
- **[Implementacja systemu Windows](#WindowsImplementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla Windows Phone i Windows platformy Uniwersalnej.
- **[Wdrażanie w kodzie udostępnionego](#Implementing_in_Shared_Code)**  &ndash; Dowiedz się, jak używać `DependencyService` do wywołania do implementacji native z udostępnionego kodu.

Aplikacji przy użyciu `DependencyService` będzie mieć następującą strukturę:

![](device-orientation-images/orientation-diagram.png "Struktura aplikacji DependencyService")

> [!NOTE]
> Istnieje możliwość wykrywania, czy urządzenie jest w orientacji pionowej lub poziomej w kodzie udostępnionego, jak pokazano w [Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation urządzenia). Metody opisanej w tym artykule używa funkcji macierzystego, aby uzyskać więcej informacji na temat orientacji, w tym, czy urządzenie jest odwrócony.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Najpierw Utwórz interfejs w kodzie udostępnionego, który określa funkcje, które planujesz wdrożyć. Na przykład interfejs zawiera jedną metodę:

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

Kodowanie tego interfejsu w kodzie udostępnione umożliwi orientacji urządzenia interfejsów API na każdej platformie dostępu do aplikacji platformy Xamarin.Forms.

> [!NOTE]
> Klasy implementującej interfejs musi mieć konstruktora bez parametrów, aby pracować z `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementacja systemu iOS

Interfejs musi być implementowana w każdym projekcie specyficzne dla platformy aplikacji. Należy pamiętać, że klasa ma konstruktora bez parametrów, aby `DependencyService` można tworzyć nowych wystąpień:

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

Na koniec należy dodać to `[assembly]` atrybutu powyżej klasy (i poza wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Ten atrybut rejestruje klasę jako implementacja `IDeviceOrientation` interfejsu, co oznacza, że `DependencyService.Get<IDeviceOrientation>` można użyć w kodzie udostępniony można utworzyć wystąpienie.

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

Dodaj tę `[assembly]` atrybutu powyżej klasy (i poza wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Ten atrybut rejestruje klasę jako implementacja `IDeviceOrientaiton` interfejsu, co oznacza, że `DependencyService.Get<IDeviceOrientation>` mogą być używane w udostępnionej kodu można utworzyć wystąpienie.

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone i implementacji platformy uniwersalnej systemu Windows

Poniższy kod implementuje `IDeviceOrientation` interfejsu Windows Phone i platformy uniwersalnej systemu Windows:

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

Dodaj `[assembly]` atrybutu powyżej klasy (i poza wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Ten atrybut rejestruje klasę jako implementacja `DeviceOrientationImplementation` interfejsu, co oznacza, że `DependencyService.Get<IDeviceOrientation>` mogą być używane w udostępnionej kodu można utworzyć wystąpienie.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementacja w kodzie udostępnionego

Teraz możemy zapisać i testowania kodu udostępnionego, który uzyskuje dostęp do `IDeviceOrientation` interfejsu. To prosta strona zawiera przycisk, która aktualizuje własnego tekstu opartych na orientacji urządzenia. Używa `DependencyService` można pobrać wystąpienia `IDeviceOrientation` interfejsu &ndash; w czasie wykonywania tego wystąpienia będzie implementację specyficzne dla platformy, która ma pełny dostęp do natywnych SDK:

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

Uruchomienie takiej aplikacji systemu iOS, Android lub platformy systemu Windows i naciśnięcie przycisku spowoduje tekst przycisku aktualizowanie orientacji urządzenia.

![](device-orientation-images/orientation.png "Przykładowe orientacji urządzenia")


## <a name="related-links"></a>Linki pokrewne

- [Przy użyciu DependencyService (przykład)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (przykład)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
