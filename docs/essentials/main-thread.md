---
title: 'Xamarin.Essentials: MainThread'
description: Klasa MainThread pozwala na uruchamianie kodu w wątku głównego wykonywania aplikacji.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080386"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**MainThread** klasa pozwala na uruchamianie kodu w głównym wątku wykonywania aplikacji i sprawdzić, czy bloku kodu jest obecnie uruchomiony w wątku głównego.

## <a name="background"></a>Tło

Większość systemów operacyjnych — z systemami iOS, Android i platformy uniwersalnej systemu Windows — korzystają z modelu wątkowości pojedynczej dla kodu obejmujące interfejsu użytkownika. Ten model jest poprawnie serializować zdarzeniami interfejsu użytkownika, w tym naciśnięć klawiszy i wprowadzania dotykowego. Ten wątek jest często nazywana _wątku głównego_ lub _wątku interfejsu użytkownika_ lub _wątku interfejsu użytkownika_. Wadą tego modelu jest, że cały kod, który uzyskuje dostęp do elementów interfejsu użytkownika należy uruchomić na wątku głównego aplikacji. 

Aplikacje czasami trzeba używać zdarzenia, które wywołań obsługi zdarzeń na wykonanie wątku dodatkowej. (Klasy Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), i [ `OrientationSensor` ](orientation-sensor.md) wszystkie może zwrócić informacji w dodatkowej wątku, gdy jest używany z prędkości szybsze.) Jeśli program obsługi zdarzeń musi dostęp do elementów interfejsu użytkownika, ten kod musi być uruchomione w głównym wątku. **MainThread** klasa umożliwia aplikacji uruchomić ten kod w głównym wątku.

## <a name="running-code-on-the-main-thread"></a>Uruchamianie kodu w głównym wątku

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Aby uruchomić kod w głównym wątku, należy wywołać statycznych `MainThread.BeginInvokeOnMainThread` metody. Argument jest [ `Action` ](xref:System.Action) obiektu, który jest po prostu metodę z żadnych argumentów i brak wartości zwracanej:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Istnieje również możliwość zdefiniowania oddzielnych metodach dla kodu, który musi zostać uruchomiony w wątku głównego:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Następnie można uruchomić tej metody w głównym wątku przez odwołanie w `BeginInvokeOnMainThread` metody:

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Platformy Xamarin.Forms ma metodę o nazwie [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) wykonuje odpowiednikiem `MainThread.BeginInvokeOnMainThread(Action)`. Za pomocą obu tych metod w aplikacji platformy Xamarin.Forms, rozważ, czy kod wywołujący ma inne potrzeby zależności na platformy Xamarin.Forms. Jeśli nie, `MainThread.BeginInvokeOnMainThread(Action)` prawdopodobnie jest lepszym rozwiązaniem.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Określanie, czy kod jest uruchomiony na wątku głównego

`MainThread` Klasa umożliwia również aplikacją w celu określenia, czy bloku kodu jest uruchomiony w głównym wątku. `IsMainThread` Zwraca `true` Jeśli kod wywoływanie właściwości działa w głównym wątku. Program ta właściwość służy do uruchomienia kodu innego wątku głównego lub dodatkowej wątku:

```csharp
if (MainThread.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

Może zastanawiasz się, jeśli należy sprawdzić, czy kod jest uruchomiony w wątku dodatkowej przed wywołaniem `BeginInvokeOnMainThread`, na przykład, podobnie do następującej:

```csharp
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Może podejrzewać, że ten test może zwiększyć wydajność, jeśli blok kodu jest już uruchomiona w wątku głównego.

_Jednak ten test nie jest konieczne._ Implementacje platformy `BeginInvokeOnMainThread` samodzielnie sprawdzić, czy połączenie jest nawiązywane w głównym wątku. Jest bardzo mały wpływ na wydajność, jeśli wywołujesz `BeginInvokeOnMainThread` Jeżeli nie jest to naprawdę konieczne.

## <a name="api"></a>interfejs API

- [Kod źródłowy MainThread](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [Dokumentacja interfejsu API MainThread](xref:Xamarin.Essentials.MainThread)
