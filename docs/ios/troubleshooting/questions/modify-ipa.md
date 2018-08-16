---
title: Można dodać pliki lub Usuń pliki z pliku IPA po utworzeniu go w programie Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6C3082FB-C3F1-4661-BE45-64570E56DE7C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 366308774a7302e54b0d47753256638e89d97b82
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350985"
---
# <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studio"></a>Można dodać pliki lub Usuń pliki z pliku IPA po utworzeniu go w programie Visual Studio?

Tak, jest możliwe, ale zazwyczaj będzie wymagać, że należy ponownie podpisać `.app` pakietu po wprowadzeniu zmian.

Należy pamiętać, że zmodyfikowanie `.ipa` pliku nie jest konieczne w czasie normalnego użycia. W tym artykule podano wyłącznie w celach informacyjnych.

## <a name="example-removing-a-file-from-a-ipa-archive"></a>Przykład: usuwanie pliku z `.ipa` archiwum

W tym przykładzie przyjęto założenie, że nazwa projektu Xamarin.iOS jest `iPhoneApp1` i `generated session id` jest `cc530d20d6b19da63f6f1c6f67a0a254`

1.  Tworzenie `.ipa` plik w zwykły sposób, w programie Visual Studio.

2.  Przeprowadź hostem kompilacji komputera Mac.

3.  Znajdowanie kompilacji w `~/Library/Caches/Xamarin/mtbs/builds` folderu. Możesz wkleić tę ścieżkę do **wyszukiwania > Przejdź > Przejdź do folderu** do Przeglądaj w poszukiwaniu folderu w programie Finder. Zwróć uwagę na folder, który jest zgodna z nazwą projektu. W tym folderze, poszukaj folderu odpowiadającego `generated session id` kompilacji. Najprawdopodobniej będzie to podfolder, który ma ostatni czas modyfikacji.

4.  Otwórz nowy `Terminal.app` okna.

5.  Typ `cd ` do okna Terminal.app i następnie przeciągania i upuszczania `generated session id` do folderu `Terminal.app` okna:

    ![](modify-ipa-images/session-id-folder.png "Lokalizowanie na początku folder id sesji wygenerowany w programie Finder")

6.  Wpisz klawisz ENTER, aby zmienić katalog na `generated session id` folderu.

7.  Rozpakuj `.ipa` plików do tymczasowej `old/` folderu przy użyciu następującego polecenia. Dostosuj `Ad-Hoc` i `iPhoneApp1` nazwy zgodnie z potrzebami dla określonego projektu.

    > jw. - xk bin/iPhone/Ad-Hoc/iPhoneApp1-1.0.ipa stare /

8.  Zachowaj `Terminal.app` otwarte okno.

9.  Usuń pliki, które z `.ipa`. Możesz przenieść je do Kosza za pomocą wyszukiwania, lub usuń je w wiersza polecenia przy użyciu `Terminal.app`. Aby wyświetlić zawartość `Payload/iPhone` pliku w programie Finder kombinacji Control + kliknięcie pliku, a następnie wybierz pozycję **Pokaż zawartość pakietu**.

10.  Przy użyciu tej samej metody ogólne, tak jak w kroku 3, Znajdź plik dziennika w ramach `~/Library/Logs/Xamarin/MonoTouchVS/` zawierający nazwę projektu i `generated session id` w nazwie: ![](modify-ipa-images/build-log.png "odszukaj w dzienniku kompilacji projektu w programie Finder")

11.  Otwieranie na przykład w dzienniku kompilacji w kroku 10, klikając go dwukrotnie.

12.  Znajdź wiersz, który zawiera `tool /usr/bin/codesign execution started with arguments: -v --force --sign`.

13.  Typ `/usr/bin/codesign ` do okna Terminal.app w kroku 8.

14.  Skopiuj wszystkie argumenty, począwszy od `-v` w wierszu w kroku 12, a następnie wklej je do okna Terminal.app.

15.  Zmień ostatni argument był `.app` pakietu znajdującymi się w `old/Payload/` folder, a następnie uruchom polecenie.

```bash
/usr/bin/codesign -v --force --sign SOME_LONG_STRING in/iPhone/Ad-Hoc/iPhoneApp1.app/ResourceRules.plist --entitlements obj/iPhone/Ad-Hoc/Entitlements.xcent old/Payload/iPhoneApp1.app
```

16.  Zmień na `old/` katalogu w terminalu:

```bash
cd old
```

17.  ZIP się zawartość katalogu w nowym `.ipa` plików przy użyciu `zip` polecenia. Możesz zmienić `"$HOME/Desktop/iPhoneApp1-1.0.ipa"` argument służący do wypełniania wyjściowego `.ipa` pliku wszędzie tam, gdzie chcesz:

```bash
zip -yr "$HOME/Desktop/iPhoneApp1-1.0.ipa" *
```

## <a name="common-error-messages"></a>Typowe komunikaty o błędach

Jeśli widzisz `Invalid Signature. A sealed resource is missing or invalid.`, zwykle oznacza to coś, co zostało zmienione w ramach `.app` pakietu, a `.app` pakiet nie został poprawnie ponownie podpisany później. Należy również zauważyć, że jeśli chcesz utworzyć `.ipa` z profilem dystrybucji możesz _musi_ kompilacji, oryginalnym `.ipa` z profilem dystrybucji. W przeciwnym razie `Entitlements.xcent` będzie niepoprawne.

Aby dać konkretny przykład jak ten błąd może wystąpić, jeśli uruchomisz następujące `codesign --verify` polecenia w oknie terminalu, po wykonaniu kroku 9, zostanie wyświetlony następujący błąd, wraz z dokładne przyczyny tego błędu:

```bash
$ codesign -dvvv --no-strict --verify old/Payload/iPhoneApp1.app
old/Payload/iPhoneApp1.app: a sealed resource is missing or invalid
file missing: /Users/macuser/Library/Caches/Xamarin/mtbs/builds/iPhoneApp1/cc530d20d6b19da63f6f1c6f67a0a254/old/Payload/iPhoneApp1.app/MyFile.png
```

A proces weryfikacji App Store, będą zgłaszać podobnego komunikatu o błędzie:

> Błąd ITMS-90035: "nieprawidłowy podpis. Zapieczętowane zasób to brakujący lub nieprawidłowy. Dane binarne w ścieżce [iPhoneApp1.app/iPhoneApp1] ma nieprawidłowy podpis. Upewnij się, że zostało zarejestrowane w aplikacji przy użyciu certyfikatu dystrybucji, a nie certyfikat ad-hoc lub certyfikatu deweloperskiego. Sprawdź, czy ustawienia podpisywania kodu w środowisku Xcode są poprawne, na poziomie docelowej, (które zastępują wszystkie wartości na poziomie projektu). Ponadto upewnij się, że przekazywany pakietu został zbudowany przy użyciu docelowej wersji w środowisku Xcode nie cel symulatora. Jeśli masz pewność, że podano prawidłowe ustawienia podpisywania kodu, wybierz pozycję "Wyczyść wszystko" w środowisku Xcode, usunąć katalog "kompilacja" w programie Finder i odbudować docelowej wersji. Aby uzyskać więcej informacji, zajrzyj [ https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html ](https://developer.apple.com/library/ios/documentation/Security/Conceptual/CodeSigningGuide/Introduction/Introduction.html)"
