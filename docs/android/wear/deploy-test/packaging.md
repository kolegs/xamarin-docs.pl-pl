---
title: Pakowanie aplikacji zużycie
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: af96c0f8cf862b7a208beb5b91ecbb30598b09d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767716"
---
# <a name="packaging-wear-apps"></a>Pakowanie aplikacji zużycie

Aplikacje dla systemu android zużycia są dostarczane z pełnej Android aplikację do dystrybucji w witrynie Google Play. 

## <a name="automatic-packaging"></a>Automatyczne tworzenie pakietów

Począwszy od platformy Xamarin Android 5.0, zużycia aplikacji, jest automatycznie dostarczana jako zasób w aplikacji urządzenia przenośnego po utworzeniu odwołanie do projektu z urządzenia przenośnego projektu do projektu zużycia. Następujące kroki służą do utworzenia tego skojarzenia: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Jeśli aplikacji zużycia nie jest już częścią rozwiązania urządzenia przenośnego, kliknij prawym przyciskiem myszy węzeł rozwiązania i wybierz **Dodaj > Dodaj istniejący projekt...** .

2. Przejdź do **.csproj** pliku aplikacji zużycia, zaznacz go, a następnie kliknij przycisk **Otwórz**. Projekt aplikacji zużycia teraz powinny być widoczne w rozwiązaniu do urządzenia przenośnego.

3. Kliknij prawym przyciskiem myszy **odwołania** a następnie wybierz węzeł **Dodaj odwołanie**.

4. W **Menedżera odwołań** okno dialogowe, zużycia projektu (kliknij, aby dodać zaznaczone), następnie kliknij pozycję Włącz **OK**.

5. Zmień nazwę pakietu w projekcie zużycia tak, aby go jest zgodna z nazwą pakietu Handheld projektu (nazwa pakietu można zmienić w obszarze **właściwości > manifestu systemu Android**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Jeśli aplikacji zużycia nie jest już częścią rozwiązania urządzenia przenośnego, kliknij prawym przyciskiem myszy węzeł rozwiązania i wybierz **Dodaj > Dodaj istniejący projekt...** .

2. Przejdź do **.csproj** pliku aplikacji zużycia, zaznacz go, a następnie kliknij przycisk **Otwórz**. Projekt aplikacji zużycia teraz powinny być widoczne w rozwiązaniu do urządzenia przenośnego.

3. Kliknij prawym przyciskiem myszy węzeł projektu urządzenia przenośnego w rozwiązaniu i kliknij **odwołuje się do edycji...** .

4. W **odwołania Edytuj** okno dialogowe, zużycia projektu (kliknij, aby dodać zaznaczone), następnie kliknij pozycję Włącz **OK**.

5. Zmień nazwę pakietu w projekcie zużycia tak, aby go jest zgodna z nazwą pakietu Handheld projektu (nazwa pakietu można zmienić w obszarze **opcje projektu > aplikacji systemu Android**).

-----


Należy pamiętać, że wystąpi **XA5211** błędu, jeśli nazwa pakietu aplikacji zużycia nie jest zgodna z nazwą pakietu aplikacji urządzenia przenośnego. Na przykład:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

Aby rozwiązać ten problem, Zmień nazwę pakietu aplikacji zużycia, aby odpowiadały one nazwę pakietu aplikacji urządzenia przenośnego.

Po kliknięciu **kompilacji > kompilacji wszystkich**, to skojarzenie wyzwala automatyczne pakowania projektu zużycia w projekcie głównym nr (telefon). Aplikacja zużycia jest automatycznie utworzony i uwzględniane jako zasób w aplikacji urządzenia przenośnego.

Zestaw, który generuje projekt aplikacji zużycia nie jest używany jako odwołanie do zestawu w projekcie nr (telefon). Zamiast tego proces kompilacji wykonuje następujące czynności:

-   Sprawdza, czy pakiet nazwy dopasowania. 

-   Generuje XML i dodaje go do projektu urządzenia przenośnego, aby skojarzyć go z aplikacji zużycia. Na przykład: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   Dodaje aplikację zużycia jako **raw** zasobów do urządzenia przenośnego projektu. 


## <a name="manual-packaging"></a>Tworzenie pakietów ręcznego

Możesz pisać aplikacje systemu Android nosić Xamarin.Android przed w wersji 5.0, ale wykonaj te instrukcje tworzenia pakietów ręcznego dystrybucję aplikacji: 

1. Upewnij się, że Wearable projekt i jego projekty nr (telefon) nie ma tej samej nazwy numer i pakietu wersji.

2. Ręcznie utworzyć Wearable projektu jako **wersji** kompilacji.

3. Ręcznie Dodaj wersji **. APK** z kroku (2) w **zasobów/raw** katalogu projektu nr (telefon).

4. Ręcznie Dodaj nowy zasób XML **Resources/xml/wearable_app_desc.xml** w projekcie urządzenia przenośnego, które odwołuje się do do noszenia **APK** z kroku (3):

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. Ręcznie Dodaj `<meta-data />` elementu do projektu Handheld **AndroidManifest.xml** `<application>` element, który odwołuje się do nowego zasobu XML:

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Zobacz też deweloperskiej Android [instrukcje ręcznego opakowaniach](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually).

