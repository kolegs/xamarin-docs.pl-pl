---
title: Raportowanie błędów
description: W tym dokumencie opisano metod w celu rozwiązania błędów.
ms.topic: article
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9b0d757c951f9244beb093a0a9b13ac1d069b507
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/29/2018
---
# <a name="reporting-bugs"></a>Raportowanie błędów

## <a name="overview"></a>Omówienie

Czasami możemy wszystkie zostać zablokowane podczas pracy nad projektem na braku możliwości można uzyskać interfejsu API w sposób którą chcemy udostępnić lub podczas próby obejść usterki. Naszym celem na platformie Xamarin jest dla Ciebie w pisanie aplikacji mobilnych i klasycznych było pomyślne, a przygotowaliśmy niektórych zasobów, aby pomóc.

Za pomocą dowolnego z tych zasobów są niektóre kroki przygotowania, które należy wykonać, aby pomóc w rozwiązywaniu problemu szybko:

- Należy ustalić główną przyczynę problemu, jak najlepiej, jak to możliwe do awarii raportu:
 
     - "Moja aplikacja ulegnie awarii" jest trudne do diagnozowania. "Moja aplikacja ulegnie awarii po powrocie pustą tablicę do tego wywołania" jest znacznie łatwiejsze do pracy na rozwiązanie problemu.

     - "Nie można pobrać NSTable pracę" warto mniej niż "Brak metody na mój NSTableDelegate wydają się można wywołać w tym przypadku."

- Jeśli to możliwe, podaj programu krótki przykład przedstawiający problem. Tu przeszukuje stronach kodu źródłowego w poszukiwaniu problem ma rzędów więcej czasu i wysiłku.

- Uzyskiwanie informacji o tym, co zmian wprowadzonych do aplikacji przyczyną problemu i pojawienie się można szybko zawęzić źródła problemu. Biorąc pod uwagę, jeśli został ostatnio uaktualniony wersji Xamarin.Mac przycinanie limit sekcje aplikacji można znaleźć części przyczyną problemu, lub testowania poprzedniej kompilacji można znaleźć, jakie zmiany wprowadzono problem może być bardzo przydatne.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>Co należy zrobić po awarii bez wpisu w Twojej aplikacji

W większości przypadków w debugerze programu Visual Studio for Mac będzie przechwytywać wyjątki i awarii aplikacji i ułatwiają identyfikowanie główną przyczynę. Istnieją jednak przypadki, gdzie aplikacja będzie Odbijanie na stacji dokującej, a następnie Zamknij z żadnych danych wyjściowych. Mogą do nich:

- Problemy dotyczące podpisywania kodu.
- Niektóre mono środowiska uruchomieniowego ulega awarii.
- Niektóre wyjątki języka Objective c i awarii.
- Niektóre awarie bardzo wczesnym okres istnienia procesu.
- Niektóre stosu przepełnienia.
- Wersja macOS na liście sieci **Info.plist** jest nowsza niż macOS obecnie zainstalowanej wersji lub jest nieprawidłowy.

Debugowanie tych programów może być irytujące, jako znajdowanie informacji konieczne może być trudne. Poniżej przedstawiono kilka metod, które mogą pomóc:

- Upewnij się, że wersja macOS na liście **Info.plist** jest taka sama jak wersja macOS aktualnie zainstalowane na komputerze.
- Sprawdź, czy program Visual Studio danych wyjściowych aplikacji Mac (**widoku** -> **tablety** -> **danych wyjściowych aplikacji**) dla stos danych śledzenia lub dane wyjściowe na czerwono z Cocoa opisujące dane wyjściowe.
- Uruchom aplikację z poziomu wiersza polecenia i w danych wyjściowych (w **Terminal** aplikacji) przy użyciu: 

     `MyApp.app/Contents/MacOS/MyApp` (gdzie `MyApp` to nazwa aplikacji)
- Dane wyjściowe można zwiększyć przez dodanie "MONO_LOG_LEVEL" polecenia w wierszu polecenia, na przykład: 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- Można dołączyć debuger natywny (`lldb`) do procesu, aby zobaczyć Jeśli zawierający wszystkie więcej informacji (wymaga licencji płatnej). Na przykład wykonaj następujące czynności:

    1. Wprowadź `lldb MyApp.app/Contents/MacOS/MyApp` w terminalu.
    2. Wprowadź `run` w terminalu.
    3. Wprowadź `c` w terminalu.
    4. Zakończ po zakończeniu debugowania.
- W ostateczności przed wywołaniem `NSApplication.Init` w Twojej `Main` — metoda (lub w innych miejscach, zgodnie z wymaganiami), tekst można zapisać do pliku w znanej lokalizacji do śledzenia na jakie krok Uruchom działają na problemy.

## <a name="known-issues"></a>Znane problemy

W poniższych częściach omówiono znane problemy i ich rozwiązania.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>Nie można nawiązać połączenia z debugera w trybie piaskownicy aplikacji

Debuger łączy się Xamarin.Mac aplikacji za pośrednictwem protokołu TCP, co oznacza, że po włączeniu sandboxing, jest domyślnie nie można nawiązać połączenia z aplikacji, więc jeśli zostanie podjęta próba uruchomienia aplikacji bez odpowiednich uprawnień, które są włączone, występuje błąd *"nie można nawiązać połączenia z Debuger"*. 

[![Edytowanie uprawnień](troubleshooting-images/debug01.png "edycji uprawnień")](troubleshooting-images/debug01-large.png#lightbox)

**Zezwalaj wychodzących połączeń sieciowych (klient)** uprawnienie jest wymagane dla debugera, włączenie tego umożliwi debugowania normalnie. Ponieważ nie można debugować bez niego, zostały zaktualizowane `CompileEntitlements` docelowe dla `msbuild` automatyczne dodawanie uprawnienie do uprawnień dla dowolnej aplikacji, która jest w trybie piaskownicy dla debugowania tylko kompilacje. Kompilacje wydania należy używać uprawnienia określone w pliku uprawnień, nie mają być modyfikowane.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: dane są niedostępne do kodowania 437
 
Po tym 3 bibliotek strony w aplikacji Xamarin.Mac, może być błąd pojawia się w formie "System.NotSupportedException: dane są niedostępne do kodowania 437" podczas próby skompilować i uruchomić aplikację. Na przykład, bibliotek, takich jak `Ionic.Zip.ZipFile`, może zgłosić wyjątek podczas operacji.

To będzie możliwe, otwierając opcje projektu Xamarin.Mac, przechodząc do **kompilacji Mac** > **internacjonalizacji** i sprawdzanie **zachodnie** ustawienia międzynarodowe:

[![Opcje kompilacji edycji](troubleshooting-images/issue01.png "edycji opcje kompilacji")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>Nie można skompilować (mm5103)

Zazwyczaj przyczyną tego błędu, gdy jest nowa wersja narzędzia xcode wersji i zainstalowaniu nowej wersji, ale nie jeszcze ją uruchomić. Zanim spróbujesz skompilować przy użyciu nowej wersji Xcode, należy najpierw uruchomić tę wersję co najmniej raz.

Podczas pierwszego uruchomienia nowej wersji Xcode, instaluje kilka narzędzi wiersza polecenia, które są wymagane przez Xamarin.Mac. Ponadto należy wykonać czystą kompilacji po uaktualnieniu z wersji Xamarin.Mac lub Xcode.

Jeśli nie możesz rozwiązać ten problem, skontaktuj się z [o usterce](#filing-a-bug).

### <a name="missing-entitlementsplist"></a>Brak entitlements.plist

Najnowszą wersję programu Visual Studio for Mac został usunięty z sekcji uprawnień **Info.plist** edytora i umieścić go w oddzielnych **Entitlements.plist** Edytor (dla lepszej obsługi wielu platform z Xamarin.iOS).

Z nowego programu Visual Studio dla komputerów Mac, podczas tworzenia nowego projektu aplikacji Xamarin.Mac **Entitlements.plist** plik zostanie automatycznie dodany do drzewa projektu:

![Wybieranie uprawnień](troubleshooting-images/entitlements01.png "wybranie uprawnień")

Po dwukrotnym kliknięciu **Entitlements.plist** pliku, Edytor uprawnień będą wyświetlane:

[![Edytowanie uprawnień](troubleshooting-images/entitlements02.png "edycji uprawnień")](troubleshooting-images/entitlements02-large.png#lightbox)

Dla istniejących projektów Xamarin.Mac, konieczne będzie ręczne tworzenie **Entitlements.plist** plików przez kliknięcie prawym przyciskiem myszy projekt w **konsoli rozwiązania** i wybierając **Dodaj**  >  **Nowego pliku...** . Następnie wybierz pozycję **Xamarin.Mac** > **pusta lista właściwości**:

![Dodawanie nowej listy właściwości](troubleshooting-images/entitlements03.png "Dodawanie nowej listy właściwości")

Wprowadź `Entitlements` dla nazwy i kliknij przycisk **nowy** przycisku. Jeśli projekt zawiera wcześniej pliku uprawnień, pojawi się monit, aby dodać go do projektu zamiast tworzenia nowego pliku:

[![Weryfikowanie nadpisanie pliku](troubleshooting-images/entitlements04.png "weryfikowanie nadpisanie pliku")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="contacting-support-business-or-enterprise-licenses"></a>Trwa nawiązywanie kontaktu z pomocy technicznej (licencji w firmie lub organizacji)

Jeśli masz firmy lub licencji enterprise, masz prawo do prosić o pomoc bezpośrednio inżynierów Xamarin za pośrednictwem biletami pomocy technicznej. Zobacz [xamarin.com/support](http://xamarin.com/support) szczegółowe informacje.

## <a name="community-support-on-the-forums"></a>Obsługa społeczności na forach

Zdarza się społeczności deweloperów używających produktów platformy Xamarin i wiele można znaleźć w naszych [fora Xamarin.Mac](http://forums.xamarin.com/categories/mac) można udostępniać ich wiedzy. Ponadto inżynierów Xamarin okresowo odwiedź forum, aby pomóc.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>Zgłaszanie usterki

Twoja opinia jest dla nas ważna. Jeśli napotkasz jakiekolwiek problemy z Xamarin.Mac:

- Wyszukiwanie [repozytorium problem](https://github.com/xamarin/xamarin-macios/issues) 
- Przed przełączeniem na problemy z GitHub, Xamarin problemy zostały śledzone na [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Wyszukaj istnieje, do dopasowania problemy.
- Jeśli nie można odnaleźć pasującego problem, plik nowy problem w [repozytorium GitHub problem](https://github.com/xamarin/xamarin-macios/issues/new).

Problemy z usługi GitHub są wszystkie publiczne. Nie jest możliwe Ukryj komentarze lub załączników. 

Dołącz tyle jedną z następujących możliwości:                                                                                                                                          

- Prosty przykład odtworzenia problemu. Jest to **nieocenione** w miarę możliwości. 
- Ślad stosu pełne awarii (Crash).
- Kod C# otaczającego tej awarii. 
