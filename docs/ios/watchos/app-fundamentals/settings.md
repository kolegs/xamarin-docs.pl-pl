---
title: Praca z ustawieniami
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 3ff8800f4e8690069f5394193d11552d917baffe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-settings"></a>Praca z ustawieniami

Apple Watch aplikacje mogą używać funkcji ustawienia jako aplikacje dla systemu iOS — wyświetlany jest interfejs użytkownika ustawień w **Apple Watch** aplikację telefonów iPhone, ale wartości są dostępne w aplikacji iPhone i rozszerzenia czujki.

![](settings-images/intro.png "Apple Watch aplikacje mogą używać funkcji ustawienia jako aplikacje dla systemu iOS")

Ustawienia będą przechowywane w lokalizacji udostępnionego pliku, który jest dostępny do aplikacji systemu iOS i rozszerzenie aplikacji czujki, zdefiniowane przez **grupy aplikacji**. Należy [skonfigurować grupę aplikacji](~/ios/watchos/app-fundamentals/app-groups.md) przed dodaniem ustawienia przy użyciu z poniższymi instrukcjami.

## <a name="add-settings-in-a-watch-solution"></a>Dodaj ustawienia w rozwiązaniu czujki

W **aplikacji iPhone** w rozwiązaniu (*nie* czujki aplikacji lub rozszerzenia):

1. Kliknij prawym przyciskiem myszy **Dodaj > Nowy plik...**  i wybierz polecenie **Settings.bundle** (nie można edytować nazwy w **nowy plik** okna dialogowego):

   [ ![](settings-images/settings-add-sml.png "Dodaj nowy pakiet ustawień")](settings-images/settings-add.png)

2. Zmień nazwę, aby **Watch.bundle ustawienia** (Wybierz i wpisz **polecenia + R** można zmienić nazwy):

   ![](settings-images/settings-rename.png "Zmień nazwę pakietu")

3. Dodaj nowy klucz `ApplicationGroupContainerIdentifier` do **Root.plist** o wartości do grupy aplikacji skonfigurowano, (np.) `group.com.xamarin.WatchSettings` w przykładzie):

   [ ![](settings-images/settings-appgroup-sml.png "Dodawanie klucza ApplicationGroupContainerIdentifier do Root.plist")](settings-images/settings-appgroup.png)

4. Edytuj **Settings-Watch.bundle/Root.plist** zawiera opcje, aby użyć — plik szablonu zawiera grupę.
  TextField, Przełącz przełącznika i suwaka (co w przypadku usunięcia i zastąpić własnymi ustawieniami):

  [ ![](settings-images/rootplist-sml.png "Edytuj Settings-Watch.bundle/Root.plist")](settings-images/rootplist.png)


## <a name="use-settings-in-the-watch-app"></a>Użyj ustawień w aplikacji czujki

Aby uzyskać dostęp do wartości wybrane przez użytkownika, należy utworzyć `NSUserDefaults` wystąpienia przy użyciu grupy aplikacji i określając `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch aplikacji

[ ![](settings-images/settings-app-sml.png "Nowa aplikacja Apple Watch na telefonie iPhone")](settings-images/settings-app.png)

Użytkownicy będzie wchodzić w interakcje ustawień za pomocą nowej **Apple Watch** aplikacji na ich iPhone. Ta aplikacja umożliwia użytkownikowi Pokaż/Ukryj aplikacje na wyrażenie kontrolne, a także Edytuj ustawienia ujawnianej za pomocą **Watch.bundle ustawienia**.

![](settings-images/applewatch-1.png "Przykład ustawień aplikacji") ![ ] (settings-images/applewatch-2.png "przykład ustawień aplikacji")



## <a name="related-links"></a>Linki pokrewne

- [Doc ustawienia firmy Apple](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
