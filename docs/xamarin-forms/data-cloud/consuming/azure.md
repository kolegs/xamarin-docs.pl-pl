---
title: Korzystanie z aplikacji mobilnej Azure
description: W tym artykule, która ma zastosowanie wyłącznie do usługi Azure Mobile Apps, która użycia zaplecza Node.js, wyjaśniono, jak zapytania, wstawiania, aktualizowania i usuwania danych przechowywanych w tabeli w wystąpieniu usługi Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: 2B3EFD0A-2922-437D-B151-4B4DE46E2095
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 73d74b59ef6e59028eec7cad19feec21908b6329
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269047"
---
# <a name="consuming-an-azure-mobile-app"></a>Korzystanie z aplikacji mobilnej Azure

_Aplikacje mobilne platformy Azure umożliwiają opracowywanie aplikacji z zapleczy skalowalne hostowanych w usłudze Azure App Service z obsługą przenośnych uwierzytelniania, synchronizacji w trybie offline i powiadomień wypychanych. W tym artykule, która ma zastosowanie wyłącznie do usługi Azure Mobile Apps, która użycia zaplecza Node.js, wyjaśniono, jak zapytania, wstawiania, aktualizowania i usuwania danych przechowywanych w tabeli w wystąpieniu usługi Azure Mobile Apps._

> [!NOTE]
> Uruchamianie 30 czerwca, wszystkie nowe usługi Azure Mobile Apps zostanie utworzony z protokołem TLS 1.2 domyślnie. Ponadto również zaleca czy istniejące usługi Azure Mobile Apps można tak skonfigurować, aby używać protokołu TLS 1.2. Aby uzyskać informacje dotyczące sposobu wymuszania protokołu TLS 1.2 w aplikacji mobilnej Azure, zobacz [Wymuszanie protokołu TLS 1.2](/azure/app-service/app-service-web-tutorial-custom-ssl#enforce-tls-1112). Aby uzyskać informacje na temat konfigurowania Xamarin projekty do użycia protokołu TLS 1.2, zobacz [zabezpieczeń TLS (Transport Layer) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md).

Aby uzyskać informacje dotyczące sposobu tworzenia wystąpienia usługi Azure Mobile Apps, które mogą być używane przez platformy Xamarin.Forms, zobacz [tworzenie aplikacji platformy Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/). Po wykonaniu tych instrukcji, możesz pobrać przykładową aplikację można skonfigurować do korzystania z wystąpieniem usługi Azure Mobile Apps przez ustawienie `Constants.ApplicationURL` do adresu URL wystąpienia usługi Azure Mobile Apps. Następnie po uruchomieniu aplikacji przykładowej go połączą się z wystąpieniem usługi Azure Mobile Apps, jak pokazano na poniższym zrzucie ekranu:

![](azure-images/portal.png "Przykładowa aplikacja")

Dostęp do usługi Azure Mobile Apps odbywa się za pośrednictwem [zestawu SDK klienta usługi Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), a wszystkie połączenia z przykładowej aplikacji platformy Xamarin.Forms Azure są nawiązywane za pośrednictwem protokołu HTTPS.

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> ATS można można korzystać z, jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

## <a name="consuming-an-azure-mobile-app-instance"></a>Korzystanie z wystąpienia aplikacji mobilnej Azure

[Zestawu SDK klienta usługi Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) zapewnia `MobileServiceClient` klasy, która jest używana przez aplikację platformy Xamarin.Forms uzyskać dostępu do wystąpienia usługi Azure Mobile Apps, jak pokazano w poniższym przykładzie:

```csharp
IMobileServiceTable<TodoItem> todoTable;
MobileServiceClient client;

public TodoItemManager ()
{
  client = new MobileServiceClient (Constants.ApplicationURL);
  todoTable = client.GetTable<TodoItem> ();
}
```

Gdy `MobileServiceClient` jest tworzone wystąpienie, aby zidentyfikować wystąpienia usługi Azure Mobile Apps, należy określić adres URL aplikacji. Tę wartość można uzyskać z poziomu pulpitu nawigacyjnego dla aplikacji mobilnej w [portalu Microsoft Azure](https://portal.azure.com/).

Odwołanie do `TodoItem` można uzyskać tabeli przechowywane w wystąpieniu usługi Azure Mobile Apps, zanim możliwe będzie wykonanie operacji w tej tabeli. Jest to osiągane przez wywołanie metody `GetTable` metoda `MobileServiceClient` wystąpienia, która zwraca `IMobileServiceTable<TodoItem>` odwołania.

### <a name="querying-data"></a>Wykonywanie zapytania na danych

Można pobrać zawartości tabeli przez wywołanie metody `IMobileServiceTable.ToEnumerableAsync` metodę, która asynchronicznie obliczane zapytania i zwraca wyniki. Dane mogą być również filtrowane po stronie serwera, umieszczając w niej `Where` klauzula w zapytaniu. `Where` Klauzuli stosuje wiersza filtrowania predykatu do zapytania dotyczącego tabeli, jak pokazano w poniższym przykładzie:

```csharp
public async Task<ObservableCollection<TodoItem>> GetTodoItemsAsync (bool syncItems = false)
{
  ...
  IEnumerable<TodoItem> items = await todoTable
              .Where (todoItem => !todoItem.Done)
              .ToEnumerableAsync ();

  return new ObservableCollection<TodoItem> (items);
}
```

To zapytanie zwraca wszystkie elementy z `TodoItem` tabeli, którego `Done` właściwości jest równa `false`. Wyniki zapytania następnie są umieszczane w `ObservableCollection` do wyświetlenia.

### <a name="inserting-data"></a>Wstawianie danych

Podczas wstawiania danych w wystąpieniu usługi Azure Mobile Apps, będą automatycznie generowane nowe kolumny w tabeli zgodnie z wymaganiami, pod warunkiem dynamiczne schemat jest włączone w wystąpieniu usługi Azure Mobile Apps. `IMobileServiceTable.InsertAsync` — Metoda służy do wstawiania nowych wierszy danych w określonej tabeli, jak pokazano w poniższym przykładzie:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.InsertAsync (item);
  ...
}
```

Podczas tworzenia żądania wstawiania, Identyfikatora nie może być określony w danych do wystąpienia usługi Azure Mobile Apps. Jeśli żądanie insert zawiera identyfikator `MobileServiceInvalidOperationException` zostanie wygenerowany.

Po `InsertAsync` ukończeniu metody, będzie można wprowadzić identyfikator danych w wystąpieniu usługi Azure Mobile Apps `TodoItem` wystąpienia w aplikacji platformy Xamarin.Forms.

### <a name="updating-data"></a>Aktualizowanie danych

Podczas aktualizowania danych w wystąpieniu usługi Azure Mobile Apps, będą automatycznie generowane nowe kolumny w tabeli zgodnie z wymaganiami, pod warunkiem dynamiczne schemat jest włączone w wystąpieniu usługi Azure Mobile Apps. `IMobileServiceTable.UpdateAsync` Metody używane do aktualizowania istniejących danych z nowymi informacjami, jak pokazano w poniższym przykładzie kodu:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.UpdateAsync (item);
  ...
}
```

Podczas wprowadzania żądanie aktualizacji, należy określić identyfikator, aby wystąpienie usługi Azure Mobile Apps można zidentyfikować dane do zaktualizowania. Ta wartość Identyfikatora jest przechowywany w `TodoItem.ID` właściwości. Jeśli żądanie aktualizacji nie zawiera identyfikator nie istnieje sposób dla wystąpienia usługi Azure Mobile Apps określenie danych, należy zaktualizować, w związku z czym `MobileServiceInvalidOperationException` zostanie wygenerowany.

### <a name="deleting-data"></a>Usuwanie danych

`IMobileServiceTable.DeleteAsync` Metoda służy do usuwania danych z tabeli Azure Mobile Apps, jak pokazano w poniższym przykładzie kodu:

```csharp
public async Task DeleteTaskAsync (TodoItem item)
{
  ...
  await todoTable.DeleteAsync(item);
  ...
}
```

Podczas wprowadzania żądanie usunięcia, należy określić identyfikator, aby sinstance aplikacji mobilnej Azure można zidentyfikować dane są usuwane. Ta wartość Identyfikatora jest przechowywany w `TodoItem.ID` właściwości. Jeśli żądanie usunięcia nie zawiera Identyfikatora, nie istnieje sposób dla wystąpienia usługi Azure Mobile Apps określenie danych, który zostanie usunięty i dlatego `MobileServiceInvalidOperationException` zostanie wygenerowany.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak używać [zestawu SDK klienta usługi Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) do zapytania, wstawiania, aktualizowania i usuwania danych przechowywanych w tabeli w wystąpieniu usługi Azure Mobile apps. Zestaw SDK udostępnia `MobileServiceClient` klasy, która jest używana przez aplikację platformy Xamarin.Forms uzyskać dostępu do wystąpienia usługi Azure Mobile Apps.


## <a name="related-links"></a>Linki pokrewne

- [TodoAzure (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)
- [Tworzenie aplikacji platformy Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)
- [Klientów mobilnych Azure SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
