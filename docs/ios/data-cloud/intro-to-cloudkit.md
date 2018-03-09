---
title: CloudKit
description: "iCloud Api は、ユーザーのアカウント間での自動同期のサポートの iCloud にデータを格納する iOS 8 アプリケーションを有効にします。 CloudKit を使用して、ユーザー、一貫性のある、シームレスなエクスペリエンス iCloud 対応のデバイス間でできます。 この記事では、利便性のための API を使用してアプリケーションを iOS 8 の CloudKit の有効化について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/11/2016
ms.openlocfilehash: f55620720bb986142a56de7e8602be56280006d4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="cloudkit"></a>CloudKit

_iCloud Api は、ユーザーのアカウント間での自動同期のサポートの iCloud にデータを格納する iOS 8 アプリケーションを有効にします。CloudKit を使用して、ユーザー、一貫性のある、シームレスなエクスペリエンス iCloud 対応のデバイス間でできます。この記事では、利便性のための API を使用してアプリケーションを iOS 8 の CloudKit の有効化について説明します。_

CloudKit フレームワークは、そのアクセス iCloud アプリケーションの開発を簡略化します。 これには、アプリケーション データと資産の権限だけでなくアプリケーションの情報を安全に保管することの取得が含まれます。 このキットは、個人情報を共有せず、iCloud Id を持つアプリケーションへのアクセスを許可することで、匿名のレイヤーにユーザーを表示します。

開発者は、そのクライアント側アプリケーションに注目し、サーバー側のアプリケーション ロジックを記述する必要がある iCloud を使用できます。 CloudKit は、認証、プライベートとパブリック データベース、および構造化データおよび資産の記憶域サービスを提供します。

## <a name="requirements"></a>必要条件

この記事で紹介する手順を完了する、次が必要。

-  **Xcode および iOS SDK** – Apple の Xcode と iOS 8 の Api がインストールして、開発者のコンピューター上に構成する必要があります。
-  **Visual Studio for Mac** – Visual Studio for Mac の最新バージョンをインストールし、ユーザーのデバイスで構成します。
-  **iOS 8 デバイス**: テスト用の iOS 8 の最新バージョンを実行している iOS デバイス。

## <a name="what-is-cloudkit"></a>CloudKit とは何ですか。

CloudKit は、サーバーの icloud 開発者アクセスできるようにする方法です。 ICloud ドライブとライブラリのフォトを iCloud の両方の基盤を提供します。 CloudKit は、Mac OS X と Apple iOS デバイスでサポートされます。

 [ ![](intro-to-cloudkit-images/image1.png "Mac OS X と Apple iOS デバイスのどちらでも CloudKit をサポートする方法")](intro-to-cloudkit-images/image1.png)

CloudKit は、iCloud アカウント インフラストラクチャを使用します。 ICloud にデバイスでアカウントにログインしたユーザーがある場合、CloudKit はユーザーを識別する、ID を使用します。 アカウントが使用できない場合は、制限付きの読み取り専用アクセスが提供されます。

CloudKit には、パブリックおよびプライベートのデータベースの両方の概念がサポートされています。 パブリックのデータベースでは、ユーザー アクセス権を持つすべてのデータの「スープ」を提供します。 秘密のデータベースは、特定のユーザーにバインドされているプライベート データを格納するものです。

CloudKit には、構造化と一括データの両方がサポートしています。 ルーターは大きなファイルの転送をシームレスに処理できます。 CloudKit では、バック グラウンドで iCloud サーバーとの間に大きなファイルを効率的に転送する、開発者はその他のタスクに重点を解放することが行われます。

> [!NOTE]
> **注:** CloudKit は、することが重要な_トランスポート テクノロジ_です。 すべての永続化は提供されていません送信して、サーバーから情報を効率的に受信アプリケーションにのみ有効にします。

現時点では、Apple は最初に、提供 CloudKit 無料の帯域幅と記憶域の両方の容量の上限。 大規模なプロジェクトまたはした多数のユーザー ベースのアプリケーションは、Apple のヒントが、手頃な価格スケールが行われます。


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Xamarin アプリケーションで CloudKit を有効にします。

Xamarin アプリケーションは、CloudKit フレームワークを利用できます、前に、アプリケーションを正しくプロビジョニングする必要がありますでに関する詳細、[機能を備えた作業](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)と[権利に関する作業](~/ios/deploy-test/provisioning/entitlements.md)ガイド

1.  Mac または Visual Studio の Visual Studio でプロジェクトを開きます。
2.  **ソリューション エクスプ ローラー**を開き、 **Info.plist**ファイルを確認して、**バンドル Id**で定義されているものと一致する**アプリ ID**プロビジョニングの一部の設定を作成します。
 
    [ ![](intro-to-cloudkit-images/image26a.png "バンドル Id を入力します。")](intro-to-cloudkit-images/image26a-orig.png "Info.plist file displaying Bundle Identifier")

3.  一番下まで下にスクロール、 **Info.plist**ファイルおよび選択した**バック グラウンド モードを有効になっている**、**場所の更新**と**リモート通知**:

    [ ![](intro-to-cloudkit-images/image27a.png "選択には、バック グラウンド モード、場所の更新およびリモートの通知が有効になっています。")](intro-to-cloudkit-images/image27a-orig.png "Info.plist file displaying background modes")
4.  クリックし、ソリューションに iOS プロジェクトを右クリックして**オプション**です。
5.  選択**iOS バンドル署名**を選択、**開発者 Identity**と**プロビジョニング プロファイル**上記で作成しました。
6.  確認してください、 **Entitlements.plist**が含まれています**iCloud を有効にする**、**キーと値の記憶域**と**CloudKit**です。
7.  確認してください、**偏在性に関して最近コンテナー** (上記で作成した) と、アプリケーションに存在します。 例 : `iCloud.com.your-company.CloudKitAtlas`
8.  変更内容をファイルに保存します。


これらの設定で、アプリケーションは CloudKit フレームワーク Api にアクセスする準備ができてようになりました。

## <a name="cloudkit-api-overview"></a>CloudKit API の概要

CloudKit を Xamarin iOS アプリケーションを実装する前にこの記事は、次のトピックも含まれます CloudKit フレームワークの基礎をカバーする予定です。

1.  **コンテナー** – iCloud 通信のサイロを分離します。
2.  **データベース**– Public と private は、アプリケーションに対して使用できます。
3.  **レコード**– CloudKit から構造化データが移動するメカニズムです。
4.  **レコードのゾーン**– レコードのグループです。
5.  **識別子は記録**: は完全に正規化およびレコードの特定の場所を表します。
6.  **参照**– 特定のデータベース内の関連するレコード間のリレーションシップの親と子を提供します。
7.  **資産**– の大規模な非構造化データを iCloud にアップロードされ、特定のレコードに関連付けられているファイルを許可します。


### <a name="containers"></a>コンテナー

IOS デバイスで実行されている特定のアプリケーションが常に実行されているその他のアプリケーションとそのデバイス上のサービス側のと共にします。 クライアント デバイスでは、アプリケーションは孤立または何らかの方法でセキュリティで保護されたしようとします。 場合によっては、これは、リテラルのサンド ボックスおり、他のユーザーは、アプリケーションで実行するだけは独自のメモリ領域です。

クライアント アプリケーションを取得し、他のクライアントから区切られた実行の概念は、非常に強力なし、次の利点があります。

1.  **セキュリティ**– 1 つのアプリケーションが他のクライアント アプリまたは OS 自体に干渉できません。
1.  **安定性**– 場合は、クライアント アプリケーションがクラッシュしてオペレーティング システムの他のアプリを取り除くことはできません。
1.  **プライバシー** – 各クライアント アプリケーションには、デバイス内に格納された個人情報へのアクセスが制限されます。


CloudKit は、前に挙げた、によって同じメリットを提供し、情報のクラウド ベースの操作に適用するよう設計されました。

 [ ![](intro-to-cloudkit-images/image31.png "CloudKit アプリのコンテナーを使用して通信します。")](intro-to-cloudkit-images/image31.png)

一対多の iCloud との通信をアプリケーションは、対象のアプリケーションと同じように 1 対多のデバイスで実行されているためです。 これらのさまざまな通信サイロの各コンテナーと呼ばれます。

コンテナーが CloudKit Framework 経由で公開されている、`CKContainer`クラスです。 既定では、1 つのコンテナーおよびこのコンテナーに 1 つのアプリケーションでは、そのアプリケーションのデータを分離します。 つまり、同じの iCloud アカウントに情報をいくつかのアプリケーションを格納することができますをまだこの情報は混在させることはありません。

ICloud データのコンテナリゼーション CloudKit ユーザー情報をカプセル化することもできます。 これにより、アプリケーションが iCloud アカウントにいくつかの制限付きアクセスを持つし、内で、すべてのユーザーのセキュリティとプライバシーを保護しながら、ユーザー情報が格納されています。

コンテナーは、WWDR ポータル経由でアプリケーションの開発者によって完全に管理されます。 コンテナーの名前空間は、すべての Apple の開発者の間でグローバル コンテナー必要がありますいないのみ一意である、指定された開発者向けのアプリケーションにはすべての Apple の開発者およびアプリケーションにします。

Apple では、アプリケーションのコンテナーの名前空間を作成する場合は、逆引き DNS 表記を使用してを提案します。 例:

```csharp
iCloud.com.company-name.application-name
```

コンテナーは、既定では、バインドされている特定のアプリケーションに一対一、中には、アプリケーションで共有することができます。 したがって複数のアプリケーションは、1 つのコンテナーで調整できます。 1 つのアプリケーションも複数のコンテナーに問い合わせてください。

### <a name="databases"></a>Databases

アプリケーションのデータ モデルとレプリケーションの最大 iCloud のサーバーには、そのモデルを実行すると CloudKit の主な機能の 1 つです。 一部の情報がそれを作成したユーザーを対象と、その他の情報は、一般的な使用を (レストランのレビュー) などのユーザーによって作成できるパブリック データまたはアプリケーションの開発者が発行されている情報がある可能性があります。 いずれの場合は、対象ユーザーが 1 人のユーザーだけではありませんが、人のユーザーのコミュニティは。

 [ ![](intro-to-cloudkit-images/image32.png "CloudKit コンテナー ダイアグラム")](intro-to-cloudkit-images/image32.png)

コンテナー内で最初の理由として、public データベース。 これは、すべてのパブリックの情報が住んでいる併置 mingles です。 また、特定のアプリケーションのユーザーごとに個々 のいくつかのプライベート データベースが使用されます。

IOS デバイスでの実行中、アプリケーションはのみ iCloud 現在ログオンしているユーザーの情報へのアクセスがあります。 コンテナーのアプリケーションのビューは次のようになります。

 [ ![](intro-to-cloudkit-images/image33.png "コンテナーの [アプリケーション] ビュー")](intro-to-cloudkit-images/image33.png)

Public データベースと iCloud 現在ログオンしているユーザーに関連付けられている秘密のデータベースにのみ参照できます。

データベースが CloudKit Framework 経由で公開されている、`CKDatabase`クラスです。 すべてのアプリケーションが 2 つのデータベースへのアクセス: public データベースと秘密の 1 つです。

コンテナーは、CloudKit への初期のエントリ ポイントです。 次のコードは、アプリケーションの既定のコンテナーからパブリックおよびプライベートのデータベースへのアクセスを使用できます。

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

<table cellpadding="1" cellspacing="1" border="1" width="100%">
<thead>
<tr>
<td></td>
<td>Public データベース</td>
<td>プライベートのデータベース</td>
</tr>
</thead>
<tbody>
<tr>
<td>データの種類</td>
<td>共有データ</td>
<td>現在のユーザーのデータ</td>
</tr>

<tr>
<td>クォータ</td>
<td>開発者のクォータに計上</td>
<td>ユーザーのクォータに計上</td>
</tr>

<tr>
<td>既定のアクセス許可</td>
<td>読み取り可能なワールド</td>
<td>読み取り可能なユーザー</td>
</tr>

<tr>
<td>アクセス許可の編集</td>
<td>レコード クラス レベルで iCloud ダッシュ ボードの役割</td>
<td>N/A</td>
</tr>
</tbody>
</table>

### <a name="records"></a>レコード

コンテナーは、データベース、およびデータベースには、レコードを保持します。 レコードは、CloudKit から構造化データが移動する機構です。

 [ ![](intro-to-cloudkit-images/image34.png "コンテナーは、データベース、およびデータベースには、レコードを保持")](intro-to-cloudkit-images/image34.png)

レコードが CloudKit Framework 経由で公開されている、`CKRecord`クラスは、キー/値ペアをラップします。 アプリケーション内のオブジェクトのインスタンスは等価、 `CKRecord` CloudKit にします。 さらに、各`CKRecord`はオブジェクトのクラスと同等、レコードの種類を所有します。

レコードが・ イン タイム スキーマである処理から渡されている前に、データが CloudKit に説明されているためです。 その時点から CloudKit は情報を解釈しを格納して、レコードの取得の流れを処理します。

`CKRecord`クラスには、さまざまなメタデータもサポートしています。 たとえば、レコードには、作成したときとそれを作成したユーザーに関する情報が含まれています。 レコードには、最終更新日と変更したユーザーに関する情報も含まれています。

レコードには、変更のタグの概念が含まれます。 これは、特定のレコードのリビジョンの以前のバージョンです。 変更のタグは、クライアントとサーバーの特定のレコードの同じバージョンであるかどうかのコンパクトな方法として使用されます。

前述の`CKRecords`キーと値のペアをラップして、次の種類のデータを格納して、レコードのような場合。

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


型以外に、1 つの値、レコードは、上記の種類の一覧のいずれかの同種の配列を含めることができます。

次のコードは、新しいレコードを作成し、データベースに格納を使用できます。

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>レコードのゾーン

単独で、特定のデータベース内のレコードが存在しない – レコードのグループは一緒にレコード ゾーン内に存在しています。 レコードのゾーン見なすことができますのテーブルと従来のリレーショナル データベースに。

 [ ![](intro-to-cloudkit-images/image35.png "レコードのグループは一緒にレコード ゾーン内に存在してください。")](intro-to-cloudkit-images/image35.png)

指定したレコード ゾーン内の複数のレコードと、特定のデータベース内の複数のレコード ゾーンが表示されることができます。 すべてのデータベースには、既定のゾーン レコードが含まれています。

 [ ![](intro-to-cloudkit-images/image36.png "すべてのデータベースには、レコードの既定のゾーンとカスタム ゾーンが含まれています。")](intro-to-cloudkit-images/image36.png)

これは、既定ではレコードを格納する場所です。 さらに、カスタム レコードのゾーンを作成できます。 ゾーンはどのアトミックがコミットされると変更の追跡で基本粒度は実行を記録します。

## <a name="record-identifiers"></a>レコードの識別子

レコードの識別子が、組として表される、両方を含むクライアント提供レコード名と、レコードが存在するゾーンです。 レコードの識別子では、次の特性があります。

-  クライアント アプリケーションによって作成されます。
-  それらは完全に正規化されたであり、レコードの特定の場所を表します。
-  レコード名を割り当てると、外部データベース内のレコードの一意の ID は、それらは CloudKit 内で格納されていないローカルのデータベースのブリッジに使用できます。


開発者は、新しいレコードを作成するときに、レコード Id に渡すを選択できます。 レコード識別子が指定されていない場合 UUID が自動的に作成され、レコードに割り当てられています。

開発者は、新しいレコードの識別子を作成するときに、各レコードが所属するレコードのゾーンを指定する選択できます。 指定されていない場合は、既定のレコードのゾーンが使用されます。

レコードの識別子が CloudKit Framework 経由で公開されている、`CKRecordID`クラスです。 次のコードは、新しいレコードの識別子を作成するために使用できます。

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>参照

参照は、特定のデータベース内の関連するレコード間のリレーションシップを提供します。

 [ ![](intro-to-cloudkit-images/image37.png "参考資料の入手、特定のデータベース内の関連するレコード間の関係")](intro-to-cloudkit-images/image37.png)

上記の例では、親は、子が親レコードの子レコードになるように子を所有します。 関係は、子レコードからは、親レコードに移動されと呼ばれます、*参照戻す*です。

参照が CloudKit Framework 経由で公開されている、`CKReference`クラスです。 ICloud サーバー レコード間の関係を理解することができるという方法です。

参照は、連鎖削除の背後にあるメカニズムを提供します。 親レコードが削除された場合、データベースから、(指定されたリレーションシップに) すべての子レコードはもデータベースから自動的に削除されます。

> [!NOTE]
> **注**: CloudKit を使用すると、ポインターのぶら下がりが可能性がします。 たとえば、時点では、アプリケーションがフェッチされたレコード ポインターのリスト、レコードを選択およびレコードを要求し、レコード可能性があります不要になったデータベースに存在します。 このような状況を滑らかに処理するには、アプリケーションをコーディングする必要があります。

不要にすると、参照戻すは優先 CloudKit フレームワークを使用する場合です。 Apple には、参照の最も効率的な型にする、システムを微調整ができます。

参照を作成するときに、開発者が既にメモリ内のレコードを提供かレコード識別子への参照を作成します。 データベース内に存在していないレコードの識別子、および指定された参照を使用して、未解決のポインターが作成されます。

既知のレコードの参照を作成する例を次に示します。

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>アセット

大規模な非構造化データを iCloud にアップロードされ、特定のレコードに関連付けられているファイルの資産を許可します。

 [ ![](intro-to-cloudkit-images/image38.png "大規模な非構造化データを iCloud にアップロードされ、特定のレコードに関連付けられているファイルの資産を許可します。")](intro-to-cloudkit-images/image38.png)

クライアントでは、`CKRecord`が作成される iCloud サーバーにアップロードするファイルをについて説明します。 A`CKAsset`を含むファイルが作成され、それを記述するレコードにリンクされます。

ファイルがサーバーにアップロードされると、レコードがデータベースに配置し、ファイルは、特別な一括記憶域データベースにコピーします。 レコード ポインターと、アップロードされたファイルの間のリンクが作成されます。

CloudKit Framework 経由で公開されている資産、`CKAsset`クラスを使用して、大量の非構造化データを格納します。 開発者は、大規模な非構造化データがメモリにあることはありませんが、ために、資産がディスク上のファイルを使用して実装されます。

ICloud へのポインターとしてレコードを使用してから取得する資産をにより、レコードでは、資産が所有しています。 資産を所有するレコードが削除されたときに、このように、サーバー ガベージ収集資産ことができます。

`CKAssets`ものでは大量のデータ ファイルの処理、Apple CloudKit 効率的にアップロードしてアセットをダウンロードするよう設計されています。

資産を作成し、レコードに関連付けるには、次のコードを使用できます。

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

説明すべて CloudKit 内の基本的なオブジェクトのようになりました。 コンテナーは、アプリケーションに関連付けられているし、データベースを格納します。 データベースには、レコードをゾーンにグループ化され、レコードの識別子によって示されるされるレコードが含まれます。 参照を使用してレコード間の親子リレーションシップが定義されます。 最後に、大きなファイルをアップロードおよびアセットを使用してレコードに関連付けられています。

## <a name="cloudkit-convenience-api"></a>CloudKit 便利な API

Apple には、CloudKit を操作するための 2 つの異なる API セットが用意されています。

-  **Operational API** – CloudKit のすべての 1 つの機能を提供します。 複雑なアプリケーションの場合は、この API は、CloudKit に対して、きめ細かい制御を提供します。
-  **便利な API** – CloudKit 機能の一般的なあらかじめ構成されたサブセットを提供します。 IOS アプリケーションに CloudKit 機能を含めるための便利な簡単なアクセス ソリューションを提供します。


利便性のための API は、通常ほとんどの iOS アプリケーションに最適な選択肢と Apple を提案して開始します。 このセクションの残りの部分は、次の便利な API のトピックを取り上げます。

-  レコードを保存します。
-  レコードをフェッチしています。
-  レコードを更新しています。


### <a name="common-setup-code"></a>一般的なセットアップ コード

CloudKit 利便性のための API の概要、前に必要ないくつかの標準セットアップ コードがあります。 アプリケーションの変更することによって開始`AppDelegate.cs`ファイルし、次のようになります。

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

上記のコードは、アプリケーションの残りの部分で作業しやすいようにへのショートカットとして、パブリックおよびプライベート CloudKit データベースを公開します。

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

取得するショートカットが追加されます、`AppDelegate`し、上記で作成されたパブリックおよびプライベートのデータベースのショートカットにアクセスします。

場所でこのコードでは、Xamarin iOS 8 アプリケーションで CloudKit 利便性のための API の実装を見てをみましょう。

### <a name="saving-a-record"></a>レコードを保存

レコードを検討する場合、上記のパターンを使用して、次のコードは新しいレコードを作成する API を使用して利便性のためパブリック データベースに保存します。

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

上記のコードについての注意事項を次の 3 つは:

1.  呼び出して、`SaveRecord`のメソッド、`PublicDatabase`開発者、データの送信方法に書き込まれているなど、どのようなゾーンを指定する必要はありません。便利な API がよう注意してそれらの詳細情報のすべての自体です。
1.  呼び出しは非同期であり、呼び出しが完了したら、成功または失敗のいずれにもコールバック ルーチンを提供します。 呼び出しに失敗した場合は、エラー メッセージが提供されます。
1.  CloudKit にローカル ストレージまたは永続化は提供されません。これは、転送メディアのみです。 したがって、要求がレコードを保存すると、iCloud サーバーに直ちに送信されます。


> [!NOTE]
> **注**: モバイル ネットワーク通信、接続が常に破棄されるか、またはその中断、CloudKit の操作がエラー処理の場合、開発者が行う必要があります、最初の考慮事項の 1 つの「非可逆」性質があるためです。

### <a name="fetching-a-record"></a>レコードのフェッチ

レコードの作成し、iCloud サーバーに正常に保存されていると、次のコードを使用して、レコードを取得します。

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

レコードの保存と同様には、上記のコードは、非同期、単純なされ、優れたエラー処理が必要です。

### <a name="updating-a-record"></a>レコードを更新します。

レコードが iCloud サーバーからフェッチされた後は、レコードを変更し、変更をデータベースに保存する次のコードを使用できます。

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

`FetchRecord`のメソッド、`PublicDatabase`を返します、`CKRecord`呼び出しが成功した場合。 その後、アプリケーションは、レコードと呼び出しを変更`SaveRecord`データベースへの変更の書き戻しをもう一度です。

このセクションには、CloudKit 利便性のための API を使用する場合、アプリケーションが使用する一般的なサイクルが示されています。 アプリケーションを iCloud にレコードを保存、iCloud からこれらのレコードを取得、レコードを変更、およびを iCloud にそれらの変更が保存されます。

## <a name="designing-for-scalability"></a>スケーラビリティのための設計

これまでにこの記事が検索の格納と-it で作業するたびに、iCloud サーバーから、アプリケーションのオブジェクト モデル全体を取得します。 このアプローチでは、少量のデータと非常に小さなユーザー基盤にも、中に、スケーラビリティが得られない情報やユーザーの量が増加を基準とします。

### <a name="big-data-tiny-device"></a>ビッグ データ、小さなデバイス

一般的なアプリケーションになると、データベースでの複数のデータとは低く可能なデバイスでその全体のデータのキャッシュを持っています。 次の手法は、この問題を解決するために使用できます。

-  **クラウドで大規模なデータを保持**– CloudKit が大規模なデータを効率的に処理するように設計されました。
-  **クライアントはそのデータのスライスのみを表示する必要があります**– 特定の時点でいずれかのタスクを処理するために必要なデータの最低限を停止します。
-  **クライアントのビューを変更することができます**– さまざまな環境設定は各ユーザーが、表示されるデータのスライスがユーザーを変更およびユーザーの個々 のための指定されたスライスの表示が異なる場合がします。
-  **クライアントでは、クエリを使用して、ビュー ポイントを集中**– クエリは、クラウド内に存在する大規模なデータセットの小さなサブセットを表示するユーザーを許可します。


### <a name="queries"></a>クエリ

前述のように、クエリを使用すると、クラウド内に存在する大規模なデータセットの小さなサブセットを選択する開発者です。 CloudKit Framework 経由で公開されているクエリ、`CKQuery`クラスです。

クエリが 3 つの異なる動作を組み合わせた: レコードの種類 ( `RecordType`)、述語 ( `NSPredicate`) と、必要に応じて、並べ替え記述子 ( `NSSortDescriptors`)。 CloudKit のほとんどをサポートしている`NSPredicate`です。

#### <a name="supported-predicates"></a>サポートされている述語

CloudKit の次の種類をサポートしている`NSPredicates`クエリを使用する場合。


1. 一致する名前が変数に格納されている値と等しいレコード。
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. キーは、コンパイル時におく必要があるないように、動的なキー値に基づいて、照合を使用できます。
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. 一致するレコードの値が指定された値より大きいレコード。
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. 一致するレコードのレコードの場所は、指定された場所の 100 個のメーター内。
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit には、トークン化された検索がサポートされています。 この呼び出しが 1 つずつ、2 つのトークンを作成する`after`用`session`です。 これら 2 つのトークンを含むレコードが返されます。
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. CloudKit サポートを使用して結合述語の複合、`AND`演算子。
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>クエリの作成

次のコードは、作成に使用できる、 `CKQuery` Xamarin iOS 8 アプリケーションで。

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

最初に、指定した名前に一致するレコードのみを選択する述語を作成します。 特定のレコード型の述語に一致するレコードを選択するクエリを作成します。

#### <a name="performing-a-query"></a>クエリを実行します。

クエリが作成されたら、および返されたレコードの処理をクエリを実行する次のコードを使用します。

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

上記のコードでは、上記で作成したクエリを受け取るされ、Public データベースに対して実行します。 レコードのゾーンが指定されていないために、すべてのゾーンが検索されます。 エラーが発生しなかった場合、配列の`CKRecords`クエリのパラメーターに一致するが返されます。

クエリを考慮する方法とは、こと、投票、大規模なデータセットのスライスに非常に便利です。 クエリ、次の理由のため、主に静的な大規模なデータセットの場合に適していますできません。

-  これらは、デバイスのバッテリの寿命の不適切なです。
-  これらは、ネットワーク トラフィックが正しくありません。
-  ユーザー エクスペリエンスの不適切なため、表示される情報は、アプリケーションは、データベースをポーリングする頻度によって制限されます。 変更が生じた場合、ユーザーは今日プッシュ通知を期待します。


### <a name="subscriptions"></a>サブスクリプション

主に静的な大規模なデータセットを扱う場合、クライアント デバイスで、クエリを実行すべき、クライアントの代わりに、サーバーでを実行する必要があります。 クエリでは、バック グラウンドで実行する必要があり、現在のデバイスまたは別のデバイスが、同じデータベースを変更することによって、すべてのレコードを保存した後に実行する必要があります。

最後に、サーバー側クエリの実行時に、データベースに接続されているすべてのデバイスにプッシュ通知を送信する必要があります。

サブスクリプションが CloudKit Framework 経由で公開されている、`CKSubscription`クラスです。 レコードの種類を組み合わせることができ ( `RecordType`)、述語 ( `NSPredicate`) や、Apple Push Notification ( `Push`)。

> [!NOTE]
> **注**: CloudKit 原因が発生するプッシュなどの特定の情報を含むペイロードを含めるように CloudKit プッシュが補完わずかです。

#### <a name="how-subscriptions-work"></a>サブスクリプションの機能

サブスクリプションを実装する c# コードで、前にサブスクリプションの機能の簡単な概要を見てみましょう。

 [ ![](intro-to-cloudkit-images/image39.png "サブスクリプションの機能の概要")](intro-to-cloudkit-images/image39.png)

上記のグラフでは、次のように、サブスクリプションの一般的なプロセスを示します。

1.  クライアント デバイスでは、サブスクリプションと、トリガーが発生したときに送信されるプッシュ通知をトリガーする条件のセットを含む、新しいサブスクリプションを作成します。
2.  サブスクリプションは、既存のサブスクリプションのコレクションに追加されたが、データベースに送信されます。
3.  2 番目のデバイスでは、新しいレコードを作成し、そのレコードをデータベースに保存します。
4.  データベースは、新しいレコードと一致する、条件のいずれかのサブスクリプションの一覧を検索します。
5.  一致が見つかった場合、プッシュ通知が送信が行われる原因となったレコードに関する情報を含むサブスクリプションを登録しているデバイスにします。


この知識を見てみましょう Xamarin iOS 8 のアプリケーションでサブスクリプションを作成します。

#### <a name="creating-subscriptions"></a>サブスクリプションを作成します。

次のコードは、サブスクリプションを作成するために使用できます。

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

最初に、サブスクリプションをトリガーする条件を提供する述語を作成します。 次に、特定のレコードの種類に対してサブスクリプションを作成し、トリガーをテストするときのオプションを設定します。 最後に、サブスクリプションがトリガーされ、サブスクリプションをアタッチするときに発生する通知の種類を定義します。

### <a name="saving-subscriptions"></a>サブスクリプションの保存

サブスクリプションでは、作成、次のコードは、データベースに保存します。

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

呼び出しは、利便性のための API を使用して、非同期の単純なものでは、し、簡単なエラー処理を提供します。

#### <a name="handling-push-notifications"></a>プッシュ通知の処理

開発者が Apple プッシュ通知 (AP) を使用していた場合は、CloudKit で生成された通知を処理する場合のプロセスが理解しておく必要があります。

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

上記のコードでは、CloudKit CloudKit 通知に、ユーザー情報を解析するように依頼します。 次に、アラートに関する情報が抽出されます。 最後に、通知の種類をテストし、通知がそれに応じて処理します。

このセクションでは、ビッグ データ、クエリとサブスクリプションを使用して上に示した小さなデバイスに関する問題に回答する方法を説明しました。 アプリケーションは、クラウド内の大規模なデータのままにし、これらのテクノロジを使用して、このデータセットにビューを提供します。

## <a name="cloudkit-user-accounts"></a>CloudKit ユーザー アカウント

この記事の先頭に述べたように、CloudKit は既存の iCloud インフラストラクチャ上に構築されます。 次のセクションでは、詳しく説明 CloudKit API を使用して開発者アカウントを公開する方法です。

### <a name="authentication"></a>認証

ユーザー アカウントを扱う場合、最初の考慮対象として、認証です。 CloudKit は、デバイスを iCloud 現在のログイン ユーザーを使用して認証をサポートします。 認証では、バック グラウンドで行われると、iOS によって処理されます。 これにより、開発者は、しない認証の実装の詳細について考慮する必要です。 テストして、ユーザーがログオンになっているかどうかのみ。

### <a name="user-account-information"></a>ユーザー アカウント情報

CloudKit は、開発者に次のユーザー情報を提供します。

-  **Identity** – ユーザーを一意に識別する方法です。
-  **メタデータ**– に保存したりユーザーに関する情報を取得します。
-  **プライバシー** – すべての情報のプライバシーに関する意識の高い manor で処理します。 ユーザーがそれに同意しない限り、何も公開されます。
-  **探索**– 同じアプリケーションを使用してその友達を検出することができます。


次に、これらのトピックで詳細に見ていきます。

#### <a name="identity"></a>同一。

前述のように、CloudKit は、アプリケーション、特定のユーザーを一意に識別するための手段を提供します。

 [ ![](intro-to-cloudkit-images/image40.png "特定のユーザーを一意に消費")](intro-to-cloudkit-images/image40.png)

ユーザーのデバイスとすべての CloudKit コンテナー内の特定のユーザー秘密データベースで実行されているクライアント アプリケーションがあります。 クライアント アプリケーションが特定のユーザーのいずれかにリンクしようとしています。 これは、デバイスのローカルに iCloud にログインしているユーザーに基づいています。

ICloud が由来、ためには豊富なユーザー情報のストアをバックアップします。 ユーザーを関連付けることができます iCloud は、コンテナーを実際にホストされているためです。 上記の図では、ユーザーの iCloud アカウント`user@icloud.com`は、現在のクライアントにリンクします。

コンテナーでコンテナーごとに、ランダムに生成される一意のユーザー ID が作成され、ユーザーの iCloud アカウント (電子メール アドレスなど) に関連付けられています。 このユーザー ID は、アプリケーションに返され、開発者が適した任意の方法で使用できます。

> [!NOTE]
> **注**: CloudKit の異なるコンテナーに接続されているため、同じ iCloud ユーザーは別のユーザー Id を持っては、同じデバイスで実行されている別のアプリケーションです。

次コードを取得 CloudKit ユーザー ID を現在ログオンしているデバイスのユーザーの iCloud に:

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

上記のコードは、現在ログインしているユーザーの ID を提供する CloudKit コンテナーを要求しています。 ICloud サーバーからこの情報が送られて、ため、呼び出しが非同期とエラー処理が必要です。

#### <a name="metadata"></a>メタデータ

CloudKit 内の各ユーザーが、それらを表すメタデータを特定します。 このメタデータは、CloudKit レコードとして表されます。

 [ ![](intro-to-cloudkit-images/image41.png "CloudKit 内の各ユーザーがそれらを表す特定のメタデータ")](intro-to-cloudkit-images/image41.png)

探している秘密のデータベース内のコンテナーがある特定のユーザーは、そのユーザーを定義する 1 つのレコードです。 コンテナーのユーザーごとに 1 つのパブリックのデータベース内の多数のユーザー レコードがあります。 現在ログオンしているユーザーのレコード ID に一致するレコード ID のいずれかになります

Public データベース内のユーザー レコードは、読み取り可能なワールドです。 扱われ、ほとんどの場合、通常のレコードの型を持つ`CKRecordTypeUserRecord`します。 これらのレコードは、システムによって予約されており、クエリでは使用できません。

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

上記のコードには、上にアクセスした id を指定したユーザーのユーザー レコードを返されるパブリック データベースが要求します。 ICloud サーバーからこの情報が送られて、ため、呼び出しが非同期とエラー処理が必要です。

#### <a name="privacy"></a>プライバシー

CloudKit が既定では、デザインは、現在ログオンしているユーザーのプライバシーを保護しました。 既定では、ユーザーの個人を特定できる情報が公開されていません。 これは、場合によっては、アプリケーションがユーザーに関する限られた情報を必要になります。

このような場合は、アプリケーションは、ユーザーがこの情報を開示することを要求できます。 ダイアログ ボックスは、アカウント情報の漏洩をオプトインするよう求めるユーザーに表示されます。

#### <a name="discovery"></a>探索

オプトインして、アプリケーションを許可する場合にユーザーには、各自のユーザー アカウント情報へのアクセスが制限されていると仮定すると、アプリケーションの他のユーザーを探索可能なできます。

 [ ![](intro-to-cloudkit-images/image42.png "ユーザーは、アプリケーションの他のユーザーを探索可能なできます。")](intro-to-cloudkit-images/image42.png)

クライアント アプリケーションとの対話は、コンテナーにし、コンテナーが情報にアクセスするユーザーの iCloud を説明します。 ユーザーを電子メール アドレスを提供し、探索を使用して、ユーザーに関する情報を取得することができます。 必要に応じて、ユーザー ID は、ユーザーに関する情報を探索するも使用できます。

CloudKit は、現在ログオンしている友人があります。 すべてのユーザーに関する情報を検出する方法も用意されています。 全体のアドレス帳のクエリを実行してユーザーを iCloud にします。 CloudKit プロセスは、ユーザーの連絡先のブックに取り込むし、電子メール アドレスを使用して、これらのアドレスに一致するアプリケーションの他のユーザーを検索できるかどうかです。

これにより、アプリケーションへのアクセスを提供することや、ユーザー、連絡先へのアクセスを承認するかを確認せず、ユーザーの連絡先の書籍を活用できます。 ありませんが、連絡先情報、アプリケーションで使用できる専用に CloudKit プロセスがアクセス権を持ちます。

要約するには 3 つの入力の種類がユーザーの探索に使用できます。

-  **ユーザー レコード ID** – 現在ログオンしているユーザー ID に対して検出を実行できます CloudKit ユーザー。
-  **ユーザーの電子メール アドレス**– ユーザーが電子メール アドレスを指定し、検出のために使用できます。
-  **連絡**– ユーザーのアドレス帳を使用するを連絡先に記載されているのと同じ電子メール アドレスを持つ、アプリケーションのユーザーを検出します。


ユーザーの探索には、次の情報が返されます。

-  **ユーザー レコード ID** -Public のデータベース内のユーザーの一意の ID。
-  **最初と最後の名前**- Public のデータベースに格納されています。


この情報が、選択に探索を持つユーザーにのみ返されます。

次のコードは、デバイスを iCloud に現在ログオンしているユーザーに関する情報が探索されます。

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

帳に連絡先のすべてのユーザーのクエリを実行するのにには、次のコードを使用します。

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

このセクションでは CloudKit をアプリケーションに提供できる、ユーザーのアカウントへのアクセスの 4 つの主要な領域を説明します。 ユーザーの Id とメタデータを取得するには、プライバシー ポリシーに組み込まれています CloudKit、さらに、アプリケーションの他のユーザーを検出できるようにします。

## <a name="the-development-and-production-environments"></a>開発と実稼働環境

CloudKit は、アプリケーションのレコードの種類とデータを別の開発および運用環境を提供します。 開発環境は、開発チームのメンバーにのみ使用可能なより柔軟な環境です。 アプリケーションでのレコードに新しいフィールドを追加、開発環境でそのレコードを保存して場合、サーバーは、スキーマ情報を自動的に更新します。

開発者は、時間の節約の開発中にスキーマを変更するのにこの機能を使用できます。 1 つの注意点は、フィールドは、レコードに追加されたが、そのフィールドに関連付けられているデータ型変更できないことプログラムです。 フィールドの型を変更するには、開発者が内のフィールドを削除する必要があります[CloudKit ダッシュ ボード](https://icloud.developer.apple.com/dashboard/)し、新しい種類を再度追加します。

アプリケーションを展開する前に、開発者に移行できます、スキーマとデータ実稼働環境を使用して、 **CloudKit ダッシュ ボード**です。 実行すると、実稼働環境に対して、サーバーにより、プログラムでスキーマを変更するアプリケーション。 開発者がで変更を加えることができますも**CloudKit ダッシュ ボード**がエラーで実稼働環境の結果のレコードにフィールドを追加しようとします。

> [!NOTE]
> **注:** 、iOS シミュレーターでのみ機能、**開発環境**です。 開発者がアプリケーションをテストする準備がの場合、**実稼働環境**、物理的な iOS デバイスが必要です。


## <a name="shipping-a-cloudkit-enabled-app"></a>アプリを有効になっている、CloudKit の配布

CloudKit を使用するアプリケーションを配布する前に構成する必要があるターゲットに、**運用 CloudKit 環境**またはアプリケーションは Apple によって拒否されます。

次の手順で行います。

1. Ma 用 Visual Studio でのアプリケーションのコンパイル**リリース** > **iOS デバイス**: 

    [![](intro-to-cloudkit-images/shipping01.png "リリースのアプリケーションをコンパイルします。")](intro-to-cloudkit-images/shipping01.png)

2. **ビルド**メニューの **アーカイブ**: 

    [![](intro-to-cloudkit-images/shipping02.png "アーカイブ を選択します。")](intro-to-cloudkit-images/shipping02.png)

3. **アーカイブ**が作成され、Mac を Visual Studio で表示されます。 

    [![](intro-to-cloudkit-images/shipping03.png "アーカイブが作成され、表示されます。")](intro-to-cloudkit-images/shipping03.png)

4. **Xcode** を起動します。
5. **ウィンドウ**メニューの **オーガナイザー**: 

    [![](intro-to-cloudkit-images/shipping04.png "開催者を選択します。")](intro-to-cloudkit-images/shipping04.png)

6. アプリケーションのアーカイブを選択し、クリックして、**をエクスポートしています.**ボタンをクリックします。 

    [![](intro-to-cloudkit-images/shipping05.png "アプリケーションのアーカイブ")](intro-to-cloudkit-images/shipping05.png)
    
7. エクスポートできる方法を選択し、クリックして、**次**ボタン。 

    [![](intro-to-cloudkit-images/shipping06.png "エクスポートする方法を選択します。")](intro-to-cloudkit-images/shipping06.png)

8. 選択、**開発チーム**クリックしてドロップダウン リストから、**選択**ボタンをクリックします。 

    [![](intro-to-cloudkit-images/shipping07.png "ドロップダウン リストから、開発チームを選択します。")](intro-to-cloudkit-images/shipping07.png)

9. 選択**運用**クリックしてドロップダウン リストから、**次**ボタン。 

    [![](intro-to-cloudkit-images/shipping08.png "ドロップダウン リストから実稼働環境を選択します。")](intro-to-cloudkit-images/shipping08.png)

10. クリックして、設定を確認、**エクスポート**ボタンをクリックします。 

    [![](intro-to-cloudkit-images/shipping09.png "設定を確認します。")](intro-to-cloudkit-images/shipping09.png)

11. 結果として得られるアプリケーションを生成する場所を選択して`.ipa`ファイル。

プロセスは、アプリケーションを iTunes Connect に直接送信するためのようなをクリックするだけ、**送信しています.**オーガナイザー ウィンドウで、アーカイブを選択した後... エクスポートではなくボタンをクリックします。

## <a name="when-to-use-cloudkit"></a>CloudKit を使用する場合

この記事でこれまで見てきたよう CloudKit は、アプリケーションを格納および iCloud のサーバーから情報を取得する簡単な方法を提供します。 つまり、CloudKit 旧式したりしない既存のツールまたはフレームワークのいずれかを非推奨。

### <a name="use-cases"></a>ユース ケース

使用例を次には、開発者が特定の iCloud フレームワークまたはテクノロジを使用するタイミングを決定を支援する必要があります。

-  **iCloud キー値ストア**– 非同期的に少量のデータを最新の状態に維持しはアプリケーションの設定を操作するために役立ちます。 ただし、非常に少量の情報に制限されます。
-  **iCloud ドライブ**– ドキュメント Api を既存の iCloud 上に構築され、ファイル システムからの非構造化データを同期する単純な API を提供します。 Mac OS X の完全なオフライン キャッシュを提供し、ドキュメント中心のアプリケーションに最適です。
-  **iCloud コア データ**– データをすべてのユーザーのデバイス間でレプリケートされるを許可します。 データは、シングル ユーザーと同期して保持するプライベート、構造化されたデータに最適です。
-  **CloudKit** – 両方の構造体のパブリック データを提供や一括や大規模なデータセットと大規模な非構造化ファイルの両方を処理できます。 ユーザーに関連付けられたその iCloud アカウントとは、データ転送にクライアントが誘導されました。


これらケースを念頭に、開発者は、両方現在必要なアプリケーションの機能を提供し、将来の成長の優れたスケーラビリティを提供するための正しい iCloud 技術を選択する必要があります。

## <a name="summary"></a>まとめ

この記事では、概要については、CloudKit API について説明しました。 これには、プロビジョニングして CloudKit を使用する Xamarin iOS アプリケーションを構成する方法が示されてます。 CloudKit 利便性のための API の機能について説明しています。 表示が、CloudKit を設計する方法は、クエリとサブスクリプションを使用して拡張性のアプリケーションを有効になっています。 および CloudKit によってアプリケーションに公開されているユーザー アカウント情報を示すが最後にします。

## <a name="related-links"></a>関連リンク

- [CloudKitAtlas (サンプル)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [iOS 8 の概要](~/ios/platform/introduction-to-ios8.md)
- [プロビジョニング プロファイルを作成します。](~/ios/get-started/installation/device-provisioning/index.md)
