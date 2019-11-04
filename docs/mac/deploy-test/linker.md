---
title: Xamarin.Mac リンカー オプション
description: このドキュメントでは、Xamarin.Mac のリンクについて説明します。 リンクは、使用していないコードを削除してアプリケーションのサイズを縮小する強力な最適化ツールです。
ms.prod: xamarin
ms.assetid: F03176C3-F8D4-4DE8-870C-7F27D8CE525A
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 11/10/2017
ms.openlocfilehash: f4ab94c4eede4a122ac834e075270a375bca0807
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030006"
---
# <a name="xamarinmac-linker-options"></a>Xamarin.Mac リンカー オプション

_リンクは、使用していないコードを削除してアプリケーションのサイズを縮小する強力な最適化ツールです。_

## <a name="overview"></a>概要

プロジェクトで使用する[ターゲット フレームワーク](~/mac/platform/target-framework.md)に基づいて、使用可能なリンカー オプションが制限されることがあります。 その理由は、リンクにはアプリケーションで使用されるすべての型のオブジェクト グラフの作成が必要ですが、Full (または Unsupported) では System.Configuration が原因でこれが可能でないためです。

次の 4 つのオプションを使用できます。

- **None** - すべてのリンクを無効にします。 モダンのデバッグ構成と Full のすべての構成の既定です。
- **SDK** - ユーザー アセンブリを除くすべての SDK アセンブリをリンクします。 モダンのリリース構成の既定です。 Full では使用できません。
- **Full** - すべてのアセンブリをリンクします。 これには、リンカーで安全に使用できるユーザー コードが必要です、詳しくは、[注意事項](~/ios/deploy-test/linker.md)をご覧ください。 Full では使用できません。
- **Platform** - Xamarin.Mac.dll だけをリンクします。 詳細については、以下を参照してください。

## <a name="platform-linking"></a>プラットフォームのリンク

Full の[ターゲット フレームワーク](~/mac/platform/target-framework.md)を使用したアプリケーションのリンクは一般に安全ではありませんが、非常に制限された形式のリンクが必要なシナリオがいくつかあります。

たとえば、macOS App Store に送信されるアプリケーションは、いくつかの非推奨の API (QTKit など) を参照してはなりません。これらの API の一部は Xamarin.Mac にバインディングが含まれます。 アプリケーションがこれらのバインディングを呼び出さない場合でも、呼び出しは SDK 内に存在するため、拒否されます。

プラットフォームのリンクは、アプリケーションと BCL が安全でないリンカーであると想定し、Xamarin.Mac.dll から使用していないコードを削除します。 

Xamarin.Mac.dll 型にリフレクションを実行しないすべてのアプリケーションは、不要な型の削除によりスタートアップが若干向上します。

モダン アプリケーションはより強力な SDK オプションを使用できるため、プラットフォームのリンクは一般に Full のターゲット フレームワークを使用するアプリケーションにのみ役立ちます。

## <a name="setting-the-linker-configuration"></a>リンカーの構成の設定

Xamarin.Mac プロジェクトのリンカー構成に変更するには、次の操作を行います。

1. Visual Studio for Mac で Xamarin.Mac プロジェクトを開きます。
2. **ソリューション エクスプローラー**で、プロジェクト ファイルをダブルクリックし、 **[プロジェクト オプション]** ダイアログ ボックスを開きます。
3. **[Mac ビルド]** タブで、アプリケーションのニーズに適した **[リンカーの動作]** の種類を選択します。

    ![使用するリンカーの動作を選択する](linker-images/link-behavior.png "使用するリンカーの動作を選択する")

4. Full のターゲット フレームワークのプラットフォーム リンクは、今後更新されるまで IDE に表示されません。 それまでは、代わりに `--linkplatform` を **追加の mmp 引数**に追加します。
5. **[OK]** をクリックして変更内容を保存します。

## <a name="related-links"></a>関連リンク

- [iOS でのリンク](~/ios/deploy-test/linker.md)
