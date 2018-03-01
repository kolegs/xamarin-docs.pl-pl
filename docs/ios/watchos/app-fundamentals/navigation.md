---
title: Praca z nawigacji
ms.topic: article
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 79277d412b87e6ac44557122fa06d4d5d873fd38
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-navigation"></a>Praca z nawigacji

Najprostsza opcja nawigacji dostępne na wyrażenie kontrolne jest prostą [modalnych menu podręczne](#modal) wyświetlonym u góry bieżącej sceny.

Dla wielu sceny czujki aplikacje są dostępne dwa wzorcami nawigacji:

- [Hierarchiczna nawigacji](#Hierarchical_Navigation)
- [Na stronie interfejsów](#Page-Based_Interfaces)

## <a name="modal-interfaces"></a>Modalne interfejsów

Użyj `PresentController` metodę, aby otworzyć kontrolera interfejsu w trybie modalnym. Kontroler interfejsu już musi być zdefiniowana w **Interface.storyboard**.

```csharp
PresentController ("pageController","some context info");
```

Kontrolery trybie modalnym przedstawione używają cały ekran (obejmujące poprzedniej sceny). Domyślnie tytuł jest ustawiona wartość **anulować** i naciskając go zignorować będzie kontrolera.

Aby programowo zamknąć trybie modalnym przedstawione kontrolera, należy wywołać `DismissController`.

```csharp
DismissController();
```

Modalne ekrany może być jednym sceny albo użyj układ oparty na stronie.


## <a name="hierarchical-navigation"></a>Hierarchiczna nawigacji

Przedstawia informacje o sceny, takich jak stosu, który można nawigować za pośrednictwem, podobnie jak `UINavigationController` działa w systemie iOS. Sceny można wypychana na stosie nawigacji i zdjęte ze stosu (programowo lub przez określonego użytkownika).

![](navigation-images/hierarchy-1.png "Sceny może zostać umieszczony na stosie nawigacji") ![ ] (navigation-images/hierarchy-2.png "sceny może zostać zdjęte ze stosu wylogowuje na stosie nawigacyjnym")

Podobnie jak w przypadku systemu iOS, po lewej edge Przejdź nawiguje wstecz do kontrolera elementu nadrzędnego w stosie nawigacji hierarchicznej.

Zarówno [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog) i [WatchTables](https://developer.xamarin.com/samples/WatchTables) przykłady obejmują hierarchiczna nawigacji.

### <a name="pushing-and-popping-in-code"></a>Wypychanie i wyświetlanie w kodzie

Obejrzyj zestawu nie wymaga nadmiernie przechodząca "controller nawigacji" ma zostać utworzony, takich jak iOS jest — po prostu push przy użyciu kontrolera `PushController` — metoda i stos nawigacji zostanie utworzone automatycznie.

```csharp
PushController("secondPageController","some context info");
```

Obejrzyj ekranu będzie zawierać **ponownie** przycisk w lewej górnej części, ale można również programowane Usuwanie sceny z stos nawigacji przy użyciu `PopController`.

```csharp
PopController();
```

Zgodnie z systemem iOS, jest również możliwe powrócić do katalogu głównego przy użyciu stos nawigacji `PopToRootController`.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Przy użyciu Segues

Segues mogą być tworzone między sceny scenorysu, aby zdefiniować hierarchiczna nawigacji. Można pobrać kontekstu dla sceny docelowy wywołania systemu operacyjnego `GetContextForSegue` zainicjować nowego kontrolera interfejsu.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```

## <a name="page-based-interfaces"></a>Na stronie interfejsów

Na stronie interfejsy szybko przesuń od lewej do prawej, podobnie jak `UIPageViewController` działa w systemie iOS. Wskaźnik kropki są wyświetlane u dołu ekranu, aby wyświetlić stronę, która jest aktualnie wyświetlany.

![](navigation-images/paged-1.png "Przykładowe pierwszej strony") ![ ] (navigation-images/paged-2.png "drugiej stronie próbki") ![ ] (navigation-images/paged-5.png "przykładową stronę piąty")


Aby interfejs oparty na stronie głównej interfejsu użytkownika dla aplikacji czujki, użyj `ReloadRootControllers` z kontrolerów interfejsu i konteksty:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

Na stronie kontrolera, który nie jest elementem głównym można także przedstawić przy użyciu `PresentController` z jednego z innych sceny w aplikacji.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>Linki pokrewne

- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WatchTables (przykład)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchTables/)
