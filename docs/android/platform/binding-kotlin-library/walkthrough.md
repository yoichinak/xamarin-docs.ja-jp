---
title: 'チュートリアル: Android Kotlin ライブラリをバインドする'
description: この記事では、既存の Kotlin ライブラリ (BubblePicker) 用の Xamarin.Android バインドの作成に関する実践的なチュートリアルを提供します。 Kotlin ライブラリのコンパイル、バインド、Xamarin.Android アプリケーションでのバインドの使用などのトピックを扱います。
ms.prod: xamarin
ms.assetid: 5E45451F-514F-4F2B-A6E8-599ED2EFDA2C
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: af926b518c55bd0d6c73180e512dd669e93778f7
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853061"
---
# <a name="walkthrough-bind-an-android-kotlin-library"></a>チュートリアル: Android Kotlin ライブラリをバインドする

> [!IMPORTANT]
> 現在、Xamarin プラットフォームでのカスタム バインディングの使用を調査しています。 今後の開発作業の発展のために、この[**アンケート**](https://www.surveymonkey.com/r/KKBHNLT)にご回答ください。

Xamarin を使用すると、モバイル開発者は、Visual Studio および C# を使用してクロスプラットフォームのネイティブ モバイル アプリを作成できます。 Android プラットフォーム SDK コンポーネントはすぐに使用できますが、多くの場合、そのプラットフォーム用に記述されたサードパーティ製の SDK を使用することもでき、Xamarin ではバインドによってこれを行うことができます。 サードパーティ製の Android フレームワークを Xamarin.Android アプリケーションに組み込むには、アプリケーションで使用する前に、そのフレームワーク用の Xamarin.Android バインドを作成する必要があります。

Android プラットフォームは、最近の Kotlin 言語の導入に見られるように、そのネイティブ言語およびツールと共に常に進化しており、最終的には Java を置き換えるように設定されています。 既に Java から Kotlin に移行されているサードパーティ製の SDK が多数あり、これにより、新たな課題が生じています。 Kotlin バインド プロセスは Java に似ていますが、Xamarin.Android アプリケーションの一部として正常にビルドして実行するには、追加の手順と構成設定が必要です。  

このドキュメントの目的は、このシナリオに対処するためのアプローチの概要を示し、簡単な例を含む詳細なステップバイステップ ガイドを提供することです。

## <a name="background"></a>背景

Kotlin は 2016 年 2 月にリリースされ、2017 年までに、標準の Java コンパイラに代わって Android Studio に配置されました。 2019 年の後半に、Google は、Kotlin プログラミング言語が Android アプリ開発者向けの優先言語になったことを発表しました。 バインド アプローチの概要は、[通常の Java ライブラリのバインド プロセス](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)に似ていますが、いくつかの Kotlin 固有の重要な手順があります。

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルを完了するための要件は次のとおりです。

- [Android Studio](https://developer.android.com/studio)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Java デコンパイラ](http://java-decompiler.github.io/)

## <a name="build-a-native-library"></a>ネイティブ ライブラリをビルドする

最初の手順では、Android Studio を使用してネイティブの Kotlin ライブラリをビルドします。 ライブラリは通常、サードパーティの開発者によって提供されるか、または [Google の Maven リポジトリ](https://maven.google.com/web/index.html)やその他のリモート リポジトリで入手できます。 一例として、このチュートリアルでは、Bubble Picker Kotlin ライブラリのバインドが作成されます。

![GitHub BubblePicker のデモ](walkthrough-images/github-bubblepicker-demo.png)

1. GitHub からライブラリに[ソース コード](https://github.com/igalata/Bubble-Picker/archive/develop.zip)をダウンロードし、**Bubble-Picker** ローカル フォルダー内に解凍します。

1. Android Studio を起動し、 **[Open an existing Android Studio project]\(既存の Android Studio プロジェクトを開く\)** メニュー オプションを選択して、Bubble-Picker ローカル フォルダーを選択します。

    ![Android Studio でプロジェクトを開く](walkthrough-images/android-studio-open-project.png)

1. Gradle を含め、Android Studio が最新であることを確認します。 ソース コードは Android Studio v3.5.3、Gradle v5.4.1 で正常にビルドできます。 Gradle を最新の Gradle バージョンに更新する方法については、[こちらを参照してください](https://gradle.org/install/)。

1. 必要な Android SDK がインストールされていることを確認します。 ソース コードには Android SDK v25 が必要です。 SDK コンポーネントをインストールするには、 **[ツール] > [SDK Manager]** メニュー オプションを開きます。

1. プロジェクト フォルダーのルートにあるメイン **build.gradle** 構成ファイルを更新して同期します。

    - Kotlin のバージョンを 1.3.10 に設定します。

        ```gradle
        buildscript {
            ext.kotlin_version = '1.3.10'
        }
        ```

    - サポート ライブラリの依存関係を解決できるように、既定の Google の Maven リポジトリを登録します。

        ```gradle
        allprojects {
            repositories {
                jcenter()
                maven {
                    url "https://maven.google.com"
                }
            }
        }
        ```

    - 構成ファイルが更新されると、同期されなくなり、Gradle に **[今すぐ同期]** ボタンが表示されます。このボタンを押して、同期プロセスが完了するまで待ちます。

        ![Android Studio Gradle の [今すぐ同期]](walkthrough-images/android-studio-gradle-syncnow.png)

        > [!TIP]
        > Gradle の依存関係キャッシュが破損している可能性があります。これは、ネットワーク接続のタイムアウト後に発生することがあります。 依存関係を再ダウンロードし、プロジェクトを同期します (ネットワークが必要です)。

        > [!TIP]
        > Gradle ビルド プロセス (デーモン) の状態が破損している可能性があります。 すべての Gradle デーモンを停止すると、この問題が解決する可能性があります。 Gradle ビルド プロセスを停止します (再起動が必要です)。 Gradle プロセスが破損している場合は、IDE を閉じて、すべての Java プロセスを終了することもできます。

        > [!TIP]
        > プロジェクトでサードパーティ製のプラグインが使用されている可能性があります。これは、プロジェクト内の他のプラグイン、またはプロジェクトによって要求されるバージョンの Gradle と互換性がありません。

1. 右側の [Gradle] メニューを開き、 **[bubblepicker] > [タスク]** メニューに移動して、 **[build]** タスクをダブルタップして実行し、ビルド プロセスが完了するまで待ちます。

    ![Android Studio Gradle でのタスクの実行](walkthrough-images/android-studio-gradle-execute-task.png)

1. ルート フォルダー ファイル ブラウザーを開き、build フォルダーに移動 ( **[Bubble-Picker] -> [bubblepicker] -> [build] -> [outputs] -> [aar]** ) し、**bubblepicker-release** ファイルを **bubblepicker-v1.0.aar** として保存します。このファイルは、後でバインド プロセスで使用されます。

    ![Android Studio AAR の出力](walkthrough-images/android-studio-aar-output.png)

AAR ファイルは Android アーカイブであり、Android でこの SDK を使用してアプリケーションを実行するために必要な、コンパイル済みの Kotlin ソース コードおよびアセットが含まれています。  

## <a name="prepare-metadata"></a>メタデータを準備する

2 番目の手順では、メタデータ変換ファイルを準備します。これは、それぞれの C# クラスを生成するために Xamarin.Android によって使用されます。 Xamarin.Android ビルド プロジェクトでは、特定の Android アーカイブのすべてのネイティブ クラスおよびメンバーが検出されてから、適切なメタデータを含む XML ファイルが生成されます。 次に、手動で作成したこのメタデータ変換ファイルが、以前に生成されたベースラインに適用されて、C# コードの生成に使用される最終的な XML 定義ファイルが作成されます。

メタデータでは  [XPath](https://www.w3.org/TR/xpath/)  構文が使用されており、このメタデータはバインド アセンブリの作成に影響を与えるためにバインド ジェネレーターによって使用されます。 「[Java バインド メタデータ](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)」の記事では、適応できる次の変換について詳しく説明されています。

1. 空の **Metadata.xml** ファイルを作成します。

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <metadata>
    </metadata>
    ```

1. xml 変換を定義します。

- ネイティブ Kotlin ライブラリには 2 つの依存関係があります。これらの依存関係を C# 環境に公開したくないので、これらを完全に無視するように 2 つの変換を定義します。 重要点として、生成されるバイナリからネイティブ メンバーが削除されることはなく、C# クラスのみが生成されなくなります。 [Java デコンパイラ](http://java-decompiler.github.io/)を使用すると、依存関係を識別できます。 ツールを実行し、前に作成した AAR ファイルを開きます。これにより、すべての依存関係、値、リソース、マニフェスト、およびクラスを反映する Android アーカイブの構造が表示されます。  

    ![Java デコンパイラの依存関係](walkthrough-images/java-decompiler-dependencies.png)

    これらのパッケージの処理をスキップする変換は、XPath 手順を使用して定義されます。

    ```xml
    <remove-node path="/api/package[starts-with(@name,'org.jbox2d')]" />
    <remove-node path="/api/package[starts-with(@name,'org.slf4j')]" />
    ```

- ネイティブ `BubblePicker` クラスには `getBackgroundColor` と `setBackgroundColor` の 2 つのメソッドがあり、次の変換によって C# `BackgroundColor` プロパティに変更されます。

    ```xml
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='getBackground' and count(parameter)=0]" name="propertyName">BackgroundColor</attr>
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='setBackground' and count(parameter)=1 and parameter[1][@type='int']]" name="propertyName">BackgroundColor</attr>
    ```

- 符号なしの型 `UInt, UShort, ULong, UByte` では、特別な処理が必要です。 これらの型の場合は、Kotlin によってメソッド名とパラメーターの型が自動的に変更され、生成されるコードに反映されます。

    ```Kotlin
    public open fun fooUIntMethod(value: UInt) : String {
        return "fooUIntMethod${value}"
    }
    ```

    このコードは、次の Java バイト コードにコンパイルされます。

    ```Java byte code
    @NotNull
    public String fooUIntMethod-WZ4Q5Ns(int value) {
    return "fooUIntMethod" + UInt.toString-impl(value);
    }
    ```

    さらに、`UIntArray, UShortArray, ULongArray, UByteArray` などの関連する型も、Kotlin の影響を受けます。 メソッド名は、追加のサフィックスを含むように変更され、パラメーターは、同じ型の符号付きバージョンの要素の配列に変更されます。 次の例では、`UIntArray` 型のパラメーターが自動的に `int[]` に変換され、メソッド名が `fooUIntArrayMethod` から `fooUIntArrayMethod--ajY-9A` に変更されます。 後者は Xamarin.Android ツールによって検出され、有効なメソッド名として生成されます。

    ```Kotlin
    public open fun fooUIntArrayMethod(value: UIntArray) : String {
        return "fooUIntArrayMethod${value.size}"
    }
    ```

    このコードは、次の Java バイト コードにコンパイルされます。

    ```Java byte code
    @NotNull
    public String fooUIntArrayMethod--ajY-9A(@NotNull int[] value) {
        Intrinsics.checkParameterIsNotNull(value, "value");
        return "fooUIntArrayMethod" + UIntArray.getSize-impl(value);
    }
    ```

    わかりやすい名前を付けるために、次のメタデータを **Metadata.xml** に追加することができます。これにより、名前が、Kotlin コードで定義されている元の名前に更新されます。

    ```xml
    <attr path="/api/package[@name='com.microsoft.simplekotlinlib']/class[@name='FooClass']/method[@name='fooUIntArrayMethod--ajY-9A']" name="managedName">fooUIntArrayMethod</attr>
    ```

    BubblePicker サンプルでは、符号なしの型を使用するメンバーが存在しないため、追加の変更は必要ありません。

- 既定では、ジェネリック パラメーターを持つ Kotlin メンバーは Java.`Lang.Object` 型のパラメーターに変換されます。 たとえば、Kotlin メソッドにはジェネリック パラメーター \<T> があります。

    ```Kotlin
    public open fun <T>fooGenericMethod(value: T) : String {
    return "fooGenericMethod${value}"
    }
    ```

    Xamarin.Android バインドが生成されると、次のようにメソッドが C# に公開されます。

    ```csharp
    [Register ("fooGenericMethod", "(Ljava/lang/Object;)Ljava/lang/String;", "GetFooGenericMethod_Ljava_lang_Object_Handler")]
    [JavaTypeParameters (new string[] {
        "T"
    })]

    public virtual string FooGenericMethod (Java.Lang.Object value);
    ```

    Java ジェネリックと Kotlin ジェネリックは、Xamarin.Android ではサポートされていないため、ジェネリック API にアクセスするための一般的な C# メソッドが作成されます。 回避策として、ラッパー Kotlin ライブラリを作成し、ジェネリックを使用せずに、必須 API を厳密に型指定された方法で公開することができます。 または、厳密に型指定された API を使用して、同じ方法で問題に対処するために、C# 側でヘルパーを作成することもできます。

    > [!TIP]
    > メタデータを変換することによって、生成されるバインドに任意の変更を適用できます。 「[Java ライブラリのバインド](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)」の記事では、メタデータの生成および処理方法の詳細について説明されています。

## <a name="build-a-binding-library"></a>バインド ライブラリをビルドする

次の手順では、Visual Studio バインド テンプレートを使用して Xamarin.Android プロジェクトを作成し、必要なメタデータとネイティブ参照を追加してから、プロジェクトをビルドして、使用できるライブラリを生成します。

1. Visual Studio for Mac を開き、新しい Xamarin.Android バインド ライブラリ プロジェクトを作成して名前 (ここでは **testBubblePicker.Binding**) を付け、ウィザードを完了します。 Xamarin.Android バインド テンプレートは、 **[Android] > [ライブラリ] > [Binding Library]\(バインド ライブラリ\)** のパスに配置されています。

    ![Visual Studio でのバインドの作成](walkthrough-images/visual-studio-create-binding.png)

    Transformations フォルダーには、次の 3 つの主な変換ファイルがあります。

    - **Metadata.xml** – 生成されたバインドの名前空間の変更など、最終的な API に変更を加えることができます。
    - **EnumFields.xml** – Java の int 定数と C# の列挙体の間のマッピングが含まれています。
    - **EnumMethods.xml** – メソッドのパラメーターと戻り値の型を Java の int 定数から C# の列挙体に変更できます。

    **EnumFields.xml** ファイルと **EnumMethods.xml** ファイルを空のままにし、**Metadata.xml** を更新して変換を定義します。

1. 既存の **transformation/Metadata.xml** ファイルを、前の手順で作成した **Metadata.xml** ファイルに置き換えます。 [プロパティ] ウィンドウで、 **[ビルド アクション]** ファイルが **TransformationFile** に設定されていることを確認します。

    ![Visual Studio メタデータ](walkthrough-images/visual-studio-metadata.png)

1. 手順 1 で作成した **bubblepicker-v1.0.aar** ファイルを、ネイティブ参照としてバインド プロジェクトに追加します。 ネイティブ ライブラリ参照を追加するには、ファインダーを開き、Android アーカイブを含むフォルダーに移動します。 アーカイブをソリューション エクスプローラーの Jars フォルダーにドラッグ アンド ドロップします。 または、Jars フォルダーで **[追加]** コンテキスト メニュー オプションを使用し、 **[既存のファイル]** を選択します。 このチュートリアルの目的に沿い、ファイルをディレクトリにコピーすることを選択します。 **[ビルド アクション]** が **LibraryProjectZip** に設定されていることを確認してください。

    ![Visual Studio ネイティブ参照](walkthrough-images/visual-studio-native-reference.png)

1. [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) パッケージへの参照を追加します。 このパッケージは、Kotlin 標準ライブラリのバインドです。 このパッケージがない場合、バインドは、Kotlin ライブラリで Kotlin 固有の型が使用されていない場合にのみ機能します。それ例外の場合は、すべてのメンバーが C# に公開されず、バインドを使用しようとするすべてのアプリが実行時にクラッシュします。

    > [!TIP]
    > Xamarin.Android の制限により、バインド ツールでは、バインド プロジェクトごとに 1 つの Android アーカイブ (AAR) のみを追加できます。 複数の AAR ファイルを含める必要がある場合は、AAR ごとに 1 つずつ、複数の Xamarin.Android プロジェクトが必要です。 このチュートリアルの場合、この手順の前の 4 つのアクションをアーカイブごとに繰り返す必要があります。 別のオプションとして、複数の Android アーカイブを 1 つのアーカイブとして手動でマージすることもできます。その結果、1 つの Xamarin.Android バインド プロジェクトを使用できます。

1. 最後のアクションでは、ライブラリをビルドし、コンパイル エラーが発生しないようにします。 コンパイル エラーが発生した場合は、xml 変換メタデータを追加して作成した Metadata.xml ファイルを使用して、これらのエラーに対処して処理できます。このメタデータにより、ライブラリ メンバーの追加、削除、または名前の変更を行うことができます。

## <a name="consume-the-binding-library"></a>バインド ライブラリを使用する

最後の手順では、Xamarin.Android アプリケーションで Xamarin.Android バインド ライブラリを使用します。 新しい Xamarin.Android プロジェクトを作成し、バインド ライブラリに参照を追加して、Bubble Picker UI をレンダリングします。

1. Xamarin.Android プロジェクトを作成します。 **[Android] > [アプリ] > [Android アプリ]** を開始点として使用し、互換性の問題を回避するために、[ターゲット プラットフォーム] オプションとして **[最新および最高]** を選択します。 次のすべての手順は、このプロジェクトを対象としています。

    ![Visual Studio でのアプリの作成](walkthrough-images/visual-studio-create-app.png)

1. バインド プロジェクトにプロジェクト参照を追加するか、前に作成した DLL に参照を追加します。

    ![Visual Studio でのバインド参照の追加 (png)](walkthrough-images/visual-studio-add-binding-reference.png)

1. 前の手順で Xamarin.Android バインド プロジェクトに追加した [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) パッケージへの参照を追加します。 これにより、実行時の処理が必要な Kotlin 固有の型へのサポートが追加されます。  このパッケージがない場合、アプリはコンパイルできますが、実行時にクラッシュします。

    ![Visual Studio での StdLib NuGet の追加](walkthrough-images/visual-studio-add-stdlib-nuget.png)

1. `MainActivity` の Android レイアウトに `BubblePicker` コントロールを追加します。 **testBubblePicker/Resources/layout/content_main.xml** ファイルを開き、ルート RelativeLayout コントロールの最後の要素として BubblePicker コントロール ノードを追加します。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout …>
        …
        <com.igalata.bubblepicker.rendering.BubblePicker
            android:id="@+id/picker"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:backgroundColor="@android:color/white" />
    </RelativeLayout>
    ```

1. アプリのソース コードを更新し、初期化ロジックを `MainActivity` に追加します。これにより、Bubble Picker SDK がアクティブになります。

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState)
    {
        ...
        var picker = FindViewById<BubblePicker>(Resource.Id.picker);
        picker.BubbleSize = 20;
        picker.Adapter = new BubblePickerAdapter();
        picker.Listener = new BubblePickerListener(picker);
        ...
    }
    ```

    `BubblePickerAdapter` と `BubblePickerListener` は、ゼロから作成する 2 つのクラスで、バブル データとコントロールの相互作用を処理するものです。

    ```csharp
    public class BubblePickerAdapter : Java.Lang.Object, IBubblePickerAdapter
    {
        private List<string> _bubbles = new List<string>();
        public int TotalCount => _bubbles.Count;
        public BubblePickerAdapter()
        {
            for (int i = 0; i < 10; i++)
            {
                _bubbles.Add($"Item {i}");
            }
        }

        public PickerItem GetItem(int itemIndex)
        {
            if (itemIndex < 0 || itemIndex >= _bubbles.Count)
                return null;

            var result = _bubbles[itemIndex];
            var item = new PickerItem(result);
            return item;
        }
    }

    public class BubblePickerListener : Java.Lang.Object, IBubblePickerListener
    {
        public View Picker { get; }
        public BubblePickerListener(View picker)
        {
            Picker = picker;
        }

        public void OnBubbleDeselected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Deselected: {item.Title}", Snackbar.LengthLong)
                .SetAction("Action", (Android.Views.View.IOnClickListener)null)
                .Show();
        }

        public void OnBubbleSelected(PickerItem item)
        {
            Snackbar.Make(Picker, $"Selected: {item.Title}", Snackbar.LengthLong)
            .SetAction("Action", (Android.Views.View.IOnClickListener)null)
            .Show();
        }
    }
    ```

1. アプリを実行します。これにより、Bubble Picker UI が表示されます。

    ![BubblePicker のデモ](walkthrough-images/bubble-picker-demo.png)

    このサンプルには、要素スタイルをレンダリングして相互作用を処理する追加のコードが必要ですが、`BubblePicker` コントロールは正常に作成されアクティブ化されています。

おめでとうございます! Kotlin ライブラリを使用する Xamarin.Android アプリとバインド ライブラリが正常に作成されました。  

これで、Xamarin.Android バインド ライブラリを介してネイティブ Kotlin ライブラリを使用する基本的な Xamarin.Android アプリケーションが完成しました。 このチュートリアルでは、紹介する主要な概念に焦点を当てるために、基本的な例を意図的に使用しています。 実際のシナリオでは、多くの場合、より多くの API を公開し、それらにメタデータ変換を適用する必要があります。

## <a name="related-links"></a>関連リンク

- [Android Studio](https://developer.android.com/studio)
- [Gradle のインストール](https://gradle.org/install/)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Java デコンパイラ](http://java-decompiler.github.io/)
- [BubblePicker Kotlin ライブラリ](https://github.com/igalata/Bubble-Picker)
- [Java ライブラリのバインド](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java バインド メタデータ](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [サンプル プロジェクト リポジトリ](https://github.com/alexeystrakh/xamarin-binding-kotlin-framework)
