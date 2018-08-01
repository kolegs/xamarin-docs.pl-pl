---
title: 'Witaj, Android Wieloekranowy: Nowości'
description: W tym przewodniku dwuczęściową Phoneword aplikacji w warstwie podstawowa (utworzonych w Hello, przewodnik Android) jest rozszerzyć, aby obsłużyć drugi ekranu. Na bieżąco wprowadzono podstawowe bloki konstrukcyjne aplikacji systemu Android. Bardziej zgłębić temat w architekturze systemu Android został uwzględniony w celu lepszego zrozumienia struktury aplikacji dla systemu Android i funkcji.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E4150036-7760-4023-BD33-B7BDE7B7AF5B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f1c19d43aa1f9010307df3fb954ac1029221ccd4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767742"
---
# <a name="hello-android-multiscreen-deep-dive"></a>Witaj, Android Wieloekranowy: Nowości

_W tym przewodniku dwuczęściową Phoneword aplikacji w warstwie podstawowa (utworzonych w Hello, przewodnik Android) jest rozszerzyć, aby obsłużyć drugi ekranu. Na bieżąco wprowadzono podstawowe bloki konstrukcyjne aplikacji systemu Android. Bardziej zgłębić temat w architekturze systemu Android został uwzględniony w celu lepszego zrozumienia struktury aplikacji dla systemu Android i funkcji._

## <a name="hello-android-multiscreen-deep-dive"></a>Witaj, Android Wieloekranowy nowości

W [Hello, Android Wieloekranowy szybkiego startu](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), wbudowane i uruchomiono pierwszej aplikacji platformy Xamarin.Android wielu ekranu.
Teraz nadszedł czas na opracowanie lepiej zrozumieć architektura i nawigacji dla systemu Android, dzięki czemu można tworzyć bardziej złożone aplikacje.

W tym przewodniku będzie Eksploruj bardziej zaawansowane Android architektura jako Android *bloków konstrukcyjnych aplikacji* zostały wprowadzone. Android nawigacja z użyciem *intencje* jest opcje zostały opisane nawigacji sprzętu wyjaśniono i Android. Kto ma zostać dodany do aplikacji Phoneword są rozcięta podczas opracowywania więcej holistyczny widok relacji aplikacji z systemu operacyjnego i innymi aplikacjami.


## <a name="android-architecture-basics"></a>Podstawowe informacje dotyczące architektury systemu android

W [Hello, Android nowości](~/android/get-started/hello-android/hello-android-deepdive.md), wiesz, że aplikacje systemu Android są programy unikatowy, ponieważ mają jeden punkt wejścia. Zamiast tego systemu operacyjnego (lub inną aplikację) uruchamia jednego z zarejestrowanych działania aplikacji, która z kolei spowoduje uruchomienie procesu dla aplikacji. Ta nowości w architekturze systemu Android rozszerza Twojej zrozumienie, jak Android aplikacji są wykonane przez wprowadzenie Android bloków konstrukcyjnych aplikacji i ich funkcje.


### <a name="android-application-blocks"></a>Bloki aplikacji systemu android

Aplikacja systemu Android składa się z kolekcją specjalne klasy systemu Android o nazwie *bloki aplikacji* powiązane razem z dowolną liczbę zasobów aplikacji — obrazy, kompozycje, klas pomocniczych, itp. &ndash; te są koordynowane przez plik XML o nazwie *manifestu systemu Android*.

Bloki aplikacja tworzą szkielet aplikacji systemu Android, ponieważ umożliwiają one wykonywanie czynności, które normalnie nie można wykonać za pomocą zwykłej klasy. Dwie najważniejsze te są _działania_ i _usług_:

-   **Działanie** &ndash; działania odpowiada ekranu z interfejsem użytkownika i jest podobny do strony sieci web w aplikacji sieci web. Na przykład w aplikacji źródła wiadomości, ekran logowania będzie pierwsze działanie, przewijanej listy elementów wiadomości będzie innego działania, a strona szczegółów dla każdego elementu byłaby innej. Dowiedz się więcej na temat działania w [cyklu życia działania](~/android/app-fundamentals/activity-lifecycle/index.md) przewodnik.

-   **Usługa** &ndash; usług dla systemu Android obsługuje działania przejęcia długotrwałych zadań i ich działania w tle. Usługi nie ma interfejsu użytkownika i są używane do obsługi zadań, które nie są powiązane ekrany &ndash; na przykład odtwarzanie utworu w tle lub wysyłanie zdjęć do serwera. Aby uzyskać więcej informacji na temat usług, zobacz [tworzenie usług](~/android/app-fundamentals/services/index.md) i [Android usług](~/android/app-fundamentals/services/index.md) przewodników.


Aplikacji systemu Android nie mogą używać wszystkich typów bloków i często ma kilka bloków jednego typu. Na przykład aplikacja Phoneword z [Hello, Android szybkiego startu](~/android/get-started/hello-android/hello-android-quickstart.md) został składać się tylko jedno działanie (ekran) i niektórych plików zasobów. Aplikacja odtwarzacza proste utworów muzycznych mogą mieć kilka działań i usługi dla odtwarzania muzyki, gdy aplikacja jest w tle.

### <a name="intents"></a>Opcje

Jest podstawową koncepcją innego w aplikacjach systemu Android *zamiar*.
Jest zaprojektowana dla systemu android *zasadą najniższych uprawnień* &ndash; aplikacje mają dostęp tylko do bloków wymagają one działać i one mieć ograniczony dostęp do bloków, które tworzą system operacyjny lub inne aplikacje. Podobnie, bloki są *luźno połączonych* &ndash; są one przeznaczone do małego wiedzę na temat i ograniczony dostęp do innych bloków (nawet bloków, które są częścią tej samej aplikacji).

Do komunikacji, bloki aplikacji wysyłać asynchroniczne o nazwie *intencje* i z powrotem. Profile zawierają informacje dotyczące odbierania bloku i czasami niektórych danych. Celem wysyłane z jednego wyzwalacze składnika aplikacji coś w inny składnik aplikacji, powiązanie dwa składniki aplikacji, dzięki czemu komunikacji. Wysyłając intencje i z powrotem, możesz uzyskać bloków do koordynowania złożonych akcje, takie jak uruchomienie aplikacji aparatu wziąć i zapisać, zbieranie informacji o lokalizacji lub przechodzenie z jednego ekranu do następnego.


### <a name="androidmanifestxml"></a>AndroidManifest.XML

Po dodaniu blok do aplikacji, jest on zarejestrowany specjalny plik XML o nazwie **manifestu systemu Android**. Manifest przechowuje informacje o wszystkich bloków aplikacji w aplikacji, a także wymagania dotyczące wersji, uprawnienia i połączone biblioteki &ndash; wszystko, co powinna wiedzieć aplikacji do uruchamiania systemu operacyjnego. **Manifestu systemu Android** działa także w przypadku działań i opcji, aby kontrolować, jakie akcje są odpowiednie dla danego działania. Te zaawansowane funkcje manifestu systemu Android są objęte [Praca z manifestu systemu Android](~/android/platform/android-manifest.md) przewodnik.

W wersji jednym ekranie Phoneword aplikacji, tylko jedno działanie zamierzone jednym i `AndroidManifest.xml` były używane obok dodatkowych zasobów, takich jak ikony. W wersji wielu ekranu Phoneword dodano dodatkowe działania; został on uruchamiany z pierwsze działanie przy użyciu zamiaru. Następna sekcja opisuje sposób intencje pomoc, aby utworzyć nawigacji w aplikacjach systemu Android.

## <a name="android-navigation"></a>Nawigacji dla systemu android

Opcje były używane do przechodzenia między ekranami. Nadszedł czas, aby przejść do tego kodu intencje pracy i zrozumieć ich roli w nawigacji dla systemu Android.


### <a name="launching-a-second-activity-with-an-intent"></a>Uruchamianie drugi działania z celem

W aplikacji Phoneword celem był używany do uruchamiania drugi ekranu (działanie). Rozpocznij od utworzenia celem, przekazując bieżącego *kontekstu* (`this`odwołujący się do bieżącego **kontekstu**) i typ bloku aplikacji, którego szukasz (`TranslationHistoryActivity`):

```csharp
Intent intent = new Intent(this, typeof(TranslationHistoryActivity));
```

**Kontekstu** jest interfejsem do globalnego informacji o środowisku aplikacji &ndash; umożliwia nowo tworzonych obiektów wiedzieć, co się dzieje z aplikacją. Jeśli myślisz o zamiar jako wiadomości, czy podajesz nazwę adresata wiadomości (`TranslationHistoryActivity`) i adres odbiorcy (`Context`).

Android udostępnia opcję, aby dołączyć proste danych do celem (złożone danych różni się). W tym przykładzie Phoneword `PutStringArrayExtra` będzie używany do dołączania do zamiar listy numerów telefonów i `StartActivity` nazywa się na odbiorcy celem. Kompletny kod wygląda następująco:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", _phoneNumbers);
    StartActivity(intent);
};
```


## <a name="additional-concepts-introduced-in-phoneword"></a>Dodatkowe założenia Phoneword

Aplikacja Phoneword wprowadzono kilka koncepcji, które nie są uwzględnione w tym przewodniku. Pojęcia te obejmują:

**Zasoby ciągu** &ndash; aplikacji w Phoneword, tekst `TranslationHistoryButton` ustawiono `"@string/translationHistory"`. `@string` Składni oznacza, że wartość ciągu jest przechowywane w _pliku zasobów ciągu_, **Strings.xml**. Długość następującej wartości `translationHistory` ciąg został dodany do **Strings.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Call History</string>
</resources>
```

Aby uzyskać więcej informacji na zasoby ciągów i innych zasobów systemu Android, zapoznaj się [Android zasoby przewodnik](~/android/app-fundamentals/resources-in-android/index.md).

**ListView i ArrayAdapter** &ndash; A _ListView_ to składnik interfejsu użytkownika, który umożliwia łatwe do prezentowania przewijanej listy wierszy. A `ListView` wymaga wystąpienia _karty_ ze źródłem go z danymi zawartymi w widokach wiersza. Następujący wiersz kodu zostało użyte do wypełnienia interfejsu użytkownika `TranslationHistoryActivity`:

```csharp
this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
```

Widokach listy i karty wykraczają poza zakres tego dokumentu, ale zostały one omówione w bardzo kompleksowe [widokach listy i karty](~/android/user-interface/layouts/list-view/index.md) przewodnik.
[Wypełnianie ListView z danych](~/android/user-interface/layouts/list-view/populating.md) dotyczy w szczególności za pomocą wbudowanych `ListActivity` i `ArrayAdapter` klasy, aby utworzyć i wypełnić `ListView` bez definiowania układu niestandardowego, tak jak została wykonana w przykładzie Phoneword.


## <a name="summary"></a>Podsumowanie

Gratulacje, zakończyła się pierwszym ekranie wielu aplikacji systemu Android! W tym przewodniku wprowadzone *bloków konstrukcyjnych aplikacji systemu Android* i *intencje* i umożliwia tworzenie wielu ekranowanej aplikacji systemu Android. Masz teraz solidną podstawę, należy rozpocząć tworzenie aplikacji platformy Xamarin.Android.

Następnie dowiesz się, czy tworzenie wieloplatformowych aplikacji za pomocą platformy Xamarin w [prowadzi tworzenie wieloplatformowych aplikacji](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
