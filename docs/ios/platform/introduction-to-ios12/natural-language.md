---
title: Xamarin で自然言語フレームワークを使用する
description: このドキュメントでは、自然言語フレームワークについて説明します。 IOS 12 で導入された自然言語フレームワークは、言語認識、音声認識の一部、および名前付きエンティティの認識に使用する、推奨される iOS API です。
ms.prod: xamarin
ms.assetid: 126C8764-F873-4EB9-98A3-D82AB5689111
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/20/2018
ms.openlocfilehash: 235628b512a63ee2f7ec4de2176ab0b90ad65487
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652610"
---
# <a name="using-the-natural-language-framework-with-xamarinios"></a>Xamarin で自然言語フレームワークを使用する

IOS 12 で導入された自然言語フレームワークは、デバイス上の自然言語処理を可能にします。 言語認識、トークン化、タグ付けがサポートされています。 トークン化は、テキストをコンポーネント語、文、または段落に分割します。タグ付けは、音声、スタッフ、場所、および組織の部分を識別します。

自然言語フレームワークは、カスタムコア ML モデルを使用して、特殊なコンテキストでテキストの分類とタグ付けを行うこともできます。

[NSLinguisticTagger](xref:Foundation.NSLinguisticTagger)クラスは引き続き使用できます。 ただし、自然言語フレームワークは、自然言語処理に使用するために推奨されるメカニズムです。

## <a name="sample-app-xamarinnl"></a>サンプルアプリ:XamarinNL

Xamarin. iOS で自然言語フレームワークを使用する方法については、 [XamarinNL サンプルアプリ](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-xamarinnl)を参照してください。
このサンプルアプリでは、自然言語フレームワークを使用して次のことを行う方法を示します。

- [言語を認識](#recognizing-languages)します。
- [単語と文にテキストをトークン化](#tokenizing-text-into-words-sentences-and-paragraphs)します。
- [名前付きエンティティと音声の一部をタグ](#tagging-named-entities-and-parts-of-speech)付けします。

## <a name="recognizing-languages"></a>言語の認識

サンプルアプリの **[レコグナイザー]** タブでは、[`NLLanguageRecognizer`](xref:NaturalLanguage.NLLanguageRecognizer)
テキストのブロックの言語を決定する場合は。

> [!NOTE]
> 言語認識は、特定の種類のテキスト分類です。 自然言語フレームワークは、開発者が提供する主要な ML モデルによるカスタムテキスト分類もサポートしています。 詳細については、「WWDC 2018 からの[自然言語フレームワーク](https://developer.apple.com/videos/play/wwdc2018/713/)セッションの導入」を参照してください。

### <a name="dominant-language"></a>主要言語

**[言語]** ボタンをタップして、ユーザー入力で優先される言語を特定します。

の`HandleDetermineLanguageButtonTap` メソッド`LanguageRecognizerViewController`では、[`GetDominantLanguage`](xref:NaturalLanguage.NLLanguageRecognizer.GetDominantLanguage*)
をフェッチ`NLLanguageRecognizer`するのメソッド[`NLLanguage`](xref:NaturalLanguage.NLLanguage)
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

クラスのメソッドは`HandleLanguageProbabilitiesButtonTap` 、をインスタンス`NLLanguageRecognizer`化し、 `LanguageRecognizerViewController`[`Process`](xref:NaturalLanguage.NLLanguageRecognizer.Process*)
ユーザーのテキスト。 次に、言語認識エンジンを呼び出します。[`GetNativeLanguageHypotheses`](xref:NaturalLanguage.NLLanguageRecognizer.GetNativeLanguageHypotheses*)
メソッド。言語の辞書と関連する確率を取得します。 クラス`LanguageRecognizerTableViewController`は、これらの言語と確率をレンダリングします。

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

次`NLLanguage`のような値が考えられます。

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

サポートされている言語の完全な一覧については、「」を参照してください。[`NLLanguage`](xref:NaturalLanguage.NLLanguage)
API のドキュメントを列挙します。

## <a name="tokenizing-text-into-words-sentences-and-paragraphs"></a>単語、文、および段落にテキストをトークン化

サンプルアプリの **[トークナイザ]** タブでは、テキストのブロックをのコンポーネント語または文[`NLTokenizer`](xref:NaturalLanguage.NLTokenizer)に分割する方法を示します。

[**単語**または**文**] ボタンをタップして、トークンの一覧を取得します。 各トークンは、元のテキストの単語または文に関連付けられています。

`ShowTokens`を呼び出すことによって、ユーザーの入力をトークンに分割します。[`GetTokens`](xref:NaturalLanguage.NLTokenizer.GetTokens*)
の`NLTokenizer`メソッド。 このメソッドは、の配列を返します。[`NSValue`](xref:Foundation.NSValue)
オブジェクト。各オブジェクトは`NSRange` 、元のテキストのトークンに対応する値をラップします。

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

`LanguageTokenizerTableViewController`各テーブルセルに1つのトークンをレンダリングします。 `NSRange` トークン`NSValue`からを抽出し、対応する文字列を元のテキストで検索し、テーブルビューのセルにラベルを設定します。

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

XamarinNL サンプルアプリの **[Tagger]** タブでは、[`NLTagger`](xref:NaturalLanguage.NLTagger)
カテゴリを入力文字列のトークンに関連付けるクラス。
自然言語フレームワークには、人間、場所、組織、および音声の一部を認識するためのサポートが組み込まれています。

> [!NOTE]
> 自然言語フレームワークでは、開発者が提供する主要な ML モデルを使用したカスタムタグ付けスキームもサポートされています。 詳細については、「WWDC 2018 からの[自然言語フレームワーク](https://developer.apple.com/videos/play/wwdc2018/713/)セッションの導入」を参照してください。

**名前付きエンティティ**または**音声認識の部分**をタップしてフェッチします。

- オブジェクトの`NSValue`配列。各オブジェクトは、 `NSRange`元のテキストのトークンのをラップします。
- 値の[`NLTag`](xref:NaturalLanguage.NLTag)配列–同じ配列インデックスに`NSValue`あるトークンのカテゴリ。

`LanguageTaggerViewController` `ShowTags` [`NLTagScheme`](xref:NaturalLanguage.NLTagScheme) `NLTagScheme.NameType`では、と`HandleNamedEntitiesButtonTap`の各呼び出しは、 (音声の一部に対して)または(名前付きエンティティの場合)を渡します。`NLTagScheme.LexicalClass` `HandlePartsOfSpeechButtonTap`

`ShowTags`を作成し`NLTagScheme` `NLTagScheme` 、クエリ対象の型の配列 (この場合は、渡された値のみ) を使用してインスタンス化します。 `NLTagger` 次のものを使用します。[`GetTags`](xref:NaturalLanguage.NLTagger.GetTags*)
ユーザー入力の`NLTagger`テキストに関連するタグを特定するためのメソッド。

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

タグは、 `LanguageTaggerTableViewController`によってテーブル内に表示されます。

次`NLTag`のような値が考えられます。

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

サポートされているタグの完全な一覧は、「」の一部として利用できます。[`NLTag`](xref:NaturalLanguage.NLTag)
API のドキュメントを列挙します。

## <a name="related-links"></a>関連リンク

- [サンプルアプリ– XamarinNL](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-xamarinnl)
- [自然言語フレームワークの概要](https://developer.apple.com/videos/play/wwdc2018/713/)
- [自然言語 (Apple)](https://developer.apple.com/documentation/naturallanguage?language=objc)
