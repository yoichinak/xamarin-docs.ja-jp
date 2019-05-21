---
title: Android API レベルを理解します。
description: Xamarin.Android では、複数のバージョンの Android アプリの互換性を確認するいくつかの Android API レベル設定があります。 このガイドは、これらの設定の意味、構成する方法、およびどのような効果について説明します。 実行時に、アプリであります。
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: 1f88525fefb83c92d5e5dda2176d3622bb67c78d
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/20/2019
ms.locfileid: "65924961"
---
# <a name="understanding-android-api-levels"></a>Android API レベルの理解

_Xamarin.Android では、複数のバージョンの Android アプリの互換性を確認するいくつかの Android API レベル設定があります。このガイドは、これらの設定の意味、構成する方法、およびどのような効果について説明します。 実行時に、アプリであります。_


## <a name="quick-start"></a>クイック スタート

Xamarin.Android では、次の 3 つの Android API レベルのプロジェクト設定を公開します。

-   [フレームワークをターゲットに](#framework)&ndash;アプリケーションの構築に使用するには、どのフレームワークを指定します。 この API のレベルに使用される*コンパイル*Xamarin.Android で時間。

-   [最小 Android バージョン](#minimum)&ndash;アプリでサポートするために使用すること、最も古い Android バージョンを指定します。 この API のレベルに使用される*実行*Android によって時間。

-   [ターゲット Android バージョン](#target)&ndash;上で実行するためのもので、アプリが Android のバージョンを指定します。 この API のレベルに使用される*実行*Android によって時間。

API レベルを構成するには、プロジェクトの前に、その API レベルの SDK プラットフォームのコンポーネントをインストールする必要があります。 ダウンロードとインストールの Android SDK コンポーネントに関する詳細については、次を参照してください。 [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)します。

> [!NOTE]
> 2018 の年 8 月以降、Google Play コンソールが必要になります新しいアプリがターゲット API レベル 26 (Android 8.0) で、またはそれ以降。
既存のアプリは、API レベル 26 または 2018 年 11 月以降の高い対象にする必要があります。 詳細については、次を参照してください。[アプリのセキュリティとする年の Google Play でのパフォーマンスの向上](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

通常、3 つすべての Xamarin.Android の API レベルは、同じ値に設定されます。 **アプリケーション**設定ページで、 **Android バージョン (ターゲット フレームワーク) を使用してコンパイル**を最新の安定した API バージョン (または、最低限必要な機能をすべての Android バージョンに)。
設定されているターゲット フレームワークでは、次のスクリーン ショットでは、 **Android 7.1 (API レベル 25 - Nougat)**:

[![ターゲット Android バージョンを使用してコンパイルするフレームワークのバージョンの既定値](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

**Android マニフェスト**最小 Android バージョンを設定 ページで、**使用の SDK バージョンを使用してコンパイル**し (次のターゲット フレームワークのバージョンと同じ値に、対象の Android バージョンを設定スクリーン ショットでは、ターゲットの Android フレームワークに設定されている**Android 7.1 (Nougat)**)。

[![最小値とターゲット Android バージョンがターゲット フレームワークのバージョンに設定します。](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Android の以前のバージョンと旧バージョンとの互換性を維持する場合は、設定**対象とする最小 Android バージョン**最も古いバージョンの Android アプリでサポートするために使用することにします。 (必要な最小 API レベルは、API レベル 14 [Google play 開発者サービスと Firebase サポート](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html))。次の例の構成には、API レベル 25 で API レベル 14 からの Android バージョンがサポートされています。

[![API レベル 25 を使用してコンパイル Nougat、API レベル 14 に設定された最小 Android バージョン](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

通常、3 つすべての Xamarin.Android の API レベルは、同じ値に設定されます。 設定**ターゲット フレームワーク**を最新の安定した API バージョン (または、最低限必要な機能をすべての Android バージョンに)。 設定する、**ターゲット フレームワーク**に移動します**ビルド > 全般**で、**プロジェクト オプション**します。 設定されているターゲット フレームワークでは、次のスクリーン ショットでは、 **、最新バージョンにインストールされたプラットフォーム (8.0) を使用して、**:

[![既定でインストールされている最新の使用のプラットフォーム ターゲット フレームワーク](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

最小値とターゲット Android バージョン設定が見つかりません**ビルド > Android アプリケーション**で**プロジェクト オプション**します。 最小 Android バージョンを設定**自動 - ターゲット フレームワーク バージョンを使用して**し、対象の Android バージョンをターゲット フレームワークのバージョンと同じ値に設定します。 設定されているターゲット Android のフレームワークでは、次のスクリーン ショットでは、 **Android 8.0 (API レベル 26)** 上のターゲット フレームワーク設定と一致します。

[![プロジェクト オプションで、ターゲット フレームワーク レベルの設定](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Android の以前のバージョンと旧バージョンとの互換性を維持する場合は、変更**Minimum Android version**最も古いバージョンの Android アプリでサポートするために使用することにします。 必要な最小 API レベルは、API レベル 14 [Google play 開発者サービスと Firebase サポート](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)します。
たとえば、次の構成はサポートと、Android バージョンは早い段階では、API レベル 14。

[![最小値とターゲットのバージョンの [自動] に設定を使用して、ターゲット フレームワークのバージョン](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


アプリでは、複数の Android バージョンをサポートする場合、コードが最低限の Android のバージョンの設定で、アプリが動作するためにランタイム チェックを含める必要があります (を参照してください[Android バージョンを実行時チェック](#runtimechecks)以下の詳細については)。 使用またはライブラリを作成する場合は、次を参照してください。 [API レベルとライブラリ](#libraries)以下の API の構成に関するベスト プラクティスのレベル ライブラリの設定。



## <a name="android-versions-and-api-levels"></a>Android のバージョンと API レベル

Android の各バージョンでと呼ばれる、一意の整数識別子が割り当てられている、Android プラットフォームの進化し、新しい Android バージョンがリリースされた、 *API レベル*します。 そのため、各 Android バージョンは、1 つの Android API レベルに対応します。 ユーザーは、以前にも、最も最近のバージョンの Android アプリをインストール、ため、複数の Android API レベルを使用する実際の Android アプリを設計する必要があります。


### <a name="android-versions"></a>Android バージョン

Android の各リリースでは、複数の名前でします。

-   Android のバージョンなど**Android 9.0**
-   コード (またはデザート) 名を_円_
-   対応する API レベルなど、 **API レベル 28**

Android のコード名は、複数のバージョンと API レベル (次の表に表示) に対応可能性がありますが、各 Android バージョンが正確に 1 つの API レベルに対応しています。

Xamarin.Android をさらに、定義*バージョン コードをビルド*現在認識されている Android API レベルにマップされます。 次の表を使用して、API レベル、Android バージョン、コードの名前、および Xamarin.Android のビルド バージョン コードの間で変換できます (でビルド バージョンのコードが定義されている、`Android.OS`名前空間)。

[!include[](~/android/includes/api-levels.md)]

新しいバージョンの Android が頻繁にリリースされた次の表に示す、 &ndash; 1 年あたり場合によっては複数のリリース。 その結果、アプリを実行する可能性のある Android デバイスの世界にはさまざまな Android バージョンが含まれます。 アプリで実行される一貫して確実に非常に多くの異なるバージョンの Android を保証する方法 Android の API レベルでは、この問題を管理できます。


### <a name="android-api-levels"></a>Android API レベル

各 Android デバイスを正確に実行で*1 つ*API レベル&ndash;この API レベルの Android プラットフォームのバージョンごとに一意であることが保証されます。 API レベルには、アプリを呼び出すことができる API のセットのバージョンを正確に識別します。開発者としてのマニフェストの要素、アクセス許可などに対してコードの組み合わせを識別します。 API レベルの android のシステムにより、Android アプリケーションは、デバイスにアプリケーションをインストールする前に、Android のシステム イメージと互換性があるかどうかを判断します。

アプリケーションをビルドするとき、次の API レベルの情報が含まれています。

-   *ターゲット*のアプリをビルドして実行する Android API レベルです。

-   *最小*アプリを実行する Android デバイスに必要な Android API レベルです。 

これらの設定は、インストール時にアプリを正しく実行するために必要な機能が Android デバイスで使用可能なであることを確認に使用されます。 それ以外の場合は、そのデバイスで実行されているから、アプリがブロックされます。 たとえば、Android デバイスの API レベルは、アプリの指定した最小 API レベルより低いが場合、Android デバイスはユーザーからアプリをインストールできません。


## <a name="project-api-level-settings"></a>API をプロジェクト レベルの設定

次のセクションでは、SDK Manager を使用して構成する方法の詳細な説明の後に、対象とする API レベルの開発環境を準備する方法を説明する*ターゲット フレームワーク*、*最小値Android バージョン*、および*Target Android version* Xamarin.Android で設定します。


### <a name="android-sdk-platforms"></a>Android SDK プラットフォーム

Xamarin.Android では、ターゲットまたは最小 API レベルを選択できますが、前に、API レベルに対応する Android SDK プラットフォームのバージョンをインストールする必要があります。 ターゲット フレームワーク、Minimum Android version、およびターゲット Android バージョンで使用できる選択肢の範囲は、インストールされている、バージョンの Android SDK の範囲に制限されます。 SDK Manager を使用するには、必要な Android SDK バージョンがインストールされている、され、使用して、アプリに必要なすべての新しい API レベルを追加するのにことを確認します。 API レベルをインストールする方法に慣れていない場合は、次を参照してください。 [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)します。

<a name="framework" />

### <a name="target-framework"></a>[対象とする Framework]

*ターゲット フレームワーク*(とも呼ばれます`compileSdkVersion`) のビルド時に、アプリがコンパイルされた特定の Android フレームワーク バージョン (API レベル) です。 この設定は、アプリの Api を指定します*は*が実行されますが、いる Api は、アプリを実際に使用できるがインストールされているときに影響を与えませんときに使用します。 結果として、ターゲット フレームワークの設定を変更しても、ランタイム動作は変更しません。

ターゲット フレームワークに対して、アプリケーションがリンクされているライブラリ バージョンを識別します&ndash;この設定は、アプリで使用できる Api を決定します。 使用する場合など、 [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) Android 5.0 Lollipop で導入されたメソッドと、ターゲット フレームワークを設定する必要があります**API レベル 21 (Lollipop)** またはそれ以降。 設定した場合、プロジェクトのターゲット フレームワーク、API レベルなど**API レベル 19 (KitKat)** 呼び出しを試みると、`SetCategory`メソッド、コードで、コンパイル エラーが表示されます。

コンパイルを常にお勧め、*最新*利用可能なターゲット フレームワークのバージョン。 これにより、コードによって呼び出される可能性を非推奨の Api の便利な警告メッセージを提供します。 最新のサポート ライブラリのリリースを使用する場合に特に重要ですが最新のターゲット フレームワーク バージョンを使用して&ndash;ライブラリごとにそのサポート ライブラリの最小 API レベルでコンパイルされた以上にするため、アプリで必要があります。 


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio でターゲット フレームワークの設定にアクセスするでプロジェクトのプロパティを開きます**ソリューション エクスプ ローラー**を選択し、**アプリケーション**ページ。

[![プロジェクトのプロパティのアプリケーション ページ](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

下にあるドロップダウン メニューで、API レベルを選択して、ターゲット フレームワークを設定**Android バージョンを使用してコンパイル**上記のようです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual studio for Mac のターゲット フレームワークの設定にアクセスするには、プロジェクト名を右クリックして**オプション**; この開きます、**プロジェクト オプション**ダイアログ。 このダイアログに移動します。**ビルド > 全般**次のように。

[![[プロジェクト オプション] ページの [全般] セクションをビルドします。](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

右側にドロップダウン メニューで、API レベルを選択して、ターゲット フレームワークを設定**ターゲット フレームワーク**上記のようです。

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Android の最小バージョン

*Minimum Android version* (とも呼ばれます`minSdkVersion`) をインストールして、アプリケーションを実行する Android OS (つまり、最低 API レベル) の最も古いバージョンです。 既定では、アプリは、必ずターゲット フレームワークの設定に一致するデバイスにインストールされているか、それ以上最小 Android バージョン設定がある場合*低い*ターゲット フレームワークの設定よりも、アプリは以前のバージョンの Android で実行もできます。 例では、ターゲット フレームワークを設定した場合の**Android 7.1 (Nougat)** 最小 Android バージョンを設定および**Android 4.0.3 (Ice Cream Sandwich)**、API レベル 15 から任意のプラットフォームでアプリをインストールすることができますAPI レベル 25、包括的です。

正常には保証されませんが、アプリが正常にビルドおよびこのさまざまなプラットフォーム上でインストール、*実行*これらすべてのプラットフォームにします。 アプリがインストールされている場合など**Android 5.0 (Lollipop)** でのみ使用できる API を呼び出すと**Android 7.1 (Nougat)** 以降で、アプリは、実行時エラーし、クラッシュする可能性があります。 したがって、コードを確認する必要があります&ndash;時&ndash;で実行されている Android デバイスでサポートされている Api のみを呼び出していること。 つまり、コードでは、アプリがそれらをサポートする最新のデバイスでのみ新しい Api が使用されるように明示的なランタイム チェックを含める必要があります。
[Android バージョンを実行時チェック](#runtimechecks)このガイドで後で、これらのランタイム チェックをコードに追加する方法について説明します。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio での最小 Android バージョンの設定にアクセスするでプロジェクトのプロパティを開きます**ソリューション エクスプ ローラー**を選択し、 **Android マニフェスト**ページ。 下にあるドロップダウン メニューで**Minimum Android version**アプリケーションの最小 Android バージョンを選択することができます。

[![最小 Android ターゲット オプションの SDK バージョンを使用してコンパイルに設定するには](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

選択した場合**使用の SDK バージョンを使用してコンパイル**、最小 Android バージョンには、ターゲット フレームワークの設定と同じになります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio での最小 Android バージョン for Mac にアクセスするには、プロジェクト名を右クリックして**オプション**; この開きます、**プロジェクト オプション**ダイアログ。 移動します**ビルド > Android アプリケーション**します。
右側にドロップダウン メニューを使用して**Minimum Android version**アプリケーションの最小 Android バージョンを設定することができます。

[![自動 - ターゲット フレームワーク バージョンを使用する Android の最小バージョンを設定](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

選択した場合**自動&ndash;ターゲット フレームワークのバージョンを使用して、**、最小 Android バージョンには、ターゲット フレームワークの設定と同じになります。

-----


<a name="target" />

### <a name="target-android-version"></a>ターゲット Android バージョン

*ターゲット Android バージョン*(とも呼ばれます`targetSdkVersion`) は、API レベルで Android デバイスのアプリで実行します。 Android では、この設定を使用する、互換性動作を有効にするかどうかを決定&ndash;これにより、アプリが期待どおりに動作し続けることです。 Android では、アプリの対象の Android のバージョンの設定を使用して、、どの動作の変更は、(これは Android が上位互換性を提供する方法) を損なうことがなく、アプリに適用できます。

ターゲット フレームワークと非常に類似した名前は、ことができますが、対象の Android バージョンが同じではありません。 ターゲット フレームワークの設定は、通信に使用するために Xamarin.Android にターゲット API レベル情報*コンパイル時*ターゲット Android バージョンはターゲット API レベルの情報を Android で使用する場合、通信中に、 *実行時*(アプリがデバイスにインストールして実行)。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio では、この設定にアクセスするでプロジェクトのプロパティを開きます**ソリューション エクスプ ローラー**を選択し、 **Android マニフェスト**ページ。 下にあるドロップダウン メニューで**Target Android version**アプリケーションの対象の Android バージョンを選択することができます。

[![ターゲット Android バージョンの SDK バージョンを使用してコンパイルする設定](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

最新バージョンの Android アプリをテストするために使用する対象の Android バージョンを明示的に設定することをお勧めします。 理想的には、最新の Android SDK バージョンに設定する必要がある&ndash;動作の変更に従って作業する前に新しい Api を使用することができます。 ほとんどの開発者*しない*ターゲット Android バージョンに設定をお勧めします**使用の SDK バージョンを使用してコンパイル**します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac を Visual Studio では、この設定にアクセスするには、プロジェクト名を右クリックして**オプション**; この開きます、**プロジェクト オプション**ダイアログ。 移動します**ビルド > Android アプリケーション**します。 右側にドロップダウン メニューを使用して**Target Android version**アプリケーションのターゲット Android バージョンを設定することができます。

[![ターゲット Android バージョンが自動 - ターゲット フレームワーク バージョンを使用する設定](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

最新バージョンの Android アプリをテストするために使用する対象の Android バージョンを明示的に設定することをお勧めします。 理想的には、最新使用可能な Android SDK バージョンに設定する必要がある&ndash;動作の変更に従って作業する前に新しい Api を使用することができます。 ほとんどの開発者にお勧めしませんターゲット Android バージョンを設定**自動 - ターゲット フレームワーク バージョンを使用して**します。

-----

一般に、最小 Android バージョンとターゲット フレームワークをターゲット Android バージョンを制限する必要があります。 つまり、

**最小 Android バージョン < ターゲット Android バージョンを = < ターゲット フレームワークを =**

SDK レベルの詳細については、Android の開発者を参照してください。[使用 sdk](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html)ドキュメント。


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Android のバージョンを実行時チェックします。

新しいを提供するフレームワークの API が更新された Android の新しい各バージョンがリリースされると、またはこれに代わる機能です。 いくつかの例外を除き、以前の Android バージョンからの API 機能は以降の Android バージョンを変更せずに前方に実行されます。 その結果、アプリは、特定の Android API レベルで実行する場合通常なります変更を加えなくてもそれ以降の Android API レベルで実行できるようします。 しかし、以前のバージョンの Android アプリの実行にもする場合でしょうか。

最小 Android バージョンを選択した場合*低い*ターゲット フレームワークの設定よりも一部の Api できない可能性がありますを実行時にアプリで使用します。 ただし、以前のデバイスが機能を制限して、アプリを実行もできます。 各 API では、最低限の Android のバージョンの設定に対応する Android のプラットフォームで使用できる、コードする必要があります明示的に値をチェックの`Android.OS.Build.VERSION.SdkInt`プロパティで、アプリが実行されているプラットフォームの API レベルを決定します。 API レベルの場合*低い*を呼び出すと、する API をサポートする最小 Android バージョンよりも、コードでこの API 呼び出しを行わずに正しく機能する方法を見つけるには。

たとえば、たとえばを使用すること、 [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/)メソッドで実行されているときに通知を分類する**Android 5.0 Lollipop** (とそれ以降) は引き続き、アプリを使用します以前のバージョンの Android などの実行**Android 4.1 Jelly Bean** (場所`SetCategory`は使用できません)。 このガイドの先頭に Android のバージョンのテーブルを指すことがわかりますのビルド バージョン コード**Android 5.0 Lollipop**は`Android.OS.BuildVersionCodes.Lollipop`します。 以前のバージョンの Android の場所をサポートするために`SetCategory`は使用できないコードは実行時に API レベルの検出、条件付きで呼び出す`SetCategory`今後、API レベルがより大きいまたはロリポップ ビルドのバージョンのコードに等しい場合はのみ。

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

この例では、アプリのターゲット フレームワークに設定されて**Android 5.0 (API レベル 21)** に設定されている最小 Android バージョンと**Android 4.1 (API レベル 16)** します。 `SetCategory` API レベルでは`Android.OS.BuildVersionCodes.Lollipop`と後で、次のコード例が呼び出されます`SetCategory`が実際に使用できる&ndash;は*いない*を呼び出そうとしました`SetCategory`と APIレベルは、16、17、18、19、または 20 です。 のみを正しく (ための種類で分類されない) と、通知の並べ替えは行われません、まだ、通知がユーザーに警告するまだ公開されてこれら以前 Android バージョンの機能が減少します。 アプリは引き続き機能しますが、その機能が若干低下します。

一般に、ビルドのバージョン チェックでは、決定を行っている従来の方法と、新しい方法の間での実行時に、コードが役立ちます。 例:

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

小さくかは、1 つまたは複数の Api が欠如している古い Android バージョンで実行時に、アプリの機能を変更する方法を説明する高速で単純なルールはありません。 場合によっては (など、`SetCategory`上記の例) が利用できない場合は、API 呼び出しを省略するだけで十分です。 ただし、それ以外の場合必要場合の代替機能を実装する`Android.OS.Build.VERSION.SdkInt`が検出されたアプリは、その最適なエクスペリエンスを提供する必要があるレベル API よりも小さくします。

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API レベルとライブラリ

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

ターゲット フレームワークの設定を構成することができます (クラス ライブラリやバインド ライブラリ) など、Xamarin.Android ライブラリ プロジェクトを作成するときに&ndash;最小 Android バージョンおよびターゲット Android バージョン設定は使用できません。 するが存在しない**Android マニフェスト**ページ。

[![Android のバージョンのオプションを使用してコンパイルのみが使用可能です](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xamarin.Android ライブラリ プロジェクトを作成するときにない**Android アプリケーション**最小 Android バージョンとターゲット Android バージョンを構成するページに&ndash;最小 Android バージョンとターゲットAndroid のバージョン設定は使用できません。
あるため、あるありません**ビルド > Android アプリケーション**ページ。

[![ビルドの最小値とターゲットのバージョンのオプションを指定せず、[全般] ページ](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

最小 Android バージョンとターゲット Android バージョン設定を利用できません、結果として得られるライブラリがスタンドアロンのアプリではないため、&ndash;ライブラリとパッケージ化されているアプリによっては、任意の Android バージョンで実行できます。 ライブラリのある方法を指定する*コンパイル*ライブラリが動作するどのプラットフォームの API レベルを予測することはできませんが、します。 これを踏まえては、次のベスト プラクティスを消費したり、ライブラリを作成するときに従ってください。

-   **Android のライブラリを使用するときに** &ndash; 、アプリケーションで、Android ライブラリを利用する場合は、レベルに設定する API には、アプリのターゲット フレームワークを設定することを確認する*少なくともと同程度に高*ターゲットライブラリのフレームワークの設定です。

-   **Android のライブラリを作成するときに**&ndash;他のアプリケーションで使用する Android のライブラリを作成する場合に、ターゲット フレームワークの設定をコンパイルするために必要な最小 API レベルに設定することを確認します。

これらのベスト プラクティスは、ライブラリが (アプリのクラッシュを発生することができます) がランタイムでは利用できない API を呼び出すしようとする状況を防ぐためお勧めします。 ライブラリ開発者の場合は、中小に確立された合計の API サーフェス領域のサブセットに対する API 呼び出しの使用量を制限するよう努力する必要があります。 こうライブラリをより広い範囲の Android バージョン間で安全に使用できることを確認します。


## <a name="summary"></a>まとめ

このガイドでは、Android API レベルを使用して Android の異なるバージョン間でアプリケーションの互換性を管理する方法について説明します。 Xamarin.Android の構成の詳細な手順が提供される*ターゲット フレームワーク*、 *Minimum Android version*、および*Target Android version*プロジェクトの設定。 Android SDK Manager を使用して、SDK パッケージをインストールするための手順は、実行時にさまざまな API レベルで処理するコードを記述する方法の例を含まれており、作成または Android ライブラリを使用するときに、API レベルを管理する方法について説明しましたが用意されています。 (Android 4.4) などの Android バージョン番号、Android バージョン名 (など Kitkat)、および Xamarin.Android のビルド バージョンのコードに API レベルを関連する包括的な一覧も提供されます。


## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [SDK の CLI ツールを変更します。](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [CompileSdkVersion、minSdkVersion、および [targetsdkversion] を選択](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [API レベルとは何ですか。](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [含めて、タグ、およびビルド番号](https://source.android.com/source/build-numbers)
