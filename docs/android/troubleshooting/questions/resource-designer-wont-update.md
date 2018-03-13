---
title: "Nie może zaktualizować pliku Resource.designer.cs systemu Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 1b1496f4a6a504c8e991f853c92f937015797aa6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Nie może zaktualizować pliku Resource.designer.cs systemu Android

> [!NOTE]
> Ten problem został rozwiązany w programie Xamarin Studio 5.1.4 i nowszych wersjach. Jednak jeśli problem wystąpi w programie Visual Studio dla komputerów Mac, pliku [nowych usterek](~/cross-platform/troubleshooting/questions/howto-file-bug.md) z Twojej wersji pełne informacje i pełny dziennik dane wyjściowe kompilacji.

Błąd w wersji 5.1 Xamarin.Studio wcześniej uszkodzone pliki .csproj usuwając częściowo lub całkowicie kod xml w pliku .csproj. To spowodowałoby ważne części Android kompilacji systemu (np. aktualizowanie Android Resource.designer.cs), aby zakończyć się niepowodzeniem. Począwszy od wersji stabilnej 5.1.4 wersji na 15 lipca, ten problem został rozwiązany; Jednak w wielu przypadkach plik projektu ma zostać naprawiona ręcznie, jak opisano poniżej.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Dwa podejścia możliwy do naprawienia pliku projektu

### <a name="either"></a>Dostępne opcje:

1) Utwórz nowy projekt aplikacji platformy Xamarin.Android, ustaw właściwości projektu do dopasowania starego projektu i dodać wszystkie zasoby, itp. pliki źródłowe z powrotem do projektu.

### <a name="or"></a>LUB

2) Utwórz kopię zapasową pliku .csproj oryginalny projekt, otwórz go w edytorze tekstów i powodują ponowne dodanie w brakujące elementy z pliku .csproj prawidłowo wygenerowany.

### <a name="if-this-does-not-solve-the-problem"></a>Jeśli to nie rozwiąże problemu

Po eksperymentowanie z tych elementów, możesz zauważyć, że po dodaniu powrotem elementy i ponownie skompilować projekt, plik Resource.designer.cs będzie aktualizował, ale następnie nadal może być konieczne Zamknij i ponownie otwórz rozwiązanie, aby uzyskać uzupełniania kodu do rozpoznania nowe typy zawarte w Resource.designer.cs. 
