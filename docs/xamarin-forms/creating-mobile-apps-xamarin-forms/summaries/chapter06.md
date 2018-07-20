---
title: Podsumowanie rozdział 6. Kliknięcie przycisku
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 6. Kliknięcie przycisku'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: 464fbdb043ac35eba7a4cc2d9ec76b78cc91ac5b
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156515"
---
# <a name="summary-of-chapter-6-button-clicks"></a>Podsumowanie rozdział 6. Kliknięcie przycisku

[ `Button` ](xref:Xamarin.Forms.Button) Jest widok, który umożliwia użytkownikowi zainicjowania polecenia. A `Button` jest identyfikowane za pomocą tekstu (i opcjonalnie obrazów, jak pokazano w [rozdział 13, mapy bitowe](chapter13.md)). W związku z tym `Button` definiuje wiele z tymi samymi właściwościami co `Label`:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` definiuje również trzy właściwości regulują wygląd obramowania, ale obsługę tych właściwości i ich wzajemne niezależność jest przeznaczona dla konkretnej platformy:

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) tego typu `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) tego typu `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) tego typu `Double`

`Button` dziedziczy także wszystkie właściwości `VisualElement` i `View`, w tym `BackgroundColor`, `HorizontalOptions`, i `VerticalOptions`.

## <a name="processing-the-click"></a>Przetwarzanie kliknięcie

`Button` Klasa definiuje [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) zdarzenia, które jest wywoływane, gdy użytkownik wybiera `Button`. `Click` Program obsługi jest typu `EventHandler`. Pierwszy argument jest `Button` obiektów, wygenerowanie zdarzenia; drugi argument funkcji jest `EventArgs` obiekt, który nie dostarcza żadnych dodatkowych informacji.

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) przykład pokazuje prosty `Clicked` obsługi.

## <a name="sharing-button-clicks"></a>Udostępnianie przycisku kliknięcia

Wiele `Button` widoki mogą współużytkować ten sam `Clicked` program obsługi, ale program obsługi ogólnie wymaga to określenie `Button` jest odpowiedzialny za określonego zdarzenia. Jednym z podejść jest do zapisywania różnych `Button` obiektów jako pola i sprawdź, co jest wyzwalanie zdarzenia w obsłudze.

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) w przykładzie pokazano tej techniki. Program pokazuje również, jak ustawić [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) właściwość `Button` do `false` po naciśnięciu klawisza `Button` nie jest już prawidłowy. Jest wyłączona `Button` nie generuje `Clicked` zdarzeń.

## <a name="anonymous-event-handlers"></a>Programy obsługi zdarzeń anonimowe

Istnieje możliwość definiowania `Clicked` obsługami jako funkcje anonimowe lambda jako [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) w przykładzie pokazano. Jednak anonimowe obsługi nie może być współużytkowana bez kodu nieuporządkowane odbicia.

## <a name="distinguishing-views-with-ids"></a>Wyróżniające się widoków z identyfikatorami

Wiele `Button` obiektów odróżnia ustawiając [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) właściwości lub [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) właściwość `string`. Ta właściwość jest zdefiniowana przez `Element` , ale nie jest używany w ramach zestawu narzędzi Xamarin.Forms. Ma być używane wyłącznie przez aplikacje.

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) próbka używa tego samego programu obsługi zdarzeń dla 10 wszystkie klawisze numeryczne na klawiaturę numeryczną i rozróżnia między nimi przy użyciu `StyleId` właściwości:

[![Potrójna zrzut ekranu przedstawiający najprostszym klawiaturze](images/ch06fg04-small.png "Kalkulator")](images/ch06fg04-large.png#lightbox "Kalkulator")

## <a name="saving-transient-data"></a>Zapisywanie danych przejściowych

Wiele aplikacji wymaga, aby zapisać dane, gdy program jest zamykany i ponownie załadować te dane, gdy program uruchamia ponownie. [ `Application` ](xref:Xamarin.Forms.Application) Klasa definiuje kilka elementów członkowskich, które pomagają program zapisywania i przywracania danych przejściowych:

- [ `Properties` ](xref:Xamarin.Forms.Application.Properties) Właściwość jest słownika przy użyciu `string` kluczy i `object` elementów. Zawartość słownika są automatycznie zapisywane w magazynie lokalnym aplikacji przed Kończenie działania programu i ponownie załadować podczas uruchamiania programu.
- `Application` Klasa definiuje trzy chronionych metod wirtualnych, że program użytkownika standardowego `App` klasy zastąpienia: [ `OnStart` ](xref:Xamarin.Forms.Application.OnStart), [ `OnSleep` ](xref:Xamarin.Forms.Application.OnSleep), i [ `OnResume` ](xref:Xamarin.Forms.Application.OnResume). Te dotyczą *zarządzania cyklem życia aplikacji* zdarzenia.
- [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) Metoda zapisuje zawartość słownika.

Nie jest konieczne do wywołania `SavePropertiesAsync`. Zawartość słownika są automatycznie zapisywane przed Kończenie działania programu i pobierane przed uruchomieniem programu. Jest to przydatne podczas testowania program w celu zapisywania danych, jeśli program ulegnie awarii.

Przydatne jest również:

- [`Application.Current`](xref:Xamarin.Forms.Application.Current), właściwość statyczna, która zwraca bieżącą `Application` obiekt, który można następnie uzyskać `Properties` słownika.

Pierwszym krokiem jest określenie wszystkich zmiennych na stronie, które chcą zachować używane, gdy program kończy. Jeśli znasz wszystkie miejsca, w którym zmiany tych zmiennych, możesz po prostu dodać je do `Properties` słownika, w tym momencie. W Konstruktorze strony można ustawić zmienne z `Properties` słownika, jeśli klucz istnieje.

Większe prawdopodobnie należy do czynienia z zdarzenia cyklu życia aplikacji. Najważniejsze jest `OnSleep` metody. Wywołanie tej metody oznacza, że program opuścił pierwszego planu. Być może użytkownik nacisnął **Home** znajdujący się na urządzeniu wyświetlane wszystkie aplikacje lub jest zamykana, numer telefonu. Wywołanie `OnSleep` jest tylko powiadomienia, program odbiera zanim zostanie zakończony. Program powinien warto wykorzystać tę okazję, aby upewnić się, że `Properties` słownika jest aktualny.

Wywołanie `OnResume` wskazuje, czy program kończy następującej po ostatnim wywołaniu `OnSleep` , ale jest teraz uruchomiona na pierwszym planie ponownie. Program może być skorzystanie z tej okazji, aby odświeżyć połączenia przez internet (na przykład).

Wywołanie `OnStart` występuje podczas uruchamiania programu. Nie jest konieczne poczekaj, aż wywołanie tej metody dostępu `Properties` słownika ponieważ zawartość zostały już przywrócone po `App` wywoływany jest Konstruktor.

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) próbki jest bardzo podobny do **SimplestKeypad** z tą różnicą, że program używa `OnSleep` należy przesłonić, aby zapisać bieżący wpis klawiatury i Konstruktor strony do przywrócenia danych.

> [!NOTE]
> Innym sposobem zapisywanie ustawień programu są dostarczane przez Xamarin.Essentials [preferencje](~/essentials/preferences.md) klasy.

## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 6 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Przykłady rozdział 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Przykłady rozdział 6 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Przycisk zestawu narzędzi Xamarin.Forms](~/xamarin-forms/user-interface/button.md)
