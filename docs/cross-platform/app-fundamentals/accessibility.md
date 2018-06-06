---
title: Ułatwienia dostępu w aplikacji platformy Xamarin
description: Ten dokument zawiera wskazówki dotyczące różnych w celu utworzenia dostępnych aplikacji. Na przykład zawiera zalecenia dotyczące duży rozmiar czcionki, duży kontrast, samoopisujące interfejsów i.
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 97cd3655ac47a017d9590e1890b93d74f10a9c34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780307"
---
# <a name="accessibility-in-xamarin-apps"></a>Ułatwienia dostępu w aplikacji platformy Xamarin

_Upewnij się, że aplikacje są użytkowane przez najszerszych odbiorców_

Dostępność odwołuje się do koncepcji projektowanie interfejsów użytkownika aplikacji, które odpowiednie funkcji pomocy danych wejściowych i wyświetlania systemu operacyjnego, takie jak powiększać dużego typu, duży kontrast, odczytu ekranu (tekst na mowę), podpowiedzi wizualne lub dotykowych opinii i alternatywne metody wprowadzania tekstu.

Wersje Desktop i mobile platformach, np. z systemem iOS, Android i Windows zapewniają wbudowane interfejsów API, które ułatwiają deweloperom tworzenie aplikacji dostępny, takich jak [Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) i [firmy Apple, VoiceOver](http://www.apple.com/accessibility/ios/voiceover/).

## <a name="platform-specific-apis"></a>Interfejsy API specyficzne dla platformy

Aby zaimplementować ze wskazówkami w tym dokumencie, przy użyciu interfejsów API dostarczonych przez każdej platformy:

- [**Ułatwień dostępu systemu android**](~/android/app-fundamentals/accessibility.md)
- [**ułatwień dostępu systemu iOS**](~/ios/app-fundamentals/accessibility.md)
- [**OS X w ułatwień dostępu**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>Lista kontrolna ułatwień dostępu

Poniższe wskazówki upewnij się, że aplikacje są dostępne dla najszerszych możliwych odbiorców. Zapoznaj się z [kontrolną testowania ułatwień dostępu systemu Android](http://developer.android.com/training/accessibility/testing.html) i [stronę o ułatwieniach dostępu firmy Apple](http://www.apple.com/accessibility/) Aby uzyskać dodatkowe informacje.

### <a name="support-large-fonts-and-high-contrast"></a>Obsługuje duże czcionki i duży kontrast

Unikaj tworzenia wymiarów kontrolki hardcoding i zamiast tego należy użyć układy, które można zmieniać rozmiar w celu uwzględnienia większym rozmiarze czcionki.
Przetestuj schematy kolorów w trybie dużego kontrastu, aby upewnić się, że są one do odczytu.

### <a name="make-the-user-interface-self-describing"></a>Upewnij się, użytkowników interface samoopisujące

Oznacz wszystkie elementy z interfejsu użytkownika, opis i wskazówki, które są zgodne z ekranu odczytywania interfejsów API na każdej z platform.

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>Upewnij się, że obrazów i ikon mają opis tekst alternatywny

Obrazy i ikony, które są częścią interfejsu użytkownika aplikacji (na przykład przycisków lub wskaźniki stanu, na przykład) powinny być oznaczane dostępny opis.

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>Projekt drzewa wizualnego z dostępnych nawigacji na uwadze

Używanie formantów odpowiedni układ lub interfejsów API, aby nawigować między formantami za pomocą alternatywnej metody wprowadzania tekstu następuje sam przepływ logiczny jako przy użyciu ekran dotykowy.

Wyklucz niepotrzebnych elementów czytników ekranu (obrazy ozdobne lub etykiety dla pól, które już są dostępne, na przykład).

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>Nie należy polegać na podpowiedzi audio lub kolor samodzielnie

Uniknąć sytuacji, w przypadku wyłącznie wskazuje postęp, wykonania lub inny stan dźwięku lub zmiana koloru. Albo Projektowanie interfejsu użytkownika do dołączania wyczyść wizualnych (z dźwięku i kolor tylko wzmocnienie) lub Dodaj wskaźniki określonych ułatwień dostępu.

W przypadku wybrania kolorów, staraj się unikać palety, który jest trudna do odróżnienia dla użytkowników z ruchu kolorów.

### <a name="captioning-for-video-text-for-audio"></a>Podpisy dla tekstu wideo, audio

Podaj podpisów dla zawartości wideo i skrypt do odczytu do zawartości audio. Warto również Podaj formantów, które dopasowywać prędkość zawartości audio i wideo, a następnie upewnij się, że wolumin i przyciski Odtwórz i Wstrzymaj są łatwe do znalezienia i użycia.

### <a name="localize"></a>Lokalizowanie

Opisy ułatwień dostępu można i powinien być lokalizowany gdzie aplikacja obsługuje wiele języków.



## <a name="related-links"></a>Linki pokrewne

- [Ułatwień dostępu systemu android](~/android/app-fundamentals/accessibility.md)
- [ułatwień dostępu systemu iOS](~/ios/app-fundamentals/accessibility.md)
- [OS X w ułatwień dostępu](~/mac/app-fundamentals/accessibility.md)
- [Ułatwienia dostępu platformy Xamarin.Forms](~/xamarin-forms/app-fundamentals/accessibility/index.md)
