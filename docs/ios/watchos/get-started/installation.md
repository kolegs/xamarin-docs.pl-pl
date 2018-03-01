---
title: Instalacja i Konfiguracja
description: Konfigurowanie na watchOS
ms.topic: article
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/05/2017
ms.openlocfilehash: f7e511d7f0a933ab7f29369e5e5f0aa46607c8f8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="installation"></a>Instalacja

watchOS 4 wymaga macOS Sierra (10.12) z Xcode 9.

watchOS 1 wymagane pierwotnie OS X Yosemite (10.10) z Xcode 7.

> [!WARNING]
> [watchOS 1 aktualizacje nie będą akceptowane po 1 kwietnia 2018](https://developer.apple.com/news/?id=11162017a). Przyszłe aktualizacje należy użyć watchOS 2 zestawu SDK lub nowszy; kompilowanie z użyciem watchOS 4 zestawu SDK jest zalecane.

## <a name="project-structure"></a>Struktury projektu

Aplikacja czujki obejmuje trzy projekty:

- **Projekt aplikacji platformy Xamarin.iOS iPhone** — jest to projekt normalne iPhone, może być tych szablonów platformy Xamarin.iOS. Obejrzyj aplikacji i jego rozszerzenie zostanie dołączone wewnątrz tego projektu.

- **Projekt rozszerzenia czujki** — zawiera kod (takich jak klasy Controller) dla aplikacji czujki.

- **Projekt aplikacji czujki** — zawiera plik scenorysu interfejsu użytkownika z wszystkimi zasobami interfejsu użytkownika dla aplikacji czujki.

[Próbki katalogu zestawu czujki](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) rozwiązania wygląda następująco w Xamarin.Studio:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](installation-images/catalog-solution.png "Rozwiązanie programu Visual Studio")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/catalog-solution-vs.png "Rozwiązanie programu Visual Studio")

-----

Pobierz i uruchom [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) przykład, aby rozpocząć pracę.
Ekrany z próbki można znaleźć w [formanty](~/ios/watchos/user-interface/index.md) strony.


## <a name="creating-a-new-project"></a>Tworzenie nowego projektu

Nie można utworzyć nowe "Obejrzyj rozwiązanie"... zamiast aplikacji czujki można dodać do istniejącej aplikacji systemu iOS. Wykonaj następujące kroki, aby utworzyć aplikację czujki:

1. Jeśli nie masz istniejący projekt, należy najpierw wybrać **Plik > nowe rozwiązanie** i Utwórz aplikację systemu iOS (na przykład **jednej aplikacji widoku**):

    [ ![](installation-images/cycle8-2-sml.png "Wybierz Plik > nowe rozwiązanie i Utwórz aplikację systemu iOS")](installation-images/cycle8-2.png)

2. Po utworzeniu aplikacji systemu iOS (lub planujesz użyć istniejącej aplikacji systemu iOS), kliknij prawym przyciskiem myszy w ramach rozwiązania i wybierz polecenie **Dodaj > Dodawanie nowego projektu.** . W **nowy projekt** wybierz okno **watchOS > aplikacji > aplikacji WatchKit**:

    [ ![](installation-images/cycle8-6-sml.png "Wybierz watchOS > aplikacji > WatchKit aplikacji")](installation-images/cycle8-6.png)

3. Następnym ekranie pozwala wybrać, który projekt aplikacji systemu iOS powinna zawierać czujki aplikacji:

    [ ![](installation-images/cycle8-7-sml.png "Wybierz, który projekt aplikacji systemu iOS powinna zawierać aplikacji czujki")](installation-images/cycle8-7.png)

4. Na koniec wybierz lokalizację do zapisania projektu (i opcjonalnie włączone kontroli źródła):

    [ ![](installation-images/cycle8-8-sml.png "Wybierz lokalizację do zapisania projektu")](installation-images/cycle8-8.png)

5. Visual Studio for Mac automatycznie konfiguruje [odwołania do projektu i **Info.plist** ustawienia](~/ios/watchos/get-started/project-references.md) dla Ciebie.

## <a name="creating-the-watch-user-interface"></a>Tworzenie interfejsu użytkownika czujki

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Za pomocą platformy Xamarin iOS projektanta

Kliknij dwukrotnie aplikacji czujki **Interface.storyboard** można edytować za pomocą projektanta dla systemu iOS. Możesz przeciągnąć kontrolery interfejsu i formantów interfejsu użytkownika do scenorysu z **przybornika** i skonfigurować je za pomocą **właściwości** konsoli:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](installation-images/iosdesigner-sml.png "Scenorysu w Projektancie")](installation-images/iosdesigner.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](installation-images/iosdesigner-sml-vs.png "Scenorysu w Projektancie")](installation-images/iosdesigner-vs.png)

-----

Należy nadać każdego nowego kontrolera interfejsu **klasy** zaznaczyć, a następnie wprowadzić nazwę w **właściwości** konsoli (spowoduje to utworzenie wymaganych plików związanym kodzie C# automatycznie):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](installation-images/iosdesigner-classname.png "Nadaj każdej nowego kontrolera interfejsu klasy")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/iosdesigner-classname-vs.png "Nadaj każdej nowego kontrolera interfejsu klasy")

-----

Utwórz segues przez **Ctrl + przeciąganie** z przycisku, tabeli lub interfejs kontrolera na inny kontroler interfejsu.


### <a name="using-xcode-on-the-mac"></a>Za pomocą środowiska Xcode dla komputerów Mac

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Można nadal używać Xcode do tworzenia interfejsu użytkownika, kliknięcie prawym przyciskiem myszy plik Interface.storyboard i wybierając **Otwórz za pomocą > konstruktora interfejsu Xcode**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Użytkowników programu Visual Studio umożliwia także Xcode kompilacji przełączając bezpośrednio używać hosta kompilacji Mac za pośrednictwem interfejsu użytkownika.
Otwórz rozwiązanie w programie Visual Studio dla komputerów Mac, a następnie kliknij prawym przyciskiem myszy plik Interface.storyboard i wybierz **Otwórz za pomocą > konstruktora interfejsu Xcode**:

-----

![](installation-images/openwith-xcode.png "Otwórz Interface.storyboard w Konstruktorze Xcode — interfejs")

Jeśli za pomocą środowiska Xcode, następnie należy wykonać te same kroki dla aplikacji czujki, podobnie jak w przypadku normalny [scenorys aplikacji systemu iOS](~/ios/user-interface/storyboards/index.md) (takich jak tworzenie gniazda i akcje przez **Ctrl + przeciąganie** do **.h**plik nagłówka).

Po zapisaniu scenorysu w Konstruktorze interfejsu Xcode automatycznie doda gniazda i akcje tworzenia do języka C# **. Designer.cs narzędzie** pliki w projekcie czujki rozszerzenia.


### <a name="adding-additional-screens-in-xcode"></a>Dodawanie dodatkowych ekranów w programie Xcode

Po dodaniu dodatkowych ekrany (przekraczające w szablonie domyślnie) z scenorysu za pomocą konstruktora interfejsu Xcode **należy ręcznie dodać pliki kodu C#** dla każdego nowego kontrolera interfejsu.

Zapoznaj się [zaawansowane instrukcje dotyczące sposobu dodawania nowych kontrolerów interfejsu do scenorysu](~/ios/watchos/troubleshooting.md#add).

*Xamarin iOS projektanta robi to automatycznie, nie są wymagane żadne czynności ręcznych.*


## <a name="building"></a>Kompilowanie

Tworzy projekt zawierający aplikację czujki podobnie jak inne projekty dla systemu iOS. Proces tworzenia spowoduje aplikacji iPhone (.app), która zawiera rozszerzenia czujki (.appex), który z kolei zawiera czujki bez kodu aplikacji (.app).


## <a name="launching"></a>Uruchamianie

Możesz uruchomić czujki aplikacji w symulatorze przy użyciu albo program Visual Studio dla komputerów Mac lub Visual Studio (rozpoczyna się na hoście kompilacji Mac).

Istnieją dwa tryby uruchomienie aplikacji WatchKit:

 - Tryb aplikacji normalne (ustawienie domyślne), a
- [Powiadomienia](~/ios/watchos/platform/notifications.md) (co wymaga ładunku powiadomienia testu w formacie JSON).

### <a name="xcode-8-support"></a>Obsługa Xcode 8

Po zainstalowaniu Xcode 8 (lub nowszym) firmy Apple Watch są niezależne od iOS symulatorów (w przeciwieństwie do [Xcode 6](#xcode6), gdzie były wyświetlane jako *wyświetlanych*).
Po wybraniu projektu aplikacji czujki i stał się projekt startowy, zostaną wyświetlone na liście symulatora *iOS symulatorów* do wyboru (jak pokazano poniżej).

[ ![](installation-images/xs-xcode8-watchos3-sml.png "Wybieranie typu symulatora")](installation-images/xs-xcode8-watchos3.png)

Po rozpoczęciu debugowania, *dwóch* symulatorów powinien rozpocząć - symulatora systemu iOS *i* Apple Watch symulatora. Użyj **polecenia + Shift + H** przejdź do powierzchni menu i zegara czujki; i użyj **sprzętu** menu, aby ustawić **wykorzystania Touch życie**. Przewijanie na trackpad lub mysz symulować, przy użyciu wierzchołek cyfrowego.

#### <a name="troubleshooting"></a>Rozwiązywanie problemów

Pojawi się następujący błąd w **danych wyjściowych aplikacji** próby Uruchom, aby symulatora niezawierający sparowanego czujki:

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Zapoznaj się [fora firmy Apple](https://forums.developer.apple.com/thread/7783) zawiera instrukcje dotyczące konfigurowania symulatorów, jeśli wartości domyślne nie działają.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Edytor Xcode 6 i watchOS 1

Należy się upewnić *projekt rozszerzenia czujki* **projekt startowy** przed uruchomieniem ani debugowania aplikacji. Nie można "uruchomić" Obejrzyj aplikacja, i po wybraniu aplikacji systemu iOS następnie zostanie uruchomiony w narzędziu iOS Simulator normalnego.

Domyślnie aplikacji czujki rozpoczyna się w normalnym **aplikacji** mode (tryb nie oka lub powiadomienia) z programu Visual Studio dla komputerów Mac w **Uruchom** lub **debugowania** poleceń.

Korzystając z Xcode 6 tylko telefonu iPhone 5, iPhone 5S, iPhone 6 i iPhone 6 Plus może aktywować wyświetlanych w obu **Apple Watch - 38mm** lub **Apple Watch - 42mm** której będzie aplikacji czujki wyświetlane.

**Uwaga:** należy pamiętać, że ekran czujki automatycznie nie w symulatorze systemu iOS przy użyciu Xcode 6.
Użyj **sprzętu > wyświetla zewnętrznych** menu, aby wyświetlić ekran czujki.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>Tryb uruchamiania powiadomień

Zapoznaj się [strony powiadomienia](~/ios/watchos/platform/notifications.md) informacji sposób obsługi powiadomień w kodzie.


Visual Studio for Mac można uruchomić aplikacji czujki z powiadomienie _trybów uruchamiania_ dla powiadomień:



Kliknij prawym przyciskiem myszy projekt aplikacji czujki i wybierz polecenie **Uruchom z > Konfiguracja niestandardowa...** :


[![](installation-images/runwith-customparams-sml.png "Uruchomiona jest Konfiguracja niestandardowa")](installation-images/runwith-customparams.png)


Spowoduje to otwarcie **niestandardowe parametry** okna, w którym można wybrać **powiadomień** (i zapewniać ładunek JSON), naciśnij klawisz **Uruchom** można uruchomić aplikacji czujki w symulatorze:


[![](installation-images/runwith-execargs-sml.png "Ustawianie powiadomień i ładunku")](installation-images/runwith-execargs.png)



## <a name="debugging"></a>Debugowanie

Debugowanie jest obsługiwane w Visual Studio dla komputerów Mac i Visual Studio.
Pamiętaj przekazać plik JSON powiadomienia podczas debugowania w trybie powiadomienia. Ten zrzut ekranu przedstawia trafienia w aplikacji czujki punkt przerwania debugowania:

![](installation-images/debug-sml.png "Ten zrzut ekranu przedstawia punktu przerwania debugowania trafienia w aplikacji czujki")

Po wykonaniu instrukcji uruchamiania będzie na końcu czujki aplikacji uruchomionej na **(czujki) symulatora systemu iOS**.
Dla trybu powiadomień można wybrać **Debuguj > Otwórz dziennik systemu** (**CMD + /**) i używać `Console.WriteLine` w kodzie.

### <a name="debugging-lifecycle-event-handlers"></a>Debugowanie programów obsługi zdarzeń cyklu życia

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

Pliki szablonów watchOS (takich jak `InterfaceController`, `ExtensionDelegate`, `NotificationController`, i `ComplicationController`) dostarczanych z ich metod wymagane cyklu życia już wdrożone. Dodaj `Console.WriteLine` wywołania i Odczyt **danych wyjściowych aplikacji** Aby dowiedzieć się więcej o zdarzeń cyklu życia.



## <a name="related-links"></a>Linki pokrewne

- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Pierwszy wideo aplikacji czujki](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Porady WatchKit firmy Apple](https://developer.apple.com/watchkit/tips/)
