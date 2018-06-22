---
title: Łączenie aplikacji w systemie Android
description: W tym przewodniku będzie omawiać jak system Android 6.0 obsługuje aplikacji łączenia, metody, która umożliwia aplikacji mobilnych odpowiedzieć na adresy URL w witrynach sieci Web. Przedstawimy jakie łączenie aplikacji jest implementowania łączenie aplikacji w aplikacji systemu Android w wersji 6.0 i konfigurowania witryny sieci Web, aby udzielić uprawnień do aplikacji mobilnej dla domeny.
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 84185bb616597ee62a35c1acacc5e3664f500c21
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436573"
---
# <a name="app-linking-in-android"></a>Łączenie aplikacji w systemie Android

_W tym przewodniku będzie omawiać jak system Android 6.0 obsługuje aplikacji łączenia, metody, która umożliwia aplikacji mobilnych odpowiedzieć na adresy URL w witrynach sieci Web. Przedstawimy jakie łączenie aplikacji jest implementowania łączenie aplikacji w aplikacji systemu Android w wersji 6.0 i konfigurowania witryny sieci Web, aby udzielić uprawnień do aplikacji mobilnej dla domeny._

## <a name="app-linking-overview"></a>Omówienie łączenie aplikacji

Aplikacje mobilne nie jest już na żywo w silosu &ndash; w wielu przypadkach są one ważne składniki ich firm, wraz z ich witryny sieci Web. Jest pożądane firmom bezproblemowo połączyć ich obecności sieci web i aplikacji dla urządzeń przenośnych, wraz z łączami w witrynie sieci Web uruchamiania aplikacji dla urządzeń przenośnych i wyświetlanie odpowiedniej zawartości w aplikacji mobilnej. *Łączenie aplikacji* (zwaną także *połączeń bezpośrednich*) jest jedną metodę, która umożliwia urządzeniu odpowiadanie na identyfikator URI i uruchamianie aplikacji mobilnej, która odnosi się do tego identyfikatora URI.

Android obsługuje łączenie aplikacji za pośrednictwem *konwersji system* &ndash; gdy użytkownik kliknie łącze w przeglądarkę dla telefonów, przenośne przeglądarki wyśle opcje Android będzie delegowane do zarejestrowanej aplikacji. Na przykład kliknięcie łącza na gotowania witryny sieci Web spowoduje otwarcie aplikacji mobilnej, który jest skojarzony z tej witryny sieci Web i wyświetlenie przepisu określonych dla użytkownika. W przypadku więcej niż jedną aplikację zarejestrowana do obsługi tego celem, a następnie Android zostanie podniesiony, co jest nazywane *okna dialogowego Uściślanie* która poprosi użytkownika wybierz aplikację, która powinna obsługiwać zamiar, dla jakiego aplikacji przykład:

![Zrzut ekranu okna dialogowego Uściślanie](app-linking-images/01-disambiguation-dialog.png)

System android 6.0 poprawia na tym za pomocą obsługi automatycznego łącza. Istnieje możliwość dla systemu Android automatycznie zarejestrować aplikację jako domyślny program obsługi dla identyfikatora URI &ndash; aplikacji są uruchamiane automatycznie i przejść bezpośrednio do odpowiednich działań. Jak system Android 6.0 decyduje o tym, do obsługi kliknij identyfikatora URI jest zależna od następujących kryteriów:

1. **Istniejącej aplikacji jest już skojarzona z identyfikatorem URI** &ndash; użytkownika mogą być już powiązane istniejącą aplikację z identyfikatorem URI. W takim przypadku Android będą w dalszym ciągu korzystać z tej aplikacji.
2. **Nie istniejącej aplikacji jest skojarzony z identyfikatorem URI, ale uzupełniające aplikacja jest instalowana** &ndash; w tym scenariuszu użytkownik nie określił istniejącej aplikacji Android użyje zainstalowanej obsługi aplikacji do obsługi żądania.
3. **Nie istniejącej aplikacji jest skojarzony z identyfikatorem URI, ale zainstalowano wiele aplikacji obsługi** &ndash; ponieważ istnieje wiele aplikacji, które obsługują identyfikator URI, wyświetli się okno dialogowe Uściślanie i użytkownik musi wybrać, która aplikacja zostanie Obsługa identyfikatora URI.

Jeśli użytkownik nie ma żadnych zainstalowane aplikacje, które obsługują identyfikator URI, a następnie jest zainstalowana jedna, następnie Android ustawi tę aplikację jako domyślny program obsługi dla identyfikatora URI po zweryfikowaniu skojarzenia z witryny sieci Web, która jest skojarzona z identyfikatorem URI.

W tym przewodniku będzie omawiać konfigurowania aplikacji systemu Android w wersji 6.0 i jak utworzyć i opublikować ten plik łącza zasobów cyfrowych obsługuje łączenia aplikacji w systemie Android w wersji 6.0.

## <a name="requirements"></a>Wymagania

W tym przewodniku wymaga platformy Xamarin.Android 6.1 i aplikacji, która jest przeznaczony dla systemu Android 6.0 (interfejs API na poziomie 23) lub nowszej.

Łączenie aplikacji jest możliwe we wcześniejszych wersjach systemu android przy użyciu [pakietu Rivets NuGet](https://www.nuget.org/packages/Rivets/) w magazynie składników Xamarin. Pakiet nity nie jest zgodny z łączenie aplikacji z systemem Android 6.0; nie obsługuje łączenie aplikacji dla systemu Android w wersji 6.0.

## <a name="configuring-app-linking-in-android-60"></a>Konfigurowanie łączenie aplikacji z systemem Android 6.0

Konfigurowanie aplikacji łącza w systemie Android 6.0 obejmuje dwa główne kroki:

1. **Dodawanie co najmniej jeden zamiar — filtry dla identyfikatora URI witryny** &ndash; konwersji filtry przewodnik dotyczący systemu Android w sposób obsługi kliknij adres URL w przeglądarce przenośnych.
2. **Publikowanie *cyfrowe JSON łącza zasobów* pliku w witrynie internetowej** &ndash; jest to plik jest przekazywany do witryny sieci Web, który jest używany przez system Android można zweryfikować relacji między aplikacjami mobilnymi i domeny witryny sieci Web. W przeciwnym razie Android nie można zainstalować aplikacji jako dojście domyślne dla identyfikatora URI; użytkownik musi to zrobić ręcznie.

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>Konfigurowanie opcji filtru

Należy skonfigurować konwersji filtr, który mapuje identyfikatora URI (lub możliwe zestaw identyfikatorów URI) z witryny sieci Web do działania w aplikacji systemu Android. W Xamarin.Android, ta relacja jest ustanawiane przez adorning działania [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). Filtr konwersji musi deklarować następujące informacje:

* **`Intent.ActionView`** &ndash; To spowoduje zarejestrowanie konwersji filtr, aby odpowiadał na żądania, aby wyświetlić informacje
* **`Categories`** &ndash;  Filtr konwersji należy zarejestrować zarówno **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)** i **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)** Aby można było poprawnie Obsługa sieci web identyfikatora URI.
* **`DataScheme`** &ndash; Filtr konwersji musi deklarować `http` i/lub `https`. Są to tylko dwa systemy prawidłowe.
* **`DataHost`** &ndash; Jest to domena, którego pochodzą z identyfikatory URI.
* **`DataPathPrefix`** &ndash; Jest opcjonalna ścieżka do zasobów w sieci Web.
* **`AutoVerify`** &ndash; `autoVerify` Atrybut informuje Android można zweryfikować relacji pomiędzy aplikacją i witryny sieci Web. To zostanie omówiona bardziej poniżej.

Poniższy przykład przedstawia użycie [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) do obsługi łączy z `https://www.recipe-app.com/recipes` i z `http://www.recipe-app.com/recipes`:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe",
              AutoVerify=true)]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android zweryfikuje co hosta, który jest identyfikowany przez filtry konwersji na cyfrowe pliku zasoby w witrynie sieci Web przed rejestracją aplikacji jako domyślny program obsługi dla identyfikatora URI. Wszystkie filtry konwersji musi upłynąć weryfikacji Android mogą nawiązywać aplikację jako domyślny program obsługi.

### <a name="creating-the-digital-assets-link-file"></a>Tworzenie pliku łącza zasobów cyfrowych

System android 6.0 łączenie aplikacji wymaga, sprawdź, czy Android skojarzenia między aplikacją a witryny sieci Web przed ustawieniem aplikacji jako domyślny program obsługi dla identyfikatora URI. Weryfikacji wystąpi podczas instalowania aplikacji. *Łącza zasobów cyfrowych* plik jest plikiem JSON hostowanej przez odpowiednie webdomain(s).

> [!NOTE]
> `android:autoVerify` Atrybut musi być ustawiony przez filtr konwersji &ndash; w przeciwnym razie Android nie przeprowadza weryfikacji.

Umieszcza plik webmastera domeny w lokalizacji **https://domain/.well-known/assetlinks.json**.

Plik zasobów cyfrowych zawiera konieczne meta-data dla systemu Android sprawdzić skojarzenia. **Assetlinks.json** plik ma następujące pary klucz wartość:

* `namespace` &ndash; przestrzeń nazw aplikacji systemu Android.
* `package_name` &ndash; Nazwa pakietu aplikacji systemu Android (zadeklarowane w manifeście aplikacji).
* `sha256_cert_fingerprints` &ndash; odciski palców SHA256 podpisany plik aplikacji. Zobacz przewodnik [znajdowanie MD5 lub SHA1 podpisu Twojego magazynu kluczy](~/android/deploy-test/signing/keystore-signature.md) Aby uzyskać więcej informacji na temat sposobu uzyskania odcisku palca SHA1 aplikacji.

Poniższy fragment jest przykładem **assetlinks.json** z jednej aplikacji na liście:

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"com.example",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

Jest można zarejestrować więcej niż jeden SHA256 odcisku palca w celu obsługi różnych wersji lub kompilacje aplikacji. Ten dalej **assetlinks.json** plik jest przykład rejestrowania wielu aplikacji:

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.puppies.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   },
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.monkeys.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

[Witryny cyfrowe łącza zasobów Google](https://developers.google.com/digital-asset-links/tools/generator) jest narzędziem internetowym, które mogą ułatwić tworzenie i testowanie pliku zasobów cyfrowych.

### <a name="testing-app-links"></a>Testowanie aplikacji łącza

Po wdrożeniu aplikacji łącza, należy przetestować różnych elementów, aby upewnić się, że działają zgodnie z oczekiwaniami.

Istnieje możliwość upewnić się, że sformatowany i hostowanych przy użyciu firmy Google cyfrowe API łącza zasobów, jak pokazano w poniższym przykładzie plik zasobów cyfrowych:

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

Istnieją dwa testy, które można wykonać, aby upewnić się, że filtry konwersji został poprawnie skonfigurowany i czy aplikacja jest ustawiony jako domyślny program obsługi dla identyfikatora URI:

1.  Plik zasobów cyfrowych znajduje się poprawnie, zgodnie z powyższym opisem. Pierwszego testu wyśle opcje, które Android powinna kierować do aplikacji mobilnej. Aplikacji systemu Android należy uruchomić i Wyświetl działania zarejestrowany dla adresu URL. W wierszu polecenia wpisz:

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  Wyświetl istniejące łącze obsługi zasady dla aplikacji zainstalowanych na danym urządzeniu. Następujące polecenie spowoduje zrzutu listę zasad łącze dla każdego użytkownika na urządzeniu z następującymi informacjami. W wierszu polecenia wpisz następujące polecenie:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; Nazwa pakietu aplikacji.
    * **`Domain`** &ndash; Domeny (rozdzielone spacjami), których łącza sieci web zostanie obsłużone przez aplikację
    * **`Status`** &ndash; Jest to bieżący stan łącza obsługi aplikacji. Wartość **zawsze** oznacza, że aplikacja ma `android:autoVerify=true` zadeklarowany i przeszedł weryfikacji systemu. Następuje liczbą szesnastkową reprezentujący preferencji Rejestr systemu Android.

    Na przykład:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Podsumowanie

W tym przewodniku opisano sposób łączenia aplikacji działa w systemie Android w wersji 6.0. Następnie objętych usługą Konfigurowanie aplikacji systemu Android 6.0 obsługuje i reagowanie na łącza do aplikacji. On również omówiona jak przetestować łączenie aplikacji w aplikacji systemu Android.


## <a name="related-links"></a>Linki pokrewne

- [Znajdowanie MD5 lub SHA1 podpisu Twojego magazynu kluczy](~/android/deploy-test/signing/keystore-signature.md)
- [Działania i profile](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Linki zasobów cyfrowych Google](https://developers.google.com/digital-asset-links/)
- [Instrukcja listy Generator i Tester](https://developers.google.com/digital-asset-links/tools/generator)
