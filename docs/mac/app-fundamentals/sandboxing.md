---
title: Xamarin.Mac アプリのサンド ボックス化
description: この記事では、App Store でのリリース用の Xamarin.Mac アプリケーションのサンド ボックス化について説明します。 すべてのコンテナー ディレクトリ、権利、ユーザー指定のアクセス許可、特権の分離、およびカーネルの実施など、サンド ボックスに要素を説明します。
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: c51960a24e1277b3faec0905da3b9a5986359681
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830670"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Xamarin.Mac アプリのサンド ボックス化

_この記事では、App Store でのリリース用の Xamarin.Mac アプリケーションのサンド ボックス化について説明します。すべてのコンテナー ディレクトリ、権利、ユーザー指定のアクセス許可、特権の分離、およびカーネルの実施など、サンド ボックスに要素を説明します。_

## <a name="overview"></a>概要

Xamarin.Mac アプリケーションで c# と .NET を使用する場合、OBJECTIVE-C または Swift で作業する際にサンド ボックス アプリケーションに同じ機能があります。

[![実行中のアプリの例](sandboxing-images/intro01.png "実行中のアプリの例")](sandboxing-images/intro01-large.png#lightbox)

この記事では、Xamarin.Mac アプリケーションと、サンド ボックスに移動する要素のすべてのサンド ボックス化操作の基本を説明します。 コンテナー ディレクトリ、権利、ユーザー指定のアクセス許可、特権の分離、およびカーネルの強制実行します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

確認することも、 [C# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での C# クラスを Objective-C オブジェクトと UI への要素に使用されます。

## <a name="about-the-app-sandbox"></a>アプリのサンド ボックスについて

アプリのサンド ボックスは、アプリケーションによるシステム リソースへのアクセスを制限することで、Mac 上で実行されている悪意のあるアプリケーションによって生じるおそれのある破損に対する強力な防御を提供します。

非サンド ボックス化されたアプリケーションは現在、アプリを実行しているユーザーの完全な権限を持つとアクセス、またはユーザーができる何も実行できます。 セキュリティ ホール (またはそれを使用している任意のフレームワーク) が、アプリが含まれる場合、ハッカーできます可能性があるこれらの弱点し、アプリで実行されている Mac の管理を使用しています。

アプリケーションごとにリソースへのアクセスを制限すると、盗難、破損または悪意があるユーザーのコンピューターで実行されているアプリケーション側に対する防御のセキュリティで保護されたアプリを提供します。

アプリのサンド ボックスでは、(カーネル レベルで適用されます) という 2 つの戦略を提供する macOS に組み込まれているアクセス制御テクノロジを示します。

1. アプリのサンド ボックスにより、記述する開発者_方法_アプリケーションは、OS と、このように操作は、完了したら、ジョブを取得するために必要とされないのは、アクセス権のみを付与は。
2. アプリのサンド ボックスは、シームレスに、オープンでシステムにさらにアクセス権を付与し、保存 ダイアログ ボックスにドラッグ アンド ドロップ操作と、一般的なその他のユーザーとのやり取りにできます。

### <a name="preparing-to-implement-the-app-sandbox"></a>アプリのサンド ボックスを実装する準備

この記事で詳しく説明するアプリのサンド ボックスの要素は次のとおりです。

- コンテナー ディレクトリ
- 権利
- ユーザー指定のアクセス許可
- 特権の分離
- カーネルの強制

これらの詳細を理解したら、Xamarin.Mac アプリケーションで、アプリのサンド ボックスの導入のための計画を作成することができます。

最初に、アプリケーションが (ほとんどのアプリケーションは) サンド ボックス化の候補であるかを判断する必要があります。 次に、API の非互換性を解決し、必要なアプリのサンド ボックスの要素を決定する必要があります。 最後に、アプリケーションの防レベルを最大限に特権の分離を使用してになります。

App Sandbox を採用する場合でも、アプリケーションが使用するいくつかのファイル システムの場所は別になります。 特に、アプリケーションでは、アプリケーションのサポート ファイル、データベース、キャッシュ、およびユーザー ドキュメントではないその他のファイルに使用されるコンテナー ディレクトリがあります。 MacOS と Xcode の両方の従来の場所からファイルのこれらの種類をコンテナーに移行するサポートを提供します。

## <a name="sandboxing-quick-start"></a>サンド ボックス化のクイック スタート

このセクションで、アプリのサンド ボックスの概要の例として (具体的には要求されない限り、サンド ボックスで制限されているネットワーク接続が必要) を Web ビューを使用した単純な Xamarin.Mac アプリ作成します。

私たちは、アプリケーションが実際にサンド ボックス ソリューションと、一般的なアプリのサンド ボックス エラーを解決する方法について説明することを確認します。

### <a name="creating-the-xamarinmac-project"></a>Xamarin.Mac プロジェクトを作成します。

それでは、サンプル プロジェクトを作成するには、次の操作を行います。

1. Visual Studio をクリックし、Mac を起動、**新しいソリューション.** リンクをクリックしてください。
2. **新しいプロジェクト**ダイアログ ボックスで、 **Mac** > **アプリ** > **Cocoa アプリ**: 

    [![新しい Cocoa アプリを作成する](sandboxing-images/sample01.png "新しい Cocoa アプリの作成")](sandboxing-images/sample01-large.png#lightbox)
3. をクリックして、**次**ボタンに、入力`MacSandbox`プロジェクト名をクリックして、**作成**ボタン。 

    [![アプリ名を入力する](sandboxing-images/sample02.png "アプリ名を入力します。")](sandboxing-images/sample02-large.png#lightbox)
4. **Solution Pad**、ダブルクリックして、 **Main.storyboard** Xcode で開くファイルを。 

    [![メインのストーリー ボードを編集](sandboxing-images/sample03.png "メインのストーリー ボードの編集")](sandboxing-images/sample03-large.png#lightbox)
5. ドラッグ、 **Web ビュー**をウィンドウにサイズがコンテンツ領域をいっぱいに拡大および縮小ウィンドウを使用するように設定します。 

    [![Web ビューを追加する](sandboxing-images/sample04.png "web ビューの追加")](sandboxing-images/sample04-large.png#lightbox)
6. という名前の web ビューのアウトレットを作成する`webView`: 

    [![新しいアウトレットを作成する](sandboxing-images/sample05.png "新しいアウトレットを作成します。")](sandboxing-images/sample05-large.png#lightbox)
7. Mac およびダブルクリック用 Visual Studio に戻り、 **ViewController.cs**ファイル、 **Solution Pad**編集用に開きます。
8. 次の追加ステートメントを使用します。 `using WebKit;`
9. ように、`ViewDidLoad`メソッドの次のようになります。 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. 変更内容を保存します。

アプリケーションを実行し、Apple web サイトが、ウィンドウで次のように表示されることを確認します。

[![アプリの例の実行を示す](sandboxing-images/sample06.png "例のアプリの実行を表示")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>署名と、アプリのプロビジョニング

アプリ サンド ボックスを有効にしましたが、前に最初をプロビジョニングし、Xamarin.Mac アプリケーションを署名する必要があります。

次のこと。

1. Apple Developer Portal にログインします。 

    [![Apple Developer ポータルにログインする](sandboxing-images/sign01.png "Apple Developer ポータルにログインします。")](sandboxing-images/sign01-large.png#lightbox)
2. 選択**証明書, Identifiers & Profiles**: 

    [![証明書、ID、プロファイルの選択](sandboxing-images/sign02.png "証明書、ID、プロファイルの選択")](sandboxing-images/sign02-large.png#lightbox)
3. **Mac アプリ**、**識別子**: 

    [![識別子を選択する](sandboxing-images/sign03.png "識別子を選択します。")](sandboxing-images/sign03-large.png#lightbox)
4. アプリケーションの新しい ID を作成します。 

    [![新しいアプリ ID を作成する](sandboxing-images/sign04.png "新しいアプリ ID を作成します。")](sandboxing-images/sign04-large.png#lightbox)
5. **Provisioning Profiles**、**開発**: 

    [![開発を選択する](sandboxing-images/sign05.png "開発を選択します。")](sandboxing-images/sign05-large.png#lightbox)
6. 新しいプロファイルを作成し、選択**Mac アプリの開発**: 

    [![新しいプロファイルを作成する](sandboxing-images/sign06.png "新しいプロファイルを作成します。")](sandboxing-images/sign06-large.png#lightbox)
7. 上記で作成したアプリ ID を選択します。 

    [![アプリ ID の選択](sandboxing-images/sign07.png "アプリ ID の選択")](sandboxing-images/sign07-large.png#lightbox)
8. このプロファイルの開発者を選択します。 

    [![追加開発者](sandboxing-images/sign08.png "追加開発者")](sandboxing-images/sign08-large.png#lightbox)
9. このプロファイルのコンピューターを選択します。 

    [![許可されているコンピューターの選択](sandboxing-images/sign09.png "許可されているコンピューターの選択")](sandboxing-images/sign09-large.png#lightbox)
10. プロファイルに名前を付けます。 

    [![プロファイルに名前を付けて](sandboxing-images/sign10.png "プロファイルに名前を付ける")](sandboxing-images/sign10-large.png#lightbox)
11. をクリックして、**完了**ボタンをクリックします。

> [!IMPORTANT]
> 一部のインスタンスでは、Apple の開発者ポータルから直接新しいプロビジョニング プロファイルをダウンロードし、ダブルクリックしてインストールする必要があります。 停止し、新しいプロファイルにアクセスが可能となる前に、Visual Studio を Mac を再起動する必要もあります。

次に、開発用コンピューターで、新しいアプリ ID とプロファイルを読み込む必要があります。 それでは、次の操作を行います。

1. Xcode を起動し、選択**設定**から、 **Xcode**メニュー。 

    ![Xcode にアカウントを編集](sandboxing-images/sign11.png "Xcode にアカウントの編集")
2. をクリックして、**の詳細を表示しています.** ボタンをクリックします。 

    ![詳細の表示 ボタンをクリックして](sandboxing-images/sign12.png "詳細の表示 をクリックします。")
3. をクリックして、**更新**(左下隅) にボタンをクリックします。
4. をクリックして、**完了**ボタンをクリックします。

次に、Xamarin.Mac プロジェクトに新しいアプリ ID とプロビジョニング プロファイルを選択する必要があります。 それでは、次の操作を行います。

1. **Solution Pad**、ダブルクリックして、 **Info.plist**ファイルを開き、編集します。
2. いることを確認、**バンドル識別子**上で作成した、アプリ ID と一致する (例: `com.appracatappra.MacSandbox`)。 

    [![バンドル Id を編集](sandboxing-images/sign13.png "バンドル Id の編集")](sandboxing-images/sign13-large.png#lightbox)
3. 次をダブルクリック、 **Entitlements.plist**ファイルを確認します、 **iCloud のキー値ストア**と**iCloud コンテナー**上で作成したアプリ ID は、すべて一致 (例。`com.appracatappra.MacSandbox`): 

    [![Entitlements.plist ファイルを編集](sandboxing-images/sign17.png "Entitlements.plist ファイルを編集")](sandboxing-images/sign17-large.png#lightbox)
3. 変更内容を保存します。
4. **Solution Pad**、編集するためのオプションを開くプロジェクト ファイルをダブルクリックします。  

    ![Editign ソリューションのオプション](sandboxing-images/sign14.png "Editign ソリューションのオプション")
5. 選択**Mac 署名**、し確認**アプリケーション バンドルに署名**と**インストーラー パッケージに署名**します。 **プロビジョニング プロファイル**、上記で作成した 1 つを選択します。 

    ![プロビジョニング プロファイルを設定して](sandboxing-images/sign15.png "プロビジョニング プロファイルの設定")
6. をクリックして、**完了**ボタンをクリックします。

> [!IMPORTANT]
> 終了し、新しいアプリ ID と Xcode によってインストールされたプロビジョニング プロファイルを認識する場合は、Visual Studio for Mac を再起動する必要があります。

#### <a name="troubleshooting-provisioning-issues"></a>プロビジョニングの問題のトラブルシューティング

この時点でアプリケーションを実行し、すべて署名され、正常にプロビジョニングされていることを確認してください。 実行する場合、アプリも以前と同様、すべて良好です。 障害が発生した場合、次のようなダイアログ ボックスが表示する可能性があります。

[![問題のダイアログをプロビジョニング例](sandboxing-images/sign16.png "問題ダイアログをプロビジョニングする例")](sandboxing-images/sign16-large.png#lightbox)

プロビジョニングと署名の問題の最も一般的な原因を次に示します。

- アプリ バンドル ID は、選択したプロファイルのアプリ ID と一致しません。
- Developer ID は、選択したプロファイルの Developer ID と一致しません。
- 選択したプロファイルの一部として登録されていないでテストされている Mac の UUID。

問題の場合、Apple 開発者ポータルの問題を修正、Xcode でプロファイルを更新および for mac。 Visual Studio でのクリーン ビルドを行う

### <a name="enable-the-app-sandbox"></a>アプリのサンド ボックスを有効にします。

プロジェクト オプションのチェック ボックスを選択して、アプリのサンド ボックスが有効にします。 次の手順で行います。

1. **Solution Pad**、ダブルクリックして、 **Entitlements.plist**ファイルを開き、編集します。
2. 両方をチェック**権利を有効にする**と**アプリのサンド ボックス化を有効にする**: 

    [![権利を編集し、サンド ボックス化を有効にする](sandboxing-images/sign17.png "権利を編集し、サンド ボックス化を有効にします。")](sandboxing-images/sign17-large.png#lightbox)
3. 変更内容を保存します。

この時点では、アプリのサンド ボックスが有効になっているが、Web ビューの必要なネットワーク アクセスが指定されていません。 今すぐアプリケーションを実行すると、空のウィンドウが表示されます。

[![ブロックされている web アクセスを示す](sandboxing-images/sample08.png "がブロックされている web アクセスを示す")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>アプリのサンド ボックス化された可能性の確認

別に動作をブロックしているリソースは、Xamarin.Mac アプリケーションが正常にサンド ボックス化されているかどうかに通知する 3 つの主な方法があります。

1. Finder での内容を確認して、`~/Library/Containers/`フォルダー - アプリが、サンド ボックス化する場合があります、アプリのバンドル識別子などという名前のフォルダー (例: `com.appracatappra.MacSandbox`)。 

    [![アプリのバンドルを開く](sandboxing-images/sample09.png "アプリのバンドルを開く")](sandboxing-images/sample09-large.png#lightbox)
2. システムでは、利用状況モニターでサンド ボックス化として、アプリが表示されます。
    - 利用状況モニターを起動 ( `/Applications/Utilities`)。 
    - 選択**ビュー** > **列**いることを確認し、**サンド ボックス**メニュー項目をチェックします。
    - サンド ボックスの列を読み取っていることを確認します。`Yes`アプリケーション。 

    [![アプリの利用状況モニターをチェック](sandboxing-images/sample10.png "アプリの利用状況モニターを確認します。")](sandboxing-images/sample10-large.png#lightbox)
3. バイナリのアプリはサンド ボックス化されたことを確認します。
    - ターミナル アプリを起動します。
    - アプリケーションに移動します`bin`ディレクトリ。
    - このコマンドを発行: `codesign -dvvv --entitlements :- executable_path` (場所`executable_path`アプリケーションへのパスです)。 

    [![コマンドラインでアプリをチェック](sandboxing-images/sample11.png "コマンド ラインで、アプリの確認")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>サンド ボックス化されたアプリのデバッグ

デバッガーは、既定ではサンド ボックスは、有効にするとそれができないことをアプリに接続するには、TCP 経由の Xamarin.Mac アプリに接続するため、エラーが発生した場合は有効になっている適切なアクセス許可がないアプリを実行しようとすると、 *"に接続できませんデバッガー"* します。 

[![必要なオプションを設定する](sandboxing-images/debug01.png "に必要なオプションの設定")](sandboxing-images/debug01-large.png#lightbox)

**発信ネットワーク接続を許可する (クライアント)** 権限は、デバッガーに必要な 1 つは、この 1 つ許可すると、通常のデバッグします。 更新したなしでデバッグすることはできませんので、`CompileEntitlements`対象の`msbuild`デバッグのセキュリティで保護されているすべてのアプリのビルドのみの場合は、権利をそのアクセス許可を自動的に追加します。 リリース ビルドでは、未変更の状態、権利ファイルで指定された資格を使用する必要があります。

### <a name="resolving-an-app-sandbox-violation"></a>アプリのサンド ボックス、違反を解決します。

アプリのサンド ボックス違反では、アプリケーションがあるリソースにアクセスしようとしました。 セキュリティで保護された Xamarin.Mac が明示的に許可されていない場合に発生します。 など、Web ビューが、Apple web サイトを表示できません。

アプリのサンド ボックス違反の最も一般的なソースは、Visual studio for Mac に指定された権利の設定、アプリケーションの要件が一致しない場合に発生します。 作業から、Web ビューを保持する権利をこの例では、不足しているネットワーク接続をもう一度、バックアップします。

#### <a name="discovering-app-sandbox-violations"></a>アプリのサンド ボックスの違反を検出します。

使用して、問題を検出する最も簡単な方法は、Xamarin.Mac アプリケーションで、アプリのサンド ボックス違反が発生している疑いがある場合、**コンソール**アプリ。

次の手順で行います。

1. 対象のアプリをコンパイルし、for mac。 Visual Studio から実行
2. 開く、**コンソール**アプリケーション (から`/Applications/Utilties/`)。
3. 選択**すべてのメッセージ**サイドバーの入力と`sandbox`検索。 

    [![コンソールで、サンド ボックス上の問題の例](sandboxing-images/resolve01.png "コンソールで、サンド ボックス上の問題の例")](sandboxing-images/resolve01-large.png#lightbox)

上記の例アプリを参照してください、Kernal によってブロックされている、`network-outbound`アプリ サンド ボックスのためその権限が要求のためのトラフィック。

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>アプリのサンド ボックスと権利の違反を修正

これでアプリのサンド ボックス化の違反を見つける、アプリケーションの権利を調整することで解決する方法を見て方法を学習しました。

次の手順で行います。

1. **Solution Pad**、ダブルクリックして、 **Entitlements.plist**ファイルを開き、編集します。
2. **[権利]** セクションで、チェック、**発信ネットワーク接続を許可する (クライアント)** チェック ボックスをオンします。 

    [![権利の編集](sandboxing-images/sign17.png "権利の編集")](sandboxing-images/sign17-large.png#lightbox)
3. アプリケーションに変更を保存します。

場合は、ビルドして実行しました、アプリの例の前述の処理、web コンテンツは期待どおりに表示されます。

## <a name="the-app-sandbox-in-depth"></a>詳細なアプリのサンド ボックス

アプリのサンド ボックスによって提供されるアクセス制御メカニズムは、いくつかと簡単に理解できます。 ただし、各アプリによって、アプリのサンド ボックスを採用する方法は、一意であり、アプリの要件に基づいてです。

(またはライブラリまたはフレームワークのいずれかを使用)、悪意のあるコードによって悪用されるを Xamarin.Mac アプリケーションを保護する最善の努力を指定するには、必要がある脆弱性は 1 つのみ、どちらかのアプリケーションでのアプリの対話を制御しますシステム。

アプリのサンド ボックスは、システムとアプリケーションの目的の相互作用を指定することによって (または損害の可能性があります)、引き継ぎを防ぐために設計されました。 システムには、ジョブを実行するアプリケーションに必要なリソースと何も詳細へのアクセスのみが付与されます。

最悪のシナリオを設計するアプリのサンド ボックスを設計する場合。 アプリケーションは、悪意のあるコードによって侵害は場合、は、ファイルとアプリのサンド ボックス内のリソースへのアクセスに制限されます。

### <a name="entitlements-and-system-resource-access"></a>権利とシステム リソースへのアクセス

上記のとおり、サンド ボックス化されていない、Xamarin.Mac アプリケーションは、アプリを実行しているユーザーのアクセスと完全な権限が付与されます。 悪意のあるコードによって侵害された場合、被害を与えることの潜在的なまでの範囲全体で、保護されていないアプリが悪意のある動作は、用のエージェントとして動作できます。

アプリのサンド ボックスを有効にすると、最低限の特権がすべて削除し、Xamarin.Mac アプリの権利を使用する必要がある専用ごとに再度有効にします。 

編集することによって、アプリケーションのアプリのサンド ボックス リソースを変更するその**Entitlements.plist**ファイルとチェックまたはエディターのドロップダウン ボックスから必要な権限を選択します。

[![権利の編集](sandboxing-images/sign17.png "権利の編集")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>コンテナーのディレクトリとファイル システムへのアクセス

Xamarin.Mac アプリケーションでは、アプリのサンド ボックスが採用されます、次の場所にアクセスがあります。

- **アプリのコンテナー ディレクトリ**-最初の実行時に、OS が特殊なを作成します_コンテナー ディレクトリ_移動してそのすべてのリソースのみにアクセスできることができます。 アプリでは、このディレクトリへの完全な読み取り/書き込みアクセスがあります。
- **アプリ グループのコンテナー ディレクトリ**-アプリを 1 つ以上のアクセスを許可できる_グループ コンテナー_同じグループ内のアプリ間で共有されています。
- **ファイルをユーザーが指定した**-アプリケーションが自動的には明示的に開きまたはドラッグ、ユーザーがアプリケーションにドロップしたファイルへのアクセスを取得します。
- **関連項目**-と適切な権利をアプリケーションにアクセスできる名前が同じで別の拡張子を持つファイル。 たとえば、両方と保存されているドキュメントを`.txt`ファイルと`.pdf`します。
- **一時ディレクトリ、コマンド ライン ツールのディレクトリ、および特定の世界が判読できる場所**-アプリでは、さまざまなレベルのアクセスをシステムで指定したとおり適切に定義された他の場所のファイルにします。

#### <a name="the-app-container-directory"></a>アプリのコンテナー ディレクトリ

Xamarin.Mac アプリケーションのアプリのコンテナー ディレクトリには、次の特徴があります。

- ユーザーのホーム ディレクトリ内の非表示の場所が (通常`~Library/Containers`) でアクセスできると、`NSHomeDirectory`アプリケーションの機能 (下記参照)。 ホーム ディレクトリになっている各ユーザーはアプリの独自のコンテナーを取得します。
- アプリは、コンテナーのディレクトリとすべてのサブディレクトリとその中のファイルに読み取り/書き込みアクセスを無制限にします。
- MacOS のパス検索 Api のほとんどは、アプリのコンテナーを基準としてです。 たとえば、コンテナーが、独自**ライブラリ**(を使用してアクセス`NSLibraryDirectory`)、**アプリケーション サポート**と**の基本設定**サブディレクトリ。
- macOS が確立し、間の接続を適用し、アプリとコード署名を使用してコンテナー。 使用して、アプリを偽装する別のアプリが試行がでもその**バンドル識別子**、コード署名のため、コンテナーにアクセスすることはできません。
- ユーザーが生成したファイルのコンテナーではありません。 代わりに、アプリケーションがデータベース、キャッシュまたはその他の特定のデータ型などで使用されるファイルです。
- _Shoebox_ (Apple のフォト アプリ) のようなアプリの種類、ユーザーのコンテンツは、コンテナーに移動されます。

> [!IMPORTANT]
> 残念ながら、Xamarin.Mac が 100% の API カバレッジまだ (とは異なり Xamarin.iOS)、その結果、 `NSHomeDirectory` Xamarin.Mac の現在のバージョンの API がマップされていません。

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

アプリケーションが使用されている Mac macOS 10.7.5 以降 (と大きい)、`com.apple.security.application-groups`グループ内のすべてのアプリケーションに共通する共有コンテナーにアクセスする権利。 この共有のコンテナーは、データベースや他の種類 (キャッシュ) などのサポート ファイルのなどの非ユーザーに公開されたコンテンツを使用できます。

グループ コンテナーが自動的に追加する各アプリのサンド ボックス コンテナー (グループの一部である) 場合にも格納は`~/Library/Group Containers/<application-group-id>`します。 グループ ID_する必要があります_など、開発チーム ID と、ピリオドで始まります。

```plist
<team-id>.com.company.<group-name>
```

詳細については、Apple を参照してください[アプリケーション グループにアプリケーションを追加する](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)で[権利キー参照](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195)します。

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>アプリ コンテナーの外部で Powerbox とファイル システムのアクセス

サンド ボックス Xamarin.Mac アプリケーションでは、次の方法でそのコンテナーの外部でファイル システムの場所をアクセスできます。

- で開くとファイルの保存またはドラッグ アンド ドロップなど他の方法) (ユーザーの特定の方向。
- 特定のファイル システムの場所の権利を使用して (よう`/bin`または`/usr/lib`)。
* ファイル システムの場所が読める (共有) など、特定のディレクトリの場合です。

_Powerbox_ macOS をサンド ボックスの Xamarin.Mac アプリのファイルのアクセス権を展開するユーザーと対話するセキュリティ テクノロジです。 Powerbox API ではありませんが、アプリを呼び出すときに透過的に起動される、`NSOpenPanel`または`NSSavePanel`します。 Powerbox アクセスは、Xamarin.Mac アプリケーションを設定する権利を使用して有効になっています。

サンド ボックス化されたアプリは、表示、開くまたは保存ダイアログ、ウィンドウは Powerbox (しない AppKit) が表示され、任意のファイルまたはディレクトリをユーザーへのアクセスを持つへのアクセスが備わっています。

ユーザー開くからファイルまたはディレクトリを選択または保存ダイアログ (またはドラッグするか、アプリのアイコンの上に)、Powerbox はアプリのサンド ボックスに関連付けられているパスを追加します。

さらに、システムは自動的にセキュリティで保護されたアプリに次のコードを許可します。

- システムへの接続は、メソッドを入力します。
- ユーザーが選択したサービスを呼び出す、**サービス**メニュー (としてマークされているサービスに対してのみ_サンド ボックス アプリケーションの安全な_サービス プロバイダーによって)。
- ユーザーから開いているファイルを選択して、**開いて最近**メニュー。
- その他のアプリケーション間でコピーと貼り付けを使用します。
- 次の世界が判読できる場所からファイルを読み取り。
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- によって作成されたディレクトリにファイルを読み書き`NSTemporaryDirectory`します。

既定をするには、ファイルを開いて、Xamarin.Mac アプリをセキュリティで保護されたによって保存されるかに引き続きアクセスできる (ない場合、アプリを終了すると、ファイルが開かれたまま)、アプリが終了するまでです。 開いているファイル、次回アプリを起動するときに自動的に macOS 再開機能を使用して、アプリのサンド ボックスに復元されます。

Xamarin.Mac アプリのコンテナーの外部にあるファイルへの永続化を提供するには、セキュリティ スコープのブックマーク (下記参照) を使用します。

#### <a name="related-items"></a>関連項目

App Sandbox アプリに許可するを各種の拡張機能が、同じファイル名を持つ関連のアイテムにアクセスします。 機能がある 2 つの部分: a)、アプリの関連する拡張機能の一覧`Info.plst`ファイル、b) コードへのアプリがある何これらのファイルをサンド ボックスに通知します。

これは適切な 2 つのシナリオがあります。

1. アプリは、(新しい拡張機能) を持つファイルの異なるバージョンを保存することができる必要があります。 たとえば、エクスポート、`.txt`ファイルを`.pdf`ファイル。 このような状況を処理するために使用する必要があります、`NSFileCoordinator`ファイルにアクセスします。 呼び出し、`WillMove(fromURL, toURL)`メソッドは、最初に、新しい拡張機能にファイルを移動し、呼び出して`ItemMoved(fromURL, toURL)`します。
2. アプリは、各種の拡張機能と拡張機能の 1 つのメイン ファイルとサポートのいくつかのファイルを開く必要があります。 たとえば、映画、字幕ファイル。 使用して、`NSFilePresenter`セカンダリ ファイルにアクセスします。 メイン ファイルを提供、`PrimaryPresentedItemURL`プロパティとセカンダリ ファイルを`PresentedItemURL`プロパティ。 メイン ファイルが開かれたときに呼び出す、`AddFilePresenter`のメソッド、`NSFileCoordinator`セカンダリ ファイルを登録するクラス。

両方のシナリオで、アプリの**Info.plist**ファイルは、アプリを開くことができるドキュメントの種類を宣言する必要があります。 ファイルの種類、追加、 `NSIsRelatedItemType` (の値を持つ`YES`) のエントリに、`CFBundleDocumentTypes`配列。

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>開き、サンド ボックス化されたアプリを使用して、ダイアログの動作を保存

次の制限は、`NSOpenPanel`と`NSSavePanel`サンド ボックス化された Xamarin.Mac アプリから呼び出すとき。

- プログラムで呼び出すことはできません、 **OK**ボタンをクリックします。
- プログラムでのユーザーの選択を変更することはできません、`NSOpenSavePanelDelegate`します。

さらに、次の継承の変更は、場所には。

- **非サンド ボックス化されたアプリ** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>ブックマークのセキュリティ スコープと永続的なリソースへのアクセス

前述のように、サンド ボックス化された Xamarin.Mac アプリケーションは、ファイルまたは (PowerBox によって提供される) との直接ユーザーの操作を使用して、コンテナーの外部にあるリソースにアクセスできます。 ただし、これらのリソースへのアクセスは自動的に保持されませんアプリの起動またはシステム全体で再起動します。

使用して_Security-Scoped ブックマーク_、サンド ボックス化された Xamarin.Mac アプリケーションはユーザーの意図を保持して、特定のアプリの後にリソースの再起動に引き続きアクセスします。

#### <a name="security-scoped-bookmark-types"></a>ブックマークのセキュリティ スコープの種類

Security-Scoped ブックマークと永続的なリソースへのアクセスを使用する場合は、2 つ sistine ユース ケースがあります。

- **App-Scoped ブックマークは、ユーザー指定のファイルまたはフォルダーへの永続的なアクセスを提供します。** 

    たとえば、サンド ボックス化された Xamarin.Mac アプリケーションを使用して外部の文書を開いて編集する場合 (を使用して、 `NSOpenPanel`)、アプリは、今後、同じファイルにアクセスできるように App-Scoped のブックマークを作成できます。
- **Document-Scoped ブックマークは、サブ ファイルへの特定のドキュメントの永続的なアクセスを提供します。** 

たとえば、ビデオ編集アプリ プロジェクト ファイルを作成するには、個々 のイメージ、ビデオ クリップおよび後で 1 つのムービーに統合するサウンド ファイルへのアクセスがあります。

ユーザーがプロジェクトにリソース ファイルをインポートする場合 (を使用して、 `NSOpenPanel`)、ファイルには常に、アプリにアクセスできるように、アプリは、プロジェクトに格納されている項目の Document-Scoped ブックマークを作成します。

Document-Scoped ブックマークは、ブックマークのデータと、ドキュメント自体に開くことができる任意のアプリケーションで解決できます。 これには、移植性、ユーザーが別のユーザーと同様にそれらの機能のすべてのブックマークのことに、プロジェクト ファイルを送信できるようにサポートしています。

> [!IMPORTANT]
> Document-Scoped ブックマークできます_のみ_1 つのファイルとフォルダーではなくをポイントし、そのファイルは、システムによって使用される場所にすることはできません (など`/private`または`/Library`)。

#### <a name="using-security-scoped-bookmarks"></a>セキュリティ スコープのブックマークを使用します。

Security-Scoped ブックマークのいずれかの型を使用して、次の手順を実行することが必要です。

1. **Security-Scoped ブックマークを使用する必要がある Xamarin.Mac アプリで適切な権利を設定する**-For App-Scoped のブックマークの設定、`com.apple.security.files.bookmarks.app-scope`権利キー`true`します。 Document-Scoped ブックマーク、設定、`com.apple.security.files.bookmarks.document-scope`権利キー`true`します。
2. **Security-Scoped ブックマークを作成**-これを行う任意ファイルまたはフォルダーへのアクセスをユーザーが提供する (を使用して`NSOpenPanel`など)、アプリに永続的なアクセスには必要があります。 使用して、`public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)`のメソッド、`NSUrl`ブックマークを作成するクラス。
3. **Security-Scoped ブックマークを解決する**- アプリが (たとえば、再起動) 後にもう一度リソースにアクセスする必要がある場合、セキュリティ スコープの URL にブックマークを解決する必要があります。 使用して、`public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)`のメソッド、`NSUrl`ブックマークを解決するのにはクラスです。
4. **Security-Scoped URL からファイルにアクセスするシステムに明示的に通知**- この手順は、上記の Security-Scoped URL の取得後すぐに実行する必要が、場合や後で構成した後、リソースへのアクセスを回復する.それへのアクセスを開放します。 呼び出す、`StartAccessingSecurityScopedResource ()`のメソッド、 `NSUrl` Security-Scoped URL へのアクセスを開始するクラス。
5. **明示的に行われるシステムに通知 Security-Scoped URL からファイルへのアクセス**-アプリが不要になった (たとえば、ユーザーが閉じる) 場合にファイルへのアクセスが必要とシステムに伝達する必要があります、できるだけ早くします。 呼び出す、`StopAccessingSecurityScopedResource ()`のメソッド、 `NSUrl` Security-Scoped URL へのアクセスを停止するクラス。

リソースへのアクセスを解放した後は、アクセスを再確立するためにもう一度 4 の手順に戻る必要があります。 Xamarin.Mac アプリが再起動された場合は、3 の手順に戻るし、ブックマークの再解決する必要があります。

> [!IMPORTANT]
> Security-Scoped URL リソースへのアクセスのリリースに失敗には、Xamarin.Mac アプリにカーネルのリソースのリークが発生します。 その結果、アプリができなく再開されるまで、そのコンテナーにファイル システムの場所を追加します。

### <a name="the-app-sandbox-and-code-signing"></a>アプリのサンド ボックスとコード署名

アプリのサンド ボックスを有効にするし、(権利) を使用して Xamarin.Mac アプリの特定の要件を有効にすると、サインオン プロジェクトを有効にするサンド ボックスのコードを作成する必要があります。 アプリのサンド ボックス化に必要な権利が、アプリの署名にリンクされているため、署名のコードを実行する必要があります。 

macOS は、アプリのコンテナーとその他のアプリケーションなしに格納されている場合でもアクセスできますアプリ バンドル id。 をスプーフィングは、この方法でそのコード署名の間のリンクを強制します。 このメカニズムはように動作します。

1. システムでは、アプリのコンテナーを作成するときに、設定、_アクセス制御リスト_(ACL) でそのコンテナー。 初期のアクセス制御エントリ一覧でには、アプリにはが含まれています_指定要件_(DR)、アプリのどの将来のバージョンを表していることができます (ときに認識するアップグレードされました)。
2. 同じバンドル ID を使用してアプリを起動するたびに、システムは、アプリのコード署名に 1 つのコンテナーの ACL のエントリで指定された指定の要件と一致することを確認します。 システムが一致するものを見つけられない場合は、アプリを起動できなくなります。

コードの署名は、次の方法。

1. Xamarin.Mac プロジェクトを作成する前に、Apple 開発者ポータルから、開発証明書、配布証明書、および Developer ID の証明書を取得します。
2. Mac App Store では、Xamarin.Mac アプリを配布するときに、Apple のコード署名で署名します。

テストとデバッグ、版 (どちらを使用するアプリのコンテナーを作成する) にサインインして、Xamarin.Mac アプリケーションを使用します。 後で、テストまたは Apple App Store からバージョンをインストールする場合、Apple 署名で署名されます、(元のアプリのコンテナーと同じコード シグネチャがあるない) からの起動に失敗します。 このような状況で表示されます、クラッシュ レポート、次のような。

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

これを解決するには、アプリの Apple の署名されたバージョンをポイントする ACL エントリを調整する必要があります。

作成して、サンド ボックス化に必要なプロビジョニング プロファイルのダウンロードの詳細についてを参照してください、[署名と、アプリのプロビジョニング](#Signing_and_Provisioning_the_App)前のセクション。

#### <a name="adjusting-the-acl-entry"></a>ACL エントリを調整します。

Xamarin.Mac アプリを実行するの Apple の署名されたバージョンを許可するのには、次の操作を行います。

1. ターミナル アプリを開きます (で`/Applications/Utilities`)。
2. Xamarin.Mac アプリの Apple の署名されたバージョンを Finder ウィンドウを開きます。
3. 型`asctl container acl add -file`ターミナル ウィンドウでします。
4. Xamarin.Mac アプリのアイコンをこの Finder ウィンドウからドラッグし、ターミナル ウィンドウにドロップします。
5. ファイルへの完全パスは、ターミナル、コマンドに追加されます。
6. キーを押して**Enter**コマンドを実行します。

コンテナーの ACL が含まれています、Xamarin.Mac アプリの両方のバージョンの指定されたコードの要件にはと、macOS と、いずれかのバージョンを実行するようになりました。

#### <a name="display-a-list-of-acl-code-requirements"></a>ACL のコードの要件の一覧を表示します。

次の手順に従って、コンテナーの ACL でコードの要件の一覧を表示できます。

1. ターミナル アプリを開きます (で`/Applications/Utilities`)。
2. 「`asctl container acl list -bundle <container-name>`」と入力します。
3. キーを押して**Enter**コマンドを実行します。

`<container-name>` Xamarin.Mac アプリケーションのバンドル識別子は、通常します。

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>アプリのサンド ボックスの Xamarin.Mac アプリの設計

App Sandbox の Xamarin.Mac アプリを設計する際に従う必要がありますが、一般的なワークフローがあります。 ただし、アプリでのサンド ボックス化の実装の詳細は特定のアプリの機能に固有になります。

### <a name="six-steps-for-adopting-the-app-sandbox"></a>アプリのサンド ボックスの導入のための 6 つの手順

通常、アプリのサンド ボックスの Xamarin.Mac アプリの設計は、次の手順で構成されます。

1. アプリがサンド ボックスに適切であるかを決定します。
2. 開発と配布の戦略を設計します。
3. API の非互換性を解決します。
4. Xamarin.Mac プロジェクトに必要なアプリのサンド ボックス権利を適用します。
5. XPC を使用して特権の分離を追加します。
6. 移行戦略を実装します。

> [!IMPORTANT]
> メインの実行可能ファイルをアプリ バンドルがすべて含まれるヘルパーもサンド ボックスだけでなくを作成する必要がありますアプリまたはバンドル内のツール。 これは、Mac App Store から配信されるすべてのアプリに必要であり、可能な場合、その他の形式のアプリの配布を行う必要があります。

Xamarin.Mac アプリのバンドルにすべての実行可能バイナリの一覧は、のターミナルで次のコマンドを入力します。

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

場所`[Your-App-Bundle]`は、アプリケーションのバンドルへのパスと名前。

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Xamarin.Mac アプリはサンド ボックス化に適しているかどうかを判断します。

ほとんどの Xamarin.Mac アプリはアプリのサンド ボックスと完全な互換性と、そのため、サンド ボックス化に適してします。 アプリには、アプリのサンド ボックスを許可しない動作が必要とする場合は、別のアプローチを検討してください。

アプリには、次の動作のいずれかが必要とする場合は、アプリのサンド ボックスと互換性がありません。

- **承認サービス**-the App Sandbox とで説明した関数を操作することはできません[承認サービス C リファレンス](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826)します。
- **アクセシビリティ Api** -スクリーン リーダーなどの補助的なアプリのサンド ボックスまたはアプリの他のアプリケーションを制御することはできません。
- **任意のアプリに Apple イベントを送信**-サンド ボックス化することはできませんが、アプリでは、不明な任意のアプリに Apple イベントを送信する必要がある場合。 既知の呼び出されたアプリの一覧を表示するには、アプリはサンド ボックス化でき、権利は呼び出されたアプリの一覧を含める必要があります。
- **通知の配布で他のタスクにユーザー情報のディクショナリを送信する**-含めることはできません、App Sandbox で、`userInfo`ディクショナリに投稿すると、`NSDistributedNotificationCenter`の他のタスクのメッセージング オブジェクト。
- **カーネルの拡張機能を読み込む**-カーネルの拡張機能の読み込みは、アプリのサンド ボックスで禁止されています。
- **オープンと保存のダイアログ ボックスでユーザー入力をシミュレートする**- プログラムで開く操作または保存ダイアログをシミュレートするか、ユーザー入力の変更は、アプリのサンド ボックスで禁止されています。
- **アクセスまたはその他のアプリの基本設定を設定**-アプリのサンド ボックスで他のアプリの設定を操作することは禁止されています。
- **ネットワーク設定を構成する**-アプリのサンド ボックスでネットワーク設定を操作することは禁止されています。
- **他のアプリを終了して**-アプリ サンド ボックスの使用が禁止されて`NSRunningApplication`を他のアプリを終了します。

### <a name="resolving-api-incompatibilities"></a>API の非互換性を解決します。

App Sandbox の Xamarin.Mac アプリを設計するときにいくつかの macOS の Api の使用状況の非互換性が発生する可能性があります。

次に、いくつかの一般的な問題とそれらを解決するためには実行できることを示します。

- **開く、保存、およびドキュメントの追跡**以外の任意のテクノロジを使用してドキュメントを管理している場合 -`NSDocument`アプリのサンド ボックスの組み込みサポートのために切り替える必要があります。 `NSDocument` 自動的に PowerBox と連携し、Finder で、ユーザーが移動する場合に、サンド ボックス内のドキュメントを保持するサポートを提供します。
- **ファイル システム リソースへのアクセスを保持**- 場合は、Xamarin.Mac アプリが依存する、コンテナー外のリソースへの永続的なアクセスは、アクセスを維持するセキュリティ スコープのブックマークを使用します。
- **アプリのログインの項目を作成**-the App Sandbox に項目を使用してログインを作成することはできません`LSSharedFileList`起動を使用してサービスの状態を操作することができますも`LSRegisterURL`します。 使用して、`SMLoginItemSetEnabled`りんごで説明されているとおりに機能[サービス管理フレームワークを使用してログイン項目の追加](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1)ドキュメント。
- **ユーザー データにアクセスする**などの POSIX 関数を使用している場合 -`getpwuid`をディレクトリ サービスからユーザーのホーム ディレクトリを取得するには、Cocoa または Core Foundation のシンボルをなど使用を検討して`NSHomeDirectory`します。
- **その他のアプリの設定にアクセスする**- アプリのサンド ボックスは、そのコンテナー内の実施の基本設定を変更して、別のアプリ設定で許可されていませんへのアクセスにパス検索 Api アプリのコンテナーを指示するためです。 
- **Web ビューでの HTML5 埋め込みのビデオを使用して**-AV Foundation フレームワークに対してアプリをリンクする、Xamarin.Mac アプリは、埋め込みの HTML5 ビデオを再生する WebKit を使用している場合もする必要があります。 アプリのサンド ボックスにより、CoreMedia play からそれ以外の場合、これらのビデオ。

### <a name="applying-required-app-sandbox-entitlements"></a>必要なアプリのサンド ボックスの権利を適用します。

アプリのサンド ボックスで実行をチェックするすべての Xamarin.Mac アプリケーションの権利を編集する必要があります、**を有効にするアプリのサンド ボックス**チェック ボックスをオンします。

アプリの機能に基づいて、OS の機能やリソースにアクセスするには、他の権利を有効にする必要があります。 そのため、アプリのサンド ボックス化の動作が要求するアプリを実行するために必要な最低限の権限を最小限に抑える場合に最適では、権利は可能ランダムにだけです。

Xamarin.Mac アプリが必要とする権利を確認するのには、次の操作を行います。

1. アプリのサンド ボックスを有効にし、Xamarin.Mac アプリを実行します。
2. アプリの機能を実行します。
3. コンソール アプリを開きます (で利用可能な`/Applications/Utilities`) を探して`sandboxd`違反、**すべてのメッセージ**ログ。
4. 各`sandboxd`違反を他のファイル システムの場所ではなく、アプリ コンテナーを使用するか、問題を解決するまたは制限付きの OS 機能へのアクセスを有効にするアプリのサンド ボックス権利を適用します。
5. 再実行し、すべての Xamarin.Mac アプリの機能をもう一度テストします。
6. すべてまで繰り返し`sandboxd`違反が解決されました。

### <a name="add-privilege-separation-using-xpc"></a>XPC を使用して特権の分離を追加します。

App Sandbox の Xamarin.Mac アプリを開発するときに特権とアクセスの条件で、アプリの動作を確認し、危険度の高い操作を独自の XPC サービスに分離することを検討してください。

詳細については、Apple を参照してください。 [XPC サービスの作成](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6)と[デーモンとサービスのプログラミング ガイド](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i)します。

### <a name="implement-a-migration-strategy"></a>移行戦略を実装します。

以前にセキュリティで保護されたなかった、Xamarin.Mac アプリケーションのサンド ボックス化された、新しいバージョンをリリースする場合は、現在のユーザーは、スムーズなアップグレード パスであることを確認する必要があります。 

 コンテナーの移行のマニフェストを実装する方法について詳しくは、「Apple の[サンド ボックス アプリを移行する](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1)ドキュメント。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションのサンド ボックス化についての詳細を取得しました。 最初に、アプリのサンド ボックスの基本事項を表示する単純に Xamarin.Mac アプリを作成します。 次に、サンド ボックスの違反を解決する方法を紹介しました。 詳細しましたし、アプリのサンド ボックスを確認し、最後に、アプリのサンド ボックスの Xamarin.Mac アプリを設計しました。



## <a name="related-links"></a>関連リンク

- [App Store への発行](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [アプリのサンド ボックスについて](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [アプリのサンド ボックス化の一般的な問題](https://developer.apple.com/library/content/qa/qa1773/_index.html)
