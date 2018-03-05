---
title: "Android API レベルを理解します。"
description: "Xamarin.Android には、複数のバージョンの Android アプリの互換性を確認するいくつかの Android API レベル設定があります。 このガイドは、これらの設定の意味、これらを構成する方法およびどのような影響について説明します。 実行時に、アプリであります。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 5a8b51f6c63d8632e71d1cddabb0c37758ee02f0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="understanding-android-api-levels"></a>Android API レベルを理解します。

_Xamarin.Android には、複数のバージョンの Android アプリの互換性を確認するいくつかの Android API レベル設定があります。このガイドは、これらの設定の意味、これらを構成する方法およびどのような影響について説明します。 実行時に、アプリであります。_

<a name="quick" />

## <a name="quick-start"></a>クイック スタート

Xamarin.Android は、次の 3 つの Android API レベルのプロジェクト設定を公開します。

-   [フレームワークをターゲットに](#framework)&ndash;アプリケーションの構築に使用するには、どのフレームワークを指定します。 この API レベルが使用される*コンパイル*Xamarin.Android によって時間。

-   [最低限の Android バージョン](#minimum)&ndash;をサポートするアプリを表示する最も古い Android バージョンを指定します。 この API レベルが使用される*実行*Android によって時間。

-   [Android バージョンを対象に](#target)&ndash;上で実行するためのもので、アプリが Android のバージョンを指定します。 この API レベルが使用される*実行*Android によって時間。

API レベルを構成するには、プロジェクトの前に、その API レベルの SDK プラットフォーム コンポーネントをインストールする必要があります。 ダウンロードして、Android SDK コンポーネントのインストールに関する詳細については、次を参照してください。 [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

通常、3 つすべての Xamarin.Android API レベルは、同じ値に設定されます。 **アプリケーション** ページで、設定**Android バージョン (ターゲット フレームワーク) を使用してコンパイル**を最新の安定した API バージョン (または、すべての必要な機能を備えている Android バージョンを少なくとも)。
次のスクリーン ショットでは、ターゲット フレームワークが に設定されている**Android 7.1 (API レベル 25 - Nougat)**:

[![Android バージョンを使用してコンパイルするフレームワークのバージョンの既定値をターゲットします。](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png)

**Android マニフェスト**に最低限の Android バージョンを設定 ページで、**を使用してコンパイル SDK バージョンを使用して**し (次のターゲット フレームワークのバージョンと同じ値にターゲット Android バージョンを設定スクリーン ショット、Android ターゲット フレームワークに設定されている**Android 7.1 (Nougat)**)。

[![最小値とターゲットの Android のバージョンがターゲット フレームワークのバージョンに設定します。](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png)

Android の以前のバージョンと旧バージョンとの互換性を維持する場合は、設定**対象とする最低限の Android バージョン**最も古いバージョンの Android アプリをサポートすることにします。 (API レベル 14 に必要な最小 API レベルでは[Google Play サービスと Firebase サポート](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html))。次の例の構成には、API レベル 25 を通じて API レベル 14 から Android バージョンがサポートされています。

[![API レベル 25 を使用してコンパイル Nougat、API レベル 14 に設定されている最低限の Android バージョン](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

通常、3 つすべての Xamarin.Android API レベルは、同じ値に設定されます。 設定**ターゲット フレームワーク**を最新の安定した API バージョン (または、すべての必要な機能を備えている Android バージョンを少なくとも)。 設定する、**ターゲット フレームワーク**に移動**ビルド > 全般**で、**プロジェクト オプション**です。 次のスクリーン ショットでは、ターゲット フレームワークが に設定されている**プラットフォームを使用して、最新インストールされている (8.0)**:

[![ターゲット フレームワークがインストールされている最新バージョンを使用するプラットフォームを既定とします](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png)

最小値とターゲットの Android バージョン設定は「**ビルド > Android アプリケーション**で**プロジェクト オプション**です。 最低限の Android バージョンを設定**自動: ターゲット フレームワークのバージョンを使用して**ターゲット Android バージョンをターゲット フレームワークのバージョンと同じ値に設定します。 次のスクリーン ショットでは、Android ターゲット フレームワークが に設定されている**Android 8.0 (API レベル 26)**上のターゲット フレームワークの設定と一致します。

[![プロジェクトのオプションで、ターゲットおよびフレームワーク レベルの設定](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png)

Android の以前のバージョンと旧バージョンとの互換性を維持する場合は、変更**最低限の Android バージョン**最も古いバージョンの Android アプリをサポートすることにします。 必要な最小 API レベルは API レベル 14、 [Google Play サービスと Firebase サポート](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)です。
たとえば、次の構成をサポートしている Android バージョン API レベル 14 できるだけ早く。

[ ![最小値とターゲット バージョンを自動に設定は、ターゲット フレームワークのバージョンを使用します。](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png)

-----


アプリでは、複数の Android バージョンをサポートする場合、コードが最低限の Android のバージョンの設定で、アプリが動作するためにランタイム チェックを含める必要があります (を参照してください[ランタイムは Android バージョンをチェック](#runtimechecks)以下詳細)。 消費またはライブラリを作成する場合は、次を参照してください。 [API レベルおよびライブラリ](#libraries)下 API の構成に関するベスト プラクティスのレベルのライブラリの設定。


<a name="verslevels" />

## <a name="android-versions-and-api-levels"></a>Android バージョンと API レベル

Android バージョンごとと呼ばれる、一意の整数識別子が割り当てられている Android プラットフォームの発展し、新しい Android バージョンがリリースされた、 *API レベル*です。 したがって、各 Android バージョンは、1 つの Android API レベルに対応します。 ユーザーは、古い同様に、最新バージョンの Android にアプリをインストール、ため複数 Android API レベルを使用する実際の Android アプリを設計しなければなりません。

<a name="versions" />

### <a name="android-versions"></a>Android バージョン

複数の名前では、Android の各リリースします。

-   Android のバージョンなど**Android 7.1**
-   コード名のように A _Nougat_
-   対応する API レベルなど**API レベル 25**

Android のコード名は、複数のバージョンと API レベル (下の一覧に表示)、対応場合がありますが、各 Android バージョンを正確に 1 つの API レベルに対応しています。

Xamarin.Android をさらに、定義*バージョン コードをビルド*現在認識されている Android API レベルにマップします。 次の一覧には、API レベル、Android バージョン、コード名および Xamarin.Android ビルドのバージョンのコードの間で変換するのに役立ちます。

-   **API 27 (Android 8.1)** &ndash; _Oreo_2017 年 12 月にリリースされた、します。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.OMr1`

-   **API 26 (Android 8.0)** &ndash; _Oreo_2017 年 8 月にリリースされた、します。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.O`

-   **API 25 (Android 7.1)** &ndash; _Nougat_年 2016年 12 月でリリースされた。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.NMr1`

-   **API 24 (Android 7.0)** &ndash; _Nougat_2016 年 8 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.N`

-   **API 23 (Android 6.0)** &ndash; _Marshmallow_2015 年 8 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.M`

-   **API 22 (Android 5.1)** &ndash; _ロリポップ_2015 年 3 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.LollipopMr1`

-   **API 21 (Android 5.0)** &ndash; _ロリポップ_2014 年 11 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.Lollipop`

-   **API 20 (Android 4.4W)** &ndash; _Kitkat ウォッチ_2014 年 6 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.KitKatWatch`

-   **API 19 (Android 4.4)** &ndash; _Kitkat_2013 年 10 月リリースします。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.KitKat`

-   **API 18 (Android 4.3)** &ndash; _ゼリー Bean_2013 年 7 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **API 17 (Android 4.2-4.2.2)** &ndash; _ゼリー Bean_2012 年 11 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **API 16 (Android 4.1-4.1.1)** &ndash; _ゼリー Bean_2012 年 6 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.JellyBean`

-   **API 15 (Android 4.0.3-4.0.4)** &ndash; _アイスクリーム サウスサンドウィッチ_2011 年 12 月にリリースされた、します。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **API 14 (Android 4.0-4.0.2)** &ndash; _アイスクリーム サウスサンドウィッチ_2011 年 10 月リリースします。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **API 13 (Android 3.2)** &ndash; _Honeycomb_2011 年 6 月にリリースされた、します。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **API 12 (Android 3.1.x)** &ndash; _Honeycomb_2011 年 5 月にリリースされた、します。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **API 11 (Android 3.0.x)** &ndash; _Honeycomb_2011 年 2 月にリリースされた、します。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.HoneyComb`

-   **API 10 (Android 2.3.3-2.3.4)** &ndash; _ジンジャーブレッド_2011 年 2 月にリリースされた、します。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **API 9 (Android 2.3-2.3.2)** &ndash; _ジンジャーブレッド_2010 年 11 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.GingerBread`

-   **API 8 (Android 2.2.x)** &ndash; _Froyo_2010 年 6 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.Froyo`

-   **API 7 (Android 2.1.x)** &ndash; _Eclair_, released January 2010. バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.EclairMr1`

-   **API 6 (Android 2.0.1)** &ndash; _Eclair_2009 年 12 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.Eclair01`

-   **API 5 (Android 2.0)** &ndash; _Eclair_、2009 年 11 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.Eclair`

-   **API 4 (Android 1.6)** &ndash; _ドーナツ_2009 年 9 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.Donut`

-   **API 3 (Android 1.5)** &ndash; _Cupcake_、2009 年 5 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.Cupcake`

-   **API 2 (Android 1.1)** &ndash; _ベース_2009 年 2 月にリリースされました。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.Base11`

-   **API の 1 (Android 1.0)** &ndash; _ベース_2008 年 10 月リリースします。 バージョンのコードをビルドします。 `Android.OS.BuildVersionCodes.Base`


新しい Android バージョンが頻繁にリリースされたこの一覧が示されているように&ndash;年間のいくつかリリース場合があります。 その結果、さまざまな Android バージョンのアプリの実行が Android デバイスの universe が含まれます。 アプリで実行される継続的かつ確実に非常に多くの異なるバージョンの Android を保証するにはどうできますか。 Android の API レベルでは、この問題を管理できます。

<a name="apilevels" />

### <a name="android-api-levels"></a>Android API レベル

Android デバイスごとに正確に実行*1 つ*API レベル&ndash;この API レベルを Android プラットフォームのバージョンごとに一意であることが保証されます。 API レベル以外に、アプリが呼び出すことができる API セットのバージョンを正確に識別します。開発者はマニフェストの要素、アクセス許可などに対してコードの組み合わせを識別します。 API レベルの android のシステムでは、Android アプリケーションがデバイスにアプリケーションをインストールする前に、Android のシステムのイメージと互換性のあるかどうかを判断するのに役立ちます。

アプリケーションのビルド時に次の API レベルの情報が含まれています。

-   *ターゲット*上で実行するアプリが組み込まれている Android API レベルです。

-   *最小*Android デバイスにアプリの実行に必要な Android API レベルです。 

これらの設定は、インストール時にアプリケーションを正しく実行するために必要な機能が Android デバイスで利用できるように使用されます。 それ以外の場合は、そのデバイスで実行がブロックされるアプリ。 たとえば、Android デバイスの API レベルが低い場合、アプリの指定した最小 API レベルよりも、Android デバイスできなくなりますユーザー アプリをインストールします。

<a name="settings" />

## <a name="project-api-level-settings"></a>API レベルのプロジェクト設定

次のセクションでは、SDK Manager を使用しての対象とする API レベルを構成する方法の詳細な説明後に、開発環境を準備する方法を説明する*ターゲット フレームワーク*、*最小値Android バージョン*、および*ターゲット Android バージョン*Xamarin.Android で設定します。

<a name="sdk" />

### <a name="android-sdk-platforms"></a>Android SDK プラットフォーム

Xamarin.Android でターゲットまたは最小 API レベルを選択することができます、前に、API レベルに対応する、Android SDK プラットフォームのバージョンをインストールする必要があります。 ターゲット フレームワーク、最低限の Android バージョン、および Android のターゲット バージョンで使用できる選択肢の範囲は、インストールされている Android SDK の範囲のバージョンに制限されます。 SDK Manager を使用して、必要な Android SDK バージョンがインストールされている、され、使用して、アプリに必要なすべての新しい API レベルを追加することを確認することができます。 API レベルをインストールする方法に慣れていない場合は、次を参照してください。 [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)です。

<a name="framework" />

### <a name="target-framework"></a>[対象とする Framework]

*ターゲット フレームワーク*(とも呼ばれる`compileSdkVersion`) のビルド時にアプリがコンパイルされている特定の Android フレームワーク バージョン (API レベル) です。 この設定は、どのような Api アプリを指定*が必要ですが*、実行しますが、いる Api は、アプリを実際に使用できるインストールされている場合の影響を与えませんときに使用します。 その結果、ターゲット フレームワークの設定を変更する変わらない実行時の動作です。

ターゲット フレームワークに対して、アプリケーションがリンクされているどのライブラリのバージョンを識別する&ndash;を Api アプリで使用できるこのプロパティを決定します。 たとえば、使用する場合、 [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Android 5.0 ロリポップで導入されたメソッドにターゲット フレームワークを設定する必要があります**API レベル 21 (ロリポップ)**またはそれ以降。 設定した場合、プロジェクトのターゲット フレームワーク API などレベル**API レベル 19 (KitKat)**呼び出そうとして、`SetCategory`コード内のメソッド、コンパイル エラーが表示されます。

常に使用してコンパイルすることをお勧め、*最新*使用可能なターゲット フレームワークのバージョン。 これにより、コードによって呼び出される可能性がありますすべての非推奨 Api 用の便利な警告メッセージと共に提供します。 最新のサポート ライブラリのリリースを使用する場合に特に重要はターゲット フレームワークの最新バージョンを使用して&ndash;各ライブラリはそのサポート ライブラリの最小 API レベルでのコンパイル以上でなければアプリが必要です。 

> [!NOTE]
> **注:**年 2018年 8 月以降、Google プレイ コンソールが必要になります API レベル 26 (Android 8.0) を対象とする新しいアプリまたはそれ以降。
既存のアプリは、API レベル 26 または年 2018年 11 月高い以降を対象とする必要があります。 詳細については、次を参照してください。[アプリのセキュリティとになる年の Google Play のパフォーマンスの向上](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)です。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio でのターゲット フレームワークの設定にアクセスするでのプロジェクト プロパティを開きます**ソリューション エクスプ ローラー**を選択し、**アプリケーション**ページ。

[![プロジェクトのプロパティのアプリケーション ページ](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png)

下にあるドロップダウン メニューの API レベルを選択して、ターゲット フレームワークを設定**Android バージョンを使用してコンパイル**上記のようにします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio でターゲット フレームワークの設定にアクセスするには、プロジェクト名を右クリックして**オプション**以外の場合はこの開きます、**プロジェクト オプション**ダイアログ。 このダイアログ ボックスに移動**ビルド > 全般**次のように。

[![プロジェクトのオプション ページの [全般] セクションをビルドします。](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png)

右側にドロップダウン メニューで API レベルを選択して、ターゲット フレームワークを設定**ターゲット フレームワーク**上記のようにします。

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>最低限の Android バージョン

*最低限の Android バージョン*(とも呼ばれる`minSdkVersion`) をインストールし、アプリケーションを実行できる Android OS (つまり、最低 API レベル) の最も古いバージョンです。 既定では、アプリは、必ずターゲット フレームワークの設定に一致するデバイスにインストールされて以降、最低限の Android バージョン設定がある場合*低い*ターゲット フレームワークの設定よりも、アプリは Android の以前のバージョンで実行もできます。 ターゲット フレームワークを設定する場合など、 **Android 7.1 (Nougat)**に最低限の Android バージョンを設定および**Android 4.0.3 (アイスクリーム サウスサンドウィッチ)**、API レベル 15 から任意のプラットフォームでアプリをインストールすることができますAPI レベル 25、包括的です。

アプリは可能性がありますが正常にビルドされ、このさまざまなプラットフォームにインストールが、これは保証されないものでは正常に*実行*これらすべてのプラットフォームにします。 アプリがインストールされている場合など、 **Android 5.0 (ロリポップ)**でのみ使用可能な API を呼び出すと**Android 7.1 (Nougat)**以降では、アプリはランタイム エラーが発生し、クラッシュする可能性があります。 したがって、コードを確認する必要があります&ndash;実行時に&ndash;で実行されている Android のデバイスでサポートされている Api のみを呼び出すことです。 つまり、コードでは、それらをサポートするのに十分な最新のデバイスでのみ、アプリが新しい Api を使用していることを確認する明示的なランタイム チェックを含める必要があります。
[ランタイムは Android バージョンをチェック](#runtimechecks)このガイドで後で、これらのランタイム チェックをコードに追加する方法について説明します。


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio での最低限の Android バージョンの設定にアクセスするでのプロジェクト プロパティを開きます**ソリューション エクスプ ローラー**を選択し、 **Android マニフェスト**ページ。 下にあるドロップダウン メニューで**最低限の Android バージョン**アプリケーションの最低限の Android バージョンを選択することができます。

[![ターゲット オプションの SDK バージョンを使用してコンパイルする設定に最低限の Android](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png)

選択した場合**を使用してコンパイル SDK バージョンを使用して**、最低限の Android のバージョンはターゲット フレームワークの設定と同じになります。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio でターゲット フレームワークの設定にアクセスするには、プロジェクト名を右クリックして**オプション**以外の場合はこの開きます、**プロジェクト オプション**ダイアログ。 移動**ビルド > Android アプリケーション**です。
右側にドロップダウン メニューを使用して**最低限の Android バージョン**アプリケーションの最低限の Android バージョンを設定することができます。

[ ![自動: ターゲット フレームワークのバージョンを使用する最低限の Android バージョンを設定](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png)

選択した場合**自動&ndash;ターゲット フレームワークのバージョンを使用して**、最低限の Android のバージョンはターゲット フレームワークの設定と同じになります。

-----


<a name="target" />

### <a name="target-android-version"></a>Android の対象バージョン

*ターゲット Android バージョン*(とも呼ばれる`targetSdkVersion`) は、API レベル、Android デバイスのアプリを実行します。 Android では、この設定を使用する任意の互換性の動作を有効にするかどうかを決定&ndash;これにより、アプリが期待どおりの動作を続行することです。 Android では、アプリのバージョンの Android ターゲット設定を使用して、分割 (これは Android が下位互換性を提供する方法) してもアプリにどの動作の変更を適用できますか。

ターゲット フレームワークと名前が非常に似ていて、Android ターゲット バージョンは同じものではありません。 ターゲット フレームワークの設定がターゲット API レベルの情報を Xamarin.Android で使用する場合は通信*コンパイル時*ターゲット Android バージョン ターゲット API レベルの情報を Android で使用するための通信中に、 *実行時*(、アプリがデバイスにインストールして実行) します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio でこの設定にアクセスするでのプロジェクト プロパティを開きます**ソリューション エクスプ ローラー**を選択し、 **Android マニフェスト**ページ。 下にあるドロップダウン メニューで**ターゲット Android バージョン**アプリケーションのターゲットの Android バージョンを選択することができます。

[![ターゲットの Android バージョンの SDK バージョンを使用してコンパイルする設定](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png)

明示的にアプリをテストするために使用する Android の最新バージョンをターゲット Android バージョンを設定することをお勧めします。 理想的には、最新の Android SDK のバージョンに設定する必要がある&ndash;動作の変更に進む前に新しい Api を使用できます。 ほとんどの開発者にとってお*しない*ターゲット Android バージョンを設定することを勧め**を使用してコンパイル SDK バージョンを使用して**です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio でターゲット フレームワークの設定にアクセスするには、プロジェクト名を右クリックして**オプション**以外の場合はこの開きます、**プロジェクト オプション**ダイアログ。 移動**ビルド > Android アプリケーション**です。
右側にドロップダウン メニューを使用して**ターゲット Android バージョン**アプリケーションのターゲット Android バージョンを設定することができます。

[![自動: ターゲット フレームワークのバージョンを使用するターゲット Android バージョンを設定](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png)

明示的にアプリをテストするために使用する Android の最新バージョンをターゲット Android バージョンを設定することをお勧めします。 理想的には、最新 Android SDK のバージョンに設定する必要がある&ndash;動作の変更に進む前に新しい Api を使用できます。 ほとんどの開発者にとってはお勧めしませんターゲット Android バージョンを設定**自動: ターゲット フレームワークのバージョンを使用して**です。

-----

一般に、ターゲットの Android バージョンは、最低限の Android バージョンおよびターゲット フレームワークによって制限する必要があります。 つまり、

**最低限の Android バージョン < ターゲット Android バージョンを = < ターゲット フレームワークを =**

SDK レベルの詳細については、Android Developer を参照してください。[使用 sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html)ドキュメント。


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Android バージョンのランタイム チェック

新しいを提供するフレームワーク API を更新する Android の新しい各バージョンがリリースされると、またはこれに代わる機能です。 いくつかの例外を除き、Android の以前のバージョンからの API 機能が引き継がれますに変更することがなく新しい Android バージョン。 その結果、アプリは、特定の Android API レベルで実行している場合は通常可能になって加えず以降、Android API レベルで実行します。 場合もたいが以前のバージョンの Android でアプリを実行しますか。

ある最低限の Android バージョンを選択した場合*低い*ターゲット フレームワークの設定よりもいくつかの Api できない可能性があります、アプリ実行時に使用します。 ただし、制限された機能では以前のデバイスでアプリを実行もできます。 Android プラットフォームでは、最低限の Android のバージョンの設定に対応するには無効な各 API のコード、必要があります明示的に確認の値、`Android.OS.Build.VERSION.SdkInt`プロパティで、アプリが実行されているプラットフォームの API レベルを決定します。 API レベルが場合*低い*呼び出すには、必要な API をサポートする最低限の Android バージョンよりも、この API 呼び出しを行わずに正しく機能する方法を見つけ、コードには。

たとえば、たとえば、使用すること、 [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/)で実行されているときに通知を分類する方法**Android 5.0 ロリポップ**(以降)、このアプリにも必要です。Android の以前のバージョンなどの実行**Android 4.1 ゼリー Bean** (ここで`SetCategory`は使用できません)。 このガイドの先頭の Android バージョン テーブルを参照する、表示するビルドのバージョンのコードを**Android 5.0 ロリポップ**は`Android.OS.BuildVersionCodes.Lollipop`します。 Android の場所の古いバージョンをサポートするために`SetCategory`が利用できない、コードが実行時に、API レベルを検出し、条件付きで呼び出せる`SetCategory`のみと API レベルより大きいか等しいロリポップ ビルドのバージョンのコードに。

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

この例では、アプリのターゲット フレームワークが に設定されている**Android 5.0 (API レベル 21)**に設定されている最低限の Android バージョンおよび**Android 4.1 (API レベル 16)**です。 `SetCategory` API レベルにある`Android.OS.BuildVersionCodes.Lollipop`と後で、このコード例は`SetCategory`が実際に使用できる&ndash;なります*いない*を呼び出そうと`SetCategory`ときに APIレベルは、16、17、18、19、または 20 です。 のみ身元を正しく (ための型によって分類されない) と、通知の並べ替えは行われません、まだでも、この通知がユーザーに警告を発行されています。 これら以前 Android のバージョンで機能が制限されます。 アプリは引き続き動作しますが、その機能が若干低下します。

一般に、ビルドのバージョン チェックは、コードが従来の方法ではなく新しい方法を何らかの間での実行時に決定に役立ちます。 例:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    // Do things the Lollipop way
}
else
{
    // Do things the pre-Lollipop way
}
```

減らすか、1 つまたは複数の Api が欠如している以前の Android バージョンでの実行時に、アプリの機能を変更する方法について説明する高速で単純なルールはありません。 場合によっては (などで、`SetCategory`上記の例)、単が利用できない場合に、API 呼び出しを省略するだけで十分です。 ただし、それ以外の場合、必要がありますのときに代替機能を実装する`Android.OS.Build.VERSION.SdkInt`レベル、アプリが、最適なエクスペリエンスを提供する必要があること、API よりも小さくすることが検出されました。

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API レベルおよびライブラリ

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

ターゲット フレームワークの設定のみを構成することができます (など、クラス ライブラリまたはライブラリにバインド) Xamarin.Android ライブラリ プロジェクトを作成するときに&ndash;最低限の Android バージョンおよびターゲットの Android バージョン設定は使用できません。 これがあるためありません**Android マニフェスト**ページ。

[![Android バージョン オプションを使用してコンパイルのみが使用可能です](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.Android ライブラリ プロジェクトを作成するときにない**Android アプリケーション**最低限の Android バージョンおよびターゲット Android バージョンを構成するページ&ndash;最低限の Android バージョンとターゲットAndroid バージョン設定は、ご利用いただけません。
これがあるためありません**ビルド > Android アプリケーション**ページ)。

[ ![最小値とターゲットのバージョンのオプションを [全般] ページをビルドします。](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png)

-----

最低限の Android バージョンおよびターゲット Android バージョン設定は使用できません、結果として得られるライブラリは、スタンドアロンのアプリではないため&ndash;ライブラリに付属するアプリによっては、すべての Android バージョンで実行できます。 ライブラリをする方法を指定することができます*コンパイル*が上のライブラリで実行されるどのプラットフォーム API レベルを予測することはできません。 これを踏まえては、次のベスト プラクティスを使用またはライブラリを作成するときに従ってください。

-   **Android ライブラリを使用する際に** &ndash; Android ライブラリを使用して、アプリケーションでは場合、は、レベルに設定する API には、アプリのターゲット フレームワークを設定することを確認する*少なくともと同程度に高*ターゲットライブラリのフレームワークの設定です。

-   **Android ライブラリを作成するときに**&ndash;他のアプリケーションで使用する Android ライブラリを作成する場合にコンパイルするために必要な最小 API レベルのターゲット フレームワークの設定を設定することを確認します。

これらのベスト プラクティスは、ライブラリが (アプリのクラッシュが発生することができます) を実行時に使用される API を呼び出すしようとした場所のような状況を防ぐために役立つことをお勧めします。 ライブラリ開発者は場合、は、API サーフェス領域全体の小規模で確立されてサブセットへの API 呼び出しの使用を制限する限り必要があります。 こうライブラリをより広い範囲の Android バージョン間で安全に使用できることを確認してください。


## <a name="summary"></a>まとめ

このガイドでは、Android API レベルを使用して Android の異なるバージョン間でアプリケーションの互換性を管理する方法について説明します。 詳細な手順を Xamarin.Android を構成するために提供*ターゲット フレームワーク*、*最低限の Android バージョン*、および*ターゲット Android バージョン*プロジェクト設定します。 これには、Android SDK Manager を使用して、SDK パッケージをインストールするための手順を実行時に、さまざまな API レベルに対処するコードを記述する方法の例が含まれているし、作成または Android ライブラリを使用する場合の API レベルを管理する方法を説明しましたが用意されています。 API レベルを Android バージョン番号 (Android 4.4) など、Android バージョン名 (など Kitkat)、および Xamarin.Android ビルド バージョンのコードに関連する包括的なリストも提供されます。


## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [SDK CLI ツールを変更します。](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [ピッキング compileSdkVersion、minSdkVersion、および targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [API レベルとは何ですか。](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [コード名、タグ、およびビルド番号](https://source.android.com/source/build-numbers)
