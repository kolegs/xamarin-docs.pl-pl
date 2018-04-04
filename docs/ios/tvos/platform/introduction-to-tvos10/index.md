---
title: Wprowadzenie do systemu tvOS 10
description: W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemu tvOS 10 dla deweloperów Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: CB9C1EC8-6008-43AD-977E-976AE7C73DD8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 642a851cbfc0450ee8f5f5c4d798c40778e3d3dd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-tvos-10"></a>Wprowadzenie do systemu tvOS 10

_W tym artykule przedstawiono wszystkich nowych i zmodyfikowanych interfejsów API i funkcje dostępne w systemu tvOS 10 dla deweloperów Xamarin.tvOS._

Z nowego systemu tvOS 10 SDK Apple dołączył nowych interfejsów API i usług, które umożliwiają deweloperom tworzenie nowej kategorii aplikacji i funkcji. 

Aby uzyskać więcej informacji dotyczących systemu tvOS 10, zobacz firmy Apple [systemu tvOS + aplikacji](https://developer.apple.com/tvos/) dokumentacji.

## <a name="whats-new-in-tvos-10"></a>Nowości w systemu tvOS 10

Apple został dodany kilka nowych interfejsów API i usługi systemu tvOS 10 wraz z wielu ulepszenia istniejących funkcji, w tym:

## <a name="new-user-interface-styles"></a>Nowe style interfejsu użytkownika

systemu tvOS 10 teraz obsługuje zarówno ciemny, jak i interfejs użytkownika jasny motyw że będą wszystkie UIKit kompilacji w kontroluje automatycznie dostosowania, zgodnie z preferencjami użytkownika.

Podczas tworzenia i wdrażania nowych niestandardowych kontrolek interfejsu użytkownika, należy używać dewelopera [UITraitCollection](https://developer.apple.com/reference/uikit/uitraitcollection) klasy w celu dostosowania do wybranego motywu użytkownika.

Aby uzyskać więcej informacji, zobacz nasze [nowych stylów interfejsu użytkownika](~/ios/tvos/platform/user-interface-styles.md) dokumentacji.

## <a name="security-and-privacy-enhancements"></a>Bezpieczeństwo i prywatność ulepszenia

Apple wprowadziła kilka ulepszeń zarówno prywatności i bezpieczeństwa w systemu tvOS 10 pomoże developer poprawy zabezpieczeń aplikacji i zapewnienia zachowania poufności użytkownika końcowego.

W związku z tym aplikacje działające na watchOS 3 (lub nowszy) statycznie musi deklarować ich próba dostępu do określonych funkcji lub informacje użytkownika, wprowadzając jeden lub więcej określonych kluczy prywatności w ich `Info.plist` pliki, które wyjaśnić użytkownikowi przyczynę aplikacja chce uzyskać dostęp.

Ponieważ systemu tvOS 10 udostępnia te zmiany z systemem iOS 10, zobacz nasze iOS 10 [zabezpieczeń i prywatności rozszerzenia](~/ios/app-fundamentals/security-privacy.md) przewodnika, aby uzyskać więcej informacji.

## <a name="video-subscriber-account"></a>Konto subskrybenta wideo

Nowe dla systemu tvOS 10, w ramach konta subskrybenta wideo umożliwia aplikacjom tej pomocy technicznej uwierzytelniony przesyłania strumieniowego lub wideo na żądanie do uwierzytelniania za pomocą kabla lub satelitarnej dostawcy TV przy użyciu pojedynczej-logowania dla użytkownika końcowego.

<!--To find out more, please see our [Video Subscriber Account](~/ios/platform-features/introduction-to-ios10/video-subscriber-account/) guide.-->

## <a name="wide-color"></a>Kolor międzynarodowe

10 systemu tvOS rozszerza do obsługi formatów pikseli rozszerzony zakres i całej gamy przestrzenie w całym systemie, w tym platform, takich jak Core grafiki, Core obrazu systemu operacyjnego i AVFoundation. Obsługa urządzeń z przedstawia szeroką kolor dalsze jest ułatwiony dostarczając to zachowanie przez cały stos całej grafiki.

Ponadto `UIKit` został zmodyfikowany do pracy w nowym rozszerzony **sRGB** kolorów, ułatwiając mieszać kolorów w one zakresami szeroki kolor bez utraty znaczących wydajności.

Apple oferuje następujące najlepsze rozwiązania podczas pracy z szeroką kolorów:

 - `UIColor` teraz używa sRGB przestrzeń kolorów i nie będą clamp wartości `0.0` do `1.0` zakresu. Jeśli aplikacja opiera się na poprzednie ograniczenie zachowanie, trzeba będzie można zmodyfikować systemu tvOS 10.
 - Jeśli aplikacja przeprowadza niestandardowe renderowanie `UIImages`, używając nowego [UIGraphicsImageRender](https://developer.apple.com/reference/uikit/uigraphicsimagerenderer) klasę, aby określić używanie formatów rozszerzony zakres lub standard-range.
 - Podaj przetwarzania obrazów za pomocą API niskiego poziomu, takich jak grafiki rdzeni lub systemu operacyjnego, aplikacji należy używać rozszerzonej zakresu kolorów miejsca i piksela formacie, który obsługuje wartości zmiennoprzecinkowych 16-bitowych. W przypadku, gdy to konieczne, aplikacja będzie musiał ręcznie clamp wartości składnika kolorów.
 - Podstawowe grafiki, Core obrazu i oferować nowych metod konwersji między dwoma przestrzenie systemu operacyjnego wydajności programów do cieniowania.

Aby dowiedzieć się więcej, zobacz nasze [wprowadzenie do szerokości kolor](~/ios/platform/wide-color.md) przewodnik.

## <a name="newly-available-existing-frameworks"></a>Nowo dostępne istniejących struktur

Wiele platform, które były dostępne w systemach iOS (i nie systemu tvOS) udostępniono dla systemu tvOS 10 takich jak:

 - ExternalAccessory
 - HomeKit
 - MultipeerConnectivity
 - Zdjęć
 - ReplayKit
 - UserNotification

## <a name="additional-framework-changes"></a>Zmiany dodatkowe Framework

Oprócz framework najważniejszych zmian i dodatków wymienionych powyżej Apple wprowadził wiele dodatkowych framework drobne zmiany systemu tvOS 10.

Aby dowiedzieć się więcej, zobacz nasze [dodatkowych zmian Framework](~/ios/tvos/platform/introduction-to-tvos10/additional-framework-changes.md) przewodnik.

## <a name="deprecated-apis"></a>Przestarzałe interfejsy API

Nie interfejsów API lub struktury były przestarzałe przez systemu tvOS 10. Zobacz firmy Apple [różnice interfejsu API systemu tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/tvOS10APIDiffs/index.html) dokumentacji pełną listę modyfikacje interfejsu API.



## <a name="related-links"></a>Linki pokrewne

- [Przykłady dla systemu tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [What's new in systemu tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
