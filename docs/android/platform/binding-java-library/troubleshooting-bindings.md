---
title: "バインディングのトラブルシューティング"
description: "この記事は、バインディング、および考えられる原因と問題を解決する方法をお勧めを生成するときに発生する可能性のあるいくつかの一般的なエラーをまとめたものです。"
ms.topic: article
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 6d31e2a22c63f8d46893dd1928b561e1a06b19b4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshooting-bindings"></a>バインディングのトラブルシューティング

_この記事は、バインディング、および考えられる原因と問題を解決する方法をお勧めを生成するときに発生する可能性のあるいくつかの一般的なエラーをまとめたものです。_


## <a name="overview"></a>概要

Android ライブラリのバインド (、 **.aar**または**.jar**) ファイルが簡単な問題ではほとんどありません。 手間 Java と .NET の違いに起因する問題を軽減するために通常必要があります。
これらの問題は Xamarin.Android が Android ライブラリをバインドするを防ぐし、ビルド ログのエラー メッセージとしてそれ自体を表示します。 このガイドは、問題のトラブルシューティングのヒントを提供のより一般的な問題とシナリオ、ボックスの一覧を Android ライブラリを正常にバインド可能なソリューションを提供します。

既存の Android ライブラリをバインドするときに、次の点に留意する必要があります。

- **ライブラリの外部の依存関係**&ndash;と Xamarin.Android プロジェクトでは、Android ライブラリで必要な任意の Java 依存関係を含める必要がある、 **ReferenceJar**か、または、 **EmbeddedReferenceJar**です。

- **Android ライブラリがターゲットである、Android API レベル** &ndash; Android API レベルを「ダウン グレード」することはできません。 Xamarin.Android バインディング プロジェクトが対象としている同じ API レベル (またはそれ以上) を確認してください、Android ライブラリです。

- **Android ライブラリをパッケージ化に使用した Android JDK のバージョン** &ndash; Xamarin.Android によって Android ライブラリが使用中のものよりも JDK の別のバージョンでビルドされた場合にバインド エラーが発生する可能性があります。 可能であれば、Xamarin.Android のインストールで使用される JDK の同じバージョンを使用して Android ライブラリを再コンパイルします。

有効にするのには、まずバインド Xamarin.Android ライブラリに関する問題のトラブルシューティングに[MSBuild の診断出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)です。
診断の出力を有効にした後 Xamarin.Android バインディング プロジェクトをリビルドし、新機能については問題の原因の手掛かりを見つけるには、ビルド ログを調べます。

Android ライブラリをコンパイルし、種類とバインドしようとして Xamarin.Android メソッドを確認すると役立つことがも検証します。 これについては、このガイドで後で詳しく説明します。


## <a name="decompiling-an-android-library"></a>逆 Android ライブラリ

クラスと Java クラスのメソッドを調べることで、ライブラリのバインドに役立つ重要な情報を提供できます。
[JD GUI](http://jd.benow.ca/)グラフィカル ユーティリティから Java ソース コードを表示できるは、**クラス**JAR に含まれるファイル。 実行できますスタンド アロン アプリケーションまたはプラグインとして IntelliJ または Eclipse 用。

Android ライブラリ開くをデコンパイル、**です。JAR** Java コンパイラを持つファイルです。 ライブラリがある場合、**です。[Aar]**ファイルでは、ファイルを抽出するために必要な**classes.jar**アーカイブ ファイルからです。 使用した JD GUI 分析のサンプルのスクリーン ショットを次に示します、[ピカソ](http://square.github.io/picasso/)JAR:

![Java コンパイラを使用してピカソ 2.5.2.jar を分析するには](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android ライブラリをデコンパイルしたが後、は、ソース コードを確認します。 一般に、確認します。

- **難読化の特性を持つクラス**&ndash;難読化されたクラスの特性を含めます。

    - クラス名が含まれています、  **$** 、つまり**$.class**
    - クラス名が完全セキュリティが侵害の小文字、つまり**a.class**      

- **`import` 参照されていないライブラリ ステートメント**&ndash;未参照のライブラリを識別し、Xamarin.Android バインディングでプロジェクトにこれらの依存関係を追加、**ビルド アクション**の**ReferenceJar**または**EmbedddedReferenceJar**です。

> [!NOTE]
> Java ライブラリを逆が禁止されていますかまたは法的制限の下の 現地の法律または Java ライブラリが公開されたライセンスに基づきます。 必要に応じて、Java ライブラリをコンパイルし、ソース コードを調査する前に有効な professional のサービスを登録します。


## <a name="inspect-apixml"></a>API を検査します。XML

Xamarin.Android が XML ファイル名を生成、バインディングのプロジェクトのビルドの一部として**obj/Debug/api.xml**:

![生成された api.xml obj/デバッグ](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

このファイルは、バインドを Xamarin.Android が試行されているすべての Java Api の一覧を提供します。 このファイルの内容は、不足している型またはメソッドを識別、バインドを複製できます。 このファイルの調査には、手間と時間がかかりますが、任意のバインドの問題の原因の手掛かりを提供できます。 たとえば、 **api.xml**プロパティが、不適切な型を返すことや、同じマネージ名を持つことがある 2 つの型を明らかに可能性があります。


## <a name="known-issues"></a>既知の問題

このセクションでは、いくつかの一般的なエラー メッセージまたは現象は一覧を my Android ライブラリをバインドしようとするときに発生します。


### <a name="problem-java-version-mismatch"></a>問題: Java バージョンが一致しません

場合によって、型は生成されませんまたはいずれかでコンパイルされたライブラリと比較して Java の古いバージョンを使用しているために、予期しないクラッシュが発生する可能性です。 Xamarin.Android プロジェクトを使用している JDK の同じバージョンの Android ライブラリを再コンパイルします。


### <a name="problem-at-least-one-java-library-is-required"></a>問題: 少なくとも 1 つの Java ライブラリが必要

エラーが発生する「には、少なくとも 1 つの Java ライブラリが必要ですが、」いなくても、します。JAR が追加されました。

#### <a name="possible-causes"></a>考えられる原因:

必ず、ビルド アクションに設定されている`EmbeddedJar`です。 複数のビルド アクションがあるためです。JAR ファイル (など`InputJar`、 `EmbeddedJar`、`ReferenceJar`と`EmbeddedReferenceJar`)、バインド ジェネレーターは、既定で使用するを推測できない自動的にします。 ビルド アクションの詳細については、次を参照してください。[構築アクション](~/android/platform/binding-java-library/index.md)です。


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>問題: を読み込むことができませんツールをバインドします。JAR ライブラリ

バインドのライブラリ コード ジェネレーターの読み込みに失敗します。JAR ライブラリです。

#### <a name="possible-causes"></a>考えられる原因

いくつか。(Proguard などのツール) を使用してコード難読化を使用するための JAR ライブラリは、Java のツールで読み込むことができません。 当社のツールは、Java のリフレクションを使用およびライブラリをエンジニア リング ASM バイトのコードでこれらのツールが依存している可能性があります reject 難読化されたライブラリには、Android ランタイム ツールを渡すことがあります。 この回避策は、手のバインド、バインドのジェネレーターを使用する代わりにこれらのライブラリにです。



### <a name="problem-missing-c-types-in-generated-output"></a>問題: では、c# で生成された出力の種類がありません。

バインディング**.dll**ビルドが一部の種類の Java をミスまたは C# の場合、生成されたソースが不足している型があることを示すエラーのために構築されません。

#### <a name="possible-causes"></a>考えられる原因:

このエラーは、以下の一覧にいくつかの理由により発生する可能性があります。

-   バインドされているライブラリは、2 つ目の Java ライブラリを参照することができません。 バインドされているライブラリのパブリック API は、2 番目のライブラリから型を使用している場合も、2 番目のライブラリの管理されているバインディングを参照する必要があります。

-   ライブラリが原因で予期しないメタデータの読み込み、上のライブラリの読み込みエラーの理由のような Java リフレクションにより挿入されたことができます。 Xamarin.Android のツールは、このような状況を現在解決できません。 このような場合は、ライブラリを手動でバインドする必要があります。

-   あるときに、アセンブリを読み込めませんでした。 .NET 4.0 ランタイムでのバグが発生しました。 この問題は、.NET 4.5 ランタイムで修正されました。

-   Java は、パブリックでないクラスからパブリック クラスを派生するが、これは .NET でサポートされていません。 バインディング ジェネレーターが非パブリック クラスのバインドを生成しないので、これらを正しく生成できませんなどクラスを派生します。 これを解決するには、これらの派生クラスで削除するノードを使用して、メタデータ エントリを削除するか**Metadata.xml**、またはパブリック非パブリック クラスを行っているメタデータを修正します。 C# のソースを作成できるように、後者の方法は、バインディングを作成、パブリックでないクラスは使用できません。

    例:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Java ライブラリを難読化ツール Xamarin.Android バインディング ジェネレーターと c# のラッパー クラスを生成するには、その機能に影響します。 次のスニペットは、更新する方法を示しています。 **Metadata.xml** unobfuscate クラス名にします。

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>問題: パラメーターの型の不一致により生成された c# ソースを構築していません。

C# の場合、生成されたソースを構築しません。 メソッドのパラメーターの型が一致しないをオーバーライドします。

#### <a name="possible-causes"></a>考えられる原因:

Xamarin.Android には、さまざまな c# のバインディングで列挙型にマップされている Java フィールドが含まれています。 生成されたバインドでこれら、型の互換性がないを可能性があります。 これを解決するには、バインディング ジェネレーターから作成されたメソッドのシグネチャは、列挙型を使用するように変更する必要があります。 詳細 imformation は、次を参照してください[列挙型を修正する](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)です。

### <a name="problem-noclassdeffounderror-in-packaging"></a>問題: NoClassDefFoundError パッケージングに入れて

`java.lang.NoClassDefFoundError` パッケージのステップでがスローされます。

#### <a name="possible-causes"></a>考えられる原因:

このエラーの最も可能性の高い原因は、必須の Java ライブラリは、アプリケーション プロジェクトに追加する必要があります (**.csproj**)。 .JAR ファイルは自動的に解決されません。 Java ライブラリ バインディングは、常に対象のデバイスまたはエミュレーターに存在しないユーザー アセンブリに対しては生成されません (Google Maps など**maps.jar**)。 これは Android ライブラリ プロジェクトのサポートの場合、ライブラリです。JAR は、ライブラリ dll に埋め込まれます。 例:[バグ 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>問題: カスタムの EventArgs 型を複製します。

ビルドは、重複するカスタム EventArgs 型のため失敗します。 次のようなエラーが発生します。

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>考えられる原因:

これは、同一の名前を持つメソッドを共有する 1 つ以上のインターフェイス「リスナー」型からのイベントの種類間の競合があるためです。 たとえば、次の例のように 2 つの Java インターフェイスがある場合、ジェネレーターが作成されます。`DismissScreenEventArgs`両方の`MediationBannerListener`と`MediationInterstitialListener`、その結果、エラーにします。

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

これは仕様イベント引数の型の長い名前を回避するようにします。 これらの競合を回避するのには、いくつかのメタデータの変換が必要です。 編集[ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml)を追加し、`argsType`属性 (またはインターフェイスのメソッドで) インターフェイスのいずれかで。

```xml
<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationBannerListener']/method[@name='onDismissScreen']"
        name="argsType">BannerDismissScreenEventArgs</attr>

<attr path="/api/package[@name='com.google.ads.mediation']/
        interface[@name='MediationInterstitialListener']/method[@name='onDismissScreen']"
        name="argsType">IntersitionalDismissScreenEventArgs</attr>

<attr path="/api/package[@name='android.content']/
        interface[@name='DialogInterface.OnClickListener']"
        name="argsType">DialogClickEventArgs</attr>
```

### <a name="problem-class-does-not-implement-interface-method"></a>問題: クラスはインターフェイス メソッドを実装していません

生成されたクラスが生成されたクラスを実装するインターフェイスに必要なメソッドを実装しないことを示すエラー メッセージが生成されます。 ただし、生成されたコードを見て確認できます、メソッドが実装されています。

次に、エラーの例を示します。

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>考えられる原因:

これは、共変の戻り値の型と Java メソッドのバインディングで発生する問題です。 この例では、メソッドで`Oauth.Signpost.Http.IHttpRequest.UnWrap()`を返す必要がある`Java.Lang.Object`です。 ただし、メソッド`Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()`の戻り値の型を持つ`HttpURLConnection`します。 これにはこの問題を解決する 2 つの方法があります。

-   部分クラス宣言を追加`HttpURLConnectionRequestAdapter`を明示的に実装と`IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   生成された c# コードから共分散を削除します。 次の変換を追加する必要があります**Transforms\Metadata.xml**起こると、生成された c# コードの戻り値の型を持つ`Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>問題: 名前の内部クラスで競合/プロパティ

継承されたオブジェクトの可視性が競合しています。

Java では、必須ではありませんが、派生クラスは、その親と同じ可視性であります。 Java はだけ問題は修正します。 C# の場合、明示的に指定するが所有することを確認する必要がありますので、階層内のすべてのクラスは適切なの可視性を持ちます。 次の例から Java のパッケージ名を変更する方法を示しています`com.evernote.android.job`に`Evernote.AndroidJob`:。

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>: 問題**.so**バインディングに必要なライブラリが読み込まれていません

一部のバインド プロジェクトによっても異なりますで機能を**.so**ライブラリです。 Xamarin.Android は自動的に読み込まれません可能であれば、 **.so**ライブラリです。 Xamarin.Android は JNI 呼び出しと、エラー メッセージを失敗ラップの Java コードが実行されると、 _java.lang.UnsatisfiedLinkError: ネイティブ メソッドが見つかりません:_アプリケーションをアウト logcat に表示されます。

この修正プログラムは、手動で読み込む、 **.so**ライブラリへの呼び出しが`Java.Lang.JavaSystem.LoadLibrary`です。 たとえば Xamarin.Android プロジェクトがライブラリを共有すると仮定して**libpocketsphinx_jni.so**ビルド アクションが使用してプロジェクトをバインドに含まれる**EmbeddedNativeLibrary**では、以下スニペット (共有ライブラリを使用する前に実行) が読み込まれます、 **.so**ライブラリ。

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>まとめ

この記事では Java のバインドに関連付けられている一般的な問題のトラブルシューティングを一覧表示され、問題を解決する方法を説明しました。


## <a name="related-links"></a>関連リンク

- [ライブラリ プロジェクト](http://developer.android.com/tools/projects/index.html#LibraryProjects)
- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [診断出力を有効にします。](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Android 開発者向けの Xamarin](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
