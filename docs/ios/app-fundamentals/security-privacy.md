---
title: iOS のセキュリティとプライバシーの機能
description: このドキュメントでは、セキュリティ、および iOS のプライバシーの機能について説明し、Xamarin.iOS でそれらを使用する方法について説明します。 IOS 10 とプライベート ユーザー データにアクセスする方法に加えられた更新を参照してくださいがかかります。
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 1a28bf394d29c09bfd264f03e0eea6c8b582f271
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854754"
---
# <a name="ios-security-and-privacy-features"></a>iOS のセキュリティとプライバシーの機能

_この記事では、iOS デバイスと Xamarin.iOS アプリへの影響についてのセキュリティとプライバシーの作業について説明します。_

Apple は iOS 10 (以降) でのプライバシーとセキュリティの両方に、開発者がアプリのセキュリティを強化し、エンドユーザーのプライバシーを確保に役立ついくつかの強化されています。 この記事では、Xamarin.iOS アプリでこれらの機能の実装について説明します。
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>一般的な機能強化

IOS 10 では、セキュリティとプライバシーに次の一般的な変更を加えられましたが。

- 一般的なデータのセキュリティ アーキテクチャ (CDSA) API は非推奨し、非対称キーを生成する SecKey API で置き換える必要があります。
- 新しい`NSAllowsArbitraryLoadsInWebContent`にキーを追加することができます、アプリの**Info.plist**ファイルし、web ページをアプリの残りの部分で、Apple Transport Security (ATS) の保護がまだ有効になっている、正しくロードできるようになります。 詳細についてを参照してください、[アプリ トランスポート セキュリティ](~/ios/app-fundamentals/ats.md)ドキュメント。
- IOS 10 デバイスと macOS で新しいクリップボード Sierra をコピーして貼り付けるデバイス間でユーザーが許可されているため、API は、特定のデバイスに制限して、特定の時点に自動的にオフにするタイムスタンプを記録するクリップボードを許可する拡張されました。 さらに、名前付きペーストはもう残っていませんし、共有ペースト ボード コンテナーに置き換える必要があります。
- すべての SSL/TLS 接続の RC4 対称暗号は既定で無効になりました。 さらに、トランスポートのセキュリティで保護された API をサポートしていません SSLv3 と、開発者は、SHA 1 および 3 des 暗号化を使用して、できるだけ早く停止をお勧めします。

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>プライベート ユーザー データにアクセスします。

IOS 10 (以降) で実行されているアプリ内の 1 つまたは複数のプライバシー キーを入力して特定の機能やユーザー情報にアクセスしようとするを宣言に静的にする必要があります、 **Info.plist**ユーザー、アプリが取得しようとした理由を説明するファイルアクセスします。

> [!IMPORTANT]
> 必要なキーが自動的に終了、システムによって、制限された機能またはユーザーについては、のいずれかへのアクセスを試みるときに指定しないとアプリ_エラーなし_! IOS 10 で予期せず失敗するアプリの起動時場合、いることを確認、必要なすべて**Info.plist**が指定されています。

次のプライバシーは、キーが使用可能な関連します。

- **プライバシー - Apple Music 使用状況の説明**(`NSAppleMusicUsageDescription`)-により、開発者は、アプリがユーザーのメディア ライブラリにアクセスしようとした理由について説明します。
- **プライバシー - Bluetooth 周辺機器の利用状況の説明**(`NSBluetoothPeripheralUsageDescription`)-により、開発者は、アプリがユーザーのデバイスで Bluetooth にアクセスしようとした理由について説明します。
- **プライバシー - 予定表の使用方法の説明**(`NSCalendarsUsageDescription`)-により、開発者は、アプリがユーザーの予定表にアクセスしようとした理由について説明します。
- **プライバシー - カメラの使用方法の説明**(`NSCameraUsageDescription`)-により、開発者は、アプリがデバイスのカメラにアクセスしようとした理由について説明します。
- **プライバシー - 連絡先の使用方法の説明**(`NSContactsUsageDescription`)-により、開発者は、アプリがユーザーの連絡先にアクセスしようとした理由について説明します。
- **プライバシー - ヘルスケア共有の使用状況の説明**(`NSHealthShareUsageDescription`)-により、開発者は、アプリがユーザーの正常性データにアクセスしようとした理由について説明します。 詳細については、Apple を参照してください[HKHealthStore クラス参照](https://developer.apple.com/reference/healthkit/hkhealthstore)します。
- **プライバシー - ヘルスケア更新の使用状況の説明**(`NSHealthUpdateUsageDescription`)-により、開発者は、アプリがユーザーの正常性データを編集する必要がある理由について説明します。 詳細については、Apple を参照してください[HKHealthStore クラス参照](https://developer.apple.com/reference/healthkit/hkhealthstore)します。
- **プライバシー - HomeKit の利用状況の説明**(`NSHomeKitUsageDescription`)-により、開発者は、アプリがユーザーの HomeKit の構成データにアクセスしようとした理由について説明します。
- **プライバシー - 位置の常に使用状況説明**(`NSLocationAlwaysUsageDescription`)-により、開発者は、アプリが常に、ユーザーの場所にアクセスしようとした理由について説明します。
- [非推奨]**プライバシー - 位置の使用状況の説明**(`NSLocationUsageDescription`)-により、開発者は、アプリがユーザーの場所にアクセスしようとした理由について説明します。 *注: このキーは iOS 8 (以降) で廃止されました。使用`NSLocationAlwaysUsageDescription`または`NSLocationWhenInUseUsageDescription`代わりにします。*
- **プライバシー - 場所ときに使用して利用状況の説明**(`NSLocationWhenInUseUsageDescription`)-により、開発者は、アプリが実行されているユーザーの場所にアクセスしようとした理由について説明します。
- [非推奨]**プライバシー - メディア ライブラリの使用状況の説明**-により、開発者は、アプリがユーザーのメディア ライブラリにアクセスしようとした理由について説明します。 *注: このキーは iOS 8 (以降) で廃止されました。使用`NSAppleMusicUsageDescription`代わりにします。*
- **プライバシー - マイクの利用状況の説明**(`NSMicrophoneUsageDescription`)-により、開発者は、アプリがデバイスのマイクにアクセスしようとした理由について説明します。
- **プライバシー - モーションの使用方法の説明**(`NSMotionUsageDescription`)-により、開発者は、アプリがデバイスの加速度計にアクセスしようとした理由について説明します。
- **プライバシー - フォト ライブラリの使用状況の説明**(`NSPhotoLibraryUsageDescription`)-により、開発者は、アプリがユーザーのフォト ライブラリにアクセスしようとした理由について説明します。
- **プライバシー - リマインダーの利用状況の説明**(`NSRemindersUsageDescription`)-により、開発者は、アプリがユーザーの通知にアクセスしようとした理由について説明します。
- **プライバシー - Siri の使用方法の説明**(`NSSiriUsageDescription`)-により、開発者は、アプリケーションが Siri にユーザー データを送信しようとした理由について説明します。
- **プライバシー - 音声認識の使用方法の説明**(`NSSpeechRecognitionUsageDescription`)-により、開発者は、アプリが Apple の音声認識のサーバーにユーザー データを送信する必要がある理由について説明します。
- **プライバシー - TV プロバイダーの利用状況の説明**(`NSVideoSubscriberAccountUsageDescription`)-により、開発者は、アプリがユーザーのテレビのプロバイダーのアカウントにアクセスしようとした理由について説明します。

操作の詳細については**Info.plist**キー、Apple を参照してください[情報プロパティ リスト キー参照](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1)します。

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>プライバシー キーの設定

IOS 10 (以降) で HomeKit にアクセスするは、次の例として、開発者は追加する必要があります、`NSHomeKitUsageDescription`アプリのキー **Info.plist**ファイルを開き、アプリがユーザーの HomeKit にアクセスしようとした理由宣言文字列を提供します。データベース。 この文字列は、ユーザーが初めてにアプリを実行するときに表示されます。

![アラートの例 NSHomeKitUsageDescription](security-privacy-images/info01.png "NSHomeKitUsageDescription アラートの例")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

現在の Xamarin.iOS for Visual Studio が編集をサポートしていない、 **Info.plist**マニフェスト エディターの既定の iOS 内からプライバシー キー。 代わりに、汎用の PList エディターを使用する必要はそのため、次の操作を行います。

1. 右クリックし、 **Info.plist**ファイル、**ソリューション エクスプ ローラー**選択**プログラムから開く.**.
2. 選択、**汎用 PList エディター**からファイルを開くプログラムの一覧をクリックし、 **OK**します。

    ![ジェネリックの PList エディターを選択](security-privacy-images/InfoEditorSelectionVs.png "ジェネリック PList エディターを選択します。")
3. をクリックして、 **+** ボタン、リストに新しいエントリを追加するエディターの最後の行をします。 「カスタム プロパティ」、種類に設定されたを呼び出すこの`String`と空の値。
4. プロパティ名をクリックして、ドロップダウン リストが表示されます。
5. ドロップダウン リストから秘密キーを選択します (など**プライバシー - HomeKit の利用状況の説明**)。 

    ![プライバシー キーを選択して](security-privacy-images/InfoPListEditorSelectKey.png "プライバシー キーを選択します")
6. 値列に、アプリが特定の機能またはユーザーの情報にアクセスしようとした理由の説明を入力します。 

    ![説明を入力します](security-privacy-images/InfoPListSetValue.png "説明を入力します")
7. 変更内容をファイルに保存します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

プライバシー キーのいずれかを設定するには、次の操作を行います。

1. **ソリューション エクスプローラー**で **Info.plist** ファイルをダブルクリックし、編集用に開きます。
2. 画面の下部に切り替えて、**ソース**ビュー。
3. 新しい追加**エントリ**一覧にします。
4. ドロップダウン リストから秘密キーを選択します (など**プライバシー - HomeKit の利用状況の説明**)。 

    ![プライバシー キーを選択して](security-privacy-images/info02.png "プライバシー キーを選択します")
5. アプリが特定の機能またはユーザーの情報にアクセスしようとした理由の説明を入力します。 

    ![説明を入力します](security-privacy-images/info03.png "説明を入力します")
6. 変更内容をファイルに保存します。

-----

> [!IMPORTANT]
> 設定に失敗した上で指定した例では、`NSHomeKitUsageDescription`キー、 **Info.plist**ファイルの場合は、アプリ_サイレント モードで失敗した_(実行時にシステムによって閉じられている) iOS 10 (実行とエラーなしまたはそれ以上)。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、セキュリティとプライバシーに関する変更が iOS 10 と Xamarin.iOS アプリへの影響についての Apple が行ったことについて説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
