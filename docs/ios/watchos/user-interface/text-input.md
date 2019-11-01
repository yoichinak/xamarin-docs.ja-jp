---
title: Xamarin での watchOS テキスト入力の操作
description: このドキュメントでは、Xamarin での watchOS テキスト入力について説明します。 ここでは、scribbling、plain text、emojis、およびディクテーションについて説明します。
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 156a31e37d14ce3e3cbe7173ae97b608e9d4c32e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032649"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>Xamarin での watchOS テキスト入力の操作

Apple Watch には、ユーザーがテキストを入力するためのキーボードは用意されていませんが、いくつかの見やすい代替手段がサポートされています。

- 定義済みのテキストオプションの一覧から選択します。
- Siri ディクテーション、
- 絵文字を選択すると、
- Scribble 文字単位の手書き認識 (watchOS 3 で導入)。

現在、シミュレーターではディクテーションはサポートされていませんが、次に示すように、テキスト入力コントローラーのその他のオプション (Scribble など) をテストできます。

![](text-input-images/textinput-sml.png "Testing the scribble option")

Watch アプリでテキスト入力を受け入れるには、次のようにします。

1. 定義済みのオプションの文字列配列を作成します。
2. 配列を使用して `PresentTextInputController` を呼び出します。絵文字を許可するかどうかにかかわらず、ユーザーが終了したときに呼び出される `Action` ます。
3. [完了] アクションで、入力結果をテストし、アプリで適切なアクションを実行します (ラベルのテキスト値を設定する場合もあります)。

次のコードスニペットは、ユーザーに対して定義済みの3つのオプションを示しています。

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

`WKTextInputMode` 列挙体には、次の3つの値があります。

- プレーン
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>プレーン

プレーンモードが設定されている場合、ユーザーは次のいずれかを選択できます。

- 音声
- Scribble、または
- アプリケーションによって提供される定義済みのリストから。

[![](text-input-images/plain-scribble-sml.png "Dictation, Scribble, or from a pre-defined list that the app supplies")](text-input-images/plain-scribble.png#lightbox)

結果は常に、`string`にキャストできる `NSObject` として返されます。

## <a name="emoji"></a>絵文字

絵文字には次の2種類があります。

- 標準の Unicode 絵文字
- アニメーション画像

ユーザーが Unicode 絵文字を選択すると、文字列として返されます。

アニメーション画像の絵文字が選択されている場合、完了ハンドラーの `result` には、絵文字 `UIImage`を含む `NSData` オブジェクトが含まれます。

## <a name="accepting-dictation-only"></a>ディクテーションのみを受け入れる

修正候補 (または Scribble オプション) を表示せずにユーザーを直接ディクテーション画面に移動するには、次のようにします。

- 候補リストに空の配列を渡します。
- `WatchKit.WKTextInputMode.Plain`を設定します。

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

ユーザーが話しているときに、認識されたテキストを含む次の画面が [ウォッチ] 画面に表示されます ("This is a test" など)。

![](text-input-images/dictation.png "When the user is speaking, the watch screen displays the text as it is understood")

**[完了]** ボタンを押すと、テキストが返されます。

## <a name="related-links"></a>関連リンク

- [Apple のテキストとラベルのドキュメント](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
