---
title: Wprowadzenie do usługi ContentProviders
description: System operacyjny Android korzysta z dostawców zawartości do ułatwienia dostępu do udostępnionych danych, takich jak pliki multimedialne, kontakty i Kalendarz informacje. W tym artykule przedstawiono klasy ContentProvider i przedstawiono dwa przykłady jak z niego korzystać.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: e534c02820bfeab3a5bc1211bf0cbb20b9821af3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764167"
---
# <a name="intro-to-contentproviders"></a>Wprowadzenie do usługi ContentProviders

_System operacyjny Android korzysta z dostawców zawartości do ułatwienia dostępu do udostępnionych danych, takich jak pliki multimedialne, kontakty i Kalendarz informacje. W tym artykule przedstawiono klasy ContentProvider i przedstawiono dwa przykłady jak z niego korzystać._


## <a name="content-providers-overview"></a>Przegląd dostawców zawartości

A *ContentProvider* hermetyzuje repozytorium danych i udostępnia interfejs API do niego dostęp. Dostawca istnieje jako część aplikacji systemu Android, która zazwyczaj udostępnia interfejs użytkownika do wyświetlania/zarządzania danymi. Najważniejszą korzyścią z używania dostawcy zawartości jest włączenie inne aplikacje, aby łatwo uzyskiwać dostęp do danych hermetyzowany za pomocą obiektu klienta dostawcy (nazywane *klasy ContentResolver*). Razem dostawcy zawartości i rozpoznawania zawartości oferują spójne wewnątrz aplikacji interfejsu API dla dostępu do danych, który jest proste tworzenie i używanie. Każdej aplikacji można wybrać opcję użycia `ContentProviders` wewnętrznie zarządzanie danymi, a także je ujawnić do innych aplikacji.

A `ContentProvider` jest również wymagany dla twojej aplikacji w celu umożliwienia sugestie dotyczące wyszukiwania niestandardowego, lub jeśli chcesz zapewnić możliwość kopiowania danych złożonych z poziomu aplikacji, aby wkleić do innych aplikacji. Ten dokument przedstawia sposób dostępu i kompilacji `ContentProviders` z platformy Xamarin.Android.

Struktury w tej sekcji jest następujący:

- **Jak działa** &ndash; omówienie co `ContentProvider` jest przeznaczony dla oraz sposób jej działania.

- **Korzystanie z dostawcy zawartości** &ndash; przykład dostęp do listy kontaktów.

- **Udostępnianie danych przy użyciu ContentProvider** &ndash; pisania i spójniejsze `ContentProvider` w tej samej aplikacji.

`ContentProviders` i kursorów, które pracują na swoje dane są często używane do wypełnienia widokach listy. Zapoznaj się [przewodnik widokach listy i karty](~/android/user-interface/layouts/list-view/index.md) Aby uzyskać więcej informacji na temat korzystania z tych klas.

`ContentProviders` udostępniany przez Android (lub inne aplikacje) są łatwo obejmować dane z innych źródeł w aplikacji. Umożliwiają dostęp i obecny dane, takie jak listy kontaktów, zdjęć lub zdarzenia kalendarza od w aplikacji i umożliwia użytkownikowi interakcji z tymi danymi.

Niestandardowe `ContentProviders` to wygodny sposób, aby pakiet danych do użycia w aplikacji lub do użytku przez inne aplikacje (w tym specjalnych zastosowań, takich jak niestandardowe wyszukiwanie i skopiuj i Wklej).

Tematy w tej sekcji zawierają przykłady proste korzystanie z oraz zapisywanie `ContentProvider` kodu.



## <a name="related-links"></a>Linki pokrewne

- [Wersja demonstracyjna ContactsAdapter (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [Przewodnik dla deweloperów dostawców zawartości](http://developer.android.com/guide/topics/providers/content-providers.html)
- [Odwołania do klasy ContentProvider](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [Odwołania do klasy ContentResolver klasy](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [Odwołania do klasy ListView](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [Odwołania do klasy CursorAdapter](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [Odwołania do klasy UriMatcher](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [Odwołania do klasy ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
