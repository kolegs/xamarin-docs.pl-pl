---
title: Układ aplikacji typu Tablet i pulpitu
description: W tym artykule opisano, jak zoptymalizować układy aplikacji platformy Xamarin.Forms dla tabletów przeciwieństwie telefonów.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: 932b4aa9c865501284b02fc35e12dc8d7e7166fc
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244848"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Układ aplikacji typu Tablet i pulpitu

Platformy Xamarin.Forms obsługuje na obsługiwanych platformach wszystkich typów urządzeń, aby oprócz telefonów, aplikacje można również uruchomić na:

* Ipad,
* Tablety z systemem android
* Tablety z systemem Windows i komputerów stacjonarnych (z systemem Windows 10).

Ta strona pokrótce omówiono:

* obsługiwane [typów urządzeń](#Device_Types), i
* jak [zoptymalizować](#optimize) układów dla tabletów i telefonów.

<a name="Device_Types" />

## <a name="device-types"></a>Typy urządzeń

Większe ekranu urządzenia są dostępne dla wszystkich platform obsługiwanych przez platformy Xamarin.Forms.

### <a name="ipads-ios"></a>iPads (iOS)

Szablon platformy Xamarin.Forms automatycznie obsługuje iPad konfigurując **Info.plist > urządzenia** ustawienie **uniwersalnych** (co oznacza zarówno iPhone i iPad są obsługiwane).

Aby zapewnić środowisko uruchamiania przyjemne i zapewnienia pełnej rozdzielczości jest używany na wszystkich urządzeniach, należy upewnić się [ekran startowy specyficznych dla urządzeń iPad](~/ios/app-fundamentals/images-icons/launch-screens.md) (przy użyciu scenorysu) jest dostępne. Dzięki temu, że aplikacja jest poprawnie renderowana na urządzeniach iPad mini, tabletów iPad i iPad Pro.

Przed systemu iOS 9 wszystkie aplikacje miały pełnego ekranu na urządzeniu, ale niektóre Ipad można teraz wykonywać [podzielić wielozadaniowości ekranu](~/ios/platform/multitasking.md).
Oznacza to, że aplikacja może zająć tylko kolumnę cienki boku ekranu, 50% szerokości ekranu lub cały ekran.

[![](tablet-images/ipad-sml.png "Przykład ekranu podziału iPad")](tablet-images/ipad.png#lightbox "iPad przykład ekranu podziału")

Funkcje podzielonym ekranem oznacza, że należy projektować aplikacji działają prawidłowo w zaledwie 320 pikseli szerokości lub jak 1366 pikseli szerokości.

### <a name="android-tablets"></a>Tablety z systemem android

Android ekosystemu ma różnych rozmiarów ekranu obsługiwana, z małych telefonów do dużych tablety. Platformy Xamarin.Forms może obsługiwać wszystkich rozmiarów ekranu, ale jako z innych platform można dostosować interfejsu użytkownika dla urządzeń większy.

Obsługa wielu różnych rozdzielczości ekranu, można zapewnić zasobów obrazu macierzystego w różnych rozmiarach, aby zoptymalizować środowisko użytkownika.
Przegląd [Android zasobów](~/android/app-fundamentals/resources-in-android/index.md) dokumentacji (w szczególności [tworzenia zasobów dla różnych rozmiarów ekranu](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) Aby uzyskać więcej informacji na temat struktury folderów i plików w aplikacji systemu Android projektu zawierającego zasoby zoptymalizowanego obrazu w aplikacji.

### <a name="windows-tablets-and-desktops"></a>Tablety z systemem Windows i komputerów stacjonarnych

Do obsługi, tablety i komputery stacjonarne z systemem Windows, musisz użyć [obsługi systemu Windows](~/xamarin-forms/platform/windows/installation/index.md), która tworzy uniwersalnych aplikacji uruchamianych w systemie Windows 10.

Aplikacje działające na tablety z systemem Windows i pulpitami może być zmniejszony do dowolnego wymiarów dodatkowo uruchomiona pełnego ekranu.

[![](tablet-images/splitscreen-sml.png "Przykład ekranu podzielić Windows")](tablet-images/splitscreen.png#lightbox "przykład ekranu podział systemu Windows")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optymalizacja dla typu Tablet i pulpitu

Można dostosować w zależności od platformy Xamarin.Forms interfejsu użytkownika, czy telefon lub tablet/pulpitu urządzenie jest używane. Oznacza to, że można zoptymalizować czynności użytkownika dla urządzeń większym ekranem, takich jak tablety i komputery stacjonarne.


### <a name="deviceidiom"></a>Device.Idiom

Można użyć [ `Device` ](~/xamarin-forms/platform/device.md) klasy w celu zmiany zachowania interfejsu użytkownika lub aplikacji. Przy użyciu `Device.Idiom` możesz — wyliczenie

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Tej metody można rozszerzyć, aby znaczących zmian do układów poszczególnych stron, lub nawet do renderowania stron zupełnie innego na większych monitorach.

### <a name="leveraging-masterdetailpage"></a>Korzystanie z usług MasterDetailPage

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) To idealne rozwiązanie w przypadku większych ekrany, szczególnie w przypadku tabletów iPad, których używa [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) zapewnia środowisko natywnej dla systemu iOS.

Przegląd [ten wpis w blogu Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) aby zobaczyć, jak dostosowania interfejsu użytkownika, aby telefony użyj jeden układ i ekrany większych może używać innego (z `MasterDetailPage`).



## <a name="related-links"></a>Linki pokrewne

- [Blog platformy Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [Przykładowe MyShoppe](https://github.com/jamesmontemagno/myshoppe)
