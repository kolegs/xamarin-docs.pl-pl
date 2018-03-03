---
title: "Dodawanie analizy z usługami kognitywnych"
description: "Kognitywnych usługi Microsoft są zestaw interfejsów API, zestawy SDK i usług dostępnych dla deweloperów można wprowadzić do swoich aplikacji inteligentne, dodając funkcje, takie jak rozpoznawanie twarzy, rozpoznawanie mowy i zrozumienia języka. Ten artykuł zawiera wprowadzenie do aplikacji przykładowej, który demonstruje sposób wywołania niektóre Microsoft kognitywnych interfejsów API usługi Service."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 650f8dceebb088b3601c21c1f5373fc4ae8c76dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="adding-intelligence-with-cognitive-services"></a>Dodawanie analizy z usługami kognitywnych

_Kognitywnych usługi Microsoft są zestaw interfejsów API, zestawy SDK i usług dostępnych dla deweloperów można wprowadzić do swoich aplikacji inteligentne, dodając funkcje, takie jak rozpoznawanie twarzy, rozpoznawanie mowy i zrozumienia języka. Ten artykuł zawiera wprowadzenie do aplikacji przykładowej, który demonstruje sposób wywołania niektóre Microsoft kognitywnych interfejsów API usługi Service._

## <a name="overview"></a>Omówienie

Przykład towarzyszący jest aplikacja listy todo, która zapewnia funkcje:

- Wyświetlanie listy zadań.
- Dodawanie i edytowanie zadań za pomocą klawiatura programowa lub wykonując rozpoznawania mowy przy użyciu interfejsu API mowy usługi Bing. Aby uzyskać więcej informacji na temat wykonywania rozpoznawania mowy, zobacz [rozpoznawania mowy przy użyciu interfejsu API mowy usługi Bing](speech-recognition.md).
- Pisowni wyboru zadań za pomocą API sprawdzania pisowni usługi Bing. Aby uzyskać więcej informacji, zobacz [sprawdzanie pisowni korzystanie z API sprawdzania pisowni usługi Bing](spell-check.md).
- Wykonuje zadania z języka angielskiego na język niemiecki, przy użyciu interfejsu API translatora. Aby uzyskać więcej informacji, zobacz [tłumaczenie tekstu przy użyciu interfejsu API Translator](text-translation.md).
- Usuwanie zadań.
- Ustaw stan zadania na "gotowe".
- Oceń aplikację do rozpoznawania emocji, przy użyciu interfejsu API rozpoznawania emocji — warstwa. Aby uzyskać więcej informacji, zobacz [rozpoznawania emocji przy użyciu interfejsu API rozpoznawania emocji — warstwa](emotion-recognition.md).

Zadania są przechowywane w lokalnej bazie danych SQLite. Aby uzyskać więcej informacji na temat korzystania z lokalnej bazy danych SQLite, zobacz [Praca z lokalnej bazy danych](~/xamarin-forms/app-fundamentals/databases.md).

`TodoListPage` Jest wyświetlane, gdy aplikacja jest uruchamiana. Ta strona zostanie wyświetlona lista wszystkich zadań w lokalnej bazie danych i umożliwia użytkownikowi utworzenie nowego zadania lub oceń aplikację:

![](images/sample-application-1.png "TodoListPage")

Mogą być tworzone nowe elementy, klikając  *+*  przycisku, który przechodzi do `TodoItemPage`. Ta strona może zostać przesłane do wybierając zadania:

![](images/sample-application-2.png "TodoItemPage")

`TodoItemPage` Pozwala utworzyć edytowany, sprawdzaniu pisowni zadania translacji, zapisywania i usunąć. Rozpoznawanie mowy może służyć do tworzenia lub edytowania zadania. Można to osiągnąć, naciskając przycisk mikrofonu, aby rozpocząć rejestrowanie i naciskając przycisk tego samego po raz drugi Aby zatrzymać rejestrowanie, które wysyła nagrywania API rozpoznawania mowy usługi Bing.

Kliknięcie przycisku smilies `TodoListPage` przechodzi do `RateAppPage`, która jest używana do wykonywania rozpoznawania emocji na obrazie twarzy wyrażenia:

![](images/sample-application-3.png "RateAppPage")

`RateAppPage` Zezwala użytkownikowi na Zrób zdjęcie ich krój, który jest przesyłany do interfejsu API rozpoznawania emocji — warstwa z emocji zwrócone, będzie wyświetlany.

## <a name="understanding-the-application-anatomy"></a>Opis Anatomia aplikacji

Projekt przenośnej biblioteki klasy (PCL) dla aplikacji przykładowej składa się z pięciu foldery główne:

<table>
    <thead>
        <tr><td><strong>Folder</strong></td><td><strong>Cel</strong></td></tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Modele</strong></td>
            <td>Zawiera klasy modelu danych dla aplikacji. Dotyczy to również <code>TodoItem</code> klasy, która modele pojedynczego elementu danych używanych przez aplikację. Folder zawiera także klasy używane do odpowiedzi JSON modelu zwrócony z różnych Microsoft kognitywnych interfejsów API usługi Service.</td>
        </tr>
        <tr>
            <td><strong>Repozytoria</strong></td>
                        <td>Zawiera <code>ITodoItemRepository</code> interfejsu i <code>TodoItemRepository</code> klasy, które są używane do wykonywania operacji bazy danych.</td>
        </tr>
        <tr>
            <td><strong>Usługi</strong></td>
                        <td>Zawiera interfejsy i klasy, które umożliwiają dostęp do różnych Microsoft kognitywnych interfejsów API usługi Service, oraz interfejsy, które są używane przez <code>DependencyService</code> klasy można znaleźć klasy, które implementują interfejsy w projektach platformy.</td>
        </tr>
        <tr>
            <td><strong>Witryny</strong></td>
            <td>Zawiera <code>Timer</code> klasy, która jest używana przez <code>AuthenticationService</code> klasy do odnawiania tokenu dostępu JWT co 9 minut.</td>
        </tr>
        <tr>
            <td><strong>Widoki</strong></td>
            <td>Zawiera strony dla aplikacji.</td>
        </tr>
    </tbody>
</table>

Projekt PCL zawiera również kilka ważnych plików:

<table>
    <thead>
      <tr><td><strong>Plik</strong></td><td><strong>Cel</strong></td></tr>
    <thead>
    <tbody>
        <tr>
            <td><strong>Constants.cs</strong></td>
            <td><code>Constants</code> Klasy, która określa klucze interfejsu API i punktów końcowych dla Microsoft kognitywnych usługi interfejsów API, które są wywoływane. Stałe klucza interfejsu API wymaga aktualizacji dostępu do różnych kognitywnych interfejsów API usługi.
        </tr>
        <tr>
          <td><strong>App.xaml.cs</strong></td>
          <td><code>App</code> Klasy jest odpowiedzialny za tworzenie wystąpień zarówno pierwszej strony wyświetlanej przez aplikację na każdej platformie i <code>TodoManager</code> klasy, która służy do wywoływania operacji w bazie danych.</td>
        </tr>
    </tbody>
</table>

### <a name="nuget-packages"></a>Pakiety NuGet

Przykładowa aplikacja korzysta z następujących pakietów NuGet:

- `Microsoft.Net.Http` — zapewnia `HttpClient` klasa do tworzenia żądań za pośrednictwem protokołu HTTP.
- `Newtonsoft.Json` — zapewnia platformę JSON dla platformy .NET.
- `Microsoft.ProjectOxford.Emotion` — biblioteki klienta do uzyskiwania dostępu do interfejsu API rozpoznawania emocji — warstwa.
- `PCLStorage` — zawiera zestaw interfejsów API We/Wy plików lokalnych i platform.
- `sqlite-net-pcl` — Umożliwia przechowywanie bazy danych SQLite.
- `Xam.Plugin.Media` — Umożliwia pobieranie zdjęć i platform i pobrania interfejsów API.

Ponadto te pakiety NuGet również zainstalować ich zależności.

### <a name="modeling-the-data"></a>Modelowanie danych

Przykładowa aplikacja korzysta z `TodoItem` klasy do modelu danych, który jest wyświetlany w lokalnej bazie danych SQLite. Poniższy kod przedstawia przykład `TodoItem` klasy:

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

`ID` Właściwość jest używana do unikatowo identyfikować każdą `TodoItem` wystąpienie i zostanie nadany SQLite atrybutów, które należy właściwość zwiększanie automatycznie klucza podstawowego w bazie danych.

### <a name="invoking-database-operations"></a>Wywoływanie operacji bazy danych

`TodoItemRepository` Klasa implementuje operacji bazy danych i wystąpienia klasy jest możliwy za pośrednictwem `App.TodoManager` właściwości. `TodoItemRepository` Klasa udostępnia następujące metody do wywołania operacji bazy danych:

- **GetAllItemsAsync** — kopiuje wszystkie elementy z lokalnej bazy danych SQLite.
- **GetItemAsync** — pobiera określony element z lokalnej bazy danych SQLite.
- **SaveItemAsync** — tworzy lub aktualizuje element w lokalnej bazie danych SQLite.
- **DeleteItemAsync** — usuwa określony element z lokalnej bazy danych SQLite.

### <a name="platform-project-implementations"></a>Implementacje projektu platformy

`Services` Folder w projekcie PCL zawiera `IFileHelper` i `IAudioRecorderService` interfejsów, które są używane przez `DependencyService` klasy można znaleźć klasy, które implementują interfejsy w projektach platformy.

`IFileHelper` Interfejs jest implementowany przez `FileHelper` klasy w każdym projekcie platformy. Ta klasa zawiera jedną metodę `GetLocalFilePath`, która zwraca ścieżkę do pliku lokalnego do przechowywania bazy danych SQLite.

`IAudioRecorderService` Interfejs jest implementowany przez `AudioRecorderService` klasy w każdym projekcie platformy. Ta klasa zawiera `StartRecording`, `StopRecording`i obsługi metod, które interfejsów API platformy umożliwia rejestrowanie dźwięku z mikrofonów urządzenia i zapisz go jako plik wav. W systemach iOS `AudioRecorderService` używa `AVFoundation` API rejestrowanie dźwięku. W systemie Android `AudioRecordService` używa `AudioRecord` API rejestrowanie dźwięku. W systemie Windows platformy Uniwersalnej, `AudioRecorderService` używa `AudioGraph` API rejestrowanie dźwięku.

### <a name="invoking-cognitive-services"></a>Wywoływanie usług kognitywnych

Przykładowa aplikacja wywołuje następujących kognitywnych usług firmy Microsoft:

- Mowy usługi Bing interfejsu API. Aby uzyskać więcej informacji, zobacz [rozpoznawania mowy przy użyciu interfejsu API mowy usługi Bing](speech-recognition.md).
- Interfejsu API sprawdzania pisowni usługi Bing. Aby uzyskać więcej informacji, zobacz [sprawdzanie pisowni korzystanie z API sprawdzania pisowni usługi Bing](spell-check.md).
- Tłumaczenie interfejsu API. Aby uzyskać więcej informacji, zobacz [tłumaczenie tekstu przy użyciu interfejsu API Translator](text-translation.md).
- Emocji interfejsu API. Aby uzyskać więcej informacji, zobacz [rozpoznawania emocji przy użyciu interfejsu API rozpoznawania emocji — warstwa](emotion-recognition.md).


## <a name="related-links"></a>Linki pokrewne

- [Dokumentacja kognitywnych usług firmy Microsoft](https://www.microsoft.com/cognitive-services/documentation)
- [Usługi kognitywnych ToDo (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
