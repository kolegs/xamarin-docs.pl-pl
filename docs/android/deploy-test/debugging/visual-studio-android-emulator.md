---
title: Visual Studio Emulator systemu Android
description: W tym przewodniku opisano sposób konfigurowania i korzystania z programu Visual Studio Emulator systemu Android umożliwiające tworzenie aplikacji platformy Xamarin.Android w programie Visual Studio 2015.
ms.topic: article
ms.prod: xamarin
ms.assetid: CD128CB9-499F-4558-B49F-77248824EFDF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2018
ms.openlocfilehash: 49ae82e1d2b72d7cdebd3ab91833be88a3e95424
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/03/2018
---
# <a name="visual-studio-android-emulator"></a>Visual Studio Emulator systemu Android

_W tym przewodniku opisano sposób konfigurowania i korzystania z programu Visual Studio Emulator systemu Android umożliwiające tworzenie aplikacji platformy Xamarin.Android w programie Visual Studio 2015._

## <a name="visual-studio-android-emulator-overview"></a>Omówienie systemu Android Emulator programu Visual Studio

Microsoft Visual Studio 2015 obejmuje emulatorze systemu Android, który może służyć jako miejsce docelowe do debugowania aplikacji platformy Xamarin.Android: *programu Visual Studio Emulator for Android*. To emulator używa funkcji Hyper-V na komputerze programowanie, co zapewnia szybsze uruchamianie i czasie wykonywania niż emulatorem domyślnym dołączoną do zestawu SDK systemu Android. Visual Studio Emulator for Android może służyć jako alternatywę do emulatora systemu Android SDK domyślne podczas tworzenia aplikacji platformy Xamarin.Android.

> [!NOTE]
> Visual Studio Emulator systemu Android jest zgodna tylko z programu Visual Studio 2015 &ndash; nie działa z programu Visual Studio 2017 r.

W tym przewodniku wyjaśniono, jak można uruchomić emulatora Android firmy Microsoft w programie Visual Studio, aby przetestować aplikację, a także opisano różne funkcje dostępne w emulatorze. Dowiesz się jak wybrać *profilów urządzeń* (podobnie jak definicje urządzenia w emulatorze domyślny zestaw SDK systemu Android) można symulować różne typy urządzeń z systemem Android. Na koniec sekcji dotyczącej rozwiązywania problemów wyjaśniono typowych problemów i rozwiązania problemu.

## <a name="requirements"></a>Wymagania

Aby uruchomić emulatora, komputer musi spełniać wymagań dotyczących uruchamiania funkcji Hyper-V. Funkcja Hyper-V wymaga 64-bitowej wersji systemu Windows 8, Windows 8.1, Windows 10 lub nowszej wersji Pro. Aby uzyskać więcej informacji na temat wymagań, zobacz [wymagania systemowe dla programu Visual Studio Emulator for Android](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

> [!NOTE]
> Nie można użyć HAXM (wykorzystywane przez emulatora Android SDK) po włączeniu funkcji Hyper-V. Aby uzyskać więcej informacji dotyczących ograniczenia i potencjalne problemy z HAXM, zobacz [konflikty wirtualizacji HAXM](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#virt-conflicts).


## <a name="running-the-emulator"></a>Uruchomiony Emulator

Visual Studio udostępnia kilka profilów wstępnie skonfigurowane urządzenia docelowego w **docelowego debugowania** menu rozwijanego (jako widoczna w poniższym zrzucie ekranu). Obiekty docelowe emulatora systemu Android firmy Microsoft są poprzedzone znakiem **emulatora VS**:

[![Profile urządzeń docelowych wstępnie skonfigurowane](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs-sml.png)](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs.png#lightbox)

Po uruchomieniu aplikacji platformy Xamarin.Android Visual Studio emulator jest uruchamiana z elementem docelowym wybranego urządzenia, a aplikacja jest wdrażana na emulatorze. W lewym dolnym rogu wskazujący Visual Studio emulator uruchamiany pojawi się komunikat:

[![Uruchamianie emulatora VS](visual-studio-android-emulator-images/02-emulator-starting-vs-sml.png)](visual-studio-android-emulator-images/02-emulator-starting-vs.png#lightbox)

Opóźnieniem uruchomienia jak pokazano poniżej po lewej stronie zostanie wyświetlony ekran emulatora. Przeciągnij ikonę blokady na ekranie, do góry, aby odblokować urządzenie.
Aplikacji platformy Xamarin.Android należy ponownie uruchomić w emulatorze, jak pokazano na prawo od:

[![Zrzuty ekranu emulatora](visual-studio-android-emulator-images/03-first-screen-vs-sml.png)](visual-studio-android-emulator-images/03-first-screen-vs.png#lightbox)

W emulatorze systemu Android SDK domyślny jest można ustawić punktów przerwania w kodzie, zbadaj zmienne i wyświetlić stosu wywołań. Pionowy pasek narzędzi z prawej strony emulatora zapewnia dostęp do emulatora funkcje:

[![Przyciski na pionowym pasku narzędzi](visual-studio-android-emulator-images/04-vertical-toolbar-vs-sml.png)](visual-studio-android-emulator-images/04-vertical-toolbar-vs.png#lightbox)

Poniższa lista zawiera podsumowanie funkcji każdego przycisku na pionowym pasku narzędzi:

-   **Zamknij** &ndash; zamknięcie aplikacji emulatora. Ten przycisk nie jest używany często &ndash; zazwyczaj jest to emulator działać po pierwszego uruchomienia (w celu uniknięcia emulatora opóźnienie ponownego uruchomienia), a zamknięty, tylko gdy nie jest już potrzebne.

-   **Minimalizowanie** &ndash; pozostawia uruchomionym emulatorem, lecz minimalizuje go do paska zadań.

-   **Power** &ndash; Simulates włączanie urządzenia i wyłączanie. (Jest to emulator pozostaje uruchomiona).

-   **Obsługa wielodotyku** &ndash; nakładki tej czynności, jak punkty punkty zaciskające i powiększanie touch wyświetlić kilka punktów na urządzeniu.
    Przeciąganie jedną kropkę powoduje, że inne kropki (.) można przenieść, w przeciwnym kierunku, symulując przepływu dwa palca.

-   **Pojedynczy punkt wejściowy myszy** &ndash; przywraca urządzenia pojedynczy punkt wejścia (po użyciu wielodotyku wprowadzania).

-   **Obróć w prawo od lewej/Obracanie** &ndash; pomaga testów, jak aplikacja reaguje na zmiany orientacji. Na przykład po raz pierwszy *Obróć w lewo* przycisku, emulator przechodzi w tryb poziomo. Gdy *Obróć w prawo* kliknięciu przycisku będzie emulatora, aby powrócić do trybu orientacji pionowej.

-   **Dopasuj do ekranu** &ndash; powiększa rozmiaru ekranu emulatora, tak aby mieścił się na pulpicie.

-   **Powiększenie** &ndash; skaluje ekran emulatora 33%, 50%, 66%, 100% lub niektóre niestandardowe wartości procentowej.


*Dodatkowe narzędzia* przycisku spowoduje wyświetlenie otwiera okno dialogowe, które wyświetla dodatkowe funkcje emulatora:

[![Dodatkowe okno narzędzi](visual-studio-android-emulator-images/05-additional-tools-vs-sml.png)](visual-studio-android-emulator-images/05-additional-tools-vs.png#lightbox)


Każdy dodatkowego funkcji jest dostępna z szeregu kart w górnej części okna dialogowego:

-   **Przyspieszeniomierza** &ndash; symuluje ruch w przestrzeni 3D.

-   **Lokalizacja** &ndash; przedstawia mapy, który może służyć do wybierz i symulowanie lokalizacji GPS. Na tej mapie *mapy punktów* można utworzyć symulowanie przenoszenia między lokalizacjami.

-   **Baterii** &ndash; zapewnia suwak do symulowania opłaty wywołało baterii.

-   **Zrzut ekranu** &ndash; na tej karcie **przechwytywania** przycisku, który przyjmuje zrzut ekranu i wyświetla natychmiastowy podgląd. *Zapisać* przycisku spowoduje zapisanie zrzut ekranu.

-   **Aparat fotograficzny** &ndash; Simulates biorąc obraz za pomocą Stały obraz animowany obraz z pliku lub dołączone kamery internetowej na komputerze hosta. Istnieje możliwość wybierz przedniej lub tylnej aparaty fotograficzne.

-   **Karta SD** &ndash; emulator można udostępnić folderu na komputerze hosta jak karta SD urządzenia.
    Odczytuje i zapisuje pliki na kartę SD. symulowane aplikacji, aby były dostępne bezpośrednio z pulpitu bez użycia `adb` polecenia.

-   **Sieci** &ndash; Wyświetla podsumowanie ustawień sieciowych emulatora (emulator ponownie używa połączenia sieciowego komputera-hosta).

Aby uzyskać więcej informacji na temat używania tych funkcji, zobacz [wprowadzenie do programu Visual Studio Emulator for Android](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/).



## <a name="configuring-device-profiles"></a>Konfigurowanie profilów urządzeń

Emulatora Android firmy Microsoft obejmuje zestaw profili urządzenia, które reprezentują najpopularniejszych wersje systemu Android, rozmiaru ekranu i właściwości sprzętu, urządzeń z systemem Android na rynku. Ponadto profile te urządzenia są już skonfigurowane dla różnych wersji systemu Android, takie jak KitKat, interfejs typu lizak i Marshmallow.

*Menedżera emulatora* służy do instalowania, odinstalowywania i uruchomić profilów urządzeń. Z **narzędzia** menu, wybierz opcję **programu Visual Studio Emulator for Android...**  zgodnie z tego zrzutu ekranu:

[![Uruchamianie emulatora w menu Narzędzia](visual-studio-android-emulator-images/06-launch-emulator-manager-vs-sml.png)](visual-studio-android-emulator-images/06-launch-emulator-manager-vs.png#lightbox)

Spowoduje to otwarcie **profilów urządzeń** okna dialogowego. Zainstalowane profile są wyróżnione na początku listy profil urządzenia. Wygaszone profilów, które nie są zainstalowane (ale są dostępne do zainstalowania):

[![Ikony profilów urządzeń](visual-studio-android-emulator-images/07-device-profiles-vs-sml.png)](visual-studio-android-emulator-images/07-device-profiles-vs.png#lightbox)

Aby zainstalować nowy profil, kliknij ikonę instalacji profilu (pobieranej wskazywany Strzałka jak pokazano na powyższym zrzucie ekranu). Na przykład po kliknięciu ikony instalacji profilu **wersji 5.7" Phone XHDPI Marshmallow (6.0.0)**, Menedżera emulatora pobierają profil, jak pokazano poniżej:

[![Przykład pobieranie profilów](visual-studio-android-emulator-images/08-downloading-profile-vs-sml.png)](visual-studio-android-emulator-images/08-downloading-profile-vs.png#lightbox)

Po pobraniu profilu urządzenia zostanie wyróżniona wskazująca, czy profil został pomyślnie zainstalowany. Kliknięcie przycisku *Pokaż szczegóły* ikonę, aby wyświetli typ platformy, architektura procesora cpu, rozmiaru/rozdzielczość ekranu i dostępnej pamięci na urządzeniu:

[![Pokaż szczegóły profilu urządzenia](visual-studio-android-emulator-images/09-show-details-vs-sml.png)](visual-studio-android-emulator-images/09-show-details-vs.png#lightbox)

Gdy programu Visual Studio **docelowego debugowania** menu rozwijane jest otwarty, profil nowo zainstalowane urządzenie jest teraz dostępna jako miejsce docelowe:

[![Nowy profil, w menu rozwijanym docelowego](visual-studio-android-emulator-images/10-debug-target-vs-sml.png)](visual-studio-android-emulator-images/10-debug-target-vs.png#lightbox)

Może zostać skrócony tej listy, klikając **Odinstaluj ten profil** w *Menedżera emulatora* usunąć profile nieużywanych urządzeń. Należy pamiętać, że nie istnieje obecnie sposób tworzenia profilu urządzenia niestandardowe w tym emulatora.


## <a name="troubleshooting"></a>Rozwiązywanie problemów

W tej sekcji opisano niektóre typowe błędy i obejścia, korzystając z *programu Visual Studio Emulator for Android* z platformy Xamarin.Android.

<a name="cant_connect" />

### <a name="emulator-will-not-start"></a>Nie można uruchomić emulatora

W niektórych przypadkach emulator nie zostanie uruchomiona, jeśli istnieją incompabilities między procesora hosta i maszyny wirtualnej funkcji Hyper-V. Aby obejść ten problem, należy skonfigurować funkcji Hyper-V, aby ograniczyć funkcje procesora, których maszyna wirtualna może mieć &ndash; zwiększa zgodność maszyny wirtualnej z wersjami procesora innego hosta.
Aby to zrobić, wykonaj następujące kroki:

1.  Kliknij przycisk **Start** przycisku, wpisz w **MMC**i naciśnij klawisz **Enter**. Kliknij przycisk **Menedżera funkcji Hyper-V** jak pokazano poniżej:

    [![Menedżer funkcji Hyper-V](visual-studio-android-emulator-images/15-launch-hyperv-manager.png)](visual-studio-android-emulator-images/15-launch-hyperv-manager.png#lightbox)

2.  W Menedżerze funkcji Hyper-V **maszyn wirtualnych** prawym okienku kliknij emulator do edycji, a następnie kliknij przycisk **ustawień...** ":

    [![Element menu Ustawienia maszyny wirtualne](visual-studio-android-emulator-images/16-vm-settings.png)](visual-studio-android-emulator-images/16-vm-settings.png#lightbox)

3.  W oknie Ustawienia Znajdź **zgodności** sekcji (w obszarze **sprzętu > procesora**) i Włącz **Migruj do komputera fizycznego z inną wersją procesora**:

    [![Migracja z zaznaczoną opcją](visual-studio-android-emulator-images/17-set-compatibility-vs-sml.png)](visual-studio-android-emulator-images/17-set-compatibility-vs.png#lightbox)

4.  Kliknij przycisk **OK** i zamknąć okno Menedżera funkcji Hyper-V.



### <a name="app-deploys-and-starts-but-fails-immediately"></a>Aplikacja wdraża i uruchamia się, ale natychmiast kończy się niepowodzeniem

W takiej sytuacji uruchomieniu emulatora, aplikacja jest wdrażana pomyślnie na emulator i uruchomieniu aplikacji. Jednak aplikacji nie powiedzie się natychmiast.
W wielu przypadkach jest to również spowodowane incompabilities między procesora hosta i maszyny wirtualnej funkcji Hyper-V. Aby rozwiązać ten problem, postępuj zgodnie z instrukcjami [nie można uruchomić emulatora](#cant_connect) (powyżej).


### <a name="emulator-stops-with-the-diagnostic-message-libaot-mscorlibdllso-not-found"></a>Emulator zatrzyma komunikatów diagnostycznych: **mscorlib.dll.so libaot, nie można odnaleźć**

Aby rozwiązać ten problem, użyj następujących kroków, aby wyłączyć szybkiego wdrażania:

1.  Postępuj zgodnie z instrukcjami [nie można uruchomić emulatora](#cant_connect) (powyżej).

2.  Kliknij dwukrotnie plik projektu **właściwości**.

3.  Kliknij przycisk **Android opcje** i usuń zaznaczenie pozycji **Użyj szybkiego wdrożenia (tylko w trybie debugowania)**:

    [![Opcja szybkiego wdrożenia unchecked](visual-studio-android-emulator-images/18-fast-deployment-vs-sml.png)](visual-studio-android-emulator-images/18-fast-deployment-vs.png#lightbox)



### <a name="drag-and-drop-does-not-work"></a>Przeciąganie i upuszczanie nie działa

Jeśli *programu Visual Studio Emulator for Android* jest uruchomiona jako Administrator (lub jeśli można uruchamiać ją z z programu Visual Studio uruchomionej Visual Studio z uprawnieniami administratora), przeciągnij i upuść z. APK lub. Pliki ZIP może nie służbowych. Aby obejść ten problem, uruchom *programu Visual Studio Emulator for Android* bez podniesionego poziomu uprawnień (tj. nie jako Administrator).


### <a name="other-errors"></a>Inne błędy

Powyższe wskazówki dotyczące rozwiązywania problemów opisano najbardziej typowe problemy podczas korzystania z programu Visual Studio Emulator systemu Android z platformy Xamarin.Android. Aby uzyskać bardziej szczegółowy przewodnik dotyczący rozwiązywania problemów programu Visual Studio Emulator systemu Android, zobacz [Rozwiązywanie problemów z programu Visual Studio Emulator for Android](https://msdn.microsoft.com/en-us/library/mt228282.aspx).



## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzone *programu Visual Studio Emulator for Android*; go wyjaśniono, jak użyć emulatora do debugowania aplikacji platformy Xamarin.Android w programie Visual Studio, go opisane funkcje przycisków na pionowym pasku narzędzi i zapewniała Krótki przegląd funkcji dostępnych w **dodatkowe narzędzia** okna dialogowego. Go wyjaśniono, jak używać *Menedżera emulatora* Aby zainstalować, odinstalować, a następnie uruchom profilów urządzeń. A *Rozwiązywanie problemów* sekcji wyjaśniono typowe problemy i rozwiązania, gdy przy użyciu emulatora.


## <a name="related-links"></a>Linki pokrewne

- [Emulator programu Visual Studio dla systemu Android](https://www.visualstudio.com/en-us/explore/msft-android-emulator-vs.aspx)
- [Wprowadzenie do programu Visual Studio Emulator for Android](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)
