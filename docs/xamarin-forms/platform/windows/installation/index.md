---
title: セットアップの Windows プロジェクト
description: 既存の Xamarin.Forms ソリューションに新しい Windows プロジェクトを追加します。
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 0ad8dedc2e92005473f8836c3cdd590ce4cab5ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="setup-windows-projects"></a>セットアップの Windows プロジェクト

_既存の Xamarin.Forms ソリューションに新しい Windows プロジェクトを追加します。_

旧バージョンの Xamarin.Forms プロジェクト (または Mac OS 上に作成されたもの&nbsp;X) これらの Windows プロジェクト設定はありません。

つまり、Windows 8.1、Windows Phone 8.1、および Windows 10 (UWP) アプリをビルドするプロジェクトの種類はこれらを手動で追加する必要があります。

## <a name="add-a-windows-81-app"></a>Windows 8.1 アプリを追加します。

* PCL テンプレートを使用する場合[プロファイルを更新](#pcl)、し、
* [Windows 8.1 アプリの追加](~/xamarin-forms/platform/windows/installation/tablet.md)のタブレット/デスクトップのフォーム ファクター。

## <a name="add-a-windows-phone-81-app"></a>追加、Windows Phone 8.1 アプリ

* PCL テンプレートを使用する場合[プロファイルを更新](#pcl)、し、
* [追加、Windows Phone 8.1 アプリ](~/xamarin-forms/platform/windows/installation/phone.md)

## <a name="add-a-universal-windows-platform-uwp-app"></a>追加のユニバーサル Windows プラットフォーム (UWP) アプリ

* 構築[UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx)アプリには、Windows 10 で実行されている Visual Studio 2015 が必要です。
* PCL テンプレートを使用する場合[プロファイルを更新](#pcl)、し、
* [追加のユニバーサル Windows プラットフォーム アプリ](~/xamarin-forms/platform/windows/installation/universal.md)

<a name="pcl" />

### <a name="update-your-pcl-profile"></a>PCL プロファイルを更新します。

既存の Xamarin.Forms アプリがポータブル クラス ライブラリ (PCL) テンプレートを使用する場合は、そのプロファイルを更新する必要があります。

1. **右クリック > プロパティ**(既存の設定が異なる場合があります)

  ![](images/targets.png "PCL ターゲット")

2. をクリックして、**変更しています.**ボタン

3. 確認してください、 **Windows 8**と**Windows Phone 8.1**オプションが選択されている (および**Windows Phone Silveright**は*選択解除して*)。

  ![](images/pcl.png "PCL ターゲット オプション")

4. キーを押して**OK**し、変更を保存します。

これに相当**プロファイル 111**ドロップダウン リストを使用して Mac を Visual Studio では、PCL を構成している場合。

  ![](images/pcl-xs.png "PCL プロファイル 111")

**注:**プロファイル 259 に PCL を設定する必要があります、ソリューションには、まだ Windows Phone 8 Silverlight プロジェクトがある場合。 Windows Phone 8 Silverlight のサポートは廃止されず、ため、このページに表示されているプロジェクトの種類で置き換えることをお勧めします。
