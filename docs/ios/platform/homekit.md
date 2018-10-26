---
title: Xamarin.iOS で HomeKit
description: HomeKit は、ホーム オートメーション デバイスを制御するための Apple のフレームワークです。 この記事では、HomeKit を紹介し、構成のテスト アクセサリ アクセサリと対話する HomeKit アクセサリ シミュレーターと単純な Xamarin.iOS アプリの作成について説明します。
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 22b6fd101c0b983fe7b4a7d0891dc4674a11a02a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121192"
---
# <a name="homekit-in-xamarinios"></a>Xamarin.iOS で HomeKit

_HomeKit は、ホーム オートメーション デバイスを制御するための Apple のフレームワークです。この記事では、HomeKit を紹介し、構成のテスト アクセサリ アクセサリと対話する HomeKit アクセサリ シミュレーターと単純な Xamarin.iOS アプリの作成について説明します。_

[![](homekit-images/accessory01.png "HomeKit の使用例には、アプリが有効になっています。")](homekit-images/accessory01.png#lightbox)

Apple は、さまざまなベンダーから複数のホーム オートメーション デバイスを一貫した単一のユニットにシームレスに統合する方法として、iOS 8 で HomeKit を導入しました。 検出するための一般的なプロトコルを昇格させることにより、ホーム オートメーション デバイスの構成と HomeKit、デバイスから関連以外のベンダーは、各ベンダーの作業を調整することがなく、連携できます。

HomeKit、Api やアプリを提供するベンダーを使用せずに、HomeKit を有効になっているデバイスを制御する Xamarin.iOS アプリを作成できます。 HomeKit では、次の操作を行うことができます。

- 新しい HomeKit を有効になっているホーム オートメーション デバイスを検出し、すべてのユーザーの iOS デバイスの間で保持されるデータベースに追加します。
- セットアップ、構成、表示、および、HomeKit の任意のデバイスを制御_構成データベース ホーム_します。
- 事前に構成された HomeKit デバイスと通信し、コマンドに個々 のアクションを実行したり、キッチンのライトのすべての有効化などの連携します。

HomeKit を有効になっているアプリにホーム構成データベース内のデバイスの機能、に加えて HomeKit は Siri 音声コマンドへのアクセスを提供します。 適切に構成された、HomeKit のセットアップを指定するには、ユーザー音声コマンドを実行できるよう"Siri、リビング ルームのライトを有効にします"。

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>ホームの構成データベース

HomeKit をホーム コレクションに指定された場所にすべてのオートメーション デバイスを整理します。 このコレクションは、ユーザーが自分のホーム オートメーション デバイスを意味のある、人間が判読できるラベルを論理的に配置後の単位にグループ化するための方法を提供します。

ホームの構成データベースで自動的にバックアップと同期されたすべてのユーザーの iOS デバイスのホームのコレクションが格納されます。 HomeKit では、構成データベースを操作するため、次のクラスを提供します。

- `HMHome` -これが最上位のコンテナーで 1 つの物理的な場所 (例: すべての情報とすべてのホーム オートメーション デバイス用の構成を保持します。 1 つのファミリの居住地)。 ユーザーのメイン ホームおよび休暇家などの 1 つ以上の住所があります。 または、さまざまな「家」で同じプロパティをメインの家やガレージの上のゲスト家などがあります。 いずれにしても、少なくとも 1 つ`HMHome`オブジェクト_する必要があります_セットアップし、その他 HomeKit の情報を入力する前に格納します。
- `HMRoom` -While 省略可能で、`HMRoom`を家庭内の特定のルームを定義できます (`HMHome`) など: キッチン、バスルーム、ガレージまたはリビング ルーム。 ユーザーがグループのすべてのホーム オートメーション デバイスに自分の家で特定の場所に、`HMRoom`単位として操作とします。 たとえば、ガレージ ライトをオフにするための Siri を求めています。
- `HMAccessory` -これは、個人を表します、物理 HomeKit には、オートメーション デバイス (スマート サーモスタット) など、ユーザーの居住地でインストールされているが有効になります。 各`HMAccessory`に割り当てられている、`HMRoom`します。 場合は、ユーザーが構成されていないすべてのルーム、HomeKit アクセサリ特別な既定のルームに割り当てます。
- `HMService` -によって提供されるサービスを表す、指定された`HMAccessory`、ライトまたは (色の変更がサポートされている) 場合は、その色のオン/オフ状態など。 各`HMAccessory`ライトも含むガレージきっかけなど、複数のサービスを持つことができます。 さらを指定した`HMAccessory`ユーザー コントロール外にあるサービス、ファームウェアの更新などがあります。
- `HMZone` -のコレクションをグループ化するユーザーを許可する`HMRoom`上、Downstairs 地下室などの論理ゾーン オブジェクト。 必須ではありません、これにより、Siri を求めるような相互作用オフ downstairs 光のすべてが有効にします。

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>HomeKit のアプリのプロビジョニング

HomeKit によるセキュリティ要件、により HomeKit フレームワークを使用する Xamarin.iOS アプリをする必要があります適切に構成する、Apple Developer ポータルと Xamarin.iOS プロジェクト ファイル。

次の手順で行います。

1. ログイン、 [Apple Developer Portal](http://developer.apple.com)します。
2. をクリックして**証明書, Identifiers & Profiles**します。
3. これをいない場合は、をクリックして**識別子**アプリの ID を作成し、(例: `com.company.appname`)、それ以外の場合、既存の ID を編集
4. いることを確認、 **HomeKit**サービスは、指定した ID のチェックが完了します。 

    [![](homekit-images/provision01.png "指定した ID の HomeKit のサービスを有効にします。")](homekit-images/provision01.png#lightbox)
5. 変更内容を保存します。
4. をクリックして**プロビジョニング プロファイル** > **開発**し、新しい開発プロビジョニング プロファイル、アプリを作成します。 

    [![](homekit-images/provision02.png "新しい開発プロビジョニング プロファイル、アプリの作成します。")](homekit-images/provision02.png#lightbox)
5. ダウンロードして、新しいプロビジョニング プロファイルをインストールするまたは、Xcode を使用してダウンロードし、プロファイルをインストールしています。
6. Xamarin.iOS プロジェクトのオプションを編集し、先ほど作成したプロビジョニング プロファイルを使用していることを確認します。 

    [![](homekit-images/provision03.png "先ほど作成したプロビジョニング プロファイルを選択します。")](homekit-images/provision03.png#lightbox)
7. 次に、編集、 **Info.plist**ファイルし、プロビジョニング プロファイルの作成に使用されたアプリ ID を使用していることを確認します。 

    [![](homekit-images/provision04.png "アプリ ID を設定します。 ")](homekit-images/provision04.png#lightbox)
8. 最後に、編集、 **Entitlements.plist**ファイルし、いることを確認、 **HomeKit**権利が選択されています。 

    [![](homekit-images/provision05.png "HomeKit の利用資格を有効にします。")](homekit-images/provision05.png#lightbox)
9. すべてのファイルに変更を保存します。

これら設定した状態で、アプリケーションは、HomeKit フレームワークの Api にアクセスする準備がようになりました。 プロビジョニングの詳細についてを参照してください、 [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)と[アプリのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)ガイド。

> [!IMPORTANT]
> HomeKit を有効になっているアプリのテストと開発が正しくプロビジョニングされている実際の iOS デバイスが必要です。 IOS シミュレーターでは、HomeKit をテストすることはできません。

## <a name="the-homekit-accessory-simulator"></a>HomeKit アクセサリ シミュレーター

Apple が作成しなくても、物理デバイスがあるすべての可能なホーム オートメーション デバイスとサービスをテストする方法を提供する、 _HomeKit アクセサリ シミュレーター_します。 このシミュレーターを使用して、セットアップおよび、HomeKit の仮想デバイスを構成できます。

### <a name="installing-the-simulator"></a>シミュレーターをインストールします。

Apple は、続行する前にインストールする必要があります、HomeKit アクセサリ シミュレーターを個別のダウンロードとして、Xcode から提供します。

次の手順で行います。

1. Web ブラウザーで、次を参照してください[Apple の開発者向けダウンロード。](https://developer.apple.com/download/more/?name=for%20Xcode)
2. ダウンロード、 **Xcode xxx の他のツール**(xxx はインストールされている Xcode のバージョンです)。 

    [![](homekit-images/simulator01.png "Xcode の他のツールをダウンロードします。")](homekit-images/simulator01.png#lightbox)
3. ディスク イメージを開き、ツールをインストール、**アプリケーション**ディレクトリ。

HomeKit アクセサリ シミュレータをインストールする、テスト用仮想 [アクセサリ] を作成できます。

### <a name="creating-virtual-accessories"></a>仮想アクセサリを作成します。

HomeKit アクセサリ シミュレーターを開始し、いくつかの仮想アクセサリを作成するには、次の操作を行います。

1. [アプリケーション] フォルダーには、HomeKit アクセサリ シミュレーターを開始します。 

    [![](homekit-images/simulator02.png "HomeKit アクセサリ シミュレーター")](homekit-images/simulator02.png#lightbox)
2. をクリックして、 **+** ボタンをクリックし、選択**新しいアクセサリ.**: 

    [![](homekit-images/simulator03.png "新しいアクセサリを追加します。")](homekit-images/simulator03.png#lightbox)
3. 新しいアクセサリについての情報を入力し、クリックして、**完了**ボタンをクリックします。 

    [![](homekit-images/simulator04.png "新しいアクセサリについての情報を入力します")](homekit-images/simulator04.png#lightbox)
4. をクリックして、**サービスを追加する.** ボタンをクリックし、ドロップダウン リストからサービスの種類を選択します。 

    [![](homekit-images/simulator05.png "ドロップダウン リストからサービスの種類を選択します。")](homekit-images/simulator05.png#lightbox)
5. 提供、**名前**サービスをクリックして、**完了**ボタン。 

    [![](homekit-images/simulator06.png "サービスの名前を入力します")](homekit-images/simulator06.png#lightbox)
6. クリックして、サービスのオプションの特性を行うことができます、**追加特性**ボタンをクリックし、必要な設定を構成します。 

    [![](homekit-images/simulator07.png "必要な設定を構成します。")](homekit-images/simulator07.png#lightbox)
7. HomeKit をサポートする仮想ホーム オートメーション デバイスの種類ごとの 1 つを作成する前の手順を繰り返します。

いくつかサンプル仮想 HomeKit アクセサリ作成し、構成、今すぐ使用し、Xamarin.iOS アプリからこれらのデバイスを制御できます。

## <a name="configuring-the-infoplist-file"></a>Info.plist ファイルを構成します。

開発者は、追加する必要があります iOS 10 の新機能 (および大きい)、`NSHomeKitUsageDescription`アプリのキー`Info.plist`ファイルを開き、アプリがユーザーの HomeKit のデータベースにアクセスしようとした理由宣言文字列を提供します。 この文字列は、ユーザーが初めてにアプリを実行するときに表示されます。

[![](homekit-images/info01.png "HomeKit のアクセス許可ダイアログ")](homekit-images/info01.png#lightbox)

このキーを設定するには、次の操作を行います。

1. ダブルクリックして、`Info.plist`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。
2. 画面の下部に切り替えて、**ソース**ビュー。
3. 新しい追加**エントリ**一覧にします。
4. ドロップダウン リストから選択**プライバシー - HomeKit の利用状況の説明**: 

    [![](homekit-images/info02.png "プライバシー - HomeKit の利用状況の説明を選択します")](homekit-images/info02.png#lightbox)
5. アプリがユーザーの HomeKit のデータベースにアクセスしようとした理由の説明を入力します。 

    [![](homekit-images/info03.png "説明を入力します")](homekit-images/info03.png#lightbox)
6. 変更内容をファイルに保存します。

> [!IMPORTANT]
> 設定に失敗した、`NSHomeKitUsageDescription`キー、`Info.plist`ファイルの場合は、アプリ_サイレント モードで失敗した_(実行時にシステムによって閉じられている) iOS 10 (以降) で実行するとエラーは発生しません。

## <a name="connecting-to-homekit"></a>HomeKit への接続

Xamarin.iOS アプリを HomeKit を通信には、最初のインスタンスをインスタンス化する必要があります、`HMHomeManager`クラス。 ホーム Manager HomeKit へのサーバーの全体のエントリ ポイントし、使用可能な自宅の一覧を提供する責任は、更新およびそのリストを維持し、登録済みユーザーの_プライマリ ホーム_します。

`HMHome`オブジェクトには、ルーム、グループ、またはインストールされている任意のホーム オートメーション アクセサリと共に、それが含む可能性のあるゾーンを含む付与ホームに関するすべての情報が含まれています。 HomeKit を少なくとも 1 つのすべての操作を実行する前に、`HMHome`作成し、プライマリ ホームとして割り当てる必要があります。

アプリは、作成とそうでない場合は、1 つを割り当てるプライマリ ホームが存在するかどうかを確認します。

### <a name="adding-a-home-manager"></a>ホーム マネージャーを追加します。

Xamarin.iOS アプリには、HomeKit の認識を追加するには、編集、 **AppDelegate.cs**ファイルを編集し、次のようになります。

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

ユーザーが回答する場合 **[ok]** アプリケーションは、HomeKit アクセサリを使用できるし、それ以外の場合にないと HomeKit への呼び出しがエラーで失敗します。

インプレース マネージャーがホーム次に、アプリケーションは必要がありますをプライマリ ホームが構成されている場合に表示し、そうでない場合は、ユーザーを作成して割り当てる 1 つの方法を提供します。

### <a name="accessing-the-primary-home"></a>プライマリ ホームにアクセスします。

前述のように、プライマリ ホームを作成および HomeKit があるし、ユーザーを作成して割り当てる場合は、1 つのプライマリ ホームの手段を提供するアプリの責任がまだ存在しないことをお勧めする前に構成する必要があります。

監視する必要がありますが、アプリが初めて起動したり、バック グラウンドから返します、ときに、`DidUpdateHomes`のイベント、`HMHomeManager`プライマリ ホームの存在を確認するクラス。 1 つが存在しない場合、ユーザーを作成する 1 つのインターフェイスを提供する必要があります。

次のコードは、プライマリ ホームをチェックするビュー コント ローラーに追加できます。

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

ホーム マネージャーが接続すると、HomeKit ときに、`DidUpdateHomes`発生するイベント、住宅のマネージャーのコレクションに読み込まれる任意の既存の家庭および使用可能な場合、プライマリ ホームが読み込まれます。

### <a name="adding-a-primary-home"></a>プライマリ ホームを追加します。

場合、`PrimaryHome`のプロパティ、`HMHomeManager`は`null`後、`DidUpdateHomes`イベント、ユーザーを作成して続行する前にプライマリ ホームを割り当てる方法を提供する必要があります。

通常、アプリは、ユーザー ホーム マネージャー プライマリ ホームとしてセットアップに渡されるを取得するための新しいホームに名前を付けるためのフォームに表示されます。 **HomeKitIntro**サンプル アプリでは、モーダル ビューが IOS Designer で作成およびメソッドを呼び出して、`AddHomeSegue`セグエ、アプリのメイン インターフェイスから。

ユーザーが新しいホームと、ホームを追加するボタンの名前を入力するためのテキスト フィールドを提供します。 ユーザーがタップしたときに、**追加ホーム**ボタン、次のコードは、ホームを追加するホーム マネージャー。

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

`AddHome`メソッドは新しいホームを作成し、指定されたコールバック ルーチンに返すことを試みます。 場合、`error`プロパティは`null`エラーが発生しました、これは、ユーザーに提示する必要があります。 最も一般的なエラーには、一意でないホーム名または HomeKit と通信できないホーム マネージャーのいずれかが原因です。

ホームが正常に作成された場合は、呼び出す必要があります。、`UpdatePrimaryHome`プライマリ ホームに新しいホームを設定します。 ここでも場合、`error`プロパティは`null`エラーが発生しました、これは、ユーザーに提示する必要があります。

ホーム マネージャーを監視することも必要があります。`DidAddHome`と`DidRemoveHome`必要に応じて、アプリのユーザー インターフェイスのイベントおよび更新します。

> [!IMPORTANT]
> `AlertView.PresentOKAlert`上記のサンプル コードで使用されるメソッドが操作できるように iOS アラート簡単 HomeKitIntro アプリケーションでヘルパー クラス。


## <a name="finding-new-accessories"></a>新しい [アクセサリ] の検索

プライマリ ホームが定義されているかホーム マネージャーから読み込まれると、Xamarin.iOS アプリを呼び出すことができます、`HMAccessoryBrowser`を任意の新しいホーム オートメーション アクセサリを見つけて、それらをホームに追加します。

呼び出す、`StartSearchingForNewAccessories`新しい [アクセサリ] の検索を開始するメソッドと`StopSearchingForNewAccessories`メソッドが完了します。

> [!IMPORTANT]
> `StartSearchingForNewAccessories` ままにしないで長期間実行されているため、バッテリの寿命と iOS デバイスのパフォーマンスが低下します。 Apple が呼び出し元を示す`StopSearchingForNewAccessories`分や付属品の検索 UI がユーザーに表示された場合にのみ検索します。

`DidFindNewAccessory`イベントが呼び出されますが、新しい [アクセサリ] の検出し、に追加されます、`DiscoveredAccessories`アクセサリ ブラウザーの一覧。

`DiscoveredAccessories`一覧のコレクションが含まれます`HMAccessory`付与 HomeKit を定義するオブジェクトには、ホーム オートメーション デバイスとライトやガレージ ドア コントロールなど、使用可能なサービスが有効になっています。

新しいアクセサリが見つかった後のユーザーに表示と選択して、ホームに追加するため。 例:

[![](homekit-images/accessory01.png "新しいアクセサリの検索")](homekit-images/accessory01.png#lightbox)

呼び出す、`AddAccessory`ホームのコレクションを選択したアクセサリを追加するメソッド。 例えば:

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

場合、`err`プロパティは`null`エラーが発生しました、これは、ユーザーに提示する必要があります。 それ以外の場合、ユーザーを追加するデバイスのセットアップ コードの入力を求められます。

[![](homekit-images/accessory02.png "追加するデバイスのセットアップ コードを入力します。")](homekit-images/accessory02.png#lightbox)

この番号は記載されて、HomeKit アクセサリ シミュレーターで、**セットアップ コード**フィールド。

[![](homekit-images/accessory03.png "HomeKit アクセサリ シミュレーターでのセットアップ コード フィールド")](homekit-images/accessory03.png#lightbox)

実際の HomeKit アクセサリ セットアップ コードは、デバイス自体で、製品の箱にまたはアクセサリのユーザーによる手動ラベルに印刷か。

アクセサリのブラウザーを監視する必要があります`DidRemoveNewAccessory`イベントと更新プログラム、ユーザーは、ユーザーが追加の"ホーム"コレクションになった後、一覧の利用可能なアクセサリを削除するインターフェイスします。

## <a name="working-with-accessories"></a>[アクセサリ] の操作

プライマリ ホーム 1 回も確立し、[アクセサリ] が追加されてを使用するユーザーの [アクセサリ] (および必要に応じてルーム) の一覧を表示することができます。

`HMRoom`オブジェクトには、特定の部屋に関するすべての情報とそれに属する任意のアクセサリが含まれています。 ルームは、1 つまたは複数のゾーンに必要に応じて整理できます。 A`HMZone`特定のゾーンのすべての情報とそれに属するルームのすべてが含まれています。

この例でを保存するモ ノ シンプルかつルームまたはゾーンに編成ではなく自宅のアクセサリを直接と連携します。

`HMHome`オブジェクトでユーザーに表示することが割り当てられた付属品の一覧が含まれています。 その`Accessories`プロパティ。 例えば:

[![](homekit-images/accessory04.png "例アクセサリ")](homekit-images/accessory04.png#lightbox)

ここでフォームで、ユーザーは特定のアクセサリを選択して、これが提供するサービスを使用します。

## <a name="working-with-services"></a>サービスの使用

ユーザーが対話 HomeKit ホーム オートメーションを有効になっている、特定のデバイスに場合、これは通常が提供するサービスを使用します。 `Services`のプロパティ、`HMAccessory`クラスのコレクションを格納する`HMService`サービスを定義するオブジェクト、デバイスは提供します。

サービスは、ライト、サーモスタット、ガレージ ドアを開ける装置、スイッチ、またはロックのようなものです。 (ガレージきっかけ) のような一部のデバイスは、光、ドアの開閉する機能など、複数のサービスを提供します。

各アクセサリを含む特定のアクセサリを提供する特定のサービスに加え、`Information Service`名、製造元、モデルのシリアル番号などのプロパティを定義します。

### <a name="accessory-service-types"></a>アクセサリのサービスの種類

次のサービスの種類は経由で使用できる、`HMServiceType`列挙型。

- **AccessoryInformation** -指定したホーム オートメーション デバイス (アクセサリ) に関する情報を提供します。
- **AirQualitySensor** -空気の品質センサーを定義します。
- **バッテリ**-アクセサリのバッテリの状態を定義します。
- **CarbonDioxideSensor** -カーボン二酸化炭素センサーを定義します。
- **CarbonMonoxideSensor** -一酸化炭素センサーを定義します。
- **ContactSensor** -連絡先のセンサー (開かれたり閉じられたりするウィンドウ) などを定義します。
- **ドア**-ドアの状態のセンサー (開かれたり閉じられたりなど) を定義します。
- **ファン**-リモート コントロール ファンを定義します。
- **GarageDoorOpener** -ガレージのきっかけを定義します。
- **HumiditySensor** ・湿度センサーを定義します。
- **LeakSensor** -リーク センサー (給湯または洗濯機のように) を定義します。
- **LightBulb** -スタンドアロン光または (ガレージきっかけ) などの他の付属品の一部であるライトを定義します。
- **LightSensor** -光センサーを定義します。
- **LockManagement** -自動ドアのロックを管理するサービスを定義します。
- **LockMechanism** -(ドアのロック) のような場合は、リモート制御されたロックを定義します。
- **MotionSensor** -モーション センサーを定義します。
- **OccupancySensor** -占有率、センサーを定義します。
- **アウトレット**-リモート制御されたコンセントを定義します。
- **SecuritySystem** -自宅のセキュリティ システムを定義します。
- **StatefulProgrammableSwitch** -(フリップ スイッチ) のような 1 回トリガーできるように状態を維持するプログラミング可能なスイッチを定義します。
- **StatelessProgrammableSwitch** -トリガー (プッシュ ボタン) のように後を初期状態を返すプログラミング可能なスイッチを定義します。
- **SmokeSensor** -煙のセンサーを定義します。
- **切り替える**-標準壁スイッチなどのオン/オフ スイッチを定義します。
- **温度センサー**の温度センサーを定義します。
- **サーモスタット**-スマート サーモスタット、HVAC システムを制御するために使用を定義します。
- **ウィンドウ**-cane の開かれたり閉じられたりするリモートでの自動ウィンドウを定義します。
- **WindowCovering** -リモートで制御されるウィンドウをカバーする、開くまたは閉じることができますをブラインドのように定義します。

### <a name="displaying-service-information"></a>サービス情報を表示します。

読み込み後、`HMAccessory`個々 のクエリを実行できる`HNService`オブジェクトを提供し、ユーザーにその情報を表示。

[![](homekit-images/accessory05.png "サービス情報を表示します。")](homekit-images/accessory05.png#lightbox)

必ず確認する必要があります、`Reachable`のプロパティを`HMAccessory`前にそれを処理します。 アクセサリには、到達できないユーザーがいないデバイスの範囲内、またはかどうかに接続されていないことを指定できます。

サービスを選択すると、ユーザーが表示またはそのサービスを監視または指定したホーム オートメーション デバイスの制御の 1 つまたは複数の特性を変更できます。

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>特性の操作

各`HMService`オブジェクトのコレクションに格納できる`HMCharacteristic`(開かれたり閉じられたりするドア) のようなサービスの状態に関する情報を提供するか (ライトの色を設定する) などの状態を調整するユーザーを許可するオブジェクト。

`HMCharacteristic` だけでなく、特性とその状態に関する情報を提供しますが、使用して状態を操作するためのメソッドも提供_特性メタデータ_(`HMCharacteristisMetadata`)。 このメタデータは、状態を変更するユーザーまたはように情報を表示する場合に便利ですが (最小と最大値の範囲) などのプロパティを提供できます。

`HMCharacteristicType`列挙型が定義されているまたは次のように変更できる特性のメタデータ値のセットを提供します。

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
 - 名前
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
 - [彩度]
 - シリアル番号
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

アプリを特定の特性の最新の状態を持つようにするため、呼び出し、`ReadValue`のメソッド、`HMCharacteristic`クラス。 場合、`err`プロパティは`null`エラーが発生しました、および、ユーザーに表示されない場合があります。

特性の`Value`プロパティには、特定の特性としての現在の状態が含まれています、 `NSObject`、ようできませんしたで直接、C#します。

値を読み取るには、次のヘルパー クラスに追加された、 **HomeKitIntro**サンプル アプリケーション。

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

`NSObjectConverter`アプリケーションは、特性の現在の状態を読み取る必要があるたびに使用されます。 たとえば、次のように入力します。

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

上記の行に値を変換する、 `float` 、Xamarin で使用し、C#コード。

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

場合、`err`プロパティは`null`エラーが発生し、ユーザーに提示する必要があります。

### <a name="testing-characteristic-value-changes"></a>特性の値の変更のテスト

使用する場合`HMCharacteristics`とシミュレートされた [アクセサリ]、変更、 `Value` HomeKit アクセサリ シミュレーター内でプロパティを監視できます。

**HomeKitIntro** HomeKit アクセサリのシミュレーターで実際の iOS デバイスのハードウェア特性の値の変更で実行されているアプリをほぼ瞬時に確認する必要があります。 たとえば、iOS アプリでのライトの状態の変更。

[![](homekit-images/test01.png "IOS アプリでのライトの状態を変更します。")](homekit-images/test01.png#lightbox)

HomeKit アクセサリのシミュレーターでのライトの状態を変更する必要があります。 値が変更されない場合は、特性の新しい値を書き込むときに、エラー メッセージの状態を確認し、アクセサーが到達可能であることを確認します。

## <a name="advanced-homekit-features"></a>HomeKit の高度な機能

この記事では、Xamarin.iOS アプリで HomeKit アクセサリを操作するために必要な基本的な機能について説明しました。 ただし、HomeKit の概要で取り上げられていないいくつかの高度な機能があります。

- **ルーム**-有効になっている HomeKit アクセサリが、エンドユーザーがルームに整理できます必要に応じて。 これにより、ユーザーの理解し、作業を簡単な方法である [アクセサリ] を HomeKit です。 作成して、ルームを管理する方法の詳細については、Apple を参照してください[HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom)ドキュメント。
- **ゾーン**-エンドユーザーがゾーンにルームが整理必要に応じてできます。 ゾーンは、ユーザーが 1 つの単位として扱うことがありますルームのコレクションを表します。 例: 上、Downstairs または地下室します。 ここでも、これにより、HomeKit 存在し、[アクセサリ]、エンドユーザーにとって意味のある方法で使用します。 作成して、ゾーンを管理する方法の詳細については、Apple を参照してください[HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone)ドキュメント。
- **アクションおよびアクション設定**-アクションは、アクセサリのサービスの特性を変更して、セットにグループ化することができます。 アクションのセットは、[アクセサリ] のグループを制御し、そのアクションを調整するためのスクリプトとして機能します。 たとえば、「テレビ番組を見る」スクリプト可能性があります、ブラインド、dim ライト、閉じ、テレビとそのサウンド システムを有効にします。 作成して、アクションとアクションのセットを維持する方法の詳細については、Apple を参照してください[HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction)と[HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet)ドキュメント。
- **トリガー** - いずれかのトリガーをアクティブ化または詳細アクション設定時に指定された一連の条件を満たしています。 など、portch 光を有効にし、外暗くときに、すべての外部のドアをロックします。 作成してトリガーを管理する方法の詳細については、Apple を参照してください[HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger)ドキュメント。

これらの機能は、上記と同じ手法を使用するため必要がある次の apple の実装が簡単[HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)、 [HomeKit のユーザー インターフェイス ガイドライン](https://developer.apple.com/homekit/ui-guidelines/)と[HomeKit フレームワーク参照](https://developer.apple.com/library/ios/home_kit_framework_ref)します。

## <a name="homekit-app-review-guidelines"></a>HomeKit アプリ レビューに関するガイドライン

ITunes Connect の iTunes App Store にリリースする Xamarin.iOS アプリを有効になっているは、HomeKit を送信する前に、HomeKit を有効になっているアプリの Apple のガイドラインに従うことを確認します。

 - アプリの主な目的_する必要があります_HomeKit フレームワークを使用する場合は、ホーム オートメーションをします。
 - HomeKit が使用されていること、およびプライバシー ポリシーを指定する必要があります、アプリのマーケティングのテキストはユーザーに通知する必要があります。
 - ユーザー情報を収集または広告のための HomeKit の使用は固く禁止されています。

完全なガイドラインを確認して、Apple を参照してください[App Store レビューに関するガイドライン](https://developer.apple.com/app-store/review/guidelines/)します。

## <a name="whats-new-in-ios-9"></a>IOS 9 で新します。

Apple が行われて、次の変更と追加 HomeKit を iOS 9。

- **既存のオブジェクトを保持**- 既存の付属品が変更された場合、ホーム マネージャー (`HMHomeManager`) が変更された特定の項目を通知します。
- **永続的な識別子**-HomeKit のすべての関連するクラスが追加されました、 `UniqueIdentifier` HomeKit の間で特定の項目を一意に識別するプロパティには、アプリ (または同じアプリのインスタンス) が有効になっています。
- **ユーザー管理**-プライマリ ユーザーのホームで HomeKit デバイスへのアクセスを持つユーザーを経由でユーザーの管理を提供する組み込みビュー コント ローラーを追加します。
- **ユーザー機能**- HomeKit のユーザーは、HomeKit で使用することはどのような機能を制御する特権のセットを今すぐになり、HomeKit アクセサリを有効にします。 アプリケーションでは、現在のユーザーに関連する機能を表示する必要がありますのみ。 など、管理者だけは、他のユーザーを管理できる必要があります。
- **定義済みのシーン**-平均 HomeKit ユーザーに対して発生する次の 4 つの一般的なイベント用に定義済みのシーンが作成されました: 起動、ままにして、返す、ベッドに移動します。 これらの定義済みのシーンは、自宅から削除できません。
- **シーンと Siri** -iOS 9 を内のシーン HomeKit で定義されている任意のシーンの名前を識別するため、Siri がサポートが強化されました。 ユーザーは、Siri にその名前を言うとするだけでシーンを実行できます。
- **アクセサリ カテゴリ**-定義済みのカテゴリのセットをすべてについて、Accessories およびホームに追加されるアクセサリの種類を識別するのに役立ちますに追加または、アプリ内から作業します。 これらの新しいカテゴリは、付属品のセットアップ時に使用できます。
- **Apple Watch サポート**- HomeKit は watchOS 可能になりましたし、Apple Watch が HomeKit がウォッチの近くにいる iPhone しないでデバイスを有効にすることになります。 WatchOS 向け HomeKit は、次の機能をサポートしています: 元の表示、アクセサリの制御およびシーンを実行します。
- **新しいイベント トリガーの種類**- に加えて、iOS 8、iOS 9 をサポートしています (センサー データなど) のアクセサリ状態または地理的位置情報にイベント トリガーがベースに変更されましたでサポートされているタイマーの種類のトリガー。 イベント トリガーを使用して、`NSPredicates`でそれらの実行条件を設定します。
- **リモート アクセス**-リモート アクセスと、ユーザーが制御できるようになりました、HomeKit は、リモートの場所で家から離れているときに、ホーム オートメーション アクセサリを有効になっています。 IOS 8 でこれがのみサポートされます、ユーザーは、次の第 3 世代自宅で Apple TV がある場合。 IOS 9 では、この制限は解除し、iCloud と HomeKit アクセサリ プロトコル (HAP) を使用してリモート アクセスをサポートします。
- **Bluetooth Low Energy (BLE) の新機能**-HomeKit よう Bluetooth Low Energy (BLE) プロトコル経由で通信できるより多くの付属品の種類になりました。 HAP セキュア トンネリングを使用して、HomeKit アクセサリを公開できます別の Bluetooth アクセサリ Wi-fi 経由で (Bluetooth の範囲外の場合)。 Ios 9 で BLE アクセサリは通知およびメタデータの完全なサポートがあります。
- **新しいアクセサリ カテゴリ**-Apple iOS 9 で、次の新しいアクセサリ カテゴリを追加する: 窓枠、電動式のドアと Windows、警報、センサー、およびプログラミング可能なスイッチです。

IOS 9 で HomeKit の新機能の詳細については、Apple を参照してください[HomeKit インデックス](https://developer.apple.com/homekit/)と[HomeKit で新](https://developer.apple.com/videos/wwdc/2015/?id=210)ビデオ。

## <a name="summary"></a>まとめ

この記事には、Apple の HomeKit ホーム オートメーション フレームワークが導入されています。 セットアップおよび HomeKit アクセサリ シミュレーターを使用してテスト デバイスを構成する方法と検出との通信および HomeKit を使用して、ホーム オートメーション デバイスを制御する単純な Xamarin.iOS アプリを作成する方法を示しました。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 を新します。](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [HomeKit のユーザー インターフェイス ガイドライン](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit フレームワーク参照](https://developer.apple.com/library/ios/home_kit_framework_ref)
