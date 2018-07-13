---
title: Układ aplikacji dla tabletu i pulpitu
description: W tym artykule wyjaśniono, jak zoptymalizować układy aplikacji platformy Xamarin.Forms dla tabletów, w przeciwieństwie do telefonów.
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: b98d1fcf0917b9e25d774a92d56bf90bdd291978
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998639"
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Układ aplikacji dla tabletu i pulpitu

Zestawu narzędzi Xamarin.Forms obsługuje wszystkich typów urządzeń dostępnych na obsługiwanych platformach, dzięki czemu oprócz telefony, aplikacje można również uruchomić na:

* tablety Ipad,
* Tablety z systemem android
* Windows, tablety i komputery stacjonarne (z systemem Windows 10).

Ta strona krótko omówiono:

* obsługiwane [typów urządzeń](#Device_Types), i
* jak [zoptymalizować](#optimize) układów dla tabletów i telefonów.

<a name="Device_Types" />

## <a name="device-types"></a>Typy urządzeń

Większe ekranu urządzenia są dostępne dla wszystkich platform obsługiwanych przez zestaw narzędzi Xamarin.Forms.

### <a name="ipads-ios"></a>iPads (iOS)

Szablon zestawu narzędzi Xamarin.Forms automatycznie obsługuje urządzenia iPad, konfigurując **Info.plist > urządzenia** ustawienie **Universal** (co oznacza zarówno urządzeń iPhone i iPad są obsługiwane).

Aby zapewnić środowisko uruchamiania przyjemny i upewnij się, służy pełnej rozdzielczości na wszystkich urządzeniach, upewnij się, [ekran startowy specyficzne dla urządzenia iPad](~/ios/app-fundamentals/images-icons/launch-screens.md) (przy użyciu scenorysu) jest dostarczany. Gwarantuje to, że poprawnie renderowana aplikacji na urządzeniach iPad mini, tabletów iPad i urządzenia iPad Pro.

Przed systemem iOS 9, wszystkie aplikacje zajmował pełnego ekranu na urządzeniu, ale niektóre urządzenia Ipad można teraz wykonywać [podziału ekranu wielozadaniowość](~/ios/platform/multitasking.md).
Oznacza to, że aplikacja może trwać tylko kolumnę kieszeń boku ekranu 50% szerokości ekranu lub cały ekran.

[![](tablet-images/ipad-sml.png "iPad przykład ekranu podziału")](tablet-images/ipad.png#lightbox "iPad przykład ekranu podziału")

Funkcje ekranu oznacza, że należy projektować aplikację, aby dobrze współpracować z zaledwie 320 pikseli szerokości lub jak 1366 pikseli szerokości.

### <a name="android-tablets"></a>Tablety z systemem android

Ekosystem dla systemu Android ma wiele rozmiarów ekranu obsługiwanych, od telefonów małych do dużych tabletów. Zestaw narzędzi Xamarin.Forms może obsługiwać wszystkich rozmiarów ekranu, ale jako z innymi platformami możesz chcieć dostosować interfejsu użytkownika dla urządzeń z większych.

W przypadku obsługi wielu różnych rozdzielczości ekranu, można zapewnić zasoby obrazów natywnych w różnych rozmiarach, aby poprawić komfort użytkowników.
Przegląd [zasoby systemu Android](~/android/app-fundamentals/resources-in-android/index.md) dokumentacji (w szczególności [tworzenia zasobów dla różnych rozmiarów ekranu](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) Aby uzyskać więcej informacji na temat struktury folderów i plików aplikacji dla systemu Android Projekt, aby uwzględnić zasoby zoptymalizowany obraz w aplikacji.

### <a name="windows-tablets-and-desktops"></a>Windows, tablety i komputery stacjonarne

Aby zapewnić obsługę, tablety i komputery stacjonarne z systemem Windows, musisz użyć [Podpora Windows UWP](~/xamarin-forms/platform/windows/installation/index.md), które kompilacje aplikacji uniwersalnych działających w systemie Windows 10.

Aplikacje działające na Windows tablety i komputery stacjonarne można zmienić rozmiar do dowolnego wymiarów dodatkowo do pracy pełnego ekranu.

[![](tablet-images/splitscreen-sml.png "Windows podziału ekranu przykładu")](tablet-images/splitscreen.png#lightbox "Windows podziału ekranu przykładu")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Optymalizacja dla tabletów i pulpitu

Możesz dostosować interfejs użytkownika platformy Xamarin.Forms w zależności od czy telefon lub tablet/komputer z systemem, jest używany. Oznacza to, że można optymalizować środowisko użytkownika dla urządzeń z dużym ekranie takich jak tablety i komputery stacjonarne.


### <a name="deviceidiom"></a>Device.Idiom

Możesz użyć [ `Device` ](~/xamarin-forms/platform/device.md) klasy, aby zmieniać zachowanie interfejsu aplikacji lub użytkownika. Za pomocą `Device.Idiom` wyliczenie możesz

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

To podejście można rozszerzyć przed wprowadzeniem znaczących zmian do poszczególnych stron układów, a nawet renderowania stron zupełnie innego na większych monitorach.

### <a name="leveraging-masterdetailpage"></a>Wykorzystując MasterDetailPage

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Jest idealnym rozwiązaniem dla większych ekranów, zwłaszcza na urządzeniu iPad, których używa [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) oferują środowisko natywne dla systemu iOS.

Przegląd [ten wpis w blogu Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) aby zobaczyć, jak dostosować interfejs użytkownika, aby telefony użyj jeden układ i większych ekranach można użyć innego (przy użyciu `MasterDetailPage`).



## <a name="related-links"></a>Linki pokrewne

- [Blog platformy Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [Przykładowe MyShoppe](https://github.com/jamesmontemagno/myshoppe)
