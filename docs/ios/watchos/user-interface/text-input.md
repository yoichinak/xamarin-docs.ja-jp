---
title: "テキスト入力の使用"
ms.topic: article
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 170131a2449b37acfa411eeca54f7aa921b0d9e4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-text-input"></a>テキスト入力の使用

ただし、いくつかのウォッチが容易な方法をサポートして、Apple Watch は、テキストを入力するためのキーボードを提供しません。

- 定義済みのテキストのオプションのリストから選択します。
- Siri 音声入力
- Emoji を選択します。
- (WatchOS 3 で導入) 文字単位の手書き認識を scribble です。

シミュレーターは現在ディクテーション モードをサポートしていませんことができますもテストすることはテキスト入力コント ローラーの他のなどのオプション、Scribble、次のように。

![](text-input-images/textinput-sml.png "テスト scribble オプション")

テキストの入力を受け付ける watch アプリで。

1. 定義済みオプションの文字列配列を作成します。
2. 呼び出す`PresentTextInputController`、配列を持つか、emoji を許可するかどうかと`Action`ユーザーが完了したときに呼び出されます。
3. 完了操作の入力の結果のテストし、(ラベルのテキスト値を設定する可能性があります)、アプリで適切な操作を行ってください。

次のコード スニペットでは、ユーザーに事前に定義された 3 つのオプションが表示されます。

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

`WKTextInputMode`列挙体には 3 つの値。

- プレーン
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>プレーン

標準モードが設定されている場合、ユーザーが選択できます。

- ディクテーション、
- Scribble、または
- アプリケーションを提供する定義済みリストです。

[![](text-input-images/plain-scribble-sml.png "ディクテーション、Scribble、または、アプリを提供する定義済み一覧から")](text-input-images/plain-scribble.png#lightbox)

結果が常として返されます、`NSObject`にキャストできる、`string`です。

## <a name="emoji"></a>Emoji

Emoji の 2 つの種類があります。

- 正規の Unicode emoji
- アニメーションの画像

Unicode emoji を選択すると、文字列として返されます。

イメージのアニメーション emoji が選択されている場合、`result`ハンドラーが完了するまでに含まれて、 `NSData` 、emoji を格納しているオブジェクト`UIImage`です。

## <a name="accepting-dictation-only"></a>ディクテーションをのみ受け入れ

ディクテーション画面に直接ユーザー提案 (または、Scribble オプション) は表示されません。

- 候補の一覧の空の配列を渡すと
- Set `WatchKit.WKTextInputMode.Plain`.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

ユーザーが言うと、ウォッチ画面 (たとえば、「これはテストです」) することを理解は、テキストが含まれます次の画面が表示されます。

![](text-input-images/dictation.png "ユーザーが発話中、ウォッチ画面が表示されます、テキストが認識")

キーを押した後、**完了**ボタンのテキストが返されます。



## <a name="related-links"></a>関連リンク

- [Apple のドキュメントのテキストおよびラベル](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
