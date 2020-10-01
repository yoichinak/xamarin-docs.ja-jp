---
title: .AAR のバインド
description: このチュートリアルでは、Android .AAR ファイルから Xamarin.Android Java バインド ライブラリを作成する詳しい手順について説明します。
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 489408400a7a900bf867a4303188cdc927020f7f
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91454469"
---
# <a name="binding-an-aar"></a>.AAR のバインド

> [!IMPORTANT]
> 現在、Xamarin プラットフォームでのカスタム バインディングの使用を調査しています。 今後の開発作業の発展のために、この[**アンケート**](https://www.surveymonkey.com/r/KKBHNLT)にご回答ください。

_このチュートリアルでは、Android .AAR ファイルから Xamarin.Android Java バインド ライブラリを作成する詳しい手順について説明します。_

## <a name="overview"></a>概要

"*Android アーカイブ (.AAR)* " ファイルは、Android ライブラリのファイル形式です。
.AAR ファイルは、次のものを含む .ZIP アーカイブです。

- コンパイル済みの Java コード
- リソース ID
- リソース
- メタデータ (アクティビティ宣言、アクセス許可など)

このガイドでは、1 つの .AAR ファイルに対するバインド ライブラリを作成するための基本について説明します。 一般的な Java ライブラリのバインドの概要 (基本的なコード例を含む) については、「[Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)」を参照してください。

> [!IMPORTANT]
> バインド プロジェクトに含めることができる .AAR ファイルは 1 つだけです。 .AAR が他の .AAR に依存している場合、これらの依存関係を独自のバインド プロジェクトに含めてから参照する必要があります。 「[バグ 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)」を参照してください。

## <a name="walkthrough"></a>チュートリアル

Android Studio で作成されたサンプルの Android アーカイブ ファイル [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) のバインド ライブラリを作成します。 この .AAR には、文字列の母音と子音の数をカウントする静的メソッドを含む `TextCounter` クラスが含まれています。 さらに、**textanalyzer.aar** には、カウント結果の表示に役立つ画像リソースが含まれています。

次の手順を使用して、.AAR ファイルからバインド ライブラリを作成します。

1. 新しい Java バインド ライブラリ プロジェクトを作成します。

2. そのプロジェクトに .AAR ファイルを 1 つ追加します。 バインド プロジェクトに含めることができる .AAR は 1 つだけです。

3. .AAR ファイルに適切なビルド アクションを設定します。

4. .AAR でサポートされるターゲット フレームワークを選択します。

5. バインド ライブラリをビルドします。

バインド ライブラリを作成したら、小規模な Android アプリを開発します。これは、ユーザーにテキスト文字列の入力を要求し、.AAR メソッドを呼び出してテキストを分析し、.AAR から画像を取得して、その画像と共に結果を表示します。

サンプル アプリは、**textanalyzer.aar** の `TextCounter` クラスにアクセスします。

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

さらに、このサンプル アプリは、**textanalyzer.aar** にパッケージ化された画像リソースを取得して表示します。

[![Xamarin のサルの画像](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

この画像リソースは **textanalyzer.aar** の **res/drawable/monkey.png** にあります。

### <a name="creating-the-bindings-library"></a>バインド ライブラリの作成

以下の手順を開始する前に、サンプルの [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android アーカイブ ファイルをダウンロードしてください。

1. Android バインド ライブラリ テンプレートから開始して、新しいバインド ライブラリ プロジェクトを作成します。 Visual Studio for Mac または Visual Studio のいずれかを使用できます (以下のスクリーンショットは Visual Studio を示していますが、Visual Studio for Mac もほぼ同じです)。 ソリューションに **AarBinding** という名前を付けます。

    [![AarBindings プロジェクトを作成する](binding-an-aar-images/01-new-bindings-library-vs-sml.w160.png)](binding-an-aar-images/01-new-bindings-library-vs.w160.png#lightbox)

2. テンプレートには、**Jars** フォルダーが含まれています。ここで、バインド ライブラリ プロジェクトに対して .AAR を追加します。 **Jars** フォルダーを右クリックし、 **[追加] > [既存の項目]** の順に選択します。

    [![既存の項目を追加する](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)

3. 前にダウンロードした **textanalyzer.aar** に移動し、それを選択して **[追加]** をクリックします。

    [![textanalayzer.aar を追加する](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)

4. **textanalyzer.aar** ファイルがプロジェクトに正常に追加されたことを確認します。

    [![textanalyzer.aar ファイルが追加された](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5. **textanalyzer.aar** の [ビルド アクション] を `LibraryProjectZip` に設定します。 Visual Studio for Mac では、**textanalyzer.aar** を右クリックして、ビルド アクションを設定します。 Visual Studio では、 **[プロパティ]** ペインでビルド アクションを設定できます。

    [![textanalyzer.aar のビルド アクションを LibraryProjectZip に設定](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6. プロジェクトのプロパティを開いて、"*ターゲット フレームワーク*" を構成します。 .AAR で任意の Android API が使用される場合、ターゲット フレームワークを .AAR で想定される API レベルに設定します。 (一般的なターゲット フレームワークの設定および Android API レベルの詳細については、「[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)」を参照してください。)

    バインド ライブラリのターゲット API レベルを設定します。 次の例では、**textanalyzer** は Android API に対して依存関係を持たないため、最新のプラットフォーム API レベル (API レベル 23) を使用できます。

    [![ターゲット レベルを API 23 に設定する](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7. バインド ライブラリをビルドします。 バインド ライブラリ プロジェクトが正常にビルドされ、次の場所に出力 .DLL が生成されます。**AarBinding/bin/Debug/AarBinding.dll**

### <a name="using-the-bindings-library"></a>バインド ライブラリの使用

Xamarin.Android アプリでこの .DLL を使用するには、まずバインド ライブラリへの参照を追加する必要があります。 次の手順に従います。

1. このチュートリアルを簡略化するため、このアプリをバインド ライブラリと同じソリューションに作成します。 (バインド ライブラリを使用するアプリは、別のソリューションに置くこともできます。)新しい Xamarin.Android アプリを作成します。ソリューションを右クリックし、 **[新しいプロジェクトの追加]** を選択します。 新しいプロジェクトに **BindingTest** という名前を付けます。

    [![新しい BindingTest プロジェクトを作成する](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2. **BindingTest** プロジェクトの **[参照]** ノードを右クリックし、 **[参照の追加]** を選択します。

    [![[参照の追加] をクリックする](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3. 先ほど作成した **AarBinding** プロジェクトを選択し、 **[OK]** をクリックします。

    [![AAR バインド プロジェクトにチェックを入れる](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4. **BindingTest** プロジェクトの **[参照]** ノードを開き、**AarBinding** 参照が存在することを確認します。

    [![AarBinding が [参照] の下に一覧表示されている](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)

バインド ライブラリ プロジェクトの内容を表示する場合は、この参照をダブルクリックして **[オブジェクト ブラウザー]** で開くことができます。 (Java `com.xamarin.textanalyzezr` パッケージからマップされた) `Com.Xamarin.Textcounter` 名前空間のマップされた内容を確認し、`TextCounter` クラスのメンバーを表示できます。

[![オブジェクト ブラウザーの表示](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

上のスクリーンショットでは、サンプル アプリが呼び出す 2 つの `TextAnalyzer` メソッド、`NumConsonants` (基になる Java `numConsonants` メソッドをラップします) と `NumVowels` (基になる Java `numVowels` メソッドをラップします) が強調表示されています。

### <a name="accessing-aar-types"></a>.AAR 型へのアクセス

バインド ライブラリを指すアプリへの参照を追加すると、(C# ラッパーによって) C# 型にアクセスする場合と同様に .AAR 内の Java 型にアクセスできます。 C# アプリ コードで、次の例に示すように `TextAnalyzer` メソッドを呼び出すことができます。

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

上記の例では、`TextCounter` クラスの静的メソッドを呼び出しています。 しかし、クラスをインスタンス化して、インスタンス メソッドを呼び出すこともできます。 たとえば、インスタンス メソッド `buildFullName` を含む `Employee` というクラスを .AAR がラップする場合は、`MyClass` をインスタンス化して、次に示すように使用できます。

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

次の手順では、コードをアプリに追加し、それがユーザーにテキストの入力を要求し、`TextCounter` を使用してそのテキストを分析し、結果を表示するようにします。

**BindingTest** レイアウト (**Main.axml**) を次の XML に置き換えます。 このレイアウトには、テキスト入力用の `EditText` と、母音と子音のカウントを開始するための 2 つのボタンがあります。

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

**MainActivity.cs** の内容を次のコードに置き換えます。 この例に示すように、ボタン イベント ハンドラーは、.AAR に存在するラップされた `TextCounter` メソッドを呼び出し、トーストを使用して結果を表示します。 バインドされたライブラリの名前空間の `using` ステートメント (この例では `Com.Xamarin.Textcounter`) に注目してください。

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

**BindingTest** プロジェクトをコンパイルして実行します。 アプリが起動し、左側のスクリーンショットのように表示されます (`EditText` は何らかのテキストで初期化されますが、それをタップして変更できます)。 **[COUNT VOWELS]** をタップすると、右側に示すように、トーストによって母音の数が表示されます。

[![BindingTest の実行のスクリーンショット](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

**[COUNT CONSONANTS]** ボタンをタップしてみます。 また、テキスト行を変更し、これらのボタンをもう一度タップして、さまざまな母音と子音のカウントをテストすることもできます。

### <a name="accessing-aar-resources"></a>.AAR リソースへのアクセス

Xamarin ツールは、.AAR からの **R** データをアプリの **Resources** クラスにマージします。 このため、プロジェクトの **Resources** パスにあるリソースにアクセスする場合と同じように、レイアウト (およびコードビハインド) から .AAR リソースにアクセスできます。

画像リソースにアクセスするには、.AAR 内でパックされた画像の **Resource.Drawable** 名を使用します。 たとえば、`@drawable/image` を使用して、.AAR ファイル内の **image.png** を参照できます。

```xml
<ImageView android:src="@drawable/image" ... />
```

.AAR に存在するリソース レイアウトにアクセスすることもできます。 これを行うには、.AAR 内にパッケージ化されたレイアウトの **Resource.Layout** 名を使用します。 次に例を示します。

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**textanalyzer.aar** の例には、**res/drawable/monkey.png** に存在する画像ファイルが含まれています。 この画像リソースにアクセスして、サンプル アプリで使用してみましょう。

**BindingTest** レイアウト (**Main.axml**) を編集し、`LinearLayout` コンテナーの末尾に `ImageView` を追加します。 この `ImageView` により、 **\@drawable/monkey** で見つかった画像が表示されます。この画像は **textanalyzer.aar** のリソース セクションから読み込まれます。

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

**BindingTest** プロジェクトをコンパイルして実行します。 アプリが起動し、左側のスクリーンショットのように表示されます。 **[COUNT CONSONANTS]** をタップすると、右側に示すように結果が表示されます。

[![子音のカウントを表示する BindingTest](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)

おめでとうございます! Java ライブラリ .AAR が正常にバインドされました。

## <a name="summary"></a>まとめ

このチュートリアルでは、.AAR ファイルのバインド ライブラリを作成して、最小限のテスト アプリにバインド ライブラリを追加し、アプリを実行して C# コードが .AAR ファイル内にある Java コードを呼び出せることを確認しました。
さらに、.AAR ファイルに存在する画像リソースにアクセスして表示するようにアプリを拡張しました。

## <a name="related-links"></a>関連リンク

- [Java バインド ライブラリのビルド (ビデオ)](https://university.xamarin.com/classes#10090)
- [.JAR のバインド](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [AarBinding (サンプル)](/samples/xamarin/monodroid-samples/javaintegration-aarbinding)
- [バグ 44573 - 1 つのプロジェクトで複数の .aar ファイルをバインドできない](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)