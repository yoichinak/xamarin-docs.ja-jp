---
title: "フラグメントを実装します。"
description: "Android 3.0 には、フラグメントが導入されました。 フラグメントは、自己完結型のモジュラー コンポーネントです。さまざまなサイズの画面で実行される可能性がある複雑なアプリケーション作成に対処するために使用されます。 この記事で事前 Android 3.0 デバイスでのフラグメントをサポートする方法と Xamarin.Android アプリケーションを開発するフラグメントを使用する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: ebb53398edba64e255f1a534556836df8734ba6f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="implementing-with-fragments"></a>フラグメントを実装します。

_Android 3.0 には、フラグメントが導入されました。フラグメントは、自己完結型のモジュラー コンポーネントです。さまざまなサイズの画面で実行される可能性がある複雑なアプリケーション作成に対処するために使用されます。この記事で事前 Android 3.0 デバイスでのフラグメントをサポートする方法と Xamarin.Android アプリケーションを開発するフラグメントを使用する方法について説明します。_

<a name="Overview" />

## <a name="overview"></a>概要

このセクションでシェイクスピアーと選択した各 play から見積もりの一覧を表示するアプリケーションを作成する方法を説明します。 1 つの場所で、UI コンポーネントを定義しましたがさまざまなフォーム ファクターでこれらを使用できるように、アプリはフラグメントを利用します。 たとえば、次のスクリーン ショットは、10"タブレット、およびスマート フォンで実行されているアプリケーションを表示します。

[![タブレットおよび電話で実行されている例のアプリのスクリーン ショット](images/intro-screenshot-sml.png)](images/intro-screenshot.png)

このセクションでは、次のトピックを取り上げます。

- **フラグメントを作成する**&ndash;シェイクスピアーの一覧を表示するフラグメントと各 play から見積もりを表示する別のフラグメントを作成する方法を示します。

- **さまざまな画面サイズをサポートする**&ndash;表示画面のサイズを大きく活用するためにアプリケーションをレイアウトする方法です。

- **Android のサポート パッケージを使用して** &ndash; Android のサポート パッケージを実装し、以前のバージョンの Android 上で実行できるように、アプリケーション内の活動に若干の変更は、します。

<a name="Requirements" />

## <a name="requirements"></a>必要条件

このチュートリアルでは、Xamarin.Android 4.0 以上が必要です。 必要があります、Android のサポート パッケージをインストールするフラグメントのドキュメントで説明したようです。

<a name="Introduction" />

## <a name="introduction"></a>はじめに

例では、このセクションの内容をビルド、アクティビティでは、一覧の読み込み、ユーザーの選択への応答または選択されている再生の見積もりを表示するためのロジックは含まれません。 このロジックは、個々 のフラグメントに存在します。
自体のフラグメントのこのロジックを配置すると、アクティビティごとに別のロジックを記述することがなく、1 つのアクティビティまたは複数の活動で小さな画面で大きな画面をサポートするために、アプリケーションのワークフローを分類できます。 タブレットでは、両方のフラグメントは 1 つのアクティビティになります。 スマート フォン、フラグメントは異なるアクティビティにホストされます。

このアプリケーションには、次の部分が含まれます。

 **MainActivity** – 画面のサイズによっては、フラグメントの一方または両方が表示されます。 これは、スタートアップ アクティビティです。

 **TitlesFragment** – シェイクスピアーのユーザーが 元の一覧が表示されます。

 **DetailsFragment** – 選択した再生の見積もりが表示されます。

 **DetailsActivity** – をホストし、DetailsFragment が表示されます。
このアクティビティは、携帯電話などの小さな画面でデバイスを使用します。



## <a name="related-links"></a>関連リンク

- [FragmentsWalkthrough (サンプル)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [Xamarin.Android サンプル: Honeycomb ギャラリー](https://developer.xamarin.com/samples/HoneycombGallery/)
- [フラグメントを実装します。](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [サポート パッケージ](http://developer.android.com/sdk/compatibility-library.html)
