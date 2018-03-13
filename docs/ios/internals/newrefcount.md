---
title: "Nowy System zliczania odwołania"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 43b357eecb0974884db645a0b2e5c8467ddf3b5d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="new-reference-counting-system"></a>Nowy System zliczania odwołania

Xamarin.iOS 9.2.1 wprowadzono rozszerzoną zliczanie systemu, aby wszystkie aplikacje domyślnie. Mogą być używane eliminuje wiele problemów pamięci, które były trudne do śledzenia i napraw we wcześniejszych wersjach platformy Xamarin.iOS.

## <a name="enabling-the-new-reference-counting-system"></a>Włączanie nowej zliczanie systemu

Począwszy od platformy Xamarin 9.2.1 nowe ref zliczania systemu jest włączona, aby **wszystkie** aplikacje domyślnie.

Jeśli tworzysz istniejącej aplikacji, można sprawdzić pliku .csproj, aby upewnić się, że wszystkie wystąpienia `MtouchUseRefCounting` są ustawione na `true`, tak jak poniżej:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Jeśli wartość jest ustawiona na `false` aplikacji nie będą używać nowego narzędzia.

### <a name="using-older-versions-of-xamarin"></a>Przy użyciu starszej wersji programu Xamarin

Xamarin.iOS 7.2.1 i powyżej funkcji rozszerzonego podglądu naszej nowej zliczanie systemu.

**Klasycznego interfejsu API:**

Aby włączyć ten nowy System zliczania odwołania, zaznacz **użyj rozszerzenia zliczanie** wyboru znalezione w **zaawansowane** kartę projektu **opcje kompilacji systemu iOS** , jak pokazano poniżej: 

[![](newrefcount-images/image1.png "Włącz nowy System zliczania odwołania")](newrefcount-images/image1.png#lightbox)

Należy pamiętać, że te opcje został usunięty w nowszej wersji programu Visual Studio dla komputerów Mac.

 **[Ujednolicony interfejs API:](~/cross-platform/macios/unified/index.md)**

 Nowe rozszerzenia zliczanie jest wymagana dla interfejsu API Unified i powinna być włączona domyślnie. Starsze wersje programu IDE nie może mieć wartości sprawdzana automatycznie i może być konieczne zaznacz przez siebie.

    
> [!IMPORTANT]
> **Uwaga:** wcześniejszej wersji tej funkcji została wokół ponieważ MonoTouch 5.2 ale była dostępna tylko dla **sgen** jako eksperymentalne podglądu. Ta wersja nowego, rozszerzone teraz jest również dostępny do **Boehm** modułu zbierającego elementy bezużyteczne.


W przeszłości pojawiły się dwa rodzaje obiekty zarządzane przez Xamarin.iOS: te, które zazwyczaj były jedynie otokę obiekt natywny (obiektów równorzędnych) i tymi, które rozszerzony lub włączyć nowe funkcje (obiekty pochodne) - przechowując bardzo stan w pamięci. Poprzednio było możliwe, że firma Microsoft może rozszerzyć obiektu równorzędnego o stanie (na przykład przez dodanie obsługi zdarzeń języka C#), ale że nie możemy udostępnić obiekt Przejdź, której nie istniały odwołania, a następnie zbierane. Może to spowodować awarię później (np. Jeśli środowiska uruchomieniowego języka Objective-C wywołanie zwrotne do zarządzanego obiektu).

Nowy system automatycznie uaktualnia obiektów równorzędnych na obiekty, które są zarządzane przez środowisko uruchomieniowe ich przechowywania dodatkowych informacji.

To rozwiązuje różnych awarii, które wystąpiły w sytuacjach, takich jak ta:

```csharp
class MyTableSource : UITableViewSource {
   public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath) {
        var cell = tableView.DequeueReusableCell ("myId");
        if (cell != null)
                return cell;

        cell = new UITableViewCell (UITableViewCellStyle.Default, "myId");
        var txt = new UITextField ();
        txt.TouchDown += delegate { Console.WriteLine ("...."); };
        cell.ContentView.AddSubview (txt);
        return cell;
   }
}
```

Bez rozszerzenia liczba odwołanie ten kod będzie awarii, ponieważ `cell` staje się kolekcjonowanych i dlatego jego `TouchDown` delegata, który będzie umożliwiło zawieszonego wskaźnika.

Reference — rozszerzenie liczba zapewnia zarządzanego obiektu pozostaje aktywne i uniemożliwia jego kolekcja dostarczony obiekt natywny jest zachowywany przez kodu natywnego.

Nowy system również eliminuje to potrzebę *większość* prywatnej kopii pola używane w powiązaniach — które podtrzymywania wystąpienia metody domyślnej. Konsolidator zarządzanych jest inteligentne usunąć wszystkie te *niepotrzebnych* rozszerzenia liczba pól z aplikacji przy użyciu nowego odwołania.

Oznacza to, że każdy wystąpień obiektów zarządzanych zużywać mniej pamięci niż przed. On również rozwiązuje pokrewne problem, gdy niektóre pola zapasowy zawiera odwołań, które nie zostały już wymagane przez środowisko uruchomieniowe języka Objective-C, utrudniając odzyskać pamięci.
