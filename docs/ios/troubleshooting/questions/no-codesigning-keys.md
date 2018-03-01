---
title: "IOS のビルドが失敗する理由と: キーチェーンに有効な iPhone コードの署名キーが見つかりませんか?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6a853bc7186647dbdae1d5d12f3b185d302a8088
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>IOS のビルドが失敗する理由と: キーチェーンに有効な iPhone コードの署名キーが見つかりませんか?

## <a name="cause-of-the-error"></a>エラーの原因
このエラー メッセージは、対象のプロジェクトが有効なコード署名資格情報を検索するときに発生するが、それらを検索することはできません。 テストと物理的な iOS デバイス上の展開に必要なコード署名できるだけでなく、アドホックおよびアプリのビルドを格納できます。 


### <a name="provisioning-devices"></a>デバイスのプロビジョニング
前に、iOS デバイスをプロビジョニングしていない場合、次のガイドを見ていきます完全ステップ バイ ステップ処理:[デバイスのプロビジョニングのガイド](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>IOS シミュレーターを使用する場合のバグ

> [!NOTE]
> この問題は、for Visual Studio で Xamarin の最新バージョンで解決されています。 ただし、ソフトウェアの最新のバージョンで問題が発生した場合を送信してください、[新しいバグ](~/cross-platform/troubleshooting/questions/howto-file-bug.md)すべてのバージョン管理情報と完全のビルド ログ出力します。


Codesign シミュレーターに Entitlements.plist ビルド; を追加する、Xamarin.Forms テンプレートで iOS プロジェクトを原因となった Xamarin.Visual Studio 3.11 にバグが発生しましたシミュレーターを使用してテストを効果的にブロックします。

### <a name="how-to-fix"></a>修正方法
削除することで、この問題を回避することができます、 `<CodesignEntitlements>` .csproj ファイル内のビルドにデバッグからのフラグ。 このことは次のように実行できます。

*警告: ため、これを行う前に、ファイルをバックアップすることをお勧め、.csproj ファイル内のエラーで、プロジェクトを中断できます。*

1. ソリューションのウィンドウで iOS プロジェクトを右クリックし、選択**プロジェクトのアンロード**
2. プロジェクトを再び右クリックし、選択**[ProjectName] .csproj の編集**
3. デバッグ Propertygroup の検索は、次のようにするフラグを使って開始する必要があります。
   - デバッグ: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - リリース: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. 各をシミュレーターを使用するビルドを削除またはコメントに、次のプロパティ。 `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. プロジェクトを再読み込みされ、シミュレーターに展開することができます。

### <a name="next-steps"></a>次の手順
詳細については、問い合わせ先、または、上記の情報を使用した後もこの問題が残っている場合を参照してください[どのようなサポート オプションは、Xamarin を使用しますか?](~/cross-platform/troubleshooting/support-options.md)推奨事項は、の連絡先のオプションについて方法だけでなく必要な場合は、新しいバグをファイルです。 
