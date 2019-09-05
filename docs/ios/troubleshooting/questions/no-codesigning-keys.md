---
title: iOS のビルドが失敗して "no valid iPhone code signing keys found in keychain" (キーチェーンに有効な iPhone コード署名キーがありません) と表示されるのはなぜですか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 9d589af6249063add3dc9b085463fde9edd8e449
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278435"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>iOS のビルドが失敗して "no valid iPhone code signing keys found in keychain" (キーチェーンに有効な iPhone コード署名キーがありません) と表示されるのはなぜですか。

## <a name="cause-of-the-error"></a>エラーの原因
このエラーメッセージは、問題のプロジェクトが、有効なコード署名資格情報を探しているものの、見つからない場合に発生します。 物理 iOS デバイスでのテストと展開には、コード署名が必要です。また、アドホック & App store ビルドもあります。 


### <a name="provisioning-devices"></a>デバイスをプロビジョニングする
前に iOS デバイスをプロビジョニングしていない場合は、次のガイドで完全な手順を実行します。[デバイスプロビジョニングガイド](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>IOS シミュレーターを使用する場合のバグ

> [!NOTE]
> この問題は、Xamarin for Visual Studio の最近のバージョンで解決されました。 ただし、最新バージョンのソフトウェアで問題が発生した場合は、完全なバージョン管理情報と完全ビルドログ出力を使用して[新しいバグを作成](~/cross-platform/troubleshooting/questions/howto-file-bug.md)してください。


Xamarin. Visual Studio 3.11 でバグが発生しました。これにより、Xamarin. Forms テンプレートの iOS プロジェクトが、シミュレータービルドに codesign の権利を追加するようになりました。シミュレーターを使用してテストを効果的にブロックします。

### <a name="how-to-fix"></a>修正方法
.Csproj ファイルのデバッグビルドからフラグを`<CodesignEntitlements>`削除することで、この問題を回避できます。 これは、次のようにして行うことができます。

*警告:.Csproj ファイルのエラーによってプロジェクトが破壊される可能性があるため、ファイルをバックアップしてから実行することをお勧めします。*

1. ソリューションウィンドウで iOS プロジェクトを右クリックし、 **[プロジェクトのアンロード]** を選択します。
2. プロジェクトをもう一度右クリックし、[**編集] [ProjectName] を選択します。 .csproj**
3. Debug PropertyGroups を見つけて、次のようなフラグで開始する必要があります。
   - デバック`<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - 解除`<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. シミュレーターを使用する各ビルドで、次のプロパティを削除またはコメントアウトします。`<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. プロジェクトを再読み込みすると、シミュレーターに配置できるようになります。

### <a name="next-steps"></a>次の手順
詳細については、お問い合わせください。または、上記の情報を利用した後もこの問題が発生する場合は、「 [Xamarin で使用できるサポートオプション](~/cross-platform/troubleshooting/support-options.md)」を参照してください。連絡先オプション、提案、および必要に応じて新しいバグをファイルに登録する方法については、こちらを参照してください. 
