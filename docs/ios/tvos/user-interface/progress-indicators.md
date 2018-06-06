---
title: Praca z systemu tvOS wskaźniki postępu w Xamarin
description: Ten dokument zawiera opis sposobu pracy z wskaźniki postępu w systemu tvOS aplikacji skompilowanej za pomocą platformy Xamarin. Zawarto informacje zarówno paski postępu i wskaźniki działania.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/25/2018
ms.openlocfilehash: f8812f6b3f8a461487dcaf548637c84b16631d6b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789219"
---
# <a name="working-with-tvos-progress-indicators-in-xamarin"></a>Praca z systemu tvOS wskaźniki postępu w Xamarin

_Ten artykuł obejmuje projektowanie i Praca z wskaźniki postępu wewnątrz aplikacji Xamarin.tvOS._

Mogą wystąpić podczas aplikacji Xamarin.tvOS musi załadować nowej zawartości lub wykonania operacji przetwarzania długie czasy. W tych godzinach powinni przedstawić wskaźnika działania lub pasek postępu użytkownikowi należy sprawdzić, czy aplikacja nadal jest uruchomiony i nadanie im niektóre wskazania długość zadanie uruchomione.

![Przykładowe wskaźniki postępu](progress-indicators-images/intro01.png "Przykładowe wskaźniki postępu")

## <a name="about-activity-indicators"></a>Wskaźniki działania informacje

Wskaźnik aktywności przedstawia jako Obracająca koło zębate i jest używana do reprezentowania zadania o nieustalonym długości. Wskaźnika jest widoczne, gdy zadanie rozpoczyna się i znika po zakończeniu zadania.

Apple ma poniższe sugestie do pracy z wskaźniki działania:

- **Jeśli to możliwe, użyj paski postępu** — ponieważ zapewnia wskaźnika działania użytkownika opinii określające, jak długie proces uruchamiany będzie mieć, zawsze używają pasek postępu, jeśli długość jest znany (np. liczbę bajtów do pobrania w pliku).
- **Zachowaj animowany wskaźnik** -użytkowników dotyczą wskaźnika działania nieruchomy zatrzymania aplikacji, więc zawsze należy animować wskaźnika, gdy jest on wyświetlany.
- **Opis zadania przetwarzania** — tylko wyświetlanie wskaźnika działania samodzielnie nie wystarcza; użytkownik musi informowany o procesie one oczekiwania. To łatwy do rozpoznania etykiety (zazwyczaj zdania jednej, pełną), która wyraźnie definiuje zadania.

## <a name="about-progress-bars"></a>Paski postępu — informacje

Pasek postępu prezentuje jako wiersz, który wypełnia kolorem wskazują stan i długość czasochłonnym zadaniem. Paski postępu zawsze należy używać, gdy długość zadań wiadomo lub można obliczyć.

Apple ma poniższe sugestie dotyczące pracy z paski postępu:

- **Dokładnie raportu postępu** -paski postępu zawsze powinni przedstawić dokładną reprezentację czas wymagany do ukończenia zadania. Nigdy nie błędnie reprezentują czas, aby zapewnić aplikacji zajęta.
- **Na użytek dobrze zdefiniowanego czasu trwania** — postęp paski nie powinien tylko wykazywać trwa długie zadanie Umieść, ale nadaj użytkownika i wskazuje, o ile zadanie zostało zakończone i oszacowanie pozostały czas.

## <a name="progress-indicators-and-storyboards"></a>Wskaźniki postępu i planów

Najprostszym sposobem, aby pracować ze wskaźnikiem postępu w aplikacji Xamarin.tvOS jest aby dodać go do interfejsu użytkownika aplikacji przy użyciu projektanta dla systemu iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. W **konsoli rozwiązania**, kliknij dwukrotnie **Main.storyboard** pliku i otwórz go do edycji.

2. Przeciągnij **wskaźnika działania** z **przybornika** i upuść go w widoku: 

    ![Wskaźnik aktywności](progress-indicators-images/activity01.png "wskaźnika działania")

3. W **elementu Widget** karcie **konsoli właściwości**, można dostosować kilka właściwości wskaźnika działania, takie jak jego **styl**, **zachowanie**, i **nazwa**: 

    ![Widżet kartę wskaźnika działania](progress-indicators-images/activity02.png "kartę Widget wskaźnika działania")
    
    **Nazwa** Określa nazwę właściwości, który reprezentuje wskaźnik aktywności w kodzie języka C#.

4. Przeciągnij **widoku postępu** z **przybornika** i upuść go w widoku: 

    ![Wyświetl postęp](progress-indicators-images/activity03.png "widoku postępu")

5. W **elementu Widget** karcie **Explorer właściwości**, można dostosować kilka właściwości widoku postępu, takie jak jego **styl**, **postępu**(procent wykonania), i **nazwa**: 

    ![Na karcie Widget postęp w widoku](progress-indicators-images/activity04.png "kartę Widget w widoku postępu")
    
    **Nazwa** Określa nazwę właściwości, która reprezentuje widok postęp w kodzie języka C#.

6. Zapisz zmiany.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **Main.storyboard** pliku i otwórz go do edycji.

2. Przeciągnij **wskaźnika działania** z **przybornika** i upuść go w widoku: 

    ![Wskaźnik aktywności](progress-indicators-images/activity01-vs.png
    "wskaźnika działania")

3. W **elementu Widget** karcie **Explorer właściwości**, można dostosować kilka właściwości wskaźnika działania, takie jak jego **styl**, **zachowanie**, i **nazwa**: 

    ![Widżet kartę wskaźnika działania](progress-indicators-images/activity02-vs.png "kartę Widget wskaźnika działania")

    **Nazwa** Określa nazwę właściwości, który reprezentuje wskaźnik aktywności w kodzie języka C#.

4. Przeciągnij **widoku postępu** z **przybornika** i upuść go w widoku: 

   ![Wyświetl postęp](progress-indicators-images/activity03-vs.png "widoku postępu")

5. W **elementu Widget** karcie **Explorer właściwości**, można dostosować kilka właściwości widoku postępu, takie jak jego **styl**, **postępu**(procent wykonania), i **nazwa**: 

    ![Na karcie Widget postęp w widoku](progress-indicators-images/activity04-vs.png "kartę Widget w widoku postępu")
    
    **Nazwa** Określa nazwę właściwości, która reprezentuje widok postęp w kodzie języka C#.

6. Zapisz zmiany.

-----

Aby uzyskać więcej informacji na temat pracy z scenorys, zobacz nasze [Hello, przewodnik Szybki Start systemu tvOS](~/ios/tvos/get-started/hello-tvos.md). 

## <a name="working-with-activity-indicators"></a>Praca z działania wskaźników

Jak wspomniano powyżej, wskaźniki działania powinny być wyświetlane długi proces nieokreślony długość uruchomionej aplikacji.

W dowolnym momencie widać, jeśli wskaźnik aktywności jest animacji, sprawdzając jego `IsAnimating` właściwości. Jeśli `HidesWhenStopped` właściwość jest `true`, wskaźnik aktywności automatycznie zostanie ukryta po zatrzymaniu animacji.

Poniższy kod służy do uruchamiania animacji: 

```csharp
ActivityIndicator.StartAnimating();
```

I poniżej spowoduje zatrzymanie animacji:

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> Te wstawki kodu zakłada, że wskaźnik aktywności **nazwa** ustawiono **ActivityIndicator** w **elementu Widget** projektanta dla systemu iOS na karcie.

## <a name="working-with-progress-bars"></a>Praca z paski postępu

Ponownie pasek postępu należy używać dowolnej chwili aplikacji jest wykonywania długotrwałych zadań trwania znane. 

`Progress` Właściwość jest używana do ustawiania ilości zadań, która została ukończona od 0 do 100% (od 0,0 do 1,0). Użyj `ProgressTintColor` właściwości, aby ustawić kolor na pasku wielkość ukończenia i `TrackTintColor` właściwości, aby ustawić kolor tła (kwota niezakończona).

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego projektowanie i Praca z wskaźniki postępu wewnątrz aplikacji Xamarin.tvOS.

## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [systemu tvOS człowieka przewodniki — interfejs](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Przewodnik programowania w języku aplikacji dla systemu tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
