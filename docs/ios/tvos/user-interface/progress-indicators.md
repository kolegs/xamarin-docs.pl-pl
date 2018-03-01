---
title: "Praca z wskaźniki postępu"
description: "Ten artykuł obejmuje projektowanie i Praca z wskaźniki postępu wewnątrz aplikacji Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: b5d3a03324e73b06bd3defe7e6610163c3d1b26d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-progress-indicators"></a>Praca z wskaźniki postępu

_Ten artykuł obejmuje projektowanie i Praca z wskaźniki postępu wewnątrz aplikacji Xamarin.tvOS._


Mogą wystąpić podczas aplikacji Xamarin.tvOS musi załadować nowej zawartości lub wykonania operacji przetwarzania długie czasy. W tych godzinach powinni przedstawić wskaźnika działania albo pasek postępu użytkownikowi należy sprawdzić, czy aplikacja nadal jest uruchomiony i nadanie im niektóre wskazania długość zadanie uruchomione.

[ ![](progress-indicators-images/intro01.png "Przykładowe wskaźniki postępu")](progress-indicators-images/intro01.png)

<a name="About-Activity-Indicators" />

## <a name="about-activity-indicators"></a>Wskaźniki działania informacje

Wskaźnik aktywności wizualnie przedstawia jako Obracająca koło zębate i jest używana do reprezentowania zadania o nieustalonym długości. Wskaźnika jest widoczne, gdy zadanie rozpoczyna się i znika po zakończeniu zadania.

Apple ma poniższe sugestie do pracy z wskaźniki działania:

- **Jeśli to możliwe, użyj w zamian paski postępu** — ponieważ wskaźnika działania daje użytkownikowi nie opinii określające, jak długo trwa uruchamianie procesu będzie mieć, zawsze używaj pasek postępu, jeśli długość jest znana (np. liczbę bajtów do pobrania w pliku).
- **Zachowaj animowany wskaźnik** -użytkowników dotyczą nieruchomy wskaźnika działania zatrzymania aplikacji dzięki powinien zawsze mieć wskaźnik animowany podczas jego wyświetlania.
- **Opis zadania przetwarzania** — tylko wyświetlanie wskaźnika działania samodzielnie nie wystarcza, użytkownik musi mieć świadomość, proces oczekuje na informacje. To łatwy do rozpoznania etykiety (zazwyczaj zdania jednej, pełną), która wyraźnie definiuje zadania.

<a name="Summary" />

## <a name="about-progress-bars"></a>Paski postępu — informacje

Pasek postępu prezentuje jako wiersz, który wypełnia kolorem wskazują stan i długość czasochłonnym zadaniem. Paski postępu zawsze należy używać, gdy długość zadań jest wiadomo lub można obliczyć.

Apple ma poniższe sugestie dotyczące pracy z paski postępu:

- **Dokładnie raportowania postępu** -paski postępu powinna być zawsze dokładną reprezentację czas wymagany do ukończenia zadania. Nigdy nie błędnie reprezentują czas, aby zapewnić aplikacji zajęta.
- **Na użytek okresów Well-Defined** — pasek postępu nie powinien tylko wykazywać trwa długie zadanie Umieść, ale nadaj użytkownika i wskazuje, o ile zadanie zostało zakończone i oszacowanie pozostały czas.

<a name="Progress-Indicators-and-Storyboards" />

## <a name="progress-indicators-and-storyboards"></a>Wskaźniki postępu i planów

Najprostszym sposobem, aby pracować ze wskaźnikiem postępu w aplikacji Xamarin.tvOS jest dodanie ich do interfejsu użytkownika aplikacji przy użyciu projektanta dla systemu iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. W **konsoli rozwiązania**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **wskaźnika działania** z **przybornika** i upuść go w widoku: 

    [ ![](progress-indicators-images/activity01.png "Wskaźnik aktywności")](progress-indicators-images/activity01.png)
1. W **kartę Widget** z **konsoli właściwości**, można dostosować kilka właściwości wskaźnika działania, takie jak jego **styl** i **zachowanie**: 

    [ ![](progress-indicators-images/activity02.png "Na karcie widżetu ")](progress-indicators-images/activity02.png)
1. Przeciągnij **widoku postępu** z **przybornika** i upuść go w widoku: 

    [ ![](progress-indicators-images/activity03.png "Wyświetlanie postępu")](progress-indicators-images/activity03.png)
1. W **kartę Widget** z **Explorer właściwości**, można dostosować kilka właściwości widoku postępu, takie jak jego **styl** i **postępu**(wykonano): 

    [ ![](progress-indicators-images/activity04.png "Na karcie widżetu")](progress-indicators-images/activity04.png)
1. Na koniec przypisać **nazwy** do formantów, dzięki czemu można odpowiedzieć je w kodzie języka C#. Na przykład: 

    [ ![](progress-indicators-images/activity05.png "Przypisz nazwę")](progress-indicators-images/activity05.png)
1. Zapisz zmiany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` pliku i otwórz go do edycji.
1. Przeciągnij **wskaźnika działania** z **przybornika** i upuść go w widoku: 

    [ ![](progress-indicators-images/activity01-vs.png "Wskaźnik aktywności")](progress-indicators-images/activity01-vs.png)
1. W **kartę Widget** z **Explorer właściwości**, można dostosować kilka właściwości wskaźnika działania, takie jak jego **styl** i **zachowanie**: 

    [ ![](progress-indicators-images/activity02-vs.png "Na karcie widżetu")](progress-indicators-images/activity02-vs.png)
1. Przeciągnij **widoku postępu** z **przybornika** i upuść go w widoku: 

    [ ![](progress-indicators-images/activity03-vs.png "Wyświetlanie postępu")](progress-indicators-images/activity03-vs.png)
1. W **kartę Widget** z **Explorer właściwości**, można dostosować kilka właściwości widoku postępu, takie jak jego **styl** i **postępu**(wykonano): 

    [ ![](progress-indicators-images/activity04-vs.png "Na karcie widżetu")](progress-indicators-images/activity04-vs.png)
1. Na koniec przypisać **nazwy** do formantów, dzięki czemu można odpowiedzieć je w kodzie języka C#. Na przykład: 

    [ ![](progress-indicators-images/activity05-vs.png "Przypisz nazwę")](progress-indicators-images/activity05-vs.png)
1. Zapisz zmiany.

-----

Aby uzyskać więcej informacji na temat pracy z Scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Activity-Indicators" />

## <a name="working-with-activity-indicators"></a>Praca z działania wskaźników

Jak wspomniano powyżej, wskaźniki działania powinny być wyświetlane, gdy aplikacja działa długi proces, ale nie znasz dokładnego długość razem, gdy zadanie będzie wymagać.

W dowolnym momencie zostanie wyświetlony, jeśli wskaźnik aktywności jest uruchomiona Animacja Obracająca sprawdzając `IsAnimating` właściwości. Jeśli `HidesWhenStopped` właściwość jest `true`, wskaźnik aktywności automatycznie zostanie ukryta po zatrzymaniu animacji.

Poniższy kod służy do uruchamiania animacji: 

```csharp
ActivityIndicator.StartAnimating();
```

I poniżej spowoduje zatrzymanie animacji:

```csharp
ActivityIndicator.StopAnimating();
```

<a name="Working-with-Progress-Bars" />

## <a name="working-with-progress-bars"></a>Praca z paski postępu

Ponownie pasek postępu należy używać dowolnej chwili aplikacji jest wykonywania długotrwałych zadań o czasie trwania wiedzieć. 

`Progress` Właściwość jest używana do ustawiania ilości zadań, która została ukończona od 0 do 100% (od 0,0 do 1,0). Użyj `ProgressTintColor` właściwości, aby ustawić kolor na pasku wielkość ukończenia i `TrackTintColor` właściwości, aby ustawić kolor tła (kwota niezakończona).

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z wskaźniki postępu wewnątrz aplikacji Xamarin.tvOS.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
