---
title: "Postęp i wskaźniki działania"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: b91c0d7504b391630beded7a52e240a0d043152f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="progress-and-activity-indicators"></a>Postęp i wskaźniki działania

Istnieje prawdopodobieństwo, że aplikacja do przeprowadzania long uruchomionych zadań, takich jak ładowania lub przetwarzania danych i to opóźnienie może spowodować opóźnienia w Interfejsie użytkownika aktualizacji. W tym czasie zawsze należy używać wskaźnika postępu przywróceniu zaufania użytkownika, że system jest zajęty, ponieważ wykonuje pracę. Dzięki temu kontrolki użytkownika, czy aplikacja działa na ich żądanie, które nie oczekuje danych wejściowych, i może stanowić sposób określające, dokładnie tak jak długo mają oczekiwania.

iOS udostępnia dwa sposoby zapewnienia wskazuje to postęp w aplikacji: wskaźniki działania (łącznie z konkretną _sieci_ wskaźnika działania) i paski postępu.

## <a name="activity-indicator"></a>Wskaźnik aktywności

Wskaźniki działania powinny być wyświetlane, gdy aplikacja działa długi proces, ale nie znasz dokładnego długość razem, gdy zadanie będzie wymagać.

Apple ma poniższe sugestie do pracy z wskaźniki działania:

- **Jeśli to możliwe, użyj w zamian paski postępu** — ponieważ wskaźnika działania daje użytkownikowi nie opinii określające, jak długo trwa uruchamianie procesu będzie mieć, zawsze używaj pasek postępu, jeśli długość jest znana (np. liczbę bajtów do pobrania w pliku).
- **Zachowaj animowany wskaźnik** -użytkowników dotyczą nieruchomy wskaźnika działania zatrzymania aplikacji dzięki powinien zawsze mieć wskaźnik animowany podczas jego wyświetlania.
- **Opis zadania przetwarzania** — tylko wyświetlanie wskaźnika działania samodzielnie nie wystarcza, użytkownik musi mieć świadomość, proces oczekuje na informacje. To łatwy do rozpoznania etykiety (zazwyczaj zdania jednej, pełną), która wyraźnie definiuje zadania.

### <a name="implementing-an-activity-indicator"></a>Implementowanie wskaźnika działania

Wskaźnik aktywności jest implementowane za pośrednictwem [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) klasy, aby wskazać, że `UIActivity` odbywa się.

### <a name="activity-indicators-and-storyboards"></a>Wskaźniki działania i planów

Jeśli używasz systemu iOS Designer do tworzenia interfejsu użytkownika, wskaźnika działania można dodać do układu z przybornika. Następujące właściwości można dostosować z konsoli właściwości:

![Właściwości konsoli](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Zarządzanie zachowanie wskaźnika działania

Użyj `StartAnimating()` i `StopAnimating()` metody uruchamianie i zatrzymywanie działania animacji wskaźnika.

Ustaw `HidesWhenStopped` właściwości `true` dokonanie wskaźnika działania zniknąć po `StopAnimating()` została wywołana. Ta wartość jest równa `true` domyślnie. W dowolnym momencie zostanie wyświetlony, jeśli wskaźnik aktywności jest uruchomiona Animacja Obracająca sprawdzając `IsAnimating` właściwości. 


### <a name="managing-activity-indicator-appearances"></a>Zarządzanie wygląd wskaźnika działania

`UIActivityIndicatorViewStyle` Wyliczenie można przekazać jako parametru podczas tworzenia wystąpienia wskaźnika działania. Umożliwia to ustawioną stylu wizualnego `Gray`, `White`, lub `WhiteLarge`, na przykład:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Można zmienić kolor dostarczonych przez `UIActivityIndicatorViewStyle` przez ustawienie `Color` właściwości.

## <a name="progress-bar"></a>pasek postępu

Pasek postępu prezentuje jako wiersz, który wypełnia kolorem wskazują stan i długość czasochłonnym zadaniem. Paski postępu zawsze należy używać, gdy długość zadań jest wiadomo lub można obliczyć.

Apple ma poniższe sugestie dotyczące pracy z paski postępu:

- **Dokładnie raportowania postępu** -paski postępu powinna być zawsze dokładną reprezentację czas wymagany do ukończenia zadania. Nigdy nie błędnie reprezentują czas, aby zapewnić aplikacji zajęta.
- **Na użytek okresów Well-Defined** — pasek postępu nie powinien tylko wykazywać trwa długie zadanie Umieść, ale nadaj użytkownika i wskazuje, o ile zadanie zostało zakończone i oszacowanie pozostały czas.

### <a name="implementing-an-progress-bar"></a>Implementacja paska postępu

Pasek postępu jest tworzona przy uruchamianiu [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>Paski postępu i planów

Można również dodać pasek postępu do interfejsu użytkownika, korzystając z systemem iOS projektanta. Wyszukaj **widoku postępu** w **przybornika** i przeciągnij go do widoku.

W konsoli właściwości można dostosować następujące właściwości:

![Właściwości konsoli](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>Zarządzanie zachowanie pasek postępu

Postęp paska można początkowo ustawić za pomocą `Progress` właściwości:

```csharp
ProgressBar.Progress = 0f;
```

Postęp można dostosować za pomocą `SetProgress` — metoda i przekazywanie wartość logiczną zadeklarowanie, jeśli chcesz, aby zmiany animowane lub nie.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Aby uzyskać więcej informacji na temat używania pasek postępu, zapoznaj się [raportowania postępu](https://developer.xamarin.com/recipes/cross-platform/networking/download_progress/#Reporting_Progress_in_iOS) przepisu i [UICatalog systemu tvOS próbki](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>Zarządzanie wygląd paska postępu

Podobne do wskaźnika działania `UIProgressViewStyle` wyliczenie można przekazać jako parametru podczas tworzenia wystąpienia pasek postępu.

Postęp i Śledź obrazu i kolor odcienia można dostosować za pomocą następujących właściwości:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



