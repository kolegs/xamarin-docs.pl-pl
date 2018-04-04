---
title: System iOS 9 zgodności
description: Nawet jeśli nie zamierzasz dodać funkcji systemu iOS 9 razu do aplikacji, należy odbudować aplikacjami za pomocą najnowszej wersji programu Xamarin.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b7cbec3f064e6000f991fb0f9ce256415f6ce5dd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="ios-9-compatibility"></a>System iOS 9 zgodności

_Nawet jeśli nie zamierzasz dodać funkcji systemu iOS 9 razu do aplikacji, należy odbudować aplikacjami za pomocą najnowszej wersji programu Xamarin._

> [!IMPORTANT]
> Informacje na tej stronie jest przeznaczona dla klientów z aplikacjami już w magazynie aplikacji przeznaczonych dla systemu iOS 8 lub starszym, który nie zostały już przesłane aktualizacje dla zgodności z systemem iOS 9. Jeśli korzystasz już z najnowszych wersji - Xcode 7 i 9 Xamarin.iOS — na potrzeby programowania aplikacji można znaleźć [wprowadzenie do systemu iOS 9](~/ios/platform/introduction-to-ios9/index.md).

Pojawił się pierwszy wersje beta systemu iOS 9, możemy zidentyfikować dwóch problemów ze starszymi wersjami programu Xamarin, który dyskowe widoczne jako starsze aplikacje nie będą mogli uruchomić na komputerze z systemem iOS 9.

Te dwie kwestie (jako [szczegółowe na naszych forach](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) zostały:

- Tworzenie aplikacji dla systemu iOS 8 lub starszym, nie jest możliwe jej uruchomienie na urządzeniach 32-bitowych (w tym aplikacji skompilowanej za pomocą [Unified API](~/cross-platform/macios/unified/index.md)).
- Nie określono metody P/Invoke niepowodzeniem z powodu pełną ścieżkę.

Aktualizowania instalacji Xamarin do najnowszej wersji kanału stabilny, a następnie odbudowanie i ponownego wdrażania aplikacji, rozwiązuje problemy z tych dwóch.

_Nawet wtedy, gdy nie są planowane natychmiast zaktualizować aplikację z funkcjami systemu iOS 9, zaleca się ponowne utworzenie do najnowszej wersji programu Xamarin i ponownie prześlij go do sklepu z aplikacjami_.



Daje to pewność, że aplikacja zostanie uruchomiona w systemie iOS 9, po uaktualnieniu klientów.
Można kontynuować do obsługi systemu iOS 8 - odbudowywania najnowsza wersja nie wpływa na docelową wersję aplikacji.

Jeśli masz inne problemy podczas testowania istniejące aplikacje w systemie iOS 9, przeczytaj [poprawy zgodności](#compat) poniższej sekcji.


### <a name="updating-with-visual-studio"></a>Aktualizowanie za pomocą programu Visual Studio

Zalecane jest jawnie sprawdzanie że Visual Studio został zaktualizowany do najnowszej wersji stabilnej.

## <a name="what-about-components-nugets-and-other-libraries"></a>Co o składnikach, Nugets i innych bibliotek?

Jak **nie** czekając nowe wersje wszystkich składników lub Nugets używasz adresów dwa wymienione powyżej problemy.
Po prostu przez ponowne kompilowanie aplikacji za pomocą najnowszej wersji stabilnej Xamarin.iOS rozwiązaniu tych problemów.

Podobnie, składnik dostawców i autorów Nuget są **nie** wymagane, aby przesłać nowe kompilacje tak, aby naprawić dwa wymienione powyżej problemy. Jednak jeśli składnika lub Nuget używa `UICollectionView` lub załadować widoki z **Xib** pliki aktualizacji *może* być wymagane w celu rozwiązania problemów ze zgodnością z systemem iOS 9 wymienione poniżej.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Poprawa zgodności w kodzie

Istnieje kilka przykładów kodu wzorców, które *używane* pracę w starszej wersji systemu iOS krytyczne w systemie iOS 9. Poniżej przedstawiono niektóre problemy (i ich rozwiązania) który mogą wystąpić podczas testowania w systemie iOS 9:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView ma wartość null w konstruktorach

**Przyczyna:** w systemie iOS 9 `initWithFrame:` Konstruktor jest już wymagane, z powodu zmian zachowania w systemie iOS 9 jako [UICollectionView dokumentacji Stanów](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Zarejestrowano klasę dla określonego identyfikatora, należy utworzyć nowe komórki komórki teraz jest inicjowany przez wywołanie jego `initWithFrame:` metody.

**Poprawka:** Dodaj `initWithFrame:` konstruktora w następujący sposób:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

Powiązane przykłady: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>UIView nie powiedzie się init z koder podczas ładowania widoku z Xib/Nib

**Przyczyna:** `initWithCoder:` Konstruktor jest wywoływana podczas ładowania widoku z pliku Xib konstruktora interfejsu. Jeśli ten konstruktor nie jest eksportowane kodu niezarządzanego nie można wywołać naszych zarządzanej wersji. Wcześniej (np.) w systemie iOS 8) `IntPtr` konstruktor został wywołany zainicjować widoku.

**Poprawka:** tworzenie i eksportowanie `initWithCoder:` konstruktora w następujący sposób:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

Przykładowe pokrewne: [rozmowy](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Komunikat Dyld: Brak pamięci podręcznej obrazu o nazwie...

Mogą wystąpić awaria następujące informacje w dzienniku:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Przyczyna:** jest to błąd w konsolidatorze natywnego firmy Apple, które odbywa się po wysłaniu prywatnej framework publiczny (JavaScriptCore dokonano publicznych w systemie iOS 7, przed był prywatnej framework), i jest cel wdrożenia aplikacji dla systemu iOS w wersji podczas została prywatnych. W takim przypadku konsolidatora firmy Apple połączy się z prywatnej wersji platformy zamiast wersji publicznej.

**Poprawka:** ten problem zostanie rozwiązany dla systemu iOS 9, ale jest łatwo obejście można zastosować się do tego czasu: tylko target nowszej wersji systemu iOS w projekcie (można spróbować systemów iOS 7 w takim przypadku). Innych platform, może powodować błędy podobne, na przykład WebKit framework został opublikowany w systemie iOS 8 (i dlatego przeznaczonych dla systemów iOS 7 spowoduje to błędu; docelowych z systemem iOS 8 używanie WebKit w aplikacji).



## <a name="related-links"></a>Linki pokrewne

- [informacje o wersji zgodności z systemem iOS 9](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [Wprowadzenie do systemu iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [Aktualizowanie aplikacji platformy Xamarin.iOS do iOS9 (klip wideo)](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
