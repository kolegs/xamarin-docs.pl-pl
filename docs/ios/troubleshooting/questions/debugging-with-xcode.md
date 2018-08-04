---
title: Debugowanie aplikacji platformy Xamarin.iOS za pomocą edytora Xcode
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5FDDEDB3-AEB9-4D9C-9F7B-FEFAA9AF0031
ms.technology: xamarin-ios
author: migueldeicaza
ms.author: miguel
ms.date: 02/22/2018
ms.openlocfilehash: e0127d4b24236d350e5fa967110316544c320d0f
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514675"
---
# <a name="debugging-xamarinios-apps-with-xcode"></a>Debugowanie aplikacji platformy Xamarin.iOS za pomocą edytora Xcode

Mogą istnieć scenariusze, w którym chcesz debugowanie niektórych części aplikacji platformy Xamarin.iOS przy użyciu środowiska Xcode. Chociaż nie można debugować kodu .NET w nim, nadal będzie możliwe jest debugowanie kodu natywnego i korzystać z niektórych natywnych wizualizatorów w środowisku Xcode.

## <a name="walkthrough"></a>Przewodnik

W trakcie wbudowaną obsługę programu Xcode debugowania w programie Visual Studio dla komputerów Mac umożliwia następujące czynności w tym:

1. Utwórz aplikację dla systemu iOS w narzędziu Xcode z tym samym Identyfikatorem pakietu, co w aplikacji platformy Xamarin.
   
    - Identyfikator pakietu projektu Xamarin.iOS można znaleźć, otwierając **Info.plist** pliku:

        ![Edytowanie pliku Info.plist](debugging-with-xcode-images/vsmac-infoplist.png "Info.list do edycji")

    - W środowisku Xcode ustawiania identyfikatora pakietu podczas tworzenia projektu lub wybranie docelowego w projekcie:

        ![Ustawienie identyfikatora pakietu w środowisku Xcode](debugging-with-xcode-images/xcode-bundle.png "ustawiania identyfikatora pakietu w środowisku Xcode")

2. Zmień projekt Xcode oczekiwania na uruchomienie zamiast automatyczne uruchamianie aplikacji:

    - Otwórz **edytować schemat panelu** , wybierając **produktu > schematu > Edytuj schemat** lub za pomocą **cmd⌘ + <** skróty klawiaturowe.

    - Wybierz **Uruchom** schemat, a następnie w prawym panelu, użytkownik powinien zostać wyświetlony **Uruchom** opcje. Wybierz **oczekiwania dla pliku wykonywalnego do uruchomienia** i kliknij przycisk **Zamknij**.

        ![Poczekaj, aż plik wykonywalny do uruchomienia](debugging-with-xcode-images/xcode-schemes.png "oczekiwania dla pliku wykonywalnego do uruchomienia")

3. Uruchom projekt Xcode.

    Zainstaluje fikcyjnego aplikację Xcode na twoim urządzeniu, ale nie będą uruchamiane.

4. Uruchamianie aplikacji platformy Xamarin.

    Xcode należy dołączyć do aplikacji platformy Xamarin, po jego uruchomieniu.

### <a name="caveats"></a>Ostrzeżenia

Może być konieczne wprowadź niewielką zmianę w aplikacji platformy Xamarin.iOS przy każdym uruchomieniu. W przeciwnym razie Visual Studio for Mac zostanie wykryta aplikacja nie musi być kompilowany *i* jest już zainstalowany, i go nie będzie ponownie ją zainstaluj nad aplikacją fikcyjnego Xcode.

## <a name="alternative---using-lldb"></a>Alternatywa — przy użyciu lldb

Jeśli masz doświadczenia z używaniem **lldb** z wiersza polecenia jest znacznie prostsze rozwiązaniem.

W powłoce wpisz następujące polecenie:

```bash
touch ~/.mtouch-launch-with-lldb
```

Otrzymasz instrukcje **dane wyjściowe aplikacji** okna co zrobić, ale zasadniczo po uruchomieniu aplikacji, będzie można używać **lldb** z wiersza polecenia w celu debugowania aplikacji.
