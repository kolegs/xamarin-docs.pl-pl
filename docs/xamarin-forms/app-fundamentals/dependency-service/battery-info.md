---
title: Sprawdzanie stanu baterii
description: W tym artykule opisano sposób użycia klasy DependencyService platformy Xamarin.Forms dostępu do informacji baterii natywnie dla każdej platformy.
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 74e191cd6a87626e887d45f823e65d57000d7463
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241087"
---
# <a name="checking-battery-status"></a>Sprawdzanie stanu baterii

W tym artykule przedstawiono tworzenie aplikacji, która umożliwia sprawdzenie stanu baterii. W tym artykule zależy od wtyczki baterii przez James Montemagno. Aby uzyskać więcej informacji, zobacz [repozytorium GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery).

Ponieważ platformy Xamarin.Forms nie obejmuje funkcjonalności sprawdzania z bieżącym stanem baterii, tej aplikacji będą musieli używać [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) mógł korzystać z natywnych interfejsów API.  W tym artykule opisano następujące kroki do używania `DependencyService`:

- **[Tworzenie interfejsu](#Creating_the_Interface)**  &ndash; zrozumieć sposób tworzenia interfejsu w kodzie udostępnionego.
- **[iOS implementacji](#iOS_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu iOS.
- **[Implementacja systemu android](#Android_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Android.
- **[Uniwersalny implementacji platformy Windows](#UWPImplementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla uniwersalnych platformy systemu Windows (UWP).
- **[Wdrażanie w kodzie udostępnionego](#Implementing_in_Shared_Code)**  &ndash; Dowiedz się, jak używać `DependencyService` do wywołania do implementacji native z udostępnionego kodu.

Po zakończeniu aplikacji przy użyciu `DependencyService` będzie mieć następującą strukturę:

![](battery-info-images/battery-diagram.png "Struktura aplikacji DependencyService")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Najpierw Utwórz interfejs w kodzie udostępnionego, który określa informacji dotyczących odpowiednich funkcji. W przypadku baterii kontroli aplikacji istotne informacje jest procent pozostałej pracy baterii, czy urządzenie jest ładowania lub nie, i jak urządzenie odbiera zasilania:

```csharp
namespace DependencyServiceSample
{
  public enum BatteryStatus
  {
    Charging,
    Discharging,
    Full,
    NotCharging,
    Unknown
  }

  public enum PowerSource
  {
    Battery,
    Ac,
    Usb,
    Wireless,
    Other
  }

  public interface IBattery
  {
    int RemainingChargePercent { get; }
    BatteryStatus Status { get; }
    PowerSource PowerSource { get; }
  }
}
```

Kodowanie tego interfejsu w kodzie udostępnione umożliwi aplikacji platformy Xamarin.Forms na dostęp do zarządzania energią interfejsów API w każdej z platform.

> [!NOTE]
> Klasy implementującej interfejs musi mieć konstruktora bez parametrów, aby pracować z `DependencyService`. Nie można definiować konstruktorów interfejsów.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementacja systemu iOS

`IBattery` Interfejs musi zostać wdrożone w każdym projekcie specyficzne dla platformy aplikacji. Implementacja systemu iOS będzie używać natywnego [ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/) interfejsy API w celu dostępu do informacji o baterii. Należy zauważyć, że następujące klasy ma konstruktora bez parametrów, aby `DependencyService` można tworzyć nowych wystąpień:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;

namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery
  {
    public BatteryImplementation()
    {
      UIDevice.CurrentDevice.BatteryMonitoringEnabled = true;
    }

    public int RemainingChargePercent
    {
      get
      {
        return (int)(UIDevice.CurrentDevice.BatteryLevel * 100F);
      }
    }

    public BatteryStatus Status
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return BatteryStatus.Charging;
          case UIDeviceBatteryState.Full:
            return BatteryStatus.Full;
          case UIDeviceBatteryState.Unplugged:
            return BatteryStatus.Discharging;
          default:
            return BatteryStatus.Unknown;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Full:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Unplugged:
            return PowerSource.Battery;
          default:
            return PowerSource.Other;
        }
      }
    }
  }
}
```

Na koniec należy dodać to `[assembly]` atrybutu powyżej klasy (i poza wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;//necessary for registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery {
    ...
```

Ten atrybut rejestruje klasę jako implementacja `IBattery` interfejsu, co oznacza, że `DependencyService.Get<IBattery>` można użyć w kodzie udostępniony można utworzyć wystąpienie:

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementacja systemu android

Android implementacja używa [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) interfejsu API. Ta implementacja jest bardziej złożone niż wersja systemu iOS, wymagających kontroli w celu obsługi braku uprawnień baterii:

```csharp
using System;
using Android;
using Android.Content;
using Android.App;
using Android.OS;
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid;

namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery
  {
    private BatteryBroadcastReceiver batteryReceiver;
    public BatteryImplementation() { }

    public int RemainingChargePercent
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              var level = battery.GetIntExtra(BatteryManager.ExtraLevel, -1);
              var scale = battery.GetIntExtra(BatteryManager.ExtraScale, -1);

              return (int)Math.Floor(level * 100D / scale);
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }

      }
    }

    public DependencyServiceSample.BatteryStatus Status
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;
              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);
              if (isCharging)
                return DependencyServiceSample.BatteryStatus.Charging;

              switch(status)
              {
                case (int)BatteryStatus.Charging:
                  return DependencyServiceSample.BatteryStatus.Charging;
                case (int)BatteryStatus.Discharging:
                  return DependencyServiceSample.BatteryStatus.Discharging;
                case (int)BatteryStatus.Full:
                  return DependencyServiceSample.BatteryStatus.Full;
                case (int)BatteryStatus.NotCharging:
                  return DependencyServiceSample.BatteryStatus.NotCharging;
                default:
                  return DependencyServiceSample.BatteryStatus.Unknown;
              }
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;

              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);

              if (!isCharging)
                return DependencyServiceSample.PowerSource.Battery;
              else if (usbCharge)
                return DependencyServiceSample.PowerSource.Usb;
              else if (acCharge)
                return DependencyServiceSample.PowerSource.Ac;
              else if (wirelessCharge)
                return DependencyServiceSample.PowerSource.Wireless;
              else
                return DependencyServiceSample.PowerSource.Other;
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }
  }
}
```

Dodaj tę `[assembly]` atrybutu powyżej klasy (i poza wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

```csharp
...
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery {
    ...
```

Ten atrybut rejestruje klasę jako implementacja `IBattery` interfejsu, co oznacza, że `DependencyService.Get<IBattery>` mogą być używane w udostępnionej kodu można utworzyć wystąpienie.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementacja platformy uniwersalnej systemu Windows

Implementacja platformy uniwersalnej systemu Windows używa `Windows.Devices.Power` interfejsów API, aby uzyskać informacje o stanie baterii:

```csharp
using DependencyServiceSample.UWP;
using Xamarin.Forms;

[assembly: Dependency(typeof(BatteryImplementation))]
namespace DependencyServiceSample.UWP
{
    public class BatteryImplementation : IBattery
    {
        private BatteryStatus status = BatteryStatus.Unknown;
        Windows.Devices.Power.Battery battery;

        public BatteryImplementation()
        {
        }

        private Windows.Devices.Power.Battery DefaultBattery
        {
            get
            {
                return battery ?? (battery = Windows.Devices.Power.Battery.AggregateBattery);
            }
        }

        public int RemainingChargePercent
        {
            get
            {
                var finalReport = DefaultBattery.GetReport();
                var finalPercent = -1;

                if (finalReport.RemainingCapacityInMilliwattHours.HasValue && finalReport.FullChargeCapacityInMilliwattHours.HasValue)
                {
                    finalPercent = (int)((finalReport.RemainingCapacityInMilliwattHours.Value /
                        (double)finalReport.FullChargeCapacityInMilliwattHours.Value) * 100);
                }
                return finalPercent;
            }
        }

        public BatteryStatus Status
        {
            get
            {
                var report = DefaultBattery.GetReport();
                var percentage = RemainingChargePercent;

                if (percentage >= 1.0)
                {
                    status = BatteryStatus.Full;
                }
                else if (percentage < 0)
                {
                    status = BatteryStatus.Unknown;
                }
                else
                {
                    switch (report.Status)
                    {
                        case Windows.System.Power.BatteryStatus.Charging:
                            status = BatteryStatus.Charging;
                            break;
                        case Windows.System.Power.BatteryStatus.Discharging:
                            status = BatteryStatus.Discharging;
                            break;
                        case Windows.System.Power.BatteryStatus.Idle:
                            status = BatteryStatus.NotCharging;
                            break;
                        case Windows.System.Power.BatteryStatus.NotPresent:
                            status = BatteryStatus.Unknown;
                            break;
                    }
                }
                return status;
            }
        }

        public PowerSource PowerSource
        {
            get
            {
                if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
                {
                    return PowerSource.Ac;
                }
                return PowerSource.Battery;
            }
        }
    }
}
```

`[assembly]` Atrybutu powyżej deklaracji namespace rejestruje klasę jako implementacja `IBattery` interfejsu, co oznacza, że `DependencyService.Get<IBattery>` można użyć w kodzie udostępniony można utworzyć wystąpienie.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementacja w kodzie udostępnionego

Teraz, gdy interfejs został zaimplementowany dla każdej platformy, aby móc korzystać z niego można pisać udostępnionej aplikacji. Aplikacja będzie zawierać strony z przyciskiem, że w przypadku wybrania aktualizacji jego tekstu z bieżącym stanem baterii. Używa `DependencyService` można pobrać wystąpienia `IBattery` interfejsu. W czasie wykonywania to wystąpienie będzie implementację specyficzne dla platformy, która ma pełny dostęp do natywnych zestawu SDK.

```csharp
public MainPage ()
{
    var button = new Button {
        Text = "Click for battery info",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    button.Clicked += (sender, e) => {
        var bat = DependencyService.Get<IBattery>();

        switch (bat.PowerSource){
          case PowerSource.Battery:
            button.Text = "Battery - ";
            break;
          case PowerSource.Ac:
            button.Text = "AC - ";
            break;
          case PowerSource.Usb:
            button.Text = "USB - ";
            break;
          case PowerSource.Wireless:
            button.Text = "Wireless - ";
            break;
          case PowerSource.Other:
          default:
            button.Text = "Other - ";
            break;
        }
        switch (bat.Status){
          case BatteryStatus.Charging:
            button.Text += "Charging";
            break;
          case BatteryStatus.Discharging:
            button.Text += "Discharging";
            break;
          case BatteryStatus.NotCharging:
            button.Text += "Not Charging";
            break;
          case BatteryStatus.Full:
            button.Text += "Full";
            break;
          case BatteryStatus.Unknown:
          default:
            button.Text += "Unknown";
            break;
        }
    };
    Content = button;
}
```

Ta aplikacja jest uruchamiana w systemie iOS, Android, lub platformy uniwersalnej systemu Windows i naciśnięcie przycisku spowoduje tekst przycisku aktualizacji w celu uwzględnienia bieżący stan zasilania urządzenia.

![](battery-info-images/battery.png "Przykładowe stan baterii")


## <a name="related-links"></a>Linki pokrewne

- [DependencyService (przykład)](https://developer.xamarin.com/samples/DependencyService)
- [Przy użyciu DependencyService (przykład)](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
