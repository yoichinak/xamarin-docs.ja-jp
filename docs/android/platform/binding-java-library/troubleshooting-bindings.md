---
title: バインドのトラブルシューティング
description: この記事では、考えられる原因と問題を解決する方法をお勧めのバインドを生成するときに発生するいくつかの一般的なエラーを説明します。
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: b0bb7cbb6160865af5b1e40d40c7b999a8bd5ebc
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668518"
---
# <a name="troubleshooting-bindings"></a>バインドのトラブルシューティング

_この記事では、考えられる原因と問題を解決する方法をお勧めのバインドを生成するときに発生するいくつかの一般的なエラーを説明します。_


## <a name="overview"></a>概要

Android のライブラリのバインド (**.aar**または **.jar**) ファイルは、簡単な例ではめったにありません。 追加の作業を Java と .NET 間の違いに起因する問題を軽減するために通常必要があります。
これらの問題は Xamarin.Android の Android ライブラリのバインドを防止し、ビルド ログのエラー メッセージとして提供します。 このガイドは、問題のトラブルシューティングのヒントを提供より一般的な問題/シナリオの一部を一覧表示し、Android ライブラリを正常にバインド可能なソリューションを提供します。

既存の Android ライブラリをバインドするときに、次の点に注意する必要があります。

- **ライブラリの外部の依存関係**&ndash;として Xamarin.Android プロジェクトでは、Android ライブラリで必要な任意の Java 依存関係を含める必要がある、 **ReferenceJar**か、または、 **EmbeddedReferenceJar**します。

- **Android のライブラリがターゲットとし、Android API レベル** &ndash; Android API レベルを「ダウン グレード」することはできません。 Xamarin.Android バインド プロジェクトのレベル (またはそれ以上) に同じ API をターゲットはことを確認しますが、Android ライブラリとして。

- **Android、Android ライブラリをパッケージ化に使用された JDK のバージョン** &ndash; Android ライブラリの Xamarin.Android で使用しているものよりも JDK の異なるバージョンで作成した場合にバインド エラーが発生する可能性があります。 可能であれば、同じバージョンの Xamarin.Android のインストールで使用される JDK を使用して、Android ライブラリを再コンパイルします。

有効にするのには、Xamarin.Android ライブラリのバインドに関する問題のトラブルシューティングをまず[診断 MSBuild 出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)します。
診断の出力を有効にした後 Xamarin.Android バインド プロジェクトをリビルドし、問題の原因に関する手がかりを検索するビルド ログを確認します。

これには、Android ライブラリの逆をバインドしようとして Xamarin.Android メソッドや型を確認するのに役立ちますも証明できます。 これについては、このガイドで後で詳しく説明します。


## <a name="decompiling-an-android-library"></a>加えて、Android ライブラリ

クラスと Java クラスのメソッドを調べると、ライブラリのバインドに役立つ貴重な情報を提供できます。
[JD GUI](http://jd.benow.ca/) 、グラフィカルなユーティリティからの Java ソース コードを表示できるは、**クラス**JAR に含まれるファイル。 実行できますスタンドアロン アプリケーションとして、またはプラグインとして IntelliJ または Eclipse 用。

デコンパイル、Android ライブラリのオープンを**します。JAR** Java デコンパイラを持つファイル。 ライブラリがある場合、**します。AAR**ファイルを抽出する必要があるファイル、 **classes.jar**アーカイブ ファイルから。 JD GUI を使用した分析のサンプルのスクリーン ショットを次に、[Picasso](http://square.github.io/picasso/)JAR:

![Java デコンパイラを使用して、picasso-2.5.2.jar を分析するには](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android のライブラリは、逆コンパイルされたが後、は、ソース コードを調べます。 一般に、検索対象。

- **難読化の特性を持つクラス**&ndash;難読化されたクラスの特性が含まれます。

    - クラス名が含まれています、 **$**、つまり **$.class**
    - クラス名がつまり侵害の小文字の場合は、まったく**a.class**      

- **`import` ステートメントが参照されていないライブラリの**&ndash;参照されていないライブラリを識別し、これらの依存関係を使用して Xamarin.Android バインド プロジェクトに追加、**ビルド アクション**の**ReferenceJar**または**EmbedddedReferenceJar**します。

> [!NOTE]
> Java ライブラリを加えて禁止または法的制限の対象は、現地の法律または Java ライブラリが公開されたこのライセンスに基づきます。 必要に応じて、Java ライブラリをコンパイルし、ソース コードを調査する前に、法務担当者のサービスを登録します。


## <a name="inspect-apixml"></a>API を検査します。XML

Xamarin.Android バインド プロジェクトのビルドの一部、として、XML ファイル名が生成されます**obj/Debug/api.xml**:

![生成された api.xml obj/デバッグ](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

このファイルは、Xamarin.Android バインドが試行されているすべての Java Api の一覧を提供します。 このファイルの内容、不足している型またはメソッドを識別、重複するバインドを使用できます。 このファイルの検査には、面倒で時間がかかりますが、任意のバインドの問題の原因の手がかりを提供できます。 たとえば、 **api.xml**プロパティが、不適切な型を返すことや、その共有と同じマネージ名の種類が 2 つのことを明らかに可能性があります。


## <a name="known-issues"></a>既知の問題

このセクションでは、いくつかの一般的なエラー メッセージまたは現象は一覧を Android のライブラリをバインドしようとするときに発生します。


### <a name="problem-java-version-mismatch"></a>問題 : Java のバージョンが一致しません

場合によって、型は生成されませんまたは新しいまたは古いバージョンでコンパイルされたライブラリと比較する Java のいずれかを使用しているために、予期しないクラッシュが発生する可能性があります。 同じバージョンの Xamarin.Android プロジェクトを使用している JDK と Android のライブラリを再コンパイルします。


### <a name="problem-at-least-one-java-library-is-required"></a>問題 : 少なくとも 1 つの Java ライブラリが必要です。

エラーが発生した「少なくとも 1 つの Java ライブラリが必要な場合は、」場合でも、します。JAR が追加されました。

#### <a name="possible-causes"></a>考えられる原因:

必ずビルド アクションに設定されて`EmbeddedJar`します。 複数のビルド アクションがあるためです。JAR ファイル (など`InputJar`、 `EmbeddedJar`、`ReferenceJar`と`EmbeddedReferenceJar`)、バインド ジェネレーターは、既定で使用するを推測できない自動的にします。 ビルド アクションの詳細については、[構築アクション](~/android/platform/binding-java-library/index.md)を参照してください。


### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>問題 : バインディング ツールを読み込むことはできません、します。JAR ライブラリ

バインディング ライブラリ コード ジェネレーターの読み込みに失敗します。JAR ライブラリです。

#### <a name="possible-causes"></a>考えられる原因

いくつか。Java ツールでは、(Proguard などのツール) を使用してコードの難読化を使用して、JAR ライブラリを読み込むことができません。 のツールを使用する Java リフレクションを使用して ASM バイトのコード ライブラリをエンジニア リングため、これらのツールが依存は可能性があります Android ランタイム ツールに渡すことがあります、難読化されたライブラリを拒否します。 この回避策は、バインド ジェネレーターを使用する代わりにこれらのライブラリを手にバインドします。



### <a name="problem-missing-c-types-in-generated-output"></a>問題 : 不足しているC#で生成された出力の種類。

バインディング **.dll**ビルドしますが、一部の種類の Java または生成に失敗したC#ソースが不足している種類があることを示すエラーのために構築されません。

#### <a name="possible-causes"></a>考えられる原因:

このエラーは、次に示すようにいくつかの理由により発生可能性があります。

-   バインドされているライブラリには、2 つ目の Java ライブラリを参照できます。 バインドされているライブラリのパブリック API では、2 つ目のライブラリから型を使用している場合も、2 つ目のライブラリの管理対象のバインドを参照する必要があります。

-   ライブラリが原因で予期しないメタデータの読み込み、上記のライブラリの読み込みエラーの理由のような Java リフレクションにより挿入されたことができます。 Xamarin.Android のツールは、このような状況を現在解決できません。 このような場合は、ライブラリを手動でバインドする必要があります。

-   .NET 4.0 ランタイムがある必要時にアセンブリの読み込みに失敗したにバグがありました。 この問題は .NET 4.5 ランタイムで修正されました。

-   Java が非パブリックのクラスからパブリック クラスの派生をできますが、これは .NET でサポートされていません。 バインディング ジェネレーターがパブリックでないクラスのバインドを生成しないので、これらを正しく生成できませんなど、クラスを派生します。 これを解決するには、[削除] ノードを使用してこれらの派生クラスのメタデータ エントリを削除するか**Metadata.xml**、またはパブリック、パブリックでないクラスを行っているメタデータを修正します。 後者の方法は、バインドを作成できるように、C#ソースのビルドは、パブリックでないクラスは使用できません。

    例えば:

    ```xml
    <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
        name="visibility">public</attr>
    ```

-   Java ライブラリを難読化ツールは、Xamarin.Android バインド ジェネレーターおよび生成するには、その機能を妨げる可能性がC#ラッパー クラス。 次のスニペットでは、更新する方法を示しています。 **Metadata.xml** unobfuscate クラス名にします。

    ```xml
    <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
        name="obfuscated">false</attr>
    ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>問題 : 生成されたC#パラメーターの型が一致しないのためのソースを構築していません。

生成されたC#ソースをビルドしません。 型が一致しないメソッドのパラメーターをオーバーライドします。

#### <a name="possible-causes"></a>考えられる原因:

Xamarin.Android には、さまざまなで列挙型にマップされている Java フィールドが含まれています、C#バインドします。 生成されたバインドで型の非互換性をによってことができます。 これを解決するには、バインディング ジェネレーターから作成されたメソッドのシグネチャは、列挙型を使用するように変更する必要があります。 詳細についてを参照してください[列挙型を修正する](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)します。

### <a name="problem-noclassdeffounderror-in-packaging"></a>問題 : パッケージで NoClassDefFoundError

`java.lang.NoClassDefFoundError` パッケージ化の手順でスローされます。

#### <a name="possible-causes"></a>考えられる原因:

このエラーの最も可能性の高い理由は、必須の Java ライブラリは、アプリケーション プロジェクトに追加する必要があります (**.csproj**)。 .JAR ファイルは自動的に解決されません。 Java ライブラリのバインドは、常に、ターゲット デバイスまたはエミュレーターで存在しないユーザー アセンブリに対しては生成されません (Google Maps など**maps.jar**)。 これは Android ライブラリ プロジェクトのサポートの場合、ライブラリです。JAR は、ライブラリの dll に埋め込まれます。 例:[バグ 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>問題 : カスタムの EventArgs 型が重複しています

ビルドは、重複するカスタムの EventArgs 型のため失敗します。 このようなエラーが発生します。

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>考えられる原因:

同一の名前を持つメソッドを共有する 1 つ以上のインターフェイス「リスナー」型に由来するイベントの種類の間にいくつかの競合があるためにです。 たとえば、次の例のように 2 つの Java インターフェイスがある場合、ジェネレーターが作成されます。`DismissScreenEventArgs`両方の`MediationBannerListener`と`MediationInterstitialListener`、エラーが発生します。

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

これは、長い名前のイベント引数の型が回避されるよう設計します。 これらの競合を回避するには、いくつかのメタデータの変換が必要です。 編集[ **Transforms\Metadata.xml** ](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml)を追加し、`argsType`インターフェイスのどちらかで (またはインターフェイスのメソッド) の属性。

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

### <a name="problem-class-does-not-implement-interface-method"></a>問題 : クラスがインターフェイス メソッドを実装していません

生成されたクラスが生成されたクラスを実装するインターフェイスに必要なメソッドを実装しないことを示すエラー メッセージが生成されます。 ただし、生成されたコードを見て確認できます、メソッドが実装されています。

エラーの例を次に示します。

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>考えられる原因:

これは、共変の戻り値の型と Java のメソッドのバインドで発生する問題です。 この例では、メソッドで`Oauth.Signpost.Http.IHttpRequest.UnWrap()`返す必要があります`Java.Lang.Object`します。 ただし、メソッド、`Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()`の戻り値の型を持つ`HttpURLConnection`します。 この問題を解決する 2 つの方法はあります。

-   部分クラス宣言を追加`HttpURLConnectionRequestAdapter`を明示的に実装および`IHttpRequest.Unwrap()`:

    ```csharp
    namespace Oauth.Signpost.Basic {
        partial class HttpURLConnectionRequestAdapter {
            Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
                return Unwrap();
            }
        }
    }
    ```

-   生成された共分散を削除するC#コード。 次の変換を追加する必要があります**Transforms\Metadata.xml** 、生成されたはC#コードの戻り値の型に`Java.Lang.Object`:

    ```xml
    <attr
        path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
        name="managedReturn">Java.Lang.Object
    </attr>
    ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>問題 : 名前の内部クラスで競合/プロパティ

継承されたオブジェクトの可視性を競合しています。

Java では、派生クラスにその親と同じ可視性があることを必要ことはありません。 Java の修正だけですが。 C#、明示的に指定するが、階層内のすべてのクラスが適切な可視性があることを確認する必要があるためです。 次の例から Java パッケージ名を変更する方法を示しています`com.evernote.android.job`に`Evernote.AndroidJob`:。

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>問題 : A **.so**バインドで必要なライブラリが読み込まれていません

一部のバインド プロジェクトによっても異なります機能、 **.so**ライブラリ。 Xamarin.Android は自動的に読み込まれませんことができます、 **.so**ライブラリ。 Xamarin.Android は JNI の呼び出しと、エラー メッセージを失敗して、ラップされた Java コードの実行時に_java.lang.UnsatisfiedLinkError:ネイティブ メソッドが見つかりません:_ アプリケーションのアウト logcat に表示されます。

この修正プログラムは、手動で読み込む、 **.so**ライブラリへの呼び出しで`Java.Lang.JavaSystem.LoadLibrary`します。 たとえば、Xamarin.Android プロジェクトに共有ライブラリがあると仮定すると**libpocketsphinx_jni.so**のビルド アクションを使用してプロジェクトをバインドに含まれる**EmbeddedNativeLibrary**、次のスニペット(共有ライブラリを使用する前に実行) が読み込まれます、 **.so**ライブラリ。

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>まとめ

この記事では Java のバインドに関連付けられている一般的な問題のトラブルシューティングを一覧表示され、問題を解決する方法について説明しました。


## <a name="related-links"></a>関連リンク

- [ライブラリ プロジェクト](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [診断出力を有効にします。](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Android 開発者向けの Xamarin](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
