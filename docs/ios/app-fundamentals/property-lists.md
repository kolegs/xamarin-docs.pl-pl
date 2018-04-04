---
title: Praca z listy właściwości
description: Tym dokumencie przedstawiono Visual Studio dla edytora listy (.plist) graficznego i zaawansowane właściwości dla komputerów Mac do pracy z Info.plist i Entitlements.plist. Przedstawia on ustawienie ikon i uruchamianie obrazów w aplikacjach systemu iOS z w programie Visual Studio dla komputerów Mac.
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f6ee7a606243f5d21d827546b528ca5d9d3f0281
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-property-lists"></a>Praca z listy właściwości

_Tym dokumencie przedstawiono Visual Studio dla edytora listy (.plist) graficznego i zaawansowane właściwości dla komputerów Mac do pracy z Info.plist i Entitlements.plist. Przedstawia on ustawienie ikon i uruchamianie obrazów w aplikacjach systemu iOS z w programie Visual Studio dla komputerów Mac._

Visual Studio for Mac funkcjami edytorze graficznego .plist, który umożliwia edytowanie właściwości aplikacji i łatwiejsze możliwości. Visual Studio for Mac ma dwa .plists - `Info.plist` do edycji właściwości aplikacji i ikony, i `Entitlements.plist` zarządzania możliwości aplikacji. W tym przewodniku przedstawiono Info.plists i zawiera omówienie pracy z nimi w programie Visual Studio dla komputerów Mac. Aby uzyskać informacje na Entitlements.plist, zobacz [Praca z uprawnieniami](~/ios/deploy-test/provisioning/entitlements.md) przewodnik.

## <a name="infoplist"></a>Info.plist

Lista właściwości informacji ( `Info.plist`) jest plikiem z systemem iOS, który zawiera informacje o konfiguracji aplikacji do systemu. Visual Studio dla komputerów Mac do niestandardowych `Info.plist` funkcje edycji trzy panele kontrolowane przez kart na dole po lewej okna edytora:

 [![](property-lists-images/tabs.png "Left Info.plist Edytor karty u dołu okna edytora")](property-lists-images/tabs.png#lightbox)

Każdy panel kontrolki inne właściwości przedstawione poniżej:

-  **Panel aplikacji** -interfejsu graficznego, aby ustawić wspólne właściwości aplikacji, a także ikony, a następnie uruchom obrazy; Określ integracji mapy i backgrounding trybów.
-  **Zaawansowane panelu** -panelu Zaawansowane jest miejscem, aby określić typy obsługiwanych dokumentów, UTIs oraz typy adresu URL.
-  **Źródło panelu** -kontrolki panelu Źródło mniej typowe właściwości, jak również właściwości niestandardowe dla aplikacji.


Trzech kolejnych sekcjach zbadać funkcje każdego panelu bardziej szczegółowo.

## <a name="application-panel"></a>Panel aplikacji

Interfejs graficzny do edycji typowe funkcje programu Visual Studio for Mac `Info.plist` wpisy dla aplikacji:

1.  Właściwości aplikacji
1.  Typy obsługiwanych urządzeń.
1.  Obsługuje orientacje dla każdego typu urządzenia
1.  Stan paska stylu i koloru
1.  Ikon i ekranów rozruchu
1.  Tryby tła i map


Te ustawienia są opisane bardziej szczegółowo w kolejnych sekcjach.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>Docelowy aplikacji systemu iOS

Ta sekcja zawiera ważne informacje, które zawiera opis aplikacji.
**Identyfikator** przechowywane w tym miejscu musi odpowiadać identyfikator pakietu, który jest wprowadzana w iTunes Connect (dla aplikacji sklepu App Store), a także na liście inicjowania obsługi administracyjnej identyfikatorów aplikacji portalu systemu iOS i rozwoju i dystrybucji certyfikatów.

 [![](property-lists-images/image24.png "Docelowy aplikacji systemu iOS")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Wdrażanie urządzenia

 [![](property-lists-images/deployment.png "Wdrażanie urządzenia")](property-lists-images/deployment.png#lightbox)

Urządzenie **wdrożenia** sekcje informacje są wyświetlane selektywnie, w zależności od opcji wybranej w **urządzeń** listy rozwijanej w **aplikacji docelowej** powyższej sekcji. **Interfejsu Main** ma ustawioną wartość listy rozwijanej **MainStoryboard** w aplikacjach opartych na scenorysu. Jeśli interfejs użytkownika jest całkowicie zapisywane w kodzie, to może być pusty.

### <a name="supported-device-orientations"></a>Obsługiwane urządzenia orientacji

 **Obsługiwane urządzenia orientacje** kontroluje sposób odpowiadania przez aplikację na obracanie urządzeń. Bardzo często iPhone/iPad aplikacje do obsługi tylko **pionowa**, lub wszystko, ale **odwrócony**. Ogólnie wszystkie aplikacje iPad, z wyjątkiem gry powinien obsługiwać wszystkie orientacje.

### <a name="status-bar-styles"></a>Style paska stanu

**Style paska stanu** sekcja jest interfejsem graficznym do edycji aplikacji `UIStatusBarStyle`:

 [![](property-lists-images/status.png "Style paska stanu")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Ikony, uruchom obrazów i iTunes kompozycji

Informacji na temat używania ikony, obrazy i kompozycji w pliku Info.plist znajdują się w [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) przewodnik.




### <a name="maps-integration-and-background-modes"></a>Integracja map i tryby tła

`Info.plist` Zawiera specjalne sekcjach, aby określić integracji mapy i backgrounding trybów. Wybranie opcji, które mają być obsługiwane dodać wymagane właściwości do aplikacji za Ciebie.

 [![](property-lists-images/maps.png "Integracja mapy")](property-lists-images/maps.png#lightbox)

Aby uzyskać więcej informacji na temat pracy z mapy, zapoznaj się Xamarin [mapy iOS](~/ios/user-interface/controls/ios-maps/index.md) przewodnik.

 [![](property-lists-images/bging.png "Tryby tła")](property-lists-images/bging.png#lightbox)

Aby uzyskać więcej informacji na trybów tła dotyczą Xamarin [Backgrounding w systemie iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) przewodnik.

## <a name="advanced-panel"></a>Zaawansowane panelu

Zaawansowane panelu kontroluje typów dokumentów i schematy adresów URL, obsługiwanych przez aplikację.

 [![](property-lists-images/image34.png "Zaawansowane panelu")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>Typy dokumentów

W przypadku aplikacji, które obsługuje otwierania określonych typów plików, zapewnia iOS `CFBundleDocumentTypes` klucza. Jeśli chcemy naszej aplikacji w celu obsługi niektórych znanych typów — na przykład pliki PDF - dodamy wartość PDF do klucza. Ta sekcja zawiera wygodny sposób wprowadzania danych, które będą przechowywane w `CFBundleDocumentTypes` klucza w `Info.plist` pliku.

Zapoznaj się z dokumentacją na [rejestrowanie pliku typy Your App obsługuje](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) szczegółowe informacje dotyczące sposobu konfigurowania tych wartości.

## <a name="utis"></a>UTIs

Czasami aplikacja musi obsługiwać otwarcia niestandardowy typ pliku. Na przykład chcemy może otworzyć plików obrazów z niestandardowego rozszerzenia *.xam*. Aby określić niestandardowy typ pliku, utworzymy niestandardowych przy użyciu UTI - uniwersalnego identyfikatora typu - `UIExportedTypeDeclarations` klucza. Poniższy zrzut ekranu przedstawia sposób tworzenia niestandardowych UTI rozszerzenia .xam:

 [![](property-lists-images/uti.png "Edytor UTIs")](property-lists-images/uti.png#lightbox)

Podobnie jak wyeksportowanego typu UTIs określić niestandardowe UTIs specyficzne dla aplikacji, *importowany typ UTIs* ( `UIImportedTypeDeclarations` klucza) określić niestandardowe typy obsługiwane, ale nie należy do Twojej aplikacji.

Aby uzyskać więcej informacji na temat używania niestandardowych UTIs odwoływać się do firmy Apple [obsługuje aplikacja: Rejestrowanie typów plików Your](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) przewodnik.

## <a name="custom-urls"></a>Niestandardowe adresy URL

Nazwa schematu adresu URL (nazywanych również protocol) jest pierwsza część adresu URL. Na przykład `http://` i `https://` są typowe schematy adresów URL. Istnieje możliwość tworzenia niestandardowych schemat adresu URL aplikacji. Niestandardowe schematy adresów URL są używane do komunikacji i przesyłania danych i z powrotem z innymi aplikacjami. Poniższy zrzut ekranu przedstawia tworzenie nowego adresu URL schemat niestandardowy o nazwie `monkeys://`:

 [![](property-lists-images/url.png "Niestandardowe adresy URL")](property-lists-images/url.png#lightbox)



Aby uzyskać więcej informacji na implementacji niestandardowych Schematy adresów URL odwoływać się do firmy Apple [Implementowanie niestandardowych Schematy adresów URL sekcji tego przewodnika](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>Panel źródło

**Źródła** karcie `Info.plist` pliku umożliwia wartości niestandardowych w celu dodania lub edytować. Visual Studio for Mac zawiera listę typowych właściwości:

 [![](property-lists-images/image31.png "Dodawanie nowej właściwości z listy rozwijanej")](property-lists-images/image31.png#lightbox)

Znane właściwości programu Visual Studio for Mac będzie listę prawidłowych wartości, jak pokazano na poniższym zrzucie ekranu:

 [![](property-lists-images/image32.png "Wybierz wartość z listy wartości wiedzieć")](property-lists-images/image32.png#lightbox)

Visual Studio for Mac wykrywa również typ właściwości, jak pokazano:

 [![](property-lists-images/image33.png "Dostępne typy właściwości")](property-lists-images/image33.png#lightbox)

Przejrzyj firmy Apple [powiązane zasoby aplikacji](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) linki do dodatkowych informacji na temat właściwości opcjonalne.

 <a name="Entitlements" />

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono przy użyciu edytory graficzne i zaawansowane .plist do edycji typowych konfiguracji aplikacji oraz określić ikon i uruchamianie obrazów. On również wprowadzone `Entitlements.plist` Dodawanie i zarządzanie możliwości aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [IDE](https://developer.xamarin.com/recipes/cross-platform/ide)
- [Zasoby dotyczące aplikacji](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Rejestrowanie pliku typy obsługuje Twojej aplikacji](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Wdrażanie systemów niestandardowy adres URL](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [Katalogi zasobów — informacje](https://developer.apple.com/library/ioshttps://developer.xamarin.com/recipes/xcode_help-image_catalog-1.0/Recipe.html)
