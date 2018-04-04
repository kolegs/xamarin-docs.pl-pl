---
title: Ułatwienia dostępu
description: Kompilowanie aplikacji dostępny gwarantuje, że aplikacja jest używana przez osoby, które osiągają interfejsu użytkownika z zakresem potrzeb i.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: e4fb151b9664df7236d2c22ed54db09bf7bc65b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility"></a>Ułatwienia dostępu

_Kompilowanie aplikacji dostępny gwarantuje, że aplikacja jest używana przez osoby, które osiągają interfejsu użytkownika z zakresem potrzeb i._

Udostępnianie aplikacji platformy Xamarin.Forms oznacza planowania układu i projektowania wiele elementów interfejsu użytkownika. Aby uzyskać wskazówki dotyczące problemów, które należy uwzględnić, zobacz [Lista kontrolna ułatwień dostępu](~/cross-platform/app-fundamentals/accessibility.md). Wiele problemów ułatwień dostępu, takie jak duży rozmiar czcionki i odpowiednie ustawienia koloru i kontrastu może już zostać zlikwidowane poprzez interfejsów API platformy Xamarin.Forms.

[Ułatwień dostępu systemu Android](~/android/app-fundamentals/accessibility.md) i [ułatwień dostępu systemu iOS](~/ios/app-fundamentals/accessibility.md) przewodniki zawierają szczegóły macierzystych interfejsów API udostępnianych przez program Xamarin i [przewodniku ułatwień dostępu platformy uniwersalnej systemu Windows w witrynie MSDN](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) wyjaśniono natywnego podejście na tej platformie. Te interfejsy API są używane do pełnego wdrożenia aplikacje dostępne na każdej z platform.

Nie ma obecnie platformy Xamarin.Forms *wbudowanych* obsługuje wszystkie dostępności interfejsami API dostępnymi w każdym podstawowej platformy. Obsługuje on jednak ustawienie wartości dostępności na elementy interfejsu użytkownika do obsługi ekranu czytnika i nawigacja narzędzia pomocy, które jest jednym z najważniejszych części budowę dostępnych aplikacji. Aby uzyskać więcej informacji, zobacz [wartości ustawienia ułatwień dostępu w przypadku elementów interfejsu użytkownika](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md).

Inne interfejsy API dostępności (takich jak [PostNotification w systemie iOS](~/ios/app-fundamentals/accessibility.md)) mogą być lepiej dostosowane do [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) lub [niestandardowego modułu renderowania](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) implementacji. Te nie są uwzględnione w tym przewodniku.

## <a name="testing-accessibility"></a>Testowanie ułatwień dostępu

Aplikacje platformy Xamarin.Forms zwykle wielu platform docelowych, co oznacza, że testowanie funkcje ułatwień dostępu, zależnie od platformy. Skorzystaj z poniższych linków, aby dowiedzieć się, jak przetestować dostępność na każdej platformie:

- [**iOS testowanie**](~/ios/app-fundamentals/accessibility.md)
- [**Testowanie systemu android**](~/android/app-fundamentals/accessibility.md)
- [**AccScope systemu Windows (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)


## <a name="related-links"></a>Linki pokrewne

- [Dostępność i platform](~/cross-platform/app-fundamentals/accessibility.md)
- [Ustawianie wartości ułatwień dostępu w elementach interfejsu użytkownika](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)
