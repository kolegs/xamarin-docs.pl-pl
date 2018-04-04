---
title: Przechowywanie i uzyskiwanie dostępu do danych w magazynie Azure
description: Magazyn Azure to rozwiązanie magazynu w chmurze skalowalne może służyć do przechowywania danych niestrukturalnych i strukturalnych. W tym artykule przedstawiono, jak za pomocą platformy Xamarin.Forms do przechowywania tekstu i danych binarnych w usłudze Azure Storage oraz sposobu uzyskiwania dostępu do danych.
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 63afeec81eff350b034e8dd3a13da52801937826
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>Przechowywanie i uzyskiwanie dostępu do danych w magazynie Azure

_Magazyn Azure to rozwiązanie magazynu w chmurze skalowalne może służyć do przechowywania danych niestrukturalnych i strukturalnych. W tym artykule przedstawiono, jak za pomocą platformy Xamarin.Forms do przechowywania tekstu i danych binarnych w usłudze Azure Storage oraz sposobu uzyskiwania dostępu do danych._

## <a name="overview"></a>Omówienie

Magazyn Azure obejmuje cztery usług magazynu:

- Blob Storage. Obiekt blob może być tekstowe lub binarne dane, takie jak kopie zapasowe, maszyny wirtualne, pliki multimedialne lub dokumentów.
- Magazyn tabel jest magazynem pary klucz atrybut NoSQL.
- Magazyn kolejek jest usługą obsługi wiadomości do przetwarzania przepływu pracy i komunikacji między usługami w chmurze.
- Magazyn plików oferuje magazynu udostępnionego przy użyciu protokołu SMB.

Istnieją dwa typy kont magazynu:

- Kont magazynu ogólnego przeznaczenia zapewnia dostęp do usług Azure Storage z jednego konta.
- Konto magazynu obiektów Blob jest specjalne konto magazynu do przechowywania obiektów blob. Ten typ konta jest zalecane, gdy potrzebne do przechowywania danych obiektów blob.

W tym artykule i towarzyszące przykładowej aplikacji pokazuje obraz i tekst plików do magazynu obiektów blob i ich pobierania. Ponadto ilustruje też podczas pobierania listy plików z magazynu obiektów blob i usuwania plików.

Aby uzyskać więcej informacji na temat usługi Azure Storage, zobacz [wprowadzenie do magazynu](https://azure.microsoft.com/documentation/articles/storage-introduction/).

## <a name="introduction-to-blob-storage"></a>Wprowadzenie do magazynu obiektów Blob

Magazyn obiektów blob składa się z trzech składników, które zostały przedstawione na poniższym diagramie:

![](azure-storage-images/blob-storage.png "Pojęcia dotyczące magazynu obiektów blob")

Dostęp do usługi Azure Storage jest za pomocą konta magazynu. Konto magazynu może zawierać nieograniczoną liczbę kontenerów, a kontener może przechowywać nieograniczoną liczbę obiektów blob w granicach pojemności konta magazynu.

Obiekt blob jest typ i rozmiar pliku. Usługa Azure Storage obsługuje trzy typy różnych obiektów blob:

- Blokowe obiekty BLOB są zoptymalizowane pod kątem przesyłania strumieniowego i przechowywania obiektów w chmurze i dobrze nadają się do przechowywania kopii zapasowych, plików multimedialnych, dokumentów itd. Blokowe obiekty BLOB może być do 195Gb.
- Dołącz obiekty BLOB są podobne do blokowych obiektów blob, lecz są zoptymalizowane pod kątem Dołącz operacje, takie jak rejestrowanie. Dołącz obiekty BLOB może być do 195Gb.
- Stronicowe obiekty BLOB są zoptymalizowane pod kątem operacji częstego odczytu/zapisu i są zwykle używane do przechowywania maszyn wirtualnych i ich dyski. Stronicowe obiekty BLOB może mieć maksymalnie 1Tb.

> [!NOTE]
> Należy pamiętać, że konta magazynu obiektów blob obsługują blokowanie i Dołącz obiektów blob, ale nie stronicowych obiektów blob.

Obiekt blob jest przekazane do magazynu Azure i pobierane z usługi Magazyn Azure, jako strumień bajtów. W związku z tym pliki muszą zostać skonwertowane do strumienia bajtów przed przekazywania i przekonwertowane wstecz do ich oryginalnej reprezentacja po pobraniu.

Każdy obiekt, który jest przechowywany w magazynie Azure ma unikatowy adres URL. Nazwa konta magazynu tworzy poddomenę tego adresu i kombinacji form nazwę domeny podrzędnej i domeny *punktu końcowego* dla konta magazynu. Na przykład, jeśli nosi nazwę konta magazynu *mojekontomagazynu*, domyślny punkt końcowy obiektu blob dla konta magazynu jest `https://mystorageaccount.blob.core.windows.net`.

Adres URL do uzyskiwania dostępu do obiektu na koncie magazynu jest tworzony przez dodanie lokalizacji obiektu na koncie magazynu do punktu końcowego. Na przykład adres obiektu blob będzie miał format `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`.

## <a name="setup"></a>Konfiguracja

Włączanie konta usługi Azure Storage w aplikacji platformy Xamarin.Forms proces wygląda następująco:

1. Utwórz konto magazynu. Aby uzyskać więcej informacji, zobacz [Utwórz konto magazynu](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account).
1. Dodaj [biblioteki klienta magazynu Azure](https://www.nuget.org/packages/WindowsAzure.Storage/) do aplikacji platformy Xamarin.Forms.
1. Skonfiguruj parametry połączenia magazynu. Aby uzyskać więcej informacji, zobacz [połączenie z magazynem Azure](#connecting).
1. Dodaj `using` dyrektywy `Microsoft.WindowsAzure.Storage` i `Microsoft.WindowsAzure.Storage.Blob` obszarów nazw do klasy, które będą uzyskiwać dostęp do usługi Azure Storage.

> [!NOTE]
> Gdy w przykładzie użyto projektu udostępnionego dostępu, biblioteki klienta magazynu Azure teraz obsługuje również są używane z projektu przenośnej biblioteki klasy (PCL).

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Połączenie z magazynem Azure

Każde żądanie dotyczące zasobów konta magazynu musi zostać uwierzytelniony. Obiekty BLOB można konfigurować do obsługi uwierzytelniania anonimowego, istnieją dwie metody main, używanych przez aplikację do uwierzytelniania za pomocą konta magazynu:

- Klucz udostępniony. Takie podejście używa usługi Azure Storage konta nazwy i klucza konta dostęp do magazynu usług. Konto magazynu jest przypisane dwa klucze prywatne na tworzenie, który może służyć do uwierzytelniania klucza współużytkowanego.
- Sygnatury dostępu współdzielonego. To jest token, który można dołączyć do adresu URL, który pozwala na delegowany dostęp do zasobów magazynu, z uprawnieniami go określa okres czasu, który jest nieprawidłowy.

Parametry połączenia można określić, które zawierają informacje dotyczące uwierzytelniania wymagany dostęp do zasobów usługi Azure Storage z aplikacji. Ponadto można skonfigurować parametry połączenia do nawiązania połączenia emulatora magazynu Azure w programie Visual Studio.

> [!NOTE]
> Usługa Azure Storage obsługuje protokoły HTTP i HTTPS w parametrach połączenia. Jednak zalecane jest używanie protokołu HTTPS.

### <a name="connecting-to-the-azure-storage-emulator"></a>Łączenie z emulatora magazynu Azure

Emulator usługi Azure Storage zapewnia środowisko lokalne, które emuluje obiektów blob platformy Azure, kolejki i usługi tabeli do celów programistycznych.

Następujące parametry połączenia należy użyć do nawiązania połączenia emulatora magazynu Azure:

```csharp
UseDevelopmentStorage=true
```

Aby uzyskać więcej informacji na temat emulatora magazynu Azure, zobacz [użyć emulatora magazynu Azure do programowania i testowania](https://azure.microsoft.com/documentation/articles/storage-use-emulator/).

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>Łączenie z magazynem platformy Azure przy użyciu klucza wspólnego

Do połączenia z magazynem Azure za pomocą klucza udostępnionego należy użyć następującego formatu ciągu połączenia:

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` należy zastąpić nazwą konta magazynu i `myAccountKey` należy zamienić na jeden z dwóch kluczy dostępu konta.

> [!NOTE]
> Gdy za pomocą udostępnionego klucza uwierzytelniania, nazwę konta i klucz konta będą dystrybuowane do każdej osoby używającej aplikacji, co umożliwi dostęp Pełna odczytu/zapisu do konta magazynu. W związku z tym uwierzytelniania klucza współużytkowanego tylko do celów testowych, a nie dystrybucję kluczy do innych użytkowników.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Nawiązywanie połączenia z magazynem Azure przy użyciu sygnaturę dostępu współdzielonego

Do połączenia z magazynem Azure z SAS należy użyć następującego formatu ciągu połączenia:

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` należy zamienić na adres URL punktu końcowego obiektu blob, a `mySharedAccessSignature` powinna zostać zastąpiona sieci SAS. Sygnatury dostępu Współdzielonego zapewnia protokół punktu końcowego usługi i poświadczeń dostępu do zasobu.

> [!NOTE]
> Uwierzytelniania sygnatury dostępu Współdzielonego jest zalecane w przypadku aplikacji produkcyjnej. Jednak w aplikacji produkcyjnej sygnatury dostępu Współdzielonego powinny zostać pobrane z wewnętrznej bazy danych usługi na żądanie, a nie są powiązane z aplikacją.

Aby uzyskać więcej informacji na temat sygnatury dostępu współdzielonego, zobacz [przy użyciu dostępu sygnatur dostępu Współdzielonego](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

## <a name="creating-a-container"></a>Utworzenie kontenera

`GetContainer` Metoda służy do pobierania odwołanie do kontenera o nazwie, której następnie można pobrać obiekty BLOB w kontenerze lub aby dodać do kontenera obiektów blob. Poniższy kod przedstawia przykład `GetContainer` metody:

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

`CloudStorageAccount.Parse` Metody analizuje parametry połączenia i zwraca `CloudStorageAccount` wystąpienia, który reprezentuje konto magazynu. A `CloudBlobClient` wystąpienia, która umożliwia pobieranie kontenerów i obiektów blob, zostanie utworzona przez `CreateCloudBlobClient` metody. `GetContainerReference` Metoda pobiera określonego kontenera jako `CloudBlobContainer` wystąpienia przed zwróceniem do wywoływania metody. W tym przykładzie nazwa kontenera jest `ContainerType` wartość wyliczenia, konwertowana na ciąg małe litery.

> [!NOTE]
> Nazwy kontenerów muszą być małymi literami i musi zaczynać się literą lub cyfrą. Ponadto one może zawierać tylko litery, cyfry i znak kreski i musi mieć długość od 3 do 63 znaków.

`GetContainer` Metoda jest wywoływana w następujący sposób:

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer` Wystąpienie może być następnie używane do utworzenia kontenera, jeśli jeszcze nie istnieje:

```csharp
await container.CreateIfNotExistsAsync();
```

Domyślnie nowo utworzony kontener jest prywatny. Oznacza to, że aby pobrać obiekty BLOB w kontenerze, należy określić klucz dostępu do magazynu. Informacje o wprowadzaniu obiektów blob w publicznego kontenera, zobacz [utworzyć kontener](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container).

## <a name="uploading-data-to-a-container"></a>Przekazywanie danych do kontenera

`UploadFileAsync` Metoda służy do przekazywania strumienia bajtów danych do magazynu obiektów blob i przedstawiono w poniższym przykładzie:

```csharp
public static async Task<string> UploadFileAsync(ContainerType containerType, Stream stream)
{
  var container = GetContainer(containerType);
  await container.CreateIfNotExistsAsync();

  var name = Guid.NewGuid().ToString();
  var fileBlob = container.GetBlockBlobReference(name);
  await fileBlob.UploadFromStreamAsync(stream);

  return name;
}
```

Po pobraniu odwołanie do kontenera, ta metoda tworzy kontener, jeśli jeszcze nie istnieje. Nowy `Guid` zostanie utworzona jako nazwę obiektu blob unikatowy i odwołanie blokowych obiektów blob jest interpretowana jako `CloudBlockBlob` wystąpienia. Strumień danych jest następnie przekazywane do obiektów blob przy użyciu `UploadFromStreamAsync` metodę, która tworzy obiektu blob, jeśli jeszcze nie istnieje lub zastąpiony, jeśli istnieje.

Zanim można przekazać pliku do obiektu blob magazynu przy użyciu tej metody, najpierw musi zostać przekonwertowany do strumienia bajtów. Zostało to przedstawione w poniższym przykładzie kodu:

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text` Danych jest konwertowany na tablicę bajtów, który jest następnie opakowane jako strumień, który jest przekazywany do `UploadFileAsync` metody.

## <a name="downloading-data-from-a-container"></a>Pobieranie danych z kontenera

`GetFileAsync` Metoda służy do pobierania danych obiektów blob z usługi Azure Storage i przedstawiono w poniższym przykładzie:

```csharp
public static async Task<byte[]> GetFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);

  var blob = container.GetBlobReference(name);
  if (await blob.ExistsAsync())
  {
    await blob.FetchAttributesAsync();
    byte[] blobBytes = new byte[blob.Properties.Length];

    await blob.DownloadToByteArrayAsync(blobBytes, 0);
    return blobBytes;
  }
  return null;
}
```

Po pobraniu odwołanie do kontenera, metoda pobiera odwołanie do obiektu blob dla przechowywanych danych. Jeśli istnieje obiektu blob, jego właściwości są pobierane przez `FetchAttributesAsync` metody. Tablica bajtowa odpowiedni rozmiar jest tworzony, co powoduje pobranie obiektu blob jako tablicę bajtów, który pobiera powrót do wywoływania metody.

Po pobraniu danych bajtowych obiektów blob, muszą zostać skonwertowane do jej oryginalnej reprezentacji. Zostało to przedstawione w poniższym przykładzie kodu:

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

Tablica bajtów są pobierane z usługi Magazyn Azure przez `GetFileAsync` ciąg kodowany w formacie metody, przed konwersją do UTF8.

## <a name="listing-data-in-a-container"></a>Wyświetlanie danych w kontenerze

`GetFilesListAsync` Metoda służy do pobierania listy przechowywany w kontenerze obiektów blob i przedstawiono w poniższym przykładzie:

```csharp
public static async Task<IList<string>> GetFilesListAsync(ContainerType containerType)
{
  var container = GetContainer(containerType);

  var allBlobsList = new List<string>();
  BlobContinuationToken token = null;

  do
  {
    var result = await container.ListBlobsSegmentedAsync(token);
    if (result.Results.Count() > 0)
    {
      var blobs = result.Results.Cast<CloudBlockBlob>().Select(b => b.Name);
      allBlobsList.AddRange(blobs);
    }
    token = result.ContinuationToken;
  } while (token != null);

  return allBlobsList;
}
```

Po pobraniu odwołanie do kontenera, metoda używa do kontenera `ListBlobsSegmentedAsync` metodę, aby pobrać odwołania do obiektów blob w kontenerze. Wyniki zwrócone przez `ListBlobsSegmentedAsync` metody są wyliczane podczas `BlobContinuationToken` wystąpienie nie jest `null`. Każdy obiekt blob jest rzutowane z zwróconego `IListBlobItem` do `CloudBlockBlob` w kolejności dostępu `Name` właściwości obiektu blob, zanim zostanie wartość jest dodawana do `allBlobsList` kolekcji. Raz `BlobContinuationToken` wystąpienie jest `null`zwrócił nazwisko obiektów blob i wykonywania kończy tryb pętli.

## <a name="deleting-data-from-a-container"></a>Usuwanie danych z kontenera

`DeleteFileAsync` Metoda służy do usuwania z kontenera obiektów blob i przedstawiono w poniższym przykładzie:

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

Po pobraniu odwołanie do kontenera, metoda pobiera odwołanie do obiektu blob dla określonego obiektu blob. Obiekt blob jest usuwany z `DeleteIfExistsAsync` metody.

## <a name="summary"></a>Podsumowanie

W tym artykule przedstawiono, jak za pomocą platformy Xamarin.Forms do przechowywania tekstu i danych binarnych w usłudze Azure Storage oraz sposobu uzyskiwania dostępu do danych. Magazyn Azure to rozwiązanie magazynu w chmurze skalowalne może służyć do przechowywania danych niestrukturalnych i strukturalnych.


## <a name="related-links"></a>Linki pokrewne

- [Usługa Azure Storage (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [Wprowadzenie do magazynu](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Jak używać magazynu obiektów Blob z Xamarin](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [Przy użyciu sygnatury dostępu współdzielonego (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage/)
