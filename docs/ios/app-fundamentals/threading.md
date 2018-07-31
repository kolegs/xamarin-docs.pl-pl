---
title: Wątki w rozszerzeniu Xamarin.iOS
description: Ten dokument zawiera informacje dotyczące korzystania z interfejsów API System.Threading aplikacji platformy Xamarin.iOS. Omówiono w nim Biblioteka zadań równoległych, tworzenie szybko reagujących aplikacji i wyrzucania elementów bezużytecznych.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 8e4ee10fdabdcbb4c6cefe02b15dc93459708364
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350423"
---
# <a name="threading-in-xamarinios"></a>Wątki w rozszerzeniu Xamarin.iOS

Środowisko uruchomieniowe platformy Xamarin.iOS zapewnia deweloperom dostęp do programu .NET wątkowości interfejsów API, zarówno jawnie przy użyciu wątków (`System.Threading.Thread, System.Threading.ThreadPool`) i niejawnie przy użyciu wzorców asynchronicznych delegata lub metody BeginXXX, a także pełnego zakresu z interfejsów API służących do obsługi Biblioteka zadań równoległych.



Xamarin zdecydowanie zaleca używanie [Biblioteka zadań równoległych](http://msdn.microsoft.com/library/dd460717.aspx) (TPL) do tworzenia aplikacji z kilku powodów:
-  Domyślnego harmonogramu TPL oddeleguje wykonywanie zadań w puli wątków, które z kolei dynamicznie rośnie liczba wątków, wymagany, ponieważ proces ma miejsce, unikając scenariusz, w której znajdą rywalizując o czas procesora CPU zbyt wiele wątków jednocześnie. 
-  Łatwiej myśleć o operacji w kontekście zadania TPL. Można łatwo manipulować nimi, zaplanować ich, serializować ich wykonania lub uruchomienia wielu równolegle z bogaty zestaw interfejsów API. 
-  Jest podstawą dla programowania, korzystając z nowego języka C# async rozszerzenia językowe. 


Pula wątków powoli rośnie liczba wątków, zgodnie z potrzebami na podstawie liczby rdzeni procesora CPU jest dostępne w systemie, obciążenia systemu i wymagań aplikacji. Możesz użyć tej puli wątków przez wywołanie metody `System.Threading.ThreadPool` lub przy użyciu domyślnej `System.Threading.Tasks.TaskScheduler` (część *środowisk równoległych*).

Zazwyczaj Deweloperzy używają wątków, gdy muszą utworzyć czasu reakcji aplikacji, a nie chcą zablokować głównego interfejsu użytkownika, uruchom pętli.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Tworzenie szybko reagujących aplikacji

Dostęp do elementów interfejsu użytkownika powinny być ograniczone do tego samego wątku, który jest uruchomiony w pętli głównej aplikacji. Jeśli chcesz wprowadzić zmiany z wątku głównego interfejsu użytkownika, należy kolejki kodu za pomocą [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), podobnie do następującego:

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

Powyższe wywołuje kod wewnątrz delegata w kontekście wątku głównego bez powodowania żadnych Sytuacje wyścigu, które potencjalnie mogło powodować awarię aplikacji.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Wątki i wyrzucania elementów bezużytecznych

W trakcie wykonywania środowisko uruchomieniowe języka Objective-C utworzysz i wersji obiektów. Jeśli obiekty są oflagowywane do "auto wersja" środowiska uruchomieniowego języka Objective-C spowoduje zwolnienie tych obiektów do wątku w bieżącym `NSAutoReleasePool`. Xamarin.iOS tworzony jest jeden `NSAutoRelease` puli dla każdego wątku z `System.Threading.ThreadPool` i wątku głównego. Przy użyciu rozszerzenia obejmują żadnych wątków, utworzone w System.Threading.Tasks przy użyciu domyślnego harmonogramu zadań systemu.

Jeśli tworzysz własny wątki używające `System.Threading` należy podać, jesteś właścicielem `NSAutoRelease` puli, aby zapobiec wyciekowi danych. Aby to zrobić, po prostu opakować wątek w poniższym fragmentem kodu:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Uwaga: Od Xamarin.iOS 5.2 nie należy podać własne `NSAutoReleasePool` już będzie należy podać jedną automatycznie dla Ciebie.


## <a name="related-links"></a>Linki pokrewne

- [Praca z wątkiem interfejsu użytkownika](~/ios/user-interface/ios-ui/ui-thread.md)
