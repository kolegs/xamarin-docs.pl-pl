---
title: MDocArchiveToMsxDocConverter.exe nie znaleziono na serwerze. BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 6d7fe8f48d22c6b5d445fa8356e52f924549f6e0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30775679"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe nie znaleziono na serwerze. BaseCommand.OnRequest

> [!IMPORTANT]
> Ten problem został rozwiązany w nowszych wersjach programu Xamarin. Jednak jeśli problem wystąpi w najnowszej wersji oprogramowania, pliku [nowych usterek](~/cross-platform/troubleshooting/questions/howto-file-bug.md) z Twojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.


## <a name="error-message"></a>Komunikat o błędzie

Ten błąd może występować w *dziennik serwera Mac* w programie Visual Studio:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

Istnieją 2 osobnych problemów w tej wiadomości:

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    Ten błąd jest bezpieczne, ale jest również mylące. On [zostaną usunięte](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) w przyszłej wersji.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    Ten błąd jest prawdziwe problem. Niestety, ze względu na [ograniczenia](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) tego ślad stosu wyjątku *niekompletne*. Jeśli zauważysz ślad stosu niekompletne następująco w dzienniku serwera Mac można sprawdzić `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` pliku na hoście kompilacji Mac w celu znalezienia cały stos śledzenia.
