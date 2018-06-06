---
title: Tworzenie interfejsów użytkownika za pomocą projektanta dla systemu iOS
description: Ten dokument zawiera opis sposobu tworzenia interfejsu użytkownika aplikacji z scenorys i .xib plików za pomocą projektanta Xamarin dla systemu iOS. Łączy do dokumentów, omówiono w nim dostępności narzędzia, jego podstawową funkcjonalność, designable formantów, które zapewniają wskazówki dotyczące jego użytkowania.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/31/2018
ms.openlocfilehash: eadc2147a44d6077436e394a4757d367ce42e5fa
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790006"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>Tworzenie interfejsów użytkownika za pomocą projektanta dla systemu iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

=======
# <a name="ios-designer"></a>iOS Designer

_Projektant Xamarin dla systemu iOS jest wizualnego projektanta dla systemu iOS formaty scenorysu i kompilatora interfejsu, który jest w pełni zintegrowana z programem Visual Studio for Mac i Visual Studio. IOS projektanta przechowuje pełną zgodność z formatów scenorysu i .xib, tak, aby pliki mogą być edytowane w Visual Studio for Mac lub Visual Studio oprócz konstruktora interfejsu w środowisku Xcode. Ponadto Projektant Xamarin dla systemu iOS obsługuje zaawansowane funkcje, takie jak Kontrolki niestandardowe, które są renderowane w czasie projektowania w edytorze._
>>>>>>> główne

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![iOS Projektant w programie Visual Studio for Mac](images/designer-vsmac-sml.png "projektanta dla systemu iOS")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Projektant w programie Visual Studio iOS](images/designer-vs.png "projektanta dla systemu iOS")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>Dostępność

Projektant Xamarin dla systemu iOS jest dostępne w programie Visual Studio dla komputerów Mac i Visual Studio 2017 w systemie Windows.

Te przewodniki założono znajomość zawartość omówione w [wprowadzenie Xamarin.iOS prowadzi](~/ios/get-started/index.md).

## <a name="ios-designer-basicsintroductionmd"></a>[Podstawy narzędzia iOS Designer](introduction.md)

Ten przewodnik obejmuje funkcje projektanta Xamarin iOS. Obejmuje ona podstawy projektanta, pokazujący sposób określania układu kontrolek wizualnie za pomocą projektanta i sposób edycji właściwości.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[Informacje o formantach designable](ios-designable-controls-overview.md)

W tym przewodniku wygląda szczegółowo w niestandardowych formantów, jak są tworzone i jakie wymagania muszą spełniać mają być odwzorowane na powierzchnię projektu. Ponadto widoczny jest sposób typowych problemów, które mogą wystąpić, gdy za pomocą formantów Designable debugowania.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[Wskazówki — za pomocą formantów niestandardowych z systemem iOS projektanta](ios-designable-controls-walkthrough.md)

Ten artykuł zawiera przewodnik krok po kroku przedstawiająca sposób utworzyć niestandardowego formantu i użyć go w Projektancie systemu iOS. Widoczny jest sposób udostępnić formantu w przyborniku projektanta, aby można było ją przeciągania/porzucić na widoku. Ponadto zawiera implementowania formantu, aby poprawnie renderowania w czasie projektowania i środowiska uruchomieniowego, jak również sposób tworzenia właściwości, które można ustawić w czasie projektowania.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Automatycznie Rozmieść elementy z Xamarin iOS projektanta](designer-auto-layout.md)

W tym przewodniku przedstawiono iOS automatycznego układu i nowego przepływu pracy ograniczenia, dostępne w Projektancie systemu iOS.
