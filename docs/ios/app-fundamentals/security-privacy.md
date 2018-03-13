---
title: "iOS のセキュリティとプライバシーの機能"
description: "この記事では、iOS および Xamarin.iOS アプリへの影響についてのセキュリティとプライバシーの作業について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 4747fb73358a60d10832a1e650acd90a5a4274d1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="ios-security-and-privacy-features"></a>iOS のセキュリティとプライバシーの機能

_この記事では、iOS および Xamarin.iOS アプリへの影響についてのセキュリティとプライバシーの作業について説明します。_

Apple には、セキュリティとプライバシーに iOS 10 (以降) の両方に、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立ついくつかの強化が行われます。 この記事では、Xamarin.iOS アプリでこれらの機能を実装することを説明します。

次のトピックで、詳しく説明します。

- [一般的な機能強化](#General-Enhancements)
- [プライベート ユーザー データにアクセスします。](#Accessing-Private-User-Data)
    - [プライバシー キーの設定](#Setting-Privacy-Keys)
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>一般的な機能強化

次の一般的な変更が 10、iOS でのセキュリティとプライバシーに加えられました。

- 一般的なデータのセキュリティ アーキテクチャ (CDSA) API は廃止されており、非対称キーを生成する SecKey API で置き換える必要があります。
- 新しい`NSAllowsArbitraryLoadsInWebContent`にキーを追加することができます、アプリの`Info.plist`ファイルし、web ページをアプリの残りの部分の Apple のトランスポート セキュリティ (ATS) 保護が有効にまだ中に正しくロードできるようになります。 詳細についてを参照してください、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)ドキュメント。
- IOS 10 や macOS で新しいクリップボード Sierra をコピーして貼り付けるデバイス間でユーザーが許可されているため、API は、クリップボード、特定のデバイスに制限して、特定の時点で自動的にクリアするタイムスタンプを記録に使用できるように拡張されました。 さらに、名前付きペーストはもう残っていませんし、共有ペースト ボード コンテナーで置き換える必要があります。
- すべての SSL/TLS 接続の RC4 対称暗号は既定では無効になります。 さらに、セキュリティで保護されたトランスポート API をサポートしていません SSLv3 をお勧め、開発者は、できるだけ早く SHA 1 および 3 des 暗号化を使用して停止します。

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>プライベート ユーザー データにアクセスします。

IOS 10 (またはそれ以降) で実行されている必要があります静的に宣言内の 1 つまたは複数のプライバシー キーを入力して特定の機能、またはユーザー情報にアクセスしようとすると、その`Info.plist`ファイルをアプリがアクセスしようとした理由をユーザーに説明します。

> [!IMPORTANT]
> **注**必要なキーはサイレント モードで終了、システムで制限された機能またはユーザーについては、のいずれかにアクセスしようとして失敗したアプリ_エラーなく_! アプリ起動した場合は iOS 10 で予期せず失敗していることを確認、必要なすべて`Info.plist`が指定されています。

次のプライバシー関連のキーは、使用可能です。

- **プライバシー - Apple の音楽の使用状況の説明**(`NSAppleMusicUsageDescription`)-により、開発者は、アプリがユーザーのメディア ライブラリにアクセスしようとした理由について説明します。
- **プライバシー - Bluetooth 周辺機器の用途説明**(`NSBluetoothPeripheralUsageDescription`)-により、開発者は、アプリがユーザーのデバイスで Bluetooth にアクセスしようとした理由について説明します。
- **プライバシー - カレンダーの使用方法の説明**(`NSCalendarsUsageDescription`)-により、開発者は、アプリがユーザーのカレンダーにアクセスしようとした理由について説明します。
- **プライバシー - カメラの使用方法の説明**(`NSCameraUsageDescription`)-により、開発者は、アプリがデバイスのカメラにアクセスしようとした理由について説明します。
- **プライバシー - 連絡先の用途説明**(`NSContactsUsageDescription`)-により、開発者は、アプリがユーザーの連絡先にアクセスしようとした理由について説明します。
- **プライバシー - 正常性の共有の使用方法の説明**(`NSHealthShareUsageDescription`)-により、開発者は、アプリがユーザーの正常性のデータにアクセスしようとした理由について説明します。 詳細については、Apple を参照してください[HKHealthStore クラス参照](https://developer.apple.com/reference/healthkit/hkhealthstore)です。
- **プライバシー - 正常性の更新プログラムの使用方法の説明**(`NSHealthUpdateUsageDescription`)-により、開発者は、アプリがユーザーの正常性のデータを編集する理由について説明します。 詳細については、Apple を参照してください[HKHealthStore クラス参照](https://developer.apple.com/reference/healthkit/hkhealthstore)です。
- **プライバシー - HomeKit 用途説明**(`NSHomeKitUsageDescription`)-により、開発者は、アプリがユーザーの HomeKit 構成データにアクセスしようとした理由について説明します。
- **プライバシー - 場所常に使用状況の説明**(`NSLocationAlwaysUsageDescription`)-により、開発者は、アプリがユーザーの所在地に常にアクセスする必要がある理由について説明します。
- [Deprecated]**プライバシー - 場所の使用方法の説明**(`NSLocationUsageDescription`)-により、開発者は、アプリがユーザーの場所にアクセスしようとした理由について説明します。 *注: iOS 8 (以降) でこのキーは廃止されました。使用して`NSLocationAlwaysUsageDescription`または`NSLocationWhenInUseUsageDescription`代わりにします。*
- **プライバシー - 場所ときに使用する使用状況の説明**(`NSLocationWhenInUseUsageDescription`)-により、開発者は、アプリが実行されているユーザーの所在地にアクセスしようとした理由について説明します。
- [Deprecated]**プライバシー - メディア ライブラリの使用方法の説明**-により、開発者は、アプリがユーザーのメディア ライブラリにアクセスしようとした理由について説明します。 *注: iOS 8 (以降) でこのキーは廃止されました。使用して`NSAppleMusicUsageDescription`代わりにします。*
- **プライバシー - マイク用途説明**(`NSMicrophoneUsageDescription`)-により、開発者は、アプリがデバイス マイクにアクセスしようとした理由について説明します。
- **プライバシー - モーション用途説明**(`NSMotionUsageDescription`)-により、開発者は、アプリがデバイスの加速度計にアクセスしようとした理由について説明します。
- **プライバシー - フォト ライブラリの使用方法の説明**(`NSPhotoLibraryUsageDescription`)-により、開発者は、アプリがユーザーの写真のライブラリにアクセスしようとした理由について説明します。
- **プライバシー - アラーム用途説明**(`NSRemindersUsageDescription`)-により、開発者は、アプリがユーザーの通知にアクセスしようとした理由について説明します。
- **プライバシー - Siri 用途説明**(`NSSiriUsageDescription`)-により、開発者は、アプリが Siri にユーザー データを送信しようとした理由について説明します。
- **プライバシー - 音声認識の使用状況の説明**(`NSSpeechRecognitionUsageDescription`)-により、開発者は、アプリがユーザー データを Apple の音声認識のサーバーに送信しようとした理由について説明します。
- **プライバシー - テレビのプロバイダーの使用方法の説明**(`NSVideoSubscriberAccountUsageDescription`)-により、開発者は、アプリがユーザーのテレビのプロバイダー アカウントにアクセスしようとした理由について説明します。

操作の詳細については`Info.plist`キー、Apple を参照してください[情報プロパティ リストのキー参照](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1)です。

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>プライバシー キーの設定

IOS 10 (以降) で HomeKit をアクセスするための次の例を実行、開発者は追加する必要があります、`NSHomeKitUsageDescription`キーをアプリの`Info.plist`ファイルし、文字列を宣言する、アプリがユーザーの HomeKit データベースにアクセスしようとした理由を提供します。 この文字列は、ユーザーが初めて実行するとき、アプリに表示されます。

[![](security-privacy-images/info01.png "例 NSHomeKitUsageDescription アラート")](security-privacy-images/info01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio の現在の Xamarin.iOS がセキュリティ拡張機能の編集をサポートしていない`Info.plist`から次の回避策が必要になるような IDE で設定します。

1. 開く、`Info.plist`外部のテキスト エディターでファイル。
2. 最後`</dict>`ノード、次のノードを追加します。 `<key>NSHomeKitUsageDescription</key>`
3. 必須の説明を提供する次のノードを追加します。 `<string>Allows the app to control HomeKit enabled devices.</string>`
4. `Info.plist`ファイルは次のようになります。 

    [![](security-privacy-images/info02vs.png "Info.plist ファイルは、次のようになります")](security-privacy-images/info02vs.png#lightbox)
4. 変更内容をファイルに保存します。
5. Visual Studio に戻り、再コンパイルのアプリ。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

プライバシー キーのいずれかを設定するには、次の操作を行います。

1. ダブルクリックして、`Info.plist`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。
2. 画面の下部に切り替え、**ソース**ビュー。
3. 新しい**エントリ**一覧にします。
4. ドロップダウン リストから秘密キーを選択します (など**プライバシー - HomeKit 用途説明**)。 

    [![](security-privacy-images/info02.png "プライバシー キーを選択します")](security-privacy-images/info02.png#lightbox)
5. アプリが特定の機能またはユーザーの情報にアクセスしようとした理由の説明を入力します。 

    [![](security-privacy-images/info03.png "説明を入力します")](security-privacy-images/info03.png#lightbox)
6. 変更内容をファイルに保存します。

-----

> [!IMPORTANT]
> **注:**の例で示した、設定に失敗した、`NSHomeKitUsageDescription`キー、`Info.plist`ファイルの場合は、アプリ_サイレント モードで障害が発生_(実行時にシステムに閉じられました) 10、iOS では実行時エラーが発生せず (または大きい)。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事が、セキュリティとプライバシーでの変更を Apple は iOS 10 および Xamarin.iOS アプリへの影響について説明します。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
