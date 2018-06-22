---
title: Wskazówki fragmenty Xamarin.Android — część 1
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 4dae59d113671c9ba1ac35dd8e4189d05a7c319a
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798489"
---
# <a name="fragments-walkthrough-ndash-phone"></a>Fragmenty wskazówki &ndash; phone]

Jest to pierwsza część wskazówki, które spowoduje utworzenie aplikacji platformy Xamarin.Android przeznaczonego dla urządzenia z systemem Android w orientacji pionowej. Tego przewodnika Przedstawimy sposób tworzenia fragmentów w Xamarin.Android i sposobu dodawania ich do próbki.

[![](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Następujące klasy zostaną utworzone dla tej aplikacji:

1. `PlayQuoteFragment` &nbsp; Ten fragment wyświetli ofertę z Odtwórz przez Szekspir łączy. Będzie obsługiwana przez `PlayQuoteActivity`.
1. `Shakespeare` &nbsp; Ta klasa będą przechowywane dwie tablice zapisane na stałe jako właściwości.
1. `TitlesFragment` &nbsp; Ten fragment wyświetli listę tytułów odtwarzania, które zostały napisane przez Szekspir łączy. Będzie obsługiwana przez `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` rozpocznie się `PlayQuoteActivity` w odpowiedzi na użytkownika, wybierając Odtwarzaj na `TitlesFragment`.

## <a name="1-create-the-android-project"></a>1. Tworzenie projektu dla systemu Android

Utwórz nowy projekt platformy Xamarin.Android o nazwie **FragmentSample**.
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Utwórz nowy projekt platformy Xamarin.Android](./walkthrough-images/01-newproject.w157-sml.png)](./walkthrough-images/01-newproject.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Tworzenie nowego projektu platformy Xamarin.Android](./walkthrough-images/01-newproject.m742-sml.png)](./walkthrough-images/01-newproject.m742.png#lightbox)

Zaleca się wybierz **nowoczesnych programowanie** w ramach tego przewodnika.

Po utworzeniu projektu, Zmień nazwę pliku **layout/Main.axml** do **layout/activity_main.axml**.

-----

## <a name="2-add-the-data"></a>2. Dodawanie danych

Dane dla tej aplikacji będą przechowywane w dwóch tablic ciągów zapisane na stałe będących właściwościami Nazwa klasy `Shakespeare`:

* `Shakespeare.Titles` &nbsp; Ta tablica będą przechowywane listy odtwarzania z Szekspir łączy. To źródło danych dla `TitlesFragment`.
* `Shakespeare.Dialogue` &nbsp; Ta tablica będą przechowywane oferty z jednego z pełni zawarty w `Shakespeare.Titles`. To źródło danych dla `PlayQuoteFragment`.

Dodaj nową klasę C# do **FragmentSample** projektu i nadaj mu nazwę **Shakespeare.cs**. Ten plik, Utwórz nowe klasy C# o nazwie `Shakespeare` z następującą zawartość

```csharp
class Shakespeare
{
    public static string[] Titles = {
                                      "Henry IV (1)",
                                      "Henry V",
                                      "Henry VIII",
                                      "Richard II",
                                      "Richard III",
                                      "Merchant of Venice",
                                      "Othello",
                                      "King Lear"
                                    };

    public static string[] Dialogue = {
                                        "So shaken as we are, so wan with care, Find we a time for frighted peace to pant, And breathe short-winded accents of new broils To be commenced in strands afar remote. No more the thirsty entrance of this soil Shall daub her lips with her own children's blood; Nor more shall trenching war channel her fields, Nor bruise her flowerets with the armed hoofs Of hostile paces: those opposed eyes, Which, like the meteors of a troubled heaven, All of one nature, of one substance bred, Did lately meet in the intestine shock And furious close of civil butchery Shall now, in mutual well-beseeming ranks, March all one way and be no more opposed Against acquaintance, kindred and allies: The edge of war, like an ill-sheathed knife, No more shall cut his master. Therefore, friends, As far as to the sepulchre of Christ, Whose soldier now, under whose blessed cross We are impressed and engaged to fight, Forthwith a power of English shall we levy; Whose arms were moulded in their mothers' womb To chase these pagans in those holy fields Over whose acres walk'd those blessed feet Which fourteen hundred years ago were nail'd For our advantage on the bitter cross. But this our purpose now is twelve month old, And bootless 'tis to tell you we will go: Therefore we meet not now. Then let me hear Of you, my gentle cousin Westmoreland, What yesternight our council did decree In forwarding this dear expedience.",
                                        "Hear him but reason in divinity, And all-admiring with an inward wish You would desire the king were made a prelate: Hear him debate of commonwealth affairs, You would say it hath been all in all his study: List his discourse of war, and you shall hear A fearful battle render'd you in music: Turn him to any cause of policy, The Gordian knot of it he will unloose, Familiar as his garter: that, when he speaks, The air, a charter'd libertine, is still, And the mute wonder lurketh in men's ears, To steal his sweet and honey'd sentences; So that the art and practic part of life Must be the mistress to this theoric: Which is a wonder how his grace should glean it, Since his addiction was to courses vain, His companies unletter'd, rude and shallow, His hours fill'd up with riots, banquets, sports, And never noted in him any study, Any retirement, any sequestration From open haunts and popularity.",
                                        "I come no more to make you laugh: things now, That bear a weighty and a serious brow, Sad, high, and working, full of state and woe, Such noble scenes as draw the eye to flow, We now present. Those that can pity, here May, if they think it well, let fall a tear; The subject will deserve it. Such as give Their money out of hope they may believe, May here find truth too. Those that come to see Only a show or two, and so agree The play may pass, if they be still and willing, I'll undertake may see away their shilling Richly in two short hours. Only they That come to hear a merry bawdy play, A noise of targets, or to see a fellow In a long motley coat guarded with yellow, Will be deceived; for, gentle hearers, know, To rank our chosen truth with such a show As fool and fight is, beside forfeiting Our own brains, and the opinion that we bring, To make that only true we now intend, Will leave us never an understanding friend. Therefore, for goodness' sake, and as you are known The first and happiest hearers of the town, Be sad, as we would make ye: think ye see The very persons of our noble story As they were living; think you see them great, And follow'd with the general throng and sweat Of thousand friends; then in a moment, see How soon this mightiness meets misery: And, if you can be merry then, I'll say A man may weep upon his wedding-day.",
                                        "First, heaven be the record to my speech! In the devotion of a subject's love, Tendering the precious safety of my prince, And free from other misbegotten hate, Come I appellant to this princely presence. Now, Thomas Mowbray, do I turn to thee, And mark my greeting well; for what I speak My body shall make good upon this earth, Or my divine soul answer it in heaven. Thou art a traitor and a miscreant, Too good to be so and too bad to live, Since the more fair and crystal is the sky, The uglier seem the clouds that in it fly. Once more, the more to aggravate the note, With a foul traitor's name stuff I thy throat; And wish, so please my sovereign, ere I move, What my tongue speaks my right drawn sword may prove.",
                                        "Now is the winter of our discontent Made glorious summer by this sun of York; And all the clouds that lour'd upon our house In the deep bosom of the ocean buried. Now are our brows bound with victorious wreaths; Our bruised arms hung up for monuments; Our stern alarums changed to merry meetings, Our dreadful marches to delightful measures. Grim-visaged war hath smooth'd his wrinkled front; And now, instead of mounting barded steeds To fright the souls of fearful adversaries, He capers nimbly in a lady's chamber To the lascivious pleasing of a lute. But I, that am not shaped for sportive tricks, Nor made to court an amorous looking-glass; I, that am rudely stamp'd, and want love's majesty To strut before a wanton ambling nymph; I, that am curtail'd of this fair proportion, Cheated of feature by dissembling nature, Deformed, unfinish'd, sent before my time Into this breathing world, scarce half made up, And that so lamely and unfashionable That dogs bark at me as I halt by them; Why, I, in this weak piping time of peace, Have no delight to pass away the time, Unless to spy my shadow in the sun And descant on mine own deformity: And therefore, since I cannot prove a lover, To entertain these fair well-spoken days, I am determined to prove a villain And hate the idle pleasures of these days. Plots have I laid, inductions dangerous, By drunken prophecies, libels and dreams, To set my brother Clarence and the king In deadly hate the one against the other: And if King Edward be as true and just As I am subtle, false and treacherous, This day should Clarence closely be mew'd up, About a prophecy, which says that 'G' Of Edward's heirs the murderer shall be. Dive, thoughts, down to my soul: here Clarence comes.",
                                        "To bait fish withal: if it will feed nothing else, it will feed my revenge. He hath disgraced me, and hindered me half a million; laughed at my losses, mocked at my gains, scorned my nation, thwarted my bargains, cooled my friends, heated mine enemies; and what's his reason? I am a Jew. Hath not a Jew eyes? hath not a Jew hands, organs, dimensions, senses, affections, passions? fed with the same food, hurt with the same weapons, subject to the same diseases, healed by the same means, warmed and cooled by the same winter and summer, as a Christian is? If you prick us, do we not bleed? if you tickle us, do we not laugh? if you poison us, do we not die? and if you wrong us, shall we not revenge? If we are like you in the rest, we will resemble you in that. If a Jew wrong a Christian, what is his humility? Revenge. If a Christian wrong a Jew, what should his sufferance be by Christian example? Why, revenge. The villany you teach me, I will execute, and it shall go hard but I will better the instruction.",
                                        "Virtue! a fig! 'tis in ourselves that we are thus or thus. Our bodies are our gardens, to the which our wills are gardeners: so that if we will plant nettles, or sow lettuce, set hyssop and weed up thyme, supply it with one gender of herbs, or distract it with many, either to have it sterile with idleness, or manured with industry, why, the power and corrigible authority of this lies in our wills. If the balance of our lives had not one scale of reason to poise another of sensuality, the blood and baseness of our natures would conduct us to most preposterous conclusions: but we have reason to cool our raging motions, our carnal stings, our unbitted lusts, whereof I take this that you call love to be a sect or scion.",
                                        "Blow, winds, and crack your cheeks! rage! blow! You cataracts and hurricanoes, spout Till you have drench'd our steeples, drown'd the cocks! You sulphurous and thought-executing fires, Vaunt-couriers to oak-cleaving thunderbolts, Singe my white head! And thou, all-shaking thunder, Smite flat the thick rotundity o' the world! Crack nature's moulds, an germens spill at once, That make ingrateful man!" 
                                    };
}
```

## <a name="3-create-the-playquotefragment"></a>3. Utwórz PlayQuoteFragment

`PlayQuoteFragment` Jest fragmentu Android wyświetlające cudzysłowu play Szekspir, który został wybrany przez użytkownika na wcześniej w tej aplikacji, ten fragment nie będzie używać pliku układu Android; zamiast tego zostanie utworzony dynamicznie interfejs użytkownika. Dodaj nową `Fragment` klasy o nazwie `PlayQuoteFragment` do projektu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dodaj nową klasę C#](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/02-addclass.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dodaj nową klasę C#](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/02-addclass.m742.png#lightbox)

-----

Następnie należy zmienić kod dla fragmentu przypominały ta Wstawka kodu:

```csharp
public class PlayQuoteFragment : Fragment
{
    public int PlayId => Arguments.GetInt("current_play_id", 0);

    public static PlayQuoteFragment NewInstance(int playId)
    {
        var bundle = new Bundle();
        bundle.PutInt("current_play_id", playId);
        return new PlayQuoteFragment {Arguments = bundle};
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        if (container == null)
        {
            return null;
        }

        var textView = new TextView(Activity);
        var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
        textView.SetPadding(padding, padding, padding, padding);
        textView.TextSize = 24;
        textView.Text = Shakespeare.Dialogue[PlayId];

        var scroller = new ScrollView(Activity);
        scroller.AddView(textView);

        return scroller;
    }
}
```

Jest wspólnego wzorca w aplikacjach systemu Android, aby podać metodę fabryka, która zostanie utworzenie wystąpienia fragmentu. Dzięki temu, że fragment zostanie utworzony z wymaganych parametrów dla prawidłowego działania. W tym przewodniku, powinien użyć aplikacji `PlayQuoteFragment.NewInstance` metodę w celu utworzenia nowego fragmentu zawsze oferty jest zaznaczone. `NewInstance` Metoda będzie przyjmować jeden parametr &ndash; indeks oferty do wyświetlenia.

`OnCreateView` — Metoda zostanie wywołany przez system Android, gdy nadejdzie czas na renderowanie fragmentu na ekranie. Zwróci Android `View` obiektu, który fragment. Aby utworzyć widok ten fragment nie używa pliku układu. Zamiast tego należy programowo zostanie utworzony widok przez utworzenie wystąpienia **TextView** do przechowywania oferty i wyświetli tego elementu widget w **ScrollView**.

> [!NOTE]
> Podklasy fragmentu musi mieć domyślnego konstruktora publicznego, który nie ma parametrów.

## <a name="4-create-the-playquoteactivity"></a>4. Utwórz PlayQuoteActivity

Fragmenty musi być hostowany w działaniu, dlatego ta aplikacja wymaga to działanie, które będą obsługiwać `PlayQuoteFragment`. Działanie zostanie dynamicznie dodać fragment układ w czasie wykonywania. Dodaj nowe działanie aplikacji i nadaj mu nazwę `PlayQuoteActivity`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dodaj działanie systemu Android do projektu](./walkthrough-images/03-addactivity.w157-sml.png)](./walkthrough-images/03-addactivity.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dodaj działanie systemu Android do projektu](./walkthrough-images/03-addactivity.m742-sml.png)](./walkthrough-images/03-addactivity.m742.png#lightbox)

-----

Edytowanie kodu `PlayQuoteActivity`:

```csharp
[Activity(Label = "PlayQuoteActivity")]
public class PlayQuoteActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        var playId = Intent.Extras.GetInt("current_play_id", 0);

        var detailsFrag = PlayQuoteFragment.NewInstance(playId);
        FragmentManager.BeginTransaction()
                        .Add(Android.Resource.Id.Content, detailsFrag)
                        .Commit();
    }
}
```

Gdy `PlayQuoteActivity` jest utworzony, będzie wystąpienia nowy `PlayQuoteFragment` i załadować tego fragmentu w widoku głównego w kontekście `FragmentTransaction`. Zwróć uwagę, że to działanie nie załadować pliku układu dla systemu Android, interfejs użytkownika. Zamiast tego nowy `PlayQuoteFragment` zostanie dodany do widoku głównego aplikacji. Identyfikator zasobu `Android.Resource.Id.Content` służy do odwoływania się do widoku głównego działania bez uprzedniego uzyskania informacji o jego określonego identyfikatora.

## <a name="5-create-titlesfragment"></a>5. Utwórz TitlesFragment

`TitlesFragment` Będzie podklasy fragmentu specjalne znany jako `ListFragment` która hermetyzuje logikę wyświetlanie `ListView` w fragmentu. A `ListFragment` przedstawia `ListAdapter` właściwości (używane przez `ListView` Aby wyświetlić jego zawartość) i program obsługi zdarzeń o nazwie `OnListItemClick` umożliwiającą fragment do odpowiadanie na kliknięcia w wierszu, który jest wyświetlany za `ListView`. 

Aby rozpocząć, Dodaj nowy fragment do projektu i nadaj jej nazwę **TitlesFragment**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dodaj Android fragment do projektu](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/04-addfragment.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dodaj Android fragment do projektu](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/04-addfragment.m742.png#lightbox)

-----

Edytuj kod wewnątrz fragment:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;

    public TitlesFragment()
    {
        // Being explicit about the requirement for a default constructor.
    }

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Po utworzeniu działania wywoła Android `OnActivityCreated` metody fragmentu; jest to, gdy karta listy `ListView` jest tworzony.  `ShowQuoteFromPlay` Metody zostanie uruchomione wystąpienie `PlayQuoteActivity` do wyświetlenia oferty dla wybranego play.

## <a name="display-titlesfragment-in-mainactivity"></a>Wyświetlanie TitlesFragment w MainActivity

Ostatnim krokiem jest do wyświetlenia `TitlesFragment` w `MainActivity`. Działanie nie dynamicznie załadować fragmentu. Zamiast tego fragment będzie statycznie ładowany przez zadeklarowanie go w pliku układu działania przy użyciu `fragment` elementu. Fragment załadować jest identyfikowany przez ustawienie `android:name` atrybutu klasy fragmentu (includeing obszar nazw tego typu). Na przykład, aby użyć `TitlesFragment`, następnie `android:name` będzie miał ustawienie `FragmentSample.TitlesFragment`.

Przeprowadź edycję pliku układu **activity_mail.axml**, zastępowanie istniejącego pliku XML z następujących czynności:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment
        android:name="FragmentSample.TitleFragment"
        android:id="@+id/titles"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

> [!NOTE]
> `class` Atrybut jest prawidłowy zastępuje `android:name`. Brak ma formalnych wskazówek na postać, która jest preferowana, istnieje wiele przykładów kodu zasad, które będą korzystać z `class` zamiennie z `android:name`.

Nie wprowadzono żadnych zmian kodu wymaganego dla MainActivity. Kod w tej klasie powinna być bardzo podobna do ta Wstawka kodu:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        SetContentView(Resource.Layout.activity_main);
    }
}
```

## <a name="run-the-app"></a>Uruchamianie aplikacji

Teraz, że kod jest zakończone, uruchom aplikację na urządzeniu w celu sprawdzenie w działaniu.

[![Zrzuty ekranu aplikacji uruchamianie na telefonie.](./walkthrough-images/05-app-screenshots-sml.png)](./walkthrough-images/05-app-screenshots.png#lightbox)

[Część 2 tego przewodnika](./walkthrough-landscape.md) będzie optimtize tej aplikacji na urządzeniach z systemem w trybie krajobraz.