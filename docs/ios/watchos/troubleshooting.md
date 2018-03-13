---
title: "watchOS Rozwiązywanie problemów"
description: "Znane problemy i rozwiązania problemów programowanie watchOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: ce850b7890265b82774534ca0daaf25bed7e0c2d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="watchos-troubleshooting"></a>watchOS Rozwiązywanie problemów

Ta strona zawiera dodatkowe informacje i obejścia funkcji nadal w fazie tworzenia. Niektóre rozwiązania te dotyczą tylko naszych wersji preview.

- [Znane problemy](#knownissues)

- [Usuwanie kanału alfa z obrazów ikony](#noalpha)

- [Ręczne dodanie plików kontrolera interfejsu](#add) dla konstruktora interfejsu Xcode.

- [Uruchamianie WatchApp z wiersza polecenia](#command_line).

<a name="knownissues" />

## <a name="known-issues"></a>Znane problemy

### <a name="general"></a>Ogólne

<a name="deploy" />
<!--
* You cannot deploy to the App Store *from within Visual Studio for Mac or Visual Studio*
    in the current release. You should create an **Archive** in Visual Studio for Mac
    and then switch to Xcode to upload the archive to iTunes Connect. Visual Studio
    is not currently supported (but will be a future release). Refer to the
    [deployment guide](~/ios/watchos/deploy-test/appstore.md) for more information.
-->

- Wcześniejszych wersjach programu Visual Studio for Mac niepoprawnie Pokaż jedną z **AppleCompanionSettings** ikony jako pikseli 88 x 88; co skutkuje **Brak ikony błędu** przy próbie przesłać do aplikacji Magazyn.
    Ikona powinny być 87 x 87 pikseli (29 jednostki dla  **@3x**  siatkówki ekrany). Nie można rozwiązać ten problem w programie Visual Studio dla komputerów Mac — albo zasób obrazu w środowisku Xcode edycji lub ręcznie edytować **Contents.json** plików (do dopasowania [w tym przykładzie](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- Jeśli projekt rozszerzenia czujki **Info.plist > identyfikator pakietu WKApp** nie jest [poprawnie ustawić](~/ios/watchos/get-started/project-references.md) odpowiadające aplikacji czujki **identyfikator pakietu**, debuger będą mogły nawiązać połączenia i Visual Studio for Mac będzie czekać z komunikatem *"Oczekiwanie na debugera połączyć"*.

- Debugowanie jest obsługiwane w **powiadomienia** tryb, ale mogą być nieprawidłowe. Ponawianie próby czasami będzie działać. Upewnij się, że aplikacja czujki **Info.plist** `WKCompanionAppBundleIdentifier` ma ustawioną wartość odpowiada identyfikator pakietu aplikacji systemu iOS/kontenera (ie. ten, który jest uruchamiany na telefonie iPhone).

<!--
- **Can't launch application on Watch simulator.** This seems to
    be an issue with the iOS Simulator hanging when trying to
    install an app that has changed. Xcode release notes (beta 4)
    includes a similar known issue:
    If the issue persists, reset the Simulator (**iOS Simulator > Reset Content and Settings...**).
-->

- iOS projektanta nie są wyświetlane strzałki punktu wejścia dla kontrolerów interfejsu oka lub powiadomienia.

- Nie można dodać dwóch `WKNotificationControllers` do scenorysu.
    Obejście problemu: `notificationCategory` element scenorysu XML jest zawsze wstawiany o tej samej `id`. Aby obejść ten problem, możesz dodać kontrolerów powiadomienia (co najmniej dwa), otwórz plik scenorysu w edytorze tekstów i ręcznie zmienić `id` element ma być unikatowy.

    [![](troubleshooting-images/duplicate-id-sml.png "Otwieranie scenorysu plik w edytorze tekstów i ręcznie zmienić elementu id być unikatowe")](troubleshooting-images/duplicate-id.png#lightbox)

- Może zostać wyświetlony błąd "nie został skompilowany aplikacji" podczas próby uruchomienia aplikacji. Występuje to po **wyczyść** gdy projekt startowy jest ustawiona na projekt rozszerzenia czujki.
    Ta poprawka jest wybranie **kompilacji > Skompiluj ponownie wszystko** , a następnie ponownie uruchom aplikację.

### <a name="visual-studio"></a>Visual Studio

Obsługa systemu iOS projektanta dla zestawu czujki *wymaga* rozwiązania, które są skonfigurowane poprawnie. Jeśli nie ustawiono odwołania do projektu (zobacz [jak ustawić odwołania](~/ios/watchos/get-started/project-references.md)) powierzchni projektu nie będzie działał prawidłowo, a następnie.

<!--
* New Watch Kit apps created in Visual Studio might not allow
    starting in Notifications mode.

* You cannot deploy to the App Store from Visual Studio (see [notes above](#deploy)
    and the [deployment guide](~/ios/watchos/deploy-test/appstore.md)). Use
    Visual Studio for Mac and Xcode on your Mac Build Host.
    -->

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Usuwanie kanału alfa z obrazów ikony

Ikony nie powinna zawierać kanału alfa (przezroczystych obszarów obrazu definiuje kanału alfa), w przeciwnym razie aplikacja zostanie odrzucone podczas przesyłania sklepu z aplikacjami ze względu na błąd podobny do poniższego:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Łatwo można usunąć kanału alfa przy użyciu systemu Mac OS X **Podgląd** aplikacji:

1. Otwórz obrazu ikony w **Podgląd** , a następnie wybierz **Plik > Eksportuj**.

2. Zostaną uwzględnione w wyświetlonym oknie dialogowym **alfa** pole wyboru, jeśli jest obecny kanał alfa.

    ![](troubleshooting-images/remove-alpha-sml.png "Wyświetlane okno dialogowe będzie zawierać alfa pole wyboru, jeśli występuje kanału alfa")

3. *Untick* **alfa** wyboru i **zapisać** pliku do poprawnej lokalizacji.

4. Obraz ikony powinny teraz weryfikacja zakończy się firmy Apple.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Ręczne dodanie plików kontrolera interfejsu

> [!IMPORTANT]
> Obsługa zestawu czujki platformy Xamarin w obejmuje projektowanie scenorys czujki w Projektancie iOS (zarówno w programie Visual Studio dla komputerów Mac i Visual Studio), które nie wymagają kroki opisane poniżej. Po prostu nazwę kontrolera interfejsu klasy w Visual Studio for Mac właściwości konsoli i kodu C#, które pliki zostaną utworzone automatycznie.


*Jeśli* używasz konstruktora interfejsu Xcode, wykonaj następujące kroki, aby utworzyć nowe kontrolery interfejsu aplikacji czujki i Włącz synchronizację z Xcode, dzięki czemu gniazda i akcji dostępnych w języku C#:


1. Otwórz aplikację czujki **Interface.storyboard** w **Xcode interfejsu konstruktora**.
    
    ![](troubleshooting-images/add-6.png "Otwieranie scenorysu w Konstruktorze interfejsu Xcode")

2. Przeciągnij nowy `InterfaceController` na scenorysu:

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. Możesz teraz przeciągnąć formantów na kontroler interfejsu (np.) etykiety i przyciski), ale nie można utworzyć gniazda lub akcje jeszcze, ponieważ nie istnieje żadne **.h** pliku nagłówka. Poniższe kroki spowoduje, że wymagane **.h** nagłówka pliku ma zostać utworzony.

    ![](troubleshooting-images/add-2.png "Przycisk w układzie")

4. Zamknij scenorys i powrócić do programu Visual Studio dla komputerów Mac. Utwórz nowy plik C# **MyInterfaceController.cs** (lub nazwa chcesz) w **Obejrzyj rozszerzenia aplikacji** projektu (nie czujki samej aplikacji w przypadku scenorysu). Dodaj następujący kod (aktualizowanie przestrzeni nazw, klasy i nazwa konstruktora):

        using System;
        using WatchKit;
        using Foundation;
        
        namespace WatchAppExtension  // remember to update this
        {
            public partial class MyInterfaceController // remember to update this
            : WKInterfaceController
            {
                public MyInterfaceController // remember to update this
                (IntPtr handle) : base (handle)
                {
                }
                public override void Awake (NSObject context)
                {
                    base.Awake (context);
                    // Configure interface objects here.
                    Console.WriteLine ("{0} awake with context", this);
                }
                public override void WillActivate ()
                {
                    // This method is called when the watch view controller is about to be visible to the user.
                    Console.WriteLine ("{0} will activate", this);
                }
                public override void DidDeactivate ()
                {
                    // This method is called when the watch view controller is no longer visible to the user.
                    Console.WriteLine ("{0} did deactivate", this);
                }
            }
        }

5. Utwórz nowy C# plik **MyInterfaceController.designer.cs** w **Obejrzyj rozszerzenia aplikacji** projektu i Dodaj poniższy kod. Pamiętaj zaktualizować przestrzeni nazw, klasy i `Register` atrybutu:

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;
    
    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```
    
    Porada: Możesz (opcjonalnie) wprowadzić ten plik z elementem podrzędnym pierwszy plik przeciągając go do innego pliku języka C# w programie Visual Studio Mac rozwiązania konsoli. Następnie pojawi się następująco:
    
    ![](troubleshooting-images/add-5.png "Konsola rozwiązania")

6. Wybierz **kompilacji > kompilacji wszystkich** tak, aby synchronizacja Xcode rozpozna nową klasę (za pośrednictwem `Register` atrybut) użytymi.

7. Otwórz ponownie scenorysu prawym przyciskiem myszy plik scenorysu czujki aplikacji i wybierając **Otwórz za pomocą > konstruktora interfejsu Xcode**:

    ![](troubleshooting-images/add-6.png "Otwieranie scenorysu w konstruktora interfejsu")

8. Wybierz nowy kontroler interfejsu i nadaj mu classname, zdefiniowanych powyżej, np. `MyInterfaceController`.
Jeśli wszystko działał prawidłowo, powinien pojawiać się automatycznie w **klasy:** listy rozwijanej i możesz wybrać je stamtąd.

    ![](troubleshooting-images/add-4.png "Ustawienie niestandardowej klasy")

9. Wybierz **Edytor Asystenta** wyświetlić w środowisku Xcode (ikona z dwie nakładające się okręgi), dzięki czemu można zobaczyć scenorysu i kod side-by-side:

    ![](troubleshooting-images/add-7.png "Element paska narzędzi edytora Asystenta")

    Gdy fokus jest w okienku kodu, upewnij się, w przypadku przeglądania **.h** pliku nagłówka, a jeśli nie prawym przyciskiem myszy pasek nawigacją i wybrać poprawny plik (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "Wybierz MyInterfaceController")

10. Teraz można utworzyć gniazda i działań przez **Ctrl i przeciągnij** z scenorysu do **.h** pliku nagłówka.

    ![](troubleshooting-images/add-9.png "Tworzenie punktów i akcji")

    Po zwolnieniu przeciągania, pojawi się monit o wybierz, czy można utworzyć gniazda lub akcję, a jego nazwa:

    ![](troubleshooting-images/add-a.png "Okno dialogowe akcji i gniazda")

11. Po scenorysu zmiany zostały zapisane i Xcode jest zamknięty, zwróć się do programu Visual Studio dla komputerów Mac. Będzie wykrywał zmiany pliku nagłówka i automatycznie Dodaj kod, aby **. Designer.cs narzędzie** pliku:


        [Register ("MyInterfaceController")]
        partial class MyInterfaceController
        {
            [Outlet]
            WatchKit.WKInterfaceButton myButton { get; set; }
        
            void ReleaseDesignerOutlets ()
            {
                if (myButton != null) {
                    myButton.Dispose ();
                    myButton = null;
                }
            }
        }


Teraz można odwołać formantu (lub wykonania akcji) w języku C#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Uruchomienie aplikacji czujki z wiersza polecenia

> [!IMPORTANT]
> Obejrzyj aplikacji można uruchomić w trybie normalnym aplikacji domyślnie, a także w **Przegląd** lub **powiadomień** trybów przy użyciu [wykonywanie niestandardowe parametry](~/ios/watchos/get-started/installation.md#custommodes) w programie Visual Studio dla komputerów Mac i Program Visual Studio.


Umożliwia także wiersza polecenia do sterowania symulatora systemu iOS. To narzędzie wiersza polecenia używane do uruchomienia aplikacji czujki **mtouch**.

Oto pełny przykład (wykonane jako pojedynczy wiersz w terminalu):

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Parametr należy zaktualizować w celu uwzględnienia aplikacji jest `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Pełna ścieżka do pakietu aplikacji głównej *aplikacji systemu iOS, który zawiera wyrażenie kontrolne aplikacji i rozszerzenia*.

> [!NOTE]
> *Uwaga:* należy podać dotyczy *pliku .app aplikacji iPhone*, tj. jeden który będzie wdrażany w symulatorze systemu iOS i zawiera rozszerzenia czujki i obejrzyj aplikacji.

Przykład:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>Tryb powiadomień

Aby przetestować aplikację [ **powiadomień** tryb](~/ios/watchos/platform/notifications.md), ustaw `watchlaunchmode` parametr `Notification` i podać ścieżkę do pliku JSON, który zawiera ładunek powiadomienia testu.

Parametr ładunku jest *wymagane* dla trybu powiadomień.

Polecenie mtouch, na przykład dodać następujące argumenty:

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>Inne argumenty

Poniżej opisano pozostałe argumenty:

### <a name="--sdkroot"></a>--sdkroot

Wymagany. Określa ścieżkę do Xcode (6.2 lub nowszej).

Przykład:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--urządzenia

Symulator urządzenia do wykonania. Można wybrać opcję na dwa sposoby, za pomocą udid konkretnego urządzenia albo przy użyciu kombinacji środowiska uruchomieniowego i typ urządzenia.

Dokładne wartości różnie w przypadku maszyn i mogą być przeszukiwane przy użyciu firmy Apple **simctl** narzędzie:

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

Przykład:

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Środowisko uruchomieniowe i typ urządzenia**

Przykład:

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```



## <a name="related-links"></a>Linki pokrewne

- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
