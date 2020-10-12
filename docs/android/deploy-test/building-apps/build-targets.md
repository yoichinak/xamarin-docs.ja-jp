---
title: ビルド ターゲット
description: このドキュメントでは、Xamarin.Android ビルド プロセスでサポートされているすべてのターゲットを一覧表示します。
ms.prod: xamarin
ms.assetid: 17DE89FF-F316-4620-B865-EF6E0963A29C
ms.technology: xamarin-android
author: jonpryor
ms.author: jopryo
ms.date: 09/17/2020
ms.openlocfilehash: 4482e6c4bfe2a6952d59d896b7c79ca82432b42b
ms.sourcegitcommit: 38496cfd4d30fd40a011011f303a31de639bd699
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2020
ms.locfileid: "91247221"
---
# <a name="build-targets"></a>ビルド ターゲット

Xamarin.Android プロジェクトに対して、次のビルド ターゲットが定義されています。

## <a name="build"></a>Build

プロジェクト内のソース コードとすべての依存関係をビルドします。

このターゲットによって Android パッケージ (`.apk` ファイル) が作成されることは "*ありません*"。
Android パッケージを作成するには、[SignAndroidPackage](#signandroidpackage) ターゲットを使用、"*または*" ビルド時に [`$(AndroidBuildApplicationPackage)](~/android/deploy-test/building-apps/build-properties.md#androidbuildapplicationpackage) プロパティを True に設定します。

```shell
msbuild /p:AndroidBuildApplicationPackage=True App.sln
```

## <a name="buildandstartaotprofiling"></a>BuildAndStartAotProfiling

埋め込みの AOT プロファイラーを使用してアプリをビルドし、プロファイラー TCP ポートを [`$(AndroidAotProfilerPort)`](~/android/deploy-test/building-apps/build-properties.md#androidaotprofilerport) に設定して、既定のアクティビティを開始します。

既定の TCP ポートは `9999` です。

Xamarin.Android 10.2 で追加されました。

## <a name="clean"></a>Clean

ビルド プロセスによって生成されたすべてのファイルを削除します。

## <a name="finishaotprofiling"></a>FinishAotProfiling

[BuildAndStartAotProfiling](#buildandstartaotprofiling) ターゲットの "*後に*" 呼び出される必要があります。

デバイスまたはエミュレーターからの AOT プロファイラー データを TCP ポート経由で収集し[`$(AndroidAotProfilerPort)`](~/android/deploy-test/building-apps/build-properties.md#androidaotprofilerport)、
[`$(AndroidAotCustomProfilePath)`](~/android/deploy-test/building-apps/build-properties.md#androidaotcustomprofilepath) に書き込みます。

ポートおよびカスタム プロファイルの既定値は `9999` と `custom.aprof` です。

追加のオプションを `aprofutil` に渡すには、それらを [`$(AProfUtilExtraOptions)`](~/android/deploy-test/building-apps/build-properties.md#aprofutilextraoptions) プロパティに設定します。
プロパティに基づきます)。

このようにすると、次の記述と同じ結果が得られます。

```shell
aprofutil $(AProfUtilExtraOptions) -s -v -f -p $(AndroidAotProfilerPort) -o "$(AndroidAotCustomProfilePath)"
```

Xamarin.Android 10.2 で追加されました。

## <a name="install"></a>Install

既定のデバイスまたは仮想デバイスに、Android パッケージを[作成、署名](#signandroidpackage)、およびインストールします。

[`$(AdbTarget)`](~/android/deploy-test/building-apps/build-properties.md#adbtarget) プロパティは、Android パッケージのインストール先または削除元となる Android ターゲット デバイスを指定します。

```bash
# Install package onto emulator via -e
# Use `/Library/Frameworks/Mono.framework/Commands/msbuild` on OS X
MSBuild /t:Install ProjectName.csproj /p:AdbTarget=-e
```

## <a name="signandroidpackage"></a>SignAndroidPackage

Android パッケージ (`.apk`) ファイルを作成して署名します。

`/p:Configuration=Release` とともに使用して、自己完結型の "リリース" パッケージを生成します。

## <a name="startandroidactivity"></a>StartAndroidActivity

デバイスまたは実行中のエミュレーターで、既定のアクティビティを開始します。

別のアクティビティを開始するには、[`$(AndroidLaunchActivity)`](~/android/deploy-test/building-apps/build-properties.md#androidlaunchactivity)
プロパティをアクティビティ名に設定します。

このようにすると、次の記述と同じ結果が得られます。

```shell
adb shell am start @PACKAGE_NAME@/$(AndroidLaunchActivity)
```

Xamarin.Android 10.2 で追加されました。

## <a name="stopandroidpackage"></a>StopAndroidPackage

デバイスまたは実行中のエミュレーターで、アプリケーション パッケージを完全に停止します。

このようにすると、次の記述と同じ結果が得られます。

```shell
adb shell am force-stop @PACKAGE_NAME@
```

Xamarin.Android 10.2 で追加されました。

## <a name="uninstall"></a>Uninstall

既定のデバイスまたは仮想デバイスから Android パッケージをアンインストールします。

[`$(AdbTarget)`](~/android/deploy-test/building-apps/build-properties.md#adbtarget) プロパティは、Android パッケージのインストール先または削除元となる Android ターゲット デバイスを指定します。

## <a name="updateandroidresources"></a>UpdateAndroidResources

`Resource.designer.cs` ファイルが更新されます。

このターゲットは通常、新しいリソースがプロジェクトに追加されたときに、IDE によって呼び出されます。
