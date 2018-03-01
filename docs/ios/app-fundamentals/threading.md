---
title: "Wątkowość"
ms.topic: article
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8be599f5b6541ef738ffa47a01374fd7f90044a4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="threading"></a>Wątkowość

Środowiska wykonawczego platformy Xamarin.iOS udostępnia deweloperom platformy .NET wątkowość interfejsów API, zarówno jawne za pomocą wątków ( `System.Threading.Thread, System.Threading.ThreadPool`), niejawnie używając wzorce asynchroniczne delegata lub metody BeginXXX, jak również pełny zakres z interfejsów API, które obsługuje zadania Biblioteka równoległych.



Xamarin zdecydowanie zaleca użycie [Biblioteka zadań równoległych](http://msdn.microsoft.com/en-us/library/dd460717.aspx)

 (TPL) do tworzenia aplikacji dla kilku powodów:-domyślny harmonogram TPL będzie delegowane wykonywanie zadań w puli wątków, która z kolei dynamicznie wzrośnie liczba wątków wymagany, ponieważ proces ma miejsce, unikając scenariusz, w którym kończyć się zbyt wiele wątków zapasowych rywalizuje czas procesora CPU. 
-  Łatwiej można traktować operacje w kategoriach TPL zadania. Można łatwo manipulować nimi, były zaplanowane, serializacji ich realizacji lub uruchomić wiele równolegle z bogaty zestaw interfejsów API. 
-  Jest podstawą do programowania za pomocą nowego C# async rozszerzenia językowe. 


Będzie powoli zwiększyć się pula wątków to liczba wątków, zgodnie z potrzebami zależności od liczby rdzeni procesorów dostępnych w systemie, obciążenia systemu i wymaga dane aplikacji. Można użyć tej puli wątków przez wywołanie metody `System.Threading.ThreadPool` lub przy użyciu domyślnej `System.Threading.Tasks.TaskScheduler` (część *równoległych struktur*).

Zwykle deweloperzy za pomocą wątków podczas muszą utworzyć reakcji aplikacji i ich nie chcesz zablokować głównego interfejsu użytkownika, uruchom pętli.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Tworzenie aplikacji reakcji

Dostęp do elementów interfejsu użytkownika powinna być ograniczona do tego samego wątku, który jest uruchomiony w pętli głównej aplikacji. Jeśli chcesz wprowadzić zmiany z wątku głównego interfejsu użytkownika, należy kolejka kodu za pomocą [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), podobnie do następującej:

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    //
    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

Powyższy kod wywołuje kodu wewnątrz delegata w kontekście wątku głównego bez spowodowania, że wszystkie warunki wyścigu, które potencjalnie może ulec awarii aplikacji.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Wątki i wyrzucanie elementów bezużytecznych

W trakcie wykonywania środowiska uruchomieniowego języka Objective-C utworzy i wersji obiektów. Jeśli obiekty są oflagowywane do "auto wersja" środowiska wykonawczego języka Objective-C spowoduje zwolnienie tych obiektów do bieżącego elementu Wątek `NSAutoReleasePool`. Tworzy Xamarin.iOS `NSAutoRelease` puli dla każdego wątku z `System.Threading.ThreadPool` i wątku głównego. Obejmuje to przez rozszerzenie jakieś wątki utworzone przy użyciu domyślnego TaskScheduler w System.Threading.Tasks.

W przypadku utworzenia własnych wątki używające `System.Threading` konieczne podanie własnej `NSAutoRelease` pulę, aby zapobiec przeciekom danych. Aby to zrobić, po prostu zawijać z wątku następujący fragment kodu:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Uwaga: Od Xamarin.iOS 5.2 nie trzeba podać własne `NSAutoReleasePool` już będzie należy podać jedną automatycznie dla Ciebie.


## <a name="related-links"></a>Linki pokrewne

- [Praca z wątku interfejsu użytkownika](~/ios/user-interface/ios-ui/ui-thread.md)
