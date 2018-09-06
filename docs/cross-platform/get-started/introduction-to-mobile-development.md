---
title: Wprowadzenie do programowania aplikacji mobilnych
description: Ten dokument zawiera wprowadzenie do opracowywania aplikacji mobilnych, omawiając Xamarin, jak działa i aplikacji, które wyświetla.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: f3b1f5c11a02710de8d0ffd09741acb3017f5cb6
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780604"
---
# <a name="introduction-to-mobile-development"></a>Wprowadzenie do programowania aplikacji mobilnych

Może być bardzo proste — wystarczy otwarcia IDE, zgłaszanie coś ze sobą, wykonując bitu szybkie testowanie i przesyłanie do App Store — wszystko gotowe po południu, tworzenia aplikacji mobilnych. Lub może być bardzo zaangażowane procesu, który obejmuje rygorystyczne ponoszonych z góry projektu, użyteczności, testowanie, QA testowania tysiące urządzeń, beta pełny cykl życia, a następnie wdrażania wiele różnych sposobów.

Ten dokument jest przeznaczony do wprowadzenia platformy Xamarin. Aby dowiedzieć się więcej na temat *procesu* w procesie tworzenia aplikacji mobilnych z projektu za pomocą testowania, można znaleźć [wprowadzenie do cyklu życia tworzenia oprogramowania Mobile](~/cross-platform/get-started/introduction-to-mobile-sdlc.md) dokumentu.

Można znaleźć na naszej [wymagania systemowe](~/cross-platform/get-started/requirements.md#macos-requirements) aby upewnić się, masz możliwość instalacji Xamarin.

## <a name="introduction-to-xamarin"></a>Wprowadzenie do platformy Xamarin

Podczas wybierania sposobu umożliwia tworzenie aplikacji dla systemu Android i iOS, wiele osób postrzega natywnych językach, Objective-C, Swift i Java, czy jedyną wybraną. Jednak w ciągu ostatnich kilku lat, pojawiło istnieje cały ekosystem nowej platformy do tworzenia aplikacji mobilnych.

Xamarin jest unikatowa w tej dziedzinie, zapewniając jednego języka — C#, biblioteki klas i środowiska uruchomieniowego, który działa we wszystkich trzech platformach urządzeń przenośnych z systemem iOS, Android i Windows Phone (Windows Phone w macierzystym języku jest już C#), podczas kompilowania nadal (natywne bez interpretowane) aplikacjom są wystarczająco wydajna nawet w przypadku najbardziej wymagających gier.

Każda z tych platform ma inny zestaw funkcji, a każdy zmienia się w jego możliwość pisania aplikacji natywnych — czyli aplikacje kompilowane do kodu macierzystego i tego współdziałania bezproblemowe w podsystemie podstawowych języka Java. Na przykład niektóre platformy tylko aplikacjom ma zostać utworzony w kodzie HTML i JavaScript, a niektóre są bardzo niskiego poziomu oraz Zezwalanie tylko na kodu C/C++. Niektóre platformy nawet nie korzystaj z narzędzi natywne kontrolki.

Xamarin jest unikatowa, ponieważ łączy wszystkie możliwości natywnych platform i dodaje zaawansowane funkcje usług własną, w tym:

1.   **Wykonaj powiązania dla podstawowych zestawów SDK** — Xamarin zawiera powiązania dla prawie całej podstawowych zestawów SDK dla platformy w systemów iOS i Android. Te powiązania jest występowanie silnie typizowane, co oznacza, że są one łatwość Przejdź i użycia i zapewnić niezawodne typów w czasie kompilacji sprawdzania i podczas programowania. Prowadzi to do mniejszej liczby błędów środowiska uruchomieniowego i aplikacji o wysokiej jakości.
1.   **Języka Objective-C, Java, C i międzyoperacyjności języka C++** — środowisko Xamarin zapewnia funkcje służące do bezpośrednie wywołanie języka Objective-C, Java, C i bibliotek języka C++, dając do użycia z szerokiej gamy innych firm 3 kod, który został już utworzony. Dzięki temu można korzystać z istniejących systemów iOS i Android bibliotek napisanych w językach Objective-C, Java lub C/C++. Dodatkowo Xamarin udostępnia powiązanych projektów, które umożliwiają łatwe powiązanie natywnej biblioteki języka Objective-C i Java za pomocą składni deklaratywnej.
1.   **Nowoczesne konstrukcji językowych** — aplikacji platformy Xamarin jest napisana w języku C#, Nowoczesny język, który obejmuje znaczne ulepszenia w języku Objective-C i Java, takie jak *dynamiczne funkcje językowe* ,  *Konstrukcje funkcjonalności* takich jak *Lambdy* , *LINQ* , *programowania równoległego* funkcje zaawansowane *typów ogólnych*  i nie tylko.
1.   **Wspaniałe Biblioteka klasy podstawowej (BCL)** — aplikacje platformy Xamarin korzystają BCL platformy .NET, ogromną kolekcję klas, które mają kompleksowe i zoptymalizowany takich funkcji zaawansowanej obsłudze XML, bazy danych, serializacji, operacji We/Wy, parametry i sieci, po prostu do Nazwa kilka. Ponadto istniejącego kodu języka C# może być kompilowane do użycia w aplikacji, która zapewnia dostęp do tysięcy od tysięcy bibliotek, które umożliwi wykonywanie czynności, które nie są już objęte BCL.
1.   **Nowoczesne programowanie środowiska IDE (Integrated)** — platforma Xamarin korzysta z programu Visual Studio dla komputerów Mac w systemie Mac OS X i programu Visual Studio w Windows. Są to zarówno nowoczesnych środowisk IDE, obejmujące funkcje, takie jak kod automatyczne uzupełnianie, zaawansowane system zarządzania projektu i rozwiązania, Biblioteka szablonów projektu kompleksowe, zintegrowana kontrola źródła i wiele innych.
1.   **Obsługa programu Mobile dla wielu Platform** — Xamarin oferuje zaawansowane obsługi wielu platform dla trzech głównych platform urządzeń przenośnych z systemem iOS, Android i Windows Phone. Aplikacje mogą być zapisywane na udostępnianie do 90% kodu i naszej bibliotece Xamarin.Mobile oferuje ujednoliconego interfejsu API, dostęp do wspólnych zasobów we wszystkich trzech platformach. Może to znacznie zmniejszyć koszty rozwoju i czas wprowadzenia na rynek dla deweloperów aplikacji mobilnych tej docelowej trzy najbardziej popularnych platform urządzeń przenośnych.


Z powodu zestawu funkcji Zaawansowane i wszechstronne platformy Xamarin wypełnia void dla deweloperów aplikacji, które mają być używane w nowoczesnym języku i platforma do tworzenia aplikacji mobilnych dla wielu platform.


> [!NOTE]
> W tej serii wprowadzenie skupia się na praktycznym rozpoczęcie tworzenia dla systemów iOS i aplikacji dla systemu Android. Firma Microsoft oferuje informacje o [rozwoju Windows platformy Uniwersalnej](https://docs.microsoft.com/windows/uwp/develop/) dla tabletów i komputerów stacjonarnych. Aby dowiedzieć się więcej o programowaniu dla wielu platform za pomocą platformy Xamarin (w tym aplikacje platformy uniwersalnej systemu Windows dla Windows), przeczytaj [przewodnik dotyczący tworzenia aplikacji dla wielu Platform](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).



## <a name="how-does-xamarin-work"></a>Jak działa program Xamarin?

Xamarin oferuje dwa produkty komercyjnych: Xamarin.iOS i Xamarin.Android. Są one zarówno przystosowane w górnej części *Mono*, typu open-source wersja programu .NET Framework oparta na opublikowanych normach .NET ECMA. Narzędzie mono zostało wokół prawie długie jako .NET framework sam i uruchamiany w prawie w każdym imaginable platformy, łącznie z systemem Linux, Unix, FreeBSD i Mac OS X.

W systemach iOS, Xamarin firmy *Ahead of Time* ( *AOT*) kompilatora kompiluje aplikacji platformy Xamarin.iOS bezpośrednio do kodu natywnego zestawu ARM. W systemie Android, Xamarin kompilatora kompiluje w dół do *języka pośredniego* ( *IL*), który jest następnie *Just-in-Time* ( *JIT*) skompilowane do natywny zestaw po uruchomieniu aplikacji.

W obu przypadkach aplikacje platformy Xamarin wykorzystywać środowisko uruchomieniowe, które automatycznie obsługuje elementy, takie jak Alokacja pamięci, wyrzucanie elementów bezużytecznych, podstawowej platformy interop itp.



### <a name="xamariniosdll-and-monoandroiddll"></a>Pliku Xamarin.iOS.dll i Mono.Android.dll

Aplikacje platformy Xamarin są tworzone dla podzbioru BCL platformy .NET, znane jako profil Xamarin Mobile. Ten profil został utworzony specjalnie dla aplikacji mobilnych i spakowane w pliku MonoTouch.dll i Mono.Android.dll (dla systemów iOS i Android odpowiednio). To jest wiele takich jak aplikacje programu Silverlight (i wersję) sposób są tworzone dla programu Silverlight/wersję profilu platformy .NET. W rzeczywistości profilu Xamarin dla urządzeń przenośnych odpowiada profil Silverlight 4.0 z wielu klas BCL dodane ponownie w.

Aby uzyskać pełną listę dostępnych zestawów i klas, zobacz [listy zestawów o Xamarin.iOS](~/cross-platform/internals/available-assemblies.md) i [listy zestawów platformy Xamarin.Android](~/cross-platform/internals/available-assemblies.md)

Oprócz BCL tych bibliotek DLL obejmują otoki dla prawie całej systemu iOS SDK i SDK systemu Android, umożliwiająca podstawowe interfejsy API zestawu SDK do wywołania bezpośrednio w języku C#.



### <a name="application-output"></a>Dane wyjściowe aplikacji

Po skompilowaniu aplikacji środowiska Xamarin, wynik jest pakiet aplikacji, pliku .app w systemie iOS lub plik apk w systemie Android. Te pliki są nie do odróżnienia od pakietów aplikacji utworzonych za pomocą platformy domyślnego środowiska IDE i możliwych do wdrożenia w dokładnie taki sam sposób.



## <a name="getting-started"></a>Wprowadzenie

Teraz znasz już trochę o tym, jak działa program Xamarin nadszedł czas, aby zacząć korzystać!

Następnym krokiem jest, aby rozpocząć tworzenie aplikacji przy użyciu jednej z tych przewodników:

* [**Witaj, iOS**](~/ios/get-started/hello-ios/index.md)

![](introduction-to-mobile-development-images/ios.png "Witaj, iOS")


* [**Hello, Android**](~/android/get-started/hello-android/index.md)

![](introduction-to-mobile-development-images/android.png "Hello, Android")


* [**Wprowadzenie do zestawu narzędzi Xamarin.Forms**](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)





## <a name="summary"></a>Podsumowanie

W tym dokumencie jedynie wprowadził platformy Xamarin. Zabawa rzeczywistych rozpoczyna się po otrzymaniu pierwszej aplikacji w górę i uruchomiona. Zapoznaj się z [Witaj, iOS](~/ios/get-started/hello-ios/index.md), [Witaj, Android](~/android/get-started/hello-android/index.md), i [wprowadzenie do zestawu narzędzi Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) przewodników, aby rozpocząć.


## <a name="related-links"></a>Linki pokrewne

- [Witaj, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](~/android/get-started/hello-android/index.md)
