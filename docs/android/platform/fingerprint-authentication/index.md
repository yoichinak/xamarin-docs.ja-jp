---
title: 指紋認証
description: このガイドでは、Android 6.0 で導入された指紋認証を Xamarin Android アプリケーションに追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: a58242e89033d6cd2652495f9466379f63f498f0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761247"
---
# <a name="fingerprint-authentication"></a>指紋認証

_このガイドでは、Android 6.0 で導入された指紋認証を Xamarin Android アプリケーションに追加する方法について説明します。_

## <a name="fingerprint-authentication-overview"></a>指紋認証の概要

Android デバイスで指紋スキャナーを使用すると、ユーザー認証の従来のユーザー名/パスワード方法の代わりにアプリケーションが提供されます。 指紋を使用してユーザーを認証することにより、アプリケーションは、ユーザー名とパスワードよりも侵入が少ないセキュリティを組み込むことができます。

FingerprintManager Api は、指紋スキャナーを使用してデバイスを対象とし、API レベル 23 (Android 6.0) 以降を実行しています。 Api は`Android.Hardware.Fingerprints`名前空間にあります。 Android サポートライブラリ v4 には、以前のバージョンの Android 用の指紋 Api のバージョンが用意されています。 互換性 api は`Android.Support.v4.Hardware.Fingerprint`名前空間にあります。この api は、 [Xamarin. Android. の NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)を通じて配布されます。

[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (およびサポートライブラリ対応の[FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) は、指紋スキャンハードウェアを使用するための主要なクラスです。 このクラスは、ハードウェア自体との対話を管理するシステムレベルサービスの Android SDK ラッパーです。 指紋スキャナーを起動し、スキャナーからのフィードバックに応答する役割を担います。 このクラスには、次の3つのメンバーのみを含む非常に単純なインターフェイスがあります。

- **`Authenticate`** &ndash;このメソッドは、ユーザーが指紋をスキャンするのを待機して、ハードウェアスキャナーを初期化し、サービスをバックグラウンドで起動します。
- **`EnrolledFingerprints`** ユーザーがデバイスに`true` 1 つ以上の指紋を登録している場合、このプロパティはを返します。 &ndash;
- **`HardwareDetected`** &ndash;このプロパティは、デバイスで指紋スキャンがサポートされているかどうかを判断するために使用されます。

この`FingerprintManager.Authenticate`メソッドは、指紋スキャナーを起動するために Android アプリケーションによって使用されます。 次のスニペットは、サポートライブラリ互換性 Api を使用して呼び出す方法を示しています。

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

このガイドでは、api を使用`FingerprintManager`して、指紋認証で Android アプリケーションを拡張する方法について説明します。 ここでは、指紋スキャナーからの結果`CryptoObject`をセキュリティで保護するために、をインスタンス化して作成する方法について説明します。 ここでは、アプリケーションが指紋スキャナー `FingerprintManager.AuthenticationCallback`からのフィードバックにサブクラス化して応答する方法について説明します。 最後に、Android デバイスまたはエミュレーターに指紋を登録する方法と、 **adb**を使用して指紋スキャンをシミュレートする方法について説明します。

## <a name="requirements"></a>必要条件

指紋認証には、Android 6.0 (API レベル 23) 以上、および指紋スキャナーを使用するデバイスが必要です。 

認証されるユーザーごとに、デバイスに指紋が既に登録されている必要があります。 これには、パスワード、PIN、スワイプパターン、または顔認識を使用する画面ロックの設定が含まれます。 Android Emulator では、一部の指紋認証機能をシミュレートすることができます。  これらの2つのトピックの詳細については、「[指紋の登録](enrolling-fingerprint.md)」セクションを参照してください。 

## <a name="related-links"></a>関連リンク

- [指紋ガイドのサンプルアプリ](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fingerprintguide)
- [フィンガープリントダイアログのサンプル](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [実行時のアクセス許可の要求](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](xref:Android.Content.Context)
- [指紋および支払い API (ビデオ)](https://youtu.be/VOn7VrTRlA4)
