---
title: Mowy systemu android
description: W tym artykule opisano podstawy za pomocą bardzo zaawansowane Android.Speech przestrzeni nazw. Od czasu jego powstania Android mógł rozpoznawanie mowy i wyjścia go jako tekst. Jest procesem stosunkowo proste. Tekst na mowę, jednak proces jest więcej wysiłku, jako nie tylko przez aparat rozpoznawania mowy trzeba należy wziąć pod uwagę, ale także języki dostępne i zainstalowane w systemie tekst na mowę (TTS).
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/02/2018
ms.openlocfilehash: bdaa9bf09485c06551a2df15a2e3a4b410a53e75
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
---
# <a name="android-speech"></a>Mowy systemu android

_W tym artykule opisano podstawy za pomocą bardzo zaawansowane Android.Speech przestrzeni nazw. Od czasu jego powstania Android mógł rozpoznawanie mowy i wyjścia go jako tekst. Jest procesem stosunkowo proste. Tekst na mowę, jednak proces jest więcej wysiłku, jako nie tylko przez aparat rozpoznawania mowy trzeba należy wziąć pod uwagę, ale także języki dostępne i zainstalowane w systemie tekst na mowę (TTS)._

## <a name="speech-overview"></a>Omówienie mowy

Mającej system, który "rozumie" ludzkiej mowy i enunciates co wpisaniu — mowy tekstu i zamiany tekstu na mowę — się kiedykolwiek rosnącym obszarze przenośnych programowanie wzrostu popytu na fizyczną komunikację z naszych urządzeń. Wiele wystąpień gdzie o funkcji, który konwertuje tekst na mowę, lub odwrotnie, jest bardzo przydatne narzędzia, aby dołączyć do aplikacji systemu android.

Na przykład z clamp dół przy użyciu telefonu komórkowego podczas kierowania użytkownicy mają sposób wolnego ręce systemu operacyjnego swoich urządzeń. Nadmiar różnych rozmiarach Android — takich jak Android nosić — i rozszerzanie kiedykolwiek uwzględnienie tych można używać urządzeń z systemem Android (takich jak tablety i tablety Uwaga), został utworzony większych fokus na niezawodnych aplikacji TTS.

Google dostarcza dewelopera przy użyciu bogaty zestaw interfejsów API w przestrzeni nazw Android.Speech, aby pokrywał się największą liczbą wystąpień dokonywania urządzenia "mowy pamiętać" (na przykład oprogramowanie przeznaczone dla blind).  Ta przestrzeń nazw obejmuje funkcji tekst na mowę za pośrednictwem `Android.Speech.Tts`, kontrolę nad aparatu używane w celu wykonania tłumaczenia, a także szereg `RecognizerIntent`s, która zezwala na mowę, które ma zostać przekonwertowane na tekst.

Gdy urządzenia są związane z mowy zrozumienie, istnieją ograniczenia na podstawie sprzętu używane. Jest mało prawdopodobne, że urządzenie zinterpretuje pomyślnie wszystko wypowiadane w każdym języku dostępne.

## <a name="requirements"></a>Wymagania

Nie ma żadnych specjalnych wymagań tego przewodnika, innego niż urządzenie mikrofon i prelegenta.

Podstawowe urządzenia z systemem Android interpretowanie mowy jest użycie `Intent` z odpowiednią `OnActivityResult`.
Jednak ważne jest rozpoznawanie mowy nie jest rozpoznawany — ale interpretowany na tekst. Różnica polega na ważne.

### <a name="the-difference-between-understanding-and-interpreting"></a>Różnica między opis i interpretowanie

Definicja prosty opis jest możliwość określenia sygnału i kontekstu są prawdziwe znaczenie. Aby zinterpretować właśnie oznacza, że słowa podjęcia i wyjścia je w innej formy.

Należy wziąć pod uwagę następujące prosty przykład używanego w zwykłych konwersacji: 

<kbd>Witaj jak się masz?</kbd>

Bez modulacją (nacisk dotyczącymi słów lub części słowa) jest proste pytanie. Jednak jeśli powolne tempo zostanie zastosowany do wiersza, osoba nasłuchiwania wykryje, że autora pytania nie jest zbyt wszystkiego i prawdopodobnie musi cheering lub autora pytania jest unwell. Jeśli nacisk jest kładziony na "to", osoby, prosząc jest zazwyczaj bardziej zainteresowane w odpowiedzi.

Bez audio dość zaawansowanego przetwarzania, aby użyć modulacją i stopień sztucznego analizy (AI) zrozumienie kontekstu, oprogramowanie nawet nie można rozpocząć zrozumieć, jakie wspomniano — najlepiej wykonać proste telefonu jest konwertowanie mowy na tekst.

## <a name="setting-up"></a>Konfigurowanie

Przed rozpoczęciem korzystania z systemu mowy dobrze jest zawsze Sprawdź, czy urządzenie ma mikrofon. Byłoby małego punktu próby uruchomienia aplikacji w konsoli Uwaga urządzeń Kindle lub Google bez mikrofon zainstalowane.

Poniższy przykładowy kod przedstawia zapytanie, jeśli mikrofon jest dostępny, a jeśli nie, można utworzyć alertu. Jeśli nie mikrofon jest dostępny w tym momencie możesz czy zakończyć działanie albo wyłącz możliwość rejestrowania mowy.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>Tworzenie celem

Celem systemu mowy używa określonego typu założeń o nazwie `RecognizerIntent`. Celem tej kontrolki dużej liczby parametrów, takich jak czas będzie zaczekać z wyciszenia nagrywania uważa się za pośrednictwem wszelkie dodatkowe języki do rozpoznania i danych wyjściowych i tekst do uwzględnienia w `Intent`w modalnego okna dialogowego, co oznacza, że instrukcji. W tym fragmencie `VOICE` jest `readonly int` używany do rozpoznawania w `OnActivityResult`.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>Konwersja mowy

Tekst interpretowane z mowy zostanie dostarczone w `Intent`, który jest zwracany, gdy działanie zostało zakończone i jest dostępny za pośrednictwem `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Spowoduje to zwrócenie `IList<string>`, z którego indeks może być używany i wyświetlane zależnie od liczby języki wymagane w przypadku zamiaru wywołującego (i w określonym `RecognizerIntent.ExtraMaxResults`). Zgodnie z żadnej listy warto jednak sprawdzania, upewnij się, że jest danych do wyświetlenia.

Podczas nasłuchiwania dla wartości zwracanej z `StartActivityForResult`, `OnActivityResult` metoda musi zostać dostarczona.

W poniższym przykładzie `textBox` jest `TextBox` używane wyprowadzania, co ma zostać definiowane. Równie może zostać wykorzystana do przekazania do jakiegoś interpretera, a tekst, aplikację można porównać tekst i gałęzi do innej części aplikacji.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
             if (matches.Count != 0)
             {
                  string textInput = textBox.Text + matches[0];
                  textBox.Text = textInput;
                  switch(matches[0].Substring(0,5).ToLower())
                  {
                     case "north":
                      MovePlayer(0);
                     break;
                   case "south":
                     MovePlayer(1);
                     break;
             }
             else
                  textBox.Text = "No speech was recognised";
        }
   }
    base.OnActivityResult(requestCode, resultVal, data);
}
```

## <a name="text-to-speech"></a>Tekst na mowę

Tekst na mowę, nie jest dość wstecznego mowy na tekst i opiera się na dwóch składnikach klucza; tekst na mowę aparat jest zainstalowany na urządzeniu i instalowane języka.

Przede wszystkim, urządzenia z systemem Android pochodzą z domyślnym zainstalowanej usługi Google TTS i co najmniej jeden język. To jest nawiązywane po pierwszym skonfigurowaniu i będą oparte na których urządzenie jest w tym czasie (na przykład telefon w Niemczech zainstaluje języka niemieckiego w Ameryce zostaną angielski).

### <a name="step-1---instantiating-texttospeech"></a>Krok 1 — Tworzenie wystąpień TextToSpeech

`TextToSpeech` może potrwać do 3 parametry, dwa pierwsze są wymagane w przypadku innych opcjonalne (`AppContext`, `IOnInitListener`, `engine`). Odbiornik jest używane dla wiązania usługi i test awarii z aparatem są dostępne dla systemu Android tekst na mowę aparaty dowolną liczbę. Co najmniej urządzenia będą mieć aparat firmy Google.

### <a name="step-2---finding-the-languages-available"></a>Krok 2 — wyszukiwanie dostępnych języków

`Java.Util.Locale` Klasa zawiera przydatne metody o nazwie `GetAvailableLocales()`. Ta lista języków obsługiwanych przez aparat rozpoznawania mowy testowana następnie zainstalowane języki.

Jest to kwestia trivial wygenerować listę języków "zrozumiał". Zawsze będzie język domyślny (język użytkownik ustawił podczas najpierw konfigurowania urządzenia), tak aby w tym przykładzie `List<string>` ma "Default" jako pierwszego parametru, w zależności od wyniku zostaną wypełnione w pozostałej części listy `textToSpeech.IsLanguageAvailable(locale)`.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

Ten kod wywołuje [TextToSpeech.IsLanguageAvailable](https://developer.xamarin.com/api/member/Android.Speech.Tts.TextToSpeech.IsLanguageAvailable/p/Java.Util.Locale/) Aby sprawdzić, czy pakiet językowy dla danego ustawień regionalnych jest już obecny na urządzeniu. Ta metoda zwraca [LanguageAvailableResult](https://developer.xamarin.com/api/type/Android.Speech.Tts.LanguageAvailableResult/), która wskazuje, czy język dla przekazanych ustawień regionalnych jest dostępne. Jeśli `LanguageAvailableResult` oznacza, że język `NotSupported`, a następnie nie ma pakietu głosu dostępnej (nawet do pobrania) dla tego języka. Jeśli `LanguageAvailableResult` ma ustawioną wartość `MissingData`, a następnie można pobrać pakiet językowy, jak wyjaśniono poniżej w kroku 4.

### <a name="step-3---setting-the-speed-and-pitch"></a>Krok 3 — Ustawianie szybkości i wysokości

Android zezwala użytkownikowi na alter dźwięk mowy, zmieniając `SpeechRate` i `Pitch` (liczba szybkości i ton mowy). To łączy się z zakresu od 0 do 1, z mowy "normal", jest 1 dla obu.

### <a name="step-4---testing-and-loading-new-languages"></a>Krok 4 — testowanie i ładowanie nowych języków

Pobieranie nowy język jest wykonywane przy użyciu `Intent`. Wynik tego celem powoduje, że [OnActivityResult](https://developer.xamarin.com/api/member/Android.App.Activity.OnActivityResult/) wywoływanej metody. W odróżnieniu od przykład mowy na tekst (, których [RecognizerIntent](https://developer.xamarin.com/api/type/Android.Speech.RecognizerIntent/) jako `PutExtra` parametr `Intent`), testowania i ładowanie `Intent`s są `Action`— na podstawie:

-   [TextToSpeech.Engine.ActionCheckTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionCheckTtsData/) &ndash; rozpoczyna działania od platformy `TextToSpeech` aparatu, aby sprawdzić prawidłowej instalacji i dostępność zasobów języka na urządzeniu.

-   [TextToSpeech.Engine.ActionInstallTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionInstallTtsData/) &ndash; rozpoczyna się działanie, które monituje użytkownika o języków niezbędnych do pobrania.

Poniższy przykładowy kod przedstawia sposób używać tych działań do testowania dla zasobów językowych i Pobierz nowy język:

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

`TextToSpeech.Engine.ActionCheckTtsData` badania dostępności zasobów językowych. `OnActivityResult` jest wywoływane po zakończeniu tego testu. Jeśli język zasobów muszą być pobierane, `OnActivityResult` generowane `TextToSpeech.Engine.ActionInstallTtsData` akcji, aby uruchomić działanie, które pozwala użytkownikom na pobieranie języków niezbędnych. Uwaga tego `OnActivityResult` implementacji nie sprawdza `Result` kodu, ponieważ w tym przykładzie uproszczony określenie podjęto już pakiet językowy muszą zostać pobrane.

`TextToSpeech.Engine.ActionInstallTtsData` Przyczyny akcji **danych głosowych Google TTS** działania będą widoczne dla użytkownika dotyczące wybierania języków do pobrania:

![Działania danych głosowych Google TTS](speech-images/01-google-tts-voice-data.png)

Na przykład użytkownik może wybrać francuski, a następnie kliknij ikonę pobierania podczas pobierania danych głosowych francuskim:

![Przykład pobieranie języka francuskiego](speech-images/02-selecting-french.png)

Instalacja tych danych odbywa się automatycznie po zakończeniu pobierania.


### <a name="step-5---the-ioninitlistener"></a>Krok 5 - IOnInitListener

Dla działania można było przekonwertować tekst na mowę, metody interfejsu `OnInit` musi być implementowana (jest to drugi parametr określony dla procesu tworzenia wystąpienia `TextToSpeech` klasy). Inicjuje odbiornika, a wyniki testów.

Odbiornik należy przetestować zarówno `OperationResult.Success` i `OperationResult.Failure` co najmniej.
W poniższym przykładzie pokazano tylko dla niej:

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>Podsumowanie

W tym przewodniku możemy sprawdzono podstawowe informacje o konwersji tekstu na mowę i mowy tekstu i możliwych metod sposób objąć własnych aplikacji. Podczas nie obejmują one każdym przypadku, teraz powinny mieć podstawową wiedzę na temat interpretacji mowy, jak zainstalować nowe języki i sposobu zwiększenia inclusivity aplikacji.



## <a name="related-links"></a>Linki pokrewne

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Tekst na mowę (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [Mowy na tekst (przykład)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Przestrzeń nazw Android.Speech](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Przestrzeń nazw Android.Speech.Tts](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
