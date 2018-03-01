---
title: Formant menu (Force Touch)
ms.topic: article
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 87ab9bdf6af68448649d1b3e518743a73bb23fe7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="menu-control-force-touch"></a>Formant menu (Force Touch)

Obejrzyj zestaw zawiera gestu Touch życie wyzwala menu implementując na ekranie app czujki.

![](menu-images/menu.png "Apple Watch przedstawiający menu.")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Odpowiada na życie Touch

Jeśli `Menu` została zaimplementowana dla kontrolera interfejsu, gdy użytkownik wykona życie Touch, zostanie wyświetlone menu. Jeśli menu nie został zaimplementowany, ekran jest krótko animowany nie innych akcji.

Życie poprawki nie są skojarzone z żadnym konkretnym elementem na ekranie; tylko jeden menu może zostać dołączony do kontrolera interfejsu i pojawi się niezależnie od tego, gdzie występuje naciśnij Touch Wymuś na ekranie.

Od 1 do 4 menu Opcje do przedstawienia.


## <a name="adding-a-menu"></a>Dodawanie do Menu

A `Menu` musi zostać dodany do `InterfaceController` scenorysu w czasie projektowania. W przypadku formantu menu jest przeciągnięto kontrolera interfejsu nie jest niewidoczne w wersji zapoznawczej scenorysu, ale **Menu** pojawia się w **konspekt dokumentu** konsoli:

![](menu-images/menu-action.png "Edytowanie menu w czasie projektowania")

Maksymalnie cztery menu można dodać elementów do formantu menu. Można je skonfigurować w **właściwości** konsoli. Można ustawić następujące atrybuty:

- Tytuł, i
- Obraz niestandardowy, lub
- Obraz systemu: Zaakceptuj Add, blok, Odrzuć, Info, może być, więcej, wyciszanie, wstrzymywanie, odtwarzania, powtórz wznowienia, udział, losowa, prelegenta, Kosza.

Utwórz `Action` wybierając **zdarzenia** sekcji **właściwości** konsoli i wpisać nazwę dla metody akcji. Metoda częściowa zostaną utworzone w kod, który można zaimplementować w klasie kontrolera interfejsu następująco:

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>Niestandardowe obrazy

Podobnie jak kartę obrazy w systemie iOS, obrazów elementów menu wymagają nieprzezroczyste wzorca z kanału alfa umożliwiający tło.

Należy dodać obrazami używanymi na potrzeby menu do projektu aplikacji czujki (nie czujki aplikacji rozszerzenia projektu) Aby uzyskać najlepszą wydajność.


## <a name="changing-the-menu-items"></a>Zmiana pozycji Menu

<!--
### Design Time Items

Menu items added the the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>Dodawanie w czasie wykonywania

Nie można spowodować `Menu` do dodania do kontrolera interfejsu w czasie wykonywania, mimo że kolekcję `MenuItem`s *można* należy programowo zmienić.
Użyj `AddMenuItem` metody, jak pokazano:

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Obecnie wymaga interfejsu API zestawu czujki Xamarin.iOS `selector` dla `AdMenuItem` metody, które powinny zostać zadeklarowane w następujący sposób:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>Usuwanie w czasie wykonywania

`ClearAllMenuItems` Można wywołać metody, aby usunąć wszystkie *programowo dodano* elementów menu.

Nie można usunąć elementów menu w scenorysu.



## <a name="related-links"></a>Linki pokrewne

- [WatchKitCatalog (przykład)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Doc Menu firmy Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
