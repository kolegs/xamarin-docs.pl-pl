---
title: Podstawowe NFC w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano, jak odczytać w pobliżu pola tagi komunikacji platformy Xamarin.iOS przy użyciu interfejsów API, wprowadzona w systemie iOS 11.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 1381a4564f93fd091f181949454df3f06b31ae6b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350836"
---
# <a name="core-nfc-in-xamarinios"></a>Podstawowe NFC w rozszerzeniu Xamarin.iOS

_Tagi odczytu niemal komunikacji Zbliżeniowej (NFC) przy użyciu systemu iOS 11_

CoreNFC to nowe środowisko w systemie iOS 11, która zapewnia dostęp do _komunikacji Zbliżeniowej_ radiowych (NFC) można odczytać tagi z w ramach aplikacji. Działa na urządzeniu iPhone 7, Plus 7, 8, 8, Plus i X.

Czytnik tagów NFC na urządzeniach z systemem iOS obsługuje wszystkie typy tagów NFC 1 do 5, która zawiera _Format wymiany danych NFC_ informacji (NDEF).

Istnieją pewne ograniczenia, w których trzeba pamiętać:

- CoreNFC obsługuje tylko tag do czytania (Zapisywanie i formatowanie).
- Skanowanie tagu musi być inicjowane przez użytkownika i limit czasu po 60 sekundach.
- Aplikacje muszą być widoczne na pierwszym planie skanowania.
- CoreNFC należy badać tylko na rzeczywistych urządzeniach (a nie w symulatorze).

Ta strona zawiera opis konfiguracji wymaganej do użycia CoreNFC i przedstawia sposób użycia interfejsu API przy użyciu ["TFCTagReader" przykładowy kod](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Konfiguracja

Aby włączyć CoreNFC, należy skonfigurować trzy elementy w projekcie:

- **Info.plist** klucz prywatności.
- **Plik Entitlements.plist** wpisu.
- Profil aprowizacji z **odczytywanie tagów NFC** możliwości.

### <a name="infoplist"></a>Info.plist

Dodaj **NFCReaderUsageDescription** klucz prywatności i tekst, który jest wyświetlany użytkownikowi podczas skanowania. Użyj wiadomość jest odpowiedni dla aplikacji (na przykład wyjaśnić celem skanowania):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Aplikacja musi żądać **odczytywanie tagów komunikacji w pobliżu pola** funkcji przy użyciu klucz/wartość pary w swojej **plik Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Profil aprowizacji

Utwórz nową **Identyfikatora aplikacji** i upewnij się, że **odczytywanie tagów NFC** zaznaczono usługi:

[![Strona nowego Identyfikatora aplikacji portalu dla deweloperów z odczytywanie tagów NFC wybrane](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Należy następnie utworzyć nowy profil inicjowania obsługi administracyjnej dla tego Identyfikatora aplikacji, a następnie pobrać i zainstalować ją na programowanie komputerów Mac.

## <a name="reading-a-tag"></a>Odczytywanie tagów

Po skonfigurowaniu projektu należy dodać `using CoreNFC;` na początku pliku i postępuj zgodnie z następujące trzy kroki, aby zaimplementować NFC tag odczytu funkcji:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Implementowanie `INFCNdefReaderSessionDelegate`

Interfejs oferuje dwie metody do zaimplementowania:

- `DidDetect` — Wywoływane, gdy tag pomyślnie jest do odczytu.
- `DidInvalidate` — Wywoływane, gdy wystąpi błąd lub osiągnięty zostanie 60, drugi limit czasu.

#### <a name="diddetect"></a>DidDetect

W przykładowym kodzie każdego zeskanowane komunikat zostanie dodany do widoku tabeli:

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

Ta metoda może być wywoływana wiele razy (i tablicą wiadomości mogą być przekazywane w) Jeśli sesji umożliwia wielu odczytów tagu. Ta opcja jest ustawiona za pomocą trzeciego parametru `Start` — metoda (wyjaśnione w [kroku 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Unieważniania może wystąpić z kilku powodów:

- Wystąpił błąd podczas skanowania.
- Aplikacja przestała się na pierwszym planie.
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

Skanowanie na początek z żądaniem użytkownika, takie jak naciśnięcie przycisku.
Poniższy kod tworzy i uruchamia sesję skanowania:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Parametry `NFCNdefReaderSession` Konstruktor są następujące:

- `delegate` — Implementacja `INFCNdefReaderSessionDelegate`. W przykładowym kodzie delegata jest zaimplementowana w kontroler widoku tabeli, w związku z tym `this` jest używany jako parametr delegata.
- `queue` — Kolejki wywołania zwrotne są obsługiwane. Może być `null`, w którym to przypadku należy użyć `DispatchQueue.MainQueue` podczas aktualizowania kontrolek interfejsu użytkownika (jak pokazano w przykładzie).
- `invalidateAfterFirstRead` — Gdy `true`, skanowanie zatrzymuje się po pierwszym pomyślnym skanowaniu; gdy `false` skanowanie będzie kontynuowany, a wiele wyników zwróconych, dopóki skanowanie zostało anulowane lub 60, drugi limit zostanie osiągnięty.


### <a name="3-cancel-the-scanning-session"></a>3. Anulowanie sesji skanowania

Użytkownik może anulować skanowania sesji za pośrednictwem przycisku dostarczane przez system w interfejsie użytkownika:

![Przycisk anulowania podczas skanowania](corenfc-images/scan-cancel-sml.png)

Aplikacja może programowo anulować skanowania przez wywołanie metody `InvalidateSession` metody:

```csharp
Session.InvalidateSession();
```

W obu przypadkach delegata firmy `DidInvalidate` metoda zostanie wywołana.

## <a name="summary"></a>Podsumowanie

CoreNFC umożliwia aplikacji odczytywanie danych z tagów NFC. Obsługuje wiele formatów tag (typy NDEF 1 do 5) do czytania, ale nie obsługuje zapisywania ani formatowania.


## <a name="related-links"></a>Linki pokrewne

- [NFCTagReader (przykład)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Wprowadzenie do podstawowych NFC (WWDC) (wideo)](https://developer.apple.com/videos/play/wwdc2017/718/)
