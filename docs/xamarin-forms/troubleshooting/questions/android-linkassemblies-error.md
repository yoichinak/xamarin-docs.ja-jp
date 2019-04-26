---
title: Android のビルド エラー – The LinkAssemblies タスクが予期せず失敗しました
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/07/2019
ms.openlocfilehash: f517aaa770fa7b2f1463954638f0afc95168bf65
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250743"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android のビルド エラー – The LinkAssemblies タスクが予期せず失敗しました

エラー メッセージが表示することがあります`The "LinkAssemblies" task failed unexpectedly`Xamarin.Android プロジェクトのビルドがフォームを使用している場合。 リンカーは、アクティブな場合 (通常で、*リリース*アプリ パッケージのサイズを小さくビルド); Android ターゲットが最新のフレームワークに更新されないために発生するとします。 (詳細については。[Android の要件の Xamarin.Forms](~/get-started/requirements.md#android))

この問題を解決最新サポート対象の Android SDK バージョンがあり、設定するかどうかを確認するには、**ターゲット フレームワーク**にインストールされている最新のプラットフォームにします。 またに設定することをお勧め、**ターゲット Android バージョン**最新インストールされているプラットフォームに、および**最小 Android バージョン**API 19 以上。 これは、サポートされる構成と見なされます。

## <a name="setting-in-visual-studio-for-mac"></a>Visual Studio for Mac 設定

1.  Android のプロジェクトを右クリックし、選択**オプション**メニュー。
2.  **プロジェクト オプション**ダイアログ ボックスに移動して**ビルド > 全般**します。
3.  設定、 **Android バージョンを使用してコンパイルします。(ターゲット フレームワーク)** にインストールされている最新のプラットフォームにします。
4.  **プロジェクト オプション**ダイアログ ボックスに移動して**ビルド > Android アプリケーション**します。
5.  設定、 **Minimum Android version** API レベル 19 以上に、 **Target Android version** (3) で選択した最新インストールされているプラットフォームにします。

## <a name="setting-in-visual-studio"></a>Visual Studio での設定

1.  Android のプロジェクトを右クリックし、選択**プロパティ**メニュー。
2.  プロジェクトのプロパティに移動**アプリケーション**します。
3.  設定、 **Android バージョンを使用してコンパイルします。(ターゲット フレームワーク)** にインストールされている最新のプラットフォームにします。
4.  プロジェクトのプロパティに移動**Android マニフェスト**します。
5.  設定、 **Minimum Android version** API レベル 19 以上に、 **Target Android version** (3) で選択した最新インストールされているプラットフォームにします。

これらの設定を更新した後は、クリーンアップ、変更内容が取り出されることを確認するようにプロジェクトを再構築してください。
