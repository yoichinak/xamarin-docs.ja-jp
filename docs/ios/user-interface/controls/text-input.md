---
title: "テキスト入力"
ms.topic: article
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: ce1014616d0cf5f6cd5228d69976dfeca546b382
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="text-input"></a>テキスト入力

実現は、ユーザーのテキスト入力を受け入れ、`UITextField`単一行の入力と複数行の編集可能なテキストの UITextView します。 これらのコントロールのいずれかの画面にドラッグし、最初のテキストを設定する をダブルクリックできます。

次のスクリーン ショットは、Mac の Visual Studio のツールボックス パッドにあるこれらのコントロールのアイコンを表示します。

 [ ![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png)

 [ ![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png)

コンセントをという名前が付いてし、ストーリー ボード ファイルを保存するには、Visual Studio for Mac は更新、`.designer.cs`部分クラスとする c# クラス ファイルにコントロールを参照するコードを追加できます。 各コントロールには、独自の固有のプロパティと c# コードでアクセスできるイベントがあります。

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

`UITextField`を 1 行のユーザー名やパスワードなどのテキスト入力を受け付けるようにコントロールを最も頻繁に使用します。 コントロールのカスタマイズに使用できるオプションの一部を次に示します。

 [ ![](text-input-images/image15a.png "UITextField プロパティ")](text-input-images/image15a.png)

これらのコントロールについて詳しく説明します。

-  **プレース ホルダー** – これは省略可能です。 かどうかを設定するには表示テキスト フィールドが空で通常想定されているどのような入力をユーザーに説明します。
-  **ボタンをオフに**– これは、標準のクリア ボタン ((X) の付いた灰色の円) が表示される場合、テキスト フィールドで、ユーザーを簡単にテキストをクリアするための手段としてを制御します。 完全に非表示、完全に表示されている、またはフィールドが編集されているかどうかに応じて、表示されているを指定できます。
-  **最小フォント サイズ**と**サイズに合わせて調整**– により、長いテキストに合わせての切り捨てが妨げに自動的に調整されるが、制限されたフォント サイズ no に指定されたサイズより小さい値です。
-  **大文字と小文字**– 単語、文、またはすべての入力を自動的に大文字に変換するかどうか。
-  **修正**– スペル チェックやご提案が有効になっているかどうか。
-  **キーボード**– 入力には、コントロールのキーボード スタイル表示され、どのようなキーは、キーボードで使用できるためです。 その他のオプションと共に数字パッド、Phone パッド、電子メール、URL が含まれます。
-  **外観**– キーボードの外観のスタイルを制御しは、暗いかされる淡色テーマです。
-  **キーを返す**: どのようなアクションは実行をより反映するように戻り値のキーのラベルを変更します。 サポートされている値には、移動、結合、次へ、Route、完了、および検索が含まれます。
-  **セキュリティで保護された**– 入力をマスクするかどうかを示します (パスワードの入力など)。


UITextField が呼び出された場合`textfield1`が追加されて、画面に、デザイナーで設定または C# の場合は、そのプロパティを次のように変更することができます。

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

などの必要な設定を選択するが簡単に適切な場所、Xamarin.iOS が列挙体を提供、`UIKeyboardType`と`UIReturnKeyType`上記のコード スニペット。

### <a name="display-text-programmatically"></a>プログラムによってテキストを表示します。

デザイナーで、画面をデザインしたくない、または実行時にいくつかのテキストを動的に追加する場合は、作成してプログラム的に UITextField を表示する場合、`ViewDidLoad`次のようにビュー コント ローラーのメソッド。

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

`UITextView`読み取り専用のテキストを表示するか、複数行テキスト入力を受け入れるように、コントロールを使用できます。 同様のオプションの多くがある、 `UITextField` (など、修正、大文字と小文字など)。

 [ ![](text-input-images/image16a.png "UITextView プロパティ")](text-input-images/image16a.png)

特定のプロパティは次のとおりです。

-  **動作**– テキストが編集可能または読み取り専用であるかどうか。
-  **検出**– クリック可能な要素にデータを入力、呼び出しをトリガーできる電話番号に対応になるなど、マップ、Safari で開く Url にリンクまたは日付し、時刻を変換し、検出は、カレンダーのイベントになります。


を、UITextView がデザイナーで画面に追加した場合は、設定または次のようにそのプロパティを変更できます。

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
