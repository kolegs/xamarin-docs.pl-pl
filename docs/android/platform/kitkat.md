---
title: Funkcje KitKat
description: "Android 4.4 (KitKat) jest dostarczany załadowany z cornucopia funkcji dla użytkowników i deweloperów. Ten przewodnik prezentuje niektóre z tych funkcji i zawiera przykłady kodu i szczegóły implementacji podejmowanie wykorzystanie KitKat."
ms.topic: article
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: ae6b89e48005ca028db5d13f1a55f237888ae08b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="kitkat-features"></a>Funkcje KitKat

_Android 4.4 (KitKat) jest dostarczany załadowany z cornucopia funkcji dla użytkowników i deweloperów. Ten przewodnik prezentuje niektóre z tych funkcji i zawiera przykłady kodu i szczegóły implementacji podejmowanie wykorzystanie KitKat._

## <a name="overview"></a>Omówienie

Android 4.4 (poziom interfejsu API 19), nazywane również "KitKat", został wydany w późne 2013. KitKat oferuje wiele nowych funkcji i ulepszeń, w tym:

-  [Środowisko użytkownika](#user_experience) &ndash; łatwe animacje framework przejścia, przezroczyste paski stanu i nawigacji i bez ramek tryb pełnoekranowy utworzenia lepsze środowisko pracy użytkownika.

-  [Zawartość użytkownika](#user_content) &ndash; uproszczone zarządzanie plikami użytkownika Framework dostępu do magazynu; drukowania obrazów, witryn sieci web i innej zawartości jest łatwiejsze drukowanie ulepszone interfejsów API.

-  [Sprzęt](#hardware) &ndash; Włącz dowolną aplikację do karty NFC z NFC oparta na hoście karty emulacja; Uruchom niskiego poboru energii czujników z `SensorManager` .

-  [Narzędzia dla deweloperów](#developer_tools) &ndash; Screencast aplikacji w akcji przy użyciu klienta mostka debugowania systemu Android, dostępne jako część zestawu SDK systemu Android.


Ten przewodnik zawiera wskazówki dotyczące migrowania istniejących aplikacji platformy Xamarin.Android KitKat, jak również ogólne omówienie KitKat dla deweloperów platformy Xamarin.Android.

## <a name="requirements"></a>Wymagania

Do tworzenia aplikacji platformy Xamarin.Android przy użyciu KitKat, należy *Xamarin.Android 4.11.0* lub 4.4 wyżej i Android (19 poziom interfejsu API), instalowane za pośrednictwem Android SDK Manager, jak pokazano na poniższym zrzucie ekranu:

[![Wybieranie Android 4.4 w Menedżerze zestawu SDK systemu Android](kitkat-images/api19.png)](kitkat-images/api19.png)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>Migrowanie aplikacji do KitKat

Ta sekcja zawiera niektóre elementy pierwszej odpowiedzi przejścia istniejących aplikacji do systemu Android 4.4.

### <a name="check-system-version"></a>Sprawdź wersję systemu

Jeśli aplikacja musi być zgodne ze starszymi wersjami systemu android, pamiętaj zawijać dowolnego kodu KitKat w systemie kontroli wersji, jak pokazano na poniższym przykładzie kodu:

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Przetwarzanie wsadowe alarmu

Android przy użyciu usług alarm wznowienie działania aplikacji w tle o określonej godzinie. KitKat ma, to kolejny krok przez przetwarzanie wsadowe alarmy, aby zachować zasilania. Oznacza to, że zamiast dokładnego podczas wznawiania pracy każdej aplikacji, KitKat preferuje grupy kilka aplikacji, które są zarejestrowane do wznawiania przedziale czasowym i wznowić je w tym samym czasie.
Mówić Android wznowienie działania aplikacji w określonym interwale, wywołaj `SetWindow` na [ `AlarmManager` ](https://developer.xamarin.com/api/type/Android.App.AlarmManager/), przekazując minimalny i maksymalny czas (w milisekundach), które mogą upłynąć, zanim aplikacja jest wybudzany i operacji do wykonania w przypadku wznowienia.
Poniższy kod stanowi przykład musi wybudzane między pół godziny i godziny od momentu, gdy ustawiono okna aplikacji:

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Aby kontynuować, dokładne podczas wznawiania pracy aplikacji, użyj `SetExact`, przekazując dokładny czas, która powinna być wybudzane w aplikacji i operacji do wykonania:

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat umożliwia już ustawiania alarmu identycznych dokładne. Aplikacje używające [ `SetRepeating` ](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/) i wymaga dokładnego alarmy pracy będzie konieczne wyzwolenia każdy alarm ręcznie.

### <a name="external-storage"></a>Magazynu zewnętrznego

Magazynu zewnętrznego teraz jest podzielony na dwa typy - magazynu unikatowy do aplikacji i danych współużytkowane przez wiele aplikacji. Odczytywanie i zapisywanie do aplikacji w określonej lokalizacji w pamięci zewnętrznej, wymaga żadne specjalne uprawnienia. Wymaga interakcji z danymi w magazynie udostępnionym teraz `READ_EXTERNAL_STORAGE` lub `WRITE_EXTERNAL_STORAGE` uprawnienia. Dwa typy mogą być klasyfikowane, takich jak:

-  Jeśli jesteś już ścieżkę pliku lub katalogu przez wywołanie metody na `Context` — na przykład [ `GetExternalFilesDir` ](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/) lub [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - aplikacja wymaga nie dodatkowe uprawnienia.

-  Jeśli ścieżka pliku lub katalogu przygotowujemy podczas uzyskiwania dostępu do właściwości lub wywołanie metody `Environment` , takich jak [ `GetExternalStorageDirectory` ](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/) lub [ `GetExternalStoragePublicDirectory` ](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/) , wymaga aplikacji `READ_EXTERNAL_STORAGE` lub `WRITE_EXTERNAL_STORAGE` uprawnienia.

> [!NOTE]
> **Uwaga:** `WRITE_EXTERNAL_STORAGE` oznacza `READ_EXTERNAL_STORAGE` uprawnień, tak tylko powinien należy ustawić jednego uprawnienia.

### <a name="sms-consolidation"></a>Konsolidacja programu SMS

KitKat upraszcza przez agregowanie całą zawartość programu SMS w jednej domyślnej aplikacji wybrane przez użytkownika do obsługi komunikatów dla użytkownika. Deweloper jest odpowiedzialny za tworzenie aplikacji wybieranych jako domyślna aplikacja do obsługi wiadomości i zachowuje się odpowiednio w kodzie i życia Jeśli aplikacja nie jest zaznaczone. Aby uzyskać więcej informacji na przejście aplikacji SMS KitKat odwoływać się do [pobierania Your SMS aplikacje gotowe do KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) przewodnik od firmy Google.

### <a name="webview-apps"></a>Widok sieci Web aplikacji

[Widok sieci Web](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) otrzymano makeover w KitKat. Zmiana największych dodaniu zabezpieczeń ładowania zawartości w `WebView`. Podczas gdy większość aplikacji przeznaczonych dla starszych wersji interfejsu API powinny działać zgodnie z oczekiwaniami, testowanie aplikacji, które używają `WebView` klasy jest zdecydowanie zalecane. Aby uzyskać więcej informacji o usterce API widoku sieci Web dotyczą Android [Migrowanie do widoku sieci Web w systemie Android 4.4](http://developer.android.com/guide/webapps/migrating.html) dokumentacji.

<a name="user_experience" />

## <a name="user-experience"></a>Doświadczenie użytkownika

KitKat zawiera kilka nowych interfejsów API ulepszyć środowisko użytkownika, łącznie z nowej struktury przejście do obsługi właściwości animacji i półprzezroczyste opcja interfejsu użytkownika do tworzenia motywów. Poniżej opisano te zmiany.

### <a name="transition-framework"></a>Przejście Framework

W ramach przejścia sprawia, że animacji jest łatwiejsze do wdrożenia. KitKat pozwala wykonywać animację właściwości prostej się tylko jeden wiersz kodu lub Dostosuj przejść za pomocą *sceny*.

#### <a name="simple-property-animation"></a>Animacja właściwości prostej

Nowej biblioteki systemu Android przejścia upraszcza kodzie właściwości animacji. Platformę umożliwia wykonywanie prostych animacji z minimalnym kodu. Na przykład poniższy kod używa próbki [ `TransitionManager.BeginDelayedTransition` ](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/) do animowania pokazywanie i ukrywanie `TextView`:

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

W powyższym przykładzie używa w ramach przejścia do utworzenia automatyczne, domyślnego przejścia między zmiany wartości właściwości. Ponieważ animacji jest obsługiwany przez pojedynczy wiersz kodu, można łatwo uczynić zgodne ze starszymi wersjami systemu android, opakowując `BeginDelayedTransition` wywołań w systemie kontroli wersji. Zobacz [Migrowanie Your App do KitKat](#Migrating_Your_App_to_KitKat) sekcji, aby uzyskać więcej informacji.

Na poniższym zrzucie ekranu pokazano aplikację przed animacji:

[![Zrzut ekranu aplikacji przed uruchomieniem animacji](kitkat-images/trans-before.png)](kitkat-images/trans-before.png)

Na poniższym zrzucie ekranu pokazano aplikację po animacji:

[![Zrzut ekranu aplikacji, po zakończeniu animacji](kitkat-images/trans-after.png)](kitkat-images/trans-after.png)

Możesz uzyskać większą kontrolę nad przejścia z sceny, które opisano w następnej sekcji.

#### <a name="android-scenes"></a>Android sceny

[Sceny](https://developer.xamarin.com/api/type/Android.Transitions.Scene/) zostały wprowadzone w ramach struktury przejścia do zapewniają większą kontrolę nad animacje dewelopera. Sceny tworzenie dynamicznych obszaru, w interfejsie użytkownika: Określ kontenera i kilku wersji lub "sceny", dla zawartości XML w kontenerze, oraz Android zajmie się resztą pracy w celu animowania przejścia między sceny. Android sceny pozwalają tworzyć złożone animacje z minimalnym pracy po stronie programowanie.

Statyczny element interfejsu użytkownika przechowywania zawartości dynamicznej jest nazywana *kontenera* lub *sceny podstawowej*. W poniższym przykładzie użyto Android Designer do tworzenia `RelativeLayout` o nazwie `container`:

[![Przy użyciu narzędzia Projektant systemu Android, aby utworzyć kontener RelativeLayout](kitkat-images/container.png)](kitkat-images/container.png)

Przykładowy układ definiuje również przycisku o nazwie `sceneButton` poniżej `container`. Ten przycisk wyzwoli przejścia.

Zawartość dynamiczna wewnątrz kontenera wymaga dwóch nowych układów systemu Android. Te układów określić tylko kod *wewnątrz* kontenera.
Poniższy przykład kodu określa układ o nazwie *Scene1* odpowiednio zawiera dwa pola tekstowe odczytywania "Kit" i "Kat", a drugi Układu o nazwie *Scene2* zawiera tych samych polach tekstowych wycofane. Plik XML wygląda następująco:

 **Scene1.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

Przykład powyżej używa `merge` krótszą kod widoku i uproszczenia hierarchii widoku. Możesz przeczytać dodatkowe informacje `merge` układów [tutaj](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Sceny jest tworzony przez wywołanie metody [ `Scene.GetSceneForLayout` ](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/), przekazując identyfikator zasobu sceny pliku układu i bieżącego obiektu kontenera `Context`, jak pokazano w poniższym przykładzie kodu:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

Kliknięcie przycisku Przerzuca między dwa sceny, które Android animuje przejście domyślnymi wartościami:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

Poniższy zrzut ekranu przedstawia sceny przed animacji:

[![Zrzut ekranu przedstawiający aplikację przed rozpoczęciem animacji](kitkat-images/trans-after.png)](kitkat-images/trans-after.png)

Poniższy zrzut ekranu przedstawia sceny po animacji:

[![Zrzut ekranu aplikacji po zakończeniu animacji](kitkat-images/scene.png)](kitkat-images/scene.png)


> [!NOTE]
> **Uwaga:** jest [znaną usterką](https://code.google.com/p/android/issues/detail?id=62450) w Android przejścia utworzone za pomocą biblioteki, która powoduje, że sceny `GetSceneForLayout` do Przerwij, gdy użytkownik nawiguje działanie po raz drugi. Opisano obejście java [tutaj](http://www.doubleencore.com/2013/11/new-transitions-framework/).


##### <a name="custom-transitions-in-scenes"></a>Niestandardowe przejścia w tle

Niestandardowe przejścia można zdefiniować w pliku xml zasobów w `transition` katalog `Resources`, jak pokazano na poniższym zrzucie ekranu:

[![Lokalizacja pliku transition.xml w katalogu zasobów/przejścia](kitkat-images/resources.png)](kitkat-images/resources.png)

Poniższy przykładowy kod definiuje przejście animuje 5 sekund, który używa [przekroczenia interpolatora](http://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Przejście jest tworzony w działaniu przy użyciu [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/), jak pokazano w poniższym kodzie:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Nowe przejście jest dodawane do `Go` wywołania rozpoczęcia animacji:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Przezroczyste interfejsu użytkownika

KitKat umożliwia bardziej kontrolowanie motywów aplikacji za pomocą pasków stanu i nawigacja transclucent opcjonalne. Możesz zmienić przejrzystości elementów interfejsu użytkownika systemu w tym samym pliku XML, który służy do definiowania kompozycji systemu Android. KitKat wprowadza następujące właściwości:

-  `windowTranslucentStatus` — Gdy opcja ma wartość true, powoduje, że pasek stanu najwyższego przezroczyste.

-  `windowTranslucentNavigation` -Gdy opcja ma wartość true, powoduje, że pasek nawigacyjny przezroczyste.

-  `fitsSystemWindows` -Ustawienia paska top lub bottom transcluent przewiduje zawartości w obszarze przezroczysty elementy interfejsu użytkownika domyślnie. Ustawienie tej właściwości na `true` to prosty sposób zawartość z nakładającymi się z elementów interfejsu użytkownika systemu przezroczyste.


Poniższy kod definiuje motyw z przezroczyste paski stanu i nawigacji:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

Poniższy zrzut ekranu przedstawia motywu powyżej ze stanem przezroczyste i pasków nawigacji:

[![Zrzut ekranu aplikacji z przezroczyste paski stanu i nawigacji](kitkat-images/theme.png)](kitkat-images/theme.png)

<a name="user_content" />

## <a name="user-content"></a>Zawartość użytkownika

### <a name="storage-access-framework"></a>Storage-Access Framework

Framework dostępu do magazynu (SAF) to nowy sposób dla użytkownikom na interakcję z przechowywana zawartość, np. obrazów, klipów wideo i dokumentów. Zamiast prezentacji użytkowników z okna dialogowego, aby wybrać aplikację do obsługi zawartości KitKat zostanie otwarty nowy interfejs użytkownika, który umożliwia użytkownikom dostęp do danych w jednej lokalizacji agregacji. Po zawartości została wybrana, użytkownika nastąpi powrót do aplikacji, która żądanej zawartości, i środowiska korzystania z aplikacji będzie normalnego.

Ta zmiana wymaga dwóch działań na stronie developer: najpierw, aplikacje, które wymagają zawartości od dostawców muszą zostać zaktualizowane do nowy sposób reqesting zawartości. Drugi, aplikacje, które zapisywać danych `ContentProvider` muszą zostać zmodyfikowane w celu obsługi nowej struktury. Oba scenariusze są zależne od nowa [ `DocumentsProvider` ](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/) interfejsu API.

#### <a name="documentsprovider"></a>DocumentsProvider

W KitKat, interakcji z `ContentProviders` zamiast tego zostają wyodrębnione z `DocumentsProvider` klasy. Oznacza to, że SAF nie szczególną uwagę których dane są fizycznie, o ile jest dostępny za pośrednictwem `DocumentsProvider` interfejsu API. Lokalne dostawców chmury usługi i zewnętrznych urządzeń pamięci masowej wszystkich użycia taki sam interfejs, są traktowane tak samo, udostępnianie projektanta i użytkownika w jednym miejscu na interakcję z zawartością użytkownika.

W tej sekcji opisano sposób obciążenia i zapisywanie zawartości z architekturą dostępu do magazynu.

#### <a name="request-content-from-a-provider"></a>Zawartość żądania od dostawcy

Możemy stwierdzić KitKat chcemy pobrać zawartość przy użyciu interfejsu użytkownika SAF z `ActionOpenDocument` przeznaczeniu oznacza chęć nawiązać wszystkich dostawców zawartości dostępnych do urządzenia. Możesz dodać niektóre Filtrowanie to zamierzone, określając `CategoryOpenable`, co oznacza, że tylko zawartości, który można otworzyć (tj. dostępne, można używać zawartości) zostaną zwrócone. KitKat także pozwala na filtrowanie zawartości z `MimeType`. Na przykład kod poniżej filtry wyników obrazu, określając obrazu `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Wywoływanie `StartActivityForResult` uruchamia SAF interfejsu użytkownika, które użytkownik może następnie Przeglądaj, aby wybrać obraz:

[![Zrzut ekranu aplikacji przy użyciu platformy dostępu do magazynu dla Nawigacja do obrazu](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png)

Po użytkownik wybrał obrazu `OnActivityResult` zwraca `Android.Net.Uri` wybranego pliku. Poniższy przykładowy kod przedstawia wybór obrazu użytkownika:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>Zapisanie zawartości do dostawcy

Oprócz ładowania zawartości w interfejsie użytkownika SAF, KitKat również umożliwia zapisanie zawartości do dowolnego `ContentProvider` implementującej `DocumentProvider` interfejsu API. Zapisywanie zawartości używa `Intent` z `ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

W powyższym przykładzie kodu ładuje SAF interfejsu użytkownika, przez użytkownika, Zmień nazwę pliku i wybierz katalog do przechowywania nowy plik:

[![Zrzut ekranu przedstawiający użytkownika, zmiana nazwy pliku do NewDoc w katalogu pliki do pobrania](kitkat-images/saf-save.png)](kitkat-images/saf-save.png)

Gdy użytkownik naciśnie **zapisać**, `OnActivityResult` jest przekazywane `Android.Net.Uri` nowo utworzonego pliku, który jest dostępny z `data.Data`. Identyfikator uri może służyć do transmisji danych do nowego pliku:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

Należy pamiętać, że [ `ContentResolver.OpenOutputStream(Android.Net.Uri)` ](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri)) zwraca `System.IO.Stream`, więc w .NET można zapisywać cały proces przesyłania strumieniowego.

Więcej informacji o ładowania, tworzenia i edytowania zawartości za pomocą platformy dostępu do magazynu, można znaleźć [Framework dostępu do magazynu w dokumentacji systemu Android](http://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Drukowanie

Drukowanie zawartości została uproszczona w KitKat wraz z wprowadzeniem [usługi drukowania](https://developer.xamarin.com/api/namespace/Android.PrintServices/) i `PrintManager`. KitKat jest również pierwszą wersję interfejsu API w pełni wykorzystać [interfejsów API usług drukowania chmury firmy Google](https://developers.google.com/cloud-print/) przy użyciu [aplikacji Google Cloud drukowania](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint).
Większość urządzeń, które są dostarczane z KitKat automatycznie pobrać aplikację usługi Google Cloud drukowania i [wtyczka usługi drukowania HP](https://play.google.com/store/apps/details?id=com.hp.android.printservice)najpierw gdy łączą się z sieci Wi-Fi. Użytkownika można sprawdzić ustawienia wydruku własnego urządzenia, przechodząc do **Ustawienia > System > Drukowanie**:

[![Zrzucie ekranu, ustawienia drukowania](kitkat-images/printing.png)](kitkat-images/printing.png)


> [!NOTE]
> **Uwaga:** chociaż drukowania interfejsów API skonfigurowaniu do pracy z usługi Google Cloud drukowania domyślnie Android nadal umożliwia deweloperom przygotowanie drukowania zawartości przy użyciu nowych interfejsów API, a następnie wyślij je do innych aplikacji do drukowania.



#### <a name="printing-html-content"></a>Drukowanie zawartość HTML

Automatycznie tworzy KitKat [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) dla widoku sieci web z `WebView.CreatePrintDocumentAdapter`. Drukowanie zawartości sieci web jest wspólnym wysiłku między [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/) oczekuje na zawartość HTML do załadowania i umożliwia działanie wiedzieć, aby udostępnić opcji drukowania w menu Opcje i Actvity, która oczekuje na użytkownikowi Zaznacz opcję drukowania i wywołania `Print`na `PrintManager`. W tej sekcji omówiono podstawowe ustawienia wymagane do wydruku na ekranie zawartość HTML.

Należy pamiętać, że ładowanie i drukowania zawartości sieci web wymaga uprawnień internetowych:

[![Ustawianie uprawnień internetowych w opcjach aplikacji](kitkat-images/internet.png)](kitkat-images/internet.png)

##### <a name="print-menu-item"></a>Drukowanie elementu Menu

Opcji drukowania pojawi się zwykle w działaniu [menu opcji](http://developer.android.com/guide/topics/ui/menus.html#options-menu).
Menu opcji umożliwia użytkownikom akcje w ramach działania. W prawym górnym rogu ekranu, a wygląda następująco:

[![Zrzut ekranu przedstawiający przykładowy dispalyed elementu menu drukowania w prawym górnym rogu ekranu](kitkat-images/menu.png)](kitkat-images/menu.png)


Można zdefiniować dodatkowe elementy menu w *menu*katalog *zasobów*. Poniższy kod definiuje menu próbki elementu o nazwie [drukowania](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Interakcja z menu opcji w działaniu odbywa się w ramach `OnCreateOptionsMenu` i `OnOptionsItemSelected` metody.
`OnCreateOptionsMenu` Umożliwia dodawanie nowych elementów menu, takie jak opcja drukowania z *menu* zasobów katalogu.
`OnOptionsItemSelected` nasłuchuje użytkownika, wybierając z menu opcji Drukuj i rozpocznie się drukowanie:

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

Powyższy kod definiuje również zmiennej o nazwie `dataLoaded` do śledzenia stanu zawartość HTML. `WebViewClient` Spowoduje ustawienie tej zmiennej na wartość true, gdy cała zawartość została załadowana, będzie wówczas traktował działania, aby dodać element menu drukowania do menu Opcje.

##### <a name="webviewclient"></a>WebViewClient

Zadaniem `WebViewClient` jest zapewnienie danych w `WebView` zostanie całkowicie załadowany przed opcji drukowania jest wyświetlana w menu za pomocą `OnPageFinished` metody. `OnPageFinished` nasłuchuje na zakończenie ładowania zawartości sieci web i informuje działanie, aby ponownie utworzyć jej menu opcji z `InvalidateOptionsMenu`:

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished` Ustawia również `dataLoaded` do wartości `true`, więc `OnCreateOptionsMenu` odtworzyć menu opcji drukowania w miejscu.

##### <a name="printmanager"></a>PrintManager

Poniższy przykład kodu wyświetla zawartość `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` przyjmuje jako argumenty: nazwę zadania drukowania ("MyWebPage" w tym przykładzie), [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) z zawartości, który generuje dokument wydruku i [ `PrintAttributes` ](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/) (`null` w przykład powyżej). Można określić `PrintAttributes` do zaplanowania układu zawartości na stronie wydruku, mimo że domyślne atrybuty powinna obsługiwać większość scenarions.

Wywoływanie `Print` ładuje wydruku interfejsu użytkownika, które wymieniono opcje dla zadania drukowania. Interfejs użytkownika umożliwia użytkownikom opcję Drukowanie lub zapisywanie zawartości HTML do pliku PDF, jak pokazano na zrzutach ekranu poniżej:

[![Zrzut ekranu PrintHtmlActivity wyświetlanie menu Drukuj](kitkat-images/print1.png)](kitkat-images/print1.png)

[![Zrzut ekranu PrintHtmlActivity wyświetlanie Zapisz jako PDF menu](kitkat-images/print2.png)](kitkat-images/print2.png)

<a name="hardware" />

## <a name="hardware"></a>Sprzęt

KitKat dodaje kilka interfejsów API w celu uwzględnienia nowych funkcji urządzenia. Najbardziej Godny uwagi te są oparte na hoście karty emulacja i nowych `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>Oparta na hoście karty emulacja w NFC

Oparta na hoście karty emulacja (HCE) umożliwia aplikacjom przypominają kart NFC lub czytniki NFC bez polegania na operatora zastrzeżonych Secure elementu. Przed skonfigurowaniem HCE, upewnij się, HCE jest dostępny na urządzeniu z `PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE wymaga, aby funkcja HCE i `Nfc` uprawnienia można zarejestrować w aplikacji `AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![Ustawianie uprawnienia NFC w opcjach aplikacji](kitkat-images/nfc.png)](kitkat-images/nfc.png)

Do pracy, HCE ma będzie można uruchamiać w tle i ma uruchomić po dokonaniu transakcji NFC nawet wtedy, gdy aplikacja za pomocą HCE nie jest uruchomiona. Firma Microsoft można to zrobić przez napisanie kodu HCE jako `Service`. Usługa HCE implementuje `HostApduService` interfejs, który implementuje następujących metod:

-  *ProcessCommandApdu* -aplikacji protokołu danych jednostki (APDU) jest co to są wysyłane między czytnika NFC a usługą HCE. Ta metoda wykorzystuje ADPU z czytnika i zwraca jednostka danych w odpowiedzi.

-  *OnDeactivated* — `HostAdpuService` jest dezaktywowany, gdy usługa HCE komunikuje się już z czytnika NFC.


Usługa HCE również musi być zarejestrowana w manifeście aplikacji oraz ozdobione permissons właściwego filtru konwersji i metadanych. Następujący kod jest przykładem `HostApduService` zarejestrowany z manifestu systemu Android przy użyciu `Service` atrybutu (Aby uzyskać więcej informacji o atrybutach dotyczą Xamarin [Praca z manifestu systemu Android](~/android/platform/android-manifest.md) przewodnik):

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("andorid.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

Powyższe usługi umożliwia czytnika NFC do interakcji z aplikacją, ale czytnik NFC nadal nie ma możliwości wiedzy, jeśli ta usługa jest emulowanie karty NFC potrzebnych do skanowania. Aby ułatwić czytelnikowi NFC identyfikowania usługi, firma Microsoft można przypisać usługi unikatowego *Identyfikatora aplikacji (Pomoc)*. Możemy określić pomocy, oraz inne metadane dotyczące usługi HCE w pliku xml zasobów zarejestrowany `MetaData` atrybutu (patrz w powyższym przykładzie kodu). Ten plik zasobu określa jeden lub więcej pomocy filtrów — Unikatowy identyfikator ciągi w formacie szesnastkowym odpowiadające pomocy co najmniej jedno urządzenie czytnika NFC:

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

Oprócz filtry pomocy plik zasobu xml zawiera również opis uwzględniającym działania użytkownika usługi HCE Określa grupę pomocy (aplikacja płatności i "innych"), a w przypadku aplikacji płatności, transparent 260 x 96 punktów dystrybucji do wyświetlenia dla użytkownika.

Instalator opisanych powyżej udostępnia podstawowe bloki konstrukcyjne aplikacji emulowanie karty NFC. NFC sam wymaga wykonania kilku czynności więcej i dalszych badań, aby skonfigurować. Aby uzyskać więcej informacji na oparta na hoście karty emulacja, zapoznaj się [portal dokumentacji Android](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
Aby uzyskać więcej informacji na temat używania NFC z platformą Xamarin, zapoznaj się z [przykłady Xamarin NFC](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Czujników

KitKat zapewnia dostęp do urządzenia czujników za pośrednictwem [ `SensorManager` ](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/).
`SensorManager` Umożliwia systemu operacyjnego, można zaplanować dostarczania informacji z czujnika do aplikacji w partiach, zachowując czas pracy baterii.

KitKat jest także dostarczany z dwóch nowych typów czujnik śledzenia czynności użytkownika. Te są oparte na przyspieszeniomierza i obejmują:

-  *StepDetector* — aplikacja jest powiadamiany/wybudzany gdy użytkownik wykona kroku i detektora zawiera wartość czasu do czasu wystąpienia kroku.

-  *StepCounter* -przechowuje informacje o liczbę czynności użytkownika ma być wykonany, ponieważ zarejestrowano czujnika *dopiero po ponownym urządzenia*.

Poniższy zrzut ekranu przedstawia licznika krok w akcji:

[![Zrzut ekranu aplikacji SensorsActivity wyświetlanie licznika kroku](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png)

Można utworzyć `SensorManager` przez wywołanie metody `GetSystemService(SensorService)` i rzutowanie wynik w postaci `SensorManager`. Aby użyć licznika kroku, należy wywołać `GetDeafultSensor` na `SensorManager`. Można zarejestrować czujnika i słuchać zmiany liczby krok za pomocą [ `ISensorEventListener` ](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/) interfejsu, jak pokazano na poniższym przykładzie kodu:

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged` jest wywoływana, gdy liczba krok aktualizacji, podczas gdy aplikacja działa na pierwszym planie. Aplikacja przechodzi w tle, czy urządzenie jest w stanie uśpienia, `OnSensorChanged` nie zostanie wywołana; kroki będą jednak nadal zliczenia do `UnregisterListener` jest wywoływana.

Należy pamiętać, że *wartość licznika kroku jest zbiorcza we wszystkich aplikacjach, które rejestrują czujnika*. Oznacza to, że nawet jeśli odinstalowanie i ponowne zainstalowanie aplikacji i Inicjowanie `count` zmiennej w lokalizacji 0 przy uruchamianiu aplikacji, wartość zgłaszaną przez czujnik pozostanie całkowita liczba podjęte, gdy czujnik został zarejestrowany, czy przez użytkownika Aplikacja lub innej. Można uniemożliwić aplikacji dodawanie do licznika kroku przez wywołanie metody `UnregisterListener` na `SensorManager`, jak pokazano w poniższym kodzie:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

Ponowne uruchomienie urządzenia Resetuje licznik krok na 0. Aplikacja będzie wymagać dodatkowego kodu upewnij się, że to jest raportowanie dokładne liczba dla aplikacji, niezależnie od tego, inne aplikacje korzystające z czujnika lub stan urządzenia.


> [!NOTE]
> **Uwaga:** podczas API wykrywania kroku i zliczanie jest dostarczany z KitKat, nie wszystkie telefony mają przypisane z czujnika. Można sprawdzić, czy czujnika jest dostępna, uruchamiając `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, lub sprawdź wartość `GetDefaultSensor` nie jest `null`.


 <a name="developer_tools" />


## <a name="developer-tools"></a>Narzędzia dla deweloperów

### <a name="screen-recording"></a>Nagrywanie ekranu

KitKat obejmuje ekranu nowe funkcje nagrywania, dzięki czemu deweloperzy można zarejestrować aplikacji w akcji. Nagrywanie ekranu jest dostępny za pośrednictwem [Android debugowania Mostek (ADB)](http://developer.android.com/tools/help/adb.html) klienta, który można pobrać jako część zestawu SDK systemu Android.

Do rejestrowania ekranu, podłącz urządzenie; następnie zlokalizować instalacji zestawu SDK systemu Android, przejdź do **narzędzi platformy** katalogu i uruchom **adb** klienta:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Powyższe polecenie zapisuje 3-minutowy film wideo domyślne w domyślnej rozdzielczości 4Mbps. Aby edytować długości, należy dodać *— limit czasu* flagi.
Aby zmienić rozdzielczość, Dodaj *— szybkości transmisji bitów* flagi. Poniższe polecenie zapisuje wideo long minutę na 8Mbps:

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Możesz znaleźć wideo na urządzeniu — pojawi się w galerii po zakończeniu rejestrowania.

<a name="other_kitkat_additions" />

## <a name="other-kitkat-additions"></a>Innych dodatków KitKat

Oprócz zmian opisane powyżej KitKat umożliwia:

-  *Korzystać z pełnego ekranu* -KitKat wprowadzono nowy [trybie bez ramek](http://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) do przeglądania zawartości, granie w gry i uruchamianie innych aplikacji, które mogłyby korzystać z obsługi pełnego ekranu.

-  *Dostosowywanie powiadomień* -uzyskać dodatkowe informacje na temat powiadomienia systemowe z [ `NotificationListenerService` ](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) . Dzięki temu można prezentować te informacje w inny sposób wewnątrz aplikacji.

-  *Duplikuj zasobów obiektów Drawable* -obiektów Drawable zasobów ma nowego [ `autoMirrored` ](http://developer.android.com/reference/android/R.attr.html#autoMirrored) atrybut, który informuje system Utwórz dublowane wersji obrazów wymagających Przerzucanie układów od lewej do prawej.

-  *Wstrzymaj animacje* -wstrzymywanie i wznawianie animacje utworzone za pomocą [ `Animator` ](https://developer.xamarin.com/api/type/Android.Animation.Animator/) klasy.

-  *Odczyt dynamicznie zmiana tekstu* -oznaczenia części interfejsu użytkownika, które aktualizują dynamicznie na nowy tekst jako "na żywo regiony" z nowym [ `accesibilityLiveRegion` ](http://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion) atrybutu tak nowego tekstu zostaną odczytane automatycznie w trybie ułatwień dostępu.

-  *Udoskonalenie Audio* — Utwórz głośniej śledzi z [ `LoudnessEnhancer` ](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/) , Znajdź szczytowe i RMS strumień audio z [ `Visualizer` ](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/) klasy, a następnie uzyskaj informacje z [audio sygnatury czasowej](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/) ułatwiające synchronizacji audio i wideo.

-  *Synchronizowanie klasy ContentResolver interwałem niestandardowe* -KitKat dodaje niektórych zmienności czasu żądanie synchronizacji jest wykonywana. Synchronizacja `ContentResolver` w czasie niestandardowych lub interwał wywołując `ContentResolver.RequestSync` i przekazując `SyncRequest`.

-  *Rozróżniaj między kontrolerami* -KitKat w kontrolery są przypisywane identyfikatory unikatowa liczba całkowita, które są dostępne za pośrednictwem urządzenia `ControllerNumber` właściwości. Ułatwia to Poinformuj odtwarzaczy od siebie w grę.

-  *Zdalne sterowanie* -kilka zmian na stronie sprzęt i oprogramowanie, KitKat pozwala włączać urządzenia przypisane z nadajnik IR do zdalnego sterowania przy użyciu `ConsumerIrService`oraz pracować interaktywnie z urządzeń peryferyjnych z nowego [ `RemoteController` ](https://developer.xamarin.com/api/type/Android.Media.RemoteController/) Interfejsów API.

Aby uzyskać więcej informacji na powyższe zmiany interfejsu API, zapoznaj się Google [Android 4.4 interfejsów API](http://developer.android.com/about/versions/android-4.4.html) omówienie.


## <a name="summary"></a>Podsumowanie

W tym artykule wprowadzone niektóre z nowych interfejsów API dostępne w Android 4.4 (19 poziom interfejsu API), a podczas przejścia aplikacji KitKat jest objęty najlepsze rozwiązania. Występują obramowane zmiany do interfejsów API, mające wpływ na użytkownika, w tym informatyczne *framework przejścia* i nowe opcje *motywów*. Następnie należy wprowadzić *Framework dostępu do magazynu* i `DocumentsProvider` klasy, a także nowy *drukowanie interfejsów API*. On eksplorowanych *NFC oparta na hoście karty emulacja* i sposobu pracy z *czujniki niskiego poboru energii*, łącznie z dwóch nowych czujnikami śledzenia czynności użytkownika. Na koniec przedstawiona przechwytywanie w czasie rzeczywistym w trakcie aplikacji z *rejestrowania ekranu*oraz szczegółowy wykaz zmian KitKat interfejsu API i dodatków.


## <a name="related-links"></a>Linki pokrewne

- [Przykładowe KitKat](https://developer.xamarin.com/samples/KitKat/)
- [Android 4.4 interfejsów API](http://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](http://developer.android.com/about/versions/kitkat.html)
