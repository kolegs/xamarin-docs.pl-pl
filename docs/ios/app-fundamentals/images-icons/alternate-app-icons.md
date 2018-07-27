---
title: Alternatywne ikony aplikacji w rozszerzeniu Xamarin.iOS
description: W tym dokumencie opisano sposób użycia alternatywne ikony aplikacji w rozszerzeniu Xamarin.iOS. Omówiono w nim sposób dodawania tych ikon do projektu Xamarin.iOS, sposób modyfikowania pliku Info.plist i się programistycznym zarządzaniem ikonę aplikacji.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 3f009d84d1d38c379f65de52949c66f3e86fc654
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275992"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Alternatywne ikony aplikacji w rozszerzeniu Xamarin.iOS

_Ten artykuł dotyczy alternatywne ikony aplikacji w rozszerzeniu Xamarin.iOS._

Apple został dodany kilka ulepszeń do systemu iOS 10.3, dzięki czemu aplikacji do zarządzania jej ikonę:

 - `ApplicationIconBadgeNumber` -Pobiera lub ustawia wskaźnik ikony aplikacji Springboard.
 - `SupportsAlternateIcons` -Jeśli `true` aplikacja ma inny zestaw ikon.
 - `AlternateIconName` -Zwraca nazwę ikonie alternatywnej zaznaczony lub `null` Jeśli za pomocą ikony głównej.
 - `SetAlternameIconName` — Ta metoda umożliwia Przełącz ikony aplikacji do danego ikonie alternatywnej.

![](alternate-app-icons-images/icons04.png "Przykładowy alert zmianie jego ikonę aplikacji")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Dodawanie ikon alternatywne do projektu Xamarin.iOS

Aby zezwolić na aplikację przełączyć się do alternatywnego ikony, zbiór obrazy ikon należy do uwzględnienia w projekcie aplikacji platformy Xamarin.iOS. Tych obrazów nie można dodać do projektu przy użyciu typowej `Assets.xcassets` metody muszą zostać dodane do **zasobów** bezpośrednio w folderze.

Wykonaj następujące czynności:

1. Wybierz obrazy ikon wymagane w folderze, zaznacz wszystko i przeciągnij je do **zasobów** folderu w **Eksploratora rozwiązań**:

    ![](alternate-app-icons-images/icons00.png "Wybierz obrazy ikon z folderu")

2. Po wyświetleniu monitu wybierz **kopiowania**, **za pomocą tej samej akcji dla wszystkich wybranych plików** i kliknij przycisk **OK** przycisku:

    ![](alternate-app-icons-images/icons02.png "Dodawanie pliku do folderu, okno dialogowe")

3. **Zasobów** folder powinien wyglądać podobnie do następującego po ukończeniu:

    ![](alternate-app-icons-images/icons01.png "Oto jak powinien wyglądać folder zasobów")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Modyfikowanie pliku Info.plist

Za pomocą wymagane obrazy dodane do **zasobów** folderze [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) klucz będzie musiał zostać dodany do projektu **Info.plist** pliku. Ten klucz będzie zdefiniować nazwę nową ikonę i obrazów, które go tworzą.

Wykonaj następujące czynności:

1. W **Eksploratora rozwiązań**, kliknij dwukrotnie **Info.plist** plik, aby otworzyć do edycji.
2. Przełącz się do **źródła** widoku.
3. Dodaj **pakietu ikony** klucza i pozostawić **typu** równa **słownika**.
4. Dodaj `CFBundleAlternateIcons` klucza i ustaw **typu** do **słownika**.
5. Dodaj `AppIcon2` klucza i ustaw **typu** do **słownika**. Są to nazwa nowego zestawu ikon alternatywnej aplikacji.
6. Dodaj `CFBundleIconFiles` klucza i ustaw **typu** do **tablicy**
7. Dodaj nowy ciąg do `CFBundleIconFiles` tablicy dla każdego pliku ikony, pomijając rozszerzenie i `@2x`, `@3x`, sufiksy itp. (przykład `100_icon`). Powtórz ten krok dla każdego pliku, który tworzy zestaw ikon alternatywne.
8. Dodaj `UIPrerenderedIcon` klucza `AppIcon2` słownika, ustaw **typu** do **logiczna** i wartości do **nie**.
9. Zapisz zmiany w pliku.

Wartość wynikowa **Info.plist** plik powinien wyglądać podobnie do następującego po ukończeniu:

![](alternate-app-icons-images/icons03.png "Ukończone pliku Info.plist")

Lub np. Jeśli to otwartego w edytorze tekstu:

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

Przy użyciu obrazów ikony zawarte w projekcie platformy Xamarin.iOS i **Info.plist** pliku prawidłowo skonfigurowane, deweloper może skorzystać jednego z wielu nowych funkcji dodanych do systemu iOS 10.3 do kontrolowania ikonę aplikacji.

`SupportsAlternateIcons` Właściwość `UIApplication` klasy umożliwia deweloperowi zobaczyć, czy aplikacja obsługuje alternatywne ikony. Na przykład:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` Właściwość `UIApplication` klasy umożliwia deweloperowi pobrać lub ustawić bieżącą liczbę wskaźnik ikony aplikacji w Springboard. Wartością domyślną jest zero (0). Na przykład:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName` Właściwość `UIApplication` klasy zezwala dla deweloperów uzyskać nazwę aktualnie wybranej aplikacji alternatywne ikony lub zwraca `null` Jeśli aplikacja korzysta z głównej ikony. Na przykład:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName` Właściwość `UIApplication` klasy umożliwia deweloperom Zmienianie ikony aplikacji. Przekaż ikonę aby wybrać nazwę lub `null` aby powrócić do głównej ikony. Na przykład:

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

Po uruchomieniu aplikacji i użytkownika, wybierz ikonę alternatywne, zostanie wyświetlony alert, jak pokazano poniżej:

![](alternate-app-icons-images/icons04.png "Przykładowy alert zmianie jego ikonę aplikacji")

Gdy użytkownik przejdzie do głównej ikony, zostanie wyświetlony alert, jak pokazano poniżej:

![](alternate-app-icons-images/icons05.png "Przykładowy alert, gdy aplikacji zmieni się na głównej ikony")

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule zawiera opisano Dodawanie alternatywne ikony aplikacji do projektu Xamarin.iOS i korzystanie z nich w aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [iOSTenThree próbki](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
