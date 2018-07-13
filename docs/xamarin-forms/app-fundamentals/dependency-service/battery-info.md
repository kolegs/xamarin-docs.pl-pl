---
title: Sprawdzanie stanu baterii
description: W tym artykule wyjaśniono, jak korzystać z zestawu narzędzi Xamarin.Forms DependencyService klasy dostępu do informacji baterii natywnie dla każdej platformy.
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: cbb4a01ac2c6d933fe40a0b3c2571d1fe3ce75c0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998403"
---
# <a name="checking-battery-status"></a>Sprawdzanie stanu baterii

W tym artykule opisano proces tworzenia aplikacji, która umożliwia sprawdzenie stanu baterii. W tym artykule jest na podstawie wtyczki baterii James Montemagno. Aby uzyskać więcej informacji, zobacz [repozytorium GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery).

Ponieważ Xamarin.Forms nie zawiera funkcji sprawdzania bieżący stan baterii, tej aplikacji będą musieli używać [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) z zalet natywnych interfejsów API.  W tym artykule opisano następujące kroki dotyczące korzystania z `DependencyService`:

- **[Tworzenie interfejsu](#Creating_the_Interface)**  &ndash; zrozumieć, jak interfejs jest tworzony w współużytkowanym kodem.
- **[iOS implementacji](#iOS_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu iOS.
- **[Implementacja systemu android](#Android_Implementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym dla systemu Android.
- **[Universal Windows Platform implementacji](#UWPImplementation)**  &ndash; Dowiedz się, jak implementować interfejs w kodzie natywnym do uniwersalnej platformy Windows (UWP).
- **[Implementacja w kodzie udostępnionego](#Implementing_in_Shared_Code)**  &ndash; Dowiedz się, jak używać `DependencyService` do wywołania w natywnych implementacji ze współużytkowanym kodem.

Po zakończeniu aplikacji przy użyciu `DependencyService` mają następującą strukturę:

![](battery-info-images/battery-diagram.png "DependencyService struktury aplikacji")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Tworzenie interfejsu

Najpierw Utwórz interfejs udostępnionego kodu, który wyraża odpowiednich funkcji. W przypadku baterii kontroli aplikacji procent pozostałej baterii jest istotne informacje, czy urządzenie jest ładowana, czy nie, i jak urządzenie otrzymuje zasilania:

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

Kodowania dla tego interfejsu w kod udostępniony umożliwi dostęp do interfejsów API zarządzania energią na każdej platformie aplikacji platformy Xamarin.Forms.

> [!NOTE]
> Klasy implementującej interfejs musi mieć konstruktora bez parametrów, aby pracować z `DependencyService`. Interfejsy nejde definovat konstruktory.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementacja systemu iOS

`IBattery` Interfejs musi zostać wdrożone w każdym projekcie specyficzne dla platformy aplikacji. Implementacja systemu iOS będzie używać natywnych [ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/) interfejsów API, aby dostęp do informacji baterii. Należy zauważyć, że następujące klasy ma konstruktora bez parametrów, aby `DependencyService` można tworzyć nowych wystąpień:

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

Na koniec należy dodać to `[assembly]` atrybut powyżej klasy (i na zewnątrz wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

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

Ten atrybut rejestruje klasę jako implementacja `IBattery` interfejsu, co oznacza, że `DependencyService.Get<IBattery>` może służyć w kodzie udostępnionej utworzyć jej wystąpienie:

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementacja systemu android

Implementacja systemu Android używa [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) interfejsu API. Ta implementacja jest bardziej skomplikowane niż wersja systemu iOS, wymagających kontroli w celu obsługi braku uprawnień baterii:

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

Dodaj tę `[assembly]` atrybut powyżej klasy (i na zewnątrz wszystkie przestrzenie nazw, które zostały zdefiniowane), w tym wszelkie wymagane `using` instrukcji:

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

Ten atrybut rejestruje klasę jako implementacja `IBattery` interfejsu, co oznacza, że `DependencyService.Get<IBattery>` mogą być używane w udostępnionego kodu można utworzyć jej wystąpienie.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>Universal Windows Platform implementacji

Implementacja platformy uniwersalnej systemu Windows używa `Windows.Devices.Power` interfejsy API w celu uzyskania informacji o stanie baterii:

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

`[assembly]` Atrybut powyżej deklaracji namespace rejestruje klasę jako implementacja `IBattery` interfejsu, co oznacza, że `DependencyService.Get<IBattery>` może służyć w kodzie udostępnionej utworzyć jej wystąpienie.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Wdrażanie w udostępnionego kodu

Teraz, gdy interfejs został zaimplementowany dla każdej platformy, udostępniony aplikacja może być napisana w taki sposób, aby z niej korzystać. Aplikacja będzie zawierać strony z przyciskiem, że w przypadku nacisnął aktualizacje jego tekstu o bieżący stan baterii. Używa ona `DependencyService` wystąpienia `IBattery` interfejsu. W czasie wykonywania to wystąpienie będzie implementacji specyficznych dla platformy, która ma pełny dostęp do natywnego zestawu SDK.

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

Ta aplikacja działająca w systemie iOS, Android i platformy uniwersalnej systemu Windows i naciskając przycisk spowoduje tekst przycisku aktualizowanie w celu odzwierciedlenia bieżącego stanu zasilania urządzenia.

![](battery-info-images/battery.png "Przykładowe stan baterii")


## <a name="related-links"></a>Linki pokrewne

- [DependencyService (przykład)](https://developer.xamarin.com/samples/DependencyService)
- [Za pomocą DependencyService (przykład)](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Przykłady zestawu narzędzi Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
