---
title: Xamarin のトラブルシューティングのヒント
description: このドキュメントでは、Xamarin iOS アプリケーションの開発時のトラブルシューティングに役立つさまざまなヒントを提供します。 具体的なエラーメッセージと、その他の潜在的な問題について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/22/2018
ms.openlocfilehash: 716999002cf90b50b90f4924adc11555cc43717f
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915360"
---
# <a name="troubleshooting-tips-for-xamarinios"></a>Xamarin のトラブルシューティングのヒント

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin. iOS では、ValueTuple を解決できません。

このエラーは、Visual Studio との互換性がないことが原因で発生します。

- **Visual Studio 2017 Update 1** (バージョン15.1 以前) は、 **System. valuetuple NuGet 4.3.0** (またはそれ以前) とのみ互換性があります。

- **Visual Studio 2017 Update 2** (バージョン15.2 以降) は、4.3.1 またはそれ以降のバージョンとのみ互換性があり**ます**。

Visual Studio 2017 のインストールに対応する適切な system.servicemodel タプル NuGet を選択してください。

## <a name="receiving-error-retrieving-update-information-error-message"></a>' 更新情報の取得エラー ' エラーメッセージを受信しています

ソフトウェアを更新しようとしたときに、このエラーメッセージが表示された場合は、ide を再起動してログアウトし、IDE でアカウントに再度ログインしてみてください。

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>Interface Builder でアウトレットまたはアクションを作成操作方法には

Visual Studio for Mac と Visual Studio の Xamarin Designer for iOS の導入により、Xamarin の開発者はストーリーボードと xib を使用して UI を作成できるようになりました。 デザイナーの使用方法の詳細については、 [「Hello, iOS ガイド」](~/ios/get-started/hello-ios/index.md)を参照してください。

また、IB でのアウトレットとアクションの使用方法の詳細については、Apple の[アウトレット](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html)と[操作](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html)に関するガイドを参照してください。

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>Encoding.getencoding が NotSupportedException をスローする

既定では追加されないエンコーディングを使用している可能性があります。 エンコードのサポートを追加する方法については、[国際化](~/ios/app-fundamentals/localization/index.md)に関するページをご覧ください。

## <a name="systemmissingmethodexception-anything-else"></a>MissingMethodException (その他)

このメンバーはリンカーによって削除される可能性があるため、実行時にアセンブリに存在しません。  これには、次のようないくつかのソリューションがあります。

- [`[Preserve]`](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute)属性をメンバーに追加します。  これにより、リンカーによる削除が防止されます。
- [**Mtouch**](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29)を呼び出すときは、 **-nolink**オプションまたは **-linksdkonly**オプションを使用します。
  - **-nolink は、すべての**リンクを無効にします。
  - **-linksdkonly**では、ユーザーが作成したアセンブリ (つまり、アプリプロジェクト) 内のすべての種類を**保持しながら**、xamarin などの ios 提供のアセンブリのみをリンクします。

アセンブリがリンクされ、結果の実行可能ファイルが小さくなることに注意してください。そのため、リンクを無効にすると、実行可能ファイルの方が望ましいよりも大きくなる可能性があります。

## <a name="you-are-getting-a-modelnotimplementedexception"></a>ModelNotImplementedException を取得しています

この例外が発生した場合は、base を呼び出していることを意味します。モデルをオーバーライドするクラスのメソッド ()。 モデルのクラスで基本メソッドを呼び出す必要はありません (これらは、[モデル] 属性でフラグが設定されたクラスです)。

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>このクラスはキー値のコードに準拠していません。

NIB ファイルの読み込み中にこのエラーが発生した場合は、値 XXXX がマネージクラスで見つからなかったことを意味します。 これは、次のような宣言がないことを意味します。

```csharp
[Connect]
TypeName XXXX {
   get {
       return (TypeName) GetNativeField ("XXXX");
   }
   set {
       SetNativeField ("XXXX", value);
   }
}
```

上記の定義は、`NAME_OF_YOUR_XIB_FILE.designer.xib.cs` ファイルで Visual Studio for Mac に追加するすべての XIB ファイルについて、Visual Studio for Mac によって自動的に生成されます。

また、上記のコードを含む型は、 [NSObject](xref:Foundation.NSObject)のサブクラスである必要があります。  含んでいる型が名前空間内にある場合は、名前空間を持たない型名を提供する[[Register]](xref:Foundation.RegisterAttribute)属性も必要になります (Interface Builder は型の名前空間をサポートしていません)。

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>Interface Builder ファイルに不明なクラス XXXX があります

このエラーは、インターフェイスビルダーファイルでクラスを定義した場合に生成されますが、実際のC#コードでの実装は提供しません。

次のようなコードを追加する必要があります。

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```

## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>MissingMethodException: Foo. Bar:: .ctor (IntPtr) のコンストラクターが見つかりません

このエラーは、コードが Interface Builder ファイルから参照したクラスのインスタンスをインスタンス化しようとすると、実行時に生成されます。 これは、1つの IntPtr をパラメーターとして受け取るコンストラクターを追加し忘れたことを意味します。

IntPtr ハンドルを持つコンストラクターは、マネージオブジェクトをアンマネージ表現にバインドするために使用されます。

この問題を解決するには、次のコード行を Foo. Bar クラスに追加します。

```csharp
public Bar (IntPtr handle) : base (handle) { }
```

## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>型 {Foo} には `GetNativeField` の定義が含まれていません。型 {Foo} の拡張メソッド `GetNativeField` が見つかりませんでした

デザイナーで生成されたファイル (*. xib.designer.cs) でこのエラーが発生した場合は、次の2つのいずれかを意味します。

 **1) 部分クラスまたは基底クラスがありません**

デザイナーによって生成される部分クラスには、`NSObject`の一部のサブクラスから継承されるユーザーコード (多くの場合 `UIViewController`) の対応する部分クラスが必要です。 エラーが発生している型のクラスがあることを確認します。

 **2) 既定の名前空間が変更されました**

デザイナーファイルは、プロジェクトの既定の名前空間設定を使用して生成されます。 これらの設定を変更した場合、またはプロジェクトの名前を変更した場合、生成された部分クラスは、ユーザーコードに対応する名前空間と同じでなくてもかまいません。

名前空間の設定は、[プロジェクトオプション] ダイアログボックスで確認できます。 既定の名前空間は、 **[General-> のメイン設定**] セクションにあります。 空白の場合は、プロジェクトの名前が既定値として使用されます。 詳細な名前空間の設定については、「**ソースコード-> .net の名前付けポリシー** 」セクションを参照してください。

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>アクションの警告: プライベートメソッド ' Foo ' は使用されません。 CS0169

インターフェイスビルダーファイルのアクションは、実行時にリフレクションによってウィジェットに接続されるため、この警告が発生します。

これらのメソッドに対してのみ警告を非表示にする場合は、「#pragma warning disable 0169」 #pragma 警告を0169有効にします。または、プロジェクト全体で無効にする場合は、コンパイラオプションの [警告を無視する] フィールドに0169を追加します (推奨しません)。

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch は次のメッセージで失敗しました: アセンブリ '/path/to/yourproject.exe ' を開けません

このエラーメッセージが表示される場合は、一般に、プロジェクトへの絶対パスにスペースが含まれています。 これは、今後のバージョンの Xamarin. iOS で修正される予定ですが、この問題を回避するには、空白のないフォルダーにプロジェクトを移動します。

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Sqlite3 のバージョンが古くなっています。少なくとも v 3.5.0 にアップグレードしてください。

これは、次のすべてを実行すると発生します。

1. Mono. Data. Sqlite を使用する
1. Mac OS X Leopard を使用する (10.5)
1. シミュレーター内でアプリを実行します。

問題は、Mono がリストで iphonesimulator の `libsqlite3.dylib` ファイルではなく、OS X `libsqlite3.dylib`を選択していることです。 アプリ*は*デバイスで動作しますが、シミュレーターでは動作しません。

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>デバイスへの配置がシステムで失敗します。例外: AMDeviceInstallApplication から3892346901が返されました

このエラーは、証明書/バンドル id のコード署名構成が、デバイスにインストールされているプロビジョニングプロファイルと一致しないことを意味します。  確認する、適切な証明書があるプロジェクトのオプションで選択されているバンドル署名 、iPhone]-> [し、プロジェクトのオプションで指定された正しいバンドル id が iPhone アプリケーション ->

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>コード補完が Visual Studio for Mac で機能していません

最新バージョンの Visual Studio for Mac と Xamarin. iOS を使用していることを確認します。

問題が解決しない場合は、[バグを提出](https://monodevelop.com/Developers#Reporting_Bugs)し、 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**、 **androidtools-{Timestamp} .log**、および**Components-{timestamp} .log**のログファイルを添付してください。

他のすべての操作が失敗した場合は、コード補完キャッシュを削除して、再生成されるようにすることができます。

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

このコマンドを正しく入力しないと、重要なファイルが誤って削除される可能性があります。

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>テキストをコピーすると Visual Studio for Mac がクラッシュする

一般的な Mac ユーティリティ QuickSilver、Google Toolbar、および起動ツールには、Visual Studio for Mac のメモリが破損しているクリップボード機能があります。 これらのオプションでは、干渉すべきではないプロセスとして Visual Studio for Mac を一覧表示できます。

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Mono 2.4 が必要である Visual Studio for Mac 文句

最新の更新プログラムによって Visual Studio for Mac を更新した場合に、もう一度開始しようとすると、Mono 2.4 が存在しないという問題が発生します。 [mono 2.4 のインストールをアップグレード](http://www.go-mono.com/mono-downloads/download.html)するだけで済みます。  

Mono 2.4.2.3/6 では、Visual Studio for Mac の実行が保証されていない重要な問題を修正し、起動時に Visual Studio for Mac ハングしたり、コード補完データベースが生成されないようにしたりすることができます。

新しい Mono をインストールすると Visual Studio for Mac が正常に開始されます。

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>アサーションを../../../../(& a)/metadata/generic/c:、条件 ' oti ' が満たされていません。

次のスタックトレースを受信している場合:

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

これは、thumb コードを使用してコンパイルされたスタティックライブラリをプロジェクトにリンクすることを意味します。 IPhone SDK リリース 3.1 (またはこのドキュメントの執筆時点以降) では、非 Thumb コード (Xamarin) を Thumb コード (スタティックライブラリ) にリンクするときに、Apple によってリンカーにバグが導入されました。この問題を軽減するには、スタティックライブラリの非 Thumb バージョンとリンクする必要があります。

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1get_count-"></a>System.executionengineexception: JIT コンパイルメソッド (ラッパーマネージ-マネージ) Foo []: system.string ' 1. get_Count () を試行しています

[] サフィックスは、ユーザーまたはクラスライブラリが、IEnumerable < >、ICollection < > や IList < > などのジェネリックコレクションを通じて配列に対してメソッドを呼び出すことを示します。 回避策として、自分でメソッドを呼び出し、例外をトリガーした呼び出しの前にこのコードが実行されるように、AOT コンパイラに明示的に強制することができます。 この場合は、次のように記述できます。

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

これにより、AOT コンパイラに get_Count メソッドが強制的に含められます。

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>ソースエディターの Visual Studio for Mac が非常に遅い

場合によっては、Visual Studio for Mac ソースエディターが非常に遅くなり、文字を入力するまでに数秒間ハングするように見えます。

この問題は非常にまれであり、再現するのは非常に困難です。通常は、Visual Studio for Mac を再起動した後に同じコンピューターで再現することはできません。 このため、Visual Studio for Mac を再起動する前にいくつかのデバッグ手順を実行し、結果を米国に送信できるかどうかを評価します。

1. エディタータブを閉じてから、再度開いてみてください。 遅延が再び発生するまで、カレットを少し編集または移動する必要がありますか。
1. "" (スポットライトを使用して見つけることができます) "開発者ツールを使用して" ビーム同期 "を無効にし、ソースエディターのパフォーマンスが正常に復元されているかどうかを確認します。
1. ビーム同期を無効にしたまま、ステップ (1) を繰り返してみてください。
1. エディターが数秒間ハングした場合は、ハングしているときに、ターミナルで "終了しました。" [Visual Studio for Mac] "を実行してみてください。 エディターがハングしている間に kill コマンドが発生するのは困難な場合がありますが、コマンドによってすべてのスレッドのスタックトレースが MD ログに書き込まれます。これは、XS がハングしている間のスレッドの状態を検出するために使用できます。

XS logs、 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**、 **androidtools-{timestamp} .Log**、および**Components-{timestamp} .log**をアタッチしてください (以前のバージョンの xs/MonoDevelop では、**最大で library/logs/モノ開発-(3.0 | 2.8 | 2.6)/MonoDevelop.log**)。

> [!NOTE]
> 上記の問題は、XS 2.2 Final * * で修正されました。

## <a name="compiled-application-is-very-large"></a>コンパイル済みアプリケーションのサイズが非常に大きい

デバッグをサポートするために、デバッグビルドには追加のコードが含まれています。 リリースモードでビルドされたプロジェクトは、サイズの一部です。

Xamarin. iOS 1.3 の場合、デバッグビルドには、Mono のすべてのコンポーネント (フレームワークのすべてのクラスのすべてのメソッド) に対するデバッグサポートが含まれていました。  

Xamarin の iOS 1.4 では、より詳細なデバッグ方法が導入されます。既定では、コードとライブラリのデバッグインストルメンテーションだけを提供し、すべての[Mono アセンブリ](~/cross-platform/internals/available-assemblies.md)に対してこれを実行しないようにします (これは可能ですが、これらのアセンブリをデバッグするにはオプトインする必要があります)。

## <a name="installation-hangs"></a>インストールがハングする

Mono と Xamarin の両方が動作します。 iPhone シミュレーターを実行している場合は、ハングします。 この問題は Mono または Xamarin に限定されていません。これは、iPhone シミュレーターがインストール時に実行されている場合に、MacOS の雪 Leopard にソフトウェアをインストールしようとするすべてのソフトウェアで一貫した問題になります。

IPhone シミュレーターを終了し、インストールを再試行してください。

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>型0の trampolines が不足しています

デバイスの実行中にこのメッセージが表示された場合は、プロジェクトオプションの "iPhone Build" セクションを変更して、さらに種類 0 trampolines (種類に固有) を作成できます。  デバイスビルドターゲットに追加の引数を追加する必要がある場合は、次のようにします。

 `-aot "ntrampolines=2048"`

既定の trampolines 数は1024です。 アプリケーションに十分な余裕が得られるまで、この数を増やしてみてください。

## <a name="ran-out-of-trampolines-of-type-1"></a>種類1の trampolines が不足しています

再帰ジェネリックを頻繁に使用する場合は、デバイスでこのメッセージが表示されることがあります。  プロジェクトオプションの "iPhone Build" セクションを変更することで、型 1 trampolines (RGCTX) をさらに作成できます。  デバイスビルドターゲットに追加の引数を追加する必要がある場合は、次のようにします。

 `-aot "nrgctx-trampolines=2048"`

既定の trampolines 数は1024です。 ジェネリックの使用に十分な数になるまで、この数を増やしてみてください。

## <a name="ran-out-of-trampolines-of-type-2"></a>型2の trampolines が不足しています

多用インターフェイスを使用すると、デバイスでこのメッセージが表示される場合があります。
プロジェクトオプションの "iPhone Build" セクションを変更して、さらに型 2 trampolines (型 IMT サンク) を作成できます。  デバイスビルドターゲットに追加の引数を追加する必要がある場合は、次のようにします。

 `-aot "nimt-trampolines=512"`

IMT サンク trampolines の既定の数は128です。 インターフェイスの使用に十分な数になるまで、この数を増やしてみてください。

## <a name="debugger-is-unable-to-connect-with-the-device"></a>デバッガーがデバイスと接続できません

デバイス構成のデバッグを開始すると、アプリケーションに接続しようとしていることを示すダイアログがデバッガーに表示されます。 接続に使用しているモード (USB または WiFi) によっては、デバッガーがアプリケーションに接続できない理由がいくつかあります。

 **デバイスとデバッガーホストが別のネットワーク上にある場合**は、ファイアウォールまたはプライベートネットワークによって、アプリケーションが WiFi モードのデバッガーホストに接続できない可能性があります。

 **Visual Studio for Mac は、ホストの正しい IP を照会できない可能性があり**ます。 WiFi モードでは Visual Studio for Mac ホストが検出できるすべての Ip がアプリケーションに与えられ、アプリケーションはそれらのすべてを使用して Visual Studio for Mac に接続できるかどうかを確認します。

 **別のデバイスがホストの USB ポートに接続されています。** 場合によっては、ホスト上の USB ポートに接続されている他のデバイスが、USB モードでのデバッグの妨げとなることがわかっています。

WiFi モードまたは USB モードが動作しない場合は、簡単に試してみてください。 Visual Studio for Mac で、好みを開き、[基本設定]、[デバッガー]、[iPhone デバッガー] の順に進んで、[USB 経由ではなく WiFi で iOS デバイスをデバッグする] チェックボックスをオンにします。   どちらも動作しない場合は、詳細モードでデバイスコンソールのエラーに関する詳細情報を確認できます (プロジェクトのオプションの追加の mtouch 引数に "-v-v-v" を追加することによって有効になります)。

## <a name="error-134-mtouch-failed-with-the-following-message"></a>エラー 134: mtouch が失敗し、次のメッセージが表示されました:

このエラーは、Xamarin. iOS 1.4 スタイルのリリースで-nolink を使用してビルドしようとした場合に発生する可能性があります。 このエラーを回避するには、monodevelop プロジェクト構成で追加の引数を指定します。

引数を追加します。

 `-nosymbolstrip`

問題を解決する必要があります。

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>配布 id が Visual Studio for Mac プロジェクト署名オプションに表示されない

Visual Studio for Mac 2.2 には、コンマを含む配布証明書を検出しない原因となるバグがあります。 Visual Studio for Mac 2.2.1 に更新してください。

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>アップロード中に "AFCFileRefWrite が返されました: 1" というエラーが発生する

デバイスにアプリをアップロードしているときに、"AFCFileRefWrite が返されました: 1" というエラーが表示されることがあります。 これは、長さゼロのファイルがある場合に発生する可能性があります。

## <a name="error-mtouch-failed-with-no-output"></a>エラー "mtouch は出力されませんでした"

Xamarin. iOS と Visual Studio for Mac の現在のリリースは、プロジェクト名またはソリューションまたはプロジェクトに格納されているディレクトリにスペースが含まれていると失敗します。
解決するには、次の操作を行います。

- プロジェクトまたは格納されているディレクトリにスペースが含まれていないことを確認してください。
- プロジェクトの [メイン設定] で、プロジェクト名にスペースが含まれていないことを確認します。

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>エラー "アップロードしたバイナリは無効です。 SDK のプレリリース版ベータ版を使用して、アプリケーションをビルドしました。 "

このエラーは通常、Xamarin の前に iPad 開発で開始されたプロジェクトで発生します。 iOS 2.0.0 がリリースされたため、情報にいくつかのキーがある可能性があります。次に例を示します。

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

このペアは、自動的に処理 Visual Studio for Mac ため、削除する必要があります。

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>"SDK のプレリリースベータ版がアプリのビルドに使用されました" というエラーが発生する

(Ed Anuff が提供)

次の手順に従います。

- IPhone ビルドの SDK バージョンを3.2 に変更します。または、iTunes connect は、3.2 よりも前のバージョンの SDK を使用してビルドされた iPad 互換アプリを表示しているため、アップロード時にそのバージョンを拒否します。
- プロジェクトに対してカスタムの MinimumOSVersion を作成し、その中で明示的に3.0 に設定します。   これにより、MinimumOSVersion によって設定された3.2 の値が上書きされます。   この操作を行わないと、アプリは iPhone で実行できなくなります。
- 再構築し、zip 接続して、iTunes connect にアップロードします。

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>ハンドルされない例外: System.exception: セレクターのセレクターが見つかりませんでした: {type}

この例外は、次の3つのうちのいずれかによって発生します。

1. 対応する [Export] 属性をメソッドに適用せずに、目的の C ランタイムのセレクターを指定しました
1. 完全リンクを有効にし、[Export] ed メソッドに [Preserve] 属性を適用していません。
1. 継承された型のプライベートメソッドに [Export] 属性を適用しました。

## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs ファイルが更新されていません

Xamarin Studio 2.4 にバグが発生しました。これにより、新しいプロジェクトで Mainwindow.xaml ファイルを使用して xib ファイルをグループ化することができなくなりました。 これは、その特定のファイルのデザイナーコードを更新しないことを意味します。

この問題は、組み込みのアップデーターで利用可能な Visual Studio for Mac のバージョンで修正されているので、新しいバージョンを使用するようにしてください。

既存のプロジェクトを修正するには、xib とそのデザイナーファイルを削除してから再度追加します。 これにより、ファイルを適切にグループ化し直す必要があります。

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>UIAlertView または Uialertview が作成された後に消える

次のようなコードがあるとします。

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

"actionSheet" オブジェクトは関数内の一時変数として存在し、関数が終了するとすぐに、オブジェクトはガベージコレクションの対象になるため、ガベージコレクションが行われます。

この問題を解決するには、メソッドの外部で "actionSheet" への参照を保持する必要があります。

## <a name="project-always-runs-in-the-ipad-simulator"></a>プロジェクトは常に iPad シミュレーターで実行されます

IPhone SDK 4.0 インストーラーでは、2つの Sdk (iPad 専用アプリを構築するための 3.2 SDK) と、iPhone とユニバーサルアプリの構築に使用される 4.0 SDK がインストールされます。 また、iPad だけをシミュレートする3.2 シミュレーターと、iPhone または iPhone 4 をシミュレートする4.0 シミュレーターもインストールされます。 以前の Sdk とシミュレーターはすべて削除されます。

Visual Studio for Mac iPhone プロジェクトのビルドオプションには、アプリのビルドに使用される SDK バージョンの設定が含まれています。 この設定は **、[プロジェクトオプション-> ビルド-> IPhone ビルド]** にあります。

Visual Studio for Mac の新しいプロジェクトは、既定の SDK 設定として最も古いインストールされた SDK を使用します。指定された SDK が存在しない場合、Visual Studio for Mac はアプリをビルドするのに最も近い場所を使用します。 これは、プロジェクトが常に最新の SDK を必要としないようにするためです。 ただし、現在は 3.2 SDK が使用されているため、iPad シミュレーターが使用されています。

4\.0 SDK を使用してこれを修正するには、ドロップダウンボックスを使用して、[**プロジェクトオプション-> ビルド-> IPhone ビルド**>] に移動し、SDK 値を "4.0" に変更します。 この操作は、構成とプラットフォームの組み合わせごとに実行する必要があります。これは、パネルの上部にあるドロップダウンを使用してアクセスします。

SDK のバージョンは、[OS の最小バージョン] 設定と混同しないようにしてください。
この値は、SDK バージョンの値と一致する必要はありません。以前の OS に存在する Api のみを使用している限り、またはランタイム OS バージョンチェックを使用して新しい機能の使用をガードしている限り、アプリがインストールされる OS の最小バージョンに影響します。ks. アプリをテストする最も古い OS バージョンに設定する必要があります。

また、プロジェクト **> IPhone シミュレーターターゲット**> メニューを使用して、プロジェクトの実行時またはデバッグ時に既定で使用されるシミュレーターを選択できます。 また、[>**で実行 >** 実行] メニューを使用して、実行する特定のシミュレーターを選択できます。

## <a name="ibtool-returns-error-133"></a>ibtool によってエラー133が返される

これは、XCode 4 がインストールされていることを意味します。   XCode 4 では、ツール ibtool が削除されているため、スタンドアロンツールを使用して XIB ファイルを編集することはできなくなりました。

Interface Builder を使用する場合は、Apple の web サイトから入手できる XCode series 3 をインストールします。

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-interface-builder"></a>"Mime の種類: application/vnd. apple-interface-builder" の表示バインドを作成できません。

このエラーは、iPhone 以外のプロジェクトから iPhone UI を作成しようとした場合に発生します。 Iphone/iPad ソリューションから始めることを確認してください。 iphone の UI 要素を iPhone 以外のプロジェクトに追加することはできません。

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>IOS シミュレーター内で実行するときのスタートアップクラッシュ

次のようなスタックトレースと共に、シミュレーター内で実行時クラッシュ (SIGSEGV) が発生した場合は、次のようになります。

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```

...その場合、シミュレーターアプリケーションディレクトリに古いアセンブリが1つ (またはそれ以上) 存在する可能性があります。 このようなアセンブリは、Apple iOS シミュレーターによってファイルが追加および更新されるが、削除されないために存在する可能性があります。 この問題が発生した場合、最も簡単な解決策は、"リセットとコンテンツと設定..." を選択することです。シミュレーターメニューから。   

> [!WARNING]
> これにより、シミュレーターからすべてのファイル、アプリケーション、およびデータが削除されます。   次にアプリケーションを実行したときに、Visual Studio for Mac によってアプリケーションがシミュレーターに配置され、クラッシュを引き起こす古い古いアセンブリは存在しません。

## <a name="simulator-hangs-during-application-installation"></a>アプリケーションのインストール中にシミュレーターがハングする

これは、アプリケーション名に '. ' が含まれている場合に発生する可能性があります。(ドット) を名前で指定します。
これは、他の多くのケース (デバイスなど) で動作する場合でも、CFBundleExecutable の実行可能ファイル名としては禁止されています。

"値には名前の拡張子を含めないでください。"

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>エラー: xib ファイルをダブルクリックすると、"カスタム属性の種類0x43 はサポートされていません" になります。

これは、環境変数が正しく設定されていない場合に xib ファイルを開こうとすると発生します。 これは、Visual Studio for Mac/Xamarin. iOS の通常の使用時には発生しません。また、アプリケーションから Visual Studio for Mac を再起動すると、問題を解決する必要があります。

ソフトウェアを更新しようとして、このエラーメッセージが表示された場合は、電子メール *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>アプリケーションはシミュレーターで実行されますが、デバイスでは失敗します

この問題はいくつかの形式でマニフェストを作成できますが、常に一貫性のあるエラーが生成されるとは限りません。 アプリケーションに. xib が含まれている場合は、xib の**ビルドアクション**が**interfacedefinition**に設定されていることを確認します。 これは、. xib の既定のビルドアクションです。

ビルドアクションを確認するには、xib ファイルを右クリックし、 **[ビルドアクション]** を選択します。

## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>NotSupportedException: エンコード437で使用できるデータがありません

Xamarin. iOS アプリにサードパーティ製のライブラリを含めると、アプリをコンパイルして実行しようとすると、"NotSupportedException: encoding 437 のデータは使用できません" という形式のエラーが表示されることがあります。 たとえば、`Ionic.Zip.ZipFile`などのライブラリでは、操作中にこの例外がスローされる場合があります。

これを解決するには、Xamarin. iOS プロジェクトのオプションを開き、[ **Ios ビルド** > **国際化**] に移動して、[**西**国際化] をオンにします。
