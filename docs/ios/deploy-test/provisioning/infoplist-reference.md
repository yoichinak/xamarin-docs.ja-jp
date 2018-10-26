---
title: Xamarin.iOS の Info.plist リファレンス
description: このドキュメントでは、Xamarin.iOS アプリケーションの Info.plist ファイルで設定できるさまざまなキーと値のペアについて説明します。 これらのキーは、アプリで場所、写真、マイク、カメラへのアクセスなど、特定のタスクを実行する場合に必要です。
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/18/2017
ms.openlocfilehash: 654eca1098f9486e0c41fd296b3f8d381ac7ea34
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105377"
---
# <a name="infoplist-reference-for-xamarinios"></a>Xamarin.iOS の Info.plist リファレンス

Info.Plist キーの使用の詳細については、「[Working with Security and Privacy](~/ios/app-fundamentals/security-privacy.md)」 (セキュリティとプライバシーの使用) ガイドを参照してください。 

## <a name="location"></a>場所 

ユーザーの場所にアクセスするには、Info.plist への変更も必要になります。 位置データに関連する次のキーを設定する必要があります。 

* **NSLocationWhenInUseUsageDescription**: アプリでやり取りしている間にユーザーの場所にアクセスする場合。 
* **NSLocationAlwaysUsageDescription**: アプリがバック グラウンドでユーザーの場所にアクセスする場合。

## <a name="photos"></a>写真 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>連絡先 

NSContactsUsageDescription 

## <a name="calendar-data"></a>予定表データ 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>リマインダー データ 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Bluetooth 周辺機器 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>マイク 

NSMicrophoneUsageDescription 

## <a name="camera"></a>カメラ 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>HealthKit の読み取り  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>バックグラウンド モード 
    
UIBackgroundModes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 


## <a name="related-links"></a>関連リンク

- [Apple リファレンス ガイド。](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
