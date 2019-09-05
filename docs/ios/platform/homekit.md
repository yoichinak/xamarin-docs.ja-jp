---
title: Xamarin. iOS のホームキット
description: ホームキットは、自宅のオートメーションデバイスを制御するための Apple のフレームワークです。 この記事では、ホームキットと、ホームキットアクセサリシミュレーターでのテストアクセサリの構成、およびこれらのアクセサリと対話するための簡単な Xamarin iOS アプリの作成について説明します。
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: f98cd3110719827d8cfeceef4dc9e73776c79f3f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292726"
---
# <a name="homekit-in-xamarinios"></a>Xamarin. iOS のホームキット

_ホームキットは、自宅のオートメーションデバイスを制御するための Apple のフレームワークです。この記事では、ホームキットと、ホームキットアクセサリシミュレーターでのテストアクセサリの構成、およびこれらのアクセサリと対話するための簡単な Xamarin iOS アプリの作成について説明します。_

[![](homekit-images/accessory01.png "ホームキットが有効になっているアプリの例")](homekit-images/accessory01.png#lightbox)

Apple では、さまざまなベンダーの複数のホームオートメーションデバイスを1つの一貫した単位にシームレスに統合する方法として、iOS 8 のホームキットを導入しました。 Home Kit では、ホームオートメーションデバイスの検出、構成、制御のために共通のプロトコルを昇格させることにより、関連のないベンダーのデバイスを連携させることができます。これにより、個々のベンダーが作業をコーディネートする必要がなくなります。

ホームキットを使用すると、ベンダーが提供する Api またはアプリを使用せずに、任意のホームキット対応デバイスを制御する Xamarin iOS アプリを作成できます。 ホームキットを使用すると、次の操作を実行できます。

- 新しいホームキットが有効になっているホームオートメーションデバイスを検出し、すべてのユーザーの iOS デバイスにわたって保持されるデータベースに追加します。
- _ホームキットホーム構成データベース_で任意のデバイスをセットアップ、構成、表示、および制御します。
- 事前に構成されたすべてのホームキットデバイスと通信し、キッチンのすべてのライトをオンにするなど、個々のアクションを実行したり、連携したりすることができます。

ホームキットが有効になっているアプリにホーム構成データベースのデバイスを提供するだけでなく、Home Kit は Siri 音声コマンドにアクセスできるようにします。 適切に構成されたホームキットのセットアップでは、ユーザーは "Siri, 生きた部屋のライトをオンにする" などの音声コマンドを発行できます。

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>ホーム構成データベース

ホームキットでは、特定の場所にあるすべてのオートメーションデバイスがホームコレクションにまとめられます。 このコレクションを使用すると、ユーザーがホームオートメーションデバイスをグループ化して、わかりやすいわかりやすいラベルを付けることができます。

ホームコレクションは、すべてのユーザーの iOS デバイスで自動的にバックアップされ、同期されるホーム構成データベースに格納されます。 ホームキットには、ホーム構成データベースを操作するための次のクラスが用意されています。

- `HMHome`-これは、1つの物理的な場所にあるすべてのホームオートメーションデバイスのすべての情報と構成を保持する最上位のコンテナーです (例: 1つの家族の住居)。 ユーザーには、自宅や休暇の家など、複数の住居が存在する場合があります。 または、主家やガレージのゲスト家など、同じプロパティに異なる "家" が存在する場合もあります。 どちらの方法でも、 `HMHome`他のすべてのホームキット情報を入力する前に、少なくとも1つのオブジェクトを設定して保存_する必要があり_ます。
- `HMRoom`-オプションでは、 `HMRoom`を使用すると、ユーザーはホーム (`HMHome`) 内に特定の部屋を定義できます。次に例を示します。キッチン、浴室、ガレージ、またはリビングルーム。 ユーザーは、家の特定の場所にあるすべてのホームオートメーションデバイスをにグループ化`HMRoom`し、1つの単位として動作させることができます。 たとえば、ガレージのライトをオフにするように Siri に要求します。
- `HMAccessory`-これは、ユーザーの居住地 (スマートサーモスタットなど) にインストールされている、個別の物理ホームキットが有効になっているオートメーションデバイスを表します。 各`HMAccessory` は`HMRoom`に割り当てられます。 ユーザーが部屋を構成していない場合は、ホームキットによって特別な既定の部屋にアクセサリが割り当てられます。
- `HMService`-ライトやその色のオン/ `HMAccessory`オフの状態 (色の変更がサポートされている場合) など、指定されたによって提供されるサービスを表します。 各`HMAccessory`には、照明を含むガレージドア unityvs など、複数のサービスを含めることができます。 また、指定さ`HMAccessory`れたには、ユーザーコントロールの外部にある、ファームウェアの更新などのサービスが含まれる場合があります。
- `HMZone`-オブジェクトのコレクションを、階、Downstairs `HMRoom` 、Basement などの論理ゾーンにグループ化することをユーザーに許可します。 省略可能ですが、これにより、Siri に対してすべてのライト downstairs をオフにするような操作を行うことができます。

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>ホームキットアプリをプロビジョニングする

ホームキットによって課せられるセキュリティ要件により、Sekit フレームワークを使用する Xamarin iOS アプリは、Apple Developer ポータルと Xamarin の iOS プロジェクトファイルの両方で適切に構成されている必要があります。

次の手順で行います。

1. [Apple Developer ポータル](https://developer.apple.com)にログインします。
2. [**証明書]、[識別子 & プロファイル**] の順にクリックします。
3. まだ行っていない場合は、 **[識別子]** をクリックし、アプリの id を作成`com.company.appname`します (例:)。それ以外の場合は、既存の id を編集します。
4. 指定された ID について、**ホームキット**サービスがチェックされていることを確認します。 

    [![](homekit-images/provision01.png "指定された ID のホームキットサービスを有効にします")](homekit-images/provision01.png#lightbox)
5. 変更内容を保存します。
6. [**プロビジョニングプロファイル** > の**開発**] をクリックし、アプリの新しい開発プロビジョニングプロファイルを作成します。 

    [![](homekit-images/provision02.png "アプリの新しい開発プロビジョニングプロファイルを作成する")](homekit-images/provision02.png#lightbox)
7. 新しいプロビジョニングプロファイルをダウンロードしてインストールするか、Xcode を使用してプロファイルをダウンロードしてインストールします。
8. Xamarin. iOS プロジェクトのオプションを編集し、先ほど作成したプロビジョニングプロファイルを使用していることを確認します。 

    [![](homekit-images/provision03.png "作成したプロビジョニングプロファイルの選択")](homekit-images/provision03.png#lightbox)
9. 次に、**情報の plist**ファイルを編集し、プロビジョニングプロファイルの作成に使用したアプリ ID を使用していることを確認します。 

    [![](homekit-images/provision04.png "アプリ ID を設定する")](homekit-images/provision04.png#lightbox)
10. 最後に、**権利の plist**ファイルを編集し、[**ホームキット**の権利] が選択されていることを確認します。 

    [![](homekit-images/provision05.png "ホームキットの権利を有効にする")](homekit-images/provision05.png#lightbox)
11. すべてのファイルに変更を保存します。

これらの設定を適用すると、アプリケーションはホームキットフレームワークの Api にアクセスする準備ができました。 プロビジョニングの詳細については、[デバイスのプロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)と[プロビジョニング](~/ios/get-started/installation/device-provisioning/index.md)に関するガイドを参照してください。

> [!IMPORTANT]
> ホームキットが有効になっているアプリをテストするには、開発用に適切にプロビジョニングされた実際の iOS デバイスが必要です。 ホームキットを iOS シミュレーターからテストすることはできません。

## <a name="the-homekit-accessory-simulator"></a>ホームキットアクセサリシミュレーター

物理デバイスを使用しなくても、すべてのホームオートメーションデバイスとサービスをテストする方法を提供するために、Apple はホーム_キットアクセサリシミュレーター_を作成しました。 このシミュレーターを使用すると、仮想ホームキットデバイスを設定して構成できます。

### <a name="installing-the-simulator"></a>シミュレーターのインストール

Apple では、Xcode からの個別のダウンロードとして、ホームキットのアクセサリシミュレーターが提供されているので、続行する前にインストールする必要があります。

次の手順で行います。

1. Web ブラウザーで、 [Apple 開発者向けダウンロード](https://developer.apple.com/download/more/?name=for%20Xcode)にアクセスします。
2. **Xcode xxx 用の追加ツール**をダウンロードします (xxx は、インストールした Xcode のバージョンです)。 

    [![](homekit-images/simulator01.png "Xcode 用の追加ツールをダウンロードする")](homekit-images/simulator01.png#lightbox)
3. ディスクイメージを開いて、**アプリケーション**ディレクトリにツールをインストールします。

ホームキットアクセサリシミュレーターがインストールされているので、テスト用に仮想アクセサリを作成できます。

### <a name="creating-virtual-accessories"></a>バーチャルアクセサリの作成

ホームキットアクセサリシミュレーターを起動し、いくつかの仮想アクセサリを作成するには、次の手順を実行します。

1. [アプリケーション] フォルダーから、ホームキットアクセサリシミュレーターを起動します。 

    [![](homekit-images/simulator02.png "ホームキットアクセサリシミュレーター")](homekit-images/simulator02.png#lightbox)
2. ボタンを **+** クリックし、 **[新しいアクセサリ...]** を選択します。 

    [![](homekit-images/simulator03.png "新しいアクセサリを追加する")](homekit-images/simulator03.png#lightbox)
3. 新しいアクセサリに関する情報を入力し、 **[完了]** ボタンをクリックします。 

    [![](homekit-images/simulator04.png "新しいアクセサリに関する情報を入力します")](homekit-images/simulator04.png#lightbox)
4. **[サービスの追加]** をクリックします。 をクリックし、ドロップダウンからサービスの種類を選択します。 

    [![](homekit-images/simulator05.png "ドロップダウンリストからサービスの種類を選択します")](homekit-images/simulator05.png#lightbox)
5. サービスの**名前**を指定し、 **[完了]** ボタンをクリックします。 

    [![](homekit-images/simulator06.png "サービスの名前を入力してください")](homekit-images/simulator06.png#lightbox)
6. **[特性の追加]** ボタンをクリックし、必要な設定を構成することにより、サービスのオプションの特性を指定できます。 

    [![](homekit-images/simulator07.png "必要な設定の構成")](homekit-images/simulator07.png#lightbox)
7. 上記の手順を繰り返して、ホームキットがサポートする各種類の仮想ホームオートメーションデバイスの1つを作成します。

Virtual ホームキットのアクセサリをいくつか作成して構成したので、これらのデバイスを Xamarin iOS アプリから使用し、制御できるようになりました。

## <a name="configuring-the-infoplist-file"></a>情報の plist ファイルの構成

IOS 10 (およびそれ以降) の新機能として、開発者`NSHomeKitUsageDescription`はアプリの`Info.plist`ファイルにキーを追加し、アプリがユーザーのホームキットデータベースにアクセスする理由を宣言する文字列を指定する必要があります。 この文字列は、アプリを初めて実行するときにユーザーに表示されます。

[![](homekit-images/info01.png "[ホームキットのアクセス許可] ダイアログ")](homekit-images/info01.png#lightbox)

このキーを設定するには、次の手順を実行します。

1. `Info.plist` **ソリューションエクスプローラー**内のファイルをダブルクリックして、編集用に開きます。
2. 画面の下部で、**ソース**ビューに切り替えます。
3. リストに新しい**エントリ**を追加します。
4. ドロップダウンリストから、 **[プライバシー-ホームキットの使用状況の説明]** を選択します。 

    [![](homekit-images/info02.png "プライバシー-ホームキットの利用状況の説明の選択")](homekit-images/info02.png#lightbox)
5. アプリがユーザーのホームキットデータベースにアクセスする理由の説明を入力します。 

    [![](homekit-images/info03.png "説明を入力してください")](homekit-images/info03.png#lightbox)
6. 変更内容をファイルに保存します。

> [!IMPORTANT]
> ファイルにキー`NSHomeKitUsageDescription`を設定しないと、iOS 10 (またはそれ以降) で実行したときに、アプリがサイレント (実行時にシステムによって終了される) エラーなしで失敗します。 `Info.plist`

## <a name="connecting-to-homekit"></a>ホームキットに接続しています

ホームキットと通信するには、Xamarin iOS アプリで`HMHomeManager`クラスのインスタンスを最初にインスタンス化する必要があります。 ホームマネージャーは、ホームキットへの中心的な入り口であり、使用可能な自宅の一覧を提供し、そのリストを更新して維持し、ユーザーの_プライマリホーム_を返す役割を担います。

オブジェクト`HMHome`には、インストールされているすべてのホームオートメーションアクセサリと共に、含まれている可能性のあるルーム、グループ、またはゾーンを含む、ホームに関するすべての情報が含まれます。 ホームキットで何らかの操作を実行する前に、 `HMHome`少なくとも1つを作成し、プライマリホームとして割り当てる必要があります。

アプリは、プライマリホームが存在するかどうかを確認し、存在しない場合は作成して割り当てます。

### <a name="adding-a-home-manager"></a>ホームマネージャーの追加

**AppDelegate.cs**ファイルを編集して編集し、次のように表示されるようにするには、[ホームキットの認識] を Xamarin iOS アプリに追加します。

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

アプリケーションが最初に実行されるときに、ユーザーが自分のホームキット情報にアクセスできるようにするかどうかを確認するメッセージが表示されます。

[![](homekit-images/home01.png "自分のホームキット情報へのアクセスを許可するかどうかを確認するメッセージがユーザーに表示されます。")](homekit-images/home01.png#lightbox)

ユーザーが **「OK」** と答えた場合、アプリケーションはホームキットのアクセサリを操作できるようになります。それ以外の場合は、それ以外の場合は失敗し、ホームキットへの呼び出しはエラーで失敗します。

ホームマネージャーが配置されていると、アプリケーションはプライマリホームが構成されているかどうかを確認する必要があります。そうでない場合は、ユーザーが作成して割り当てることができるようにします。

### <a name="accessing-the-primary-home"></a>プライマリホームへのアクセス

前述のように、ホームキットを使用できるようにする前にプライマリホームを作成して構成する必要があります。また、プライマリホームがまだ存在しない場合は、ユーザーがプライマリホームを作成して割り当てる方法を提供する必要があります。

アプリが初めて起動したとき、またはバックグラウンドから戻ったとき`DidUpdateHomes` 、プライマリホーム`HMHomeManager`の存在を確認するには、クラスのイベントを監視する必要があります。 存在しない場合は、ユーザーが作成するインターフェイスを提供する必要があります。

次のコードをビューコントローラーに追加して、プライマリホームを確認することができます。

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

Home Manager がホームキット`DidUpdateHomes`に接続すると、イベントが発生し、既存のすべての自宅がマネージャーの自宅のコレクションに読み込まれ、プライマリホームが読み込まれます (使用可能な場合)。

### <a name="adding-a-primary-home"></a>プライマリホームの追加

`PrimaryHome`のプロパティ`null`がイベントの後にある場合は、続行する前に、ユーザーがプライマリホームを作成して割り当てる方法を指定する必要があります。 `DidUpdateHomes` `HMHomeManager`

通常、アプリは新しいホームに名前を付け、ホームマネージャーに渡され、プライマリホームとしてセットアップするためのフォームを提示します。 **HomeKitIntro**サンプルアプリでは、モーダルビューが IOS デザイナーで作成され、アプリのメイン`AddHomeSegue`インターフェイスからセグエによって呼び出されました。

ユーザーが新しいホームの名前を入力するためのテキストフィールドと、ホームを追加するボタンが用意されています。 ユーザーが [ホームの**追加**] ボタンをタップすると、次のコードはホームマネージャーを呼び出してホームを追加します。

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

メソッド`AddHome`は、新しいホームの作成を試行し、指定されたコールバックルーチンに返します。 プロパティがでない`null`場合は、エラーが発生し、ユーザーに表示されます。 `error` 最も一般的なエラーは、一意でないホーム名が原因であるか、Home Manager がホームキットと通信できないことが原因で発生します。

自宅が正常に作成された場合は、 `UpdatePrimaryHome`メソッドを呼び出して、新しいホームをプライマリホームとして設定する必要があります。 この場合も、 `error`プロパティがで`null`ない場合はエラーが発生し、ユーザーに表示されます。

また、ホームマネージャーの`DidAddHome`イベントと`DidRemoveHome`イベントを監視し、必要に応じてアプリのユーザーインターフェイスを更新する必要があります。

> [!IMPORTANT]
> 上記のサンプルコードで使用されているメソッドは、iOSアラートを簡単に操作できるようにするHomeKitIntroアプリケーションのヘルパークラスです。`AlertView.PresentOKAlert`


## <a name="finding-new-accessories"></a>新しいアクセサリの検索

ホームマネージャーでプライマリホームが定義または読み込まれると、Xamarin iOS アプリはを呼び出して、 `HMAccessoryBrowser`新しいホームオートメーションアクセサリを検索し、自宅に追加することができます。

メソッドを呼び出して、完了時に新しいアクセサリ`StopSearchingForNewAccessories`とメソッドの検索を開始します。 `StartSearchingForNewAccessories`

> [!IMPORTANT]
> `StartSearchingForNewAccessories`は、iOS デバイスのバッテリ寿命とパフォーマンスの両方に悪影響を与えるため、長時間実行されないようにする必要があります。 Apple は、 `StopSearchingForNewAccessories` 1 分後にの呼び出しを提案します。または、検索アクセサリ UI がユーザーに表示される場合にのみ検索します。

この`DidFindNewAccessory`イベントは、新しいアクセサリが検出されると呼び出され、アクセサリブラウザーの`DiscoveredAccessories`一覧に追加されます。

リスト`DiscoveredAccessories`には、[ホーム] `HMAccessory`キットが有効になっているホームオートメーションデバイスと、ライトやガレージドアコントロールなどの利用可能なサービスを定義するオブジェクトのコレクションが含まれます。

新しいアクセサリが見つかると、ユーザーに表示され、それを選択してホームに追加できるようになります。 例:

[![](homekit-images/accessory01.png "新しいアクセサリの検索")](homekit-images/accessory01.png#lightbox)

`AddAccessory`メソッドを呼び出して、選択したアクセサリをホームのコレクションに追加します。 例えば:

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

プロパティがでない`null`場合は、エラーが発生し、ユーザーに表示されます。 `err` それ以外の場合、ユーザーは、追加するデバイスのセットアップコードを入力するように求められます。

[![](homekit-images/accessory02.png "追加するデバイスのセットアップコードを入力してください")](homekit-images/accessory02.png#lightbox)

ホームキットアクセサリシミュレーターでは、この数値は **[セットアップコード]** フィールドにあります。

[![](homekit-images/accessory03.png "ホームキットアクセサリシミュレーターのセットアップコードフィールド")](homekit-images/accessory03.png#lightbox)

実際のホームキットのアクセサリの場合、セットアップコードは、デバイス自体、製品ボックス、またはアクセサリのユーザーマニュアルのラベルに印刷されます。

アクセサリブラウザーの`DidRemoveNewAccessory`イベントを監視し、ユーザーがホームコレクションに追加した後で、使用可能な一覧からアクセサリを削除するようにユーザーインターフェイスを更新する必要があります。

## <a name="working-with-accessories"></a>アクセサリの操作

プライマリホームが確立され、アクセサリが追加されたら、ユーザーが操作できるアクセサリの一覧 (および必要に応じてルーム) を提示できます。

`HMRoom`オブジェクトには、特定の部屋とそれに属するアクセサリに関するすべての情報が含まれています。 必要に応じて、1つまたは複数のゾーンにルームを整理することができます。 に`HMZone`は、特定のゾーンとそれに属するすべてのルームに関するすべての情報が含まれます。

この例では、これらを部屋やゾーンに分類するのではなく、シンプルで、自宅のアクセサリを直接操作します。

オブジェクトには、 `Accessories`プロパティでユーザーに提示できる、割り当てられたアクセサリの一覧が含まれています。 `HMHome` 例えば:

[![](homekit-images/accessory04.png "アクセサリの例")](homekit-images/accessory04.png#lightbox)

このフォームでは、ユーザーは特定のアクセサリを選択し、提供されているサービスを使用できます。

## <a name="working-with-services"></a>サービスの操作

指定されたホームオートメーションデバイスでユーザーが操作を行うときは、通常、提供されているサービスを使用します。 クラスのプロパティに`Services`は、デバイスが提供`HMService`するサービスを定義するオブジェクトのコレクションが含まれています。 `HMAccessory`

サービスは、ライト、サーモスタット、ガレージドアの openers、スイッチ、またはロックのようなものです。 一部のデバイス (ガレージドア unityvs など) では、光やドアを開いたり閉じたりする機能など、複数のサービスが提供されます。

各アクセサリには、特定のアクセサリが提供する特定のサービスに加え`Information Service`て、名前、製造元、モデル、シリアル番号などのプロパティを定義するが含まれています。

### <a name="accessory-service-types"></a>アクセサリサービスの種類

`HMServiceType`列挙型では、次のサービスの種類を使用できます。

- **AccessoryInformation** -指定されたホームオートメーションデバイス (アクセサリ) に関する情報を提供します。
- **AirQualitySensor** -航空品質センサーを定義します。
- **バッテリ**-アクセサリのバッテリの状態を定義します。
- **CarbonDioxideSensor** -カーボン二酸化炭素センサーを定義します。
- **CarbonMonoxideSensor** -カーボンモノ Xide センサーを定義します。
- **Contactsensor** -連絡先センサー (開いているウィンドウや閉じているウィンドウなど) を定義します。
- **ドア**-ドアの状態センサー (opened、closed など) を定義します。
- **ファン**-リモートで制御されるファンを定義します。
- **GarageDoorOpener** -ガレージドア unityvs を定義します。
- **HumiditySensor** -湿度センサーを定義します。
- **LeakSensor** -リークセンサー (ホットウォーター加熱器や洗濯機など) を定義します。
- **電球**-スタンドアロンライト、または別のアクセサリ (ガレージドア unityvs など) の一部であるライトを定義します。
- ライト**センサー** -光センサーを定義します。
- **Lockmanagement** -自動ドアロックを管理するサービスを定義します。
- **Lockmechanism** -リモートで制御されるロック (ドアロックなど) を定義します。
- **Motionsensor** -モーションセンサーを定義します。
- **OccupancySensor** -占有センサーを定義します。
- **アウトレット**-リモートで制御される壁面コンセントを定義します。
- **Securitysystem** -ホームセキュリティシステムを定義します。
- **StatefulProgrammableSwitch** -(フリップスイッチのように) トリガーされると、状態が "実行可能" になるプログラム可能なスイッチを定義します。
- **StatelessProgrammableSwitch** -トリガーされた (プッシュボタンのような) 初期状態に戻るプログラミング可能なスイッチを定義します。
- **SmokeSensor** -スモークセンサーを定義します。
- **スイッチ**-標準の壁面スイッチのようなオン/オフスイッチを定義します。
- **温度センサー** -温度センサーを定義します。
- **サーモスタット**-HVAC システムを制御するために使用されるスマートサーモスタットを定義します。
- **Window** -リモートで開いたり閉じたりする cane の自動化されたウィンドウを定義します。
- **Windowcovering** -開いたり閉じたりできるブラインドなど、リモートで制御されるウィンドウを定義します。

### <a name="displaying-service-information"></a>サービス情報の表示

を`HMAccessory`読み込むと、提供された個々`HNService`のオブジェクトに対してクエリを実行し、その情報をユーザーに表示できます。

[![](homekit-images/accessory05.png "サービス情報の表示")](homekit-images/accessory05.png#lightbox)

の使用を試みる前に`Reachable` 、 `HMAccessory`常にのプロパティを確認する必要があります。 ユーザーがデバイスの範囲内にない場合、または取り外されている場合、アクセサリに到達できないことがあります。

サービスを選択すると、そのサービスの1つまたは複数の特性を表示または変更して、特定のホームオートメーションデバイスを監視または制御することができます。

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>特性の操作

各`HMService`オブジェクトには、サービスの`HMCharacteristic`状態に関する情報 (ドアを開いたり閉じたりするなど) を提供したり、ユーザーが状態を調整したり (ライトの色の設定など) できるオブジェクトのコレクションを含めることができます。

`HMCharacteristic`は、特性とその状態に関する情報だけでなく、_特性メタデータ_(`HMCharacteristisMetadata`) を介して状態を操作するためのメソッドも提供します。 このメタデータは、ユーザーに情報を表示したり、状態の変更を許可したりするときに役立つプロパティ (最小値と最大値の範囲など) を提供できます。

列挙`HMCharacteristicType`型には、次のように定義または変更できる特性メタデータ値のセットが用意されています。

- AdminOnlyAccess
- AirParticulateDensity
- AirParticulateSize
- 航空品質
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
- ハードウェアのバージョン
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
- Out・ Inuse
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

### <a name="working-with-a-characteristics-value"></a>特性の値の操作

アプリに特定の特性の最新の状態が確実に含まれるように`ReadValue`するには`HMCharacteristic` 、クラスのメソッドを呼び出します。 プロパティがでない`null`場合は、エラーが発生し、ユーザーに表示される場合と表示されない場合があります。 `err`

特性の`Value`プロパティは、指定された特性の現在の状態`NSObject`をとして格納します。そのためC#、で直接操作することはできません。

この値を読み取るために、次のヘルパークラスが**HomeKitIntro**サンプルアプリケーションに追加されました。

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

は`NSObjectConverter` 、アプリケーションが特性の現在の状態を読み取る必要があるときに常に使用されます。 たとえば、次のように入力します。

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

上の行は、値`float`をに変換して、Xamarin C#コードで使用できるようにします。

を変更`HMCharacteristic`するには、 `WriteValue`メソッドを呼び出し、 `NSObject.FromObject`呼び出しで新しい値をラップします。 たとえば、次のように入力します。

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

プロパティがでない`null`場合は、エラーが発生したため、ユーザーに表示される必要があります。 `err`

### <a name="testing-characteristic-value-changes"></a>特性値の変更のテスト

およびシミュレートされたアクセサリを使用する場合`Value` 、プロパティの変更は、ホームキットアクセサリシミュレーター内で監視できます。 `HMCharacteristics`

実際の iOS デバイスハードウェアで実行されている**HomeKitIntro**アプリでは、特性の値に対する変更は、ホームキットアクセサリシミュレーターでほぼ瞬時に表示されます。 たとえば、iOS アプリのライトの状態を変更すると、次のようになります。

[![](homekit-images/test01.png "IOS アプリのライトの状態を変更する")](homekit-images/test01.png#lightbox)

では、ホームキットアクセサリシミュレーターのライトの状態を変更する必要があります。 値が変更されない場合は、新しい特性値を書き込むときにエラーメッセージの状態を確認し、アクセサリに到達可能であることを確認します。

## <a name="advanced-homekit-features"></a>高度なホームキットの機能

この記事では、Xamarin iOS アプリでホームキットのアクセサリを操作するために必要な基本機能について説明しました。 ただし、この概要では説明されていない、ホームキットにはいくつかの高度な機能があります。

- **ルーム**-ホームキットが有効なアクセサリは、必要に応じて、エンドユーザーがルームに整理することができます。 これにより、ホームキットは、ユーザーが簡単に理解して操作できる方法でアクセサリを提示できます。 ルームの作成と管理の詳細については、Apple の[Hmroom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom)のドキュメントを参照してください。
- **ゾーン**-ルームは、必要に応じて、エンドユーザーがゾーンに整理することができます。 ゾーンとは、ユーザーが1つの単位として扱うことができるルームのコレクションを指します。 例えば:階、Downstairs、または Basement。 ここでも、ホームキットを使用して、エンドユーザーにとってわかりやすい方法でアクセサリを操作できます。 ゾーンの作成と管理の詳細については、Apple の[Hmzone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone)のドキュメントを参照してください。
- アクション**とアクションセット**-アクションによってアクセサリサービスの特性が変更され、セットごとにグループ化できるようになります。 アクションセットは、アクセサリのグループを制御し、そのアクションを調整するスクリプトとして機能します。 たとえば、"テレビを見る" スクリプトを使用すると、ブラインドを閉じ、ライトを暗くし、テレビとそのサウンドシステムをオンにすることができます。 アクションとアクションセットの作成と管理の詳細については、Apple の[Hmaction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction)および[hmactionset](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet)のドキュメントを参照してください。
- **トリガー** -トリガーは、特定の条件セットが満たされたときに1つ以上のアクションセットをアクティブ化できます。 たとえば、portch light をオンにし、外側に外側にあるすべての外部ドアをロックします。 トリガーの作成と管理の詳細については、Apple の[Hmtrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger)のドキュメントを参照してください。

これらの機能では上記と同じ手法が使用されるため、Apple の[HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)、[ホームキットのユーザーインターフェイスガイドライン](https://developer.apple.com/homekit/ui-guidelines/)、および[ホームキットフレームワークのリファレンス](https://developer.apple.com/library/ios/home_kit_framework_ref)に従って、簡単に実装する必要があります。

## <a name="homekit-app-review-guidelines"></a>ホームキットアプリレビューのガイドライン

ホームキットが有効になっている Xamarin を送信する前に、iTunes App Store でリリース用に iTunes Connect を使用して、ホームキットが有効になっているアプリに関する Apple のガイドラインに従ってください。

- Home Kit フレームワークを使用する場合、アプリの主な目的はホームオートメーションで_ある必要があり_ます。
- アプリのマーケティングテキストは、ホームキットが使用されていること、およびプライバシーポリシーを提供する必要があることをユーザーに通知する必要があります。
- ユーザー情報の収集、または広告のためのホームキットの使用は、厳密には禁止されています。

完全なレビューのガイドラインについては、Apple の[App Store のレビューに関するガイドライン](https://developer.apple.com/app-store/review/guidelines/)を参照してください。

## <a name="whats-new-in-ios-9"></a>IOS 9 の新機能

Apple は、iOS 9 用のホームキットに次のような変更と追加を加えました。

- **既存のオブジェクトの保持**-既存のアクセサリが変更された場合`HMHomeManager`、ホームマネージャー () によって、変更された特定の項目が通知されます。
- **永続的な識別子**-関連するすべてのホームキットクラス`UniqueIdentifier`に、ホームキットが有効になっているアプリ (または同じアプリのインスタンス) をまたいで特定の項目を一意に識別するプロパティが含まれるようになりました。
- **ユーザー管理**-プライマリユーザーのホームにあるホームキットデバイスへのアクセス権を持つユーザーを管理するための組み込みビューコントローラーが追加されました。
- **ユーザーの機能**-ホームキットのユーザーは、ホームキットおよびホームキットを有効にしたアクセサリで使用できる機能を制御する一連の特権を持つようになりました。 アプリでは、現在のユーザーにのみ関連する機能を表示する必要があります。 たとえば、他のユーザーを管理できるのは管理者だけです。
- **定義済みのシーン**-平均ホームキットのユーザーに対して発生する4つの一般的なイベントに対して、定義済みのシーンが作成されています。Up、Leave、返却、ベッドに移動します。 これらの定義済みシーンをホームから削除することはできません。
- **バックグラウンドおよび siri** -siri は、iOS 9 でのバックグラウンドのサポートをさらに強化し、ホームキットで定義されているシーンの名前を認識できます。 ユーザーは、その名前を Siri と話すだけでシーンを実行できます。
- **アクセサリカテゴリ**-定義済みのカテゴリのセットがすべてのアクセサリに追加され、自宅に追加されているアクセサリやアプリ内で動作しているアクセサリの種類を特定するのに役立ちます。 これらの新しいカテゴリは、アクセサリのセットアップ中に利用できます。
- **Apple Watch サポート**-ホームキットを watchOS で使用できるようになりました。また、Apple Watch は iPhone が Watch に近づいていなくても、ホームキット対応デバイスを制御できるようになります。 WatchOS 用のホームキットでは、次の機能がサポートされています。自宅の表示、アクセサリの制御、およびシーンの実行。
- **新しいイベントトリガーの種類**-ios 8 でサポートされているタイマーの種類のトリガーに加え、ios 9 では、アクセサリの状態 (センサーデータなど) または位置情報に基づくイベントトリガーがサポートされるようになりました。 イベントトリガーは`NSPredicates` 、実行の条件を設定するために使用します。
- リモート**アクセス**-リモートアクセスを使用すると、ユーザーはホームキットが有効になっているホームオートメーションアクセサリを、遠隔地にある家から離れた場所で管理できるようになりました。 IOS 8 では、このことは、ユーザーが自宅で第3世代の Apple TV を持っている場合にのみサポートされていました。 IOS 9 では、この制限は解除されており、リモートアクセスは iCloud とホームキットアクセサリプロトコル (HAP) を介してサポートされています。
- **新しい Bluetooth 低エネルギー (b)** 機能-ホームキットでは、Bluetooth 低エネルギー (b) プロトコルを介して通信できるアクセサリの種類がサポートされるようになりました。 HAP セキュアトンネリングを使用すると、ホームキットアクセサリは Wi-fi 経由で別の Bluetooth アクセサリを公開できます (Bluetooth の範囲外の場合)。 IOS 9 では、付属のアクセサリが通知とメタデータを完全にサポートしています。
- **新しいアクセサリカテゴリ**-Apple では、iOS 9 に次の新しいアクセサリカテゴリが追加されました。ウィンドウの表示、原動機のドアとウィンドウ、警報システム、センサー、およびプログラミング可能なスイッチ。

IOS 9 のホームキットの新機能の詳細については、Apple の[ホームキットのインデックス](https://developer.apple.com/homekit/)と、「[ホームキットの新](https://developer.apple.com/videos/wwdc/2015/?id=210)機能」ビデオを参照してください。

## <a name="summary"></a>Summary

この記事では、Apple のホームキット home automation フレームワークについて紹介しました。 この例では、ホームキットアクセサリシミュレーターを使用してテストデバイスをセットアップして構成する方法と、簡単な Xamarin. iOS アプリを作成して、home Kit を使用してホームオートメーションデバイスを検出、通信、および制御する方法を示しました。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [IOS 9.0 の新機能](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [HomeKitDeveloper ガイド](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [ホームキットのユーザーインターフェイスのガイドライン](https://developer.apple.com/homekit/ui-guidelines/)
- [ホームキットフレームワークリファレンス](https://developer.apple.com/library/ios/home_kit_framework_ref)
