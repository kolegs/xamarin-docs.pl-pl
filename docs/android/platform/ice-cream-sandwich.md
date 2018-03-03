---
title: "Funkcje Sandwich lodów"
description: "W tym artykule opisano niektóre z nowych funkcji dostępnych dla deweloperów aplikacji z systemem Android API 4 - Sandwich lodów. Obejmuje kilka nowych technologii interfejsu użytkownika, a następnie sprawdza szereg nowych możliwości dostępnych w systemie Android 4 do udostępniania danych między aplikacjami i między urządzeniami."
ms.topic: article
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 9726ec83cd38dd2f237152f0f8e7373f509a3e01
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="ice-cream-sandwich-features"></a>Funkcje Sandwich lodów

_W tym artykule opisano niektóre z nowych funkcji dostępnych dla deweloperów aplikacji z systemem Android API 4 - Sandwich lodów. Obejmuje kilka nowych technologii interfejsu użytkownika, a następnie sprawdza szereg nowych możliwości dostępnych w systemie Android 4 do udostępniania danych między aplikacjami i między urządzeniami._

## <a name="overview"></a>Omówienie

System operacyjny android w wersji 4.0 (14 poziom interfejsu API) reprezentuje głównych konieczności wprowadzania zmian Android systemu operacyjnego i obejmuje kilka ważnych zmian i uaktualnień, w tym:

-   **Zaktualizowany interfejs użytkownika** — kilka nowych funkcji interfejsu użytkownika dają deweloperom więcej zasilania i interfejsy elastyczność podczas tworzenia aplikacji użytkownika. Te nowe funkcje obejmują: `GridLayout` , `PopupMenu` , `Switch` widget i `TextureView` . 
-   **Lepsze przyspieszanie sprzętowe** — 2D renderowania teraz ma miejsce na procesorze GPU formantów wszystkie systemu Android. Ponadto przyspieszanie sprzętowe jest włączona, domyślnie we wszystkich aplikacjach opracowanych dla systemu Android 4.0. 
-   **Nowe interfejsy API danych** — nowy dostęp do danych, który był wcześniej oficjalnie niedostępny, takich jak dane kalendarza i profilu użytkownika właściciela urządzenia. 
-   **Udostępnianie danych aplikacji** — udostępnianie danych między aplikacjami i urządzeniami jest teraz łatwiejsze niż kiedykolwiek za pomocą technologii takich jak `ShareActionProvider` , który można łatwo utworzyć akcję udostępniania pasku akcji i *Android światła* Aby uzyskać *niemal komunikacji Zbliżeniowej (NFC)* , co czyni go przystawki udostępniania danych między urządzeniami w pobliżu ze sobą. 


W tym artykule chcemy poznać te funkcje i inne zmiany, które zostały wprowadzone do interfejsu API systemu Android 4.0 i wyjaśniamy sposób korzystania z platformy Xamarin.Android każdej funkcji.

## <a name="user-interface-features"></a>Funkcje interfejsu użytkownika

Szereg nowych technologii interfejsu użytkownika są dostępne z systemem Android 4, w tym:

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  — obsługuje 2D siatki układu kontrolek. 
-   **[Przełącz widget](~/android/user-interface/controls/switch.md)**  — umożliwia przełączanie ON lub OFF. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  — umożliwia wideo i OpenGL zawartości w widoku. 
-   **[Pasek nawigacyjny](~/android/user-interface/controls/navigation-bar.md)**  — zawiera wirtualnego przyciski Wstecz, w domu i wielozadaniowych. 


Ponadto inne elementy interfejsu użytkownika zostały rozszerzone, takie jak `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, jest teraz łatwiejsze do pracy z oraz kart, które mają bardziej profesjonalny wygląd.

## <a name="sharing-features"></a>Udostępnianie funkcji

Android 4 zawiera kilka nowych technologii pozwalających nam udostępniać dane między urządzeniami i w aplikacjach. Umożliwia także dostęp do różnych typów danych, które nie były wcześniej dostępne, takie jak informacje kalendarza i właściciel urządzenia, profil użytkownika. W tej sekcji zajmiemy się różne funkcje oferowane przez systemu Android 4 tego adresu tych obszarów, w tym:

-  **[Android światła](~/android/platform/android-beam.md)**  — danych umożliwia współużytkowanie za pośrednictwem NFC.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  — tworzy dostawcę, który umożliwia deweloperom Określ akcje udostępniania na pasku akcji. 
-   **[Profil użytkownika](~/android/user-interface/user-profile.md)**  — zapewnia dostęp do danych profilu właściciela urządzenia. 
-   **[Interfejs API kalendarza](~/android/user-interface/controls/calendar.md)**  — zapewnia dostęp do kalendarza danych od dostawcy kalendarza. 

## <a name="x86-emulators"></a>x86 emulatory

Usługi Udostępnianie połączenia internetowego jeszcze nie obsługuje programowanie z x86 emulatora. x86 emulatory są obsługiwane tylko z systemem Android 2.3.3, interfejs API na poziomie 10. Zobacz [Konfigurowanie x86 emulatora](~/android/get-started/installation/android-emulator/index.md) Aby uzyskać więcej informacji.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano szereg nowych technologii, które są teraz dostępne w systemie Android 4. Nowe funkcje interfejsu użytkownika firma Microsoft przejrzeć takie jak *GridLayout*, *elementu PopupMenu*, i *przełącznika* elementu widget. Również analizujemy niektóre nowa funkcja obsługi kontrolowanie systemu interfejsu użytkownika, oraz do pracy z *TextureView*. Następnie omówiono szereg nowych technologii udostępniania. Firma Microsoft objęte jak *Android światła* umożliwia udostępnianie informacji między urządzeniami, które używają *NFC*, opisano nowe *API kalendarza*i również pokazano, jak użyć wbudowanych w  *ShareActionProvider*.
Na koniec mamy się zbadana sposób użycia *ContactsContract* dostawcy, aby dane profilu użytkownika dostępu.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady Sandwich lodów](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (przykład)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (przykład)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [Samouczek układu kartę](~/android/user-interface/layouts/tab-layout/index.md)
- [Sandwich lodów](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Platformy systemu android 4.0](http://developer.android.com/about/versions/android-4.0.html)
