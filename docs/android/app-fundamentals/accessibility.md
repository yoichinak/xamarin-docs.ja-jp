---
title: Android でのアクセシビリティ
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: d004b753c89f3995e8dc511877bd115a894396fc
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671625"
---
# <a name="accessibility-on-android"></a>Android でのアクセシビリティ

このページは、Android のユーザー補助の Api を使用して、に従ってアプリを構築する方法をについて説明します、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)します。
参照してください、 [iOS アクセシビリティ](~/ios/app-fundamentals/accessibility.md)と[OS X アクセシビリティ](~/mac/app-fundamentals/accessibility.md)他のプラットフォーム Api のページ。


## <a name="describing-ui-elements"></a>UI 要素を記述します。

Android に用意されて、`ContentDescription`コントロールの目的のユーザー補助の説明を提供する Api の読み取り 画面で使用されるプロパティです。

いずれかでコンテンツの説明を設定できますC#または AXML レイアウト ファイルです。

**C#**

説明は、任意の文字列 (または文字列リソース) をコードで設定できます。

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML レイアウト**

レイアウトを使用して、xml、`android:contentDescription`属性。

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>TextView のヒントを使用します。

`EditText`と`TextView`データの入力コントロールを使用して、`Hint`想定されているどのような入力の説明を入力するプロパティ (の代わりに`ContentDescription`)。
いくつかのテキストを入力すると、テキスト自体がする「読み取り」、ヒントの代わりにします。

**C#**

設定、`Hint`コード内のプロパティ。

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML レイアウト**

レイアウト ファイルを使用して、xml、`android:hint`属性。

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>LabelFor リンク ラベルを持つフィールドを入力します。

データの入力コントロールにラベルを関連付けるには、`LabelFor`プロパティを

**C#**

C#、設定、`LabelFor`プロパティをこのコンテンツを表すコントロールのリソース ID (通常このプロパティがラベルに設定およびその他のいくつかの入力コントロールを参照)。

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML レイアウト**

レイアウト XML で使用で、`android:labelFor`別のコントロールの識別子を参照するプロパティ。

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>ユーザー補助機能を発表します。

使用して、`AnnounceForAccessibility`いずれかのメソッドは、ユーザー補助機能が有効にすると、ユーザーに、イベントまたは状態変更を通信するためにコントロールを表示します。 このメソッドは、ナレーションの組み込みが、十分なフィードバックを提供しますが、追加情報をユーザーの役に立ちますを使用するほとんどの操作に必要です。

次のコードは、簡単な例の呼び出しを示しています`AnnounceForAccessibility`:。

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>フォーカスの設定の変更

アクセス可能なナビゲーションは、使用可能な操作を理解することで、ユーザーを支援するためにフォーカスを持つコントロールに依存します。 Android に用意されて、`Focusable`プロパティを具体的には、ナビゲーション中にフォーカスを受け取ることができるようにコントロールのタグを付けることができます。

**C#**

コントロールがフォーカスを取得するを防ぐためにC#、設定、`Focusable`プロパティを`false`:

```csharp
label.Focusable = false;
```

**AXML レイアウト**

XML ファイル セットのレイアウトで、`android:focusable`属性。

```xml
<android:focusable="false" />
```

フォーカスの順序を制御することも、 `nextFocusDown`、 `nextFocusLeft`、 `nextFocusRight`、 `nextFocusUp` AXML レイアウトで通常設定の属性。 これらの属性を使用して、ユーザーが画面上のコントロールを簡単に移動できます。


## <a name="accessibility-and-localization"></a>アクセシビリティとローカライズ

ヒントとコンテンツの説明は、上記の例では、表示値に直接設定します。 値を使用することをお勧め、 **Strings.xml**このなどのファイル。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

文字列のファイルからテキストを使用して、以下に示したC#と AXML レイアウト ファイル。

**C#**

コードで文字列リテラルを使用する代わりに検索翻訳済みの値を持つ文字列ファイルを`Resources.GetText`:

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

別のファイルにテキストを格納する利点は、アプリ、ファイルの複数の言語の翻訳を指定することができますです。 参照してください、 [Android ローカリゼーション ガイド](~/android/app-fundamentals/localization.md)については、アプリケーション プロジェクトにファイルのローカライズされた文字列を追加する方法。


## <a name="testing-accessibility"></a>アクセシビリティのテスト

次の[手順](https://developer.android.com/training/accessibility/testing.html#how-to)Android デバイスでのアクセシビリティをテストするには、TalkBack とタッチして探索を有効にします。

インストールする必要があります[TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback)が表示されない場合、Google Play から**設定 > ユーザー補助**します。


## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのアクセシビリティ](~/cross-platform/app-fundamentals/accessibility.md)
- [ユーザー補助の android Api](https://developer.android.com/guide/topics/ui/accessibility/index.html)
