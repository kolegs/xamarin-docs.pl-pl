---
title: Elementów nadrzędnych.
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 29726450e0899177f3223deeade1cb4766d933a5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="actionbar"></a>Elementów nadrzędnych.


## <a name="overview"></a>Omówienie

Korzystając z `TabActivity`, kod w celu utworzenia ikony kartę nie ma wpływu uruchomienia względem framework 4.0 dla systemu Android. Chociaż funkcjonalnie działa tak jak w wersjach systemu android przed 2.3 `TabActivity` samej klasy jest przestarzała w wersji 4.0. Nowy sposób tworzenia z kartami interfejsu został wprowadzony używającej pasku akcji, które omówiono dalej.


## <a name="action-bar-tabs"></a>Karty na pasku akcji

Na pasku akcji obsługuje dodawanie interfejsów z kartami w systemie Android 4.0.
Poniższy zrzut ekranu przedstawia przykład takiego interfejsu.

[![Zrzut ekranu przedstawiający aplikacji uruchomionej w emulatorze; dwie karty są wyświetlane.](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

Aby utworzyć karty na pasku akcji, najpierw musimy ustawić jej `NavigationMode` właściwości do obsługi kart. W systemie Android 4 `ActionBar` właściwość jest dostępna w klasie działania, które firma Microsoft można użyć do skonfigurowania `NavigationMode` podobnie do następującej:

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

Po zakończeniu kartę można utworzyć przez wywołanie metody `NewTab` metody na pasku akcji. Z tym wystąpieniem kartę można nazywamy `SetText` i `SetIcon` metod można ustawić na karcie tekst etykiety i ikony; tych wywołań w kolejności, w poniższym kodzie:

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

Zanim jednak możemy dodać kartę, trzeba obsłużyć `TabSelected` zdarzeń. W tym obsługi można utworzyć zawartości karty. Karty na pasku akcji są zaprojektowane do pracy z *fragmenty*, które są klasy, które stanowią część interfejsu użytkownika w działaniu. W tym przykładzie widok fragmentu zawiera jeden `TextView`, którego możemy zwiększyć w naszym `Fragment` podklasy następująco:

```csharp
class SampleTabFragment: Fragment
{           
    public override View OnCreateView (LayoutInflater inflater,
        ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreateView (inflater, container, savedInstanceState);
       
        var view = inflater.Inflate (
            Resource.Layout.Tab, container, false);

        var sampleTextView =
            view.FindViewById<TextView> (Resource.Id.sampleTextView);            
        sampleTextView.Text = "sample fragment text";


        return view;
    }
}
```

Przekazano argument zdarzenia `TabSelected` zdarzenie jest typu `TabEventArgs`, która obejmuje `FragmentTransaction` właściwości, które możemy użyć, aby dodać fragment, jak pokazano poniżej:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

Na koniec można dodać karty na pasku akcji przez wywołanie metody `AddTab` — metoda, jak pokazano w tym kodzie:

```csharp
this.ActionBar.AddTab (tab);
```

Aby uzyskać pełny przykład, zobacz *HelloTabsICS* projektu w przykładowym kodzie dla tego dokumentu.


## <a name="shareactionprovider"></a>ShareActionProvider

`ShareActionProvider` Klasa umożliwia udostępnianie akcja została wykonana z paska akcji. Odpowiada on za tworzenie widoku akcji listę aplikacji, które może obsłużyć zamiar udostępniania i przechowuje historię poprzednio używanych aplikacji łatwy dostęp do nich później na pasku akcji. Dzięki temu aplikacje mogą udostępniać dane za pomocą środowiska użytkownika, który jest spójny we Android.


### <a name="image-sharing-example"></a>Przykład udostępnianie obrazu

Na przykład poniżej przedstawiono zrzut ekranu na pasku akcji z elementem menu Udostępnianie obrazu (pobierane z [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/) próbka). Po naciśnięciu element menu na pasku akcji ShareActionProvider ładuje aplikację do obsługi skojarzonego z zamiarem `ShareActionProvider`. W tym przykładzie komunikatów aplikacji został wcześniej użyty, więc znajduje się na pasku akcji.

[![Zrzut ekranu przedstawiający ikonę aplikacji na pasku akcji do obsługi komunikatów](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


Po kliknięciu elementu w pasku akcji, jest uruchamiana aplikacja obsługi wiadomości, która zawiera udostępniany obraz, jak pokazano poniżej:

[![Zrzut ekranu przedstawiający komunikatów aplikacji wyświetlanie małp obrazu](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>Określenie akcji klasy dostawcy

Aby użyć `ShareActionProvider`ustaw `android:actionProviderClass` atrybutu element menu w pliku XML na pasku akcji menu w następujący sposób:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>Pompowania Menu

Aby zwiększyć menu, możemy zastąpić `OnCreateOptionsMenu` w podklasę działania. Po mamy odwołanie do menu można pobrać `ShareActionProvider` z `ActionProvider` właściwości elementu menu, a następnie użycie metody SetShareIntent, aby ustawić `ShareActionProvider`jego celem, jak pokazano poniżej:

```csharp
public override bool OnCreateOptionsMenu (IMenu menu)
{
    MenuInflater.Inflate (Resource.Menu.ActionBarMenu, menu);       
           
    var shareMenuItem = menu.FindItem (Resource.Id.shareMenuItem);           
    var shareActionProvider =
       (ShareActionProvider)shareMenuItem.ActionProvider;
    shareActionProvider.SetShareIntent (CreateIntent ());
}
```


### <a name="creating-the-intent"></a>Tworzenie celem

`ShareActionProvider` Użyje zamiar przekazany do `SetShareIntent` metody w kodzie powyżej, aby uruchomić odpowiednie działanie. W takim przypadku utworzymy celem wysyłania obrazu przy użyciu następującego kodu:

```csharp
Intent CreateIntent ()
{  
    var sendPictureIntent = new Intent (Intent.ActionSend);
    sendPictureIntent.SetType ("image/*");
    var uri = Android.Net.Uri.FromFile (GetFileStreamPath ("monkey.png"));          
    sendPictureIntent.PutExtra (Intent.ExtraStream, uri);
    return sendPictureIntent;
}
```

Obraz w powyższym przykładzie kodu jest dołączany jako zasób w aplikacji i kopiowane do lokalizacji publicznie po utworzeniu działania, będzie ona dostępna dla innych aplikacji, takich jak aplikacji obsługi wiadomości. Przykładowy kod, który towarzyszy w tym artykule zawiera źródło pełny przykład pokazujący jej użycia.



## <a name="related-links"></a>Linki pokrewne

- [Witaj karty ICS (przykład)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [Wersja demonstracyjna ShareActionProvider (przykład)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [Wprowadzenie Sandwich lodów](http://www.android.com/about/ice-cream-sandwich/)
- [Platformy systemu android 4.0](http://developer.android.com/sdk/android-4.0.html)
