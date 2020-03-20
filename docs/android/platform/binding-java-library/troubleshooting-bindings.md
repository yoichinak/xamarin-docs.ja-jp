---
title: バインドのトラブルシューティング
description: この記事では、バインドの生成時に発生する可能性のあるいくつかの一般的なエラー、考えられる原因、推奨される解決方法について簡単に説明します。
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 0c273797d7512f062260e49e0f71fdd1132f037b
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "76723804"
---
# <a name="troubleshooting-bindings"></a>バインドのトラブルシューティング

_この記事では、バインドの生成時に発生する可能性のあるいくつかの一般的なエラー、考えられる原因、推奨される解決方法について簡単に説明します。_

## <a name="overview"></a>概要

Android ライブラリ ( **.aar** または **.jar**) ファイルのバインドが簡単な作業であることはほとんどありません。通常は、Java と .NET の違いに起因する問題を軽減するための追加作業が必要となります。
これらの問題により、Xamarin.Android で Android ライブラリをバインドできなくなり、ビルド ログにエラー メッセージとして示されます。 このガイドでは、問題のトラブルシューティングのヒント、より一般的な問題/シナリオ、Android ライブラリを正常にバインドするための考えられる解決策を示します。

既存の Android ライブラリをバインドする場合は、次の点に注意する必要があります。

- **ライブラリの外部依存関係** &ndash; Android ライブラリに必要な Java 依存関係は、**ReferenceJar** または **EmbeddedReferenceJar** として Xamarin.Android プロジェクトに含める必要があります。

- **Android ライブラリがターゲットとしている Android API レベル** &ndash; Android API レベルを "ダウングレード" することはできません。Xamarin.Android バインド プロジェクトが、Android ライブラリと同じ API レベル (またはそれ以上) をターゲットとしていることを確認します。

- **Android ライブラリのパッケージ化に使用された Android JDK のバージョン** &ndash; Android ライブラリが、Xamarin.Android で使用されているものとは異なるバージョンの JDK でビルドされている場合、バインド エラーが発生することがあります。 可能であれば、Xamarin.Android のインストールで使用されているものと同じバージョンの JDK を使用して、Android ライブラリを再コンパイルしてください。

Xamarin.Android ライブラリのバインドに関する問題のトラブルシューティングを行うには、まず、[MSBuild の診断出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)を有効にします。
診断出力を有効にしたら、Xamarin.Android バインド プロジェクトをリビルドし、ビルド ログを調べて問題の原因に関する手掛かりを見つけます。

また、Android ライブラリを逆コンパイルし、Xamarin.Android がバインドしようとしている型やメソッドを調べることも役立ちます。 これについては、このガイドで後ほど詳しく説明します。

## <a name="decompiling-an-android-library"></a>Android ライブラリの逆コンパイル

Java のクラスと Java クラスのメソッドを調べると、ライブラリのバインドに役立つ有用な情報が得られます。
[JD-GUI](http://jd.benow.ca/) は、JAR に含まれる **CLASS** ファイルから Java ソース コードを表示できるグラフィカル ユーティリティです。 スタンドアロン アプリケーションとして実行することも、IntelliJ または Eclipse のプラグインとして実行することもできます。

Android ライブラリを逆コンパイルするには、Java 逆コンパイラで **.JAR** ファイルを開きます。 ライブラリが **.AAR** ファイルの場合は、アーカイブ ファイルから **classes.jar** ファイルを抽出する必要があります。 JD-GUI を使用した [Picasso](https://square.github.io/picasso/) JAR の解析のサンプル スクリーンショットを次に示します。

![Java 逆コンパイラを使用した picasso-2.5.2.jar の解析](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android ライブラリを逆コンパイルしたら、ソース コードを調べます。 一般に、以下を調べます。

- **難読化の特性を持つクラス** &ndash; 難読化されたクラスの特性は次のとおりです。

  - クラス名に **$** が含まれ、**a$.class** のようになります。
  - クラス名は、**a.class** のように小文字の場合は完全に危険にさらされています。      

- **参照されていないライブラリの `import` ステートメント** &ndash; 参照されていないライブラリを特定し、**ビルド アクション**が **ReferenceJar** または **EmbedddedReferenceJar** である Xamarin.Android バインド プロジェクトにそれらの依存関係を追加します。

> [!NOTE]
> Java ライブラリの逆コンパイルは、現地の法律または Java ライブラリを公開する際のライセンスに基づき、禁止されている場合や法的規制の対象となる場合があります。 Java ライブラリを逆コンパイルしてソース コードを調べる前に、必要に応じて法律専門家に相談してください。

## <a name="inspect-apixml"></a>api.xml の検査

バインド プロジェクトのビルドの一環として、Xamarin.Android では **obj/Debug/api.xml** という XML ファイルが生成されます。

![obj/Debug の下に生成された api.xml](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

このファイルは、Xamarin.Android がバインドしようとしているすべての Java API のリストを提供します。 このファイルの内容は、欠落している型またはメソッドや、重複するバインドの特定に役立ちます。 このファイルの検査は面倒で時間がかかりますが、バインドの問題の考えられる原因に関する手掛かりが得られます。 たとえば、**api.xml** によって、プロパティから不適切な型が返されていることや、同じマネージド名を共有する 2 つの型が存在することが明らかになります。

## <a name="known-issues"></a>既知の問題

このセクションでは、Android ライブラリをバインドしようとしたときに発生する可能性のある一般的なエラー メッセージや症状について説明します。

### <a name="problem-java-version-mismatch"></a>問題: Java バージョンの不一致

ライブラリのコンパイルに使用されたバージョンよりも新しいバージョンまたは古いバージョンの Java を使用しているために、型が生成されなかったり、予期しないクラッシュが発生したりすることがあります。 Xamarin.Android プロジェクトで使用しているものと同じバージョンの JDK で Android ライブラリを再コンパイルします。

### <a name="problem-at-least-one-java-library-is-required"></a>問題: Java ライブラリが少なくとも 1 つ必要

.JAR が追加されているにもかかわらず、Java ライブラリが少なくとも 1 つ必要であることを示すエラーが表示されます。

#### <a name="possible-causes"></a>考えられる原因:

ビルド アクションが `EmbeddedJar` に設定されていることを確認します。 .JAR ファイルのビルド アクションは複数あるため (`InputJar`、`EmbeddedJar`、`ReferenceJar`、`EmbeddedReferenceJar` など)、バインド ジェネレーターは既定で使用するアクションを自動的に推測することはできません。 ビルド アクションの詳細については、「[Build Actions (ビルド アクション)](~/android/platform/binding-java-library/index.md)」をご覧ください。

### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>問題: バインド ツールで .JAR ライブラリを読み込むことができない

バインド ライブラリ ジェネレーターが .JAR ライブラリの読み込みに失敗します。

#### <a name="possible-causes"></a>考えられる原因

(Proguard などのツールによる) コード難読化を使用する一部の .JAR ライブラリは、Java ツールで読み込むことができません。 これらのツールでは Java リフレクションと ASM バイト コード エンジニアリング ライブラリを使用するため、難読化されたライブラリは、Android ランタイム ツールでは通過しても、これらの依存ツールでは拒否される場合があります。 これを回避するには、バインド ジェネレーターを使用するのではなく、これらのライブラリを手動でバインドします。

### <a name="problem-missing-c-types-in-generated-output"></a>問題: 生成された出力に C# の型がない

バインド **.dll** はビルドされますが、一部の Java の型が欠落しています。または、欠落している型があることを示すエラーによって、生成された C# ソースがビルドされません。

#### <a name="possible-causes"></a>考えられる原因:

このエラーは、次のようないくつかの理由で発生する可能性があります。

- バインドされているライブラリが、別の Java ライブラリを参照している可能性があります。 バインドされたライブラリのパブリック API が別のライブラリの型を使用する場合、そのライブラリのマネージド バインドも参照する必要があります。

- 上記のライブラリ読み込みエラーの理由と同様に、Java リフレクションによってライブラリが挿入されたことが原因で、メタデータの予期しない読み込みが発生した可能性があります。 現在、Xamarin.Android のツールでは、この状況を解決することはできません。 このような場合、ライブラリを手動でバインドする必要があります。

- .NET 4.0 ランタイムには、必要なときにアセンブリの読み込みに失敗するバグがありました。 この問題は、.NET 4.5 ランタイムで修正されました。

- Java では非パブリック クラスからパブリック クラスを派生できますが、これは .NET ではサポートされていません。 バインド ジェネレーターは非パブリック クラスのバインドを生成しないため、このような派生クラスを正しく生成することはできません。 これを修正するには、**Metadata.xml** で remove-node を使用してこれらの派生クラスのメタデータ エントリを削除するか、非パブリック クラスをパブリックにするようにメタデータを修正します。 後者の解決策では、C# ソースがビルドされるようにバインドが作成されますが、非パブリック クラスを使用することはできません。

  次に例を示します。

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- Java ライブラリを難読化するツールは、Xamarin.Android のバインド ジェネレーターおよび C# ラッパー クラスを生成する機能に干渉する可能性があります。 次のスニペットは、**Metadata.xml** を更新してクラス名の難読化を解除する方法を示しています。

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>問題: パラメーターの型の不一致が原因で、生成された C# ソースがビルドされない

生成された C# ソースがビルドされません。 オーバーライドされたメソッドのパラメーターの型が一致しません。

#### <a name="possible-causes"></a>考えられる原因:

Xamarin.Android には、C# バインディングで列挙型にマップされるさまざまな Java フィールドが含まれています。 これらによって、生成されたバインディングで型の非互換性が発生する可能性があります。 これを解決するには、バインド ジェネレーターから作成されたメソッド シグネチャを変更して、列挙型を使用する必要があります。 詳細については、[列挙型の修正](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)に関する記事をご覧ください。

### <a name="problem-noclassdeffounderror-in-packaging"></a>問題: パッケージ化での NoClassDefFoundError

パッケージ化手順で `java.lang.NoClassDefFoundError` がスローされます。

#### <a name="possible-causes"></a>考えられる原因:

このエラーの最も考えられる理由は、必須の Java ライブラリをアプリケーション プロジェクト ( **.csproj**) に追加する必要があることです。 .JAR ファイルは自動的に解決されるわけではありません。 Java ライブラリのバインドは、ターゲット デバイスまたはエミュレーター (Google Maps の **maps.jar** など) に存在しないユーザー アセンブリに対して常に生成されるとは限りません。 ライブラリ .JAR はライブラリ dll に埋め込まれているため、これは Android ライブラリ プロジェクトのサポートには当てはまりません。 次に例を示します。[Bug 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>問題: カスタム EventArgs 型が重複している

カスタム EventArgs 型の重複が原因でビルドが失敗します。 次のようなエラーが発生します。

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>考えられる原因:

これは、同じ名前のメソッドを共有する複数の "リスナー" 型のインターフェイスによって、イベントの型の競合が発生するためです。 たとえば、次の例のように 2 つの Java インターフェイスがある場合、ジェネレーターは、`MediationBannerListener` と `MediationInterstitialListener` の両方に対して `DismissScreenEventArgs` を作成するため、エラーが発生します。

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

これは、イベントの引数の型の長い名前を回避するための仕様です。 これらの競合を回避するには、何らかのメタデータ変換が必要です。 **Transforms\Metadata.xml** を編集し、いずれかのインターフェイス (またはインターフェイス メソッド）に `argsType` 属性を追加します)。

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

### <a name="problem-class-does-not-implement-interface-method"></a>問題: クラスがインターフェイス メソッドを実装していない

生成されたクラスが実装しているインターフェイスに必要なメソッドをそのクラスが実装しないことを示すエラー メッセージが生成されます。 しかし、生成されたコードを見ると、そのメソッドが実装されていることがわかります。

このエラーの例を次に示します。

```shell
obj\Debug\generated\src\Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.cs(8,23):
error CS0738: 'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter' does not
implement interface member 'Oauth.Signpost.Http.IHttpRequest.Unwrap()'.
'Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.Unwrap()' cannot implement
'Oauth.Signpost.Http.IHttpRequest.Unwrap()' because it does not have the matching
return type of 'Java.Lang.Object'
```

#### <a name="possible-causes"></a>考えられる原因:

この問題は、Java メソッドを共変の戻り値の型にバインドしたときに発生します。 この例では、`Oauth.Signpost.Http.IHttpRequest.UnWrap()` メソッドは `Java.Lang.Object` を返す必要があります。 しかし、`Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` メソッドの戻り値の型は `HttpURLConnection` です。 この問題を修正するには、次の 2 つの方法があります。

- `HttpURLConnectionRequestAdapter` の部分クラスの宣言を追加し、`IHttpRequest.Unwrap()` を明示的に実装します。

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- 生成された C# コードから共変性を削除します。 これを行うには、**Transforms\Metadata.xml** に次の変換を追加します。これにより、生成された C# コードの戻り値の型が `Java.Lang.Object` になります。

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>問題: 内部クラス/プロパティでの名前の競合

継承されたオブジェクトの可視性が競合しています。

Java では、派生クラスの可視性がその親と同じである必要はありません。 これは自動的に修正されます。 C# では、これは明示的である必要があるため、階層内のすべてのクラスの可視性が適切であることを確認する必要があります。 次の例は、Java パッケージ名を `com.evernote.android.job` から `Evernote.AndroidJob` に変更する方法を示しています。

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>問題: バインドに必要な **.so** ライブラリが読み込まれない

一部のバインド プロジェクトは、 **.so** ライブラリの機能に依存している場合があります。 Xamarin.Android で **.so** ライブラリが自動的に読み込まれない可能性があります。 ラップされた Java コードを実行すると、Xamarin.Android は JNI の呼び出しに失敗し、アプリケーションの logcat 出力に、_java.lang.UnsatisfiedLinkError: Native method not found:_ というエラー メッセージが表示されます。

これを解決するには、`Java.Lang.JavaSystem.LoadLibrary` を呼び出して **.so** ライブラリを手動で読み込みます。 たとえば、Xamarin.Android プロジェクトに、ビルド アクションが **EmbeddedNativeLibrary** のバインド プロジェクトに含まれる共有ライブラリ **libpocketsphinx_jni.so** があるとします。次のスニペット (共有ライブラリを使用する前に実行) によって **.so** ライブラリが読み込まれます。

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>まとめ

この記事では、Java バインディングに関連する一般的なトラブルシューティングの問題を示し、それらの解決方法を説明しました。

## <a name="related-links"></a>関連リンク

- [ライブラリ プロジェクト](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [JNI の使用](~/android/platform/java-integration/working-with-jni.md)
- [診断出力の有効化](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Android 開発者向け Xamarin](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
