---
title: 'チュートリアル: Android Kotlin ライブラリをバインドする'
description: この記事では、既存の Kotlin ライブラリ (BubblePicker) 用の Xamarin. Android バインドの作成に関する実践的なチュートリアルを提供します。 Kotlin ライブラリのコンパイル、バインド、Xamarin. Android アプリケーションでのバインドの使用などのトピックについて説明します。
ms.prod: xamarin
ms.assetid: 5E45451F-514F-4F2B-A6E8-599ED2EFDA2C
ms.technology: xamarin-android
author: alexeystrakh
ms.author: alstrakh
ms.date: 02/11/2020
ms.openlocfilehash: cbd7c796cd13aa45dc107bddf06ca44d6adbdf9d
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77519677"
---
# <a name="walkthrough-bind-an-android-kotlin-library"></a>チュートリアル: Android Kotlin ライブラリをバインドする

Xamarin を使用すると、モバイル開発者は、Visual Studio およびC#を使用してクロスプラットフォームのネイティブモバイルアプリを作成できます。 Android platform SDK コンポーネントはすぐに使用できますが、多くの場合、そのプラットフォーム用に記述されたサードパーティ製の Sdk を使用することもできます。また、Xamarin ではバインドによってこれを行うことができます。 サードパーティ製の Android フレームワークを Xamarin Android アプリケーションに組み込むには、アプリケーションで使用する前に、そのための Xamarin Android のバインドを作成する必要があります。

Android プラットフォームは、そのネイティブ言語およびツールと共に、常に進化しています。これには、Kotlin 言語の最近の導入が含まれています。これは、最終的に Java を置き換えるように設定されています。 既に Java から Kotlin に移行されている3d パーティ Sdk が多数あり、新しい課題があります。 Kotlin binding プロセスは Java に似ていますが、Xamarin Android アプリケーションの一部として正常にビルドして実行するには、追加の手順と構成設定が必要です。  

このドキュメントの目的は、このシナリオに対処するための高度なアプローチを概説し、簡単な例を示した詳細なステップバイステップガイドを提供することです。

## <a name="background"></a>背景

Kotlin は2016年2月にリリースされ、標準 Java コンパイラの代わりとして2017によって Android Studio に配置されました。 2019の後半で、Google は、Kotlin プログラミング言語が Android アプリ開発者向けの優先言語になったことを発表しました。 高レベルのバインディングアプローチは、[通常の Java ライブラリのバインドプロセス](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)に似ていますが、いくつかの重要な Kotlin 固有の手順があります。

## <a name="prerequisites"></a>前提条件

このチュートリアルを完了するための要件は次のとおりです。

- [Android Studio](https://developer.android.com/studio)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Java デコンパイラ](http://java-decompiler.github.io/)

## <a name="build-a-native-library"></a>ネイティブライブラリを構築する

最初の手順は Android Studio を使用してネイティブの Kotlin ライブラリを構築することです。 このライブラリは、通常、サードパーティの開発者によって提供されるか、 [Google の Maven リポジトリ](https://maven.google.com/web/index.html)や他のリモートリポジトリで入手できます。 たとえば、このチュートリアルでは、バブルピッカー Kotlin ライブラリのバインドが作成されます。

![GitHub BubblePicker のデモ](walkthrough-images/github-bubblepicker-demo.png)

1. GitHub からライブラリ用の[ソースコード](https://github.com/igalata/Bubble-Picker/archive/develop.zip)をダウンロードし、ローカルフォルダーの**バブルピッカー**にアンパックします。

1. Android Studio を起動し、 **[既存の Android Studio プロジェクトを開く]** メニューオプションを選択して、バブルピッカーローカルフォルダーを選択します。

    ![プロジェクトを開く Android Studio](walkthrough-images/android-studio-open-project.png)

1. Gradle を含む Android Studio が最新であることを確認します。 ソースコードは Android Studio v 3.5.3, Gradle v 5.4.1 で正常にビルドできます。 Gradle を最新の Gradle バージョンに更新する方法については、[こちらを参照](https://gradle.org/install/)してください。

1. 必要な Android SDK がインストールされていることを確認します。 ソースコードには Android SDK v25 が必要です。 Sdk コンポーネントをインストールするには、 **[ツール > Sdk マネージャー]** メニューオプションを開きます。

1. プロジェクトフォルダーのルートにあるメインの**gradle**構成ファイルを更新して同期します。

    - Kotlin のバージョンを1.3.10 に設定します。

        ```gradle
        buildscript {
            ext.kotlin_version = '1.3.10'
        }
        ```

    - サポートライブラリの依存関係を解決できるように、既定の Google の Maven リポジトリを登録します。

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

    - 構成ファイルが更新されると、同期されなくなり、 **[今すぐ同期]** ボタンが表示され、Gradle を押して同期プロセスが完了するまで待機します。

        ![今すぐ Gradle Sync を Android Studio](walkthrough-images/android-studio-gradle-syncnow.png)

        > [!TIP]
        > Gradle の依存関係キャッシュが破損している可能性があります。これは、ネットワーク接続のタイムアウト後に発生することがあります。 依存関係を再ダウンロードし、プロジェクトを同期します (ネットワークが必要です)。

        > [!TIP]
        > Gradle ビルドプロセス (デーモン) の状態が破損している可能性があります。 すべての Gradle デーモンを停止すると、この問題が解決する可能性があります。 Gradle ビルドプロセスを停止します (再起動が必要)。 Gradle プロセスが破損している場合は、IDE を閉じて、すべての Java プロセスを終了することもできます。

        > [!TIP]
        > プロジェクトがサードパーティプラグインを使用している可能性があります。これは、プロジェクト内の他のプラグイン、またはプロジェクトによって要求されたバージョンの Gradle と互換性がありません。

1. 右側の Gradle メニューを開き、 **bubblepicker > Tasks** メニューに移動します。次に、**ビルド**タスクをダブルタップして実行し、ビルドプロセスが完了するまで待ちます。

    ![Android Studio Gradle タスクの実行](walkthrough-images/android-studio-gradle-execute-task.png)

1. ルートフォルダーの [ファイル] ブラウザーを開き、build フォルダーに移動します。 [ **bubblepicker-> > outputs-> aar] を > 選択**し、 **bubblepicker-release**ファイルを**bubblepicker-v 1.0. aar**として保存します。このファイルは、後でバインドプロセスで使用されます。

    ![Android Studio AAR の出力](walkthrough-images/android-studio-aar-output.png)

AAR ファイルは、android でこの SDK を使用してアプリケーションを実行するために必要な、コンパイル済みの Kotlin ソースコードとアセットを含む Android アーカイブです。  

## <a name="prepare-metadata"></a>メタデータの準備

2番目の手順では、メタデータ変換ファイルを準備します。これは、それぞれC#のクラスを生成するために Xamarin Android によって使用されます。 Xamarin Android プロジェクトは、特定の Android アーカイブのすべてのネイティブクラスとメンバーを検出した後、適切なメタデータを含む XML ファイルを生成します。 次に、手動で作成したメタデータ変換ファイルが、以前に生成されたベースラインに適用さC#れ、コードの生成に使用される最終的な XML 定義ファイルが作成されます。

メタデータは、 [XPath](https://www.w3.org/TR/xpath/) 構文を使用し、バインドジェネレーターがバインディングアセンブリの作成に影響を与えるために使用します。 [Java バインドメタデータ](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)の記事では、変換の詳細について説明しています。

1. 空**の Metadata .xml**ファイルを作成します。

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <metadata>
    </metadata>
    ```

1. Xml 変換の定義:

- ネイティブ Kotlin ライブラリには、世界にC#公開しない2つの依存関係があり、それらを完全に無視するために2つの変換を定義します。 重要として、ネイティブメンバーが結果のバイナリから削除されることC#はありません。クラスのみが生成されません。 [Java デコンパイラ](http://java-decompiler.github.io/)を使用して、依存関係を識別できます。 ツールを実行し、前に作成した AAR ファイルを開きます。その結果、すべての依存関係、値、リソース、マニフェスト、およびクラスを反映した Android アーカイブの構造が表示されます。  

    ![Java デコンパイラの依存関係](walkthrough-images/java-decompiler-dependencies.png)

    これらのパッケージの処理をスキップする変換は、XPath 命令を使用して定義されます。

    ```xml
    <remove-node path="/api/package[starts-with(@name,'org.jbox2d')]" />
    <remove-node path="/api/package[starts-with(@name,'org.slf4j')]" />
    ```

- ネイティブ `BubblePicker` クラスには `getBackgroundColor` と `setBackgroundColor` の2つのメソッドがあり、次の変換C#はそれを `BackgroundColor` プロパティに変更します。

    ```xml
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='getBackground' and count(parameter)=0]" name="propertyName">BackgroundColor</attr>
    <attr path="/api/package[@name='com.igalata.bubblepicker.rendering']/class[@name='BubblePicker']/method[@name='setBackground' and count(parameter)=1 and parameter[1][@type='int']]" name="propertyName">BackgroundColor</attr>
    ```

- 符号なしの型 `UInt, UShort, ULong, UByte` 特別な処理が必要です。 これらの型の場合、Kotlin はメソッド名とパラメーターの型を自動的に変更し、生成されたコードに反映されます。

    ```Kotlin
    public open fun fooUIntMethod(value: UInt) : String {
        return "fooUIntMethod${value}"
    }
    ```

    このコードは、次の Java バイトコードにコンパイルされます。

    ```Java byte code
    @NotNull
    public String fooUIntMethod-WZ4Q5Ns(int value) {
    return "fooUIntMethod" + UInt.toString-impl(value);
    }
    ```

    さらに、`UIntArray, UShortArray, ULongArray, UByteArray` などの関連する型も、Kotlin の影響を受けます。 メソッド名は、追加のサフィックスを含むように変更され、パラメーターは、同じ型の符号付きバージョンの要素の配列に変更されます。 次の例では `UIntArray` 型のパラメーターが自動的に `int[]` に変換され、メソッド名は `fooUIntArrayMethod` から `fooUIntArrayMethod--ajY-9A`に変更されます。 後者は Xamarin Android ツールによって検出され、有効なメソッド名として生成されます。

    ```Kotlin
    public open fun fooUIntArrayMethod(value: UIntArray) : String {
        return "fooUIntArrayMethod${value.size}"
    }
    ```

    このコードは、次の Java バイトコードにコンパイルされます。

    ```Java byte code
    @NotNull
    public String fooUIntArrayMethod--ajY-9A(@NotNull int[] value) {
        Intrinsics.checkParameterIsNotNull(value, "value");
        return "fooUIntArrayMethod" + UIntArray.getSize-impl(value);
    }
    ```

    わかりやすい名前を付けるために、次のメタデータを**metadata .xml**に追加することができます。これにより、Kotlin コードで定義されている最初のメタデータに名前が更新されます。

    ```xml
    <attr path="/api/package[@name='com.microsoft.simplekotlinlib']/class[@name='FooClass']/method[@name='fooUIntArrayMethod--ajY-9A']" name="managedName">fooUIntArrayMethod</attr>
    ```

    BubblePicker サンプルでは、符号なしの型を使用するメンバーは存在しないため、追加の変更は必要ありません。

- 既定では、ジェネリックパラメーターを持つ Kotlin メンバーが Java のパラメーターに変換されます。`Lang.Object` 各種. たとえば、Kotlin メソッドには \<T > のジェネリックパラメーターがあります。

    ```Kotlin
    public open fun <T>fooGenericMethod(value: T) : String {
    return "fooGenericMethod${value}"
    }
    ```

    Xamarin Android バインドが生成されると、メソッドは次のようC#に公開されます。

    ```csharp
    [Register ("fooGenericMethod", "(Ljava/lang/Object;)Ljava/lang/String;", "GetFooGenericMethod_Ljava_lang_Object_Handler")]
    [JavaTypeParameters (new string[] {
        "T"
    })]

    public virtual string FooGenericMethod (Java.Lang.Object value);
    ```

    Java および Kotlin ジェネリックは、Xamarin Android ではサポートされていないC#ため、汎用 API にアクセスするための一般化されたメソッドが作成されます。 回避策として、ラッパー Kotlin ライブラリを作成し、ジェネリックを使用せずに、必要な Api を厳密に型指定された方法で公開することができます。 または、厳密に型指定C#された api を使用して同じ方法で問題に対処するために、側でヘルパーを作成することもできます。

    > [!TIP]
    > メタデータを変換することによって、生成されたバインディングにすべての変更を適用できます。 [Java ライブラリのバインド](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)に関する記事では、メタデータの生成方法と処理方法の詳細について説明しています。

## <a name="build-a-binding-library"></a>バインドライブラリを構築する

次の手順では、Visual Studio バインドテンプレートを使用して Xamarin Android プロジェクトを作成し、必要なメタデータとネイティブ参照を追加してから、プロジェクトをビルドして使用できるライブラリを生成します。

1. Visual Studio for Mac を開き、新しい Xamarin. Android バインドライブラリプロジェクトを作成して名前を付けます。この場合は、 **testBubblePicker**を実行し、ウィザードを完了します。 Xamarin. Android バインドテンプレートは次のパスにあります: **android > Library > Binding library**:

    ![Visual Studio のバインドの作成](walkthrough-images/visual-studio-create-binding.png)

    [変換] フォルダーには、次の3つの主な変換ファイルがあります。

    - **Metadata .xml** –生成されたバインディングの名前空間を変更するなど、最終的な API に変更を加えることができます。
    - **Enumfields .xml** – Java int 定数とC#列挙型の間のマッピングが含まれます。
    - **Enummethods .xml** –メソッドのパラメーターと戻り値の型を Java int 定数からC#列挙型に変更できます。

    **Enumfields** .xml ファイルと**enumfields .xml**ファイルを空のままにし、**メタデータ**を更新して変換を定義します。

1. 既存の**変換/メタデータの .xml**ファイルを、前の手順で作成した**メタデータの .xml**ファイルに置き換えます。 [プロパティ] ウィンドウで、[ファイルの**ビルド] アクション**が [変換**ファイル**] に設定されていることを確認します。

    ![Visual Studio メタデータ](walkthrough-images/visual-studio-metadata.png)

1. 手順 1. で作成した**bubblepicker-v 1.0. aar**ファイルを、ネイティブ参照としてバインドプロジェクトに追加します。 ネイティブライブラリ参照を追加するには、finder を開き、Android アーカイブのあるフォルダーに移動します。 アーカイブをソリューションエクスプローラーの [Jar] フォルダーにドラッグアンドドロップします。 または、Jar フォルダーの コンテキストメニューの**追加** オプションを使用して、**既存のファイル** を選択します。 このチュートリアルの目的で、ファイルをディレクトリにコピーすることを選択します。 **ビルドアクション**が**libraryprojectzip**に設定されていることを確認してください。

    ![Visual Studio ネイティブリファレンス](walkthrough-images/visual-studio-native-reference.png)

1. [Kotlin. Stdlib.h> NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)パッケージへの参照を追加します。 このパッケージは、Kotlin Standard Library のバインドです。 このパッケージがない場合、バインディングは、Kotlin ライブラリが Kotlin 固有の型を使用していない場合にのみ機能します。 C#それ以外の場合は、すべてのメンバーがに公開されず、バインディングを使用しようとするすべてのアプリが実行時にクラッシュします。

    > [!TIP]
    > Xamarin Android の制限により、バインドツールはバインドプロジェクトごとに1つの Android アーカイブ (AAR) のみを追加できます。 複数の AAR ファイルを含める必要がある場合は、AAR ごとに1つずつ、複数の Xamarin Android プロジェクトが必要です。 このチュートリアルの場合、この手順の前の4つのアクションをアーカイブごとに繰り返す必要があります。 別のオプションとして、複数の Android アーカイブを1つのアーカイブとして手動でマージすることができます。その結果、1つの Xamarin. Android バインドプロジェクトを使用できます。

1. 最後のアクションは、ライブラリをビルドし、コンパイルエラーが発生しないようにすることです。 コンパイルエラーが発生した場合は、ライブラリメンバーの追加、削除、または名前の変更を行う xml 変換メタデータを追加して作成したメタデータ .xml ファイルを使用して、これらのファイルに対処して処理できます。

## <a name="consume-the-binding-library"></a>バインドライブラリを使用する

最後の手順では、Xamarin Android アプリケーションで Xamarin. Android バインドライブラリを使用します。 新しい Xamarin Android プロジェクトを作成し、バインドライブラリに参照を追加し、バブルピッカー UI をレンダリングします。

1. Xamarin Android プロジェクトを作成します。 Android **> アプリ > Android アプリ**を開始点として使用し、互換性の問題を回避するためにプラットフォームのターゲットとして [**最新] と [最大**] を選択します。 次のすべての手順で、このプロジェクトを対象とします。

    ![Visual Studio のアプリの作成](walkthrough-images/visual-studio-create-app.png)

1. バインドプロジェクトにプロジェクト参照を追加するか、前に作成した DLL に参照を追加します。

    ![Visual Studio のバインドの追加の参照 .png](walkthrough-images/visual-studio-add-binding-reference.png)

1. 前に Kotlin バインドプロジェクトに追加した[Stdlib.h> NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)パッケージへの参照を追加します。 これにより、実行時の処理が必要な Kotlin 固有の型にサポートが追加されます。  このパッケージがないと、アプリはコンパイルできますが、実行時にクラッシュします。

    ![Visual Studio の Stdlib.h> NuGet の追加](walkthrough-images/visual-studio-add-stdlib-nuget.png)

1. `MainActivity`の Android レイアウトに `BubblePicker` コントロールを追加します。 **TestBubblePicker/Resources/layout/content_main .xml**ファイルを開き、BubblePicker コントロールノードをルート RelativeLayout コントロールの最後の要素として追加します。

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

1. アプリのソースコードを更新し、初期化ロジックを `MainActivity`に追加します。これにより、バブルピッカー SDK がアクティブになります。

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

    `BubblePickerAdapter` と `BubblePickerListener` は、バブルデータと制御操作を処理する2つのクラスをゼロから作成します。

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

1. アプリケーションを実行します。これにより、バブルピッカー UI が表示されます。

    ![BubblePicker のデモ](walkthrough-images/bubble-picker-demo.png)

    このサンプルでは、要素スタイルをレンダリングして相互作用を処理する追加のコードが必要ですが、`BubblePicker` コントロールが正常に作成およびアクティブ化されています。

おめでとうございます! Kotlin ライブラリを使用する Xamarin Android アプリとバインドライブラリが正常に作成されました。  

これで、Xamarin Android のバインドライブラリを介してネイティブの Kotlin ライブラリを使用する基本的な Xamarin Android アプリケーションが完成しました。 このチュートリアルでは、基本的な例を意図的に使用して、導入される主要な概念に重点を置いています。 実際のシナリオでは、多くの場合、より多くの Api を公開し、メタデータ変換を適用する必要があります。

## <a name="related-links"></a>関連リンク

- [Android Studio](https://developer.android.com/studio)
- [Gradle のインストール](https://gradle.org/install/)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Java デコンパイラ](http://java-decompiler.github.io/)
- [BubblePicker Kotlin ライブラリ](https://github.com/igalata/Bubble-Picker)
- [Java ライブラリのバインド](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java バインドメタデータ](https://docs.microsoft.com/xamarin/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata)
- [Kotlin. Stdlib.h> NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [サンプルプロジェクトリポジトリ](https://github.com/alexeystrakh/xamarin-binding-kotlin-framework)
