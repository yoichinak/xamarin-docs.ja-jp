---
title: "Android でのユーザー補助機能"
ms.topic: article
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: 2dea77b4c52db0c032aba9bde471e76eb36ba3ad
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="accessibility-on-android"></a>Android でのユーザー補助機能

Api を使用して、Android ユーザー補助機能によるとアプリをビルドする方法の説明、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)です。
参照してください、 [iOS アクセシビリティ](~/ios/app-fundamentals/accessibility.md)と[OS X ユーザー補助](~/mac/app-fundamentals/accessibility.md)Api の他のプラットフォーム用のページです。


## <a name="describing-ui-elements"></a>UI 要素について説明します。

Android の提供、`ContentDescription`コントロールの目的のユーザー補助の説明を提供する Api の読み取り 画面で使用されるプロパティです。

いずれか (C#) または AXML レイアウト ファイルで、コンテンツの説明を設定できます。

**C#**

説明は、任意の文字列 (または文字列リソース) をコードで設定できます。

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML レイアウト**

XML でのレイアウトを使用して、`android:contentDescription`属性。

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>TextView のヒントを使用します。

`EditText`と`TextView`データ入力には、コントロールを使用して、`Hint`が想定されているどのような入力の説明を提供するプロパティ (の代わりに`ContentDescription`)。
いくつかのテキストを入力すると、テキスト自体がする「読み取り」、ヒントの代わりにします。

**C#**

設定、`Hint`コード内のプロパティ。

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML レイアウト**

レイアウト ファイルを使用して XML で、`android:hint`属性。

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>LabelFor リンク ラベルを持つフィールドの入力

データの入力コントロールにラベルを関連付けるには使用、`LabelFor`プロパティ

**C#**

C# の場合は、設定、`LabelFor`プロパティをこのコンテンツではこのコントロールのリソース ID について説明します (通常このプロパティがラベルに設定および他の入力コントロールを参照)。

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML レイアウト**

XML で使用するレイアウトで、`android:labelFor`別のコントロールの識別子を参照するプロパティ。

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>ユーザー補助機能をアナウンスします。

使用して、`AnnounceForAccessibility`のいずれかの方法を表示するユーザー補助機能が有効になっているときにユーザーに、イベントまたは状態変更の通信を制御します。 このメソッドは、組み込みのナレーションの十分なフィードバックを提供は追加情報がユーザーの役に立ちます使用する必要がありますが、ほとんどの操作に必要ありません。

次のコードは、簡単な例の呼び出し元を示しています`AnnounceForAccessibility`:。

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>フォーカス設定を変更します。

アクセス可能なナビゲーションは、どのような操作は使用を理解することで、ユーザーを支援するためにフォーカスを持つコントロールに依存します。 Android の提供、`Focusable`プロパティを具体的にはナビゲーション中にフォーカスを受け取ることができるようにコントロールにタグを付けることができます。

**C#**

C# でのフォーカスを得たからコントロールを防ぐため、設定、`Focusable`プロパティを`false`:

```csharp
label.Focusable = false;
```

**AXML レイアウト**

XML ファイル セットのレイアウトで、`android:focusable`属性。

```xml
<android:focusable="false" />
```

フォーカス順序を制御することも、 `nextFocusDown`、 `nextFocusLeft`、 `nextFocusRight`、`nextFocusUp`レイアウト AXML で通常設定の属性です。 これらの属性を使用して、画面上のコントロールをユーザーが簡単に移動できることを確認します。


## <a name="accessibility-and-localization"></a>ユーザー補助機能とローカライズ

ヒントとコンテンツの説明は、上記の例では、表示値に直接設定します。 内の値を使用することをお勧め、 **Strings.xml**このなどのファイル。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

テキスト ファイルを使用して、文字列については、c# および AXML レイアウト ファイルで次に示します。

**C#**

コードで文字列リテラルを使用する代わりに値を検索する翻訳済みの文字列のファイルから`Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

アクセシビリティ属性などの XML のレイアウトで`hint`と`contentDescription`文字列識別子に設定することができます。

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

別のファイルにテキストを格納する利点は、アプリでファイルの複数の言語の翻訳を指定することができますです。 参照してください、 [Android ローカリゼーション ガイド](~/android/app-fundamentals/localization.md)については、アプリケーション プロジェクトにローカライズされた文字列のファイルを追加する方法です。


## <a name="testing-accessibility"></a>ユーザー補助のテスト

次の[手順](http://developer.android.com/training/accessibility/testing.html#how-to)Android デバイスでユーザー補助機能をテストするには、応答とタッチで探索を有効にします。

インストールする必要があります[応答](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback)が表示されない場合、Google Play から**設定 > アクセシビリティ**です。


## <a name="related-links"></a>関連リンク

- [クロスプラット フォームのユーザー補助機能](~/cross-platform/app-fundamentals/accessibility.md)
- [Android のユーザー補助 Api](http://developer.android.com/guide/topics/ui/accessibility/index.html)
