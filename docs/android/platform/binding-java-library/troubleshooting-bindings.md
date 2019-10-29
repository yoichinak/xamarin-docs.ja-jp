---
title: バインドのトラブルシューティング
description: この記事では、バインドの生成時に発生する可能性のある一般的なエラーの概要と、考えられる原因と解決方法について説明します。いくつかです。
ms.prod: xamarin
ms.assetid: BB81FCCF-F7BF-4C78-884E-F02C49AA819A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 2eea51764e0e0f13c1a1a91db664872a67420d33
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020556"
---
# <a name="troubleshooting-bindings"></a>バインドのトラブルシューティング

_この記事では、バインドの生成時に発生する可能性のある一般的なエラーの概要と、考えられる原因と解決方法について説明します。いくつかです。_

## <a name="overview"></a>概要

Android ライブラリ ( **aar**または **.jar**) ファイルのバインドは、ほとんどの場合、単純申し込みです。通常、Java と .NET の違いに起因する問題を軽減するには、追加の作業が必要です。
これらの問題により、Xamarin android は Android ライブラリをバインドできなくなり、ビルドログにエラーメッセージとして表示されます。 このガイドでは、問題のトラブルシューティングを行うためのヒントを紹介し、いくつかの一般的な問題とシナリオの一覧を示し、Android ライブラリを正常にバインドするための解決策を提供します。

既存の Android ライブラリをバインドする場合は、次の点に注意する必要があります。

- Android ライブラリに必要な Java 依存関係 &ndash;**ライブラリの外部依存関係は、** **Referencejar**または**EmbeddedReferenceJar**として Xamarin プロジェクトに含まれている必要があります。

- Android ライブラリがターゲットとして**いる ANDROID api レベル**&ndash; android api レベルを "ダウングレード" することはできません。Xamarin. Android バインドプロジェクトが、Android ライブラリと同じ API レベル (またはそれ以降) を対象としていることを確認します。

- Android ライブラリを**パッケージ化するために使用した ANDROID JDK のバージョンは、** android ライブラリが、Xamarin android で使用されているものとは異なるバージョンの JDK でビルドされた場合に発生する可能性があり &ndash;。 可能であれば、Xamarin Android のインストールで使用されているものと同じバージョンの JDK を使用して、Android ライブラリを再コンパイルします。

Xamarin Android ライブラリのバインドに関する問題をトラブルシューティングするための最初の手順は、[診断 MSBuild の出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)を有効にすることです。
診断出力を有効にした後、Xamarin. Android バインドプロジェクトをリビルドし、ビルドログを調べて、問題の原因に関する手掛かりを見つけます。

また、Android ライブラリを逆コンパイルして、Xamarin がバインドしようとしている型とメソッドを確認すると役立つ場合があります。 詳細については、このガイドの後半で説明します。

## <a name="decompiling-an-android-library"></a>Android ライブラリの逆コンパイル

Java クラスのクラスとメソッドを検査すると、ライブラリのバインドに役立つ有用な情報が得られます。
[Jd-GUI](http://jd.benow.ca/)は、JAR に含まれる**クラス**ファイルから Java ソースコードを表示できるグラフィカルユーティリティです。 スタンドアロンアプリケーションとして、または IntelliJ または Eclipse のプラグインとして実行できます。

Android ライブラリをデコンパイルするには、を開き**ます。** Java デコンパイラの JAR ファイル。 ライブラリがの場合 **。AAR**ファイルの場合は、アーカイブファイルから**jar**ファイルを抽出する必要があります。 JD-GUI を使用して[ピカソ](https://square.github.io/picasso/)JAR を分析するサンプルスクリーンショットを次に示します。

![Java デコンパイラを使用した picasso-2.5.2 の分析](troubleshooting-bindings-images/troubleshoot-bindings-01.png)

Android ライブラリのデコンパイルが完了したら、ソースコードを確認します。 一般に、次のものを探します。

- 難読化されたクラスの難読化 &ndash; 特性を**持つクラスに**は、次のようなものがあります。

  - クラス名には、 **$** 、つまり **$. クラス**が含まれています。
  - クラス名は、小文字、つまり **. クラス**で完全に侵害されます。      

- 参照されていないライブラリ**のステートメントを`import`** &ndash; 参照されていないライブラリを特定し、 **Referencejar**または EmbedddedReferenceJar の**ビルドアクション**を使用してそれらの依存関係を Xamarin. Android バインドプロジェクトに追加します。.

> [!NOTE]
> 逆コンパイル Java ライブラリが禁止されている可能性があります。または、Java ライブラリが公開されている地域の法律またはライセンスに基づいて、法的な制限が適用される場合があります。 必要に応じて、Java ライブラリをデコンパイルしてソースコードを検査する前に、法的担当者のサービスを参加させます。

## <a name="inspect-apixml"></a>API を検査します。XML

バインドプロジェクトのビルドの一部として、Xamarin Android では、XML ファイル名**obj/Debug/api .xml**が生成されます。

![生成された api .xml が obj/Debug にあります](troubleshooting-bindings-images/troubleshoot-bindings-02.png)

このファイルには、Xamarin Android でバインドを試行しているすべての Java Api の一覧が表示されます。 このファイルの内容は、欠落している型またはメソッドを特定するのに役立ち、重複するバインドです。 このファイルの検査は面倒で時間がかかりますが、どのようなバインドの問題が発生しているかについての手掛かりを提供できます。 たとえば、 **api .xml**では、プロパティが不適切な型を返していることや、同じマネージ名を共有する2つの型があることが明らかになる場合があります。

## <a name="known-issues"></a>既知の問題

このセクションでは、Android ライブラリをバインドしようとしたときに発生する一般的なエラーメッセージまたは現象の一部を示します。

### <a name="problem-java-version-mismatch"></a>問題: Java バージョンの不一致

ライブラリがコンパイルされた場合と比較して、新しいバージョンまたは古いバージョンの Java を使用している場合、型が生成されないか、予期しないクラッシュが発生することがあります。 Xamarin Android プロジェクトで使用している JDK と同じバージョンの Android ライブラリを再コンパイルします。

### <a name="problem-at-least-one-java-library-is-required"></a>問題: 少なくとも1つの Java ライブラリが必要です

にもかかわらず、"少なくとも1つの Java ライブラリが必要です" というエラーが表示されます。JAR が追加されました。

#### <a name="possible-causes"></a>考えられる原因:

ビルドアクションが `EmbeddedJar`に設定されていることを確認します。 には複数のビルドアクションがあるためです。JAR ファイル (`InputJar`、`EmbeddedJar`、`ReferenceJar`、`EmbeddedReferenceJar`など) では、バインドジェネレーターが既定で使用する JAR ファイルを自動的に推測することはできません。 ビルドアクションの詳細については、「[ビルドアクション](~/android/platform/binding-java-library/index.md)」を参照してください。

### <a name="problem-binding-tools-cannot-load-the-jar-library"></a>問題: バインドツールでを読み込めません。JAR ライブラリ

バインドライブラリジェネレーターがの読み込みに失敗しました。JAR ライブラリ。

#### <a name="possible-causes"></a>考えられる原因

一部.コード難読化を使用する JAR ライブラリ (Proguard などのツールを使用) を Java ツールで読み込むことはできません。 このツールでは Java リフレクションと ASM バイトコードエンジニアリングライブラリが使用されるため、これらの依存ツールは、Android ランタイムツールが成功したときに、難読化されたライブラリを拒否する場合があります。 これを回避するには、バインディングジェネレーターを使用する代わりに、これらのライブラリを手動でバインドします。

### <a name="problem-missing-c-types-in-generated-output"></a>問題: 生成C#された出力に型がありません。

バインド .dll は一部の Java 型をビルドし**ます**が、不足C#している型があることを示すエラーのために生成されたソースはビルドしません。

#### <a name="possible-causes"></a>考えられる原因:

このエラーは、次に示すいくつかの理由により発生する可能性があります。

- バインドされているライブラリが2番目の Java ライブラリを参照している可能性があります。 バインドされたライブラリのパブリック API が2番目のライブラリの型を使用している場合は、2番目のライブラリのマネージバインドも参照する必要があります。

- 上記のライブラリの読み込みエラーの理由と同様に、ライブラリが Java リフレクションによって挿入されたことが原因で、メタデータの予期しない読み込みが発生する可能性があります。 現在、Xamarin Android のツールでは、この状況を解決できません。 このような場合は、ライブラリを手動でバインドする必要があります。

- .NET 4.0 ランタイムで、アセンブリの読み込みに失敗したバグが発生しました。 この問題は、.NET 4.5 ランタイムで修正されました。

- Java では、パブリッククラス以外のクラスからパブリッククラスを派生させることができますが、これは .NET ではサポートされていません。 バインディングジェネレーターは、パブリックでないクラスのバインドを生成しないため、このような派生クラスを正しく生成することはできません。 この問題を解決するには、 **metadata .xml**で削除ノードを使用してこれらの派生クラスのメタデータエントリを削除するか、パブリックでないクラスを公開するメタデータを修正します。 後者のソリューションでは、 C#ソースがビルドされるようにバインディングが作成されますが、非パブリッククラスを使用することはできません。

  (例:

  ```xml
  <attr path="/api/package[@name='com.some.package']/class[@name='SomeClass']"
      name="visibility">public</attr>
  ```

- Java ライブラリを難読化するツールは、Xamarin の Android バインドジェネレーターとラッパークラスを生成C#する機能に干渉する可能性があります。 次のスニペットは、unobfuscate を更新してクラス名を変更する方法を示して**い**ます。

  ```xml
  <attr path="/api/package[@name='{package_name}']/class[@name='{name}']"
      name="obfuscated">false</attr>
  ```

### <a name="problem-generated-c-source-does-not-build-due-to-parameter-type-mismatch"></a>問題: パラメーター C#の型が一致しないため、生成されたソースはビルドされません

生成さC#れたソースはビルドされません。 オーバーライドされたメソッドのパラメーターの型が一致しません。

#### <a name="possible-causes"></a>考えられる原因:

Xamarin. Android には、 C#バインド内の列挙にマップされるさまざまな Java フィールドが含まれています。 これらの場合、生成されたバインディングで型の非互換性が発生する可能性があります。 これを解決するには、バインドジェネレーターから作成されたメソッドシグネチャを、列挙型を使用するように変更する必要があります。 詳細については、「列挙型の[修正](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md)」を参照してください。

### <a name="problem-noclassdeffounderror-in-packaging"></a>問題: パッケージの NoClassDefFoundError

パッケージ化の手順で `java.lang.NoClassDefFoundError` がスローされます。

#### <a name="possible-causes"></a>考えられる原因:

このエラーの原因として最も可能性が高いのは、必須の Java ライブラリをアプリケーションプロジェクト ( **.csproj**) に追加する必要があることです。 .JAR ファイルは自動的には解決されません。 Java ライブラリのバインドは、ターゲットデバイスまたはエミュレーターに存在しないユーザーアセンブリに対して常に生成されるとは限りません (Google Maps **maps. jar**など)。 これは、ライブラリとしての Android ライブラリプロジェクトのサポートには当てはまりません。JAR はライブラリ dll に埋め込まれています。 例: [Bug 4288](https://bugzilla.xamarin.com/show_bug.cgi?id=4288)

### <a name="problem-duplicate-custom-eventargs-types"></a>問題: カスタム EventArgs の種類が重複しています

カスタム EventArgs の種類が重複しているため、ビルドに失敗します。 次のようなエラーが発生します。

```shell
error CS0102: The type `Com.Google.Ads.Mediation.DismissScreenEventArgs' already contains a definition for `p0'
```

#### <a name="possible-causes"></a>考えられる原因:

これは、同じ名前を持つメソッドを共有する複数のインターフェイス "リスナ" 型に由来するイベントの種類が競合するためです。 たとえば、次の例に示すように Java インターフェイスが2つある場合、ジェネレーターは `MediationBannerListener` と `MediationInterstitialListener`の両方に対して `DismissScreenEventArgs` を作成し、エラーが発生します。

```java
// Java:
public interface MediationBannerListener {
    void onDismissScreen(MediationBannerAdapter p0);
}
public interface MediationInterstitialListener {
    void onDismissScreen(MediationInterstitialAdapter p0);
}
```

これは、イベント引数の型の長い名前が回避されるように設計されています。 これらの競合を回避するには、いくつかのメタデータ変換が必要です。 [**Transformraxml**](https://github.com/xamarin/monodroid-samples/blob/master/AdMob/AdMob/Transforms/Metadata.xml)を編集し、いずれかのインターフェイス (またはインターフェイスメソッド) に `argsType` 属性を追加します。

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

### <a name="problem-class-does-not-implement-interface-method"></a>問題: クラスがインターフェイスメソッドを実装していません

生成されたクラスが実装しているインターフェイスに必要なメソッドが実装されていないことを示すエラーメッセージが生成されます。 ただし、生成されたコードを見ると、メソッドが実装されていることがわかります。

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

これは、Java メソッドを共変の戻り値の型と共にバインドする場合に発生する問題です。 この例では、メソッド `Oauth.Signpost.Http.IHttpRequest.UnWrap()` は `Java.Lang.Object`を返す必要があります。 ただし、メソッド `Oauth.Signpost.Basic.HttpURLConnectionRequestAdapter.UnWrap()` には `HttpURLConnection`の戻り値の型があります。 この問題を解決するには、次の2つの方法があります。

- `HttpURLConnectionRequestAdapter` の部分クラス宣言を追加し、`IHttpRequest.Unwrap()`を明示的に実装します。

  ```csharp
  namespace Oauth.Signpost.Basic {
      partial class HttpURLConnectionRequestAdapter {
          Java.Lang.Object OauthSignpost.Http.IHttpRequest.Unwrap() {
              return Unwrap();
          }
      }
  }
  ```

- 生成さC#れたコードから共変性を削除します。 これには、次の変換を**transformthe xml**に追加する必要がありC#ます。これにより、生成されたコードの戻り値の型が`Java.Lang.Object`になります。

  ```xml
  <attr
      path="/api/package[@name='oauth.signpost.basic']/class[@name='HttpURLConnectionRequestAdapter']/method[@name='unwrap']"
      name="managedReturn">Java.Lang.Object
  </attr>
  ```

### <a name="problem-name-collisions-on-inner-classes--properties"></a>問題: 内部クラス/プロパティの名前の競合

継承されたオブジェクトの可視性が競合しています。

Java では、派生クラスの可視性が親と同じである必要はありません。 Java では、これを修正するだけです。 でC#は、これが明示的である必要があるため、階層内のすべてのクラスに適切な可視性があることを確認する必要があります。 次の例では、Java パッケージ名を `com.evernote.android.job` から `Evernote.AndroidJob`に変更する方法を示します。

```xml
<!-- Change the visibility of a class -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']" name="visibility">public</attr>

<!-- Change the visibility of a method -->
<attr path="/api/package[@name='namespace']/class[@name='ClassName']/method[@name='MethodName']" name="visibility">public</attr>
```

### <a name="problem-a-so-library-required-by-the-binding-is-not-loading"></a>問題: バインドに必要なライブラリが読み込まれ**ていませ**ん

バインドプロジェクトによっては、 **...** ライブラリの機能に依存している場合もあります。 Xamarin. Android では、 **...** ライブラリが自動的に読み込まれない可能性があります。 ラップされた Java コードを実行すると、JNI の呼び出しに失敗し、エラーメッセージ_UnsatisfiedLinkError: ネイティブメソッドが見つからないことを示します_。は、アプリケーションの logcat out に表示されます。

この問題を解決するには、`Java.Lang.JavaSystem.LoadLibrary`の呼び出しを使用して、 **..** . ライブラリを手動で読み込みます。 たとえば、Xamarin Android プロジェクトに共有ライブラリ libpocketsphinx_jni が含まれていると**します。そのため**、 **EmbeddedNativeLibrary**のビルドアクションを使用してバインドプロジェクトにインクルードします。次のスニペット (共有ライブラリを使用する前に実行)は、 **...** ライブラリを読み込みます。

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="summary"></a>まとめ

この記事では、Java バインディングに関連する一般的なトラブルシューティングの問題とその解決方法について説明しました。

## <a name="related-links"></a>関連リンク

- [ライブラリプロジェクト](https://developer.android.com/tools/projects/index.html#LibraryProjects)
- [JNI の操作](~/android/platform/java-integration/working-with-jni.md)
- [診断出力を有効にする](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)
- [Android 開発者向け Xamarin](~/android/get-started/java-developers.md)
- [JD-GUI](http://jd.benow.ca/)
