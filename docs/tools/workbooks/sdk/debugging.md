---
title: Debugowanie integracji
description: W tym dokumencie opisano sposób debugowania skoroszyty Xamarin integracji, zarówno po stronie agenta, jak i po stronie klienta w systemach Windows i Mac.
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: topgenorth
ms.author: toopge
ms.date: 06/19/2018
ms.openlocfilehash: 6e37b1ac3d0fb78b5737ebe97b5a28ab40adb648
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269060"
---
# <a name="debugging-integrations"></a>Debugowanie integracji

## <a name="debugging-agent-side-integrations"></a>Debugowanie po stronie agenta integracji

Debugowanie po stronie agenta integracji najlepiej odbywa się przy użyciu metody rejestrowania udostępniane przez `Log` klasy w `Xamarin.Interactive.Logging`. Zobacz [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) dla metod do wywołania.

Na macOS, wiadomości dziennika są wyświetlane w obu menu Podgląd dziennika (**okna > Podgląd dziennika**) i w dzienniku klienta. W systemie Windows komunikaty są wyświetlane tylko w dziennika klienta, ponieważ nie ma Podgląd dziennika nie istnieje.

Dziennik klienta jest w następujących lokalizacjach w macOS i Windows:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- System Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Jedyną operacją, należy pamiętać o oznacza, że podczas ładowania integracji za pośrednictwem zwykle `#r` mechanizmu podczas tworzenia zestawu integracji będą pobierane jako _zależności_ skoroszytu i spakowanych z nim, jeśli jest ścieżką bezwzględną nie jest używany. Może to spowodować zmiany prawdopodobnie nie są propagowane, jakby odbudowywania integrację nic nie zostało.

## <a name="debugging-client-side-integrations"></a>Debugowanie po stronie klienta integracji

Gdy integracji po stronie klienta są napisane w języku JavaScript i ładowane do naszej powierzchni przeglądarki sieci web (zobacz [architektura](~/tools/workbooks/sdk/architecture.md) dokumentacji), najlepszym sposobem debugowania ich korzystania z narzędzi deweloperskich WebKit dla komputerów Mac, lub za pomocą selektora F12 w systemie Windows .

Oba zestawy narzędzia pozwalają na wyświetlanie źródłowy JavaScript/TypeScript, ustaw punkty przerwania, wyświetlić dane wyjściowe z konsoli i sprawdzanie i modyfikowanie modelu DOM.

### <a name="mac"></a>Mac

Aby włączyć narzędzia deweloperskie dla skoroszytów Xamarin dla komputerów Mac, uruchom następujące polecenie w terminalu:

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

a następnie ponownie uruchom program Xamarin skoroszytów. Po wykonaniu powinna zostać wyświetlona **sprawdzić Element** są wyświetlane w menu kontekstowe programu kliknij prawym przyciskiem myszy, a nowy **Developer** okienko będą dostępne w preferencjach skoroszytów. Ta opcja pozwala wybrać otwarty podczas uruchamiania narzędzi deweloperskich:

[![Okienko Developer](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Ta opcja jest tylko do ponownego uruchomienia również — konieczne będzie ponowne uruchomienie klienta skoroszytów w celu wprowadzone na nowych skoroszytów. Aktywowanie narzędzi deweloperskich za pomocą menu kontekstowego lub preferencje wyświetli znanych Safari interfejsu użytkownika:

[![Narzędzia deweloperów Safari](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Informacje dotyczące korzystania z narzędzi deweloperskich programu Safari, zobacz [dokumentacji inspektora WebKit][webkit-docs].

### <a name="windows"></a>Windows

W systemie Windows, zespołu programu IE udostępnia narzędzia, znany jako "Wybór F12" który jest zdalny debuger dla osadzonych wystąpień programu Internet Explorer. Narzędzie można znaleźć w następującej lokalizacji:

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Uruchom selektor F12 i powinna zostać wyświetlona osadzonych wystąpienie, które obsługuje skoroszyty powierzchni klienta na liście. Wybierz i znanych naciśnięcia klawisza F12, narzędzi debugowania w programie Internet Explorer zostanie wyświetlony, dołączone do klienta:

[![Narzędzia F12](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
