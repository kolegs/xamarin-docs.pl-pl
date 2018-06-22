---
title: Android Audio
description: System operacyjny Android zapewnia zaawansowaną obsługę multimediów, obejmujące audio i wideo. Ten przewodnik koncentruje się na audio w systemie Android i obejmuje odtwarzanie oraz nagrywanie dźwięku za pomocą wbudowanych odtwarzacz audio i klasy rejestratora, jak również interfejs API audio niskiego poziomu. Obejmuje ona również pracy ze zdarzeniami Audio emisji przez inne aplikacje, dzięki czemu deweloperzy mogą tworzyć aplikacje dobrze behaved.
ms.prod: xamarin
ms.assetid: 646ED563-C34E-256D-4B56-29EE99881C27
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: aff0d67549707129bfc85246318c33c522e4f1f6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2018
ms.locfileid: "30771798"
---
# <a name="android-audio"></a>Android Audio

_System operacyjny Android zapewnia zaawansowaną obsługę multimediów, obejmujące audio i wideo. Ten przewodnik koncentruje się na audio w systemie Android i obejmuje odtwarzanie oraz nagrywanie dźwięku za pomocą wbudowanych odtwarzacz audio i klasy rejestratora, jak również interfejs API audio niskiego poziomu. Obejmuje ona również pracy ze zdarzeniami Audio emisji przez inne aplikacje, dzięki czemu deweloperzy mogą tworzyć aplikacje dobrze behaved._


## <a name="overview"></a>Omówienie

Nowoczesnych urządzeń przenośnych wdrożyły funkcje, które wcześniej wymagałoby części dedykowanych urządzeń &ndash; aparaty fotograficzne, odtwarzacze muzyki i urządzenia wideo. W związku z tym multimedialnych struktur stały się najwyższej jakości funkcji w przenośnych interfejsów API.

Android zapewnia zaawansowaną obsługę multimediów. W tym artykule sprawdza pracy z dźwiękiem w systemie Android i obejmuje następujące tematy

1.  **Odtwarzanie dźwięku za pomocą MediaPlayer** &ndash; za pomocą wbudowanych `MediaPlayer` klasy do odtwarzania dźwięku, łącznie z lokalnych plików audio i plików audio przesyłanych strumieniowo z `AudioTrack` klasy.

2.  **Nagrywanie** &ndash; za pomocą wbudowanych `MediaRecorder` klasy rejestrowanie dźwięku.

3.  **Praca z powiadomienia Audio** &ndash; używając audio powiadomienia do tworzenia aplikacji dobrze behaved poprawnej odpowiedzi na zdarzenia (np. przychodzących połączeń telefonicznych) wstrzymanie lub anulowanie ich wyjścia audio.

4.  **Praca z niższego poziomu Audio** &ndash; odtwarzanie dźwięku za pomocą `AudioTrack` klasy pisząc bezpośrednio do buforów pamięci. Rejestrowanie dźwięku za pomocą `AudioRecord` klasy i odczytu bezpośrednio z buforów pamięci.


## <a name="requirements"></a>Wymagania

W tym przewodniku wymaga 2.0 systemu Android (interfejs API na poziomie 5) lub nowszej. Należy pamiętać, że profilowanie audio w systemie Android musi zostać wykonane na urządzeniu.

Konieczne jest żądanie `RECORD_AUDIO` uprawnienia w **AndroidManifest.XML**:

![Wymagane uprawnienia sekcji manifestu systemu Android z REKORDEM\_AUDIO włączone](android-audio-images/image01.png)



## <a name="playing-audio-with-the-mediaplayer-class"></a>Odtwarzanie dźwięku za pomocą klasy MediaPlayer

Najprostszym sposobem Odtwórz dźwięk w systemie Android jest dzięki wbudowanej [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) klasy.
`MediaPlayer` pliki lokalnego lub zdalnego można odtwarzać przez przekazywanie ścieżkę pliku. Jednak `MediaPlayer` jest bardzo ważnych stanu i wywoływanie jednej z metod w niewłaściwym stanie spowoduje, że wyjątek zostanie wygenerowany. Ważne jest, aby interaktywnie `MediaPlayer` w kolejności opisane poniżej, aby uniknąć błędów.



### <a name="initializing-and-playing"></a>Inicjowanie i odtwarzania

Odtwarza dźwięk z `MediaPlayer` wymaga następującej kolejności:

1. Utwórz wystąpienie nowego [MediaPlayer](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/) obiektu.

1. Skonfigurować plik, aby odtworzyć za pomocą [SetDataSource](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.SetDataSource/p/Java.IO.FileDescriptor/) metody.

1. Wywołanie [Prepare](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Prepare/) metodę, aby zainicjować odtwarzacza.

1. Wywołanie [Start](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start/) metodę, aby rozpocząć odtwarzanie dźwięku.


Poniższy przykładowy kod przedstawia tego użycia:

```csharp
protected MediaPlayer player;
public void StartPlayer(String  filePath)
{
  if (player == null) {
    player = new MediaPlayer();
  } else {
    player.Reset();
    player.SetDataSource(filePath);
    player.Prepare();
    player.Start();
  }
}
```


### <a name="suspending-and-resuming-playback"></a>Wstrzymywanie i wznawianie odtwarzania

Odtwarzanie może zostać zawieszone przez wywołanie metody [Wstrzymaj](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Pause%28%29/) metody:

```csharp
player.Pause();
```

Aby wznawia odtwarzanie wstrzymanej, należy wywołać [Start](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Start%28%29/) metody.
Wznawia działanie wstrzymanej lokalizacji podczas odtwarzania:

```csharp
player.Start();
```

Wywoływanie [zatrzymać](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Stop%28%29/) metoda odtwarzacz kończy się bieżące odtwarzaniem:

```csharp
player.Stop();
```

Gdy odtwarzacz nie jest już potrzebne, zasoby muszą zostać zwolnione, wywołując [wersji](https://developer.xamarin.com/api/member/Android.Media.MediaPlayer.Release%28%29/) metody:

```csharp
player.Release();
```



## <a name="using-the-mediarecorder-class-to-record-audio"></a>Za pomocą klasy MediaRecorder do rekordu Audio

Następstwem do `MediaPlayer` jest nagrywanie dźwięku w systemie Android [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) klasy. Podobnie jak `MediaPlayer`, jest zależne od stanu i przechodzi przez kilka stanów na uzyskanie dostępu do punktu, w którym można uruchomić rejestrowania. Aby zarejestrować dźwięk, `RECORD_AUDIO` należy ustawić uprawnienia. Aby uzyskać instrukcje dotyczące sposobu konfigurowania aplikacji uprawnienia Zobacz [Praca z pliku AndroidManifest.xml](~/android/platform/android-manifest.md).


### <a name="initializing-and-recording"></a>Inicjowanie i rejestrowanie

Rejestrowanie dźwięku z `MediaRecorder` wymaga wykonania następujących czynności:

1. Utwórz wystąpienie nowego [MediaRecorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/) obiektu.

2. Określ, które urządzenie do przechwycenia wejście audio za pośrednictwem [SetAudioSource](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioSource/p/Android.Media.AudioSource/) metody.

3. Ustaw dane wyjściowe plików audio format przy użyciu [SetOutputFormat](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFormat/p/Android.Media.OutputFormat/) metody. Aby uzyskać listę obsługiwanych typów audio zobacz [Android obsługiwane formaty Media](http://developer.android.com/guide/appendix/media-formats.html).

4. Wywołanie [SetAudioEncoder](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetAudioEncoder/p/Android.Media.AudioEncoder/) metodę, aby ustawić audio typ kodowania.

5. Wywołanie [SetOutputFile](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.SetOutputFile/p/System.String/) metodę, aby określić nazwę pliku wyjściowego, który są zapisywane dane audio.

6. Wywołanie [Prepare](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Prepare%28%29/) metodę, aby zainicjować rejestratora.

7. Wywołanie [Start](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Start%28%29/) metodę, aby rozpocząć nagrywanie.


Poniższy przykładowy kod przedstawia tej sekwencji:

```csharp
protected MediaRecorder recorder;
void RecordAudio (String filePath)
{
  try {
    if (File.Exists (filePath)) {
      File.Delete (filePath);
    }
    if (recorder == null) {
      recorder = new MediaRecorder (); // Initial state.
    } else {
      recorder.Reset ();
      recorder.SetAudioSource (AudioSource.Mic);
      recorder.SetOutputFormat (OutputFormat.ThreeGpp);
      recorder.SetAudioEncoder (AudioEncoder.AmrNb);
      // Initialized state.
      recorder.SetOutputFile (filePath);
      // DataSourceConfigured state.
      recorder.Prepare (); // Prepared state
      recorder.Start (); // Recording state.
    }
  } catch (Exception ex) {
    Console.Out.WriteLine( ex.StackTrace);
  }
}
```


### <a name="stopping-recording"></a>Zatrzymywanie rejestrowania

Aby zatrzymać rejestrowanie, należy wywołać `Stop` metoda `MediaRecorder`:

```csharp
recorder.Stop();
```



### <a name="cleaning-up"></a>Czyszczenie

Raz `MediaRecorder` została zatrzymana, wywołaj [zresetować](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Reset%28%29/) metody, aby ją z powrotem do stanu bezczynności:

```csharp
recorder.Reset();
```

Gdy `MediaRecorder` jest już potrzebne, jego zasoby muszą zostać zwolnione, wywołując [wersji](https://developer.xamarin.com/api/member/Android.Media.MediaRecorder.Release%28%29/) metody:

```csharp
recorder.Release();
```


## <a name="managing-audio-notifications"></a>Zarządzanie Audio powiadomienia



### <a name="the-audiomanager-class"></a>Klasa AudioManager

[AudioManager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/) klasy zapewnia dostęp do powiadomień audio, umożliwiających aplikacji wiedzieć, kiedy wystąpienia zdarzeń audio. Ta usługa również udostępnia inne funkcje audio, takie jak wolumin i dzwonka kontroli trybu. `AudioManager` Umożliwia aplikacji do obsługi audio powiadomienia do sterowania odtwarzania audio.



### <a name="managing-audio-focus"></a>Zarządzanie Audio fokus

Audio zasoby urządzenia (wbudowane player i rejestratora) są współużytkowane przez wszystkie uruchomione aplikacje.

Koncepcyjnie, jest podobna do aplikacji na komputerze stacjonarnym, gdzie tylko jedna aplikacja ma fokus klawiatury: po wybraniu jednej z aplikacji uruchomionej myszy kliknięciem, wejście klawiatury przechodzi tylko do tej aplikacji.

Audio fokus jest podobne i uniemożliwia więcej niż jedną aplikację z odtwarzanie lub rejestrowanie dźwięku w tym samym czasie. Jest bardziej skomplikowane niż fokus klawiatury, ponieważ jest dobrowolne &ndash; aplikacji można zignorować ten fakt, że nie jest aktualnie ma fokus audio i odtwarzać niezależnie od tego &ndash; i dlatego istnieją różne typy fokus audio, które mogą być żądanie. Na przykład jeśli obiekt żądający jest oczekiwane tylko do odtwarzania dźwięku bardzo krótki czas, może zażądać przejściowej fokus.

Fokus audio można udzielić natychmiast, lub początkowo odmowa i później przyznane. Na przykład jeśli aplikacja żądań audio fokus podczas rozmowy telefonicznej, będą odrzucane, ale fokus może również otrzymać po zakończeniu rozmowy telefonicznej. W takim przypadku odbiornik został zarejestrowany odpowiedzi w związku z tym jeśli odebrane audio fokus. Żąda fokus audio służy do sprawdzenia, czy jest OK, aby odtworzyć rejestrowanie dźwięku.

Aby uzyskać więcej informacji na temat fokus audio, zobacz [Zarządzanie Audio fokus](http://developer.android.com/training/managing-audio/audio-focus.html).



#### <a name="registering-the-callback-for-audio-focus"></a>Rejestrowanie wywołania zwrotnego dla zespołu Audio

Rejestrowanie `FocusChangeListener` wywołania zwrotnego z `IOnAudioChangeListener` jest ważnym elementem uzyskiwania i zwalniania audio fokus. Jest to spowodowane przyznawania audio fokus może zostać odroczona na później. Na przykład aplikacja może zażądać odtwarzać muzykę, gdy trwa połączenie telefoniczne. Fokus audio nie zostaną przyznane dopiero po zakończeniu rozmowy telefonicznej.

Z tego powodu obiektu wywołania zwrotnego jest przekazywana jako parametr do `GetAudioFocus` metody `AudioManager`, i jest to wywołanie, który rejestruje wywołanie zwrotne. Jeśli początkowo odmowa audio fokus, ale później przyznane, aplikacja zostanie poinformowany, wywołując `OnAudioFocusChange` w wywołaniu zwrotnym. Ta sama metoda jest używana aplikacji stwierdzić, że audio fokus jest przełączana do optymalizacji.

Po zakończeniu aplikacji przy użyciu zasobów audio wywołuje `AbandonFocus` metody `AudioManager`i ponownie przekazuje podczas wywołania zwrotnego. To wywołanie zwrotne deregisters i zwalnia zasoby audio, dzięki czemu inne aplikacje mogą uzyskać fokusu audio.



#### <a name="requesting-audio-focus"></a>Żąda Audio fokus

Poniżej przedstawiono kroki wymagane do zażądania audio zasoby urządzenia:

1.  Uzyskać dojścia do `AudioManager` usługi systemowej.

2.  Tworzenie wystąpienia klasy wywołania zwrotnego.

3.  Żądanie audio zasoby urządzenia przez wywołanie metody `RequestAudioFocus` metoda `AudioManager` . Parametry są obiektu wywołania zwrotnego, typ strumienia (muzyki, rozmowy, pierścień itp.) i typ żądanego dostępu bezpośrednio (audio zasobów może zażądać na chwilę lub nieokreślony, na przykład).

4.  W przypadku przyznania żądania `playMusic` metoda jest wywoływana natychmiast i rozpoczyna odtwarzanie dźwięku.

5.  Jeśli żądanie zostanie odrzucone, są wykonywane nie dalsze działania. W takim przypadku audio zostanie odtworzona tylko, jeśli otrzymuje żądanie w późniejszym czasie.


Poniższy przykładowy kod przedstawia następujące kroki:

```csharp
Boolean RequestAudioResources(INotificationReceiver parent)
{
  AudioManager audioMan = (AudioManager) GetSystemService(Context.AudioService);
  AudioManager.IOnAudioFocusChangeListener listener  = new MyAudioListener(this);
  var ret = audioMan.RequestAudioFocus (listener, Stream.Music, AudioFocus.Gain );
  if (ret == AudioFocusRequest.Granted) {
    playMusic();
    return (true);
  } else if (ret == AudioFocusRequest.Failed) {
    return (false);
  }
  return (false);
}
```


#### <a name="releasing-audio-focus"></a>Zwalnianie Audio fokus

Po zakończeniu odtwarzania ścieżki `AbandonFocus` metoda `AudioManager` jest wywoływana. Dzięki temu inna aplikacja uzyskanie audio zasoby urządzenia. Inne aplikacje otrzymają powiadomienie z informacją o tę zmianę fokusu audio zarejestrowanych własnych odbiorników.


## <a name="low-level-audio-api"></a>Niski poziom interfejsu API Audio

Niskiego poziomu audio interfejsów API zapewniają większą kontrolę nad odtwarzanie dźwięku i nagrywanie, ponieważ ich bezpośrednią interakcję z buforów pamięci zamiast pliku identyfikatorów URI. Istnieją sytuacje, w którym ta metoda jest preferowana. Takie scenariusze obejmują:

1.  Podczas odtwarzania z zaszyfrowanych plików audio.

2.  Podczas odtwarzania kolejne krótkich klipów.

3.  Przesyłanie strumieniowe audio.


### <a name="audiotrack-class"></a>Klasa AudioTrack

[AudioTrack](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/) klasa używa niskiego poziomu audio interfejsów API do rejestrowania i jest odpowiednikiem niskiego poziomu `MediaPlayer` klasy.


#### <a name="initializing-and-playing"></a>Inicjowanie i odtwarzania

Do odtwarzania audio, nowe wystąpienie klasy `AudioTrack` musi zostać utworzona. Lista argumentów przekazywane do [Konstruktor](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/#memberlist) określa sposób odtwarzania próbkowania zawarte w buforze. W argumentach są:

1.  Typ strumienia &ndash; głosowych, dzwonek, muzyka, system lub alarm.

2.  Częstotliwość &ndash; próbkowania wyrażone w Hz.

3.  Konfiguracja kanału &ndash; Mono lub stereo.

4.  Audio format &ndash; 8-bitową lub 16-bitowego kodowania.

5.  Rozmiar buforu &ndash; w bajtach.

6.  Tryb buforowania &ndash; przesyłania strumieniowego lub statycznej.


Po wykonaniu konstrukcji [odtwarzanie](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Play%28%29/) metoda `AudioTrack` jest wywoływana, aby ustawić ją do rozpoczęcia odtwarzania. Pisanie audio bufor do `AudioTrack` rozpoczyna odtwarzanie:

```csharp
void PlayAudioTrack(byte[] audioBuffer)
{
  AudioTrack audioTrack = new AudioTrack(
    // Stream type
    Stream.Music,
    // Frequency
    11025,
    // Mono or stereo
    ChannelOut.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length,
    // Mode. Stream or static.
    AudioTrackMode.Stream);

    audioTrack.Play();
    audioTrack.Write(audioBuffer, 0, audioBuffer.Length);
}
```


#### <a name="pausing-and-stopping-the-playback"></a>Wstrzymywanie i zatrzymywanie odtwarzania

Wywołanie [wstrzymać](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Pause%28%29/) metodę, aby wstrzymać odtwarzanie:

```csharp
audioTrack.Pause();
```

Wywoływanie [zatrzymać](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Stop%28%29/) metody spowoduje przerwanie odtwarzania stałe:

```csharp
audioTrack.Stop();
```


#### <a name="cleanup"></a>Oczyszczanie

Gdy `AudioTrack` jest już potrzebne, jego zasoby muszą zostać zwolnione, wywołując [wersji](https://developer.xamarin.com/api/member/Android.Media.AudioTrack.Release%28%29/):

```csharp
audioTrack.Release();
```


### <a name="the-audiorecord-class"></a>Klasa AudioRecord

[AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) klasy jest odpowiednikiem `AudioTrack` po stronie rejestrowania. Podobnie jak `AudioTrack`, bezpośrednio, używa buforów pamięci zamiast plików i identyfikatorów URI. Wymaga, aby `RECORD_AUDIO` uprawnienia można ustawić w manifeście.


#### <a name="initializing-and-recording"></a>Inicjowanie i rejestrowanie

Pierwszym krokiem jest do utworzenia nowego [AudioRecord](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/) obiektu. Lista argumentów przekazywane do [Konstruktor](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/#memberlist) zawiera wszystkie informacje wymagane do rejestrowania. W odróżnieniu od w `AudioTrack`, gdzie argumentami są przede wszystkim wyliczenia, równoważnymi argumentami w `AudioRecord` są liczbami całkowitymi. Należą do nich następujące elementy:

1.  Sprzęt audio źródło danych wejściowych takie jak mikrofon.

2.  Typ strumienia &ndash; głosowych, dzwonek, muzyka, system lub alarm.

3.  Częstotliwość &ndash; próbkowania wyrażone w Hz.

4.  Konfiguracja kanału &ndash; Mono lub stereo.

5.  Audio format &ndash; 8-bitową lub 16-bitowego kodowania.

6.  Bufor rozmiar w bajtach


Raz `AudioRecord` jest tworzony, jego [StartRecording](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.StartRecording%28%29/) wywołania metody. Jest teraz gotowy do rozpoczęcia rejestrowania. `AudioRecord` Stale audio buforu dla danych wejściowych odczytuje i zapisuje wejściowego się do pliku dźwiękowego.

```csharp
void RecordAudio()
{
  byte[] audioBuffer = new byte[100000];
  var audRecorder = new AudioRecord(
    // Hardware source of recording.
    AudioSource.Mic,
    // Frequency
    11025,
    // Mono or stereo
    ChannelIn.Mono,
    // Audio encoding
    Android.Media.Encoding.Pcm16bit,
    // Length of the audio clip.
    audioBuffer.Length
  );
  audRecorder.StartRecording();
  while (true) {
    try
    {
      // Keep reading the buffer while there is audio input.
      audRecorder.Read(audioBuffer, 0, audioBuffer.Length);
      // Write out the audio file.
    } catch (Exception ex) {
      Console.Out.WriteLine(ex.Message);
      break;
    }
  }
}
```


#### <a name="stopping-the-recording"></a>Zatrzymywanie rejestrowania

Wywoływanie [zatrzymać](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Stop%28%29/) metoda kończy rejestrowania:

```csharp
audRecorder.Stop();
```


#### <a name="cleanup"></a>Oczyszczanie

Gdy `AudioRecord` obiektu nie jest już potrzebne, wywoływanie jej [wersji](https://developer.xamarin.com/api/member/Android.Media.AudioRecord.Release%28%29/) metoda zwalnia wszystkie zasoby skojarzone z nią:

```csharp
audRecorder.Release();
```


## <a name="summary"></a>Podsumowanie

System operacyjny Android udostępnia zaawansowane framework odtwarzanie, rejestrowanie i zarządzanie audio. W tym artykule objętych odtwarzanie i rejestrowanie dźwięku za pomocą wysokiego poziomu `MediaPlayer` i `MediaRecorder` klasy. Przedstawione go dalej, jak używać powiadomień audio udostępnianie audio zasobów między różnymi aplikacjami urządzenia. Na koniec go traktowane jak odtwarzanie i rejestrowanie dźwięku z użyciem interfejsów API usługi niskiego poziomu, które interfejs bezpośrednio z buforów pamięci.


## <a name="related-links"></a>Linki pokrewne

- [Praca z Audio (przykład)](https://developer.xamarin.com/samples/Example_WorkingWithAudio/)
- [Media Player](https://developer.xamarin.com/api/type/Android.Media.MediaPlayer/)
- [Media Recorder](https://developer.xamarin.com/api/type/Android.Media.MediaRecorder/)
- [Audio Manager](https://developer.xamarin.com/api/type/Android.Media.AudioManager/)
- [Ścieżka audio](https://developer.xamarin.com/api/type/Android.Media.AudioTrack/)
- [Audio Recorder](https://developer.xamarin.com/api/type/Android.Media.AudioRecord/)
