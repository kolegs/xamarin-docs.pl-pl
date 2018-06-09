---
title: Synchronizowanie danych w trybie Offline z usługi Azure Mobile Apps
description: W tym artykule opisano sposób dodawania funkcji synchronizacji w trybie offline do aplikacji platformy Xamarin.Forms, która korzysta z poziomu zaplecza Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: e8b0eeeb4f0033fccd7a61b4acb286bb8457e6c2
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243600"
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Synchronizowanie danych w trybie Offline z usługi Azure Mobile Apps

_Synchronizacja w trybie offline umożliwia użytkownikom na interakcję z aplikacji mobilnej, przeglądanie, dodawanie lub modyfikowanie danych, nawet jeśli nie ma połączenia sieciowego. Zmiany są przechowywane w lokalnej bazie danych, a gdy urządzenie jest w trybie online, zmiany można synchronizować z wystąpieniem usługi Azure Mobile Apps. W tym artykule opisano sposób dodawania funkcji synchronizacji w trybie offline do aplikacji platformy Xamarin.Forms._

## <a name="overview"></a>Omówienie

[Zestawu SDK klienta usługi Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) zapewnia `IMobileServiceTable` interfejs, który reprezentuje operacje, które można wykonywać w tabelach przechowywanych w wystąpieniu usługi Azure Mobile Apps. Te operacje nawiązać bezpośrednie połączenie z wystąpieniem usługi Azure Mobile Apps i zakończy się niepowodzeniem, jeśli urządzenie przenośne nie dysponują połączeniem sieciowym.

Aby zapewnić obsługę synchronizacji w trybie offline, obsługuje zestawu SDK klienta usługi Azure Mobile *synchronizacji tabel*, które są dostarczane przez `IMobileServiceSyncTable` interfejsu. Ten interfejs zawiera samą tworzenia, odczytu, aktualizacji, operacji usuwania (CRUD) jako `IMobileServiceTable` interfejsu, ale operacje odczytywanie i zapisywanie do lokalnego magazynu. Magazyn lokalny nie jest wypełniane przy użyciu nowych danych z wystąpienia usługi Azure Mobile Apps momentu wywołania *ściągania* danych. Podobnie nie jest wysyłana danych do wystąpienia usługi Azure Mobile Apps momentu wywołania *wypychania* zmian lokalnych.

Synchronizacja w trybie offline obejmuje również obsługę wykrywania konfliktów ten sam rekord został zmieniony w magazynie lokalnym i w wystąpieniu usługi Azure Mobile Apps i rozwiązywania konfliktów niestandardowych. Konflikty może być obsługiwany w magazynie lokalnym lub w wystąpieniu usługi Azure Mobile Apps.

Aby uzyskać więcej informacji o synchronizacji w trybie offline, zobacz [synchronizacji danych w trybie Offline w usłudze Azure Mobile Apps](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) i [włączenia synchronizacji w trybie offline dla aplikacji mobilnej platformy Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/).

## <a name="setup"></a>Konfiguracja

Proces synchronizacji w trybie offline między aplikacją platformy Xamarin.Forms i wystąpienie usługi Azure Mobile Apps integrowanie wygląda następująco:

1. Utwórz wystąpienie usługi Azure Mobile Apps. Aby uzyskać więcej informacji, zobacz [korzystanie z aplikacji mobilnej Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Dodaj [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) pakiet NuGet do wszystkich projektów w rozwiązaniu platformy Xamarin.Forms.
1. (Opcjonalnie) Włączanie uwierzytelniania w wystąpieniu usługi Azure Mobile Apps i aplikacji platformy Xamarin.Forms. Aby uzyskać więcej informacji, zobacz [uwierzytelniania użytkowników z usługą Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Poniższa sekcja instrukcje dodatkowe ustawienia dotyczące konfigurowania systemu Windows platformy Uniwersalnej projekty do użycia pakietu Microsoft.Azure.Mobile.Client.SQLiteStore NuGet. Nie dodatkowe ustawienia jest wymagana do użycia pakietu Microsoft.Azure.Mobile.Client.SQLiteStore NuGet w systemach iOS i Android.

### <a name="universal-windows-platform"></a>Platforma uniwersalna systemu Windows

Aby użyć SQLite na uniwersalnych platformy systemu Windows (UWP), wykonaj następujące kroki:

1. Zainstaluj [SQLite dla platformy uniwersalnej systemu Windows](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) programu Visual Studio rozszerzenia w środowisku projektowania.
1. W projekcie platformy uniwersalnej systemu Windows w programie Visual Studio, kliknij prawym przyciskiem myszy **odwołania > Dodaj odwołanie**, przejdź do **rozszerzenia** i Dodaj **SQLite dla platformy uniwersalnej systemu Windows** i  **2015 środowiska uruchomieniowego Visual C++ dla uniwersalnych aplikacji platformy systemu Windows** rozszerzeń w projekcie platformy uniwersalnej systemu Windows.

## <a name="initializing-the-local-store"></a>Inicjowanie lokalnego magazynu

Lokalny magazyn muszą zostać zainicjowane, aby można było wykonać wszystkie operacje tabeli synchronizacji. Jest to osiągane w projekcie przenośnej biblioteki klasy (PCL) platformy Xamarin.Forms rozwiązania:

```csharp
using Microsoft.WindowsAzure.MobileServices;
using Microsoft.WindowsAzure.MobileServices.SQLiteStore;
using Microsoft.WindowsAzure.MobileServices.Sync;

namespace TodoAzure
{
    public partial class TodoItemManager
    {
        static TodoItemManager defaultInstance = new TodoItemManager();
        IMobileServiceClient client;
        IMobileServiceSyncTable<TodoItem> todoTable;

        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);
            var store = new MobileServiceSQLiteStore("localstore.db");
            store.DefineTable<TodoItem>();
            this.client.SyncContext.InitializeAsync(store);
            this.todoTable = client.GetSyncTable<TodoItem>();
        }
        ...
  }
}
```

Nowe lokalne bazy danych SQLite jest tworzony przez `MobileServiceSQLiteStore` klasy, pod warunkiem, że jeszcze nie istnieje. Następnie `DefineTable<T>` metoda tworzy tabelę w lokalnym magazynie odpowiadającego pola `TodoItem` typ, pod warunkiem, że jeszcze nie istnieje.

A *kontekstu synchronizacji* jest skojarzony z `MobileServiceClient` wystąpienia i śledzi zmiany w tabelach synchronizacji. Kontekst synchronizacji zachowuje się, że kolejka podtrzymujące uporządkowaną listę operacji tworzenia, aktualizowania lub usuwania (CUD) zostaną wysłane do wystąpienia usługi Azure Mobile Apps później. `IMobileServiceSyncContext.InitializeAsync()` Metoda jest używana do skojarzenia z kontekstem synchronizacji magazynu lokalnego.

`todoTable` Pole jest `IMobileServiceSyncTable`, i dlatego wszystkich operacji CRUD użyć lokalnego magazynu.

## <a name="performing-synchronization"></a>Wykonywanie synchronizacji

Magazyn lokalny jest zsynchronizowany z usługą Azure Mobile Apps wystąpienia, kiedy `SyncAsync` wywołania metody:

```csharp
public async Task SyncAsync()
{
  ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

  try
  {
    await this.client.SyncContext.PushAsync();

    // The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
    // Use a different query name for each unique query in your program.
    await this.todoTable.PullAsync("allTodoItems", this.todoTable.CreateQuery());
  }
  catch (MobileServicePushFailedException exc)
  {
    if (exc.PushResult != null)
    {
      syncErrors = exc.PushResult.Errors;
    }
  }

  // Simple error/conflict handling.
  if (syncErrors != null)
  {
    foreach (var error in syncErrors)
    {
      if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
      {
        // Update failed, revert to server's copy
        await error.CancelAndUpdateItemAsync(error.Result);
      }
      else
      {
        // Discard local change
        await error.CancelAndDiscardItemAsync();
      }

      Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
    }
  }
}
```

`IMobileServiceSyncTable.PushAsync` — Metoda działa w kontekście synchronizacji, a nie w określonej tabeli i wysyła wszystkie zmiany CUD od ostatniego powiadomienia wypychanego.

Ściągania odbywa się za pośrednictwem `IMobileServiceSyncTable.PullAsync` metodę dla pojedynczej tabeli. Pierwszy parametr `PullAsync` metody jest to nazwa zapytania jest używana tylko na urządzeniu przenośnym. Zapewnianie zapytania inną niż null powoduje nazwa wykonywania zestawu SDK klienta usługi Azure Mobile *synchronizacja przyrostowa*, gdzie każdy czas operacji ściągania zwraca wyniki, najnowsze `updatedAt` sygnatury czasowej z wyników jest przechowywany w lokalnym tabel systemowych. Operacji ściągania kolejnych następnie tylko pobieranie rekordów po tym sygnatury czasowej. Alternatywnie *pełnej synchronizacji* można osiągnąć przez przekazanie `null` jako nazwa zapytania, które powoduje wszystkich rekordów pobieranych w każdej operacji ściągania. Po żadnej operacji synchronizacji odebranych danych są wstawiane do lokalnego magazynu.

> [!NOTE]
> Jeśli ściąganie jest wykonywane względem tabeli, która ma oczekujące aktualizacje lokalnej, ściągania najpierw wykonać wypychanej w kontekście synchronizacji. Pozwala to zmniejszyć konfliktów między zmiany, które już są umieszczane w kolejce i nowych danych z wystąpienia usługi Azure Mobile Apps.

`SyncAsync` Metoda zawiera również podstawowe implementacji konfliktów, gdy ten sam rekord został zmieniony w magazynie lokalnym i w wystąpieniu usługi Azure Mobile Apps. Jeśli występuje konflikt, że dane został zaktualizowany, zarówno w lokalnym magazynie, jak i w wystąpieniu usługi Azure Mobile Apps `SyncAsync` metody aktualizacji danych w lokalnym magazynie danych przechowywanych w wystąpieniu usługi Azure Mobile Apps. W przypadku innych konflikt `SyncAsync` metody spowoduje odrzucenie zmian lokalnych. Obsługuje scenariusza, gdy istnieje lokalne zmiany danych, który jest usunięty z wystąpienia usługi Azure Mobile Apps.

W aplikacji produkcyjnej, deweloperzy należy zapisać niestandardowego `IMobileServiceSyncHandler` implementacji obsługi konfliktu, który jest odpowiedni dla ich przypadek użycia. Aby uzyskać więcej informacji, zobacz [optymistycznej współbieżności używany do rozwiązywania konfliktów](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) w portalu Azure i [bezpośrednich zajrzyj dostęp w trybie offline Obsługa w zestawem SDK zarządzanego klienta](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) na blogi MSDN.

## <a name="purging-data"></a>Czyszczenie danych

Można wyczyścić tabel w lokalnym magazynie danych z `IMobileServiceSyncTable.PurgeAsync` metody. Ta metoda obsługuje scenariusze, takich jak usuwanie starych danych aplikacji nie wymaga już. Na przykład przykładowej aplikacji są wyświetlane tylko `TodoItem` wystąpień, które nie są kompletne. W związku z tym ukończonych elementów nie muszą już być przechowywane lokalnie. Trwałe usuwanie elementów ukończone z lokalnego magazynu można zrobić w następujący sposób:

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

Wywołanie `PurgeAsync` wyzwala również operacji push. W związku z tym wszystkie elementy, które są oznaczone jako zakończone lokalnie będą wysyłane do wystąpienia usługi Azure Mobile Apps przed usuwany z magazynu lokalnego. Jednak w przypadku operacji oczekujących synchronizacji z wystąpieniem usługi Azure Mobile Apps przeczyszczanie zgłosi `InvalidOperationException` chyba że `force` ustawiono parametr `true`. Strategia alternatywnych jest zbadanie `IMobileServiceSyncContext.PendingOperations` właściwość, która zwraca liczbę oczekujących operacji, które nie zostały przekazane do wystąpienia usługi Azure Mobile Apps i wykonywać przeczyszczanie, tylko jeśli właściwość jest równy zero.

> [!NOTE]
> Wywoływanie `PurgeAsync` z `force` ustawiona `true` spowoduje utratę wszystkich oczekujących zmian.

## <a name="initiating-synchronization"></a>Inicjowanie synchronizacji

W przykładowej aplikacji `SyncAsync` metoda jest wywoływana za pośrednictwem `TodoList.OnAppearing` metody:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

Oznacza to, że aplikacja będzie podejmować próby synchronizacji z wystąpieniem usługi Azure Mobile Apps, podczas jej uruchamiania.

Ponadto synchronizacji może zostać zainicjowany w systemach iOS i Android przy użyciu replikacji ściąganej Aby odświeżyć listę danych i na platformach systemu Windows przy użyciu **synchronizacji** przycisk w interfejsie użytkownika. Aby uzyskać więcej informacji, zobacz [ściągnięcia do odświeżania](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh).

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób dodawania funkcji synchronizacji w trybie offline do aplikacji platformy Xamarin.Forms. Synchronizacja w trybie offline umożliwia użytkownikom na interakcję z aplikacji mobilnej, przeglądanie, dodawanie lub modyfikowanie danych, nawet jeśli nie ma połączenia sieciowego. Zmiany są przechowywane w lokalnej bazie danych, a gdy urządzenie jest w trybie online, zmiany można synchronizować z wystąpieniem usługi Azure Mobile Apps.


## <a name="related-links"></a>Linki pokrewne

- [TodoAzureAuthOfflineSync (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Korzystanie z aplikacji mobilnej Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Uwierzytelniania użytkowników przy użyciu usługi Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Klientów mobilnych Azure SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
