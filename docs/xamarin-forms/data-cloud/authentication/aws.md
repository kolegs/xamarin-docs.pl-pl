---
title: Uwierzytelnianie użytkowników przy użyciu usługi Amazon SimpleDB usługi
description: Amazon SimpleDB nie oferuje własny system uprawnień opartych na zasobach. Zamiast tego aby upewnić się, że użytkownicy mają tylko dostęp do swoich własnych danych w domenie SimpleDB można uwierzytelniania względem dostawcy tożsamości. W tym artykule wyjaśniono, jak ograniczyć dostęp użytkowników do ich własnych danych SimpleDB.
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 592e957e0c64e7189d6f01f1ba0f23da074c4bec
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/12/2018
---
# <a name="authenticating-users-with-an-amazon-simpledb-service"></a>Uwierzytelnianie użytkowników przy użyciu usługi Amazon SimpleDB usługi

_Amazon SimpleDB nie oferuje własny system uprawnień opartych na zasobach. Zamiast tego aby upewnić się, że użytkownicy mają tylko dostęp do swoich własnych danych w domenie SimpleDB można uwierzytelniania względem dostawcy tożsamości. W tym artykule wyjaśniono, jak ograniczyć dostęp użytkowników do ich własnych danych SimpleDB._

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth) jest używany w przykładowej aplikacji do zarządzania procesem uwierzytelniania użytkownika i bezpiecznie przechowywane szczegóły konta użytkowników na urządzeniu. Aby uzyskać więcej informacji, zobacz [uwierzytelnianie użytkowników przy użyciu dostawcy tożsamości](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="allowing-an-authenticated-user-access-to-simpledb-domain-data"></a>Zezwalanie na dostęp uwierzytelniony użytkownik domeny SimpleDB danych

Przykładowa aplikacja korzysta z `TodoItem` klasy do modelu danych. Do przechowywania `TodoItem` wystąpienia usługi SimpleDB, najpierw musi zostać przekonwertowane na `List` z `ReplaceableAttribute` obiektów. Aby uzyskać więcej informacji, zobacz [SimpleDB Tworzenie obiektów](~/xamarin-forms/data-cloud/consuming/aws.md).

> [!NOTE]
> W systemie iOS 9 i większości aplikacji zabezpieczeń transportu (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.
> ATS można można korzystać z, jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).

Aby upewnić się, że użytkownicy mają dostęp tylko do własnych danych w domenie SimpleDB `ToSimpleDBReplaceableAttributes` metody przechowuje dodatkowe atrybut `TodoItem` wystąpienie, jak pokazano w poniższym przykładzie:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

Ten atrybut zapewnia, że każdy element przechowywane w domenie SimpleDB ma powiązanego adresu e-mail dla użytkownika, który jest używany do unikatowego identyfikowania użytkownika, do którego należy dane. Gdy zawartość domeny są pobierane przez wywołanie metody `AmazonSimpleDBClient.SelectAsync` metody, wyrażenia zapytania gwarantuje, że są pobierane tylko elementy dla tego uwierzytelnionego użytkownika, jak pokazano w poniższym przykładzie:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

`SelectAsync` Metoda zwraca odpowiedź zawierający kolekcję elementów i skojarzonych z nimi atrybutów, które pasują do wyrażenia zapytania. Wyrażenia zapytania zapewnia zostaną pobrane tylko elementów, które odpowiadają adres e-mail użytkownika. Aby uzyskać więcej informacji o wyrażeniach zapytań, zobacz [przy użyciu umożliwia tworzenie kwerend SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) w witrynie sieci Web firmy Amazon.

> [!NOTE]
> Należy zachować ostrożność, postępuj zgodnie z regułami quoting podczas tworzenia wyrażenia zapytania. Aby uzyskać więcej informacji, zobacz [Wybierz reguły zamykający](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) w witrynie sieci Web firmy Amazon.

## <a name="summary"></a>Podsumowanie

W tym artykule wyjaśniono, jak ograniczyć dostęp użytkowników do ich własnych danych SimpleDB. Amazon SimpleDB nie oferuje własny system uprawnień opartych na zasobach. Zamiast tego aby upewnić się, że użytkownicy mają dostęp tylko do własnych danych w domenie SimpleDB można uwierzytelniania względem dostawcy tożsamości.


## <a name="related-links"></a>Linki pokrewne

- [TodoAWSAuth (przykład)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Consuming an Amazon SimpleDB Service](~/xamarin-forms/data-cloud/consuming/aws.md)
- [Uwierzytelnianie użytkowników przy użyciu dostawcy tożsamości](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB Developer Documentation](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Amazon Web Services SDK for .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
