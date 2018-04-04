---
title: 'Android のビルド エラー: LinkAssemblies タスクが予期せず失敗しました'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: de6e0b66cac688955d27ba2d0165d5a059d36c38
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android のビルド エラー: LinkAssemblies タスクが予期せず失敗しました

エラー メッセージが表示することがあります`The "LinkAssemblies" task failed unexpectedly`フォームを使用する Xamarin.Android プロジェクトを構築する場合。 これは、エラーは、リンカーは、アクティブな場合 (通常の*リリース*アプリ パッケージのサイズを縮小するビルド); し、Android の対象は、最新のフレームワークに更新されないために発生します。 (詳細については: [Android 要件の Xamarin.Forms](~/xamarin-forms/get-started/installation.md#android))

この問題を解決を設定し、最新サポートされている Android SDK のバージョンをしたことを確認するには、**ターゲット フレームワーク**に**インストールされている最新バージョンを使用してプラットフォーム**です。 設定することも推奨、**ターゲット Android バージョン**に**ターゲット フレームワークのバージョンを使用して**と**最低限の Android バージョン**API 15 以降。 これは、サポートされる構成と見なされます。

## <a name="setting-in-visual-studio-for-mac"></a>Mac 用の Visual Studio での設定

1.  Android のプロジェクトを右クリックします。
2.  移動して**ビルド > [全般] > のターゲット フレームワーク**です。
3.  設定、**ターゲット フレームワーク: インストールされている最新バージョンを使用してプラットフォーム**です。
4.  プロジェクトのオプションの中に移動**ビルド > Android アプリケーション**です。
5.  設定、**最低限の Android バージョン**API レベル 15 以上に (&)、**ターゲット Android バージョン**に**自動: ターゲット フレームワークのバージョンを使用して**です。

## <a name="setting-in-visual-studio"></a>Visual Studio での設定

1.  Android のプロジェクトを右クリックします。
2.  移動して**アプリケーション**プロジェクト オプション。
3.  設定、 **Android バージョンを使用してコンパイル** & **ターゲット Android バージョン**設定を**使用最新プラットフォーム** / **使用SDK のバージョンを使用してコンパイル**です。
4.  設定、**ターゲットに最低限の Android** API 15 以上を設定します。

これらの設定を更新するとは、クリーンアップ、変更内容が選択されているを確認するようにプロジェクトを再構築してください。
