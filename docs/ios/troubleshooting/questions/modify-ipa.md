---
title: "Można pliki, aby dodać lub usunąć pliki z pliku IPA po utworzeniu go w programie Visual Studio?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: be792c95de7aa66d64278e47b2ca6b354e611273
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Można pliki, aby dodać lub usunąć pliki z pliku IPA po utworzeniu go w programie Visual Studio?

Tak, można, ale trzeba będzie zwykle, że należy ponownie podpisać `.app` pakietu po wprowadzeniu zmian.

Należy pamiętać, że zmodyfikowanie `.ipa` pliku nie jest konieczne w normalnych warunkach użytkowania. W tym artykule podano wyłącznie do celów informacyjnych.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Przykład: usuwanie pliku z `.ipa` archiwum

W tym przykładzie założono, że nazwa projektu platformy Xamarin.iOS jest `iPhoneApp1` i `generated session id` jest `cc530d20d6b19da63f6f1c6f67a0a254`

1.  Tworzenie `.ipa` pliku normalne z programu Visual Studio.

2.  Przełączyć się do hosta kompilacji Mac.

3.  Znajdź kompilacji w `~/Library/Caches/Xamarin/mtbs/builds` folderu. Można wkleić tę ścieżkę do **wyszukiwania > Przejdź > Przejdź do folderu** do przeglądania folderów w wyszukiwanie. Wyszukaj folder, w którym jest zgodna z nazwą projektu. W tym folderze, wyszukaj folder, który odpowiada `generated session id` kompilacji. Najprawdopodobniej będzie to podfolder, który ma czas ostatniej modyfikacji.

4.  Otwórz nowe `Terminal.app` okna.

5.  Typ `cd ` do okna Terminal.app i następnie przeciągania i upuszczania `generated session id` do folderu `Terminal.app` okno:

    ![](modify-ipa-images/session-id-folder.png "Lokalizowanie folderu identyfikator sesji wygenerowane w wyszukiwania")

6.  Wpisz klucz zwracany Zmień katalog na `generated session id` folderu.

7.  Rozpakuj `.ipa` plików do tymczasowej `old/` folder przy użyciu następującego polecenia. Dostosuj `Ad-Hoc` i `iPhoneApp1` nazw zgodnie z potrzebami dla określonego projektu.

    > jw. stare bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa - xk /

8.  Zachowaj `Terminal.app` otwartego okna.

9.  Usuń pliki, które z `.ipa`. Można przenieść je do Kosza za pomocą wyszukiwania, lub usuń je przy użyciu wiersza polecenia `Terminal.app`. Aby wyświetlić zawartość `Payload/iPhone` pliku w polu wyszukiwania, kliknij Control plik i wybierz **Pokaż zawartość pakietu**.

10.  Przy użyciu tej samej metody ogólne w kroku 3, Znajdź plik dziennika pod `~/Library/Logs/Xamarin/MonoTouchVS/` mający nazwę projektu i `generated session id` w nazwie: ![ ] (modify-ipa-images/build-log.png "zlokalizować w wyszukiwarce dziennika kompilacji projektu")

11.  Otwórz na przykład dziennika kompilacji z kroku 10, kliknij go dwukrotnie.

12.  Znajdź wiersz, który zawiera `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13.  Typ `/usr/bin/codesign ` do okna Terminal.app w kroku 8.

14.  Skopiuj wszystkie argumenty, począwszy od `-v` z wiersza w kroku 12 i wklej je do okna Terminal.app.

15.  Ostatni argument można zmienić `.app` pakietu znajduje się w obrębie `old/Payload/` folder, a następnie uruchom polecenie.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  Zmiany w `old/` katalogu w terminalu:

```bash
cd old
```

17.  ZIP się zawartość katalogu w nowym `.ipa` plik za pomocą `zip` polecenia. Możesz zmienić `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` argument wyjściowy `.ipa` pliku wszędzie tam, gdzie chcesz:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>Typowe komunikaty o błędach

Jeśli widzisz `Invalid Signature. A sealed resource is missing or invalid.`, zwykle oznacza to, że coś została zmieniona w `.app` pakietu, a `.app` pakietu nie został poprawnie ponownie podpisany później. Należy również zauważyć, że jeśli chcesz utworzyć `.ipa` z profilem dystrybucji możesz _musi_ zbudować oryginału `.ipa` z profilem dystrybucji. W przeciwnym razie `Entitlements.xcent` będzie nieprawidłowa.

Aby dać konkretny przykład sposobu ten błąd może wystąpić, jeśli uruchomisz następujące `codesign --verify` polecenia w oknie terminalu po wykonaniu kroku 9, zostanie wyświetlony błąd wraz z dokładne przyczyny tego błędu:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

I procesu weryfikacji sklepu z aplikacjami będzie zgłaszać podobne komunikat o błędzie:

> Błąd ITMS-90035: "nieprawidłowy podpis. Zapieczętowane zasobów jest brakujący lub nieprawidłowy. Dane binarne w ścieżce [iPhoneApp1.app/iPhoneApp1] zawiera nieprawidłowy podpis. Upewnij się, że zalogowano aplikacji za pomocą certyfikatu dystrybucji, a nie certyfikat ad hoc lub certyfikatu deweloperskiego. Sprawdź, czy ustawienia podpisywania kodu w środowisku Xcode są poprawne, na poziomie docelowej (które przesłaniają wszystkie wartości na poziomie projektu). Ponadto upewnij się, że pakiet przekazywany został zbudowany przy użyciu docelowej wersji w środowisku Xcode nie docelowy symulatora. Jeśli masz pewność, że ustawienia podpisywania kodu są prawidłowe, wybierz pozycję "Wyczyść wszystko" w programie Xcode, Usuń katalog "kompilacji" w wyszukiwaniu i skompiluj ponownie urządzenie docelowe wersji. Aby uzyskać więcej informacji, zapoznaj się [https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
