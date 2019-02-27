---
title: バインドします。AAR
description: このチュートリアルでは、Android から Xamarin.Android Java バインド ライブラリを作成する手順が説明します。AAR ファイルです。
ms.prod: xamarin
ms.assetid: 380413B8-6A99-4BB8-B64C-3EAF9F359C22
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/11/2018
ms.openlocfilehash: 7f71ccf4ff61c176e73be6d3855136697a5c2130
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103999"
---
# <a name="binding-an-aar"></a>バインドします。AAR

_このチュートリアルでは、Android から Xamarin.Android Java バインド ライブラリを作成する手順が説明します。AAR ファイルです。_


## <a name="overview"></a>概要

*Android アーカイブ (します。AAR)* ファイルは、Android ライブラリのファイル形式。
します。AAR ファイルは、します。以下を含む ZIP アーカイブ:

-   コンパイル済みの Java コード
-   リソース Id
-   リソース
-   メタ データ (たとえば、アクティビティの宣言、アクセス許可)

このガイドでは、1 つのバインド ライブラリの作成の基礎を手順説明します。AAR ファイルです。 Java ライブラリによるバインディングの一般 (基本的なコード例では) の概要については、次を参照してください。 [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)します。


> [!IMPORTANT]
> バインド プロジェクトは 1 つのみ含めることができます。AAR ファイルです。 場合、します。他の依存関係を AAR します。AAR、し、これらの依存関係を独自のバインド プロジェクトに含まれているし、参照する必要があります。 参照してください[バグ 44573](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)します。


## <a name="walkthrough"></a>チュートリアル

Android アーカイブ ファイル例は、Android Studio で、作成されたために、バインド ライブラリを作成します[textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true)します。 これ。AAR が含まれています、`TextCounter`母音と文字列の子音の数をカウントする静的メソッドを持つクラス。 さらに、 **textanalyzer.aar**カウントの結果を表示するイメージ リソースが含まれています。

次の手順を使用してからバインド ライブラリを作成します。AAR ファイル:

1. 新しい Java バインド ライブラリ プロジェクトを作成します。

2. 1 つを追加します。AAR ファイルをプロジェクト。 バインド プロジェクトには、1 つのみを含めることがあります。AAR します。

3. 適切なビルド アクションを設定します。AAR ファイルです。

4. ターゲット フレームワークを選択します。AAR をサポートします。

5. バインド ライブラリをビルドします。

バインド ライブラリを作成したらの呼び出しのテキスト文字列をユーザーに求めます小さい Android アプリを開発します。AAR メソッド、テキストの分析からイメージを取得します。AAR、および画像と共に結果が表示されます。

サンプル アプリのアクセスは、`TextCounter`クラスの**textanalyzer.aar**:

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

さらに、このサンプル アプリを取得し、でパッケージ化されている画像リソースの表示は**textanalyzer.aar**:

[![Xamarin monkey イメージ](binding-an-aar-images/00-monkey-sml.png)](binding-an-aar-images/00-monkey.png#lightbox)

このイメージ リソースが存在する**res/drawable/monkey.png**で**textanalyzer.aar**します。



### <a name="creating-the-bindings-library"></a>バインド ライブラリを作成します。

次の手順を開始する前に、例をダウンロードしてください[textanalyzer.aar](https://github.com/xamarin/monodroid-samples/blob/master/JavaIntegration/AarBinding/Resources/textanalyzer.aar?raw=true) Android アーカイブ ファイル。

1.  以降、Android バインド ライブラリ テンプレートで新しいバインド ライブラリ プロジェクトを作成します。 Visual Studio for Mac または Visual Studio (次のスクリーン ショットに表示する、Visual Studio ですが Visual Studio for Mac は非常に似ています) のいずれかを使用することができます。 ソリューションの名前を**AarBinding**:

    [![AarBindings プロジェクトを作成します。](binding-an-aar-images/01-new-bindings-library-vs-sml.w157.png)](binding-an-aar-images/01-new-bindings-library-vs.w157.png#lightbox)

2.  テンプレートに含まれる、 **Jars**フォルダーを追加する、します。バインド ライブラリ プロジェクトに AAR(s) します。 右クリックし、 **Jars**フォルダーと選択**追加 > 既存項目の**:

    [![既存の項目を追加する](binding-an-aar-images/02-add-existing-item-vs-sml.png)](binding-an-aar-images/02-add-existing-item-vs.png#lightbox)


3.  移動し、 **textanalyzer.aar**以前にダウンロードしたファイルを選択し、 をクリックして**追加**:

    [![Textanalayzer.aar を追加します。](binding-an-aar-images/03-select-aar-file-vs-sml.png)](binding-an-aar-images/03-select-aar-file-vs.png#lightbox)


4.  いることを確認、 **textanalyzer.aar**ファイルがプロジェクトに正常に追加します。

    [![Textanalyzer.aar ファイルが追加されました](binding-an-aar-images/04-aar-added-vs-sml.png)](binding-an-aar-images/04-aar-added-vs.png#lightbox)

5.  ビルド アクションは、設定**textanalyzer.aar**に`LibraryProjectZip`します。 Visual Studio for Mac では、右クリックして**textanalyzer.aar**ビルド アクションを設定します。 Visual Studio でビルド アクションで設定できる、**プロパティ**ウィンドウ)。

    [![LibraryProjectZip textanalyzer.aar ビルド アクションに設定](binding-an-aar-images/05-embedded-aar-vs-sml.png)](binding-an-aar-images/05-embedded-aar-vs.png#lightbox)

6.  プロジェクトを構成するプロパティを開き、*ターゲット フレームワーク*します。 場合、します。AAR は Android API を使用して、API レベルにターゲット フレームワークを設定します。AAR が必要です。 (ターゲット フレームワークの設定と一般的な Android API レベルの詳細については、次を参照してください[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md)。)。

    バインド ライブラリのターゲット API レベルを設定します。 この例では自由にために API レベル (API レベル 23) の最新のプラットフォームを使用して、 **textanalyzer** Android API に依存関係はありません。

    [![ターゲット レベル API 23 を設定します。](binding-an-aar-images/06-set-target-framework-vs-sml.png)](binding-an-aar-images/06-set-target-framework-vs.png#lightbox)

7.  バインド ライブラリをビルドします。 バインド ライブラリ プロジェクトは正常にビルドされ、出力が生成する必要があります。次の場所に DLL: **AarBinding/bin/Debug/AarBinding.dll**



### <a name="using-the-bindings-library"></a>バインド ライブラリを使用します。

これを使用します。Xamarin.Android アプリで DLL をバインディング ライブラリへの参照を追加する必要があります最初。 次の手順に従います。

1.  このチュートリアルを簡略化するためにバインディング ライブラリと同じソリューションでこのアプリを作成しています。 (バインド ライブラリを使用するアプリでしたも存在する別のソリューション。)新しい Xamarin.Android アプリを作成します。 ソリューションを右クリックして**新しいプロジェクトの追加**。 新しいプロジェクトの名前**BindingTest**:

    [![新しい BindingTest プロジェクトを作成します。](binding-an-aar-images/07-add-new-project-vs-sml.w157.png)](binding-an-aar-images/07-add-new-project-vs.w157.png#lightbox)

2.  右クリックし、**参照**のノード、 **BindingTest**順に選択して**参照の追加.**:

    [![参照を追加します。](binding-an-aar-images/08-add-reference-vs-sml.png)](binding-an-aar-images/08-add-reference-vs.png#lightbox)

3.  選択、 **AarBinding**前に作成したプロジェクトをクリックします**OK**:

    [![AAR バインド プロジェクトを確認してください。](binding-an-aar-images/09-choose-aar-binding-vs-sml.png)](binding-an-aar-images/09-choose-aar-binding-vs.png#lightbox)

4.  開く、**参照**のノード、 **BindingTest**ことを確認するプロジェクト、 **AarBinding**参照が存在します。

    [![参照 AarBinding が一覧表示します。](binding-an-aar-images/10-references-shows-aarbinding-vs-sml.png)](binding-an-aar-images/10-references-shows-aarbinding-vs.png#lightbox)


バインド ライブラリ プロジェクトの内容を表示する場合で開くへの参照をダブルクリックすることができます、**オブジェクト ブラウザー**します。 マップされた内容を確認できます、`Com.Xamarin.Textcounter`名前空間 (Java からマップされた`com.xamarin.textanalyzezr`パッケージ) のメンバーを表示して、`TextCounter`クラス。

[![オブジェクト ブラウザーの表示](binding-an-aar-images/11-object-browser-vs-sml.png)](binding-an-aar-images/11-object-browser-vs.png#lightbox)

上記のスクリーン ショットには、2 つが強調表示されます。`TextAnalyzer`例のアプリが呼び出すメソッド: `NumConsonants` (ラップしています。 基になる Java`numConsonants`メソッド)、および`NumVowels`(ラップしています。 基になる Java`numVowels`メソッド)。



### <a name="accessing-aar-types"></a>アクセスします。AAR 型

バインド ライブラリを指すアプリへの参照を追加すると、Java 型にアクセスできるようにします。AAR するとしてアクセスC#型 (方々 に感謝します、C#ラッパー)。 C#アプリのコードを呼び出すことができます`TextAnalyzer`メソッドを次の例に示すようにします。

```csharp
using Com.Xamarin.Textcounter;
...
int numVowels = TextCounter.NumVowels (myText);
int numConsonants = TextCounter.NumConsonants (myText);
```

上記の例静的メソッドを呼び出しています、`TextCounter`クラス。 ただし、クラスのインスタンスを作成し、インスタンス メソッドを呼び出すことできますも。 たとえば場合、します。AAR というクラスをラップする`Employee`インスタンス メソッドを持つ`buildFullName`、インスタンス化できます`MyClass`を使用して、表示は、ここ。

```csharp
var employee = new Com.MyCompany.MyProject.Employee();
var name = employee.BuildFullName ();
```

次の手順では使用して、テキストのユーザーに促すようにアプリにコードを追加`TextCounter`テキスト、および、表示結果を分析します。

置換、 **BindingTest**レイアウト (**Main.axml**) 次の XML を使用します。 このレイアウトには、`EditText`テキスト入力の母音と子音カウントを開始するための 2 つのボタン。

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

内容を置き換える**MainActivity.cs**を次のコード。 この例で示すように、ボタンのイベント ハンドラーの呼び出しがラップ`TextCounter`メソッド内に存在する、します。結果を表示するトースト AAR および使用します。 通知、`using`バインド ライブラリの名前空間のステートメント (この場合、 `Com.Xamarin.Textcounter`)。

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

コンパイルし、実行、 **BindingTest**プロジェクト。 アプリが起動し、左側のスクリーン ショットを表示 (、`EditText`テキストを使用して初期化されますが、これを変更することをタップすることができます)。 タップすると**カウント母音**トースト、右に示すようには、母音の数を表示します。

[![BindingTest を実行しているのスクリーン ショット](binding-an-aar-images/12-count-vowels.png)](binding-an-aar-images/12-count-vowels.png#lightbox)

タップしてください、**カウント子音**ボタンをクリックします。 また、行のテキストを変更して別の母音をテストするには、もう一度これらのボタンをタップします、子音がカウントされます。



### <a name="accessing-aar-resources"></a>アクセスします。AAR リソース

マージ ツール、Xamarin、 **R**からデータをします。アプリの AAR**リソース**クラス。 その結果、次のようにアクセスすることができます。AAR リソース レイアウト (および分離コード) に含まれるリソースにアクセスする場合と同じで、**リソース**プロジェクトのパス。

イメージ リソースをアクセスするには、 **Resource.Drawable**イメージ内にパックの名前します。AAR します。 たとえば、参照**image.png**でします。AAR ファイルを使用して`@drawable/image`:

```xml
<ImageView android:src="@drawable/image" ... />
```

内に存在するリソースのレイアウトをアクセスすることもできます、します。AAR します。 使用するには、 **Resource.Layout**内部にパッケージ化レイアウトの名前します。AAR します。 例えば:

```csharp
var a = new ArrayAdapter<string>(this, Resource.Layout.row_layout, ...);
```

**Textanalyzer.aar**例に配置されているイメージ ファイルが含まれています**res/drawable/monkey.png**します。 このイメージのリソースにアクセスして、アプリの例で使用します。

編集、 **BindingTest**レイアウト (**Main.axml**) を追加し、`ImageView`の末尾に、`LinearLayout`コンテナー。 これは、`ImageView`で見つかったイメージが表示されます**@drawable/monkey**; のリソース セクションからこのイメージが読み込まれます**textanalyzer.aar**:

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

[![BindingTest 子音数を表示します。](binding-an-aar-images/13-count-consonants.png)](binding-an-aar-images/13-count-consonants.png#lightbox)


おめでとうございます! Java ライブラリを正常に連結されています。AAR!



## <a name="summary"></a>まとめ

このチュートリアルでのバインド ライブラリを作成しました、します。AAR ファイルを最小限のテストをアプリにバインド ライブラリを追加し、ことを確認するアプリを実行、C#コード内に存在する Java コードを呼び出すことができます、します。AAR ファイルです。
アクセスして内にあるイメージ リソースを表示するアプリをさらに、拡張します。AAR ファイルです。


## <a name="related-links"></a>関連リンク

- [Java バインド ライブラリ (ビデオ) を構築](https://university.xamarin.com/classes#10090)
- [.JAR のバインド](~/android/platform/binding-java-library/binding-a-jar.md)
- [Java ライブラリのバインド](~/android/platform/binding-java-library/index.md)
- [AarBinding (サンプル)](https://developer.xamarin.com/samples/monodroid/JavaIntegration/AarBinding)
- [バグ 44573 1 つのプロジェクトが複数 .aar ファイルをバインドできません。](https://bugzilla.xamarin.com/show_bug.cgi?id=44573)
