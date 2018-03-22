---
title: Wprowadzenie do watchOS
description: "Omówienie struktury rozwiązania watchOS i ograniczenia"
ms.topic: article
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: fbf430e16506bdcf89ea3e280d42f27b0406f153
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-watchos"></a>Wprowadzenie do watchOS

> [!NOTE]
> Zapoznaj się z [wprowadzenie do watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md) omówienie najnowszych funkcji.

## <a name="about-watchos"></a>O watchOS

Rozwiązanie aplikacji watchOS ma projektów 3:

- **Obejrzyj rozszerzenia** — projekt, który zawiera kod aplikacji czujki.
- **Obejrzyj aplikacji** — zawiera scenorysu interfejsu użytkownika i zasoby.
- **Nadrzędny aplikacja systemu iOS** — jest aplikacja normalne iPhone. Obejrzyj aplikacji i rozszerzenia są połączone w aplikacji iPhone do dostarczenia czujki użytkownika.

W aplikacjach watchOS 1 kod w rozszerzeniu działa na telefonie iPhone — Apple Watch jest wyświetlanych. Uruchom aplikacje watchOS 2 i 3 na Apple Watch. Na poniższym diagramie przedstawiono to tej różnicy:

[ ![](intro-to-watchos-images/arch-sml.png "Różnica między watchOS 1 i watchOS 2 (lub nowszej) jest wyświetlany na tym diagramie")](intro-to-watchos-images/arch.png#lightbox)

Niezależnie od tego, która wersja watchOS podlega w Visual Studio for Mac w rozwiązaniu konsoli kompletne rozwiązanie będzie wyglądać następująco:

[![](intro-to-watchos-images/projectstructure-sml.png "Konsola rozwiązania")](intro-to-watchos-images/projectstructure.png#lightbox)

*Aplikacji nadrzędnej* w watchOS rozwiązaniem jest aplikacji iOS regularne. Jest to tylko projektu w rozwiązaniu, które są widoczne **na telefonie**. Przypadki użycia dla tej aplikacji obejmuje samouczki, ekranów administracyjnych i warstwy środkowej filtrowania cacheing itp. Warto jednak użytkownik może zainstalować i uruchomić rozszerzenia czujki aplikacji/bez **kiedykolwiek** po otwarciu aplikacji nadrzędnej Jeśli potrzebujesz aplikacji nadrzędnej przeprowadzić jednorazowe inicjowanie lub administracyjnej, należy więc program Twojej czujki aplikacji/rozszerzenia Monituj użytkownika, który.

Chociaż aplikacji nadrzędnej zapewnia czujki aplikacji i rozszerzenia, działają w różnych obszarów izolowanych.

Na watchOS 1 mogą współużytkować dane za pośrednictwem grupy udostępnionej aplikacji lub za pomocą funkcji statycznej `WKInterfaceController.OpenParentApplication`, które powodują `UIApplicationDelegate.HandleWatchKitExtensionRequest` metody w aplikacji nadrzędnej `AppDelegate` (zobacz [Praca z aplikacji nadrzędnej](~/ios/watchos/app-fundamentals/parent-app.md)).

Na watchOS 2 lub nowszym framework czujki łączności jest używany do komunikacji z aplikacją nadrzędnego przy użyciu `WCSession` klasy.

## <a name="application-lifecycle"></a>Cykl życia aplikacji

W rozszerzeniu czujki, podklasa klasy `WKInterfaceController` klasy jest tworzona dla każdej sceny scenorysu.

Te `WKInterfaceController` klasy są odpowiednikiem `UIViewController` obiektów w programowaniu dla systemu iOS, ale nie ma taki sam poziom dostępu do widoku.
Na przykład nie można dynamicznie dodawanie formantów do lub restrukturyzacji interfejsu użytkownika.
Możesz, jednak Ukryj i ujawnić formantów i, niektóre funkcje kontroli, zmienić ich rozmiar, przezroczystość i opcje wyglądu.

Cykl życia `WKInterfaceController` obiektu obejmuje następujące wywołania:

- [Wznowione](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) : większość Twojej inicjowania należy wykonać w ramach tej metody.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : wywoływana tuż przed aplikacja czujki widoczna dla użytkownika. Użyj tej metody do wykonania inicjowania moment ostatniego uruchomienia animacji itp.
- W tym momencie aplikacja czujki widoczna i rozszerzenie rozpoczyna odpowiada na żądania użytkownika argument wejściowy i aktualizowanie aplikacji czujki wyświetlania na logiki aplikacji.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) po czujki aplikacji został odrzucony przez użytkownika, ta metoda jest wywoływana. Po powrocie z tej metody nie można zmodyfikować formantów interfejsu użytkownika do następnego wznowienia `WillActivate` jest wywoływana. Również będzie można wywołać tej metody, jeśli połączenie telefonów iPhone zostanie przerwane.
- Po dezaktywacji rozszerzenia jest niedostępny do programu. Oczekujące funkcje asynchroniczne **nie** można wywołać. Obejrzyj zestaw rozszerzeń nie mogą używać trybów przetwarzania w tle. Jeśli program ponownie uaktywnić przez użytkownika, ale aplikacja nie została przerwana przez system operacyjny, będzie pierwsza metoda wywoływana `WillActivate`.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Przegląd cyklu życia aplikacji")

## <a name="types-of-user-interface"></a>Typy interfejsu użytkownika

Istnieją trzy typy interakcji, które użytkownik może mieć ze swoją aplikacją czujki.
Wszystkie zaprogramowane, za pomocą niestandardowej klasy podrzędnej `WKInterfaceController`, więc sekwencji wspomnianej wcześniej cyklu życia stosuje powszechnie (powiadomienia są zaprogramowane w taki sposób, z klasy podrzędnej `WKUserNotificationController`, które jest podklasą klasy `WKInterfaceController`):

### <a name="normal-interaction"></a>Normalne interakcji

Większość interakcji aplikacji/rozszerzenia czujki będzie z klasy podrzędnej `WKInterfaceController` czy możesz zapisywać do odpowiadają sceny aplikację czujki **Interface.storyboard**. To jest omówiona szczegółowo w [instalacji](~/ios/watchos/get-started/installation.md) i [wprowadzenie](~/ios/watchos/get-started/index.md) artykułów.
Na poniższej ilustracji przedstawiono część [katalogu zestawu czujki](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) scenorysu w próbce. Dla każdego pokazano, w tym miejscu sceny, jest odpowiednie niestandardowego `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`, itd.) w projekcie rozszerzenia.

![](intro-to-watchos-images/scenes.png "Przykłady normalny interakcji")

### <a name="notifications"></a>Powiadomienia

[Powiadomienia](~/ios/watchos/platform/notifications.md) dotyczą Apple Watch najważniejszych przypadków użycia. Lokalne i zdalne powiadomienia są obsługiwane. Interakcja z powiadomieniami występuje w dwóch etapach o nazwie wyglądu krótki i Long.

Krótki wygląda są wyświetlane krótko i wyświetlić ikona aplikacji monitorowania, jego nazwa i tytuł (jak określono z `WKInterfaceController.SetTitle`).

Szukaj długi łączy dostarczane przez system **skrzydła** obszarów i przycisk Odrzuć z niestandardowej zawartości na podstawie scenorysu.

`WKUserNotificationInterfaceController` Rozszerza `WKInterfaceController` z metodami `DidReceiveLocalNotification` i `DidReceiveRemoteNotification`.
Zastąp te metody reagowanie na zdarzenia powiadomień.

Więcej informacji dotyczących projektu interfejsu użytkownika powiadomień, się [Apple Watch Human Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "Przykładowe powiadomienia")

## <a name="screen-sizes"></a>Rozmiaru ekranu

Apple Watch ma dwa rozmiary krój: 38 do 42mm, zarówno z 5:4 stosunek wyświetlania i wyświetlania siatkówki. Ich użyteczny rozmiary są:

- 38 mm: 136 x 170 logicznej pikseli (w pikselach fizycznych 272 x 340)
- 42 mm: 156 x 195 logicznej pikseli (w pikselach fizycznych 312 x 390).

Użyj `WKInterfaceDevice.ScreenBounds` ustalenie, na którym wyświetlana jest uruchomiony aplikacji czujki.

Ogólnie rzecz biorąc łatwiej jest opracowanie tekstu i układ projektu z bardziej ograniczone wyświetlania 38mm i skalowanie w górę.
Po uruchomieniu ze środowiskiem większych skalowania może prowadzić do ugly nakładają się na siebie lub obcięcie tekstu.

Przeczytaj więcej na temat [Praca z rozmiaru ekranu](~/ios/watchos/app-fundamentals/screen-sizes.md).


## <a name="limitations-of-watchos"></a>Ograniczenia watchOS

Istnieją pewne ograniczenia watchOS pod uwagę podczas opracowywania aplikacji watchOS:

- Apple Watch urządzenia mają ograniczoną magazynu — należy pamiętać o dostępne miejsce przed pobraniem dużych plików (np.) pliki audio lub film).

- Wiele watchOS [formanty](~/ios/watchos/user-interface/index.md) mają podobne produkty w UIKit, ale są różnych klas (`WKInterfaceButton` zamiast `UIButton`, `WKInterfaceSwitch` dla `UISwitch`, itd.) i mają ograniczony zestaw metod w porównaniu do ich UIKit odpowiedniki. Ponadto watchOS ma niektóre formanty takich jak `WKInterfaceDate` (do wyświetlania, Data i godzina) nie ma tego UIKit.

  - Nie można przekierować powiadomień czujki tylko lub iPhone tylko (jakiego rodzaju formantu ma użytkownik nad routingu nie ogłoszono przez firmę Apple).

Znane ograniczenia / często zadawane pytania:

- Apple nie zezwala na kroje czujki niestandardowych 3rd firm.

- Interfejsy API umożliwiające Obejrzyj, aby kontrolować iTunes połączenia telefonicznego są prywatne.


## <a name="further-reading"></a>Dalsze informacje

Zapoznaj się dokumentacją firmy Apple:

* [Tworzenie aplikacji dla zestawu czujki](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [Obejrzyj przewodnik programowania w języku Kit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Wskazówki dotyczące interfejsu Apple Watch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>Linki pokrewne

- [watchOS 3 katalogu (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [watchOS 1 katalogu (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Konfigurowanie i instalowanie oprogramowania](~/ios/watchos/get-started/installation.md)
- [Pierwszy wideo aplikacji czujki](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple do projektowania dla przewodnik zestawu czujki](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Porady WatchKit firmy Apple](https://developer.apple.com/watchkit/tips/)
- [Wprowadzenie do systemu watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
