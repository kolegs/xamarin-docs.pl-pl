---
title: Ponowne ładowanie na żywo
description: Zobacz zmiany użytkownika XAML odzwierciedlone na żywo, bez konieczności innej kompilacji i wdrażania.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 05/11/2018
ms.openlocfilehash: 12b677c8cc4a709a865d2eaee3ea44a6babf1b05
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38860670"
---
# <a name="xamarin-live-reload"></a>Ponowne ładowanie na żywo platformy Xamarin

![Wersja zapoznawcza](~/media/shared/preview.png)

Ponowne ładowanie na żywo w środowisku Xamarin umożliwia **wprowadzić zmiany w swojej XAML i ich odzwierciedlone na żywo, bez kompilacji innej oraz wdrażanie**. Wszelkie zmiany wprowadzone do Twojej XAML będzie ponownie wdrażana na zapisanie i odzwierciedlone na urządzenie docelowe wdrażania.

Ponieważ aplikacja jest kompilowana, korzystając z Reload na żywo, działa ze wszystkimi bibliotekami i formanty innych firm. Załaduj ponownie działa na żywo na wszystkich platformach platformy Xamarin.Forms obsługuje, w tym systemów Android, iOS i platformy uniwersalnej systemu Windows i działa na wszystkie elementy docelowe prawidłowe wdrożenie, w tym symulatorów, emulatorów i urządzeń fizycznych.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Ponowne ładowanie na żywo jest obecnie dostępna tylko w programie Visual Studio 2017.

## <a name="requirements"></a>Wymagania

* [Visual Studio 2017 w wersji 15.7 lub nowszej](https://visualstudio.microsoft.com/vs/) lub nowszy z **opracowywania aplikacji mobilnych przy użyciu platformy .NET** obciążenia.
* [Zestaw narzędzi Xamarin.Forms 3.0.0 lub nowszej](https://www.nuget.org/packages/Xamarin.Forms/) lub nowszej.

## <a name="getting-started"></a>Wprowadzenie
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Zainstaluj ponowne ładowanie na żywo platformy Xamarin z witryny Marketplace programu Visual Studio

Ponowne ładowanie na żywo w środowisku Xamarin jest dystrybuowane za pośrednictwem witryny Marketplace programu Visual Studio. Aby zainstalować rozszerzenie, odwiedź stronę [Xamarin Live Załaduj ponownie stronę w witrynie Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) witryny sieci Web i kliknij przycisk **Pobierz**.

Otwórz .vsix, który jest pobierany, a następnie kliknij przycisk **zainstalować**.

![Instalator programu Visual Studio potwierdzenie ponowne ładowanie na żywo w środowisku Xamarin](images/LiveReloadVSIXInstall.png)

Alternatywnie, możesz wyszukać ją w **Online** karcie **rozszerzenia i aktualizacje** okna dialogowego w programie Visual Studio.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Konfigurowanie aplikacji do użycia na żywo Załaduj ponownie

Dodawanie na żywo Załaduj ponownie do istniejących aplikacji mobilnych może odbywać się w trzech krokach:

1. Upewnij się, wszystkie projekty zostały zaktualizowane pod kątem użycia [Xamarin.Forms 3.0.0 lub nowszej](https://www.nuget.org/packages/Xamarin.Forms/) lub nowszej.

2. Dodaj **Xamarin.LiveReload** pakietu NuGet:

    a. **.NET standard** — instalowanie **Xamarin.LiveReload** NuGet do biblioteki programu .NET Standard 2.0. Nie musi być zainstalowany w projektach platformy. Upewnij się, że **źródła pakietu** ustawiono **wszystkich**.
    
    b. **Projekty udostępnione** — instalowanie **Xamarin.LiveReload** NuGet do wszystkich projektów platformy (takich jak Android, iOS, platformy uniwersalnej systemu Windows, itd.). Upewnij się, że **źródła pakietu** ustawiono **wszystkich**.

    [![Dodaj pakiet NuGet ponowne ładowanie na żywo platformy Xamarin przy użyciu Menedżera pakietów NuGet](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. Dodaj `LiveReload.Init();` do konstruktora w `Application` klasy, jak pokazano w poniższym fragmencie kodu:

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        #if DEBUG
        LiveReload.Init();
        #endif
        
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Uruchom ponowne ładowanie na żywo

Skompiluj i wdróż aplikację. Gdy aplikacja jest wdrożona, otwórz plik XAML, wprowadzić pewne zmiany, a następnie zapisz plik. Zmiany są ponownie wdrożyć cel wdrożenia.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Załaduj ponownie działania wprowadzania zmian do pliku XAML na żywo. Zmiany w języku C# lub dodawania/usuwania pakietów NuGet wymaga nowej kompilacji i wdrażania zaczęły obowiązywać.

## <a name="frequently-asked-questions"></a>Często zadawane pytania 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Ponowne ładowanie na żywo w środowisku Xamarin jest dostępna w programie Visual Studio dla komputerów Mac? 

Wersja zapoznawcza początkowej ponowne ładowanie na żywo w środowisku Xamarin jest dostępna tylko dla programu Visual Studio 2017. Obsługa programu Visual Studio dla komputerów Mac jest planowana w przyszłej wersji.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>To działa przy użyciu wszystkich bibliotek, takich jak biblioteki Prism? 

Ponieważ aplikacja jest kompilowana, na żywo Załaduj ponownie współpracuje ze wszystkich bibliotek, takich jak biblioteki Prism i bibliotek kontrolek innych firm, takich jak Telerik, Infragistics, Syncfusion ArcGIS, GrapeCity i innych dostawców kontroli.

### <a name="what-changes-does-live-reload-redeploy"></a>Jakie zmiany na żywo Załaduj ponownie Wdróż ponownie? 

Ponowne ładowanie na żywo ma zastosowanie tylko zmiany wprowadzone w XAML lub CSS. Jeśli wprowadzisz zmiany w pliku C#, wymagane będzie ponownej kompilacji. Obsługa ponownego ładowania C# jest planowana w przyszłej wersji.

### <a name="what-platforms-are-supported"></a>Jakie platformy są obsługiwane? 

Ponowne ładowanie na żywo działa na dowolnej platformie, obsługiwana przez platformy Xamarin.Forms, w tym systemów Android, iOS i platformy uniwersalnej systemu Windows.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>To działa na emulatorów, symulatorach i urządzeniach fizycznych? 

Tak, na żywo Załaduj ponownie współpracuje z wszystkie elementy docelowe prawidłowe wdrożenie, w tym emulatorów systemu Android, iOS symulatorów i urządzeń fizycznych. Wdrażanie na urządzeniu wymaga urządzeń i komputerów w tej samej sieci Wi-Fi.

### <a name="does-this-work-with-corporate-networks"></a>To działa w sieciach firmowych?

Jeśli debugujesz z emulatora systemu Android lub symulatora systemu iOS, na żywo Załaduj ponownie używa localhost do komunikacji. Jeśli chcesz wdrożyć na urządzeniu, na urządzeniu i na komputerze muszą być w tej samej sieci Wi-Fi. W scenariuszach, w którym nie jest to możliwe, możesz [skonfigurowanie serwera na żywo Załaduj ponownie](#live-reload-server), która pozwoli na ponowne ładowanie na żywo, niezależnie od ustawienia łączności sieciowej.

### <a name="does-it-require-debugging-the-app"></a>Wymaga on debugowania aplikacji? 

Nie. W rzeczywistości można nawet uruchomić wszystkich obsługiwanych aplikacji elementy docelowe (Android, iOS i platformy uniwersalnej systemu Windows) na dowolnej liczbie urządzeń i symulatorów/emulatorów i wyświetlenia ich wszystkich jednocześnie uaktualnić. 

## <a name="limitations"></a>Ograniczenia

* Obsługiwane jest tylko ponowne załadowanie z XAML.
* Stan interfejsu użytkownika może się nie powieść między wdraża ponownie, chyba że korzysta z modelem MVVM.

## <a name="known-issues"></a>Znane problemy

* Obsługiwane tylko w programie Visual Studio.
* Połączenie musi być równa **nie łącz** lub **tylko łącze Framework SDK** 
* Ponowne ładowanie zasobów aplikacji (czyli **App.xaml** lub udostępnione słowniki zasobów), aplikacja nawigacji jest resetowany. Ten problem zostanie rozwiązany w następnej wersji (wersja zapoznawcza).
* Edytowanie XAML podczas debugowania platformy uniwersalnej systemu Windows może spowodować awarię środowiska uruchomieniowego. Obejście: Użyj **Uruchom bez debugowania (Ctrl + F5)** zamiast **Rozpocznij debugowanie (F5)**.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="error-codes"></a>Kody błędów

* **XLR001**: *bieżący projekt odwołuje się do pakietu NuGet "Xamarin.LiveReload" w wersji [wersja], ale rozszerzenie ponowne ładowanie na żywo w środowisku Xamarin wymaga wersji [wersja].*

  Aby umożliwić szybkie iteracji i rozwój funkcji Live Załaduj ponownie, pakiet nuget i rozszerzenia programu Visual Studio musi dokładnie pasować. Zaktualizuj pakiet nuget do tej samej wersji rozszerzenia, które zostały zainstalowane.

* **XLR002**: *Reload na żywo wymaga co najmniej właściwość "MqttHostname" podczas kompilowania z wiersza polecenia. Alternatywnie można ustawić "EnableLiveReload" na "false", aby wyłączyć funkcję.*

  Właściwości wymagane przez Reload na żywo nie są dostępne podczas kompilowania z wiersza polecenia (lub ciągłej integracji) i dlatego musi być podana w sposób jawny. 

* **XLR003**: *pakietu nuget na żywo Załaduj ponownie wymaga instalacji Xamarin Live ponowne załadowanie rozszerzenia programu Visual Studio.*

  Próba skompilowania projektu, która odwołuje się na żywo ponowne załadowanie pakietu nuget, ale nie zainstalowano rozszerzenia Visual.  

* *Wyjątek podczas ładowania zestawów: System.IO.FileNotFoundException: nie można załadować zestawu "Xamarin.Live.Reload, Version = 0.3.27.0, Culture = neutral, PublicKeyToken =".*

  Projekt hosta powinien używać `PackageReference` zamiast `packages.config`

### <a name="app-doesnt-connect"></a>Połączenie nie zostanie nawiązane aplikacji

Podczas kompilowania aplikacji, informacji z **Narzędzia > Opcje > Xamarin > Live Załaduj ponownie** (nazwy hosta, portu i szyfrowania kluczy) są osadzone w aplikacji, dlatego że w przypadku `LiveReload.Init()` uruchamia parowania lub konfiguracja jest wymagane dla połączenia zakończyło się sukcesem.

Inne niż normalny problemy z siecią (zapory, urządzenia, które w innej sieci) głównym powodem, że aplikacja nie może pomyślnie połączyć IDE jest, ponieważ jego konfiguracja różni się od tego, w programie Visual Studio. Może się to zdarzyć, jeśli:

* Aplikacja został skompilowany na innym komputerze.
* Aplikacji został skompilowany i wdrażane w innej sesji programu Visual Studio i **automatyczne generowanie kluczy szyfrowania** jest zaznaczone (ustawienie domyślne) **Narzędzia > Opcje > Xamarin > Live Załaduj ponownie**.
* Ustawienia programu Visual Studio zostały zmienione (np. klucze nazwy hosta, portu lub szyfrowania), ale aplikacja nie została zbudowana i ponownie wdrożyć.

Te przypadki są rozwiązać przez tworzenie i wdrażanie jej ponownie.

### <a name="uninstalling-preview-1"></a>Odinstalowywanie wersji zapoznawczej 1

Jeśli masz starszą (wersja zapoznawcza) i masz problemy z jego odinstalowanie, wykonaj następujące kroki:

1. Usuń folder **\Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload C:\Program Files (x86)** (Uwaga: Zastąp "Enterprise" swoją wersję zainstalowanego i "Wersja zapoznawcza" z "2017", jeśli użytkownik zainstalowane do stabilne programu VS)
2. Otwórz **wiersz polecenia dla deweloperów** do tego programu Visual Studio i wykonywania `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>Tipy a triky

* Tak długo, jak nie należy zmieniać ustawień na żywo Reload (łącznie z kluczami szyfrowania, np. Jeśli wyłączysz **automatyczne generowanie kluczy szyfrowania**) i kompilujesz z tym samym komputerze, nie potrzebujesz do tworzenia i wdrażania aplikacji po początkowej wdrażanie, chyba że zmienił się kodu lub zależności. Możesz po prostu uruchomić ponownie wdrożonej wcześniej aplikacji i będzie się łączyć z ostatnim hosta używane.

* Nie ma ograniczeń dotyczących na liczbę urządzeń, możesz nawiązać połączenie tej samej sesji programu Visual Studio. Można wdrożyć i uruchomić aplikację w dowolnej liczby urządzeń/symulatorów zgodnie z potrzebami, aby wyświetlić na żywo ponownego ładowania pracę na wszystkich z nich w tym samym czasie.

* Ponowne ładowanie na żywo spowoduje ponowne załadowanie tylko część interfejsu użytkownika aplikacji, ale go nie *nie* ponowne tworzenie stron, ani go zastąpić model widoku (lub kontekstu powiązania). Oznacza to, że *całego* stan aplikacji jest zawsze zachowywane między ponownie ładuje, w tym wprowadzonego zależności.

## <a name="live-reload-server"></a>Ponowne ładowanie na żywo serwera

W scenariuszach w przypadku, gdy połączenie z aplikacji uruchomionej na komputerze (oznaczony za pomocą `localhost` lub `127.0.0.1` w **Narzędzia > Opcje > Xamarin > Live Załaduj ponownie**) nie jest możliwe (czyli zapór, różnych sieci) można skonfigurować serwera zdalnego zamiast tego, który IDE i aplikacja zostaną połączyć do.

Ponowne ładowanie na żywo używa standardu [protokołu MQTT](http://mqtt.org/) do wymiany wiadomości, a w związku z tym może komunikować się z [serwerów stron trzecich](https://github.com/mqtt/mqtt.github.io/wiki/servers). Istnieją jeszcze [publiczne serwery](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (znany także jako *brokerów*) dostępny, których można używać. Ponowne ładowanie na żywo był testowany z `broker.hivemq.com` i `iot.eclipse.org` nazw hostów, a także usług świadczonych przez [www.cloudmqtt.com](https://www.cloudmqtt.com) i [www.cloudamqp.com](https://www.cloudamqp.com). Można także wdrożyć własny serwer MQTT w chmurze, takich jak [HiveMQ na platformie Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud).

Można skonfigurować dowolnego portu, ale często jest domyślnym portem 1883 na użytek serwerów zdalnych. Ponowne ładowanie, wiadomości za pomocą silnych end-to-end symetrycznego szyfrowania AES, aby bezpiecznie połączyć się z serwerami zdalnymi na żywo. Domyślnie zarówno klucz szyfrowania i wektor inicjowania (IV) są generowane w każdej sesji programu Visual Studio.

Prawdopodobnie Najprostszym sposobem jest zainstalowanie [mosquitto](https://mosquitto.org) serwera w pustej maszyny Wirtualnej systemu Ubuntu na platformie Azure:

1. Utwórz nową maszynę Wirtualną Ubuntu Server w witrynie Azure Portal
2. Dodawanie nowej reguły portów ruchu przychodzącego dla 1883 (domyślny port MQTT) na karcie sieci
3. Otwórz [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (powłoki bash tryb)
4. Wpisz `ssh [USERNAME]@[PUBLIC_IP]` przy użyciu nazwy użytkownika została wybrana opcja 1) i publiczny adres IP, wyświetlany na stronie Omówienie maszyny Wirtualnej
5. Uruchom `sudo apt-get install mosquitto`, wprowadzenie hasła w 1)

Teraz można użyć tego adresu IP nawiązać połączenia z serwerem MQTT.
