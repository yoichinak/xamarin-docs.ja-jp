---
title: Xamarin.Android フラグメント チュートリアルのパート 1
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
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798487"
---
# <a name="fragments-walkthrough-ndash-phone"></a>チュートリアルをフラグメント化&ndash;電話]

これは、縦向きの Android デバイスを対象とする Xamarin.Android アプリを作成するチュートリアルの最初の部分です。 このチュートリアルは Xamarin.Android でフラグメントを作成する方法とサンプルを追加する方法について説明します。

[![](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

このアプリについて、次のクラスが作成されます。

1. `PlayQuoteFragment` &nbsp; このフラグメントいえいえ引用形再生が表示されます。 によってホストされている`PlayQuoteActivity`です。
1. `Shakespeare` &nbsp; このクラスは、プロパティとして 2 つのハードコードされた配列を保持します。
1. `TitlesFragment` &nbsp; このフラグメントいえいえ記述されている再生のタイトルの一覧が表示されます。 によってホストされている`MainActivity`です。
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` 起動、`PlayQuoteActivity`で再生を選択すると、ユーザーへの応答`TitlesFragment`です。

## <a name="1-create-the-android-project"></a>1.Android プロジェクトを作成します。

いう新しい Xamarin.Android プロジェクトの作成**FragmentSample**です。
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![新しい Xamarin.Android プロジェクトを作成します。](./walkthrough-images/01-newproject.w157-sml.png)](./walkthrough-images/01-newproject.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![新しい Xamarin.Android プロジェクトの作成](./walkthrough-images/01-newproject.m742-sml.png)](./walkthrough-images/01-newproject.m742.png#lightbox)

選択することをお勧め**最新の開発**このチュートリアルでします。

プロジェクトを作成した後、ファイルの名前を変更**layout/Main.axml**に**layout/activity_main.axml**です。

-----

## <a name="2-add-the-data"></a>2.データを追加します。

このアプリケーションのデータをクラス名のプロパティでは 2 つのハードコードされた文字列配列に格納する`Shakespeare`:

* `Shakespeare.Titles` &nbsp; この配列では、再生のいえいえの一覧を保持します。 これは、データ ソースを`TitlesFragment`です。
* `Shakespeare.Dialogue` &nbsp; この配列に含まれているアプローチのいずれかの引用符の一覧を保持する`Shakespeare.Titles`です。 これは、データ ソースを`PlayQuoteFragment`です。

新しい c# クラスを追加、 **FragmentSample**プロジェクトおよび名前を付けます**Shakespeare.cs**です。 このファイル内で、新しいクラスを作成 (C#) と呼ばれる`Shakespeare`で、次の内容

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

## <a name="3-create-the-playquotefragment"></a>3.PlayQuoteFragment を作成します。

`PlayQuoteFragment`アプリケーション内で事前にユーザーによって選択されたシェイクスピアー プレイの見積もりを表示する Android フラグメントは、このフラグメントでは、Android のレイアウト ファイルは使用しません。 代わりに、動的に作成されます、ユーザー インターフェイス。 新しい`Fragment`という名前のクラス`PlayQuoteFragment`をプロジェクトに。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![新しい c# クラスを追加します。](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/02-addclass.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![新しい c# クラスを追加します。](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/02-addclass.m742.png#lightbox)

-----

このスニペットと同じように、フラグメントのコードを次に、変更するには。

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

フラグメントのインスタンスを作成するファクトリ メソッドを提供する Android アプリで一般的なパターンです。 これにより、フラグメントを適切に機能するために必要なパラメーターを作成することです。 このチュートリアルで使用するアプリが必要、`PlayQuoteFragment.NewInstance`メソッドを引用符を選択するたびに、新しいフラグメントを作成します。 `NewInstance`メソッドは、単一のパラメーターを受け取る&ndash;を表示する引用符のインデックス。

`OnCreateView`際に、画面上のフラグメントを表示するために Android でメソッドが呼び出されます。 Android が返されます`View`フラグメントであるオブジェクト。 このフラグメントでは、ビューを作成するのにレイアウト ファイルを使用しません。 代わりに、プログラムでビューが作成、インスタンス化して、 **TextView** 、引用符を保持するためでそのウィジェットが表示されます、 **ScrollView**です。

> [!NOTE]
> フラグメント サブクラスは、パラメーターを持たずパブリックの既定コンス トラクターを持つ必要があります。

## <a name="4-create-the-playquoteactivity"></a>4.PlayQuoteActivity を作成します。

フラグメントのでこのアプリをホストするアクティビティが必要です、アクティビティ内でホストする必要があります、`PlayQuoteFragment`です。 アクティビティは、実行時にそのレイアウトにフラグメントを動的に追加されます。 アプリケーションに新しいアクティビティを追加し、名前`PlayQuoteActivity`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android のアクティビティをプロジェクトに追加します。](./walkthrough-images/03-addactivity.w157-sml.png)](./walkthrough-images/03-addactivity.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android のアクティビティをプロジェクトに追加します。](./walkthrough-images/03-addactivity.m742-sml.png)](./walkthrough-images/03-addactivity.m742.png#lightbox)

-----

コードを編集`PlayQuoteActivity`:

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

ときに`PlayQuoteActivity`が作成されると、それがインスタンス化され、新しい`PlayQuoteFragment`のコンテキストでは、そのルート ビューでは、そのフラグメントを読み込むと、`FragmentTransaction`です。 このアクティビティがユーザー インターフェイスに、Android のレイアウト ファイルを読み込めなかったことに注意してください。 代わりに、新しい`PlayQuoteFragment`アプリケーションのルート ビューに追加されます。 リソース識別子`Android.Resource.Id.Content`特定の識別子がわからなくても、アクティビティのルート ビューを参照するために使用します。

## <a name="5-create-titlesfragment"></a>5.TitlesFragment を作成します。

`TitlesFragment`と呼ばれる特殊なフラグメントをサブクラスには、`ListFragment`を表示するためのロジックをカプセル化、`ListView`フラグメント。 A`ListFragment`公開、`ListAdapter`プロパティ (によって使用される、`ListView`内容を表示する) と名前付きイベント ハンドラー`OnListItemClick`によって表示される行をクリックに応答するフラグメントこれにより、`ListView`です。 

最初に、新しいフラグメントをプロジェクトに追加し、名前**TitlesFragment**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android の追加をプロジェクトにフラグメント](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/04-addfragment.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Android の追加をプロジェクトにフラグメント](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/04-addfragment.m742.png#lightbox)

-----

フラグメント内のコードを編集します。

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

Android を呼び出すアクティビティが作成されたときに、 `OnActivityCreated` ; フラグメントのメソッドここには用のアダプターを一覧、`ListView`を作成します。  `ShowQuoteFromPlay`メソッドのインスタンスが開始されます、`PlayQuoteActivity`選択されている再生の見積もりを表示します。

## <a name="display-titlesfragment-in-mainactivity"></a>MainActivity に TitlesFragment を表示します。

最後の手順が表示するには`TitlesFragment`内`MainActivity`です。 アクティビティは、フラグメントを動的に読み込まれません。 代わりにフラグメントは、静的に読み込まれるアクティビティを使用して、レイアウト ファイルで宣言することによって、`fragment`要素。 読み込むフラグメントは、設定によって識別される、`android:name`属性をフラグメント クラス (includeing 型の名前空間) にします。 例については、使用する、 `TitlesFragment`、し`android:name`に設定すると`FragmentSample.TitlesFragment`です。

レイアウト ファイルを編集して**activity_mail.axml**、既存の XML を次に置き換えます。

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
> `class`属性の有効な代替手段は、`android:name`です。 正式なガイダンスはありませんが使用するコード ベースの例の多くがあるフォームは優先を`class`と同じ意味で`android:name`です。

MainActivity に必要なコード変更はありません。 そのクラスのコードは、このスニペットに類似する必要があります。

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

## <a name="run-the-app"></a>アプリを実行する

現在表示されているコードは、完了すると、動作を確認するデバイスでアプリを実行します。

[![スマート フォンで実行されているアプリケーションのスクリーン ショット。](./walkthrough-images/05-app-screenshots-sml.png)](./walkthrough-images/05-app-screenshots.png#lightbox)

[このチュートリアルの第 2 部](./walkthrough-landscape.md)optimtize を横モードで実行しているデバイス用にこのアプリケーションになります。