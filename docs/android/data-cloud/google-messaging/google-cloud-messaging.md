---
title: "Usługa Google Cloud Messaging"
description: "Google Cloud Messaging (GCM) to usługa, która umożliwia wysyłanie komunikatów między aplikacjami mobilnymi i aplikacji serwera. W tym artykule omówiono sposób działania usługi GCM, oraz wyjaśniono sposób konfigurowania usług Google dzięki aplikacji za pomocą usługi GCM."
ms.topic: article
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: f44899ecf5ba2d904333b71226cdd6c7dcea8db0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="google-cloud-messaging"></a>Usługa Google Cloud Messaging

_Google Cloud Messaging (GCM) to usługa, która umożliwia wysyłanie komunikatów między aplikacjami mobilnymi i aplikacji serwera. W tym artykule omówiono sposób działania usługi GCM, oraz wyjaśniono sposób konfigurowania usług Google dzięki aplikacji za pomocą usługi GCM._

[![Logo usługi Google Cloud Messaging](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

Ten temat zawiera omówienie sposobu Google Cloud Messaging tras wiadomości między aplikacji i serwera aplikacji, i zapewnia szczegółową procedurę uzyskania poświadczeń, dzięki czemu aplikacja może używać usługi GCM.


## <a name="overview"></a>Omówienie

Google Cloud Messaging (GCM) to usługa, która obsługuje wysyłanie, routing i usługi kolejkowania komunikatów między aplikacjami klientów urządzeń przenośnych i aplikacji serwera. A *aplikacji klienckiej* jest uruchamiany na urządzeniu aplikacji z obsługą usługi GCM. *Serwera aplikacji* (udostępniony przez użytkownik lub jego firma) jest serwer z obsługą usługi GCM aplikacji klient komunikuje się za pośrednictwem usługi GCM:

[![GCM znajduje się między aplikacją klienta i serwera aplikacji](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

Przy użyciu usługi GCM, serwery aplikacji może wysyłać komunikaty do jednego urządzenia, grupy urządzeń lub liczbę urządzeń, które subskrybuje tematu. Aplikację klienta można użyć usługi GCM do subskrybowania podrzędne wiadomości z aplikacji serwera (na przykład, aby otrzymywać powiadomienia zdalne). Ponadto usługi GCM umożliwia aplikacji klienta do wysyłania wiadomości nadrzędnego do serwera aplikacji.

Informacje o implementacji serwera aplikacji dla usługi GCM, zobacz [o GCM połączenia serwera](https://developers.google.com/cloud-messaging/server).



## <a name="google-cloud-messaging-in-action"></a>Usługa Google Cloud Messaging w akcji

Gdy podrzędne wiadomości są wysyłane z serwera aplikacji do aplikacji klienckich, serwerów aplikacji wysyła wiadomość do *GCM połączenia serwera*; serwerze połączenia usługi GCM z kolei przekazuje komunikat do urządzenia, które działa aplikację klienta. Wiadomości mogą być wysyłane za pośrednictwem protokołu HTTP lub [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible obsługi wiadomości i obecności Protocol). Ponieważ aplikacje klienta nie są zawsze połączone lub uruchamiania usługi GCM połączenia serwera enqueues i magazyny wiadomości, wysłanie ich do aplikacji klienckich, połącz się ponownie i stają się dostępne. Podobnie GCM będzie umieścić w kolejce nadrzędnego komunikatów w aplikacji klienta do serwera aplikacji czy serwera aplikacji jest niedostępna.

GCM używa następujących poświadczeń do identyfikowania serwera aplikacji i aplikację klienta i używa tych poświadczeń do autoryzacji transakcji wiadomości za pomocą usługi GCM:

-   **Klucz interfejsu API** &ndash; *klucz interfejsu API* daje dostęp do Twojej aplikacji serwera do usług Google. GCM używa tego klucza do uwierzytelniania serwera aplikacji.
    Zanim użyjesz usługi GCM, należy najpierw uzyskać klucz interfejsu API z [konsoli dla deweloperów Google](https://console.developers.google.com/) tworząc *projektu*. Klucz interfejsu API powinny być zabezpieczone; Aby uzyskać więcej informacji o ochronie klucza interfejsu API, zobacz [najlepsze rozwiązania dotyczące bezpiecznego korzystania z kluczy interfejsu API](https://support.google.com/cloud/answer/6310037?hl=en).

-   **Identyfikator nadawcy** &ndash; *identyfikator nadawcy* zezwala serwerowi aplikacji aplikację klienta &ndash; jest unikatowy numer, który identyfikuje serwer aplikacji, którym zezwolono na wysyłanie komunikatów do aplikacji klienta.
    Identyfikator nadawcy jest także numer projektu; Identyfikator nadawcy można uzyskać z konsoli deweloperów Google, podczas rejestrowania projektu.

-   **Token rejestracji** &ndash; *tokenu rejestracji* jest tożsamość usługi GCM w aplikacji klienta na danym urządzeniu. Token rejestracji jest generowane w czasie wykonywania &ndash; aplikacji otrzymuje token rejestracji, gdy najpierw rejestruje z usługą GCM podczas uruchamiania na urządzeniu. Token rejestracji autoryzuje wystąpienia aplikacji klienta (działające na tym urządzeniu określonego) do odbierania wiadomości z usługi GCM.

-   **Identyfikator aplikacji** &ndash; tożsamości aplikacji klienta (niezależnie od wszelkich danego urządzenia) rejestruje odbierać komunikaty z usługi GCM. W systemie Android, identyfikator aplikacji jest rejestrowane w nazwę pakietu **AndroidManifest.xml**, takich jak `com.xamarin.gcmexample`.

[Ustawienie zapasowej Google Cloud Messaging](#settingup) (dalszej części tego przewodnika) zawiera szczegółowe instrukcje dotyczące tworzenia projektu i generowania tych poświadczeń.

W poniższych sekcjach opisano, jak te poświadczenia są używane, gdy aplikacje klienckie komunikują się z serwerami aplikacji za pomocą usługi GCM.



### <a name="registration-with-gcm"></a>Rejestracja w usłudze GCM

Aplikacji klienckiej, zainstalowane na urządzeniu, najpierw należy zarejestrować się GCM przed wiadomości mogą mieć miejsce. Aplikacja kliencka musi wykonać kroki rejestracji pokazano na poniższym diagramie:

[![Kroki rejestracji aplikacji](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  Aplikacja kliencka kontaktuje się z usługi GCM w celu uzyskania tokenu rejestracji przekazywanie identyfikator nadawcy do usługi GCM.

2.  GCM zwraca token rejestracji aplikacji klienckiej.

3.  Aplikacja kliencka przekazuje token rejestracji do serwerów aplikacji.

Serwer aplikacji buforuje tokenu rejestracji kolejnych komunikacji z aplikacji klienckiej. Opcjonalnie serwer aplikacji może wysyłać potwierdzenia powrót do aplikacji klienckiej, aby wskazać, czy odebrano tokenu rejestracji. Po wystąpieniu tego uzgadnianie aplikacji klienckiej mogą odbierać komunikaty z (lub wysyłanie komunikatów do) serwera aplikacji.

Gdy już aplikacji klienckiej chce odbierać komunikaty z serwerów aplikacji, może wysłać żądania do serwera aplikacji, można usunąć tokenu rejestracji. Jeśli aplikacja kliencka odbiera komunikaty tematu (co omówiono w dalszej części tego artykułu), można zrezygnować z tematu.
Jeśli aplikacja klienta zostanie odinstalowana z urządzenia, GCM wykryje to i automatycznie powiadamia serwer aplikacji, można usunąć tokenu rejestracji.

Firmy Google [rejestrowanie aplikacji klienta](https://developers.google.com/cloud-messaging/registration) opisano proces rejestracji bardziej szczegółowo; wyjaśniono wyrejestrowanie i anulowanie subskrypcji, a także opisano proces wyrejestrowanie po odinstalowaniu aplikacji klienta.



### <a name="downstream-messaging"></a>Podrzędne obsługi wiadomości

Gdy serwer aplikacji wysyła wiadomość podrzędne do aplikacji klienckiej, wykonuje kroki przedstawione na poniższym diagramie:

[![Podrzędne magazynie obsługi komunikatów i do przodu diagramu](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  Serwer aplikacji wysyła komunikat do usługi GCM.

2.  Jeśli urządzenie klienckie nie jest dostępna, serwer usługi GCM przechowuje wiadomości w kolejce do nowszej przesłania.

3.  Po udostępnieniu urządzenia klienta usługi GCM wysyła wiadomość do aplikacji klienckiej na tym urządzeniu.

4.  Aplikacja kliencka odbiera wiadomości z usługi GCM i odpowiednio je obsługuje. Na przykład jeśli komunikat jest zdalny powiadomień, jest prezentowane użytkownikowi.

W tym scenariuszu obsługi komunikatów (gdzie serwera aplikacji wysyła komunikat do aplikacji przez jednego klienta) komunikaty mogą zostać maksymalnie 4kB.

Szczegółowe informacje (w tym przykłady kodu) dotyczące odbierania podrzędne wiadomości usługi GCM w systemie Android, zobacz [zdalnego powiadomienia](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md).


#### <a name="topic-messaging"></a>Temat wiadomości

*Temat wiadomości* jest typem podrzędnym obsługi komunikatów, których serwer aplikacji wysyła pojedynczą wiadomość na wielu urządzeniach aplikacji klienta, które subskrypcji do tematu (na przykład prognozie pogody). Temat wiadomości mogą być maksymalnie 2KB, i tematu wiadomości obsługuje maksymalnie milion subskrypcji na aplikacji. Jeśli GCM jest używana tylko dla tematu wiadomości, do wysyłania tokenu rejestracji do serwerów aplikacji nie jest wymagana aplikacji klienckiej. Firmy Google [implementacja tematu wiadomości](https://developers.google.com/cloud-messaging/topic-messaging) wyjaśniono, jak wysyłać wiadomości z aplikacji serwera na wielu urządzeniach, które subskrypcji określonego tematu.



#### <a name="group-messaging"></a>Grupy do obsługi komunikatów

*Grupowanie komunikatów* jest typem podrzędnym obsługi komunikatów, których serwer aplikacji wysyła pojedynczą wiadomość na wielu urządzeniach aplikacji klienta, które należą do grupy (na przykład grupy urządzeń, które należą do jednego użytkownika). Grupy komunikaty mogą zostać maksymalnie 2KB długości w przypadku urządzeń z systemem iOS i maksymalnie 4KB długości w przypadku urządzeń z systemem Android. Grupa jest ograniczone do maksymalnie 20 członków. Firmy Google [Device Messaging grupy](https://developers.google.com/cloud-messaging/notifications) wyjaśniono, jak serwery aplikacji można wysłać wiadomość jeden do wielu wystąpień aplikacji klienta uruchomiony na urządzeniach, które należą do grupy.


### <a name="upstream-messaging"></a>Upstream Messaging

Jeśli Twoja aplikacja kliencka łączy się z serwera, który obsługuje [XMPP](https://developers.google.com/cloud-messaging/ccs), jak pokazano na poniższym diagramie może wysłać wiadomości do serwera aplikacji:

[![Nadrzędny diagram obsługi wiadomości](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  Aplikacja kliencka wysyła wiadomość do serwera usługi GCM XMPP połączenia.

2.  Po odłączeniu serwera aplikacji, serwer usługi GCM przechowuje wiadomości w kolejce do przesyłania dalej.

3.  Gdy serwer aplikacji jest ponownie połączony, GCM przekazuje komunikat do serwerów aplikacji.

4.  Serwer aplikacji analizuje komunikat, aby sprawdzić tożsamość aplikacji klienckiej, a następnie wysyła do usługi GCM w celu otrzymanie komunikatu "potwierdzenia".

5.  Serwer aplikacji przetwarza wiadomości.

Firmy Google [nadrzędnego wiadomości](https://developers.google.com/cloud-messaging/ccs#upstream) wyjaśniono, jak struktury wiadomości w formacie JSON i wyślij je do serwerów aplikacji, z systemem Server połączenia chmury na podstawie XMPP firmy Google.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Konfigurowanie usługi Google Cloud Messaging

Zanim użyjesz usługi GCM w aplikacji, należy najpierw uzyskać poświadczenia dostępu do serwerów usługi GCM firmy Google. W poniższych sekcjach opisano kroki wymagane do ukończenia tego procesu:



### <a name="enable-google-services-for-your-app"></a>Włącz usługi Google dla aplikacji

1.  Zaloguj się do [konsoli deweloperów Google](https://developers.google.com/mobile/add?platform=android) z programu Google konta (tj adres gmail), a następnie utwórz nowy projekt. Jeśli masz istniejący projekt, wybierz projekt, który ma być włączone usługi GCM. W poniższym przykładzie nowego projektu o nazwie **XamarinGCM** utworzeniu:

    [![Tworzenie projektu XamarinGCM](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  Następnie wprowadź nazwę pakietu aplikacji (w tym przykładzie nazwa pakietu jest **com.xamarin.gcmexample**) i kliknij przycisk **kontynuować, wybierz i skonfiguruj usługi**:

    [![Wprowadzanie nazwy pakietu](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    Pamiętaj, że ta nazwa pakietu jest również identyfikator aplikacji dla aplikacji.

3.  **Wybierz i skonfiguruj usługi** sekcja zawiera listę usług Google, które można dodać do aplikacji. Kliknij przycisk **Cloud Messaging**:

    [![Wybierz chmurę do obsługi komunikatów](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  Następnie kliknij przycisk **włączyć GOOGLE CLOUD MESSAGING**:

    [![Włączanie usługi Google Cloud Messaging](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  A **klucz interfejsu API serwera** i **identyfikator nadawcy** są generowane dla aplikacji. Zapisz te wartości, a następnie kliknij przycisk **Zamknij**:

    [![Klucz interfejsu API serwera i wyświetla identyfikator nadawcy](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    Chroń klucz interfejsu API &ndash; nie jest przeznaczony do użytku publicznego. Jeśli klucz interfejsu API zostanie naruszony, nieautoryzowane serwery można opublikować wiadomości dla aplikacji klienckich.
    [Najlepsze rozwiązania dotyczące bezpiecznego korzystania z kluczy interfejsu API](https://support.google.com/cloud/answer/6310037?hl=en) wskazówki przydatne do ochrony klucza interfejsu API.



### <a name="view-your-project-settings"></a>Wyświetl ustawienia projektu

Ustawienia projektu można wyświetlić w dowolnym momencie po zalogowaniu się do [Google Cloud Console](https://console.cloud.google.com/) i wybierając projektu. Na przykład można wyświetlić **identyfikator nadawcy** po wybraniu projektu w ściągania menu rozwijane u góry strony (w tym przykładzie nosi nazwę projektu **XamarinGCM**). Identyfikator nadawcy jest numer projektu, jak pokazano w tym zrzucie ekranu pokazano (identyfikator nadawcy w tym miejscu **9349932736**):

[![Wyświetlanie identyfikator nadawcy](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

Aby wyświetlić **klucz interfejsu API**, kliknij przycisk **Menedżer interfejsu API** , a następnie kliknij przycisk **poświadczenia**:

[![Wyświetlanie klucz interfejsu API](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>Dalsze informacje

-   Firmy Google [rejestrowanie aplikacji klienta](https://developers.google.com/cloud-messaging/registration) proces rejestracji klienta bardziej szczegółowo opisano oraz zawiera informacje o konfigurowaniu automatyczny i synchronizacja stanu rejestracji.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) i [RFC 6121](https://tools.ietf.org/html/rfc6121) wyjaśnić i zdefiniuj Extensible obsługi wiadomości i obecności protokołu (XMPP).



## <a name="summary"></a>Podsumowanie

W tym artykule podać Omówienie programu Google Cloud Messaging (GCM). Wyjaśniono jej różnych poświadczeń, które są używane do identyfikacji i autoryzować komunikatów między aplikacjami klienta i serwerów aplikacji. Go przedstawia najbardziej typowe scenariusze obsługi komunikatów, a jego szczegółowe kroki dotyczące rejestrowania aplikacji za pomocą usługi GCM w celu korzystania z usługi GCM usług.


## <a name="related-links"></a>Linki pokrewne

- [Obsługa wiadomości w chmurze](https://developers.google.com/cloud-messaging/)
