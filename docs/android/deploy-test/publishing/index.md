---
title: アプリケーションの発行
ms.prod: xamarin
ms.assetid: 51E19000-040A-2B74-C462-EC57C617085C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 765adf10e5bdf20191c5ee1c089d39032ea07ce0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021377"
---
# <a name="publishing-an-application"></a>アプリケーションの発行

優れたアプリケーションを開発されると、人はそれを使ってみたくなります。 このセクションには、電子メール、プライベート Web サーバー、Google Play、Amazon Android アプリ ストアなどのチャネルによって、Xamarin.Android で作成したアプリケーションを公開配信するための手順が含まれます。

## <a name="overview"></a>概要

Xamarin.Android アプリケーション開発の最終手順は、アプリケーションの公開です。 公開とは、ユーザーがそのデバイスにインストールできるようにするための、Xamarin.Android アプリケーションをコンパイルするプロセスです。2 つの重要な作業から構成されます。

- **公開に向けた準備** &ndash; Android 搭載デバイスに展開できるようにアプリケーションのリリース版が作成されます (リリースの準備に関する詳細については、「[リリースに向けてアプリケーションを準備する](~/android/deploy-test/release-prep/index.md)」を参照してください)。

- **配布** &ndash; アプリケーションのリリース版は、さまざまな配布チャネルのうち 1 つまたは複数のチャネルから入手できます。

次の図は、Xamarin.Android アプリケーションの公開手順をまとめたものです。

[![ビルドと展開の流れ図](images/build-and-deploy-steps.png)](images/build-and-deploy-steps.png#lightbox)

上の図からわかるように、利用される配信方法に関係なく、準備は同じです。 Android アプリケーションはいくつかの方法でユーザーに公開できます。

- **Web サイトから** &ndash; Xamarin.Android アプリケーションを Web サイトでダウンロードできます。ユーザーはサイトでリンクをクリックしてアプリケーションをインストールします。
- **電子メールで** &ndash; ユーザーは Xamarin.Android アプリケーションを電子メールからインストールできます。 Android 搭載デバイスで添付ファイルを開くと、アプリケーションがインストールされます。
- **マーケットから** &ndash; [Google Play](https://play.google.com/) や [Amazon App Store for Android](https://www.amazon.com/mobile-apps/b?ie=UTF8&node=2350149011) など、配信を行っているアプリケーション マーケットプレイスがいくつかあります。

人気のマーケットプレイスをアプリケーション公開に使うことが一般的です。市場が広く、配布管理が行き届いています。 ただし、マーケットプレイスでアプリケーションを公開する場合、さらに一手間かかります。

Xamarin.Android アプリケーションは複数のチャンネルで同時に配信できます。 たとえば、Google Play と Amazon App Store for Android でアプリケーションを公開し、Web サーバーからもダウンロードできるようにします。

他の 2 つの配信方法 (ダウンロードまたは電子メール) は、ユーザーが小集団で管理されている企業環境などに最適です。少数の利用者が決まっているアプリケーションにも使われます。
サーバー配信と電子メール配信もシンプルな公開モデルです。アプリケーションの公開に必要な準備が少なくて済みます。

Amazon のモバイル アプリ配信プログラムでは、モバイル アプリ開発者は Amazon でアプリケーションを配信、販売できます。 ユーザーは Amazon App Store アプリケーションを利用し、自分の Android 搭載デバイスでアプリを見つけ、購入できます。 以下は、Android デバイスで実行されている Amazon App Store のスクリーンショットです。

ほぼ間違いなく、Google Play は Android アプリケーションのマーケットプレイスとして最も人気があり、すべてを網羅したマーケットプレイスです。 Google Play では、ユーザーはデバイスまたはコンピューターのアイコンをクリックし、アプリを検索、ダウンロード、評価し、アプリの代金を支払うことができます。 Google Play は、売上や市場の傾向を分析するためのツールも用意しています。アプリケーションをダウンロードするデバイスやユーザーを管理することもできます。 以下は、Android デバイスで実行されている Google Play のスクリーンショットです。

[![Google Play スクリーンショット](images/google-play-app.png)](images/google-play-app.png#lightbox)

このセクションでは、適切なプロモーション素材と共に、Google Play などのストアにアプリケーションをアップロードする方法を示します。 APK 拡張ファイルの概念的な概要としくみについて示し、APK 拡張ファイルについて説明しています。 Google のライセンス サービスについても説明します。 最後に、HTTP Web サーバーの使用、単純な電子メールの配信、Amazon Android アプリ ストアなど、配信の代替方法について紹介しています。

## <a name="related-links"></a>関連リンク

- [HelloWorldPublishing (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/helloworldpublishing)
- [ビルド プロセス](~/android/deploy-test/building-apps/build-process.md)
- [リンク](~/android/deploy-test/linker.md)
- [Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [アプリケーション署名](https://source.android.com/security/apksigning/)
- [Google Play で公開する](https://developer.android.com/distribute/googleplay/publish/index.html)
- [Google アプリケーション ライセンス](https://developer.android.com/guide/google/play/licensing/index.html)
- [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary)
- [モバイル アプリの配布ポータル](https://developer.amazon.com/welcome.html)
- [Amazon モバイル アプリの配布に関する FAQ](https://developer.amazon.com/help/faq.html)
