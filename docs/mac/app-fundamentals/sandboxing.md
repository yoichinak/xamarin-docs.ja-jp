---
title: Xamarin. Mac アプリのサンドボックス化
description: この記事では、App Store でリリースするための Xamarin. Mac アプリケーションのサンドボックスについて説明します。 コンテナーディレクトリ、権利、ユーザーが決定したアクセス許可、特権の分離、カーネル強制など、サンドボックスに入るすべての要素について説明します。
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 4558a9bd19810f8759010861d8a2e4b8cab09c56
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770300"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Xamarin. Mac アプリのサンドボックス化

_この記事では、App Store でリリースするための Xamarin. Mac アプリケーションのサンドボックスについて説明します。コンテナーディレクトリ、権利、ユーザーが決定したアクセス許可、特権の分離、カーネル強制など、サンドボックスに入るすべての要素について説明します。_

## <a name="overview"></a>概要

Xamarin. Mac C#アプリケーションでおよび .net を使用する場合は、目的の C または Swift を使用する場合と同じように、アプリケーションをサンドボックス化することができます。

[![実行中のアプリの例](sandboxing-images/intro01.png "実行中のアプリの例")](sandboxing-images/intro01-large.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでのサンドボックスの使用の基本について説明します。また、サンドボックスに入るすべての要素 (コンテナーディレクトリ、権利、ユーザーが決定したアクセス許可、特権の分離、およびカーネル強制) についても説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

確認することも、 [C# を公開するクラス/Objective-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性ネットワーク上での C# クラスを Objective-C オブジェクトと UI への要素に使用されます。

## <a name="about-the-app-sandbox"></a>アプリのサンドボックスについて

アプリサンドボックスは、アプリケーションがシステムリソースに対して持つアクセスを制限することで、Mac で悪意のあるアプリケーションが実行されていることが原因で発生する可能性がある破損に対する強力な防御を提供します。

サンドボックス化されていないアプリケーションは、アプリを実行しているユーザーの完全な権限を持ち、ユーザーが実行できるすべての操作にアクセスしたり実行したりできます。 アプリにセキュリティホール (またはそれが使用している任意のフレームワーク) が含まれている場合、ハッカーはそれらの弱点を悪用し、アプリを使用して、実行されている Mac を制御できます。

アプリケーションごとにリソースへのアクセスを制限することで、サンドボックス化されたアプリは、ユーザーのコンピューターで実行されるアプリケーションの一部に対して、盗難、破損、または悪意のある目的に対する防御策を提供します。

アプリのサンドボックスは、macOS に組み込まれたアクセス制御テクノロジであり、2つの方法があります。

1. アプリサンドボックスを使用すると、開発者は、アプリケーションが OS と_どのよう_に対話するかを記述できます。また、この方法では、ジョブを完了するために必要なアクセス権のみが付与されます。
2. アプリサンドボックスを使用すると、[開く] および [保存] ダイアログボックス、ドラッグアンドドロップ操作、およびその他の一般的なユーザー操作によって、システムへのアクセスをシームレスに付与することができます。

### <a name="preparing-to-implement-the-app-sandbox"></a>アプリのサンドボックスを実装する準備をしています

この記事で詳しく説明されているアプリサンドボックスの要素は次のとおりです。

- コンテナーディレクトリ
- 権利
- ユーザーによって決定されたアクセス許可
- 特権の分離
- カーネル強制

これらの詳細を理解したら、Xamarin. Mac アプリケーションでアプリサンドボックスを採用するためのプランを作成できます。

まず、アプリケーションがサンドボックスに適しているかどうかを判断する必要があります (ほとんどのアプリケーションは)。 次に、API の非互換性を解決し、必要となるアプリのサンドボックスの要素を決定する必要があります。 最後に、特権の分離を使用して、アプリケーションの防御レベルを最大化します。

アプリのサンドボックスを使用すると、アプリケーションで使用されるファイルシステムの場所が異なります。 特に、アプリケーションには、アプリケーションサポートファイル、データベース、キャッシュ、およびユーザードキュメントではないその他のファイルに使用されるコンテナーディレクトリがあります。 MacOS と Xcode はどちらも、これらの種類のファイルを従来の場所からコンテナーに移行するためのサポートを提供します。

## <a name="sandboxing-quick-start"></a>サンドボックスのクイックスタート

このセクションでは、アプリサンドボックスの使用を開始する例として、Web ビューを使用する単純な Xamarin. Mac アプリを作成します (これには、特に要求がない限り、サンドボックスで制限されたネットワーク接続が必要です)。

アプリケーションが実際にサンドボックス化されていることを確認し、一般的なアプリサンドボックスエラーをトラブルシューティングして解決する方法を学習します。

### <a name="creating-the-xamarinmac-project"></a>Xamarin. Mac プロジェクトを作成する

サンプルプロジェクトを作成するには、次の手順を実行します。

1. Visual Studio for Mac を開始し、**新しいソリューション**をクリックします。 リンクをクリックしてください。
2. **[新しいプロジェクト]** ダイアログボックスで、[ **Mac** > **アプリ** > **cocoa アプリ**] を選択します。

    [![新しい Cocoa アプリを作成する](sandboxing-images/sample01.png "新しい Cocoa アプリを作成する")](sandboxing-images/sample01-large.png#lightbox)
3. **[次へ]** ボタンを`MacSandbox`クリックし、プロジェクト名として「」と入力して、 **[作成]** ボタンをクリックします。

    [![アプリ名の入力](sandboxing-images/sample02.png "アプリ名の入力")](sandboxing-images/sample02-large.png#lightbox)
4. **Solution Pad**で、**メインの storyboard**ファイルをダブルクリックして、Xcode で編集するために開きます。

    [![メインストーリーボードの編集](sandboxing-images/sample03.png "メインストーリーボードの編集")](sandboxing-images/sample03-large.png#lightbox)
5. **Web ビュー**をウィンドウにドラッグし、コンテンツ領域に合わせてサイズを変更し、ウィンドウで拡大および縮小するように設定します。

    [![Web ビューの追加](sandboxing-images/sample04.png "Web ビューの追加")](sandboxing-images/sample04-large.png#lightbox)
6. Web ビュー `webView`のアウトレットを次のように作成します。

    [![新しいアウトレットの作成](sandboxing-images/sample05.png "新しいアウトレットの作成")](sandboxing-images/sample05-large.png#lightbox)
7. Visual Studio for Mac に戻り、 **Solution Pad**で**ViewController.cs**ファイルをダブルクリックして編集用に開きます。
8. 次の using ステートメントを追加します。`using WebKit;`
9. メソッドを`ViewDidLoad`次のようにします。

    ```csharp
    public override void AwakeFromNib ()
    {
        base.AwakeFromNib ();
        webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
    }
    ```

10. 変更内容を保存します。

アプリケーションを実行し、次のように、Apple Web サイトがウィンドウに表示されていることを確認します。

[![アプリの実行例を表示する](sandboxing-images/sample06.png "アプリの実行例を表示する")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>アプリの署名とプロビジョニング

アプリのサンドボックスを有効にする前に、まず、Xamarin. Mac アプリケーションをプロビジョニングして署名する必要があります。

次の手順を実行します。

1. Apple Developer Portal にログインします。

    [![Apple Developer ポータルへのログイン](sandboxing-images/sign01.png "Apple Developer ポータルへのログイン")](sandboxing-images/sign01-large.png#lightbox)
2. **証明書、識別子 & プロファイル**を選択します。

    [![証明書、ID、プロファイルの選択](sandboxing-images/sign02.png "証明書、ID、プロファイルの選択")](sandboxing-images/sign02-large.png#lightbox)
3. **[Mac アプリ]** で、 **[識別子]** を選択します。

    [![識別子の選択](sandboxing-images/sign03.png "識別子の選択")](sandboxing-images/sign03-large.png#lightbox)
4. アプリケーションの新しい ID を作成します。

    [![新しいアプリ ID を作成し]ています(sandboxing-images/sign04.png "新しいアプリ ID を作成し")ています](sandboxing-images/sign04-large.png#lightbox)
5. **[プロビジョニングプロファイル]** で、 **[開発]** を選択します。

    [![開発の選択](sandboxing-images/sign05.png "開発の選択")](sandboxing-images/sign05-large.png#lightbox)
6. 新しいプロファイルを作成し、 **[Mac アプリ開発]** を選択します。

    [![新しいプロファイルを作成し]ています(sandboxing-images/sign06.png "新しいプロファイルを作成し")ています](sandboxing-images/sign06-large.png#lightbox)
7. 上で作成したアプリ ID を選択します。

    [![アプリ ID の選択](sandboxing-images/sign07.png "アプリ ID の選択")](sandboxing-images/sign07-large.png#lightbox)
8. このプロファイルの開発者を選択してください:

    [![開発者の追加](sandboxing-images/sign08.png "開発者の追加")](sandboxing-images/sign08-large.png#lightbox)
9. このプロファイルのコンピューターを選択してください:

    [![許可するコンピューターの選択](sandboxing-images/sign09.png "許可するコンピューターの選択")](sandboxing-images/sign09-large.png#lightbox)
10. プロファイルに名前を付けます。

    [![プロファイルに名前を付ける](sandboxing-images/sign10.png "プロファイルに名前を付ける")](sandboxing-images/sign10-large.png#lightbox)
11. **[完了]** ボタンをクリックします。

> [!IMPORTANT]
> 場合によっては、Apple の開発者ポータルから新しいプロビジョニングプロファイルを直接ダウンロードし、それをダブルクリックしてインストールすることが必要になる場合があります。 新しいプロファイルにアクセスするには、Visual Studio for Mac を停止して再起動することが必要になる場合もあります。

次に、開発用コンピューターに新しいアプリ ID とプロファイルを読み込む必要があります。 次の手順を実行してみましょう。

1. Xcode を起動し、 **Xcode**メニューから **[基本設定]** を選択します。

    ![Xcode のアカウントの編集](sandboxing-images/sign11.png "Xcode のアカウントの編集")
2. [詳細の**表示...** ] ボタンをクリックします。

    [![詳細の表示] ボタンをクリックする][(sandboxing-images/sign12.png "詳細の表示] ボタンをクリックする")
3. **[更新]** ボタン (左下隅) をクリックします。
4. **[完了]** ボタンをクリックします。

次に、Xamarin. Mac プロジェクトで新しいアプリ ID とプロビジョニングプロファイルを選択する必要があります。 次の手順を実行してみましょう。

1. **Solution Pad**で **、ファイルを**ダブルクリックして編集用に開きます。
2. **バンドル識別子**が、上で作成したアプリ ID と一致して`com.appracatappra.MacSandbox`いることを確認します (例:)。

    [![バンドル識別子の編集](sandboxing-images/sign13.png "バンドル識別子の編集")](sandboxing-images/sign13-large.png#lightbox)
3. 次に、**資格の plist**ファイルをダブルクリックし、Icloud のキーと**値のストア**と**icloud コンテナー**が、上記で作成したアプリ ID と一致`com.appracatappra.MacSandbox`していることを確認します (例:)。

    [![権利の plist ファイルの編集](sandboxing-images/sign17.png "権利の plist ファイルの編集")](sandboxing-images/sign17-large.png#lightbox)
4. 変更内容を保存します。
5. **Solution Pad**で、プロジェクトファイルをダブルクリックして、編集用のオプションを開きます。

    ![ソリューションのオプションの Editign](sandboxing-images/sign14.png "ソリューションのオプションの Editign")
6. **[Mac 署名]** を選択し、 **[アプリケーションバンドルの署名]** をオンにして **、インストーラーパッケージに署名**します。 **[プロビジョニングプロファイル]** で、上記で作成したものを選択します。

    ![プロビジョニングプロファイルを設定しています](sandboxing-images/sign15.png "プロビジョニングプロファイルを設定しています")
7. **[完了]** ボタンをクリックします。

> [!IMPORTANT]
> Xcode によってインストールされた新しいアプリ ID とプロビジョニングプロファイルを認識できるように、Visual Studio for Mac を終了して再起動することが必要になる場合があります。

#### <a name="troubleshooting-provisioning-issues"></a>プロビジョニングに関する問題のトラブルシューティング

この時点で、アプリケーションを実行し、すべてが正しく署名され、プロビジョニングされていることを確認する必要があります。 アプリが以前と同じように実行されている場合は、すべて問題ありません。 エラーが発生した場合、次のようなダイアログボックスが表示されることがあります。

[![プロビジョニングの問題の例ダイアログ](sandboxing-images/sign16.png "プロビジョニングの問題の例ダイアログ")](sandboxing-images/sign16-large.png#lightbox)

次に、プロビジョニングと署名に関する問題の最も一般的な原因を示します。

- アプリバンドル ID が、選択したプロファイルのアプリ ID と一致しません。
- 開発者 ID が、選択したプロファイルの開発者 ID と一致しません。
- テスト対象の Mac の UUID が、選択したプロファイルの一部として登録されていません。

問題が発生した場合は、Apple Developer Portal で問題を修正し、Xcode でプロファイルを更新して、Visual Studio for Mac でクリーンビルドを実行します。

### <a name="enable-the-app-sandbox"></a>アプリのサンドボックスを有効にする

アプリのサンドボックスを有効にするには、プロジェクトのオプションでチェックボックスをオンにします。 次の手順で行います。

1. **Solution Pad**で、**権利の plist**ファイルをダブルクリックして編集用に開きます。
2. [**権利を有効に**し、**アプリのサンドボックスを有効に**する] をオンにします。

    [![権利の編集とサンドボックスの有効化](sandboxing-images/sign17.png "権利の編集とサンドボックスの有効化")](sandboxing-images/sign17-large.png#lightbox)
3. 変更内容を保存します。

この時点では、アプリサンドボックスは有効になっていますが、Web ビューに必要なネットワークアクセスが提供されていません。 ここでアプリケーションを実行すると、空のウィンドウが表示されます。

[![ブロックされている web アクセスを表示しています](sandboxing-images/sample08.png "ブロックされている web アクセスを表示しています")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>アプリがサンドボックス化されていることを確認しています

リソースブロックの動作とは別に、Xamarin アプリケーションが正常にサンドボックス化されているかどうかを確認するには、主に次の3つの方法があります。

1. Finder で`~/Library/Containers/`フォルダーの内容を確認します。アプリがサンドボックス化されている場合は、アプリのバンドル識別子のような名前の`com.appracatappra.MacSandbox`フォルダーがあります (例:)。

    [![アプリのバンドルを開く](sandboxing-images/sample09.png "アプリのバンドルを開く")](sandboxing-images/sample09-large.png#lightbox)
2. システムにより、アプリはアクティビティモニターでサンドボックス化されていると認識されます。
    - 利用状況モニター ( `/Applications/Utilities`) を起動します。
    - [**列**の**表示** > ] を選択し、 **[サンドボックス]** メニュー項目がオンになっていることを確認します。
    - アプリケーションのサンドボックス列が`Yes`次のように読み取られていることを確認します。

    [![利用状況モニターのアプリを確認し]ています(sandboxing-images/sample10.png "利用状況モニターのアプリを確認し")ています](sandboxing-images/sample10-large.png#lightbox)
3. アプリのバイナリがサンドボックス化されていることを確認します。
    - ターミナルアプリを起動します。
    - アプリケーション`bin`のディレクトリに移動します。
    - 次のコマンドを`codesign -dvvv --entitlements :- executable_path`実行し`executable_path`ます (はアプリケーションへのパスです)。

    [![コマンドラインでのアプリの確認](sandboxing-images/sample11.png "コマンドラインでのアプリの確認")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>サンドボックスアプリのデバッグ

デバッガーは、TCP を通じて Xamarin. Mac アプリに接続します。つまり、サンドボックスを有効にすると、既定ではアプリに接続できないため、適切なアクセス許可を有効にせずにアプリを実行しようとすると、 *"デバッガーに接続できません" というエラーが表示されます。* .

[![必須オプションの設定](sandboxing-images/debug01.png "必須オプションの設定")](sandboxing-images/debug01-large.png#lightbox)

**[発信ネットワーク接続 (クライアント) を許可する]** アクセス許可は、デバッガーに必要なアクセス許可です。これを有効にすると、デバッグが正常に実行できるようになります。 デバッグを行わずにデバッグすることはできない`CompileEntitlements`ため、 `msbuild`のターゲットを更新して、デバッグビルド用にサンドボックスされているすべてのアプリの権利にそのアクセス許可を自動的に追加するようにしました。 リリースビルドでは、権利ファイルで指定されている権利を変更せずに使用する必要があります。

### <a name="resolving-an-app-sandbox-violation"></a>アプリサンドボックス違反の解決

サンドボックス化された Xamarin. Mac アプリケーションが明示的に許可されていないリソースにアクセスしようとした場合、アプリサンドボックス違反が発生します。 たとえば、Web ビューは Apple の Web サイトを表示できなくなりました。

アプリサンドボックス違反の最も一般的な原因は、Visual Studio for Mac で指定された権利設定がアプリケーションの要件と一致しない場合です。 この例に戻ると、Web ビューの動作を維持するネットワーク接続の権利が不足しています。

#### <a name="discovering-app-sandbox-violations"></a>アプリサンドボックス違反の検出

Xamarin. Mac アプリケーションでアプリサンドボックス違反が発生していると思われる場合は、**コンソール**アプリを使用して問題を最も簡単に見つけることができます。

次の手順で行います。

1. 問題のアプリをコンパイルし、Visual Studio for Mac から実行します。
2. (から `/Applications/Utilties/`) コンソールアプリケーションを開きます。
3. サイドバーで **[すべてのメッセージ]** を`sandbox`選択し、検索に「」と入力します。

    [![コンソールでのサンドボックスの問題の例](sandboxing-images/resolve01.png "コンソールでのサンドボックスの問題の例")](sandboxing-images/resolve01-large.png#lightbox)

上記のアプリの例では、アプリのサンドボックスが原因で`network-outbound` 、カーネルがトラフィックをブロックしていることを確認できます。これは、その権限が要求されていないためです。

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>権利を使用したアプリサンドボックス違反の修正

アプリのサンドボックス違反を見つける方法を説明したので、次に、アプリケーションの権利を調整して、それらを解決する方法を見てみましょう。

次の手順で行います。

1. **Solution Pad**で、**権利の plist**ファイルをダブルクリックして編集用に開きます。
2. **[権利]** セクションで、 **[発信ネットワーク接続 (クライアント) を許可する]** チェックボックスをオンにします。

    [![権利の編集](sandboxing-images/sign17.png "権利の編集")](sandboxing-images/sign17-large.png#lightbox)
3. アプリケーションに変更を保存します。

上記の例のアプリをビルドして実行すると、web コンテンツが想定どおりに表示されるようになります。

## <a name="the-app-sandbox-in-depth"></a>アプリサンドボックスの詳細

アプリのサンドボックスによって提供されるアクセス制御メカニズムは、ごく簡単に理解できます。 ただし、各アプリによってアプリのサンドボックスが適用される方法は、アプリの要件に基づいて異なります。

Xamarin. Mac アプリケーションを悪意のあるコードによって悪用されないように保護するための最善の方法として、アプリ (またはそれが使用するライブラリまたはフレームワーク) には1つの脆弱性のみが必要です。system.

アプリのサンドボックスは、アプリケーションのシステムとの対話を指定できるようにすることで、引き継ぎ (または発生する可能性のある損害の軽減) を防止するように設計されています。 システムは、アプリケーションがジョブを完了するために必要なリソースへのアクセスのみを許可し、それ以外は何も行いません。

アプリのサンドボックスの設計時には、最悪のシナリオに対応する設計になっています。 悪意のあるコードによってアプリケーションが侵害されると、アプリのサンドボックス内のファイルとリソースのみにアクセスできるようになります。

### <a name="entitlements-and-system-resource-access"></a>権利とシステムリソースへのアクセス

既に説明したように、サンドボックス化されていない Xamarin アプリケーションには、アプリを実行しているユーザーの完全な権限とアクセス権が付与されています。 悪意のあるコードによって侵害された場合、保護されていないアプリは悪意のある動作のためにエージェントとして機能する可能性があります。これは、危害を及ぼす可能性があります。

アプリのサンドボックスを有効にすることで、最小限の特権セットを削除するだけで、Xamarin. Mac アプリの権利を使用して、必要に応じて再度有効にすることができます。

アプリケーションのアプリサンドボックスのリソースを変更するには、その権利の**plist**ファイルを編集し、エディターのドロップダウンボックスで必要な権限を確認して選択します。

[![権利の編集](sandboxing-images/sign17.png "権利の編集")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>コンテナーディレクトリとファイルシステムアクセス

Xamarin アプリケーションでアプリサンドボックスを採用すると、次の場所にアクセスできます。

- **アプリコンテナーディレクトリ**-最初の実行時に、OS は、すべてのリソースが移動する特別な_コンテナーディレクトリ_を作成します。 アプリには、このディレクトリに対する完全な読み取り/書き込みアクセス権が付与されます。
- **アプリグループコンテナーのディレクトリ**-アプリには、同じグループ内のアプリ間で共有される1つ以上の_グループコンテナー_へのアクセスを許可できます。
- **ユーザー指定のファイル**-アプリケーションは、ユーザーによって明示的に開かれた、またはアプリケーションにドラッグアンドドロップされたファイルへのアクセスを自動的に取得します。
- **関連項目**-適切な権利があれば、アプリケーションは同じ名前で拡張子が異なるファイルにアクセスできます。 たとえば、 `.txt`ファイル`.pdf`ととして保存されたドキュメントなどです。
- **一時ディレクトリ、コマンドラインツールのディレクトリ、および世界で読み取り可能な特定の場所**: アプリには、システムによって指定された他の適切に定義された場所にあるファイルへのさまざまなアクセスレベルがあります。

#### <a name="the-app-container-directory"></a>アプリコンテナーディレクトリ

Xamarin アプリケーションのアプリコンテナーディレクトリには、次の特性があります。

- ユーザーのホームディレクトリ内の非表示の場所 (通常`~Library/Containers`は) にあり、アプリケーション内の`NSHomeDirectory`関数 (下記参照) を使用してアクセスできます。 このファイルはホームディレクトリにあるため、各ユーザーはアプリ用の独自のコンテナーを取得します。
- このアプリは、コンテナーディレクトリとそのサブディレクトリ内のすべてのサブディレクトリとファイルに対して、無制限の読み取り/書き込みアクセス権を持っています。
- MacOS のパス検索 Api のほとんどは、アプリのコンテナーを基準としています。 たとえば、コンテナーには、(経由で `NSLibraryDirectory`アクセスされる) 独自のライブラリと、アプリケーションの**サポート**と**基本設定**のサブディレクトリがあります。
- macOS は、コード署名を使用して、アプリとそのコンテナーの間の接続を確立し、適用します。 別のアプリが**バンドル識別子**を使用してアプリのスプーフィングを試行する場合でも、コード署名のためにコンテナーにアクセスすることはできません。
- コンテナーは、ユーザーが生成したファイル用ではありません。 代わりに、データベース、キャッシュ、その他の特定の種類のデータなど、アプリケーションで使用されるファイルを対象としています。
- _Shoebox_の種類のアプリ (Apple の写真アプリなど) では、ユーザーのコンテンツはコンテナーに入ります。

> [!IMPORTANT]
> 残念ながら、xamarin. mac にはまだ 100% の api カバレッジがありません (xamarin とは異なります`NSHomeDirectory` )。その結果、api は現在のバージョンの xamarin. mac でマップされていません。

一時的な回避策として、次のコードを使用できます。

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>アプリグループコンテナーディレクトリ

Mac macOS 10.7.5 (以降) 以降では、アプリケーションは`com.apple.security.application-groups`権利を使用して、グループ内のすべてのアプリケーションに共通の共有コンテナーにアクセスできます。 この共有コンテナーは、データベースやその他の種類のサポートファイル (キャッシュなど) など、ユーザーによって接続されていないコンテンツに使用できます。

グループコンテナーは、各アプリのサンドボックスコンテナー (グループの一部である場合) に自動的に追加され、 `~/Library/Group Containers/<application-group-id>`に格納されます。 グループ ID は、開発チーム ID とピリオドで始まる_必要があり_ます。次に例を示します。

```plist
<team-id>.com.company.<group-name>
```

詳細については、「[権利キーのリファレンス](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195)」の「アプリケーション[グループへのアプリケーションの追加](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)」を参照してください。

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>アプリコンテナーの外部にある powerbox とファイルシステムアクセス

サンドボックス化された Xamarin. Mac アプリケーションは、次の方法でコンテナーの外部のファイルシステムの場所にアクセスできます。

- ユーザーの特定の方向 ([開く] および [保存] ダイアログを使用するか、ドラッグアンドドロップなどの他の方法で)。
- 特定のファイルシステムの場所 ( `/bin`や`/usr/lib`など) に対して権利を使用する。
- ファイルシステムの場所が、世界中で読み取り可能な特定のディレクトリにある場合 (共有など)。

_Powerbox_は、ユーザーと対話して、サンドボックス化された Xamarin. Mac アプリのファイルアクセス権を拡張する macOS セキュリティテクノロジです。 Powerbox には API がありませんが、アプリがまたは`NSOpenPanel` `NSSavePanel`を呼び出すときに透過的にアクティブ化されます。 Powerbox へのアクセスは、Xamarin. Mac アプリケーションに設定した権利によって有効になります。

サンドボックス化されたアプリが開いたり保存のダイアログを表示したりすると、このウィンドウは Powerbox (および AppKit ではなく) によって表示されるため、ユーザーがアクセスできる任意のファイルまたはディレクトリにアクセスできます。

ユーザーが [開く] または [保存] ダイアログからファイルまたはディレクトリを選択すると (または、アプリのアイコンにドラッグして)、関連付けられているパスがアプリのサンドボックスに追加されます。

さらに、システムは、サンドボックス化されたアプリに対して次のことを自動的に許可します。

- システム入力メソッドへの接続。
- **[サービス]** メニューからユーザーが選択したサービスを呼び出します (サービスプロバイダーによって_サンドボックスアプリに対して安全_としてマークされているサービスのみ)。
- 開いている [**最近使っ**たファイル] メニューからユーザーが選択したファイルを開きます。
- コピー & 使用して、他のアプリケーションに貼り付けます。
- 次の世界で読み取り可能な場所からファイルを読み取ります。
  - `/bin`
  - `/sbin`
  - `/usr/bin`
  - `/usr/lib`
  - `/usr/sbin`
  - `/usr/share`
  - `/System`
- によって`NSTemporaryDirectory`作成されたディレクトリ内のファイルの読み取りと書き込みを行います。

既定では、サンドボックス化された Xamarin. Mac アプリによって開かれたファイルまたは保存されたファイルは、アプリが終了するまでアクセスできます (アプリが終了したときにファイルが開いていない場合を除きます)。 開いているファイルは、次回アプリを起動したときに macOS の再開機能を使用して、アプリのサンドボックスに自動的に復元されます。

Xamarin. Mac アプリのコンテナーの外部にあるファイルに永続化を提供するには、セキュリティスコープのブックマークを使用します (下記参照)。

#### <a name="related-items"></a>関連項目

アプリサンドボックスを使用すると、アプリは、同じファイル名で拡張子が異なる関連項目にアクセスできます。 この機能には2つの部分があります。 a) アプリの`Info.plst`ファイル b) コード内の関連する拡張機能の一覧を使用して、これらのファイルでアプリが実行する処理をサンドボックスに指示します。

これには、次の2つのシナリオがあります。

1. アプリは、別のバージョンのファイル (新しい拡張子を持つ) を保存できる必要があります。 たとえば、ファイル`.pdf`への`.txt`ファイルのエクスポートです。 この状況に対処するには、を`NSFileCoordinator`使用してファイルにアクセスする必要があります。 最初に`WillMove(fromURL, toURL)`メソッドを呼び出し、ファイルを新しい拡張機能に移動して、を`ItemMoved(fromURL, toURL)`呼び出します。
2. アプリは、1つの拡張子を持つメインファイルと、拡張機能が異なるいくつかのサポートファイルを開く必要があります。 たとえば、ムービーやサブタイトルファイルなどです。 セカンダリファイル`NSFilePresenter`にアクセスするには、を使用します。 `PrimaryPresentedItemURL`プロパティにメインファイルを、 `PresentedItemURL`プロパティにセカンダリファイルを指定します。 メインファイルが開いたら、 `AddFilePresenter` `NSFileCoordinator`クラスのメソッドを呼び出して、セカンダリファイルを登録します。

どちらのシナリオでも、アプリの**情報の plist**ファイルは、アプリが開くことのできるドキュメントの種類を宣言する必要があります。 任意のファイルの種類につい`NSIsRelatedItemType`て、 `CFBundleDocumentTypes`配列内の`YES`エントリに (値を持つ) を追加します。

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>サンドボックスアプリを使用してダイアログの動作を開いて保存する

には、次の制限が`NSOpenPanel`適用`NSSavePanel`されます。これらの制限は、サンドボックス化された Xamarin. Mac アプリから呼び出すときに行われます。

- プログラムを使用して **[OK** ] ボタンを呼び出すことはできません。
- では、 `NSOpenSavePanelDelegate`ユーザーの選択をプログラムで変更することはできません。

また、次の継承の変更が行われています。

- **サンドボックスではないアプリ** -  `NSOpenPanel``NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>セキュリティスコープのブックマークと永続的なリソースへのアクセス

前述のように、サンドボックス化された Xamarin. Mac アプリケーションは、(PowerBox によって提供されるように) 直接ユーザーの操作を行うことによって、コンテナーの外部にあるファイルまたはリソースにアクセスできます。 ただし、これらのリソースへのアクセスは、アプリの起動またはシステムの再起動にわたって自動的に保持されません。

_セキュリティスコープのブックマーク_を使用することにより、サンドボックス化された Xamarin. Mac アプリケーションは、ユーザーの意図を維持し、アプリの再起動後に特定のリソースへのアクセスを維持することができます。

#### <a name="security-scoped-bookmark-types"></a>セキュリティスコープのブックマークの種類

セキュリティスコープのブックマークと永続的なリソースへのアクセスを使用する場合は、次の2つの sistine のユースケースがあります。

- **アプリスコープのブックマークは、ユーザーが指定したファイルまたはフォルダーへの永続的なアクセスを提供します。**

    たとえば、サンドボックス化された Xamarin. Mac アプリケーションでを使用して編集用に (を使用`NSOpenPanel`して) 外部ドキュメントを開くことが許可されている場合、アプリでは、アプリスコープのブックマークを作成して、後で同じファイルに再びアクセスできるようにすることができます。
- **ドキュメントスコープのブックマークは、特定のドキュメントのサブファイルへの永続的なアクセスを提供します。**

たとえば、個々の画像、ビデオクリップ、および後で1つのムービーに結合されるサウンドファイルにアクセスできるプロジェクトファイルを作成するビデオ編集アプリなどです。

ユーザーが (を`NSOpenPanel`使用して) プロジェクトにリソースファイルをインポートすると、アプリはプロジェクトに格納されている項目のドキュメントスコープのブックマークを作成します。これにより、ファイルは常にアプリにアクセスできるようになります。

ドキュメントスコープのブックマークは、ブックマークデータとドキュメント自体を開くことのできる任意のアプリケーションによって解決できます。 これにより、移植性がサポートされるため、ユーザーはプロジェクトファイルを別のユーザーに送信し、すべてのブックマークを使用することもできます。

> [!IMPORTANT]
> ドキュメントスコープのブックマークは、フォルダーではなく1つのファイル_のみ_を指すことができ、そのファイルは、システムによって使用さ`/private`れる`/Library`場所 (やなど) には配置できません。

#### <a name="using-security-scoped-bookmarks"></a>セキュリティスコープのブックマークの使用

どちらの種類のセキュリティスコープのブックマークを使用する場合でも、次の手順を実行する必要があります。

1. **セキュリティスコープのブックマークを使用する必要がある Xamarin アプリで適切な権限を設定**します。アプリスコープのブックマークでは`com.apple.security.files.bookmarks.app-scope` 、権利キー `true`をに設定します。 ドキュメントスコープのブックマークの場合は、 `com.apple.security.files.bookmarks.document-scope`権利キーを`true`に設定します。
2. **セキュリティスコープのブックマークを作成**する-この操作は、ユーザーがアクセスを提供したファイルまたはフォルダー ( `NSOpenPanel`など) に対して、アプリが永続的なアクセスを必要とする場合に実行します。 クラスのメソッドを使用して、ブックマークを作成します。 `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` `NSUrl`
3. **セキュリティスコープのブックマークを解決**する-アプリがリソースにもう一度アクセスする必要がある場合 (たとえば、再起動後)、セキュリティスコープの URL にブックマークを解決する必要があります。 クラスのメソッドを使用して、ブックマークを解決します。 `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` `NSUrl`
4. **ファイルへのアクセスを必要としているシステムに、セキュリティスコープの url から明示的に通知**する-この手順は、上のセキュリティスコープの url を取得した直後に行う必要があります。また、後でリソースへのアクセスを回復する場合は、放棄へのアクセスを許可します。 クラスのメソッドを呼び出して`StartAccessingSecurityScopedResource ()` 、セキュリティスコープの URL へのアクセスを開始します。 `NSUrl`
5. **セキュリティスコープの URL からのファイルへのアクセスが完了したことをシステムに明示的に通知**する-できるだけ早く、アプリがファイルにアクセスする必要がなくなったことをシステムに通知する必要があります (ユーザーがファイルを閉じた場合など)。 セキュリティスコープの URL へ`NSUrl`のアクセスを停止するには、クラスのメソッドを呼び出します。`StopAccessingSecurityScopedResource ()`

リソースへのアクセスを解放した後、再度手順4に戻ってアクセスを再確立する必要があります。 Xamarin アプリが再起動された場合は、手順3に戻ってブックマークを再解決する必要があります。

> [!IMPORTANT]
> セキュリティスコープの URL リソースへのアクセスを解放できないと、Xamarin. Mac アプリでカーネルリソースがリークします。 その結果、アプリケーションは、再起動されるまで、ファイルシステムの場所をコンテナーに追加できなくなります。

### <a name="the-app-sandbox-and-code-signing"></a>アプリのサンドボックスとコード署名

アプリのサンドボックスを有効にし、(権利を使用して) Xamarin. Mac アプリの特定の要件を有効にした後、サンドボックスが有効になるようにプロジェクトにコード署名する必要があります。 アプリのサンドボックスに必要な資格はアプリの署名にリンクされるため、コード署名を実行する必要があります。

macOS では、アプリのコンテナーとそのコード署名の間にリンクが適用されます。この方法では、アプリバンドル ID をスプーフィングする場合でも、他のアプリケーションがそのコンテナーにアクセスすることはできません。 このメカニズムは次のように機能します。

1. システムは、アプリのコンテナーを作成するときに、そのコンテナーに_Access Control リスト_(ACL) を設定します。 一覧の最初のアクセス制御エントリには、アプリの_指定_された要件 (DR) が含まれています。これは、アプリの将来のバージョンを認識できるかどうかを示します (アップグレードされた場合)。
2. 同じバンドル ID を持つアプリが起動するたびに、システムは、アプリのコード署名が、コンテナーの ACL 内のいずれかのエントリで指定された要件に一致することを確認します。 一致するものが見つからない場合は、アプリを起動できなくなります。

コード署名は次のように機能します。

1. Xamarin プロジェクトを作成する前に、Apple Developer ポータルから開発証明書、配布証明書、および開発者 ID 証明書を取得します。
2. Mac App Store が Xamarin. Mac アプリを配布すると、Apple コード署名で署名されます。

テストとデバッグを行う場合は、署名したバージョンの Xamarin. Mac アプリケーションを使用します (アプリコンテナーの作成に使用されます)。 後で、Apple App Store からバージョンをテストまたはインストールする場合、Apple の署名で署名され、(元のアプリコンテナーと同じコード署名がないため) 起動に失敗します。 この状況では、次のようなクラッシュレポートが表示されます。

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

この問題を解決するには、Apple の署名されたバージョンのアプリを指すように ACL エントリを調整する必要があります。

サンドボックスに必要なプロビジョニングプロファイルの作成とダウンロードの詳細については、前の「[アプリの署名とプロビジョニング](#Signing_and_Provisioning_the_App)」セクションを参照してください。

#### <a name="adjusting-the-acl-entry"></a>ACL エントリの調整

Apple の署名されたバージョンの Xamarin. Mac アプリを実行できるようにするには、次の操作を行います。

1. ターミナルアプリ ( `/Applications/Utilities`) を開きます。
2. Apple の署名されたバージョンの Xamarin. Mac アプリに Finder ウィンドウを開きます。
3. ターミナル`asctl container acl add -file`ウィンドウに「」と入力します。
4. [ファインダー] ウィンドウから Xamarin アプリのアイコンをドラッグし、ターミナルウィンドウにドロップします。
5. ファイルへの完全なパスは、ターミナルのコマンドに追加されます。
6. **Enter**キーを押してコマンドを実行します。

コンテナーの ACL には、Xamarin の両方のバージョンに対して指定されたコード要件が含まれるようになりました。 Mac アプリと macOS では、どちらのバージョンも実行できるようになりました。

#### <a name="display-a-list-of-acl-code-requirements"></a>ACL コード要件の一覧を表示する

コンテナーの ACL でコード要件の一覧を表示するには、次の手順を実行します。

1. ターミナルアプリ ( `/Applications/Utilities`) を開きます。
2. 「`asctl container acl list -bundle <container-name>`」と入力します。
3. **Enter**キーを押してコマンドを実行します。

は`<container-name>` 、通常、Xamarin. Mac アプリケーションのバンドル識別子です。

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>アプリサンドボックス用の Xamarin. Mac アプリの設計

アプリサンドボックス用の Xamarin. Mac アプリを設計するときは、共通のワークフローがあります。 ただし、アプリでのサンドボックスの実装の詳細は、特定のアプリの機能に固有のものになります。

### <a name="six-steps-for-adopting-the-app-sandbox"></a>アプリのサンドボックスを導入するための6つの手順

アプリサンドボックス用の Xamarin. Mac アプリの設計は、通常、次の手順で構成されています。

1. アプリがサンドボックスに適しているかどうかを判断します。
2. 開発と配布の戦略を設計します。
3. API の非互換性を解決します。
4. 必要なアプリサンドボックスの権利を Xamarin プロジェクトに適用します。
5. XPC を使用して特権の分離を追加します。
6. 移行戦略を実装します。

> [!IMPORTANT]
> アプリバンドルのメインの実行可能ファイルをサンドボックスにするだけでなく、そのバンドルに含まれるすべてのヘルパーアプリまたはツールにもサンドボックスを追加する必要があります。 これは、Mac App Store から配布されたすべてのアプリに必要であり、可能な場合は、他の形式のアプリ配布に対して実行する必要があります。

Xamarin. Mac アプリのバンドルに含まれるすべての実行可能バイナリの一覧を表示するには、ターミナルで次のコマンドを入力します。

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

ここ`[Your-App-Bundle]`で、はアプリケーションのバンドルの名前とパスです。

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Xamarin アプリがサンドボックスに適しているかどうかを判断する

ほとんどの Xamarin. Mac アプリは、アプリサンドボックスと完全に互換性があるため、サンドボックスに適しています。 アプリのサンドボックスで許可されていない動作がアプリに必要な場合は、別の方法を検討する必要があります。

アプリが次のいずれかの動作を必要とする場合は、アプリサンドボックスと互換性がありません。

- **承認サービス**-アプリサンドボックスでは、「[承認サービス C リファレンス](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826)」で説明されている機能を使用することはできません。
- **ユーザー補助 api** -スクリーンリーダーや、他のアプリケーションを制御するアプリなどの補助的なアプリをサンドボックスにすることはできません。
- **Apple イベントを任意のアプリに送信する**-アプリで不明な任意のアプリに apple イベントを送信する必要がある場合は、サンドボックス化できません。 呼び出されたアプリの既知の一覧については、アプリをサンドボックス化することができ、権利には呼び出されたアプリの一覧を含める必要があります。
- **他のタスクへの分散通知でのユーザー情報ディクショナリの送信**-アプリのサンドボックスを`userInfo`使用すると、他`NSDistributedNotificationCenter`のタスクのメッセージング用にオブジェクトに投稿するときに辞書を含めることはできません。
- **カーネル拡張機能の読み込み**-カーネル拡張機能の読み込みは、アプリのサンドボックスでは禁止されています。
- **[開いて保存] ダイアログでユーザー入力をシミュレート**する-プログラムによって開いたり保存のダイアログを操作してユーザー入力をシミュレートまたは変更することは、アプリのサンドボックスでは禁止されています。
- **他のアプリの基本設定へのアクセスまたは設定**-他のアプリの設定の操作は、アプリのサンドボックスでは禁止されています。
- **ネットワーク設定の構成**-ネットワーク設定の操作は、アプリのサンドボックスでは禁止されています。
- **他のアプリを終了**する-アプリサンド`NSRunningApplication`ボックスは、を使用して他のアプリを終了することを禁止します。

### <a name="resolving-api-incompatibilities"></a>API の非互換性の解決

アプリサンドボックス用の Xamarin. Mac アプリを設計する場合、一部の macOS Api の使用に関して非互換性が発生する可能性があります。

いくつかの一般的な問題とその解決方法を次に示します。

- **ドキュメントを開く、保存、追跡**する-以外`NSDocument`のテクノロジを使用してドキュメントを管理している場合は、アプリサンドボックスの組み込みサポートにより、ドキュメントに切り替える必要があります。 `NSDocument`PowerBox で自動的に動作し、ユーザーがファインダーでドキュメントを移動した場合に、サンドボックス内でドキュメントを保持するためのサポートを提供します。
- **ファイルシステムリソースへのアクセスを保持**する-Xamarin アプリケーションがコンテナーの外部にあるリソースへの永続的なアクセスに依存している場合は、セキュリティスコープのブックマークを使用してアクセスを維持します。
- **アプリのログイン項目を作成**する-アプリサンドボックスを使用して`LSSharedFileList`ログイン項目を作成することはできません`LSRegisterURL`。また、を使用して起動サービスの状態を操作することもできません。 「 `SMLoginItemSetEnabled` [サービス管理フレームワークのドキュメントを使用してログイン項目を追加する](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1)」で説明されているように、関数を使用します。
- **ユーザーデータへのアクセス**-などの POSIX 関数`getpwuid`を使用してディレクトリサービスからユーザーのホームディレクトリを取得する場合は、のような cocoa または Core `NSHomeDirectory`Foundation のシンボルを使用することを検討してください。
- **他のアプリの基本設定**へのアクセス-アプリのサンドボックスでは、アプリのコンテナーにパス検索 api が指示されるため、設定の変更はそのコンテナー内で行われ、別のアプリの設定へのアクセスは許可されていません。
- **Web ビューでの HTML5 Embedded ビデオの使用**-Xamarin アプリで WebKit を使用して埋め込み HTML5 ビデオを再生する場合は、アプリを AV Foundation framework にもリンクする必要があります。 アプリのサンドボックスでは、CoreMedia がこれらのビデオを再生できないようにします。

### <a name="applying-required-app-sandbox-entitlements"></a>必要なアプリサンドボックスの権利を適用しています

アプリサンドボックスで実行するすべての Xamarin. Mac アプリケーションの権利を編集し、[**アプリのサンドボックスを有効に**する] チェックボックスをオンにする必要があります。

アプリの機能に基づいて、OS の機能やリソースにアクセスするために他の権利を有効にすることが必要になる場合があります。 アプリのサンドボックス化は、アプリの実行に必要な最低限の権限に要求する権利を最小限に抑える場合に最適です。そのため、資格をランダムに有効にするだけで済みます。

Xamarin. Mac アプリが必要とする権利を確認するには、次の手順を実行します。

1. アプリサンドボックスを有効にし、Xamarin. Mac アプリを実行します。
2. アプリの機能を実行します。
3. コンソールアプリ (で`/Applications/Utilities`利用可能) を開き、[すべての**メッセージ**] ログで [違反] を`sandboxd`探します。
4. 違反が`sandboxd`発生するたびに、他のファイルシステムの場所ではなくアプリコンテナーを使用して問題を解決するか、アプリサンドボックスの権利を適用して制限された OS 機能にアクセスできるようにします。
5. すべての Xamarin. Mac アプリの機能を再実行して、もう一度テストします。
6. すべて`sandboxd`の違反が解決されるまで繰り返します。

### <a name="add-privilege-separation-using-xpc"></a>XPC を使用して特権の分離を追加する

アプリサンドボックス用の Xamarin. Mac アプリを開発する場合、特権とアクセスの観点からアプリの動作を確認し、危険度の高い操作を独自の XPC サービスに分離することを検討してください。

詳細については、「Apple の[XPC サービスの作成](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6)」および「[デーモンとサービスのプログラミングガイド](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i)」を参照してください。

### <a name="implement-a-migration-strategy"></a>移行戦略を実装する

以前にサンドボックス化されていなかった新しいサンドボックス化されたバージョンの Xamarin アプリケーションをリリースする場合は、現在のユーザーがスムーズなアップグレードパスを持っていることを確認する必要があります。

 コンテナー移行マニフェストを実装する方法の詳細については、Apple の「[アプリのサンドボックスドキュメントへの移行」を](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1)参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションのサンドボックス化について詳しく説明しました。 まず、アプリのサンドボックスの基本を示す単なる Xamarin. Mac アプリを作成しました。 次に、サンドボックス違反の解決方法を説明しました。 次に、アプリのサンドボックスについて詳しく説明しました。最後に、アプリのサンドボックス用の Xamarin. Mac アプリの設計について見てきました。

## <a name="related-links"></a>関連リンク

- [App Store への発行](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [アプリサンドボックスについて](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [アプリのサンドボックス化に関する一般的な問題](https://developer.apple.com/library/content/qa/qa1773/_index.html)
