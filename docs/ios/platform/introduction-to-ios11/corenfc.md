---
title: Podstawowe NFC
description: "Tagi odczytu w pobliżu komunikacji Zbliżeniowej (NFC) przy użyciu systemu iOS 11"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2016
ms.openlocfilehash: 72c19ef09843c137514983b1d7ee7104e3cb32c5
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="core-nfc"></a>Podstawowe NFC

_Tagi odczytu w pobliżu komunikacji Zbliżeniowej (NFC) przy użyciu systemu iOS 11_

CoreNFC jest nowa struktura w systemie iOS 11, która zapewnia dostęp do _komunikacji Zbliżeniowej_ radiowych (NFC) można odczytać tagi z aplikacji. Działa na telefonie iPhone 7, Plus 7, 8, 8 Plus i X.

Czytnik tagu NFC na urządzeniach z systemem iOS obsługuje wszystkie typy tagu NFC 1 do 5, która zawiera _Format wymiany danych NFC_ informacji (NDEF).

Istnieją pewne ograniczenia pod uwagę:

- CoreNFC obsługuje tylko tag odczytywania (nie zapisywania lub formatowanie).
- Skanowanie znacznika musi być inicjowane przez użytkownika i limit czasu po 60 sekundach.
- Aplikacje muszą być widoczne na pierwszym planie skanowania.
- CoreNFC należy badać tylko na urządzeniach rzeczywistych (nie w symulatorze).

Ta strona zawiera opis konfiguracji wymaganej do użycia CoreNFC i przedstawia sposób użycia za pomocą interfejsu API ["TFCTagReader" przykładowy kod](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Konfiguracja

Aby włączyć CoreNFC, należy skonfigurować trzy elementy w projekcie:

- **Info.plist** klucz prywatności.
- **Entitlements.plist** wpisu.
- Profil inicjowania obsługi administracyjnej z **odczytu tagu NFC** możliwości.

### <a name="infoplist"></a>Info.plist

Dodaj **NFCReaderUsageDescription** klucz prywatności i tekst, który jest wyświetlany na ekranie podczas skanowania jest wykonywana. Użyj wiadomość jest odpowiedni dla twojej aplikacji (na przykład wyjaśnić celem skanowanie):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Aplikacja musi żądać **obok pola komunikacji Tag odczytu** Sparuj funkcji przy użyciu klucza i wartości w Twojej **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Profil inicjowania obsługi administracyjnej

Utwórz nową **identyfikator aplikacji** i upewnij się, że **odczytu tagu NFC** zaznaczono usługi:

[![Strona nowy identyfikator aplikacji portalu dewelopera z odczytu tagu NFC wybrane](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Należy następnie utwórz nowy profil inicjowania obsługi administracyjnej dla tego Identyfikatora aplikacji, a następnie pobrać i zainstalować go na komputerze Mac.

## <a name="reading-a-tag"></a>Odczytywanie Tag

Po skonfigurowaniu projektu dodać `using CoreNFC;` na początku pliku i wykonaj następujące trzy kroki, aby zaimplementować NFC tagu odczytu funkcji:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Wdrożenie `INFCNdefReaderSessionDelegate`

Interfejs ma dwie metody implementowane:

- `DidDetect` — Wywoływana, gdy tag pomyślnie jest do odczytu.
- `DidInvalidate` — Wywoływana, gdy wystąpi błąd lub osiągnięciu 60 drugi limitu czasu.

#### <a name="diddetect"></a>DidDetect

W przykładowym kodzie każdy komunikat zeskanowane został dodany do tabeli widoku:

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

Ta metoda może zostać wywołana wiele razy (i tablicę wiadomości mogą być przekazywane w) Jeśli sesji umożliwia wielu odczytów znaczników. Ta opcja jest ustawiona przy użyciu trzeci parametr funkcji `Start` — metoda (wyjaśniono w [krok 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Unieważnienie może wystąpić z kilku powodów:

- Wystąpił błąd podczas skanowania.
- Aplikacja przestała być na pierwszym planie.
- Użytkownik zdecydował się anulować skanowania.
- Skanowanie zostało anulowane przez aplikację.

Poniższy kod błędu:

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

Gdy sesja została unieważniona, należy utworzyć nowego obiektu sesji w celu Skanuj ponownie.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Rozpocznij `NFCNdefReaderSession`

Skanowanie powinien się rozpoczynać żądania użytkownika, takich jak naciśnięcie przycisku.
Poniższy kod tworzy i rozpoczyna sesję skanowania:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Parametry `NFCNdefReaderSession` konstruktora są następujące:

- `delegate` — Implementacja `INFCNdefReaderSessionDelegate`. W przykładowym kodzie delegat jest zaimplementowana w kontroler widoku tabeli, w związku z tym `this` jest używany jako parametr delegata.
- `queue` — Kolejki wywołań zwrotnych są obsługiwane. Można ją `null`, w którym to przypadku należy użyć `DispatchQueue.MainQueue` podczas aktualizowania formantów interfejsu użytkownika (jak pokazano w przykładzie).
- `invalidateAfterFirstRead` — Gdy `true`, skanowania zatrzymuje po pierwszym pomyślnym skanowaniu; gdy `false` skanowanie będzie kontynuowana i wielu zwracane, dopóki skanowanie zostało anulowane lub osiągnięciu 60 drugi limitu czasu.


### <a name="3-cancel-the-scanning-session"></a>3. Anuluj skanowania sesji

Użytkownik może anulować sesję skanowania za pomocą przycisku dostarczane przez system w interfejsie użytkownika:

![Przycisk "Anuluj" podczas skanowania](corenfc-images/scan-cancel-sml.png)

Aplikacji programowo anulować skanowanie pod wywołując `InvalidateSession` metody:

```csharp
Session.InvalidateSession();
```

W obu przypadkach delegata w `DidInvalidate` metoda zostanie wywołana.

## <a name="summary"></a>Podsumowanie

CoreNFC umożliwia aplikacji odczytanie danych z tagów NFC. Obsługuje odczytywanie różnych formatach tag (NDEF typy 1 do 5), ale nie obsługuje zapisywania ani formatowania.


## <a name="related-links"></a>Linki pokrewne

- [NFCTagReader (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Wprowadzenie do podstawowych NFC (WWDC) (klip wideo)](https://developer.apple.com/videos/play/wwdc2017/718/)
