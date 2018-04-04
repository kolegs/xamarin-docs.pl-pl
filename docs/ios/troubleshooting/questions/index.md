---
title: Często zadawane pytania
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: f8264c48b3b4679d45d5603b5637df0016cd3d17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="frequently-asked-questions"></a>Często zadawane pytania

## <a name="general-questions"></a>Pytania ogólne

### <a name="can-i-use-a-mac-vm-with-xamarinmac-vmmd"></a>[Czy do obsługi platformy Xamarin można używać maszyny wirtualnej Mac?](mac-vm.md)
Tak, ale tylko na sprzęcie Mac.

### <a name="how-can-i-downgrade-xcodedowngrade-xcodemd"></a>[Jak obniżyć wersję narzędzia Xcode?](downgrade-xcode.md)
Ten przewodnik zawiera łącza do dostęp do poprzednich wersji Xcode, a także najnowszej wersji.

### <a name="where-can-i-set-my-ios-sdk-locationsios-sdkmd"></a>[Gdzie mogę ustawić moje lokalizacje zestawu SDK systemu iOS?](ios-sdk.md)
Dla większości użytkowników są ustawione na odpowiednie miejsca automatycznie. Ten przewodnik zawiera listę domyślnych lokalizacji zestawu SDK oraz sposobu zmiany ich w razie potrzeby.

### <a name="how-can-i-reenable-developer-options-after-updating-iosupdate-developer-optionsmd"></a>[Jak włączyć opcje dewelopera po zaktualizowaniu systemu iOS](update-developer-options.md)
Usterki dla systemu iOS może spowodować opcje dewelopera zniknąć po zaktualizowaniu wersje systemu iOS, to zaobserwowano podczas przełączania do iOS 8.x. W tym przewodniku opisano, jak można reenabled opcje.

### <a name="user-location-not-working-in-ios-8ios8-user-locationmd"></a>[Lokalizacja użytkownika nie działa w systemie iOS 8](ios8-user-location.md)
W tym przewodniku wyjaśniono, jak edytować info.plist, aby umożliwić lokalizację użytkownika w systemie iOS 8.

### <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logssymbolicate-ios-crashmd"></a>[Gdzie można znaleźć plik dSYM w celu symbolizacji dzienników awarii systemu iOS?](symbolicate-ios-crash.md)
Ten przewodnik zawiera opis podstawowych kroków symbolicating dzienniki awarii systemu iOS, aby pomóc w zdiagnozowaniu awarii (Crash). Także łącza do dodatkowych zasobów do bardziej zaawansowanych technik symbolication & informacje o interpretowanie dzienników awarii systemu iOS.


### <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studioxs-mono-runtimemd"></a>[Jak ustawić zmienne środowiska uruchomieniowego Mono dla projektów systemu iOS w programie Xamarin Studio?](xs-mono-runtime.md)
Jeśli musisz ustawić zmienne środowiskowe dowolnego środowiska uruchomieniowego dla Mono można można ustawić w **opcje projektu > Uruchom > Ogólne** strony.

## <a name="publishing-questions"></a>Pytania dotyczące publikowania

### <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submissioninvalid-bundle-bitcodemd"></a>[Błąd podczas przesyłania do sklepu z aplikacjami: "Nieprawidłowy pakiet — opcje nie może być osadzony w kodu bitowego są wykrywane przy przekazywaniu"](invalid-bundle-bitcode.md)

Przesyłanie aplikacji który _wymagają_ kodu bitowego, takich jak aplikacje watchOS i systemu tvOS, musi odbywać się z Xcode 9.

### <a name="can-i-change-the-output-path-of-the-ipa-fileipa-output-pathmd"></a>[Czy można zmienić ścieżki wyjściowej pliku IPA?](ipa-output-path.md)
Począwszy od 7 cyklu Xamarin można użyć niestandardowych docelowych elementów MSBuild można to osiągnąć.

### <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folderipa-tfsmd"></a>[Jak skopiować pliki wyjściowe IPA do folderu przechowywania TFS](ipa-tfs.md)
Tak, w tym przewodniku opisano sposób.

### <a name="can-i-add-files-to-or-remove-files-from-an-ipa-file-after-building-it-in-visual-studiomodify-ipamd"></a>[Można pliki, aby dodać lub usunąć pliki z pliku IPA po utworzeniu go w programie Visual Studio?](modify-ipa.md)
Tak, można, ale trzeba będzie zwykle, że należy ponownie podpisać `.app` pakietu po wprowadzeniu zmian. Należy pamiętać, że zmodyfikowanie `.ipa` pliku nie jest konieczne w normalnych warunkach użytkowania. W tym artykule podano wyłącznie do celów informacyjnych.

### <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studiocreate-xcarchivemd"></a>[Czy jest możliwe utworzenie archiwum .xcarchive z programu Visual Studio](create-xcarchive.md)
Począwszy od Xamarin 4, obecnie istnieje możliwość tworzenia `.xcarchive` z systemu Windows przez ustawienie `ArchiveOnBuild` właściwości `true`.

### <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--itunesmetadata-disallowed-pathsmd"></a>[Dlaczego nie mogę przesłać aplikacji i otrzymuję błąd dotyczący niedozwolonych ścieżek („iTunesMetadata.plist”)?](itunesmetadata-disallowed-paths.md)
Ten błąd jest w wyniku zmiany w procesie weryfikacji sklepu z aplikacjami firmy Apple. Ten konkretny błąd to _nie_ związane z określonej wersji programu Xamarin został zainstalowany, więc zmiany na starszą wersję będzie _nie_ pomocy. Ten przewodnik prowadzi do więcej informacji na temat sposobu rozwiązania problemu.


## <a name="diagnosing-specific-error-messages"></a>Diagnozowanie określone komunikaty o błędach

### <a name="ios-designer-error-with-registerserviceporterror-registerserviceportmd"></a>[Błąd narzędzia iOS Designer dotyczący portu RegisterServicePort](error-registerserviceport.md)
Błędy `RegisterServicePort` i podobne komunikaty o błędach jak powyżej są często problem z programami szpiegującymi i złośliwym oprogramowaniem na komputerze. Szczegóły tego przewodnika potwierdzenie diagnostyki i informacje dotyczące usuwania programów szpiegujących i złośliwym oprogramowaniem.

### <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychainno-codesigning-keysmd"></a>[Dlaczego moja kompilacja systemu iOS kończy się niepowodzeniem z błędem dotyczącym braku prawidłowych kluczy podpisywania kodu telefonu iPhone w łańcuchu kluczy?](no-codesigning-keys.md)
Ten komunikat o błędzie występuje, gdy danego projektu jest szuka prawidłowe poświadczenia podpisywania kodu, ale można je znaleźć. Podpisywanie kodu jest wymagane do testowania i wdrażania na urządzeniach iOS fizycznych; Podobnie jak Ad hoc & aplikacji magazynu kompilacji.

### <a name="why-does-my-ios-9-app-fail-with-systemexception-failed-to-marshal-the-objective-c-objectexception-marshal-obj-cmd"></a>[Dlaczego moja aplikacja systemu iOS 9 nie działa i generuje błąd wyjątku systemu dotyczący braku możliwości marshalingu obiektu języka Objective-C?](exception-marshal-obj-c.md)
Zmiany interfejsu API dla systemu iOS 9 wymagają, że Konstruktor wywołania zwrotnego używane podczas wywoływania kodu niezarządzanego, jako podstawowy interfejs API teraz oczekuje.

### <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loadederror-mscorlib-not-foundmd"></a>[Błąd środowiska uruchomieniowego: plik mscorlib.dll zestawu nie został znaleziony lub nie można go załadować](error-mscorlib-not-found.md)
Ten problem występuje, gdy *ukryte* `.monotouch-32` i `.monotouch-64` brakuje folderów `.xcarchive` do podpisywania / tworzenia IPA wyzwalają błąd w czasie wykonywania.

## <a name="deprecated"></a>Przestarzałe

> [!IMPORTANT]
> Artykuły poniżej dotyczą problemów, które zostały rozwiązane w nowszych wersjach programu Xamarin. Jednak jeśli problem wystąpi w najnowszej wersji oprogramowania, pliku [nowych usterek](~/cross-platform/troubleshooting/questions/howto-file-bug.md) z Twojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.



### <a name="ipa-file-is-0-bytesipa-zero-bytesmd"></a>[Plik IPA ma rozmiar 0 bajtów](ipa-zero-bytes.md)
Wystąpiły znane problemy w poprzednich wersjach programu Xamarin, która może spowodować, że plik IPA w systemie Windows jako 0 bajtów.

### <a name="ibtool-error-the-operation-couldnt-be-completederror-ibtoolmd"></a>[Błąd narzędzia IBTool: nie można ukończyć operacji.](error-ibtool.md)
Apple [stałej](https://developer.apple.com/library/ios/releasenotes/DeveloperTools/RN-Xcode/Chapters/xc6_release_notes.html) to `ibtool` usterki w programie Xcode 6.1.1, więc uaktualnianie xcode 6.1.1 lub nowszej jest najprostszym.

### <a name="error-mt1009-could-not-copy-the-assemblyerror-mt1009md"></a>[Błąd MT1009: nie można skopiować zestawu](error-mt1009.md)
Ma to wpływ na użytkowników systemu Xamarin.iOS 7.2.6. Ten problem jest następstwem uprawnienia do plików wymagających wyższej uprawnienia po zainstalowaniu platformy Xamarin.iOS przy użyciu konta innego użytkownika następnie dewelopera konta głównego.

### <a name="systemexception-amdevicenotificationsubscribe-returned-exception-amddevicenotificationsubscribemd"></a>[Zwracany jest błąd System.Exception AMDeviceNotificationSubscribe...](exception-amddevicenotificationsubscribe.md)
Ten komunikat może się pojawić okna dialogowego błędu, przy pierwszym uruchomieniu programu Visual Studio dla komputerów Mac lub w `mtbserver.log` pliku. Należy pamiętać, że jest to rzadko problem. Jeśli program Visual Studio występują problemy z połączeniem hosta kompilacji Mac, są inne błędy, które mogą być wyświetlane w `mtbserver.log` pliku.

### <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequestmdocarchivetomsxdocconverter-not-foundmd"></a>[Błąd „MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest”](mdocarchivetomsxdocconverter-not-found.md)
Ten błąd może występować w `Mac Server Log` w programie Visual Studio.
