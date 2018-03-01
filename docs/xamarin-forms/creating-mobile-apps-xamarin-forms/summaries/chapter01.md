---
title: "Podsumowanie rozdział 1. Jak platformy Xamarin.Forms pasują do?"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c0f3313fa3c4d1075be7deeb871e303006c533e8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Podsumowanie rozdział 1. Jak platformy Xamarin.Forms pasują do?

Jest jednym z najbardziej nieprzyjemny zadania w programowaniu eksportowanie kod podstawowy z jedną platformę do innego, zwłaszcza w wypadku tej platformy wymaga innego języka programowania. Istnieje możliwość przesłania, gdy eksportowanie kod, aby go również Refaktoryzuj, ale jeśli obu platform muszą zostać zachowane równolegle, następnie różnice między baz kodu dwóch będzie konserwacji w przyszłości trudniejsze.

## <a name="cross-platform-mobile-development"></a>Wiele platform przenośnych

Ten problem jest typowe w przypadku przeznaczonych dla platformy urządzeń przenośnych. Obecnie istnieje dwóch głównych platform urządzeń przenośnych, rodzina Apple iPhone i Ipad zainstalowano system operacyjny iOS oraz Android systemu operacyjnego, który działa na różne telefony i tablety. Innej platformie istotne jest firmy Microsoft Windows platformy Uniwersalnej, dzięki czemu jeden program pod kątem zarówno systemu Windows 10 i Windows 10 Mobile.

Dostawcy oprogramowania, która chce te trzy platform docelowych musi uwzględniać wzorcami inny interfejs użytkownika, trzy różne środowiska projektowania, trzech różnych interfejsów programowania i & #x 2014; prawdopodobnie większość nienaturalnej & #x 2014; trzy różnych językach programowania: Objective-C dla urządzenia iPhone i iPad, Java dla systemu Android i C# dla systemu Windows.

## <a name="the-c-and-net-solution"></a>Rozwiązanie C# i .NET

Mimo że Objective-C, Java i C# są uzyskiwane z język programowania C, ich usprawnionych przez bardzo różne ścieżki. C# jest najnowsza z tych języków i zostały dojrzewania bardzo użyteczne sposoby. Ponadto, C# jest ściśle powiązana z całej infrastruktury programowania o nazwie .NET, który zapewnia obsługę matematyczne, debugowanie, odbicia, kolekcje, globalizacji, we/wy pliku, sieci, zabezpieczeń, wątki, usług sieci web, obsługi danych i XML i JSON, Odczyt i zapis.

Xamarin obecnie udostępnia narzędzia pod kątem natywnej Mac, iOS i Android interfejsów API przy użyciu języka C# i .NET. Te narzędzia są nazywane Xamarin.Mac, Xamarin.iOS i Xamarin.Android, nazywanych zbiorczo platformy Xamarin. Są to bibliotek i powiązania, które express macierzystych interfejsów API tych platform z .NET idioms.

Deweloperzy mogą używać Xamarin platform do pisania aplikacji w języku C# tego docelowego Mac, iOS lub Android. Ale jeśli systemem docelowym jest więcej niż jednej platformie, ułatwia dużo warto udostępnić części kodu między na platformach docelowych. Obejmuje to rozdzielić program kodu zależny od platformy (zazwyczaj obejmujące interfejsu użytkownika) i kod niezależny od platformy, który zwykle wymaga tylko podstawowej platformy .NET framework. Ten kod niezależny od platformy albo może znajdować się w przenośnej biblioteki klas (PCL) lub w projekcie udostępnionym, często nazywane udostępnionego projektu zasobów lub SAP.

## <a name="introducing-xamarinforms"></a>Wprowadzenie do platformy Xamarin.Forms

Gdy przeznaczonych dla wielu platform urządzeń przenośnych, platformy Xamarin.Forms pozwala na udostępnianie jeszcze więcej kodu. Jeden program przeznaczony dla platformy Xamarin.Forms można kierować pięciu różnych platform:

- dla systemu iOS dla programów uruchamianych na telefonie iPhone, tabletów iPad i iPod touch
- Dla systemu android dla programów uruchamianych na telefony i tablety
- Platformy uniwersalnej systemu Windows do docelowego systemu Windows 10 i Windows 10 Mobile
- środowiska uruchomieniowego systemu Windows API Windows 8.1
- środowiska uruchomieniowego systemu Windows API Windows Phone 8.1

Bieżące szablony rozwiązań platformy Xamarin.Forms nie obejmują szablony projektów dla platform Windows 8.1 i Windows Phone 8.1.

Istnieje zbiorczego program platformy Xamarin.Forms PCL lub SAP. Każdej z platform składa się z stub małych aplikacji, który odwołuje się do PCL. Interfejsy API platformy Xamarin.Forms zamapować na kontrolki natywne na każdej platformie, jego charakterystyczny wygląd i działanie obsługuje każdej platformy:

[![Potrójna zrzut ekranu przedstawiający elementy wizualne platformy udostępnianie](images/ch01fg03-small.png "platformy Xamarin.Forms formantów na każdej platformie")](images/ch01fg03-large.png "platformy Xamarin.Forms formantów na każdej platformie")

Zrzuty ekranu od lewej do prawej Pokaż iPhone, telefonie z systemem Android i na telefon Windows 10 Mobile. Na każdym ekranie strona zawiera platformy Xamarin.Forms [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) do wyświetlania tekstu, [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) za inicjowanie akcje, [ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) dla Włącz/Wyłącz wartości i [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) służący do określania wartości zakresu ciągłe. Elementy podrzędne są wszystkie cztery tych widoków [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) na [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

Także dołączyć do strony jest pasek narzędzi platformy Xamarin.Forms składające się z kilku [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) obiektów. Te są widoczne jako ikony w górnej części z systemem iOS i Android osłony, a w dolnej części ekranu systemu Windows 10 Mobile.

Platformy Xamarin.Forms obsługuje również XAML, Extensible Application Markup Language opracowany przez firmę Microsoft dla różnych platform aplikacji. Wszystkie elementy wizualne programu pokazanym powyżej są zdefiniowane w XAML, jak pokazano w [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) próbki.

Program platformy Xamarin.Forms można określić platformy, które jest uruchomiona na i odpowiednio wykonanie innego kodu. Wydajniejsze deweloperzy mogą pisanie kodu niestandardowego dla różnych platform i uruchomić kod z programu platformy Xamarin.Forms w sposób niezależny od platformy. Deweloperzy mogą również tworzyć dodatkowe funkcje kontroli pisząc renderowania dla każdej platformy.

Gdy platformy Xamarin.Forms jest dobrym rozwiązaniem dla aplikacji biznesowych z lub dla prototypowania lub wprowadzeniem szybkie pokaz Weryfikacja koncepcji, jest mniej nadaje się doskonale dla aplikacji, które wymagają grafiki wektorowej lub złożonych touch interakcji.

## <a name="your-development-environment"></a>Środowiska deweloperskiego

Zależy od platformy, z którymi ma być docelowa, a co maszyny, które ma być środowiska deweloperskiego.

Jeśli chcesz docelowy z systemem iOS należy Mac z Xcode i Xamarin platforma jest zainstalowana. Obsługa systemu Android oraz wymaga instalowanie Java i wymaganych zestawów SDK. Następnie można wskazać zarówno dla systemu iOS i Android przy użyciu programu Visual Studio dla komputerów Mac.

Instalacja programu Visual Studio umożliwia na komputer docelowy z systemem iOS, Android i wszystkich platform Windows. Jednak przeznaczonych dla systemu iOS w programie Visual Studio nadal wymaga Mac z Xcode i Xamarin platforma jest zainstalowana.

Można przetestować programy albo rzeczywistego urządzenia połączone przez USB do komputera lub symulatora.

## <a name="installation"></a>Instalacja

Przed utworzeniem i tworzenia aplikacji platformy Xamarin.Forms, należy utworzyć i oddzielnie tworzenie aplikacji systemu iOS, aplikację systemu Android i aplikacją platformy uniwersalnej systemu Windows, w zależności od platformy, z którymi ma być docelowy i środowisko programowania.

Witryny sieci web firmy Microsoft i Xamarin zawierają informacje na temat sposobu wykonania tej czynności:

- [Rozpoczynanie pracy z systemem iOS](~/ios/get-started/index.md)
- [Wprowadzenie do korzystania z systemu Android](~/android/get-started/index.md)
- [Centrum deweloperów systemu Windows](http://dev.windows.com)

Po utworzeniu i uruchomić projektów dla tych poszczególnych platform, powinien mieć żadnych problemów, tworzenie i uruchamianie aplikacji platformy Xamarin.Forms.



## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 1 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Przykładowe rozdział 1](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
