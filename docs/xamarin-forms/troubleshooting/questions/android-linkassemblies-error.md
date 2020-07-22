---
title: 'Android ビルドエラー: LinkAssemblies タスクが予期せず失敗しました'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/07/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9c0403203345ed78fae9f30c623c3e501f24ea0e
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135903"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android ビルドエラー: LinkAssemblies タスクが予期せず失敗しました

`The "LinkAssemblies" task failed unexpectedly`フォームを使用する Xamarin Android プロジェクトをビルドすると、エラーメッセージが表示される場合があります。 これは、リンカーがアクティブなときに発生します (通常は*リリース*ビルドで、アプリケーションパッケージのサイズを小さくします)。また、Android ターゲットが最新のフレームワークに更新されないために発生します。 (詳細情報: [ Xamarin.Forms サポートされているプラットフォーム](~/get-started/supported-platforms.md#android-platform-support))

この問題を解決するには、サポートされている最新の Android SDK バージョンがあることを確認し、インストールされている最新のプラットフォームに**ターゲットフレームワーク**を設定します。 また、**ターゲットの Android バージョン**を、インストールされている最新のプラットフォームに、 **android の最小バージョン**を API 19 以降に設定することもお勧めします。 これは、サポートされる構成と見なされます。

## <a name="setting-in-visual-studio-for-mac"></a>Visual Studio for Mac での設定

1. Android プロジェクトを右クリックし、メニューの [**オプション**] を選択します。
2. [**プロジェクトオプション**] ダイアログで、 **[ビルド > 全般**] にアクセスします。
3. **Android バージョンを使用したコンパイル (ターゲットフレームワーク)** を、インストールされている最新のプラットフォームに設定します。
4. [**プロジェクトオプション**] ダイアログで、[**ビルド > Android アプリケーション**] にアクセスします。
5. Android の**最小バージョン**を API レベル19以上に設定し、**ターゲットの android バージョン**を、(3) で選択した最新のインストール済みプラットフォームに設定します。

## <a name="setting-in-visual-studio"></a>設定 (Visual Studio での)

1. Android プロジェクトを右クリックし、メニューの [**プロパティ**] を選択します。
2. プロジェクトのプロパティで、[**アプリケーション**] にアクセスします。
3. **Android バージョンを使用したコンパイル (ターゲットフレームワーク)** を、インストールされている最新のプラットフォームに設定します。
4. プロジェクトのプロパティで、[ **Android マニフェスト**] にアクセスします。
5. Android の**最小バージョン**を API レベル19以上に設定し、**ターゲットの android バージョン**を、(3) で選択した最新のインストール済みプラットフォームに設定します。

これらの設定を更新したら、プロジェクトをクリーンにしてリビルドし、変更が確実に取得されるようにしてください。
