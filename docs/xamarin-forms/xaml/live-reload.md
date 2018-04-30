---
title: Załaduj ponownie na żywo
description: Zobacz zmiany Twojego XAML odzwierciedlone na żywo, bez konieczności innego kompilacji i wdrażania.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 04/23/2018
ms.openlocfilehash: bfb53af420b64fb9af994d3fb19293406d3acd7b
ms.sourcegitcommit: 180a8411d912de40545f9624e2127a66ee89e7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/30/2018
---
# <a name="xamarin-live-reload"></a>Załaduj ponownie na żywo Xamarin

![Wersja zapoznawcza](~/media/shared/preview.png)

Załaduj ponownie Xamarin na żywo umożliwia **dokonać zmian z XAML i one odzwierciedlone na żywo, bez konieczności innego kompilacji i wdrażania**. Wszelkie zmiany wprowadzone do Twojej XAML zostaną wdrożone na zapisywanie i odzwierciedlone na docelowym Wdróż.

Ponieważ aplikacja jest skompilowana używając przeładowania na żywo, działa ze wszystkimi bibliotek i innych kontrolek. Na wszystkich platformach platformy Xamarin.Forms obsługuje z systemami Android, iOS i platformy uniwersalnej systemu Windows i działa na wszystkie elementy docelowe prawidłowe wdrożenie, w tym symulatorów, emulatorów i fizyczne urządzenia na żywo działa Załaduj ponownie.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Załaduj ponownie na żywo jest obecnie dostępna tylko w programie Visual Studio 2017 r.

## <a name="requirements"></a>Wymagania

* [Visual Studio 2017 15.7 Preview 4](https://www.visualstudio.com/vs/preview/) lub nowszego z **Mobile development z platformą .NET** obciążenia.
* [3.0.354232-pre3 platformy Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3) lub nowszej.

## <a name="getting-started"></a>Wprowadzenie
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Zainstaluj na żywo Załaduj ponownie Xamarin z witryny Marketplace programu Visual Studio

Załaduj ponownie Xamarin na żywo jest dystrybuowane za pomocą programu Visual Studio Marketplace. Aby zainstalować rozszerzenie, odwiedź stronę [Xamarin Live Załaduj ponownie stronę w Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) witryny sieci Web i kliknij przycisk **Pobierz**.

Otwórz .vsix, zostanie pobrana, a następnie kliknij przycisk **zainstalować**.

![Instalator programu Visual Studio potwierdzenie Załaduj ponownie Xamarin na żywo](images/LiveReloadVSIXInstall.png)

Alternatywnie można wyszukiwać w **Online** karcie **rozszerzenia i aktualizacje** okna dialogowego w programie Visual Studio.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Skonfiguruj aplikację, aby użyć przeładowania na żywo

Dodawanie na żywo Załaduj ponownie do istniejącej aplikacji mobilnych można wykonać w trzy kroki:

1. Upewnij się, wszystkie projekty zostały zaktualizowane do używania [3.0.354232-pre3 platformy Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3) lub nowszej.
2. Zainstaluj **Xamarin.LiveReload** NuGet do biblioteki .NET 2.0 standardowa. Nie musi być zainstalowany w projektach platformy. Upewnij się, że **źródła pakietu** ustawiono **wszystkich**.

![Dodaj Xamarin na żywo Załaduj ponownie NuGet Menedżera pakietów NuGet](images/addlivereloadnuget.png)

3. Dodaj `LiveReload.Init();` do konstruktora w `Application` klasy, jak pokazano w poniższy fragment kodu:

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        LiveReload.Init();
    
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. Uruchom ponowne załadowanie na żywo

Skompiluj i wdróż aplikację. Gdy aplikacja zostanie wdrożone, otwórz plik XAML, wprowadzić kilka zmian i Zapisz plik. Zmiany są wdrażane ponownie w celu wdrożenia.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Na żywo działa Załaduj ponownie zmiany do dowolnego pliku XAML. Zmiany w języku C# lub dodawania/usuwania pakietów NuGet wymaga nowej kompilacji i wdrażania zaczęły obowiązywać.

## <a name="frequently-asked-questions"></a>Często zadawane pytania 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Załaduj ponownie Xamarin na żywo jest dostępna w programie Visual Studio dla komputerów Mac? 

W wersji zapoznawczej początkowej ponowne załadowanie Xamarin na żywo jest dostępna tylko dla programu Visual Studio 2017 r. Obsługa programu Visual Studio for Mac jest planowane w przyszłości.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Czy to współpracuje z wszystkie biblioteki, takich jak biblioteki Prism? 

Ponieważ aplikacja jest skompilowana, załaduj Live współpracuje z wszystkie biblioteki, takich jak biblioteki Prism i biblioteki formantów innych firm, takich jak strony firmy Telerik, Infragistics Syncfusion, ArcGIS, GrapeCity i innych dostawców kontroli.

### <a name="what-changes-does-live-reload-redeploy"></a>Jakie zmiany Live Załaduj ponownie wdrożyć? 

Załaduj ponownie na żywo dotyczy tylko zmiany wprowadzone w języku XAML. Jeśli wprowadzisz zmiany w pliku C# ponownej kompilacji będzie wymagane. Obsługa ponownego ładowania C# jest planowane w przyszłości.

### <a name="what-platforms-are-supported"></a>Jakie platformy są obsługiwane? 

Załaduj ponownie na żywo działa na żadnej platformy obsługiwanej przez platformy Xamarin.Forms, łącznie z systemami Android, iOS i platformy uniwersalnej systemu Windows.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>Jest to odpowiednie na emulatory, symulatorów i fizyczne urządzenia? 

Tak, załaduj Live współpracuje z wszystkie elementy docelowe prawidłowe wdrożenie, w tym Android emulatorów, symulatorów iOS i urządzenia fizycznego. Wdrożenia na urządzenie wymaga urządzeń i komputerów w tej samej sieci Wi-Fi.

### <a name="does-this-work-with-corporate-networks"></a>Czy to współpracuje z sieci firmowej?

Jeśli debugujesz emulatora systemu Android lub iOS simulator Live Załaduj ponownie używa localhost do komunikowania się. Jeśli chcesz wdrożyć na urządzeniu, na urządzeniu i na komputerze muszą być w tej samej sieci Wi-Fi. W scenariuszach, w którym nie jest to możliwe, można [skonfigurowanie serwera na żywo Załaduj ponownie](#live-reload-server), które umożliwią aby załadować ponownie na żywo, niezależnie od ustawienia łączności sieciowej.

### <a name="does-it-require-debugging-the-app"></a>Wymaga on debugowania aplikacji? 

Nie. W rzeczywistości nawet uruchomić na dowolnej liczbie urządzeń lub symulatorów/emulatory wszystkich obsługiwanych aplikacji celów (Android, iOS i platformy uniwersalnej systemu Windows) i wyświetlenia wszystkich aktualizacji na raz. 

## <a name="limitations"></a>Ograniczenia

* Obsługiwane jest tylko ponowne ładowanie języka XAML.
* Obsługiwane tylko w programie Visual Studio.
* Działa tylko z bibliotekami .NET Standard.
* Arkusze stylów CSS nie są obsługiwane.
* Stan interfejsu użytkownika może się nie powieść między wdraża ponownie, chyba że za pomocą MVVM.
* Ponowne ładowanie zasobów aplikacji firmy (tj. **App.xaml** lub udostępnionego słowniki zasobów), nawigacji aplikacji zostanie zresetowana.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

### <a name="app-doesnt-connect"></a>Nie connect aplikacji

Gdy aplikacji są wbudowane i informacji z **Narzędzia > Opcje > Xamarin > Live Załaduj ponownie** (host name, port i kluczy szyfrowania) są osadzone w aplikacji, tak że w przypadku `LiveReload.Init()` uruchamia parowania lub konfiguracja jest niezbędne do pomyślnego połączenia.

Inne niż normalne problemy z siecią (zapory i urządzenia w ramach innej sieci) główną przyczyną się, że aplikacja nie może pomyślnie połączyć IDE jest, ponieważ jego konfiguracja różni się od tego, w programie Visual Studio. Może się tak zdarzyć, jeśli:

* Aplikacji został skompilowany na innym komputerze.
* Aplikacja został skompilowany i wdrożone w innej sesji programu Visual Studio i **automatycznego generowania kluczy szyfrowania** jest zaewidencjonowany (ustawienie domyślne) **Narzędzia > Opcje > Xamarin > Live Załaduj ponownie**.
* Ustawienia programu Visual Studio zostały zmienione (tj. klucze nazwy hosta, port lub szyfrowania), ale aplikacja nie została zbudowana i wdrożyć ponownie.

Tych przypadkach można rozwiązać przy tworzeniu i wdrażaniu aplikacji ponownie.

### <a name="uninstalling-preview-1"></a>Odinstalowywanie Preview 1

Jeśli masz starszej wersji zapoznawczej i występują problemy z odinstalowywania go, wykonaj następujące kroki:

1. Usuń folder **\Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload C:\Program Files (x86)** (Uwaga: Zastąp "Przedsiębiorstwa" posiadanej wersji zainstalowanego i "Wersja zapoznawcza" z "2017" Jeśli użytkownik zainstalowana tak, aby stabilna VS)
2. Otwórz **wiersza polecenia dewelopera** dla tego programu Visual Studio i uruchomić `devenv /updateconfiguration`. 

## <a name="tips--tricks"></a>Porady i wskazówki

* Tak długo, jak nie należy zmieniać ustawień na żywo Załaduj ponownie (w tym klucze szyfrowania, np. Jeśli wyłączysz **automatycznego generowania kluczy szyfrowania**) i kompilacji z tym samym komputerze, nie potrzebujesz do tworzenia i wdrażania aplikacji po początkowej wdrażanie, chyba że zostanie zmienione, kodu lub zależności. Wystarczy uruchomić je ponownie wdrożonej wcześniej aplikacji i będzie się łączyć do ostatnio używane hosta.

* Bez ograniczeń, na ile urządzenia możesz nawiązać połączenie tej samej sesji programu Visual Studio nie istnieje. Możesz wdrożyć i uruchomić aplikację w dowolnej liczby urządzeń/symulatorów w razie potrzeby, aby wyświetlić na żywo pracy przeładunku na wszystkich z nich w tym samym czasie.

* Załaduj ponownie na żywo spowoduje ponowne załadowanie tylko część interfejsu użytkownika aplikacji, ale go nie *nie* ponowne tworzenie stron, ani go zamienić modelu widoku (lub kontekstu powiązania). Oznacza to, że *całego* stan aplikacji zawsze są zachowywane między przeładowania, łącznie z wprowadzonym zależności.

## <a name="live-reload-server"></a>Załaduj ponownie serwer na żywo

W scenariuszach, w przypadku, gdy połączenie z aplikacji uruchomionych na komputerze (oznaczony za pomocą `localhost` lub `127.0.0.1` w **Narzędzia > Opcje > Xamarin > Live Załaduj ponownie**) nie jest możliwe (tj. zapór, różnych sieciach), można skonfigurować serwera zdalnego, który zarówno IDE i aplikacja zostanie połączyć do.

Załaduj ponownie na żywo korzysta ze standardu [protokołu MQTT](http://mqtt.org/) do wymiany wiadomości i w związku z tym może komunikować się z [serwerów innych firm](https://github.com/mqtt/mqtt.github.io/wiki/servers). Istnieje nawet [publicznych serwerów](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (znanej także jako *brokerzy*) dostępne, których można używać. Załaduj ponownie na żywo był testowany z `broker.hivemq.com` i `iot.eclipse.org` nazwy hostów, a także usług świadczonych przez [www.cloudmqtt.com](https://www.cloudmqtt.com) i [www.cloudamqp.com](https://www.cloudamqp.com). Można także wdrożyć własny serwer MQTT w chmurze, takich jak [HiveMQ na platformie Azure](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud) lub [MQ króliczego na usług AWS](http://www.rabbitmq.com/ec2.html). 

Można skonfigurować dowolnego portu, ale często jest używany jest domyślny port 1883 dotyczących serwerów zdalnych. Na żywo wiadomości Załaduj ponownie użyj silnego end-to-end AES szyfrowania symetrycznego, więc bezpiecznie nawiązać połączenia z serwerami zdalnymi. Domyślnie zarówno klucz szyfrowania i wektor inicjowania (IV) są generowane w każdej sesji programu Visual Studio.

Prawdopodobnie Najprostszym sposobem jest zainstalowanie [mosquitto](https://mosquitto.org) serwera w puste maszyny Wirtualnej systemu Ubuntu na platformie Azure:

1. Tworzenie nowej maszyny Wirtualnej systemu Ubuntu Server w portalu Azure
2. Dodaj nową regułę portu wejściowego dla 1883 (domyślny port MQTT) na karcie sieci
3. Otwórz [powłoki chmury](https://docs.microsoft.com/azure/cloud-shell/overview) (tryb bash)
4. Wpisz `ssh [USERNAME]@[PUBLIC_IP]` przy użyciu nazwy użytkownika została wybrana opcja 1) i publiczny adres IP wyświetlany na stronie Omówienie maszyny Wirtualnej
5. Uruchom `sudo apt-get install mosquitto`, wprowadzenie hasła w 1)

Teraz można tego adresu IP połączenie z serwerem MQTT.
