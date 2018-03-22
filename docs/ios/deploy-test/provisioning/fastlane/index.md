---
title: Wprowadzenie do fastlane dla systemu iOS
description: "W tym przewodniku przedstawiono różne narzędzia fastlane, które mogą służyć do kodu logowania aplikacji systemu iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8202C57D-22FF-4224-A5B1-AAEF12B7C106
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4bba92180e77accaa42b70843fb5dbf12c94d632
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-fastlane-for-ios"></a>Wprowadzenie do fastlane dla systemu iOS

_W tym przewodniku przedstawiono różne narzędzia fastlane, które mogą służyć do kodu logowania aplikacji systemu iOS_

fastlane to projekt typu open source utworzona w celu uproszczenia procesu mylące i często nużące zwolnienia z systemami iOS i aplikacje dla systemu Android. Składa się z kilku narzędzi czy określonej proporcji wersji aplikacji, takich jak obsługują:

- [dostarczanie](https://github.com/fastlane/fastlane/tree/master/deliver#readme) — służy do zarządzania i zrzuty ekranu przekazywania, metadane i pakiety aplikacji iTunes Connect.
- [Tworzy](https://github.com/fastlane/fastlane/tree/master/produce#readme) — tworzy i aplikacji w iTunes Connect i portalu dla deweloperów (często nazywane AppID). Obejmuje również obsługę grupy aplikacji i usług aplikacji.
- [PEM](https://github.com/fastlane/fastlane/tree/master/pem#readme) — tworzy i zarządza nimi profile inicjowania obsługi powiadomień wypychanych.
- [siłowni](https://github.com/fastlane/fastlane/tree/master/gym#readme) — ten może służyć do tworzenia i podpisywanie aplikacji systemu iOS. (Aplikacji platformy Xamarin już umożliwia MSBuild kompilacji, logowania i aplikacje archiwum)
- [CERT](https://github.com/fastlane/fastlane/tree/master/cert#readme) — tworzy i którymi zarządza certyfikaty podpisywania kodu 
- [sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme) — tworzy i zarządza nimi profile inicjowania obsługi administracyjnej
- [odpowiada](https://github.com/fastlane/fastlane/tree/master/match#readme) — tworzy i obsługuje certyfikatów oraz profile i przechowuje je w repozytorium git, dzięki czemu mogą być synchronizowane przez zespół deweloperów.

fastlane mogą być używane w różnych sposobów: za pomocą polecenia terminalu, za pośrednictwem środków plikowym lub za pomocą zmiennych środowiskowych dla kompilacji ciągłej integracji. 

Ten przewodnik dotyczy w szczególności Konfigurowanie środowiska deweloperskiego przy użyciu aplikacji systemu iOS urządzenia i koncentruje się na **cert**, **sigh** i **odpowiada** narzędzia. 

Zawartość można jako punktu wyjścia do pomocy z dystrybucji aplikacji, w tym pełni automatyzacji procesu na serwerze ciągłej integracji. Jednak ważne jest, aby należy pamiętać, że fastlane 3 strony, który narzędzia do obsługi projektów Xcode i w związku z tym niektóre narzędzia lub polecenia, takich jak `fastlane init` może nie działać zgodnie z oczekiwaniami z plikami csproj. Aby uzyskać więcej informacji na temat używania fastlane dodatkowych narzędzi lub zwalniania dla systemu Android przy użyciu fastlane, dotyczą [https://fastlane.tools/](https://fastlane.tools/)

<a name="Installation" />

## <a name="installation"></a>Instalacja

1. Upewnij się, że Xcode narzędzi wiersza polecenia są zainstalowane na komputerze macOS. Aby zainstalować narzędzia, użyj polecenia `xcode-select --install` w terminalu. Jeśli zostały one jeszcze zainstalowane, zostanie wyświetlony następujący błąd:

    ```bash
    error: command line tools are already installed, use "Software Update" to install updates
    ```

2. Pobierz narzędzia fastlane z [ https://download.fastlane.tools ](https://download.fastlane.tools). 

    > [!NOTE]
    > Istnieje możliwość zainstalować narzędzia fastlane z przy użyciu oprogramowania Homebrew `brew cask install fastlane` lub za pośrednictwem Rubygems (2.0 lub nowszej) przy użyciu `sudo gem install fastlane –NV`. Jednak za pomocą Instalatora zostanie upewnij się, że dostępne są prawidłowymi zależnościami. 

3. Zainstaluj fastlane przy rozpakować pliku, a następnie kliknij dwukrotnie `install` pliku wykonywalnego. Jeśli wystąpi błąd udzielanie porad, który "nie można otworzyć pliku, ponieważ jest z programisty niezidentyfikowane", naciśnij przycisk OK i wykonaj następujące czynności:
    - Control + kliknięcie `install` pliku wykonywalnego. Spowoduje to wyświetlenie okna dialogowego poniżej:

      ![](images/fastlane-image12.png "Okno dialogowe instalacji")
    
    - Naciśnij przycisk OK, aby rozpocząć instalowanie narzędzia fastlane

4. Terminal wyświetli monit z okna dialogowego zostały pokazane poniżej. Naciśnij klawisz `y`:

  ![](images/fastlane-image13.png "Wiersz terminala")
 
4. Uruchom `which fastlane` przed użyciem fastlane po raz pierwszy. Ścieżka powinna wyglądać następująco: 

    ```bash
    /Users/[user]/.fastlane/bin
    ```

5. Jeśli ścieżka zgodna powyższego, możesz rozpocząć pracę.

     Jeśli nie, wykonaj następujące czynności: na macOS Otwórz `.bash_profile`, która jest zwykłego tekstu ukrytego pliku w katalogu macierzystym, przy użyciu następującego polecenia:

    ```bash
    open ~/.bash_profile
    ```

6. Dodaj następujące zmiennej środowiskowej PATH, a następnie zapisz go: 

    ```bash
    export PATH="$HOME/.fastlane/bin:$PATH"
    ```

7.  Uruchom `which fastlane` ponownie, aby potwierdzić ścieżka wygląda `/Users/[user]/.fastlane/bin`


## <a name="updating-fastlane"></a>Aktualizowanie fastlane

fastlane jest projektem bardzo aktywny typu open source, który regularnie wypycha nowych wersji. Po udostępnieniu nowej wersji fastlane książek po uruchomieniu polecenia fastlane:

[![](images/fastlane-image0.png "Wiersz aktualizacji fast lane")](images/fastlane-image0.png#lightbox)


Aby uaktualnić do nowej wersji fastlane, Pobierz najnowszy pakiet z [tutaj](https://download.fastlane.tools) i kliknij dwukrotnie pakiet instalacyjny do jego działania:

[![](images/fastlane-image0a.png "Uruchomiony pakiet instalacyjny")](images/fastlane-image0a.png#lightbox)


## <a name="contents"></a>Spis treści

Ta seria przewodników przedstawia niektóre narzędzia czy zastosowania fastlane podpisywanie kodu aplikacji systemu iOS w ramach przygotowania do programowanie lub dystrybucję. Narzędzia objętych obecnie są:

- [cert](~/ios/deploy-test/provisioning/fastlane/cert.md)
- [sigh](~/ios/deploy-test/provisioning/fastlane/sigh.md)
- [match](~/ios/deploy-test/provisioning/fastlane/match.md)

certyfikat i sigh zajmuje się tworzenie i zarządzanie certyfikatami podpisywania i profilów aprowizacji na komputerze lokalnym. dopasowanie przyjmuje ten proces krok dalej. Tworzy i zarządza certyfikatów oraz profile inicjowania obsługi i przechowuje je w repozytorium git, dzięki czemu mogą być dostępne dla wszystkich członków zespołu deweloperów. Zapoznaj się z artykułem każdej sekcji, aby dowiedzieć się, jak działają i jak mogą z nich korzystać.

## <a name="using-fastlane-tools-with-xamarin"></a>Za pomocą narzędzi fastlane za pomocą platformy Xamarin

Po utworzeniu tożsamości podpisywania i inicjowania obsługi administracyjnej profile z użyciem fastlane ustawienie opcji w programie Visual Studio for Mac podpisywania pakietu powinny być proste, zapewniając, że certyfikaty i klucze prywatne są w macOS łańcucha kluczy, który Profile inicjowania obsługi administracyjnej znajdują się w folderze `~/Library/MobileDevice/Provisioning Profiles`.

Aby ustawić opcje dla aplikacji platformy Xamarin.iOS podpisywania kodu, kliknij prawym przyciskiem myszy nazwę projektu, zaznacz **opcje projektu > kompilacji > iOS podpisywania pakietu** i ustaw tożsamość podpisywania i profilu inicjowania obsługi administracyjnej jawnie, jako przedstawiona poniżej:

[![](images/fastlane-image11.png "Jawnie ustaw tożsamość podpisywania i profilu inicjowania obsługi administracyjnej")](images/fastlane-image11.png#lightbox)

## <a name="related-links"></a>Linki pokrewne

- [fastlane dokumentów](https://fastlane.tools/)
- [fastlane Docs podpisywania kodu](https://docs.fastlane.tools/codesigning/getting-started/)
- [Przewodnik podpisywanie kodu](https://codesigning.guide/)
