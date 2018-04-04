---
title: Zwracany typ System.Exception AMDeviceNotificationSubscribe...
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 257e0c14de3c23825b6abe6601c25438db81c58e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="systemexception-amdevicenotificationsubscribe-returned-"></a>Zwracany typ System.Exception AMDeviceNotificationSubscribe...

> [!IMPORTANT]
> Ten problem został rozwiązany w nowszych wersjach programu Xamarin. Jednak jeśli problem wystąpi w najnowszej wersji oprogramowania, pliku [nowych usterek](~/cross-platform/troubleshooting/questions/howto-file-bug.md) z Twojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.


## <a name="fix"></a>Poprawka

1.  Kasowanie `usbmuxd` proces tak, aby system zostanie uruchomiony ponownie go:

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  Jeśli to nie rozwiąże problemu, ponowny rozruch komputerów Mac.

## <a name="error-message"></a>Komunikat o błędzie

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

Ten komunikat może się pojawić okna dialogowego błędu, przy pierwszym uruchomieniu programu Visual Studio dla komputerów Mac lub w `mtbserver.log` plik w aplikacji platformy Xamarin.iOS hosta kompilacji (**hosta kompilacji Xamarin.iOS > Wyświetl dziennik kompilacji hosta**).

Należy pamiętać, że jest to rzadko problem. Jeśli program Visual Studio występują problemy z połączeniem hosta kompilacji Mac, są inne błędy, które mogą być wyświetlane w `mtbserver.log` pliku.

### <a name="errors-in-systemlog"></a>Błędy w system.log

W niektórych przypadkach dwa błąd komunikaty mogą być wyświetlane również wielokrotnie w `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## <a name="additional-information"></a>Dodatkowe informacje

Jeden wynik na główną przyczynę tego błędu to, że OS X usług systemowych odpowiedzialny za raportowania informacji urządzenia i symulatora systemu iOS mogą w rzadkich przypadkach wprowadź nieoczekiwanym stanie. Xamarin nie mogą oddziaływać prawidłowo z usługami systemu w tym stanie. Ponowne uruchomienie komputera uruchamia ponownie usługi systemowe i rozwiązuje problem.

Oparte na błędy z `system.log` wygląda na to, że ten problem może być związana z Bonjour (`mDNSResponder`). Zmiana między różnymi sieciami sieci Wi-Fi wydaje się zwiększyć prawdopodobieństwo naciśnięcie problem.

## <a name="references"></a>Odwołania

*   [Usterka 11789 - MonoTouch.MobileDevice.MobileDeviceException: AMDeviceNotificationSubscribe zwrócił: 0xe8000063 [NORESPONSE ROZWIĄZANY]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
