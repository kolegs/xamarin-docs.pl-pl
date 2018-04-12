---
title: Consuming an Amazon SimpleDB Service
description: Amazon SimpleDB to usługa sieci web, która zapewnia możliwość przechowywania i zapytania na danych w chmurze firmy Amazon. W tym artykule opisano sposób używania usług AWS zestawu SDK dla platformy .NET do zapytania, tworzenie, Zastąp i usuwanie danych przechowywanych w usłudze SimpleDB.
ms.prod: xamarin
ms.assetid: 823819AA-15F9-4144-B355-78A10AD37513
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 1602319dbf5a5d00ac5de75f2d438b9aea692699
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/12/2018
---
# <a name="consuming-an-amazon-simpledb-service"></a>Consuming an Amazon SimpleDB Service

_Amazon SimpleDB to usługa sieci web, która zapewnia możliwość przechowywania i zapytania na danych w chmurze firmy Amazon. W tym artykule opisano sposób używania usług AWS zestawu SDK dla platformy .NET do zapytania, tworzenie, Zastąp i usuwanie danych przechowywanych w usłudze SimpleDB._

SimpleDB usług korzystają z modelu żądań i odpowiedzi znanych klientom korzystającym z usługi REST. Operacje są wywoływane w usłudze SimpleDB, wysyłając żądanie, które mogą zawierać dane. Po przetworzeniu żądania, usługa SimpleDB zwraca odpowiedź zawierającą wszystkie wyniki. Usługa SimpleDB muszą zostać utworzone programowo i nie można utworzyć za pomocą [konsoli usług AWS](https://aws.amazon.com). Jednak konto dla usług AWS jest wymagane do utworzenia i dostępu do usługi sieci web firmy Amazon.

W usłudze SimpleDB dane są zorganizowane w domenach, w których można umieścić dane i uruchom zapytania względem danych. Domeny składają się z elementów, które są opisane przez pary nazwa wartość atrybutu. Domeny można traktować jako zbliżone do tabel z atrybutami są podobne do kolumn i elementów są podobne do wierszy. Aby uzyskać więcej informacji na temat SimpleDB modelu danych, zobacz [modelu danych](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/DataModel.html) w witrynie sieci Web firmy Amazon.

Instrukcje na temat konfigurowania wymaganych usług Amazon można znaleźć w pliku readme, który towarzyszy przykładowej aplikacji. Przykładowa aplikacja jest uruchamiana, będą łączyć puli tożsamości Amazon Cognito do autoryzowania dostępu do usługi SimpleDB, jak pokazano na poniższym zrzucie ekranu:

![](aws-images/portal.png "Przykładowa aplikacja")

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> ATS można można korzystać z, jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

## <a name="consuming-a-simpledb-service"></a>Korzystanie z usługi SimpleDB

Tożsamość Cognito Amazon umożliwia usług AWS, takich jak SimpleDB wywoływanej z aplikacji bez poświadczeń usług AWS kodować do aplikacji. Zamiast tego pulę unikatowych tożsamości jest tworzony w [konsoli Cognito Amazon](https://console.aws.amazon.com/cognito/home). Pula tożsamości zawiera tożsamości, które umożliwia określenie zasoby, takie jak SimpleDB, który tożsamości mogą uzyskiwać dostęp do ról.

[Usług AWS SDK dla platformy .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22) zapewnia `CognitoAWSCredentials` i `AmazonSimpleDBClient` klasy, które są używane przez aplikację platformy Xamarin.Forms uzyskać dostęp do usługi SimpleDB, jak pokazano w poniższym przykładzie:

```csharp
AmazonSimpleDBClient client;
...

public SimpleDBStorage ()
{
  var credentials = new CognitoAWSCredentials (
                      Constants.CognitoIdentityPoolId,
                      RegionEndpoint.USEast1);
  var config = new AmazonSimpleDBConfig ();
  config.RegionEndpoint = RegionEndpoint.USWest2;
  client = new AmazonSimpleDBClient (credentials, config);
  ...
}
```

Nowe wystąpienie klasy `CognitoAWSCredentials` klasy jest tworzony przez podanie identyfikator puli unikatową tożsamość i region konta Cognito tożsamości. W momencie pisania Cognito tożsamości jest dostępna tylko w regionach USEast1 i EUWest1. Jednak może komunikować się z usługi Amazon poza regionach.

Gdy `AmazonSimpleDBClient` jest tworzone wystąpienie, `CognitoAWSCredentials` wystąpienia należy podać, wraz z `AmazonSimpleDBConfig` wystąpienia, która określa region geograficzny, w którym znajduje się usługa SimpleDB. `CognitoAWSCredentials` Wystąpienia zapewnia, że usługa SimpleDB, który jest dostępny jest jeden skojarzone z kontem usług AWS, w którym utworzono puli tożsamości, eliminując konieczność osadzone klucz dostępu do usług AWS i klucz tajny aplikacji.

Domena usługi SimpleDB jest tworzony przez wywołanie metody `AmazonSimpleDBClient.CreateDomainAsync` metody, jak pokazano w poniższym przykładzie:

```csharp
string tableName = "Todo";
...

async Task CreateDomain ()
{
  ...
  await client.CreateDomainAsync (new CreateDomainRequest { DomainName = tableName });
  ...
}
```

`CreateDomainAsync` Metoda wymaga `CreateDomainRequest` wystąpienia jako parametr. `CreateDomainRequest` Wystąpienie inicjuje `DomainName` na wartość ma być używany do identyfikowania domeny. Aby utworzyć domenę, ta wartość musi być unikatowa wśród domeny skojarzone z kontem usług AWS. W przeciwnym razie nie będzie można utworzyć domeny i będą wysyłane odpowiedzi na błąd z informacją, że. Następnie nastąpi żadnych operacji dla nazwy domeny dla istniejącej domeny, a nie z domeną nowo utworzony.

### <a name="creating-simpledb-objects"></a>Tworzenie obiektów SimpleDB

Przykładowa aplikacja korzysta z `TodoItem` klasy do modelu danych. Do przechowywania `TodoItem` wystąpienia usługi SimpleDB, najpierw musi zostać przekonwertowane na `List` z `ReplaceableAttribute` obiektów. Jest to osiągane przez `ToSimpleDBReplaceableAttributes` metody, jak pokazano w poniższym przykładzie:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    new ReplaceableAttribute () {
      Name = "Name",
      Value = item.Name,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Notes",
      Value = item.Notes,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Done",
      Value = item.Done.ToString (),
      Replace = true
    }
  };
}
```

Ta metoda tworzy `List` z nowego `ReplaceableAttribute` wystąpienia, z `List` reprezentujący pojedynczy `TodoItem` wystąpienia. Każdy `ReplaceableAttribute` wystąpienie reprezentuje jedną właściwość z `TodoItem` wystąpienia. Aby uzyskać więcej informacji na temat `ReplaceableAttribute` , zobacz [klasy ReplaceableAttribute](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_ReplaceableAttribute.htm) w witrynie sieci Web firmy Amazon.

Podobnie, gdy dane są pobierane z usługi SimpleDB, należy go przekonwertować z `List` z `Attribute` wystąpień do `TodoItem` wystąpienia. Jest to realizowane przy użyciu `FromSimpleDBAttributes` metody, jak pokazano w poniższym przykładzie:

```csharp
TodoItem FromSimpleDBAttributes (List<Amazon.SimpleDB.Model.Attribute> attributeList, string id)
{
  var todoItem = new TodoItem ();
  todoItem.ID = id;
  todoItem.Name = attributeList.Where (attr => attr.Name == "Name").FirstOrDefault ().Value;
  todoItem.Notes = attributeList.Where (attr => attr.Name == "Notes").FirstOrDefault ().Value;
  todoItem.Done = Convert.ToBoolean (attributeList.Where (attr => attr.Name == "Done").FirstOrDefault ().Value);
  return todoItem;
}
```

Ta metoda po prostu pobiera każdego `Attribute` wystąpienia z `List` i ustawia go w nowo utworzonej `TodoItem` wystąpienia.

Aby uzyskać więcej informacji na temat `Attribute` , zobacz [klasy atrybutów](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_Attribute.htm) w witrynie sieci Web firmy Amazon.

### <a name="querying-data"></a>Wykonywanie zapytania na danych

Zawartość domeny można pobranej poprzez wywołanie `AmazonSimpleDBClient.SelectAsync` metody, jak pokazano w poniższym przykładzie:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
      SelectExpression = string.Format ("SELECT * from {0}", tableName)
  };
  var response = await client.SelectAsync (request);
  foreach (var item in response.Items) {
    Items.Add (FromSimpleDBAttributes (item.Attributes, item.Name));
  }
  ...
}
```

`SelectAsync` Metoda przyjmuje `SelectRequest` wystąpienia jako parametr, który określa `Select` zapytania wyrażenie w jego `SelectExpression` właściwości. Format wyrażenia zapytania jest podobny do formatu standardowych SQL `SELECT` instrukcji. Aby uzyskać więcej informacji na temat wyrażenia zapytania, zobacz [przy użyciu umożliwia tworzenie kwerend SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) w witrynie sieci Web firmy Amazon.

> [!NOTE]
> Należy zachować ostrożność, postępuj zgodnie z regułami quoting podczas tworzenia wyrażenia zapytania. Aby uzyskać więcej informacji, zobacz [Wybierz reguły zamykający](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) w witrynie sieci Web firmy Amazon.

`SelectAsync` Metoda zwraca odpowiedź zawierający kolekcję elementów i skojarzonych z nimi atrybutów, które pasują do wyrażenia zapytania. Ta kolekcja jest następnie konwertowana na `List` z `TodoItem` wystąpień do wyświetlenia.

### <a name="creating-and-replacing-data"></a>Tworzenie i zastępowanie danych

`AmazonSimpleDBClient.PutAttributesAsync` Metoda służy do tworzenia i zamienić dane w domenie usługi SimpleDB, jak pokazano w poniższym przykładzie:

```csharp
public async Task SaveTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBReplaceableAttributes (todoItem);
  var request = new PutAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.PutAttributesAsync (request);
  ...
}
```

`PutAttributesAsync` Metoda przyjmuje `PutAttributesRequest` wystąpienia jako parametr. `PutAttributesRequest` Wystąpienia Określa pary nazwa wartość atrybutu, które mają być tworzone jako nowy element lub w istniejący element. `List` z `ReplaceableAttribute` wystąpień jest konstruowany przez `ToSimpleDBReplaceableAttributes` metody. Ta metoda określa również `Replace` właściwości każdego `ReplaceableAttribute` do `true`. To spowoduje, że wartość atrybutu zastąpić istniejącą wartość atrybutu, jeśli danych jest zastępowany. Jednak próba zastąpienia wartości atrybutów, które nie istnieją nie spowoduje odpowiedzi na błąd.

Wartość `PutAttributesRequest.ItemName` właściwość określa, czy zostanie dodany nowy element do domeny lub zostanie zastąpiony istniejący element. Gdy aplikacja tworzy nowy element, ustawia `TodoItem.ID` właściwości na nowy `Guid`. Gwarantuje to, że każdy `TodoItem` wystąpienie ma unikatowy identyfikator. W związku z tym jeśli `PutAttributesRequest.ItemName` właściwość jest ustawiona na wartość, która nie istnieje w domenie, usługa SimpleDB utworzy nowy element zawierający pary nazwa wartość określonego atrybutu. Jeśli `PutAttributesRequest.ItemName` właściwość ma ustawioną wartość, która już istnieje w domenie, usługa SimpleDB zaktualizuje element pary nazwa wartość określonego atrybutu.

### <a name="deleting-data"></a>Usuwanie danych

`AmazonSimpleDBClient.DeleteAttributesAsync` Metoda służy do usuwania danych z domeny usługi SimpleDB, jak pokazano w poniższym przykładzie kodu:

```csharp
public async Task DeleteTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBAttributes (todoItem);
  var request = new DeleteAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.DeleteAttributesAsync (request);
  ...
}
```

`DeleteAttributesAsync` Metoda przyjmuje `DeleteAttributesRequest` wystąpienia jako parametr.  `DeleteAttributesRequest` Wystąpienia określa atrybuty, które mają zostać usunięte z poziomu elementu z `List` z `Attribute` wystąpienia mają zostać usunięte konstruowany przez `ToSimpleDBAttributes` metody. Element zostanie usunięty, pod warunkiem, że wszystkie atrybuty elementu są usuwane.

## <a name="summary"></a>Podsumowanie

W tym artykule opisano sposób używania usług AWS zestawu SDK dla platformy .NET zapytania, Utwórz i zastąpić i usunięcia danych przechowywanych w usłudze SimpleDB. Zestaw SDK zawiera `CognitoAWSCredentials` i `AmazonSimpleDBClient` klasy, które są używane przez aplikację platformy Xamarin.Forms do uzyskania dostępu do usługi SimpleDB.


## <a name="related-links"></a>Linki pokrewne

- [TodoAWS (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS/)
- [Przewodnik dewelopera Xamarin zestawu SDK usług sieci Web firmy Amazon](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB Developer Documentation](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [AmazonSimpleDBClient Class](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_AmazonSimpleDBClient.htm)
- [Amazon Web Services SDK for .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
