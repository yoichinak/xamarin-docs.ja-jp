---
title: Xamarin.iOS でのテキスト入力
description: このドキュメントでは、Xamarin.iOS アプリでのテキスト入力について説明します。 これは、UITextField と UITextVIew を使用してプログラムと iOS デザイナーの両方について説明します。
ms.prod: xamarin
ms.assetid: 03A7F1DC-017D-4501-91FD-82C78272CDB1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: b309cbdf37acaa71740a4d5d03e4824efd40f359
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107496"
---
# <a name="text-input-in-xamarinios"></a>Xamarin.iOS でのテキスト入力

ユーザーのテキスト入力を受け取るためにでは、`UITextField`の単一行の入力と複数行の編集可能なテキストの UITextView します。 これらのコントロールのいずれかの画面にドラッグして、初期テキストを設定する をダブルクリックします。

次のスクリーン ショットは、Mac の Visual Studio のツールボックス パッドにあるこれらのコントロールのアイコンを表示します。

 [![](text-input-images/image11a.png "UITextField")](text-input-images/image11a.png#lightbox)

 [![](text-input-images/image13a.png "UITextView")](text-input-images/image13a.png#lightbox)

アウトレットを付けたし、ストーリー ボード ファイルを保存するには、Visual Studio for Mac の更新は、`.designer.cs`部分クラスを追加C#クラス ファイルにコントロールを参照しているコード。 各コントロールには独自の一意のプロパティとイベントにアクセスできる、C#コード。

 <a name="UITextField" />


## <a name="uitextfield"></a>UITextField

`UITextField`コントロールは、1 行のユーザー名やパスワードなどのテキスト入力を受け入れるように最もよく使用されます。 コントロールをカスタマイズするために使用できるオプションの一部を次に示します。

 [![](text-input-images/image15a.png "UITextField プロパティ")](text-input-images/image15a.png#lightbox)

これらのコントロールは以下について説明します。

-  **プレース ホルダー** – これは省略可能です。 設定すると、それが表示されます、テキスト フィールドが想定されているどのような入力をユーザーに説明するには、通常は、空の場合。
-  **クリア ボタン**– これは、標準のクリア ボタン ((X) で灰色の円) が表示されたら、テキスト フィールドで、ユーザーをすぐにテキストをクリアするための手段としてを制御します。 完全に非表示、完全に表示するか、フィールドを編集するかどうかに応じて、表示されることができます。
-  **最小フォント サイズ**と**に合わせて調整**– により、長いテキストに合わせてし、切り捨てを防ぐために自動的に調整されますが、制限されたフォント サイズを指定されたサイズより小さい。
-  **大文字と小文字**– 単語、文章、またはすべての入力を自動的に大文字にするかどうか。
-  **修正**– スペル チェックやご提案が有効になっているかどうか。
-  **キーボード**– コントロール キーボード スタイルが、入力に表示され、どのようなキーは、キーボードの使用可能なためです。 その他のオプションと共に数字パッド、パッドの電話、電子メール、URL が含まれます。
-  **外観**– キーボードの外観のスタイルを制御およびがいずれかの濃いまたはライト テーマとしました。
-  **キーを返す**– どのような操作が実行されるより的確に反映する戻り値のキーのラベルを変更します。 サポートされている値には、Go、結合、[次へ]、終了、ルート、検索が含まれます。
-  **セキュリティで保護された**– 入力をマスクするかどうかを識別します (パスワードの入力など)。


UITextField が呼び出された場合`textfield1`が追加されて画面に、デザイナーの設定またはそのプロパティを変更できますC#次のようにします。

```csharp
textfield1.Placeholder = "type email here...";
textfield1.KeyboardType = UIKeyboardType.EmailAddress;
textfield1.ReturnKeyType = UIReturnKeyType.Send;
textfield1.MinimumFontSize = 17f;
textfield1.AdjustsFontSizeToFitWidth = true;
```

Xamarin.iOS などの必要な設定を選択するが簡単に適切な場所の列挙体を提供します、`UIKeyboardType`と`UIReturnKeyType`上記のコード スニペット。

### <a name="display-text-programmatically"></a>プログラムでテキストを表示します。

デザイナーを使用した、画面をデザインしたくない、または実行時にテキストを動的に追加する場合は、作成し UITextField でプログラムを表示する場合、`ViewDidLoad`次のようにビュー コント ローラーのメソッド。

```csharp
var frame = new CGRect(10, 10, 300, 40);
textfield1 = new UITextField(frame);
View.Add(textfield1);
```

 <a name="UITextView" />


## <a name="uitextview"></a>UITextView

`UITextView`読み取り専用のテキストを表示する、または複数行テキスト入力を受け入れるように、コントロールを使用できます。 同じオプションの多くが、 `UITextField` (など、修正、大文字と小文字など)。

 [![](text-input-images/image16a.png "UITextView プロパティ")](text-input-images/image16a.png#lightbox)

特定のプロパティは次のとおりです。

-  **動作**– テキストを編集可能または読み取り専用かどうか。
-  **検出**– 検出にクリック可能な要素にデータを入力、呼び出しをトリガーできる電話番号をアドレスになるなどマップ、Safari で開く Url にリンクまたは日付し、時刻を変換し、カレンダーのイベントになります。


UITextView は、デザイナー画面に追加されている場合は、設定またはこのようなプロパティを変更できます。

```csharp
textview1.Text = "Lorem ipsum..."; // lots of text can go here
textview1.Editable = true;
textview1.DataDetectorTypes = UIDataDetectorType.PhoneNumber | UIDataDetectorType.Link;
```



## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
