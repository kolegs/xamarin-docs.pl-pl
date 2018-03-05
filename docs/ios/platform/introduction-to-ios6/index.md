---
title: Wprowadzenie do systemu iOS 6
description: "iOS 6 zawiera szereg nowych technologii umożliwiający projektowanie aplikacji, które Xamarin.iOS 6 oferuje deweloperom C#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 0d08b2ff5131996984dbe41862ad1d6aac33b3d0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-6"></a>Wprowadzenie do systemu iOS 6

_iOS 6 zawiera szereg nowych technologii umożliwiający projektowanie aplikacji, które Xamarin.iOS 6 oferuje deweloperom C#._

[ ![](images/ios6-large.jpg "Logo systemu iOS 6")](images/ios6-large.jpg)

IOS 6 i Xamarin.iOS 6 deweloperzy teraz dysponują wiele możliwości do tworzenia aplikacji systemu iOS, w tym te, że docelowy telefonu iPhone 5.
Ten dokument zawiera listę niektórych ciekawsze nowe funkcje, które są dostępne i łącza do artykułów dla każdego tematu. Ponadto dotyka na kilka zmian, które będą ważne, jak przenieść deweloperzy systemu iOS 6 i nowe rozwiązanie telefonu iPhone 5.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Wprowadzenie do kolekcji widoków](~/ios/user-interface/controls/uicollectionview.md)

Kolekcja widoków zezwala na zawartość mają być wyświetlane przy użyciu dowolnego układów. Umożliwiają one łatwe tworzenie układów siatki fabrycznej, obsługuje również układy niestandardowe. Aby uzyskać więcej informacji, zobacz, [wprowadzenie do kolekcji widoków](~/ios/user-interface/controls/uicollectionview.md) [ ](~/ios/user-interface/controls/uicollectionview.md)przewodnik.


## <a name="introduction-to-pass-kitiosplatformpasskitmd"></a>[Wprowadzenie do przekazania zestawu](~/ios/platform/passkit.md)

Framework przekazać zestawu umożliwia aplikacjom komunikowanie się z przekazuje cyfrowe, które są zarządzane w aplikacji Passbook. Aby uzyskać więcej informacji, zobacz, [wprowadzenie do przewodnika przekazać zestawu](~/ios/platform/passkit.md).


##  <a name="introduction-to-event-kitiosplatformeventkitmd"></a>[Wprowadzenie do zestawu zdarzeń](~/ios/platform/eventkit.md)

W ramach EventKit umożliwia dostęp do kalendarzy, zdarzenia kalendarza i przypomnienia dane przechowywane na bazę danych kalendarza. Dostęp do kalendarzy i kalendarza zdarzeń została dostępne począwszy od zestawu iOS 4, ale iOS 6 udostępnia teraz dostęp do danych przypomnienia. Aby uzyskać więcej informacji, zobacz [I](~/ios/platform/eventkit.md) [ntroduction do EventKit](~/ios/platform/eventkit.md) przewodnik.


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Wprowadzenie do struktury społecznościowych](~/ios/platform/social-framework.md)

Społecznościowych Framework zapewnia ujednolicony interfejs API do interakcji z sieciami społecznościowymi, łącznie z serwisem Twitter i Facebook, a także SinaWeibo dla użytkowników w Chinach. Aby uzyskać więcej informacji, zobacz, [wprowadzenie do struktury społecznościowych](~/ios/platform/social-framework.md) przewodnik.


##  <a name="changes-to-store-kitchanges-to-storekitmd"></a>[Zmiany do przechowywania zestawu](changes-to-storekit.md)

Apple wprowadziła dwóch nowych funkcji w zestawie magazynu: zakupów i pobieranie iTunes lub zawartość sklepu z aplikacjami z aplikacji i hosting pliki zawartości dla zakupy w aplikacjach!. Aby uzyskać więcej informacji, zobacz, [zmiany w magazynie zestawu](changes-to-storekit.md) przewodnik.


## <a name="other-changes"></a>Inne zmiany


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload i ViewDidUnload przestarzałe

`ViewWillUnload` i `ViewDidUnload` metody `UIViewController` nazywane są już w systemie iOS 6. W poprzednich wersjach systemu IOS te metody mogą były używane przez aplikacje odpowiednio zapisywania stanu przed zwalnia widoku i oczyszczanie kodu.

Na przykład utworzyć metodę o nazwie programu Visual Studio for Mac `ReleaseDesignerOutlets`, pokazano poniżej, który następnie będzie można wywołać z `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Jednak w systemie iOS 6, nie jest już konieczne wywołać `ReleaseDesignerOutlets`.   
   
   
   
Oczyszczanie kodu aplikacji systemu iOS 6 należy używać `DidReceiveMemoryWarning`. Jednak kod tego wywołania `Dispose` powinny być używane rzadko i tylko dla pamięci znacznym obiektów jak pokazano poniżej:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Ponownie, wywoływania `Dispose` zgodnie z powyższym rzadko powinny być potrzebne. W ogólności, które najbardziej aplikacji należy wykonać, jest usunięcie programów obsługi zdarzeń.

W przypadku zapisania stanu aplikacji można wykonać w `ViewWillDisappear` i `ViewDidDisappear` zamiast `ViewWillUnload`.


### <a name="iphone-5-resolution"></a>telefonu iPhone 5 rozwiązania

urządzenia iPhone 5 mieć rozdzielczość 640 x 1136. Aplikacje, które objęte poprzedniej wersji systemu iOS zostaną wyświetlone letterboxed uruchamiania na telefonie iPhone 5, jak pokazano poniżej:

 [ ![](images/01-letterboxed.png "Aplikacje, które objęte poprzedniej wersji systemu iOS będą wyświetlane letterboxed uruchamianych na telefonie iPhone 5")](images/01-letterboxed.png)

W kolejności stosowania pojawią się pełnego ekranu na telefonach iPhone 5, po prostu Dodaj obraz o nazwie `Default-568h@2x.png` posiadające rozdzielczość 640 x 1136. Poniższy zrzut ekranu przedstawia aplikacja była uruchomiona po uwzględniono tego obrazu:

 [ ![](images/02-fullscreen.png "Ten zrzut ekranu przedstawia aplikacja była uruchomiona po ten obraz został dołączony")](images/02-fullscreen.png)

### <a name="subclassing-uinavigationbar"></a>Tworzenie podklas UINavigationBar

W systemie iOS 6 `UINavigationBar` może być podklasą klasy. Dzięki temu dodatkową kontrolę wyglądu i działania `UINavigationBar`. Na przykład aplikacje mogą podklasy dodać widoków podrzędnych, animować tych widoków i modyfikowania granice `UINavigationBar`.

Poniższy kod przedstawia przykład podklasą `UINavigationBar` dodaje `UIImageView`:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

Aby dodać podklasą `UINavigationBar` do `UINavigationController`, użyj `UINavigationController` konstruktora, który przyjmuje typ `UINavigationBar` i `UIToolbar`, jak pokazano poniżej:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

Za pomocą tej `UINavigationBar` podklasy powoduje wyświetlanie obrazu będzie wyświetlany, jak pokazano na poniższym zrzucie ekranu:

 [ ![](images/03-navbar.png "Za pomocą tego UINavigationBar podklasy wyniki w widoku obrazu wyświetlanego, jak pokazano w tym zrzut ekranu")](images/03-navbar.png)

### <a name="interface-orientation"></a>Orientacja interfejsu

Przed iOS 6 aplikacje można zastąpić `ShouldAutorotateToInterfaceOrientation`, zwraca true dla dowolnego orientacji kontroler obsługiwane. Na przykład następujący kod będzie służyć do obsługi tylko w orientacji pionowej:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

W systemie iOS 6 `ShouldAutorotateToInterfaceOrientation` jest przestarzały.
Zamiast tego można zastąpić aplikacji `GetSupportedInterfaceOrientations` na kontroler widoku głównego, jak pokazano poniżej:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

Na urządzeniu iPad tej wartości domyślnie przyjmowana wszystkie orientacje cztery Jeśli `GetSupportedInterfaceOrientation` nie jest zaimplementowana. Na telefonie iPhone i iPod Touch, wartość domyślna to wszystkie orientacje z wyjątkiem `PortraitUpsideDown`.