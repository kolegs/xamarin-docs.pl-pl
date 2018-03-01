---
title: GridViewPager
ms.topic: article
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: f156d802d807c4087331ca5796b046f8f5f2fa0d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="gridviewpager"></a>GridViewPager

[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/) przykładowych pokazano, jak implementują wzorzec nawigacji 2D selektora dla systemu Android zużycia.

![Zrzut ekranu z GridViewPager na kwadratowym wyświetlanej](gridviewpager-images/gridviewpager.png)

Najpierw dodaj [Obsługa platformy Xamarin Android nosić](http://www.nuget.org/packages/Xamarin.Android.Wear/) pakiet NuGet do projektu.

Układ XML wygląda następująco:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Utwórz [ `GridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html) (lub podklasa klasy, takie jak [ `FragmentGridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html) podać widoków do wyświetlenia jako użytkownik nawiguje.

[Karty próbki](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) przedstawia sposób wdrożenia wymagane metod, w tym zastąpienia `RowCount`, `GetColumnCount`, `GetBackground`, i `GetFragment`

Połączenie karty, jak pokazano:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Linki pokrewne

- [2D doc selektora firmy Google](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [android.support.wearable dokumentów](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (przykład)](https://developer.xamarin.com/samples/GridViewPager/)
