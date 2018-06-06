---
title: Komunikaty o błędach Xamarin.Mac (mmp)
description: Ten dokument zawiera listę błędów generowanych przez mmp, narzędzie używane do pakietu skompilowane zestawy do pliku wykonywalnego aplikacji dla komputerów Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: f5cf4a9003d3fb468ffcf337c33730cb12238c44
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792762"
---
# <a name="xamarinmac-error-messages-mmp"></a>Komunikaty o błędach Xamarin.Mac (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: komunikaty o błędach mmp

Np. Parametry, środowisko, Brak narzędzia.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM0000: Wystąpił nieoczekiwany błąd - pliku błędu, zgłoś na http://bugzilla.xamarin.com

Wystąpił nieoczekiwany błąd. Sprawdź [raport o usterkach](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) jak najwięcej informacji jak to możliwe, w tym:

* Pełnej kompilacji dzienników z zapewniały maksymalną szczegółowość (np. `-v -v -v -v` w **mmp dodatkowe argumenty**);
* Minimalny przypadek testowy, który występuje błąd; i
* Wszystkie informacje w wersji

Najprostszym sposobem, aby uzyskać informacje o wersji dokładnie jest użycie **Xamarin Studio** menu **Xamarin Studio** elementu **Pokaż szczegóły** przycisk i kopiowania/wklejania wersji informacje (można użyć **kopiowanie informacji** przycisku).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: Ta wersja programu Xamarin.Mac wymaga Mono {0} (bieżąca wersja Mono to {1}). Zaktualizuj Mono.framework z http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: Nazwa aplikacji "{0}.exe" powoduje konflikt z nazwą zestawu (.dll) zestawu SDK lub produkt.

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: Zestawu głównego "{0}" nie istnieje

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: Należy podać jedną zestawu głównego tylko, znaleziono {0} zestawów: "{1}"

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: Nie można przeanalizować argumenty wiersza polecenia: {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: Opcja "{0}" jest przestarzała.

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: Należy podać zestawu głównego

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: Nieznany argument wiersza polecenia: "{0}"

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: Prawidłowych opcji "{0}"są"{1}".

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: Nazwa aplikacji "{0}.exe" powoduje konflikt z innego zestawu użytkownika.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: Nie można przeanalizować argument wiersza polecenia "{0}": {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Moduł zbierający elementy bezużyteczne Boehm nie jest obsługiwane. Zamiast tego wybrano modułu zbierającego elementy bezużyteczne SGen.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: Nie można udostępnić w zestawie głównym Jeśli — zestawu w przypadku głównego nie została przekazana.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: Katalog wyjściowy (— dane wyjściowe) jest wymagany, jeśli — zestawu w przypadku głównego nie została przekazana.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Nie można wyłączyć nowego licznikiem przy użyciu interfejsu API Unified.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: Nie można odnaleźć Xcode w jednym z naszych domyślne lokalizacje. Zainstaluj program Xcode lub Przekaż niestandardową ścieżkę przy użyciu--sdkroot =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: Nie można odnaleźć Xcode aktualnie wybranego w systemie: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: Nie można odnaleźć Xcode aktualnie wybranego w systemie. "xcode — wybierz — Ścieżka drukowania" zwrócił "{0}", ale ten katalog nie istnieje.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: Nieprawidłowa wartość dla platformy docelowej: {0}.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: Nieznany platformy: *. Wskazuje to zazwyczaj na usterkę w Xamarin.Mac; Raport o usterce w pliku https://bugzilla.xamarin.com z przypadkiem testowym.

Wskazuje to zazwyczaj na usterkę w Xamarin.Mac; Raport o usterce w pliku [ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) z przypadkiem testowym.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: Błąd wewnętrzny. nie pliku wykonywalnego został skopiowany do pakietu aplikacji. Skontaktuj się z "support@xamarin.com"

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: Wyłączanie NewRefCount, — nowe-refcount:false, jest przestarzały.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Ta wersja programu Xamarin.Mac wymaga * SDK (dostarczone z Xcode *). Albo uaktualnić Xcode do pobrania plików wymaganego nagłówka lub użyć Rejestratora dynamicznych lub ustawienia zachowania konsolidatora zarządzanych platformy łącze lub tylko łącze Framework SDK (należy unikać nowych interfejsów API).

Xamarin.Mac wymaga pliki nagłówków z zestawu SDK w wersji określony w komunikacie o błędzie, aby skompilować aplikację w rejestrze statycznych. Zalecany sposób, aby naprawić ten błąd jest uaktualnienie Xcode, aby uzyskać wymagany zestaw SDK, to będzie zawierać wszystkie pliki wymaganego nagłówka. Jeśli wiele wersji Xcode zainstalowane lub chcesz użyć Xcode w lokalizacji innej niż domyślna, należy ustawić prawidłową lokalizację Xcode w preferencjach środowiskiem IDE.

Jedno potencjalnych, alternatywne rozwiązanie jest umożliwienie konsolidator zarządzanych. Spowoduje to usunięcie nieużywanych interfejsu API w tym, w większości przypadków nowy interfejs API, gdzie pliki nagłówkowe brakuje (lub są niekompletne). Niemniej to nie będzie działać, jeśli projekt używa interfejsu API, który został wprowadzony w nowszych zestawu SDK niż jeden z Xcode zawiera.

Drugi potencjalnych, alternatywne rozwiązanie zamiast tego użyj dynamicznych rejestratora. Będzie to nałożyć koszt uruchomienia przez dynamiczne rejestrowanie typów, ale usunąć wymaganie pliku nagłówka. 

Ostatnie słomy rozwiązaniem byłoby użyć starszej wersji Xamarin.Mac, obsługującą SDK projektu wymaga.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: plik machine.config "{0}" nie można odnaleźć.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: Kompilacja drzewa obiektów aplikacji jest dostępna tylko na Unified

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: Błąd wewnętrzny {0}. Raport o usterce z przypadkiem testowym pliku (http://bugzilla.xamarin.com).

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: Hybrydowe drzewa obiektów aplikacji kompilacji wymaga wszystkie zestawy być skompilowany drzewa obiektów aplikacji.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: skopiowanie pliku / łączy symbolicznych (związane z projektu)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: Nie można utworzyć łącza symbolicznego '{Plik}' -> "{target}": błąd {number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: Zestawy produktu

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: Wymagane "{0}" zestaw nie ma odwołań

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: Zestaw '{0}' nie jest zgodny z tego narzędzia

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} "{1}" nie można odnaleźć. Platforma docelowa "{0}" nie nadaje się do pakietu aplikacji.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: Platforma docelowa "{0}" jest nieprawidłowy.

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework musi zawsze platformy docelowej .NET 4.5, nie "{0}' który jest nieprawidłowy

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: Platforma docelowa "{0}" jest nieprawidłowa, gdy framwork Xamarin.Mac 4.5 .NET Profilowanie grup.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Niezgodność odwołania Xamarin.Mac "{0}"i platforma docelowa wybrana"{1}".

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: Błędy zbieranie (niewymagające konsolidatora) zestawu

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: Nie można rozpoznać odwołania: {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: Nie cenę O dynamicznej biblioteki (Nieznany nagłówek "0 x{0}"): {1}.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: Nie biblioteki statycznej (Nieznany nagłówek "{0}"): {1}.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: Nie cenę O dynamicznej biblioteki (Nieznany nagłówek "0 x{0}"): {1}.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: Nieznany format wpisu fat na pozycji {0} w {1}.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: Plik typu {0} nie jest plikiem MachO ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx: konsolidatora

### <a name="mm20xx-linker-general-errors"></a>MM20xx: Błędy konsolidatora (Ogólne)

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: Nie można połączyć zestawów

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: Nie można rozpoznać odwołania: {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: Opcja "{0}" zostanie zignorowany, ponieważ połączenie jest wyłączona

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: Plik definicji konsolidatora dodatkowy "{0}" nie można zlokalizować.

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: Definicje z "{0}" nie można przeanalizować.

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: Natywnej biblioteki "{0}" został przywołany, ale nie można odnaleźć.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Interfejsu API Unified Xamarin.Mac na podstawie pełnego profilu platformy .NET nie obsługuje łączenia. Przekaż flagi - nolink.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: Odwołuje się do {0}.{1}     ** Ten komunikat jest powiązany z MM2006 **

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: Nieznana klasa HttpMessageHandler `{0}`. Prawidłowe wartości to HttpClientHandler (ustawienie domyślne), CFNetworkHandler lub NSUrlSessionHandler

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: Nieznany TLSProvider "{0}.  Prawidłowe wartości to domyślne lub appletls

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: Tylko pierwszy {0} z {1} "Odwołuje się" wyświetlane ostrzeżenia. ** Tej wiadomości dotyczące 2009 **

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: Nie można rozpoznać odwołania do "{0}", do którego istnieje odwołanie w"{1}". Aplikacja nie będzie zawierać przywoływanego zestawu i może się nie powieść w czasie wykonywania.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.Mac rozszerzenia nie obsługują połączeń. Żądanie łączenia zostanie zignorowany. ** Ten komunikat jest przestarzała w XM 3,6 + **

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: Nieprawidłowy TlsProvider `{0}` opcji. Jedyną prawidłową wartością `{1}` będą używane.

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: Nie można przetworzyć opisu XML: {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: Powiązanie optymalizatora nie powiodło się przetwarzanie `...`.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.Mac klasycznego interfejsu API nie obsługuje platformy łączenia.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: Błąd podczas przetwarzania zestawu '\*': *

Wystąpił nieoczekiwany błąd podczas przetwarzania zestawu.

W komunikacie o błędzie nosi nazwę zestawu przyczyną problemu. Aby rozwiązać ten problem, zestaw musi znajdować się w [usterek — raport](https://bugzilla.xamarin.com) wraz z dziennika kompilacji pełne o poziomie szczegółowości włączone (tj. `-v -v -v -v` w **mtouch dodatkowe argumenty**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: Nie można połączyć z zestawu '{0}' ponieważ jest on trybu mieszanego.

Zestawy mieszane nie mogą być przetwarzane przez konsolidator.

Zobacz https://msdn.microsoft.com/library/x0w2664k.aspx uzyskać więcej informacji dotyczących trybu mieszanego zestawów.

## <a name="mm3xxx-aot"></a>MM3xxx: drzewa obiektów aplikacji

### <a name="mm30xx-aot-general-errors"></a>MM30xx: Błędy drzewa obiektów aplikacji (Ogólne)

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: Można drzewa nie obiektów aplikacji zestawu "{0}"

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: Drzewa obiektów aplikacji z "{0}" żądano, ale nie został znaleziony

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: Wyłączenie drzewa obiektów aplikacji z "{0}" żądano, ale nie został znaleziony

## <a name="mm4xxx-code-generation"></a>MM4xxx: generowanie kodu

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: Szablonu głównego nie można rozszerzyć do `{0}`.

### <a name="mm41xx-registrar"></a>MM41xx: rejestratora

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: Aplikacja korzysta "{0}' struktury, która nie jest zawarty w zestawie SDK MacOS używany do utworzenia aplikacji (platforma została wprowadzona w systemie OSX {2}, podczas gdy tworzysz MacOS {1} zestawu SDK.) Ta konfiguracja nie jest obsługiwana w rejestrze statyczne (pass — rejestratora: dynamiczne jako argument w opcji Mac kompilacji projektu, aby wybrać dodatkowe mmp). W opcjach Mac kompilacji aplikacji można również wybrać nowszej zestawu SDK.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC i łańcuch narzędzi

### <a name="mm51xx-compilation"></a>MM51xx: kompilacji

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: Brak "{0}" kompilatora. Zainstaluj program Xcode składnik "Narzędzia wiersza polecenia".

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: Kompilacja nie powiodła się. Kod błędu: - {0}. Raport o usterce w pliku http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: łączenie

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono.framework MDK nie istnieje. Zainstaluj MDK wersji Mono.framework z http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: Nie można odnaleźć libxammac.a, prawdopodobnie z powodu uszkodzonych instalacji Xamarin.Mac. Zainstaluj ponownie Xamarin.Mac.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204: Nieprawidłowa architektura. X86_64 jest obsługiwana tylko na profile z systemem innym niż klasycznego.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205: Nieprawidłowa architektura "{0}". Prawidłowe architektury to i386 i x86_64 (gdy — profile = przenośnych).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: Nie można zignorować dynamiczne symbol {symbol} (--Ignoruj dynamic — symbol = {symbol}), ponieważ nie została wykryta jako symbolu dynamicznych.

Zobacz [ostrzeżenie równoważne mtouch](~/ios/troubleshooting/mtouch-errors.md#MT5218).

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx: inne narzędzia

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: nie można odnaleźć pkg-config. Zainstaluj Mono.framework z http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: Brak narzędzie "otool". Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: Brak zależności. Zainstaluj program Xcode składnik "narzędzia wiersza polecenia.

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode licencji mogą nie zostały zaakceptowane.  Uruchom program Xcode.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: Łączenie natywnego nie powiodło się z kodem błędu 1.  Sprawdź szczegóły w dzienniku kompilacji.

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: install_name_tool nie powiodło się z kodem błędu "{0}". Sprawdź szczegóły w dzienniku kompilacji.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: środowiska wykonawczego

### <a name="mm800x-misc"></a>MM800x: różne

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Moduł zbierający elementy bezużyteczne Boehm nie jest obsługiwane. Zamiast tego użyj SGen.

