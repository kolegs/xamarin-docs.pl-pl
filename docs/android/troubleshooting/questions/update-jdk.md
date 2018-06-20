---
title: Jak zaktualizować wersję Java Development Kit (JDK)?
description: W tym artykule przedstawiono sposób zaktualizuj wersję Java Development Kit (JDK) w systemach Windows i Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/18/2018
ms.openlocfilehash: 979bf4572e0e0865c2254c3e1c2f707c8eecae8d
ms.sourcegitcommit: 57f9a9ba2f199697cb75e7be67f1a372c35a861b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36269663"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Jak zaktualizować wersję Java Development Kit (JDK)?

_W tym artykule przedstawiono sposób zaktualizuj wersję Java Development Kit (JDK) w systemach Windows i Mac._

## <a name="overview"></a>Omówienie

Xamarin.Android używa języka Java Development Kit (JDK) do integracji z zestawu SDK systemu Android do tworzenia aplikacji systemu Android i systemem Android projektanta. Najnowsze wersje zestawu SDK systemu Android (interfejs API 24 i nowsze) wymagają JDK 8 (1.8). Jeśli nie zostały jeszcze zaktualizowane JDK 8, wykonaj następujące kroki, aby zainstalować i włączyć go:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Pobieranie 8 JDK (1.8) z [witryny sieci Web programu Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Zrzut ekranu przedstawiający zestaw JDK pobierania strony w witrynie sieci Web programu Oracle](update-jdk-images/image1.png)

2.  Wybierz wersję 64-bitowych Umożliwia renderowanie form [Kontrolki niestandardowe](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) w Projektancie dla systemu Xamarin Android:

    ![Wybieranie pakietu JDK x64 systemu Windows można pobrać ze strony pobierania JDK](update-jdk-images/image2.png)

3.  Uruchom .exe i zainstaluj **narzędzi programistycznych**:

    ![Instalowanie narzędzi programistycznych w Instalatorze JDK](update-jdk-images/image3.png)

4.  Otwórz program Visual Studio i aktualizacji **Java Development Kit lokalizacji** wskaż nowy zestaw JDK w obszarze **Narzędzia > Opcje > Xamarin > ustawień systemu Android > Java Development Kit lokalizacji > Zmień**:

    [![Ścieżka ustawienie JDK na stronie ustawień systemu Android opcji IDE](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Pamiętaj ponownie program Visual Studio po zaktualizowaniu lokalizacji.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Pobieranie 8 JDK (1.8) z [witryny sieci Web programu Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Zrzut ekranu przedstawiający zestaw JDK pobierania strony w witrynie sieci Web programu Oracle](update-jdk-images/image1.png)

2.  Otwórz plik dmg i uruchom Instalator .pkg:

    ![Uruchomiony Instalator JDK na macOS](update-jdk-images/image5.png)

System Mac OS automatycznie ustawi nowej wersji JDK jako domyślny, aktualizując **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. Następnie można dokładnie który **Java SDK (JDK)** lokalizacji ma ustawioną wartość domyślną oczekiwanego **usr** w obszarze **programu Visual Studio for Mac > Preferencje > Projekty > lokalizacji zestawu SDK > Android > Java SDK (JDK) > lokalizacji**:

![Ustawianie lokalizacji JDK na stronie lokalizacji zestawu SDK systemu Android](update-jdk-images/image6.png)

-----

