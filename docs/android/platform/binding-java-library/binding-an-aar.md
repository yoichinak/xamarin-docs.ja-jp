---
title: "バインドにします。[AAR]"
description: "このチュートリアルでは、Android から Xamarin.Android Java バインド ライブラリを作成するための手順を説明します。[Aar] ファイルです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: ae209f8099925cc160e16cb5365625e48e6c384d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="binding-an-aar"></a>バインドにします。[AAR]

_このチュートリアルでは、Android から Xamarin.Android Java バインド ライブラリを作成するための手順を説明します。[Aar] ファイルです。_


## <a name="overview"></a>概要

*Android アーカイブ (です。[Aar])*ファイルは、Android ライブラリのファイル形式。
.[Aar] のファイルは、します。以下を含む ZIP アーカイブ。

-   コンパイル済みの Java コード
-   リソース Id
-   リソース
-   メタデータ (アクティビティ宣言、アクセス許可など)

このガイドでは 1 つのバインドのライブラリの作成の基本おステップをします。[Aar] ファイルです。 Java ライブラリ バインドの一般 (基本的なコード例では) の概要については、次を参照してください。[バインド Java ライブラリ](~/android/platform/binding-java-library/index.md)です。


> [!IMPORTANT]
> バインディングのプロジェクトでは、1 つ含めることができますのみです。[Aar] ファイルです。 場合、します。[Aar] 他の依存関係。AAR、し、それらの依存関係を独自のバインディング プロジェクトに含まれているされ、参照する必要があります。 参照してください[バグの 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)です。


## <a name="walkthrough"></a>チュートリアル

Android のアーカイブ ファイルをたとえばが Android Studio で、作成されたバインディング ライブラリを作成[textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true)です。 これ。[Aar] が含まれています、`TextCounter`母音と文字列の子音の数をカウントする静的メソッドを持つクラス。 さらに、 **textanalyzer.aar**カウントの結果を表示するイメージ リソースが含まれています。

次の手順を使用してからバインド ライブラリを作成お、します。[Aar] ファイル:

1. 新しい Java バインド ライブラリ プロジェクトを作成します。

2. 1 つを追加します。[Aar] ファイルをプロジェクト。 バインド プロジェクトには、1 つのみを含めることがあります。[AAR] です。

3. 適切なビルド アクションを設定します。[Aar] ファイルです。

4. ターゲット フレームワークを選択します。[Aar] をサポートします。

5. バインドのライブラリをビルドします。

バインドのライブラリで作成した後は、呼び出しのテキスト文字列の入力を求めます小規模の Android アプリを作成おします。テキストを分析する AAR メソッドがからイメージを取得します。[Aar]、および画像と共に結果が表示されます。

サンプル アプリのアクセスは、`TextCounter`のクラス**textanalyzer.aar**:

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

さらに、このサンプル アプリでは取得し、パッケージ化されている画像リソースを表示**textanalyzer.aar**:

[![Xamarin サル イメージ](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

このイメージ リソースが存在する**res/drawable/monkey.png**で**textanalyzer.aar**です。



### <a name="creating-the-bindings-library"></a>バインドのライブラリを作成します。

下記の手順を開始する前に、例をダウンロードしてください。 [textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android アーカイブ ファイル。

1.  以降、Android のバインドのライブラリ テンプレートで新しいバインドのライブラリ プロジェクトを作成します。 Visual Studio for Mac または Visual Studio (次のスクリーン ショットに表示する、Visual Studio ですが Visual Studio for Mac とよく似ています) を使用することができます。 ソリューションの名前を付けます**AarBinding**:

    [![AarBindings プロジェクトを作成します。](binding-an-aar-images/01-new-bindings-library-vs-sml.png)](binding-an-aar-images/01-new-bindings-library-vs.png#lightbox)

2.  テンプレートに含まれる、 **Jar**フォルダーを追加する場所、します。バインドのライブラリ プロジェクトへ AAR(s) です。 右クリックし、 **Jar**フォルダーと選択**追加 > 既存の項目**:

    [![既存の項目を追加する](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  移動し、 **textanalyzer.aar**ファイルは事前にダウンロードを選択し、をクリックして**追加**:

    [![Textanalayzer.aar を追加します。](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  いることを確認、 **textanalyzer.aar**プロジェクトにファイルを正常に追加します。

    [![Textanalyzer.aar ファイルが追加されました](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  ビルド アクションは、設定**textanalyzer.aar**に`LibraryProjectZip`です。 Mac 用 Visual Studio で、右クリック**textanalyzer.aar**ビルド アクションを設定します。 Visual Studio では、ビルド アクション 設定することができます、**プロパティ**ペイン)。

    [![LibraryProjectZip に textanalyzer.aar ビルド アクションを設定します。](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  プロジェクトを構成するプロパティを開き、*ターゲット フレームワーク*です。 場合、します。[Aar] は、Android Api を使用して、API レベルにターゲット フレームワークを設定します。[Aar] が必要です。 (ターゲット フレームワークの設定と全般の Android API レベルの詳細については、次を参照してください[Android API レベルの理解](~/android/app-fundamentals/android-api-levels.md)。)。

    バインド ライブラリのターゲット API レベルを設定します。 この例では、ために、最新のプラットフォームでは、API レベル (API level 23) を使用するために解放は、 **textanalyzer** Android Api に依存関係はありません。

    [![ターゲット レベル API 23 を設定します。](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  バインドのライブラリをビルドします。 バインドのライブラリ プロジェクトは、正常にビルドされ、出力が生成する必要があります。次の場所に DLL: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>バインドのライブラリを使用します。

これを使用します。Xamarin.Android アプリ内の DLL は、最初バインド ライブラリへの参照を追加する必要があります。 次の手順を使用します。

1.  このチュートリアルを簡略化するライブラリをバインドと同じソリューションでこのアプリを作成しています。 (バインド ライブラリを使用するアプリでしたにも置くを別のソリューションです。)Xamarin.Android アプリを新規作成: ソリューションを右クリックし **新しいプロジェクトの追加**です。 新しいプロジェクトの名前**BindingTest**:

    [![新しい BindingTest プロジェクトを作成します。](binding-an-aar-images/07-add-new-project-vs-sml.png)](binding-an-aar-images/07-add-new-project-vs.png#lightbox)

2.  右クリックし、**参照**のノード、 **BindingTest**プロジェクトし、選択**の参照を追加しています.**:

    [![参照を追加します。](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  選択、 **AarBinding**先ほど作成したプロジェクトとクリック**OK**:

    [![[Aar] バインド プロジェクトをチェック アウトします。](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  開く、**参照**のノード、 **BindingTest**ことを確認するプロジェクト、 **AarBinding**参照が存在します。

    [![AarBinding 参照に表示されます。](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


開くへの参照をダブルクリックする場合は、バインドのライブラリ プロジェクトの内容を表示するには、**オブジェクト ブラウザー**です。 マップされたコンテンツを表示できる、`Com.Xamarin.Textcounter`名前空間 (Java からマップされた`com.xamarin.textanalyzezr`パッケージ) のメンバーを表示して、`TextCounter`クラス。

[![オブジェクト ブラウザーの表示](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

上のスクリーン ショットには、2 つが強調表示`TextAnalyzer`例のアプリが呼び出すメソッド: `NumConsonants` (をラップする、基になる Java`numConsonants`メソッド)、および`NumVowels`(をラップする、基になる Java`numVowels`メソッド)。



### <a name="accessing-aar-types"></a>アクセスします。[Aar] の種類

バインドのライブラリを指すアプリへの参照を追加すると後で Java 型にアクセスできます、します。[Aar] したのとは c# (c# ラッパー) ご協力に感謝の型にアクセスします。 C# アプリ コードを呼び出すことができます`TextAnalyzer`メソッドに次の例で示すようにします。

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

上記の例の静的メソッドを呼んでいる、`TextCounter`クラスです。 ただしもクラスのインスタンスを作成してインスタンス メソッドを呼び出してください。 たとえば場合、します。[Aar] と呼ばれるクラスをラップする`Employee`インスタンス メソッドを持つ`buildFullName`、インスタンス化できる`MyClass`異なりを使用して、ここで。

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

次の手順は、求めるメッセージがテキストで使用できるように、アプリにコードを追加`TextCounter`テキスト、および、表示結果を分析します。

置換、 **BindingTest**レイアウト (**Main.axml**) を次の XML です。 このレイアウトには、`EditText`テキスト入力と母音と子音カウントを開始するための 2 つのボタン。

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

内容を置き換える**MainActivity.cs**を次のコード。 この例からわかるように、ボタンのイベント ハンドラーの呼び出しがラップ`TextCounter`メソッド内に存在する、します。結果を表示するトースト AAR しを使用します。 通知、`using`バインドされているライブラリの名前空間のステートメント (この場合、 `Com.Xamarin.Textcounter`)。

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

コンパイルし、実行、 **BindingTest**プロジェクト。 アプリが起動し、左側のスクリーン ショットを提示 (、`EditText`はいくつかのテキストで初期化を変更することをタップすることができますが、)。 タップすると**カウント母音**トーストが右側のように、母音の数を表示します。

[![実行中の BindingTest からスクリーン ショット](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

タップしてください、**カウント子音**ボタンをクリックします。 また、行のテキストを変更して別の母音をテストするには、もう一度これらのボタンをタップして、子音カウントします。



### <a name="accessing-aar-resources"></a>アクセスします。[Aar] リソース

マージのツールを Xamarin、 **R**からデータをします。[Aar] に、アプリの**リソース**クラスです。 その結果、次のようにアクセスすることができます。[Aar] リソースのレイアウト (および分離コード) と同じ方法では、リソースにアクセスするように、**リソース**プロジェクトのパス。

使用するイメージ リソースにアクセスするには**Resource.Drawable**イメージ内にパックの名前は、します。[AAR] です。 たとえば、参照**image.png**でします。[Aar] ファイルを使用して`@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

内に存在するリソースのレイアウトをアクセスすることも、します。[AAR] です。 これを行うには、使用する、 **Resource.Layout**内部にパッケージ化レイアウトの名前、します。[AAR] です。 例:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**Textanalyzer.aar**の例には、イメージ ファイルに存在するが含まれています。 **res/drawable/monkey.png**です。 このイメージのリソースにアクセスして、例のアプリで使用してみましょう。

編集、 **BindingTest**レイアウト (**Main.axml**) を追加し、`ImageView`の末尾に、`LinearLayout`コンテナーです。 これは、`ImageView`で見つかったイメージが表示されます **@drawable/monkey** ; のリソース セクションからこのイメージが読み込まれます**textanalyzer.aar**:

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

コンパイルし、実行、 **BindingTest**プロジェクト。 アプリが起動し、左側のスクリーン ショットを提示&ndash;タップすると**カウント子音**右に示すように、結果が表示されます。

[![BindingTest 子音カウントを表示します。](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


おめでとうございます!  Java ライブラリを正常に連結されています。[AAR] です。



## <a name="summary"></a>まとめ

このチュートリアルでのバインドのライブラリを作成しました、します。AAR ファイル バインド ライブラリ アプリに追加して、最小限のテストと、c# コードが内に存在する Java コードを呼び出すことを確認するアプリを実行します。[Aar] ファイルです。
アクセスしてに存在するイメージ リソースを表示するアプリをさらに、拡張は、します。[Aar] ファイルです。


## <a name="related-links"></a>関連リンク

- [Java バインド ライブラリ (ビデオ) のビルド](https://university.xamarin.com/classes#10090)
- [.JAR のバインド](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [AarBinding (サンプル)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [バグ 44573 1 つのプロジェクトは複数の .aar ファイルをバインドできません。](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
