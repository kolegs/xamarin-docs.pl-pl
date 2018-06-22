---
title: Wskazówki dotyczące uwierzytelniania linii papilarnych
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2b66c3660f6d8af9217089a7615784957fcc6ed7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763309"
---
# <a name="fingerprint-authentication-guidance"></a>Wskazówki dotyczące uwierzytelniania linii papilarnych

## <a name="fingerprint-authentication-guidance"></a>Wskazówki dotyczące uwierzytelniania linii papilarnych

Zaobserwowano pojęcia i interfejsów API otaczającego Android 6.0 odcisków palców uwierzytelniania, omówimy niektóre ogólne wskazówki dotyczące użytkowania interfejsów API linii papilarnych.

1. **Korzystanie z interfejsów API biblioteki obsługi systemu Android w wersji 4 zgodności** &ndash; to uprościć kod aplikacji, usuwając wyboru interfejsu API z kodu i zezwolić aplikacji pod kątem najwięcej możliwości urządzenia.
2. **Alternatywę dla odcisku palca uwierzytelniania** &ndash; odcisku palca uwierzytelniania jest doskonałym, szybki sposób do uwierzytelnienia użytkownika w aplikacji, jednak nie można założyć, że będzie zawsze pracy lub być dostępne. Istnieje możliwość, że linii papilarnych może zakończyć się niepowodzeniem, obiektywu może być zmieniony, użytkownik nie może skonfigurować urządzenie do używania uwierzytelniania odcisk palca lub palców, ponieważ przeszły Brak. Istnieje również możliwość, że użytkownik nie mogą chcieć przy użyciu odcisku palca uwierzytelniania z aplikacji. Z tego względu aplikacji systemu Android powinno dostarczyć proces uwierzytelniania alternatywnych, takich jak nazwa użytkownika i hasło.
3. **Użyj ikony odcisku palca firmy Google** &ndash; wszystkie aplikacje należy użyć tego samego ikony odcisk palca dostarczony przez firmę Google. Użyj standardowych ikony ułatwia użytkownicy systemu Android do rozpoznawania, gdzie w aplikacjach odcisku palca uwierzytelniania jest używany: 
    
    ![Ikona android linii papilarnych](summary-images/ic-fp-40px.png)
    
4. **Powiadom użytkownika** &ndash; określonego rodzaju powiadomienie do użytkownika, czy czytnik linii papilarnych jest aktywny powinien być wyświetlany w aplikacji oczekujących na touch lub Przejdź. 

## <a name="summary"></a>Podsumowanie

Odcisk palca uwierzytelniania jest to dobry sposób, aby umożliwić aplikacji platformy Xamarin.Android szybko sprawdzić użytkowników, ułatwiając użytkownikom na interakcję z ważne funkcje, takie jak zakupy w aplikacji. W tym przewodniku opisano pojęcia i kod, który jest wymagane uwzględnienie odcisku palca Android 6.0 do interfejsu API w aplikacji platformy Xamarin.Android.

Najpierw Rozmawialiśmy odcisku palca interfejsu API przez siebie, `FingerprintManager` (i `FingerprintManagerCompat`). Możemy się zbadana jak `FingerprintManager.AuthenticationCallbacks` klasy abstrakcyjnej muszą być rozszerzony przez aplikację i używany jako pośrednik między sprzętu odcisk palca i samej aplikacji. Następnie możemy zbadać sprawdzanie integralności wyniki skaner linii papilarnych za pomocą języka Java `Cipher` obiektu. Na koniec mamy dotknięciu nieco na temat testowania zawierająca opis sposobu rejestrowania urządzenia przy użyciu linii papilarnych, i używając **adb** do symulowania Przejdź linii papilarnych, emulatora. 

Jeśli nie zostało to jeszcze zrobione, należy zwrócić uwagę na [Przykładowa aplikacja](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) dołączony w tym przewodniku. [Przykładowe okno dialogowe odcisk palca](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) systemie za pomocą języka Java do platformy Xamarin.Android i zawiera inny przykład, jak dodać odcisku palca uwierzytelniania do aplikacji systemu Android.



## <a name="related-links"></a>Linki pokrewne

- [Odcisk palca przewodnik przykładowej aplikacji](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Przykładowe okno dialogowe linii papilarnych](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Ikona linii papilarnych](https://developer.android.comhttps://developer.xamarin.com/samples/FingerprintDialog/res/drawable-hdpi/ic_fp_40px.html)
