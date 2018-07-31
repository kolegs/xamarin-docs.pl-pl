---
title: System iOS 9 zgodności
description: Nawet jeśli nie planujesz razu Dodaj funkcje systemu iOS 9 do swojej aplikacji, należy ponownie skompilować swoje aplikacje za pomocą najnowszej wersji programu Xamarin.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 2f22fdeaad1b276bb94d2b1ee5af4a6c24d22cb7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350784"
---
# <a name="ios-9-compatibility"></a>System iOS 9 zgodności

_Nawet jeśli nie planujesz razu Dodaj funkcje systemu iOS 9 do swojej aplikacji, należy ponownie skompilować swoje aplikacje za pomocą najnowszej wersji programu Xamarin._

> [!IMPORTANT]
> Informacje na tej stronie są przeznaczone dla klientów z aplikacjami już w Store aplikacji przeznaczonych dla systemu iOS 8 lub wcześniej, który nie przesłano już aktualizacje dla zgodności z systemem iOS 9. Jeśli już używasz najnowszej wersji — Xcode 7 i platformy Xamarin.iOS 9 — tworzenie aplikacji odwiedź [wprowadzenie do systemu iOS 9](~/ios/platform/introduction-to-ios9/index.md).

Pojawił się pierwsze wersje beta systemu iOS 9, firma Microsoft ustaliła dwa problemy ze starszymi wersjami programu Xamarin, która dyskowe widoczne jako starsze aplikacje, które są nie można uruchomić w systemie iOS 9.

Te dwa problemy (jako [szczegółowo opisywane na nasze fora](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) zostały:

- Aplikacje kompilacji dla systemu iOS 8 lub starszym, nie będzie mogła uruchomić na urządzeniach 32-bitowych (w tym aplikacje utworzone za pomocą [ujednoliconego interfejsu API](~/cross-platform/macios/unified/index.md)).
- Nie określono P/Invoke kończy się niepowodzeniem z pełną ścieżką.

Aktualizowanie instalacji Xamarin do najnowszej wersji kanału stabilne i następnie ponownego tworzenia i wdrażania aplikacji, rozwiązuje te dwa problemy.

_Nawet jeśli nie planujesz go natychmiast zaktualizować aplikację za pomocą funkcji systemu iOS 9, zaleca się stopniowo ponownie utwórz za pomocą najnowszej wersji programu Xamarin i ponownie prześlij, aby Store App_.



Pozwoli to zagwarantować, że aplikacja zostanie uruchomiona w systemie iOS 9, po uaktualnieniu swoich klientów.
Mogą być nadal obsługiwać system iOS 8 - odbudowywania z najnowszą wersją nie wpływa na docelową wersję aplikacji.

Jeśli masz inne problemy podczas testowania istniejących aplikacji w systemie iOS 9, zapoznaj się z [zwiększanie zgodności](#compat) poniższej sekcji.


### <a name="updating-with-visual-studio"></a>Aktualizowanie za pomocą programu Visual Studio

Zalecane jest jawne sprawdzenie, czy że Visual Studio jest zaktualizowane do najnowszej stabilnej wersji.

## <a name="what-about-components-nugets-and-other-libraries"></a>Co o składnikach, rozszerzeń Nuget i inne biblioteki?

Możesz zrobić **nie** konieczne odczekanie aż nowe wersje składników ani rozszerzeń Nuget, używane są dwa rozwiązywania problemów wymienione powyżej.
Po prostu tworząc ponownie swoją aplikację przy użyciu najnowszej wersji stabilne platformy Xamarin.iOS rozwiązaniu tych problemów.

Podobnie, dostawców składników i autorzy Nuget są **nie** wymagane, aby przesłać nowe kompilacje tak, aby rozwiązać dwa wymienione powyżej problemy. Jednak ewentualne składnika lub Nuget używa `UICollectionView` lub widoki z obciążenia **Xib** pliki aktualizacji *może* być wymagane, aby rozwiązać problemy ze zgodnością z systemem iOS 9, które są wymienione poniżej.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Poprawa zgodności w kodzie

Istnieje kilka przypadków kod wzorców, które *używane* pracę w starszej wersji systemu iOS istotne w systemie iOS 9. Oto niektóre potencjalne problemy (i ich rozwiązania), mogą wystąpić podczas testowania w systemie iOS 9:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView ma wartość null w konstruktorach

**Przyczyna:** w systemie iOS 9 `initWithFrame:` Konstruktor jest już wymagane, ze względu na zmiany zachowania w systemie iOS 9 jako [stany dokumentacji UICollectionView](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Jeśli zarejestrowano klasę dla określonego identyfikatora, należy utworzyć nową komórkę komórki teraz jest inicjowany przez wywołanie jego `initWithFrame:` metody.

**Poprawka:** Dodaj `initWithFrame:` konstruktora w następujący sposób:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Powiązane przykłady: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView nie powiedzie się init za pomocą koder podczas ładowania widoku z Xib/Nib

**Przyczyna:** `initWithCoder:` Konstruktor jest wywoływana podczas ładowania widoku z pliku Xib konstruktora interfejsu. Jeśli ten konstruktor nie jest eksportowane niezarządzanego kodu nie można wywołać nasz zarządzanej wersji. Wcześniej (np.) w systemie iOS 8) `IntPtr` Konstruktor wywołano zainicjować widoku.

**Poprawka:** tworzenie i eksportowanie `initWithCoder:` konstruktora w następujący sposób:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Powiązane próbki: [rozmowy](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Komunikat o błędzie Dyld: Brak pamięci podręcznej obrazu o nazwie...

Może wystąpić awaria, używając następujących informacji w dzienniku:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Przyczyna:** to usterka w konsolidatorze natywnego firmy Apple, które odbywa się w momencie prywatny framework publiczny (JavaScriptCore dokonano publicznych w systemie iOS 7, wcześniej był prywatny framework), a cel wdrożenia aplikacji jest przeznaczony dla wersji dla systemu iOS podczas Framework jest prywatny. W tym przypadku konsolidatora firmy Apple połączy się z wersją prywatny framework zamiast wersji publicznej.

**Poprawka:** ten problem zostanie rozwiązany dla systemu iOS 9, ale nie jest dostępne obejście łatwo możesz stosować samodzielnie w międzyczasie: po prostu docelowe nowszej wersji systemu iOS w projekcie (możesz wypróbować system iOS 7 w tym przypadku). Inne struktury mogą wykazywać podobne problemy, na przykład framework aparatu WebKit została wprowadzona w publicznych w systemie iOS 8 (i tak przeznaczonych dla systemu iOS 7 spowoduje to błąd; docelowych z systemem iOS 8 można używać aparatu WebKit w aplikacji).



## <a name="related-links"></a>Linki pokrewne

- [informacje o wersji zgodności z systemem iOS 9](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Wprowadzenie do systemu iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [Aktualizowanie aplikacji platformy Xamarin.iOS do iOS9 (wideo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
