---
title: Konfiguracja aplikacji Mac
description: "Ten przewodnik przeprowadzi Cię przez konfigurację aplikacji Xamarin.Mac dla publikacji."
ms.topic: article
ms.prod: xamarin
ms.assetid: fea66a34-1581-4cd6-b714-3fbff215a542
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: 0bc64d0b03aa4f80b19ea098904dc1e2155313f6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="mac-app-configuration"></a>Konfiguracja aplikacji Mac

_Ten przewodnik przeprowadzi Cię przez konfigurację aplikacji Xamarin.Mac dla publikacji._


## <a name="mac-app-configuration"></a>Konfiguracja aplikacji Mac

Kliknij prawym przyciskiem myszy projekt aplikacji Mac w programie Visual Studio dla komputerów Mac, a następnie wybierz pozycję **opcje**.


### <a name="application-settings"></a>Ustawienia aplikacji

Aby zmienić ustawienia aplikacji w aplikacji Xamarin.Mac, kliknij dwukrotnie **Info.plist** w pliku **konsoli rozwiązania**:

![Wybieranie pliku Info.plist](app-configuration-images/config04.png "wybierając plik Info.plist")

Spowoduje to wyświetlenie opcji dostępnych dla aplikacji:

 [![Edytowanie pliku Info.plist](app-configuration-images/config01.png "edytowania pliku Info.plist")](app-configuration-images/config01-large.png#lightbox)

Uruchamianie aplikacji Mac utworzone za pomocą Xamarin.Mac mają następujące wymagania systemowe:

- Komputer Mac z systemem systemu Mac OS X 10,7 lub większą.


### <a name="signing-settings"></a>Podpisywanie ustawienia

**Podpisywania Mac** sekcji **opcje projektu** okno dialogowe umożliwia deweloperowi podpisać aplikację Xamarin.Mac do testowania dla własnych wersji lub wersji za pośrednictwem sklepu z aplikacjami firmy Apple:

[![Edytor podpisywania Mac](app-configuration-images/config02.png "okien podpisywania Mac")](app-configuration-images/config02-large.png#lightbox)

Z tym wybierz opcję tożsamości, profilu inicjowania obsługi i wszystkie niestandardowe uprawnienia używany do podpisywania aplikacji, gdy jest on skompilowany. Deweloper może opcjonalnie Podpisz Instalatora, służącego do instalowania aplikacji na komputerach Mac.


### <a name="build-settings"></a>Ustawienia kompilacji

**Kompilacji Mac** sekcji **opcje projektu** okno dialogowe umożliwia deweloperowi wybierz Architektura aplikacji Xamarin.Mac, aby kontrolować, jakiej wersji macOS aplikacja będzie obsługiwać i opcjonalnie utworzyć pakiet instalacji, gdy aplikacja jest pomyślnie skompilowano:

 [![Edytowanie ustawień kompilacji](app-configuration-images/config03.png "edycji ustawień kompilacji")](app-configuration-images/config03-large.png#lightbox)


## <a name="related-links"></a>Linki pokrewne

- [Instalacja](/visualstudio/mac/installation/)
- [Witaj, przykładowe Mac](~/mac/get-started/hello-mac.md)
- [Dystrybucję aplikacji ze sklepu Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Identyfikator deweloperów i strażnika](https://developer.apple.com/resources/developer-id/)
