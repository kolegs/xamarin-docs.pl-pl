---
title: Wskazówki — Praca z programem WCF
description: W tym przewodniku opisano, jak aplikacji mobilnej skompilowanej za pomocą platformy Xamarin może korzystać z usługi sieci web WCF za pomocą klasy BasicHttpBinding.
ms.prod: xamarin
ms.assetid: D0E83342-2E79-4D25-BD57-43718F5628C4
author: asb3993
ms.author: amburns
ms.date: 02/17/2018
ms.openlocfilehash: b0502028985cf35498a5811ec88b9e32d74d0311
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="walkthrough---working-with-wcf"></a>Wskazówki — Praca z programem WCF

_W tym przewodniku opisano, jak aplikacji mobilnej skompilowanej za pomocą platformy Xamarin może korzystać z usługi sieci web WCF za pomocą klasy BasicHttpBinding._


Jest wymagane wspólne dla aplikacji mobilnych można było nawiązać połączenia z systemów zaplecza. Istnieje wiele opcji i opcje dla struktury wewnętrznej bazy danych, z których jeden jest [Windows Communication Foundation](http://msdn.microsoft.com/library/ms731082.aspx) (WCF). Ten przewodnik zapewnia przykładowy sposób aplikacjami mobilnymi Xamarin może korzystać z usługi WCF za pomocą `BasicHttpBinding` klasy. Przewodnik zawiera następujące tematy:

1.  **Tworzenie usługi WCF** — w tej sekcji utworzymy bardzo podstawowe usługi WCF mających dwie metody. Pierwsza metoda potrwa parametr typu string, podczas gdy inne metody spowoduje przejście obiektu C#. W tej sekcji przedstawimy również sposób konfigurowania stacji roboczej developer, aby umożliwić uzyskiwanie zdalnego dostępu do usługi WCF.
1.  **Tworzenie aplikacji platformy Xamarin.Android** — po utworzeniu usługi WCF, utworzysz prostą aplikację platformy Xamarin.Android, który będzie używany przez usługę WCF. W tej sekcji opisano sposób tworzenia klasy serwera proxy usługi WCF, ułatwiający komunikację z usługą WCF.
1.  **Tworzenie aplikacji platformy Xamarin.iOS** -ostatnia część ten samouczek obejmuje tworzenia prostej aplikacji platformy Xamarin.iOS, który będzie używany przez usługę WCF.

<a name="Requirements" />

## <a name="requirements"></a>Wymagania

W tym przewodniku przyjęto założenie, że masz pewną znajomość tworzenia i używania usługi WCF.

<a name="Creating_a_WCF_Service" />

## <a name="creating-a-wcf-service"></a>Tworzenie usługi WCF

Pierwszym zadaniem przed nami jest tworzenie usługi WCF dla aplikacji mobilnych do komunikowania się z.

1. Uruchom program Visual Studio 2017 i Utwórz nowy projekt.
1. W **nowy projekt** okno dialogowe, wybierz opcję **WCF > biblioteki usługi WCF** szablonu i nazwy rozwiązania `HelloWorldService`:

    ![](walkthrough-working-with-wcf-images/new-wcf-service.png "Tworzenie nowej biblioteki usługi WCF")

1. W **Eksploratora rozwiązań**, Dodaj nową klasę o nazwie `HelloWorldData` do projektu:

    ```csharp
        using System.Runtime.Serialization;

        namespace HelloWorldService
        {
            [DataContract]
            public class HelloWorldData
            {
                [DataMember]
                public bool SayHello { get; set; }

                [DataMember]
                public string Name { get; set; }

                public HelloWorldData()
                {
                    Name = "Hello ";
                    SayHello = false;
                }
            }
        }
    ```


1. W **Eksploratora rozwiązań**, Zmień nazwę `IService1.cs` do `IHelloWorldService.cs`i Zmień nazwę `Service1.cs` do `HelloWorldService.cs`.
1. W **Eksploratora rozwiązań**, otwórz `IHelloWorldService.cs` i Zastąp kod następującym kodem:

    ```csharp
        using System.ServiceModel;

        namespace HelloWorldService
        {
            [ServiceContract]
            public interface IHelloWorldService
            {
                [OperationContract]
                string SayHelloTo(string name);

                [OperationContract]
                HelloWorldData GetHelloData(HelloWorldData helloWorldData);
            }
        }
    ```
  
    Ta usługa udostępnia dwie metody — pobierający ciąg dla parametru i drugą wyższy obiektu .NET.

1. W **Eksploratora rozwiązań**, otwórz `HelloWorldService.cs` i Zastąp kod następującym kodem:

    ```csharp
        using System;

        namespace HelloWorldService
        {
            public class HelloWorldService : IHelloWorldService
            {
                public HelloWorldData GetHelloData(HelloWorldData helloWorldData)
                {
                    if (helloWorldData == null)
                        throw new ArgumentException("helloWorldData");

                    if (helloWorldData.SayHello)
                        helloWorldData.Name = "Hello World to {helloWorldData.Name}";

                    return helloWorldData;
                }

                public string SayHelloTo(string name)
                {
                    return "Hello World to you, {name}";
                }
            }
        }
    ```

1. W **Eksploratora rozwiązań**, otwórz `App.config`, zaktualizuj `name` atrybutu `<service>` węzła, `contract` atrybutu `<endpoint>` węzeł i `baseAddress` atrybutu `<add>` węzła:

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            ...
            <services>
              <service name="HelloWorldService.HelloWorldService">
                <endpoint address="" binding="basicHttpBinding" contract="HelloWorldService.IHelloWorldService">
                  <identity>
                    <dns value="localhost" />
                  </identity>
                </endpoint>
                <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange" />
                <host>
                  <baseAddresses>
                    <add baseAddress="http://localhost:8733/Design_Time_Addresses/HelloWorldService/" />
                  </baseAddresses>
                </host>
              </service>
            </services>
            ...
        </configuration>
    ```

1. Skompiluj i uruchom usługę WCF. Usługa będzie obsługiwana przez klienta testowego WCF:

    ![](walkthrough-working-with-wcf-images/hosted-wcf-service.png "Usługi WCF uruchomionej w kliencie testowym")

1. Z klienta testowego WCF uruchomiona Uruchom przeglądarkę i przejdź do punktu końcowego usługi WCF:

    ![](walkthrough-working-with-wcf-images/wcf-service-browser.png "Strona informacje o przeglądarce usługi WCF")

> [!IMPORTANT]
> Poniższa sekcja jest wymagane tylko jeśli potrzebujesz akceptowanie połączeń zdalnych na stacji roboczej systemu Windows 10. Sekcji można zignorować, jeśli masz alternatywny platformy, na którym chcesz wdrożyć usługę WCF.

<a name="Allow_Remote_Access_to_IIS_Express" />

## <a name="configuring-remote-access-to-iis-express"></a>Konfigurowanie zdalnego dostępu do usług IIS Express

Hosting usług WCF lokalnie jest odpowiednia, gdy występują tylko połączenia z komputera lokalnego. Jednak zdalnego urządzenia (takich jak urządzenia z systemem Android lub iPhone) nie ma dostępu do lokalnej usługi WCF. W związku z tym w tej sekcji opisano sposób konfigurowania systemu Windows 10 i usług IIS Express, akceptowanie połączeń zdalnych:

1.  **Konfigurowanie usług IIS Express do akceptowania zdalnych połączeń** — ten krok polega na edycji pliku konfiguracji dla usług IIS Express akceptowanie połączeń zdalnych na określonym porcie, a następnie konfigurowania reguł dla usług IIS Express do akceptowania ruchu przychodzącego.
1.  **Dodaj wyjątek do zapory systemu Windows** — należy otworzyć port przez zaporę systemu Windows, które aplikacje zdalne mogą używać do komunikowania się z usługą WCF.

    Musisz znać adres IP stacji roboczej. Na potrzeby tego przykładu przyjmiemy, że nasze stacji roboczej ma adres IP 192.168.1.143.

1. Zacznijmy przez skonfigurowanie usług IIS Express do nasłuchiwania żądań zewnętrznych. Firma Microsoft może to zrobić, edytując plik konfiguracji dla usług IIS Express w `[solutiondirectory]\.vs\config\applicationhost.config`, jak pokazano na poniższym zrzucie ekranu:

    [![](walkthrough-working-with-wcf-images/image05.png "Firma Microsoft to zrobić, edytując plik konfiguracji dla usług IIS Express w solutiondirectory.vsconfigapplicationhost.config, jak pokazano w tym zrzut ekranu")](walkthrough-working-with-wcf-images/image05.png#lightbox)

    Zlokalizuj `site` elementu o nazwie `HelloWorldWcfHost`. Powinien on wyglądać podobnie jak następujący fragment kodu XML:

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
            </bindings>
        </site>
    ```
 
    Musimy dodać kolejne `binding` do otwarcia portu 8734 poza ruchu. Dodaj następujący kod XML `bindings` elementu, zastępując adres IP z adresu IP:

    ```xml
    <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
    ```
    
    Spowoduje to skonfigurowanie usług IIS Express do akceptowania ruchu HTTP z dowolnego zdalnego adresu IP na porcie 8734 zewnętrzny adres IP komputera. Powyżej fragment przy założeniu, że adres IP komputera z programem IIS Express jest 192.168.1.143. Po wprowadzeniu zmian `bindings` element powinien wyglądać następująco:

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
                <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
            </bindings>
        </site>
    ```

1. Następnie należy skonfigurować usługi IIS Express akceptować połączeń przychodzących na porcie 8734. Uruchamianie Konfigurowanie administracyjny wiersz polecenia i uruchom to polecenie:

    `> netsh http add urlacl url=http://192.168.1.143:9608/ user=everyone`

1. Ostatnim krokiem jest skonfigurowanie zapory systemu Windows, aby zezwolić na ruch zewnętrzny na porcie 8734. Z wiersza polecenia z uprawnieniami administracyjnymi uruchom następujące polecenie:

    `> netsh advfirewall firewall add rule name="IISExpressXamarin" dir=in protocol=tcp localport=8734 profile=private remoteip=localsubnet action=allow`

    To polecenie umożliwia ruch przychodzący na porcie 8734 ze wszystkich urządzeń w tej samej podsieci, jako stacją roboczą systemu Windows 10.

Utworzono bardzo podstawowe usługi WCF hostowanych w usługach IIS Express, która będzie akceptować połączenia przychodzące z innymi urządzeniami lub komputerami w naszym podsieci. Możesz przetestować ten limit, uruchamiania aplikacji i odwiedzający `http://localhost:8733/Design_Time_Addresses/HelloWorldService/` na stacji roboczej i `http://192.168.1.143:8734/Design_Time_Addresses/HelloWorldService/` z innego komputera w podsieci.

Aby umożliwić usług IIS Express do zachowania uruchomiona i obsługująca usługę, należy wyłączyć **Edytuj i Kontynuuj** opcji *właściwości projektu > sieci Web > debugery*.

## <a name="creating-a-proxy-for-the-web-service"></a>Tworzenie serwera Proxy usługi sieci Web

Serwer proxy usługi sieci web musi zostać utworzony dla usługi WCF, zanim aplikacja może korzystać z usługi. Można to zrobić w następujący sposób:

1. Dodawanie biblioteki klas standardowe .NET o nazwie `HelloWorldServiceProxy`i usunąć wszystkie klasy w projekcie.
1. Uruchom `HelloWorldService` projektu.
1. Z `HelloWorldService` projektu działa prawidłowo, należy dodać nowy **podłączonej usługi** do projektu przy użyciu **dostawcy odwołania dla usług sieci Web firmy Microsoft WCF**.
1. W **punktu końcowego usługi** karcie **skonfigurować odwołania usługi sieci Web WCF** okna dialogowego, kliknij przycisk **odnajdowania** przycisku, Usuń `mex` od końca wykrytych punkt końcowy w **URI** listy rozwijanej, wprowadź `HelloWorldServiceProxy` jako **Namespace**i kliknij przycisk **dalej** przycisku.
1. W **opcje typu danych** karcie **skonfigurować odwołania usługi sieci Web WCF** okna dialogowego, zaakceptuj ustawienia domyślne, klikając **dalej** przycisku.
1. W **opcje klienta** karcie **skonfigurować odwołania usługi sieci Web WCF** okna dialogowego, upewnij się, że **publicznego** pole wyboru jest zaznaczone, a następnie kliknij przycisk **Zakończ**  przycisku.
1. Tworzenie `HelloWorldServiceProxy` projektu.

> [!NOTE]
> Zamiast tworzenia proxy przy użyciu dostawcy odwołanie usług sieci Web WCF firmy Microsoft w programie Visual Studio 2017 jest za pomocą narzędzia narzędzie metadanych elementu ServiceModel (svcutil.exe). Aby uzyskać więcej informacji, zobacz [narzędzie narzędzia metadanych elementu ServiceModel (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Creating_a_Xamarin_Android_Application" />

## <a name="creating-a-xamarinandroid-application"></a>Tworzenie aplikacji platformy Xamarin.Android

Serwer proxy usługi WCF mogą być używane przez aplikację platformy Xamarin.Android w następujący sposób:

1. W programie Visual Studio, należy dodać nowy pusty projekt systemu Android do rozwiązania i nadaj mu nazwę `HelloWorld.Android`.
1. W `HelloWorld.Android` projekt, Dodaj odwołanie do `HelloWorldServiceProxy` projektu i odwołania do `System.ServiceModel` przestrzeni nazw.
1. W **Eksploratora rozwiązań**, otwórz `Resources/layout/main.axml` i Zastąp istniejące XML następujący kod XML:

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                  android:orientation="vertical"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent">
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/sayHelloWorldButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/say_hello_world" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/sayHelloWorldTextView" />
            </LinearLayout>
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/getHelloWorldDataButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/get_hello_world_data" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/getHelloWorldDataTextView" />
            </LinearLayout>
        </LinearLayout>
    ```
    
    Poniższe zrzuty ekranu pokazuje interfejsu użytkownika w Projektancie:

    [![](walkthrough-working-with-wcf-images/image09.png "To, jak wygląda interfejs ten w Projektancie zrzut ekranu")](walkthrough-working-with-wcf-images/image09.png#lightbox)
    
1. W **Eksploratora rozwiązań**, otwórz `Resources/values/Strings.xml` i Dodaj następujący kod XML:

    ```xml
    <string name="say_hello_world">Say Hello World</string>
    <string name="get_hello_world_data">Get Hello World data</string>
    ```
    
1. W **Eksploratora rozwiązań**, otwórz `MainActivity.cs` i Zastąp istniejący kod następującym kodem:

    ```csharp
        [Activity(Label = "HelloWorld.Android", MainLauncher = true)]
        public class MainActivity : Activity
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");

            HelloWorldServiceClient _client;
            Button _getHelloWorldDataButton;
            TextView _getHelloWorldDataTextView;
            Button _sayHelloWorldButton;
            TextView _sayHelloWorldTextView;
            ...
        }
    ```

    Zastąp `<insert_WCF_service_endpoint_here>` adres punktu końcowego WCF.

1. W `MainActivity.cs`, zmodyfikuj `OnCreate` metodę, którą zawiera następujący kod:

    ```csharp
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            InitializeHelloWorldServiceClient();

            // This button will invoke the GetHelloWorldData - the method that takes a C# object as a parameter.
            _getHelloWorldDataButton = FindViewById<Button>(Resource.Id.getHelloWorldDataButton);
            _getHelloWorldDataButton.Click += GetHelloWorldDataButtonOnClick;
            _getHelloWorldDataTextView = FindViewById<TextView>(Resource.Id.getHelloWorldDataTextView);

            // This button will invoke SayHelloWorld - this method takes a simple string as a parameter.
            _sayHelloWorldButton = FindViewById<Button>(Resource.Id.sayHelloWorldButton);
            _sayHelloWorldButton.Click += SayHelloWorldButtonOnClick;
            _sayHelloWorldTextView = FindViewById<TextView>(Resource.Id.sayHelloWorldTextView);
        }
    ```
    
    Powyższy kod inicjuje zmienne wystąpienia klasy i tworzącej niektóre programy obsługi zdarzeń.

1. W `MainActivity.cs`, utworzyć wystąpienia klasy serwera proxy klienta przez dodanie następujących dwóch metod:

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
    
    Powyższy kod tworzy i inicjuje `HelloWorldServiceClient` obiektu.

1. W `MainActivity.cs`, Dodaj obsługę nawet dla dwóch przycisków w `Activity`:

    ```csharp
        async void GetHelloWorldDataButtonOnClick(object sender, EventArgs e)
        {
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            _getHelloWorldDataTextView.Text = "Waiting for WCF...";
            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                _getHelloWorldDataTextView.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButtonOnClick(object sender, EventArgs e)
        {
            _sayHelloWorldTextView.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                _sayHelloWorldTextView.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```
  
1. Uruchom aplikację, upewnij się, że usługa WCF jest uruchomiona, a następnie kliknij polecenie dwóch przycisków. Aplikacja będzie wywoływać usługi WCF asynchronicznie, pod warunkiem, że `Endpoint` poprawnie skonfigurowano pola:

    [![](walkthrough-working-with-wcf-images/image08.png "W ciągu 30 sekund odpowiedzi powinien być pobrany z każdej metody WCF i naszej aplikacji powinien wyglądać jak tego zrzutu ekranu")](walkthrough-working-with-wcf-images/image08.png#lightbox)

<a name="Creating_a_Xamarin_iOS_Application" />

## <a name="creating-a-xamarinios-application"></a>Tworzenie aplikacji platformy Xamarin.iOS

Serwer proxy usługi WCF mogą być używane przez aplikację platformy Xamarin.iOS w następujący sposób:

1. W programie Visual Studio, Dodaj nowe iPhone **pojedynczą aplikacją widoku** projektu do rozwiązania i nadaj mu nazwę `HelloWorld.iOS`.
1. W `HelloWorld.iOS` projekt, Dodaj odwołanie do `HelloWorldServiceProxy` projektu i odwołania do `System.ServiceModel` przestrzeni nazw.
1. W **Eksploratora rozwiązań**, kliknij dwukrotnie `Main.storyboard` można otworzyć pliku w Projektancie systemu iOS. Następnie należy dodać następujące `UIButton` i `UITextView` kontrolki:

    ||Nazwa|Tytuł|
    |--- |--- |--- |
    |`UIButton`|`sayHelloWorldButton`|Powiedz "Hello, World"|
    |`UITextView`|`sayHelloWorldText`||
    |`UIButton`|`getHelloWorldDataButton`|Get "Hello, World" Data|
    |`UITextView`|`getHelloWorldDataText`||

    Po dodaniu formantów, interfejs użytkownika powinien wyglądać Poniższy zrzut ekranu:

    [![](walkthrough-working-with-wcf-images/image12.png "Po dodaniu formantów, interfejs użytkownika powinien przypominać ten zrzut ekranu")](walkthrough-working-with-wcf-images/image12.png#lightbox)

1. W **Eksploratora rozwiązań**, otwórz `ViewController.cs` i Dodaj następujący kod:

    ```xml
        public partial class ViewController : UIViewController
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");
            HelloWorldServiceClient _client;
            ...
        }
    ```
  
    Zastąp `<insert_WCF_service_endpoint_here>` adres punktu końcowego WCF.

1. W `ViewController.cs`, zaktualizuj `ViewDidLoad` metodę, którą jest podobny do następującego:

    ```csharp
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            InitializeHelloWorldServiceClient();

            getHelloWorldDataButton.TouchUpInside += GetHelloWorldDataButton_TouchUpInside;
            sayHelloWorldButton.TouchUpInside += SayHelloWorldButton_TouchUpInside;
        }
    ```
  
1. W `ViewController.cs`, Dodaj `InitializeHelloWorldServiceClient` i `CreateBasicHttpBinding` metod:

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
  
1. W `ViewController.cs`, Dodaj obsługę zdarzeń `TouchUpInside` zdarzeń na dwa `UIButton` wystąpień:

    ```csharp
        async void GetHelloWorldDataButton_TouchUpInside(object sender, EventArgs e)
        {
            getHelloWorldDataText.Text = "Waiting for WCF...";
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                getHelloWorldDataText.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButton_TouchUpInside(object sender, EventArgs e)
        {
            sayHelloWorldText.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                sayHelloWorldText.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```

1. Uruchom aplikację, upewnij się, że usługa WCF jest uruchomiona, a następnie kliknij polecenie dwóch przycisków. Aplikacja będzie wywoływać usługi WCF asynchronicznie, pod warunkiem, że `Endpoint` poprawnie skonfigurowano pola:

    [![](walkthrough-working-with-wcf-images/image10.png "W ciągu 30 sekund odpowiedzi powinien być pobrany z każdej metody WCF i naszej aplikacji powinno wyglądać podobnie do tego zrzutu ekranu")](walkthrough-working-with-wcf-images/image10.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Podsumowanie

W tym samouczku opisano sposób pracy z usługą WCF w aplikacji mobilnej przy użyciu platformy Xamarin.Android i Xamarin.iOS. On pokazano, jak utworzyć usługę WCF i wyjaśniono sposób konfigurowania systemu Windows 10 i usług IIS Express do akceptowania połączeń z urządzeń zdalnych. Następnie wyjaśniono sposób generowania klienta serwera proxy usługi WCF i przedstawiono sposób użycia klienta serwera proxy w aplikacjach zarówno platformy Xamarin.Android i Xamarin.iOS.


## <a name="related-links"></a>Linki pokrewne

- [HelloWorld (przykład)](https://developer.xamarin.com/samples/mobile/WCF-Walkthrough/)
- [Tworzenie zorientowane na usługę aplikacji za pomocą usługi WCF](https://docs.microsoft.com/dotnet/framework/wcf/index)
- [Porady: Tworzenie klienta Windows Communication Foundation](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [Narzędzie do obsługi metadanych elementu ServiceModel (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
