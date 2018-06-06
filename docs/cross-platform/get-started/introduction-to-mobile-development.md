---
title: Wprowadzenie do rozwoju przenośnych
description: Ten dokument zawiera wprowadzenie do rozwoju przenośnych, dyskutować Xamarin, jak to działa i aplikacje, które generuje on.
ms.prod: xamarin
ms.assetid: 33C83E13-F3E5-17B4-6512-207F3D3C5AB6
author: asb3993
ms.author: amburns
ms.date: 03/28/2017
ms.openlocfilehash: c6f0233d736c51142d6d83996361558709fd2070
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781923"
---
# <a name="introduction-to-mobile-development"></a>Wprowadzenie do rozwoju przenośnych

Tworzenie aplikacji dla urządzeń przenośnych można w prosty jako otwarcia IDE, wyrzucanie coś ze sobą, podczas szybkiego bit testowania i przesyłanie do sklepu z aplikacjami — wszystkie wykonane w popołudnie. Lub może być bardzo zaangażowany procesu, który obejmuje rygorystyczne projektowania początkowych użyteczność, testowania i testują rozwiązania na tysiące urządzeń, beta pełny cykl życia, a następnie wdrożenia na kilka różnych sposobów pytań i odpowiedzi.

Ten dokument jest przeznaczony do wprowadzenia do platformy Xamarin. Aby dowiedzieć się więcej o *procesu* tworzenia aplikacji dla urządzeń przenośnych z projektu za pomocą do testowania, zobacz [wprowadzenie do cyklu programistycznym oprogramowania Mobile](~/cross-platform/get-started/introduction-to-mobile-sdlc.md) dokumentu.

Zapoznaj się z naszym [wymagania systemowe](~/cross-platform/get-started/requirements.md#mac) do potwierdza, że jest możliwe do zainstalowania programu Xamarin.

## <a name="introduction-to-xamarin"></a>Wprowadzenie do platformy Xamarin

Podczas określania, jak tworzyć aplikacje dla systemu Android systemów iOS i, wiele osób postrzega natywnego języków, Objective-C, Swift i języka Java, czy tylko opcja. Jednak w ciągu ostatnich kilku lat, pojawiło cały ekosystem nowych platform do tworzenia aplikacji dla urządzeń przenośnych.

Xamarin jest unikatowa w tym miejscu, oferując jeden język — C#, biblioteki klas i środowisko uruchomieniowe, które działa we wszystkich trzech platform urządzeń przenośnych z systemami iOS, Android i Windows Phone (Windows Phone językiem macierzystym jest już C#), podczas Trwa kompilowanie natywnego () aplikacje z systemem innym niż — interpretowane), które są wystarczająco wydajność nawet w przypadku wymagających gier.

Każdy z tych platform ma inny zestaw funkcji i każdego różni się w jego możliwości zapisu natywnych aplikacji — to znaczy, aplikacje, które skompiluj do kodu macierzystego i że interop fluently w podsystemie źródłowy Java. Na przykład niektóre platformy Zezwalaj tylko aplikacje mają zostać utworzone w kodzie HTML i JavaScript, niektóre są bardzo niskiego poziomu i Zezwalaj tylko na kodu C/C++. Zestaw narzędzi do macierzystego formantu nawet nie korzystać z niektórych platformach.

Xamarin jest unikatowa, ponieważ łączy wszystkie możliwości platformy natywnego i dodaje wiele zaawansowanych funkcji własnej, w tym:

1.   **Zakończenie powiązania dla podstawowych zestawów SDK** — Xamarin zawiera powiązań dla prawie cały podstawowej platformy zestawów SDK zarówno dla systemu iOS i Android. Te powiązania jest występowanie jednoznacznie, co oznacza, że są one łatwe do nawigowania i użyj i zapewniają niezawodne typu kompilacji sprawdzanie i podczas tworzenia. Prowadzi to do mniej błędy podczas wykonywania i wyższej jakości aplikacji.
1.   **Objective-C, Java, C i międzyoperacyjności języka C++** — Xamarin zapewnia urządzenia do bezpośredniego wywoływania Objective-C, Java, C i biblioteki języka C++, co pozwala używać szerokiej gamy 3 strona kod, który został już utworzony. Dzięki temu można korzystać z istniejącego systemu iOS i Android bibliotek napisany w języku Objective-C, Java lub C/C++. Ponadto Xamarin oferuje powiązania projektów, które umożliwiają łatwe powiązać natywnych bibliotek języka Objective C i Java za pomocą składni deklaratywnej.
1.   **Nowoczesny konstrukcji językowych** — aplikacje platformy Xamarin są zapisywane w języku C#, Nowoczesny języku, który obejmuje następujące istotne ulepszenia w języku Objective C i Java takich jak *dynamiczne funkcje językowe* ,  *Konstrukcje funkcjonalności* takich jak *Lambdas* , *LINQ* , *Programowanie równoległe* funkcje, zaawansowane *ogólne*  itd.
1.   **Niesamowite biblioteki klasy podstawowej (BCL)** — aplikacje platformy Xamarin używać .NET BCL, zbiór ogromną klasami, które zawierają kompleksowe i zoptymalizowany funkcje takie jak obsługa zaawansowanych XML, bazy danych serializacji, we/wy, ciągu i sieci, tylko do Nazwa kilka. Ponadto istniejącego kodu C# mogą być kompilowane do użycia w aplikacji, która zapewnia dostęp do tysięcy po tysiące bibliotek, które umożliwi wykonywanie czynności, które nie są już objęte BCL.
1.   **Nowoczesne projektowanie środowiska IDE (Integrated)** — Xamarin korzysta z programu Visual Studio for Mac w systemie Mac OS X i Visual Studio w systemie Windows. Są to zarówno IDEs nowoczesnych, które obejmują funkcje, takie jak kod automatycznego uzupełniania, zaawansowane system zarządzania projektem i rozwiązaniem Biblioteka szablonów projektu kompleksowe, zintegrowanej kontroli źródła i wielu innych.
1.   **Obsługa Platform między Mobile** — Xamarin oferuje zaawansowane Obsługa platform dla trzech głównych platform urządzeń przenośnych z systemem iOS, Android i Windows Phone. Aplikacje mogą być zapisywane na udział do 90% ich kodu i nasza Biblioteka Xamarin.Mobile oferuje ujednolicony interfejs API dostęp do wspólnych zasobów na wszystkich platformach trzy. To może znacznie zmniejszyć zarówno koszty rozwoju i w czasie na rynek dla deweloperów przenośnych tego docelowego trzech najbardziej popularnych platform urządzeń przenośnych.


Z powodu zestaw funkcji Zaawansowane i wszechstronne na platformie Xamarin umieszcza void dla deweloperów aplikacji, które mają być używane do opracowywania aplikacji dla urządzeń przenośnych i platform nowoczesny język i platformy.


> [!NOTE]
> Ta seria wprowadzenie koncentruje się na pobieranie rozpoczęto tworzenie systemów iOS i Android aplikacje. Firma Microsoft udostępnia informacje o [programowanie uniwersalnych platformy systemu Windows (UWP)](https://docs.microsoft.com/windows/uwp/develop/) tablety i komputery stacjonarne. Aby dowiedzieć się więcej na temat aplikacji dla wielu platform za pomocą platformy Xamarin (w tym aplikacji platformy uniwersalnej systemu Windows dla systemu Windows), przeczytaj [przewodnik tworzenie wieloplatformowych aplikacji](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).



## <a name="how-does-xamarin-work"></a>Jak działa program Xamarin?

Xamarin oferuje dwa produkty komercyjnych: Xamarin.iOS i platformy Xamarin.Android. Są one zarówno przystosowane nad *Mono*, .NET Framework w wersji open source opartych na opublikowanych standardach .NET ECMA. Mono została wokół prawie długie jako platforma .NET się i działa na niemal każdej platformy imaginable, łącznie z systemem Linux, Unix FreeBSD i Mac OS X.

W systemach iOS, Xamarin w *Ahead z czasu* ( *drzewa obiektów aplikacji*) kompilatora kompiluje aplikacji platformy Xamarin.iOS bezpośrednio do kodu macierzystego zestawów ARM. W systemie Android na platformie Xamarin kompilatora kompiluje do *język pośredni* ( *IL*), która jest następnie *Just in Time* ( *JIT*) do natywny zestaw po uruchomieniu aplikacji.

W obu przypadkach aplikacji platformy Xamarin wykorzystywać środowisko uruchomieniowe, które automatycznie obsługuje czynności, takie jak alokacji pamięci, wyrzucanie elementów bezużytecznych, podstawowej platformy interop itp.



### <a name="xamariniosdll-and-monoandroiddll"></a>Xamarin.iOS.dll i Mono.Android.dll

Aplikacje platformy Xamarin są tworzone względem podzbiór BCL .NET, znane jako profil mobilnymi Xamarin. Ten profil został utworzony specjalnie dla aplikacji dla urządzeń przenośnych i opakowane w MonoTouch.dll i Mono.Android.dll (dla systemów iOS i Android odpowiednio). Pozwala to znacznie jak sposób aplikacji Silverlight (i wersję) są tworzone na podstawie profilu .NET Silverlight/wersję. W rzeczywistości profilu mobilnymi Xamarin odpowiada profil Silverlight 4.0 wyświetlonych klasy BCL dodane ponownie.

Aby uzyskać pełną listę dostępnych zestawów i klas, zobacz [listy zestawów platformy Xamarin.iOS](~/cross-platform/internals/available-assemblies.md) i [listy zestawów platformy Xamarin.Android](~/cross-platform/internals/available-assemblies.md)

Oprócz BCL biblioteki te obejmują otoki dla niemal całego systemu iOS SDK i Android umożliwiający podstawowych interfejsów API zestawu SDK do wywołania bezpośrednio w języku C#.



### <a name="application-output"></a>Dane wyjściowe aplikacji

Gdy aplikacje platformy Xamarin są kompilowane, wynikiem jest pakietu aplikacji w pliku .app w systemie iOS lub plik apk w systemie Android. Te pliki są nierozróżnialne z pakietów aplikacji skompilowanej za pomocą domyślnego platformy IDEs i możliwych do wdrożenia w dokładnie tak samo.



## <a name="getting-started"></a>Wprowadzenie

Teraz znasz nieco działania Xamarin nadszedł czas, aby przejść!

Następnym krokiem jest, aby rozpocząć tworzenie aplikacji przy użyciu jednej z tych wskazówek:

* [**Witaj, z systemem iOS**](~/ios/get-started/hello-ios/index.md)

![](introduction-to-mobile-development-images/ios.png "Witaj, z systemem iOS")


* [**Hello, Android**](~/android/get-started/hello-android/index.md)

![](introduction-to-mobile-development-images/android.png "Hello, Android")


* [**Wprowadzenie do platformy Xamarin.Forms**](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)





## <a name="summary"></a>Podsumowanie

Ten dokument ma jedynie wprowadzono platformy Xamarin. Rzeczywiste fun rozpoczyna się po otrzymaniu pierwszej aplikacji w górę i uruchomiona. Zapoznaj się z [Hello, iOS](~/ios/get-started/hello-ios/index.md), [Hello, Android](~/android/get-started/hello-android/index.md), i [wprowadzenie do platformy Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) przewodniki, aby rozpocząć.


## <a name="related-links"></a>Linki pokrewne

- [Witaj, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, Android](~/android/get-started/hello-android/index.md)
