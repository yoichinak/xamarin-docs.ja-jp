---
title: サンド ボックス Xamarin.Mac アプリ
description: この記事では、サンド ボックスでは、アプリ ストアのリリースの Xamarin.Mac アプリケーションについて説明します。 すべてのコンテナー ディレクトリ、資格、ユーザー指定のアクセス許可、特権の分離、およびカーネル実施など、サンド ボックスに要素を説明します。
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a02d7639975de092b05f31bacedd6bde4c9392f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30787910"
---
# <a name="sandboxing-a-xamarinmac-app"></a>サンド ボックス Xamarin.Mac アプリ

_この記事では、サンド ボックスでは、アプリ ストアのリリースの Xamarin.Mac アプリケーションについて説明します。すべてのコンテナー ディレクトリ、資格、ユーザー指定のアクセス許可、特権の分離、およびカーネル実施など、サンド ボックスに要素を説明します。_

## <a name="overview"></a>概要

Xamarin.Mac アプリケーションでは、c# と .NET を使用する場合、OBJECTIVE-C または Swift を使用する場合と同様にサンド ボックス アプリケーションに同じ機能があります。

[![実行中のアプリの使用例](sandboxing-images/intro01.png "実行中のアプリの例")](sandboxing-images/intro01-large.png#lightbox)

この記事では、Xamarin.Mac アプリケーションと、サンド ボックスに移動した要素のすべてのサンド ボックス化の操作の基礎をについて説明します。 コンテナー ディレクトリ、資格、ユーザー指定のアクセス許可、特権の分離、およびカーネル実施します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での c# クラスを Objective-C オブジェクトと UI への要素に使用されます。

## <a name="about-the-app-sandbox"></a>アプリのサンド ボックスについて

アプリのサンド ボックスは、アプリケーションによるシステム リソースへのアクセスを制限することによって、Mac で実行される悪意のあるアプリケーションをによる破損に対する厳密な防策を提供します。

非サンド ボックス化されたアプリケーションがアプリを実行しているユーザーの完全な権利とアクセスしたり、ユーザーができることを行います。 場合は、アプリには、セキュリティ ホール (または使用されている任意のフレームワーク) が含まれています、ハッカー可能性のあるそれらの弱点でき、アプリで実行されている、Mac の管理を使用できます。

アプリケーションごとの単位でリソースへのアクセスを制限すると、盗難、破損、またはユーザーのコンピューターで実行中のアプリケーション部分の悪意のある攻撃に対する防御のサンド ボックス化されたアプリを提供します。

アプリ サンド ボックスは、2 つの戦略を提供する (カーネル レベルで適用) macOS に組み込まれているアクセス制御テクノロジを示します。

1. アプリのサンド ボックスにより、記述する開発者_方法_OS とし、この方法で、アプリケーションが対話、付与されているアクセス権を完了したら、ジョブを取得するために必要とされません。
2. アプリのサンド ボックスは、シームレスに開く経由でシステムにさらにアクセス権を付与し、保存 ダイアログ ボックス、ドラッグ アンド ドロップ操作と、一般的な他のユーザーとのやり取りできます。

### <a name="preparing-to-implement-the-app-sandbox"></a>アプリのサンド ボックスを実装する準備をしています

記事で詳しく説明するアプリのサンド ボックスの要素は次のとおりです。

- コンテナー ディレクトリ
- 権利
- ユーザー指定のアクセス許可
- 権限の分離
- カーネルの実施

これらの詳細を理解するを Xamarin.Mac アプリケーションでアプリのサンド ボックスを採用する計画を作成することはされます。

最初に、アプリケーションが (ほとんどのアプリケーションは) のサンド ボックス化の候補かどうかを判断する必要があります。 次に、API 互換性問題を解決して、必要なアプリのサンド ボックスの要素を決定する必要があります。 最後に、アプリケーションの多層レベルを最大化する特権の分離を使用して検索します。

アプリ サンド ボックスを採用するアプリケーションで使用されるいくつかのファイル システムの場所が異なるなります。 特に、アプリケーションでは、アプリケーションのサポート ファイル、データベース、キャッシュおよびその他のユーザー ドキュメントではないファイルに使用されるコンテナー ディレクトリがあります。 MacOS と Xcode の両方に移行するファイルのこれらの種類、従来の場所から、コンテナーのサポートを提供します。

## <a name="sandboxing-quick-start"></a>サンド ボックス化のクイック スタート

このセクションでをアプリのサンド ボックスの概要の例として、(具体的には要求されない限り、サンド ボックスの下で禁止されているネットワーク接続が必要) を Web ビューを使用した簡単な Xamarin.Mac アプリを作成します。

おすることを確認アプリケーションが実際にはセキュリティで保護されたトラブルシューティングし、一般的なアプリのサンド ボックス エラーを解決する方法について説明します。

### <a name="creating-the-xamarinmac-project"></a>Xamarin.Mac プロジェクトを作成します。

それでは、サンプル プロジェクトを作成するには、次の操作を行います。

1. Mac とクリックの Visual Studio を起動、**新しいソリューション.** リンクをクリックしてください。
2. **新しいプロジェクト**ダイアログ ボックスで、 **Mac** > **アプリ** > **Cocoa アプリ**: 

    [![新しい Cocoa アプリを作成する](sandboxing-images/sample01.png "新しい Cocoa アプリの作成")](sandboxing-images/sample01-large.png#lightbox)
3. をクリックして、 **次へ**ボタンを入力`MacSandbox`のプロジェクトの名前と をクリック、**作成**ボタン。 

    [![アプリ名を入力する](sandboxing-images/sample02.png "アプリ名を入力します。")](sandboxing-images/sample02-large.png#lightbox)
4. **ソリューション パッド**をダブルクリックして、 **Main.storyboard**ファイルを開いて Xcode で編集するファイル。 

    [![メインのストーリー ボードの編集](sandboxing-images/sample03.png "メインのストーリー ボードの編集")](sandboxing-images/sample03-large.png#lightbox)
5. ドラッグ、 **Web ビュー**ウィンドウのサイズをコンテンツ領域を入力し、拡大および縮小ウィンドウを使用するように設定します。 

    [![Web ビューを追加する](sandboxing-images/sample04.png "web ビューを追加します。")](sandboxing-images/sample04-large.png#lightbox)
6. という名前の web ビューのコンセントを作成する`webView`: 

    [![新しいコンセントを作成する](sandboxing-images/sample05.png "新しいコンセントを作成します。")](sandboxing-images/sample05-large.png#lightbox)
7. Mac とダブルクリックの Visual Studio に戻り、 **ViewController.cs**ファイルで、**ソリューション パッド**編集用に開きます。
8. 次の追加ステートメントを使用します。 `using WebKit;`
9. ように、`ViewDidLoad`次のようなメソッドの検索。 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. 変更内容を保存します。

アプリケーションを実行し、Apple web サイトが次のように、ウィンドウに表示されていることを確認します。

[![例のアプリの実行を示す](sandboxing-images/sample06.png "例のアプリの実行を表示")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>署名とアプリのプロビジョニング

アプリのサンド ボックスを有効にしましたが、前に、Xamarin.Mac アプリケーションへの署名をプロビジョニングお必要があります。

次の操作ができます。

1. Apple 開発者ポータルにログインします。 

    [![Apple 開発者ポータルにログイン](sandboxing-images/sign01.png "Apple 開発者ポータルにログインします。")](sandboxing-images/sign01-large.png#lightbox)
2. 選択**証明書、識別子、およびプロファイル**: 

    [![証明書、ID、プロファイルの選択](sandboxing-images/sign02.png "証明書、ID、プロファイルの選択")](sandboxing-images/sign02-large.png#lightbox)
3. **Mac アプリ****識別子**: 

    [![識別子を選択すると](sandboxing-images/sign03.png "識別子を選択します。")](sandboxing-images/sign03-large.png#lightbox)
4. アプリケーションの新しい ID を作成します。 

    [![新しいアプリ ID を作成する](sandboxing-images/sign04.png "新しいアプリ ID を作成します。")](sandboxing-images/sign04-large.png#lightbox)
5. **プロビジョニング プロファイル****開発**: 

    [![開発を選択すると](sandboxing-images/sign05.png "開発を選択します。")](sandboxing-images/sign05-large.png#lightbox)
6. 新しいプロファイルを作成し、選択**Mac アプリの開発**: 

    [![新しいプロファイルを作成する](sandboxing-images/sign06.png "新しいプロファイルを作成します。")](sandboxing-images/sign06-large.png#lightbox)
7. 上で作成したアプリ ID を選択します。 

    [![アプリ ID を選択すると](sandboxing-images/sign07.png "アプリ ID を選択します。")](sandboxing-images/sign07-large.png#lightbox)
8. このプロファイルの開発者を選択します。 

    [![追加する開発者](sandboxing-images/sign08.png "追加開発者")](sandboxing-images/sign08-large.png#lightbox)
9. このプロファイルのコンピューターを選択します。 

    [![許可されているコンピューターを選択すると](sandboxing-images/sign09.png "許可されているコンピューターの選択")](sandboxing-images/sign09-large.png#lightbox)
10. プロファイルに名前を付けます。 

    [![プロファイルの名前を付ける](sandboxing-images/sign10.png "プロファイルの名前を付ける")](sandboxing-images/sign10-large.png#lightbox)
11. クリックして、**完了**ボタンをクリックします。

> [!IMPORTANT]
> 一部のインスタンスでは、Apple の開発者ポータルから直接新しいプロビジョニング プロファイルをダウンロードし、ダブルクリックしてインストールする必要があります。 停止し、新しいプロファイルにアクセスが可能となる前に、Mac 用 Visual Studio を再起動する必要もあります。

次に、開発用コンピューターに新しいアプリ ID とプロファイルを読み込む必要があります。 それでは、次の操作を行います。

1. Xcode を起動し、選択**設定**から、 **Xcode**メニュー。 

    ![Xcode にアカウントを編集](sandboxing-images/sign11.png "Xcode にアカウントの編集")
2. をクリックして、**の詳細を表示しています.** ボタンをクリックします。 

    ![詳細の表示 ボタンをクリックして](sandboxing-images/sign12.png "詳細の表示 をクリックします。")
3. をクリックして、**更新** ボタン (左隅)。
4. をクリックして、**完了**ボタンをクリックします。

次に、Xamarin.Mac プロジェクトに新しいアプリ ID とプロビジョニング プロファイルを選択する必要があります。 それでは、次の操作を行います。

1. **ソリューション パッド**をダブルクリックして、 **Info.plist**ファイルを開いて編集するファイル。
2. いることを確認、**バンドル Id**上で作成した、アプリ ID と一致する (例: `com.appracatappra.MacSandbox`)。 

    [![バンドル Id を編集](sandboxing-images/sign13.png "バンドル Id の編集")](sandboxing-images/sign13-large.png#lightbox)
3. 次をダブルクリックして、 **Entitlements.plist**ファイルを確認して、 **iCloud キー値ストア**と**iCloud コンテナー**上で作成したアプリ ID は、すべて一致 (例。`com.appracatappra.MacSandbox`): 

    [![Entitlements.plist ファイルを編集](sandboxing-images/sign17.png "Entitlements.plist ファイルの編集")](sandboxing-images/sign17-large.png#lightbox)
3. 変更内容を保存します。
4. **ソリューション パッド**、プロジェクト ファイルを編集するためのオプションを開く をダブルクリックします。  

    ![Editign ソリューションのオプション](sandboxing-images/sign14.png "Editign ソリューションのオプション")
5. 選択**Mac 署名**、し確認**アプリケーション バンドルに署名**と**インストーラー パッケージに署名**です。 **プロビジョニング プロファイル**、上で作成した 1 つを選択します。 

    ![プロビジョニング プロファイルの設定](sandboxing-images/sign15.png "プロビジョニング プロファイルの設定")
6. クリックして、**完了**ボタンをクリックします。

> [!IMPORTANT]
> 終了し、新しいアプリ ID と Xcode でインストールされているプロビジョニング プロファイルを認識するための Mac 用の Visual Studio を再起動する必要があります。

#### <a name="troubleshooting-provisioning-issues"></a>プロビジョニングの問題のトラブルシューティング

この時点でをアプリケーションを実行し、すべてしを正しくプロビジョニングされていることを確認してください。 実行する場合、アプリが以前と同様、すべてのものをお勧めします。 障害が発生した場合は、次のようなダイアログ ボックスをする可能性があります。

[![問題のダイアログをプロビジョニング例](sandboxing-images/sign16.png "問題ダイアログをプロビジョニングする例")](sandboxing-images/sign16-large.png#lightbox)

プロビジョニングおよび署名に関する問題についての最も一般的な原因を次に示します。

- アプリ バンドル ID は、選択したプロファイルのアプリ ID と一致しません。
- 開発者の ID は、開発者の選択したプロファイルの ID と一致しません。
- テストされている Mac の UUID は、選択したプロファイルの一部として登録されていません。

問題の場合、Apple 開発者ポータルで問題を修正、Xcode でプロファイルを更新および for mac Visual Studio で、クリーン ビルドを行う

### <a name="enable-the-app-sandbox"></a>アプリのサンド ボックスを有効にします。

アプリのサンド ボックスを有効にするには、プロジェクト オプションのチェック ボックスを選択します。 次の手順で行います。

1. **ソリューション パッド**をダブルクリックして、 **Entitlements.plist**ファイルを開いて編集するファイル。
2. 両方を確認**権利を有効にする**と**アプリ サンド ボックス化を有効にする**: 

    [![権利を編集し、サンド ボックス化を有効にする](sandboxing-images/sign17.png "権利を編集し、サンド ボックス化を有効にします。")](sandboxing-images/sign17-large.png#lightbox)
3. 変更内容を保存します。

この時点では、アプリのサンド ボックスが有効になっているが、Web ビューに必要なネットワーク アクセスが指定されていません。 今すぐアプリケーションを実行する場合は、空白のウィンドウを取得する必要があります。

[![Web アクセスがブロックされていることを示す](sandboxing-images/sample08.png "web アクセスがブロックされていることを示す")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>アプリがサンド ボックス化されたことを確認しています

ブロックの動作、リソースとは別 Xamarin.Mac アプリケーションが正常にサンド ボックス化されたかどうかに指示する 3 つの主な方法があります。

1. Finder での内容を確認、`~/Library/Containers/`フォルダーの場合は、アプリは、サンド ボックスがあります、アプリのバンドル Id などという名前のフォルダー (例: `com.appracatappra.MacSandbox`)。 

    [![アプリのバンドルを開く](sandboxing-images/sample09.png "アプリのバンドルを開く")](sandboxing-images/sample09-large.png#lightbox)
2. システムでは、利用状況モニターでサンド ボックスとして、アプリが表示されます。
    - 利用状況モニターを起動して ( `/Applications/Utilities`)。 
    - 選択**ビュー** > **列**ことを確認して、**サンド ボックス**メニュー項目をチェックします。
    - サンド ボックスの列を読み取っていることを確認してください。`Yes`アプリケーション。 

    [![アプリの利用状況モニターをチェック](sandboxing-images/sample10.png "アプリの利用状況モニターを確認しています")](sandboxing-images/sample10-large.png#lightbox)
3. バイナリのアプリがサンド ボックス化されたことを確認します。
    - ターミナル アプリを起動します。
    - アプリケーションに移動`bin`ディレクトリ。
    - このコマンドを発行: `codesign -dvvv --entitlements :- executable_path` (ここで`executable_path`アプリケーションへのパスです)。 

    [![コマンドライン上でのアプリをチェック](sandboxing-images/sample11.png "コマンドライン上でのアプリを確認しています")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>サンド ボックス化されたアプリのデバッグ

既定では、サンド ボックス化を有効にすることができないことアプリへの接続には、TCP を介して Xamarin.Mac アプリにデバッガーが接続するため、エラーが発生した場合は有効になっている適切な権限がない場合、アプリを実行しようとすると、 *"に接続できません。デバッガー"* です。 

[![必要なオプションを設定する](sandboxing-images/debug01.png "必要なオプションの設定")](sandboxing-images/debug01-large.png#lightbox)

**送信ネットワーク接続を許可する (クライアント)** 権限は、デバッガーのために必要な 1 つは、この許可すると、通常どおりにデバッグします。 更新したなしでデバッグすることはできませんので、`CompileEntitlements`対象の`msbuild`ビルドでのみデバッグのセキュリティで保護されているすべてのアプリの権利にそのアクセス許可を自動的に追加します。 リリース ビルドでは、未変更の状態、権利ファイルで指定された権限を使用してください。

### <a name="resolving-an-app-sandbox-violation"></a>アプリのサンド ボックス違反を解決します。

アプリ サンド ボックス違反は、アプリケーションを持つリソースにアクセスしようとしました。 セキュリティで保護された Xamarin.Mac で明示的に許可されていない場合に発生します。 たとえば、当社 Web ビューされなくなることを Apple web サイトを表示できません。

アプリのサンド ボックス違反の最も一般的なソースは、Mac を Visual Studio で指定された権利の設定は、アプリケーションの要件と一致しないときに発生します。 処理中から、Web ビューを保持する権利をこの例では、不足しているネットワーク接続をもう一度、バックアップします。

#### <a name="discovering-app-sandbox-violations"></a>アプリのサンド ボックスの違反を検出します。

使用して、問題を検出する最も簡単な方法は、Xamarin.Mac アプリケーションで、アプリのサンド ボックス違反が発生するいると思われる場合、**コンソール**アプリ。

次の手順で行います。

1. 対象のアプリからコンパイルおよび実行に Visual Studio for mac
2. 開く、**コンソール**アプリケーション (から`/Applications/Utilties/`)。
3. 選択**すべてのメッセージ**、サイドバーで入力および`sandbox`検索に。 

    [![コンソールで、サンド ボックス上の問題の例](sandboxing-images/resolve01.png "コンソールで、サンド ボックス上の問題の例")](sandboxing-images/resolve01-large.png#lightbox)

この例のアプリ上を参照してください、Kernal をブロックしている、`network-outbound`トラフィック アプリ サンド ボックス、権限がない要求しているためです。

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>アプリのサンド ボックスの権利に関する違反の修正

これでアプリのサンド ボックス化の違反を検索する方法をまで見てきたを見てみましょう方法、アプリケーションの権利を調整することで解決することができます。

次の手順で行います。

1. **ソリューション パッド**をダブルクリックして、 **Entitlements.plist**ファイルを開いて編集するファイル。
2. 下にある、**権利** セクションで、チェック、**送信ネットワーク接続を許可する (クライアント)**  チェック ボックス。 

    [![権利を編集](sandboxing-images/sign17.png "と権利の編集")](sandboxing-images/sign17-large.png#lightbox)
3. アプリケーションに変更を保存します。

例のアプリの前述の処理、し、ビルドを実行する場合、web コンテンツは期待どおりに表示されます。

## <a name="the-app-sandbox-in-depth"></a>詳細なアプリのサンド ボックス

アプリ サンド ボックスによって提供されるアクセス制御メカニズムとは、いくつか理解し、やすいです。 ただし、各アプリでアプリのサンド ボックスを採用することは一意であり、アプリケーションの要件に基づいています。

最大限の努力 Xamarin.Mac アプリケーションから悪意のあるコードによって悪用される保護を指定すると、必要がある脆弱性は 1 つのみにありますか、アプリ (または消費するライブラリやフレームワークのいずれか) と、アプリの対話の制御を取得しますシステムです。

アプリのサンド ボックスは、システムとアプリケーションの目的の相互作用を指定することにより、引き継ぎを防ぐため (または、損害の可能性があります) に設計されました。 システムには、のみと、リソース、アプリケーションがそのジョブの実行の取得を必要として何も詳細へのアクセスが与えられます。

アプリのサンド ボックスを設計する際は、最悪のシナリオを設計します。 アプリケーションが悪意のあるコードによって侵害された場合は場合、は、ファイルと、アプリのサンド ボックス内のリソースへのアクセスに制限されます。

### <a name="entitlements-and-system-resource-access"></a>権利とシステム リソースへのアクセス

上で見たとサンド ボックス化されていない Xamarin.Mac アプリケーションに完全な権限と、アプリを実行しているユーザーのアクセスが付与されます。 悪意のあるコードが侵害された場合、保護されていないアプリが損害を与えることが発生する可能性の範囲全体に悪意のある動作では、エージェントとして動作できます。

アプリ サンド ボックスを有効にするは、する特権の最小限のセット以外はすべて削除し、必要に応じて、Xamarin.Mac アプリの権利を使用するで再び有効にします。 

編集して、アプリケーションのアプリのサンド ボックスのリソースを変更するその**Entitlements.plist**ファイル、またはエディターのドロップダウン ボックスから必要な権限を選択します。

[![権利を編集](sandboxing-images/sign17.png "と権利の編集")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>コンテナー ディレクトリおよびファイル システム アクセス

Xamarin.Mac アプリケーションには、アプリのサンド ボックスが採用されています、ときに、次の場所にアクセスがあります。

- **アプリのコンテナー ディレクトリ**-最初の実行、OS を使用して作成、特殊な_コンテナー ディレクトリ_のみにアクセスできるように、すべてのリソースの移動、します。 アプリでは、このディレクトリへの完全な読み取り/書き込みアクセスがあります。
- **アプリ グループのコンテナー ディレクトリ**-アプリが 1 つまたは複数のアクセスを許可することができます_グループ コンテナー_同じグループ内のアプリ間で共有されています。
- **ユーザー指定ファイル**-アプリケーションが自動的には明示的に開かれたかにドラッグ アンド ドロップ アプリケーション ユーザーがファイルへのアクセスを取得します。
- **関連するアイテム**-、適切な権利と、アプリケーションが保有できるアクセス別の拡張機能は、同じ名前を持つファイルです。 たとえば、両方と保存されているドキュメント、`.txt`ファイルと`.pdf`です。
- **一時ディレクトリに、コマンド ライン ツール ディレクトリ、および特定の世界が判読できる場所**-アプリでは、さまざまなレベルのアクセスをシステムによって指定されたように適切に定義された他の場所にファイルにします。

#### <a name="the-app-container-directory"></a>アプリのコンテナー ディレクトリ

Xamarin.Mac アプリケーションのアプリのコンテナー ディレクトリには、次の特徴があります。

- ユーザーのホーム ディレクトリ内の非表示の場所にある (通常`~Library/Containers`) し、アクセスするのには、`NSHomeDirectory`関数 (下記参照)、アプリケーション内で。 ホーム ディレクトリになっているため各ユーザーはアプリの独自のコンテナーを取得します。
- アプリでは、コンテナーのディレクトリとそのすべてのサブディレクトリとその中のファイルに読み取り/書き込みアクセスは無制限です。
- MacOS のパス検索 Api のほとんどはアプリのコンテナーを基準として、します。 たとえば、コンテナーがある独自**ライブラリ**(を使用してアクセス`NSLibraryDirectory`)、**アプリケーションのサポート**と**設定**サブディレクトリです。
- macOS 確立し、間の接続を適用し、アプリとコード署名を使用してコンテナーです。 使用して、アプリを偽装する別のアプリ試行がでもその**バンドル Id**、コード署名のため、コンテナーにアクセスすることはできません。
- コンテナーのユーザーが生成したファイルではありません。 代わりには、アプリケーションは、データベース、キャッシュ、またはその他の特定のデータ型などで使用されるファイルです。
- _靴箱_(Apple のフォト アプリ) のようなアプリの種類、コンテナーに、ユーザーのコンテンツが移動されます。

> [!IMPORTANT]
> 残念ながら、Xamarin.Mac が 100 %api カバレッジまだ (とは異なり Xamarin.iOS)、その結果、 `NSHomeDirectory` Xamarin.Mac の現在のバージョンの API がマップされていません。

一時的な回避策としては、次のコードを使用できます。

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>アプリ グループのコンテナー ディレクトリ

アプリケーション起動し、Mac macOS 10.7.5 (以降)、使用、`com.apple.security.application-groups`グループ内のすべてのアプリケーションに共通する共有コンテナーにアクセスする権利。 この共有コンテナーは、データベースや他の種類 (キャッシュ) などのサポート ファイルのなどの非ユーザーに公開されたコンテンツを使用することができます。

グループのコンテナーが自動的に追加各アプリのサンド ボックスのコンテナーに (グループの一部である) 場合とで保存される`~/Library/Group Containers/<application-group-id>`です。 グループ ID_必要があります_など、開発チーム ID と、時間と開始します。

```plist
<team-id>.com.company.<group-name>
```

詳細については、Apple を参照してください[アプリケーション グループにアプリケーションの追加](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)で[権利キー参照](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195)です。

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>アプリケーション コンテナー外の Powerbox とファイルのシステムへのアクセス

サンド ボックス Xamarin.Mac アプリケーションは、次のように、コンテナーの外部でファイル システムの場所にアクセスできます。

- (開く、ファイルの保存、またはドラッグ アンド ドロップなど他のメソッド) 経由でユーザーの特定の方向。
- 特定のファイル システム上の場所の権利を使用して (など`/bin`または`/usr/lib`)。
* ファイル システムの場所がの場合は読める (共有) などの特定のディレクトリです。

_Powerbox_ macOS サンド ボックス化された Xamarin.Mac アプリのファイルのアクセス権を展開するユーザーと対話するセキュリティ テクノロジがします。 Powerbox が存在せず、API は、アプリは呼び出しと透過的にアクティブな`NSOpenPanel`または`NSSavePanel`です。 権利 Xamarin.Mac アプリケーションの設定を使用して、Powerbox アクセスが有効です。

サンド ボックス化されたアプリを開いているを表示またはダイアログを保存するときに、Powerbox (およびいない AppKit) プレゼンターし、ウィンドウがすべてのファイルまたはユーザー アクセス権を持つディレクトリへのアクセス。

ユーザー開くからファイルまたはディレクトリを選択または保存ダイアログ (またはアプリのアイコンの上にいずれかがドラッグ)、Powerbox はアプリのサンド ボックスに関連付けられたパスを追加します。

さらに、システムは自動的にサンド ボックス化されたアプリには、次を許可します。

- システムへの接続は、メソッドを入力します。
- ユーザーが選択したサービスを呼び出す、 **Services**メニュー (としてマークされたサービスのみ_サンド ボックス アプリケーションの安全な_サービス プロバイダーによって)。
- ユーザーが開いているファイルを選択して、**開く最近**メニュー。
- その他のアプリケーション間でコピーと貼り付けを使用します。
- 次の世界が判読できる場所からファイルを読み取り。
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- によって作成されたディレクトリ内のファイルの読み書き`NSTemporaryDirectory`です。

既定値をするには、ファイルを開くか、セキュリティで保護された Xamarin.Mac アプリによって保存された引き続きアクセスできる (場合を除く、ファイルは、アプリが終了したときに開いていた) に、アプリが終了するまで。 開いているファイル、次回アプリを起動するときに自動的に macOS 再開機能を使用して、アプリのサンド ボックスに復元されます。

Xamarin.Mac アプリのコンテナーの外部にあるファイルへの永続化を提供するには、セキュリティ スコープのブックマーク (下記参照) を使用します。

#### <a name="related-items"></a>関連項目

アプリ サンド ボックスを使用するアプリを別の拡張機能が、同じファイル名を持つ関連するアイテムにアクセスします。 機能には、2 つの部分:) でアプリの関連する拡張機能の一覧`Info.plst`ファイル、b) コードへのアプリがある何をこれらのファイルをサンド ボックスに通知します。

これには、これは、2 つのシナリオがあります。

1. アプリは、別のバージョンの拡張子を持つ、新しいファイルを保存できる必要があります。 たとえば、エクスポート、`.txt`ファイルの名前を`.pdf`ファイル。 このような状況を処理するために使用する必要があります、`NSFileCoordinator`ファイルへのアクセスします。 呼び出すことになります、`WillMove(fromURL, toURL)`メソッドは、最初に、新しい拡張機能にファイルを移動して、呼び出す`ItemMoved(fromURL, toURL)`です。
2. アプリは、別の拡張子を持つ 1 つの拡張子を持つメイン ファイルといくつかのサポート ファイルを開く必要があります。 たとえば、ムービー、字幕ファイル。 使用して、`NSFilePresenter`セカンダリ ファイルにアクセスするためにします。 メイン ファイルを提供、`PrimaryPresentedItemURL`プロパティおよびセカンダリ ファイルを`PresentedItemURL`プロパティです。 メイン ファイルが開かれたときに呼び出す、`AddFilePresenter`のメソッド、`NSFileCoordinator`セカンダリ ファイルを登録するクラス。

両方のシナリオで、アプリの**Info.plist**ファイルは、アプリを開くことができるドキュメントの種類を宣言する必要があります。 任意のファイルの種類を追加、 `NSIsRelatedItemType` (の値を持つ`YES`) のエントリに、`CFBundleDocumentTypes`配列。

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>開き、サンド ボックス化されたアプリを使用して、ダイアログの動作を保存

適用されるは、次の制限、`NSOpenPanel`と`NSSavePanel`サンド ボックス化された Xamarin.Mac アプリから呼び出すとき。

- プログラムで呼び出すことはできません、 **OK**ボタンをクリックします。
- ユーザーの選択をプログラムから変更することはできません、`NSOpenSavePanelDelegate`です。

さらに、次の継承の変更は、場所には。

- **サンド ボックスではないアプリ** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>ブックマークのセキュリティ スコープと永続的なリソースへのアクセス

前述のように、サンド ボックス化された Xamarin.Mac アプリケーションは、ファイルまたはそのコンテナー (PowerBox によって提供される) とユーザーの直接の相互作用を使用して外部リソースにアクセスできます。 ただし、これらのリソースへのアクセスは自動的に保持されないアプリが起動またはシステムで再起動します。

使用して_Security-Scoped ブックマーク_、サンド ボックス化された Xamarin.Mac アプリケーションでユーザーの意図の保持および特定のアプリの後にリソースを再起動へのアクセスを維持します。

#### <a name="security-scoped-bookmark-types"></a>ブックマークのセキュリティ スコープの種類

Security-Scoped ブックマークと永続的なリソースへのアクセスを使用する場合は、次の 2 つの sistine ユース ケースがあります。

- **App-Scoped ブックマークは、ユーザー指定のファイルまたはフォルダーへの永続的なアクセスを提供します。** 

    たとえば、セキュリティで保護された Xamarin.Mac アプリケーションを使用して外部の文書を開いて編集する場合 (を使用して、 `NSOpenPanel`)、今後、同じファイルにアクセスできるように、アプリは App-Scoped ブックマークを作成できます。
- **Document-Scoped ブックマークは、サブ ファイルへの特定のドキュメントの永続的なアクセスを提供します。** 

たとえば、ビデオ編集アプリ プロジェクト ファイルを作成するには、個々 のイメージ、ビデオ クリップおよびが後で 1 つのムービーに結合されるサウンドのファイルへのアクセスがあります。

ユーザーがプロジェクトにリソース ファイルをインポートする場合 (を使用して、 `NSOpenPanel`)、ファイルには常に、アプリにアクセスできるように、アプリは、プロジェクトに格納されている項目の Document-Scoped ブックマークを作成します。

Document-Scoped ブックマークは、ブックマークのデータと、ドキュメント自体に開くことができる任意のアプリケーションで解決できます。 これは、ユーザーが別のユーザーと同様にそれらの機能すべてのブックマークを持つにプロジェクト ファイルを送信できるように、移植性をサポートします。

> [!IMPORTANT]
> Document-Scoped ブックマークできます_のみ_1 つのファイルとフォルダーではなくをポイントし、そのファイルは、システムによって使用される場所にすることはできません (など`/private`または`/Library`)。

#### <a name="using-security-scoped-bookmarks"></a>セキュリティ スコープのブックマークの使用

Security-Scoped ブックマークのいずれかの型を使用してには、次の手順を実行する必要があります。

1. **Security-Scoped ブックマークを使用する必要がある Xamarin.Mac アプリで、適切な権利を設定する**-For App-Scoped ブックマークの設定、`com.apple.security.files.bookmarks.app-scope`権利キー`true`です。 Document-Scoped ブックマーク、設定、`com.apple.security.files.bookmarks.document-scope`権利キー`true`です。
2. **Security-Scoped ブックマークを作成**-任意のファイルまたはフォルダー、ユーザーがへのアクセスを提供するを行います (を介して`NSOpenPanel`など) に、アプリへの永続的なアクセスが必要です。 使用して、`public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)`のメソッド、`NSUrl`ブックマークを作成するクラス。
3. **解決するには、Security-Scoped ブックマーク**アプリがリソースにアクセスする、もう一度 (たとえば、再起動後) が必要とするときにセキュリティ スコープの URL にブックマークを解決する必要があります。 使用して、`public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)`のメソッド、`NSUrl`ブックマークを解決するのにはクラスです。
4. **Security-Scoped URL からファイルにアクセスするシステムに明示的に通知**- この手順は、上記の Security-Scoped URL を取得した直後に実行する必要がありますか、後で場合に発生した後に、リソースへのアクセスを回復それへのアクセスを開放します。 呼び出す、`StartAccessingSecurityScopedResource ()`のメソッド、 `NSUrl` Security-Scoped URL へのアクセスを開始するクラス。
5. **明示的に通知が完了したら、システム Security-Scoped URL からファイルへのアクセス**-(たとえば、ユーザーが閉じる) 場合に、アプリがファイルへのアクセスに不要になった必要なときに、システムに通知する必要があります、できるだけ早くです。 呼び出す、`StopAccessingSecurityScopedResource ()`のメソッド、 `NSUrl` Security-Scoped URL へのアクセスを停止するクラス。

リソースへのアクセスを放棄した後は、アクセスを再確立するためにもう一度 4 の手順に戻る必要があります。 Xamarin.Mac アプリを再起動する場合は、手順 3 に戻ります、ブックマークを再び解決する必要があります。

> [!IMPORTANT]
> エラー Security-Scoped URL リソースへのアクセスを解放するには、カーネル リソース リークが発生する Xamarin.Mac アプリが発生します。 その結果、アプリができなくなりますが再起動されるまで、そのコンテナーにファイル システムの場所を追加します。

### <a name="the-app-sandbox-and-code-signing"></a>アプリのサンド ボックスとコード署名

アプリ サンド ボックスを有効にして (権限) を介した Xamarin.Mac アプリの特定の要件を有効にした後に有効にするサンド ボックスの署名対象のプロジェクトをコーディングする必要があります。 アプリのサンド ボックス化に必要な権利がアプリの署名にリンクされているために、署名のコードを実行する必要があります。 

macOS、アプリのコンテナーとその他のアプリケーションなし格納されている場合でもアクセスできますアプリ バンドル id。 をスプーフィングは、この方法でそのコード署名の間のリンクを強制します。 このメカニズムはように機能します。

1. システムでは、アプリのコンテナーを作成するときに、設定、_アクセス制御リスト_(ACL) をコンテナーにします。 一覧で初期のアクセス制御エントリを含むアプリの_指定要件_(DR)、どの将来のバージョンのアプリを記述することができます (ときに認識することがアップグレードされました)。
2. 同じバンドル ID を持つ、アプリが起動するたびに、システムでは、アプリのコード署名が 1 つのコンテナーの acl エントリで指定された指定の要件と一致することを確認します。 システムが一致するものを見つけられない場合、アプリはの起動されません。

署名は、次のようにコーディングできます。

1. Xamarin.Mac プロジェクトを作成する前には、Apple 開発者ポータルから開発の証明書、配布の証明書および開発者 ID の証明書を取得します。
2. Mac App Store Xamarin.Mac アプリを配布するときに、Apple のコード署名で署名されています。

デバッグと、テスト時を使用します (どちらを使用するアプリのコンテナーを作成する) にサインインしている Xamarin.Mac アプリケーションのバージョン。 後で、テストまたは Apple App Store からバージョンをインストールする場合は、Apple 署名で署名し、(元のアプリのコンテナーと同じコード シグネチャがない) からの起動に失敗します。 このような状況が表示されます、クラッシュ レポート次のように。

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

この問題を解決するには、Apple の符号付きバージョンのアプリ をポイントする ACL エントリを調整する必要があります。

作成して、サンド ボックス化に必要なプロビジョニング プロファイルのダウンロードの詳細についてを参照してください、[署名とアプリのプロビジョニング](#Signing_and_Provisioning_the_App)前のセクションです。

#### <a name="adjusting-the-acl-entry"></a>ACL エントリを調整します。

Apple 署名バージョン Xamarin.Mac アプリを実行するを許可するのには、次の操作を行います。

1. ターミナル アプリを開きます (で`/Applications/Utilities`)。
2. Apple 署名バージョン Xamarin.Mac アプリの検索ウィンドウを開きます。
3. 型`asctl container acl add -file `ターミナル ウィンドウにします。
4. Finder ウィンドウから Xamarin.Mac アプリのアイコンをドラッグし、ターミナル ウィンドウの上にドロップします。
5. ファイルへの完全パスは、ターミナルのコマンドに追加されます。
6. キーを押して**Enter**コマンドを実行します。

コンテナーの ACL には今すぐ Xamarin.Mac アプリの両方のバージョンの指定されたコードの要件が含まれているし、macOS と、いずれかのバージョンを実行するようになりました。

#### <a name="display-a-list-of-acl-code-requirements"></a>コードの ACL の要件の一覧を表示します。

次の手順を実行して、コンテナーの ACL でコードの要件の一覧を表示できます。

1. ターミナル アプリを開きます (で`/Applications/Utilities`)。
2. 「`asctl container acl list -bundle <container-name>`」と入力します。
3. キーを押して**Enter**コマンドを実行します。

`<container-name>` Xamarin.Mac アプリケーションのバンドル Id は、通常します。

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>アプリのサンド ボックスの Xamarin.Mac アプリの設計

アプリのサンド ボックスの Xamarin.Mac アプリを設計するときに従う必要があります一般的なワークフローがあります。 ただし、アプリ内でサンド ボックス化の実装の詳細については特定のアプリの機能に対して一意であるしようとしています。

### <a name="six-steps-for-adopting-the-app-sandbox"></a>アプリのサンド ボックスを採用する 6 つの手順

通常、アプリのサンド ボックスの Xamarin.Mac アプリを設計は、次の手順で構成されます。

1. アプリがサンド ボックスに適切であるかを決定します。
2. 開発と配布の戦略を設計します。
3. API 互換性問題を解決します。
4. Xamarin.Mac プロジェクトに必要なアプリのサンド ボックス権利を適用します。
5. XPC を使用して特権の分離を追加します。
6. 移行戦略を実装します。

> [!IMPORTANT]
> 必要なだけでなく、メインの実行可能ファイル、アプリ バンドルが含まれる支援者にもサンド ボックス アプリまたはそのバンドル内のツールです。 これは Mac App Store から配信されるすべてのアプリに必要であり、可能な場合、アプリの配布の他の形式を行う必要があります。

Xamarin.Mac アプリのバンドル内のすべての実行可能バイナリの一覧は、ターミナルで、次のコマンドを入力してください。

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

ここで`[Your-App-Bundle]`は、アプリケーションのバンドルへのパスと名前。

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Xamarin.Mac アプリがサンド ボックス化に適しているかどうかを判断します。

ほとんどのアプリで Xamarin.Mac はアプリのサンド ボックスに完全に互換性のあるため、サンド ボックス化に適してします。 アプリには、アプリのサンド ボックスを使用できない動作が必要とする場合は、その他の方法を検討してください。

アプリには、次の現象のいずれかが必要とする場合、アプリのサンド ボックスと互換性がありません。

- **承認サービス**-でこのアプリ サンド ボックスで説明した関数で操作することはできません[承認サービス C リファレンス](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826)です。
- **アクセシビリティ Api** -スクリーン リーダーなどのサンド ボックス補助的なアプリまたはその他のアプリケーションを制御するアプリすることはできません。
- **任意のアプリに Apple イベントを送信**-サンド ボックス化することはできません、アプリでは、不明な任意のアプリに Apple イベントを送信する必要がある場合。 既知の呼び出されたアプリの一覧を表示するには、アプリは、引き続きセキュリティで保護されたと、権利が呼び出されたアプリの一覧を含める必要があります。
- **他のタスクに通知の分散にユーザー情報の辞書を送信**-含めることはできませんと、アプリ サンド ボックス、`userInfo`ディクショナリに投稿すると、`NSDistributedNotificationCenter`メッセージングの他のタスクに対してオブジェクト。
- **カーネルの拡張の読み込み**-カーネル拡張の読み込みは、アプリのサンド ボックスによって禁止されています。
- **オープンと保存のダイアログでユーザー入力をシミュレートする**- プログラムで開く操作または保存をシミュレートするか、ユーザー入力を変更するダイアログ ボックスは、アプリのサンド ボックスで禁止されています。
- **アクセスやその他のアプリの設定**-アプリのサンド ボックスによって禁止されている他のアプリの設定を操作します。
- **ネットワーク設定を構成する**-ネットワーク設定の操作は、アプリのサンド ボックスによって禁止されています。
- **他のアプリを終了して**-このアプリのサンド ボックスの使用が禁止されて`NSRunningApplication`を他のアプリを終了します。

### <a name="resolving-api-incompatibilities"></a>API の非互換性を解決します。

アプリ サンド ボックスの Xamarin.Mac アプリを設計するときに、いくつか macOS Api の使用状況と互換性の問題があります。

次に、いくつかの一般的な問題と解決のために実行できることを示します。

- **開く、保存、およびドキュメントの追跡**- 以外の任意のテクノロジを使用してドキュメントを管理している場合は、`NSDocument`アプリのサンド ボックスの組み込みをサポートするために切り替える必要があります。 `NSDocument` 自動的に PowerBox と連携し、Finder で、ユーザーが移動する場合は、サンド ボックス内のドキュメントを保つためのサポートを提供します。
- **ファイル システムのリソースへのアクセスを保持する**- アプリは、そのコンテナー外のリソースへ永続的なアクセス権によって異なります。 Xamarin.Mac ブックマークのセキュリティ スコープを使用してアクセスを維持する場合。
- **アプリのログイン項目の作成**-でこのアプリ サンド ボックス、項目を使用してログインを作成することはできません`LSSharedFileList`起動を使用してサービスの状態を操作することができますも`LSRegisterURL`します。 使用して、`SMLoginItemSetEnabled`リンゴの説明どおりに動作[サービス管理フレームワークを使用してログインの項目を追加する](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1)ドキュメント。
- **ユーザー データにアクセスする**- などの POSIX 関数を使用している場合は、`getpwuid`をディレクトリ サービスからユーザーのホーム ディレクトリを取得するには、Cocoa または基盤のシンボルをなど使用を検討して`NSHomeDirectory`です。
- **その他のアプリの環境設定にアクセスする**- アプリのサンド ボックスは、そのコンテナー内の環境設定は場所を変更して、別のアプリ設定で許可されていませんへのアクセスにパス検索 Api アプリのコンテナーを導くためです。 
- **Web ビューでの HTML5 埋め込まれたビデオの使用**-AV Foundation フレームワークに対してアプリをリンクする Xamarin.Mac アプリでは、埋め込みの HTML5 ビデオを再生する WebKit を使用している場合もあります。 アプリ サンド ボックスにより、CoreMedia play からこれらのビデオはそれ以外の場合。

### <a name="applying-required-app-sandbox-entitlements"></a>必要なアプリのサンド ボックスの権利を適用します。

アプリのサンド ボックス内で実行を確認する Xamarin.Mac アプリケーションの権利を編集する必要があります、**を有効にするアプリのサンド ボックス**チェック ボックスをオンします。

アプリの機能に基づいた、OS の機能やリソースにアクセスするには、その他の権利を有効にする必要があります。 アプリのサンド ボックス化は、アプリを実行するために必要な最低限を要求する権利を最小化するときに最適なためだけにランダムに有効にしないで権利。

Xamarin.Mac アプリが必要とする権利を確認するのには、次の操作を行います。

1. アプリのサンド ボックスを有効にし、Xamarin.Mac アプリを実行します。
2. アプリの機能を実行します。
3. コンソール アプリを開きます (で利用可能な`/Applications/Utilities`) を探します`sandboxd`違反、**すべてのメッセージ**ログ。
4. 各`sandboxd`違反、他のファイル システムの場所ではなく、アプリ コンテナーを使用するか、問題を解決または制限付きの OS の機能へのアクセスを有効にするアプリのサンド ボックス権利を適用します。
5. 再実行し、もう一度 Xamarin.Mac アプリのすべての機能をテストします。
6. すべてまで繰り返し`sandboxd`違反が解決されました。

### <a name="add-privilege-separation-using-xpc"></a>XPC を使用して特権の分離を追加します。

アプリ サンド ボックスの Xamarin.Mac アプリを開発する場合は、権限とアクセスの観点で、アプリの動作を確認し、危険度の高い操作を独自の XPC サービスに分離することを検討してください。

詳細については、Apple を参照してください。[作成 XPC Services](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6)と[デーモンと Services プログラミング ガイド](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i)です。

### <a name="implement-a-migration-strategy"></a>移行戦略を実装します。

使用されていたサンド ボックス化された Xamarin.Mac アプリケーションのセキュリティで保護された、新しいバージョンをリリースする場合は、現在のユーザーは、スムーズなアップグレード パスであることを確認する必要があります。 

 コンテナー移行マニフェストを実装する方法について詳しくは、「Apple の[サンド ボックス アプリを移行する](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1)ドキュメント。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションのサンド ボックス化についての詳細を取得しました。 最初に、単に Xamarin.Mac アプリにアプリ サンド ボックスの基本の表示を作成します。 次に、サンド ボックスの違反を解決する方法を紹介します。 次に、作成しました、詳細なアプリのサンド ボックス拝見し、アプリのサンド ボックスの Xamarin.Mac アプリの設計で最後に、について説明しました。



## <a name="related-links"></a>関連リンク

- [App Store への発行](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [アプリのサンド ボックスについて](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [アプリのサンド ボックスの一般的な問題](https://developer.apple.com/library/content/qa/qa1773/_index.html)
