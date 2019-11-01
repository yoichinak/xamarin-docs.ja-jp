---
title: Android でのユーザー補助機能
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/28/2018
ms.openlocfilehash: 6e86663be0bb06697fbfe0e8c360d733bca18da0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017020"
---
# <a name="accessibility-on-android"></a>Android でのユーザー補助機能

このページでは、[ユーザー補助のチェックリスト](~/cross-platform/app-fundamentals/accessibility.md)に従って、Android アクセシビリティ api を使用してアプリを構築する方法について説明します。
他のプラットフォーム Api については、 [iOS のアクセシビリティ](~/ios/app-fundamentals/accessibility.md)に[関するページを参照して](~/mac/app-fundamentals/accessibility.md)ください。

## <a name="describing-ui-elements"></a>UI 要素の記述

Android には `ContentDescription` プロパティが用意されています。このプロパティは、コントロールの目的についてのユーザー補助の説明を提供するために、画面読み取り Api で使用されます。

コンテンツの説明は、AXML レイアウトC#ファイルのまたはで設定できます。

**C#**

説明は、任意の文字列 (または文字列リソース) にコードで設定できます。

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML レイアウト**

XML レイアウトでは、`android:contentDescription` 属性を使用します。

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>TextView のヒントを使用する

データ入力の `EditText` および `TextView` コントロールについては、`Hint` プロパティを使用して、必要な入力 (`ContentDescription` ではなく) についての説明を入力します。
テキストが入力されると、ヒントの代わりにテキスト自体が "読み取り" になります。

**C#**

コードで `Hint` プロパティを設定します。

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML レイアウト**

XML レイアウトファイルでは、`android:hint` 属性を使用します。

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```

### <a name="labelfor-links-input-fields-with-labels"></a>ラベルを含むリンクの入力フィールド

データ入力コントロールにラベルを関連付けるには、`LabelFor` プロパティを使用します。

**C#**

でC#、[`LabelFor`] プロパティを、このコンテンツが記述するコントロールのリソース ID に設定します (通常、このプロパティはラベルに設定され、他の入力コントロールを参照します)。

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML レイアウト**

レイアウト XML では、`android:labelFor` プロパティを使用して、別のコントロールの識別子を参照します。

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>ユーザー補助のためのアナウンス

ユーザー補助機能が有効になっている場合は、任意のビューコントロールの `AnnounceForAccessibility` メソッドを使用して、イベントまたは状態の変更をユーザーに通知します。 この方法は、組み込みのナレーションによって十分なフィードバックが得られるが、ユーザーにとって追加情報が役に立つ場合に使用する必要があるほとんどの操作には必要ありません。

次のコードは、`AnnounceForAccessibility` を呼び出す簡単な例を示しています。

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>フォーカス設定の変更

アクセス可能なナビゲーションは、使用可能な操作をユーザーが理解できるように、フォーカスを持つコントロールに依存します。 Android には `Focusable` プロパティが用意されており、ナビゲーション中にフォーカスを受け取ることができるように、コントロールにタグを付けることができます。

**C#**

コントロールがとのフォーカスを得らC#れないようにするには、`Focusable` プロパティを `false` に設定します。

```csharp
label.Focusable = false;
```

**AXML レイアウト**

レイアウト XML ファイルで、`android:focusable` 属性を設定します。

```xml
<android:focusable="false" />
```

また、レイアウト AXML で通常設定される `nextFocusDown`、`nextFocusLeft`、`nextFocusRight`、`nextFocusUp` の属性を使用して、フォーカスの順序を制御することもできます。 これらの属性を使用して、ユーザーが画面上のコントロールを簡単に移動できるようにします。

## <a name="accessibility-and-localization"></a>アクセシビリティとローカライズ

上の例では、ヒントとコンテンツの説明が、表示値に直接設定されています。 次のように、**文字列 .xml**ファイルの値を使用することをお勧めします。

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

文字列ファイルからテキストを使用する方法を次C#に示します。

**C#**

コードで文字列リテラルを使用する代わりに、`Resources.GetText` を使用して文字列ファイルから翻訳された値を参照します。

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**MAIN.AXML**

`hint` や `contentDescription` などのレイアウト XML アクセシビリティ属性は、文字列識別子に設定できます。

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

テキストを別のファイルに格納することの利点は、複数の言語翻訳ファイルをアプリで提供できることです。 ローカライズされた文字列ファイルをアプリケーションプロジェクトに追加する方法については、「 [Android のローカリゼーションガイド](~/android/app-fundamentals/localization.md)」を参照してください。

## <a name="testing-accessibility"></a>アクセシビリティのテスト

[次の手順](https://developer.android.com/training/accessibility/testing.html#how-to)に従って、TalkBack を有効にし、タッチで探索して、Android デバイスでアクセシビリティをテストします。

Google Play からの[TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback)のインストールが必要になる場合があります。 [**設定] > [ユーザー補助**] に表示されません。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのアクセシビリティ](~/cross-platform/app-fundamentals/accessibility.md)
- [Android アクセシビリティ Api](https://developer.android.com/guide/topics/ui/accessibility/index.html)
