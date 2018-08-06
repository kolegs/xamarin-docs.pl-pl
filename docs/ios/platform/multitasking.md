---
title: Wielozadaniowości dla tabletu iPad w Xamarin.iOS
description: System iOS 9 obsługuje dwie aplikacje działające na tym samym czasie, przy użyciu slajdów przez lub podzielony widok. Obsługuje ona również wideo odtwarzanie obraz w obrazie.
ms.prod: xamarin
ms.assetid: 0F2266D7-21FF-404D-A148-0CFDE76B12AA
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 7eacd9ece067d2ddf6363c0551055daa3df4433a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787958"
---
# <a name="multitasking-for-ipad-in-xamarinios"></a>Wielozadaniowości dla tabletu iPad w Xamarin.iOS

_System iOS 9 obsługuje dwie aplikacje działające na tym samym czasie, przy użyciu slajdów przez lub podzielony widok. Obsługuje ona również wideo odtwarzanie obraz w obrazie._

![](multitasking-images/about02-sml.png "Podziel przykład ekranu") ![](multitasking-images/about03-sml.png "przykład obraz w obrazie")

System iOS 9 dodaje obsługę wielozadaniowości sprzęt określonego urządzenia iPad z systemem dwie aplikacje w tym samym czasie. Wielozadaniowości dla tabletu iPad jest obsługiwany przez następujące funkcje:

- [**Slajd za pośrednictwem** ](#Slide-Over) — zezwala użytkownikowi na tymczasowo uruchomić drugi aplikacji dla systemu iOS na slajdzie limit panelu (albo po prawej lub lewej stronie ekranu, w zależności od kierunku języka), który obejmuje około 25% głównej aplikacji aktualnie uruchomione. Slajd za pośrednictwem jest dostępna tylko na iPad Pro, iPad lotniczego, iPad 2 lotniczego, iPad Mini 2, iPad Mini 3 lub 4 dla urządzenia iPad.
- [**Widok podzielony** ](#Split-View) -na urządzeniu iPad obsługiwane (iPad lotniczego 2, 4 dla urządzeń iPad i Pro tylko iPad), użytkownik może Wybierz drugi aplikacji i uruchom go side-by-side z aktualnie uruchomionej aplikacji w trybie ekranu podziału. Użytkownik może określić procent ekranu głównego, który zajmuje każdej aplikacji.
- [**Obraz na rysunku** ](#Picture-in-Picture) — dla aplikacji, które odtwarzania zawartości wideo, filmów może teraz zostać odtworzone w oknie ruchome i o zmiennych rozmiarach, które pojawia się na inne aplikacje uruchomione na urządzeniu z systemem iOS. Użytkownik ma pełną kontrolę nad rozmiar i położenie tego okna. Obraz na rysunku jest dostępna tylko na iPad Pro, iPad lotniczego, iPad 2 lotniczego, iPad Mini 2, iPad Mini 3 lub 4 dla urządzenia iPad.

Kilka rzeczy, które należy wziąć pod uwagę podczas [Obsługa wielozadaniowości w aplikacji](#Supporting-Multitasking-in-your-App), takie jak:

- [Rozmiaru i orientacji ekranu](#Screen-Size-Considerations)
- [Skróty klawiaturowe niestandardowego sprzętu](#Custom-Hardware-Keyboard-Shortcuts)
- [Zarządzanie zasobami](#Resource-Management-Considerations)

Deweloper aplikacji można też [zrezygnować z wielozadaniowości](#Opting-Out-of-Multitasking), takie jak [wyłączenie odtwarzanie wideo PIP](#Disabling-PIP-Video-Playback).

W tym artykule opisano kroki wymagane do zapewnienia, że aplikacja Xamarin.iOS będzie działać poprawnie w środowisku wielozadaniowości jak rezygnacji z wielozadaniowości, jeśli nie jest dobrze nadające się do aplikacji.

> [!VIDEO https://youtube.com/embed/GctYAozoLr8]

**Wielozadaniowości na urządzeniu iPad przez [Xamarin University](https://university.xamarin.com)**


<a name="Multitasking-QuickStart" />

## <a name="multitasking-quickstart"></a>Wielozadaniowości Szybki Start

Do obsługi **slajd za pośrednictwem** lub **podzielony widok** aplikacji należy wykonać następujące czynności:

 - Zostać utworzony dla systemu iOS 9 (lub nowsza).
 - Przy użyciu scenorysu dla jego uruchomienie ekranu (a nie obrazu zasobów).
 - Za pomocą scenorysu Autolayout i rozmiar klasy dla jego interfejsie użytkownika.
 - Obsługuje wszystkie 4 iOS urządzenia orientacje (pionowa, pionowa dołu, pozioma po lewej i prawej pozioma).

<a name="Multitasking" />

## <a name="about-multitasking-for-ipad"></a>O wielozadaniowości dla urządzeń iPad

System iOS 9 oferuje nowe możliwości wielozadaniowości na iPad wraz z wprowadzeniem _slajd za pośrednictwem_, _podzielony widok_ (iPad lotniczego 2, 4 dla urządzeń iPad i iPad Pro tylko) i _obraz na rysunku_. Bliższe spojrzenie na te funkcje zostaną wykonane w poniższych sekcjach.

<a name="Slide-Over" />

### <a name="slide-over"></a>Slajd za pośrednictwem

Funkcja slajd za pośrednictwem zezwala użytkownikowi na pobranie drugi aplikacji i wyświetl ją w małych panelu przesuwanego, aby zapewnić szybkie interakcji. Slajd za pośrednictwem panelu jest tymczasowy i zostanie zamknięty, gdy użytkownik przechodzi wstecz do pracy z głównej aplikacji ponownie.

[![](multitasking-images/about01.png "Slajd za pośrednictwem Panelu")](multitasking-images/about01.png#lightbox)

Główny jest, aby pamiętać jest, że użytkownik zdecyduje się dwie aplikacje, które będą uruchomione side-by-side i że deweloper nie ma kontroli nad tego procesu. W związku z tym istnieje kilka rzeczy, które należy wykonać, aby upewnić się, że aplikacji platformy Xamarin.iOS działa poprawnie na slajd za pośrednictwem panelu:

- **Użyj klasy rozmiar i Autolayout** — ponieważ aplikacji platformy Xamarin.iOS może zostać uruchomiony w panelu po stronie poza slajdów, możesz już polegać urządzenia, jego rozmiaru ekranu lub jego orientację na układ interfejsu użytkownika. W celu zapewnienia aplikacji poprawnie skaluje jego interfejsu, należy użyć klasy rozmiar i Autolayout. Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentacji.
- **Zasoby efektywnie korzystać z** — ponieważ aplikacji można teraz udostępnianie system z innego uruchomionej aplikacji, jest bardzo istotne, czy Twoja aplikacja korzysta z zasobów systemowych wydajnie. Gdy pamięć staje się rozrzedzonych, system automatycznie spowoduje przerwanie aplikację, która zajmuje najwięcej pamięci. Zobacz firmy Apple [energii przewodnik wydajności dla aplikacji systemu iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) więcej szczegółów.

Slajd za pośrednictwem jest dostępna tylko na iPad Pro, iPad lotniczego, iPad 2 lotniczego, iPad Mini 2, iPad Mini 3 lub 4 dla urządzenia iPad. Aby dowiedzieć się więcej na temat przygotowania aplikacji do slajd za pośrednictwem, zobacz firmy Apple [przyjmowanie ulepszenia wielozadaniowości na iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) dokumentacji.

<a name="Split-View" />

### <a name="split-view"></a>Widok podzielony

Na urządzeniu iPad obsługiwane (iPad lotniczego 2, 4 dla urządzeń iPad i Pro tylko iPad) użytkownik może Wybierz drugi aplikacji i uruchom go side-by-side z aktualnie uruchomionej aplikacji w trybie ekranu podziału. Użytkownik może określić procent ekranie głównym zajmowany przez przeciągnięcie każdej aplikacji na ekranie podziału.

[![](multitasking-images/about02.png "Widok podzielony")](multitasking-images/about02.png#lightbox)

Podobnie jak slajd przez użytkownik zdecyduje się dwie aplikacje, które będą uruchomione side-by-side i ponownie projektanta nie ma kontroli nad tego procesu. W związku z tym podzielony widok umieszcza podobne wymagania w aplikacji platformy Xamarin.iOS:

- **Użyj klasy rozmiar i Autolayout** — ponieważ aplikacji platformy Xamarin.iOS może zostać uruchomiony w trybie ekranu podzielonym na użytkownika określony rozmiar, możesz już polegać urządzenia, jego rozmiaru ekranu lub jego orientację na układ interfejsu użytkownika. W celu zapewnienia aplikacji poprawnie skaluje jego interfejsu, należy użyć klasy rozmiar i Autolayout. Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentacji.
- **Zasoby efektywnie korzystać z** — ponieważ aplikacji można teraz udostępnianie system z innego uruchomionej aplikacji, jest bardzo istotne, czy Twoja aplikacja korzysta z zasobów systemowych wydajnie. Gdy pamięć staje się rozrzedzonych, system automatycznie spowoduje przerwanie aplikację, która zajmuje najwięcej pamięci. Zobacz firmy Apple [energii przewodnik wydajności dla aplikacji systemu iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) więcej szczegółów.

Aby dowiedzieć się więcej o przygotowywanie aplikacji dla widoku podziału, zobacz firmy Apple [przyjmowanie ulepszenia wielozadaniowości na iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145) dokumentacji.

<a name="Picture-in-Picture" />

### <a name="picture-in-picture"></a>Obraz w obrazie

Nowy obraz w funkcji obrazu (znanej także jako _PIP_) umożliwia użytkownikowi obejrzeć film wideo w oknie małe, przestawne, które użytkownik może umieścić w dowolnym miejscu na ekranie powyżej innych uruchomionych aplikacji.

[![](multitasking-images/about03.png "Przykład obrazu w obraz przestawne okno")](multitasking-images/about03.png#lightbox)

Jako slajd za pośrednictwem i widok podzielony użytkownik ma pełną kontrolę nad oglądanie wideo na obrazie w trybie obrazu. Jeśli główną funkcją aplikacji Obejrzyj klip wideo, należy modyfikacji niektórych będzie działać prawidłowo w trybie PIP. W przeciwnym razie wartość do obsługi PIP nie są konieczne nie zmiany.

Dla aplikacji do wyświetlenia PIP wideo na żądanie użytkownika, należy używać albo _AVKit_ lub _AV Foundation API_. Platformę Media Player zostały amortyzacji w systemie iOS 9 i nie obsługuje PIP.

Obraz na rysunku jest dostępna tylko na iPad Pro, iPad lotniczego, iPad 2 lotniczego, iPad Mini 2, iPad Mini 3 lub 4 dla urządzenia iPad. Aby uzyskać więcej informacji, zobacz nasze [PictureInPicture Przykładowa aplikacja](https://developer.xamarin.com/samples/ios/iOS9/) i firmy Apple [obrazu w obraz Szybki Start](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/QuickStartForPictureInPicture.html#//apple_ref/doc/uid/TP40015145-CH14) dokumentacji.

<a name="Supporting-Multitasking-in-your-App" />

## <a name="supporting-multitasking-in-your-app"></a>Obsługa wielozadaniowości w aplikacji

Żadnych istniejących aplikacji platformy Xamarin.iOS Obsługa wielozadaniowości jest przezroczysty zadań tak długo, jak aplikacji jest już zgodny przewodniki projektowe firmy Apple i najlepsze rozwiązania dla systemu iOS 8. Oznacza to, że aplikacji powinien być używany scenorys z klasy rozmiar i Autolayout dla jego układów interfejsu użytkownika (zobacz nasze [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) Aby uzyskać więcej informacji).

Te aplikacje są wymagane do obsługi wielozadaniowości i zachowywać się dobrze w niewielkiego lub żadnego zmiany. Jeśli w Interfejsie użytkownika aplikacji, został utworzony przy użyciu innych metod, takich jak bezpośrednio pozycjonowanie i rozmiary elementów interfejsu użytkownika w kodzie języka C# lub jeśli go zależy od rozmiaru ekranu określonego urządzenia lub kierunki, konieczne będzie znaczące zmiany do obsługi systemu iOS 9 wielozadaniowości poprawnie.

Do obsługi systemu iOS 9 wielozadaniowości na dowolnym nowej aplikacji platformy Xamarin.iOS, ponownie użyć scenorys z Autolayout i klasy rozmiar wszystkich układów interfejsu użytkownika aplikacji i wdrożenie zgodnie z instrukcjami w poniższych sekcjach.

<a name="Screen-Size-Considerations" />

### <a name="screen-size-and-orientation-considerations"></a>Zagadnienia dotyczące orientacji i rozmiaru ekranu

Przed iOS 9 można zaprojektować aplikacji przed rozmiaru ekranu określonego urządzenia i orientacji. Ponieważ aplikację można uruchomić w panelu przesuwa się lub w trybie podzielony widok, znajduje się działających w jednej klasy compact lub regularnych rozmiar poziomych na iPad, niezależnie od urządzenia fizycznego orientacji lub rozmiaru ekranu.

[![](multitasking-images/sizeclasses01.png "Zagadnienia dotyczące orientacji i rozmiaru ekranu")](multitasking-images/sizeclasses01.png#lightbox)

Na urządzeniu iPad aplikacji pełny ekran ma zwykłych klas rozmiar poziomie i w pionie. Wszystkie urządzenia iPhone, ale telefonów iPhone 6 Plus oraz iPhone 6s Plus, należy mieć rozmiar Compact klas w obu kierunkach w dowolnym orientacji. IPhone 6 Plus oraz iPhone 6s Plus w trybie krajobraz mają zwykłej poziomy klasy rozmiar i Compact klasy rozmiar pionowy (podobne do iPad Mini).

Na Ipad obsługujące slajd za pośrednictwem i widok podzielony może kończyć przy użyciu następujących kombinacji:

| **Orientacja** | **Głównej aplikacji** | **Dodatkowej aplikacji** |
|--- |--- |--- |
| **Orientacji pionowej** |75% ekranu<br />Poziomy Compact<br />Regularne pionowe|25% ekranu<br />Poziomy Compact<br />Regularne pionowe|
| **orientacji poziomej** |75% ekranu<br />Regularne poziomej<br />Regularne pionowe|25% ekranu<br />Poziomy Compact<br />Regularne pionowe|
| **orientacji poziomej** |50% ekranu<br />Poziomy Compact<br />Regularne pionowe|50% ekranu<br />Poziomy Compact<br />Regularne pionowe|

W przykładzie [MuliTask](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) aplikacji, jeśli pełny ekran jest uruchamiane na urządzeniu iPad w trybie krajobraz przedstawi zarówno listy jak i widoku szczegółów w tym samym czasie:

[![](multitasking-images/sizeclasses03.png "Listy i widok szczegółów przedstawione w tym samym czasie")](multitasking-images/sizeclasses03.png#lightbox)

Jeśli ta sama aplikacja jest uruchamiana w slajd za pośrednictwem panelu, jest poukładany jako klasę Compact rozmiar poziome i wyświetla tylko na liście:

[![](multitasking-images/sizeclasses04.png "Tylko lista wyświetlana, gdy urządzenie jest pozioma")](multitasking-images/sizeclasses04.png#lightbox)

Aby zapewnić poprawne działanie aplikacji w takich sytuacjach, należy przyjąć kolekcje cechy wraz z klasy wielkości i są zgodne z `IUIContentContainer` i `IUITraitEnvironment` interfejsów. Zobacz firmy Apple [odwołania do klasy UITraitCollection](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITraitCollection_ClassReference/index.html#//apple_ref/doc/uid/TP40014202) i naszych [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) przewodnika, aby uzyskać więcej informacji.

Ponadto możesz już polegać granice ekranu urządzenia, aby zdefiniować widocznym obszarze aplikacji, musisz użyć granice okna aplikacji. Ponieważ granice okna są w pełni pod kontrolą użytkownika, nie można programowo je dopasować lub uniemożliwić użytkownikowi zmienianie tych granic.

Na koniec aplikacji należy użyć pliku scenorysu do prezentowania jego uruchomienie ekranu zamiast przy użyciu zestawu **.png** pliki obrazów i obsługują wszystkie orientacje cztery interfejsu (pionowy, pionowej dołu, pozioma po lewej i prawej orientacji poziomej) to zostać uwzględnione podczas pracy w trybie podzielony widok lub Przesuń za pośrednictwem panelu.

<a name="Custom-Hardware-Keyboard-Shortcuts" />

### <a name="custom-hardware-keyboard-shortcuts"></a>Skróty klawiaturowe niestandardowego sprzętu

W iOS 9 uruchomione na urządzeniu iPad, Apple została rozszerzona obsługa klawiatury sprzętu. Ipad zawsze zawiera Obsługa podstawowe klawiatury zewnętrznej za pośrednictwem połączenia Bluetooth i niektóre klawiatury producentów utworzone klawiatury, które uwzględnione zakodowane klucze specyficzne dla systemu iOS.

Teraz iOS 9, aplikacje umożliwia tworzenie własnych niestandardowych skrótów klawiaturowych. Ponadto niektóre podstawowe skróty klawiaturowe są dostępne, takich jak **polecenia C** (Kopiuj) **X polecenia** (Wytnij), **V polecenia** (Wklej) i **polecenia-Shift-H**  (głównej), nie jest specjalnie napisane odpowiedź do nich aplikacji.

**Karta poleceń** pojawi się przełącznik aplikacji, który umożliwia użytkownikowi szybkie przełączanie między aplikacjami z klawiatury, podobnie jak Mac OS:

[![](multitasking-images/keyboard01.png "Przełącznik aplikacji")](multitasking-images/keyboard01.png#lightbox)

Jeśli aplikacja systemu iOS 9 zawiera skróty klawiaturowe, użytkownik może przytrzymaj **polecenia**, **opcji** lub **kontroli** kluczy, aby je wyświetlić w okienku podręcznym:

[![](multitasking-images/keyboard02.png "Menu podręczne skróty klawiaturowe")](multitasking-images/keyboard02.png#lightbox)

#### <a name="defining-custom-keyboard-shortcuts"></a>Definiowanie niestandardowych skrótów klawiaturowych

Jeśli firma Microsoft Dodaj następujący kod do widoku lub widok kontroler w naszej aplikacji, gdy tego widoku lub kontrolera jest widoczna, skrót klawiaturowy niestandardowe będą dostępne:

```csharp
#region Custom Keyboard Shortcut
public override bool CanBecomeFirstResponder {
    get { return true; }
}

public override UIKeyCommand[] KeyCommands {
    get {

        var keyCommand = UIKeyCommand.Create (new NSString("n"), UIKeyModifierFlags.Command, new Selector ("NewEntry"), new NSString("New Entry"));
        return new UIKeyCommand[]{ keyCommand };
    }
}

[Export("NewEntry")]
public void NewEntry() {

    // Add a new entry
    ...

}
#endregion
```

Po pierwsze, możemy zastąpić `CanBecomeFirstResponder` właściwości i przywracać `true` , widoku lub widok kontroler może odbierać wprowadzanie z klawiatury. 

Następnie możemy zastąpić `KeyCommands` właściwości i utworzyć nową `UIKeyCommand` dla **polecenia N** naciśnięcie klawisza. Po aktywowaniu naciśnięcie klawisza nazywamy `NewEntry` — metoda (który uwidaczniamy przy użyciu systemu iOS 9 `Export` polecenie) można wykonać żądanej akcji.

Jeśli firma Microsoft Uruchom tę aplikację na urządzeniu iPad z dołączoną klawiaturą sprzętu i typy użytkownika **N polecenia**, nowy wpis zostanie dodany do listy. Jeśli użytkownik jest przechowywana w dół **polecenia** klucza, skrótów zostanie wyświetlona lista:

[![](multitasking-images/keyboard03.png "Menu podręczne skróty klawiaturowe")](multitasking-images/keyboard03.png#lightbox)

Zobacz przykład [aplikacji MultiTask](http://developer.xamarin.com/samples/monotouch/ios9/MultiTask/) dla przykładem implementacji.

<a name="Resource-Management-Considerations" />

### <a name="resource-management-considerations"></a>Zagadnienia dotyczące zarządzania zasobów

Nawet w przypadku aplikacji, które już używają przewodniki projektowania i najlepsze rozwiązania dla systemu iOS 8 zarządzanie zasobami wydajne może wystąpić problem. W systemie iOS 9 aplikacje nie muszą już wyłączne użycie pamięci, procesora CPU lub innych zasobów.

W związku z tym należy dostosować aplikację platformy Xamarin.iOS, aby skutecznie wykorzystuje zasoby systemowe lub zwróconym zakończenia w sytuacji braku pamięci. Dotyczy to jednakowo aplikacji, które zrezygnować z wielozadaniowości, od drugiej aplikacji mogą nadal działać w panelu slajd za pośrednictwem lub obrazu w oknie Obraz wymagających dodatkowe zasoby lub powoduje częstotliwość odświeżania spadnie poniżej 60 klatek na sekundę.

Należy wziąć pod uwagę następujące akcje użytkownika i ich wpływ na:

- **Wprowadzanie tekstu w panelu slajd za pośrednictwem** — nawet wtedy, gdy aplikacja ma nie wprowadzania tekstu, klawiatury systemu może być teraz wyświetlana w jego interfejsie użytkownika. W związku z tym aplikacji może być konieczne odpowiadanie na klawiaturze wyświetlane powiadomienia (na przykład wyświetlanie i ukrywanie klawiatury).
- **Systemem drugiej aplikacji w panelu slajd za pośrednictwem** — nowej aplikacji jest teraz uruchomiona na pierwszym planie i konkurujących z istniejącej aplikacji dla zasobów systemowych, takich jak pamięć i cykle procesora CPU.
- **Odtwarzanie wideo w oknie narzędzia PIP** — nie tylko do tego okna pokryje część interfejsu użytkownika aplikacji, ale nadal uruchomione w tle i korzystanie z zasobów Procesora i pamięci aplikacji, który uruchamiany wideo.

Aby upewnić się, że aplikacja używa zasobów wydajnie, należy wykonać następujące:

- **Profil aplikacji z instrumentów** -Sprawdź, czy wyciek pamięci, widocznych użycie procesora CPU i obszary, w którym mogą blokować aplikacji wątku głównego.
- **Odpowiadanie na metod przejścia stanu** — w Twojej **AppDelegate.cs** zastąpienie pliku i odpowiedzi do stanu zmiana metody takie jak aplikacji, wprowadzając lub zwracanie z tła. Zwalnia wszelkie niewymaganych zasobów, takich jak obrazy, danych lub widoków i kontrolera widoku.
- **Testowanie Side-by-Side z aplikacji intensywnie korzystających z pamięci** — Uruchom aplikację przy użyciu zarówno przesuwa się i widok podzielony na sprzęcie fizycznych z systemem iOS z pamięci znacznym aplikacji, takie jak mapy (w trybie widoku satelity) i testowania czy obie aplikacje pozostają reakcji i nie awarii.

Zobacz firmy Apple [energii przewodnik wydajności dla aplikacji systemu iOS](https://developer.apple.com/library/prerelease/ios/documentation/Performance/Conceptual/EnergyGuide-iOS/index.html#//apple_ref/doc/uid/TP40015243) uzyskać więcej informacji dotyczących zarządzania zasobami.

<a name="Opting-Out-of-Multitasking" />

## <a name="opting-out-of-multitasking"></a>Rezygnacja z wielozadaniowości

Podczas Apple sugeruje, że wszystkie aplikacje systemu iOS 9 obsługują wielozadaniowości, może być powodów bardzo specyficzne dla aplikacji nie zbyt takich jak gry lub aparatu aplikacji, które wymagają pełny ekran, aby działać poprawnie.

Dla aplikacji platformy Xamarin.iOS rezygnacji z uruchomione w trybie podzielony widok lub albo przesuwa się panelu, Edytuj projektu **Info.plist** plik i sprawdź **wymaga pełnego ekranu**:

[![](multitasking-images/fullscreen01.png "Rezygnacja z wielozadaniowości")](multitasking-images/fullscreen01.png#lightbox)

> [!IMPORTANT]
> Gdy rezygnacja z wielozadaniowości uniemożliwia aplikację uruchamianą w przesuwa się lub podzielony widok, zapobiega ono innej aplikacji działa w przesuwa się lub obraz obrazu wideo przy wyświetlaniu wraz z aplikacji.

<a name="Disabling-PIP-Video-Playback" />

### <a name="disabling-pip-video-playback"></a>Wyłączanie PIP odtwarzania wideo.

W większości przypadków aplikacji należy zezwolić użytkownikowi na odtwarzać zawartość wideo, wyświetlania obrazu w obraz przestawne okno. Jednak może się zdarzyć, gdzie to może być niepożądane, takich jak wideo gier wycięte sceny.

Aby zrezygnować z odtwarzania wideo PIP, wykonaj następujące czynności w aplikacji:

 - Jeśli używasz `AVPlayerViewController` Aby wyświetlić wideo, ustaw `AllowsPictureInPicturePlayback` właściwości `false`.
 - Jeśli używasz `AVPlayerLayer` wystąpienia nie można wyświetlić wideo, `AVPictureInPictureController`.
 - Jeśli używasz `WKWebView` Aby wyświetlić wideo, ustaw `AllowsPictureInPictureMediaPlayback` właściwości `false`.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule ma opisano kroki wymagane w celu zapewnienia, że aplikacji platformy Xamarin.iOS zostaną uruchomione i poprawnie zachowanie systemu iOS 9 w nowych możliwości wielozadaniowości dla urządzenia Ipad. Ponadto objętych usługą Rezygnacja ruch wychodzący wielowątkowości w przypadku aplikacji, gdy nie jest odpowiedni.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady z systemem iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Równoczesną (przykład)](https://developer.xamarin.com/samples/monotouch/ios9/MultiTask/)
- [Wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md)
- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Przyjmowanie ulepszenia wielozadaniowości na iPad](https://developer.apple.com/library/prerelease/ios/documentation/WindowsViews/Conceptual/AdoptingMultitaskingOniPad/index.html#//apple_ref/doc/uid/TP40015145)
- [wpis w blogu](https://blog.xamarin.com/using-auto-layouts-for-ios-9-splitview/)
