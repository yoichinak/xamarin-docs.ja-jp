---
title: 指紋認証
description: このガイドでは、Android 6.0 で導入された指紋認証を Xamarin.Android アプリケーションに追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 4a4b6ee7a123683a9d5a140c46c0b3542767ffa3
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027521"
---
# <a name="fingerprint-authentication"></a>指紋認証

"_このガイドでは、Android 6.0 で導入された指紋認証を Xamarin Android アプリケーションに追加する方法について説明します。_ "

## <a name="fingerprint-authentication-overview"></a>指紋認証の概要

Android デバイス上での指紋スキャナーの登場により、ユーザー認証における従来のユーザー名とパスワードの方法の代替手段がアプリケーションに提供されます。 指紋を使用してユーザーを認証することにより、ユーザー名とパスワードよりも侵入の少ないセキュリティをアプリケーションに組み込むことができます。

FingerprintManager API は、指紋スキャナーを備えたデバイスを対象とし、API レベル 23 (Android 6.0) 以降を実行しています。 API は `Android.Hardware.Fingerprints` 名前空間にあります。 Android サポート ライブラリ v4 では、以前のバージョンの Android 用の指紋 API のバージョンが提供されます。 互換性 API は `Android.Support.v4.Hardware.Fingerprint` 名前空間にあり、[Xamarin.Android.Support.v4 NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)を通じて配布されます。

[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (およびそのサポート ライブラリに対応する [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) は、指紋スキャン ハードウェアを使用するためのプライマリ クラスです。 このクラスは、ハードウェア自体とのやりとりを管理するシステム レベル サービスの Android SDK ラッパーです。 これは、指紋スキャナーを起動し、スキャナーからのフィードバックに応答する役割を担います。 このクラスには、非常にわかりやすいインターフェイスが備わっており、次の 3 つのメンバーのみが含まれます。

- **`Authenticate`** &ndash; このメソッドは、ユーザーが指紋をスキャンするのを待機して、ハードウェア スキャナーを初期化し、サービスをバックグラウンドで起動します。
- **`EnrolledFingerprints`** &ndash; ユーザーがデバイスに 1 つまたは複数の指紋を登録している場合、このプロパティは `true` を返します。
- **`HardwareDetected`** &ndash; このプロパティは、デバイスで指紋スキャンがサポートされているかどうかを判断するために使用されます。

`FingerprintManager.Authenticate` メソッドは、指紋スキャナーを起動するために、Android アプリケーションによって使用されます。 次のスニペットは、サポート ライブラリ互換性 API を使用して呼び出す方法の例を示しています。

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

このガイドでは、`FingerprintManager` API を使用して、指紋認証で Android アプリケーションを拡張する方法について説明します。 ここでは、指紋スキャナーからの結果をセキュリティで保護するために、`CryptoObject` をインスタンス化して作成する方法について説明します。 ここでは、アプリケーションで `FingerprintManager.AuthenticationCallback` をサブクラス化して、指紋スキャナーからのフィードバックに応答する方法について確認します。 最後に、Android デバイスまたはエミュレーターに指紋を登録する方法と、**adb** を使用して指紋スキャンをシミュレートする方法について確認します。

## <a name="requirements"></a>必要条件

指紋認証には、Android 6.0 (API レベル 23) 以上および指紋スキャナーを使用するデバイスが必要です。 

認証されるユーザーごとに、デバイスに指紋が既に登録されている必要があります。 これは、パスワード、PIN、スワイプ パターン、または顔認識を使用する画面ロックの設定に含まれます。 Android Emulator では、一部の指紋認証機能をシミュレートすることができます。  これらの 2 つのトピックの詳細については、「[指紋の登録](enrolling-fingerprint.md)」セクションを参照してください。 

## <a name="related-links"></a>関連リンク

- [指紋ガイド サンプル アプリ](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fingerprintguide)
- [指紋ダイアログ サンプル](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [実行時に権限をリクエストする](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](xref:Android.Content.Context)
- [指紋および支払い API (ビデオ)](https://youtu.be/VOn7VrTRlA4)
