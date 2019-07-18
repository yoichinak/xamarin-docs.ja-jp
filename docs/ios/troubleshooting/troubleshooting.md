---
title: Xamarin.iOS のトラブルシューティングのヒント
description: このドキュメントでは、Xamarin.iOS アプリケーションの開発中のトラブルシューティングに役立つさまざまなヒントを提供します。 これは、特定のエラー メッセージとその他の潜在的な問題について説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B50FE9BD-9E01-AE88-B178-10061E3986DA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/22/2018
ms.openlocfilehash: 38c0ece3e8f0361f3c891713e53b033351512f94
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829921"
---
# <a name="troubleshooting-tips-for-xamarinios"></a>Xamarin.iOS のトラブルシューティングのヒント 

## <a name="xamarinios-cannot-resolve-systemvaluetuple"></a>Xamarin.iOS は System.ValueTuple を解決することはできません。

Visual Studio と互換性がないのため、このエラーが発生します。

- **Visual Studio 2017 Update 1** (バージョン 15.1 以前) は互換性のみ**System.ValueTuple NuGet 4.3.0** (またはそれ以前)。

- **Visual Studio 2017 Update 2** (バージョン 15.2 以降) と互換性のあるのみ、 **System.ValueTuple NuGet 4.3.1**またはそれ以降。

Visual Studio 2017 のインストールに対応する正しい System.ValueTuple NuGet を選択してください。


## <a name="receiving-error-retrieving-update-information-error-message"></a>'の更新情報の取得エラー' にエラー メッセージ

ソフトウェアと、このエラー メッセージを更新しようとしてが表示されたら、IDE で、IDE とログアウトして、アカウントに戻りますを再起動してください。

## <a name="how-do-i-create-outlets-or-actions-with-interface-builder"></a>インターフェイス ビルダーで outlet またはアクションを作成する方法

Mac および Visual studio 用 Xamarin Visual Studio で iOS デザイナーの導入に伴いに、Xamarin.iOS の開発者は、ストーリー ボードと .xibs によって UI を作成する利点にようになりましたをできます。 参照してください、[こんにちは, iOS](~/ios/get-started/hello-ios/index.md)デザイナーの使用の詳細についてガイドします。

Apple を参照することもできます。[アウトレット](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html)と[アクション](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingAction.html)IB で Outlet と Action を使用する方法についてガイドします。

## <a name="systemtextencodinggetencoding-throws-notsupportedexception"></a>System.Text.Encoding.GetEncoding NotSupportedException をスローします。

既定で追加されていないエンコーディングを使用することがあります。 チェック、[国際化](~/ios/app-fundamentals/localization/index.md)ページに詳細をエンコードするためのサポートを追加する方法について説明します。

## <a name="systemmissingmethodexception-anything-else"></a>(その他) System.MissingMethodException

メンバーは、リンカーによって削除された可能性があり、そのため、実行時にアセンブリに存在しません。  このソリューションをいくつかあります。

- 追加、 [ `[Preserve]` ](http://www.go-mono.com/docs/index.aspx?link=T:MonoTouch.Foundation.PreserveAttribute)属性のメンバーにします。  これは、ため、リンカーは削除されなくなります。
- 呼び出すときに[ **mtouch**](http://www.go-mono.com/docs/index.aspx?link=man:mtouch%281%29)を使用して、 **- nolink**または **- linksdkonly**オプション。
  - **-nolink**すべてのリンクを無効にします。
  - **-linksdkonly** 、Xamarin.iOS で提供されるアセンブリをなどリンクのみ**xamarin.ios.dll**ユーザーが作成したアセンブリ内のすべての型を維持しながら (つまり。 アプリ プロジェクト)。

アセンブリがリンクされているため、結果として得られる実行可能ファイルが小さことに注意してください。したがって、リンクを無効にすることがありますが望ましいよりも大きな実行可能。

## <a name="you-are-getting-a-modelnotimplementedexception"></a>ModelNotImplementedException を取得します。

この例外が発生する場合は、基本呼び出すことを意味します。モデルをオーバーライドするクラスで () メソッド (これらは、[モデル] 属性でフラグが設定されたクラス) モデルのクラスで基本メソッドを呼び出す必要はありません。

## <a name="this-class-is-not-key-value-coding-compliant-for-the-key-xxxx"></a>このクラスは、キー値コーディングに準拠して XXXX キーです。

NIB ファイルの読み込み時にこのエラーが発生した場合は、つまり値 XXXX は、マネージ クラスには見つかりませんでした。 これは、このような宣言がないことを意味します。

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

上記の定義が自動的に生成 Visual Studio for Mac を Visual Studio for Mac で追加するすべての XIB ファイルに対して、`NAME_OF_YOUR_XIB_FILE.designer.xib.cs`ファイル。

サブクラスである必要がありますさらに、上記のコードを含む型[NSObject](xref:Foundation.NSObject)します。  含んでいる型が名前空間内にある場合であることが、 [[登録]](xref:Foundation.RegisterAttribute) (Interface Builder では、型の名前空間をサポートしない) ために、名前空間のない型名を提供する属性。

```csharp
namespace Samples.GLPaint {

    // The [Register] attribute overrides the type name registered
    // with the Objective-C runtime, in this case removing the namespace.
    [Register ("AppDelegate")]
    public class AppDelegate {/* ... */}
}
```

## <a name="unknown-class-xxxx-in-interface-builder-file"></a>不明なクラス ファイルの Interface Builder で XXXX

インターフェイス ビルダー ファイルにクラスを定義するのでは、実際の実装を指定しない場合、このエラーが生成された、C#コード。

このようなコードを追加する必要があります。

```csharp
public partial class MyImageView : UIView {
   public MyImageView (IntPtr handle) : base (handle {}
}
```
## <a name="systemmissingmethodexception-no-constructor-found-for-foobarctorsystemintptr"></a>: System.MissingMethodExceptionFoo.Bar::ctor(System.IntPtr) のコンス トラクターがありません。

コードが、インターフェイス ビルダー ファイルから参照されているクラスのインスタンスをインスタンス化を試みると、実行時にこのエラーが生成されます。 これは、1 つの IntPtr をパラメーターとして受け取るコンス トラクターを追加するを忘れてことを意味します。

IntPtr ハンドルを持つコンス トラクターは、アンマネージ表現を持つマネージ オブジェクトのバインドに使用されます。

これを解決するには、Foo.Bar クラスに次のコード行を追加します。

```csharp
public Bar (IntPtr handle) : base (handle) { }
```
## <a name="type-foo--does-not-contain-a-definition-for-getnativefield-and-no-extension-method-getnativefield-of-type-foo-could-be-found"></a>型 {0} Foo} の定義が含まれていない`GetNativeField`拡張メソッド`GetNativeField`型の {Foo} が見つかりませんでした

生成されたデザイナーのファイルでこのエラーが発生したかどうか (*. xib.designer.cs)、次の 2 つのいずれかになります。

 **1) 部分クラスまたは基底クラスがありません。**

デザイナーで生成された部分クラスのいくつかのサブクラスから継承するユーザー コードでの対応する部分クラスがあります`NSObject`、多くの場合、`UIViewController`します。 このようなエラーを提供する型のクラスがあることを確認します。

 **2) 既定の名前空間が変更されました**

デザイナー ファイルは、プロジェクトの既定の名前空間の設定を使用して生成されます。 これらの設定を変更またはプロジェクトの名前を変更した場合、生成された部分クラスが対応するユーザー コードと同じ名前空間ではなく可能性があります。

Namespace 設定は、プロジェクト オプション ダイアログにあります。 既定の名前空間がある、**全般にメインの設定]-> [** セクション。 空白の場合、プロジェクトの名前は、既定値として使用されます。 名前空間のより高度な設定が記載されて、**ソース コードに .NET の命名ポリシー]-> [** セクション。

## <a name="warning-for-actions-the-private-method-foo-is-never-used-cs0169"></a>アクションの警告:プライベート メソッド 'Foo' は使用されません。 (CS0169)

インターフェイス ビルダー ファイルに対する操作は、この警告が予想されるため、実行時にリフレクションによって、ウィジェットに接続されます。

"#Pragma warning disable 0169"を使用することができます"#pragma 警告の有効化 0169" 操作をこれらのメソッドだけにこの警告を抑制するか (not プロジェクト全体を無効にする場合は、コンパイラ オプションの「を無視する警告」のフィールドに 0169 を追加する場合推奨)。

## <a name="mtouch-failed-with-the-following-message-cannot-open-assembly-pathtoyourprojectexe"></a>mtouch は、次のメッセージで失敗しました。アセンブリを開くことができません '/path/to/yourproject.exe'

このエラー メッセージを表示する場合は、一般に問題は、プロジェクトへの絶対パスにスペースが含まれています。 これは、Xamarin.iOS の将来のバージョンで修正されますが、スペースなしのフォルダーにプロジェクトを移動することによって問題を回避することができます。

## <a name="your-sqlite3-version-is-old---please-upgrade-to-at-least-v350"></a>Sqlite3 バージョンが古い - 以上にアップグレードしてください v3.5.0!

これは、次のすべての操作を行う場合に発生します。

1.  Mono.Data.Sqlite を使用します。
1.  Mac OS X Leopard (10.5) を使用して、
1.  シミュレーターでアプリを実行します。


問題は、Mono は、OS X の集荷こと`libsqlite3.dylib`、いない iPhoneSimulator の`libsqlite3.dylib`ファイル。 アプリ*は*デバイスが、シミュレーターではないだけで動作します。

## <a name="deploy-to-device-fails-with-systemexception-amdeviceinstallapplication-returned-3892346901"></a>System.Exception で失敗するデバイスに展開します。AMDeviceInstallApplication 3892346901 が返されます

このエラーは、コード署名証明書/バンドル id の構成では、デバイスにインストールされているプロビジョニング プロファイルが一致しないことを意味します。  確認する、適切な証明書があるプロジェクトのオプションで選択されているバンドル署名 、iPhone]-> [し、プロジェクトのオプションで指定された正しいバンドル id が iPhone アプリケーション ->

## <a name="code-completion-is-not-working-in-visual-studio-for-mac"></a>For Mac、Visual Studio でのコード補完機能が動作しません。

Mac および Xamarin.iOS for Visual Studio の最新バージョンを使用していることを確認します。

問題がまだ存在する場合は、次のようにしてください[バグ](http://monodevelop.com/Developers#Reporting_Bugs)、アタッチ、 **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**、 **AndroidTools-{のタイムスタンプ} .log**。、および**コンポーネント-{のタイムスタンプ} .log**ログ ファイル。

すべてうまくいかなかった場合が再生成されるように、コード入力候補のキャッシュを削除を行うことができます。

 `[rm -r ~/.config/XamarinStudio-{VERSION}/CodeCompletionData]`

このコマンドを正しく入力したこと、または誤って重要なファイルを削除する可能性がありますに注意します。

## <a name="visual-studio-for-mac-crashes-when-you-copy-text"></a>テキストをコピーするときに、visual Studio for Mac がクラッシュします。

人気のある Mac ユーティリティ形式、Google ツールバーおよび起動バーには、Visual Studio を Mac のメモリが破損するクリップボード機能があります。 それぞれのオプションでを妨害しないように、プロセスとして Visual Studio for Mac に一覧表示できます。

## <a name="visual-studio-for-mac-complains-about-mono-24-required"></a>Visual Studio for Mac に必要な Mono 2.4 に関する文句を言う

すべて行う必要がある場合は、最近の更新のための Visual Studio for Mac のアップデートを開始しようとするときにもう一度それに関する文句を言うが存在しない Mono 2.4、 [2.4 の Mono のインストールをアップグレード](http://www.go-mono.com/mono-downloads/download.html)します。  

Mono 2.4.2.3_6 では、Visual Studio for Mac を実行できない、確実にハングした場合の Visual Studio for Mac の起動時にまたはから生成されるコードの入力候補データベースを防止するいくつかの重要な問題を修正します。

新しい Mono をインストールした後、Visual Studio for Mac は、想定どおりに開始されます。

## <a name="assertion-at-monometadatageneric-sharingc704-condition-oti-not-met"></a>アサーションに./../../../mono/metadata/generic-sharing.c:704、条件が満たされていない ' oti'

次のスタック トレース: 受信した場合

```csharp
 - Assertion at ../../../../mono/metadata/generic-sharing.c:704, condition `oti' not met
Stacktrace:
    at System.Collections.Generic.List`1<object>..cctor () <0xffffffff>
    at System.Collections.Generic.List`1<object>..cctor () <0x0001c>
    at (wrapper runtime-invoke) object.runtime_invoke_dynamic (intptr,intptr,intptr,intptr) <0xffffffff>`
```

プロジェクトに thumb コードでコンパイルされた静的ライブラリをリンクしていることを意味します。 3\.1 (またはこの記事の執筆時に高い) に iPhone SDK のリリースの時点で非 Thumb コード (Xamarin.iOS) と Thumb コード (スタティック ライブラリ) をリンクするときに、Apple は、リンカーのバグを導入しました。この問題を軽減するスタティック ライブラリの Thumb 以外のバージョンとリンクする必要があります。

## <a name="systemexecutionengineexception-attempting-to-jit-compile-method-wrapper-managed-to-managed-foosystemcollectionsgenericicollection1getcount-"></a>System.ExecutionEngineException:Attempting to JIT compile method (wrapper managed-to-managed) Foo[]:System.Collections.Generic.ICollection`1.get_Count ()

[サフィックスは、ことか、クラス ライブラリ メソッドを呼び出す IEnumerable <>、ICollection <> IList <> などのジェネリック コレクションを配列のことを示します。 この問題を回避するを自分でメソッドを呼び出すことによってこのようなメソッドを含める AOT コンパイラを明示的に強制することができ、ことを確認することによって、例外をトリガーした呼び出しの前にこのコードを実行します。 この場合は、次のように記述する可能性があります。

```csharp
Foo [] array = null;
int count = ((ICollection<Foo>) array).Count;
```

Get_Count メソッドを含める AOT コンパイラを強制します。

## <a name="visual-studio-for-mac-source-editor-is-extremely-slow"></a>Visual Studio for Mac ソース エディターが極端に遅くなります。

Visual Studio for Mac ソース エディターなる非常に低速のハングを文字の入力間の数秒間に表示されます。

この問題には、非常にまれであり、再現するために非常に困難 - は、通常は再現されません、同じコンピューター上 for mac。 Visual Studio を再起動した後 このためご提案があるその for Mac、Visual Studio を再起動する前にいくつかのデバッグ手順を実行して結果を送信していただいた場合。

1.  エディターのタブを終了してもう一度開くとします。 少しの編集や、速度の低下が再び発生するまで、カレットの移動がかかるのでしょうか。
1.  "Quartz Debug"の開発者ツールを使用して"ビーム Sync"を無効にする (検索できる Spotlight を使用して)、ソース エディターのパフォーマンスが正常に復元するかどうかを確認します。
1.  ビーム同期が無効のままで、(1) の手順を繰り返しお試しください。
1.  エディターが数秒以上ハングした場合は、実行するお試しください"killall-終了 [Visual Studio for Mac]"がハングしているときに、端末でします。 時間の中に、エディターがハングしても、コマンドがすべてのスレッドのスタック トレースをログに書き込む、MD、使用できる、スレッドは、XS が停止しているときに、どのような状態を検出する Mono を強制するため、このようにする必要が発生する、kill コマンドにくい場合があります。



XS のログを添付してください **~/Library/Logs/XamarinStudio-{VERSION}/Ide-{TIMESTAMP}.log**、 **AndroidTools-{のタイムスタンプ} .log**、および**コンポーネント-{のタイムスタンプ} .log**(XS/MonoDevelop の以前のバージョンでのみ送信 **~/Library/Logs/MonoDevelop-(3.0|2.8|2.6)/MonoDevelop.log**)。

 **注:上の問題は、XS 2.2 最後で修正されました**

## <a name="compiled-application-is-very-large"></a>コンパイルされたアプリケーションが非常に大きい

デバッグをサポートするには、デバッグ ビルドには追加のコードが含まれます。 リリース モードでビルドされたプロジェクトは、サイズの割合です。

Xamarin.iOS 1.3、デバッグの時点では、ビルドは、Mono (フレームワークのすべてのクラスのすべてのメソッド) の 1 つのコンポーネントはすべてのデバッグをサポート含まれています。  

Xamarin.iOS 1.4 でデバッグをよりきめ細かな設定メソッドを導入する予定、のみ、コードや、ライブラリのデバッグのインストルメンテーションを提供し、このすべての操作が既定となり、 [Mono アセンブリ](~/cross-platform/internals/available-assemblies.md)(これがあります。可能であればはそれらのアセンブリのデバッグにオプトインする必要があります)。

## <a name="installation-hangs"></a>インストールがハングします。

実行されている iPhone シミュレーターがある場合、Mono と Xamarin.iOS の両方のインストーラーがハングします。 この問題は、Mono または Xamarin.iOS に制限することはありません、iPhone シミュレーターがインストール時に実行されている場合は、MacOS 雪ヒョウ柄のソフトウェアのインストールを試行しているすべてのソフトウェアの間で一貫性のある問題になります。

ように、iPhone シミュレーターを終了して、インストールを再試行してください。

<a name="trampolines" />

## <a name="ran-out-of-trampolines-of-type-0"></a>0 の種類の領域不足になりました

デバイスの実行中にこのメッセージを取得する場合は、プロジェクトのオプションの"iPhone ビルド"セクションを変更することでより多くの種類 (特定の入力) 0 の領域を作成できます。  余分な引数は、デバイスのビルド ターゲットを追加するには。

 `-aot "ntrampolines=2048"`

領域の既定の数は、1024 です。  アプリケーションには十分に設定するまで、この数を増やしてください。

## <a name="ran-out-of-trampolines-of-type-1"></a>種類 1 の領域不足になりました

再帰的なジェネリックの頻繁に使用する場合は、このメッセージがデバイスを取得します。  プロジェクト オプションの"iPhone ビルド"セクションを変更することでより多くの種類 1 の領域 (型 RGCTX) を作成できます。  余分な引数は、デバイスのビルド ターゲットを追加するには。

 `-aot "nrgctx-trampolines=2048"`

領域の既定の数は、1024 です。  ジェネリックの使用量の十分なになるまで、この数を増やしてください。

## <a name="ran-out-of-trampolines-of-type-2"></a>タイプ 2 の領域不足になりました

インターフェイスを多用する場合は、このメッセージをデバイスで発生した可能性があります。
プロジェクト オプションの"iPhone ビルド"セクションを変更することでは、2 つの領域 (型 IMT サンク) でより多くの種類を作成できます。  余分な引数は、デバイスのビルド ターゲットを追加するには。

 `-aot "nimt-trampolines=512"`

IMT サンク領域の既定の数は 128 です。  インターフェイスの使用量の十分なになるまで、この数を増やしてください。

## <a name="debugger-is-unable-to-connect-with-the-device"></a>デバッガーがデバイスに接続できません。

デバイスの構成のデバッグを開始するときに、アプリケーションに接続する中であることを示すダイアログ ボックスを表示する、デバッガーが表示されます。 (USB または WiFi) 接続に使用するモードによってはいくつかの理由が、デバッガーでアプリケーションに接続されない場合があります。

 **デバイスとデバッガー ホストが異なるネットワーク上にある場合**、ファイアウォールまたはプライベート ネットワークを妨げる可能性がある WiFi モードでデバッガー ホストへの接続からアプリケーション。

 **Visual Studio for Mac で、ホストの適切な IP をクエリできない**します。 WiFi でモード Visual Studio for Mac では、アプリケーションのホストのすべての Ip が見つかるし、アプリケーションがすべてを参照するようかどうかは、Visual studio for mac の接続に使用、いずれかができます。

 **別のデバイスは、ホスト上の USB ポートに接続されます。** その他のデバイスが USB に接続されているいくつかのケースで、ホスト上のポートは何らかの形で USB モードのデバッグの妨げに知られています。

USB または WiFi のいずれかのモードが機能しないことができます簡単にしようとした他の: Visual studio for Mac では、環境設定を開き、デバッガーの設定/デバッガー/iPhone ページに移動し、"の代わりに WiFi 経由での iOS デバイスを USB 経由でに Debug"のチェック ボックスを切り替えます。   どちらでもない場合は、詳細モードでデバイスのコンソール内の失敗に関する詳細を確認できます (追加することで有効になっています"-v-v-v-v"をプロジェクトのオプションで追加 mtouch 引数)。

## <a name="error-134-mtouch-failed-with-the-following-message"></a>エラー 134: mtouch は、次のメッセージで失敗しました。

リリースの Xamarin.iOS 1.4 スタイルを - nolink でビルドする場合、このエラーを発生可能性があります。 このエラーは、monodevelop プロジェクト構成に追加の引数を指定することによって回避できます。

引数を追加します。

 `-nosymbolstrip`

問題を解決する必要があります。

## <a name="distribution-identity-is-not-shown-in-visual-studio-for-mac-project-signing-options"></a>Distribution id に表示されない Visual Studio for Mac プロジェクトの署名オプション

Visual Studio for Mac 2.2 は、問題があるため、コンマを含む配布証明書が検出されないようにします。 Mac 2.2.1 の Visual Studio を更新してください。

## <a name="error-afcfilerefwrite-returned-1-during-upload"></a>エラー"AFCFileRefWrite が返されます。1"アップロード中

デバイスにアプリのアップロード中にエラーが表示される可能性があります"AFCFileRefWrite が返されます。1". これは、長さゼロのファイルがある場合に発生することができます。

## <a name="error-mtouch-failed-with-no-output"></a>エラー"mtouch 出力なしで失敗しました"

Xamarin.iOS と Visual Studio for Mac の現在のリリースが失敗する場合に、プロジェクト名またはスペースを含むソリューションまたはプロジェクトが格納されているディレクトリ。
これを修正するには、次を実行してください。


-  プロジェクトでも、ディレクトリの場所に格納されますが、空白文字を含めることを確認します。
-  プロジェクト「Main 設定」では、プロジェクト名にスペースが含まれていないことを確認します。

## <a name="error-the-binary-you-uploaded-was-invalid-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-application"></a>エラー"アップロードしたバイナリが無効です。 SDK のプレリリース ベータ版は、アプリケーションのビルドに使用された"

このエラーは、通常、Xamarin.iOS 2.0.0 をリリースする前に、iPad の開発で開始されたプロジェクトで発生、info.plist のようないくつかのキーがある可能性があります。

```xml
<key>UIDeviceFamily</key>
       <array>
               <string>1</string>
       </array>
```

Visual Studio for Mac にするには自動的に処理と、このキー ペアを削除する必要があります。

## <a name="error-a-pre-release-beta-version-of-the-sdk-was-used-to-build-the-app"></a>エラー「アプリをビルドする SDK のプレリリース ベータ版が使用されました」

(Ed Anuff に起因)

この場合は、以下の手順に従ってください。

-  SDK のバージョン 3.2 または iTunes を iPhone ビルドでの接続の変更によって拒否されますアップロード時に 3.2 より小さい、SDK のバージョンを使用して構築された iPad 互換性のあるアプリを見たので、
-  プロジェクトのカスタム Info.plist を作成しが 3.0 に MinimumOSVersion を明示的に設定します。   これにより、Xamarin.iOS で設定された MinimumOSVersion 3.2 値が上書きされます。   これを行わない場合、アプリは iPhone で実行することはできません。
-  再構築、zip、およびアップロードを iTunes に接続します。

## <a name="unhandled-exception-systemexception-failed-to-find-selector-someselector-on-type"></a>未処理の例外:System.Exception: セレクター someSelector が見つかりませんでした: {type} の

この例外が発生して、次の 3 つのいずれかで。


1.  メソッドに対応する [エクスポート] 属性を適用せず、OBJECTIVE-C ランタイムのセレクターを指定しました。
1.  フル リンクを有効にし、[エクスポート] ed メソッドに [Preserve] 属性は適用されません。
1.  [エクスポート] 属性を継承された型のプライベート メソッドに適用したとします。


## <a name="mainwindowxibdesignercs-file-is-not-updated"></a>MainWindow.xib.designer.cs ファイルが更新されません。

Xamarin Studio 2.4 MainWindow.xib ファイルを新しいプロジェクトで MainWindow.xib.designer ファイル グループがその原因となったバグが発生しました。 これは、ため、これはその特定のファイルのデザイナーのコードを更新できません。

For Mac の組み込みの更新処理で使用できるバージョンの Visual Studio でこの問題が修正されますので、新しいバージョンを使用するかを確認してください。

既存のプロジェクトを修正するには (削除しない) を削除して xib とそのデザイナー ファイルでは、それを追加するバックアップを作成します。 ファイルを正しく再グループこのする必要があります。

## <a name="uialertview-or-uiactionsheet-vanish-after-being-created"></a>作成した後の"uialertview"または UIActionSheet 消える

このようないくつかのコードを場合。

```csharp
var actionSheet = new UIActionSheet ("My ActionSheet", null, null, "OK", null){
   Style = UIActionSheetStyle.Default
};

actionSheet.Clicked += delegate (sender, args){
    Console.WriteLine ("Clicked on item {0}", args.ButtonIndex);
};
```

一時変数の関数として"actionSheet"オブジェクトが存在し、オブジェクトをガベージ コレクションの対象が生じますが、ガベージ コレクションの対象となる、関数が終了するとすぐにします。

この問題を解決するには、"actionSheet"どこかに、メソッドの外部が過ぎると、メソッドへの参照を保持する必要があります。

## <a name="project-always-runs-in-the-ipad-simulator"></a>プロジェクト常に実行 iPad シミュレーター内

IPhone SDK 4.0 インストーラーには、-iPad 専用のアプリを構築するため、3.2、SDK と 4.0 の SDK では、iPhone とユニバーサル アプリを構築するための 2 つの Sdk がインストールされます。 3\.2 シミュレーターが含まれ、iPad のみをシミュレートし、iPhone または iPhone 4 をシミュレートする 4.0 シミュレーターもインストールされます。 古い Sdk とシミュレーターのすべてが削除されます。

Visual Studio for Mac iPhone プロジェクトのビルド オプションでは、アプリのビルドで使用される SDK バージョンの設定が含まれます。 この設定が記載されて**プロジェクト オプション]、ビルドを [iPhone ビルド]-> [** します。

Visual Studio for Mac で新しいプロジェクトの既定の SDK 設定として、最も古いインストールされている SDK を使用して、Visual Studio for Mac が最も近いアプリケーションを作成することを参照して使用して指定されている SDK が存在しない場合。 これは、プロジェクトは常に必要としないように、最新の SDK で行われました。 ただし、この現在結果、3.2、SDK が使用されている iPad シミュレーターで結果を使用します。

4\.0 の SDK を使用してこれを解決するには**プロジェクト オプション]、ビルドを [iPhone ビルド]-> [** > SDK 値をドロップダウン ボックスを使用して「4.0」に変更します。 これは、構成およびアクセス パネルの上部にあるドロップダウン リストを使用して、プラットフォームの組み合わせごとに行う必要があります。

SDK のバージョンの「最小 OS バージョン」の設定とを混同しないでください。
この値は SDK のバージョンの値と一致する必要はありません - 以前の OS に存在するか、ランタイムの OS バージョン チェックを使用して新しい機能の使用を保護する Api のみを使用すると、SDK より古くなる可能性、アプリのインストール、OS の最小バージョンに影響を与えるks します。 アプリをテストする最も古い OS バージョンに設定する必要があります。

なお、**プロジェクト]、[iPhone シミュレーター ターゲット**> プロジェクトの実行/デバッグするときに、既定で使用するシミュレーターを選択するメニューを使用できます。 さらに、**実行で実行]-> [** > を実行する特定のシミュレーターを選択するメニューを使用できます

## <a name="ibtool-returns-error-133"></a>ibtool エラー 133 を返します

これは XCode 4 がインストールされているがあることを意味します。   XCode の 4 ツール ibtool が削除された、スタンドアロン ツールで XIB ファイルを編集することができなくなった。

Interface Builder を使用する場合は、インストール[XCode シリーズ 3](http://connect.apple.com/cgi-bin/WebObjects/MemberSite.woa/wa/getSoftware?bundleID=20792)Apple の web サイトから入手できます。

## <a name="cant-create-display-binding-for-mime-type-applicationvndapple-wbrinterface-builder"></a>"Mime の種類の表示のバインドを作成することはできません: application/vnd.apple-<wbr/>インターフェイス ビルダー"

IPhone ではないプロジェクトから iPhone UI を作成しようとすると、このエラーが発生します。 IPhone または iPad のソリューションを開始する、iPhone または iPad のプロジェクトに iPhone UI 要素を追加することはできませんのことを確認してください。

## <a name="startup-crash-when-executing-inside-the-ios-simulator"></a>IOS シミュレーター内で実行するときにスタートアップ クラッシュ

: 次のようなスタック トレースと共にシミュレーター内でランタイム クラッシュ (SIGSEGV) が発生した場合

```csharp
  at (wrapper managed-to-native) System.Reflection.Assembly.GetTypes (System.Reflection.Assembly,bool)
  at MonoTouch.ObjCRuntime.Runtime.RegisterAssembly (System.Reflection.Assembly)
  at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr)
```
.. 後、シミュレーターのアプリケーション ディレクトリに 1 つ (以上) の古いアセンブリがある可能性があります。 このようなアセンブリは、Apple iOS シミュレーターが追加され、更新ファイルしますが、それらを削除するために存在する可能性があります。 これは最も簡単なソリューションで発生し場合は、シミュレーターのメニューから [リセットおよびコンテンツと設定] を選択するは。   

> [!WARNING]
> すべてのファイル、アプリケーション、データは、シミュレーターからこの削除されます。   アプリケーションの実行時に [次へ] は、Visual Studio for Mac を使用すると、シミュレーターに配置され、古い、古いアセンブリ、クラッシュが発生することはありません。

## <a name="simulator-hangs-during-application-installation"></a>アプリケーションのインストール中にシミュレーターがハングします。

これは、アプリケーション名が含まれる場合に発生することができます、'.'(ドット)、名前にします。
これは、その他の多くの場合 (デバイス) のような動作を可能な場合でも、CFBundleExecutable - で実行可能ファイル名として禁止されています。

 *「、値は、名の任意の拡張子を含める必要がありますしない」です。- [https://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf](https://developer.apple.com/library/mac/documentation/General/Reference/InfoPlistKeyReference/InfoPlistKeyReference.pdf)

## <a name="error-custom-attribute-type-0x43-is-not-supported-when-double-clicking-xib-files"></a>エラー :「0x43 カスタム属性の型がサポートされていません」.xib ファイルをダブルクリックしたとき

これは、環境変数が正しく設定されていないときに、.xib ファイルを開くしようとして原因です。 Mac/Xamarin.iOS の Visual Studio の通常の使用は、この発生する必要がありますしないと、問題の解決/Applications から Mac に Visual Studio を再度開く必要があります。

更新試行時に、ソフトウェアと、このエラー メッセージが表示されたら、ください電子メール *support@xamarin.com*

## <a name="application-runs-on-simulator-but-fails-on-device"></a>アプリケーションは、シミュレーターで実行されますが、デバイスが失敗しました。

この問題は、複数の形式が発生することができ、常に一貫性のあるエラーを生成しません。 アプリケーションに、.xib が含まれている場合は、ことを確認、**ビルド アクション**、.xib でに設定されて**インタ フェース定義**します。 これは、既定のビルド アクションは .xibs です。

ビルド アクションを確認するには、.xib ファイルを右クリックし、選択**ビルド アクション**します。


## <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException:437 をエンコードできるデータがありません。

Xamarin.iOS アプリでは、サード パーティ製ライブラリを含む、ときにフォームでエラーが発生する可能性があります"System.NotSupportedException:エンコード 437"をコンパイルして、アプリを実行しようとするときはデータは有効ではありません。 たとえば、ライブラリなど`Ionic.Zip.ZipFile`操作中にこの例外をスローする可能性があります。

これは、問題を Xamarin.iOS プロジェクトのオプションを開くことで解決できます**iOS ビルド** > **国際化**とチェック、**西部**国際化します。
