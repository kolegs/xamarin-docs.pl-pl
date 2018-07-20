---
title: Podsumowanie rozdział 1. Jak zestawu narzędzi xamarin.Forms?
description: 'Tworzenie aplikacji mobilnych za pomocą zestawu narzędzi Xamarin.Forms: Podsumowanie rozdział 1. Jak zestawu narzędzi xamarin.Forms?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: abf30f2cd828d67ef6fb04f809fce6235e1add9b
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156486"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Podsumowanie rozdział 1. Jak zestawu narzędzi xamarin.Forms?

> [!NOTE] 
> Uwagi na tej stronie wskazać obszary, w którym Xamarin.Forms podzielił z materiału znajdujące się w książce.

Jest jedną z najbardziej nieprzyjemnych zadania w programowaniu przenoszenie kodu bazowego z jednej platformy do innego, szczególnie w przypadku, gdy tej platformy obejmuje innym języku programowania. Istnieje możliwość przesłania portowaniu kodu, jak również będzie je refraktoryzować, ale jeśli obie platformy utrzymuje się w sposób równoległy, następnie różnice między bazami kodu dwóch będzie konserwacji w przyszłości trudniejsze.

## <a name="cross-platform-mobile-development"></a>Programowanie aplikacji mobilnych dla wielu platform

Ten problem jest typowa, gdy przeznaczonych dla platform urządzeń przenośnych. Obecnie istnieją dwa główne platformy mobilne, Apple rodziny urządzeń iPhone i tabletach Ipad z systemem systemu operacyjnego iOS i Android system operacyjny, który działa na wielu różnych telefonach i tabletach. Innej platformie istotne jest firmy Microsoft Windows platformy Uniwersalnej, który umożliwia jeden program pod kątem systemu Windows 10 i Windows 10 Mobile.

Dostawcy oprogramowania, który chce tych trzech platformach musi obsłużyć paradygmatów innego interfejsu użytkownika, trzy różne środowiska projektowania, trzy różne interfejsy programowania i&mdash;może być najbardziej nienaturalnej&mdash; trzy różne języki programowania: Objective-C, dla telefonu iPhone i iPad, Java dla systemu Android i C# dla Windows.

## <a name="the-c-and-net-solution"></a>Rozwiązanie języka C# i platformy .NET

Mimo że języka Objective-C, Java i C# są uzyskiwane z języka programowania C, ich usprawnionych przy bardzo różnych ścieżek. C# jest najnowsza z tych języków i ma zostały dojrzałych w sposób bardzo przydatne. Ponadto, C# jest ściśle powiązana z całej infrastruktury programowania, o nazwie .NET, który zapewnia obsługę matematyczne, debugowanie, odbicia, kolekcje, globalizacji, we/wy pliku, sieci, zabezpieczeń, wątki, usług sieci web, obsługi danych i XML i JSON, Odczyt i zapis.

Środowisko Xamarin zapewnia obecnie narzędzia pod kątem natywnej Mac, iOS i interfejsy API systemu Android przy użyciu języka C# i .NET. Te narzędzia są nazywane platformy Xamarin.Mac, Xamarin.iOS i Xamarin.Android, nazywane zbiorczo platformy Xamarin. Są to biblioteki i powiązań, które express natywnych interfejsów API dla tych platform przy użyciu idiomy .NET.

Deweloperzy mogą używać platformy Xamarin, do pisania aplikacji w języku C#, że docelowy Mac, iOS lub Android. Jednak gdy więcej niż jedną platformę, to sprawia, że jest bardzo rozsądne, aby udostępnić część kodu między platformami docelowego. Obejmuje to oddzielenie program zależny od platformy kod (zazwyczaj obejmujące interfejsu użytkownika) i kod niezależny od platformy, co zwykle wymaga tylko podstawowych platformy .NET framework. Ten kod niezależny od platformy albo mogą znajdować się w bibliotece klas przenośnych (PCL) lub projektu udostępnionego, często nazywane projektu udostępnionego zasobu lub SAP.

> [!NOTE] 
> Biblioteki klas przenośnych zostały zastąpione przez biblioteki .NET Standard. Cały kod przykładowy z książki został przekonwertowany przy użyciu standardowych bibliotek platformy .NET.

## <a name="introducing-xamarinforms"></a>Wprowadzenie do zestawu narzędzi Xamarin.Forms

Przeznaczone dla wielu platform mobilnych, zestawu narzędzi Xamarin.Forms umożliwia jeszcze bardziej udostępniania kodu. Jeden program przeznaczony dla platformy Xamarin.Forms można kierować pięć różnych platform:

- dla systemu iOS dla programów uruchamianych na urządzeniu iPhone, tabletów iPad i iPod touch
- System android dla programów, które działają na telefonach z systemem Android i tabletach
- Platforma uniwersalna Windows, aby obiekt docelowy systemu Windows 10 i Windows 10 Mobile
- Windows interfejsu API środowiska wykonawczego systemu Windows 8.1
- Windows interfejsu API środowiska wykonawczego systemu Windows Phone 8.1

> [!NOTE] 
> Zestaw narzędzi Xamarin.Forms już nie obsługuje Windows 8.1, Windows Phone 8.1 lub Windows 10 Mobile, ale aplikacje Xamarin.Forms są uruchamiane na pulpicie systemu Windows 10. Istnieje również wersję zapoznawczą obsługi [Mac](~/xamarin-forms/platform/mac.md), [WPF](~/xamarin-forms/platform/wpf.md), [GTK #](~/xamarin-forms/platform/gtk.md), i [Tizen](/xamarin-forms/platform/tizen.md) platform.

Duża część programu Xamarin.Forms istnieje w bibliotece lub SAP. Każdej z platform składa się z klasy zastępczej mała aplikacja, która wywołuje ten kod udostępniony. 

Mapowania interfejsów API zestawu narzędzi Xamarin.Forms natywne kontrolki na każdej platformie, aby w każdej z platform zachowuje jego charakterystyczny wygląd i działanie:

[![Potrójna zrzut ekranu przedstawiający wizualizacje platformy udostępnianie](images/ch01fg03-small.png "kontrolek zestawu narzędzi Xamarin.Forms na każdej platformie")](images/ch01fg03-large.png#lightbox "kontrolek zestawu narzędzi Xamarin.Forms na każdej platformie.")

Zrzuty ekranu, od lewej do prawej Pokaż telefonu iPhone, telefonów z systemem Android i na telefon Windows 10 Mobile. 

> [!NOTE] 
> Zestaw narzędzi Xamarin.Forms nie obsługuje już Windows 10 Mobile.

Na każdym ekranie strona zawiera rozwiązanie Xamarin.Forms [ `Label` ](xref:Xamarin.Forms.Label) do wyświetlania tekstu, [ `Button` ](xref:Xamarin.Forms.Button) za zainicjowanie działań, [ `Switch` ](xref:Xamarin.Forms.Switch) dla Wybieranie wł. / wył. wartości i [ `Slider` ](xref:Xamarin.Forms.Slider) dla podanie wartości w ramach ciągłego zakresu. Wszystkie cztery te widoki są elementami podrzędnymi [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) na [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

Również dołączone do strony jest narzędzi Xamarin.Forms, składający się z kilku [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) obiektów. Są one widoczne jako ikony w górnej części ekranu dla systemu Android i iOS, a w dolnej części ekranu Windows 10 Mobile.

Zestaw narzędzi Xamarin.Forms obsługuje również XAML, Extensible Application Markup Language opracowywane w dziale Microsoft dla różnych platform aplikacji. Wszystkie wizualizacje programu powyżej są zdefiniowane w XAML, jak pokazano w [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) próbki.

Program platformy Xamarin.Forms można określić platformy jest uruchomiona na i wykonać różny kod odpowiednio. Wydajniejsze deweloperzy mogą pisania niestandardowego kodu dla różnych platform i uruchamianie kodu z programu Xamarin.Forms w sposób niezależny od platformy. Deweloperzy, można również utworzyć dodatkowe formanty, pisząc programy renderujące dla każdej platformy.

Zestaw narzędzi Xamarin.Forms jest doskonałym rozwiązaniem dla aplikacji line-of-business, lub do tworzenia prototypów lub dokonywania szybkiego pokaz weryfikacji koncepcji, jest mniej idealne dla aplikacji, które wymagają grafiki wektorowej lub interakcji dotykowych złożone.

## <a name="your-development-environment"></a>Środowiska deweloperskiego

Środowiska deweloperskiego zależy od platformy, która ma pod kątem i co można maszyn ma być używany.

Jeśli chcesz docelowy z systemem iOS należy Mac za pomocą środowiska Xcode i platforma Xamarin jest zainstalowana. Obsługa systemu Android oraz wymaga instalacji języka Java i wymaganych zestawów SDK. Można skierować systemów iOS i Android przy użyciu programu Visual Studio dla komputerów Mac.

Instalowanie programu Visual Studio pozwala na komputerze docelowym z systemem iOS, Android i wszystkich platform Windows. Przeznaczonych dla systemu iOS w programie Visual Studio nadal wymaga jednak Mac za pomocą środowiska Xcode i platforma Xamarin jest zainstalowana.

Możesz przetestować programy albo rzeczywistego urządzenia połączone przez USB do komputera lub symulatora.

## <a name="installation"></a>Instalacja

Przed utworzeniem i tworzenie aplikacji platformy Xamarin.Forms, należy utworzyć i skompilować oddzielnie aplikacji systemu iOS, aplikacji systemu Android oraz aplikacji platformy uniwersalnej systemu Windows, w zależności od platformy, którą chcesz obiektu docelowego i środowiska deweloperskiego.

Witryny sieci web platformy Xamarin i Microsoft zawierają informacje na temat sposobu, w tym:

- [Wprowadzenie do systemu iOS](~/ios/get-started/index.md)
- [Wprowadzenie do systemu Android](~/android/get-started/index.md)
- [Centrum deweloperów Windows](http://dev.windows.com)

Po utworzeniu i uruchamiania projektów dla tych poszczególnych platform, powinien mieć żadnych problemów, tworzenie i uruchamianie aplikacji platformy Xamarin.Forms.

## <a name="related-links"></a>Linki pokrewne

- [Pełny tekst rozdział 1 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Przykładowe rozdział 1](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
