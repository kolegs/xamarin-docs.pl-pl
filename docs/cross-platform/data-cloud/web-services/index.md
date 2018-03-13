---
title: "Wprowadzenie do usługi sieci Web"
description: "W tym przewodniku pokazano, jak korzystać z technologii usług innej witryny sieci web. Tematy obejmują komunikacji z usługi REST, usług SOAP i usług Windows Communication Foundation."
ms.topic: article
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 48489ca7dc28dcc14a7810b15dc1ffa1fd4f7cf4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-web-services"></a>Wprowadzenie do usługi sieci Web

_W tym przewodniku pokazano, jak korzystać z technologii usług innej witryny sieci web. Tematy obejmują komunikacji z usługi REST, usług SOAP i usług Windows Communication Foundation._

Do poprawnego działania są zależne od chmury wiele aplikacji dla urządzeń przenośnych, i dlatego włączenie usług sieci web do aplikacji dla urządzeń przenośnych jest typowym scenariuszem. Platformy Xamarin obsługuje korzystanie z technologii usług sieci web w innej oraz oferuje obsługę w utworzony i innych firm na korzystanie z usług RESTful, ASMX i Windows Communication Foundation (WCF).

W tym artykule omówiono następujące zagadnienia:

- [Usługi REST](#rest)
- [Usługi sieci Web platformy ASP.Net (ASMX)](#asmx)
- [WCF Services](#wcf)

Dla klientów korzystających z platformy Xamarin.Forms, są kompletne przykłady każdego z tych technologii w [usługi sieci Web platformy Xamarin.Forms](~/xamarin-forms/data-cloud/index.md) dokumentacji.

> [!IMPORTANT]
> **Uwaga dotycząca platformy Xamarin.iOS:** w systemie iOS 9, zabezpieczeń transportu aplikacji (ATS) wymusza bezpiecznego połączenia między zasobami Internetu (na przykład serwera zaplecza aplikacji) i aplikacji, zapobiegając w ten sposób przypadkowego ujawnienia informacji poufnych. Ponieważ ATS jest domyślnie włączone w aplikacje dla systemu iOS 9, wszystkie połączenia będą podlegać ATS wymagania dotyczące zabezpieczeń. W przypadku połączeń nie spełniają tych wymagań, zakończy się niepowodzeniem z powodu wyjątku.


Użytkownik może zrezygnować z ATS Jeśli nie jest możliwe użycie `HTTPS` protokołu i zabezpieczania komunikacji dla zasobów w Internecie. Można to osiągnąć przez zaktualizowanie aplikacji **Info.plist** pliku. Aby uzyskać więcej informacji, zobacz [zabezpieczeń transportu aplikacji](~/ios/app-fundamentals/ats.md).



<a name="rest" />

## <a name="rest"></a>REST

Representational State (Transfer REST) jest architektury stylu do tworzenia usług sieci web. Żądania REST są wysyłane za pośrednictwem protokołu HTTP, przy użyciu tego samego polecenia HTTP, korzystających z przeglądarki sieci web do pobierania strony sieci web i wysyłać dane do serwerów. Zlecenia, które są:

- **Pobierz** — ta operacja jest używana do pobierania danych z usługi sieci web.
- **POST** — ta operacja jest używana do utworzenia nowego elementu danych w usłudze sieci web.
- **Umieść** — ta operacja jest używane do aktualizowania elementu danych w usłudze sieci web.
- **POPRAWKA** — ta operacja jest używane do aktualizowania elementu danych w usłudze sieci web przez opisujące zestaw instrukcji dotyczących jak element powinien być modyfikowany. Tego zlecenia nie jest używany w przykładowej aplikacji.
- **Usuń** — ta operacja jest używana do usuwania elementu danych w usłudze sieci web.

Usługa sieci Web interfejsów API REST jest zgodna są nazywane interfejsy API RESTful i są definiowane przy użyciu:

- Podstawowy identyfikator URI.
- Metody HTTP, takich jak GET, POST, PUT, PATCH lub DELETE.
- Typ nośnika dla danych, takich jak JavaScript Object Notation (JSON).

Łatwość REST pomogła była podstawowej metody dostępu do usług sieci web w aplikacjach mobilnych.

## <a name="consuming-rest-services"></a>Korzystające usługi REST

Istnieje wiele bibliotek i klasy, które mogą służyć do korzystania z usług REST i następujące podsekcje omówiono je. Aby uzyskać więcej informacji dotyczących używania usługi REST, zobacz [konsumowania usługi sieci Web RESTful](~/xamarin-forms/data-cloud/consuming/rest.md).

### <a name="httpclient"></a>HttpClient

[Bibliotek klienta HTTP Microsoft](https://www.nuget.org/packages/Microsoft.Net.Http) zapewnia `HttpClient` klasy, która służy do wysyłania i odbierania żądań za pośrednictwem protokołu HTTP. Zapewnia funkcje do wysyłania żądań HTTP i odbierania odpowiedzi HTTP z zasobu zidentyfikowanego identyfikatora URI. Każde żądanie jest wysyłany jako operację asynchroniczną. Aby uzyskać więcej informacji na temat operacji asynchronicznych, zobacz [Przegląd pomocy technicznej Async](~/cross-platform/platform/async.md).

`HttpResponseMessage` Klasa reprezentuje komunikat odpowiedzi HTTP, otrzymał od usługi sieci web po dokonaniu żądania HTTP. Zawiera informacje o odpowiedzi, łącznie z kodem stanu, nagłówki i treść. `HttpContent` Klasa reprezentuje treści HTTP i nagłówków zawartości, takich jak `Content-Type` i `Content-Encoding`. Zawartość może zostać odczytany przy użyciu dowolnego `ReadAs` metod, takich jak `ReadAsStringAsync` i `ReadAsByteArrayAsync`, zależnie od formatu danych.

Aby uzyskać więcej informacji na temat `HttpClient` , zobacz [tworzenia obiektu HTTPClient](~/xamarin-forms/data-cloud/consuming/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Wywoływanie usługi sieci web z `HTTPWebRequest` obejmuje:

-  Tworzenie wystąpienia żądania dla określonego identyfikatora URI.
-  Ustawianie właściwości HTTP różne wystąpienia żądania.
-  Trwa pobieranie `HttpWebResponse` z żądania.
-  Odczytywanie danych z odpowiedzi.

Na przykład następujący kod pobiera dane ze Stanów Zjednoczonych Usługa sieci web National biblioteki:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

W powyższym przykładzie jest tworzony `HttpWebRequest` który zwróci dane w formacie JSON. Dane są zwracane w `HttpWebResponse`, z którego `StreamReader` można uzyskać odczytać danych.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

Używa innego podejścia do konsumowania usługi REST [RestSharp](http://restsharp.org/) biblioteki. RestSharp hermetyzuje żądania HTTP, w tym obsługę pobierania wyników jako zawartość nieprzetworzony ciąg albo zdeserializowany obiekt C#. Na przykład następujący kod zgłasza żądanie do Stanów Zjednoczonych Usługa sieci web biblioteki krajowych i pobiera wyniki w formacie JSON ciąg w formacie:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` jest to metoda, która spowoduje przejście nieprzetworzonego ciągu JSON z `RestSharp.RestResponse.Content` właściwości i przekonwertować go na obiekt C#. Podczas deserializacji danych zwróconych z usług sieci web omówione w dalszej części tego artykułu.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Oprócz klasy dostępne w podstawowym Mono Biblioteka klas (BCL), takich jak `HttpWebRequest`i innych firm C# biblioteki, takich jak RestSharp, klasy specyficzne dla platformy są także dostępne służący do konsumowania usługi sieci web. Na przykład w systemie iOS `NSUrlConnection` i `NSMutableUrlRequest` klasy mogą być używane.

Poniższy przykład kodu pokazuje sposób wywoływania USA Usługa sieci web National biblioteki przy użyciu klasy iOS:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

Ogólnie rzecz biorąc klasy specyficzne dla platformy służący do konsumowania usługi sieci web powinna być ograniczona do scenariuszy, w którym kodu natywnego jest są przenoszone do języka C#. Jeśli to możliwe, kodu dostępu do usługi sieci web powinny być przenośne, tak aby mogły być udostępniony i platform.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Inną opcją w przypadku wywoływania usługi sieci web jest [stosu usługi](http://www.servicestack.net/) biblioteki. Na przykład poniższy kod przedstawia sposób użycia usługi stosu `IServiceClient.GetAsync` metody do wystawiania żądania obsługi:

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> **Uwaga:** podczas narzędzi, takich jak ServiceStack i RestSharp nawiązywanie połączenia i korzystać z REST usługi, jest czasami nieuproszczony użycie XML lub JSON, który nie jest zgodny z normą _DataContract_ szeregowanie konwencje. W razie potrzeby wywołać żądanie i obsługi odpowiednich serializacji jawnie za pomocą biblioteki ServiceStack.Text omówiony poniżej.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Korzystanie z danych RESTful

Usługi sieci web rESTful zazwyczaj używają komunikatów JSON do zwracania danych do klienta. JSON to opartego na tekście wymiany danych formatu, który tworzy compact ładunków, co powoduje zmniejszenie przepustowości wymagania podczas wysyłania danych. W tej sekcji zostaną zbadane mechanizmów służący do konsumowania RESTful odpowiedzi w formacie JSON i zwykłego stary XML (POX).

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Platformy Xamarin jest dostarczany z obsługą JSON bez. Za pomocą `JsonObject`, wyniki mogą być pobierane, jak pokazano w poniższym przykładzie:

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

Jednak ważne jest, aby należy pamiętać, że `System.Json` narzędzia załadować całości danych do pamięci.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[NewtonSoft JSON.NET biblioteki](http://www.newtonsoft.com/json) jest powszechnie używaną Biblioteka serializacji i deserializacji JSON wiadomości. W poniższym przykładzie pokazano, jak używać struktury JSON.NET do deserializacji komunikatu JSON na obiekt C#:

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text" />

### <a name="servicestacktext"></a>ServiceStack.Text

ServiceStack.Text jest przeznaczona do pracy z biblioteką ServiceStack Biblioteka serializacji JSON. Poniższy przykładowy kod przedstawia sposób przeanalizowana z zastosowaniem formatu JSON `ServiceStack.Text.JsonObject`:

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq" />

### <a name="systemxmllinq"></a>System.Xml.Linq

W przypadku konsumowania usługi sieci web REST opartych na języku XML, LINQ do XML można przeanalizować pliku XML i umieścić w C# wbudowanego obiektu, jak pokazano w poniższym przykładzie:

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx" />

## <a name="aspnet-web-service-asmx"></a>Usługa sieci Web ASP.NET (ASMX)

ASMX umożliwia tworzenie usług sieci web, które wysyłanie wiadomości przy użyciu obiektu dostępu protokołu SOAP (Simple). SOAP jest protokołem niezależne od platformy i języka umożliwiające tworzenie i uzyskiwanie dostępu do usług sieci web. Korzystającym z usług ASMX nie trzeba niczego wiedzieć o platformie, model obiektów lub język programowania używany do wdrażania usługi. Tylko muszą zrozumieć sposób wysyłania i odbierania wiadomości protokołu SOAP.

Wiadomości SOAP jest dokument XML zawierający następujące elementy:

- Element główny o nazwie *koperty* identyfikującym dokumentu w formacie XML jako wiadomości protokołu SOAP.
- Opcjonalny *nagłówka* element, który zawiera informacje specyficzne dla aplikacji, takie jak dane uwierzytelniania. Jeśli *nagłówka* elementu musi być pierwszym elementem podrzędnym *koperty* elementu.
- Wymaganą *treści* element, który zawiera przeznaczone dla adresata komunikatu protokołu SOAP.
- Opcjonalny *błędów* element, który służy do wskazywania komunikaty o błędach. Jeśli *błędów* element jest obecny, musi być elementem podrzędnym *treści* elementu.

SOAP mogą pracować nad wiele protokołów transportu, w tym HTTP, SMTP, TCP i UDP. Jednak usługa ASMX może wykonywać operacje tylko za pośrednictwem protokołu HTTP. Platformy Xamarin obsługuje standardowe implementacje protokołu SOAP 1.1 za pośrednictwem protokołu HTTP, a to obsługuje wiele standardowych konfiguracji usługi ASMX.

### <a name="generating-a-proxy"></a>Generowanie serwera Proxy

A *proxy* musi zostać wygenerowany użycie usługi ASMX, która umożliwia aplikacji połączyć się z usługą. Serwer proxy jest tworzony przez odbierającą metadanych usługi definiujący metody i skojarzona usługa konfiguracji. Te metadane są widoczne jako dokument Web Services Description Language (WSDL), który jest generowany przez usługę sieci web. Serwer proxy jest tworzone za pomocą programu Visual Studio for Mac lub Visual Studio można dodać odwołania sieci web dla usługi sieci web do projektów specyficzne dla platformy.

Adres URL usługi sieci web może być hostowana zdalnego źródła lub zasób systemowy pliku lokalnego jest dostępny za pośrednictwem `file:///` prefiks ścieżki, na przykład:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "Adres URL usługi sieci web może być hostowana zdalnego źródła lub zasób systemowy pliku lokalnego dostępny za pośrednictwem prefiks ścieżka pliku")](images/add-webreference-dialog.png#lightbox)

Spowoduje to wygenerowanie serwera proxy sieci Web lub usługi odwołuje się do folderu projektu. Ponieważ serwer proxy jest generowany kod nie powinien być modyfikowany.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Ręcznie dodać serwer Proxy do projektu

Jeśli masz istniejący serwer proxy został wygenerowany za pomocą narzędzi zgodne te dane wyjściowe mogą być używane, gdy uwzględnione w ramach projektu. W programie Visual Studio dla komputerów Mac, należy użyć **dodawania plików...** Opcja menu Dodaj serwer proxy. Ponadto wymaga *System.Web.Services.dll* można odwoływać się bezpośrednio przy użyciu **Dodaj odwołania...** okno dialogowe.

### <a name="consuming-the-proxy"></a>Korzystanie z serwera Proxy

Klasy wygenerowany serwer proxy, podaj metody służący do konsumowania usługi sieci web, które używają wzorca projektowego asynchronicznego programowania modelu (APM). W tym wzorcu operacji asynchronicznej jest zaimplementowany jako dwie metody o nazwie *BeginOperationName* i *EndOperationName*, który rozpoczęcia i zakończenia operacji asynchronicznej.

*BeginOperationName* metody rozpoczyna operację asynchroniczną i zwraca obiekt, który implementuje `IAsyncResult` interfejsu. Po wywołaniu *BeginOperationName*, aplikacja może kontynuować wykonywania instrukcji w wątku wywołującym, gdy operacja asynchroniczna odbywa się w wątku puli wątków.

Dla każdego wywołania *BeginOperationName*, również powinny wywoływać aplikacji *EndOperationName* Aby uzyskać wyniki operacji. Wartość zwracana *EndOperationName* jest ten sam typ zwracany przez metodę usługi sieci web synchronicznego. Poniższy przykładowy kod przedstawia przykład:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Zadania biblioteki równoległych (TPL) może uprościć proces zużywa pary metod begin/end APM hermetyzując operacji asynchronicznych w tym samym `Task` obiektu. Ta hermetyzacja są dostarczane przez wielu przeładowań `Task.Factory.FromAsync` metody. Ta metoda tworzy `Task` , który jest wykonywany `TodoService.EndGetTodoItems` metody raz `TodoService.BeginGetTodoItems` metody zakończeniu z `null` parametr wskazujący, że żadne dane nie jest przekazywany do `BeginGetTodoItems` delegowanie. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

Aby uzyskać więcej informacji na temat APM, zobacz [Model programowania asynchronicznego](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx) i [TPL i tradycyjnych .NET Framework asynchronicznego programowania](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx) w witrynie MSDN.

Aby uzyskać więcej informacji na temat konsumowania usługi ASMX, zobacz [konsumowania usługi sieci Web platformy ASP.NET (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF jest strukturą ujednoliconego firmy Microsoft do tworzenia aplikacji korzystających z usług. Umożliwia ona deweloperom tworzenie bezpieczne, niezawodne, transakcyjne i interoperacyjne aplikacji rozproszonej.

Usługi WCF zawiera opis usługi z wieloma różnymi umowami, takich jak:

- **Kontrakty danych** — zdefiniuj struktury danych, które stanowią podstawę dla treści wiadomości.
- **Kontrakty komunikatu** — redagowanie wiadomości z istniejących kontraktów danych.
- **Odporność kontrakty** — Zezwalaj na niestandardowe błędach SOAP, należy określić.
- **Umowy o świadczenie usług** — Określ operacje, które obsługują usługi i komunikaty wymagane do interakcji z każdej operacji. Określają również zachowanie żadnych błędów niestandardowych, które mogą być skojarzone z operacjami w każdej usłudze.

Istnieją różnice między usług sieci Web platformy ASP.NET (ASMX) i WCF, ale ważne jest, aby zrozumieć, że WCF obsługuje te same możliwości udostępniające ASMX — wiadomości SOAP za pośrednictwem protokołu HTTP.

Ogólnie rzecz biorąc platformy Xamarin obsługuje samej podzbiór po stronie klienta WCF, który jest dostarczany z środowisko uruchomieniowe Silverlight. Obejmuje to najczęściej używane kodowanie i protokół implementacje WCF — transportu kodowany tekst wiadomości SOAP za pośrednictwem protokołu HTTP przy użyciu protokołu `BasicHttpBinding` klasy. Ponadto obsługa usług WCF wymaga użycia narzędzia dostępne tylko w środowisku systemu Windows, aby wygenerować serwera proxy.

Aby uzyskać więcej informacji o korzystać z usług WCF za pomocą platformy Xamarin w sieci web usługi o `BasicHttpBinding` , zobacz [wskazówki — Praca z programem WCF](walkthrough-working-with-wcf.md).

### <a name="generating-a-proxy"></a>Generowanie serwera Proxy

A *proxy* musi zostać wygenerowany użycie usługi WCF, dzięki czemu aplikacji połączyć się z usługą. Serwer proxy jest tworzony przez odbierającą metadanych usługi definiują metody i skojarzona usługa konfiguracji. Te metadane jest widoczna w formularzu sieci Web Services Description Language (WSDL) dokument, który jest generowany przez usługę sieci web. Serwer proxy mogą być tworzone przy użyciu dostawcy odwołanie usług sieci Web WCF firmy Microsoft w programie Visual Studio 2017 można dodać odwołania do usługi dla usługi sieci web do biblioteki standardowej .NET.

Zamiast tworzenia proxy przy użyciu dostawcy odwołanie usług sieci Web WCF firmy Microsoft w programie Visual Studio 2017 jest za pomocą narzędzia narzędzie metadanych elementu ServiceModel (svcutil.exe). Aby uzyskać więcej informacji, zobacz [narzędzie narzędzia metadanych elementu ServiceModel (Svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Konfigurowanie serwera Proxy

Konfigurowanie serwera proxy wygenerowanego zazwyczaj potrwa dwa argumenty konfiguracji (w zależności od protokołu SOAP 1.1/ASMX lub WCF) podczas inicjowania: `EndpointAddress` i/lub informacje o powiązaniu skojarzony, jak pokazano w poniższym przykładzie:

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

Powiązanie służy do określania transportu, kodowanie i szczegóły protokołu wymagane dla aplikacji i usług komunikować się ze sobą. `BasicHttpBinding` Określa, że kodowany tekst wiadomości SOAP będą wysyłane za pośrednictwem protokołu transportu HTTP. Określanie adresu punktu końcowego umożliwia aplikacjom nawiązać połączenia z różnymi wystąpieniami usługi WCF, pod warunkiem, że istnieje wiele wystąpień opublikowane.

### <a name="consuming-the-proxy"></a>Korzystanie z serwera Proxy

Klasy wygenerowany serwer proxy podania metod służący do konsumowania usługi sieci web, które korzystają z wzorca projektowego asynchronicznego programowania modelu (APM). W tym wzorcu operacji asynchronicznej jest zaimplementowany jako dwie metody o nazwie *BeginOperationName* i *EndOperationName*, który rozpoczęcia i zakończenia operacji asynchronicznej.

*BeginOperationName* metody rozpoczyna operację asynchroniczną i zwraca obiekt, który implementuje `IAsyncResult` interfejsu. Po wywołaniu *BeginOperationName*, aplikacja może kontynuować wykonywania instrukcji w wątku wywołującym, gdy operacja asynchroniczna odbywa się w wątku puli wątków.

Dla każdego wywołania *BeginOperationName*, również powinny wywoływać aplikacji *EndOperationName* Aby uzyskać wyniki operacji. Wartość zwracana *EndOperationName* jest ten sam typ zwracany przez metodę usługi sieci web synchronicznego. Poniższy przykładowy kod przedstawia przykład:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Zadania biblioteki równoległych (TPL) może uprościć proces zużywa pary metod begin/end APM hermetyzując operacji asynchronicznych w tym samym `Task` obiektu. Ta hermetyzacja są dostarczane przez wielu przeładowań `Task.Factory.FromAsync` metody. Ta metoda tworzy `Task` , który jest wykonywany `TodoServiceClient.EndGetTodoItems` metody raz `TodoServiceClient.BeginGetTodoItems` metody zakończeniu z `null` parametr wskazujący, że żadne dane nie jest przekazywany do `BeginGetTodoItems` delegowanie. Na koniec wartości `TaskCreationOptions` wyliczenie Określa, czy domyślne zachowanie dla tworzenia i uruchamiania zadań ma być używany.

Aby uzyskać więcej informacji na temat APM, zobacz [Model programowania asynchronicznego](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx) i [TPL i tradycyjnych .NET Framework asynchronicznego programowania](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx) w witrynie MSDN.

Aby uzyskać więcej informacji dotyczących używania usługi WCF, zobacz [konsumowania usługi sieci Web Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>Za pomocą zabezpieczeń transportu

Usługi WCF, mogą stosować zabezpieczeń na poziomie transportu do ochrony przed przechwyceniem wiadomości. Platformy Xamarin obsługuje powiązań korzystających z zabezpieczeń na poziomie transportu przy użyciu protokołu SSL. Jednak może być przypadki, w których stosu może być konieczne weryfikacji certyfikatu, co powoduje nieprzewidziane zachowanie. Sprawdzanie poprawności może zostać przesłonięta przez zarejestrowanie `ServerCertificateValidationCallback` delegata przed wywołaniem usługi, jak pokazano w poniższym przykładzie:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Zapewnia to dostępność szyfrowania transportu podczas ignorowanie weryfikacji certyfikatu po stronie serwera. Jednak takie podejście skutecznie pomija dotyczy zaufania skojarzony z certyfikatem i mogą nie być odpowiednie. Aby uzyskać więcej informacji, zobacz [przy użyciu zaufanego Respectfully katalogów głównych](http://www.mono-project.com/UsingTrustedRootsRespectfully) na [mono project.com](http://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>Korzystanie z zabezpieczeń poświadczeń klienta

Usługi WCF, mogą także wymagać klientom usługi uwierzytelniania za pomocą poświadczeń. Platformy Xamarin nie obsługuje protokołu WS-Security, który umożliwia klientom wysyłanie poświadczenia wewnątrz koperty wiadomości SOAP. Platformy Xamarin obsługuje jednak możliwość wysyłania poświadczenia podstawowego uwierzytelniania HTTP do serwera, określając odpowiedni `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Następnie można określić poświadczenia uwierzytelniania podstawowego:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

W powyższym przykładzie, jeśli zostanie wyświetlony komunikat "Zabrakło trampolines typu 0" można zwiększyć liczbę trampolines typu 0, dodając `–aot “trampolines={number of trampolines}”` argument do kompilacji. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów](~/ios/troubleshooting/troubleshooting.md#trampolines).

Aby uzyskać więcej informacji na temat uwierzytelniania podstawowego HTTP, mimo że w kontekście usługi sieci web REST, zobacz [uwierzytelniania usługi sieci Web RESTful](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="summary"></a>Podsumowanie

W tym przewodniku przedstawiono sposób korzystać z technologii usług sieci web w innej. Tematy obejmują komunikacji z usługi REST, usług SOAP i usług Windows Communication Foundation.

## <a name="related-links"></a>Linki pokrewne

- [Przykładowe usług sieci Web](https://developer.xamarin.com/samples/mobile/WebServices/WebServiceSamples/)
- [Usługi sieci Web w platformy Xamarin.Forms](~/xamarin-forms/data-cloud/index.md)
- [Narzędzie do obsługi metadanych elementu ServiceModel (svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](http://msdn.microsoft.com/en-us/library/system.servicemodel.basichttpbinding.aspx)
