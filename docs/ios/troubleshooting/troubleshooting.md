---
title: "Rozwiązywanie problemów"
ms.topic: article
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/0201
ms.openlocfilehash: 95d4bfd78ee77f9afafce61f52b7874299df543d
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="troubleshooting"></a>Rozwiązywanie problemów

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS nie można rozpoznać System.ValueTuple

Ten błąd występuje z powodu niezgodności z programem Visual Studio.

- **Visual Studio 2017 Update 1** (15,1 lub starsza wersja) jest zgodna tylko z **System.ValueTuple NuGet 4.3.0** (lub starszy).

- **Visual Studio 2017 Update 2** (wersja 15.2 lub nowsza) jest zgodna tylko z **System.ValueTuple NuGet 4.3.1** lub nowszej.

Wybierz prawidłowe System.ValueTuple NuGet odpowiadający z instalacją programu Visual Studio 2017 r.


## <a name="receiving-error-retrieving-update-information-error-message"></a>Odbieranie komunikatu o błędzie "Błąd podczas pobierania informacji o aktualizacji"

Podczas próby aktualizacji oprogramowania, a ten komunikat o błędzie pojawia się, spróbuj ponowne uruchomienie programu IDE i wylogowywanie, a następnie powtórz w do swojego konta w IDE.

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Jak utworzyć gniazda lub akcje z konstruktora interfejsu?

Wraz z wprowadzeniem projektanta Xamarin dla systemu iOS w programie Visual Studio for Mac i Visual studio Xamarin.iOS deweloperzy mogą teraz korzystać z Tworzenie interfejsu użytkownika za pośrednictwem Scenorys i .xibs. Zapoznaj się [Hello, iOS](~/ios/get-started/hello-ios/index.md) przewodniki, aby uzyskać więcej informacji na temat używania projektanta.

Można także skorzystać firmy Apple [gniazda](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) i [akcje](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html) przewodniki, aby uzyskać więcej informacji o korzystaniu z gniazda i akcje w IB.

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>Notsupportedexception — zgłasza System.Text.Encoding.GetEncoding

Być może korzystasz, kodowanie, które nie jest domyślnie dodawany. Sprawdź [internacjonalizacji](~/ios/app-fundamentals/localization/index.md) stronę, aby dowiedzieć się, jak dodać obsługę więcej kodowania.

## <a name="systemmissingmethodexception-anything-else"></a>System.MissingMethodException (inny)

Element członkowski, prawdopodobnie został usunięty przez konsolidator i w związku z tym nie istnieje w zestawie w czasie wykonywania.  Istnieje kilka rozwiązań to:

-  Dodaj [[zachować]](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute) atrybutu do elementu członkowskiego.  Uniemożliwi to konsolidator usunięcie go.
-  Podczas wywoływania [mtouch](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29) , użyj **- nolink** lub **- linksdkonly** opcje. -    **-nolink** powoduje wyłączenie wszystkich połączeń.
-    **-linksdkonly** łączenie zestawów platformy Xamarin.iOS-pod warunkiem, od takich jak *monotouch.dll* lub xamarin.ios.dll.

Należy pamiętać, że zestawy są połączone, dzięki czemu wynikowy plik wykonywalny jest mniejszy; w związku z tym wyłączanie łączenia może powodować większe plik wykonywalny, niż jest to pożądane.

## <a name="you-are-getting-a-modelnotimplementedexception"></a>W przypadku uzyskiwania ModelNotImplementedException

W przypadku uzyskiwania tego wyjątku oznacza to, że wywołujesz podstawowej. (Metoda) dla klasy, która zastępuje modelu. Nie trzeba wywołać metodę podstawową w klasie dla modeli (są to klasy, które są oznaczone atrybutem [Model]).

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>Ta klasa nie jest wartość klucza kodowania zgodne klucza XXXX

Jeśli ten błąd podczas ładowania pliku NIB oznacza, że wartość, którą XXXX nie został znaleziony w klasie zarządzanej. Oznacza to, Brak deklaracji następująco:

```csharp
[Connect]
TypeName XXXX {
   get {
       return (TypeName) GetNativeField ("XXXX");
   }
   set {
       SetNativeField ("XXXX", value);
   }
}
```

Powyższej definicji jest generowane automatycznie przez program Visual Studio dla komputerów Mac dla XIB pliki, które można dodać do programu Visual Studio dla komputerów Mac w `NAME_OF_YOUR_XIB_FILE.designer.xib.cs` pliku.

Ponadto typy powyżej kodem musi być podklasą klasy [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).  Jeśli typ zawierający jest w przestrzeni nazw, również powinny mieć [[zarejestrować]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) atrybut, który zawiera nazwę typu bez przestrzeni nazw (zgodnie z konstruktora interfejs nie obsługuje przestrzeni nazw w typach):

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Nieznana klasa XXXX w pliku konstruktora interfejsu

Ten błąd jest generowany, jeśli Definiowanie klasy, w plikach konstruktora interfejsu, ale nie podano rzeczywistego wykonania dla niego w kodzie C#.

Należy dodać kodu następująco:

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException: Brak znaleziono Foo.Bar::ctor(System.IntPtr) konstruktora

Ten błąd jest generowany w czasie wykonywania, gdy kod próbuje utworzyć wystąpienia klasy, które odwołuje się do pliku kompilatora interfejsu. Oznacza to, czy zapomniano dodać konstruktora, który przyjmuje jeden IntPtr jako parametr.

Konstruktor z dojściem IntPtr służy do powiązanie obiektów zarządzanych z ich niezarządzane oświadczenia.

Aby rozwiązać ten problem, Dodaj następujący wiersz kodu do klasy Foo.Bar:

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>Typ {Foo} nie zawiera definicji dla `GetNativeField' and no extension method `GetNativeField' typu odnaleźć {Foo}

Jeśli ten błąd wystąpi w projektanta wygenerowane pliki (*. xib.designer.cs), oznacza to jedna z następujących:

 **1) braku częściowej klasy lub klasy podstawowej**

Wygenerowany przez projektanta klas częściowych musi mieć odpowiednie klasy częściowe w kodzie użytkownika, które dziedziczą z niektórych podklasą klasy `NSObject`, często `UIViewController`. Upewnij się, że masz klasy dla typu, który jest nadanie błędu.

 **2) domyślnej przestrzeni nazw zmienione**

Pliki designer są generowane przy użyciu projektu domyślnych ustawień obszaru nazw. Jeśli masz zmienić te ustawienia, lub zmieniono jego nazwę projektu, wygenerowane klasy częściowe może nie być już w tej samej przestrzeni nazw, jak ich odpowiedniki kodu użytkownika.

Namespace ustawienia można znaleźć w oknie dialogowym Opcje projektu. Domyślny obszar nazw znajduje się w **główne Ustawienia -> Ogólne** sekcji. Jeśli to pole jest puste, nazwa projektu jest używany jako domyślny. Bardziej zaawansowane ustawienia przestrzeni nazw można znaleźć w **kodu źródłowego -> zasady nazewnictwa platformy .NET** sekcji.

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>Ostrzeżenie dla akcji: Metoda prywatna "Foo" nie jest nigdy używane. (CS0169)

Akcje dla plików konstruktora interfejsu są połączone elementy widget przez odbicie w czasie wykonywania, więc to ostrzeżenie jest oczekiwane.

Można użyć "ostrzeżenie #pragma Wyłącz 0169" "ostrzeżenie #pragma Włącz 0169" wokół czynności użytkownika, jeśli chcesz pominąć to ostrzeżenie tylko dla tych metod, lub Dodaj 0169 do pola "Ignoruj ostrzeżenia" w opcjach kompilatora, aby wyłączyć dla całego projektu (nie zalecane).

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch nie powiodło się z następującym komunikatem: nie można otworzyć zestawu "/ path/to/yourproject.exe"

Jeśli zostanie wyświetlony ten komunikat o błędzie, zazwyczaj problem jest ścieżka bezwzględna do projektu zawiera spację. Ten problem zostanie rozwiązany w przyszłej wersji platformy Xamarin.iOS, ale można obejść ten problem, przenosząc projektu do folderu bez spacji.

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Wersja sqlite3 jest stary — Przeprowadź uaktualnienie do co najmniej v3.5.0!

Dzieje się tak, gdy wykonaj następujące czynności:

1.  Use Mono.Data.Sqlite
1.  Użyć systemu Mac OS X Leopard (10.5)
1.  Uruchom aplikację w symulatorze.


Problem jest, że Mono jest przechwycenie OS X `libsqlite3.dylib`, nie iPhoneSimulator w `libsqlite3.dylib` pliku. Aplikacja *będzie* pracy na urządzeniu, ale nie z symulatora.

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>Wdrażanie do urządzenia kończy się niepowodzeniem z elementu System.Exception: AMDeviceInstallApplication zwrócił 3892346901

Ten błąd oznacza, że konfiguracji identyfikatora pakietu/certyfikatu podpisywania kodu nie odpowiada profilu inicjowania obsługi administracyjnej zainstalowanej na urządzeniu.  Upewnij się, ma odpowiednie certyfikatu wybranego w opcje projektu -> iPhone podpisywania pakietu i identyfikator pakietu poprawne określonego w opcji projektu -> iPhone aplikacji

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>Uzupełnianie kodu nie działa w programie Visual Studio dla komputerów Mac

Upewnij się, że używasz najnowszej wersji programu Visual Studio for Mac i platformy Xamarin.iOS

Jeśli ten problem występuje nadal, skontaktuj się z [o usterce](http://monodevelop.com/Developers#Reporting_Bugs), dołączenia **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{sygnatury czasowej} .log**, i **składniki-{sygnatury czasowej} .log** pliki dziennika.

Jeśli wszystkie inne niepowodzenia, można spróbować usunięcie pamięci podręcznej uzupełniania kodu, dzięki czemu zostanie ponownie wygenerowany:

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

Pamiętaj, że poprawnie wpisz następujące polecenie lub przypadkowo można usunąć ważnych plików.

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>Visual Studio for Mac ulega awarii podczas kopiowania tekstu

Popularne narzędzia Mac QuickSilver, narzędzi Google i pasek uruchamiania ma funkcje Schowka uszkodzony Visual Studio dla komputerów Mac w pamięci. W oknie dialogowym Opcje ich można wyświetlić listę Visual Studio dla komputerów Mac jako proces, który nie mogą one kolidować z.

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Visual Studio for Mac narzeka o 2.4 Mono wymagane

Jeśli aktualizacji programu Visual Studio dla komputerów Mac z powodu niedawnej aktualizacji, a gdy spróbujesz ją uruchomić ponownie go narzeka o 2.4 Mono nie jest obecny, musisz wykonać jest [uaktualnić instalację Mono 2.4](http://www.go-mono.com/mono-downloads/download.html).  

Mono 2.4.2.3_6 rozwiązuje niektóre istotne problemy, które uniemożliwił Visual Studio for Mac zwiększające czasami zawieszone programu Visual Studio for Mac podczas uruchamiania lub uniemożliwił generowaną baza danych uzupełniania kodu.

Po zainstalowaniu nowego Mono programu Visual Studio for Mac zostanie uruchomione zgodnie z oczekiwaniami.

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>Potwierdzenia w... /.. /.. /.. /mono/METADATA/Generic-Sharing.c:704, warunek "oti" nie zostały spełnione

W przypadku otrzymania następującym Śladem stosu:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

Oznacza to, że łączysz się skompilować z kodem przycisku suwaka do projektu biblioteki statycznej. Począwszy od wersji zestawu SDK iPhone 3.1 (lub nowszej w momencie pisania tego dokumentu) Firma Apple wprowadziła usterki w ich konsolidatora podczas łączenia z systemem innym niż Thumb kodu (Xamarin.iOS) z kodem Thumb (biblioteki statycznej). Należy połączyć się z wersją z systemem innym niż pamięci przenośnej biblioteki statyczne, aby zminimalizować ten problem.

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>System.ExecutionEngineException: Podjęto próbę (JIT kompilacji — metoda (zarządzane — do otoki) Foo[]:System.Collections.Generic.ICollection'1.get_Count)

Sufiks [] wskazuje, czy użytkownik lub biblioteki klas są wywołania metody na tablicy za pomocą kolekcji uniwersalnej, takie jak IEnumerable <>, ICollection <> lub IList <>. Jako rozwiązanie alternatywne można jawnie życie kompilatora drzewa obiektów aplikacji do uwzględnienia takiej metody, wywołując metodę samodzielnie i upewniając się, że ten kod jest wykonywany przed wywołaniem, który wywołał wyjątek. W takim przypadku można zapisać:

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Która wymusi kompilatora drzewa obiektów aplikacji do uwzględnienia metody get_Count.

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Programu Visual Studio for Mac Edytor źródła jest bardzo wolno

Czasami programu Visual Studio for Mac Edytor źródła staje się bardzo wolno znajdujących się zawieszenie przez kilka sekund między wpisywania znaków.

Ten problem jest bardzo rzadko i bardzo trudne do odtworzenia — on zwykle nie można odtworzyć na tym samym komputerze po ponownym uruchomieniu programu Visual Studio dla komputerów Mac. Z tego względu prosimy o wyrażenie go, jeśli można wykonać kilka kroków debugowania przed ponownym uruchomieniem programu Visual Studio dla komputerów Mac i wysyłać wyniki do nas.

1.  Spróbuj zamknąć kartę Edytor i ponowne otwarcie go. Trwa niewielki edycji lub przenoszenia karetkę do momentu spowolnienie wystąpi ponownie?
1.  Wyłączyć "Światła synchronizacji" za pomocą narzędzia developer "Kwarcowy Debug" (który można znaleźć przy użyciu Spotlight) i sprawdź, czy wydajność edytora źródła jest przywrócone na normalny.
1.  Spróbuj powtórzyć krok (1) z synchronizacją światła nadal jest wyłączona.
1.  Jeśli Edytor zawiesza się więcej niż kilka sekund, spróbuj uruchomić "killall — Zamknij [Visual Studio for Mac]" w terminalu, gdy jest ona rozłączona. Może być trudne do czasu polecenia kill nastąpić odpowiada edytora, ale jest niezbędne, aby to zrobić, ponieważ polecenie wymusza Mono można zapisywać dane śledzenia stosu wszystkich wątków w dzienniku MD, możemy użyć, aby dowiedzieć się, jakie stanu wątki są w trakcie odpowiada XS.



Dołącz dzienniki XS **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**, **AndroidTools-{sygnatury czasowej} .log**, i **składniki-{sygnatury czasowej} .log**(w starszych wersjach XS/MonoDevelop, po prostu wyślij **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**).

 **Uwaga: Powyższy problem został rozwiązany w ostatnim 2.2 XS**

## <a name="compiled-application-is-very-large"></a>Skompilowana aplikacja jest bardzo duży

Aby zapewnić obsługę debugowania, kompilacji do debugowania zawiera dodatkowy kod. Projekty utworzony w trybie wersji są ułamek rozmiaru.

Począwszy od platformy Xamarin.iOS 1.3 debugowania kompilacji włączone debugowanie obsługę każdego pojedynczego składnika Mono (każdej metody w każdej klasie struktury).  

Z Xamarin.iOS 1.4 Firma Microsoft wprowadzi skuteczniejszą szczegółowych metodę na potrzeby debugowania, domyślnie będzie zapewniają Instrumentacji debugowania tylko kodu i bibliotek i nie to zrobić dla wszystkich [Mono zestawy](~/cross-platform/internals/available-assemblies.md) (to będzie nadal możliwe, jednak należy wyrazić zgodę na debugowanie te zestawy).

## <a name="installation-hangs"></a>Instalacja zawiesza się

Zarówno Mono i Xamarin.iOS instalatorów wykraczać, jeśli masz uruchamiania symulatora telefonu iPhone. Ten problem nie jest ograniczona do Mono lub Xamarin.iOS, jest to problem spójne przez dowolne oprogramowanie, które próbuje zainstalować oprogramowanie na System MacOS śniegu Leopard przypadku telefonów iPhone symulator jest uruchomiony w czasie instalacji.

Upewnij się, musisz zamknąć symulatora telefonu iPhone i ponów próbę instalacji.

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>Za mało trampolines typu 0

Ten komunikat zostanie wyświetlony podczas uruchamiania urządzenia, można utworzyć więcej typu trampolines 0 (typ określonych), modyfikując sekcji "iPhone kompilacji" Opcje projektu.  Chcesz dodać obiekty docelowe kompilacji dodatkowe argumenty dla urządzenia:

 `-aot "ntrampolines=2048"`

Domyślna liczba trampolines to 1024.  Spróbuj zwiększyć ten numer, dopóki nie jest wystarczająca ilość dla aplikacji.

## <a name="ran-out-of-trampolines-of-type-1"></a>Za mało trampolines typu 1

Jeśli użycie typów ogólnych cykliczne, ten komunikat może wystąpić na urządzeniu.  Można utworzyć więcej typ trampolines 1 (typ RGCTX), modyfikując sekcji "iPhone kompilacji" Opcje projektu.  Chcesz dodać obiekty docelowe kompilacji dodatkowe argumenty dla urządzenia:

 `-aot "nrgctx-trampolines=2048"`

Domyślna liczba trampolines to 1024.  Spróbuj zwiększyć ten numer, dopóki nie jest wystarczająca ilość dla użycie typów ogólnych.

## <a name="ran-out-of-trampolines-of-type-2"></a>Za mało trampolines typu 2

Jeśli wprowadzisz dużego wykorzystania interfejsów, ten komunikat może wystąpić na urządzeniu.
Można utworzyć więcej typ trampolines 2 (typ Osadzeń IMT), modyfikując sekcji "iPhone kompilacji" Opcje projektu.  Chcesz dodać obiekty docelowe kompilacji dodatkowe argumenty dla urządzenia:

 `-aot "nimt-trampolines=512"`

Domyślna liczba trampolines IMT Thunk to 128.  Spróbuj zwiększyć ten numer, dopóki nie jest wystarczająca ilość dla Ciebie interfejsów.

## <a name="debugger-is-unable-to-connect-with-the-device"></a>Debuger nie może połączyć się z urządzeniem

Podczas uruchamiania debugowania konfigurację urządzenia, zobaczysz debugera, Pokaż okno dialogowe wskazujący, że próbuje połączyć się z aplikacją. Istnieje kilka przyczyn, debuger może nie być w stanie połączyć się z aplikacją, w zależności od trybu używasz połączenia (USB lub Wi-Fi).

 **Jeśli urządzenie i host debuger znajdują się w różnych sieciach**, zaporą lub siecią prywatną uniemożliwiają aplikacji połączenie się z hostem debugera w trybie sieci Wi-Fi.

 **Visual Studio for Mac nie można zbadać poprawny adres IP hosta**. W sieci Wi-Fi tryb programu Visual Studio for Mac daje aplikacji wszystkie adresy IP może znaleźć hosta i aplikacja próbuje je wszystkie aby zobaczyć, jeśli on ich używać do łączenia z programu Visual Studio dla komputerów Mac.

 **Inne urządzenie jest podłączony do portu USB na hoście.** W niektórych przypadkach do USB podłączone inne urządzenia portów na hoście być znane jakiś sposób zakłócać debugowania w trybie USB.

Jeśli tryb sieci Wi-Fi lub USB nie działa, możesz łatwo spróbować innych: w programie Visual Studio dla komputerów Mac, Otwórz preferencje, przejdź do strony Preferencje/debugera/iPhone debugera i Przełącz pole wyboru "Debug urządzeń z systemem iOS za pośrednictwem sieci Wi-Fi, zamiast USB".   Jeśli nie działa, możesz wyświetlić więcej informacji o awarii w konsoli urządzenia w trybie informacji pełnej (który jest włączona, dodając "-v - v - v" do argumentów mtouch dodatkowe opcje projektu).

## <a name="error-134-mtouch-failed-with-the-following-message"></a>Błąd 134: mtouch nie powiodło się z następującym komunikatem:

Ten błąd może wygenerowany, jeśli próbujesz kompilacji z - nolink w stylu platformy Xamarin.iOS 1.4 wersji. Ten błąd można obejść, określając dodatkowe argumenty monodevelop konfiguracji projektu.

Dodaj argument

 `-nosymbolstrip`

a problem należy rozwiązać.

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Tożsamość dystrybucji nie jest wyświetlane w programie Visual Studio dla projektu Mac opcje podpisywania

Visual Studio for Mac 2.2 zawiera usterkę, która powoduje, że nie, aby wykryć dystrybucji certyfikaty, które zawierają przecinkami. Zaktualizuj dla programu Visual Studio for Mac 2.2.1.

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>Błąd "AFCFileRefWrite zwrócił: 1" podczas przekazywania

Podczas przekazywania aplikacji na urządzeniu może zostać wyświetlony błąd "AFCFileRefWrite zwrócił: 1". Może to nastąpić, jeśli plik o zerowej długości.

## <a name="error-mtouch-failed-with-no-output"></a>Błąd "nie powiodło się bez wpisu mtouch"

Bieżącej wersji platformy Xamarin.iOS i Visual Studio for Mac się niepowodzeniem, gdy nazwa projektu lub katalogu, w którym przechowywane są rozwiązanie lub projekt zawierać spacji.
Aby rozwiązać ten problem:


-  Upewnij się, że ani projektu lub katalogu, w którym są przechowywane zawiera spację.
-  Upewnij się, że nazwa projektu nie zawiera spacji w "Main ustawień projektu".

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>Błąd "binary, który został przekazany nieprawidłowy. Wersję wstępną beta zestawu SDK użytego do skompilowania aplikacji"

Zazwyczaj przyczyną tego błędu z projektu, który został uruchomiony w rozwoju iPad, zanim Xamarin.iOS 2.0.0 został zwolniony, prawdopodobnie zostały niektóre klucze w Twojej Info.plist, takich jak:

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Ta kluczy powinien zostać usunięty, zgodnie z programu Visual Studio for Mac je obsługuje można automatycznie.

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>Błąd "wersję wstępną beta zestawu SDK użytego do tworzenia aplikacji"

(Przekazanych przez Ed Anuff)

Wykonaj następujące kroki:

-  Zmień wersję zestawu SDK w iPhone kompilacji 3.2 lub iTunes połączenia spowoduje odrzucenie go podczas przekazywania, ponieważ jest niewidoczny iPad zgodne aplikacji utworzony przy użyciu zestawu SDK wcześniejszą niż wersja 3.2
-  Tworzenie niestandardowych Info.plist dla projektu i ustawić MinimumOSVersion 3.0 w nim.   Spowoduje to zastąpienie wartości MinimumOSVersion 3.2 przez platformy Xamarin.iOS.   Jeśli nie zrobisz, aplikacja nie będzie można uruchamiać na telefonie iPhone.
-  Połącz odbudowy, zip i przekazanie iTunes.

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>Nieobsługiwany wyjątek: System.Exception: nie można odnaleźć someSelector selektora: na {type}

Ten wyjątek jest spowodowany przez jeden z trzech zdarzeń:


1.  Bez stosowania odpowiedniego atrybutu [eksportu] do metody podano selektora do środowiska wykonawczego języka Objective C
1.  Włączono pełny linking lub nie zastosować atrybut [Preserve] do metody ed [eksportu].
1.  Atrybut [eksportu] zostały zastosowane do prywatnej metody w typie dziedziczonym.


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>Plik MainWindow.xib.designer.cs nie został zaktualizowany.

Wystąpił błąd w Xamarin Studio 2.4, który spowodował nie grupy plik MainWindow.xib z plikiem MainWindow.xib.designer w nowych projektach. Oznacza to, że nie zaktualizuje kod projektanta dla tego określonego pliku.

Ten problem zostanie usunięty w wersji programu Visual Studio for Mac, są dostępne w jego wbudowanych updater, sprawdź, czy przy użyciu nowszej wersji.

Istniejące projekty można rozwiązać przez usunięcie (Jeśli nie usuniesz) xib i jego pliku projektanta, następnie dodać ją ponownie. To powinno ponownie grupy plików poprawnie.

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>UIAlertView lub UIActionSheet znikają po tworzona

Jeśli masz kod następująco:

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

znajduje się obiekt "actionSheet" jako zmiennej tymczasowej w funkcji a jak funkcja zakończenie, obiekt do wyczyszczenia, więc kończy się jako elementu bezużytecznego zbierane.

Aby rozwiązać ten problem, należy utrzymywać odwołania do "actionSheet" poza metodę, gdzieś, który będzie funkcjonować poza metodę.

## <a name="project-always-runs-in-the-ipad-simulator"></a>Projekt zawsze jest uruchamiany w symulatorze tabletów iPad

Instalator iPhone SDK 4.0 instaluje SDK 2 - 3.2 zestawu SDK do tworzenia aplikacji tylko do iPad i 4.0 zestaw SDK, używany do tekstowe iPhone oraz aplikacje uniwersalne. Instaluje również 3.2 symulator, która symuluje tylko iPad, i 4.0 symulator, która symuluje iPhone lub telefonu iPhone 4. Wszystkie zestawy SDK i symulatorów starsze są usuwane.

Kompilowanie aplikacji programu Visual Studio for Mac iPhone projektu kompilacji opcje Dołącz ustawienie wersji zestawu SDK, który będzie używany. To ustawienie znajduje się w **kompilacji -> Opcje projektu -> iPhone kompilacji**.

Nowe projekty w programie Visual Studio for Mac jako ich domyślne ustawienie SDK najstarsze zainstalowany zestaw SDK, i, jeśli określony zestaw SDK nie istnieje, Visual Studio for Mac użyje najbliższego który można znaleźć tworzenie aplikacji. Wykonano tak, aby projektów nie zawsze będą requre najnowszego zestawu SDK. Jednak obecnie w efekcie 3.2 zestawu SDK są używane — które powoduje w symulatorze iPad używany.

Aby rozwiązać ten problem przy użyciu zestawu SDK w wersji 4.0, przejdź do **kompilacji -> Opcje projektu -> iPhone kompilacji**> i zmień wartość zestawu SDK "4.0", korzystając z pola listy rozwijanej. Należy to zrobić dla każdej kombinacji platformy, dostęp za pomocą list rozwijanych w górnej części panelu i konfiguracji.

Wersja zestawu SDK nie należy mylić z ustawieniem "Wersja minimalna wersja systemu operacyjnego".
Ta wartość musi odpowiadać wartości wersji zestawu SDK — minimalna wersja systemu operacyjnego zainstaluje aplikację, która może być starsza niż zestaw SDK, tak długo, jak używać tylko interfejsów API, który istnieje w starszej wersji systemu operacyjnego lub zabezpieczenia użyj nowszej funkcji za pomocą środowiska uruchomieniowego OS wersji Sprawdź ma wpływ łączy. Powinien być skonfigurowany do najstarsza wersja systemu operacyjnego, na którym testowanie aplikacji.

Należy zauważyć, że **projektu -> iPhone docelowy symulatora**> menu może służyć do pobrania symulator używany domyślnie podczas uruchamiania/debugowania projektu. Ponadto **Uruchom -> Uruchom z**> menu może służyć do pobrania określonego symulatora uruchamiania

## <a name="ibtool-returns-error-133"></a>ibtool zwraca błąd 133

Oznacza to, że masz zainstalowany 4 XCode.   W środowisku XCode 4 usunięto ibtool narzędzia, nie jest już możliwe edytowanie plików XIB autonomiczne narzędzie.

Jeśli chcesz użyć konstruktora interfejsu, zainstaluj [XCode serii 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792), dostępne w witrynie sieci web firmy Apple.

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Nie można utworzyć powiązania wyświetlania typu mime: application/vnd.apple -<wbr/>kompilatora interfejsu"

Ten błąd wystąpi, jeśli próbujesz utworzyć iPhone interfejsu użytkownika z projektu z systemem innym niż iPhone. Upewnij się, że rozpoczyna się z rozwiązaniem iPhone/iPad, nie jest możliwe po prostu Dodaj do projektu z systemem innym niż — iPhone/iPad iPhone elementy interfejsu użytkownika.

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>Uruchamianie awarii podczas wykonywania w symulatorze systemu iOS

Jeśli otrzymasz awarii środowiska uruchomieniowego (sigsegv —) wewnątrz symulatora wraz ze śladu stosu, który wygląda następująco:

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
.. .i prawdopodobnie jednej (lub więcej) starych zestawu w symulatorze katalogu aplikacji. Takie zestawy mogą istnieje, ponieważ Apple iOS simulator dodaje i aktualizuje pliki, ale nigdy nie usunie je. W takim przypadku następnie najlepszym rozwiązaniem jest menu symulator i wybierz polecenie "Resetowania i zawartości i ustawień...".   

**Ostrzeżenie:** spowoduje to usunięcie wszystkich plików, aplikacji i danych z symulatora.   Następnym wykonaniu aplikacji, Visual Studio for Mac wdroży go w symulatorze i nie będzie żadnych stare, stare zestawu spowodować awarię (crash).

## <a name="simulator-hangs-during-application-installation"></a>Symulator zawieszeń podczas instalacji aplikacji

Może się to zdarzyć, gdy obejmują nazwy aplikacji "." (kropka) w nazwie.
Jest to zabronione jako nazwę pliku wykonywalnego w CFBundleExecutable — nawet wtedy, gdy może działa w wielu przypadkach (na przykład urządzenia).

 * "Wartość nie może zawierać żadnych rozszerzeń na nazwę." - [http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>Błąd: "typ atrybutu niestandardowego 0x43 nie jest obsługiwana" gdy podwójne kliknięcie .xib plików

Jest to spowodowane próby otwarcia plików .xib, gdy zmienne środowiskowe są ustawione niepoprawnie. To nie powinno się zdarzyć z normalnego użycia programu Visual Studio dla Mac/Xamarin.iOS i ponownym otwarciu programu Visual Studio for Mac z /Applications powinno rozwiązać problem.

Podczas próby aktualizacji oprogramowania, a ten komunikat o błędzie pojawia się, sprawdź wiadomości e-mail *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>Aplikacja jest uruchamiana w symulatorze, ale nie powiedzie się na urządzeniu

Ten problem można manifestu w wielu formularzach, a nie zawsze tworzy spójne błędu. Jeśli aplikacja zawiera .xib, sprawdź, upewnij się, że **Akcja kompilacji** na .xib jest ustawiona na **InterfaceDefinition**. Jest to domyślne działanie kompilacji dla .xibs.

Aby sprawdzić działanie kompilacji, kliknij prawym przyciskiem myszy plik .xib i wybierz **Akcja kompilacji**.


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: Dane są niedostępne do kodowania 437

Po tym 3 bibliotek strony w aplikacji platformy Xamarin.iOS, może być błąd pojawia się w formie "System.NotSupportedException: dane są niedostępne do kodowania 437" podczas próby skompilować i uruchomić aplikację. Na przykład, bibliotek, takich jak `Ionic.Zip.ZipFile`, może zgłosić wyjątek podczas operacji.

To będzie możliwe, otwierając opcje projektu platformy Xamarin.iOS, przechodząc do **kompilacji systemu iOS** > **internacjonalizacji** i sprawdzanie **zachodnie** Przystosowywanie do warunków międzynarodowych.



<a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>Nie można uaktualnić Indie/firmy z konta wersji próbnej

Jeśli ostatnio zakupione Xamarin.iOS i wcześniej uruchomione z wersji próbnej platformy Xamarin.iOS, może być konieczne wykonaj następujące kroki, aby uzyskać tę zmianę licencji pobrana przez Visual Studio dla komputerów Mac lub Visual Studio.

-  Zamknij program Visual Studio for Mac/Visual Studio
-  Usuń wszystkie pliki z ~/Library/MonoTouch dla komputerów Mac lub %PROGRAMDATA%\MonoTouch\License\ dla systemu Windows
-  Otwórz ponownie program Visual Studio for Mac/Visual Studio i utworzyć projektu platformy Xamarin.iOS


## <a name="receiving-activation-incomplete-error-message"></a>Odbieranie komunikatu o błędzie "Aktywacji niekompletne"

Ten problem może wystąpić w przypadku używania platformy Xamarin.iOS dla programu Visual Studio. Aby rozwiązać ten problem, należy wysłać dzienniki z następującej lokalizacji w celu [ contact@xamarin.com ](mailto:contact@xamarin.com).

-  Lokalizacja dziennika: %LocalAppData%/Xamarin/Logs
