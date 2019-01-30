---
title: Xamarin.iOS で自然言語フレームワークを使用します。
description: このドキュメントでは、自然言語、フレームワークについて説明します。 IOS 12 で導入された、自然言語、フレームワークは、言語認識、品詞の識別情報、および名前付きエンティティ認識を使用する推奨される iOS API です。
ms.prod: xamarin
ms.assetid: 126C8764-F873-4EB9-98A3-D82AB5689111
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/20/2018
ms.openlocfilehash: 0b3fb7d467ae64e2cbfdb61644b1537bc5ae1161
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233069"
---
# <a name="using-the-natural-language-framework-with-xamarinios"></a>Xamarin.iOS で自然言語フレームワークを使用します。

IOS 12 で導入された、自然言語フレームワークにより、デバイスで自然言語処理です。 言語の認識、トークン化、およびタグ付けをサポートします。 トークン化のテキストにそのコンポーネントの単語、文章、または段落の分割します。タグ付けの品詞、人物、場所、および組織を識別します。

自然言語、フレームワークを分類し、特殊なコンテキストでのテキストをタグ付けもカスタムの Core ML モデルを使用できます。

[NSLinguisticTagger](xref:Foundation.NSLinguisticTagger)クラスは引き続き使用できます。 ただし、自然言語、フレームワークは、自然言語処理に使用する推奨されるメカニズムです。

## <a name="sample-app-xamarinnl"></a>サンプル アプリ:XamarinNL

Xamarin.iOS で自然言語、フレームワークを使用する方法についてを参照してください、 [XamarinNL サンプル アプリ](https://developer.xamarin.com/samples/monotouch/iOS12/XamarinNL)します。
このサンプル アプリでは、自然言語フレームワークを使用する方法を示します。

- [言語の認識](#recognizing-languages)します。
- [単語や文にテキストをトークン化](#tokenizing-text-into-words-sentences-and-paragraphs)します。
- [という名前のエンティティとの品詞のタグ](#tagging-named-entities-and-parts-of-speech)します。

## <a name="recognizing-languages"></a>言語の認識

**レコグナイザー**サンプル アプリのタブを使用する方法を示します、 [`NLLanguageRecognizer`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguageRecognizer/)
テキストのブロックの言語を確認します。

> [!NOTE]
> 言語の認識は、特定の種類のテキストの分類です。 自然言語、フレームワークには、開発者が提供の Core ML モデルを使用してカスタム テキスト分類もサポートしています。 詳細についてを参照してください、[自然言語フレームワークの導入](https://developer.apple.com/videos/play/wwdc2018/713/)WWDC 2018 からセッション。

### <a name="dominant-language"></a>主要な言語

タップして、**言語**ユーザー入力の主要な言語を識別するためにボタンをクリックします。

`HandleDetermineLanguageButtonTap`のメソッド、`LanguageRecognizerViewController`を使用して、 [`GetDominantLanguage`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.GetDominantLanguage/)
メソッド、`NLLanguageRecognizer`をフェッチします [`NLLanguage`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguage/)
テキストの第一言語。

```csharp
partial void HandleDetermineLanguageButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        NLLanguage lang = NLLanguageRecognizer.GetDominantLanguage(UserInput.Text);
        DominantLanguageLabel.Text = lang.ToString();
    }
}
```

### <a name="language-probabilities"></a>言語の確率

タップして、**言語確率**ユーザーの入力言語仮説の一覧を取得するボタンをクリックします。

`HandleLanguageProbabilitiesButtonTap`のメソッド、`LanguageRecognizerViewController`クラスをインスタンス化、`NLLanguageRecognizer`することを確認します。 [`Process`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.Process/)
ユーザーのテキスト。 呼び出して言語認識エンジン [`GetNativeLanguageHypotheses`](https://developer.xamarin.com/api/member/NaturalLanguage.NLLanguageRecognizer.GetNativeLanguageHypotheses)
メソッドは、言語と関連付けられている確率のディクショナリを取得します。 `LanguageRecognizerTableViewController`クラスは、これらの言語と確率にし、表示します。

```csharp
partial void HandleLanguageProbabilitiesButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var recognizer = new NLLanguageRecognizer();
        recognizer.Process(UserInput.Text);
        NSDictionary<NSString, NSNumber> probabilities = recognizer.GetNativeLanguageHypotheses(10);
        PerformSegue(ShowLanguageProbabilitiesSegue, this);
    }
}
```

潜在的な`NLLanguage`値が含まれます。

- `Amharic`
- `Arabic`
- `Armenian`
- `Bengali`
- `Bulgarian`
- `Burmese`
- `Catalan`
- `Cherokee`
- `Croatian`
- `Czech`
- `Danish`
- `Dutch`
- `English`
- `Finnish`
- `French`
- `Georgian`
- `German`
- `Greek`
- `Gujarati`
- `Hebrew`
- `Hindi`
- `Hungarian`
- `Icelandic`
- `Indonesian`
- `Italian`
- `Japanese`
- `Kannada`
- `Khmer`
- `Korean`
- `Lao`
- `Malay`
- `Malayalam`
- `Marathi`
- `Mongolian`
- `Norwegian`
- `Oriya`
- `Persian`
- `Polish`
- `Portuguese`
- `Punjabi`
- `Romanian`
- `Russian`
- `SimplifiedChinese`
- `Sinhalese`
- `Slovak`
- `Spanish`
- `Swedish`
- `Tamil`
- `Telugu`
- `Thai`
- `Tibetan`
- `TraditionalChinese`
- `Turkish`
- `Ukrainian`
- `Undetermined`
- `Urdu`
- `Vietnamese`

サポートされる言語の完全な一覧については、としての一部で、 [`NLLanguage`](https://developer.xamarin.com/api/type/NaturalLanguage.NLLanguage/)
列挙型の API のドキュメントです。

## <a name="tokenizing-text-into-words-sentences-and-paragraphs"></a>単語や文、段落にテキストをトークン化

**トークナイザ**サンプル アプリのタブのテキストのブロックをそのコンポーネントの単語に分割する方法を示しますまたはの文を[ `NLTokenizer`](https://developer.xamarin.com/api/type/NaturalLanguage.NLTokenizer/)します。

タップして、**単語**または**文**トークンの一覧をフェッチするボタンをクリックします。 各トークンは、単語や文では、元のテキストに関連付けられます。

`ShowTokens` 呼び出すことによって、ユーザーのトークンに入力を分割します [`GetTokens`](https://developer.xamarin.com/api/member/NaturalLanguage.NLTokenizer.GetTokens/)
メソッド、`NLTokenizer`します。 このメソッドの配列を返します [`NSValue`](xref:Foundation.NSValue)
オブジェクトの場合、各折り返し、`NSRange`元のテキスト内のトークンに対応する値。

```csharp
void ShowTokens(NLTokenUnit unit)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tokenizer = new NLTokenizer(unit);
        tokenizer.String = UserInput.Text;
        var range = new NSRange(0, UserInput.Text.Length);
        NSValue[] tokens = tokenizer.GetTokens(range);
        PerformSegue(ShowTokensSegue, this);
    }
}
```

`LanguageTokenizerTableViewController` 各テーブル セルに 1 つのトークンを表示します。 抽出、`NSRange`トークンから`NSValue`元のテキストに対応する文字列を検索し、テーブル ビューのセルのラベルを設定。

```csharp
public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
    var cell = TableView.DequeueReusableCell(TokenCell);
    NSRange range = Tokens[indexPath.Row].RangeValue;
    cell.TextLabel.Text = Text.Substring((int)range.Location, (int)range.Length);
    return cell;
}
```

## <a name="tagging-named-entities-and-parts-of-speech"></a>名前付きエンティティとの品詞タグ付け

**タガー**タブ XamarinNL サンプル アプリの使用方法をについて説明します [`NLTagger`](https://developer.xamarin.com/api/type/NaturalLanguage.NLTagger/)
入力文字列のトークンにカテゴリを関連付けるクラスです。
自然言語、フレームワークには、人物、場所、組織ではの品詞を認識するための組み込みサポートが含まれています。

> [!NOTE]
> 自然言語、フレームワークには、開発者が提供の Core ML モデルを使用してカスタム タグ付けのスキームもサポートしています。 詳細についてを参照してください、[自然言語フレームワークの導入](https://developer.apple.com/videos/play/wwdc2018/713/)WWDC 2018 からセッション。

タップして、**名前付きエンティティ**または**の品詞**をフェッチするボタン。

- 配列の`NSValue`オブジェクトの場合、各折り返し、`NSRange`元のテキスト内のトークン。
- 配列の[ `NLTag` ](https://developer.xamarin.com/api/type/NaturalLanguage.NLTag/)値 – のカテゴリ、`NSValue`同じ配列のインデックス位置にあるトークンです。

`LanguageTaggerViewController`、`HandlePartsOfSpeechButtonTap`と`HandleNamedEntitiesButtonTap`の各呼び出し`ShowTags`に沿ってを渡して、 [ `NLTagScheme` ](https://developer.xamarin.com/api/type/NaturalLanguage.NLTagScheme/)か – `NLTagScheme.LexicalClass` (用の品詞) または`NLTagScheme.NameType`(名前のエンティティ)。

`ShowTags` 作成、`NLTagger`の配列をインスタンス化`NLTagScheme`をそのクエリを実行する型 (のみ渡されるでは、ここでは`NLTagScheme`値)。 次を使用して、 [`GetTags`](https://developer.xamarin.com/api/member/NaturalLanguage.NLTagger.GetTags/)
メソッドを`NLTagger`をユーザーが入力内のテキストに関連するタグを確認します。

```csharp
void ShowTags(NLTagScheme tagScheme)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tagger = new NLTagger(new NLTagScheme[] { tagScheme });
        var range = new NSRange(0, UserInput.Text.Length);
        tagger.String = UserInput.Text;

        NLTag[] tags = tagger.GetTags(range, NLTokenUnit.Word, tagScheme, NLTaggerOptions.OmitWhitespace, out NSValue[] ranges);
        NSValue[] tokenRanges = ranges;
        detailViewTitle = tagScheme == NLTagScheme.NameType ? "Named Entities" : "Parts of Speech";

        PerformSegue(ShowEntitiesSegue, this);
    }
}
```

表に、タグが表示されます、`LanguageTaggerTableViewController`します。

潜在的な`NLTag`値が含まれます。

- `Adjective`
- `Adverb`
- `Classifier`
- `CloseParenthesis`
- `CloseQuote`
- `Conjunction`
- `Dash`
- `Determiner`
- `Idiom`
- `Interjection`
- `Noun`
- `Number`
- `OpenParenthesis`
- `OpenQuote`
- `OrganizationName`
- `Other`
- `OtherPunctuation`
- `OtherWhitespace`
- `OtherWord`
- `ParagraphBreak`
- `Particle`
- `PersonalName`
- `PlaceName`
- `Preposition`
- `Pronoun`
- `Punctuation`
- `SentenceTerminator`
- `Verb`
- `Whitespace`
- `Word`
- `WordJoiner`

サポートされているタグの完全な一覧については、としての一部で、 [`NLTag`](https://developer.xamarin.com/api/type/NaturalLanguage.NLTag/)
列挙型の API のドキュメントです。

## <a name="related-links"></a>関連リンク

- [サンプル アプリ – XamarinNL](https://developer.xamarin.com/samples/monotouch/iOS12/XamarinNL)
- [自然言語フレームワークの概要](https://developer.apple.com/videos/play/wwdc2018/713/)
- [自然言語 (Apple)](https://developer.apple.com/documentation/naturallanguage?language=objc)
