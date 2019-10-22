---
title: iOS のセキュリティとプライバシーの機能
description: このドキュメントでは、iOS のセキュリティとプライバシーの機能について説明し、Xamarin iOS での使用方法について説明します。 IOS 10 で行われた更新と、プライベートユーザーデータへのアクセス方法について説明します。
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: 7847148551c20dbcf49bcc263bdc50716a6ef14e
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70283172"
---
# <a name="ios-security-and-privacy-features"></a>iOS のセキュリティとプライバシーの機能

_この記事では、iOS でのセキュリティとプライバシーの使用方法と、それらが Xamarin iOS アプリに与える影響について説明します。_

Apple は、iOS 10 (およびそれ以降) のセキュリティとプライバシーの両方に対していくつかの機能強化を行っており、開発者がアプリのセキュリティを向上させ、エンドユーザーのプライバシーを確保するのに役立ちます。 この記事では、これらの機能を Xamarin iOS アプリに実装する方法について説明します。

<a name="General-Enhancements" />

## <a name="general-enhancements"></a>全般的な機能強化

IOS 10 のセキュリティとプライバシーには、次の一般的な変更が加えられています。

- Common Data Security Architecture (CDSA) API は非推奨とされており、非対称キーを生成するには SecKey API で置き換える必要があります。
- 新しい `NSAllowsArbitraryLoadsInWebContent` キーをアプリの**情報 plist**ファイルに追加すると、Apple Transport SECURITY (ATS) 保護がアプリの残りの部分で有効になっている間も web ページを正しく読み込むことができます。 詳細については、[アプリトランスポートのセキュリティ](~/ios/app-fundamentals/ats.md)に関するドキュメントを参照してください。
- IOS 10 と macOS Sierra の新しいクリップボードを使用すると、デバイス間でコピーと貼り付けを行うことができます。そのため、特定のデバイスに対してクリップボードを制限し、特定の時点で自動的に消去されるようにタイムスタンプを設定できるように、API が拡張されています。 また、名前付き pasteboards は保持されなくなったため、共有のペーストボードコンテナーに置き換える必要があります。
- すべての SSL/TLS 接続では、RC4 対称暗号が既定で無効になっています。 また、セキュリティで保護されたトランスポート API は SSLv3 をサポートしなくなりました。開発者は、できるだけ早く SHA-1 と3DES 暗号化の使用を停止することをお勧めします。

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>プライベートユーザーデータへのアクセス

IOS 10 (またはそれ以降) で実行されているアプリでは、特定の機能またはユーザー情報にアクセスする目的を静的に宣言する必要があります。そのためには、アプリがアクセスする必要がある理由をユーザーに説明する**情報 plist**ファイルに1つ以上のプライバシーキーを入力します。

> [!IMPORTANT]
> 必要なキーの提供に失敗したアプリは、制限された機能またはユーザー情報のいずれかにアクセスしようとしたときに、システムによって自動的に終了されます。_エラーは発生しませ_ん。 IOS 10 でアプリの起動が予期せず失敗した場合は、必要なすべての**情報**が指定されていることを確認してください。

次のプライバシー関連キーを使用できます。

- **プライバシー-Apple Music 使用状況の説明**(`NSAppleMusicUsageDescription`)-開発者は、アプリがユーザーのメディアライブラリにアクセスする理由を記述できます。
- **プライバシー-Bluetooth 周辺機器使用状況の説明**(`NSBluetoothPeripheralUsageDescription`)-アプリがユーザーのデバイスの Bluetooth にアクセスする理由を開発者が記述できるようにします。
- **プライバシー-予定表の使用状況の説明**(`NSCalendarsUsageDescription`)-アプリがユーザーの予定表にアクセスする理由を開発者が記述できるようにします。
- **プライバシー-カメラの使用状況の説明**(`NSCameraUsageDescription`)-アプリがデバイスのカメラにアクセスする理由を開発者が記述できるようにします。
- **プライバシー-連絡先の使用状況の説明**(`NSContactsUsageDescription`)-アプリがユーザーの連絡先にアクセスする理由を開発者が記述できるようにします。
- **プライバシー-正常性共有の使用状況の説明**(`NSHealthShareUsageDescription`)-アプリがユーザーの正常性データにアクセスする理由を開発者が記述できるようにします。 詳細については、Apple の[HKHealthStore クラスのリファレンス](https://developer.apple.com/reference/healthkit/hkhealthstore)を参照してください。
- **プライバシー-正常性更新の使用状況の説明**(`NSHealthUpdateUsageDescription`)-開発者は、アプリがユーザーの正常性データを編集する理由を記述できます。 詳細については、Apple の[HKHealthStore クラスのリファレンス](https://developer.apple.com/reference/healthkit/hkhealthstore)を参照してください。
- **プライバシー-ホームキットの使用法の説明**(`NSHomeKitUsageDescription`)-アプリがユーザーのホームキットの構成データにアクセスする理由を開発者が記述できるようにします。
- **プライバシー-場所常に使用する説明**(`NSLocationAlwaysUsageDescription`)-アプリが常にユーザーの場所にアクセスする必要がある理由を開発者が記述できるようにします。
- れ**プライバシー-場所の使用法の説明**(`NSLocationUsageDescription`)-アプリがユーザーの場所にアクセスする理由を開発者が記述できるようにします。 *注: このキーは、iOS 8 以降では非推奨となりました。代わりに、`NSLocationAlwaysUsageDescription` または `NSLocationWhenInUseUsageDescription` を使用してください。*
- **プライバシー-使用時の使用状況の説明**(`NSLocationWhenInUseUsageDescription`)-開発者は、アプリの実行中にユーザーの場所にアクセスする理由を記述できます。
- れ**プライバシー-メディアライブラリの利用状況の説明**-開発者は、アプリがユーザーのメディアライブラリにアクセスする理由を記述できます。 *注: このキーは、iOS 8 以降では非推奨となりました。代わりに `NSAppleMusicUsageDescription` を使用してください。*
- **プライバシー-マイクの使用状況の説明**(`NSMicrophoneUsageDescription`)-アプリがデバイスのマイクにアクセスする理由を開発者が記述できるようにします。
- **プライバシー-モーションの使用方法の説明**(`NSMotionUsageDescription`)-アプリがデバイスの加速度計にアクセスする理由を開発者が記述できるようにします。
- **プライバシー-フォトライブラリの使用法の説明**(`NSPhotoLibraryUsageDescription`)-アプリがユーザーの写真ライブラリにアクセスする理由を開発者が記述できるようにします。
- **プライバシー-リマインダーの使用状況の説明**(`NSRemindersUsageDescription`)-開発者は、アプリがユーザーの通知にアクセスする理由を記述できます。
- **プライバシー-Siri 使用状況の説明**(`NSSiriUsageDescription`)-アプリがユーザーデータを siri に送信する理由を開発者が記述できるようにします。
- **プライバシー-音声認識の使用状況の説明**(`NSSpeechRecognitionUsageDescription`)-アプリがユーザーデータを Apple の音声認識サーバーに送信する理由を開発者が記述できるようにします。
- **プライバシー-Tv プロバイダーの利用状況の説明**(`NSVideoSubscriberAccountUsageDescription`)-アプリがユーザーのテレビプロバイダーアカウントにアクセスする理由を開発者が記述できるようにします。

**Plist**キーの使用方法の詳細については、Apple の[Information プロパティリストキーのリファレンス](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1)を参照してください。

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>プライバシーキーの設定

次の例では、iOS 10 (以上) のホームキットにアクセスするために、開発者は `NSHomeKitUsageDescription` キーをアプリの**情報 plist**ファイルに追加し、アプリがユーザーのホームキットデータベースにアクセスする理由を宣言する文字列を指定する必要があります。 この文字列は、アプリを初めて実行するときにユーザーに表示されます。

![NSHomeKitUsageDescription アラートの例](security-privacy-images/info01.png "NSHomeKitUsageDescription アラートの例")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

現在、Xamarin iOS for Visual Studio では、既定の iOS マニフェストエディター内からの**情報の plist**プライバシーキーの編集はサポートされていません。 代わりに、汎用 PList エディターを使用する必要があるため、次の手順を実行します。

1. **ソリューションエクスプローラー**の**情報の plist**ファイルを右クリックし、[**ファイルを開くアプリケーション**の選択] をクリックします。
2. プログラムの一覧から**汎用 PList エディター**を選択してファイルを開き、[ **OK]** をクリックします。

    ![汎用 PList エディターを選択します](security-privacy-images/InfoEditorSelectionVs.png "汎用 PList エディターを選択します")
3. エディターの最後の行にある [ **+** ] ボタンをクリックして、一覧に新しいエントリを追加します。 これは "カスタムプロパティ" と呼ばれ、型が `String` に設定され、値が空になります。
4. プロパティ名をクリックすると、ドロップダウンが表示されます。
5. ドロップダウンリストから、プライバシーキー (**プライバシーホームキットの使用状況の説明**など) を選択します。 

    ![プライバシーキーを選択してください](security-privacy-images/InfoPListEditorSelectKey.png "プライバシーキーを選択してください")
6. 特定の機能またはユーザー情報にアプリがアクセスする理由の説明を 値列に入力してください。 

    ![説明を入力してください](security-privacy-images/InfoPListSetValue.png "説明の入力")
7. 変更内容をファイルに保存します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

任意のプライバシーキーを設定するには、次の手順を実行します。

1. **ソリューション エクスプローラー**で **Info.plist** ファイルをダブルクリックし、編集用に開きます。
2. 画面の下部で、**ソース**ビューに切り替えます。
3. リストに新しい**エントリ**を追加します。
4. ドロップダウンリストから、プライバシーキー (**プライバシーホームキットの使用状況の説明**など) を選択します。 

    ![プライバシーキーを選択してください](security-privacy-images/info02.png "プライバシーキーを選択してください")
5. アプリが特定の機能またはユーザー情報にアクセスする理由の説明を入力します。 

    ![説明を入力してください](security-privacy-images/info03.png "説明の入力")
6. 変更内容をファイルに保存します。

-----

> [!IMPORTANT]
> 上記の例では、**情報 plist**ファイルで `NSHomeKitUsageDescription` キーを設定しなかった場合、iOS 10 (以上) で実行したときに、エラーが発生することなく (実行時にシステムによって終了される) アプリが_サイレントに失敗_することになります。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、iOS 10 で Apple が行ったセキュリティとプライバシーの変更点と、それらが Xamarin iOS アプリに与える影響について説明しました。

## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
