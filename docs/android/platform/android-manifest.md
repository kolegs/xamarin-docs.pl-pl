---
title: Praca z manifestu systemu Android
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 18817063900437baa625d8572f0ae28fec77be1e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-android-manifest"></a>Praca z manifestu systemu Android


## <a name="overview"></a>Omówienie

**AndroidManifest.xml** jest plikiem zaawansowanych w platformy systemu Android umożliwiający objaśniono funkcje i wymagania dotyczące aplikacji w systemie Android. Jednak pracy z nim nie jest łatwe. Xamarin.Android pozwala zminimalizować ten problemy, umożliwiając dodać niestandardowe atrybuty do klas, które zostanie następnie użyte do automatycznego generowania manifestu dla Ciebie. Naszym celem jest, że 99% naszych użytkowników nigdy nie powinien trzeba ręcznie zmodyfikować **AndroidManifest.xml**. 

**AndroidManifest.xml** wygenerowaniu jako część procesu kompilacji, a znaleziono w pliku XML **Properties/AndroidManifest.xml** jest scalany z XML, który jest generowany na podstawie atrybutów niestandardowych. Powstałe w ten sposób scalić **AndroidManifest.xml** znajduje się w **obj** podkatalogu; na przykład znajduje się on w **obj/Debug/android/AndroidManifest.xml** w przypadku kompilacji debugowania . Proces scalania jest prosta: używa atrybutów niestandardowych z kodem, aby wygenerować elementy XML i *wstawia* tych elementów do **AndroidManifest.xml**. 



## <a name="the-basics"></a>Podstawy

W czasie kompilacji zestawy są skanowane pod kątem inną niż`abstract` klasy, które pochodzą z [działania](https://developer.xamarin.com/api/type/Android.App.Activity/) i mieć [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) atrybut zadeklarowany na nich. Te klasy i atrybuty następnie używa do kompilacji do manifestu. Rozważmy na przykład następujący kod: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Powoduje to nic generowane w **AndroidManifest.xml**. Jeśli chcesz `<activity/>` element ma zostać wygenerowane, należy użyć [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) atrybutu niestandardowego: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

W tym przykładzie powoduje, że poniższy fragment xml, który ma zostać dodany do **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` Atrybut nie ma wpływu `abstract` typów; `abstract` typy są ignorowane.



### <a name="activity-name"></a>Nazwa działania

Począwszy od platformy Xamarin.Android 5.1, nazwa typu działania jest oparty na MD5SUM nazwa kwalifikowana zestawu typu eksportowane. Dzięki tej samej nazwie pełną, należy podać z dwóch różnych zestawów i nie błąd tworzenia pakietów. (Przed Xamarin.Android 5.1 z małej przestrzeni nazw i nazwę klasy została utworzona nazwa domyślnego typu działania). 

Jeśli chcesz zastąpić to ustawienie domyślne i jawnie określić nazwę działanie, użyj [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) właściwości: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

W tym przykładzie powoduje poniższy fragment xml:

```xml
<activity android:name="awesome.demo.activity" />
```

*Uwaga*: należy używać `Name` właściwość tylko dla zgodności z poprzednimi wersjami powodów, takich jak zmiana nazwy może to spowolnić wyszukiwania typów w czasie wykonywania. Jeśli masz starszego kodu, który oczekuje, że nazwa domyślnego typu działania opartego na małej przestrzeni nazw i nazwy klasy, zobacz [Android można wywołać nazw otoki](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) porady na temat zachowania zgodności. 


### <a name="activity-title-bar"></a>Działania, pasek tytułu

Domyślnie Android daje aplikacji paska tytułu po jego uruchomieniu. Jest to wartość [ `/manifest/application/activity/@android:label` ](http://developer.android.com/guide/topics/manifest/activity-element.html#label). W większości przypadków ta wartość będzie różnić się od na nazwę klasy. Aby określić aplikacji etykiety na pasku tytułu, użyj [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) właściwości.
Na przykład: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

W tym przykładzie powoduje poniższy fragment xml:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>Uruchamiana z wybór aplikacji

Domyślnie działanie nie pojawi się na ekranie uruchamiania aplikacji dla systemu Android. Jest to, ponieważ będzie prawdopodobnie wiele działań w aplikacji i nie ma ikony dla każdego z nich. Aby określić, która powinna być uruchamiana z uruchamiający aplikację, użyj [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) właściwości. Na przykład: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

W tym przykładzie powoduje poniższy fragment xml:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```



### <a name="activity-icon"></a>Ikona działania

Domyślnie działanie będzie mógł skorzystać z domyślną ikonę uruchamiania systemu. Aby korzystać z ikoną niestandardową, należy najpierw dodać Twojego **.png** do **obiektów drawable/zasoby**, jego Akcja kompilacji ustawioną **AndroidResource**, następnie użyć [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) właściwości w celu określenia ikonę do używania. Na przykład: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

W tym przykładzie powoduje poniższy fragment xml:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```


### <a name="permissions"></a>Uprawnienia

Podczas dodawania uprawnień do manifestu systemu Android (zgodnie z opisem w [Dodaj uprawnienia do manifestu systemu Android](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/)), te uprawnienia są rejestrowane w **Properties/AndroidManifest.xml**. Na przykład jeśli ustawisz `INTERNET` uprawnień, następujący element zostanie dodany do **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Kompilacje debugowania ustawiony automatycznie niektóre uprawnienia, aby łatwiej debugowania (takich jak `INTERNET` i `READ_EXTERNAL_STORAGE`) &ndash; te ustawienia tylko w wygenerowanym **obj/Debug/android/AndroidManifest.xml** i nie są pokazano, jak włączyć w **wymagane uprawnienia** ustawienia. 

Na przykład, jeśli należy zbadać wygenerowanego pliku manifestu w **obj/Debug/android/AndroidManifest.xml**, może zostać wyświetlony dodaje elementy uprawnienia: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

W wersji kompilacji wersji manifestu (w **obj/Debug/android/AndroidManifest.xml**), te uprawnienia są *nie* automatycznie skonfigurowana. Jeśli okaże się, że przełączanie do kompilacji wydania powoduje utratę uprawnienia, które były dostępne w kompilacji debugowania aplikacji, sprawdź jawnie ustawić te uprawnienia **wymagane uprawnienia** ustawień dla aplikacji (zobacz  **Tworzenie > aplikacji systemu Android** w programie Visual Studio dla komputerów Mac; zobacz **właściwości > manifestu systemu Android** w programie Visual Studio). 




## <a name="advanced-features"></a>Funkcje zaawansowane


### <a name="intent-actions-and-features"></a>Konwersji akcje i funkcje

Manifestu systemu Android udostępnia sposób opisano możliwości działanie. Odbywa się za pośrednictwem [intencje](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) i [ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) atrybutu niestandardowego. Możesz określić akcje, które są odpowiednie dla Twojej aktywności za pomocą [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) Konstruktor i kategorie, które są odpowiednie z [ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) właściwości. Co najmniej jedno działanie należy podać (czyli Dlaczego działania znajdują się w konstruktorze). `[IntentFilter]` można podać kilka razy, a wyniki każdego zastosowania w oddzielnej `<intent-filter/>` w elemencie `<activity/>`. Na przykład:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

W tym przykładzie powoduje poniższy fragment xml:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```


### <a name="application-element"></a>Element aplikacji

Manifestu systemu Android udostępnia również sposób można zadeklarować właściwości dla całej aplikacji. Odbywa się za pośrednictwem `<application>` elementu i jego odpowiednik [aplikacji](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) atrybutu niestandardowego. Należy pamiętać, że są one ustawienia dotyczące całej aplikacji (całego zestawu), a nie ustawienia na działania. Zazwyczaj zadeklarować `<application>` właściwości dla całej aplikacji, a następnie zastąpić te ustawienia (w razie potrzeby), na podstawie na działania. 

Na przykład następująca `Application` jest dodawany atrybut **AssemblyInfo.cs** aby wskazać, że aplikacja może być debugowany, że jego nazwa użytkownika do odczytu jest **Moja aplikacja**, i korzysta z `Theme.Light` styl jako motyw domyślny dla wszystkich działań: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Ta deklaracja powoduje poniższy fragment XML, który ma być generowany w **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
W tym przykładzie wszystkie działania w aplikacji zostaną domyślnie `Theme.Light` stylu. Jeśli ustawiono motywu działania `Theme.Dialog`, tylko który będzie używany przez działanie `Theme.Dialog` stylów, gdy wszystkie działania w aplikacji zostaną domyślnie `Theme.Light` styl zgodnie z `<application>` elementu. 

`Application` Element nie jest jedynym sposobem, aby skonfigurować `<application>` atrybutów. Alternatywnie można wstawić atrybuty bezpośrednio do `<application>` elementu **Properties/AndroidManifest.xml**. Te ustawienia są scalane w końcowym `<application>` element, który znajduje się w **obj/Debug/android/AndroidManifest.xml**. Należy pamiętać, że zawartość **Properties/AndroidManifest.xml** zawsze zastępują dane dostarczone przez atrybuty niestandardowe. 

Istnieje wiele atrybutów całej aplikacji, które można skonfigurować w `<application>` element; Aby uzyskać więcej informacji o tych ustawieniach, zobacz [właściwości publiczne](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) sekcji [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Lista atrybutów niestandardowych

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : generuje [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) fragmentu XML 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : generuje [/manifest aplikacji](http://developer.android.com/guide/topics/manifest/application-element.html) fragmentu XML 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : generuje [/manifest/Instrumentacji](http://developer.android.com/guide/topics/manifest/instrumentation-element.html) fragmentu XML 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : generuje [//intent-filter](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) fragmentu XML 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : generuje [//meta-data](http://developer.android.com/guide/topics/manifest/meta-data-element.html) fragmentu XML 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : generuje [//permission](http://developer.android.com/guide/topics/manifest/permission-element.html) fragmentu XML 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : generuje [//permission-group](http://developer.android.com/guide/topics/manifest/permission-group-element.html) fragmentu XML 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : generuje [//permission-tree](http://developer.android.com/guide/topics/manifest/permission-tree-element.html) fragmentu XML 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : generuje [/manifest/application/service](http://developer.android.com/guide/topics/manifest/service-element.html) fragmentu XML 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : generuje [/manifest/application/uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) fragmentu XML 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : generuje [/manifest/uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) fragmentu XML 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : generuje [/manifest/application/receiver](http://developer.android.com/guide/topics/manifest/receiver-element.html) fragmentu XML 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : generuje [/manifest/application/provider](http://developer.android.com/guide/topics/manifest/provider-element.html) fragmentu XML 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : generuje [/manifest/application/provider/grant-uri-permission](http://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) fragmentu XML

