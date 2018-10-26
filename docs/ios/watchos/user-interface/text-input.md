---
title: WatchOS Xamarin でのテキスト入力の操作
description: このドキュメントでは、Xamarin で watchOS テキスト入力について説明します。 PresentTextInputController メソッド、書き留めたり、プレーン テキスト、絵文字、および音声入力がについて説明します。
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 2092b12254008936f2c5b6a7d9dd610ff751e802
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122362"
---
# <a name="working-with-watchos-text-input-in-xamarin"></a>WatchOS Xamarin でのテキスト入力の操作

ただし、いくつかの監視に適した代替をサポートして、Apple Watch のは、テキストを入力するためのキーボードを提供しません。

- テキストのオプションの定義済み一覧から選択します。
- Siri の音声入力
- 、、の絵文字を選択します。
- (WatchOS 3 で導入) 文字で、手書き認識を scribble します。

シミュレーターがディクテーションを現在サポートしていませんがすることができますもテスト テキスト入力コント ローラーの他のオプション、Scribble などは、ここに示すように。

![](text-input-images/textinput-sml.png "Scribble オプションのテスト")

Watch アプリでのテキスト入力を受け取る。

1. 定義済みオプションの文字列配列を作成します。
2. 呼び出す`PresentTextInputController`、配列を持つか、絵文字を許可するかどうかと`Action`ユーザーが終了すると呼び出されます。
3. 完了したアクションでは、入力の結果をテストし、(ラベルのテキスト値を設定する可能性があります)、アプリで適切な処置を実行します。

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

プレーンなモードが設定されている場合、ユーザーが選択できます。

- ディクテーション、
- Scribble、または
- アプリケーションを提供する定義済みリスト。

[![](text-input-images/plain-scribble-sml.png "ディクテーション、Scribble の場合、または、アプリを提供する定義済みリストから")](text-input-images/plain-scribble.png#lightbox)

結果が常として返されます、`NSObject`にキャストできる、`string`します。

## <a name="emoji"></a>絵文字

絵文字の 2 種類あります。

- 正規の Unicode の絵文字
- アニメーション化されたイメージ

ユーザーが Unicode 絵文字を文字列として返されます。

アニメーション画像、絵文字が選択されている場合、`result`ハンドラーが含まれますが完了するまで、`NSData`絵文字を含むオブジェクト`UIImage`します。

## <a name="accepting-dictation-only"></a>ディクテーションののみ受け入れ

提案 (または Scribble オプション) を表示せず、ディクテーション画面に直接ユーザーを実行します。

- 候補の一覧の空の配列を渡すと
- 設定`WatchKit.WKTextInputMode.Plain`します。

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

ユーザーが言うと、ウォッチ画面 (たとえば、"This is a test") することを理解は、テキストを含む次の画面が表示されます。

![](text-input-images/dictation.png "ウォッチの画面にテキストが表示されますが認識されることと、ユーザーが言うと、")

キーを押した後、**完了**ボタン、テキストが返されます。



## <a name="related-links"></a>関連リンク

- [Apple のドキュメントのテキストとラベル](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
