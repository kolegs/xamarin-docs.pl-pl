---
title: Odpowiada na żądania wywołań zwrotnych uwierzytelniania
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: b8a3ed64e66cd97faeff78b4d0b008a1a0b14477
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765772"
---
# <a name="responding-to-authentication-callbacks"></a>Odpowiada na żądania wywołań zwrotnych uwierzytelniania

Linii papilarnych działa w tle w jego własnym wątku i po zakończeniu zgłasza wyniki skanowania za pomocą jednej metody `FingerprintManager.AuthenticationCallback` w wątku interfejsu użytkownika. Aplikacja systemu Android podać własne obsługi, który rozszerza ta klasa ogólna wykonania następujących metod:

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Wywoływane, gdy wystąpił nieodwracalny błąd. Nie ma nic więcej, które aplikacji lub użytkownik może wykonywać Aby naprawić tę sytuację, z wyjątkiem prawdopodobnie spróbuj ponownie.
* **`OnAuthenticationFailed()`** &ndash; Ta metoda jest wywoływana, gdy przy użyciu linii papilarnych zostało wykryte, ale nie został rozpoznany przez urządzenie.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Wywoływane, gdy jest to błąd odwracalny, takich jak palca trwa płacenia do szybkiego przez skaner.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Jest to po rozpoznaniu przy użyciu linii papilarnych.

Jeśli `CryptoObject` była używana przy wywoływaniu `Authenticate`, zalecane jest wywołać `Cipher.DoFinal` w `OnAuthenticationSuccessful`.
`DoFinal` spowoduje zgłoszenie wyjątku, jeśli określony szyfr zostało naruszone lub niewłaściwie zainicjowane, wskazując, że wynik linii papilarnych może zostały naruszone lokalizację poza aplikacją.


> [!NOTE]
> Zaleca się, aby zachować wagi stosunkowo małe klasy wywołania zwrotnego i zwolnij logiki określonej aplikacji. Wywołania zwrotne ma pełnić rolę "ruchu cop" między aplikacji systemu Android i wyniki z linii papilarnych.

## <a name="a-sample-authentication-callback-handler"></a>Przykładowe obsługi wywołania zwrotnego uwierzytelniania

Następujące klasy jest przykładem minimalnym `FingerprintManager.AuthenticationCallback` implementacji: 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` sprawdza, czy `Cipher` dostarczono `FingerprintManager` podczas `Authentication` został wywołany. Jeśli tak, `DoFinal` wywoływana jest metoda szyfrowania. Spowoduje to zamknięcie `Cipher`, przywrócenie oryginalnego stanu. Jeśli wystąpił problem z szyfrowania, następnie `DoFinal` spowoduje zgłoszenie wyjątku i próby uwierzytelnienia należy traktować jako powiodła się.

`OnAuthenticationError` i `OnAuthenticationHelp` całkowitą wskazującą problem został odbierać wywołania zwrotne każdego. Poniższa sekcja zawiera wszystkich możliwych pomocy lub kody błędów. Dwa wywołania zwrotne służyć do podobnych celów &ndash; poinformowanie aplikacji o odcisku palca niepowodzeniu uwierzytelnienia. Różnice między jest ważność. `OnAuthenticationHelp` jest to błąd odwracalny użytkownika, takie jak szybko przesuwając odcisku palca zbyt wysoka; `OnAuthenticationError` jest bardziej poważny błąd, takie jak uszkodzony linii papilarnych.

Należy pamiętać, że `OnAuthenticationError` zostanie wywołany, gdy skanowanie linii papilarnych zostało anulowane za pośrednictwem `CancellationSignal.Cancel()` wiadomości. `errMsgId` Parametr będzie mieć wartość 5 (`FingerprintState.ErrorCanceled`). W zależności od wymagań, implementacja `AuthenticationCallbacks` tej sytuacji mogą traktować inaczej niż inne błędy. 

`OnAuthenticationFailed` wywoływane, gdy odcisku palca przeskanowano pomyślnie, ale nie był zgodny linii papilarnych, wszystkie zarejestrowane urządzenia. 

## <a name="help-codes-and-error-message-ids"></a>Kody i identyfikatory komunikat błędu 

Listę i opis kody pomocy i kody błędów można znaleźć w [dokumentacji zestawu SDK systemu Android](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) dla klasy FingerprintManager. Xamarin.Android reprezentuje tych wartości za pomocą `Android.Hardware.Fingerprints.FingerprintState` wyliczenia:


-   **`AcquiredGood`** &ndash; (wartość 0) Obraz nabyte jest dobra.


-   **`AcquiredImagerDirty`** &ndash; (wartość 3) Obraz odcisk palca: zbyt wiele szumów z powodu podejrzenia lub wykrytego zabrudzenie na czujnika. Na przykład, jest uzasadnione do zwracania to po wielu `AcquiredInsufficient` lub rzeczywistego wykrywania brudu na czujnik (zablokowane piksele, swaths itp.). Użytkownik powinien podjąć działania w celu czyszczenia czujnika, gdy ten błąd jest zwracany.


-   **`AcquiredInsufficient`** &ndash; (wartość 2) Obraz odcisk palca: zbyt dużo przetwarzania z powodu wykrytego warunku (tj. suchej skórki) lub prawdopodobnie zanieczyszczone czujnik (zobacz `AcquiredImagerDirty`.



-   **`AcquiredPartial`** &ndash; (wartość 1) Wykryto tylko obraz częściowej linii papilarnych. Podczas rejestracji, użytkownik powinien otrzymywać informacje na co powinno nastąpić, aby rozwiązać ten problem, np. &ldquo;mocno naciśnij na czujnika.&rdquo;



-   **`AcquiredTooFast`** &ndash; (wartość 5) Obraz odcisk palca nie zostało ukończone z powodu szybkiego ruchu. Gdy przede wszystkim właściwe dla tablicy liniowej czujników, również możliwe palca została przeniesiona podczas nabywania. Użytkownik powinien zostać poproszony o Przesuń palcem wolniejsze (liniowej) lub pozostaw palca na czujnik dłużej.




-   **`AcquiredToSlow`** &ndash; (wartość 4) Obraz odcisk palca: nie można go odczytać z powodu braku ruchu. Jest to najbardziej odpowiednia w liniowej tablicy czujników, które wymagają ruchu Przejdź.



-   **`ErrorCanceled`** &ndash; (wartość 5) Operacja została anulowana, ponieważ czujnika odcisków palców jest niedostępny. Na przykład może to być spowodowane przełączono użytkownik, urządzenie jest zablokowane lub innej oczekującej operacji uniemożliwia lub wyłącza je.



-   **`ErrorHwUnavailable`** &ndash; (wartość 1) Sprzęt jest niedostępny. Spróbuj ponownie później.




-   **`ErrorLockout`** &ndash; (wartość 7) Operacja została anulowana, ponieważ interfejs API jest zablokowane z powodu zbyt wielu prób.




-   **`ErrorNoSpace`** &ndash; (wartość 4) Stan błędu zwrócony dla operacje, takie jak rejestracji; Nie można ukończyć operacji, ponieważ nie ma wystarczającej ilości pamięci masowej pozostałych do ukończenia tej operacji.



-   **`ErrorTimeout`** &ndash; (wartość 3) Stan błędu zwracany, gdy bieżące żądanie działa zbyt długo. Ma to zapobiec programów z czujnika odcisków palców oczekiwania przez czas nieokreślony. Limit czasu jest specyficzne dla czujnik i platformy, ale jest zazwyczaj około 30 sekund.



-   **`ErrorUnableToProcess`** &ndash; (wartość 2) Stan błędu zwracany, gdy czujnik nie może przetworzyć bieżącego obrazu.



## <a name="related-links"></a>Linki pokrewne

- [Szyfrowania](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
