---
title: .AAR のバインド
description: このチュートリアルでは、Android から Xamarin Android Java バインドライブラリを作成する手順について説明します。AAR ファイル。
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: cdd68def895fea362d9ad3147e3d622471d73a63
ms.sourcegitcommit: 4ff181101d76f048b949c9613b2c72cf02618f8b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "71994892"
---
# <a name="binding-an-aar"></a>.AAR のバインド

_このチュートリアルでは、Android から Xamarin Android Java バインドライブラリを作成する手順について説明します。AAR ファイル。_

## <a name="overview"></a>概要

*Android アーカイブ (.AAR)* ファイルは、Android ライブラリのファイル形式です。
込み.AAR ファイルはです。次のものを含む ZIP アーカイブ:

- コンパイル済み Java コード
- リソース Id
- リソース
- メタデータ (アクティビティの宣言、アクセス許可など)

このガイドでは、1つのにバインドライブラリを作成する方法の基本について説明します。AAR ファイル。 一般的な Java ライブラリバインディングの概要 (基本的なコード例を含む) については、「 [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)」を参照してください。

> [!IMPORTANT]
> バインドプロジェクトに含めることができるのは1つだけです。AAR ファイル。 の場合は。他のに依存関係を AAR します。AAR、これらの依存関係は、独自のバインドプロジェクトに含めてから参照する必要があります。 [バグ 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)を参照してください。

## <a name="walkthrough"></a>チュートリアル

Android Studio、 [aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true)で作成したサンプルの Android アーカイブファイルのバインドライブラリを作成します。 これ。AAR には、文字列の母音と子音の数をカウントする静的メソッドを含む `TextCounter` のクラスが含まれています。 さらに、aar には、カウントの結果を表示するのに役立つイメージリソースが含まれてい**ます。**

からバインドライブラリを作成するには、次の手順を使用します。AAR ファイル (i):

1. 新しい Java バインドライブラリプロジェクトを作成します。

2. を1つ追加します。AAR ファイルをプロジェクトにします。 バインドプロジェクトには、1つだけを含めることができます。AAR.

3. に適切なビルドアクションを設定します。AAR ファイル。

4. をインストールするターゲットフレームワークを選択します。AAR は、をサポートしています。

5. バインドライブラリをビルドします。

バインドライブラリを作成したら、ユーザーにテキスト文字列を要求する小さな Android アプリを開発し、を呼び出します。AAR メソッドは、テキストを分析するために、からイメージを取得します。AAR を使用すると、イメージと共に結果が表示されます。

サンプルアプリは、 **textanalyzer. aar**の `TextCounter` クラスにアクセスします。

```java
package com.xamarin.textcounter;

public class TextCounter
{
    ...
    public static int numVowels (String text) { ... };
    ...
    public static int numConsonants (String text) { ... };
    ...
}
```

さらに、このサンプルアプリは、 **textanalyzer. aar**にパッケージ化されているイメージリソースを取得して表示します。

[![Xamarin のサル画像](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

このイメージリソースは、 **aar**の**res/描画/サル .png**に置かれています。

### <a name="creating-the-bindings-library"></a>バインドライブラリの作成

以下の手順を実行する前に、サンプルの[textanalyzer. aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android アーカイブファイルをダウンロードしてください。

1. Android バインドライブラリテンプレートを使用して、新しいバインドライブラリプロジェクトを作成します。 Visual Studio for Mac または Visual Studio のいずれかを使用できます (以下のスクリーンショットは Visual Studio を示していますが、Visual Studio for Mac はよく似ています)。 ソリューションに**AarBinding**という名前を指定します。

    [![Create プロジェクトの作成](binding-an-aar-images/01-new-bindings-library-vs-sml.w160.png)](binding-an-aar-images/01-new-bindings-library-vs.w160.png#lightbox)

2. テンプレートには、を追加する**jar**フォルダーが含まれています。AAR (s) をバインドライブラリプロジェクトに対して行います。 **[Jar]** フォルダーを右クリックし、 **[既存の項目の追加 >]** を選択します。

    [![既存の項目を追加する](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)

3. 先ほどダウンロードした**aar**ファイルに移動して選択し、 **[追加]** をクリックします。

    [![Add aar を追加します。](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)

4. **Aar**ファイルがプロジェクトに正常に追加されたことを確認します。

    [![The textanalyzer. aar ファイルが追加されました](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. **Aar**のビルドアクションを `LibraryProjectZip` に設定します。 Visual Studio for Mac で、 **aar**を右クリックして、ビルドアクションを設定します。 Visual Studio では、 **[プロパティ]** ペインでビルドアクションを設定できます)。

    [![Textanalyzer. aar ビルドアクションを LibraryProjectZip に設定しています](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. プロジェクトのプロパティを開いて、*ターゲットフレームワーク*を構成します。 の場合は。AAR は、任意の Android Api を使用して、ターゲットフレームワークをに設定されている API レベルに設定します。AAR が必要です。 (ターゲットフレームワークの設定と、一般的な Android API レベルの詳細については、「 [ANDROID Api レベル](~/android/app-fundamentals/android-api-levels.md)について」を参照してください)。

    バインドライブラリのターゲット API レベルを設定します。 この例では自由にために API レベル (API レベル 23) の最新のプラットフォームを使用して、 **textanalyzer** Android API に依存関係はありません。

    [![ ターゲットレベルを API 23 に設定しています](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. バインドライブラリをビルドします。 バインドライブラリプロジェクトが正常にビルドされ、出力が生成されます。次の場所にある DLL:**AarBinding/bin/Debug/AarBinding**

### <a name="using-the-bindings-library"></a>バインドライブラリの使用

これを使用する場合は。DLL Xamarin Android アプリでは、最初にバインドライブラリへの参照を追加する必要があります。 次の手順を使用します。

1. このチュートリアルを簡略化するために、このアプリをバインドライブラリと同じソリューションに作成しています。 (バインディングライブラリを使用するアプリケーションは、別のソリューションにも存在する可能性があります)。新しい Xamarin Android アプリを作成します。ソリューションを右クリックし、 **[新しいプロジェクトの追加]** を選択します。 新しいプロジェクトに**Bindingtest**という名前を指定します。

    [![Create BindingTest プロジェクトの作成](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. **Bindingtest**プロジェクトの **[参照]** ノードを右クリックし、 **[参照の追加]** を選択します。

    [![Click 参照の追加 をクリック](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. 前の手順で作成した**AarBinding**プロジェクトを選択し、[ **OK]** をクリックします。

    [![Check binding プロジェクトを確認する](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. **Bindingtest**プロジェクトの **[参照設定]** ノードを開き、 **AarBinding**参照が存在することを確認します。

    [[AarBinding が [参照] の下に一覧表示されます。](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)

バインドライブラリプロジェクトの内容を表示するには、参照をダブルクリックして、**オブジェクトブラウザー**で開きます。 (Java `com.xamarin.textanalyzezr` パッケージからマップされた) `Com.Xamarin.Textcounter` 名前空間のマップされた内容を確認できます。また、`TextCounter` クラスのメンバーを表示できます。

[![ オブジェクトブラウザーを表示しています](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

上のスクリーンショットでは、例のアプリが呼び出す2つの `TextAnalyzer` メソッドが強調表示されています。 `NumConsonants` (基になる Java `numConsonants` メソッドをラップします)、および `NumVowels` (基になる Java `numVowels` メソッドをラップ) です。

### <a name="accessing-aar-types"></a>しよう.AAR 型

バインドライブラリを参照するアプリへの参照を追加すると、で Java 型にアクセスできるようになります。( C#ラッパーに感謝) C#型にアクセスする場合と同様に AAR。 C#アプリコードは、次の例に示すように `TextAnalyzer` メソッドを呼び出すことができます。

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

上の例では、`TextCounter` クラスで静的メソッドを呼び出しています。 ただし、クラスをインスタンス化して、インスタンスメソッドを呼び出すこともできます。 たとえば、の場合はです。AAR は、インスタンスメソッド `buildFullName` を持つ `Employee` というクラスをラップします。 `MyClass` をインスタンス化して、次のように使用することができます。

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

次の手順では、ユーザーにテキストの入力を求めるコードをアプリに追加し、`TextCounter` を使用してテキストを分析し、結果を表示します。

**Bindingtest**レイアウト (メインの**axml**) を次の XML に置き換えます。 このレイアウトには、テキスト入力の場合は `EditText` と、母音と子音のカウントを開始する場合は2つのボタンがあります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation             ="vertical"
    android:layout_width            ="fill_parent"
    android:layout_height           ="fill_parent" >
    <TextView
        android:text                ="Text to analyze:"
        android:textSize            ="24dp"
        android:layout_marginTop    ="30dp"
        android:layout_gravity      ="center"
        android:layout_width        ="wrap_content"
        android:layout_height       ="wrap_content" />
    <EditText
        android:id                  ="@+id/input"
        android:text                ="I can use my .AAR file from C#!"
        android:layout_marginTop    ="10dp"
        android:layout_gravity      ="center"
        android:layout_width        ="300dp"
        android:layout_height       ="wrap_content"/>
    <Button
        android:id                  ="@+id/vowels"
        android:layout_marginTop    ="30dp"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Vowels" />
    <Button
        android:id                  ="@+id/consonants"
        android:layout_width        ="240dp"
        android:layout_height       ="wrap_content"
        android:layout_gravity      ="center"
        android:text                ="Count Consonants" />
</LinearLayout>
```

**MainActivity.cs**の内容を次のコードに置き換えます。 この例に示すように、ボタンイベントハンドラーの呼び出しは、に格納されている `TextCounter` メソッドをラップしています。AAR を使用し、結果を表示するためにトーストを使用します。 バインドされたライブラリの名前空間の `using` ステートメントに注意してください (この場合は、`Com.Xamarin.Textcounter`)。

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using Android.Views.InputMethods;
using Com.Xamarin.Textcounter;

namespace BindingTest
{
    [Activity(Label = "BindingTest", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        InputMethodManager imm;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            imm = (InputMethodManager)GetSystemService(Context.InputMethodService);

            var vowelsBtn = FindViewById<Button>(Resource.Id.vowels);
            var consonBtn = FindViewById<Button>(Resource.Id.consonants);
            var edittext = FindViewById<EditText>(Resource.Id.input);
            edittext.InputType = Android.Text.InputTypes.TextVariationPassword;

            edittext.KeyPress += (sender, e) =>
            {
                imm.HideSoftInputFromWindow(edittext.WindowToken, HideSoftInputFlags.NotAlways);
                e.Handled = true;
            };

            vowelsBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumVowels(edittext.Text);
                string msg = count + " vowels found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

            consonBtn.Click += (sender, e) =>
            {
                int count = TextCounter.NumConsonants(edittext.Text);
                string msg = count + " consonants found.";
                Toast.MakeText (this, msg, ToastLength.Short).Show ();
            };

        }
    }
}
```

**Bindingtest**プロジェクトをコンパイルして実行します。 アプリが起動し、左側にスクリーンショットが表示されます (`EditText` はいくつかのテキストで初期化されますが、それをタップして変更できます)。 [母音の**カウント**] をタップすると、右側に示されている母音の数がトーストによって表示されます。

[![Bindingtest の実行中のスクリーンショット](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

**COUNT 子音**ボタンをタップしてみてください。 また、テキストの行を変更し、これらのボタンをもう一度タップして、さまざまな母音と子音のカウントをテストすることもできます。

### <a name="accessing-aar-resources"></a>しよう.AAR リソース

Xamarin ツールは、から**R**データをマージします。アプリの**リソース**クラスに AAR します。 このため、にアクセスできます。プロジェクトの**resources**パスにあるリソースにアクセスするのと同じように、レイアウト (および分離コード) からリソースを AAR します。

イメージリソースにアクセスするには、内でパックされたイメージに対して、**リソース**の作成可能な名前を使用します。AAR. たとえば、で**image.png**を参照できます。`@drawable/image` を使用した AAR ファイル:

```xml
<ImageView android:src="@drawable/image" ... />
```

に格納されているリソースレイアウトにアクセスすることもできます。AAR. これを行うには、内にパッケージ化されたレイアウトに対して、**リソースのレイアウト**名を使用します。AAR. 以下に例を示します。

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**Aar**の例には、 **res//サル**に存在するイメージファイルが含まれています。 このイメージリソースにアクセスして、サンプルアプリで使用してみましょう。

**Bindingtest**レイアウト (メインの**axml**) を編集し、`LinearLayout` コンテナーの末尾に `ImageView` を追加します。 この `ImageView` は、 **\@drawable/monkey**で見つかったイメージを表示します。このイメージは、 **textanalyzer. aar**のリソースセクションから読み込まれます。

```xml
    ...
    <ImageView
        android:src                 ="@drawable/monkey"
        android:layout_marginTop    ="40dp"
        android:layout_width        ="200dp"
        android:layout_height       ="200dp"
        android:layout_gravity      ="center" />

</LinearLayout>
```

**Bindingtest**プロジェクトをコンパイルして実行します。 アプリが起動し、左側にスクリーンショットが表示され &ndash; **[COUNT 子音]** をタップすると、結果が右側に表示されます。

[![ Bindingtest が子音のカウントを表示しています](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)

おめでとうございます! Java ライブラリが正常にバインドされました。AAR!

## <a name="summary"></a>まとめ

このチュートリアルでは、のバインドライブラリを作成しました。AAR ファイルを使用して、バインドライブラリを最小テストアプリに追加し、アプリを実行しC#て、コードが内に存在する Java コードを呼び出すことができることを確認します。AAR ファイル。
さらに、に格納されているイメージリソースにアクセスして表示するようにアプリを拡張しています。AAR ファイル。

## <a name="related-links"></a>関連リンク

- [Java バインドライブラリの構築 (ビデオ)](https://university.xamarin.com/classes#10090)
- [.JAR のバインド](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [AarBinding (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/javaintegration-aarbinding)
- [バグ 44573-1 つのプロジェクトで複数の aar ファイルをバインドできない](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
