---
title: Odcisk palca uwierzytelniania
description: "W tym przewodniku omówiono sposób dodawania odcisku palca uwierzytelniania, wprowadzone w systemie Android 6.0 do aplikacji platformy Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 79f5f81e11f62359c3b951500d4ab5cbd63fb507
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="fingerprint-authentication"></a>Odcisk palca uwierzytelniania

_W tym przewodniku omówiono sposób dodawania odcisku palca uwierzytelniania, wprowadzone w systemie Android 6.0 do aplikacji platformy Xamarin.Android._


## <a name="fingerprint-authentication-overview"></a>Omówienie uwierzytelniania linii papilarnych

Przybyciu skanery linii papilarnych na urządzeniach z systemem Android zapewnia aplikacjom zamiast metody tradycyjnych nazwy użytkownika i hasła uwierzytelniania użytkowników. Stosowanie odcisków palców do uwierzytelnienia użytkownika umożliwia dla aplikacji, aby dołączyć do nich zabezpieczeń, który jest ta opcja jest mniej pożądana niż nazwa użytkownika i hasło.

Interfejsy API FingerprintManager docelowych urządzeń linii papilarnych, i działają interfejsu API na poziomie 23 (Android 6.0) lub nowszej. Interfejsy API znajdują się w `Android.Hardware.Fingerprints` przestrzeni nazw. Biblioteka obsługi systemu Android w wersji 4 zawiera wersje linii papilarnych interfejsów API przeznaczone dla wcześniejszych wersji systemu android. Zgodność interfejsów API znajdują się w `Android.Support.v4.Hardware.Fingerprint` przestrzeni nazw są dystrybuowane za pośrednictwem [pakietu Xamarin.Android.Support.v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

[FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (i jego odpowiednik Biblioteka obsługi [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) jest klasą głównej dla za pomocą odcisku palca skanowanie sprzętu. Ta klasa jest zestaw SDK systemu Android otokę systemu poziomu usługi, która zarządza interakcji z samego sprzętu. Jest odpowiedzialna za uruchamianie linii papilarnych oraz w odpowiedzi na opinie od skanera. Ta klasa ma bardzo prosta interfejs o tylko trzech elementów członkowskich:

* **`Authenticate`** &ndash; Ta metoda będzie zainicjować skanera sprzętu i uruchomić usługę w tle, oczekiwanie na użytkownika do skanowania ich odcisku palca.
* **`EnrolledFingerprints`** &ndash; Ta właściwość będzie zwracać `true` Jeśli użytkownik został zarejestrowany odciski palców co najmniej jeden z urządzeniem.
* **`HardwareDetected`** &ndash; Ta właściwość jest używana do określenia, czy urządzenie obsługuje skanowanie linii papilarnych.

`FingerprintManager.Authenticate` Metody jest używany przez aplikację systemu Android do uruchamiania linii papilarnych. Poniższy fragment kodu jest przykładem, można wywołać za pomocą zgodności pomocy technicznej biblioteki interfejsów API:

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

W tym przewodniku będzie omawiać temat użyj `FingerprintManager` interfejsów API w celu zwiększenia aplikacji systemu Android przy użyciu odcisku palca uwierzytelniania. Opisano sposób tworzenia wystąpienia i utworzyć `CryptoObject` do zabezpieczania wyników z linii papilarnych. Zajmiemy się, jak aplikacja powinna podklasy `FingerprintManager.AuthenticationCallback` i uwzględniał opinie użytkowników z linii papilarnych. Na koniec zajmiemy się tym, jak zarejestrować przy użyciu linii papilarnych na urządzeniu z systemem Android lub w emulatorze i sposobu użycia **adb** do symulowania skanowanie linii papilarnych.

## <a name="requirements"></a>Wymagania

Odcisk palca uwierzytelniania wymagane jest system Android 6.0 (interfejs API na poziomie 23) lub wyższej oraz urządzenia z linii papilarnych. 

Odcisk palca musi być już zarejestrowane za pomocą urządzeń dla każdego użytkownika, który ma zostać uwierzytelniona. Obejmuje to skonfigurowanie blokady ekranu, korzystającą z hasła, numer PIN, przejdź wzorzec lub rozpoznawanie twarzy. Istnieje możliwość symulowania niektórych funkcji uwierzytelniania odcisku palca w emulatorze systemu Android.  Aby uzyskać więcej informacji o tych dwóch tematów, zobacz [rejestrowanie przy użyciu linii papilarnych](enrolling-fingerprint.md) sekcji. 






## <a name="related-links"></a>Linki pokrewne

- [Odcisk palca przewodnik przykładowej aplikacji](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [Przykładowe okno dialogowe linii papilarnych](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Żądanie uprawnień w czasie wykonywania](http://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](http://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Odcisk palca i płatności interfejsu API (klip wideo)](https://youtu.be/VOn7VrTRlA4)
