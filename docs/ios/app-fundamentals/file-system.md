---
title: "Praca w systemie plików"
description: "Xamarin.iOS można użyć tej samej klasy System.IO do pracy z plików i katalogów w systemie iOS, który ma zostać użyty w dowolnej aplikacji .NET. Pomimo znanych klasy i metody, iOS implementuje pewne ograniczenia na plikach, które można utworzyć lub uzyskać dostępu do i udostępnia funkcje specjalne określonych katalogów. W tym artykule opisano te ograniczenia i funkcje oraz pokazano, jak działa dostęp do plików w aplikacji platformy Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 36c793e7a9b7b30bcb0cdf2c7959fd2df36c8775
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-the-file-system"></a>Praca w systemie plików

_Xamarin.iOS można użyć tej samej klasy System.IO do pracy z plików i katalogów w systemie iOS, który ma zostać użyty w dowolnej aplikacji .NET. Pomimo znanych klasy i metody, iOS implementuje pewne ograniczenia na plikach, które można utworzyć lub uzyskać dostępu do i udostępnia funkcje specjalne określonych katalogów. W tym artykule opisano te ograniczenia i funkcje oraz pokazano, jak działa dostęp do plików w aplikacji platformy Xamarin.iOS._

Można użyć Xamarin.iOS i `System.IO` klas w *biblioteki klasy podstawowej platformy .NET (BCL)* uzyskać dostępu do systemu plików z systemem iOS. `File` Klasa umożliwia tworzenie, usuwanie i odczytywać pliki i `Directory` klasa umożliwia tworzenie, usuwanie lub wyliczyć zawartości katalogów. Można również użyć `Stream` podklas, które zapewniają lepszej kontroli nad operacji na plikach (takie jak wyszukiwanie kompresji lub pozycji w pliku).

iOS nakłada pewne ograniczenia dotyczące aplikacji czynności w systemie plików, aby zachować bezpieczeństwo danych aplikacji i chronić użytkowników przed złośliwe aplikacje. Ograniczenia te są częścią *aplikacji piaskownicy* — zestaw reguł, który ogranicza dostęp aplikacji do plików, preferencje, zasoby sieciowe, sprzęt, itp. Aplikacja jest ograniczona do odczytywania i zapisywania plików znajdujących się w jego katalogu macierzystego (zainstalowana lokalizacja); nie ma dostępu do plików w innej aplikacji.

iOS ma także niektóre funkcje specyficzne dla systemu plików: niektórych katalogów wymagają szczególnego traktowania względem kopii zapasowych i aktualizacje i aplikacje mogą również współużytkować plików za pomocą programu iTunes.

W tym artykule omówiono funkcje i ograniczenia systemu IOS system plików szczegółowo i zawiera przykładową aplikację prezentującą wykonać pewne operacje systemu plików prostych przy użyciu platformy Xamarin.iOS:

 [![](file-system-images/05-sampleapp.png "Przykładowe wykonywania pewnych operacji systemu prostego pliku systemu IOS")](file-system-images/05-sampleapp.png#lightbox)

 <a name="General_File_Access" />


## <a name="general-file-access"></a>Dostęp do plików ogólne

Xamarin.iOS umożliwia przy użyciu programu .NET `System.IO` klasy dla operacji systemu plików w systemie iOS.

Poniższe fragmenty kodu przedstawiają pewne typowe operacje na plikach. Znajdziesz je wszystkie poniżej `SampleCode.cs` pliku w przykładowej aplikacji w tym artykule.

 <a name="Working_with_directories" />


### <a name="working-with-directories"></a>Praca z katalogami

Ten kod wylicza podkatalogów w bieżącym katalogu (określonego przez ". /" parametru), czyli lokalizacji pliku wykonywalnego Twojej aplikacji.
Dane wyjściowe będą listę wszystkich plików i folderów, które zostały wdrożone za pomocą aplikacji (wyświetlane w oknie konsoli podczas debugowania).

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

 <a name="Reading_files" />


### <a name="reading-files"></a>Odczytywanie plików

Aby odczytać plik tekstowy, wystarczy pojedynczy wiersz kodu. W tym przykładzie zostanie wyświetlona zawartość pliku tekstowego w oknie danych wyjściowych aplikacji.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

 <a name="XML_Serialization" />


### <a name="xml-serialization"></a>Serializacji XML

Mimo że praca z pełną `System.Xml` przestrzeni nazw wykracza poza zakres tego artykułu, można łatwo deserializacji dokumentu XML z systemu plików za pomocą StreamReader następująco:

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

Zapoznaj się z dokumentacją MSDN dla [System.Xml](http://msdn.microsoft.com/en-us/library/system.xml.aspx) przestrzeni nazw, aby uzyskać więcej informacji na temat [serializacji](http://msdn.microsoft.com/en-us/library/system.xml.serialization.aspx). Należy także przejrzeć [dokumentacji platformy Xamarin.iOS](~/ios/deploy-test/linker.md) na konsolidator — zwykle należy dodać `[Preserve]` atrybutu klasy mają do serializacji.

 <a name="Creating_Files_and_Directories" />


### <a name="creating-files-and-directories"></a>Tworzenie plików i katalogów

Ten przykład przedstawia sposób użycia `Environment` klasy w celu uzyskania dostępu do folderu Dokumenty, gdzie można utworzyć plików i katalogów.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

Tworzenie katalogu jest bardzo podobny proces:

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

Aby uzyskać więcej informacji na temat nazw System.IO zobacz [dokumentacji MSDN](http://msdn.microsoft.com/en-us/library/system.io.aspx).


### <a name="serializing-json"></a>Serializacja Json

Praca z Json danych w aplikacji platformy Xamarin.iOS jest bardzo łatwe przy użyciu [Json.NET](http://www.newtonsoft.com/json) wysokiej wydajności JSON framework .NET pakietu NuGet. Po prostu Dodaj pakiet NuGet do projektu aplikacji: 

[![](file-system-images/json01.png "Dodawanie pakietu NuGet do projektu aplikacji")](file-system-images/json01.png#lightbox)

Następnie Dodaj klasę do działania jako model danych do serializacji/deserializacji (w tym przypadku `Account.cs`):

```csharp
using System;
using System.Collections.Generic;
using Foundation; // for Preserve attribute, which helps serialization with Linking enabled

namespace FileSystem
{
    [Preserve]
    public class Account
    {
        #region Computed Properties
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }
        #endregion

        #region Constructors
        public Account() {

        }
        #endregion
    }
}
```

Na koniec Utwórz wystąpienie `Account` klasy, go serializować danych json i zapisze go w pliku:

```csharp
// Create a new record
var account = new Account(){
    Email = "monkey@xamarin.com",
    Active = true,
    CreatedDate = new DateTime(2015, 5, 27, 0, 0, 0, DateTimeKind.Utc),
    Roles = new List<string> {"User", "Admin"}
};

// Serialize object
var json = JsonConvert.SerializeObject(account, Newtonsoft.Json.Formatting.Indented);

// Save to file
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "account.json");
File.WriteAllText(filename, json);
```
Zapoznaj się Json .NET [dokumentacji](http://www.newtonsoft.com/json/help) Aby uzyskać więcej informacji na temat pracy z danymi json w aplikacji .NET.

<a name="Special_Considerations" />

## <a name="special-considerations"></a>Uwagi

Pomimo podobieństwa między Xamarin.iOS i operacji na plikach .NET, iOS i Xamarin.iOS różnią się od .NET w pewnym sensie ważne.

 <a name="runtimeaccessible" />


### <a name="making-project-files-accessible-at-runtime"></a>Udostępnianie plików projektu w czasie wykonywania

Domyślnie po dodaniu pliku do projektu, go nie będą uwzględniane w zestawie końcowym, a w związku z tym nie będą dostępne dla aplikacji. Aby można było dołączyć plik w zestawie, możesz oznaczyć go z akcją specjalne kompilacji o nazwie zawartość.

Aby oznaczyć pliku do włączenia, kliknij prawym przyciskiem myszy na pliki i wybierz polecenie **Akcja kompilacji &gt; zawartości** w programie Visual Studio dla komputerów Mac. Możesz również zmienić **Akcja kompilacji** w pliku **właściwości** arkusza.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Uwzględniana wielkość liter

Ważne jest, aby dowiedzieć się, że w systemie plików iOS *z uwzględnieniem wielkości liter*. Oznacza to, że nazwy plików i katalogów musi dokładnie odpowiadać — README.txt i readme.txt mogą być uważane za różne nazwy plików.

Może to być mylące dla deweloperów platformy .NET, którzy zapoznali się więcej o systemu plików, który jest *bez uwzględniania wielkości liter*— "Files", "FILES" i "files" czy wszystkie odnoszą się do tego samego katalogu.

Tak mimo że urządzenia z systemem iOS jest uwzględniana wielkość liter i kod powinien być zapisywany z tym pamiętać, jest symulatora systemu iOS nie Uwzględnij wielkość liter domyślnie. Oznacza to, jeśli wielkość liter w filename różni się między nim a odwołania do niego w kodzie, kod może nadal działać w symulatorze, ale aby go spowoduje niepowodzenie rzeczywistego urządzenia. Jest to jedna z przyczyn, dlaczego ważne jest, aby wdrożyć do rzeczywistego urządzenia wczesne i często podczas opracowywania aplikacji systemu iOS.

 <a name="Path_Separator" />


### <a name="path-separator"></a>Separator ścieżki

iOS używa ukośnik "/" jako separatora ścieżki (który jest inny niż Windows, który używa ukośniku odwrotnym "\").

Z powodu tej różnicy mylące, jest dobrym rozwiązaniem jest użycie `System.IO.Path.Combine` metodę, która dostosowuje dla bieżącej platformy, a nie umieszczaj separatorem określonej ścieżki. Jest to prosty krok, który powoduje, że kod przenośną do innych platform.

 <a name="Application_Sandbox" />


## <a name="application-sandbox"></a>Piaskownica aplikacji

Dostęp aplikacji do systemu plików (i innych zasobów, takich jak funkcje sieci i sprzętu) jest ograniczony, ze względów bezpieczeństwa. To ograniczenie jest nazywany *aplikacji piaskownicy*. W systemie plików aplikacji jest ograniczona do tworzenia i usuwania plików i katalogów w jego katalogu macierzystego.

Katalog macierzysty jest unikatową lokalizację w systemie plików, w którym są przechowywane jego danych i aplikacji. Nie można wybrać (ani zmienić) lokalizację katalogu macierzystego dla aplikacji; jednak z systemami iOS i Xamarin.iOS Podaj właściwości i metod do zarządzania plikami i katalogami wewnątrz.

 <a name="The_Application_Bundle" />


## <a name="the-application-bundle"></a>Pakiet aplikacji

*Pakietu aplikacji* jest folder, który zawiera aplikację.
Odróżnić od innych folderach dzięki użyciu sufiks .app dodane do nazwy katalogu. Twojego pakietu aplikacji zawiera pliku wykonywalnego, a cała zawartość (pliki, obrazy, itp.) wymagane dla projektu.

Po przejściu do Twojego pakietu aplikacji w systemie Mac OS, wydaje się z inną ikonę niż widoczne w innych katalogów (i sufiks .app jest ukryty); jest jednak tylko regularne katalogu, w którym system operacyjny są wyświetlane inaczej.

Aby wyświetlić przykładowy kod pakietu aplikacji, kliknij prawym przyciskiem myszy projekt w programie Visual Studio dla komputerów Mac i wybierz **Otwórz Folder zawierający**. Następnie przejdź do **bin/debugowanie/** gdzie powinna być widoczna ikona aplikacji (podobnie jak na poniższym zrzucie ekranu).

 [![](file-system-images/40-bundle.png "Przejdź do bin/Debug, aby znaleźć podobne do tego zrzutu ekranu ikony aplikacji")](file-system-images/40-bundle.png#lightbox)

Kliknij prawym przyciskiem myszy tę ikonę, a następnie wybierz pozycję **Wyświetl zawartość pakietu** do przeglądania zawartości katalogu pakietu aplikacji. Zawartość jest wyświetlana tak samo jak zawartość katalogu regularne, jak pokazano poniżej:

 [![](file-system-images/45-bundle.png "Zawartości pakietu aplikacji")](file-system-images/45-bundle.png#lightbox)

Pakiet aplikacji jest zainstalowanych w symulatorze lub na urządzeniu podczas testowania, a ostatecznie to, co jest przesyłany do firmy Apple w celu włączenia w sklepie z aplikacjami.

 <a name="Application_Directories" />


## <a name="application-directories"></a>Katalogów aplikacji

Gdy aplikacja jest zainstalowana na urządzeniu, system operacyjny tworzy jego katalogu macierzystego i umieszcza Twojego pakietu aplikacji wewnątrz. Kod mogą uzyskiwać dostęp do pakietu aplikacji na odczytywanie danych, ale nic nie powinna być zapisana do tego katalogu głównego jest podpisany, a wszelkie modyfikacje spowoduje unieważnienie aplikacji i uniemożliwić jego uruchomienie.

Tak, mimo że nic nie powinny być zapisywane w katalogu głównym <b>w systemie iOS 7 i wcześniejszych</b> tworzy liczbę katalogów w katalogu głównym aplikacji, które są dostępne do użycia. <b>W systemie iOS 8 są dostępne dla użytkownika katalogów <a href="https://developer.apple.com/library/ios/technotes/tn2406/_index.html" target="_blank">nie znajduje się</a> w katalogu głównym aplikacji</b>.

Poniżej wymieniono tych katalogów oraz ich celów:

&nbsp;

<table>
  <tbody>
    <tr>
      <td>
Katalog </td>
      <td>
Opis </td>
    </tr>
    <tr>
      <td>
        <p>[ApplicationName].app/</p>
      </td>
      <td>
        <p><b>W systemie iOS 7 i wcześniejszych</b> to <code>ApplicationBundle</code> katalogu, w którym przechowywana jest aplikacja pliku wykonywalnego. Struktury katalogów, które są tworzone w aplikacji istnieje w tym katalogu (na przykład obrazów i innych typów plików, które zostały oznaczone jako zasoby w Visual Studio dla projektu Mac).</p>
        <p>Jeśli potrzebujesz dostępu do zawartości plików wewnątrz Twojego pakietu aplikacji, ścieżka do tego katalogu jest dostępna za pośrednictwem <code>NSBundle.MainBundle.BundlePath</code> właściwości.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>Dokumenty /</p>
      </td>
      <td>
        <p>Użyj tego katalogu do przechowywania dokumentów użytkownika i pliki danych aplikacji.</p>
        <p>Zawartość tego katalogu mogą dostępne użytkownika za pośrednictwem udostępniania (mimo że to jest domyślnie wyłączona) plików programu iTunes. Dodaj <code>UIFileSharingEnabled</code> logiczna klucz do pliku Info.plist, aby umożliwić użytkownikom dostęp do tych plików.</p>
        <p>Nawet jeśli aplikacja nie natychmiast włączone udostępnianie plików, należy unikać umieszczania plików, które mają być ukryte przed użytkownicy w tym katalogu (takich jak pliki bazy danych, chyba że chcesz udostępniać je). Tak długo, jak poufne pliki pozostają ukryte, te pliki zostanie nie widoczne (i potencjalnie przeniesiony, zmodyfikowany lub usunięty przez iTunes) po włączeniu udostępniania plików w przyszłych wersjach.</p>
        <p>Można użyć <code>Environment.GetFolderPath
(Environment.SpecialFolder.MyDocuments)</code> metody można uzyskać ścieżki do katalogu dokumentów aplikacji.</p>
        <p>Zawartość tego katalogu kopie zapasowe są wykonywane iTunes.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>Biblioteka /</p>
      </td>
      <td>
        <p>Katalog biblioteki jest dobrym miejscem do przechowywania plików, które nie są tworzone bezpośrednio przez użytkownika, takie jak bazy danych lub inne pliki wygenerowane w aplikacji.
Zawartość tego katalogu nigdy nie są widoczne dla użytkownika za pomocą programu iTunes.</p>
        <p>Można utworzyć własny podkatalogów w bibliotece; istnieją już niektóre systemowy katalogów w tym miejscu należy pamiętać o tym preferencje i pamięci podręczne.</p>
        <p>Zawartość tego katalogu (z wyjątkiem podkatalogu pamięci podręcznych) kopie zapasowe są wykonywane iTunes. Katalogi niestandardowe, które należy utworzyć w bibliotece będą kopii zapasowej.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>Preferencje dotyczące/biblioteki /</p>
      </td>
      <td>
        <p>Pliki preferencji specyficzne dla aplikacji są przechowywane w tym katalogu. Nie bezpośrednio tworzenia tych plików. Zamiast tego należy użyć <code>NSUserDefaults</code> klasy.</p>
        <p>Zawartość tego katalogu kopie zapasowe są wykonywane iTunes.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>Biblioteka/pamięci podręcznych /</p>
      </td>
      <td>
        <p>Katalog pamięci podręcznej jest uruchomienie dobrym miejscem do przechowywania plików danych, które mogą pomóc aplikacji, ale który można łatwo utworzyć ponownie w razie potrzeby. Aplikacji należy utworzyć i usunąć te pliki, zgodnie z potrzebami i mieć możliwość ponownego tworzenia tych plików, jeśli to konieczne. iOS 5 może to również usunięcie tych plików (w obszarze sytuacji bardzo małych magazynu), jednak nie będzie wykonywał, gdy aplikacja jest uruchomiona.</p>
        <p>Zawartość tego katalogu nie kopie zapasowe są wykonywane iTunes, co oznacza, że nie będzie wyświetlany, jeśli użytkownik przywraca urządzenia, a nie mogą być obecne po zainstalowaniu zaktualizowanej wersji aplikacji.</p>
        <p>Na przykład w przypadku, gdy aplikacja nie może połączyć się z siecią, można użyć katalogu pamięci podręcznej do przechowywania danych lub plików, aby zapewnić dobrą środowisko w trybie offline. Aplikację można zapisywać i szybko pobrać te dane podczas oczekiwania na odpowiedzi sieci, ale go nie należy wykonać kopię i łatwo można odzyskać lub ponownie utworzyć po aktualizacji Przywróć lub wersji.</p>
      </td>
    </tr>
    <tr>
      <td>
        <p>TMP /</p>
      </td>
      <td>
        <p>Aplikacje można przechowywać pliki tymczasowe, które są wymagane tylko przez krótki czas, w tym katalogu. Aby zaoszczędzić miejsce na pliki mają zostać usunięte, gdy nie są już wymagane. System operacyjny może również usunięcie plików z tego katalogu, jeśli aplikacja nie jest uruchomiona.</p>
        <p>Zawartość tego katalogu nie jest wykonywana przez program iTunes.</p>
        <p>Na przykład katalogu tmp może służyć do przechowywania plików tymczasowych, które zostaną pobrane do wyświetlenia dla użytkownika (na przykład awatarów Twitter lub załączników wiadomości e-mail), ale które można usunąć, po ich zostały zostały wyświetlić (i ponownie pobrany, jeśli są wymagane w przyszłości ).</p>
      </td>
    </tr>
  </tbody>
</table>

Ten zrzut ekranu przedstawia struktury katalogów, w oknie wyszukiwania:

 [![](file-system-images/08-library-directory.png "Ten zrzut ekranu przedstawia struktura katalogów w oknie wyszukiwania")](file-system-images/08-library-directory.png#lightbox)

 <a name="Accessing_Other_Directories_Programmatically" />

### <a name="accessing-other-directories-programmatically"></a>Uzyskiwanie dostępu do innych katalogów programowo

Wcześniej przykłady katalogów i plików, dostęp do `Documents` katalogu. Można zapisać w innym katalogu należy tworzyć przy użyciu ścieżki ".." składni, jak pokazano poniżej:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

Tworzenie katalogu jest bardzo podobne:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

Ścieżki do `Caches` i `tmp` katalogów można skonstruować następująco:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

 <a name="Sharing_Files_with_the_User_through_iTunes" />


## <a name="sharing-files-with-the-user-through-itunes"></a>Udostępnianie plików użytkownika za pomocą programu iTunes

Użytkownicy mają dostęp do plików w katalogu dokumentów aplikacji, edytując `Info.plist` i tworzenie **aplikacja obsługuje udostępnianie iTunes** (`UIFileSharingEnabled`) wpisu w **źródła** widoku jako przedstawione tutaj:

 [![](file-system-images/09-uifilesharingenabled-plist.png "Dodawanie aplikacji obsługuje iTunes udostępnianie właściwości")](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

Te pliki będą dostępne w programach iTunes, gdy urządzenie jest podłączone i użytkownik wybierze `Apps` kartę. Poniższy zrzut ekranu przedstawia pliki w wybranej aplikacji udostępnianych za pośrednictwem programu iTunes:

 [![](file-system-images/10-itunes-file-sharing.png "Ten zrzut ekranu przedstawia pliki w wybranej aplikacji udostępnianych za pośrednictwem programu iTunes")](file-system-images/10-itunes-file-sharing.png#lightbox)

Użytkownicy mogą uzyskiwać dostęp tylko do elementów najwyższego poziomu, w tym katalogu za pomocą programu iTunes. Zawartość podkatalogów nie widzą (mimo że można je skopiować na swoich komputerach lub usunąć je). Na przykład GoodReader, plików PDF i EPUB mogą być udostępniane z aplikacją, dzięki czemu użytkownicy mogą odczytywać je na swoich urządzeniach z systemem iOS.

Użytkownicy, którzy zmodyfikować zawartość folderu Dokumenty, ich może spowodować problemy, jeśli nie są one dokładne. Aplikację należy to uwzględnić i zapewnienie odporności na destrukcyjnego aktualizacji folderu dokumenty.

Przykładowy kod w tym artykule tworzy plik i folderu w folderze dokumenty (w **SampleCode.cs**) i umożliwia udostępnianie plików w **Info.plist** pliku. Ten zrzut ekranu pokazuje, jak są one wyświetlane w programach iTunes:

 [![](file-system-images/15-itunes-file-sharing-example.png "Ten zrzut ekranu pokazuje, jak pliki są wyświetlane w programach iTunes")](file-system-images/15-itunes-file-sharing-example.png#lightbox)

Zapoznaj się [Praca z obrazami](~/ios/app-fundamentals/images-icons/index.md) artykuł, aby uzyskać informacje na temat ustawienie ikon dla aplikacji i dla wszystkich typów dokumentów niestandardowych tworzenia.

Jeśli `UIFileSharingEnabled` klucz ma wartość FAŁSZ lub nie istnieje, a następnie udostępnianie plików jest domyślnie wyłączona i użytkownicy nie będą mogli wchodzić w interakcje z Twojego Documentsdirectory.

 <a name="Backup_and_Restore" />

## <a name="backup-and-restore"></a>Kopia zapasowa i przywracanie

Gdy urządzenie jest wykonywana kopia zapasowa przez programu iTunes, wszystkie katalogi, które są tworzone w katalogu macierzystym aplikacji zostanie zapisany z wyjątkiem następujących:

-   **[ApplicationName] .app** — pakiet aplikacji *jest* dodawane do kopii zapasowej, ale tylko wtedy, gdy zmieniono pakietu (Jeśli aplikacja jest zainstalowana aktualizacja, na przykład). Ten katalog nie należy modyfikować mimo to, ponieważ jest on podpisany w związku z czym musi pozostać bez zmian po zakończeniu instalacji. 
-   **Biblioteka/pamięci podręcznych** — katalog pamięci podręcznej jest przeznaczony do plików roboczych, które nie są do wykonania kopii zapasowej. 
-   **TMP** — ten katalog jest używany na pliki tymczasowe, które są tworzone i usuwane, gdy nie są już potrzebne, lub dla plików usuwa wymaga miejsca na tym iOS. 


Tworzenie kopii zapasowej dużej ilości danych może zająć dużo czasu. Jeśli zdecydujesz, należy wykonać kopię zapasową wszelkich określonego dokumentu lub danych, aplikacji należy używać tylko dokumenty i biblioteki foldery dla tego. Przejściowa danych lub pliki, które można łatwo pobrać z sieci używać pamięci podręczne lub katalogu tmp.

Należy pamiętać, że iOS "wyczyści" system plików uruchomienie urządzenia krytycznie mała miejsca na dysku. Ten proces spowoduje usunięcie wszystkich plików z biblioteki/pamięci podręcznych i tmp folderu aplikacji, które nie są obecnie uruchomione.

 <a name="Complying_with_iOS5_iCloud_Backup_Restrictions" />

## <a name="complying-with-ios5-icloud-backup-restrictions"></a>Zgodne z iCloud iOS5 ograniczenia kopii zapasowej

Apple wprowadzone *iCloud kopii zapasowej* funkcji z systemem iOS 5. Po włączeniu iCloud kopii zapasowej, wszystkie pliki w katalogu macierzystym aplikacji (z wyłączeniem katalogi, które nie są zwykle kopii zapasowej, np. pakietu aplikacji `Caches` i `tmp`) są przechowywane w kopii zapasowej na serwerach usługi iCloud. Ta funkcja zapewnia użytkownikowi pełną kopię zapasową w przypadku, gdy ich urządzenie jest utraty, kradzieży lub uszkodzony.

Ponieważ iCloud zapewnia tylko 5Gb miejsca "free" do wszystkich użytkowników i aby uniknąć niepotrzebnego przy użyciu przepustowości, Apple oczekuje aplikacjom tylko tworzenia kopii zapasowych podstawowych wygenerowaną przez użytkowników dane. Do wykonania wskazówek magazynu danych z systemem iOS należy ograniczyć ilość danych, który pobiera kopię zapasową przestrzegać następujących elementów:

-  Tylko dane generowane przez użytkownika i przechowywać dane, które nie mogą być ponownie tworzone w katalogu dokumenty (która jest przechowywana kopia zapasowa). 
-  Wszystkie inne dane, które można łatwo ponownie utworzyć lub ponownie pobrana w `Library/Caches` lub `tmp` (który nie jest kopii zapasowej i może być "oczyszczone"). 
-  Jeśli pliki, które może być odpowiednie dla `Library/Caches` lub `tmp` folder, ale nie powinien być "czyszczenia", przechowywać je w innym miejscu (takie jak `Library/YourData` ) i Zastosuj "nie kopii zapasowej" atrybutu, aby zapobiec wykorzystania iCloud ba kopii zapasowej plików ndwidth oraz miejsce do magazynowania. Te dane są nadal używa miejsca na urządzeniu, dlatego należy uważnie możesz nim zarządzać i usuń go, jeśli to możliwe. 

"Nie kopii zapasowej" atrybut jest ustawiony za pomocą `NSFileManager` klasy. Upewnij się, jest klasa `using Foundation` i Wywołaj `SetSkipBackupAttribute` podobnie do następującej:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

Gdy `SetSkipBackupAttribute` jest `true` plik nie zostanie kopii zapasowej, niezależnie od tego, są przechowywane w katalogu (nawet `Documents` katalogu). Można zbadać za pomocą atrybutu `GetSkipBackupAttribute` metoda i można je zresetować przez wywołanie metody `SetSkipBackupAttribute` metody z `false`, podobnie do następującej:

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>Udostępnianie danych między iOS aplikacje i rozszerzeń aplikacji

Ponieważ rozszerzenia aplikacji uruchomione jako część hosta aplikacji (w przeciwieństwie do swoich aplikacji zawierającego), udostępniania danych nie jest automatyczny uwzględniona, więc nie są wymagane dodatkowe czynności. Grupy aplikacji to używa mechanizmu z systemem iOS, aby umożliwić różnych aplikacji do udostępniania danych. Jeśli aplikacje zostały poprawnie skonfigurowane przy prawidłowe uprawnienia i inicjowania obsługi, uzyskać dostęp do udostępnionego katalogu poza ich normalne iOS piaskownicy.

### <a name="configure-an-app-group"></a>Skonfiguruj grupy aplikacji

Lokalizacji udostępnionej została skonfigurowana przy użyciu [grupy aplikacji](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), która jest skonfigurowana w **certyfikaty, identyfikatory & profile** sekcji na [Centrum deweloperów systemu iOS](https://developer.apple.com/devcenter/ios/). Ta wartość musi się też odwoływać w każdym projekcie **Entitlements.plist**.

Aby uzyskać informacje na temat tworzenia i konfigurowania grupy aplikacji, zapoznaj się [możliwości grupy aplikacji](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) przewodnik.

### <a name="files"></a>Pliki

Aplikacja systemu iOS i rozszerzenia można również udostępniać pliki przy użyciu wspólnej ścieżki pliku (podane zostały one prawidłowo skonfigurowany z użyciem prawidłowe uprawnienia i inicjowania obsługi administracyjnej):

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```
> [!IMPORTANT]
> **Uwaga:** Jeśli zwrócony ścieżce grupy `null`, sprawdź konfigurację uprawnień i profilu inicjowania obsługi administracyjnej i upewnij się, że są poprawne.


<a name="Application_Version_Updates" />

## <a name="application-version-updates"></a>Aktualizacje wersji aplikacji

Po pobraniu nowych wersji aplikacji iOS tworzy nowy katalog macierzysty i zapisuje w nim nowy pakiet aplikacji. iOS zostaje następnie przeniesiona następujące foldery z poprzedniej wersji Twojego pakietu aplikacji do katalogu macierzystego:

-  **Dokumenty**
-  **Biblioteka**


Innych katalogów również mogą być kopiowane i umieścić w obszarze katalogu macierzystego, ale ich jest nie ma gwarancji, należy skopiować, więc aplikacja nie należy polegać na takie zachowanie systemu.

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, że operacje systemu plików są prosta z Xamarin.iOS tak jak w przypadku innych aplikacji .NET. Również wprowadzono piaskownicy aplikacji i zbadane konsekwencje zabezpieczeń, które powoduje. Następnie go przedstawione pojęcie pakietu aplikacji. Na koniec wyliczyć katalogów specjalne dostępne dla aplikacji i wyjaśniono ich ról podczas tworzenia kopii zapasowych i uaktualnień aplikacji.


## <a name="related-links"></a>Linki pokrewne

- [FileSystemSampleCode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [Przewodnik programowania w języku systemu plików](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [Rejestrowanie pliku typy obsługuje Twojej aplikacji](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
