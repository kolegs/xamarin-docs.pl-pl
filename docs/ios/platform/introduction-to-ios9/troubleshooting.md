---
title: Xamarin.iOS 9 — Rozwiązywanie problemów
description: Ten artykuł zawiera wskazówki dotyczące rozwiązywania problemów różnych do pracy z systemem iOS 9 w platformy Xamarin.iOS. Wskazówki obejmują analiza kodu XML, symulatorów ograniczenia układu, problemy z siecią i wiele innych tematach.
ms.prod: xamarin
ms.assetid: DCE83E36-CBD9-4D96-8E7F-384CB8A54563
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: c44d737efcf5092eb4b27d5311271005de65318b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787666"
---
# <a name="xamarinios-9--troubleshooting"></a>Xamarin.iOS 9 — Rozwiązywanie problemów

_Ten artykuł zawiera kilka porad dotyczących rozwiązywania problemów do pracy z systemem iOS 9 w aplikacji platformy Xamarin.iOS._

## <a name="there-was-a-problem-parsing-the-xml"></a>Wystąpił problem podczas analizowania pliku XML

Xamarin iOS Projektant nie obsługuje jeszcze funkcji Xcode 7. Scenorys nie będzie można załadować w Projektancie z _"Wystąpił problem podczas analizowania pliku XML"_ podczas próby użycia nowego systemu iOS 9 (Xcode 7) projektanta elementów, takich jak StackView.

iOS projektanta obsługę funkcji Xcode 7 jest przeznaczony dla nową wersją funkcji 6 cyklu. Wersja zapoznawcza 6 cyklu jest obecnie dostępny w kanału alfa i ma ograniczoną obsługę nowych funkcji Xcode 7.

Obejście częściowego dla programu Visual Studio dla komputerów Mac: kliknij prawym przyciskiem myszy scenorysu, a następnie wybierz pozycję **Otwórz za pomocą** > **Xcode interfejsu konstruktora**.

## <a name="where-are-the-ios-8-simulators"></a>Gdzie są iOS 8 symulatorów?

Jeśli zainstalowano Xcode 7 (lub nowszego) automatycznie zastąpi wszystkie symulatorów iOS 8 z systemem iOS 9 symulatorów domyślnie. Nadal należy przeprowadzić testy w systemie iOS 8, można uruchomić Xcode, a następnie pobrać i zainstalować symulatorów iOS 8.

W programie Xcode, wybierz **Xcode** następnie menu **Preferencje...**   >  **Pobiera**:

[![](troubleshooting-images/ios8.png "Pobiera symulatorów iOS 8")](troubleshooting-images/ios8.png#lightbox)

Kliknij przycisk **Sprawdź i zainstaluj teraz** przycisk, aby ponownie zainstalować symulatorów iOS 8.

## <a name="layout-constraint-with-leftright-attribute-errors"></a>Układ ograniczenie atrybutu prawej/lewej strony błędów

IOS 8 (i przed) elementy interfejsu użytkownika w Scenorys można używać różnych zarówno **prawej** & **lewej** atrybutów (`NSLayoutAttributeRight` & `NSLayoutAttributeLeft`) i  **Początkowe** & **końcowych** atrybutów (`NSLayoutAttributeLeading` & `NSLayoutAttributeTrailing`) w taki sam układ.

Jeśli tego samego scenorysu w uruchomione w systemie iOS 9, spowodują wyjątek w następującym formacie:

> Trwa przerywanie wykonywania aplikacji z powodu nieprzechwycony wyjątek "NSInvalidArgumentException", przyczyny: "*** + [NSLayoutConstraint constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:]: ograniczenie nie może być pomylone wiodących/kończących atrybut i atrybut prawej i lewej. Użyj wiodących/kończących dla obu lub żaden. "

System iOS 9 wymusza układów użyć **prawo** & **lewej** _lub_ **początkowe**  &   **Końcowe** atrybuty, ale *nie* jednocześnie. Aby rozwiązać ten problem, Zmień wszystkie ograniczenia układu, aby użyć tego samego atrybutu ustawiona w pliku scenorysu.

Aby uzyskać więcej informacji, zobacz [błąd ograniczenia systemu iOS 9](http://stackoverflow.com/questions/32692841/ios-9-constraint-error) dyskusji przepełnienie stosu.

## <a name="error-itms-90535-unexpected-cfbundleexecutable-key"></a>Błąd ITMS-90535: Nieoczekiwany klucz CFBundleExecutable

Po przełączeniu do systemu iOS 9, z aplikacji używa 3 składników strony (w szczególności naszych istniejący składnik map programu Google), które kompilowane i został uruchomiony w systemie iOS 8 (lub starszym), podczas próby przesłać nową kompilację do iTunes Connect błędu można uzyskać w postaci:

> Błąd ITMS-90535: Nieoczekiwany klucz CFBundleExecutable. Pakiet na "Payload/app-name.app/component.bundle" nie zawiera pliku wykonywalnego pakietu...

To problemy można zwykle można rozwiązać przez wyszukiwanie nazwanego pakietu w projekcie, a następnie — zgodnie z sugestią, komunikat o błędzie - edytować `Info.plist` znajdujący się w pakiecie przez usunięcie `CFBundleExecutable` klucza. `CFBundlePackageType` Klucz powinien być ustawiony na `BNDL` również.

Po wprowadzeniu tych zmian należy wykonać czystą i skompiluj ponownie całego projektu. Można przesłać do iTunes Connect bez problemu po wprowadzeniu tych zmian.

Aby uzyskać więcej informacji, zobacz to [przepełnienie stosu](http://stackoverflow.com/questions/32096130/unexpected-cfbundleexecutable-key) dyskusji.

## <a name="cfnetwork-sslhandshake-failed--9824-error"></a>CFNetwork SSLHandshake błąd (-9824)

Podczas próby połączenia z Internetem bezpośrednio lub z widoku sieci web w systemie iOS 9, może być błąd pojawia się w postaci:

```csharp
2015-09-04 14:38:05.757 FormsWebViewiOS[2553:30362] CFNetwork SSLHandshake failed (-9824)
2015-09-04 14:38:05.758 FormsWebViewiOS[2553:30363] NSURLSession/NSURLConnection HTTP load failed (kCFStreamErrorDomainSSL, -9824)
```

Lub w postaci:

```csharp
2015-09-04 14:39:17.881 FormsWebViewiOS[2568:30974] App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.
Temporary exceptions can be configured via your app's Info.plist file.
```

W iOS9 zabezpieczeń transportu aplikacji (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji. Ponadto ATS wymaga komunikacji przy użyciu `HTTPS` protokołu i być szyfrowana przy użyciu protokołu TLS w wersji 1.2 z utajnienie wysokiego poziomu komunikacji interfejsu API.

Ponieważ ATS jest domyślnie włączona w aplikacji skompilowany dla systemu iOS 9 i OS X 10.11 (El Capitan), wszystkie połączenia przy użyciu `NSURLConnection`, `CFURL` lub `NSURLSession` podlegają ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.

Zobacz [Opting poza ATS](~/ios/app-fundamentals/ats.md) sekcji naszych [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md) przewodnika, aby uzyskać informacje na temat rozwiązywania tego problemu.

## <a name="my-existing-apps-dont-run-on-ios-9"></a>Moje istniejące aplikacje nie działają w systemie iOS 9

Zobacz nasze [informacje o zgodności z systemem iOS 9](~/ios/platform/introduction-to-ios9/ios9.md) instrukcje na ponowne utworzenie i ponowne wdrażanie istniejące aplikacje do uruchamiania w systemie iOS 9.

<a name="UICollectionViewCell.ContentView-is-null-in-constructors" />

## <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView ma wartość Null w konstruktorach

**Przyczyna:** w systemie iOS 9 `initWithFrame:` Konstruktor jest już wymagane, z powodu zmian zachowania w systemie iOS 9 jako [UICollectionView dokumentacji Stanów](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Zarejestrowano klasę dla określonego identyfikatora, należy utworzyć nowe komórki komórki teraz jest inicjowany przez wywołanie jego `initWithFrame:` metody.

**Poprawka:** Dodaj `initWithFrame:` konstruktora w następujący sposób:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Powiązane przykłady: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)

<a name="UIView-fails-to-Init-with-Coder-when-Loading-a-View-from-a-Xib/Nib" />

## <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView nie powiedzie się Init z koder podczas ładowania widoku z Xib/Nib

**Przyczyna:** `initWithCoder:` Konstruktor jest wywoływana podczas ładowania widoku z pliku Xib konstruktora interfejsu. Jeśli ten konstruktor nie jest eksportowane kodu niezarządzanego nie można wywołać naszych zarządzanej wersji. Wcześniej (np.) w systemie iOS 8) `IntPtr` konstruktor został wywołany zainicjować widoku.

**Poprawka:** tworzenie i eksportowanie `initWithCoder:` konstruktora w następujący sposób:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Przykładowe pokrewne: [rozmowy](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)

## <a name="dyld-message-no-cache-image-with-name"></a>Komunikat Dyld: Brak pamięci podręcznej obrazu o nazwie...

Mogą wystąpić awaria następujące informacje w dzienniku:

```csharp
Dyld Error Message:
Dyld Message: no cach image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Przyczyna:** jest to błąd w konsolidatorze natywnego firmy Apple, które odbywa się po wysłaniu prywatnej framework publiczny (JavaScriptCore dokonano publicznych w systemie iOS 7, przed był prywatnej framework), i jest cel wdrożenia aplikacji dla systemu iOS w wersji podczas została prywatnych. W takim przypadku konsolidatora firmy Apple połączy się z prywatnej wersji platformy zamiast wersji publicznej.

**Poprawka:** ten problem zostanie rozwiązany dla systemu iOS 9, ale jest łatwo obejście można zastosować się do tego czasu: tylko target nowszej wersji systemu iOS w projekcie (można spróbować systemów iOS 7 w takim przypadku). Innych platform, może powodować błędy podobne, na przykład WebKit framework został opublikowany w systemie iOS 8 (i dlatego przeznaczonych dla systemów iOS 7 spowoduje to błędu; docelowych z systemem iOS 8 używanie WebKit w aplikacji).

## <a name="untrusted-enterprise-developer"></a>Niezaufane Enterprise Developer

Podczas uruchamiania aplikacji platformy Xamarin.iOS wersję systemu iOS 9 na sprzęcie rzeczywistych iOS, może otrzymać komunikat informujący o tym, że nie został zaakceptowany konta dewelopera na urządzeniu. Na przykład:

[![](troubleshooting-images/untrusted01.png "Alert Enterprise Developer niezaufanych")](troubleshooting-images/untrusted01.png#lightbox)

Aby rozwiązać ten problem, wykonaj następujące czynności:

1. Uruchom środowisko Xcode (Najnowsza wersja beta) na programowanie Mac.
2. Wybierz **urządzeń** z **okna** menu, aby otworzyć okno urządzeń: 

    [![](troubleshooting-images/untrusted02.png "Oknie urządzenia")](troubleshooting-images/untrusted02.png#lightbox)
3. W obszarze **urządzeń** po stronie panelu, wybierz urządzenie, kliknij prawym przyciskiem myszy i wybierz **Pokaż profile inicjowania obsługi administracyjnej...** : 

    [![](troubleshooting-images/untrusted03.png "Profile inicjowania obsługi SShow")](troubleshooting-images/untrusted03.png#lightbox)
4. Każdy profil inicjowania obsługi administracyjnej obecnie na urządzenie i kliknij przycisk Wybierz **-** przycisk, aby usunąć go: 

    [![](troubleshooting-images/untrusted04.png "Usuwanie profilu inicjowania obsługi administracyjnej")](troubleshooting-images/untrusted04.png#lightbox)
5. Z **Xcode** menu, wybierz opcję **Preferencje...**  i **kont**: 

    [![](troubleshooting-images/untrusted05.png "Preferencje konta Xcode")](troubleshooting-images/untrusted05.png#lightbox)
6. Kliknij przycisk **wyświetlić szczegóły...**  przycisk, a następnie kliknij przycisk **pobranie wszystkich** przycisk: 

    [![](troubleshooting-images/untrusted06.png "Pobierz wszystkie profile")](troubleshooting-images/untrusted06.png#lightbox)
7. Po zakończeniu liście aktualizacji kliknij **gotowe** przycisk, a następnie zamknij okno Preferencje.
8. Usuń istniejącą wersję aplikacji platformy Xamarin.iOS, które próbowano przetestować z urządzenia z systemem iOS.
9. Wróć do programu Visual Studio dla komputerów Mac, wykonać czystą kompilację i spróbuj ponownie uruchomić aplikację na urządzeniu.

Należy zatrzymać i uruchomić ponownie program Visual Studio dla komputerów Mac przed nowych profilów aprowizacji załadowanych przez Xcode są widoczne. Może być również konieczne dostosowanie **iOS podpisywania pakietu** opcji Wybierz profile inicjowania obsługi administracyjnej nowej aplikacji platformy Xamarin.iOS.

## <a name="launch-screen-issues"></a>Uruchamianie problemów ekranu

System iOS 9 teraz wymusza wymagania dotyczące uruchamiania ekranu tak, aby ten sam obraz uruchamiania nie mogą być ponownie używane do obsługi orientacje innego interfejsu. Zobacz firmy Apple [odwołania UILanchImage](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW28) Aby uzyskać więcej informacji.

Opcjonalnie, możesz użyć pliku scenorysu do prezentowania ekranu uruchamiania aplikacji zamiast przy użyciu zestawu **.png** pliki obrazów. To jest teraz preferowana przez firmy Apple w sposób przedstawia uruchamianie ekranów. Zobacz nasze [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) przewodnika, aby uzyskać więcej informacji.

Na koniec aplikacji należy użyć pliku scenorysu dla jego uruchomienie ekranu i obsługują wszystkie orientacje cztery interfejsu (pionowy, dołu pionowej, pozioma po lewej i prawej pozioma) to zostać uwzględnione podczas pracy w trybie podzielony widok lub Przesuń za pośrednictwem panelu. Aby dowiedzieć się więcej na temat nowych możliwości wielozadaniowości systemu IOS 9, zobacz nasze [wielozadaniowości dla tabletu iPad](~/ios/platform/multitasking.md) przewodnik.

## <a name="nsinternalinconsistencyexception-exception"></a>Wyjątek NSInternalInconsistencyException

Podczas kompilowania i uruchamiania istniejącej aplikacji platformy Xamarin.iOS dla systemu iOS 9 w formularzu może być wyświetlany komunikat o błędzie:

> Zgłoszono wyjątek języka Objective-C.  Nazwa: NSInternalInconsistencyException Przyczyna: aplikacji systemu windows mogą mieć kontroler widoku głównej po zakończeniu uruchamiania aplikacji

Jest to błąd jest zgłaszanych ponieważ aplikacji systemu Windows mogą mieć kontroler widoku głównej po zakończeniu uruchamiania aplikacji, a nie istniejących aplikacji.

Istnieją co najmniej dwa możliwe obejścia tego problemu:

1. Aktualizacja aplikacji przy użyciu pliku scenorysu zamiast `xib` pliki, aby zdefiniować interfejs użytkownika. Ta wymaga bardzo dużo czasu, w zależności od rozmiaru aplikacji i wiedzę na temat sposobu używania projektanta (lub w środowisku Xcode interfejsu konstruktora) z systemem iOS do scenorysu układu. Aby uzyskać więcej informacji, zobacz nasze [wprowadzenie do ujednoliconego Scenorys](~/ios/user-interface/storyboards/unified-storyboards.md) dokumentacji.
2. Instalator `RootViewController` właściwości aplikacji okna w `FinishedLaunching` metoda `AppDelegate` klasy, aby wskazywał kontrolera widoku w Interfejsie użytkownika aplikacji.

## <a name="when-to-initialize-views-and-view-controllers"></a>Kiedy należy zainicjować widokach i kontrolerach widoku

Z Xamarin.iOS jest możliwość inicjowania widoku lub widok kontroler wewnątrz konstruktorów, które są wywoływane, gdy coś jest uwidaczniany kodu zarządzanego, ale przerywa projektu systemu iOS.

Ogólnie należy nie zainicjować wszystko, co może wywołania zwrotnego kod języka Objective-C z konstruktora, ponieważ nie można mieć pewność, gdy zostanie wywołana. Oznacza to również, istnieje lepsze miejsca (inne elementu .ctor) lub połączeń do zastąpienia (jak Objective-C nie ma żadnych zdarzeń) gdzie należy zrobić to inicjowania.

## <a name="related-links"></a>Linki pokrewne

- [System iOS 9 dla deweloperów](https://developer.apple.com/ios/pre-release/)
- [Nowości w systemie iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Aktualizowanie aplikacji platformy Xamarin.iOS do iOS9 (klip wideo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
