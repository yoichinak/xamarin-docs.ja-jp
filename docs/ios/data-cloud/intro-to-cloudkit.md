---
title: Xamarin.iOS で CloudKit
description: このドキュメントでは、Xamarin.iOS で CloudKit を操作する方法について説明します。 CloudKit の概要を説明し、CloudKit の利便性のための API、スケーラビリティ、ユーザー アカウント、および開発および運用環境を有効にする方法について説明します。
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/11/2016
ms.openlocfilehash: daea27472ac7c0578c1cfd79ebd96428212fafb3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107444"
---
# <a name="cloudkit-in-xamarinios"></a>Xamarin.iOS で CloudKit

CloudKit フレームワークでは、アプリケーションの開発に、そのアクセス iCloud が効率化します。 これには、アプリケーション データと資産の権利とアプリケーションの情報を安全に保管することの取得が含まれます。 このキットでは、個人情報を共有することがなく、iCloud Id とアプリケーションへのアクセスを許可することでユーザーの匿名性レイヤーを提供します。

開発者では、そのクライアント側アプリケーションに集中でき、サーバー側のアプリケーション ロジックを記述する必要がある iCloud を使用することができます。 CloudKit では、認証、プライベートおよびパブリックのデータベースと構造化データおよび資産のストレージ サービスを提供します。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

## <a name="requirements"></a>必要条件

この記事で紹介する手順を完了する、次が必要。

-  **Xcode と iOS SDK** – Api がインストールされ、開発者のコンピューターに構成する必要がある Apple の Xcode と iOS 8。
-  **Visual Studio for Mac** – Visual Studio for Mac の最新バージョンをインストールして、ユーザーのデバイスで構成する必要があります。
-  **iOS 8 デバイス**– iOS デバイスをテストするための iOS 8 の最新バージョンを実行します。

## <a name="what-is-cloudkit"></a>CloudKit とは何ですか。

CloudKit は、iCloud のサーバーに開発者のアクセスを付与する方法です。 ICloud ドライブと iCloud フォト ライブラリの両方の基盤を提供します。 CloudKit は、Mac OS X と Apple iOS デバイスでサポートされます。

 [![](intro-to-cloudkit-images/image1.png "Mac OS X と Apple iOS デバイスの両方で CloudKit のサポートについて")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit は、iCloud アカウント インフラストラクチャを使用します。 ICloud にデバイスのアカウントにログインしたユーザーがある場合、CloudKit は、ユーザーを識別するために、ID を使用します。 アカウントが使用できない場合は、制限付きの読み取り専用アクセスが提供されます。

CloudKit には、パブリックおよびプライベートのデータベースの両方の概念がサポートされています。 パブリックのデータベースでは、ユーザーにアクセスできるすべてのデータの「スープ」を提供します。 秘密のデータベースは、特定のユーザーにバインドされているプライベート データを格納するものです。

CloudKit では、構造化および一括の両方のデータをサポートします。 大きなファイルの転送をシームレスに処理できるになります。 CloudKit では、バック グラウンドで iCloud サーバーとの間に大きなファイルを効率的に転送するその他のタスクに集中する開発者を解放することが行われます。

> [!NOTE]
> CloudKit は、することが重要な_トランスポート テクノロジ_します。 すべての永続化は提供されません。アプリケーションを効率的にサーバーから情報を送受信のみできます。

この執筆時点で、Apple は最初に提供 CloudKit 無料を高帯域幅とストレージの両方の容量制限します。 大規模なまたは大規模なユーザー ベースのアプリケーション プロジェクトは、Apple のヒントが、手頃な価格の価格のスケールが提供されることです。


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Xamarin アプリケーションで有効にすると CloudKit

Xamarin アプリケーションは、CloudKit フレームワークを利用できます、前に、アプリケーションを正しくプロビジョニングする必要がありますで説明されている、[機能の使用](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)と[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)ガイド

1.  Visual studio for Mac または Visual Studio プロジェクトを開きます。
2.  **ソリューション エクスプ ローラー**、オープン、 **Info.plist**ファイルを確認します、**バンドル識別子**で定義されているものと一致する**アプリ ID**プロビジョニングの一部の設定を作成します。
 
    [![](intro-to-cloudkit-images/image26a.png "バンドル Id を入力します")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  一番下までスクロール、 **Info.plist**ファイルおよび選択**バック グラウンド モードを有効になっている**、**場所の更新**と**リモート通知**:

    [![](intro-to-cloudkit-images/image27a.png "位置情報の更新とリモート通知が有効なバック グラウンド モードを選択します。")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  クリックし、ソリューションで iOS プロジェクトを右クリックして**オプション**します。
5.  選択**iOS バンドル署名**を選択、**開発者 Identity**と**プロビジョニング プロファイル**上記で作成しました。
6.  確認、 **Entitlements.plist**が含まれています**iCloud を有効にする**、 **key-value ストレージ**と**CloudKit**します。
7.  確認、**ユビキタス コンテナー** (上記で作成した) と、アプリケーションに存在します。 例 : `iCloud.com.your-company.CloudKitAtlas`
8.  変更内容をファイルに保存します。


これら設定した状態で、アプリケーションでは CloudKit フレームワークの Api にアクセスする準備ができてようになりました。

## <a name="cloudkit-api-overview"></a>CloudKit API の概要

Xamarin iOS アプリケーションで CloudKit を実装する前に、この記事では、次のトピックが含まれる CloudKit フレームワークの基礎を網羅すること。

1.  **コンテナー** – iCloud 通信のサイロを分離します。
2.  **データベース**– Public と private は、アプリケーションで使用できます。
3.  **レコード**– と CloudKit の間に構造化データが移動するメカニズムです。
4.  **レコード ゾーン**– レコードのグループします。
5.  **識別子は記録**– を完全に正規化して、レコードの特定の場所を表します。
6.  **参照**– 特定のデータベース内の関連レコード間のリレーションシップの親と子を提供します。
7.  **資産**– を iCloud にアップロードして、特定のレコードに関連付けられている、大規模な非構造化データのファイルを許可します。


### <a name="containers"></a>コンテナー

IOS デバイスで実行されている特定のアプリケーションが常に実行されているその他のアプリケーションとそのデバイス上のサービスをと共に側します。 クライアント デバイスでは、アプリケーションは孤立したまたは何らかの方法でサンド ボックスになります。 場合によっては、リテラル サンド ボックスでは、これは、他のユーザー アプリケーションが実行するだけと独自のメモリ領域。

クライアント アプリケーションを取得し、他のクライアントからは別個の操作を実行している概念は、非常に強力な次の利点します。

1.  **セキュリティ**– 1 つのアプリケーションが他のクライアント アプリまたは OS 自体に干渉できません。
1.  **安定性**– クライアント アプリケーションがクラッシュした場合、OS の他のアプリを取り除くことはできません。
1.  **プライバシー** – 各クライアント アプリケーションがデバイスに格納される個人情報へのアクセスを制限します。


CloudKit の上記に示すように同じ利点を提供し、クラウド ベースの情報の操作に適用するもの。

 [![](intro-to-cloudkit-images/image31.png "CloudKit のアプリのコンテナーを使用した通信します。")](intro-to-cloudkit-images/image31.png#lightbox)

多の 1 つのデバイスで実行されているは対象のアプリケーションと同様、一対多の iCloud との通信をアプリケーションのためです。 これらのさまざまな通信手段の各コンテナーと呼ばれます。

コンテナーが CloudKit Framework 経由で公開されている、`CKContainer`クラス。 既定では、1 つのコンテナーと、このコンテナーに 1 つのアプリケーションでは、そのアプリケーションのデータを分離します。 つまり、ことについては、同じ iCloud アカウントにいくつかのアプリケーションを格納することがまだ、この情報は混在させることはありません。

ICloud のデータのコンテナー化 CloudKit ユーザー情報をカプセル化することもできます。 これで、アプリケーションは、iCloud アカウントにいくつかの制限付きアクセスし、で、ユーザーのセキュリティとプライバシーを保護しながらすべてのユーザー情報が格納されています。

コンテナーは、WWDR ポータルを使用して、アプリケーションの開発者によって完全に管理されます。 コンテナーのみすることはできません、特定の開発者のアプリケーションに一意であるために、コンテナーの名前空間は、すべての Apple の開発者の間でグローバルがすべての Apple の開発者およびアプリケーションにします。

Apple では、アプリケーション コンテナーの名前空間を作成するときに、逆引き DNS 表記を使用をお勧めします。 例:

```csharp
iCloud.com.company-name.application-name
```

コンテナーは、既定で、バインドされている特定のアプリケーションに一対一、中には、アプリケーション間で共有できます。 このため、複数のアプリケーションは、1 つのコンテナーで調整できます。 1 つのアプリケーションは、複数のコンテナーにも説明します。

### <a name="databases"></a>Databases

ICloud サーバーまでそのモデルをアプリケーションのデータ モデルとレプリケーションにかかる CloudKit の主な機能の 1 つです。 作成したユーザーのためのものがいくつかの情報やその他の情報は、パブリック データ (など、レストランのレビュー) を公開して使用するためにユーザーが作成したもの、開発者がアプリケーションの公開情報がある可能性があります。 どちらの場合は、対象ユーザーが 1 人のユーザーだけではありませんが、人のコミュニティします。

 [![](intro-to-cloudkit-images/image32.png "CloudKit コンテナー ダイアグラム")](intro-to-cloudkit-images/image32.png#lightbox)

コンテナーの内部で第一には、パブリック データベースです。 これは、存在し、併置 mingles すべて公開情報の場所です。 また、特定のアプリケーションのユーザーごとに個々 のいくつかのプライベート データベースが使用されます。

IOS デバイスで実行されている、ときに、アプリケーションは、iCloud 現在ログオンしているユーザーの情報へのアクセスをのみがあります。 コンテナーのアプリケーションのビューに次のようになります。

 [![](intro-to-cloudkit-images/image33.png "コンテナーのアプリケーション ビュー")](intro-to-cloudkit-images/image33.png#lightbox)

Public データベースおよび iCloud に現在ログオンしているユーザーに関連付けられている秘密のデータベースにのみ参照できます。

データベースが CloudKit Framework 経由で公開されている、`CKDatabase`クラス。 すべてのアプリケーションが 2 つのデータベースへのアクセス: パブリック データベースと秘密の 1 つ。

コンテナーは、CloudKit への初期のエントリ ポイントです。 次のコードは、アプリケーションの既定のコンテナーからパブリックおよびプライベートのデータベースへのアクセスに使用できます。

```csharp
using CloudKit;
...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

データベースの種類の違いを次に示します。

||Public データベース|プライベート データベース|
|---|--- |--- |
|**データ型**|共有データ|現在のユーザーのデータ|
|**クォータ**|開発者のクォータに計上|ユーザーのクォータに計上|
|**既定のアクセス許可**|読み取り可能な世界|読み取り可能なユーザー|
|**アクセス許可の編集**|レコード クラス レベルで iCloud ダッシュ ボードの役割|N/A|

### <a name="records"></a>レコード

コンテナーは、データベース、およびデータベースには、レコードを保持します。 レコードと CloudKit の間に構造化データが移動する機構です。

 [![](intro-to-cloudkit-images/image34.png "コンテナーは、データベース、およびデータベースには、レコードを保持")](intro-to-cloudkit-images/image34.png#lightbox)

レコードが CloudKit Framework 経由で公開されている、`CKRecord`クラスは、キーと値のペアをラップします。 アプリケーション内のオブジェクトのインスタンスが等しく、 `CKRecord` CloudKit でします。 さらに、各`CKRecord`オブジェクトのクラスと等価であるレコード型を所有します。

レコードは、データが CloudKit の処理から渡されている前に説明したように、ジャストイン タイム スキーマを持っています。 時点から、CloudKit は情報を解釈し、処理の格納とレコードを取得する方法について説明します。

`CKRecord`クラスには、さまざまなメタデータもサポートしています。 たとえば、レコードには、作成したときとそれを作成したユーザーに関する情報が含まれています。 レコードには、最終更新日と変更したユーザーに関する情報も含まれています。

レコードには、変更のタグの概念が含まれています。 これは、特定のレコードのリビジョンの以前のバージョンです。 変更のタグは、クライアントとサーバーの特定のレコードの同じバージョンであるかどうかの軽量な方法として使用されます。

、前述のよう`CKRecords`キーと値のペアをラップし、レコードのため、次の種類のデータを格納できます。

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


1 つの値の種類に加えて、レコードが上記の種類の一覧の任意の同種の配列を含めることができます。

新しいレコードを作成し、データベースに格納する、次のコードを使用できます。

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>ゾーンのレコード

単独で、特定のデータベース内のレコードが存在しない – レコードのグループはレコード ゾーン内で一緒に存在します。 ゾーンのレコードは従来のリレーショナル データベースにテーブルとしてのを考えることができます。

 [![](intro-to-cloudkit-images/image35.png "レコードのグループはレコード ゾーン内で一緒に存在します。")](intro-to-cloudkit-images/image35.png#lightbox)

特定のレコード ゾーン内で複数のレコードと、特定のデータベース内の複数のレコード ゾーンが表示されることができます。 すべてのデータベースには、既定のレコード ゾーンが含まれています。

 [![](intro-to-cloudkit-images/image36.png "すべてのデータベースにはレコード ゾーンの既定およびカスタムのゾーンが含まれています")](intro-to-cloudkit-images/image36.png#lightbox)

これは、レコードが既定で格納されます。 さらに、カスタム レコードのゾーンを作成することができます。 ゾーンの表すするアトミックなコミットと変更の追跡に基本の粒度の完了を記録します。

## <a name="record-identifiers"></a>レコード識別子

レコード識別子はタプルとして表されます、両方を含む、クライアント提供しているレコードの名前と、ゾーンのレコードが存在します。 レコード識別子では、次の特性があります。

-  クライアント アプリケーションによって作成されます。
-  完全に正規化され、レコードの特定の場所を表します。
-  外部データベース内のレコードの一意の ID、レコード名に割り当てるは、ブリッジ CloudKit 内に格納されていないローカル データベースが使用できます。


開発者は、新しいレコードを作成するときに、レコードの識別子を渡す選択できます。 レコード識別子が指定されていない場合、UUID が自動的に作成されレコードに割り当てられています。

開発者は、新しいレコードの識別子を作成するときに、各レコードが所属するレコードのゾーンを指定する選択できます。 指定されていない場合は、既定のレコード ゾーンが使用されます。

レコード識別子が CloudKit Framework 経由で公開されている、`CKRecordID`クラス。 新しいレコードの識別子を作成する次のコードを使用できます。

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>参照

参照は、特定のデータベース内の関連レコード間の関係を提供します。

 [![](intro-to-cloudkit-images/image37.png "参照は、特定のデータベース内の関連レコード間の関係を提供します。")](intro-to-cloudkit-images/image37.png#lightbox)

上記の例では、親の子を所有して、子が親レコードの子レコードように。 関係は、親レコードに子レコードから移動されと呼ばれます、*参照戻り*します。

参照が CloudKit Framework 経由で公開されている、`CKReference`クラス。 これは、iCloud サーバーがレコード間の関係を理解できるようにすることの方法です。

参照は、連鎖削除の背後にあるメカニズムを提供します。 親レコードがデータベースから削除する場合 (リレーションシップで指定) として任意の子レコードは、データベースから自動的に削除されます。

> [!NOTE]
> 未解決のポインターは、CloudKit を使用する場合に可能性が。 たとえば、アプリケーションのレコード ポインターのリストをフェッチ、レコードが選択されているし、レコードのよく寄せられるまでに、レコード可能性がありますが存在しません、データベース内。 このような状況を適切に処理するためには、アプリケーションをコーディングする必要があります。

必須ではありません CloudKit Framework での作業時に参照戻りが望ましいです。 Apple では、最も効率的な参照の型にするシステムを微調整が。

参照を作成するときに、開発者する既にメモリ内にあるレコードを提供するか、レコード識別子への参照を作成します。 データベース内に存在していないレコード識別子、および指定した参照を使用して、未解決のポインターが作成されます。

既知のレコードの参照を作成する例を次に示します。

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>アセット

ICloud にアップロードされ、特定のレコードに関連付けられている、大規模な非構造化データのファイルの資産を許可します。

 [![](intro-to-cloudkit-images/image38.png "ICloud にアップロードされ、特定のレコードに関連付けられている、大規模な非構造化データのファイルの資産を許可します。")](intro-to-cloudkit-images/image38.png#lightbox)

クライアントを`CKRecord`作成 iCloud サーバーにアップロードするファイルをについて説明します。 A`CKAsset`を含むファイルが作成され、それを記述するレコードにリンクされます。

サーバーにファイルがアップロードされると、レコードがデータベース内に配置し、ファイルは特殊な一括ストレージ データベースにコピーします。 レコード ポインターと、アップロードされたファイルの間のリンクが作成されます。

資産が CloudKit Framework 経由で公開されている、`CKAsset`クラスし、大量の非構造化データの格納に使用します。 開発者は、大規模な非構造化データがメモリにあることはありませんが、ために、ディスク上のファイルを使用して資産が実装されます。

資産は、これにより資産のポインターとしてレコードを使用して iCloud から取得するのには、レコードによって所有されます。 この方法は、サーバーで資産を所有するレコードが削除されたときにガベージ収集資産ことができます。

`CKAssets`ものでは、大きなデータ ファイルを処理して、Apple に効率的にアップロードして資産をダウンロードする CloudKit に設計されています。

資産を作成し、レコードに関連付けるには、次のコードを使用できます。

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

説明すべて CloudKit 内の基本的なオブジェクトのようになりました。 コンテナーは、アプリケーションに関連付けられているし、データベースを格納します。 データベースには、ゾーンのレコードにグループ化され、レコードの識別子によって示されるされるレコードが含まれます。 参照を使用してレコード間の親子リレーションシップが定義されます。 最後に、大きなファイルをアップロードして資産を使用してレコードに関連付けられています。

## <a name="cloudkit-convenience-api"></a>CloudKit の利便性 API

Apple では、CloudKit を操作するための 2 つの異なる API セットが用意されています。

-  **Operational API** – CloudKit のすべての機能を提供します。 複雑なアプリケーションの場合は、この API は、CloudKit の詳細に制御を提供します。
-  **便利な API** – CloudKit の機能の共通の事前構成済みのサブセットを提供します。 CloudKit の機能を iOS アプリケーションなどの便利な簡単にアクセスできるソリューションを提供します。


利便性のための API は、ほとんどの iOS アプリケーションの最適な選択肢では通常し、Apple は、以降では、それを提案します。 このセクションの残りの部分では、次の便利な API トピックについて説明します。

-  レコードを保存しています。
-  レコードをフェッチしています。
-  レコードを更新しています。


### <a name="common-setup-code"></a>共通のセットアップ コード

CloudKit の便利な API の概要、前に、必要ないくつかの標準セットアップ コードがあります。 まず、アプリケーションの変更を`AppDelegate.cs`ファイルを開き、次のようになります。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using UIKit;
using CloudKit;

namespace CloudKitAtlas
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Computed Properties
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            application.RegisterForRemoteNotifications ();

            // Get the default public and private databases for
            // the application
            PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
            PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;

            return true;
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            Console.WriteLine ("Registered for Push notifications with token: {0}", deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications (UIApplication application, NSError error)
        {
            Console.WriteLine ("Push subscription failed");
        }

        public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
        {
            Console.WriteLine ("Push received");
        }
        #endregion
    }
}
```

上記のコードは、アプリケーションの残りの部分で操作しやすくためのショートカットとして、パブリックおよびプライベート CloudKit のデータベースを公開します。

次に、すべてのビューと CloudKit を使用するビューのコンテナーに次のコードを追加します。

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

取得するショートカットが追加されます、`AppDelegate`上記で作成したパブリックおよびプライベートのデータベースのショートカットにアクセスします。

配置でこのコードでは、Xamarin iOS 8 のアプリケーションで CloudKit の利便性のための API の実装を見てをみましょう。

### <a name="saving-a-record"></a>レコードを保存

レコードを検討する場合は、上記のパターンを使用して、次のコードは新しいレコードを作成、便利な API を使用してパブリック データベースに保存します。

```csharp
private const string ReferenceItemRecordName = "ReferenceItems";
...

// Create a new record
var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;

// Save it to the database
ThisApp.PublicDatabase.SaveRecord(newRecord, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

上記のコードに関する注意事項を次の 3 つは:

1.  呼び出すことによって、`SaveRecord`のメソッド、`PublicDatabase`開発者、データの送信方法、どのゾーンが書き込まれることなどを指定する必要はありません。利便性のための API は、対処、詳細な情報のすべての自体。
1.  呼び出しは非同期であり、成功または失敗のいずれにも、呼び出しの完了時にコールバック ルーチンを提供します。 呼び出しに失敗した場合は、エラー メッセージが提供されます。
1.  CloudKit がローカル ストレージ/永続化; を提供していませんこれは、転送メディアのみです。 したがって、レコードを保存する要求が行われると、iCloud のサーバーに直ちに送信されます。


> [!NOTE]
> 「データ損失の」接続が常に破棄されるか、中断する場合に、CloudKit 処理は、エラー処理と、開発者が行う必要があります、最初の考慮事項の 1 つのモバイル ネットワーク通信の性質です。

### <a name="fetching-a-record"></a>レコードをフェッチしています

レコードの作成し、iCloud サーバーに正常に格納されていると、レコードを取得するのに次のコードを使用します。

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

レコードの保存と同じように、上記のコードは非同期の単純なはされ、優れたエラー処理が必要です。

### <a name="updating-a-record"></a>レコードを更新します。

ICloud サーバーからフェッチされたレコードを後、は、レコードを変更し、変更をデータベースに保存する次のコードを使用できます。

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {

    } else {
        // Modify the record
        record["name"] = (NSString)"New Name";

        // Save changes to database
        ThisApp.PublicDatabase.SaveRecord(record, (r, e) => {
            // Was there an error?
            if (e != null) {
                 ...
            }
        });
    }
});
```

`FetchRecord`のメソッド、`PublicDatabase`を返します、`CKRecord`呼び出しが成功した場合。 その後、アプリケーションは、レコードおよび呼び出しを変更します。 `SaveRecord` 、変更をデータベースに書き戻すをもう一度です。

このセクションでは、CloudKit の利便性のための API を使用する場合、アプリケーションが使用する一般的なサイクルを説明しました。 アプリケーションは、iCloud へのレコードを保存、iCloud からこれらのレコードを取得、レコードを変更、および iCloud にそれらの変更が保存されます。

## <a name="designing-for-scalability"></a>スケーラビリティのための設計

これまでにこの記事が検索の保存とされるたびにこれは、iCloud のサーバーから、アプリケーションの全体のオブジェクト モデルを取得します。 このアプローチは、少量のデータと非常に小さいユーザー ベースでは、中に、スケーラビリティが得られない情報やユーザーの量が増加をベースとします。

### <a name="big-data-tiny-device"></a>ビッグ データ、小型のデバイス

一般的なアプリケーションが、データベース内の複数のデータと、その全体のデータ、デバイス上のキャッシュしておくが可能な少なくなり、します。 この問題を解決するために、次の手法を使用できます。

-  **クラウドで大規模なデータを保持**– CloudKit が大規模なデータを効率的に処理するために設計されています。
-  **クライアントがそのデータのスライスを表示する必要がありますのみ**– 特定の時点で任意のタスクを処理するために必要なデータの最低限を停止します。
-  **クライアントのビューを変更できます**: 各ユーザーがさまざまな環境設定、表示されているデータのスライスがユーザーからユーザーを変更、および、ユーザーが個々 のため、特定のスライスのビューが異なるなることができます。
-  **クライアントでは、クエリを使用して、視点を集中**– クエリは、クラウド内に存在する大規模なデータセットの小さなサブセットを表示するユーザーを許可します。


### <a name="queries"></a>クエリ

前述のように、クエリを使用すると、開発者をクラウドに存在する大規模なデータセットの小さなサブセットを選択します。 クエリが CloudKit Framework 経由で公開されている、`CKQuery`クラス。

クエリが 3 つの異なる動作を組み合わせた: レコードの種類 ( `RecordType`)、述語 ( `NSPredicate`) と、必要に応じて、並べ替え記述子 ( `NSSortDescriptors`)。 CloudKit のほとんどをサポートする`NSPredicate`します。

#### <a name="supported-predicates"></a>サポートされている述語

CloudKit の次の種類をサポートしている`NSPredicates`クエリを使用する場合。


1. 一致するレコードの名前は、変数に格納されている値。
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. キーは、コンパイル時に認識する必要があるないように、動的なキー値に基づいて一致を使用できます。
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. 一致するレコードの値が指定された値より大きいレコード。
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. 一致するレコード内の特定の場所の 100 m でレコードの場所は。
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit では、トークン化された検索をサポートします。 この呼び出しは 1 つずつ、2 つのトークンを作成`after`し、もう`session`します。 これら 2 つのトークンを含むレコードが返されます。
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. CloudKit のサポートを使用して結合述語の複合、`AND`演算子。
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>クエリの作成

次のコードは、作成に使用できる、 `CKQuery` Xamarin iOS 8 のアプリケーションで。

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

最初に、指定した名前と一致するレコードのみを選択する述語を作成します。 指定されたレコード型の述語に一致するレコードを選択するクエリを作成します。

#### <a name="performing-a-query"></a>クエリを実行します。

クエリが作成されたら、次のコードを使用して、クエリを実行し、返されるレコードの処理します。

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = {0}", recordName));
var query = new CKQuery("CloudRecords", predicate);

ThisApp.PublicDatabase.PerformQuery(query, CKRecordZone.DefaultRecordZone().ZoneId, (NSArray results, NSError err) => {
    // Was there an error?
    if (err != null) {
       ...
    } else {
        // Process the returned records
        for(nint i = 0; i < results.Count; ++i) {
            var record = (CKRecord)results[i];
        }
    }
});
```

上記のコードは、上記で作成したクエリを受け取り、Public データベースに対して実行します。 ゾーンのレコードが指定されていないために、すべてのゾーンが検索されます。 エラーが発生しなかった場合、配列の`CKRecords`がクエリのパラメーターに一致するが返されます。

クエリについて考慮する方法では、アンケート、されていては、大規模なデータセットのスライスにあることです。 クエリでは、次の理由により、主に静的な大規模なデータセットに適してはできません。

-  デバイスのバッテリ寿命の不適切なです。
-  これらは、ネットワーク トラフィックが正しくありません。
-  表示情報は、アプリケーションがデータベースをポーリングする頻度によって制限されますので、ユーザー エクスペリエンスの不適切なです。 ユーザーは、変更が生じた場合、プッシュ通知を今すぐ期待します。


### <a name="subscriptions"></a>サブスクリプション

主に静的な大規模なデータセットを扱う場合、クライアント デバイスで、クエリを実行すべき、クライアントの代わりに、サーバー上に実行する必要があります。 クエリでは、バック グラウンドで実行する必要があり、現在のデバイスまたは別のデバイスが同じデータベースの手を加えることによって、すべて 1 つのレコードを保存した後に実行する必要があります。

最後に、サーバー側クエリの実行時に、データベースに接続されているすべてのデバイスにプッシュ通知を送信する必要があります。

サブスクリプションが CloudKit Framework 経由で公開されている、`CKSubscription`クラス。 レコードの種類を組み合わせることができ ( `RecordType`)、述語 ( `NSPredicate`) や、Apple Push Notification ( `Push`)。

> [!NOTE]
> CloudKit など、プッシュが発生する原因の特定の情報を含むペイロードが含まれていると、CloudKit プッシュは少し拡張されました。

#### <a name="how-subscriptions-work"></a>サブスクリプションのしくみ

サブスクリプションを実装する前にC#コードは、サブスクリプションの機能の簡単な概要を見てみましょう。

 [![](intro-to-cloudkit-images/image39.png "サブスクリプションの機能の概要")](intro-to-cloudkit-images/image39.png#lightbox)

上記のグラフは、次のようにサブスクリプションの一般的なプロセスを示しています。

1.  クライアント デバイスでは、サブスクリプションと、トリガーが発生したときに送信されるプッシュ通知をトリガーする条件のセットを含む新しいサブスクリプションを作成します。
2.  サブスクリプションは、既存のサブスクリプションのコレクションに追加されたが、データベースに送信されます。
3.  2 番目のデバイスでは、新しいレコードを作成し、そのレコードをデータベースに保存します。
4.  データベースは、新しいレコードが、その条件のいずれかと一致するかどうかに表示するサブスクリプションの一覧を検索します。
5.  一致が見つかった場合は、プッシュ通知がトリガーされるようにその原因となったレコードに関する情報を含むサブスクリプションを登録するデバイスに送信されます。


この知識では、Xamarin iOS 8 のアプリケーションでサブスクリプションを作成を見てみましょう。

#### <a name="creating-subscriptions"></a>サブスクリプションを作成します。

次のコードは、サブスクリプションの作成に使用できます。

```csharp
// Create a new subscription
DateTime date;
var predicate = NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date));
var subscription = new CKSubscription("RecordType", predicate, CKSubscriptionOptions.FiresOnRecordCreation);

// Describe the type of notification
var notificationInfo = new CKNotificationInfo();
notificationInfo.AlertLocalizationKey = "LOCAL_NOTIFICATION_KEY";
notificationInfo.SoundName = "ping.aiff";
notificationInfo.ShouldBadge = true;

// Attach the notification info to the subscription
subscription.NotificationInfo = notificationInfo;
```

最初に、サブスクリプションをトリガーする条件を提供する述語を作成します。 次に、特定レコードの種類に対して、サブスクリプションを作成し、トリガーをテストするときのオプションを設定します。 最後に、サブスクリプションがトリガーされ、サブスクリプションに接続されるときに発生する通知の種類を定義します。

### <a name="saving-subscriptions"></a>サブスクリプションを保存しています

作成されたサブスクリプションで、次のコードはデータベースに保存します。

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

利便性のための API を使用して、呼び出しは非同期、単純と、簡単なエラー処理を提供します。

#### <a name="handling-push-notifications"></a>プッシュ通知の処理

開発者は、Apple プッシュ通知 (AP) を使用したが場合、その CloudKit で生成された通知の処理のプロセスはよくあります。

`AppDelegate.cs`、オーバーライド、`ReceivedRemoteNotification`クラスの次のようにします。

```csharp
public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
{
    // Parse the notification into a CloudKit Notification
    var notification = CKNotification.FromRemoteNotificationDictionary (userInfo);

    // Get the body of the message
    var alertBody = notification.AlertBody;

    // Was this a query?
    if (notification.NotificationType == CKNotificationType.Query) {
        // Yes, convert to a query notification and get the record ID
        var query = notification as CKQueryNotification;
        var recordID = query.RecordId;
    }
}
```

上記のコードでは、CloudKit 通知に、ユーザー情報を解析する CloudKit を確認します。 次に、アラートに関する情報が抽出されます。 最後に、通知の種類をテストし、通知がそれに応じて処理します。

このセクションでは、ビッグ データ、クエリとサブスクリプションを使用して上で示した小型のデバイスの問題に回答する方法を説明しました。 アプリケーションは、クラウドで大規模なデータのままにし、これらのテクノロジを使用して、このデータセットにビューを提供します。

## <a name="cloudkit-user-accounts"></a>CloudKit のユーザー アカウント

この記事の冒頭でメモ、CloudKit iCloud の既存のインフラストラクチャ上にビルドされます。 次のセクションでは取り上げ、詳細、CloudKit API を使用して開発者アカウントを公開する方法。

### <a name="authentication"></a>認証

ユーザー アカウントを扱う場合、最初の考慮事項は認証です。 CloudKit は、デバイスを iCloud にログインして現在のユーザーを使用して認証をサポートします。 認証は、バック グラウンドで行わを受け取り、iOS によって処理されます。 これにより、開発者は決して認証の実装の詳細について心配する必要です。 テストして、ユーザーがログオンになっているかどうかのみ。

### <a name="user-account-information"></a>ユーザー アカウント情報

CloudKit では、開発者に次のユーザー情報を提供します。

-  **Identity** – ユーザーを一意に識別する方法。
-  **メタデータ**– を保存し、ユーザーに関する情報を取得する機能。
-  **プライバシー** – すべての情報はプライバシーに関する意識の高い manor で処理されます。 ユーザーがそれに同意した場合を除き、何も公開されます。
-  **探索**– ユーザーに、同じアプリケーションを使用している友人を検出する機能を提供します。


次に、これらのトピックの詳細を紹介します。

#### <a name="identity"></a>同一。

前述のように、CloudKit は、アプリケーション、特定のユーザーを一意に識別するための手段を提供します。

 [![](intro-to-cloudkit-images/image40.png "特定のユーザーを一意に特定")](intro-to-cloudkit-images/image40.png#lightbox)

CloudKit のコンテナー内で特定のユーザーの秘密のデータベースのすべてのユーザーのデバイスで実行されているクライアント アプリケーションがあります。 クライアント アプリケーションが、特定のユーザーのいずれかにリンクしようとしています。 これは、デバイスのローカルで iCloud にログインしているユーザーに基づきます。

ICloud これからは、バックアップ ストアのユーザー情報豊富なは。 ユーザーを関連付けることができる iCloud は、コンテナーを実際にホストされているためです。 上記の図で、ユーザーの iCloud アカウント`user@icloud.com`現在のクライアントにリンクさせます。

コンテナーのコンテナーごとに、ランダムに生成された一意のユーザー ID が作成され、ユーザーの iCloud アカウント (電子メール アドレス) に関連付けられています。 このユーザー ID は、アプリケーションに返され、開発者が収まるように何らかの方法で使用することができます。

> [!NOTE]
> CloudKit の異なるコンテナーに接続されているために、同じ iCloud ユーザーに対して、同じデバイスで実行されている別のアプリケーションは別のユーザー Id があります。

次コードを取得 CloudKit のユーザー ID に現在ログオンしているデバイスで iCloud ユーザー:

```csharp
public CKRecordID UserID { get; set; }
...

// Get the CloudKit User ID
CKContainer.DefaultContainer.FetchUserRecordId ((recordID, err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}", err.LocalizedDescription);
    } else {
        // Save user ID
        UserID = recordID;
    }
});
```

上記のコードは、現在ログインしているユーザーの ID を指定する CloudKit のコンテナーを要求しています。 以降、この情報は iCloud サーバーから、呼び出しが非同期とエラー処理が必要です。

#### <a name="metadata"></a>メタデータ

CloudKit 内の各ユーザーが、それらを記述するメタデータを特定します。 このメタデータは CloudKit レコードとして表されます。

 [![](intro-to-cloudkit-images/image41.png "CloudKit 内の各ユーザーがそれらを記述する特定のメタデータ")](intro-to-cloudkit-images/image41.png#lightbox)

コンテナーを特定のユーザーを探して、プライベートのデータベース内では、そのユーザーを定義する 1 つのレコードです。 コンテナーのユーザーごとに 1 つのパブリックのデータベース内の多数のユーザー レコードがあります。 現在ログオンしているユーザーのレコード ID と一致するレコード ID のいずれかになります

Public データベース内のユーザー レコードは、読み取り可能な世界です。 ほとんどの場合、通常のレコードとして、扱われ、タイプは、`CKRecordTypeUserRecord`します。 これらのレコードは、システムによって予約されており、クエリでは使用できません。

ユーザー レコードにアクセスするのにには、次のコードを使用します。

```csharp
public CKRecord UserRecord { get; set; }
...

// Get the user's record
PublicDatabase.FetchRecord(UserID, (record ,er) => {
    //was there an error?
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Save the user record
        UserRecord = record;
    }
});
```

上記のコードは上記にアクセスしました ID であるユーザーのユーザー レコードが返されるパブリック データベースを要求しています。 以降、この情報は iCloud サーバーから、呼び出しが非同期とエラー処理が必要です。

#### <a name="privacy"></a>プライバシー

CloudKit が、既定では、デザインは、現在ログオンしているユーザーのプライバシーを保護します。 既定で公開されているユーザーの個人を特定できる情報はありません。 これは、場合によっては、アプリケーションがユーザーの限定された情報を必要になります。

このような場合は、アプリケーションは、ユーザーがこの情報を開示することを要求できます。 ダイアログ ボックスは、自分のアカウント情報を公開することをオプトインするかを確認するユーザーに表示されます。

#### <a name="discovery"></a>探索

により、アプリケーションにオプトインとしてユーザーには、ユーザー アカウント情報へのアクセスが制限されていると仮定アプリケーションの他のユーザーを検出できます。

 [![](intro-to-cloudkit-images/image42.png "ユーザーは、アプリケーションの他のユーザーから検出できることができます。")](intro-to-cloudkit-images/image42.png#lightbox)

クライアント アプリケーションは、コンテナーと通信し、コンテナーの情報にアクセスするユーザーの iCloud が対話します。 ユーザーが電子メール アドレスを入力し、検出を使用して、ユーザーに関する情報を取得することができます。 必要に応じて、ユーザー ID は、ユーザーに関する情報を検出するも使用できます。

CloudKit は、友人の現在ログオンしている可能性があるすべてのユーザーに関する情報を検出する方法も用意されています。 全体のアドレス帳のクエリを実行してユーザーを iCloud にします。 CloudKit プロセスは、ユーザーの連絡先の書籍でプルし、電子メール アドレスを使用して、これらのアドレスに一致するアプリケーションの他のユーザーを検索できるかどうか。

これにより、アプリケーションへのアクセスを提供することや、連絡先へのアクセスを承認するように求めることがなく、ユーザーの連絡先の書籍を活用できます。 ありませんが、連絡先情報専用にして、アプリケーションで使用できる CloudKit プロセスがあるアクセス。

要約するには 3 つの入力の種類がユーザーの探索の使用可能です。

-  **ユーザー レコード ID** – 検出は、現在ログオンしているは、ユーザー ID に対して行うことができます CloudKit ユーザー。
-  **ユーザーの電子メール アドレス**– ユーザーが電子メール アドレスを入力し、検出のために使用できます。
-  **連絡**– ユーザーのアドレス帳を使用すると、その連絡先に記載されている、同じ電子メール アドレスのあるアプリケーションのユーザーを検出します。


ユーザーの探索には、次の情報が返されます。

-  **ユーザー レコード ID** -パブリック データベース内のユーザーの一意の ID。
-  **最初と最後の名前**- Public のデータベースに格納されています。


この情報は、ユーザーの選択に探索がのみ返されます。

次のコードでは、デバイスで iCloud に現在ログインして、ユーザーに関する情報を検出します。

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
...

// Get the user's metadata
CKContainer.DefaultContainer.DiscoverUserInfo(UserID, (info, e) => {
    // Was there an error?
    if (e != null) {
        Console.WriteLine("Error: {0}", e.LocalizedDescription);
    } else {
        // Save the user info
        UserInfo = info;
    }
});
```

帳に連絡先のすべてのユーザーを照会するのにには、次のコードを使用します。

```csharp
// Ask CloudKit for all of the user's friends information
CKContainer.DefaultContainer.DiscoverAllContactUserInfos((info, er) => {
    // Was there an error
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Process all returned records
        for(int i = 0; i < info.Count(); ++i) {
            // Grab a user
            var userInfo = info[i];
        }
    }
});
```

このセクションでは、CloudKit をアプリケーションに提供できるユーザーのアカウントへのアクセスの 4 つの主要な領域について説明しました。 ユーザーの Id とメタデータを取得するには、プライバシー ポリシーに組み込まれている CloudKit と最後に、アプリケーションの他のユーザーを検出する機能。

## <a name="the-development-and-production-environments"></a>開発と運用環境

CloudKit は、アプリケーションのレコードの種類とデータの個別の開発および運用環境を提供します。 開発環境は、開発チームのメンバーでのみ使用可能なより柔軟な環境です。 アプリケーションでは、レコードに新しいフィールドを追加し、開発環境でそのレコードを保存します、ときに、サーバーが自動的にスキーマ情報を更新します。

開発者は、時間の節約の開発中にスキーマを変更するのに、この機能を使用できます。 1 つ注意点は、フィールドは、レコードに追加されたが、そのフィールドに関連付けられているデータ型変更できないことプログラムでです。 フィールドの型を変更するには、開発者が内のフィールドを削除する必要があります[CloudKit のダッシュ ボード](https://icloud.developer.apple.com/dashboard/)新しい型でもう一度追加します。

アプリケーションを展開する前に、開発者に移行できます、スキーマとデータは、実稼働環境を使用して**CloudKit のダッシュ ボード**します。 運用環境に対して実行するとき、サーバーにより、プログラムでスキーマを変更するアプリケーション。 開発者の変更を加えることができます**CloudKit のダッシュ ボード**がエラーで運用環境の結果内のレコードにフィールドを追加しようとします。

> [!NOTE]
> IOS シミュレーターはでのみ機能、**開発環境**します。 開発者がアプリケーションをテストする準備が場合、**実稼働環境**、物理 iOS デバイスが必要です。


## <a name="shipping-a-cloudkit-enabled-app"></a>アプリを有効になっている、CloudKit の配布

CloudKit を使用するアプリケーションを出荷する前に構成する必要があるターゲットに、**運用 CloudKit 環境**またはアプリケーションが Apple によって拒否されます。

次の手順で行います。

1. Ma の Visual Studio でアプリケーションをコンパイル**リリース** > **iOS デバイス**: 

    [![](intro-to-cloudkit-images/shipping01.png "リリース用のアプリケーションをコンパイルします。")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. **ビルド**メニューの **アーカイブ**: 

    [![](intro-to-cloudkit-images/shipping02.png "アーカイブを選択します。")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. **アーカイブ**が作成され、Visual studio for Mac に表示されます。 

    [![](intro-to-cloudkit-images/shipping03.png "アーカイブが作成され、表示されます。")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. **Xcode** を起動します。
5. **ウィンドウ**メニューの **オーガナイザー**: 

    [![](intro-to-cloudkit-images/shipping04.png "開催者を選択します。")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. アプリケーションのアーカイブを選択し、クリックして、**エクスポートしています.** ボタンをクリックします。 

    [![](intro-to-cloudkit-images/shipping05.png "アプリケーションのアーカイブ")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. エクスポートする方法を選択し、クリックして、**次**ボタン。 

    [![](intro-to-cloudkit-images/shipping06.png "エクスポートする方法を選択します。")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. 選択、**開発チーム** をクリックして、ドロップダウン リストから、**選択**ボタン。 

    [![](intro-to-cloudkit-images/shipping07.png "ドロップダウン リストから、開発チームを選択します。")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. 選択**運用** をクリックして、ドロップダウン リストから、**次**ボタン。 

    [![](intro-to-cloudkit-images/shipping08.png "ドロップダウン リストから運用環境を選択します。")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. 設定とクリックを確認、**エクスポート**ボタンをクリックします。 

    [![](intro-to-cloudkit-images/shipping09.png "設定を確認してください。")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. 結果として得られるアプリケーションを生成する場所を選択`.ipa`ファイル。

ITunes Connect に直接アプリケーションを送信するために似ていますクリックするだけに、**送信しています。** オーガナイザー ウィンドウで、アーカイブを選択した後に... エクスポートではなくボタンをクリックします。

## <a name="when-to-use-cloudkit"></a>CloudKit を使用する場合

この記事で述べたように、CloudKit はアプリケーションを格納および iCloud サーバーから情報を取得するための簡単な方法を提供します。 また、CloudKit 廃止されたまたはしませんの既存のツールやフレームワークを廃止します。

### <a name="use-cases"></a>ユース ケース

次のユース ケースでは、iCloud の特定のフレームワークやテクノロジを使用するときに、開発者を支援する必要があります。

-  **iCloud のキー値ストア**-非同期的に少量のデータを最新の状態は保持されアプリケーションの設定操作に最適ですが。 ただし、情報量が非常に小さいが制限されます。
-  **iCloud ドライブ**: Api のドキュメントを既存の iCloud の上に構築し、ファイル システムからの非構造化データを同期する単純な API を提供します。 Mac OS X で完全なオフライン キャッシュを提供し、ドキュメント中心のアプリケーションに適しています。
-  **iCloud Core Data** – データのすべてのユーザーのデバイス間でレプリケートすることができます。 データは、1 ユーザーとの同期保持するプライベートの構造化データに適しています。
-  **CloudKit** – 両方の構造体のパブリック データを提供しますや一括や大規模なデータセットと大規模な非構造化ファイルの両方を処理できます。 ユーザーに関連付けられたその iCloud アカウントとは、クライアントにデータ転送が送られます。


これらのユース ケースでに注意してください、開発者は、両方の現在の必要なアプリケーション機能を提供し、将来の成長の優れたスケーラビリティを提供するための正しい iCloud 技術を選択する必要があります。

## <a name="summary"></a>まとめ

この記事では、CloudKit API の概要について説明しました。 プロビジョニングおよび CloudKit を使用する Xamarin iOS アプリケーションを構成する方法が説明しました。 CloudKit の利便性のための API の機能をによってカバーされています。 表示が、CloudKit を設計する方法は、クエリとサブスクリプションを使用して拡張性のアプリケーションを有効になっています。 および CloudKit をアプリケーションに公開されているユーザー アカウント情報を示すが最後にします。

## <a name="related-links"></a>関連リンク

- [CloudKitAtlas (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
- [プロビジョニング プロファイルを作成します。](~/ios/get-started/installation/device-provisioning/index.md)
