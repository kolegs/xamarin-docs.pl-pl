---
title: Praca z listy właściwości w rozszerzeniu Xamarin.iOS
description: Ten dokument stanowi wprowadzenie programu Visual Studio dla komputerów Mac graficznego i zaawansowane właściwości listy (.plist) edytora do pracy z pliku Info.plist i plik Entitlements.plist. Zawiera ono Ustawienia ikon i obrazów uruchamiania dla aplikacji systemu iOS z poziomu programu Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b102f268fd457ca42f3d64b5766a2b3824e3849d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242332"
---
# <a name="working-with-property-lists-in-xamarinios"></a>Praca z listy właściwości w rozszerzeniu Xamarin.iOS

_Ten dokument stanowi wprowadzenie programu Visual Studio dla komputerów Mac graficznego i zaawansowane właściwości listy (.plist) edytora do pracy z pliku Info.plist i plik Entitlements.plist. Zawiera ono Ustawienia ikon i obrazów uruchamiania dla aplikacji systemu iOS z poziomu programu Visual Studio dla komputerów Mac._

Program Visual Studio for Mac zawiera edytor graficzny .plist, który sprawia, że edytowanie właściwości aplikacji i możliwości łatwiejsze. Program Visual Studio dla komputerów Mac zawiera dwa .plists — `Info.plist` do edycji właściwości aplikacji i ikon, a `Entitlements.plist` do zarządzania funkcjami aplikacji. Ten przewodnik przedstawia Info.plists i zawiera omówienie pracy z nimi w programie Visual Studio dla komputerów Mac. Aby uzyskać informacje na plik Entitlements.plist, zobacz [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

## <a name="infoplist"></a>Info.plist

Lista właściwości informacji ( `Info.plist`) jest plikiem z systemem iOS, który dostarcza informacje o konfiguracji aplikacji do systemu. Visual Studio dla komputerów Mac w niestandardowych `Info.plist` funkcje edytora trzy panele kontrolowane przez karty u dołu po lewej części okna edytora:

 [![](property-lists-images/tabs.png "Info.plist Edytor karty u dołu po lewej części okna edytora")](property-lists-images/tabs.png#lightbox)

Każdy panel kontrolki różnych właściwości, tak jak pokazano poniżej:

-  **Aplikacja Panel** — interfejs graficzny umożliwiający Ustawianie wspólnych właściwości aplikacji, a także ikon i uruchamianie obrazów; określ uprawnienie maps integration i uruchamianie procesów w tle tryby.
-  **Zaawansowane panelu** -panelu Zaawansowane jest miejscem, aby określić typy obsługiwanych dokumentów, identyfikatory Uti oraz typy adresów URL.
-  **Źródło panelu** — panel Źródło kontroluje mniej typowe właściwości, a także niestandardowe właściwości aplikacji.


Trzech kolejnych sekcjach Zbadaj funkcje każdego panelu bardziej szczegółowo.

## <a name="application-panel"></a>Panel aplikacji

Program Visual Studio for Mac funkcji interfejsu graficznego służącego do edytowania wspólnej `Info.plist` wpisy dla aplikacji:

1.  Właściwości aplikacji
1.  Typy obsługiwanych urządzeń.
1.  Orientacje pomocy technicznej dla każdego typu urządzenia
1.  Stan paska stylu i koloru
1.  Ikony i ekrany uruchamiania
1.  Mapy i tryby w tle


Te ustawienia są opisane bardziej szczegółowo w następnych sekcjach.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>Cel aplikacji systemu iOS

Ta sekcja zawiera ważne informacje opisujące aplikację.
**Identyfikator** przechowywane w tym miejscu musi odpowiadać identyfikator pakietu, który jest wprowadzana w usłudze iTunes Connect (w przypadku aplikacji App Store), a także tworzenia i dystrybucji certyfikaty oraz listy inicjowania obsługi administracyjnej identyfikatory aplikacji Portal dla systemu iOS.

 [![](property-lists-images/image24.png "Cel aplikacji systemu iOS")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Wdrażanie urządzeń

 [![](property-lists-images/deployment.png "Wdrażanie urządzeń")](property-lists-images/deployment.png#lightbox)

Urządzenie **wdrożenia** sekcje informacje są wyświetlane selektywnie, w zależności od opcji wybranej w **urządzeń** liście rozwijanej **cel aplikacji** powyższej sekcji. **Interfejsu Main** listy rozwijanej jest równa **MainStoryboard** w aplikacjach opartych na scenorysu. Jeśli interfejs użytkownika całkowite są zapisywane w kodzie następnie to może być puste.

### <a name="supported-device-orientations"></a>Obsługiwane orientacje urządzenia

 **Obsługiwane orientacje urządzenia** kontroluje sposób aplikacja reaguje na obracanie urządzenia. Jest ono często w przypadku aplikacji dla telefonu iPhone/iPad do obsługi tylko **pionowa**, lub wszystko, ale **nogami**. Ogólnie wszystkie aplikacje dla urządzenia iPad, z wyjątkiem gry powinien obsługiwać wszystkie orientacje.

### <a name="status-bar-styles"></a>Style paska stanu

**Style paska stanu** interfejsu graficznego dla aplikacji do edycji jest sekcja `UIStatusBarStyle`:

 [![](property-lists-images/status.png "Style paska stanu")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Ikony, obrazy uruchomieniowe i kompozycji w programie iTunes

Informacje na temat korzystania z ikon, obrazów i grafiki w pliku Info.plist można znaleźć w [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) przewodnik.




### <a name="maps-integration-and-background-modes"></a>Uprawnienie Maps Integration i tryby w tle

`Info.plist` Zawiera specjalne sekcje, aby określić uprawnienie maps integration i uruchamianie procesów w tle tryby. Wybierając opcje, które mają być obsługiwane dodać wymagane właściwości do aplikacji za Ciebie.

 [![](property-lists-images/maps.png "Uprawnienie Maps Integration")](property-lists-images/maps.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z usługą mapy dotyczą Xamarin [mapy iOS](~/ios/user-interface/controls/ios-maps/index.md) przewodnik.

 [![](property-lists-images/bging.png "Tryby tła")](property-lists-images/bging.png#lightbox)

Aby uzyskać więcej informacji na temat trybów tła, dotyczą Xamarin [uruchamianie procesów w tle w systemie iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) przewodnik.

## <a name="advanced-panel"></a>Panel zaawansowane

Zaawansowane panelu kontroluje typów dokumentów i schematy adresów URL, obsługiwanych przez aplikację.

 [![](property-lists-images/image34.png "Panel zaawansowane")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>Typy dokumentów

W przypadku aplikacji, które obsługują określonych typów plików, otwierając systemu iOS zapewnia `CFBundleDocumentTypes` klucza. Jeśli chcemy, aby nasza aplikacja do obsługi niektórych znanych typów plików — na przykład plików PDF — dodamy wartość PDF do klucza. Ta sekcja zawiera wygodny sposób wprowadź dane, które będą przechowywane w `CFBundleDocumentTypes` w `Info.plist` pliku.

Zapoznaj się z dokumentacją na [rejestrowanie pliku typy Your App obsługuje](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) Aby uzyskać szczegółowe informacje dotyczące sposobu konfigurowania tych wartości.

## <a name="utis"></a>Identyfikatory Uti

Czasami aplikacja musi obsługiwać, otwierając niestandardowy typ pliku. Na przykład warto otwieranie plików obrazów za pomocą niestandardowego rozszerzenia *.xam*. Aby określić niestandardowy typ pliku, utworzymy niestandardowy identyfikator UTI - uniwersalnego identyfikatora typu — przy pomocy `UIExportedTypeDeclarations` klucza. Poniższy zrzut ekranu przedstawia sposób tworzenia niestandardowych identyfikatorów UTI dla rozszerzenia .xam:

 [![](property-lists-images/uti.png "Edytor identyfikatorów Uti")](property-lists-images/uti.png#lightbox)

Po prostu jako wyeksportowane identyfikatory Uti typu określ niestandardowe identyfikatory Uti, które są specyficzne dla aplikacji, *zaimportowane identyfikatory Uti typu* ( `UIImportedTypeDeclarations` klucza) określ niestandardowe typy obsługiwane, ale nie są własnością Twojej aplikacji.

Aby uzyskać więcej informacji na temat korzystania z niestandardowych identyfikatorów Uti odnoszą się do firmy Apple [obsługuje aplikacja: Rejestrowanie typów plików Your](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) przewodnik.

## <a name="custom-urls"></a>Niestandardowe adresy URL

Nazwa schematu adresu URL (nazywane również protocol) jest pierwszą część adresu URL. Na przykład `http://` i `https://` są typowe schematy adresów URL. Masz możliwość tworzenia niestandardowych schemat adresu URL aplikacji. Niestandardowe schematy adresów URL są używane do komunikacji i przesyłania danych i z powrotem z innymi aplikacjami. Poniższy zrzut ekranu przedstawia, tworząc nowy schemat adresu URL niestandardowej o nazwie `monkeys://`:

 [![](property-lists-images/url.png "Niestandardowe adresy URL")](property-lists-images/url.png#lightbox)



Aby uzyskać więcej informacji dotyczących implementowania niestandardowe schematy adresów URL, dotyczą firmy Apple [Implementowanie niestandardowe schematy adresów URL części tego przewodnika](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Panel źródło

**Źródła** karcie `Info.plist` pliku zezwala na można dodawać ani edytować wartości niestandardowych. Program Visual Studio for Mac zawiera listę typowych właściwości:

 [![](property-lists-images/image31.png "Dodawanie nowej właściwości z listy rozwijanej")](property-lists-images/image31.png#lightbox)

Znane właściwości program Visual Studio for Mac obejmuje listę prawidłowych wartości, jak pokazano na poniższym zrzucie ekranu:

 [![](property-lists-images/image32.png "Wybierz wartość z listy wartości wie")](property-lists-images/image32.png#lightbox)

Visual Studio dla komputerów Mac wykrywa także z typem właściwości, jak pokazano:

 [![](property-lists-images/image33.png "Dostępne typy właściwości")](property-lists-images/image33.png#lightbox)

Przejrzyj firmy Apple [powiązane zasoby aplikacji](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) linki do dodatkowych informacji na temat właściwości opcjonalne.

 <a name="Entitlements" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, Edytuj typowe konfiguracje aplikacji również określenie ikony i obrazy uruchamiania przy użyciu edytory graficzne i zaawansowanego pliku plist. On również wprowadzonymi `Entitlements.plist` do dodawania i zarządzania nimi możliwości aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [ŚRODOWISKO IDE](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide)
- [Zasoby związane z aplikacją](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Rejestrowanie pliku typy obsługiwanej przez aplikację](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Implementowanie niestandardowego adresu URL schematów](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Wykaz zasobów dokument referencyjny dotyczący formatowania](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/index.html#//apple_ref/doc/uid/TP40015170-CH18-SW1)
