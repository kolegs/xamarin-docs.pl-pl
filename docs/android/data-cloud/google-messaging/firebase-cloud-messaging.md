---
title: Usługa firebase Cloud Messaging
description: Firebase Cloud Messaging (FCM) to usługa, która ułatwia przesyłanie komunikatów między aplikacji mobilnych i aplikacji serwera. Ten artykuł zawiera omówienie sposobu działania usługi FCM i wyjaśniono, jak skonfigurować usługi Google, aby Twoja aplikacja może używać usługi FCM.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: 92c402085627bbe67d4cd8ccaf60a68aa979f3e7
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514298"
---
# <a name="firebase-cloud-messaging"></a>Usługa firebase Cloud Messaging

_Firebase Cloud Messaging (FCM) to usługa, która ułatwia przesyłanie komunikatów między aplikacji mobilnych i aplikacji serwera. Ten artykuł zawiera omówienie sposobu działania usługi FCM i wyjaśniono, jak skonfigurować usługi Google, aby Twoja aplikacja może używać usługi FCM._

[![Firebase Cloud Messaging — obraz hero](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

Ten temat zawiera ogólne omówienie usługi Firebase Cloud Messaging kierowanie komunikatów między aplikacji platformy Xamarin.Android i na serwerze aplikacji i zapewnia procedury krok po kroku pobierania poświadczeń, aby Twoja aplikacja może używać usługi FCM.

## <a name="overview"></a>Omówienie

Firebase Cloud Messaging (FCM) to usługa dla wielu platform, która obsługuje wysyłanie, routingu i Kolejkowanie komunikatów między aplikacjami klientów urządzeń przenośnych i aplikacji serwera. FCM jest następcą do Google Cloud Messaging (GCM) i jest on oparty na usług Google Play.

Jak pokazano na poniższym diagramie, FCM działa jako pośrednik między nadawców wiadomości i klientami. A *aplikację kliencką* to aplikacja z obsługą usługi FCM, która jest uruchamiana na urządzeniu. *Serwera aplikacji* (udostępnione przez Ty lub Twoja firma) jest włączone FCM serwera, na którym Twoja aplikacja kliencka komunikuje się z usługą za pomocą usługi FCM. W przeciwieństwie do usługi GCM FCM ułatwia umożliwiają wysyłanie komunikatów do aplikacji klienckich bezpośrednio za pośrednictwem powiadomienia konsoli Firebase graficznego interfejsu użytkownika:

[![FCM znajduje się między aplikacją klienta i serwera aplikacji](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

Za pomocą usługi FCM, serwery aplikacji wiadomości można wysyłać do jednego urządzenia, grupy urządzeń lub liczba urządzeń mających subskrypcję tematu. Aplikacja kliencka można użyć usługi FCM do subskrybowania transmisji komunikatów z serwera aplikacji (na przykład, aby otrzymywać powiadomienia zdalne). Aby uzyskać więcej informacji o różnych typach Firebase wiadomości, zobacz [o wiadomości FCM](https://firebase.google.com/docs/cloud-messaging/concept-options).

## <a name="fcm-in-action"></a>Usługi firebase Cloud Messaging w działaniu

Podczas transmisji komunikatów są wysyłane do aplikacji klienckich z serwera aplikacji, serwer aplikacji wysyła komunikat do *FCM połączenia serwera* dostarczane przez firmę Google; serwer połączenia usługi FCM z kolei przekazuje komunikat do urządzenia, na którym działa Aplikacja klienta. Komunikaty mogą być wysyłane za pośrednictwem protokołu HTTP lub [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging and Presence Protocol). Ponieważ aplikacje klienckie nie są zawsze połączone lub uruchomione FCM połączenia serwera umieszczeniu i magazynami komunikatów, wysyłając je do aplikacji klienckich, jak długo będą ponownie i stają się dostępne. Podobnie FCM będzie umieścić nadrzędnego wiadomości z aplikacji klienckich z serwerem aplikacji przypadku niedostępności serwera aplikacji. Aby uzyskać więcej informacji o serwerach połączenia usługi FCM zobacz [o Firebase Cloud Messaging serwera](https://firebase.google.com/docs/cloud-messaging/server).

FCM przy użyciu następujących poświadczeń do identyfikowania serwera aplikacji i aplikację kliencką i używa tych poświadczeń w celu autoryzowania transakcji wiadomości za pomocą usługi FCM:

-   <a name="fcm-in-action-sender-id"></a>**Identyfikator nadawcy** &ndash; *identyfikator nadawcy* to unikatowa wartość liczbowa przypisana podczas tworzenia projektu Firebase. Identyfikator nadawcy jest używany do identyfikowania każdego serwera aplikacji, którą można wysłać wiadomości do aplikacji klienckiej. Identyfikator nadawcy jest także numer projektu; Identyfikator nadawcy można uzyskać z poziomu konsoli usługi Firebase, po zarejestrowaniu Twojego projektu. Na przykład identyfikator nadawcy `496915549731`.

-   <a name="fcm-in-action-api-key"></a>**Klucz interfejsu API** &ndash; *klucz interfejsu API* zapewnia dostęp do serwera aplikacji do usługi Firebase; FCM używa tego klucza do uwierzytelniania serwera aplikacji. To poświadczenie jest również nazywany *klucz serwera* lub *klucz interfejsu API sieci Web*. Na przykład klucz interfejsu API `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`.

-   <a name="fcm-in-action-app-id"></a>**Identyfikator aplikacji** &ndash; tożsamości aplikację kliencką (niezależnie od wszelkich danego urządzenia) który rejestruje, aby odbierać komunikaty z usługi FCM. Na przykład identyfikator aplikacji `1:415712510732:android:0e1eb7a661af2460`.

-   <a name="fcm-in-action-registration-token"></a>**Token rejestracji** &ndash; *tokenu rejestracji* (nazywane także *identyfikator wystąpienia*) jest tożsamością usługi FCM aplikację kliencką na danym urządzeniu. Token rejestracji jest generowany w czasie wykonywania &ndash; aplikacja otrzymuje token rejestracji, gdy najpierw rejestruje z usługą FCM podczas uruchamiania na urządzeniu. Tokenu rejestracji autoryzuje wystąpienie aplikację kliencką (działające na tym określonym urządzeniu) aby odbierać komunikaty z usługi FCM.
    Na przykład tokenu rejestracji `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (bardzo długi ciąg).

[Ustawienie zapasowej Firebase Cloud Messaging](#setup_fcm) (w dalszej części tego przewodnika) zawiera szczegółowe instrukcje dotyczące tworzenia projektu i generowanie tych poświadczeń. Podczas tworzenia nowego projektu w [konsoli Firebase](https://console.firebase.google.com/), plik poświadczeń o nazwie **google-services.json** utworzeniu &ndash; Dodaj ten plik do projektu Xamarin.Android, jak wyjaśniono w [ Zdalne powiadomienia z usługą FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

W poniższych sekcjach opisano, jak te poświadczenia są używane, gdy aplikacje klienckie komunikują się z serwerów aplikacji za pośrednictwem usługi FCM.


<a name="registration" />

### <a name="registration-with-fcm"></a>Rejestracji usługi FCM

Aplikacja kliencka musi najpierw zarejestrować z usługą FCM komunikaty mogą być wykonywane. Aplikacja kliencka, należy wykonać kroki rejestracji pokazano na poniższym diagramie:

[![Diagramu kroków rejestracji aplikacji](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  Aplikacja kliencka kontaktuje się FCM w celu uzyskania tokenu rejestracji, przekazując identyfikator nadawcy, klucz interfejsu API i identyfikator aplikacji do usługi FCM.

2.  FCM zwraca token rejestracji aplikacji klienta.

3.  Aplikacja kliencka (opcjonalnie) przekazuje tokenu rejestracji z serwerem aplikacji.

Serwer aplikacji buforuje tokenu rejestracji dla kolejnych komunikacji za pomocą aplikacji klienckiej. Serwer aplikacji może wysyłać potwierdzenia do aplikacji klienckiej, aby wskazać, czy odebrano tokenu rejestracji. Po wystąpieniu tego uzgadnianie aplikacja kliencka może odbierać komunikaty z (lub wysyłać komunikaty do) serwera aplikacji. Jeśli stary token zostanie naruszony, aplikacja kliencka może zostać wyświetlony nowy token rejestracji (zobacz [zdalne powiadomienia z usługą FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) przykład jak aplikacja odbiera aktualizacje tokenu rejestracji).

Gdy aplikacja kliencka nie jest już chce odbierać komunikaty z serwera aplikacji, może wysłać żądanie do serwera aplikacji, można usunąć tokenu rejestracji. Jeśli aplikacja klienta nie zostanie odinstalowana z urządzenia, FCM wykryje to i automatycznie powiadamia, serwera aplikacji, można usunąć tokenu rejestracji.



### <a name="downstream-messaging"></a>Transmisji komunikatów

Na poniższym diagramie przedstawiono, jak usługi Firebase Cloud Messaging przechowuje i przekazuje komunikaty podrzędne:

[![FCM korzysta z magazynu i do przodu transmisji komunikatów](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

Gdy serwer aplikacji wysyła transmisji komunikatów do aplikacji klienckiej, używa następujące kroki jako wyliczany na powyższym diagramie:

1.  Serwer aplikacji wysyła komunikat do usługi FCM.

2.  Jeśli urządzenie klienckie nie jest dostępna, serwer usługi FCM przechowuje komunikat w kolejce transmisji nowsze. Komunikaty są przechowywane w magazynie usługi FCM maksymalnie 4 tygodnie (Aby uzyskać więcej informacji, zobacz [ustawienie cyklem życia komunikatu](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  Po udostępnieniu urządzenia klienckiego FCM przekazuje komunikat do aplikacji klienckich na tym urządzeniu.

4.  Aplikacja kliencka odbiera komunikaty z usługi FCM, przetwarza je i wyświetla dla użytkownika. Na przykład jeśli wiadomość ma zdalne powiadomienia, są prezentowane użytkownikowi w obszarze powiadomień.

W tym scenariuszu obsługi komunikatów (gdzie serwera aplikacji wysyła komunikat do aplikacji przez jednego klienta) komunikaty mogą mieć maksymalnie 4kB.

Aby uzyskać szczegółowe informacje dotyczące odbierania transmisji wiadomości FCM w systemie Android, zobacz [zdalne powiadomienia z usługą FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

### <a name="topic-messaging"></a>Temat wiadomości

*Obsługa komunikatów w temacie* pozwala na serwerze aplikacji wysłać wiadomość do wielu urządzeń, które wybranych do określonego tematu. Możesz również tworzenie i wysyłanie komunikatów z tematów za pośrednictwem powiadomienia konsoli Firebase graficznego interfejsu użytkownika. FCM obsługuje routing i dostarczenia komunikatów do tematu do subskrybowanego klientów. Ta funkcja może być używana dla wiadomości, takich jak alerty o pogodzie, giełdowych i nagłówek wiadomości.

[![Temat wiadomości diagramu](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

(Po aplikacja klienta uzyskuje token rejestracji, jak wyjaśniono wcześniej), w temacie wiadomości używane są następujące czynności:

1.  Aplikacja kliencka subskrybowanie tematu, wysyłając wiadomość Subskrybuj do usługi FCM.

2.  Serwer aplikacji wysyła komunikaty do tematu do FCM do dystrybucji.

3.  FCM przekazuje tematu wiadomości do klientów, którzy mają subskrypcję tego tematu.

Aby uzyskać więcej informacji na temat usługi Firebase tematu wiadomości Zobacz firmy Google [tematu wiadomości w systemie Android](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging).


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Konfigurowanie usługi Firebase Cloud Messaging

Zanim użyjesz usługi FCM w swojej aplikacji, należy utworzyć nowy projekt (lub zaimportować istniejący projekt) za pośrednictwem [konsoli Firebase](https://console.firebase.google.com/). Aby utworzyć projekt usługi Firebase Cloud Messaging dla aplikacji, należy użyć następujące czynności:

1.  Zaloguj się do [konsoli Firebase](https://console.firebase.google.com/) przy użyciu swojego konta Google (czyli adres Gmail) i kliknij przycisk **Utwórz nowy projekt**:

    [![Tworzenie przycisku Nowy projekt](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    Jeśli masz istniejący projekt, kliknij przycisk **Importuj projekt Google**.

2.  W **Utwórz projekt** okno dialogowe, wprowadź nazwę projektu, a następnie kliknij przycisk **Tworzenie projektu**. W poniższym przykładzie nowy projekt o nazwie **XamarinFCM** zostanie utworzony:

    [![Tworzenie projektu okna dialogowego](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  W konsoli Firebase **Przegląd**, kliknij przycisk **Dodaj Firebase do aplikacji systemu Android**:

    [![Dodawanie usługi Firebase do aplikacji systemu Android](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  Na następnym ekranie wprowadź nazwę pakietu aplikacji. W tym przykładzie nazwa pakietu jest **com.xamarin.fcmexample**. Ta wartość musi odpowiadać nazwa pakietu aplikacji systemu Android. Pseudonim aplikacji można również wprowadzić w **pseudonim aplikacji** pola:

    [![Wprowadzanie przykład FCM jako pseudonim aplikacji](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  Jeśli aplikacja używa uwierzytelniania Google łączy dynamicznych i zaproszenia, to należy także podać swoje debugowania certyfikatu podpisywania. Aby uzyskać więcej informacji na temat znajdowania certyfikatu podpisywania, zobacz [znajdowaniu Twojego magazynu kluczy MD5 lub SHA1 podpisu](~/android/deploy-test/signing/keystore-signature.md).
    W tym przykładzie certyfikat podpisywania jest puste.

6.  Kliknij przycisk **Dodaj Aplikację**:

    [![Kliknięcie przycisku Dodaj aplikację](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    Klucz interfejsu API serwera i identyfikator klienta są generowane automatycznie dla aplikacji. Te informacje są spakowane w **google-services.json** pliku, który jest automatycznie pobierany po kliknięciu **Dodaj Aplikację**.
    Pamiętaj zapisać ten plik w bezpiecznym miejscu.

Aby uzyskać szczegółowy przykład sposobu dodawania **google-services.json** do projektu aplikacji do odbierania komunikatów powiadomień wypychanych usługi FCM w systemie Android, zobacz [zdalne powiadomienia z usługą FCM](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).



## <a name="for-further-reading"></a>Aby uzyskać dalsze informacje

-   Firmy Google [usługi Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) zawiera omówienie usługi Firebase Cloud Messaging jego kluczowych możliwości, wyjaśnienie, jak to działa, a instrukcje dotyczące konfigurowania.

-   Firmy Google [kompilacji serwera wysyłania żądań dotyczących aplikacji](https://firebase.google.com/docs/cloud-messaging/send-message) wyjaśnia, jak wysyłać komunikaty z serwerem aplikacji.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) i [RFC 6121](https://tools.ietf.org/html/rfc6121) wyjaśnić i zdefiniuj Extensible Messaging and Presence Protocol (XMPP).

-   [Temat wiadomości FCM](https://firebase.google.com/docs/cloud-messaging/concept-options) opisano różne typy komunikatów, które mogą być wysyłane z usługi Firebase Cloud Messaging.

## <a name="summary"></a>Podsumowanie

W tym artykule podano przegląd dla usługi Firebase Cloud Messaging (FCM). Wyjaśniono jej różnych poświadczeń, które są używane do identyfikowania i autoryzować obsługi komunikatów między aplikacjami klienta i serwerów aplikacji. Jego zilustrowane, rejestracji i podrzędne scenariusze dotyczące przesyłania komunikatów, a jego szczegółowe kroki rejestracji aplikacji z usługą FCM do użycia usługi FCM.


## <a name="related-links"></a>Linki pokrewne

- [Usługa Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/)
