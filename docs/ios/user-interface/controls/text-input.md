---
title: Xamarin. iOS でのテキスト入力
description: このドキュメントでは、Xamarin iOS アプリでのテキスト入力について説明します。 ここでは、プログラムと iOS Designer の両方で UITextField と Uitextfield を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 4833d8a03649341cb5c6d9f2692410b89e6cea4c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021820"
---
# <a name="text-input-in-xamarinios"></a>Xamarin. iOS でのテキスト入力

ユーザーテキスト入力の受け入れは、単一行入力の `UITextField` と、複数行の編集可能なテキストの UITextView を使用して行われます。 これらのコントロールのいずれかを画面にドラッグしてダブルクリックすると、初期のテキストを設定できます。

次のスクリーンショットは、Visual Studio for Mac のツールボックスパッドにあるこれらのコントロールのアイコンを示しています。

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

アウトレットに名前を付けてストーリーボードファイルを保存すると、Visual Studio for Mac によって `.designer.cs` 部分クラスが更新C#され、そのコントロールを参照するコードをクラスファイルに追加できます。 各コントロールには、 C#コード内でアクセスできる独自の固有のプロパティとイベントがあります。

 <a name="UITextField" />

## <a name="uitextfield"></a>UITextField

`UITextField` コントロールは、ユーザー名やパスワードなど、1行のテキスト入力を受け入れるためによく使用されます。 コントロールをカスタマイズするために使用できるオプションの一部を次に示します。

 [![](text-input-images/image15a.png "UITextField Properties")](text-input-images/image15a.png#lightbox)

これらのコントロールについて以下に説明します。

- **Placeholder** –これは省略可能です。 設定すると、テキストフィールドが空のときに表示されます。通常は、ユーザーに入力が必要であることをユーザーに説明します。
- **クリアボタン**–ユーザーがテキストをすばやくクリアする方法として、標準のクリアボタン ((X) を含む灰色の円) がテキストフィールドに表示されるタイミングを制御します。 フィールドが編集されているかどうかによって、完全に非表示にしたり、永続的に表示したり、表示したりすることができます。
- [**フォントサイズの最小値とサイズ** **に合わせて調整**する] –テキストに合わせてフォントサイズを自動的に調整し、切り捨てを防止します。ただし、指定したサイズより小さくすることはできません。
- **大文字と小文字**の区別–単語、文、またはすべての入力を自動的に大文字にするかどうか。
- **修正**–スペルチェックと提案が有効になっているかどうか。
- **キーボード**–入力に表示されるキーボードのスタイルを制御します。したがって、キーボードで使用できるキーを制御します。 これには、テンキー、電話パッド、電子メール、URL、およびその他のオプションが含まれます。
- **[表示]** –キーボードの外観スタイルを制御します。濃いまたは淡色のテーマが適用されます。
- **Return key** –返されるアクションをより正確に反映するために、リターンキーのラベルを変更します。 サポートされる値は、[検索]、[結合]、[次へ]、[ルート]、[完了]、[検索] です。
- **Secure** –入力がマスクされているかどうか (パスワード入力など) を識別します。

`textfield1` という UITextField がデザイナーを使用して画面に追加されている場合は、次のようC#にのプロパティを設定または変更できます。

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin には、上記のコードスニペットの `UIKeyboardType` や `UIReturnKeyType` など、必要な設定を簡単に選択できるようにするための列挙が用意されています。

### <a name="display-text-programmatically"></a>プログラムによるテキストの表示

デザイナーを使用して画面をデザインしない場合、または実行時にいくつかのテキストを動的に追加する場合は、次のようにビューコントローラーの `ViewDidLoad` メソッドで UITextField をプログラムによって作成および表示できます。

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />

## <a name="uitextview"></a>UITextView

`UITextView` コントロールを使用して、読み取り専用のテキストを表示したり、複数行のテキスト入力を許可したりできます。 `UITextField` と同じオプションが多数あります (大文字と小文字の修正など)。

 [![](text-input-images/image16a.png "UITextView Properties")](text-input-images/image16a.png#lightbox)

具体的なプロパティは次のとおりです。

- **動作**–テキストを編集可能にするか、読み取り専用にするかを指定します。
- **検出**–入力データを検出し、呼び出しをトリガーできる電話番号、マップへのリンクになるアドレス、Safari で開かれている Url、カレンダーでイベントになる日付と時刻などの、クリック可能な要素に変換します。

UITextView がデザイナーを使用して画面に追加されている場合は、次のようにプロパティを設定または変更できます。

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
