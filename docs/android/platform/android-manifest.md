---
title: Praca z manifestu systemu Android
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0857b70e6e1d9104f62ec2e26f8edbab385d06f3
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242254"
---
# <a name="working-with-the-android-manifest"></a>Praca z manifestu systemu Android


## <a name="overview"></a>Omówienie

**AndroidManifest.xml** jest plikiem Zaawansowane na platformie systemu Android umożliwiające objaśniono funkcje i wymagania aplikacji systemu Android. Pracę z nim nie jest jednak proste. Platforma Xamarin.Android pomaga zminimalizować trudności w tym, co pozwala na dodawanie atrybutów niestandardowych do swoich klas, które będą następnie używane do automatycznego generowania manifestu dla Ciebie. Naszym celem jest, że 99% naszych użytkowników powinno nigdy nie musisz ręcznie modyfikować **AndroidManifest.xml**. 

**AndroidManifest.xml** zostanie wygenerowany jako część procesu kompilacji i XML można znaleźć w **Properties/AndroidManifest.xml** jest scalany z danymi XML, który jest generowany na podstawie atrybutów niestandardowych. Wynikowy scalony **AndroidManifest.xml** znajduje się w **obj** podkatalogu; na przykład znajduje się on w **obj/Debug/android/AndroidManifest.xml** w przypadku kompilacji debugowania . Proces scalanie jest proste: używa niestandardowych atrybutów w obrębie kodu do generowania elementów XML i *wstawia* tych elementów do **AndroidManifest.xml**. 



## <a name="the-basics"></a>Podstawowe informacje

W czasie kompilacji zestawy są skanowane pod kątem non -`abstract` klas, które wynikają z [działania](https://developer.xamarin.com/api/type/Android.App.Activity/) i [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) atrybut jest zadeklarowany na nich. Te klasy i atrybuty następnie używa do tworzenia manifestu. Na przykład rozważmy następujący kod: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Skutkuje to nic generowanych w **AndroidManifest.xml**. Jeśli chcesz `<activity/>` element zostanie wygenerowany, należy użyć [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) atrybutów niestandardowych: 

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

Począwszy od platformy Xamarin.Android 5.1, nazwa typu działania opiera się na MD5SUM kwalifikowanych dla zestawu Nazwa typu eksportowany. Dzięki temu tej samej w pełni kwalifikowaną nazwę aby musi dostarczyć dwa różne zestawy i nie otrzymać błąd tworzenia pakietów. (Przed 5.1 platformy Xamarin.Android, domyślna nazwa typu działania utworzono pisany małymi literami przestrzeni nazw i nazwę klasy.) 

Jeśli chcesz zastąpić to ustawienie domyślne i jawnie określić nazwę działania, użyj [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) właściwości: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

Ten przykład generuje poniższy fragment xml:

```xml
<activity android:name="awesome.demo.activity" />
```

*Uwaga*: należy używać `Name` właściwości tylko dla przyczyn zgodności z poprzednimi wersjami, w związku z tym zmiana nazwy może spowolnić wyszukiwanie typu w czasie wykonywania. W przypadku starszego kodu, który oczekuje, że domyślna nazwa typu działania była oparta na pisany małymi literami przestrzeni nazw i nazwy klasy, zobacz [Android nazewnictwa wywoływalnej otoki](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) porady na temat zachowania zgodności. 


### <a name="activity-title-bar"></a>Pasek tytułu działania

Domyślnie system Android zapewnia aplikacji paska tytułu po jej uruchomieniu. Jest to wartość [ `/manifest/application/activity/@android:label` ](http://developer.android.com/guide/topics/manifest/activity-element.html#label). W większości przypadków ta wartość będzie różnić się od Twoja nazwa klasy. Aby określić etykiety Twojej aplikacji na pasku tytułu, użyj [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) właściwości.
Na przykład: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

Ten przykład generuje poniższy fragment xml:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>Uruchamiana z wybór aplikacji

Domyślnie Twoje działania nie pojawią się na ekranie uruchamiania aplikacji systemu Android. Jest to spowodowane będą najprawdopodobniej wiele działań w aplikacji, a nie chcesz, aby dla każdego z nich ikony. Aby określić, który z nich powinna być uruchamiana z uruchamiania aplikacji, użyj [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) właściwości. Na przykład: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

Ten przykład generuje poniższy fragment xml:

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

Domyślnie działania będzie miał domyślną ikonę uruchamianie udostępnianej przez system. Aby korzystać z ikoną niestandardową, należy najpierw dodać swoje **.png** do **zasobów/drawable**, ustaw Build Action **AndroidResource**, następnie za pomocą [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) właściwości w celu określenia ikonę do używania. Na przykład: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

Ten przykład generuje poniższy fragment xml:

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

Po dodaniu uprawnień do manifestu systemu Android (zgodnie z opisem w [Dodaj uprawnienia do manifestu systemu Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)), te uprawnienia są rejestrowane w **Properties/AndroidManifest.xml**. Na przykład jeśli ustawisz `INTERNET` uprawnienie, następujący element zostanie dodany do **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Kompilacje debugowania automatycznie ustawić niektóre uprawnienia, aby upewnić się, łatwiejsze debugowanie (takie jak `INTERNET` i `READ_EXTERNAL_STORAGE`) &ndash; te ustawienia są ustawione tylko w wygenerowanym **obj/Debug/android/AndroidManifest.xml** i nie są przedstawione jako włączony w **wymagane uprawnienia** ustawienia. 

Na przykład, jeśli wygenerowane w pliku manifestu w **obj/Debug/android/AndroidManifest.xml**, może zostać wyświetlony, dodaje elementów uprawnienia: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

W wersji wersja manifestu kompilacji (w **obj/Debug/android/AndroidManifest.xml**), te uprawnienia są *nie* automatycznie skonfigurowana. Jeśli okaże się, że przełączanie do kompilacji wydania powoduje utratę uprawnienia, które były dostępne w kompilacji debugowania aplikacji, sprawdź jawnie nadajesz to uprawnienie w **wymagane uprawnienia** ustawień aplikacji (zobacz  **Tworzenie > Aplikacja dla systemu Android** w programie Visual Studio dla komputerów Mac; zobacz **właściwości > manifestu systemu Android** w programie Visual Studio). 




## <a name="advanced-features"></a>Funkcje zaawansowane


### <a name="intent-actions-and-features"></a>Intencji akcje i funkcje

Manifest systemu Android umożliwia możesz opisać możliwości działania. Odbywa się za pośrednictwem [intencji](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) i [ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) atrybutu niestandardowego. Możesz określić akcje, które są odpowiednie dla Twojego działania o [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) Konstruktor i kategorie, które są odpowiednie z [ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) właściwości. Co najmniej jedno działanie musi być podana (jest to, dlaczego działań znajdują się w konstruktorze). `[IntentFilter]` można podać kilka razy, a każde użycie wyników w osobnym `<intent-filter/>` elemencie `<activity/>`. Na przykład:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

Ten przykład generuje poniższy fragment xml:

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

Manifest systemu Android także sposób deklarowania właściwości dla całej aplikacji. Odbywa się za pośrednictwem `<application>` elementu i jego odpowiednika [aplikacji](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) atrybutu niestandardowego. Należy pamiętać, że są to ustawienia (na poziomie zestawu) całej aplikacji, a nie ustawienia poszczególnych działań. Zazwyczaj zadeklarować `<application>` właściwości dla całej aplikacji, a następnie zastąpić te ustawienia (zgodnie z potrzebami), na podstawie poszczególnych działań. 

Na przykład następująca `Application` jest dodawany atrybut **AssemblyInfo.cs** aby wskazać, że aplikacja może być debugowany, że jego czytelny dla użytkownika nazwa jest **Moja aplikacja**, i że używa on `Theme.Light` styl jako motyw domyślny dla wszystkich działań: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Ta deklaracja powoduje, że poniższy fragment XML do wygenerowania w **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
W tym przykładzie wszystkie działania w aplikacji będą domyślnie `Theme.Light` stylu. Jeśli motyw działania jest ustawiona na `Theme.Dialog`, tylko, że działanie użyje `Theme.Dialog` stylu, podczas gdy inne działania w Twojej aplikacji będą domyślnie `Theme.Light` styl zgodnie z `<application>` elementu. 

`Application` Element nie jest jedynym sposobem, aby skonfigurować `<application>` atrybutów. Alternatywnie można wstawić atrybuty bezpośrednio do `<application>` elementu **Properties/AndroidManifest.xml**. Te ustawienia są scalane w końcowym `<application>` element, który znajduje się w **obj/Debug/android/AndroidManifest.xml**. Należy pamiętać, że zawartość **Properties/AndroidManifest.xml** zawsze zastępuje dane dostarczone przez atrybuty niestandardowe. 

Istnieje wiele atrybutów całej aplikacji, które można skonfigurować w `<application>` elementu; Aby uzyskać więcej informacji o tych ustawieniach, zobacz [właściwości publiczne](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) części [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Listę atrybutów niestandardowych

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : generuje [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) fragmentu XML 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : generuje [/manifest/aplikacji](http://developer.android.com/guide/topics/manifest/application-element.html) fragmentu XML 
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

