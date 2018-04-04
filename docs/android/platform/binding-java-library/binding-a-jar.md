---
title: Powiązanie. JAR
description: Ten przewodnik zawiera instrukcje krok po kroku do tworzenia biblioteki powiązania Xamarin.Android Java z systemem Android. Plik JAR.
ms.prod: xamarin
ms.assetid: 93F1D5C5-E2AF-46EA-8460-485A0860C176
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: a6cb08f19aac46ffa089914e28c732660caa52b2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="binding-a-jar"></a>Powiązanie. JAR

_Ten przewodnik zawiera instrukcje krok po kroku do tworzenia biblioteki powiązania Xamarin.Android Java z systemem Android. Plik JAR._

## <a name="overview"></a>Omówienie

Android społeczności oferuje wiele bibliotek języka Java, których można użyć w aplikacji. Te biblioteki Java często są popakowane w. Format JAR (archiwum Java), ale można spakować. JAR w *biblioteka języka Java powiązania* tak, aby jego działanie jest dostępne dla aplikacji platformy Xamarin.Android. Biblioteka języka Java powiązania ma na celu upewnij interfejsów API w. Plik JAR jest dostępna dla kodu C# za pomocą otoki automatycznie wygenerowany kod.

Xamarin narzędzi można wygenerować biblioteki powiązania z co najmniej jednego wejścia. Pliki JAR. Biblioteka powiązania (. Biblioteka DLL zestawu) zawiera następujące elementy: 

-   Zawartość oryginalnej. Pliki JAR.

-   Tego zawijania Java odpowiednich typów w ramach typy zarządzane można wywołać otoki (MCW), które są C#. Pliki JAR.

Wygenerowany kod MCW używa JNI (interfejs natywny Java) do przekazywania wywołania interfejsu API do podstawowych. Plik JAR. Dla każdego można utworzyć powiązania biblioteki. Plik JAR, który pierwotnie był przeznaczony do użytku z systemem Android (Zauważ, że narzędzia Xamarin nie obsługuje obecnie powiązanie biblioteki z systemem innym niż - Android Java). Można również zdecydować się na tworzenie biblioteki powiązania bez uwzględniania zawartości. JAR pliku tak, aby biblioteka DLL ma zależność. JAR w czasie wykonywania.

W tym przewodniku możemy czynności w podstawy tworzenia biblioteki powiązania z jednym. Plik JAR. Firma Microsoft będzie ilustrują z przykładem, gdzie wszystko odbędzie się prawo &ndash; gdzie dostosowanie nie lub debugowania powiązań jest wymagana. 
[Tworzenie powiązania przy użyciu metadanych](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) oferuje przykład bardziej zaawansowanym scenariuszu, gdy proces wiązania nie jest całkowicie automatyczne i niektóre ilość ręcznej interwencji jest wymagana. Omówienie Java biblioteki powiązanie ogólnie (za pomocą przykładowego kodu podstawowych), zobacz [powiązanie biblioteka języka Java](~/android/platform/binding-java-library/index.md). 

 
## <a name="walkthrough"></a>Wskazówki

W następujące wskazówki, utworzymy biblioteki powiązania [Picasso](http://square.github.io/picasso/), popularnych Android. JAR dostarczający obrazu podczas ładowania i buforowanie funkcji. Użyjemy poniższych kroków można powiązać **picasso 2.x.x.jar** można utworzyć nowego zestawu .NET, który możemy użyć w projekcie platformy Xamarin.Android: 

1. Utwórz nowy projekt języka Java powiązania biblioteki.

2. Dodaj. Plik JAR do projektu.

3. Ustawić akcji kompilacji odpowiednie. Plik JAR.

4. Wybierz pozycję platformy docelowej. Obsługuje JAR.

5. Tworzenie biblioteki powiązania.

Po utworzyliśmy biblioteki powiązań, firma Microsoft polega na opracowywaniu małych aplikacji systemu Android, który pokazuje możliwości do wywoływania interfejsów API w bibliotece powiązania. W tym przykładzie chcemy uzyskać dostępu do metody **picasso 2.x.x.jar**:

```java
package com.squareup.picasso

public class Picasso
{ 
    ...
    public static Picasso with (Context context) { ... };
    ...
    public RequestCreator load (String path) { ... };
    ...
}

```csharp
After we generate a Bindings Library for **picasso-2.x.x.jar**,
we can call these methods from C#. For example:

```csharp
using Com.Squareup.Picasso;
...
Picasso.With (this)
    .Load ("http://mydomain.myimage.jpg")
    .Into (imageView);

```


### <a name="creating-the-bindings-library"></a>Tworzenie biblioteki powiązania

Przed rozpoczęciem poniższe kroki, Pobierz [picasso 2.x.x.jar](http://repo1.maven.org/maven2/com/squareup/picasso/picasso/2.5.2/picasso-2.5.2.jar).

Najpierw utwórz nowy projekt biblioteki powiązania. W programie Visual Studio dla komputerów Mac lub Visual Studio, Utwórz nowe rozwiązanie i wybierz *biblioteki systemu Android powiązania* szablonu. (Zrzuty ekranu w tym przewodniku użyć programu Visual Studio, ale programu Visual Studio for Mac jest bardzo podobny). Nazwa rozwiązania **JarBinding**: 

[![Tworzenie projektu biblioteki JarBinding](binding-a-jar-images/01-new-bindings-library-sml.png)](binding-a-jar-images/01-new-bindings-library.png#lightbox)

Szablon zawiera **Jars** folder, w którym można dodać użytkownika. JAR(s) do projektu biblioteki powiązania. Kliknij prawym przyciskiem myszy **Jars** i wybierz polecenie **Dodaj > istniejący element**: 

[![Dodaj istniejący element](binding-a-jar-images/02-add-existing-item-sml.png)](binding-a-jar-images/02-add-existing-item.png#lightbox)

Przejdź do **picasso 2.x.x.jar** wcześniej pobrany plik, zaznacz go i kliknij **Dodaj**: 

[![Wybierz plik jar, a następnie kliknij przycisk Dodaj](binding-a-jar-images/03-select-jar-file-sml.png)](binding-a-jar-images/03-select-jar-file.png#lightbox)

Sprawdź, czy **picasso 2.x.x.jar** został pomyślnie dodany do projektu: 

[![JAR dodany do projektu](binding-a-jar-images/04-jar-added-sml.png)](binding-a-jar-images/04-jar-added.png#lightbox)

Podczas tworzenia projektu biblioteki powiązania Java, należy określić czy. JAR jest osadzony w bibliotece powiązania lub umieszczane w pakietach oddzielnie. Aby to zrobić, należy określić jedną z następujących *akcji*: 

-   **EmbeddedJar** &ndash; . JAR zostanie osadzony w bibliotece powiązania.

-   **InputJar** &ndash; . JAR będą przechowywane oddzielnie od biblioteki powiązania.

Zazwyczaj **EmbeddedJar** Akcja kompilacji, aby. JAR jest automatycznie umieszczone w bibliotece powiązania. Jest to najprostsza opcja &ndash; kodu bajtowego Java w. JAR jest konwertowany na kod bajtowy Dex i jest osadzony (wraz z zarządzanych wywoływane otoki) do sieci APK. Jeśli chcesz zachować. JAR niezależnie od biblioteki powiązań, możesz użyć **InputJar** opcja; upewnij się, że. Plik JAR jest dostępny na urządzeniu, który uruchamia aplikację. 

Akcja kompilacji ustawioną **EmbeddedJar**: 

[![Wybierz akcję kompilacji EmbeddedJar](binding-a-jar-images/05-embeddedjar-sml.png)](binding-a-jar-images/05-embeddedjar.png#lightbox)

Następnie otwórz właściwości, aby skonfigurować projekt *platformy docelowej*. Jeśli. JAR używa żadnych API systemu Android, ustaw docelową platformę na poziom interfejsu API. JAR oczekuje. Zazwyczaj dewelopera. Plik JAR będzie wskazać poziom interfejsu API (lub poziomy) który. JAR jest zgodny z. (Aby uzyskać więcej informacji o ustawianiu platformy docelowej i poziomy interfejsu API systemu Android w ogólności, zobacz [poziomy interfejsu API systemu Android opis](~/android/app-fundamentals/android-api-levels.md).)

Ustaw docelowego interfejsu API poziom biblioteki powiązania (w tym przykładzie używamy poziom interfejsu API 19): 

[![Docelowy poziom interfejsu API do interfejsu API 19](binding-a-jar-images/06-set-target-framework-sml.png)](binding-a-jar-images/06-set-target-framework.png#lightbox)


Na koniec Utwórz bibliotekę powiązania. Mimo że niektóre ostrzeżenia mogą być wyświetlane, projektu biblioteki wiązania należy pomyślnie kompilacji i utworzenie danych wyjściowych. Biblioteki DLL w następującej lokalizacji: **JarBinding/bin/Debug/JarBinding.dll**
    


### <a name="using-the-bindings-library"></a>Za pomocą biblioteki powiązania

Aby korzystać z tego. Biblioteki DLL w aplikacji platformy Xamarin.Android, wykonaj następujące czynności:

1.  Dodaj odwołanie do biblioteki powiązania.

2.  Wykonywanie wywołań do. JAR za pomocą zarządzanego wywoływane otoki. 

W poniższych krokach, utworzymy minimalnego aplikację, która korzysta z biblioteki powiązania, aby pobrać i wyświetlić obraz w `ImageView`; "lifting ciężki" odbywa się przez kod, który znajduje się w. Plik JAR. 

Najpierw utwórz nową aplikację platformy Xamarin.Android, który wykorzystuje biblioteki powiązania. Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodawanie nowego projektu**; Nazwa nowego projektu **BindingTest**. W tym samym rozwiązaniu jako biblioteki powiązania tworzymy tej aplikacji w celu uproszczenia tego przewodnika; Aplikacja, która korzysta z biblioteki powiązania zamiast tego można jednak znajdować się w inne rozwiązanie: 

[![Dodaj nowy projekt BindingTest](binding-a-jar-images/07-add-new-project-sml.png)](binding-a-jar-images/07-add-new-project.png#lightbox)

Kliknij prawym przyciskiem myszy **odwołania** węzła **BindingTest** projekt i wybierz **Dodawanie odwołania...** :

[![Prawo Dodaj odwołanie](binding-a-jar-images/08-add-reference.png)](binding-a-jar-images/08-add-reference.png#lightbox)

Sprawdź **JarBinding** wcześniej utworzony projekt i kliknij przycisk **OK**:

[![Wybierz projekt JarBinding](binding-a-jar-images/09-choose-jar-binding-sml.png)](binding-a-jar-images/09-choose-jar-binding.png#lightbox)

Otwórz **odwołania** węzła **BindingTest** projektu i sprawdź, czy **JarBinding** występuje odwołanie: 

[![JarBinding jest wyświetlany w obszarze odwołań](binding-a-jar-images/10-references-shows-jarbinding-sml.png)](binding-a-jar-images/10-references-shows-jarbinding.png#lightbox)

Modyfikowanie **BindingTest** układu (**Main.axml**), aby pojedynczy `ImageView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/imageView" />
</LinearLayout>
```

Dodaj następujące `using` instrukcji **MainActivity.cs** &ndash; dzięki temu można łatwo uzyskiwać dostęp do metody opartej na języku Java `Picasso` klasy, która znajduje się w bibliotece powiązania:

```csharp
using Com.Squareup.Picasso;
```

Modyfikowanie `OnCreate` metodę, tak że używa `Picasso` klasy załadować obrazu spod adresu URL i wyświetlić je w `ImageView`: 

```csharp
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
        ImageView imageView = FindViewById<ImageView>(Resource.Id.imageView);

        // Use the Picasso jar library to load and display this image:
        Picasso.With (this)
            .Load ("http://i.imgur.com/DvpvklR.jpg")
            .Into (imageView);
    }
}
```

Kompilowanie i uruchamianie **BindingTest** projektu. Aplikacja zostanie uruchomione, i z niewielkim opóźnieniem (w zależności od warunki sieciowe), należy pobrać i wyświetlić obraz podobny do następującego zrzutu ekranu:

[![Zrzut ekranu BindingTest uruchomiona](binding-a-jar-images/11-result-sml.png)](binding-a-jar-images/11-result.png#lightbox)

Gratulacje! Biblioteka języka Java pomyślnie zostały powiązane. JAR i użyć go w aplikacji platformy Xamarin.Android.
 
 
## <a name="summary"></a>Podsumowanie

W tym przewodniku utworzyliśmy Biblioteka powiązań dla innych firm. Plik JAR dodane biblioteki powiązania do aplikacji minimalnego testu, a następnie uruchomiono aplikację w celu zweryfikowania, że naszego kodu C# można wywołać kodu języka Java znajdującej się w. Plik JAR. 



## <a name="related-links"></a>Linki pokrewne

- [Kompilowanie biblioteki powiązania Java (klip wideo)](https://university.xamarin.com/classes#10090)
- [Tworzenie powiązań biblioteki języka Java](~/android/platform/binding-java-library/index.md)
