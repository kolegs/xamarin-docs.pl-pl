---
title: Podsumowanie rozdział 6. Kliknięcie przycisku
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a45d1f1663b141912744e83763e7d618866159d7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-6-button-clicks"></a>Podsumowanie rozdział 6. Kliknięcie przycisku

[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Jest widok, który umożliwia użytkownikowi zainicjowania polecenia. A `Button` jest identyfikowany przez tekst (i opcjonalnie obrazu, jak pokazano w [13 rozdziału, mapy bitowe](chapter13.md)). W rezultacie `Button` definiuje wiele z tymi samymi właściwościami co `Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` definiuje również trzech właściwości które będą zarządzały sposobem wygląd obramowania, ale obsługa tych właściwości, oraz ich wzajemne niezależność jest przeznaczona dla konkretnej platformy:

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) typu `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) typu `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) typu `Double`

`Button` wszystkie właściwości dziedziczy także `VisualElement` i `View`, takie jak `BackgroundColor`, `HorizontalOptions`, i `VerticalOptions`.

## <a name="processing-the-click"></a>Następnie kliknij pozycję przetwarzania

`Button` Klasa definiuje [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) zdarzenie, które jest wywoływane po naciśnięciu `Button`. `Click` Program obsługi jest typu `EventHandler`. Pierwszy argument jest `Button` obiekt generowania zdarzenia; drugi argument jest `EventArgs` obiekt, który zapewnia żadnych dodatkowych informacji.

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) przykładzie pokazano prosty `Clicked` obsługi.

## <a name="sharing-button-clicks"></a>Przycisk udostępniania kliknie

Wiele `Button` widoki mogą udostępniać takie same `Clicked` obsługi, ale program obsługi zwykle musi ustalić, który `Button` jest odpowiedzialny za określonego zdarzenia. Jednym z podejść jest do zapisywania różnych `Button` obiektów, a pola wyboru, które z nich jest wyzwoleniem zdarzenia programu obsługi.

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) przykładzie pokazano tej metody. Program również pokazano, jak ustawić [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) właściwość `Button` do `false` po naciśnięciu klawisza `Button` nie jest już prawidłowy. A wyłączone `Button` nie generuje `Clicked` zdarzeń.

## <a name="anonymous-event-handlers"></a>Programy obsługi zdarzeń anonimowe

Istnieje możliwość definiowania `Clicked` obsługi jako funkcje anonimowe lambda jako [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) przykładzie pokazano. Jednak anonimowe obsługi nie może być współużytkowana bez kodu lepiej odbicia.

## <a name="distinguishing-views-with-ids"></a>Widoki odróżniający z identyfikatorami

Wiele `Button` obiektów odróżnia przez ustawienie [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) właściwości lub [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) właściwości `string`. Ta właściwość jest zdefiniowana przez `Element` , ale nie jest on używany w ramach platformy Xamarin.Forms. Ma ona służyć wyłącznie przez aplikacje.

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) przykładowe używa tego samego obsługi zdarzeń dla wszystkich kluczy numer 10 na klawiaturze numerycznej i rozróżnia je za pomocą `StyleId` właściwości:

[![Potrójna zrzut ekranu przedstawiający najprostszym klawiatury](images/ch06fg04-small.png "Kalkulator")](images/ch06fg04-large.png#lightbox "Kalkulator")

## <a name="saving-transient-data"></a>Zapisywanie danych przejściowej

Wiele aplikacji należy zapisać dane, gdy program zostanie zakończony i załaduj ponownie dane, gdy program uruchamiany ponownie. [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) Klasa definiuje kilka elementów członkowskich, które Pomoc programu zapisywania i przywracania przejściowej danych:

- [ `Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) Właściwości jest słownikiem z `string` kluczy i `object` elementów. Zawartość słownika są automatycznie zapisywane w lokalnym magazynie aplikacji przed Kończenie działania programu i ponownie załadowany podczas uruchamiania programu.
- `Application` Klasa definiuje trzy metody wirtualnej chronione czy program użytkownika standardowego `App` klasy zastąpienia: [ `OnStart` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/), [ `OnSleep` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/), i [ `OnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). Te dotyczą *cyklem życia aplikacji* zdarzenia.
- [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) Metody zapisuje zawartość słownika.

Nie jest konieczne do wywołania `SavePropertiesAsync`. Zawartość słownika są automatycznie zapisywane przed Kończenie działania programu i pobrać przed uruchamianie programu. Jest przydatne podczas testowania programu w celu zapisywania danych, jeśli program ulegnie awarii.

Przydatne jest również:

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/), statycznej właściwości, która zwraca bieżącą `Application` obiekt, który można następnie uzyskać `Properties` słownika.

Pierwszym krokiem jest określenie wszystkich zmiennych strony, który chcesz zachować, gdy program zakończy. Jeśli znasz wszędzie tam, gdzie zmienić tych zmiennych, po prostu dodaniem ich do `Properties` w tym momencie słownika. W Konstruktorze strony można ustawić zmienne z `Properties` słownika, jeśli ten klucz istnieje.

Większy program prawdopodobnie należy uwzględniać zdarzenia cyklu życia aplikacji. Najważniejsze jest `OnSleep` metody. Wywołanie tej metody oznacza, że program opuścił pierwszego planu. Prawdopodobnie użytkownik nacisnął **Home** znajdującego się na urządzeniu lub wyświetlane wszystkie aplikacje lub jest zamykany przez telefon. Wywołanie `OnSleep` jest tylko powiadomienia czy program odbiera zanim zostanie zakończony. Program należy skorzystać z okazji upewnij się, że `Properties` słownik jest aktualny.

Wywołanie `OnResume` wskazuje, że program nie zakończył się po ostatnim wywołaniem `OnSleep` , ale jest teraz uruchomiona na pierwszym planie ponownie. Program może używać tej możliwości można odświeżyć połączenia z Internetem (na przykład).

Wywołanie `OnStart` występuje podczas uruchamiania programu. Nie jest konieczne poczekaj na wywołanie tej metody dostępu `Properties` słownika ponieważ zawartość zostały już przywrócone po `App` Konstruktor jest wywoływany.

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) próbki jest bardzo podobny do **SimplestKeypad** z tą różnicą, że jest używana `OnSleep` należy przesłonić, aby zapisać bieżący wpis klawiatury i Konstruktor strony, aby przywrócić te dane.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 6 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Przykłady rozdział 6](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Przykłady rozdział 6 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
