---
title: Android Beam
ms.topic: article
ms.prod: xamarin
ms.assetid: 4172A798-89EC-444D-BC0C-0A7DD67EF98C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: bea8480c66a2ecf499375636c98511ca55ce7693
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="android-beam"></a>Android Beam

Android światła jest nową technologią w pobliżu komunikacji Zbliżeniowej (NFC) w systemie Android 4, który umożliwia aplikacjom na udostępnianie informacji przez NFC w pobliżu.

[![Diagram pokazujący dwoma urządzeniami w pobliżu udostępnianie informacji](android-beam-images/androidbeam.png)](android-beam-images/androidbeam.png)

Android światła działa przez wypychanie wiadomości za pośrednictwem NFC, gdy dwa urządzenia znajdują się w zakresie. Urządzenia około 4cm od siebie mogą udostępniać danych przy użyciu światła systemu Android. Działanie na jednym urządzeniu tworzy komunikat i określa działania (lub działań) które może obsłużyć wypychanie go. Gdy działanie określonego znajduje się na pierwszym planie i urządzenia znajdują się w zakresie, Android światła przeprowadzi wypychanie komunikat do drugiego urządzenia. Na urządzenie odbierające celem jest wywoływany, zawierający dane komunikatów.

Android obsługuje Ustawianie komunikatów z systemem Android światła na dwa sposoby:

-   `SetNdefPushMessage` — Przed Android światła jest inicjowane, aplikację można wywołać SetNdefPushMessage, aby określić NdefMessage do dystrybuowania za pośrednictwem NFC i działania, który wypycha go. Ten mechanizm najlepiej jest używany, gdy wiadomość nie powoduje zmiany, gdy aplikacja jest w użyciu.

-   `SetNdefPushMessageCallback` — Po zainicjowaniu światła systemu Android, aplikacja może obsłużyć wywołanie zwrotne do utworzenia NdefMessage. Mechanizm ten umożliwia tworzenie komunikatu opóźnionych urządzeń znajdujących się w zakresie. Obsługuje scenariusze, w którym komunikat może się różnić oparte na tym, co się dzieje w aplikacji.


W obu przypadkach do wysyłania danych z systemem Android światła aplikacji wysyła `NdefMessage`, pakowanie danych w kilku `NdefRecords`. Spójrzmy na klucz wskazuje, które muszą być kierowane przed firma Microsoft może wyzwolić światła systemu Android. Najpierw będzie współpracujemy z wywołania zwrotnego styl tworzenie `NdefMessage`.

<a name="Creating_a_Message" />

## <a name="creating-a-message"></a>Tworzenie komunikatu

Firma Microsoft może rejestrowania wywołań zwrotnych z `NfcAdapter` w działaniu `OnCreate` metody. Na przykład przy założeniu `NfcAdapter` o nazwie `mNfcAdapter` jest zadeklarowany jako zmienną klasy w działaniu, firma Microsoft może zapisywać następujący kod, aby utworzyć wywołania zwrotnego, które utworzy komunikat:

```csharp
mNfcAdapter = NfcAdapter.GetDefaultAdapter (this);
mNfcAdapter.SetNdefPushMessageCallback (this, this);
```

Działania, która implementuje `NfcAdapter.ICreateNdefMessageCallback`, są przekazywane do `SetNdefPushMessageCallback` metod powyżej. Po zainicjowaniu światła Android wywoła system `CreateNdefMessage`, z można konstruować działania `NdefMessage` w sposób przedstawiony poniżej:

```csharp
public NdefMessage CreateNdefMessage (NfcEvent evt)
{
    DateTime time = DateTime.Now;
    var text = ("Beam me up!\n\n" + "Beam Time: " +
        time.ToString ("HH:mm:ss"));
    NdefMessage msg = new NdefMessage (
        new NdefRecord[]{ CreateMimeRecord (
            "application/com.example.android.beam",
            Encoding.UTF8.GetBytes (text)) });
        } };
    return msg;
}

public NdefRecord CreateMimeRecord (String mimeType, byte [] payload)
{
    byte [] mimeBytes = Encoding.UTF8.GetBytes (mimeType);
    NdefRecord mimeRecord = new NdefRecord (
        NdefRecord.TnfMimeMedia, mimeBytes, new byte [0], payload);
    return mimeRecord;
}
```

<a name="Receiving_a_Message" />

## <a name="receiving-a-message"></a>Odbieranie wiadomości

Po stronie odbiorczej systemu wywołuje celem z `ActionNdefDiscovered` akcji, z których firma Microsoft może wyodrębnić NdefMessage w następujący sposób:

```csharp
IParcelable [] rawMsgs = intent.GetParcelableArrayExtra (NfcAdapter.ExtraNdefMessages);
NdefMessage msg = (NdefMessage) rawMsgs [0];
```

Pełny przykład kodu używającej światła systemu Android, pokazano uruchomiony na zrzucie ekranu poniżej, aby zapoznać [Android światła pokaz](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/) w galerii próbki.

[![Zrzuty ekranu przykład z pokaz światła systemu Android](android-beam-images/24.png)](android-beam-images/24.png)



## <a name="related-links"></a>Linki pokrewne

- [Pokaz światła systemu android (przykład)](https://developer.xamarin.com/samples/monodroid/AndroidBeamDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
