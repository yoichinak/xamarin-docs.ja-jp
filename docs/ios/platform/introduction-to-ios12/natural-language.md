---
title: Xamarin で自然言語フレームワークを使用する
description: このドキュメントでは、自然言語フレームワークについて説明します。 IOS 12 で導入された自然言語フレームワークは、言語認識、音声認識の一部、および名前付きエンティティの認識に使用する、推奨される iOS API です。
ms.prod: xamarin
ms.assetid: 126C8764-F873-4EB9-98A3-D82AB5689111
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/20/2018
ms.openlocfilehash: 1598bad7bdbea8334b7fdfa2b950400b698579b0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031995"
---
# <a name="using-the-natural-language-framework-with-xamarinios"></a>Xamarin で自然言語フレームワークを使用する

IOS 12 で導入された自然言語フレームワークは、デバイス上の自然言語処理を可能にします。 言語認識、トークン化、タグ付けがサポートされています。 トークン化は、テキストをコンポーネント語、文、または段落に分割します。タグ付けは、音声、スタッフ、場所、および組織の部分を識別します。

自然言語フレームワークは、カスタムコア ML モデルを使用して、特殊なコンテキストでテキストの分類とタグ付けを行うこともできます。

[NSLinguisticTagger](xref:Foundation.NSLinguisticTagger)クラスは引き続き使用できます。 ただし、自然言語フレームワークは、自然言語処理に使用するために推奨されるメカニズムです。

## <a name="sample-app-xamarinnl"></a>サンプルアプリ: XamarinNL

Xamarin. iOS で自然言語フレームワークを使用する方法については、 [XamarinNL サンプルアプリ](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-xamarinnl)を参照してください。
このサンプルアプリでは、自然言語フレームワークを使用して次のことを行う方法を示します。

- [言語を認識](#recognizing-languages)します。
- [単語と文にテキストをトークン化](#tokenizing-text-into-words-sentences-and-paragraphs)します。
- [名前付きエンティティと音声の一部をタグ](#tagging-named-entities-and-parts-of-speech)付けします。

## <a name="recognizing-languages"></a>言語の認識

サンプルアプリの **[レコグナイザー]** タブは、の使用方法を示してい[`NLLanguageRecognizer`](xref:NaturalLanguage.NLLanguageRecognizer)
テキストのブロックの言語を決定する場合は。

> [!NOTE]
> 言語認識は、特定の種類のテキスト分類です。 自然言語フレームワークは、開発者が提供する主要な ML モデルによるカスタムテキスト分類もサポートしています。 詳細については、「WWDC 2018 からの[自然言語フレームワーク](https://developer.apple.com/videos/play/wwdc2018/713/)セッションの導入」を参照してください。

### <a name="dominant-language"></a>主要言語

**[言語]** ボタンをタップして、ユーザー入力で優先される言語を特定します。

`LanguageRecognizerViewController` の `HandleDetermineLanguageButtonTap` メソッドは[`GetDominantLanguage`](xref:NaturalLanguage.NLLanguageRecognizer.GetDominantLanguage*)を使用します。
[`NLLanguage`](xref:NaturalLanguage.NLLanguage)をフェッチするための `NLLanguageRecognizer` のメソッド
テキストで見つかった主言語:

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

**[言語確率]** ボタンをタップして、ユーザー入力の言語仮説の一覧を取得します。

`LanguageRecognizerViewController` クラスの `HandleLanguageProbabilitiesButtonTap` メソッドは `NLLanguageRecognizer` をインスタンス化し、それを要求し[`Process`](xref:NaturalLanguage.NLLanguageRecognizer.Process*)
ユーザーのテキスト。 次に、言語認識エンジンの[`GetNativeLanguageHypotheses`](xref:NaturalLanguage.NLLanguageRecognizer.GetNativeLanguageHypotheses*)を呼び出します。
メソッド。言語の辞書と関連する確率を取得します。 次に、`LanguageRecognizerTableViewController` クラスによって、これらの言語と確率がレンダリングされます。

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

考えられる `NLLanguage` 値は次のとおりです。

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

サポートされている言語の完全な一覧については、 [`NLLanguage`](xref:NaturalLanguage.NLLanguage)を参照してください。
API のドキュメントを列挙します。

## <a name="tokenizing-text-into-words-sentences-and-paragraphs"></a>単語、文、および段落にテキストをトークン化

サンプルアプリの **[トークナイザ]** \ (構成 \) タブでは、テキストのブロックをコンポーネント語や文に分割する方法を、 [`NLTokenizer`](xref:NaturalLanguage.NLTokenizer)で示します。

[**単語**または**文**] ボタンをタップして、トークンの一覧を取得します。 各トークンは、元のテキストの単語または文に関連付けられています。

`ShowTokens` は、 [`GetTokens`](xref:NaturalLanguage.NLTokenizer.GetTokens*)を呼び出して、ユーザーの入力をトークンに分割します
`NLTokenizer`のメソッド。 このメソッドは、の配列を返し[`NSValue`](xref:Foundation.NSValue)
オブジェクト。各オブジェクトは、元のテキストのトークンに対応する `NSRange` 値をラップします。

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

`LanguageTokenizerTableViewController` では、各テーブルセルに1つのトークンがレンダリングされます。 トークン `NSValue`から `NSRange` を抽出し、対応する文字列を元のテキストで検索し、テーブルビューのセルにラベルを設定します。

```csharp
public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
    var cell = TableView.DequeueReusableCell(TokenCell);
    NSRange range = Tokens[indexPath.Row].RangeValue;
    cell.TextLabel.Text = Text.Substring((int)range.Location, (int)range.Length);
    return cell;
}
```

## <a name="tagging-named-entities-and-parts-of-speech"></a>名前付きエンティティと音声の一部のタグ付け

XamarinNL サンプルアプリの**Tagger**タブは[`NLTagger`](xref:NaturalLanguage.NLTagger) 、の使用方法を示しています。
カテゴリを入力文字列のトークンに関連付けるクラス。
自然言語フレームワークには、人間、場所、組織、および音声の一部を認識するためのサポートが組み込まれています。

> [!NOTE]
> 自然言語フレームワークでは、開発者が提供する主要な ML モデルを使用したカスタムタグ付けスキームもサポートされています。 詳細については、「WWDC 2018 からの[自然言語フレームワーク](https://developer.apple.com/videos/play/wwdc2018/713/)セッションの導入」を参照してください。

**名前付きエンティティ**または**音声認識の部分**をタップしてフェッチします。

- `NSValue` オブジェクトの配列。それぞれが元のテキストのトークンの `NSRange` をラップします。
- [`NLTag`](xref:NaturalLanguage.NLTag)値の配列–同じ配列インデックスにある `NSValue` トークンのカテゴリ。

`LanguageTaggerViewController`では、各呼び出し `ShowTags`を `HandlePartsOfSpeechButtonTap` して `HandleNamedEntitiesButtonTap`、 [`NLTagScheme`(](xref:NaturalLanguage.NLTagScheme)音声の一部) または `NLTagScheme.LexicalClass` (名前付きエンティティの場合) のいずれかを渡します。

`ShowTags` は、`NLTagger`を作成し、クエリを実行する `NLTagScheme` 型の配列を使用してインスタンス化します (この例では、渡された `NLTagScheme` 値のみ)。 次に、 [`GetTags`](xref:NaturalLanguage.NLTagger.GetTags*)を使用します。
`NLTagger` のメソッドを指定して、ユーザー入力のテキストに関連するタグを決定します。

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

タグは、`LanguageTaggerTableViewController`によってテーブルに表示されます。

考えられる `NLTag` 値は次のとおりです。

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

サポートされているタグの完全な一覧については、 [`NLTag`](xref:NaturalLanguage.NLTag)を参照してください。
API のドキュメントを列挙します。

## <a name="related-links"></a>関連リンク

- [サンプルアプリ– XamarinNL](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-xamarinnl)
- [自然言語フレームワークの概要](https://developer.apple.com/videos/play/wwdc2018/713/)
- [自然言語 (Apple)](https://developer.apple.com/documentation/naturallanguage?language=objc)
