---
title: Skanowanie w poszukiwaniu odciski palców
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/23/2016
ms.openlocfilehash: 2b20402fd2cd6782247fb2c62513d7aff43a95a6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="scanning-for-fingerprints"></a>Skanowanie w poszukiwaniu odciski palców

Teraz, gdy zaobserwowano sposobu przygotowania aplikacji platformy Xamarin.Android odcisku palca uwierzytelniania umożliwia powrót do `FingerprintManager.Authenticate` metody i przedyskutować jego miejsce w uwierzytelnianiu odcisk palca dla systemu Android w wersji 6.0. Szybki przegląd przepływu pracy dla odcisku palca uwierzytelniania jest opisane w tej listy:

1. Wywołanie `FingerprintManager.Authenticate`, przechodzącą `CryptoObject` i `FingerprintManager.AuthenticationCallback` wystąpienia. `CryptoObject` Jest używany do zapewniania, że wynik uwierzytelniania odcisk palca nie została zmieniona. 
2. Podklasy [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) klasy. Wystąpienie tej klasy, które zostaną dostarczone do `FingerprintManager` po odcisków palców uruchamia uwierzytelniania. Po zakończeniu operacji linii papilarnych wywoła jednej z metod wywołania zwrotnego dla tej klasy.
3. Napisz kod umożliwiający aktualizowanie interfejsu użytkownika, aby umożliwić użytkownikowi wiedział, że urządzenie rozpoczęto linii papilarnych, a następnie czeka na interakcji z użytkownikiem. 
4. Po zakończeniu linii papilarnych Android zwraca wyniki do aplikacji przez wywołanie metody na `FingerprintManager.AuthenticationCallback` wystąpienie, które podano w poprzednim kroku.
5. Aplikacja poinformowania użytkownika o wynikach odcisku palca uwierzytelniania i reagowania na wyniki, zależnie od potrzeb. 

Poniższy fragment kodu przedstawiono przykładowy metody w działaniu, które rozpocznie skanowanie w poszukiwaniu odcisków palców:

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Omówimy każdego z tych parametrów w `Authenticate` metody nieco bardziej szczegółowo:

* Pierwszym parametrem jest _kryptograficznego_ obiekt, czy uwierzytelnienie wyniki skanowania odcisk palca użyje linii papilarnych. Ten obiekt może być `null`, w takim przypadku aplikacja ma ślepo zaufania, że żaden element nie zmienił się z wynikami linii papilarnych. Zalecane jest `CryptoObject` można utworzyć wystąpienia i dostępne do `FingerprintManager` zamiast wartości null. [Tworzenie CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) szczegółowo objaśnia sposób tworzenia wystąpienia `CryptoObject` na podstawie `Cipher`.
* Drugi parametr jest zawsze zero. Android dokumentacja identyfikuje to jako zestaw flag i jest najprawdopodobniej zarezerwowany do użytku w przyszłości. 
* Trzeci parametr `cancellationSignal` jest obiekt służący do wyłączyć linii papilarnych i Anuluj bieżącego żądania. Jest to [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html), a nie typu z programu .NET framework.
* Czwarty parametr jest obowiązkowy i jest klasą tego podklasy `AuthenticationCallback` klasy abstrakcyjnej. Metody tej klasy wywoływane w celu sygnał do klientów podczas `FingerprintManager` zostało zakończone i jakie są wyniki. Jak wiele zrozumienie o implementacji `AuthenticationCallback`, będzie uwzględnione w [jest własnych sekcji](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* Parametrowi piątej to opcjonalna `Handler` wystąpienia. Jeśli `Handler` podany obiekt `FingerprintManager` użyje `Looper` z tego obiektu podczas przetwarzania komunikatów od sprzętu linii papilarnych. Zwykle nie należy podać `Handler`, użyje FingerprintManager `Looper` z aplikacji.

## <a name="cancelling-a-fingerprint-scan"></a>Anulowanie skanowania linii papilarnych

Może być konieczne dla użytkownika (lub aplikacji) anulować skanowanie linii papilarnych została zainicjowana. W takiej sytuacji należy wywołać [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) metoda [ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html) który został dostarczony do `FingerprintManager.Authenticate` gdy wywołano uruchomić skanowanie linii papilarnych.

Teraz, gdy zaobserwowano `Authenticate` metody Przeanalizujmy niektóre parametry ważniejsze bardziej szczegółowo. Najpierw przyjrzymy [odpowiada na żądania wywołań zwrotnych uwierzytelniania](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), która będzie omawiać jak podklasy [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), włączanie aplikacji systemu Android zareagować na wyniki dostarczone przez skaner linii papilarnych.




## <a name="related-links"></a>Linki pokrewne

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
