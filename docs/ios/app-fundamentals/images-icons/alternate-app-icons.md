---
title: Ikony aplikacji alternatywnej
description: "W tym artykule omówiono w platformy Xamarin.iOS przy użyciu ikony aplikacji alternatywny."
ms.topic: article
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: ff24a1411a7ddf2ca78c7997f1dc37744013ece4
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="alternate-app-icons"></a>Ikony aplikacji alternatywnej

_W tym artykule omówiono w platformy Xamarin.iOS przy użyciu ikony aplikacji alternatywny._

Apple został dodany kilka ulepszeń w systemach iOS 10.3 Zezwalaj aplikacji na zarządzanie jego ikonę:

 - `ApplicationIconBadgeNumber` -Pobiera lub ustawia identyfikator ikony aplikacji w Springboard.
 - `SupportsAlternateIcons` -Jeśli `true` aplikacja ma inny zestaw ikon.
 - `AlternateIconName` -Zwraca nazwę alternatywnego ikony aktualnie wybrane lub `null` Jeśli za pomocą ikony podstawowego.
 - `SetAlternameIconName` — Użyj tej metody, aby przełączyć się ikona aplikacji do danego ikona alternatywny.

![](alternate-app-icons-images/icons04.png "Przykładowy alert zmianie jego ikona aplikacji")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Dodawanie alternatywnego ikony do projektu platformy Xamarin.iOS

Aby umożliwić aplikacji przełączyć się do alternatywnego ikony, kolekcją obrazów ikony należy do uwzględnienia w projekcie aplikacji platformy Xamarin.iOS. Tych obrazów nie można dodać do projektu przy użyciu typowego `Assets.xcassets` metody, muszą zostać dodane do **zasobów** bezpośrednio w folderze.

Wykonaj następujące czynności:

1. Wybierz ikonę wymagane obrazy w folderze, wybierz wszystkie i przeciągnij je do **zasobów** folderu w **Eksploratora rozwiązań**:

    ![](alternate-app-icons-images/icons00.png "Wybierz obrazy ikon z folderu")

2. Po wyświetleniu monitu wybierz **kopiowania**, **dla wszystkich wybranych plików, użyj tego samego akcji** i kliknij przycisk **OK** przycisk:

    ![](alternate-app-icons-images/icons02.png "Dodaj plik do folderu, okno dialogowe")

3. **Zasobów** folder powinien wyglądać podobnie do następującej po zakończeniu:

    ![](alternate-app-icons-images/icons01.png "Folderu zasobów powinien wyglądać następująco")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Modyfikowanie pliku Info.plist

Przy użyciu obrazów wymaganych dodane do **zasobów** folderu, [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) klucz będzie musi zostać dodany do projektu **Info.plist** pliku. Ten klucz będzie zdefiniować nazwę ikonę Nowy i obrazów, które go tworzą.

Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **Info.plist** plik, aby otworzyć do edycji.
2. Przełącz się do **źródła** widoku.
3. Dodaj **pakietu ikony** klucza i pozostawić **typu** ustawioną **słownika**.
4. Dodaj `CFBundleAlternateIcons` klucza i ustaw **typu** do **słownika**.
5. Dodaj `AppIcons2` klucza i ustaw **typu** do **słownika**. Będzie nazwa nowego zestawu ikona aplikacji alternatywny.
6. Dodaj `CFBundleIconFiles` klucza i ustaw **typu** do **tablicy**
7. Dodaj nowy ciąg do `CFBundleIconFiles` tablicy dla każdego pliku ikony, pomijając rozszerzenia i `@2x`, `@3x`, sufiksy itp. (przykład `100_icon`). Powtórz ten krok dla każdego pliku, który stanowi zestaw ikona alternatywny.
8. Dodaj `UIPrerenderedIcon` klucza `AppIcons2` słownika, ustaw **typu** do **logiczna** i wartości do **nr**.
9. Zapisz zmiany w pliku.

Powstałe w ten sposób **Info.plist** pliku powinna wyglądać podobnie do następującej po zakończeniu:

![](alternate-app-icons-images/icons03.png "Ukończono plik Info.plist")

Podobnie jak w przypadku tego otworzyć lub w edytorze tekstu:

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>Zarządzanie ikona aplikacji 

Przy użyciu obrazów ikony dołączony do projektu platformy Xamarin.iOS i **Info.plist** pliku prawidłowo skonfigurowane, deweloper może użyć jednego z wielu nowe funkcje dodane do systemu iOS 10.3 do sterowania ikonę aplikacji.

`SupportsAlternateIcons` Właściwość `UIApplication` klasy umożliwia deweloperowi sprawdzić, czy aplikacja obsługuje alternatywny ikon. Na przykład:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` Właściwość `UIApplication` klasy umożliwia deweloperowi get lub set w Springboard wskaźnika bieżąca liczba ikonę aplikacji. Wartość domyślna to zero (0). Na przykład:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName` Właściwość `UIApplication` klasa umożliwia deweloperowi uzyskać nazwy ikony aktualnie wybranej aplikacji alternatywnego lub zwraca `null` Jeśli aplikacja używa podstawowego ikony. Na przykład:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName` Właściwość `UIApplication` klasy umożliwia deweloperowi zmienić ikonę aplikacji. Przekaż ikonę, aby wybrać nazwę lub `null` aby powrócić do głównej ikony. Na przykład:

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

Gdy aplikacja jest uruchamiana i użytkownika wybierz ikonę alternatywne, zostanie wyświetlony alert podobne do poniższych:

![](alternate-app-icons-images/icons04.png "Przykładowy alert zmianie jego ikona aplikacji")

Jeśli użytkownik zmienia do głównej ikony, zostanie wyświetlony alert podobne do poniższych:

![](alternate-app-icons-images/icons05.png "Przykładowy alert, gdy aplikacji zmieni się na głównej ikony")

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokrywającego dodawanie ikon aplikacji alternatywny do projektu platformy Xamarin.iOS i ich użycie wewnątrz aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [iOSTenThree próbki](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
