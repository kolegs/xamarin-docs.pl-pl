---
title: Błędy platformy Xamarin.iOS
description: W tym dokumencie opisano różne błędy generowane przez mtouch, narzędzie używane do pakietu aplikacji platformy Xamarin.iOS. Błędy są wyświetlane według kodu i jest pełny opis.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: e9332ba34f113f56859065c74c24c116a331eceb
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789450"
---
# <a name="xamarinios-errors"></a>Błędy platformy Xamarin.iOS

## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: komunikaty o błędach mtouch

Np. Parametry, środowisko, Brak narzędzia.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
    -->

<a name="MT0000" />

### <a name="mt0000-unexpected-error---please-fill-a-bug-report-at-httpbugzillaxamarincom"></a>MT0000: Wystąpił nieoczekiwany błąd - wypełnij raport o usterce w http://bugzilla.xamarin.com

Wystąpił nieoczekiwany błąd. Sprawdź [raport o usterkach](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) jak najwięcej informacji jak to możliwe, w tym:

* Pełnej kompilacji dzienników z zapewniały maksymalną szczegółowość (np. `-v -v -v -v` w **mtouch dodatkowe argumenty**);
* Minimalny przypadek testowy, który występuje błąd; i
* Wszystkie informacje w wersji

Najprostszym sposobem, aby uzyskać informacje o wersji dokładnie jest użycie **programu Visual Studio for Mac** menu **dotyczące programu Visual Studio for Mac** elementu **Pokaż szczegóły** przycisk i kopiowanie i wklejanie informacje o wersji (można użyć **kopiowanie informacji** przycisku).

<a name="MT0001" />

### <a name="mt0001--devname-was-provided-without-any-device-specific-action"></a>MT0001: "-devname" został podany bez żadnych działań specyficzne dla urządzenia

To ostrzeżenie, że jest emitowany Jeśli - devname jest przekazywana do mtouch, gdy żadna akcja ze strony specyficzne dla urządzenia (-logdev/installdev/killdev/launchdev /-listapps) zażądano.

<a name="MT0002" />

### <a name="mt0002-could-not-parse-the-environment-variable-"></a>MT0002: Nie można przeanalizować wartości zmiennej środowiskowej *.

Ten błąd wystąpi, Jeśli spróbujesz ustawić środowiska nieprawidłowy klucz = wartość zmiennej pary. Poprawny format to: `mtouch --setenv=VARIABLE=VALUE`

<a name="MT0003" />

### <a name="mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MT0003: Nazwa aplikacji "* .exe" powoduje konflikt z nazwą zestawu (.dll) zestawu SDK lub produkt.

Nazwa pliku wykonywalnego zestawu i nazwa aplikacji nie może odpowiadać nazwie każdej biblioteki dll w aplikacji. Zmień nazwę pliku wykonywalnego.

<a name="MT0004" />

### <a name="mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a>MT0004: Nowe logiki refcounting wymaga SGen zbyt włączenia.

Po włączeniu rozszerzenia refcounting należy również włączyć moduł zbierający elementy bezużyteczne SGen w projektu iOS opcje kompilacji (karta Zaawansowane).

Począwszy od platformy Xamarin.iOS 7.2.1 to wymaganie ma podnoszone, nowe logiki refcounting można włączyć zarówno Boehm, jak i pamięci SGen modułów zbierających dane.

<a name="MT0005" />

### <a name="mt0005-the-output-directory--does-not-exist"></a>MT0005: Katalog wyjściowy * nie istnieje.

Utwórz katalog.

Ten błąd nie jest już wygenerowany, mtouch automatycznie utworzyć katalogu, jeśli nie istnieje.

<a name="MT0006" />

### <a name="mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a>MT0006: Brak nie platformy devel *, użyj opcji--platform = PLAT, aby określić zestaw SDK.

Xamarin.iOS nie można odnaleźć katalogu zestawu SDK w lokalizacji wymienionych w komunikacie o błędzie. Sprawdź, czy ścieżka jest poprawna.

<a name="MT0007" />

### <a name="mt0007-the-root-assembly--does-not-exist"></a>MT0007: Zestawu głównego * nie istnieje.

Xamarin.iOS nie można znaleźć zestawu w lokalizacji wymienionych w komunikacie o błędzie. Sprawdź, czy ścieżka jest poprawna.

<a name="MT0008" />

### <a name="mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a>MT0008: Należy podać jedną zestawy # tylko, liczba znalezionych zestawu głównego: *.

Więcej niż jednym zestawie główny został przekazany do mtouch, mogą być tylko jeden główny zestaw.

<a name="MT0009" />

### <a name="mt0009-error-while-loading-assemblies-"></a>MT0009: Błąd podczas ładowania zestawów: *.

Wystąpił błąd podczas ładowania zestawów odwołań zestawu głównego. Więcej informacji może znajdować się w danych wyjściowych kompilacji.

<a name="MT0010" />

### <a name="mt0010-could-not-parse-the-command-line-arguments-"></a>MT0010: Nie można przeanalizować argumenty wiersza polecenia: *.

Wystąpił błąd podczas analizowania argumenty wiersza polecenia. Sprawdź, czy wszystkie poprawne.

<a name="MT0011" />

### <a name="mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a>MT0011: * został utworzony przed nowsza środowiska uruchomieniowego (*) nie obsługuje MonoTouch.

To ostrzeżenie jest zwykle raportowane, ponieważ projekt zawiera odwołanie do biblioteki klas, który nie został utworzony przy użyciu BCL platformy Xamarin.iOS.

Taki sam sposób, który aplikacji za pomocą zestawu SDK .NET 4.0 może nie działać w systemie tylko obsługa .NET 2.0 biblioteki, który został utworzony za pomocą programu .NET 4.0 może nie działać na Xamarin.iOS, wykorzystujący interfejs API nie znajduje się na platformy Xamarin.iOS.

Ogólne rozwiązanie jest do tworzenia biblioteki jako biblioteki klas platformy Xamarin.iOS. Można osiągnąć przez utworzenie nowego projektu platformy Xamarin.iOS biblioteki klas i Dodaj do niej wszystkich plików źródłowych. Jeśli nie masz kodu źródłowego w bibliotece, należy skontaktować się z dostawcą i żąda ich Xamarin.iOS zgodnej wersji biblioteki ich.

<a name="MT0012" />

### <a name="mt0012-incomplete-data-is-provided-to-complete-"></a>MT0012: Niekompletne dane są dostarczane do ukończenia *.

Ten błąd nie jest zgłaszany już w bieżącej wersji platformy Xamarin.iOS.

<a name="MT0013" />

### <a name="mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a>MT0013: Profilowanie obsługi wymaga sgen zbyt włączenia tej funkcji.

SGen (--sgen) musi być włączony, jeśli profilowania (--profilowania) jest włączona.

<a name="MT0014" />

### <a name="mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a>MT0014: IOS * zestawu SDK nie obsługuje tworzenie aplikacji przeznaczonych dla *.

Może się to zdarzyć w następujących okolicznościach:

*  ARMv6 jest włączona i Xcode 4.5 lub nowszej jest zainstalowane.
*  ARMv7s jest włączona i Xcode 4.4 lub starszym jest zainstalowany.

Sprawdź, czy zainstalowana wersja programu Xcode obsługuje wybranych architektur.

<a name="MT0015" />

### <a name="mt0015-invalid-abi--supported-abis-are-i386-x8664--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a>MT0015: Nieprawidłowy ABI: *. Są obsługiwane ABIs: i386, x86_64, armv7, armv7 llvm, armv7 + llvm + thumb2, armv7s, armv7s + llvm, armv7s + llvm + thumb2, arm64 i arm64 + llvm.

Do mtouch przekazano nieprawidłowy ABI. Określ prawidłowy ABI.

<a name="MT0016" />

### <a name="mt0016-the-option--has-been-deprecated"></a>MT0016: Opcja * jest przestarzała.

Opcja mtouch opisane powyżej jest przestarzała i zostanie zignorowana.

<a name="MT0017" />

### <a name="mt0017-you-should-provide-a-root-assembly"></a>MT0017: Należy podać zestawu głównego.

Jest wymagana do określania zestawu głównego (zazwyczaj głównego pliku wykonywalnego) podczas kompilowania aplikacji.

<a name="MT0018" />

### <a name="mt0018-unknown-command-line-argument-"></a>MT0018: Nieznany argument wiersza polecenia: *.

Mtouch nie może rozpoznać argumentu wiersza polecenia wymienione w komunikacie o błędzie.

<a name="MT0019" />

### <a name="mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a>MT0019: Tylko jeden — [dziennika | zainstalować | kill | uruchamianie] deweloperów lub--[Uruchamianie | debugowania] sim opcja może być używana.

Dostępnych jest kilka opcji dla mtouch, której nie można użyć jednocześnie:

-  --logdev
-  --installdev
-  --killdev
-  --launchdev
-  --launchdebug
-  --launchsim

<a name="MT0020" />

### <a name="mt0020-the-valid-options-for--are-"></a>MT0020: Prawidłowych opcji "\*"są"\*".

<a name="MT0021" />

### <a name="mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a>MT0021: Nie można skompilować przy użyciu gcc / g ++ (--Użyj gcc) korzystając z Rejestratora statyczne (jest to wartość domyślna w przypadku kompilowania kodu dla urządzenia). Usuń Użyj gcc flaga lub Użyj dynamicznych rejestratora (--rejestratora: dynamiczne).

<a name="MT0022" />

### <a name="mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a>MT0022: Opcji "--nieobsługiwany — Włącz — ogólne w — rejestratora" i "--rejestratora" są niezgodne.

Usuń obie opcje `--unsupported--enable-generics-in-registrar` i `--registrar`. Począwszy od platformy Xamarin.iOS 7.2.1 rejestratora domyślne obsługuje typów ogólnych.

Ten błąd nie jest już wyświetlany (argumentu wiersza polecenia `--unsupported--enable-generics-in-registrar` został usunięty z mtouch).

<a name="MT0023" />

### <a name="mt0023-application-name-exe-conflicts-with-another-user-assembly"></a>MT0023: Nazwa aplikacji "* .exe" powoduje konflikt z innego zestawu użytkownika.

Nazwa pliku wykonywalnego zestawu i nazwa aplikacji nie może odpowiadać nazwie każdej biblioteki dll w aplikacji. Zmień nazwę pliku wykonywalnego.

<a name="MT0024" />

### <a name="mt0024-could-not-find-required-file-"></a>MT0024: Nie można odnaleźć wymaganego pliku ' *'.

<a name="MT0025" />

### <a name="mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a>MT0025: Brak wersji zestawu SDK nie został podany. Dodaj `--sdk=X.Y` Aby określić, które iOS SDK powinien być używany do tworzenia aplikacji.

<a name="MT0026" />

### <a name="mt0026-could-not-parse-the-command-line-argument--"></a>MT0026: Nie można przeanalizować argument wiersza polecenia "*": *

<a name="MT0027" />

### <a name="mt0027-the-options--and--are-not-compatible"></a>MT0027: Opcje\*"i"\*"są niezgodne.

<a name="MT0028" />

### <a name="mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a>MT0028: Nie można włączyć koła (-kołowego) podczas przeznaczonych dla systemu iOS 4.1 lub starszym. Wyłącz koła (-kołowego: false) lub ustawić cel wdrożenia co najmniej iOS 4.2

<a name="MT0029" />

### <a name="mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a>MT0029: REPL (--Włącz repl) jest obsługiwana tylko w symulatorze (--sim).

REPL jest obsługiwana tylko, jeśli tworzysz symulatora. Oznacza to, że w przypadku przekazania `--enable-repl` do mtouch, należy także podać `--sim`.

<a name="MT0030" />

### <a name="mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a>MT0030: Nazwa pliku wykonywalnego (\*) i nazwy aplikacji (\*) są różne, może to uniemożliwić dzienniki awarii pobierania symbolicated poprawnie.

Gdy symbolicates Xcode (dokonuje translacji adresów pamięci funkcji nazwy i numery wiersza/w pliku) proces może zakończyć się niepowodzeniem, jeśli plik wykonywalny i aplikacji mieć różne nazwy (bez rozszerzenia).

Aby rozwiązać ten Zmień "Nazwa aplikacji" w opcje aplikacji iOS/kompilacji projektu lub zmień "Nazwa zestawu" w opcjach danych wyjściowych i kompilacji projektu.

<a name="MT0031" />

### <a name="mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a>MT0031: Argumenty wiersza polecenia "--enable--pobieranie w tle" i "— uruchamiania dla tła pobierania" wymagają "--launchsim" zbyt.

<a name="MT0032" />

### <a name="mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a>MT0032: Opcja "--debugtrack" jest ignorowana, chyba że "--debug" jest też określona.

<a name="MT0033" />

### <a name="mt0033-a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a>MT0033: Projektu platformy Xamarin.iOS musi odwoływać monotouch.dll lub Xamarin.iOS.dll

<a name="MT0034" />

### <a name="mt0034-cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a>MT0034: Nie może zawierać zarówno "monotouch.dll" i "Xamarin.iOS.dll" w tym samym projekcie platformy Xamarin.iOS - "\*" jest jawnie odwołać się do while "\*" odwołuje się do niego "*".

<!-- MT0035 unused -->

<a name="MT0036" />

### <a name="mt0036-cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a>MT0036: Nie można uruchomić * symulator * aplikacji. Włącz poprawne architecture(s) w projektu iOS opcje kompilacji (strona zaawansowanych).

<a name="MT0037" />

### <a name="mt0037-monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x8664"></a>MT0037: monotouch.dll jest zgodny z nie 64-bitowych. Odwołanie Xamarin.iOS.dll albo nie Kompiluj dla architektury 64-bitowych (ARM64 i/lub x86_64).

<a name="MT0038" />

### <a name="mt0038-the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a>MT0038: Starego rejestratorów (--rejestratora: oldstatic | olddynamic) nie są obsługiwane podczas odwoływania się do Xamarin.iOS.dll.

<a name="MT0039" />

### <a name="mt0039-applications-targeting-armv6-cannot-reference-xamariniosdll"></a>MT0039: ARMv6 aplikacji nie może odwoływać się Xamarin.iOS.dll.

<a name="MT0040" />

### <a name="mt0040-could-not-find-the-assembly--referenced-by-"></a>MT0040: Nie można znaleźć zestawu "\*", do którego istnieje odwołanie poprzez"\*".

<a name="MT0041" />

### <a name="mt0041-cannot-reference-both-monotouchdll-and-xamariniosdll"></a>MT0041: Nie można odwołać się zarówno "monotouch.dll" i "Xamarin.iOS.dll".

<a name="MT0042" />

### <a name="mt0042-no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a>MT0042: Nie monotouch.dll lub Xamarin.iOS.dll znaleziono odwołania. Odwołanie do monotouch.dll zostanie dodany.

<a name="MT0043" />

### <a name="mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a>MT0043: Moduł zbierający elementy bezużyteczne Boehm aktualnie nie jest obsługiwane podczas odwoływania się do "Xamarin.iOS.dll". Zamiast tego wybrano modułu zbierającego elementy bezużyteczne SGen.

Tylko moduł zbierający elementy bezużyteczne SGen jest obsługiwana z Unified projektów. Upewnić nie ma żadnych flag mtouch dodatkowe Określanie Boehm jako moduł garbage collector.

<a name="MT0044" />

### <a name="mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a>MT0044:--listsim jest obsługiwana tylko z Xcode 6.0 lub nowszym.

Zainstaluj nowszą wersję Xcode.

<a name="MT0045" />

### <a name="mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a>MT0045:--rozszerzenia jest obsługiwana tylko w przypadku korzystania z systemu iOS SDK 8.0 (lub nowszej).

<!-- MT0046 is not reported anymore -->

<a name="MT0047" />

### <a name="mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a>MT0047: Celu minimalna wdrożenia dla aplikacji Unified 5.1.1, bieżącą lokalizację docelową wdrożenia jest "*". Wybierz nowsze cel wdrożenia projektu iOS Opcje aplikacji.

<!-- MT0048 is not reported anymore -->

<a name="MT0049" />

### <a name="mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a>MT0049: *.framework jest obsługiwana tylko wtedy, gdy cel wdrożenia jest 8.0 lub nowszej. * funkcje mogą nie działać poprawnie.

Określony framework nie jest obsługiwana w celu wdrożenia odwołuje się do wersji systemu iOS. Należy zaktualizować do nowszej wersji systemu iOS cel wdrożenia, albo usunąć użycie określonej struktury z aplikacji.

<!-- MT0050 is not reported anymore -->

<a name="MT0051" />

### <a name="mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a>MT0051: Xamarin.iOS * wymaga Xcode 5.0 lub nowszej. Bieżąca wersja Xcode (znalezione w *) jest *.

Zainstaluj nowszą Xcode.

<a name="MT0052" />

### <a name="mt0052-no-command-specified"></a>MT0052: Nie określono polecenia.

Dla mtouch została podana żadna akcja.

<!-- 0053 is used by mmp -->

<a name="MT0054" />

### <a name="mt0054-unable-to-canonicalize-the-path--"></a>MT0054: Nie można canonicalize ścieżkę "*": *

Jest to błąd wewnętrzny. Jeśli zostanie wyświetlony ten błąd, proszę o usterce [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT0055" />

### <a name="mt0055-the-xcode-path--does-not-exist"></a>MT0055: Ścieżka Xcode "*" nie istnieje.

Ścieżka Xcode przekazywane przy użyciu `--sdkroot` nie istnieje. Określ prawidłową ścieżkę.

<a name="MT0056" />

### <a name="mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a>MT0056: Nie można odnaleźć Xcode w domyślnej lokalizacji (/ Applications/Xcode.app). Zainstaluj program Xcode lub Przekaż niestandardową ścieżkę przy użyciu — sdkroot <path>.

<a name="MT0057" />

### <a name="mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a>MT0057: Nie można ustalić ścieżki do Xcode.app z katalogu głównego zestawu sdk "*". Określ pełną ścieżkę do pakietu Xcode.app.

Ścieżka przekazywane przy użyciu `--sdkroot` nie określa prawidłowej aplikacji Xcode. Określ ścieżkę do aplikacji w środowisku Xcode.

<a name="MT0058" />

### <a name="mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a>MT0058: Xcode.app "\*" jest nieprawidłowy (plik "\*" nie istnieje).

Ścieżka przekazywane przy użyciu `--sdkroot` nie określa prawidłowej aplikacji Xcode. Określ ścieżkę do aplikacji w środowisku Xcode.

<a name="MT0059" />

### <a name="mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a>MT0059: Nie można odnaleźć Xcode aktualnie wybranego w systemie: *

<a name="MT0060" />

### <a name="mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a>MT0060: Nie można odnaleźć Xcode aktualnie wybranego w systemie. "xcode — wybierz — Ścieżka drukowania" zwrócił "*", ale ten katalog nie istnieje.

<a name="MT0061" />

### <a name="mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a>MT0061: Nie określono (przy użyciu--sdkroot), Xcode.app przy użyciu systemu Xcode, zgodnie z raportem "xcode — wybierz — ścieżkę drukowania": *

To jest ostrzeżenie informacyjny, wyjaśniający, która będzie Xcode używane, ponieważ nie została określona.

<a name="MT0062" />

### <a name="mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a>MT0062: Nie określono (przy użyciu--sdkroot lub "xcode — wybierz — Ścieżka drukowania"), zamiast domyślnej Xcode Xcode.app: *

To jest ostrzeżenie informacyjny, wyjaśniający, która będzie Xcode używane, ponieważ nie została określona.

<a name="MT0063" />

### <a name="mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a>MT0063: Nie można odnaleźć pliku wykonywalnego w rozszerzeniu * (CFBundleExecutable wpisu w pliku Info.plist)

Każdy Info.plist musi mieć plik wykonywalny (przy użyciu wpisu CFBundleExecutable), jednak wpis powinny być generowane automatycznie podczas kompilacji.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0064" />

### <a name="mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a>MT0064: Xamarin.iOS obsługuje tylko osadzone struktury z projektami Unified.

Xamarin.iOS obsługuje tylko struktury osadzone przy użyciu Unified API; Zaktualizuj projektu do interfejsu API Unified.

<a name="MT0065" />

### <a name="mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a>MT0065: Xamarin.iOS tylko obsługuje osadzone struktury, gdy cel wdrożenia jest co najmniej 8.0 (bieżącą lokalizację docelową wdrożenia: * osadzone struktury: *)

Xamarin.iOS obsługuje tylko struktury osadzone, gdy cel wdrożenia jest co najmniej 8.0 (z powodu wcześniejszych wersji systemu iOS nie obsługuje osadzonych Platform).

Zaktualizuj cel wdrożenia w pliku Info.plist projektu do 8.0 lub nowszej.

<a name="MT0066" />

### <a name="mt0066-invalid-build-registrar-assembly-"></a>MT0066: Nieprawidłowa kompilacji zestawu rejestratora: *

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0067" />

### <a name="mt0067-invalid-registrar-"></a>MT0067: Nieprawidłowy rejestratora: *

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0068" />

### <a name="mt0068-invalid-value-for-target-framework-"></a>MT0068: Nieprawidłowa wartość dla platformy docelowej: *.

Platforma docelowa nieprawidłowy został przekazany za pomocą argumentu--platformy docelowej. Określ prawidłowej struktury docelowej.

<a name="MT0069" />

<!--### MT0069: currently unused -->

<a name="MT0070" />

### <a name="mt0070-invalid-target-framework--valid-target-frameworks-are-"></a>MT0070: Nieprawidłowa target framework: *. Platformy prawidłowym obiektem docelowym są: *.

Platforma docelowa nieprawidłowy został przekazany za pomocą argumentu--platformy docelowej. Określ prawidłowej struktury docelowej.

<a name="MT0071" />

### <a name="mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT0071: Nieznany platformy: *. Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku http://bugzilla.xamarin.com z przypadkiem testowym.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0072" />

### <a name="mt0072-extensions-are-not-supported-for-the-platform-"></a>MT0072: Rozszerzenia nie są obsługiwane dla platformy ' *'.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0073" />

### <a name="mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a>MT0073: Xamarin.iOS * nie obsługuje cel wdrożenia z * (minimalna liczba to *). Wybierz cel wdrożenia nowszej Info.plist projektu.

Cel wdrożenia minimalna jest określony w komunikacie o błędzie; Wybierz cel wdrożenia nowszej Info.plist projektu.

Jeśli cel wdrożenia aktualizacji nie jest możliwe, następnie użyj starszą wersję platformy Xamarin.iOS.

<a name="MT0074" />

### <a name="mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a>MT0074: Xamarin.iOS * nie obsługuje cel wdrożenia minimalne z * (maksymalna liczba to *). Wybierz starszą cel wdrożenia w pliku Info.plist projektu lub uaktualnienia do nowszej wersji platformy Xamarin.iOS.

Xamarin.iOS nie obsługuje ustawiania cel wdrożenia minimalną na wyższą wersję niż wersja, którą tej konkretnej wersji platformy Xamarin.iOS został utworzony dla.

Wybierz starszą cel wdrożenia minimalne w pliku Info.plist projektu lub uaktualnienia do nowszej wersji platformy Xamarin.iOS.

<a name="MT0075" />

### <a name="mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a>MT0075: Nieprawidłowa architektura "*" dla * projektów. Są prawidłowe architektury: *

Określono nieprawidłowy architekturę. Sprawdź, czy architektura jest prawidłowa.

<a name="MT0076" />

### <a name="mt0075-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a>MT0075: Nie architektury (przy użyciu argumentu--abi). Architektura jest wymagany dla * projektów.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0077" />

### <a name="mt0076-watchos-projects-must-be-extensions"></a>MT0076: WatchOS projekty muszą być rozszerzenia.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0078" />

### <a name="mt0077-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a>MT0077: Kompilacje przyrostowe są włączone z wdrożenia docelowego < 8.0 (obecnie *). Jest to nieobsługiwane (wynikowej aplikacji nie będą uruchamiane w systemie iOS 9), więc cel wdrożenia zostanie ustawiona do 8.0.

To ostrzeżenie dowie się, że cel wdrożenia została ustawiona jako 8.0 dla tej kompilacji, aby kompilacje przyrostowe pracy poprawnie.

Kompilacje przyrostowe są obsługiwane tylko w przypadku, gdy cel wdrożenia jest co najmniej 8.0 (ponieważ wynikowa aplikacji nie będą uruchamiane w systemie iOS 9 w przeciwnym razie).

<a name="MT0079" />

### <a name="mt0078-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a>MT0078: Zalecane Xcode w wersji dla platformy Xamarin.iOS * jest Xcode * lub nowszym. Bieżąca wersja Xcode (znalezione w *) jest *.

To ostrzeżenie dowie się, że w bieżącej wersji programu Xcode nie jest zalecana wersja Xcode dla tej wersji platformy Xamarin.iOS.

Uaktualnij Xcode w celu zapewnienia optymalnego działania.

<a name="MT0080" />

### <a name="mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MT0080: Wyłączanie NewRefCount, — nowe-refcount:false, jest przestarzały.

To jest ostrzeżenie dowie się, że żądanie o wyłączenie nowe licznikiem (--new - refcount:false) został zignorowany.

Nowa funkcja licznikiem teraz jest wymagana dla wszystkich projektów i w związku z tym nie jest możliwe już wyłączanie.

<a name="MT0081" />

### <a name="mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a>MT0081: Argument wiersza polecenia — pobierania awarii raport wymaga również — pobierania awarii raportu na.

<a name="MT0082" />

### <a name="mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a>MT0082: REPL (--Włącz repl) jest obsługiwana tylko w przypadku połączeń nie jest używana (--nolink).

<a name="MT0083" />

### <a name="mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a>MT0083: Bitcode tylko do funkcji Asm nie jest obsługiwana na watchOS. Użyj albo kodu bitowego —: znacznikowy lub--bitcode: Pełna.

<a name="MT0084" />

### <a name="mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a>MT0084: Bitcode nie jest obsługiwana w symulatorze. Nie przekazuj--bitcode podczas budowania symulatora.

<a name="MT0085" />

### <a name="mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a>MT0085: Nie odwołania do "*" został znaleziony. Zostanie ona dodana automatycznie.

<a name="MT0086" />

### <a name="mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a>MT0086: Platformy docelowej (--platformę docelową) należy określić podczas kompilowania systemu TVOS lub WatchOS.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0087" />

### <a name="mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a>MT0087: Kompilacje przyrostowe (--fastdev) nie jest obsługiwany w wykazie Globalnym Boehm. Kompilacje przyrostowe zostanie wyłączone.

<a name="MT0088" />

### <a name="mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a>MT0088: Wykaz Globalny musi być w trybie współpracy watchOS aplikacji. Usuń argument mtouch coop: false.

<a name="MT0089" />

### <a name="mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a>MT0089: Opcja "\*"nie może przyjąć wartość"\*" po włączeniu trybu wspólnych dla GC.

<a name="MT0091" />

### <a name="mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MT0091: Wymaga to wersja platformy Xamarin.iOS * SDK (dostarczone z Xcode *). Albo uaktualnić Xcode do pobrania plików wymaganego nagłówka lub ustawienia zachowania zarządzanych konsolidatora łącze Framework SDK tylko do odczytu (należy unikać nowych interfejsów API).

Xamarin.iOS wymaga pliki nagłówków z zestawu SDK w wersji określony w komunikacie o błędzie, aby skompilować aplikację. Zalecany sposób, aby naprawić ten błąd jest uaktualnienie Xcode, aby uzyskać wymagany zestaw SDK, to będzie zawierać wszystkie pliki wymaganego nagłówka. Jeśli wiele wersji Xcode zainstalowane lub chcesz użyć Xcode w lokalizacji innej niż domyślna, należy ustawić prawidłową lokalizację Xcode w preferencjach środowiskiem IDE.

Potencjalne, alternatywne rozwiązanie jest umożliwienie konsolidator zarządzanych. Spowoduje to usunięcie nieużywanych interfejsu API w tym, w większości przypadków nowy interfejs API, gdzie pliki nagłówkowe brakuje (lub są niekompletne). Niemniej to nie będzie działać, jeśli projekt używa interfejsu API, który został wprowadzony w nowszych zestawu SDK niż jeden z Xcode zawiera.

Ostatnie słomy rozwiązaniem byłoby użyć starszej wersji platformy Xamarin.iOS, obsługującą SDK projektu wymaga.

<!-- MT0092 used by mlaunch -->

<a name="MT0093" />

### <a name="mt0093-could-not-find-mlaunch"></a>MT0093: Nie można odnaleźć "mlaunch".

<!-- MT0094 is not reported anymore -->

<a name="MT0095" />

### <a name="mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a>MT0095: Drzewa obiektów aplikacji nie można skopiować plików do katalogu docelowego {dest}: {error}

<a name="MT0096" />

### <a name="mt0096-no-reference-to-xamariniosdll-was-found"></a>MT0096: Nie można Xamarin.iOS.dll znaleziono odwołania.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

<a name="MT0099" />

### <a name="mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0099: Błąd wewnętrzny *. Raport o usterce z przypadkiem testowym pliku (http://bugzilla.xamarin.com).

Ten komunikat o błędzie jest zgłaszany, gdy wewnętrzne sprawdzenie spójności w Xamarin.iOS nie powiodło się.

To wskazuje na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0100" />

### <a name="mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MT0100: Nieprawidłowy zestaw kompilacji docelowych: "*". Raport o usterce z przypadkiem testowym pliku (http://bugzilla.xamarin.com).

Ten komunikat o błędzie jest zgłaszany, gdy wewnętrzne sprawdzenie spójności w Xamarin.iOS nie powiodło się.

Jest to zawsze na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z przypadkiem testowym.

<a name="MT0101" />

### <a name="mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a>MT0101: Zestaw ' *' został określony wiele razy w--zestawu kompilacji docelowe argumentów.

Zestaw wymienionych w komunikacie o błędzie został określony wiele razy w--zestawu kompilacji docelowe argumentów. Upewnij się, że każdy zestaw jest wymieniony tylko raz.

<a name="MT0102" />

### <a name="mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a>MT0102: Zestawy\*"i"\*"mają taką samą nazwę docelowej ("\*"), ale różnych celów ("\*"i" * ").

Zestawy wymienionych w komunikacie o błędzie ma sprzeczne cele kompilacji.

Na przykład:

    --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary

W tym przykładzie jest w trakcie tworzenia zarówno bibliotekę dynamiczną i to struktura z tym samym upewnij (`MyBinary`).

<a name="MT0103" />

### <a name="mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a>MT0103: Statyczne obiektu "\*" zawiera więcej niż jednym zestawie ("\*"), ale każdy obiekt statyczny musi być zgodna z dokładnie jednym zestawie.

Wymienione w komunikacie o błędzie zestawy są kompilowane do pojedynczego obiektu statycznego. Jest to niedozwolone, każdy zestaw należy skompilować do innego obiektu statycznego.

Na przykład:

    --assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary

W tym przykładzie próbuje utworzyć obiektu statycznego (`MyBinary`) składają się z dwóch zestawów (`Assembly1.dll` i `Assembly2.dll`), co jest niedozwolone.

<a name="MT0105" />

### <a name="mt0105-no-assembly-build-target-was-specified-for-"></a>MT0105: Nie zestawu kompilacji określono miejsca docelowego dla "*".

Podczas określania zestawu kompilować docelowych przy użyciu `--assembly-build-target`, wszystkie zestawy w aplikacji musi mieć przypisany cel kompilacji.

Ten błąd jest zgłaszany, gdy zestaw wymienionych w komunikacie o błędzie nie ma zestawu kompilacji docelowy przypisany.

Można znaleźć w dokumentacji `--assembly-build-target` Aby uzyskać więcej informacji.

<a name="MT0106" />

### <a name="mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a>MT0106: Nazwa docelowej kompilacji zestawu "\*" jest nieprawidłowy: znak '\*"jest niedozwolone.

Nazwa docelowego zestawu kompilacji musi być prawidłową nazwą pliku.

Na przykład wartości tych wyzwoli ten błąd:

    --assembly-build-target:Assembly1.dll=staticobject=my/path.o

Ponieważ `my/path.o` nie jest prawidłową nazwą pliku z powodu znakiem separatora katalogu.

<a name="MT0107" />

### <a name="mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a>MT0107: Zestawy\*"mają różne niestandardowych optymalizacje LLVM (\*), co jest niedozwolone, gdy są one wszystkich kompilowane do pojedynczego pliku binarnego.

<a name="MT0108" />

### <a name="mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a>MT0108: Tworzenie zestawu docelowego "*" nie pasuje do żadnych zestawów.

<a name="MT0109" />

### <a name="mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a>MT0109: Zestaw '{0}' został załadowany z inną ścieżką niż podana ścieżka (podana ścieżka: {1}, rzeczywistej ścieżce: {2}).

To ostrzeżenie wskazujący załadowany z innej lokalizacji niż żądany zestaw przywoływany przez aplikację.

Może to oznaczać, że aplikacja odwołuje się wiele zestawów o takiej samej nazwie, ale z innej lokalizacji, co może prowadzić do nieoczekiwanych wyników (będzie można używać tylko pierwszy zestaw).

<a name="MT0110" />

### <a name="mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a>MT0110: Kompilacji przyrostowej zostały wyłączone, ponieważ ta wersja platformy Xamarin.iOS nie obsługuje kompilacji przyrostowej w projekty zawierające bibliotek powiązania innych firm i który kompiluje się do kodu bitowego.

Kompilacje przyrostowe zostały wyłączone, ponieważ ta wersja platformy Xamarin.iOS nie obsługuje kompilacji przyrostowej w projekty zawierające bibliotek powiązania innych firm i który kompiluje się do kodu bitowego (projekty systemu tvOS i watchOS).

Nie są wymagane nie działania ze strony użytkownika, ten komunikat jest wyłącznie informacyjne.

Aby uzyskać więcej informacji, zobacz usterek #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710).

To ostrzeżenie nie jest już zgłoszony.

<a name="MT0111" />

### <a name="mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a>MT0111: Bitcode zostało włączone, ponieważ ta wersja platformy Xamarin.iOS nie obsługuje tworzenia watchOS projektów przy użyciu LLVM bez włączania kodu bitowego.

Kodu bitowego została włączona automatycznie, ponieważ ta wersja platformy Xamarin.iOS nie obsługuje tworzenia watchOS projektów przy użyciu LLVM bez włączania kodu bitowego.

Nie są wymagane nie działania ze strony użytkownika, ten komunikat jest wyłącznie informacyjne.

Aby uzyskać więcej informacji, zobacz usterek #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634).

<a name="MT0112" />

### <a name="mt0112-native-code-sharing-has-been-disabled-because-"></a>MT0112: Udostępnianie kodu natywnego została wyłączona, ponieważ *

Istnieje wiele powodów można wyłączyć udostępnianie kodu:

* Ponieważ cel wdrożenia aplikacji kontenera jest starsza niż iOS 8.0 (ma *)).

Udostępnianie kodu natywnego wymaga systemu iOS 8.0 ponieważ udostępnianie kodu natywnego jest implementowane za pomocą struktury użytkownika, które wprowadzono w systemie iOS 8.0.

* ponieważ aplikacja kontenera obejmuje zestawy I18N (*).

Udostępnianie kodu natywnego nie jest obecnie obsługiwane, jeśli aplikacja kontenera zawiera zestawy I18N.

* ponieważ aplikacja kontenera ma niestandardowy plik xml definicji dla konsolidatora zarządzanych (*).

Udostępnianie kodu natywnego wymaga nie jest obsługiwane dla projektów programu definicje niestandardowy plik xml dla konsolidatora zarządzanych.

<a name="MT0113" />

### <a name="mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a>MT0113: Udostępnianie kodu natywnego została wyłączona dla rozszerzenia ' *' ponieważ *.

* Ponieważ opcje kodu bitowego różnią się w aplikacji kontenera (\*) i rozszerzenia (\*).

  Udostępnianie kodu natywnego musi być zgodna Opcje kodu bitowego między projektami, które mają kod.

* ponieważ zestawu kompilacji docelowe dostępne opcje zależą od aplikacji kontenera (\*) i rozszerzenia (\*).

  Udostępnianie kodu natywnego wymaga, aby zestawu kompilacji docelowe są identyczne między projektami, które mają kod.

  Ten stan może wystąpić, jeśli kompilacje przyrostowe nie albo włączone lub wyłączone we wszystkich projektach.

* ponieważ zestawy I18N różnią się od aplikacji kontenera (\*) i rozszerzenia (\*).

  Udostępnianie kodu natywnego nie jest obecnie obsługiwane dla rozszerzeń, które zawierają zestawy I18N.

* Ponieważ argumenty kompilatora drzewa obiektów aplikacji różnią się od aplikacji kontenera (\*) i rozszerzenia (\*).

  Udostępnianie kodu natywnego wymaga, że argumenty kompilatora drzewa obiektów aplikacji nie różnią się między projektami, które mają kod.

* ponieważ inne argumenty do kompilatora drzewa obiektów aplikacji różnią się od aplikacji kontenera (\*) i rozszerzenia (\*).

  Udostępnianie kodu natywnego wymaga, że argumenty kompilatora drzewa obiektów aplikacji nie różnią się między projektami, które mają kod.

  Ten stan występuje, jeśli 'wykonywać wszystkie operacje 32-bitowych liczb zmiennoprzecinkowych jako 64-bit float"różni się między projektami.

* ponieważ LLVM nie jest włączone lub wyłączone w aplikacji kontenera (\*) i rozszerzenia (\*).

  Udostępnianie kodu natywnego wymaga, aby LLVM jest włączona lub wyłączona dla wszystkich projektów, które mają kod.

* ponieważ ustawienia konsolidatora zarządzanych różnią się od aplikacji kontenera (\*) i rozszerzenia (\*).

  Udostępnianie kodu natywnego wymaga, że ustawienia konsolidatora zarządzane są identyczne dla wszystkich projektów, które mają kod.

* Ponieważ pominięto zestawy dla zarządzanych konsolidator różnią się od aplikacji kontenera (\*) i rozszerzenia (\*).

  Udostępnianie kodu natywnego wymaga, że ustawienia konsolidatora zarządzane są identyczne dla wszystkich projektów, które mają kod.

* ponieważ rozszerzenia ma niestandardowy plik xml definicji dla konsolidatora zarządzanych (*).

  Udostępnianie kodu natywnego wymaga nie jest obsługiwane dla projektów programu definicje niestandardowy plik xml dla konsolidatora zarządzanych.

* ponieważ aplikacja kontenera nie kompilacji dla ABI * (gdy rozszerzenie tworzy dla tego interfejsu ABI).

  Udostępnianie kodu natywnego wymaga czy aplikacji kontenera kompilacje dla wszystkich architektur wszelkich rozszerzenie aplikacji kompilacje dla.

  Na przykład: ten stan występuje, gdy rozszerzenie kompilacje dla ARM64 + ARMv7, ale aplikacji kontenera tylko kompilacje dla ARM64.

* ponieważ tworzenie aplikacji kontenera jest interfejsem ABI \*, który nie jest zgodny z interfejsem ABI rozszerzenia (\*).

  Udostępnianie kodu natywnego wymaga, że wszystkie projekty kompilacji dokładnie tego samego interfejsu API.

  Na przykład: ten stan występuje, gdy rozszerzenie kompilacje dla ARMv7 llvm + thumb2, ale aplikacji kontenera tylko kompilacje dla ARMv7 + llvm.

* ponieważ aplikacja kontenera jest odwołanie do zestawu "\*"od"\*", a rozszerzenie odwołuje się do innej wersji z "*".

  Udostępnianie kodu natywnego wymaga, aby wszystkie projekty, które mają kod używać tej samej wersji dla wszystkich zestawów.

<!-- MT0114: used by mmp -->

<a name="MT0115" />

### <a name="mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a>MT0115: Zaleca się odwoływać dynamiczne symbole, przy użyciu kodu (--dynamicznie symbol trybu = kodu) po włączeniu kodu bitowego.

Projekty platformy Xamarin.iOS będzie często odwoływać symboli natywnych dynamicznie, co oznacza, że konsolidator natywnych może spowodować usunięcie takich symboli natywnych podczas procesu łączenia natywnego ponieważ konsolidator macierzysty nie widzi służą tych symboli.

Zazwyczaj Xamarin.iOS poprosi natywnego konsolidator, aby zachować tych symboli (przy użyciu `-u symbol` flagi konsolidatora), ale podczas kompilowania kodu dla kodu bitowego konsolidator macierzysty nie akceptuje `-u` flagi.

Xamarin.iOS zaimplementowała alternatywnego rozwiązania: Firma Microsoft wygenerować bardzo natywny kod, który odwołuje się do tych symboli, a w związku z tym natywnego konsolidator będą widzieć służą tych symboli. Jest to realizowane automatycznie podczas kompilowania do kodu bitowego.

Jeśli `--dynamic-symbol-mode=linker` jest przekazywana do mtouch tym alternatywne rozwiązanie zostanie wyłączone, a Xamarin.iOS spróbuje przekazać `-u` do konsolidatora macierzystego. Najprawdopodobniej będzie to powodować błędy konsolidatora natywnego.

Rozwiązanie to Usuń `--dynamic-symbol-mode=linker` argumentu z argumentów mtouch dodatkowe opcje kompilacji projektu.

<!-- 0116 - 0124: free to use -->

<a name="MT0116" />

### <a name="mt0116-invalid-architecture-arch-32-bit-architectures-are-not-supported-when-deployment-target-is-11-or-later-make-sure-the-project-does-not-build-for-a-32-bit-architecture"></a>MT0116: Nieprawidłowa architektura: {arch}. architektury 32-bitowe nie są obsługiwane, gdy cel wdrożenia jest 11 lub nowszy. Upewnij się, że projekt nie kompiluje się dla architektury 32-bitowych.

iOS 11 nie zawiera obsługę aplikacji 32-bitowych, więc kompilacji dla 32-bitowej aplikacji, gdy cel wdrożenia jest iOS 11 lub nowszy nie jest obsługiwane.

Zmień architektury docelowej w opcjach kompilacji systemu iOS projektu na arm64 albo zmień cel wdrożenia w pliku Info.plist projektu z wcześniejszą wersją systemu iOS.

<a name="MT0117" />

### <a name="mt0117-cant-launch-a-32-bit-app-on-a-simulator-that-only-supports-64-bit"></a>MT0117: Nie można uruchomić 32-bitowych aplikacji w symulatorze, który obsługuje tylko 64-bitowych.

<a name="MT0118" />

### <a name="mt0118-aot-files-could-not-be-found-at-the-expected-directory-msymdir"></a>MT0118: Pliki drzewa obiektów aplikacji nie można odnaleźć oczekiwanego katalogu {msymdir}.

<!-- 0119 - 0123: free to use -->

<a name="MT0123" />

### <a name="mt0123-the-executable-assembly--does-not-reference-"></a>MT0123: Wykonywalnego zestawu * nie odwołuje się *.

Nie można odnaleźć odwołania do zestawu platformy (Xamarin.iOS.dll / Xamarin.TVOS.dll / Xamarin.WatchOS.dll) w zestawie pliku wykonywalnego.

Zazwyczaj dzieje się tak w przypadku, gdy nie jest wykonywany kod w projekcie pliku wykonywalnego, który korzysta z żadnych z zestawu platformy; na przykład pusta metody Main (i żadnego innego kodu) czy pokazano, ten błąd:

```csharp
class Program {
    void Main (string[] args)
    {
    }
}
```

Przy użyciu interfejsu API z zestawu platformy rozwiąże błąd:

```csharp
class Program {
    void Main (string[] args)
    {
        System.Console.WriteLine (typeof (UIKit.UIWindow));
    }
}
```

<a name="MT0124" />

### <a name="mt0124-could-not-set-the-current-language-to-lang-according-to-langlang-exception"></a>MT0124: Nie można ustawić bieżącego języka aby {lang} (zgodnie z LANG = {LANG}): {wyjątku}

To ostrzeżenie, wskazujący, że nie można ustawić bieżącego języka na język w komunikacie o błędzie.

Bieżący język będzie domyślny język systemu.

<a name="MT0125" />

### <a name="mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a>MT0125: Zestawu kompilacji docelowe argument wiersza polecenia jest ignorowany w symulatorze.

Nie jest wymagana żadna akcja, ten komunikat jest wyłącznie informacyjne.

<a name="MT0126" />

### <a name="mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a>MT0126: Kompilacji przyrostowej zostały wyłączone, ponieważ kompilacji przyrostowej nie są obsługiwane w symulatorze.

Nie jest wymagana żadna akcja, ten komunikat jest wyłącznie informacyjne.

<a name="MT0127" />

### <a name="mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a>MT0127: Kompilacji przyrostowej zostały wyłączone, ponieważ ta wersja platformy Xamarin.iOS nie obsługuje kompilacji przyrostowej w projektach, które zawierają więcej niż jedna trzecia strona powiązanie bibliotek.

Kompilacje przyrostowe zostały wyłączone automatycznie, ponieważ ta wersja platformy Xamarin.iOS nie zawsze kompilować projektów z wielu innych firm powiązanie bibliotek poprawnie.

Nie jest wymagana żadna akcja, ten komunikat jest wyłącznie informacyjne.

Aby uzyskać więcej informacji, zobacz usterek #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727).

<a name="MT0128" />

### <a name="mt0128-could-not-touch-the-file--"></a>MT0128: Nie można touch pliku "*": *

Wystąpił błąd podczas dotykania plik (który jest gotowe do upewnij się, że zostaną prawidłowo wykonane częściowe kompilacji).

Prawdopodobnie można zignorować to ostrzeżenie; w przypadku problemów o usterce (https://bugzilla.xamarin.com] (https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)) i będzie się zbadana.

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: Projekt powiązane komunikaty o błędach

### <a name="mt10xx-installer--mtouch"></a>MT10xx: Instalator / mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

<a name="MT1001" />

### <a name="mt1001-could-not-find-an-application-at-the-specified-directory"></a>MT1001: Nie można odnaleźć aplikacji w określonym katalogu

<a name="MT1002" />

### <a name="mt1002-could-not-create-symlinks-files-were-copied"></a>MT1002: Nie można utworzyć łączy symbolicznych, pliki zostały skopiowane

<a name="MT1003" />

### <a name="mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a>MT1003: Nie można zakończyć aplikację "*". Może być konieczne ręczne kill aplikacji.

<a name="MT1004" />

### <a name="mt1004-could-not-get-the-list-of-installed-applications"></a>MT1004: Nie można pobrać listy zainstalowanych aplikacji.

<a name="MT1005" />

### <a name="mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a>MT1005: Nie można zakończyć aplikację "\*"na urządzeniu"\*": *-może być konieczne ręczne kill aplikacji.

<a name="MT1006" />

### <a name="mt1006-could-not-install-the-application--on-the-device--"></a>MT1006: Nie można zainstalować aplikacji "\*"na urządzeniu"\*": *.

<a name="MT1007" />

### <a name="mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a>MT1007: Nie można uruchomić aplikacji "\*"na urządzeniu"\*": *. Aplikację można uruchomić ręcznie, naciskając na nim.

<a name="MT1008" />

### <a name="mt1008-failed-to-launch-the-simulator"></a>MT1008: Nie można uruchomić symulatora

Ten błąd jest zgłaszany, gdy mtouch nie można uruchomić symulatora.   Może to się zdarzyć czasami jest już przestarzałe lub martwy symulatora procesu uruchomionego.

Polecenie wydane w wierszu polecenia systemu Unix może służyć do kill zablokowane Symulator procesów:

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

<a name="MT1009" />

### <a name="mt1009-could-not-copy-the-assembly--to--"></a>MT1009: Nie można skopiować zestawu "\*"do"\*": *

Jest to znany problem w niektórych wersjach platformy Xamarin.iOS.

W takiej sytuacji do Ciebie, wypróbuj następujące rozwiązania:

```bash
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

Jednak ponieważ ten problem został rozwiązany w najnowszej wersji platformy Xamarin.iOS, proszę o usterce nowe na [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z pełną wersję informacji i danych wyjściowych dziennika kompilacji.

<a name="MT1010" />

### <a name="mt1010-could-not-load-the-assembly--"></a>MT1010: Nie można załadować zestawu "*": *

<a name="MT1011" />

### <a name="mt1011-could-not-add-missing-resource-file-"></a>MT1011: Nie można dodać brakujący plik zasobu: "*"

<a name="MT1012" />

### <a name="mt1012-failed-to-list-the-apps-on-the-device--"></a>MT1012: Nie można wyświetlić listy aplikacji na urządzeniu "*": *

<a name="MT1013" />

### <a name="mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a>MT1013: Zależności błąd: Brak plików do porównania. Raport o usterce w pliku http://bugzilla.xamarin.com z przypadkiem testowym.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z caes testu.

<a name="MT1014" />

### <a name="mt1014-failed-to-re-use-cached-version-of--"></a>MT1014: Nie można ponownie użyć buforowanej wersji "*": *.

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: Nie można utworzyć pliku wykonywalnego "*": *

<a name="MT1015" />

### <a name="mt1015-failed-to-create-the-executable--"></a>MT1015: Nie można utworzyć pliku wykonywalnego "*": *

<a name="MT1016" />

### <a name="mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a>MT1016: Nie można utworzyć pliku powiadomienie, ponieważ istnieje już katalog o tej samej nazwie.

Usuń katalog `NOTICE` z projektu.

<a name="MT1017" />

### <a name="mt1017-failed-to-create-the-notice-file-"></a>MT1017: Nie można utworzyć pliku powiadomienie: *.

<a name="MT1018" />

### <a name="mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a>MT1018: Aplikacja nie przeszły pomyślnie sprawdzania podpisywania kodu i nie można zainstalować na urządzeniu "*". Sprawdź certyfikaty, profile, aprowizacji i powiązać identyfikatorów. Prawdopodobnie Twoje urządzenie nie jest częścią wybranego profilu inicjowania obsługi administracyjnej (błąd: 0xe8008015).

<a name="MT1019" />

### <a name="mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a>MT1019: Aplikacja zawiera uprawnienia nie są obsługiwane przez bieżący profil inicjowania obsługi administracyjnej i nie można zainstalować na urządzeniu "*". Sprawdź dziennika urządzenia z systemem iOS, aby uzyskać szczegółowe informacje (błąd: 0xe8008016).

Może się to zdarzyć, jeśli:

* Aplikacja ma uprawnienia, które nie obsługują bieżącego profilu inicjowania obsługi administracyjnej.
  Możliwe rozwiązania:
  - Określ innego profilu inicjowania obsługi administracyjnej, który obsługuje uprawnienia do potrzeb aplikacji.
  - Usuwanie uprawnień, nie są obsługiwane w bieżącym profilu inicjowania obsługi administracyjnej.
* Urządzenie, które próbujesz wdrożyć nie jest uwzględniony w profilu inicjowania obsługi administracyjnej, którego używasz.
  Możliwe rozwiązania:
  - Utwórz nową aplikację z szablonu w środowisku Xcode, wybierz ten sam profil inicjowania obsługi administracyjnej i wdrożyć na tym samym urządzeniu. Czasami Xcode automatycznie odświeżyć profile inicjowania obsługi administracyjnej z nowych urządzeń (w innych przypadkach Xcode zapyta, co należy zrobić).
  -Przejdź do Centrum deweloperów systemu iOS i zaktualizować profil inicjowania obsługi administracyjnej o nowe urządzenie, a następnie Pobierz zaktualizowany profil inicjowania obsługi administracyjnej na tym komputerze.

W większości przypadków więcej informacji na temat niepowodzenia będą wypisywane w dzienniku urządzenia z systemem iOS, która pomaga diagnozowania problemu.

<a name="MT1020" />

### <a name="mt1020-failed-to-launch-the-application--on-the-device--"></a>MT1020: Nie można uruchomić aplikacji "\*"na urządzeniu"\*": *

<a name="MT1021" />

### <a name="mt1021-could-not-copy-the-file--to--2"></a>MT1021: Nie można skopiować pliku "\*"do"\*": {2}

Nie można skopiować pliku. Komunikat o błędzie z operacji kopiowania ma więcej informacji o tym błędzie.

<a name="MT1022" />

### <a name="mt1022-could-not-copy-the-directory--to--2"></a>MT1022: Nie można skopiować katalogu "\*"do"\*": {2}

Nie można skopiować katalogu. Komunikat o błędzie z operacji kopiowania ma więcej informacji o tym błędzie.

<a name="MT1023" />

### <a name="mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a>MT1023: Nie można nawiązać komunikacji z urządzeniem, aby znaleźć aplikacji "*": *

Wystąpił błąd podczas próby wyszukania aplikacji na urządzeniu.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.

<a name="MT1024" />

### <a name="mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a>MT1024: Aplikacji nie można zweryfikować podpisu na urządzeniu "*". Upewnij się, że profil inicjowania obsługi administracyjnej jest zainstalowane i nie utracił ważności (błąd: 0xe8008017).

Urządzenie odrzucone instalacji aplikacji, ponieważ nie można zweryfikować podpisu.

Upewnij się, że profil inicjowania obsługi administracyjnej jest zainstalowane i nie wygasł.

<a name="MT1025" />

### <a name="mt1025-could-not-list-the-crash-reports-on-the-device-"></a>MT1025: Nie można utworzyć listy raporty awarii na urządzeniu *.

Wystąpił błąd podczas próby wyświetlić raporty awarii na urządzeniu.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<a name="MT1026" />

### <a name="mt1026-could-not-download-the-crash-report--from-the-device-"></a>MT1026: Nie można pobrać raport o awarii * z urządzenia *.

Wystąpił błąd podczas próby pobrania raporty awarii z urządzenia.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<a name="MT1027" />

### <a name="mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a>MT1027: Nie można użyć Xcode 7 + do uruchamiania aplikacji na urządzeniach z systemem iOS * (Xcode 7 obsługuje tylko system iOS 6 i nowsze).

Nie jest możliwe uruchamianie aplikacji na urządzeniach z wersją systemu iOS poniżej 6.0 Xcode 7 +.

Użyj starszej wersji programu Xcode, lub naciśnij w aplikacji ręcznie, aby uruchomić go.

<a name="MT1028" />

### <a name="mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a>MT1028: Nieprawidłowe urządzenie specyfikacji: "*". Oczekiwano "ios", "watchos" lub "all".

Specyfikacja urządzenia przekazany za pomocą — urządzenie jest nieprawidłowy. Prawidłowe wartości to: "ios", "watchos" lub "all".

<a name="MT1029" />

### <a name="mt1029-could-not-find-an-application-at-the-specified-directory-"></a>MT1029: Nie można odnaleźć aplikacji w określonym katalogu: *

Przekazany do--launchdev ścieżka aplikacji nie istnieje. Określ pakiet prawidłowej aplikacji.

<a name="MT1030" />

### <a name="mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a>MT1030: Uruchamianie aplikacji na urządzeniu za pomocą identyfikatora pakietu jest przestarzały. Przekaż pełną ścieżkę do pakietu do uruchomienia.

Zaleca się Przekaż ścieżka do aplikacji, które można uruchomić na urządzeniu, a nie tylko identyfikator pakietu.

<a name="MT1031" />

### <a name="mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1031: Nie można uruchomić aplikacji "\*"na urządzeniu"\*", ponieważ urządzenie jest zablokowane. Odblokuj urządzenie i spróbuj ponownie.

Odblokuj urządzenie i spróbuj ponownie.

<a name="MT1032" />

### <a name="mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a>MT1032: Ten plik wykonywalny aplikacji może być zbyt duży (* MB) można wykonać na urządzeniu. Jeśli włączono kodu bitowego można wyłączyć tę funkcję do tworzenia aplikacji, jest wymagana tylko do przesyłania aplikacji do firmy Apple.

<a name="MT1033" />

### <a name="mt1033-could-not-uninstall-the-application--from-the-device--"></a>MT1033: Nie można odinstalować aplikacji "\*"z urządzenia"\*": *

<!-- 1034 used by mmp -->

<a name="MT1035" />

### <a name="mt1035-cannot-include-different-versions-of-the-framework-name"></a>MT1035: Nie może zawierać różne wersje platformy "{name}"

Nie jest możliwe do łączenia się z różnymi wersjami tego samego framework.

Dzieje się tak zazwyczaj, gdy rozszerzenie odwołuje się do innej wersji framework niż aplikacji kontenera (prawdopodobnie przy użyciu zestawu powiązań innych firm).

Po tym błędzie będzie wiele [MT1036](#MT1036) błędy wyświetlania listy ścieżek dla każdego innego framework.

<a name="MT1036" />

### <a name="mt1036-framework-name-included-from-path-related-to-previous-error"></a>MT1036: Framework "{name}" uwzględniona, korzystając z: {path} (powiązane z poprzednim błędem)

Ten błąd jest zgłaszany tylko razem z [MT1036](#MT1036). Zobacz [MT1036](#MT1036) Aby uzyskać więcej informacji.

### <a name="mt11xx-debug-service"></a>MT11xx: Debugowanie

<!--
  MT11xx DebugService.cs
  -->

<a name="MT1101" />

### <a name="mt1101-could-not-start-app"></a>MT1101: Nie można uruchomić aplikacji

<a name="MT1102" />

### <a name="mt1102-could-not-attach-to-the-app-to-kill-it-"></a>MT1102: Nie można dołączyć do aplikacji (w celu zakończenia jego): *

<a name="MT1103" />

### <a name="mt1103-could-not-detach"></a>MT1103: Nie można odłączyć

<a name="MT1104" />

### <a name="mt1104-failed-to-send-packet-"></a>MT1104: Nie można wysłać pakietu: *

<a name="MT1105" />

### <a name="mt1105-unexpected-response-type"></a>MT1105: Typ nieoczekiwaną odpowiedź

<a name="MT1106" />

### <a name="mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a>MT1106: Nie można pobrać listy aplikacji na urządzeniu: Upłynął limit czasu żądania.

<a name="MT1107" />

### <a name="mt1107-application-failed-to-launch-"></a>MT1107: Nie można uruchomić aplikacji: *

Sprawdź, czy urządzenie jest zablokowane.

Jeśli wdrażana aplikacja przedsiębiorstwa lub przy użyciu wolnego profilu inicjowania obsługi administracyjnej, może mieć zaufanie do projektanta (jest to wyjaśniono <a href="http://stackoverflow.com/a/30726375/183422">tutaj</a>).

<a name="MT1108" />

### <a name="mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a>MT1108: Nie można odnaleźć narzędzia deweloperskie dla tego urządzenia XX (RR).

Wymagaj kilka operacji mtouch <tt>DeveloperDiskImage.dmg</tt> pliku obecności.   Ten plik jest częścią środowiska Xcode i zazwyczaj znajduje się względem zestawu SDK, używanej do kompilacji przed, w <tt>Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg</tt>.

Ten błąd może się zdarzyć, albo nie masz DeveloperDiskImage.dmg, odpowiadający urządzeniu, który był wcześniej podłączany.

<a name="MT1109" />

### <a name="mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a>MT1109: Nie można uruchomić aplikacji, ponieważ urządzenie jest zablokowane. Odblokuj urządzenie i spróbuj ponownie.

Sprawdź, czy urządzenie jest zablokowane.

<a name="MT1110" />

### <a name="mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a>MT1110: Nie można uruchomić aplikacji z powodu ograniczeń zabezpieczeń systemu iOS. Upewnij się, że deweloper jest zaufany.

Jeśli wdrażana aplikacja przedsiębiorstwa lub przy użyciu wolnego profilu inicjowania obsługi administracyjnej, może mieć zaufanie do projektanta (jest to wyjaśniono <a href="http://stackoverflow.com/a/30726375/183422">tutaj</a>).

<a name="MT1111" />

### <a name="mt1111-application-launched-successfully-but-its-not-possible-to-wait-for-the-app-to-exit-as-requested-because-its-not-possible-to-detect-app-termination-when-launching-using-gdbserver"></a>MT1111: Aplikacja uruchomiona pomyślnie, ale nie jest możliwe oczekiwania dla aplikacji zakończyć zgodnie z żądaniem, ponieważ nie jest możliwe do wykrycia zakończenia aplikacji podczas uruchamiania przy użyciu gdbserver.

### <a name="mt12xx-simulator"></a>MT12xx: symulatora

<!--
  MT12xx simcontroller.cs
  -->

<a name="MT1201" />

### <a name="mt1201-could-not-load-the-simulator-"></a>MT1201: Nie można załadować symulator: *

<a name="MT1202" />

### <a name="mt1202-invalid-simulator-configuration-"></a>MT1202: Nieprawidłowy symulatora konfiguracji: *

<a name="MT1203" />

### <a name="mt1203-invalid-simulator-specification-"></a>MT1203: Nieprawidłowy symulatora specyfikacji: *

<a name="MT1204" />

### <a name="mt1204-invalid-simulator-specification--runtime-not-specified"></a>MT1204: Nieprawidłowy symulatora specyfikacji "*": nie określono środowiska uruchomieniowego.

<a name="MT1205" />

### <a name="mt1205-invalid-simulator-specification--device-type-not-specified"></a>MT1205: Nieprawidłowy symulatora specyfikacji "*": nie określono typu urządzenia.

<a name="MT1206" />

### <a name="mt1206-could-not-find-the-simulator-runtime-"></a>MT1206: Nie można znaleźć środowiska uruchomieniowego symulator "*".

<a name="MT1207" />

### <a name="mt1207-could-not-find-the-simulator-device-type-"></a>MT1207: Nie można odnaleźć typu urządzenia symulator "*".

<a name="MT1208" />

### <a name="mt1208-could-not-find-the-simulator-runtime-"></a>MT1208: Nie można znaleźć środowiska uruchomieniowego symulator "*".

<a name="MT1209" />

### <a name="mt1209-could-not-find-the-simulator-device-type-"></a>MT1209: Nie można odnaleźć typu urządzenia symulator "*".

<a name="MT1210" />

### <a name="mt1210-invalid-simulator-specification--unknown-key-"></a>MT1210: Nieprawidłowy symulatora specyfikacji: \*, nieznany klucz "\*"

<a name="MT1211" />

### <a name="mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a>MT1211: Wersja symulator "\*"nie obsługuje typu symulator"\*"

<a name="MT1212" />

### <a name="mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a>MT1212: Nie można utworzyć wersji symulatora wpisuje = * i środowiska uruchomieniowego = *.

<a name="MT1213" />

### <a name="mt1213-invalid-simulator-specification-for-xcode-4-"></a>MT1213: Nieprawidłowy symulatora specyfikacji Xcode 4: *

<a name="MT1214" />

### <a name="mt1214-invalid-simulator-specification-for-xcode-5-"></a>MT1214: Nieprawidłowy symulatora specyfikacji Xcode 5: *

<a name="MT1215" />

### <a name="mt1215-invalid-sdk-specified-"></a>MT1215: Nieprawidłowy zestaw SDK określona: *

<a name="MT1216" />

### <a name="mt1216-could-not-find-the-simulator-udid-"></a>MT1216: Nie można odnaleźć symulatora UDID "*".

<a name="MT1217" />

### <a name="mt1217-could-not-load-the-app-bundle-at-"></a>MT1217: Nie można załadować pakietu aplikacji na "*".

<a name="MT1218" />

### <a name="mt1218-no-bundle-identifier-found-in-the-app-at-"></a>MT1218: Nie identyfikatora pakietu znaleziono w aplikacji na "*".

<a name="MT1219" />

### <a name="mt1219-could-not-find-the-simulator-for-"></a>MT1219: Nie można odnaleźć symulatora dla "*".

<a name="MT1220" />

### <a name="mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a>MT1220: Nie można znaleźć najnowsze środowisko uruchomieniowe symulatora dla urządzenia "*".

Zwykle oznacza to problem z Xcode.

Co należy zrobić rozwiązać ten problem:

* Użyj symulator raz w środowisku Xcode.
* Przekaż jawne zestawu SDK w wersji przy użyciu zestawu sdk — <version>.
* Zainstaluj ponownie Xcode.

<a name="MT1221" />

### <a name="mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a>MT1221: Nie można odnaleźć symulatora sparowanego iPhone symulatora WatchOS "*".

Podczas uruchamiania aplikacji przez WatchOS w symulatorze WatchOS, musi być sparowanego symulatora systemu iOS oraz.

Oglądania symulatorów mogą łączyć się z systemem iOS przy użyciu interfejsu użytkownika urządzenia w środowisku Xcode symulatorów (menu okna -> urządzenia).

### <a name="mt13xx-linkwith"></a>MT13xx: [LinkWith]

<!--
  MT13xx [LinkWith]
  -->

<a name="MT1301" />

### <a name="mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a>MT1301: Natywnej biblioteki `*` (\*) została zignorowana, ponieważ nie pasuje do bieżącego architecture(s) kompilacji (\*)

<a name="MT1302" />

### <a name="mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a>MT1302: Nie można wyodrębnić natywnej biblioteki ' *' z '+'. Upewnij się, że natywnej biblioteki został poprawnie osadzony w zestaw zarządzany (Jeśli zestaw został utworzony za pomocą programu project powiązania natywnej biblioteki musi być dołączony do projektu i jego Akcja kompilacji musi być "ObjcBindingNativeLibrary").

<a name="MT1303" />

### <a name="mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a>MT1303: Nie można zdekompresować natywnego framework "\*"od"\*". Przejrzyj dziennik kompilacji, aby uzyskać więcej informacji polecenia natywnego zip.

Nie można zdekompresować określonego framework macierzystego z biblioteki powiązania.

Przejrzyj dziennik bulid, aby uzyskać więcej informacji o tym niepowodzeniu polecenia natywnego zip.

<a name="MT1304" />

### <a name="mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a>MT1304: Osadzone platformę "*" w * jest nieprawidłowy: nie zawiera pliku Info.plist.

Określony framework embedded nie zawiera Info.plist i w związku z tym nie jest prawidłową framework.

Upewnij się, że w ramach jest prawidłowy.

<a name="MT1305" />

### <a name="mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a>MT1305: Biblioteka powiązania "\*" zawiera framework użytkownika (\*), ale struktury osadzone użytkownika wymaga systemu iOS 8.0 (bieżącą lokalizację docelową wdrożenia jest *). W pliku Info.plist do co najmniej 8.0 Ustaw cel wdrożenia.

Biblioteka określone powiązanie zawiera osadzony framework, ale Xamarin.iOS obsługuje tylko struktury osadzone w systemie iOS 8.0 lub nowszej.

Ustaw cel wdrożenia w pliku Info.plist do co najmniej 8.0, aby poprawić ten błąd (lub nie jest używany osadzony Platform).

### <a name="mt14xx-crash-reports"></a>MT14xx: Raporty awarii

<!--
  MT14xx    CrashReports.cs
  -->

<a name="MT1400" />

### <a name="mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a>MT1400: Nie można otworzyć usługi raport awarii: AFCConnectionOpen zwrócił *

Wystąpił błąd podczas próby uzyskania dostępu raporty awarii z urządzenia.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<a name="MT1401" />

### <a name="mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a>MT1401: Nie można zamknąć usługi raport awarii: AFCConnectionClose zwrócił *

Wystąpił błąd podczas próby uzyskania dostępu raporty awarii z urządzenia.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<a name="MT1402" />

### <a name="mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a>MT1402: Nie można odczytać informacji o pliku dla *: AFCFileInfoOpen zwrócił *

Wystąpił błąd podczas próby uzyskania dostępu raporty awarii z urządzenia.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<a name="MT1403" />

### <a name="mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a>MT1403: Nie można odczytać raport o awarii: AFCDirectoryOpen (*) zwrócił: *

Wystąpił błąd podczas próby uzyskania dostępu raporty awarii z urządzenia.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<a name="MT1404" />

### <a name="mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a>MT1404: Nie można odczytać raport o awarii: AFCFileRefOpen (*) zwrócił: *

Wystąpił błąd podczas próby uzyskania dostępu raporty awarii z urządzenia.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<a name="MT1405" />

### <a name="mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a>MT1405: Nie można odczytać raport o awarii: AFCFileRefRead (*) zwrócił: *

Wystąpił błąd podczas próby uzyskania dostępu raporty awarii z urządzenia.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<a name="MT1406" />

### <a name="mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a>MT1406: Nie można utworzyć listy raportów awarii: AFCDirectoryOpen (*) zwrócił: *

Wystąpił błąd podczas próby uzyskania dostępu raporty awarii z urządzenia.

Co należy zrobić rozwiązać ten problem:

* Usuń aplikację z urządzenia i spróbuj ponownie.
* Odłącz urządzenia i połącz go ponownie.
* Ponownego uruchomienia urządzenia.
* Ponowny rozruch komputerów Mac.
* Synchronizowanie urządzenia z iTunes (spowoduje to usunięcie wszystkich raportów awarii z urządzenia).

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

<a name="MT1600" />

### <a name="mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1600: Nie cenę O dynamicznej biblioteki (Nieznany nagłówek "0 x *"): *.

Wystąpił błąd podczas przetwarzania dynamiczne w bibliotece.

Upewnij się dynamicznej biblioteki jest prawidłową biblioteką dynamiczne O cenę.

Format biblioteki można sprawdzić za pomocą `file` polecenia z terminala:

    file -arch all -l /path/to/library.dylib

<a name="MT1601" />

### <a name="mt1601-not-a-static-library-unknown-header--"></a>MT1601: Nie biblioteki statycznej (Nieznany nagłówek "*"): *.

Wystąpił błąd podczas przetwarzania z biblioteką statyczną.

Upewnij się, że biblioteka statyczna jest prawidłowy biblioteki statycznej O cenę.

Format biblioteki można sprawdzić za pomocą `file` polecenia z terminala:

    file -arch all -l /path/to/library.a

<a name="MT1602" />

### <a name="mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a>MT1602: Nie cenę O dynamicznej biblioteki (Nieznany nagłówek "0 x *"): *.

Wystąpił błąd podczas przetwarzania dynamiczne w bibliotece.

Upewnij się dynamicznej biblioteki jest prawidłową biblioteką dynamiczne O cenę.

Format biblioteki można sprawdzić za pomocą `file` polecenia z terminala:

    file -arch all -l /path/to/library.dylib

<a name="MT1603" />

### <a name="mt1603-unknown-format-for-fat-entry-at-position--in-"></a>MT1603: Nieznany format wpisu fat na pozycji * w *.

Wystąpił błąd podczas przetwarzania w archiwum fat.

Upewnij się, że fat archiwum jest nieprawidłowa.

Format fat archiwum można sprawdzić za pomocą `file` polecenia z terminala:

    file -arch all -l /path/to/file

<a name="MT1604" />

### <a name="mt1604-file-of-type--is-not-a-macho-file-"></a>MT1604: Plik typu * nie jest plikiem MachO (*).

Wystąpił błąd podczas przetwarzania pliku MachO zagrożona.

Upewnij się, że plik jest prawidłową biblioteką dynamiczne O cenę.

Format pliku można sprawdzić za pomocą `file` polecenia z terminala:

    file -arch all -l /path/to/file

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: Komunikaty o błędach konsolidatora

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

<a name="MT2001" />

### <a name="mt2001-could-not-link-assemblies"></a>MT2001: Nie można połączyć zestawów

Ten błąd oznacza, że konsolidator zarządzanych napotkał nieoczekiwany błąd, np. Wystąpił wyjątek i nie można ukończyć lub zapisać zestawu przetwarzane. Więcej informacji na temat dokładnego błąd będą stanowić część dziennika kompilacji, np.

```
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

Należy do pliku raport o usterce takich problemów. W przypadku większości obejście tego problemu można podać do momentu opublikowania poprawki właściwe. Powyższe informacje są krytyczne (wraz z przypadkiem testowym i/lub binairy zestawu) Aby rozwiązać ten problem.

<a name="MT2002" />

### <a name="mt2002-can-not-resolve-reference-"></a>MT2002: Nie można rozpoznać odwołania: *

<a name="MT2003" />

### <a name="mt2003-option--will-be-ignored-since-linking-is-disabled"></a>MT2003: Opcja "*" zostanie zignorowany, ponieważ połączenie jest wyłączona

<a name="MT2004" />

### <a name="mt2004-extra-linker-definitions-file--could-not-be-located"></a>MT2004: Plik definicji konsolidatora dodatkowy "*" nie można zlokalizować.

<a name="MT2005" />

### <a name="mt2005-definitions-from--could-not-be-parsed"></a>MT2005: Definicji "*" nie można przeanalizować.

<a name="MT2006" />

### <a name="mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a>MT2006: Nie można załadować pliku mscorlib.dll z: *. Zainstaluj ponownie platformy Xamarin.iOS.

Zwykle oznacza to, że jest problem z instalacją platformy Xamarin.iOS. Spróbuj zainstalować ponownie platformy Xamarin.iOS.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

<a name="MT2010" />

### <a name="mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MT2010: Nieznana klasa HttpMessageHandler `*`. Prawidłowe wartości to HttpClientHandler (ustawienie domyślne), CFNetworkHandler lub NSUrlSessionHandler

<a name="MT2011" />

### <a name="mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a>MT2011: Nieznany TlsProvider `*`.  Prawidłowe wartości to domyślne lub appletls.

Wartość `tls-provider=` nie jest prawidłowym dostawcą TLS (Transport Layer Security).

`default` i `appletls` są jedyne prawidłowe wartości, a oba reprezentują tego samego opcja, która jest zapewnienie obsługi protokołu SSL/TLS za pomocą natywnego interfejsu API TLS firmy Apple.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

<a name="MT2015" />

### <a name="mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a>MT2015: Nieprawidłowa klasa HttpMessageHandler `*` dla watchOS. Jedyne prawidłowe wartości to NSUrlSessionHandler.

To ostrzeżenie, że występuje, ponieważ plik projektu określa nieprawidłowy HttpMessageHandler.

Starsze niż narzędziach firmy Microsoft w wersji zapoznawczej generowany domyślnie nieprawidłową wartość w pliku projektu.

Aby usunąć to ostrzeżenie, otwórz plik projektu w edytorze tekstów i usuwania wszystkich węzłów HttpMessageHandler z pliku XML.

<a name="MT2016" />

### <a name="mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a>MT2016: Nieprawidłowy TlsProvider `legacy` opcji. Jedyną prawidłową wartością `appletls` będą używane.

`legacy` Dostawcy, który został w pełni zarządzana protokołów SSLv3 / TLSv1 jedyny dostawca nie jest wysyłany z Xamarin.iOS już. Projekty, które zostały przy użyciu tego dostawcy stary i teraz kompilacji z nowszej `appletls` jeden.

Aby usunąć to ostrzeżenie, otwórz plik projektu w edytorze tekstów i usunięcie wszystkich "MtouchTlsProvider'' węzłów z pliku XML.

<a name="MT2017" />

### <a name="mt2017-could-not-process-xml-description"></a>MT2017: Nie można przetworzyć opisu XML.

Oznacza to, występuje błąd na [niestandardowego pliku konfiguracji konsolidatora XML](https://developer.xamarin.com/guides/cross-platform/advanced/custom_linking/) została podana, przejrzyj plik.

<a name="MT2018" />

### <a name="mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a>MT2018: Zestaw '\*"odwołuje się z dwóch różnych lokalizacji:"\*' i ' *'.

Zestaw określonych w komunikacie o błędzie jest załadowany z wielu lokalizacji. Upewnij się, że zawsze używać tej samej wersji zestawu.

<a name="MT2019" />

### <a name="mt2019-can-not-load-the-root-assembly-"></a>MT2019: Nie można załadować zestawu głównego "*"

Nie można załadować zestawu głównego. Sprawdź, czy ścieżka w komunikacie o błędzie odwołuje się do istniejącego pliku i że jest prawidłowy zestaw .NET.

<a name="MT202x" />

### <a name="mt202x-binding-optimizer-failed-processing-"></a>MT202x: Powiązanie optymalizatora nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby zoptymalizować wygenerowany kod powiązania. Element przyczyną problemu jest nazywane komunikat o błędzie. Aby rozwiązać ten problem, zestaw o nazwie (lub zawierający typ lub metoda o nazwie) musi znajdować się w [usterek — raport](http://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

Ostatnia cyfra `x` będzie:
* `0` dla nazwy zestawu;
* `1` Nazwa typu;
* `3` dla nazwy metody;

<a name="MT2030" />

### <a name="mt2030-remove-user-resources-failed-processing-"></a>MT2030: Usuń użytkownika zasobów nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby usunięcia zasobów użytkownika. W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem zestawu będzie musiał podać w [usterek — raport](http://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

Zasoby użytkownika są pliki uwzględnione w zestawy (jako zasoby), które ma zostać wyodrębniony w czasie kompilacji, aby utworzyć pakiet aplikacji. Możliwości obejmują:

* `__monotouch_content_*` i `__monotouch_pages_*` zasobów; i
* Natywnych bibliotek osadzone wewnątrz zestawu powiązania;

<a name="MT2040" />

### <a name="mt2040-default-httpmessagehandler-setter-failed-processing-"></a>MT2040: HttpMessageHandler domyślne metody ustawiającej nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby ustawienia domyślne `HttpMessageHandler` dla aplikacji. Zgłoś [usterek — raport](http://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2050" />

### <a name="mt2050-code-remover-failed-processing-"></a>MT2050: Usuwająca została kodu nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby usunięcia kodu z BCL wysyłania z aplikacją. Zgłoś [usterek — raport](http://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2060" />

### <a name="mt2060-sealer-failed-processing-"></a>MT2060: Sealer nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby zapieczętować typach lub metodach (końcowego) lub devirtualizing niektóre metody. W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem zestawu będzie musiał podać w [usterek — raport](http://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2070" />

### <a name="mt2070-metadata-reducer-failed-processing-"></a>MT2070: Reduktor metadanych nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby zmniejszenia metadanych z aplikacji. W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem zestawu będzie musiał podać w [usterek — raport](http://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2080" />

### <a name="mt2080-marknsobjects-failed-processing-"></a>MT2080: MarkNSObjects nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby oznaczyć `NSObject` podklasy z aplikacji. W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem zestawu będzie musiał podać w [usterek — raport](http://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2090" />

### <a name="mt2090-inliner-failed-processing-"></a>MT2090: Inliner nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby wbudowany kod z aplikacji. W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem, zestaw musi znajdować się w [usterek — raport](https://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

<a name="MT2100" />

### <a name="mt2100-smart-enum-conversion-preserver-failed-processing-"></a>MT2100: Inteligentne Preserver konwersji wyliczenia nie powiodło się przetwarzanie `...`.

Nieoczekiwane wystąpił podczas próby oznaczyć metody konwersji inteligentne wyliczenia z aplikacji. W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem, zestaw musi znajdować się w [usterek — raport](https://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2101" />

### <a name="mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a>MT2101: Nie można rozpoznać odwołania "\*", do którego istnieje odwołanie z metody"\*" w "*".

Nieprawidłowe odwołanie do zestawu podczas przetwarzania metody wymienionych w komunikacie o błędzie.

W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem zestawu będzie musiał podać w [usterek — raport](https://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2102" />

### <a name="mt2102-error-processing-the-method--in-the-assembly--"></a>MT2102: Błąd podczas przetwarzania metody "\*"w zestawie"\*": *

Nieoczekiwane wystąpił podczas próby Oznacz metodę wymienionych w komunikacie o błędzie.

W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem zestawu będzie musiał podać w [usterek — raport](https://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2103" />

### <a name="mt2103-error-processing-assembly--"></a>MT2103: Błąd podczas przetwarzania zestawu '\*': *

Wystąpił nieoczekiwany błąd podczas przetwarzania zestawu.

W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem, zestaw musi znajdować się w [usterek — raport](https://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MT2104" />

### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: Nie można połączyć z zestawu '{0}' ponieważ jest on trybu mieszanego.

Zestawy mieszane nie mogą być przetwarzane przez konsolidator.

Zobacz https://msdn.microsoft.com/library/x0w2664k.aspx uzyskać więcej informacji dotyczących trybu mieszanego zestawów.

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: Komunikaty o błędach drzewa obiektów aplikacji

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

<a name="MT3001" />

### <a name="mt3001-could-not-aot-the-assembly-"></a>MT3001: Drzewa nie obiektów aplikacji zestawu można "*"

Oznacza to błąd w kompilatorze drzewa obiektów aplikacji. Prosimy o usterce [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) z projektem, który służy do odtwarzania błędu.

Czasami jest możliwe obejść ten problem przez wyłączenie kompilacje przyrostowe w projektu iOS opcji kompilacji (, ale nadal jest usterki, dlatego należy zgłaszać mimo wszystko).

<a name="MT3002" />

### <a name="mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-httpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbackshttpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbacks"></a>MT3002: Ograniczenie drzewa obiektów aplikacji: metoda "*" musi być statyczna, ponieważ ma ona [MonoPInvokeCallback]. Zobacz [https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks)

Ten komunikat o błędzie pochodzi z kompilatora drzewa obiektów aplikacji.

<a name="MT3003" />

### <a name="mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a>MT3003: Konflikt--opcje debugowania i--llvm. Debugowanie soft jest wyłączone.

Debugowanie nie jest obsługiwane, gdy LLVM jest włączona. Jeśli potrzebujesz do debugowania aplikacji, najpierw wyłącz LLVM.

<a name="MT3004" />

### <a name="mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a>MT3004: Można drzewa nie obiektów aplikacji zestawu ' *' ponieważ nie istnieje.

<a name="MT3005" />

### <a name="mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a>MT3005: Zależność "\*'z zestawu'\*' nie został znaleziony. Zapoznaj się z tematem odwołania do projektu.

Dzieje się tak zazwyczaj, gdy inna wersja zestawu platformy (zazwyczaj .NET 4 wersja pliku mscorlib.dll) odwołuje się do zestawu.

To nie jest obsługiwana i może się nie kompilacji lub wykonania prawidłowo (zestaw może użyć interfejsu API w wersji .NET 4 mscorlib.dll, który nie ma wersji platformy Xamarin.iOS).

<a name="MT3006" />

### <a name="mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a>MT3006: Nie można obliczyć mapy pełną zależności dla projektu. To spowoduje wolniejsze czasy kompilacji ponieważ Xamarin.iOS prawidłowo nie może wykryć, co wymaga odbudowania (i co nie jest konieczne odbudowanie). Przejrzyj poprzednie ostrzeżenia, aby uzyskać więcej informacji.

 kompilacji lub wykonania prawidłowo (zestaw może użyć interfejsu API w wersji .NET 4 mscorlib.dll, który nie ma wersji platformy Xamarin.iOS).

<a name="MT3007" />

### <a name="mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a>MT3007: Pliki informacji debugowania (*.mdb) nie będą ładowane, gdy llvm jest włączona.

<a name="MT3008" />

### <a name="mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a>MT3008: Obsługi kodu bitowego wymaga użycia zaplecza LLVM drzewa obiektów aplikacji (--llvm)

Obsługa kodu bitowego wymaga użycia zaplecza LLVM drzewa obiektów aplikacji (--llvm).

Wyłącz obsługę kodu bitowego albo Włącz LLVM.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: Komunikaty o błędach generowania kodu

### <a name="mt40xx-main"></a>MT40xx: Main

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

<a name="MT4001" />

### <a name="mt4001-the-main-template-could-not-be-expanded-to-"></a>MT4001: Szablonu głównego nie można rozszerzyć do `*`.

Wystąpił błąd podczas generowania main.m. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4002" />

### <a name="mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4002: Nie można skompilować wygenerowany kod dla metody P/Invoke. Raport o usterce w pliku http://bugzilla.xamarin.com

Nie można skompilować wygenerowany kod dla metody P/Invoke. Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt41xx-registrar"></a>MT41xx: rejestratora

<!--
  MT41xx registrar.m
  -->

<a name="MT4101" />

### <a name="mt4101-the-registrar-cannot-build-a-signature-for-type-"></a>MT4101: Rejestrator nie można utworzyć podpisu dla typu `*`.

Typ został znaleziony w wyeksportowanego interfejsu API, który środowiska uruchomieniowego nie wiadomo, jak kierować z Objective-C.

Jeśli Twoim zdaniem Xamarin.iOS powinna obsługiwać danego typu pliku żądania rozszerzenie [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4102" />

### <a name="mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a>MT4102: Rejestratora znaleziono nieprawidłowy typ `*` w podpisie metody `*`. Zamiast nich należy używać słów kluczowych `*`.

Obecnie tylko dzieje się to z jednego typu: System.DateTime. Zamiast tego użyj odpowiednikiem Objective-C (NSDate).

<a name="MT4103" />

### <a name="mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a>MT4103: Rejestratora znaleziono nieprawidłowy typ `*` w podpisie metody `*`: typ implementuje INativeObject, ale nie ma konstruktora, który przyjmuje dwa (IntPtr, wartość logiczna) argumentów

Dzieje się tak, gdy rejestratora wystąpienia typu w sygnaturze o charakterystyce opisane powyżej. Rejestratora może być konieczne utworzenie nowych wystąpień tego typu, i w takim przypadku wymaga konstruktora z (IntPtr, wartość logiczna) podpis — pierwszy argument (IntPtr) określa zarządzanych dojście podczas drugiego, jeśli element wywołujący przekazuje własność natywnego Obsługa (Jeśli ta wartość wynosi false, "Zachowaj" zostanie wywołana dla obiektu).

<a name="MT4104" />

### <a name="mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a>MT4104: Rejestratora nie można zorganizować typu wartość zwracana `*` w podpisie metody `*`.

Typ został znaleziony w wyeksportowanego interfejsu API, który środowiska uruchomieniowego nie wiadomo, jak kierować z Objective-C.

Jeśli Twoim zdaniem Xamarin.iOS powinna obsługiwać danego typu pliku żądania rozszerzenie [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4105" />

### <a name="mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4105: Rejestrator nie można zorganizować parametr typu `*` w podpisie metody `*`.

Jeśli Twoim zdaniem Xamarin.iOS powinna obsługiwać danego typu pliku żądania rozszerzenie [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4106" />

### <a name="mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a>MT4106: Rejestrator nie można zorganizować wartość zwracaną dla struktury `*` w podpisie metody `*`.

Typ został znaleziony w wyeksportowanego interfejsu API, który środowiska uruchomieniowego nie wiadomo, jak kierować z Objective-C.

Jeśli Twoim zdaniem Xamarin.iOS powinna obsługiwać danego typu pliku żądania rozszerzenie [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4107" />

### <a name="mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a>MT4107: Rejestrator nie można zorganizować parametr typu `*` w podpisie metody `+`.

Typ został znaleziony w wyeksportowanego interfejsu API, który środowiska uruchomieniowego nie wiadomo, jak kierować z Objective-C.

Jeśli Twoim zdaniem Xamarin.iOS powinna obsługiwać danego typu pliku żądania rozszerzenie [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4108" />

### <a name="mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a>MT4108: Rejestrator nie można pobrać typu ObjectiveC dla typu zarządzanego `*`.

Typ został znaleziony w wyeksportowanego interfejsu API, który środowiska uruchomieniowego nie wiadomo, jak kierować z Objective-C.

Jeśli Twoim zdaniem Xamarin.iOS powinna obsługiwać danego typu pliku żądania rozszerzenie [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com).

<a name="MT4109" />

### <a name="mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4109: Nie można skompilować kodu wygenerowanego rejestratora. Raport o usterce w pliku http://bugzilla.xamarin.com

Nie można skompilować wygenerowany kod dla rejestratora. Dziennik kompilacji będzie zawierać dane wyjściowe z macierzystego kompilatora wyjaśniający, dlaczego nie jest kompilowanie kodu.

Jest to zawsze na usterkę w Xamarin.iOS; Raport o usterce w pliku [ http://bugzilla.xamarin.com ](http://bugzilla.xamarin.com) z projektu lub przypadkiem testowym.

<a name="MT4110" />

### <a name="mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a>MT4110: Rejestrator nie można zorganizować parametr wyjściowy typu `*` w podpisie metody `*`.

<a name="MT4111" />

### <a name="mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a>MT4111: Rejestrator nie można utworzyć podpisu dla typu `*` w metodzie `*`.

<a name="MT4112" />

### <a name="mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvancedtopicsregistrarhttpsdeveloperxamarincomguidesiosadvancedtopicsregistrar-for-more-information"></a>MT4112: Rejestratora znaleziono nieprawidłowy typ `*`. Rejestrowanie typów ogólnych w języku Objective C nie jest obsługiwana i może prowadzić do losowe działania i/lub awarie (dla wstecz zgodność ze starszymi wersjami programu Xamarin.iOS jest możliwe do zignorowania tego błędu, przekazując `--unsupported--enable-generics-in-registrar` jako dodatkowe mtouch argument w strony Opcje kompilacji systemu iOS projektu. Zobacz [developer.xamarin.com/guides/ios/advanced_topics/registrar](https://developer.xamarin.com/guides/ios/advanced_topics/registrar) Aby uzyskać więcej informacji).

<a name="MT4113" />

### <a name="mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a>MT4113: Metoda ogólna można odnaleźć rejestratora: "\*.\*". Eksportowanie metody ogólne nie jest obsługiwane i spowoduje losowe działania i/lub awarii (Crash).

<a name="MT4114" />

### <a name="mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4114: Wystąpił nieoczekiwany błąd w rejestratora dla metody "\*.\*" — raport o usterce w pliku http://bugzilla.xamarin.com

<a name="MT4116" />

### <a name="mt4116-could-not-register-the-assembly--"></a>MT4116: Nie można zarejestrować zestawu "*": *

<a name="MT4117" />

### <a name="mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4117: Znaleziono rejestratora niezgodność podpisu w metodzie "*.*" — selektor wskazuje metoda korzysta z * parametrów, gdy zarządzana metoda ma * parametrów.

<a name="MT4118" />

### <a name="mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a>MT4118: Nie można zarejestrować dwa typy zarządzane ("\*"i"\*") o takiej samej nazwie natywnego ("*").

<a name="MT4119" />

### <a name="mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a>MT4119: Nie można zarejestrować selektor "\*"elementu członkowskiego"\*. *", ponieważ selektor już jest zarejestrowany na inny element członkowski.

<a name="MT4120" />

### <a name="mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4120: Rejestratora znaleziono nieznany typ pola "\*"w polu"\*. *". Raport o usterce w pliku http://bugzilla.xamarin.com

Ten błąd wskazuje na usterkę w platformy Xamarin.iOS. Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4121" />

### <a name="mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a>MT4121: Nie można użyć GCC / G ++ do kompilowania wygenerowanego kodu z statycznych rejestratora po przy użyciu platformy kont (pliki nagłówkowe dostarczonymi przez firmę Apple, używane podczas kompilowania wymagają Clang). Użyj Clang (--kompilatora: clang) lub dynamiczny rejestratora (--rejestratora: dynamiczne).

<a name="MT4122" />

### <a name="mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a>MT4122: Nie można użyć kompilatora Clang w *.* Zestaw SDK do kompilowania wygenerowanego kodu z statycznych rejestratora po innych niż ASCII nazwy typów ("*") znajdują się w aplikacji. Użyj GCC / G ++ (--kompilatora: gcc | g ++), dynamiczne rejestratora (— rejestratora: dynamiczne) lub nowszych zestawu SDK.

<a name="MT4123" />

### <a name="mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a>MT4123: Typ parametru ze zmienną liczbą argumentów ze zmienną liczbą argumentów funkcji ' *' musi być System.IntPtr.

<a name="MT4124" />

### <a name="mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4124: Nieprawidłowa * na "*". Raport o usterce w pliku http://bugzilla.xamarin.com

Ten błąd wskazuje na usterkę w platformy Xamarin.iOS. Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4125" />

### <a name="mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a>MT4125: Rejestratora znaleziono nieprawidłowy typ "\*"w podpisie metody"\*": interfejs musi mieć atrybut protokołu określanie jego typ otoki.

<a name="MT4126" />

### <a name="mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a>MT4126: Nie można zarejestrować dwóch protokołów zarządzanego ("\*"i"\*") o takiej samej nazwie natywnego ("*").

<a name="MT4127" />

### <a name="mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a>MT4127: Nie można zarejestrować więcej niż jednej metody interfejsu dla metody "\*" (która implementuje "\*").

<a name="MT4128" />

### <a name="mt4128-the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a>MT4128: Rejestratora znaleziono nieprawidłowy ogólny typ parametru "\*"w metodzie"\*". Parametr musi mieć ograniczenie "NSObject".

<a name="MT4129" />

### <a name="mt4129-the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a>MT4129: Rejestratora znaleziono nieprawidłowy typ zwracany ogólny "\*"w metodzie"\*". Zwracany typ ogólny musi mieć ograniczenie "NSObject".

<a name="MT4130" />

### <a name="mt4130-the-registrar-cannot-export-static-methods-in-generic-classes-"></a>MT4130: Rejestrator nie można wyeksportować metod statycznych klas ogólnych ("*").

<a name="MT4131" />

### <a name="mt4131-the-registrar-cannot-export-static-properties-in-generic-classes-"></a>MT4131: Rejestrator nie można wyeksportować statycznej właściwości klasy ogólne ("\*.\*").

<a name="MT4132" />

### <a name="mt4132-the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a>MT4132: Rejestratora znaleziono nieprawidłowy typ zwracany ogólny "\*"we właściwości"\*". Zwracany typ musi mieć ograniczenie "NSObject".

<a name="MT4133" />

### <a name="mt4133-cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a>MT4133: Nie można utworzyć wystąpienia typu "*" w języku Objective-C, ponieważ typ jest rodzajowy. [Wyjątek środowiska wykonawczego]

<a name="MT4134" />

### <a name="mt4134-your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a>MT4134: Aplikacja korzysta ' *' struktury, która nie jest zawarte w systemie iOS SDK używany do utworzenia aplikacji (platforma została wprowadzona w systemie iOS *, podczas gdy tworzysz iOS * zestawu SDK.) Wybierz nowsze zestawu SDK w aplikacji systemu iOS opcje kompilacji.

<a name="MT4135" />

### <a name="mt4135-the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a>MT4135: Element członkowski "\*.\*" ma atrybut eksportu, która nie określa selektora. Wymagany jest selektora.

<a name="MT4136" />

### <a name="mt4136-the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a>MT4136: Rejestrator nie można zorganizować typu parametru "\*"parametru"\*"w metodzie"\*. *"

<!-- MT4137 is unused -->

<a name="MT4138" />

### <a name="mt4138-the-registrar-cannot-marshal-the-property-type--of-the-property-"></a>MT4138: Rejestrator nie można zorganizować typu właściwości '\*"właściwości"\*. * ".

<a name="MT4139" />

### <a name="mt4139-the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a>MT4139: Rejestrator nie można zorganizować typu właściwości '\*"właściwości"\*. * ". Właściwości z atrybutem [Connect] musi mieć właściwość typu NSObject (lub podklasa klasy NSObject).

<a name="MT4140" />

### <a name="mt4140-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a>MT4140: Rejestratora znaleziono niezgodność podpisu w metodzie "*.*"-selektor wskazuje metoda ze zmienną liczbą argumentów przyjmuje * parametrów, gdy zarządzana metoda ma * parametrów.

<a name="MT4141" />

### <a name="mt4141-cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a>MT4141: Nie można zarejestrować selektor "\*"w elemencie członkowskim"\*" ponieważ Xamarin.iOS niejawnie rejestruje ten selektor.

Ten błąd występuje podczas tworzenie podklas typu framework, a próba wykonania "zachować", "release" lub "dealloc" metody:

```csharp
class MyNSObject : NSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

Jest jednak możliwe do zastąpienia tych metod, jeśli klasa nie jest pierwszym podklasy struktury, wpisz:

```csharp
class MyNSObject : NSObject
{
}

class MyCustomNSObject : MyNSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

W takim przypadku przesłoni Xamarin.iOS `retain`, `release` i `dealloc` na `MyNSObject` klasy i nie było konfliktu.

<a name="MT4142" />

### <a name="mt4142-failed-to-register-the-type-"></a>MT4142: Nie można zarejestrować typ "*".

<a name="MT4143" />

### <a name="mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a>MT4143: Klasa ObjectiveC ' *' może nie być zarejestrowany, nie wydaje się pochodzić z żadnych znanych klasy ObjectiveC (w tym NSObject).

<a name="MT4144" />

### <a name="mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4144: Nie można zarejestrować metody ' *', ponieważ nie ma skojarzonego trampoline. Raport o usterce w pliku http://bugzilla.xamarin.com.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4145" />

### <a name="mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a>MT4145: Nieprawidłowy wyliczenia "*": Typy wyliczeniowe atrybutem [kodu natywnego] musi mieć podstawowy typ wyliczenia "long" lub "ulong".

<a name="MT4146" />

### <a name="mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a>MT4146: Parametr Name atrybutu rejestratora dla tej klasy\*"("\*") zawiera nieprawidłowy znak:"\*"(\*).

Nazwa klasy Objectice C nie może zawierać spacji, co oznacza, że `Register` nie może mieć atrybutu w odpowiedniej klasy zarządzanej `Name` parametr nie może zawierać spacji albo.

Sprawdź, czy `Register` atrybutu zarządzanej klasy wymienionych w komunikacie o błędzie nie zawiera żadnych spacji.

<a name="MT4147" />

### <a name="mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a>MT4147: Wykryto protokół dziedziczenie z protokołem JSExport podczas korzystania z dynamicznej rejestratora. Nie jest możliwe wyeksportować protokołów do JavaScriptCore dynamicznie; statyczne rejestratora musi być używany (Dodaj "--rejestratora: statyczne do argumentów dodatkowe mtouch projektu iOS opcji kompilacji, aby wybrać statycznych rejestratora).

<a name="MT4148" />

### <a name="mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a>MT4148: Znaleziono rejestratora Protokół ogólny: "*". Eksportowanie protokoły ogólnego nie jest obsługiwane.

<a name="MT4149" />

### <a name="mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a>MT4149: Nie można zarejestrować metody "\*.\*" ponieważ typ pierwszego parametru ("\*") nie jest zgodny z typem kategorii ("\*").

<a name="MT4150" />

### <a name="mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a>MT4150: Nie można zarejestrować typu "\*" ponieważ właściwość Type ("\*") w danej kategorii atrybutu nie dziedziczy NSObject.

<a name="MT4151" />

### <a name="mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a>MT4151: Nie można zarejestrować typu ' *' ponieważ nie ustawiono właściwości Type w atrybucie jego kategorii.

<a name="MT4152" />

### <a name="mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a>MT4152: Nie można zarejestrować typu "*" jako kategoria ponieważ implementuje on INativeObject lub podklasy NSObject.

<a name="MT4153" />

### <a name="mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a>MT4153: Nie można zarejestrować typu "\*" jako kategorii, ponieważ jest ona typu ogólnego.

<a name="MT4154" />

### <a name="mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a>MT4154: Nie można zarejestrować metody "\*" jako metoda kategorii, ponieważ jest ona typu ogólnego.

<a name="MT4155" />

### <a name="mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a>MT4155: Nie można zarejestrować metody "\*"z selektorem"\*" jako metoda kategorii na "*" ponieważ języka Objective C jest już implementację tego selektora.

<a name="MT4156" />

### <a name="mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a>MT4156: Nie można zarejestrować dwóch kategorii ("\*"i"\*") o takiej samej nazwie natywnego ("*").

<a name="MT4157" />

### <a name="mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a>MT4157: Nie można zarejestrować metody kategorii "\*" ponieważ wymagane jest co najmniej jeden parametr (i jej typ musi odpowiadać typowi kategorii "\*")

<a name="MT4158" />

### <a name="mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a>MT4158: Nie można zarejestrować konstruktora * w kategorii * konstruktorów w kategorii nie są obsługiwane.

<a name="MT4159" />

### <a name="mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a>MT4159: Nie można zarejestrować metody "*" jako metoda kategorii ponieważ metody kategorii muszą być statyczne.

<a name="MT4160" />

### <a name="mt4160-invalid-character---found-in-selector--for-"></a>MT4160: Nieprawidłowy znak "\*" (\*) w selektora "\*"for"\*".

<a name="MT4161" />

### <a name="mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a>MT4161: Rejestratora znaleziono nieobsługiwane struktury "\*": wszystkie pola w strukturze musi być również struktur (pola "\*"z typem"{2}" nie jest strukturą).

Rejestratora znaleziono struktury z polami nieobsługiwany.

Wszystkie pola w strukturze, który jest dostępny dla języka Objective-C musi być również struktur (nie klasy).

<a name="MT4162" />

### <a name="mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a>MT4162: Typ "\*" (używany jako * {2}) nie jest dostępna w ** (został wprowadzony w * *)\* skompiluj dla nowszej * SDK (zazwyczaj wykonywane za pomocą najnowszej wersji narzędzia xcode.

Rejestratora znaleziono typ, który nie jest dołączony do bieżącego zestawu SDK.

Uaktualnij Xcode.

<a name="MT4163" />

### <a name="mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT4163: Błąd wewnętrzny w rejestrze (*). Raport o usterce w pliku http://bugzilla.xamarin.com

Ten błąd wskazuje na usterkę w platformy Xamarin.iOS. Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4164" />

### <a name="mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a>MT4164: Nie można wyeksportować właściwość "\*" ponieważ selektorze "\*" jest słowem kluczowym języka Objective-C. Użyj innej nazwy.

Selektor zagrożona właściwość nie jest prawidłowy identyfikator języka Objective-C.

Należy używać prawidłowy identyfikator języka Objective-C selektorów.

<a name="MT4165" />

### <a name="mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a>MT4165: Rejestrator nie można znaleźć typu "System.Void" w jednym z zestawów występujących w odwołaniach.

Ten błąd prawdopodobnie wskazuje na usterkę w platformy Xamarin.iOS. Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4166" />

### <a name="mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a>MT4166: Nie można zarejestrować metody "\*", ponieważ podpis ma typ (\*), która nie jest typem referencyjnym.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4167" />

### <a name="mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a>MT4167: Nie można zarejestrować metody "\*", ponieważ podpis zawiera typu ogólnego (\*) z typem argumentu ogólnego, który nie jest podklasą NSObject (*).

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT4168" />

### <a name="mt4168-cannot-register-the-type-managedname-because-its-objective-c-name-exportedname-is-an-objective-c-keyword-please-use-a-different-name"></a>MT4168: Nie można zarejestrować typu "{zarządzane\_nazwa}" ponieważ nazwy języka Objective-C "{wyeksportowane\_nazwa}" jest słowem kluczowym języka Objective-C. Użyj innej nazwy.

Nazwa języka Objective C dla danego typu nie jest prawidłowym identyfikatorem języka Objective-C.

Użyj prawidłowego identyfikatora języka Objective-C.

<a name="MT4169" />

### <a name="mt4169-failed-to-generate-a-pinvoke-wrapper-for-method-message"></a>MT4169: Nie można wygenerować otokę P/Invoke {metody}: {message}

Nie można wygenerować funkcji P/Invoke otoki dla wymienionymi platformy Xamarin.iOS.
Sprawdź, czy komunikat zgłoszonego błędu dla podstawową przyczyną.

<a name="MT4170" />

### <a name="mt4170-the-registrar-cant-convert-from-managed-type-to-native-type-for-the-return-value-in-the-method-method"></a>MT4170: Rejestratora nie można przekonwertować z '{typu zarządzanego}' "{typu macierzystego}" dla wartości zwracanej w metodzie {metody}.

Zobacz opis błędu <a href="#MT4172">MT4172</a>.

<a name="MT4171" />

### <a name="mt4171-the-bindas-attribute-on-the-member-member-is-invalid-the-bindas-type-type-is-different-from-the-property-type-type"></a>MT4171: Atrybut BindAs w elemencie członkowskim {członka} jest nieprawidłowy: typ BindAs {type} jest inny niż typ właściwości {type}.

Upewnij się, że typ w atrybucie BindAs jest zgodny z typem elementu członkowskiego, który jest dołączony do.

<a name="MT4172" />

### <a name="mt4172-the-registrar-cant-convert-from-native-type-to-managed-type-for-the-parameter-parameter-name-in-the-method-method"></a>MT4172: Rejestratora nie można przekonwertować z '{typu macierzystego}' "{typu zarządzanego}" dla parametru "{parametr name}" w metodzie {metody}.

Rejestrator nie obsługuje konwersji między typami opisane powyżej.

Jest to błąd w Xamarin.iOS przypadku użyciu interfejsu API przez Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ] [ 1].

Jeśli napotkasz to podczas tworzenia powiązania projekt natywnej biblioteki, jest otwarty w celu dodawania obsługi dla nowych kombinacji typów. Jeśli jest to możliwe, zgłoś żądanie rozszerzenia ([http://bugzilla.xamarin.com][2]) z testem wielkość liter i firma Microsoft będzie oszacowania.

[1]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS
[2]: https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS&component=General&bug_severity=enhancement

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: GCC i łańcuch narzędzi komunikaty o błędach

### <a name="mt51xx-compilation"></a>MT51xx: kompilacji

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

<a name="MT5101" />

### <a name="mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a>MT5101: Brak "*" kompilatora. Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.

<a name="MT5102" />

### <a name="mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5102: Nie można utworzyć pliku "*". Raport o usterce w pliku http://bugzilla.xamarin.com

<a name="MT5103" />

### <a name="mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5103: Nie można skompilować pliku "*". Raport o usterce w pliku http://bugzilla.xamarin.com

<a name="MT5104" />

### <a name="mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a>MT5104: Nie można odnaleźć ani "\*"ani"\*" kompilatora. Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.

<!-- 5105 is used by mmp -->

<a name="MT5106" />

### <a name="mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5106: Nie można skompilować plików ' *'. Raport o usterce w pliku http://bugzilla.xamarin.com

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt52xx-linking"></a>MT52xx: Linking

<!--
  MT52xx linking
  -->

<a name="MT5201" />

### <a name="mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a>MT5201: Łączenie natywnego nie powiodło się. Przejrzyj dziennik kompilacji i flag użytkownika do gcc: *

<a name="MT5202" />

### <a name="mt5202-native-linking-failed-please-review-the-build-log"></a>MT5202: Łączenie natywnego nie powiodło się. Przejrzyj dziennik kompilacji.

<a name="MT5203" />

### <a name="mt5203-native-linking-warning-"></a>MT5203: Natywny łączenie Ostrzeżenie: *

<!--- 5204-5208 are not used -->

<a name="MT5209" />

### <a name="mt5209-native-linking-error-"></a>MT5209: Natywny błąd łączenia: *

<a name="MT5210" />

### <a name="mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a>MT5210: Łączenie natywnego nie powiodło się, Niezdefiniowany symbol: *. Sprawdź, czy odwołanie do wszystkich platform niezbędnych i natywnych bibliotek są poprawnie połączone w.

Dzieje się tak, gdy konsolidator natywny nie może znaleźć symbol, który odwołuje się do lokalizacji. Istnieje kilka przyczyn, że może to mieć miejsce:

* Powiązanie innej firmy wymaga RAM, ale powiązanie nie został określony w jej `[LinkWith]` atrybutu. Rozwiązania:
  - Tworzenie powiązania innych firm, lub mają dostęp do źródła, zmodyfikuj wiązania `[LinkWith]` atrybut, aby uwzględnić właściwość framework wymaga:

            [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]

  - Nie można zmodyfikować powiązania innych firm, można ręcznie połączyć z wymaganej struktury przez przekazanie `-gcc_flags '-framework SystemFramework'` do `mtouch` (jest to realizowane przez zmodyfikowanie mtouch dodatkowe argumenty strony Opcje kompilacji systemu iOS projektu. Należy pamiętać, że należy to zrobić dla każdej konfiguracji projektu).
* W niektórych przypadkach zarządzanych powiązanie składa się z kilku natywnych bibliotek i wszystkie muszą być zawarte w powiązaniach. Istnieje możliwość mają więcej niż jeden natywnej biblioteki w każdym projekcie powiązanie, więc rozwiązanie to po prostu Dodaj wszystkie wymagane biblioteki natywnej do powiązania projektu.</li>
* Powiązanie zarządzanych odwołuje się do symboli natywnych, które nie istnieją w natywnej biblioteki.
    Zazwyczaj dzieje się tak, gdy powiązanie zarania przez pewien czas i kod macierzysty został zmodyfikowany w tym samym czasie, aby określonej klasy natywnej albo został usunięty lub zmieniono jego nazwę, podczas gdy powiązanie nie zostały zaktualizowane.
* P/Invoke odwołuje się do natywnej symbol, który nie istnieje. Począwszy od platformy Xamarin.iOS 7.4 <a href="#MT5214">MT5214</a> zostanie zgłoszony błąd, w tym przypadku (więcej informacji zawiera MT5214).
* Powiązanie innych firm / biblioteki został zbudowany przy użyciu języka C++, ale powiązanie nie określa to jego `[LinkWith]` atrybutu. To jest zwykle stosunkowo łatwa do rozpoznania, ponieważ ma symbole są zniekształcone symbole C++ (jeden typowym przykładem jest `__ZNKSt9exception4whatEv`).
  - Tworzenie powiązania innych firm, lub mają dostęp do źródła, zmodyfikuj wiązania `[LinkWith]` atrybutu, aby ustawić `IsCxx` flagi:

            [LinkWith ("mylib.a", IsCxx = true)]

  - Jeśli nie można zmodyfikować powiązania innych firm lub łączysz ręcznie z biblioteką innych firm, można ustawić flagi równoważne przez przekazanie <code>-cxx</code> do mtouch (jest to realizowane przez zmodyfikowanie mtouch dodatkowe argumenty strony Opcje kompilacji systemu iOS projektu . Należy pamiętać, że należy to zrobić dla każdej konfiguracji projektu).

<a name="MT5211" />

### <a name="mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a>MT5211: Łączenie natywnego nie powiodło się, Niezdefiniowana klasa Objective-C: \*. Symbol "\*" nie można odnaleźć w żadnej z bibliotek i platform związane z aplikacją.

Dzieje się tak, konsolidator macierzysty nie można znaleźć klasy Objective-C, do którego odwołuje się w innym. Istnieje kilka przyczyn, może to mieć miejsce: takie same jak w przypadku [MT5210](#MT5210) oraz dodatkowo:

* Powiązanie innych firm powiązany protokół Objective-C, ale nie go przy użyciu adnotacji <code>[Protocol]</code> atrybutu w definicji interfejsu api. Rozwiązania:
  - Dodaj brakujące `[Protocol]` atrybutu:

              [BaseType (typeof (NSObject))]
              [Protocol] // Add this
              public interface MyProtocol
              {
              }

<a name="MT5212" />

### <a name="mt5212-native-linking-failed-duplicate-symbol-"></a>MT5212: Łączenie natywnego nie powiodło się, zduplikowany symbol: *.

Dzieje się tak, gdy natywnego konsolidator napotka zduplikowanych symbole między wszystkich natywnych bibliotek. Po tym błędzie mogą istnieć co najmniej jeden [MT5213](#MT5213) błędy występujące w lokalizacji dla każdego wystąpienia symbolu. Możliwe przyczyny tego błędu:

* Dwa razy jest dołączony sam natywnej biblioteki.
* Dwie różne biblioteki natywnego nastąpić do definiowania tego samego symbole.
* Natywnej biblioteki nie jest poprawnie zbudowany i zawiera więcej niż raz samego symbolu.
  Można to potwierdzić przy użyciu następujący zestaw poleceń z terminalu (zamiast i386 x86_64/armv7/armv7s/arm64 zgodnie z której architektury tworzysz dla):

        # Native libraries are usually fat libraries, containing binary code for
        # several architectures in the same file. First we extract the binary
        # code for the architecture we're interested in.
        lipo libNative.a -thin i386 -output libNative.i386.a

        # Now query the native library for the duplicated symbol.
        nm libNative.i386.a | fgrep 'SYMBOL'

        # You can also list the object files inside the native library.
        # In most cases this will reveal duplicated object files.
        ar -t libNative.i386.a

  Istnieje kilka możliwych sposobach rozwiązania problemu:

  - Żądanie, że dostawca natywnej biblioteki go rozwiązać i udostępnia zaktualizowaną wersję.
  - Rozwiąż problem samodzielnie dzięki usunięciu plików dodatkowy obiekt (to działa tylko, jeśli problem nie zostanie w rzeczywistości pliki zduplikowanych obiektów)

            # Find out if the library is a fat library, and which
            # architectures it contains.
            lipo -info libNative.a

            # Extract each architecture (i386/x86_64/armv7/armv7s/arm64) to a separate file
            lipo libNative.a -thin ARCH -output libNative.ARCH.a

            # Extract the object files for the offending architecture
            # This will remove the duplicates by overwriting them
            # (since they have the same filename)
            mkdir -p ARCH
            cd ARCH
            ar -x ../libNative.ARCH.a

            # Reassemble the object files in an .a
            ar -r ../libNative.ARCH.a *.o
            cd ..

            # Reassemble the fat library
            lipo *.a -create -output libNative.a

  - Poproś konsolidator, aby usunąć nieużywane kod. Xamarin.iOS wykona to automatycznie, gdy są spełnione wszystkie następujące warunki:
    - Wszystkie powiązania innych firm `[LinkWith]` atrybuty włączono SmartLink:

            [assembly: LinkWith ("libNative.a", SmartLink = true)]

    - Nie `-gcc_flags` jest przekazywana do mtouch (w polu mtouch dodatkowe argumenty projektu iOS opcje kompilacji).
    - Istnieje również możliwość zwrócenia konsolidator bezpośrednio, aby usunąć nieużywane kodu dodając `-gcc_flags -dead_strip` do argumentów dodatkowe mtouch projektu iOS opcje kompilacji.

<a name="MT5213" />

### <a name="mt5213-duplicate-symbol-in--location-related-to-previous-error"></a>MT5213: Zduplikowany symbol w: * (lokalizacji związanego z poprzednim błędem)

Ten błąd jest zgłaszany tylko razem z [MT5212](#MT5212). Zobacz [MT5212](#MT5212) Aby uzyskać więcej informacji.

<a name="MT5214" />

### <a name="mt5214-native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a>MT5214: Łączenie natywnego nie powiodło się, Niezdefiniowany symbol: *. Istnieje odwołanie do tego symbolu zarządzanego elementu członkowskiego *. Upewnij się, że wszystkie niezbędne struktury zostały przywoływany i natywne biblioteki połączone.

Ten błąd jest zgłaszany, gdy kod zarządzany zawiera P/Invoke do natywnej metody, która nie istnieje. Na przykład:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

Istnieje kilka możliwych rozwiązań:

  -  Usuń P/Invoke zagrożona z kodu źródłowego.
  -  Włącz zarządzane konsolidator dla wszystkich zestawów (w tym celu w systemie iOS projektu opcje kompilacji ustawienie "konsolidatora" do "Wszystkie zestawy"). Efektywne spowoduje to usunięcie wszystkich P/Invokes nie jest używany z poziomu aplikacji (automatycznie, zamiast ręcznie, takich jak poprzedni punkt). Wadą podwyższonego jest będzie kompilacje symulatora nieco wolniej, czy może spowodować nieprawidłowe działanie aplikacji, jeśli jest on za pomocą odbicia — można znaleźć więcej informacji na temat konsolidator [tutaj](~/ios/deploy-test/linker.md) )
  -  Utwórz drugi natywnego bibliotekę, która zawiera brakujące symboli natywnych. Należy pamiętać, że to jedynie obejście (w przypadku próby wywołania tych funkcji, aplikacja ulegnie awarii).

<a name="MT5215" />

### <a name="mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a>MT5215: Odwołuje się do ' *' może wymagać dodatkowych - framework = XXX lub lXXX — instrukcje do natywnej konsolidatora

To ostrzeżenie, wskazującą wykryto P/Invoke do odwołania w bibliotece, że aplikacja nie jest łączenie z nim.

<a name="MT5216" />

### <a name="mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT5216: Natywnego Konsolidacja nie powiodła się dla *. Raport o usterce w pliku http://bugzilla.xamarin.com

Ten błąd jest zgłaszany w trakcie łączenia dane wyjściowe kompilatora drzewa obiektów aplikacji.

Ten błąd prawdopodobnie wskazuje na usterkę w platformy Xamarin.iOS. Raport o usterce w pliku [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT5217" />

### <a name="mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a>MT5217: Łączenie natywnego prawdopodobnie nie powiodła się ponieważ wiersza polecenia konsolidatora jest za długa (* znaków).

Łączenie natywnego nie powiodło się i jest możliwe, ten błąd wystąpił, ponieważ polecenia konsolidatora jest za długi.

Projekty platformy Xamarin.iOS będzie często odwoływać symboli natywnych dynamicznie, co oznacza, że konsolidator natywnych może spowodować usunięcie takich symboli natywnych podczas procesu łączenia natywnego ponieważ konsolidator macierzysty nie widzi służą tych symboli.

Zazwyczaj Xamarin.iOS poprosi natywnego konsolidator, aby zachować takie symbole, przy użyciu `-u symbol` flag konsolidatora, ale jeśli istnieje wiele takich symboli, całego wiersza polecenia może zostać przekroczona maksymalna długość wiersza polecenia określone przez system operacyjny.

Istnieje kilka możliwych źródeł dla takich dynamiczne symboli:

* P/Invokes do metod w statycznie połączonych bibliotek (gdzie to nazwa biblioteki dll `__Internal` w atrybucie DllImport `[DllImport ("__Internal")]`).
* Pole odwołania do lokalizacji pamięci w bibliotekach statycznie połączone z powiązania projektów (`[Field]` atrybutów).
* Klasy języka Objective-C w biblioteki statycznie połączonej z powiązania projektów (używając kompilacji przyrostowej lub gdy nie używa statycznego rejestratora).

Możliwe rozwiązania:

* Włącz zarządzane konsolidator (jeśli jest to możliwe wszystkie zestawy, a nie tylko zestawy SDK). To może usunąć wystarczającą ilość źródeł dynamiczne symbole, tak aby konsolidator elementu wiersza polecenia nie przekroczyły maksymalną.
* Zmniejsz liczbę P/Invoke, odwołania do pól i/lub klasy Objective-C.
* Ponowne zapisywanie adresów dynamicznych symbole mieć krótszej nazwy.
* Przekaż `-dlsym:false` jako argumentu dodatkowe mtouch projektu iOS opcje kompilacji. Po wybraniu tej opcji Xamarin.iOS wygeneruje odwołanie do natywnego kodu skompilowanego drzewa obiektów aplikacji i nie musisz poprosić konsolidator, aby zachować ten symbol. Jednak kompilacje ta działa tylko w przypadku urządzeń, a następnie spowoduje błędy konsolidatora, jeśli istnieją P/Invoke do funkcji, które nie istnieją w bibliotece statycznej.
* Przekaż `--dynamic-symbol-mode=code` mtouch dodatkowe argumenty projektu iOS opcje kompilacji. Po wybraniu tej opcji Xamarin.iOS wygeneruje dodatkowe kodu macierzystego, który odwołuje się do tych symboli zamiast pytaniem natywnego konsolidator, aby zachować te symbole, przy użyciu argumentów wiersza polecenia. Wadą tego podejścia interfejsu jest, że jej spowoduje zwiększenie rozmiaru pliku wykonywalnego nieco.
* Włącz statycznych rejestratora przez przekazanie `--registrar:static` jako argument dodatkowe mtouch w systemie iOS projektu kompilacji opcje (na symulatorze kompilacji, ponieważ statycznych rejestratora jest już domyślnie tworzy urządzenia). Statyczne rejestratora wygeneruje kod, który odwołuje się do klasy Objective-C statycznie, więc nie musi poprosić natywnego konsolidator, aby zachować tych klas.
* Wyłącz kompilacji przyrostowej (dla kompilacji urządzenia). Po włączeniu kompilacji przyrostowej kod wygenerowany przez rejestrator statycznych nie uwzględnianych przez konsolidator macierzystego, co oznacza, że Xamarin.iOS nadal musisz poprosić konsolidator, aby zachować odwołuje się do klasy Objective-C. W związku z tym wyłączenie kompilacji przyrostowej uniemożliwi te wymagania.

<a name="MT5218" />

### <a name="mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MT5218: Nie można zignorować dynamiczne symbol {symbol} (--Ignoruj dynamic — symbol = {symbol}), ponieważ nie została wykryta jako symbolu dynamicznych.

Argument wiersza polecenia `--ignore-dynamic-symbol=symbol` został przekazany, ale ten symbol nie jest symbol, który został rozpoznany jako symbolu dynamiczne, które muszą zostać zachowane ręcznie.

Istnieją dwa główne powody to:

* Nazwa symbolu jest niepoprawna.
    * Nie dołączy podkreślenie na nazwę symbolu.
    * Symbol dla klas języka Objective C jest `OBJC_CLASS_$_<classname>`.
* Symbol jest poprawny, ale to symbol, który już jest zachowywana w normalny sposób (niektóre kompilacji przyczyny opcje dokładnej listy dynamicznej symbole różnicującej).

### <a name="mt53xx-other-tools"></a>MT53xx: Inne narzędzia

<!--
  MT53xx other tools
  -->

<a name="MT5301" />

### <a name="mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a>MT5301: Brak narzędzia "Usuń". Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.

<a name="MT5302" />

### <a name="mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a>MT5302: Brak narzędzie "dsymutil". Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.

<a name="MT5303" />

### <a name="mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a>MT5303: Nie można wygenerować symboli debugowania (dSYM katalogu). Przejrzyj dziennik kompilacji.

Wystąpił błąd podczas uruchamiania dsymutil w katalogu .app końcowego można utworzyć symboli debugowania. Przejrzyj dziennik kompilacji, aby wyświetlić dane wyjściowe w dsymutil i zobacz, jak można było usunąć.

<a name="MT5304" />

### <a name="mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a>MT5304: Nie powiodło się paska końcowego pliku binarnego. Przejrzyj dziennik kompilacji.

Wystąpił błąd podczas uruchamiania narzędzia "Usuń", aby usunąć informacje o debugowaniu z aplikacji.

<a name="MT5305" />

### <a name="mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a>MT5305: Brak narzędzie "lipo". Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.

<a name="MT5306" />

### <a name="mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a>MT5306: Nie można utworzyć biblioteki fat. Przejrzyj dziennik kompilacji.

Wystąpił błąd podczas uruchamiania narzędzia "lipo". Przejrzyj dziennik kompilacji, aby wyświetlić błąd zgłoszony przez "lipo".

<a name="MT5307" />

### <a name="mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a>MT5307: Nie można podpisać pliku wykonywalnego. Przejrzyj dziennik kompilacji.

Wystąpił błąd podczas podpisywania aplikacji. Przejrzyj dziennik kompilacji, aby wyświetlić błąd zgłoszony przez "codesign".

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: wewnętrzny mtouch narzędzi komunikaty o błędach

### <a name="mt600x-stripper"></a>MT600x: odpędowej

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

<a name="MT6001" />

### <a name="mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a>MT6001: Uruchomiona wersja Cecil nie obsługuje usuwanie zestawu

<a name="MT6002" />

### <a name="mt6002-could-not-strip-assembly-"></a>MT6002: Nie można usunąć zestawu `*`.

Wystąpił błąd podczas usuwanie kodu zarządzanego (usuwanie kodu IL) z zestawów w aplikacji.

<a name="MT6003" />

### <a name="mt6003-unauthorizedaccessexception-message"></a>MT6003: [Unauthorizedaccessexception — komunikat]

Wystąpił błąd zabezpieczeń podczas usuwanie debugowania symbole z aplikacji.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: Komunikaty o błędach MSBuild

<!--
 MT7xxx msbuild errors
  -->

<a name="MT7001" />

### <a name="mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a>MT7001: Nie można rozpoznać hosta adresów IP dla sieci Wi-Fi ustawień debugera.

*Zadanie programu MSBuild: DetectDebugNetworkConfigurationTaskBase*

Kroki rozwiązywania problemów:

- Spróbuj uruchomić `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (który powinien zapewnić adres IP i nie jest błąd oczywiście).
- Spróbuj uruchomić "ping \`hostname\`" która może ułatwić więcej informacji, takie jak: `cannot resolve MyHost.local: Unknown host`

W niektórych przypadkach jest to problem "sieci lokalnej" i może zostać zlikwidowane przez dodanie Nieznany host `127.0.0.1   MyHost.local` w `/etc/hosts`.

<a name="MT7002" />

### <a name="mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a>MT7002: Ten komputer nie ma żadnych kart sieciowych. Jest to wymagane, gdy debugowanie lub profilowania na urządzeniu za pośrednictwem sieci Wi-Fi.

*Zadanie programu MSBuild: DetectDebugNetworkConfigurationTaskBase*

<a name="MT7003" />

### <a name="mt7003-the-app-extension--does-not-contain-an-infoplist"></a>MT7003: Rozszerzenie aplikacji "*" nie zawiera pliku Info.plist.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7004" />

### <a name="mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a>MT7004: Rozszerzenie aplikacji "*" nie określa CFBundleIdentifier.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7005" />

### <a name="mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a>MT7005: Rozszerzenie aplikacji "*" nie określa CFBundleExecutable.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7006" />

### <a name="mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7006: Rozszerzenie aplikacji "\*' ma nieprawidłowy CFBundleIdentifier (\*), nie zaczyna się od CFBundleIdentifier pakietu aplikacji głównej (*).

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7007" />

### <a name="mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7007: Rozszerzenie aplikacji "\*' ma CFBundleIdentifier (\*) który kończy się wraz z sufiksem niedozwolony".key".

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7008" />

### <a name="mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a>MT7008: Rozszerzenie aplikacji "*" nie określa CFBundleShortVersionString.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7009" />

### <a name="mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7009: Rozszerzenie aplikacji ' *' ma nieprawidłowy Info.plist: nie zawiera słownik NSExtension.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7010" />

### <a name="mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a>MT7010: Rozszerzenie aplikacji ' *' ma nieprawidłowy Info.plist: słownika NSExtension nie zawiera wartości NSExtensionPointIdentifier.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7011" />

### <a name="mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a>MT7011: Rozszerzenie WatchKit ' *' ma nieprawidłowy Info.plist: słownika NSExtension nie zawiera słownik NSExtensionAttributes.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7012" />

### <a name="mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a>MT7012: Rozszerzenie WatchKit "*" nie ma dokładnie jedno wyrażenie kontrolne aplikacji.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7013" />

### <a name="mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a>MT7013: Rozszerzenie WatchKit ' *' ma nieprawidłowy Info.plist: UIRequiredDeviceCapabilities musi zawierać możliwość "watch pomocnika".

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7014" />

### <a name="mt7014-the-watch-app--does-not-contain-an-infoplist"></a>MT7014: Aplikacja czujki "*" nie zawiera pliku Info.plist.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7015" />

### <a name="mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7015: Aplikacja czujki "*" nie określa CFBundleIdentifier.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7016" />

### <a name="mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7016: Aplikacja czujki "\*' ma nieprawidłowy CFBundleIdentifier (\*), nie zaczyna się od CFBundleIdentifier pakietu aplikacji głównej (*).

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7017" />

### <a name="mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a>MT7017: Aplikacja czujki "\*" nie ma prawidłowej wartości UIDeviceFamily. Oczekiwano "[4] Obejrzyj" ale znaleziono "\* (*)".

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7018" />

### <a name="mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7018: Aplikacja czujki "*" nie określa CFBundleExecutable

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7019" />

### <a name="mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a>MT7019: Aplikacja czujki "\*" ma nieprawidłową wartość WKCompanionAppBundleIdentifier ("\*"), nie odpowiada CFBundleIdentifier pakietu aplikacji głównej ("*").

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7020" />

### <a name="mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a>MT7020: Aplikacja czujki ' *' ma nieprawidłowy Info.plist: klucz WKWatchKitApp musi być obecny i ma wartość "true".

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7021" />

### <a name="mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7021: Aplikacja czujki ' *' ma nieprawidłowy Info.plist: klucz LSRequiresIPhoneOS nie mogą być obecne.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7022" />

### <a name="mt7022-the-watch-app--does-not-contain-a-watch-extension"></a>MT7022: Aplikacja czujki "*" nie zawiera rozszerzenia czujki.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7023" />

### <a name="mt7023-the-watch-extension--does-not-contain-an-infoplist"></a>MT7023: Rozszerzenie czujki "*" nie zawiera pliku Info.plist.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7024" />

### <a name="mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a>MT7024: Rozszerzenie czujki "*" nie określa CFBundleIdentifier.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7025" />

### <a name="mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a>MT7025: Rozszerzenie czujki "*" nie określa CFBundleExecutable.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7026" />

### <a name="mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a>MT7026: Rozszerzenie czujki "\*' ma nieprawidłowy CFBundleIdentifier (\*), nie zaczyna się od CFBundleIdentifier pakietu aplikacji głównej (*).

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7027" />

### <a name="mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a>MT7027: Rozszerzenie czujki "\*' ma CFBundleIdentifier (\*) który kończy się wraz z sufiksem niedozwolony".key".

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7028" />

### <a name="mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a>MT7028: Rozszerzenie czujki ' *' ma nieprawidłowy Info.plist: nie zawiera słownik NSExtension.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7029" />

### <a name="mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a>MT7029: Rozszerzenie czujki ' *' ma nieprawidłowy Info.plist: NSExtensionPointIdentifier musi być "com.apple.watchkit".

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7030" />

### <a name="mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a>MT7030: Rozszerzenie czujki ' *' ma nieprawidłowy Info.plist: słownik NSExtension musi zawierać NSExtensionAttributes.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7031" />

### <a name="mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a>MT7031: Rozszerzenie czujki ' *' ma nieprawidłowy Info.plist: słownik NSExtensionAttributes musi zawierać WKAppBundleIdentifier.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7032" />

### <a name="mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a>MT7032: Rozszerzenie WatchKit ' *' ma nieprawidłowy Info.plist: UIRequiredDeviceCapabilities nie powinna zawierać możliwość "watch pomocnika".

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7033" />

### <a name="mt7033-the-watch-app--does-not-contain-an-infoplist"></a>MT7033: Aplikacja czujki "*" nie zawiera pliku Info.plist.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7034" />

### <a name="mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a>MT7034: Aplikacja czujki "*" nie określa CFBundleIdentifier.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7035" />

### <a name="mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a>MT7035: Aplikacja czujki "\*" nie ma prawidłowej wartości UIDeviceFamily. Oczekiwano "\*"ale znaleziono"\* (\*)".

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7036" />

### <a name="mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a>MT7036: Aplikacja czujki "*" nie określa CFBundleExecutable.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7037" />

### <a name="mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a>MT7037: Rozszerzenie WatchKit "{extensionName}" ma nieprawidłową wartość WKAppBundleIdentifier ("\*"), nie pasuje do aplikacji czujki CFBundleIdentifier ("\*").

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7038" />

### <a name="mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a>MT7038: Aplikacja czujki ' *' ma nieprawidłowy Info.plist: WKCompanionAppBundleIdentifier musi istnieć i musi odpowiadać CFBundleIdentifier pakiecie głównej aplikacji.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7039" />

### <a name="mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a>MT7039: Aplikacja czujki ' *' ma nieprawidłowy Info.plist: klucz LSRequiresIPhoneOS nie mogą być obecne.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7040" />

### <a name="mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a>MT7040: {AppBundlePath} pakietu aplikacji nie zawiera pliku Info.plist.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7041" />

### <a name="mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a>MT7041: Ścieżka Info.plist głównego nie określono CFBundleIdentifier.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7042" />

### <a name="mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a>MT7042: Ścieżka Info.plist głównego nie określono CFBundleExecutable.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7043" />

### <a name="mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a>MT7043: Ścieżka Info.plist głównego nie określono CFBundleSupportedPlatforms.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7044" />

### <a name="mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a>MT7044: Ścieżka Info.plist głównego nie określono UIDeviceFamily.

*Zadanie programu MSBuild: ValidateAppBundleTaskBase*

<a name="MT7045" />

### <a name="mt7045-unrecognized-format-"></a>MT7045: Unrecognized Format: *.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

Gdzie * może być:

- string
- tablica
- dict
- bool
- rzeczywiste
- integer
- Data
- dane

<a name="MT7046" />

### <a name="mt7046-add-entry--incorrectly-specified"></a>MT7046: Dodaj: wpis, *, niepoprawnie określony.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7047" />

### <a name="mt7047-add-entry--contains-invalid-array-index"></a>MT7047: Dodaj: wpis, *, zawiera nieprawidłowy indeks tablicy.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7048" />

### <a name="mt7048-add--entry-already-exists"></a>MT7048: Dodaj: * wpis już istnieje.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7049" />

### <a name="mt7049-add-cant-add-entry--to-parent"></a>MT7049: Dodaj: nie można dodać wpisu, *, aby element nadrzędny.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7050" />

### <a name="mt7050-delete-cant-delete-entry--from-parent"></a>MT7050: Usuń: nie można usunąć wpisu, *, od elementu nadrzędnego.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7051" />

### <a name="mt7051-delete-entry--contains-invalid-array-index"></a>MT7051: Usuń: wpis, *, zawiera nieprawidłowy indeks tablicy.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7052" />

### <a name="mt7052-delete-entry--does-not-exist"></a>MT7052: Usuń: wpis, *, nie istnieje.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7053" />

### <a name="mt7053-import-entry--incorrectly-specified"></a>MT7053: Importuj: wpis, *, niepoprawnie określony.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7054" />

### <a name="mt7054-import-entry--contains-invalid-array-index"></a>MT7054: Importuj: wpis, *, zawiera nieprawidłowy indeks tablicy.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7055" />

### <a name="mt7055-import-error-reading-file-"></a>MT7055: Importuj: błąd podczas odczytu pliku: *.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7056" />

### <a name="mt7056-import-cant-add-entry--to-parent"></a>MT7056: Importuj: nie można dodać wpisu, *, aby element nadrzędny.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7057" />

### <a name="mt7057-merge-cant-add-array-entries-to-dict"></a>MT7057: Scal: nie można dodać pozycji tablicy do dict.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7058" />

### <a name="mt7058-merge-specified-entry-must-be-a-container"></a>MT7058: Scal: określony wpis musi być kontenerem.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7059" />

### <a name="mt7059-merge-entry--contains-invalid-array-index"></a>MT7059: Scal: wpis, *, zawiera nieprawidłowy indeks tablicy.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7060" />

### <a name="mt7060-merge-entry--does-not-exist"></a>MT7060: Scal: wpis, *, nie istnieje.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7061" />

### <a name="mt7061-merge-error-reading-file-"></a>MT7061: Scal: błąd podczas odczytu pliku: *.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7062" />

### <a name="mt7062-set-entry--incorrectly-specified"></a>MT7062: Ustaw: wpis, *, niepoprawnie określony.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7063" />

### <a name="mt7063-set-entry--contains-invalid-array-index"></a>MT7063: Ustaw: wpis, *, zawiera nieprawidłowy indeks tablicy.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7064" />

### <a name="mt7064-set-entry--does-not-exist"></a>MT7064: Ustaw: wpis, *, nie istnieje.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7065" />

### <a name="mt7065-unknown-propertylist-editor-action-"></a>MT7065: Nieznany ListaWłaściwości Edytor akcji: *.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7066" />

### <a name="mt7066-error-loading--"></a>MT7066: Wystąpił błąd podczas ładowania "*": *.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

<a name="MT7067" />

### <a name="mt7067-error-saving--"></a>MT7067: Błąd podczas zapisywania "*": *.

*Zadanie programu MSBuild: PropertyListEditorTaskBase*

## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: Komunikaty o błędach środowiska uruchomieniowego

<!--
 MT8xxx runtime
  MT800x misc
  -->

<a name="MT8001" />

### <a name="mt8001-version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a>MT8001: Niezgodność wersji między natywnej obsługi platformy Xamarin.iOS i monotouch.dll. Zainstaluj ponownie platformy Xamarin.iOS.

<a name="MT8002" />

### <a name="mt8002-could-not-find-the-method--in-the-type-"></a>MT8002: Nie można odnaleźć metody "\*"w typie"\*".

<a name="MT8003" />

### <a name="mt8003-failed-to-find-the-closed-generic-method--on-the-type-"></a>MT8003: Nie można odnaleźć zamkniętą metodą ogólną "\*"w typie"\*".

<a name="MT8004" />

### <a name="mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a>MT8004: Nie można utworzyć wystąpienia * dla obiekt natywny 0 x * (typu "*"), ponieważ istnieje już inne wystąpienie tego obiektu natywnego (typu *).

<a name="MT8005" />

### <a name="mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a>MT8005: Typ otoki "\*"brakuje klasy ObjectiveC jego natywnej"\*".

<a name="MT8006" />

### <a name="mt8006-failed-to-find-the-selector--on-the-type-"></a>MT8006: Nie można odnaleźć selektor "\*"w typie"\*"

<a name="MT8007" />

### <a name="mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a>MT8007: Nie można pobrać deskryptora metody selektora "\*"w typie"\*", ponieważ selektor nie odpowiada — metoda

<a name="MT8008" />

### <a name="mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8008: Załadowana wersja Xamarin.iOS.dll został skompilowany dla * bitów, gdy proces jest * usługi bits. Zgłoś usterkę w http://bugzilla.xamarin.com.

Oznacza to, że dany element jest nieprawidłowy w procesie kompilacji. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8009" />

### <a name="mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8009: Nie można zlokalizować bloku, aby delegować metody konwersji dla metody *.*" s parametr #*. Zgłoś usterkę w http://bugzilla.xamarin.com.

Oznacza to, że interfejs API nie został poprawnie powiązane. Jeśli jest to interfejs API udostępnianych przez program Xamarin, Zgłoś usterkę w naszym bugzilla ([http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)), jeśli jest powiązanie innej firmy, skontaktuj się z dostawcą.

<a name="MT8010" />

### <a name="mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a>MT8010: Typ macierzysty rozmiar niezgodność między Xamarin. [iOS | Architektura wykonywania i .dll Mac]. Xamarin. [iOS | .Dll Mac] został utworzony dla *-bitowe, gdy bieżący proces jest *-bitowych.

Oznacza to, że dany element jest nieprawidłowy w procesie kompilacji. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8011" />

### <a name="mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8011: Nie można zlokalizować delegata do bloku atrybutu konwersji ([DelegateProxy]) dla wartości zwracanej przez metodę *.*. Zgłoś usterkę w http://bugzilla.xamarin.com.

Xamarin.iOS nie może znaleźć wymaganej metody w czasie wykonywania (Aby przekonwertować delegata bloku).

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8012" />

### <a name="mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8012: Nieprawidłowy DelegateProxyAttribute dla wartości zwracanej przez metodę *.*: DelegateType ma wartość null. Zgłoś usterkę w http://bugzilla.xamarin.com.

Atrybut DelegateProxy metodą jest nieprawidłowy.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8013" />

### <a name="mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8013: Nieprawidłowy DelegateProxyAttribute dla wartości zwracanej przez metodę *.*: DelegateType ({2}) określa typ bez pola "Handler". Zgłoś usterkę w http://bugzilla.xamarin.com.

Atrybut DelegateProxy metodą jest nieprawidłowy.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8014" />

### <a name="mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8014: Nieprawidłowy DelegateProxyAttribute dla wartości zwracanej przez metodę *.*: DelegateType ({2}) pola "Handler" ma wartość null. Zgłoś usterkę w http://bugzilla.xamarin.com.

Atrybut DelegateProxy metodą jest nieprawidłowy.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8015" />

### <a name="mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8015: Nieprawidłowy DelegateProxyAttribute dla wartości zwracanej przez metodę *.*: DelegateType ({2}) pola "Handler" nie jest obiektem delegowanym, jest *. Zgłoś usterkę w http://bugzilla.xamarin.com.

Atrybut DelegateProxy metodą jest nieprawidłowy.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8016" />

### <a name="mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a>MT8016: Nie można przekonwertować obiektu delegowanego mają być blokowane dla wartości zwracanej przez metodę *.*, ponieważ dane wejściowe nie jest delegatem, jest *. Zgłoś usterkę w http://bugzilla.xamarin.com.

Atrybut DelegateProxy metodą jest nieprawidłowy.

Wskazuje to zazwyczaj na usterkę w Xamarin.iOS; Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<!-- 8017 is used by mmp -->

<a name="MT8018" />

### <a name="mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8018: Wewnętrzny błąd zgodności. Raport o usterce w pliku http://bugzilla.xamarin.com.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8019" />

### <a name="mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a>MT8019: Nie można odnaleźć zestawu * w załadowanych zestawów.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8020" />

### <a name="mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a>MT8020: Nie można odnaleźć modułu o MetadataToken * w zestawie *.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8021" />

### <a name="mt8021-unknown-implicit-token-type-"></a>MT8021: Nieznany typ tokenu niejawne: *.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8022" />

### <a name="mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8022: Oczekiwano odwołania do tokenu * jako *, ale jest *. Raport o usterce w pliku http://bugzilla.xamarin.com.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8023" />

### <a name="mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MT8023: Obiektu wystąpienia jest wymagana do utworzenia zamkniętą metodą ogólną dla metody ogólnej Otwórz: * (tokenu odwołania: *). Raport o usterce w pliku http://bugzilla.xamarin.com.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<a name="MT8024" />

### <a name="mt8024-could-not-find-a-valid-extension-type-for-the-smart-enum-smarttype-please-file-a-bug-at-httpsbugzillaxamarincom"></a>MT8024: Nie można odnaleźć typu prawidłowe rozszerzenie dla wyliczenia inteligentne {smart_type}. Zgłoś usterkę w https://bugzilla.xamarin.com.

To wskazuje na usterkę w platformy Xamarin.iOS. Zgłoś usterkę w [ http://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).
