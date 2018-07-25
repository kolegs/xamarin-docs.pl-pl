---
title: Wskaźniki postępu i aktywności w rozszerzeniu Xamarin.iOS
description: W tym dokumencie omówiono sposób używania wskaźniki postępu i aktywności w rozszerzeniu Xamarin.iOS. Przedstawiono sposób ich używać, programowo i za pomocą scenorysu.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 24ad47bd37693eccf38033159acef72a9d4942be
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242182"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Wskaźniki postępu i aktywności w rozszerzeniu Xamarin.iOS

Istnieje prawdopodobieństwo, że aplikacji do przeprowadzania długo wykonywanych zadań, takich jak ładowanie lub przetwarzania danych i to opóźnienie może spowodować opóźnienie podczas aktualizowania interfejsu użytkownika. W tym czasie możesz zawsze należy używać wskaźnik postępu przywróceniu zaufania użytkownika, że system jest zajęty, ponieważ praca. Dzięki temu kontrolkę użytkownika, czy aplikacja działa na żądanie, które nie oczekuje się na swoje dane wejściowe i może stanowić sposób ze szczegółami dotyczącymi dokładnie tak jak długo mają oczekiwania.

dla systemu iOS zawiera dwa główne sposoby zapewnienia to oznaczenie postęp w swojej aplikacji: wskaźniki działania (w tym konkretnym _sieci_ wskaźnik aktywności) i pasków postępu.

## <a name="activity-indicator"></a>Wskaźnik aktywności

Powinna być pokazywana wskaźniki działanie, gdy uruchomiona jest aplikacja, długi proces, ale nie znasz dokładną długość czasu, przez który zadanie będzie wymagać.

Apple zawiera poniższe sugestie dotyczące pracy ze wskaźnikami działania:

- **W miarę możliwości należy użyć postępu paski** — ponieważ wskaźnik aktywności daje użytkownikowi nie informacje zwrotne, o ile proces uruchamiany w potrwa, zawsze używaj pasek postępu, jeśli długość jest znany (np. liczbę bajtów do pobrania w pliku).
- **Zachowaj animowany wskaźnik** — użytkownicy dotyczą stacjonarnych wskaźnik aktywności zatrzymania aplikacji, dzięki czemu zawsze powinien mieć wskaźnik animowany podczas, gdy są wyświetlane.
- **Opisz zadanie trwa przetwarzanie** — tylko wyświetlanie wskaźnik aktywności przez siebie nie jest wystarczająco dużo, użytkownik powinien zostać poinformowany o proces oczekuje na. Obejmują zrozumiałą etykietę (zwykle jednej, pełną zdanie) wyraźnie definiujący zadania.

### <a name="implementing-an-activity-indicator"></a>Implementowanie wskaźnik aktywności

Wskaźnik aktywności jest implementowane za pomocą [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) klasy w celu wskazania, że `UIActivity` odbywa się.

### <a name="activity-indicators-and-storyboards"></a>Wskaźniki działania i scenorysów

Jeśli używasz narzędzia iOS Designer do tworzenia interfejsu użytkownika, wskaźnik aktywności można dodać do układu z przybornika. W okienku właściwości można dostosować następujące właściwości:

![Właściwości konsoli](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Zarządzanie zachowaniem wskaźnik aktywności

Użyj `StartAnimating()` i `StopAnimating()` metody, aby uruchomić i zatrzymać animację wskaźnik aktywności.

Ustaw `HidesWhenStopped` właściwości `true` się wskaźnik aktywności zniknąć po `StopAnimating()` została wywołana. Jest ono ustawione na `true` domyślnie. W dowolnym momencie zostanie wyświetlony, jeśli wskaźnik aktywności jest uruchomiona Animacja obrotowych, sprawdzając `IsAnimating` właściwości. 


### <a name="managing-activity-indicator-appearances"></a>Zarządzanie wygląd wskaźnik aktywności

`UIActivityIndicatorViewStyle` Wyliczenia można przekazać jako parametru podczas tworzenia wystąpienia wskaźnik aktywności. Umożliwia to ustaw styl wizualny `Gray`, `White`, lub `WhiteLarge`, na przykład:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Można zastąpić kolorów, dostarczone przez `UIActivityIndicatorViewStyle` , ustawiając `Color` właściwości.

## <a name="progress-bar"></a>pasek postępu

Przedstawia pasek postępu jako wykres liniowy, który wypełnia kolor, aby wskazać, stanu i długość bardzo czasochłonnym zadaniem. Paski postępu zawsze należy używać, gdy długość zadania jest znana, lub może zostać obliczony.

Apple zawiera poniższe sugestie dotyczące pracy z paski postępu:

- **Precyzyjnie raportować postęp** -paski postępu powinien zawsze być dokładne odzwierciedlenia czas wymagany do ukończenia zadania. Nigdy nie błędnie reprezentują czas, aby zapewnić aplikacji zajęta.
- **Na użytek czasów trwania Well-Defined** — pasek postępu powinien nie pokazuj tylko zajmuje długie zadanie Umieść, ale przyznać użytkownikowi i wskazanie, o ile zadanie zostało zakończone i oszacowanie pozostały czas.

### <a name="implementing-an-progress-bar"></a>Implementowanie pasek postępu

Pasek postępu jest tworzony przez utworzenie wystąpienia [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>Paski postępu i scenorysów

Można również dodać pasek postępu do interfejsu użytkownika, korzystając z narzędzia iOS Designer. Wyszukaj **widoku postępu** w **przybornika** i przeciągnij go do widoku.

W okienku właściwości można dostosować następujące właściwości:

![Właściwości konsoli](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>Zarządzanie zachowaniem pasek postępu

Postęp paska można początkowo ustawić za pomocą `Progress` właściwości:

```csharp
ProgressBar.Progress = 0f;
```

Postęp można dostosować za pomocą `SetProgress` metody i przekazywanie na wartość logiczną deklarowania, jeśli chcesz, aby zmiany animowane lub nie.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Aby uzyskać więcej informacji na temat korzystania z pasek postępu, dotyczą [raportowania postępu](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress) przepisu i [przykładowe systemu tvOS UICatalog](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>Zarządzanie wygląd paska postępu

Podobnie jak wskaźnik aktywności `UIProgressViewStyle` wyliczenia można przekazać jako parametru podczas tworzenia wystąpienia pasek postępu.

Postęp i śledzić obrazu i kolor odcienia, może być regulowany poprzez z następującymi właściwościami:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



