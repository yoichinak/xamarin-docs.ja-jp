---
title: HomeKit
description: "HomeKit は、ホーム オートメーションのデバイスを制御するための Apple のフレームワークです。 この記事では、HomeKit を紹介し、HomeKit アクセサリ シミュレーターと単純な Xamarin.iOS アプリの作成を操作これらアクセサリの構成のテスト accessories について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: ea51dc2c7dadc5cc430df990c9ce79eac6e941da
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="homekit"></a>HomeKit

_HomeKit は、ホーム オートメーションのデバイスを制御するための Apple のフレームワークです。この記事では、HomeKit を紹介し、HomeKit アクセサリ シミュレーターと単純な Xamarin.iOS アプリの作成を操作これらアクセサリの構成のテスト accessories について説明します。_

[![](homekit-images/accessory01.png "HomeKit 例には、アプリが有効になっています。")](homekit-images/accessory01.png#lightbox)

Apple には、一貫性のある、1 つの単位にさまざまなベンダーから複数のホーム オートメーション デバイスをシームレスに統合する方法と iOS 8 での HomeKit が導入されました。 検出するための一般的なプロトコル、昇格によって構成およびホーム オートメーション デバイスを制御する HomeKit、デバイス、各ベンダーの作業を調整することがなく、連携する関連以外のベンダーからできます。

HomeKit、仕入先を使用して、Api またはアプリに提供されることがなく、任意の有効になっている HomeKit デバイスを制御する Xamarin.iOS アプリを作成できます。 HomeKit では、次の操作を行うことができます。

- 有効になっている HomeKit ホーム オートメーションの新しいデバイスを検出し、によって、すべてのユーザーの iOS デバイスの間で永続化されるデータベースに追加します。
- セットアップ、構成、表示、および、HomeKit で任意のデバイスを制御_構成データベース ホーム_です。
- 構成済み HomeKit デバイスと通信し、コマンドを個々 のアクションを実行したり、キッチンでライトのすべての有効化などの連携にします。

に加えて、ホーム構成データベースを有効になっている HomeKit アプリでデバイスの処理、HomeKit Siri 音声コマンドへのアクセスを提供します。 適切に構成された、HomeKit セットアップを指定するには、発行することで音声コマンドなど"Siri、リビング ルームのライトを有効にします"。

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>ホーム構成データベース

HomeKit ホーム コレクションに指定された場所にすべてのオートメーション デバイスを整理します。 このコレクションは、ユーザーがホーム オートメーション デバイスを意味のある、人間が判読できるラベルが付いた論理的に配置の単位にグループ化するための方法を提供します。

ホーム コレクションは、ホーム構成データベースを自動的にバックアップと同期したすべてのユーザーの iOS デバイスに格納されます。 HomeKit は、構成データベースを操作するため、次のクラスを提供します。

- `HMHome` -これは、1 つの物理的な場所 (のすべての情報とホーム オートメーションのすべてのデバイス用の構成を保持する最上位のコンテナー 1 つのファミリの居住地)。 ユーザーは、メイン ホームおよび休暇家など、1 つ以上の居住地があります。 または、異なる「格納」、同じプロパティのメイン家とガレージ経由でゲスト ハウスなどがあります。 どちらの方法でも、少なくとも 1 つ`HMHome`オブジェクト_必要があります_をセットアップして、その他の HomeKit 情報を入力する前に格納します。
- `HMRoom` -While 省略可能で、`HMRoom`自宅内で特定の部屋を定義することができます (`HMHome`) など: 調理、洗面、ガレージまたはリビング ルーム。 ユーザーがグループのすべてのホーム オートメーション デバイスに自分の家で特定の場所に、`HMRoom`およびそれらの単位として操作します。 たとえば、ガレージ ライトをオフに Siri を求めています。
- `HMAccessory` -これは、個人を表します、物理 HomeKit (スマート サーモスタット) など、ユーザーの居住地にインストールされているオートメーション デバイスを有効にします。 各`HMAccessory`に割り当てられている、`HMRoom`です。 ユーザーは、すべてのルームを構成していない、HomeKit は特殊な既定のルームにアクセサリを割り当てます。
- `HMService` -によって提供されるサービスを表す、指定された`HMAccessory`、ライトまたは (色を変更すると、サポートされています) 場合は、その色のオン/オフ状態などです。 各`HMAccessory`も含まれており、ライト ガレージきっかけなど、複数のサービスを持つことができます。 さらに、特定`HMAccessory`などのサービス、ファームウェアの更新プログラム、ユーザー コントロールの外にある場合があります。
- `HMZone` -により、ユーザーのコレクションをグループ化を`HMRoom`上、Downstairs 地下室などの論理ゾーンのオブジェクト。 省略可能で、これにより、Siri を求めるように相互作用オフ downstairs 光のすべてが有効にします。

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>HomeKit アプリのプロビジョニング

HomeKit によって課せられるセキュリティ要件のため、Xamarin.iOS アプリ HomeKit framework を使用する必要があります適切に構成して Xamarin.iOS プロジェクト ファイル、Apple 開発者ポータルでします。

次の手順で行います。

1. ログイン、 [Apple 開発者ポータル](http://developer.apple.com)です。
2. をクリックして**証明書、識別子、およびプロファイル**です。
3. 完了していない場合は、をクリックして**識別子**、アプリの ID を作成し、(例: `com.company.appname`)、それ以外の場合、既存の ID を編集
4. いることを確認、 **HomeKit**サービスは、指定された ID のチェックが完了します。 

    [![](homekit-images/provision01.png "指定した ID の HomeKit サービスを有効にします。")](homekit-images/provision01.png#lightbox)
5. 変更内容を保存します。
4. をクリックして**プロビジョニング プロファイル** > **開発**し、プロビジョニング プロファイル、アプリを新規の開発を作成します。 

    [![](homekit-images/provision02.png "プロビジョニング プロファイル、アプリを新規の開発を作成します。")](homekit-images/provision02.png#lightbox)
5. ダウンロードし、新しいプロビジョニング プロファイルをインストールするか、Xcode を使用してダウンロードし、プロファイルをインストールしてください。
6. Xamarin.iOS プロジェクトのオプションを編集し、先ほど作成したプロビジョニング プロファイルを使用していることを確認します。 

    [![](homekit-images/provision03.png "先ほど作成したプロビジョニング プロファイルを選択します。")](homekit-images/provision03.png#lightbox)
7. 次に、編集、 **Info.plist**ファイルをプロビジョニング プロファイルの作成に使用されたアプリ ID を使用していることを確認してください。 

    [![](homekit-images/provision04.png "アプリ ID を設定します。 ")](homekit-images/provision04.png#lightbox)
8. 最後に、編集、 **Entitlements.plist**ファイルのことを確認して、 **HomeKit**権利が選択されています。 

    [![](homekit-images/provision05.png "HomeKit 権利を有効にします。")](homekit-images/provision05.png#lightbox)
9. すべてのファイルに変更を保存します。

これらの設定で、アプリケーションは HomeKit フレームワーク Api にアクセスする準備ができてようになりました。 プロビジョニングの詳細についてを参照してください、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)と[してアプリをプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイドです。

> [!IMPORTANT]
> **注:**の開発が正しくプロビジョニングされている実際の iOS デバイスを必要と HomeKit が有効になっているアプリをテストします。 IOS シミュレーターでは、HomeKit をテストすることはできません。

## <a name="the-homekit-accessory-simulator"></a>HomeKit アクセサリ シミュレーター

Apple が作成しなくても、物理デバイスがあるすべての可能なホーム オートメーション デバイスとサービスをテストする方法を提供する、 _HomeKit アクセサリ シミュレーター_です。 このシミュレーターを使用して、セットアップし、仮想 HomeKit デバイスを構成できます。

### <a name="installing-the-simulator"></a>シミュレーターをインストールします。

Apple は、続行する前にインストールする必要がありますので HomeKit アクセサリ シミュレーターを個別のダウンロードとして、Xcode から提供します。

次の手順で行います。

1. Web ブラウザーで、次を参照してください[Apple 開発者向けダウンロード。](https://developer.apple.com/download/more/?name=for%20Xcode)
2. ダウンロード、 **Xcode xxx の他のツール**(xxx はインストールされている Xcode のバージョン)。 

    [![](homekit-images/simulator01.png "Xcode の他のツールをダウンロードします。")](homekit-images/simulator01.png#lightbox)
3. ディスク イメージを開き、ツールをインストール、**アプリケーション**ディレクトリ。

インストールされている、HomeKit アクセサリ シミュレーターでは、テスト用仮想アクセサリ の順に作成できます。

### <a name="creating-virtual-accessories"></a>仮想アクセサリを作成します。

を HomeKit アクセサリ シミュレーターを開始してから、いくつかの仮想アクセサリを作成するには、次の操作を行います。

1. アプリケーション フォルダーから、HomeKit アクセサリ シミュレーターを開始します。 

    [![](homekit-images/simulator02.png "HomeKit アクセサリ シミュレーター")](homekit-images/simulator02.png#lightbox)
2. クリックして、  **+** ボタンをクリックして**新しいアクセサリしています.**: 

    [![](homekit-images/simulator03.png "新しいアクセサリを追加します。")](homekit-images/simulator03.png#lightbox)
3. 新しいアクセサリについての情報を入力し、クリックして、**完了**ボタンをクリックします。 

    [![](homekit-images/simulator04.png "新しいアクセサリについての情報を入力します。")](homekit-images/simulator04.png#lightbox)
4. クリックして、**サービスを追加する.** ボタンをクリックし、ドロップダウン リストから、サービスの種類を選択します。 

    [![](homekit-images/simulator05.png "ドロップダウン リストから、サービスの種類を選択します。")](homekit-images/simulator05.png#lightbox)
5. 提供、**名前**、サービスとクリック、**完了**ボタン。 

    [![](homekit-images/simulator06.png "サービスの名前を入力します。")](homekit-images/simulator06.png#lightbox)
6. クリックして、サービスの省略可能な特性が実現することができます、**追加特性**ボタンをクリックし、必要な設定を構成します。 

    [![](homekit-images/simulator07.png "必要な設定を構成します。")](homekit-images/simulator07.png#lightbox)
7. HomeKit をサポートしているホーム オートメーションの仮想デバイスの種類ごとの 1 つを作成する上記の手順を繰り返します。

いくつかのサンプル仮想 HomeKit アクセサリと作成、構成、今すぐを消費し、Xamarin.iOS アプリからこれらのデバイスを制御できます。

## <a name="configuring-the-infoplist-file"></a>Info.plist ファイルを構成します。

IOS 10 の新しい (大きい)、開発者は必要がありますを追加、`NSHomeKitUsageDescription`キーをアプリの`Info.plist`ファイルし、文字列を宣言する、アプリがユーザーの HomeKit データベースにアクセスしようとした理由を提供します。 この文字列は、ユーザーが初めて実行するとき、アプリに表示されます。

[![](homekit-images/info01.png "HomeKit アクセス許可ダイアログ")](homekit-images/info01.png#lightbox)

このキーを設定するには、次の操作を行います。

1. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
2. 画面の下部に切り替え、**ソース**ビュー。
3. 新しい**エントリ**一覧にします。
4. ドロップダウン リストから選択**プライバシー - HomeKit 用途説明**: 

    [![](homekit-images/info02.png "プライバシー - HomeKit 使用状況の説明を選択します。")](homekit-images/info02.png#lightbox)
5. アプリがユーザーの HomeKit データベースにアクセスしようとした理由の説明を入力します。 

    [![](homekit-images/info03.png "説明を入力します")](homekit-images/info03.png#lightbox)
6. 変更内容をファイルに保存します。

> [!IMPORTANT]
> **注:**設定に失敗した、`NSHomeKitUsageDescription`でキー、`Info.plist`ファイルの場合は、アプリ_サイレント モードで障害が発生_(実行時にシステムに閉じられました) 10 (またはそれ以上)、iOS では実行時エラーなし。

## <a name="connecting-to-homekit"></a>HomeKit への接続

Xamarin.iOS アプリを HomeKit を通信するためには、最初のインスタンスにインスタンス化する必要があります、`HMHomeManager`クラスです。 ホーム Manager HomeKit へのサーバーの全体のエントリ ポイントは、使用可能な家庭の一覧を提供する責任を更新およびその一覧を維持し、ユーザーを返す_プライマリ ホーム_です。

`HMHome`オブジェクトには、任意のルーム、グループ、またはインストールされている任意のホーム オートメーション アクセサリと共に、含まれるゾーンを含む付与ホームに関するすべての情報が含まれています。 HomeKit、少なくとも 1 つでの操作を実行する前に`HMHome`作成し、プライマリ ホームとして割り当てる必要があります。

アプリは、プライマリ ホームが存在するかをチェックして作成し、そうでない場合に割り当てる 1 つを担当します。

### <a name="adding-a-home-manager"></a>ホーム マネージャーを追加します。

HomeKit 対応アプリを追加する、Xamarin.iOS、編集、 **<code>appdelegate.cs</code>**ファイルを編集し、次のようになります。

```csharp
using HomeKit;
...

public HMHomeManager HomeManager { get; set; }
...

public override void FinishedLaunching (UIApplication application)
{
    // Attach to the Home Manager
    HomeManager = new HMHomeManager ();
    Console.WriteLine ("{0} Home(s) defined in the Home Manager", HomeManager.Homes.Count());

    // Wire-up Home Manager Events
    HomeManager.DidAddHome += (sender, e) => {
        Console.WriteLine("Manager Added Home: {0}",e.Home);
    };

    HomeManager.DidRemoveHome += (sender, e) => {
        Console.WriteLine("Manager Removed Home: {0}",e.Home);
    };
    HomeManager.DidUpdateHomes += (sender, e) => {
        Console.WriteLine("Manager Updated Homes");
    };
    HomeManager.DidUpdatePrimaryHome += (sender, e) => {
        Console.WriteLine("Manager Updated Primary Home");
    };
}
```

アプリケーションが最初に実行時に、HomeKit 情報にアクセスすることを許可するかどうか、ユーザーが求められます。

[![](homekit-images/home01.png "HomeKit 情報にアクセスすることを許可するかどうか、ユーザーが求められます")](homekit-images/home01.png#lightbox)

答えた場合**OK**アプリケーションがその HomeKit アクセサリを操作できるし、それ以外の場合にない HomeKit への呼び出しは失敗し、エラーです。

Manager では、ホームの場所で、次にアプリケーションは必要がありますをプライマリ ホームが構成されていない場合は、ユーザーを作成して割り当てる 1 つの方法を提供します。

### <a name="accessing-the-primary-home"></a>プライマリ ホームにアクセスします。

前述のように、プライマリ ホームを作成および HomeKit を使用する方法を作成して割り当てる場合は 1 つのプライマリ ホーム ユーザーを提供するアプリの責任が存在しないことをお勧めする前に構成されている必要があります。

監視する必要があるアプリが初めて開始したり、バック グラウンドからが返されます、ときに、`DidUpdateHomes`のイベント、`HMHomeManager`プライマリ ホームの存在を確認するクラス。 いずれかが存在しない場合は、ユーザーを 1 つを作成するためのインターフェイスを提供します。

次のコードは、プライマリ ホームを確認するビュー コント ローラーに追加することができます。

```csharp
using HomeKit;
...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
...

// Wireup events
ThisApp.HomeManager.DidUpdateHomes += (sender, e) => {

    // Was a primary home found?
    if (ThisApp.HomeManager.PrimaryHome == null) {
        // Ask user to add a home
        PerformSegue("AddHomeSegue",this);
    }
};
```

ホーム マネージャーに接続する HomeKit、ときに、`DidUpdateHomes`発生するイベント、住宅のマネージャーのコレクションに読み込まれる、既存の家庭に設置、および使用可能な場合、プライマリ ホームが読み込まれます。

### <a name="adding-a-primary-home"></a>プライマリ ホームを追加します。

場合、`PrimaryHome`のプロパティ、`HMHomeManager`は`null`後、`DidUpdateHomes`イベント、ユーザーを作成して続行する前にプライマリ ホームを割り当てる方法を提供する必要があります。

通常、アプリはユーザーがプライマリ ホームとしてセットアップするホーム マネージャーに渡されるを取得する新しいホームという名前のフォームに表示されます。 **HomeKitIntro**サンプル アプリケーション、モーダル ビューが IOS デザイナーで作成され、によって呼び出されます、`AddHomeSegue`アプリのメイン インターフェイスから話題です。

ユーザーが新しいホーム組織とホームを追加するボタンの名前を入力するためのテキスト フィールドを提供します。 ユーザーがタップしたときに、**追加ホーム**ボタン、次のコード マネージャーを呼び出し、ホーム ホームを追加します。

```csharp
// Add new home to HomeKit
ThisApp.HomeManager.AddHome(HomeName.Text,(home,error) =>{
    // Did an error occur
    if (error!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Add Home Error",string.Format("Error adding {0}: {1}",HomeName.Text,error.LocalizedDescription),this);
        return;
    }

    // Make the primary house
    ThisApp.HomeManager.UpdatePrimaryHome(home,(err) => {
        // Error?
        if (err!=null) {
            // Inform user of error
            AlertView.PresentOKAlert("Add Home Error",string.Format("Unable to make this the primary home: {0}",err.LocalizedDescription),this);
            return ;
        }
    });

    // Close the window when the home is created
    DismissViewController(true,null);
});
```

`AddHome`メソッドは新しいホームを作成し、指定されたコールバック ルーチンに戻すことを試みます。 場合、`error`プロパティは使用されません`null`エラーが発生しました、および、ユーザーに提示する必要があります。 最も一般的なエラーには、一意でないホーム名、またはホーム Manager HomeKit と通信することはできませんが原因です。

ホームが正常に作成された場合は、呼び出す必要があります。、`UpdatePrimaryHome`プライマリ ホームとして新しいホームを設定します。 もう一度場合、`error`プロパティは使用されません`null`エラーが発生しました、および、ユーザーに提示する必要があります。

ホーム マネージャーを監視することも必要があります。`DidAddHome`と`DidRemoveHome`必要に応じて、アプリのユーザー インターフェイスのイベントおよび更新します。

> [!IMPORTANT]
> **注:** 、`AlertView.PresentOKAlert`上記のサンプル コードで使用されるメソッドは、作業を ios アラート容易 HomeKitIntro アプリケーションでヘルパー クラス。


## <a name="finding-new-accessories"></a>新しいアクセサリを検索します。

プライマリ ホームが定義されているかホーム マネージャーから読み込まれると、Xamarin.iOS アプリを呼び出すことができます、`HMAccessoryBrowser`をすべて新しいホーム オートメーション アクセサリを見つけて、自宅に追加します。

呼び出す、`StartSearchingForNewAccessories`新しいアクセサリを探して開始するメソッド、および`StopSearchingForNewAccessories`メソッドが完了します。

> [!IMPORTANT]
> **注:** `StartSearchingForNewAccessories`残さないようにバッテリ寿命と iOS デバイスのパフォーマンスの両方に影響が悪影響を及ぼすために、長期間に対して実行されています。 Apple 提案通話`StopSearchingForNewAccessories`分または検索アクセサリ UI が、ユーザーに表示される場合にのみ検索後にします。

`DidFindNewAccessory`イベントと呼び出される新しいアクセサリが検出され、それらに追加するが、`DiscoveredAccessories`アクセサリ ブラウザーの一覧です。

`DiscoveredAccessories`のコレクションを含みます。 一覧`HMAccessory`ホーム オートメーション デバイスと使用可能なサービス、ランプ、ガレージ ドア コントロールなど、付与 HomeKit を定義するオブジェクトが有効にします。

新しいアクセサリが見つかった後、ユーザーに提示する必要があり、選択し、ホームに追加のためです。 例:

[![](homekit-images/accessory01.png "新しいアクセサリを検索します。")](homekit-images/accessory01.png#lightbox)

呼び出す、`AddAccessory`ホームのコレクションを選択したアクセサリを追加するメソッド。 例:

```csharp
// Add the requested accessory to the home
ThisApp.HomeManager.PrimaryHome.AddAccessory (_controller.AccessoryBrowser.DiscoveredAccessories [indexPath.Row], (err) => {
    // Did an error occur
    if (err !=null) {
        // Inform user of error
        AlertView.PresentOKAlert("Add Accessory Error",err.LocalizedDescription,_controller);
    }
});
```

場合、`err`プロパティは使用されません`null`エラーが発生しました、および、ユーザーに提示する必要があります。 それ以外の場合、ユーザーを追加するデバイスのセットアップ コードを入力するように求められます。

[![](homekit-images/accessory02.png "デバイスを追加するには、このセットアップ コードを入力してください。")](homekit-images/accessory02.png#lightbox)

HomeKit アクセサリ シミュレーターでこの数は「、**セットアップ コード**フィールド。

[![](homekit-images/accessory03.png "HomeKit アクセサリ シミュレーターでセットアップ コード フィールド")](homekit-images/accessory03.png#lightbox)

実際の HomeKit アクセサリ、セットアップ コードはか、デバイス自体に、製品の箱にまたはアクセサリのユーザーによる手動でラベルに印刷されます。

アクセサリ ブラウザーを監視する必要があります`DidRemoveNewAccessory`外すアクセサリ一覧から、ユーザーがそのコレクションに追加のホーム インターフェイスのイベントとユーザーを更新します。

## <a name="working-with-accessories"></a>アクセサリの操作

プライマリ ホーム 1 回も確立アクセサリをそれに追加されているを使用するユーザーのアクセサリ (および必要に応じてルーム) の一覧を表示することができます。

`HMRoom`オブジェクトには、特定のルームに関するすべての情報とそれに属する任意のアクセサリが含まれています。 ルームは、1 つまたは複数のゾーンに必要に応じて編成できます。 A`HMZone`特定のゾーンのすべての情報とそれに属するルームのすべてが含まれています。

この例では、おを保存する点単純な使用して作業ホームのアクセサリを直接編成しルームまたはゾーンの代わりにします。

`HMHome`オブジェクトをユーザーに提示する割り当ての付属品の一覧が含まれています。 その`Accessories`プロパティです。 例:

[![](homekit-images/accessory04.png "例アクセサリ")](homekit-images/accessory04.png#lightbox)

ここでフォームで、ユーザーは特定のアクセサリを選択してで提供されるサービスを使用します。

## <a name="working-with-services"></a>サービスの使用方法

ユーザーが対話 HomeKit ホーム オートメーションを有効になっている、特定のデバイス場合、これは通常で提供されるサービスを使用します。 `Services`のプロパティ、`HMAccessory`クラスのコレクションを格納する`HMService`サービスを定義するオブジェクト、デバイスを提供します。

サービスは、ライト、サーモスタット、ガレージ ドアを開ける装置、スイッチ、またはロックのようなものです。 (ガレージきっかけ) のような一部のデバイスでは、ライトやドアを開いたり、閉じたりする機能など、複数のサービスを提供します。

各アクセサリを含む指定されたアクセサーを提供する特定のサービスだけでなく、`Information Service`プロパティ名、製造元、モデルやシリアル番号などを定義します。

### <a name="accessory-service-types"></a>アクセサリ サービスの種類

次のサービスの種類は経由で使用できる、`HMServiceType`列挙型。

- **AccessoryInformation** -ホーム オートメーションが指定されたデバイス (アクセサリ) に関する情報を提供します。
- **AirQualitySensor** -空気品質センサーを定義します。
- **バッテリ**-アクセサリのバッテリの状態を定義します。
- **CarbonDioxideSensor** -二酸化炭素センサーの二酸化炭素排出量を定義します。
- **CarbonMonoxideSensor** -一酸化炭素センサーを定義します。
- **ContactSensor** -連絡先のセンサー (開かれたり閉じられたりするウィンドウ) などを定義します。
- **ドア**-ドア状態センサー (開かれたり閉じられたりなど) を定義します。
- **ファン**-リモート コントロール ファンを定義します。
- **GarageDoorOpener** -ガレージきっかけを定義します。
- **HumiditySensor** -湿度センサーを定義します。
- **LeakSensor** -リーク センサー (給湯または洗濯機のような) を定義します。
- **電球**・ スタンド アロンの明るいまたは (ガレージきっかけ) などの他の付属品の一部であるライトを定義します。
- **LightSensor** -光センサーを定義します。
- **LockManagement** -自動ドアのロックを管理するサービスを定義します。
- **LockMechanism** -(ドアのロック) のようにリモート制御されたロックを定義します。
- **MotionSensor** -するモーション センサーを定義します。
- **OccupancySensor**の占有率センサーを定義します。
- **コンセント**-リモート コントロール コンセントを定義します。
- **SecuritySystem** -ホーム セキュリティ システムを定義します。
- **StatefulProgrammableSwitch** -1 回 (フリップ スイッチ) のようなトリガーにより状態に維持されるプログラミング可能なスイッチを定義します。
- **StatelessProgrammableSwitch** -(プッシュ ボタン) のようなトリガーされています。 後の初期状態に戻りますをプログラミング可能なスイッチを定義します。
- **SmokeSensor** -スモーク センサーを定義します。
- **切り替える**-壁の標準スイッチと同様に、オン/オフ切り替えを定義します。
- **温度センサー**の温度センサーを定義します。
- **サーモスタット**-空調システムを制御するために使用したスマート サーモスタットを定義します。
- **ウィンドウ**-, 飴の開かれたり閉じられたりするリモートの自動化されたウィンドウを定義します。
- **WindowCovering**に記載されている、開かれたり閉じられたりすることができますをブラインドのようにリモートで制御されるウィンドウを定義します。

### <a name="displaying-service-information"></a>サービス情報を表示します。

読み込み後、 `HMAccessory` 、個々 のクエリを実行できます`HNService`オブジェクトを提供し、ユーザーにその情報を表示します。

[![](homekit-images/accessory05.png "サービス情報を表示します。")](homekit-images/accessory05.png#lightbox)

必ず確認する必要があります、`Reachable`のプロパティ、`HMAccessory`前にそれを処理します。 アクセサリには、到達できない、ユーザーがデバイスの範囲内かかどうか、接続されていないことができます。

サービスを選択すると、ユーザーが表示または監視または指定されたホーム オートメーション デバイスを制御するには、そのサービスの 1 つまたは複数の特性を変更できます。

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>特性の操作

各`HMService`オブジェクトのコレクションを含めることができます`HMCharacteristic`(開かれたり閉じられたりするドア) のようなサービスの状態に関する情報を提供したり (光源の色の設定) などの状態を調整するユーザーを許可するオブジェクト。

`HMCharacteristic` 特性とその状態に関する情報だけでなくが経由で状態を操作するためのメソッドも提供_特性メタデータ_(`HMCharacteristisMetadata`)。 このメタデータは、ユーザーにできるようになる状態を変更する情報を表示するときに、便利なプロパティ (最小と最大値の範囲) などを提供できます。

`HMCharacteristicType`列挙型が定義されるか、次のように変更する特性のメタデータ値のセットを提供します。

 - AdminOnlyAccess
 - AirParticulateDensity
 - AirParticulateSize
 - AirQuality
 - AudioFeedback
 - BatteryLevel
 - [明るさ]
 - CarbonDioxideDetected
 - CarbonDioxideLevel
 - CarbonDioxidePeakLevel
 - CarbonMonoxideDetected
 - CarbonMonoxideLevel
 - CarbonMonoxidePeakLevel
 - ChargingState
 - ContactState
 - CoolingThreshold
 - CurrentDoorState
 - CurrentHeatingCooling
 - CurrentHorizontalTilt
 - CurrentLightLevel
 - CurrentLockMechanismState
 - CurrentPosition
 - CurrentRelativeHumidity
 - CurrentSecuritySystemState
 - CurrentTemperature
 - CurrentVerticalTilt
 - FirmwareVersion
 - HardwareVersion
 - HeatingCoolingStatus
 - HeatingThreshold
 - HoldPosition
 - [色合い]
 - Identify
 - InputEvent
 - LeakDetected
 - LockManagementAutoSecureTimeout
 - LockManagementControlPoint
 - LockMechanismLastKnownAction
 - ログ
 - 製造元
 - モデル
 - MotionDetected
 - name
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
 - [彩度]
 - SerialNumber
 - SmokeDetected
 - SoftwareVersion
 - StatusActive
 - StatusFault
 - StatusJammed
 - StatusLowBattery
 - StatusTampered
 - TargetDoorState
 - TargetHeatingCooling
 - TargetHorizontalTilt
 - TargetLockMechanismState
 - TargetPosition
 - TargetRelativeHumidity
 - TargetSecuritySystemState
 - TargetTemperature
 - TargetVerticalTilt
 - TemperatureUnits
 - Version

### <a name="working-with-a-characteristics-value"></a>特徴の値の操作

そのするアプリが特定の特性の最新の状態を呼び出して、`ReadValue`のメソッド、`HMCharacteristic`クラスです。 場合、`err`プロパティは使用されません`null`エラーが発生しました、および、ユーザーに表示されない場合があります。

特性の`Value`プロパティにはとして指定された特性の現在の状態が含まれています、 `NSObject`、よう c# で直接に動作することはできません。

値の読み取り、次のヘルパー クラスに追加されました、 **HomeKitIntro**サンプル アプリケーション。

```csharp
using System;
using Foundation;
using System.Globalization;
using CoreGraphics;

namespace HomeKitIntro
{
    /// <summary>
    /// NS object converter is a helper class that helps to convert NSObjects into
    /// C# objects
    /// </summary>
    public static class NSObjectConverter
    {
        #region Static Methods
        /// <summary>
        /// Converts to an object.
        /// </summary>
        /// <returns>The object.</returns>
        /// <param name="nsO">Ns o.</param>
        /// <param name="targetType">Target type.</param>
        public static Object ToObject (NSObject nsO, Type targetType)
        {
            if (nsO is NSString) {
                return nsO.ToString ();
            }

            if (nsO is NSDate) {
                var nsDate = (NSDate)nsO;
                return DateTime.SpecifyKind ((DateTime)nsDate, DateTimeKind.Unspecified);
            }

            if (nsO is NSDecimalNumber) {
                return decimal.Parse (nsO.ToString (), CultureInfo.InvariantCulture);
            }

            if (nsO is NSNumber) {
                var x = (NSNumber)nsO;

                switch (Type.GetTypeCode (targetType)) {
                case TypeCode.Boolean:
                    return x.BoolValue;
                case TypeCode.Char:
                    return Convert.ToChar (x.ByteValue);
                case TypeCode.SByte:
                    return x.SByteValue;
                case TypeCode.Byte:
                    return x.ByteValue;
                case TypeCode.Int16:
                    return x.Int16Value;
                case TypeCode.UInt16:
                    return x.UInt16Value;
                case TypeCode.Int32:
                    return x.Int32Value;
                case TypeCode.UInt32:
                    return x.UInt32Value;
                case TypeCode.Int64:
                    return x.Int64Value;
                case TypeCode.UInt64:
                    return x.UInt64Value;
                case TypeCode.Single:
                    return x.FloatValue;
                case TypeCode.Double:
                    return x.DoubleValue;
                }
            }

            if (nsO is NSValue) {
                var v = (NSValue)nsO;

                if (targetType == typeof(IntPtr)) {
                    return v.PointerValue;
                }

                if (targetType == typeof(CGSize)) {
                    return v.SizeFValue;
                }

                if (targetType == typeof(CGRect)) {
                    return v.RectangleFValue;
                }

                if (targetType == typeof(CGPoint)) {
                    return v.PointFValue;
                }
            }

            return nsO;
        }

        /// <summary>
        /// Convert to string
        /// </summary>
        /// <returns>The string.</returns>
        /// <param name="nsO">Ns o.</param>
        public static string ToString(NSObject nsO) {
            return (string)ToObject (nsO, typeof(string));
        }

        /// <summary>
        /// Convert to date time
        /// </summary>
        /// <returns>The date time.</returns>
        /// <param name="nsO">Ns o.</param>
        public static DateTime ToDateTime(NSObject nsO){
            return (DateTime)ToObject (nsO, typeof(DateTime));
        }

        /// <summary>
        /// Convert to decimal number
        /// </summary>
        /// <returns>The decimal.</returns>
        /// <param name="nsO">Ns o.</param>
        public static decimal ToDecimal(NSObject nsO){
            return (decimal)ToObject (nsO, typeof(decimal));
        }

        /// <summary>
        /// Convert to boolean
        /// </summary>
        /// <returns><c>true</c>, if bool was toed, <c>false</c> otherwise.</returns>
        /// <param name="nsO">Ns o.</param>
        public static bool ToBool(NSObject nsO){
            return (bool)ToObject (nsO, typeof(bool));
        }

        /// <summary>
        /// Convert to character
        /// </summary>
        /// <returns>The char.</returns>
        /// <param name="nsO">Ns o.</param>
        public static char ToChar(NSObject nsO){
            return (char)ToObject (nsO, typeof(char));
        }

        /// <summary>
        /// Convert to integer
        /// </summary>
        /// <returns>The int.</returns>
        /// <param name="nsO">Ns o.</param>
        public static int ToInt(NSObject nsO){
            return (int)ToObject (nsO, typeof(int));
        }

        /// <summary>
        /// Convert to float
        /// </summary>
        /// <returns>The float.</returns>
        /// <param name="nsO">Ns o.</param>
        public static float ToFloat(NSObject nsO){
            return (float)ToObject (nsO, typeof(float));
        }

        /// <summary>
        /// Converts to double
        /// </summary>
        /// <returns>The double.</returns>
        /// <param name="nsO">Ns o.</param>
        public static double ToDouble(NSObject nsO){
            return (double)ToObject (nsO, typeof(double));
        }
        #endregion
    }
}
```

`NSObjectConverter`アプリケーションは、特性の現在の状態を読み取る必要がある場合に使用します。 たとえば、次のように入力します。

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

上記の行に値を変換します、 `float` Xamarin c# コードで使用しています。

変更する、 `HMCharacteristic`、呼び出すその`WriteValue`メソッドで新しい値をラップし、`NSObject.FromObject`を呼び出します。 たとえば、次のように入力します。

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

場合、`err`プロパティは使用されません`null`エラーが発生し、ユーザーに提示する必要があります。

### <a name="testing-characteristic-value-changes"></a>特徴的な値の変更のテスト

使用するときに`HMCharacteristics`とシミュレートされたアクセサリへの変更、 `Value` HomeKit アクセサリ シミュレーターの内部プロパティを監視することができます。

**HomeKitIntro**実際 iOS デバイスのハードウェア、特徴の値への変更で実行されているアプリ見なす必要がありますほぼ瞬時に HomeKit アクセサリ シミュレーター。 たとえば、iOS アプリ内の明るいの状態の変更。

[![](homekit-images/test01.png "IOS アプリのライトの状態を変更します。")](homekit-images/test01.png#lightbox)

HomeKit アクセサリ シミュレーターのライトの状態を変更する必要があります。 値が変更されない場合は、新しい特徴的な値を書き込むときにエラー メッセージの状態をチェックし、アクセサーが到達可能であることを確認します。

## <a name="advanced-homekit-features"></a>高度な HomeKit 機能

この記事では、Xamarin.iOS アプリで HomeKit アクセサリを操作するために必要な基本的な機能について説明しました。 ただし、HomeKit のこの概要で説明されていないいくつかの高度な機能があります。

- **ルーム**-有効になっている HomeKit アクセサリをエンドユーザーがルームに整理必要に応じて。 これにより、HomeKit を理解して使用するユーザーが容易な方法で存在します。 作成およびルームの管理の詳細については、Apple を参照してください。 [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom)ドキュメント。
- **ゾーン**-エンドユーザー ルームがゾーンに分割する必要に応じてことができます。 ゾーンは、ユーザーが 1 つの単位として扱うことがありますのルームのコレクションを参照します。 例: 上、Downstairs または地下室します。 ここでも、これにより、HomeKit を提示してアクセサリをエンドユーザーに意味のある方法で使用します。 作成およびゾーンの管理の詳細については、Apple を参照してください。 [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone)ドキュメント。
- **アクションおよびアクション設定**-アクションが付属品のサービスの特性を変更し、セットにグループ化することができます。 アクションのセットは、accessories のグループを制御し、その動作を調整するためのスクリプトとして機能します。 たとえば、「テレビ番組を見る」スクリプト可能性があります、ブラインド、dim ライト、閉じ、テレビとそのサウンド システムを有効にします。 作成してアクションおよびアクションのセットを管理する方法の詳細については、Apple を参照してください[HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction)と[HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet)ドキュメント。
- **トリガー** - トリガーは 1 つをアクティブ化することができますか詳細アクション設定時に指定された一連の条件が満たされています。 たとえば、portch 光を有効にし、外暗くときに、すべての外部のドアをロックします。 作成してトリガーを管理する方法の詳細については、Apple を参照してください[HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger)ドキュメント。

これらの機能は、上に示したのと同じ手法を使用するため必要がありますで次の Apple の実装が簡単[HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)、 [HomeKit ユーザー インターフェイス ガイドライン](https://developer.apple.com/homekit/ui-guidelines/)と[HomeKit フレームワーク参照](https://developer.apple.com/library/ios/home_kit_framework_ref)です。

## <a name="homekit-app-review-guidelines"></a>HomeKit アプリ レビューに関するガイドライン

HomeKit を送信する前に、iTunes アプリケーション ストアのリリースの iTunes Connect を有効になっている Xamarin.iOS アプリでは、有効になっている HomeKit アプリ用の Apple のガイドラインに従うことを確認します。

 - 主な目的は、アプリの_必要があります_HomeKit フレームワークを使用する場合は、ホーム オートメーションをします。
 - HomeKit が使用されていること、およびプライバシー ポリシーを提供する必要があります、アプリのマーケティング テキストはユーザーに通知する必要があります。
 - ユーザー情報を収集または広告の HomeKit を使って禁じられています。

完全用ガイドラインを確認して、Apple を参照してください[App Store レビューに関するガイドライン](https://developer.apple.com/app-store/review/guidelines/)です。

## <a name="whats-new-in-ios-9"></a>新機能 iOS 9

Apple がに対して、次の変更と追加 HomeKit iOS 9 の。

- **既存のオブジェクトを保持する**既存アクセサリを変更する場合、ホーム マネージャー - (`HMHomeManager`) の特定の項目が変更されたことを通知します。
- **永続的な Id** -すべての関連する HomeKit クラスが用意されました、 `UniqueIdentifier` HomeKit 間で特定のアイテムを一意に識別するプロパティには、アプリ (または、同じアプリケーションのインスタンス) が有効になっています。
- **ユーザー管理**-プライマリ ユーザーのホームで HomeKit デバイスへのアクセスを持つユーザーを経由でユーザーの管理を提供する組み込みビュー コント ローラーを追加します。
- **ユーザー機能**- HomeKit ユーザー今すぐ HomeKit で使用することはどのような機能を制御する特権のセットがあり、HomeKit アクセサリを有効にします。 アプリでは、現在のユーザーに関連する機能を表示するのみです。 など、管理者だけは、他のユーザーを管理できる必要があります。
- **定義済みのシーン**-平均 HomeKit ユーザーに対して発生する 4 つの一般的なイベントの定義済みのシーンが作成されて: 稼働のままにして、返す、ベッドに移動します。 これらの定義済みのシーンは、自宅から削除できません。
- **シーンと Siri** -iOS 9 および内のシーン HomeKit で定義されている任意のシーンの名前を認識するため、Siri がサポートが強化されました。 ユーザーは、Siri にその名前を話すことによって、単にシーンを実行できます。
- **アクセサリ カテゴリ**の定義済みのカテゴリのセットをすべてのアクセサリおよびホームに追加されている付属品の種類を識別するために役立ちますに追加またはから、アプリ内で作業します。 これらの新しいカテゴリは、付属品のセットアップ中に利用します。
- **Apple Watch サポート**- HomeKit は watchOS に使用できるようになりましたし、Apple Watch がコントロール HomeKit せず、ウォッチ近くする iPhone デバイスを有効にすることができます。 HomeKit watchOS は、次の機能をサポートしています。 表示の家庭に設置、制御するアクセサリおよびシーンの実行。
- **新しいイベント トリガーの種類**- iOS 8、iOS 9 サポート イベント トリガーが (センサー データなど) のアクセサリ状態または地理的位置情報に基づくようになりましたでサポートされるタイマーの種類のトリガー以外。 イベント トリガーを使用して`NSPredicates`でそれらの実行条件を設定します。
- **リモート アクセス**-リモート アクセスでは、ユーザーがコントロールにできるようになりましたリモートの場所に格納されていない場合、その HomeKit がホーム オートメーション アクセサリを有効にします。 IOS 8 のこれのみサポートされていました、ユーザーは、次の 3 番目の世代自宅で Apple TV があった場合。 IOS 9 では、この制限を解除し、iCloud と HomeKit アクセサリ プロトコル (HAP) 経由でリモート アクセスをサポートします。
- **新しい Bluetooth 低電力 (BLE) への対応力**-HomeKit では、Bluetooth 低電力 (BLE) プロトコルを使用して通信できるより多くの付属品の種類をサポートします。 HAP セキュア トンネリングを使用して、HomeKit アクセサリを公開できます他の Bluetooth 付属品 Wi-fi 経由で (Bluetooth 範囲外である) 場合。 Ios 9、BLE アクセサリは通知およびメタデータの完全なサポートがあります。
- **新しいアクセサリ カテゴリ**-Apple iOS 9 で、次の新しいアクセサリ カテゴリを追加する: ウィンドウの表面、電動式のドアとウィンドウ、警報、センサー、およびプログラミング可能なスイッチです。

IOS 9 の HomeKit の新機能に関する詳細については、Apple を参照してください[HomeKit インデックス](https://developer.apple.com/homekit/)と[HomeKit 新](https://developer.apple.com/videos/wwdc/2015/?id=210)ビデオです。

## <a name="summary"></a>まとめ

この記事には、Apple の HomeKit ホーム オートメーション フレームワークが導入されました。 これは、検出との通信および HomeKit を使用してホーム オートメーション デバイスを制御する単純な Xamarin.iOS アプリを作成する方法と、セットアップして HomeKit アクセサリ シミュレーターを使用してテスト デバイスを構成する方法を示しました。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [新機能 iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit ユーザー インターフェイスのガイドライン](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Framework リファレンス](https://developer.apple.com/library/ios/home_kit_framework_ref)
