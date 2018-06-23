---
title: Platforma Xamarin.Essentials
description: Klasa platformy pozwala na uruchamianie kodu w wątku głównego wykonywania aplikacji.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E0
author: charlespetzold
ms.author: chape
ms.date: 06/03/2018
ms.openlocfilehash: 82e248ee702e104dff98b342aec72179273fc34f
ms.sourcegitcommit: d9ecac62bcf743aff52408fbbd09c5509921a000
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36329938"
---
# <a name="xamarinessentials-platform"></a>Platforma Xamarin.Essentials

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**Platformy** klasa pozwala na uruchamianie kodu w głównym wątku wykonywania aplikacji i sprawdzić, czy bloku kodu jest obecnie uruchomiony w wątku głównego.

## <a name="background"></a>Tło

Większość systemów operacyjnych — z systemami iOS, Android i platformy uniwersalnej systemu Windows — korzystają z modelu wątkowości pojedynczej dla kodu obejmujące interfejsu użytkownika. Ten model jest poprawnie serializować zdarzeniami interfejsu użytkownika, w tym naciśnięć klawiszy i wprowadzania dotykowego. Ten wątek jest często nazywana _wątku głównego_ lub _wątku interfejsu użytkownika_ lub _wątku interfejsu użytkownika_. Wadą tego modelu jest, że cały kod, który uzyskuje dostęp do elementów interfejsu użytkownika należy uruchomić na wątku głównego aplikacji. 

Aplikacje czasami trzeba używać zdarzenia, które wywołań obsługi zdarzeń na wykonanie wątku dodatkowej. (Klasy Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), i [ `OrientationSensor` ](orientation-sensor.md) wszystkie może zwrócić informacji w dodatkowej wątku, gdy jest używany z prędkości szybsze.) Jeśli program obsługi zdarzeń musi dostęp do elementów interfejsu użytkownika, ten kod musi być uruchomione w głównym wątku. **Platformy** klasa umożliwia aplikacji uruchomić ten kod w głównym wątku.

## <a name="running-code-on-the-main-thread"></a>Uruchamianie kodu w głównym wątku

Dodaj odwołanie do Xamarin.Essentials w swojej klasy:

```csharp
using Xamarin.Essentials;
```

Aby uruchomić kod w głównym wątku, należy wywołać statycznych `Platform.BeginInvokeOnMainThread` metody. Argument jest [ `Action` ](xref:System.Action) obiektu, który jest po prostu metodę z żadnych argumentów i brak wartości zwracanej:

```csharp
Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Aby wywołać tę metodę w aplikacji platformy Xamarin.Forms w obrębie klasy, która jest pochodną `Element` (która obejmuje wszystkie klasy, która pochodzi z `View` lub `Page`), `Platform` Nazwa klasy musi być w pełni kwalifikowana z Nazwa przestrzeni nazw:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(() =>
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
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Platformy Xamarin.Forms ma metodę o nazwie [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) wykonuje odpowiednikiem `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)`. Za pomocą obu tych metod w aplikacji platformy Xamarin.Forms, rozważ, czy kod wywołujący ma inne potrzeby zależności na platformy Xamarin.Forms. Jeśli nie, `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)` prawdopodobnie jest lepszym rozwiązaniem.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Określanie, czy kod jest uruchomiony na wątku głównego

`Platform` Klasa umożliwia również aplikacją w celu określenia, czy bloku kodu jest uruchomiony w głównym wątku. `IsMainThread` Zwraca `true` Jeśli kod wywoływanie właściwości działa w głównym wątku. Program ta właściwość służy do uruchomienia kodu innego wątku głównego lub dodatkowej wątku:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
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
if (Xamarin.Essentials.Platform.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

Może podejrzewać, że ten test może zwiększyć wydajność, jeśli blok kodu jest już uruchomiona w wątku głównego.

_Jednak ten test nie jest konieczne._ Implementacje platformy `BeginInvokeOnMainThread` samodzielnie sprawdzić, czy połączenie jest nawiązywane w głównym wątku. Jest bardzo mały wpływ na wydajność, jeśli wywołujesz `BeginInvokeOnMainThread` Jeżeli nie jest to naprawdę konieczne.

## <a name="api"></a>interfejs API

- [Kod źródłowy platformy](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Platform)
- [Dokumentacja interfejsu API platformy](xref:Xamarin.Essentials.Platform)
