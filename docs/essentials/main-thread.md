---
title: 'Xamarin.Essentials: MainThread'
description: Klasa MainThread umożliwia uruchamianie kodu w wątku głównym wykonywania aplikacji.
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831428"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![NuGet w wersji wstępnej](~/media/shared/pre-release.png)

**MainThread** klasa umożliwia uruchamianie kodu w głównym wątku wykonywania aplikacji i do określenia, czy określonego bloku kodu jest obecnie uruchomiona w wątku głównym.

## <a name="background"></a>Tło

Większość systemów operacyjnych — w tym systemów iOS, Android i platformy uniwersalnej Windows — używają modelu wątkowości pojedynczego kodu dotyczące interfejsu użytkownika. Ten model jest prawidłowo serializacji zdarzenia interfejsu użytkownika, w tym naciśnięcia klawiszy i wprowadzania dotykowego. Ten wątek jest często określany mianem _wątku głównego_ lub _wątku interfejsu użytkownika_ lub _wątku interfejsu użytkownika_. Wadą tego modelu to, że cały kod, który uzyskuje dostęp do elementów interfejsu użytkownika, należy uruchomić na wątku głównego aplikacji. 

Czasami konieczne aplikacji korzystanie ze zdarzeń, które wywołują procedurę obsługi zdarzeń dla pomocniczego wątku wykonywania. (Klasy Xamarin.Essentials [ `Accelerometer` ](accelerometer.md), [ `Compass` ](compass.md), [ `Gyroscope` ](gyroscope.md), [ `Magnetometer` ](magnetometer.md), i [ `OrientationSensor` ](orientation-sensor.md) wszystkie może zwrócić informacje na wątek pomocniczy, gdy jest używane z szybkość.) Jeśli program obsługi zdarzeń musi uzyskać dostęp do elementów interfejsu użytkownika, kod musi być uruchomione w wątku głównym. **MainThread** klasa umożliwia aplikacji uruchomić ten kod w wątku głównym.

## <a name="running-code-on-the-main-thread"></a>Uruchamianie kodu w głównym wątku

Dodaj odwołanie do Xamarin.Essentials w klasie:

```csharp
using Xamarin.Essentials;
```

Aby uruchomić kod w głównym wątku, wywołaj statyczną `MainThread.BeginInvokeOnMainThread` metody. Argument jest [ `Action` ](xref:System.Action) obiektu, który jest po prostu metodę, bez argumentów i nie zwraca wartości:

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

Istnieje również możliwość definiowania oddzielne metody dla kodu, który należy uruchomić na wątku głównego:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

Następnie możesz uruchomić tę metodę w wątku głównym, odwołując się do jej w `BeginInvokeOnMainThread` metody:

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Zestaw narzędzi Xamarin.Forms ma metodę o nazwie [ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) , działa tak samo jak `MainThread.BeginInvokeOnMainThread(Action)`. Podczas korzystania z jednej z metod, w aplikacji platformy Xamarin.Forms, należy rozważyć, czy kod wywołujący ma inne potrzeby zależność zestawu narzędzi Xamarin.Forms. W przeciwnym razie `MainThread.BeginInvokeOnMainThread(Action)` prawdopodobnie jest lepszym rozwiązaniem.

## <a name="determining-if-code-is-running-on-the-main-thread"></a>Określanie, czy kod jest uruchomiony na główny wątek

`MainThread` Klasy umożliwia także aplikację, aby określić, czy określonego bloku kodu jest uruchomiona w wątku głównym. `IsMainThread` Właściwość zwraca `true` Jeśli kodu, wywoływanie właściwości jest uruchomiona w wątku głównym. Program można użyć tej właściwości, aby uruchomić inny kod dla głównego wątku lub wątek pomocniczy:

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

Być może zastanawiasz się, jeśli należy sprawdzić, czy kod jest uruchomiony na wątek pomocniczy przed wywołaniem `BeginInvokeOnMainThread`, na przykład następująco:

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

Może być podejrzewać, że ten test może zwiększyć wydajność, jeśli blok kodu jest już uruchomiona w wątku głównym.

_Jednak ten test nie jest konieczne._ Implementacje platformy `BeginInvokeOnMainThread` samodzielnie sprawdzić, jeśli połączenie jest nawiązywane w wątku głównym. Jest bardzo mały spadek wydajności, jeśli wywołujesz `BeginInvokeOnMainThread` gdy nie jest to naprawdę konieczne.

## <a name="api"></a>interfejs API

- [Kod źródłowy MainThread](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [Dokumentacja interfejsu API MainThread](xref:Xamarin.Essentials.MainThread)
