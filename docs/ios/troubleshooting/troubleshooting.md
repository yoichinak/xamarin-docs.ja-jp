---
title: Xamarin.iOS に対するトラブルシューティングのヒント
description: このドキュメントでは、Xamarin.iOS アプリケーションの開発時のトラブルシューティングに役立つさまざまなヒントを提供します。 これは、特定のエラー メッセージとその他の潜在的な問題について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/0201
ms.openlocfilehash: 26fe2fb848fb81940bc01a34c69b1b28897005dc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789354"
---
# <a name="troubleshooting-tips-for-xamarinios"></a>Xamarin.iOS に対するトラブルシューティングのヒント 

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS System.ValueTuple を解決できません。

このエラーは、Visual Studio と互換性がないのために発生します。

- **Visual Studio 2017 Update 1** (15.1 またはそれ以前のバージョン) とのみ互換性が**System.ValueTuple NuGet 4.3.0** (またはそれ以前)。

- **Visual Studio 2017 Update 2** (15.2 またはそれ以降のバージョン) とのみ互換性が、 **System.ValueTuple NuGet 4.3.1**またはそれ以降。

Visual Studio 2017 のインストールに対応する正しい System.ValueTuple NuGet を選択してください。


## <a name="receiving-error-retrieving-update-information-error-message"></a>'の更新情報の取得エラー' のエラー メッセージ

表示されたら、ソフトウェアとこのエラー メッセージを更新しようとすると、IDE で、IDE とログアウトして、アカウントに戻りますを再起動してください。

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>インターフェイスのビルダーをコンセントまたはアクションを作成する方法

Mac と Visual studio の Visual Studio での iOS 用の Xamarin デザイナーの導入に伴い、Xamarin.iOS 開発者今すぐを利用のストーリー ボードと .xibs を通じて UI を作成できます。 参照してください、[こんにちは, iOS](~/ios/get-started/hello-ios/index.md)デザイナーの使用方法についてガイドします。

Apple を参照することもできます。[コンセント](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html)と[アクション](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html)IB でコンセントとアクションの使用方法についてガイドします。

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>System.Text.Encoding.GetEncoding スロー NotSupportedException

既定では追加されていないエンコーディングを使用することがあります。 チェック、[国際化](~/ios/app-fundamentals/localization/index.md)詳細エンコードのサポートを追加する方法を説明するページ。

## <a name="systemmissingmethodexception-anything-else"></a>(その他の要素) System.MissingMethodException

メンバーは、リンカーによって削除された可能性があり、実行時にアセンブリに存在しないためです。  これをいくつかの解決があります。

-  追加、 [[保持]](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute)属性のメンバーにします。  これにより、削除することから、リンカーができなくなります。
-  呼び出すときに[mtouch](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29)を使用して、 **- nolink**または **- linksdkonly**オプション。 -    **-nolink**すべてのリンクを無効にします。
-    **-linksdkonly** Xamarin.iOS 標準のアセンブリをなどリンクのみ*monotouch.dll*または xamarin.ios.dll です。

アセンブリは、結果の実行可能ファイルが小さく; ようにリンクに注意してください。したがって、リンクを無効にすることがありますが望ましいよりも大きな実行可能。

## <a name="you-are-getting-a-modelnotimplementedexception"></a>ModelNotImplementedException を取得します。

この例外を取得する場合は、基本呼び出していることを意味します。モデルをオーバーライドするクラスのメソッド () です。 (これらは、[モデル] 属性を持つフラグが設定されたクラス) モデルのクラスで基本メソッドを呼び出す必要はありません。

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>このクラスは、コーディング準拠 XXXX キーのキーの値ではありません。

NIB ファイルの読み込み時にこのエラーが発生した場合は、ことを意味して値 XXXX は、マネージ クラスには見つかりませんでした。 これは、次のように宣言がないことを意味します。

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

上記の定義が自動的に生成 Visual Studio によって Mac 用に Mac を Visual Studio に追加するすべての XIB ファイルについて、`NAME_OF_YOUR_XIB_FILE.designer.xib.cs`ファイル。

さらに、上記のコードを含む型がのサブクラスをする必要があります[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)です。  包含する型が名前空間内にある場合は、それも必要、 [[登録]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/)属性は、名前空間のない型名 (型のインターフェイスのビルダーに名前空間をサポートしていません)。

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>不明なクラス インターフェイスのビルダー ファイルで XXXX

このエラーは、インターフェイス ビルダー ファイルにクラスを定義する c# コードでそのため、実際の実装を指定しない場合に生成されます。

次のようにコードを追加する必要があります。

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>System.MissingMethodException: コンス トラクターがない Foo.Bar::ctor(System.IntPtr) が見つかりました

コードが、インターフェイスのビルダー ファイルから参照されているクラスのインスタンスをインスタンス化しようとするとき、実行時にこのエラーが生成されます。 これは、パラメーターとして 1 つの IntPtr を受け取るコンス トラクターを追加するを忘れてことを意味します。

IntPtr ハンドルを持つコンス トラクターを使用すると、マネージ オブジェクトのアンマネージ表現にバインドされます。

この問題を解決するには、共通のクラスに次のコード行を追加します。

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>定義を含まない型 {Foo} `GetNativeField' and no extension method `GetNativeField' 型の {Foo} が見つかりませんでした

デザイナー生成されたファイルでこのエラーが発生したかどうか (*. xib.designer.cs)、次の 2 つのいずれかのことを意味します。

 **1) 部分クラスまたは基底クラスがありません。**

デザイナーで生成される部分クラスは、対応する部分でユーザー コードから継承したクラスのいずれかのサブクラスをいる必要があります`NSObject`、多くの場合、`UIViewController`です。 このようなエラーを提供する型のクラスがあることを確認します。

 **2) 既定の名前空間の変更**

デザイナーのファイルは、プロジェクトの既定の名前空間の設定を使用して生成されます。 これらの設定を変更または、プロジェクトの名前を変更した場合、生成された部分クラスで可能性があります不要になった対応するユーザー コードと同じ名前空間。

Namespace 設定は、プロジェクトのオプション ダイアログに含まれています。 既定の名前空間に存在、**全般にメインの設定]-> [** セクションです。 空白の場合は、プロジェクトの名前が既定値として使用されます。 名前空間のより高度な設定は含まれて、**ソース コードには .NET の名前付けポリシー]-> [** セクションです。

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>アクションの警告:"Foo"が使用されていません。 プライベート メソッド。 (CS0169)

インターフェイスのビルダー ファイルに対する操作は、この警告が予想されるように、実行時にリフレクションによって、ウィジェットに接続されます。

#Pragma 警告 [無効] に 0169 を使用することができます"#pragma 警告の有効化 0169"、アクションの周囲だけこれらのメソッドには、この警告を抑制するか (not プロジェクト全体を無効にする場合、コンパイラ オプションの「警告を無視する」フィールドに 0169 を追加する場合推奨)。

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>次のメッセージで失敗しました mtouch: アセンブリを開くことができません '/path/to/yourproject.exe'。

このエラー メッセージを表示する場合は通常は問題が、プロジェクトへの絶対パスにスペースが含まれています。 これは、Xamarin.iOS の将来のバージョンで修正しますが、スペースを含まないフォルダーをプロジェクトに移動して問題を回避することができます。

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Sqlite3 バージョンが古い場合は、以上にアップグレードしてください - v3.5.0!

これは、次のすべての操作を行う場合に発生します。

1.  Mono.Data.Sqlite を使用します。
1.  Mac OS X Leopard (10.5) を使用します。
1.  シミュレーターでアプリを実行します。


問題は、Mono は OS X を拾いこと`libsqlite3.dylib`、されません、iPhoneSimulator の`libsqlite3.dylib`ファイル。 アプリ*は*作業して、デバイスが、シミュレーターではないだけです。

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>System.Exception で失敗するデバイスに展開します AMDeviceInstallApplication 3892346901 が返されます。

このエラーは、コード署名証明書/バンドル id の構成では、デバイスにインストールされているプロビジョニング プロファイルが一致しないことを意味します。  確認する、適切な証明書があるプロジェクトのオプションで選択されているバンドル署名 、iPhone]-> [し、プロジェクトのオプションで指定された正しいバンドル id が iPhone アプリケーション ->

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>コード補完が機能していない Visual Studio で Mac

Mac と Xamarin.iOS の最新バージョンの Visual Studio を使用していることを確認してください。

問題がまだ存在している場合は、次のようにしてください[バグ](http://monodevelop.com/Developers#Reporting_Bugs)、アタッチ、 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**、 **AndroidTools-{のタイムスタンプ} .log**。、および**コンポーネント-{のタイムスタンプ} .log**ログ ファイルです。

増やせる場合は、他のすべてが失敗するが再生成されるコード補完キャッシュを削除します。

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

このコマンドを正しく入力したこと、または重要なファイルを誤って削除した可能性がありますに注意します。

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>テキストをコピーするときに、Mac 用の visual Studio がクラッシュします。

形式、Google ツールバーおよび起動バーは、一般的な Mac ユーティリティでは、Visual Studio を Mac のメモリが破損するクリップボード機能があります。 それぞれのオプションでを妨害しないように、プロセスとして Visual Studio for Mac に一覧表示できます。

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Visual Studio for Mac が必要なモノラル 2.4 は苦情します。

最近の更新により Mac 用 Visual Studio を更新すると、開始しようとしたときに再度エラーが発生存在できないモノラル 2.4 は、行う必要があるは[モノラル 2.4 インストールのアップグレード](http://www.go-mono.com/mono-downloads/download.html)です。  

モノラル 2.4.2.3_6 は、Visual Studio for Mac を実行できない、確実に停止したことがありますは Visual Studio の起動時に Mac をまたはコード補完データベースが生成されていることを防止するいくつかの重要な問題を修正します。

新しいモノラルをインストールすると、Visual Studio for Mac は期待どおりに起動します。

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>アサーション./../../../mono/metadata/generic-sharing.c:704、条件 'oti' が満たされていません

次のスタック トレース: 受信した場合

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

これは、プロジェクトに thumb コードでコンパイルされた静的ライブラリをリンクしていることを意味します。 3.1 (またはこの記事の執筆時に高い) に iPhone SDK のリリースの時点で非 Thumb コード (Xamarin.iOS) と Thumb コード (スタティック ライブラリ) をリンクするときに、Apple が、リンカーのバグを導入しました。この問題を軽減するために、スタティック ライブラリの非 Thumb バージョンとリンクする必要があります。

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>JIT コンパイル Foo[]:System.Collections.Generic.ICollection'1.get_Count () メソッド (管理対象からマネージ ラッパー) しようとして System.ExecutionEngineException:

のサフィックスは、こと、またはクラス ライブラリは、メソッドを呼び出す IEnumerable <>、ICollection <> IList <> などのジェネリック コレクションを配列を示します。 この問題を回避するには、する明示的 force を自分でメソッドを呼び出すことによってこのようなメソッドを含める AOT コンパイラできことを確認することによって、例外をトリガーした呼び出しの前にこのコードを実行できます。 この例では、次のように記述する可能性があります。

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Get_Count メソッドを含める AOT コンパイラを強制します。

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio for Mac ソース エディターは非常に遅い

Visual Studio for Mac ソース エディターなる非常に遅く、数秒間の文字の入力のハングに表示されます。

この問題が非常にまれなとを再現する非常に困難 - その通常再現できない、同じコンピューター上 for mac Visual Studio を再起動した後 このため、いただければ for Mac を Visual Studio を再起動する前にいくつかのデバッグ手順を実行し、弊社にとって、結果を送信する可能性があります。

1.  エディターのタブを閉じてから再度開くことです。 少しの編集や遅延が再び発生するまで、カレットを移動がかかりますか。
1.  "ビーム Sync""Quartz Debug"開発者ツールを使用して無効にする (見つけることができますメディアを使用して)、ソース エディターのパフォーマンスが正常に復元するかどうかを確認します。
1.  ビーム同期が無効のままで手順 (1) を試してください。
1.  数秒以上のエディターがハングした場合を実行する再試行してください"killall-終了 [Visual Studio for Mac]"がハングしているときに、ターミナル。 エディターがハング状態にがためには、重要なため、コマンドは、強制的にすべてのスレッドのスタック トレースをログに書き込む、MD、X がハングしているときに、スレッドは、どのような状態を検出する際に使用できるモノラル中に起こるを kill コマンドの時間にくい場合があります。



XS ログをアタッチしてください **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**、 **AndroidTools-{のタイムスタンプ} .log**、および**コンポーネント-{のタイムスタンプ} .log**。(XS/MonoDevelop の旧バージョンでは送信 **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**)。

 **メモ: 上の問題は、XS 2.2 最終で修正されました**

## <a name="compiled-application-is-very-large"></a>コンパイル済みのアプリケーションが非常に大きい

デバッグをサポートするには、デバッグ ビルドにはでは追加のコードが含まれています。 リリース モードでビルドされたプロジェクトとは、サイズの割合です。

Xamarin.iOS 1.3、デバッグの時点では、ビルドは、Mono (フレームワークのすべてのクラスですべてのメソッド) のすべての 1 つのコンポーネントのデバッグをサポート含まれています。  

デバッグの詳細なメソッドを導入した Xamarin.iOS 1.4 で、既定値は、のみ、コードと、ライブラリのデバッグのインストルメンテーションを提供し、このしないですべての[Mono アセンブリ](~/cross-platform/internals/available-assemblies.md)(これが利用可能可能であれば、するが、それらのアセンブリのデバッグにオプトインする必要が)。

## <a name="installation-hangs"></a>インストールがハング

モノラルと Xamarin.iOS の両方のインストーラーは、iPhone シミュレーターの実行がある場合にハングします。 この問題はモノラルまたは Xamarin.iOS に限定されません、iPhone シミュレーターがインストール時に実行されている場合は、MacOS ユキヒョウでソフトウェアのインストールしようとするすべてのソフトウェアの間で一貫性のある問題です。

IPhone シミュレーターを終了して、インストールを再試行することを確認してください。

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>0 の種類の領域不足になりました

デバイスを実行中にこのメッセージを取得する場合は、プロジェクトのオプションの「iPhone ビルド」セクションを変更することでより多くの種類 (型固有の仕様) 0 の領域を作成できます。  デバイスの余分な引数のビルド ターゲットを追加するには。

 `-aot "ntrampolines=2048"`

領域の既定の数は 1024 です。  なるまで、アプリケーションに対して十分では、この数を増やしてください。

## <a name="ran-out-of-trampolines-of-type-1"></a>種類 1 の領域不足になりました

再帰的なジェネリック頻繁に使用する場合は、デバイスでこのメッセージが表示されます。  プロジェクトのオプションの「iPhone ビルド」セクションを変更することでより多くの種類 1 の領域 (型 RGCTX) を作成できます。  デバイスの余分な引数のビルド ターゲットを追加するには。

 `-aot "nrgctx-trampolines=2048"`

領域の既定の数は 1024 です。  ジェネリックの使用状況に十分なが得られるまで、この数を増やしてください。

## <a name="ran-out-of-trampolines-of-type-2"></a>タイプ 2 の領域不足になりました

インターフェイスを頻繁に使用している場合、デバイスにこのメッセージが表示されます。
プロジェクト オプションの「iPhone ビルド」セクションを変更することでは、2 つの領域 (型 IMT サンク) でより多くの種類を作成できます。  デバイスの余分な引数のビルド ターゲットを追加するには。

 `-aot "nimt-trampolines=512"`

IMT サンク領域の既定の数は 128 です。  インターフェイスの使用状況に十分なが得られるまで、この数を増やしてください。

## <a name="debugger-is-unable-to-connect-with-the-device"></a>デバッガーは、デバイスに接続できません。

デバイスの構成のデバッグを開始するときに、アプリケーションへの接続を試行されていることを示すダイアログ ボックスを表示する、デバッガーが表示されます。 デバッガーでアプリケーションに接続できないいくつかの理由がある、(USB または WiFi) 接続を使用するモードによって異なります。

 **デバイスとデバッガー ホストが別のネットワーク上にある場合**、ファイアウォールまたはプライベート ネットワークが WiFi モード デバッガー ホストへの接続からのアプリケーションを妨げている可能性があります。

 **Mac 用の visual Studio で、正しい ip アドレス、ホストのクエリを実行できない**です。 WiFi のモード Mac 用の Visual Studio では、アプリケーション ホストのすべての Ip を検索できるとアプリケーションすべてを参照するようかどうかも使用できますうち for mac を Visual Studio に接続するには

 **別のデバイスは、ホスト上の USB ポートに接続します。** その他のデバイスが USB に接続されているいくつかのケースで、ホスト上のポートがわかっている USB モードでのデバッグに干渉する何らかの理由でします。

WiFi または USB のいずれかのモードが機能しない場合はことが簡単にしようとする他の: Mac を Visual Studio で、設定を開きの設定/デバッガー/iPhone デバッガーのページに移動し、"の代わりに WiFi 経由での iOS デバイスを USB 上に Debug"のチェック ボックスを切り替えます。   どちらで動作する場合は、詳細モードでデバイスのコンソール内の失敗に関する詳細についてを確認できます (追加することで有効になっている"-v-v-v"プロジェクトのオプションで追加 mtouch 引数に)。

## <a name="error-134-mtouch-failed-with-the-following-message"></a>次のメッセージと共にエラー 134: mtouch が失敗しました。

リリースの Xamarin.iOS 1.4 スタイル - nolink でビルドする場合は、このエラーを発生でした。 このエラーは、monodevelop プロジェクト構成に余分な引数を指定することによって回避できます。

引数を追加します。

 `-nosymbolstrip`

問題を解決する必要があります。

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Distribution id が署名オプション Mac プロジェクトの Visual Studio に表示されていません

Mac 2.2 用の visual Studio が、問題があるため、コンマを含める配布証明書が検出されないようにします。 Mac 2.2.1 用 Visual Studio には、更新してください。

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>エラー"が返されます AFCFileRefWrite: 1"のアップロード中に

デバイスにアプリのアップロード中にエラーが発生する可能性があります"返される AFCFileRefWrite: 1"です。 これは、長さゼロのファイルがある場合に発生することができます。

## <a name="error-mtouch-failed-with-no-output"></a>エラー"mtouch なしの出力で失敗しました。"

Mac 用、Xamarin.iOS および Visual Studio の現在のリリースが失敗する場合に、プロジェクト名またはスペースを含むソリューションまたはプロジェクトが格納されているディレクトリ。
これを解決するには。


-  プロジェクトでも、ディレクトリの場所に格納されますが、空白文字を含めることを確認します。
-  プロジェクト「Main の設定」では、プロジェクト名にスペースが含まれていないことを確認します。

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>エラー"アップロードしたバイナリが無効でした。 SDK のプレリリース版ベータ版は、アプリケーションのビルドに使用された"

このエラーは通常発生 Xamarin.iOS 2.0.0 がリリースされる前に、iPad 開発で開始されたプロジェクトと同様に、Info.plist にいくつかのキーがある可能性があります。

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Visual Studio for Mac 処理を自動的にために、このキー ペアを削除する必要があります。

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>エラー「アプリをビルドする SDK のプレリリース版ベータ版が使用されました」

(Ed Anuff から提供された)

この場合は、以下の手順に従ってください。

-  SDK のバージョン 3.2 または iTunes に iPhone ビルドでの接続の変更によって拒否されますアップロード時に 3.2 未満の SDK バージョンを使用して構築された iPad 互換性のあるアプリ見たため
-  プロジェクトのカスタム Info.plist を作成しが 3.0 に MinimumOSVersion を明示的に設定します。   これにより、Xamarin.iOS によって設定された MinimumOSVersion 3.2 値が上書きされます。   これを行わない場合、アプリは iPhone 上で実行することはできません。
-  再構築、ジップと iTunes へのアップロードを次のように接続します。

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>未処理の例外: System.Exception: セレクター someSelector が見つかりませんでした {} 型で。

この例外は、次の 3 つのいずれかによって発生は。


1.  メソッドに対応する [エクスポート] 属性を適用せず、Objective C ランタイムのセレクターを指定しました。
1.  フル リンクを有効にして、[エクスポート] ed メソッドに、[保存] 属性は適用されません。
1.  [エクスポート] 属性が継承型のプライベート メソッドに適用します。


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs ファイルが更新されません。

Xamarin Studio 2.4 MainWindow.xib ファイルを新しいプロジェクトで MainWindow.xib.designer ファイル グループがその原因となったでバグが発生しました。 これは、ため、そのファイルにデザイナーのコードは更新されません。

組み込みの更新処理で使用可能な Mac 用の Visual Studio のバージョンでこの問題が解決ようにしてください。 新しいバージョンを使用することを確認します。

既存のプロジェクトを修正するには、(削除しない) を削除することによって、xib とそのデザイナー ファイルでは、追加したうえ、バックアップを作成します。 正しくファイル再グループ必要があります。

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>作成後に表示されなく UIAlertView または UIActionSheet

このようなコードを場合。

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

"actionSheet"オブジェクトが、関数内の一時変数として存在、関数が終了すると、すぐオブジェクト ガベージ コレクションのガベージ コレクションの対象が生じます。

この問題を解決するには、"actionSheet"どこかに、メソッドの外側が過ぎると、メソッドへの参照を保持する必要があります。

## <a name="project-always-runs-in-the-ipad-simulator"></a>プロジェクト常に実行する iPad シミュレーターで

IPhone SDK 4.0 のインストールには、2 つの Sdk - iPad 専用のアプリを構築するための 3.2、SDK、および 4.0 の SDK では、文書の iPhone アプリとユニバーサル アプリの使用が表示されます。 3.2 シミュレーターは、iPad のみをシミュレートすると iPhone または iPhone 4 をシミュレートする 4.0 シミュレーターにもインストールされます。 以前のすべての Sdk およびシミュレーターが削除されます。

Mac iPhone プロジェクトのビルド オプション用の visual Studio では、アプリの構築に使用される SDK バージョンの設定を含めます。 この設定は含まれて**プロジェクトのオプションには、ビルドが]-> [iPhone ビルド]-> [** です。

Mac 用の Visual Studio で新しいプロジェクトでは、最も古いインストールされている SDK を使用して、その既定の SDK 設定としてと指定されている SDK が存在しない場合は、Visual Studio for Mac が最も近いアプリのビルドを発見して、使用します。 これは、プロジェクトが requre 最新の SDK では常にではありませんできるようにします。 ただし、この現在結果 3.2 SDK の中で使用されている iPad シミュレーターで結果を使用します。

4.0 SDK を使用してこれを解決するには**プロジェクトのオプションには、ビルドが]-> [iPhone ビルド]-> [**> SDK 値ボックスの一覧を使用して「4.0」に変更します。 構成およびアクセス パネルの上部にあるドロップダウンを使用して、プラットフォームの組み合わせごとに、これを行う必要があります。

SDK のバージョンは、「最小 OS バージョン」設定と混同しないでください。
この値は、SDK のバージョンの値に一致する必要はありません - 以前の OS に存在するか、ランタイムの OS バージョンのチェックを使用して新しい機能の使用を保護する Api のみを使用する限り、SDK よりも古くなる可能性が、アプリのインストール先、OS の最小バージョンに影響を与えるks です。 アプリをテストする最も古い OS バージョンに設定する必要があります。

なお、**プロジェクト iPhone シミュレーターのターゲット]-> [**> プロジェクトを実行/デバッグするときに、既定で使用するシミュレーターを選択するメニューを使用できます。 さらに、**で実行]-> [実行**> 実行に使用される特定のシミュレーターを選択するメニューを使用できます

## <a name="ibtool-returns-error-133"></a>ibtool には、エラー 133 が返されます。

これは XCode 4 がインストールされてがあることを意味します。   XCode 4 ツール ibtool が削除された、スタンドアロン ツールで XIB ファイルを編集することができなくなります。

インターフェイスのビルダーを使用する場合は、インストール[XCode シリーズ 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792)は、Apple の web サイトから利用できます。

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Mime の種類の表示のバインドを作成できません: application/vnd.apple-<wbr/>インターフェイス ビルダー"

IPhone ではないプロジェクトから iPhone UI を作成しようとすると、このエラーが発生します。 IPhone または iPad ソリューションから始めることは、iPhone または iPad プロジェクトに iPhone UI 要素を追加することはできません確認します。

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>IOS シミュレーター内で実行中のスタートアップ クラッシュ

場合は、実行時のクラッシュ (SIGSEGV) を取得するには次のようなスタック トレースと、シミュレーター内。

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
.. うちシミュレーター アプリケーション ディレクトリに 1 つ (以上) の古いアセンブリがある可能性があります。 このようなアセンブリは、Apple iOS シミュレーターが追加され、更新は、ファイルが、それらを削除するために存在する可能性があります。 この場合、最も簡単なソリューションは、「をリセットし、コンテンツと設定...」メニューからクリックして、シミュレーターです。   

**警告:** これシミュレーターからすべてのファイル、アプリケーションおよびデータが削除されます。   アプリケーションを実行する際に [次へ] は、Visual Studio for Mac を使用すると、シミュレーターに展開されますおよびは行われません、クラッシュが発生する古い、古いアセンブリ。

## <a name="simulator-hangs-during-application-installation"></a>アプリケーションのインストール中にシミュレーターのハングします。

これは、アプリケーション名が含まれる場合に発生することができます、'.'(ドット) 名前にします。
これは、(デバイス) のようなその他の多くの場合の動作を可能な場合でも、CFBundleExecutable - で実行可能ファイル名として禁止されています。

 *「値は、名の任意の拡張子を含める必要がありますいない」です。- [http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](http://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>エラー:「カスタム属性の型示しますがサポートされていません」とダブル .xib ファイルをクリックして

これについては、環境変数が正しく設定されていないときに .xib ファイルを開こうとしたにより発生します。 これは発生しません Visual Studio の使用率が通常の Mac/Xamarin.iOS と/Applications から Mac を Visual Studio を開き直すと、問題が解決する必要があります。

更新を試みているときに、ソフトウェアと、このエラー メッセージが表示されたら、ください電子メール *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>アプリケーションは、シミュレーターで実行されますが、デバイスで失敗します。

この問題は、複数の形式でのマニフェストでき、常に一貫性のあるエラーを生成しません。 場合は、アプリケーション、.xib が含まれていることを確認して、**ビルド アクション**、.xib 上に設定されている**インタ フェース定義**です。 これは、.xibs の既定のビルド アクションです。

確認するには、ビルド アクション .xib ファイルを右クリックし、選択**ビルド アクション**です。


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: データがない 437 をエンコードするため

Xamarin.iOS アプリでは、サード パーティ製ライブラリを含む、ときに、フォームでエラーが発生した可能性があります"System.NotSupportedException: 437 をエンコードするために使用可能なデータがない"、アプリをコンパイルして実行しようとしているときにします。 たとえば、ライブラリなど`Ionic.Zip.ZipFile`操作中にこの例外をスローする可能性があります。

これは、問題を Xamarin.iOS プロジェクトのオプションを開くことによって解決できます**iOS ビルド** > **国際化**をチェックし、**西部**国際化します。



<a name="Can't_upgrade_to_Indie/Business_from_Trial_Account" />


## <a name="cant-upgrade-to-indiebusiness-from-trial-account"></a>試用版アカウントから Indie/ビジネスへのアップグレードことはできません。

最近 Xamarin.iOS を購入し、以前、Xamarin.iOS 試用版を開始すると場合、は、Mac または Visual Studio の Visual Studio によってピックアップこのライセンスの変更を取得する次の手順を完了する必要があります。

-  Mac または Visual Studio の Visual Studio を閉じます
-  Windows ~/Library/MonoTouch Mac 上または %PROGRAMDATA%\MonoTouch\License\ からすべてのファイルを削除します。
-  再 Mac または Visual Studio の Visual Studio を開き、Xamarin.iOS プロジェクトをビルド


## <a name="receiving-activation-incomplete-error-message"></a>'不完全なアクティブ化' エラー メッセージを受信

この問題は、Xamarin.iOS を Visual Studio を使用する場合に発生する可能性があります。 この問題を解決するには、次の場所からしてください。 ログを送信[ contact@xamarin.com](mailto:contact@xamarin.com)です。

-  ログの場所: %LocalAppData%/Xamarin/Logs
