---
title: Android API レベルについて
description: Xamarin Android には、android の複数のバージョンとのアプリの互換性を判断する複数の Android API レベル設定があります。 このガイドでは、これらの設定の意味、構成方法、実行時のアプリへの影響について説明します。
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: fba49e21ac75ec1ebb00614f3891bebaa57a3ed5
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249761"
---
# <a name="understanding-android-api-levels"></a>Android API レベルの理解

_Xamarin Android には、android の複数のバージョンとのアプリの互換性を判断する複数の Android API レベル設定があります。このガイドでは、これらの設定の意味、構成方法、実行時のアプリへの影響について説明します。_

## <a name="quick-start"></a>クイック スタート

Xamarin Android では、次の3つの Android API レベルのプロジェクト設定が公開されています。

- [ターゲットフレームワーク](#framework)&ndash;アプリケーションのビルドに使用するフレームワークを指定します。 この API レベルは、*コンパイル*時に Xamarin Android によって使用されます。

- [Android の最小バージョン](#minimum)&ndash;アプリでサポートする最も古い Android バージョンを指定します。 この API レベルは、*実行*時に Android によって使用されます。

- [ターゲット Android バージョン](#target)&ndash;アプリを実行する対象の Android のバージョンを指定します。 この API レベルは、*実行*時に Android によって使用されます。

プロジェクトの API レベルを構成する前に、その API レベルの SDK プラットフォームコンポーネントをインストールする必要があります。 Android SDK コンポーネントのダウンロードとインストールの詳細については、「 [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)」を参照してください。

> [!NOTE]
> 2018年8月以降、Google Play コンソールでは、新しいアプリが API レベル 26 (Android 8.0) 以降を対象とする必要があります。
既存のアプリは、2018年11月以降の API レベル26以降をターゲットにする必要があります。 詳細については、「 [Google Play でのアプリのセキュリティとパフォーマンスの向上](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)」を参照してください。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

通常、3つの Xamarin Android API レベルはすべて同じ値に設定されます。 **[アプリケーション]** ページで、 **[Android バージョン (ターゲットフレームワーク) を使用してコンパイル]** を最新の安定した API バージョン (少なくとも、必要なすべての機能を備えた android バージョンに) に設定します。
次のスクリーンショットでは、ターゲットフレームワークは**Android 7.1 (API レベル 25-Nougat)** に設定されています。

[![ターゲットフレームワークバージョンの既定値は、Android バージョンを使用してコンパイルされる](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

**[Android マニフェスト]** ページで、 **[SDK バージョンを使用したコンパイルを使用]** するように最小 Android バージョン を設定し、ターゲットの Android バージョンをターゲットフレームワークのバージョンと同じ値に設定します (次のスクリーンショットでは、ターゲットの android フレームワークがに設定されています)。**Android 7.1 (Nougat)** ):

[![ターゲットのフレームワークバージョンに設定されている最小およびターゲットの Android バージョン](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

以前のバージョンの Android との下位互換性を維持するには、アプリでサポートする最も古いバージョンの android を対象とするように**最小 android バージョン**を設定します。 (API レベル14は、 [Google Play services および焼討 base サポート](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)に必要な最小 api レベルです)。次の構成例では、api レベル14から API レベル25までの Android バージョンがサポートされています。

[![API レベル 25 Nougat を使用してコンパイルし、最小 Android バージョンを API レベル14に設定します](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

通常、3つの Xamarin Android API レベルはすべて同じ値に設定されます。 **ターゲットフレームワーク**を最新の安定した API バージョン (少なくとも、必要なすべての機能を備えた Android バージョンに設定します) に設定します。 **ターゲットフレームワーク**を設定するには、 **[プロジェクトオプション]** の **[ビルド > 全般**] に移動します。 次のスクリーンショットでは、ターゲットフレームワークは、**インストールされている最新のプラットフォーム (8.0) を使用**するように設定されています。

[![ターゲットフレームワークの既定でインストールされている最新のプラットフォームを使用する](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Android バージョンの最小値とターゲット設定は、 **[プロジェクトオプション]** の **[ビルド > android アプリケーション]** の下にあります。 Android の最小バージョンを **自動** に設定し、ターゲットフレームワークのバージョンを ターゲットフレームワークのバージョン と同じ値に設定します。 次のスクリーンショットでは、ターゲットの Android フレームワークが、上記のターゲットフレームワークの設定と一致するように**android 8.0 (API レベル 26)** に設定されています。

[![プロジェクトオプションでターゲットレベルとフレームワークレベルを設定する](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

以前のバージョンの Android との下位互換性を維持するには、 **android の最小バージョン**を、アプリでサポートする最も古いバージョンの android に変更します。 API レベル14は、 [Google Play services および焼討 base サポート](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html)に必要な api レベルの最小値であることに注意してください。
たとえば、次の構成では、API レベル14より早い段階で Android バージョンがサポートされています。

[![最小およびターゲットバージョンが自動に設定されている-ターゲットフレームワークのバージョンを使用する](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----

アプリが複数の Android バージョンをサポートしている場合は、アプリが Android の最小バージョン設定で動作することを確認するためのランタイムチェックをコードに含める必要があります (詳細については、以下[の「Android バージョンのランタイムチェック](#runtimechecks)」を参照してください)。 ライブラリを使用または作成する場合は、ライブラリの API レベル設定を構成するためのベストプラクティスについて、以下の[Api レベルとライブラリ](#libraries)を参照してください。

## <a name="android-versions-and-api-levels"></a>Android のバージョンと API レベル

Android プラットフォームが進化し、新しい Android バージョンがリリースされると、各 Android バージョンには*API レベル*と呼ばれる一意の整数識別子が割り当てられます。 したがって、Android の各バージョンは、1つの Android API レベルに対応します。 ユーザーはアプリを古いバージョンの Android と最新バージョンの Android にインストールするため、実際の Android アプリは複数の Android API レベルで動作するように設計する必要があります。

### <a name="android-versions"></a>Android のバージョン

Android の各リリースには、次の複数の名前が付いています。

- Android **9.0**などの android バージョン
- _円_などのコード (またはデザート) の名前
- **Api レベル 28**などの対応する api レベル

Android コード名は、次の表に示すように、複数のバージョンおよび API レベルに対応している場合がありますが、各 Android バージョンは正確に1つの API レベルに対応しています。

さらに、Xamarin Android は、現在認識されている Android API レベルにマップされる*ビルドバージョンコード*を定義します。 次の表は、API レベル、android バージョン、コード名、Xamarin の各ビルドバージョンコードを変換する際に役立ちます (ビルドバージョンコードは`Android.OS`名前空間で定義されています)。

[!include[](~/android/includes/api-levels.md)]

この表に示すように、新しい Android バージョンは&ndash; 、頻繁にリリースされる場合があります。 その結果、アプリを実行する可能性があるさまざまな android デバイスに、古いバージョンと新しい Android バージョンが含まれています。 Android のさまざまなバージョンで、アプリが一貫して確実に動作することを保証するには、どうすればよいでしょうか。 Android の API レベルは、この問題を管理するのに役立ちます。

### <a name="android-api-levels"></a>Android API レベル

各 android デバイスは、厳密に*1 つ*の api レベル&ndash;で実行されます。この api レベルは、android プラットフォームのバージョンごとに一意であることが保証されています。 API レベルは、アプリが呼び出すことができる API セットのバージョンを正確に識別します。開発者としてコーディングするマニフェスト要素、アクセス許可などの組み合わせを識別します。 Android の API レベルのシステムにより、android は、アプリケーションをデバイスにインストールする前に Android システムイメージと互換性があるかどうかを判断します。

アプリケーションがビルドされると、次の API レベル情報が含まれます。

- アプリを実行するためにビルドされる Android の*ターゲット*API レベル。

- Android デバイスでアプリを実行するために必要な*最低限*の android API レベル。 

これらの設定は、インストール時に Android デバイスでアプリを正しく実行するために必要な機能を使用できるようにするために使用されます。 それ以外の場合、アプリはそのデバイスでの実行がブロックされます。 たとえば、Android デバイスの API レベルが、アプリに指定した最小 API レベルよりも低い場合、Android デバイスはユーザーがアプリをインストールできなくなります。

## <a name="project-api-level-settings"></a>プロジェクト API レベルの設定

以下のセクションでは、SDK Manager を使用して、対象とする API レベルに合わせて開発環境を準備する方法について説明します。その後、*ターゲットフレームワーク*、*最小 Android バージョン*、および*の構成方法について詳しく説明します。* Android のバージョン設定を Xamarin android で対象にします。

### <a name="android-sdk-platforms"></a>Android SDK プラットフォーム

Xamarin. Android でターゲットまたは最小 API レベルを選択するには、その API レベルに対応する Android SDK プラットフォームのバージョンをインストールする必要があります。 ターゲットフレームワーク、Android の最小バージョン、およびターゲット Android のバージョンに使用できる選択肢の範囲は、インストールされている Android SDK バージョンの範囲に限定されます。 SDK Manager を使用して、必要な Android SDK バージョンがインストールされていることを確認できます。また、このマネージャーを使用して、アプリに必要な新しい API レベルを追加できます。 API レベルのインストール方法に慣れていない場合は、「 [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)」を参照してください。

<a name="framework" />

### <a name="target-framework"></a>[対象とする Framework]

*ターゲットフレームワーク*(と`compileSdkVersion`も呼ばれます) は、ビルド時にアプリがコンパイルされる特定の Android Framework のバージョン (API レベル) です。 この設定では、アプリの実行*時に使用する api*を指定しますが、アプリのインストール時にアプリで実際に使用可能な api には影響しません。 結果として、[ターゲットフレームワーク] の設定を変更しても、実行時の動作は変わりません。

ターゲットフレームワークによって、アプリケーションがリンクしている&ndash;ライブラリのバージョンが特定されます。この設定によって、アプリで使用できる api が決まります。 たとえば、Android 5.0 のロリポップで導入された[Notificationbuilder. SetCategory](xref:Android.App.Notification.Builder.SetCategory*)メソッドを使用する場合は、ターゲットフレームワークを**API レベル 21 (ロリポップ)** 以降に設定する必要があります。 プロジェクトのターゲットフレームワークを api レベル**19 (KitKat)** などの api レベルに設定して、コード内で`SetCategory`メソッドを呼び出そうとすると、コンパイルエラーが発生します。

常に、利用可能な*最新*のターゲットフレームワークのバージョンでコンパイルすることをお勧めします。 これにより、コードから呼び出される可能性のある非推奨の Api について、有用な警告メッセージが表示されます。 最新のターゲットフレームワークのバージョンを使用することは、最新のサポートライブラリを&ndash;使用する場合に特に重要です。各ライブラリのリリースでは、そのサポートライブラリの最小 API レベルでアプリをコンパイルすることを想定しています。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio でターゲットフレームワークの設定にアクセスするには、**ソリューションエクスプローラー**でプロジェクトのプロパティを開き、 **[アプリケーション]** ページを選択します。

[![プロジェクトのプロパティの [アプリケーション] ページ](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

前に示したように、 **[Android バージョンを使用してコンパイルする]** の下にあるドロップダウンメニューで API レベルを選択して、ターゲットフレームワークを設定します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac でターゲットフレームワークの設定にアクセスするには、プロジェクト名を右クリックし、 **[オプション]** を選択します。これにより、 **[プロジェクトオプション]** ダイアログボックスが開きます。 このダイアログでは、次に示すように、 **[ビルド > 全般**] に移動します。

[![[プロジェクトオプション] ページの [ビルドの全般] セクション](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

上に示すように、 **[ターゲットフレームワーク]** の右側にあるドロップダウンメニューで API レベルを選択して、ターゲットフレームワークを設定します。

-----

<a name="minimum" />

### <a name="minimum-android-version"></a>Android の最小バージョン

*Android の最小バージョン*(と`minSdkVersion`も呼ばれます) は、アプリケーションをインストールして実行できる最も古いバージョンの android OS (つまり、最も低い API レベル) です。 既定では、アプリはターゲットフレームワーク設定以上のデバイスにのみインストールできます。[Android の最小バージョン] 設定が [ターゲットフレームワーク] 設定よりも*低い*場合、アプリは以前のバージョンの android でも実行できます。 たとえば、ターゲットフレームワークを**android 7.1 (Nougat)** に設定し、Android の最小バージョンを**Android 4.0.3 (アイスクリームサンドイッチ)** に設定した場合、アプリは API レベル15から api レベル25までの任意のプラットフォームにインストールできます。

このようなプラットフォームでは、アプリが正常にビルドされてインストールされる場合がありますが、これらのプラットフォームで正常に*動作*することは保証されません。 たとえば、アプリが**android 5.0 (ロリポップ)** にインストールされていて、コードが**Android 7.1 (Nougat)** 以降でのみ使用可能な API を呼び出す場合、アプリには実行時エラーが発生し、クラッシュする可能性があります。 そのため、コードでは&ndash; 、実行&ndash;中の Android デバイスでサポートされている api のみを呼び出すように、実行時に確認する必要があります。 つまり、アプリが最新の Api を使用していることを確認するために、コードには明示的なランタイムチェックを含める必要があります。
[Android バージョンのランタイムチェック](#runtimechecks)。このガイドの後半では、これらのランタイムチェックをコードに追加する方法について説明します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で [Android の最小バージョン] 設定にアクセスするには、**ソリューションエクスプローラー**でプロジェクトのプロパティを開き、 **[android マニフェスト]** ページを選択します。 **[Android の最小バージョン]** の下のドロップダウンメニューで、アプリケーションの最小 android バージョンを選択できます。

[![SDK バージョンを使用してコンパイルするための最小 Android ターゲットオプションセット](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

[ **SDK バージョンを使用してコンパイルを使用**する] を選択した場合、Android の最小バージョンはターゲットフレームワーク設定と同じになります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac の最小 Android バージョンにアクセスするには、プロジェクト名を右クリックし、 **[オプション]** を選択します。これにより、 **[プロジェクトオプション]** ダイアログボックスが開きます。 **[Build > Android アプリケーション]** に移動します。
**[最小 android バージョン]** の右側にあるドロップダウンメニューを使用して、アプリケーションの最小 android バージョンを設定できます。

[![Android の最小バージョンが自動に設定される-ターゲットフレームワークのバージョンを使用する](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

[**ターゲットフレームワークの&ndash;バージョンを自動で使用**する] を選択した場合、Android の最小バージョンはターゲットフレームワークの設定と同じになります。

-----

<a name="target" />

### <a name="target-android-version"></a>ターゲット Android バージョン

*ターゲット android バージョン*(と`targetSdkVersion`も呼ばれます) は、アプリの実行を想定している android デバイスの API レベルです。 Android では、この設定を使用して、互換性&ndash;の動作を有効にするかどうかを決定します。これにより、アプリが期待どおりに動作することが保証されます。 Android では、アプリの [ターゲットの Android バージョン] 設定を使用して、アプリを中断せずに適用できる動作の変更を判別します (これは Android が上位互換性を提供する方法です)。

ターゲットフレームワークとターゲット Android バージョンは、非常に類似した名前を持っていますが、同じものではありません。 ターゲットフレームワークの設定では、*コンパイル時*に使用するターゲット api レベル情報を Xamarin. android に伝えます。ターゲットの android バージョンでは、*実行時*にターゲット Api レベル情報を android に通信します (アプリがデバイスにインストールされ、実行されている)。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio でこの設定にアクセスするには、**ソリューションエクスプローラー**でプロジェクトのプロパティを開き、 **[Android マニフェスト]** ページを選択します。 **[ターゲット android バージョン]** の下のドロップダウンメニューで、アプリケーションに対してターゲットとなる android バージョンを選択できます。

[![SDK バージョンを使用してコンパイルするように設定されるターゲット Android バージョン](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

対象の Android バージョンは、アプリのテストに使用する最新バージョンの Android に明示的に設定することをお勧めします。 理想的には、最新の Android SDK バージョン&ndash;に設定する必要があります。これにより、動作変更を処理する前に新しい api を使用できるようになります。 ほとんどの開発者は、 **SDK バージョンを使用**したコンパイルを使用するように対象の Android バージョンを設定することはお勧めし*ません*。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac でこの設定にアクセスするには、プロジェクト名を右クリックし、 **[オプション]** を選択します。これにより、 **[プロジェクトオプション]** ダイアログボックスが開きます。 **[Build > Android アプリケーション]** に移動します。 **ターゲット android バージョン**の右側にあるドロップダウンメニューを使用して、アプリケーションのターゲット android バージョンを設定できます。

[![ターゲット Android バージョンが自動に設定される-ターゲットフレームワークのバージョンを使用する](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

対象の Android バージョンは、アプリのテストに使用する最新バージョンの Android に明示的に設定することをお勧めします。 理想的には、使用可能な最新の Android SDK バージョン&ndash;に設定する必要があります。これにより、動作変更を処理する前に新しい api を使用できるようになります。 ほとんどの開発者は、ターゲットの Android バージョンを**自動使用のターゲットフレームワークのバージョン**に設定することはお勧めしません。

-----

一般に、ターゲット Android のバージョンは、Android の最小バージョンとターゲットフレームワークによって制限されている必要があります。 つまり、

**Android の最小バージョン < = ターゲット Android バージョン < = ターゲットフレームワーク**

SDK レベルの詳細については、Android Developer [uses-SDK](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html)のドキュメントを参照してください。

<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Android バージョンのランタイムチェック

Android の新しいバージョンがリリースされるたびに、フレームワーク API が更新され、新機能または代替機能が提供されます。 いくつかの例外を除き、以前の Android バージョンの API 機能は、変更せずに新しい Android バージョンに引き継がれます。 その結果、アプリが特定の Android API レベルで実行されている場合は、通常、変更を加えなくても後で Android API レベルで実行できるようになります。 しかし、以前のバージョンの Android でもアプリを実行する場合はどうでしょうか。

ターゲットフレームワークの設定よりも*低い*Android の最小バージョンを選択した場合、実行時にアプリで使用できない api もあります。 ただし、アプリは以前のデバイスでも実行できますが、機能が制限されています。 Android の最小バージョン設定に対応する android プラットフォームで使用できない api ごとに、コードで`Android.OS.Build.VERSION.SdkInt`プロパティの値を明示的に確認して、アプリが実行されているプラットフォームの api レベルを確認する必要があります。 Api レベルが、呼び出す API をサポートしている最小 Android バージョンより*低い*場合、コードは、この api 呼び出しを行わずに正常に機能する方法を見つける必要があります。

たとえば、 [Notificationbuilder. SetCategory](xref:Android.App.Notification.Builder.SetCategory*)メソッドを使用して、 **android 5.0 ロリポップ**(以降) で実行しているときに通知を分類するとしますが、それでもアプリを以前のバージョンの Android で実行する必要があるとし**ましょう。Android 4.1 Jelly Bean** ( `SetCategory`は使用できません)。 このガイドの最初に説明した Android バージョンの表を参照すると、 **android 5.0 ロリポップ**のビルドバージョンコードが`Android.OS.BuildVersionCodes.Lollipop`であることがわかります。 以前のバージョンの Android `SetCategory`をサポートするために、が使用できない場合、コードは実行時に api レベルを検出し、api レベルがロリポップビルドバージョンコード以上の場合にのみ条件付きで呼び出す`SetCategory`ことができます。

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

この例では、アプリのターゲットフレームワークは**android 5.0 (Api レベル 21)** に設定されており、Android の最小バージョンは**ANDROID 4.1 (api レベル 16)** に設定されています。 は`SetCategory` api レベル`Android.OS.BuildVersionCodes.Lollipop`以降で使用できるため、このコード例では`SetCategory` 、実際に使用可能&ndash;な場合にのみが呼び出され`SetCategory`ます。 api レベルが16、17、18、19、または20の場合は、の呼び出しは試行され*ません*。 この機能は、以前のバージョンの Android では、通知が (種類別に分類されないため) 適切に並べ替えられない程度に限定されていますが、通知は引き続き公開され、ユーザーに警告されます。 アプリは引き続き機能しますが、その機能は若干低下します。

一般に、ビルドバージョンチェックによって、コードは、新しい方法と古い方法のどちらを実行するかを実行時に決定できます。 次に例を示します。

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

1つ以上の Api が不足している古い Android バージョンで実行されている場合に、アプリの機能を削減または変更する方法を説明する迅速で簡単なルールはありません。 場合によっては (上記の`SetCategory`例のように)、API 呼び出しを使用できないときに省略するだけで十分です。 ただし、アプリが最適なエクスペリエンスを提供するために必要な API `Android.OS.Build.VERSION.SdkInt`レベルよりも少ないことが検出された場合は、の代替機能を実装する必要がある場合があります。

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>API レベルとライブラリ

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin android ライブラリプロジェクト (クラスライブラリやバインドライブラリなど) を作成する場合は、[ターゲットフレームワーク] の [最小 Android バージョン] を&ndash;設定し、[ターゲット android バージョン] 設定は使用できません。 これは、 **Android マニフェスト**ページがないためです。

[![[Android バージョンを使用してコンパイルする] オプションのみ使用できます。](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xamarin android ライブラリプロジェクトを作成するときに、android**アプリケーション**ページがありません。このページでは、android の最小バージョンとターゲット&ndash;の android バージョンを構成できます。このページでは、android の最小バージョンとターゲット android バージョンを設定できます。使用できません。
これは、 **Android アプリケーションページ > ビルド**がないためです。

[![最小およびターゲットバージョンのオプションなしのビルド全般ページ](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Android の最小バージョンとターゲットの android バージョンの設定は使用できません。作成されたライブラリが&ndash;スタンドアロンアプリではないため、ライブラリをパッケージ化するアプリによっては、すべての android バージョンでライブラリを実行できます。 ライブラリをどのように*コンパイル*するかを指定できますが、ライブラリが実行されるプラットフォーム API レベルを予測することはできません。 この点を考慮して、ライブラリを使用または作成する際には、次のベストプラクティスに従う必要があります。

- **Android ライブラリを使用する場合**アプリケーションで Android ライブラリを使用している場合は、アプリの [ターゲットフレームワーク] 設定を、ライブラリのターゲットフレームワーク設定*と少なくとも高い*API レベルに設定してください。 &ndash;

- **Android ライブラリを作成する場合**&ndash;他のアプリケーションで使用するための Android ライブラリを作成する場合は、そのターゲットフレームワークの設定をコンパイルするために必要な最小限の API レベルに設定してください。

これらのベストプラクティスは、ライブラリが実行時に使用できない API (アプリがクラッシュする可能性があります) を呼び出そうとする状況を防ぐために推奨されます。 ライブラリ開発者は、api 呼び出しの使用状況を、API の合計領域の小規模で適切に確立されたサブセットに制限することをお勧めします。 これにより、さまざまな Android バージョンでライブラリを安全に使用できるようになります。

## <a name="summary"></a>まとめ

このガイドでは、android API レベルを使用して異なるバージョンの Android 間でアプリの互換性を管理する方法について説明しました。 Xamarin Android の*ターゲットフレームワーク*、android の*最小バージョン*、および*ターゲットの android バージョン*のプロジェクト設定を構成するための詳細な手順が提供されました。 Android SDK マネージャーを使用して SDK パッケージをインストールする手順、実行時にさまざまな API レベルを処理するコードを記述する方法の例、Android ライブラリを作成または使用するときに API レベルを管理する方法などについて説明しました。 また、API レベルを Android のバージョン番号 (Android 4.4 など)、Android のバージョン名 (Kitkat など)、Xamarin Android のビルドバージョンコードに関連付ける包括的な一覧も提供しました。

## <a name="related-links"></a>関連リンク

- [Android SDK セットアップ](~/android/get-started/installation/android-sdk.md)
- [SDK CLI ツールの変更](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Compilesdkversion、minSdkVersion、targetSdkVersion の選択](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [API レベルとは](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames、タグ、およびビルド番号](https://source.android.com/source/build-numbers)
