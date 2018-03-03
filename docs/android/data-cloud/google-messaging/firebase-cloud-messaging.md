---
title: Firebase Cloud Messaging
description: "Firebase Cloud Messaging (FCM) to usługa, która umożliwia wysyłanie komunikatów między aplikacjami mobilnymi i aplikacji serwera. Ten artykuł zawiera omówienie działania FCM oraz wyjaśniono sposób konfigurowania usług Google, dzięki czemu aplikacja może używać FCM."
ms.topic: article
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2017
ms.openlocfilehash: 9f084899f44e0104d0aa2d4b3c0509812bd3fdd2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud Messaging

_Firebase Cloud Messaging (FCM) to usługa, która umożliwia wysyłanie komunikatów między aplikacjami mobilnymi i aplikacji serwera. Ten artykuł zawiera omówienie działania FCM oraz wyjaśniono sposób konfigurowania usług Google, dzięki czemu aplikacja może używać FCM._

[![Firebase Cloud Messaging bohater obrazu](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png)

Ten temat zawiera omówienie sposobu Firebase Cloud Messaging tras wiadomości między aplikacji platformy Xamarin.Android i serwera aplikacji, i zapewnia szczegółową procedurę uzyskania poświadczeń, dzięki czemu aplikacja może używać FCM usług.


<a name="overview" />

## <a name="overview"></a>Omówienie

Firebase Cloud Messaging (FCM) to usługa i platform, która obsługuje wysyłanie, routing i usługi kolejkowania komunikatów między aplikacjami klientów urządzeń przenośnych i aplikacji serwera. FCM jest następnika do usługi Google Cloud Messaging (GCM) i jest zbudowany na usług Google Play.

Jak pokazano na poniższym diagramie, FCM działa jako pośrednik między nadawcy wiadomości i klientami. A *aplikacji klienckiej* jest włączone FCM aplikacji, która jest uruchamiana na urządzeniu. *Serwera aplikacji* (udostępniony przez użytkownik lub jego firma) jest serwer z obsługą FCM aplikacji klient komunikuje się za pośrednictwem FCM. W przeciwieństwie do usługi GCM FCM umożliwia wysyłanie komunikatów do aplikacji klienckich bezpośrednio za pomocą powiadomień konsoli Firebase graficznego interfejsu użytkownika:

[![FCM znajduje się między aplikacją klienta i serwera aplikacji](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png)

Przy użyciu FCM, serwery aplikacji może wysyłać komunikaty do jednego urządzenia do grupy urządzeń lub liczbę urządzeń, które subskrybuje tematu. Aplikacja kliencka umożliwia FCM subskrybować podrzędne wiadomości z aplikacji serwera (na przykład, aby otrzymywać powiadomienia zdalne). Aby uzyskać więcej informacji o różnych typach Firebase komunikatów, zobacz [o wiadomości FCM](https://firebase.google.com/docs/cloud-messaging/concept-options).


<a name="inaction" />

## <a name="firebase-cloud-messaging-in-action"></a>Chmura firebase wiadomości w akcji

Podczas podrzędne komunikatów są wysyłane do aplikacji klienckich z serwera aplikacji, serwer aplikacji wysyła komunikat do *FCM połączenia serwera* dostarczony przez firmę Google; serwer FCM połączenia z kolei przekazuje komunikat do urządzenia z systemem Aplikacja kliencka. Wiadomości mogą być wysyłane za pośrednictwem protokołu HTTP lub [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible obsługi wiadomości i obecności Protocol). Ponieważ aplikacje klienta nie są zawsze połączone lub systemem FCM połączenia serwera enqueues i magazyny wiadomości, wysłanie ich do aplikacji klienckich, połącz się ponownie i stają się dostępne. Podobnie FCM będzie umieścić w kolejce nadrzędnego komunikatów w aplikacji klienta do serwera aplikacji czy serwera aplikacji jest niedostępna. Aby uzyskać więcej informacji o serwerach połączenia FCM zobacz [o Firebase Cloud Messaging serwera](https://firebase.google.com/docs/cloud-messaging/server).

FCM używa następujących poświadczeń do identyfikowania serwera aplikacji i aplikacji klienckiej i używa tych poświadczeń do autoryzacji transakcji wiadomości za pośrednictwem FCM:

-   **Identyfikator nadawcy** &ndash; *identyfikator nadawcy* to unikatowa wartość liczbową przypisany podczas tworzenia projektu Firebase. Identyfikator nadawcy jest używany do identyfikowania każdego serwera aplikacji, który można wysłać wiadomości do aplikacji klienckiej. Identyfikator nadawcy jest także numer projektu; Identyfikator nadawcy można uzyskać z poziomu konsoli Firebase podczas rejestrowania projektu. Na przykład identyfikator nadawcy `496915549731`.

-   **Klucz interfejsu API** &ndash; *klucz interfejsu API* daje dostęp do serwera aplikacji do usług Firebase; FCM używa tego klucza do uwierzytelnienia serwera aplikacji. To poświadczenie jest również nazywany *klucz serwera* lub *klucz interfejsu API sieci Web*. Na przykład klucz interfejsu API `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   **Identyfikator aplikacji** &ndash; tożsamości aplikacji klienta (niezależnie od wszelkich danego urządzenia) rejestruje odbierać komunikaty z FCM. Na przykład identyfikator aplikacji `1:415712510732:android:0e1eb7a661af2460`.

-   **Token rejestracji** &ndash; *tokenu rejestracji* (zwaną także *identyfikator wystąpienia*) jest tożsamość FCM aplikację klienta na danym urządzeniu. Token rejestracji jest generowane w czasie wykonywania &ndash; aplikacji otrzymuje token rejestracji, gdy najpierw rejestruje z FCM podczas uruchamiania na urządzeniu. Token rejestracji autoryzuje wystąpienia aplikacji klienta (działające na tym urządzeniu określonego) do odbierania wiadomości z FCM.
    Na przykład z tokenem rejestracji `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (bardzo długi ciąg).

[Ustawienie zapasowej Firebase Cloud Messaging](#setup_fcm) (dalszej części tego przewodnika) zawiera szczegółowe instrukcje dotyczące tworzenia projektu i generowania tych poświadczeń. Podczas tworzenia nowego projektu w [konsoli Firebase](https://console.firebase.google.com/), plik poświadczeń o nazwie **google services.json** utworzeniu &ndash; zgodnie z objaśnieniem w ,DodajtenplikdoprojektuplatformyXamarin.Android[ Zdalne powiadomienia z FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

W poniższych sekcjach opisano, jak te poświadczenia są używane, gdy aplikacje klienckie komunikują się z serwerami aplikacji za pośrednictwem FCM.


<a name="registration" />

### <a name="registration-with-fcm"></a>Rejestracja FCM

Aplikacja kliencka najpierw należy zarejestrować się FCM przed wiadomości mogą mieć miejsce. Aplikacja kliencka musi wykonać kroki rejestracji pokazano na poniższym diagramie:

[![Diagram kroki rejestracji aplikacji](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png)

1.  Aplikacja kliencka kontaktuje się FCM do uzyskania tokenu rejestracji, przekazywanie do FCM identyfikator nadawcy, klucz interfejsu API i identyfikator aplikacji.

2.  FCM zwraca token rejestracji aplikacji klienckiej.

3.  Aplikacja kliencka (opcjonalnie) przekazuje token rejestracji do serwerów aplikacji.

Serwer aplikacji buforuje tokenu rejestracji kolejnych komunikacji z aplikacji klienckiej. Serwer aplikacji może wysyłać potwierdzenia powrót do aplikacji klienckiej, aby wskazać, czy odebrano tokenu rejestracji. Po wystąpieniu tego uzgadnianie aplikacji klienckiej mogą odbierać komunikaty z (lub wysyłanie komunikatów do) serwera aplikacji. Aplikacja kliencka może zostać wyświetlony nowy token rejestracji w przypadku złamania zabezpieczeń stary token (zobacz [zdalnego powiadomienia z FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) przykład jak aplikacja odbiera aktualizacje tokenu rejestracji).

Gdy już aplikacji klienckiej chce odbierać komunikaty z serwerów aplikacji, może wysłać żądania do serwera aplikacji, można usunąć tokenu rejestracji. Jeśli aplikacja klienta zostanie odinstalowana z urządzenia, FCM wykryje to i automatycznie powiadamia serwer aplikacji, można usunąć tokenu rejestracji.


<a name="downstream" />

### <a name="downstream-messaging"></a>Podrzędne obsługi wiadomości

Na poniższym diagramie przedstawiono sposób Firebase Cloud Messaging przechowuje i przekazuje komunikaty podrzędne:

[![FCM korzysta z magazynu i do przodu podrzędnym obsługi wiadomości](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png)

Gdy serwer aplikacji wysyła wiadomość podrzędne do aplikacji klienckiej, jako wyliczenia na powyższym diagramie korzysta następujące kroki:

1.  Serwer aplikacji wysyła komunikat do FCM.

2.  Jeśli urządzenie klienckie nie jest dostępna, serwer FCM przechowuje wiadomości w kolejce transmisji nowsze. Wiadomości są przechowywane w magazynie FCM maksymalnie 4 tygodnie (Aby uzyskać więcej informacji, zobacz [ustawienia czasu działania komunikat](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  Po udostępnieniu urządzenia klienckiego FCM przesyła dalej wiadomości do aplikacji klienckiej na tym urządzeniu.

4.  Aplikacja kliencka odbiera wiadomości z FCM, procesy i wyświetla dla użytkownika. Na przykład jeśli komunikat jest zdalny powiadomień, jest prezentowane do użytkownika w obszarze powiadomień.

W tym scenariuszu obsługi komunikatów (gdzie serwera aplikacji wysyła komunikat do aplikacji przez jednego klienta) komunikaty mogą zostać maksymalnie 4kB.

Aby uzyskać szczegółowe informacje dotyczące odbierania podrzędne FCM wiadomości w systemie Android, zobacz [zdalnego powiadomienia z FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).


<a name="topic" />

### <a name="topic-messaging"></a>Temat wiadomości

*Temat wiadomości* umożliwia serwera aplikacji wysłać wiadomość na wielu urządzeniach, które wybranych do określonego tematu. Można również utworzyć i wysyłanie komunikatów tematu za pośrednictwem powiadomienia konsoli Firebase graficznego interfejsu użytkownika. FCM obsługi routingu i dostarczania komunikatów tematu do subskrybowanego klientów. Ta funkcja może być używana dla komunikaty, takie jak pogody alertów, giełdowych i nagłówek wiadomości.

[![Temat wiadomości diagramu](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png)

Poniższe kroki są używane w temacie wiadomości (po aplikacji klienckiej uzyskuje token rejestracji, zgodnie z opisem):

1.  Aplikacja kliencka subskrybuje tematu, wysyłając wiadomość Subskrybuj FCM.

2.  Serwer aplikacji wysyła tematu wiadomości do FCM dystrybucji.

3.  FCM przekazuje tematu wiadomości do klientów, którzy masz subskrypcję tego tematu.

Aby uzyskać więcej informacji na temat Firebase tematu wiadomości, zobacz firmy Google [tematu wiadomości w systemie Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Konfigurowanie Firebase Cloud Messaging

Zanim użyjesz usługi FCM w aplikacji, należy utworzyć nowy projekt (lub zaimportować istniejący projekt) za pośrednictwem [konsoli Firebase](https://console.firebase.google.com/). Poniższe kroki umożliwiają utworzenie projektu Firebase Cloud Messaging dla aplikacji:

1.  Zaloguj się do [konsoli Firebase](https://console.firebase.google.com/) z konta Google (tj. adres Gmail) i kliknij przycisk **Utwórz nowy projekt**:

    [![Utwórz nowy projekt przycisku](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png)

    Jeśli masz istniejący projekt, kliknij przycisk **Importowanie projektu Google**.

2.  W **Utwórz projekt** okna dialogowego, wprowadź nazwę projektu, a następnie kliknij przycisk **tworzenia projektu**. W poniższym przykładzie nowego projektu o nazwie **XamarinFCM** utworzeniu:

    [![Tworzenie projektu okna dialogowego](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png)

3.  W konsoli Firebase **omówienie**, kliknij przycisk **Firebase dodać do aplikacji systemu Android**:

    [![Dodaj Firebase do aplikacji systemu Android](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png)

4.  Na następnym ekranie wprowadź nazwę pakietu aplikacji. W tym przykładzie nazwa pakietu jest **com.xamarin.fcmexample**. Ta wartość musi odpowiadać nazwie pakietu aplikacji systemu Android. Pseudonim aplikacji można także wpisać w **pseudonim aplikacji** pola:

    [![Wprowadzanie przykład FCM jako pseudonim aplikacji](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png)

5.  Jeśli aplikacja korzysta z dynamicznego łącza, zaproszeń lub uwierzytelniania Google, to należy także podać Twoje debugowania certyfikatu podpisywania. Aby uzyskać więcej informacji na temat lokalizowania certyfikatu podpisywania, zobacz [znajdowanie MD5 lub SHA1 podpisu Twojego magazynu kluczy](~/android/deploy-test/signing/keystore-signature.md).
    W tym przykładzie certyfikatu podpisywania jest puste.

6.  Kliknij przycisk **aplikacji Dodaj**:

    [![Kliknięcie przycisku Dodaj aplikację](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png)

    Klucz interfejsu API serwera i identyfikator klienta są generowane automatycznie dla aplikacji. Te informacje jest spakowany w **google services.json** pliku, który jest automatycznie pobierany po kliknięciu **Dodaj Aplikację**.
    Pamiętaj zapisać ten plik w bezpiecznym miejscu.

Aby uzyskać szczegółowy przykład sposobu dodawania **google services.json** do projektu aplikacji na odbieranie komunikatów powiadomień wypychanych FCM w systemie Android, zobacz [zdalnego powiadomienia z FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).


<a name="furtherreading" />

## <a name="for-further-reading"></a>Dalsze informacje

-   Firmy Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) zawiera omówienie Firebase Cloud Messaging dla najważniejszych możliwości, wyjaśnienie, jak to działa, a także instrukcje instalacji.

-   Firmy Google [kompilacji żądań wysyłania serwera aplikacji](https://firebase.google.com/docs/cloud-messaging/send-message) wyjaśniono, jak wysyłać wiadomości z serwerem aplikacji.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) i [RFC 6121](https://tools.ietf.org/html/rfc6121) wyjaśnić i zdefiniuj Extensible obsługi wiadomości i obecności protokołu (XMPP).


<a name="summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule podać Omówienie programu obsługi wiadomości chmury Firebase (FCM). Wyjaśniono jej różnych poświadczeń, które są używane do identyfikacji i autoryzować komunikatów między aplikacjami klienta i serwerów aplikacji. Przedstawia on podrzędne scenariusze obsługi wiadomości i rejestracji, a on szczegółowe kroki dotyczące rejestrowania aplikacji za pomocą FCM się użyć usług FCM.


## <a name="related-links"></a>Linki pokrewne

- [Usługa Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
