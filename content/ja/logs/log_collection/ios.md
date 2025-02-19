---
dependencies:
- https://github.com/DataDog/dd-sdk-ios/blob/master/docs/log_collection.md
description: iOS アプリケーションからログを収集する。
further_reading:
- link: https://github.com/DataDog/dd-sdk-ios
  tag: Github
  text: dd-sdk-ios ソースコード
- link: logs/explorer
  tag: ドキュメント
  text: ログの調査方法
kind: ドキュメント
title: iOS ログ収集
---
[Datadog の `dd-sdk-ios` クライアント側ロギングライブラリ][1]を使用すると、iOS アプリケーションから Datadog へログを送信すると共に、次の機能を利用できます。

* Datadog に JSON 形式でネイティブに記録する。
* デフォルトを使用し、送信される各ログにカスタム属性を追加する。
* 実際のクライアント IP アドレスとユーザーエージェントを記録する。
* 自動一括ポストによって最適化されたネットワークの利用を活用します。

**注意**: `dd-sdk-ios` ライブラリは iOS 11 以降のすべてのバージョンに対応しています。

## セットアップ

1. パッケージマネージャーに応じてライブラリを依存関係として宣言します。

    {{< tabs >}}
    {{% tab "CocoaPods" %}}

[CocoaPods][6] を使用して、 `dd-sdk-ios`をインストールできます。
```
pod 'DatadogSDK'
```

[6]: https://cocoapods.org/

    {{% /tab %}}
    {{% tab "Swift Package Manager (SPM)" %}}

Apple の Swift Package Manager を使用して統合するには、`Package.swift` に以下を依存関係として追加します。
```swift
.package(url: "https://github.com/Datadog/dd-sdk-ios.git", .upToNextMajor(from: "1.0.0"))
```

    {{% /tab %}}
    {{% tab "Carthage" %}}

[Carthage][7] を使用して、 `dd-sdk-ios`をインストールできます。
```
github "DataDog/dd-sdk-ios"
```

[7]: https://github.com/Carthage/Carthage

    {{% /tab %}}
    {{< /tabs >}}

2. アプリケーションコンテキストと [Datadog クライアントトークン][2]でライブラリを初期化します。セキュリティ上の理由から、クライアントトークンを使用する必要があります。API キーがクライアント側の iOS アプリケーションの IPA バイトコードで公開されてしまうため、[Datadog API キー][3]を使用して `dd-sdk-ios` ライブラリを構成することはできません。クライアントトークンの設定に関する詳細は、[クライアントトークンに関するドキュメント][2]を参照してください。

{{< site-region region="us" >}}
{{< tabs >}}
{{% tab "Swift" %}}
```swift
Datadog.initialize(
    appContext: .init(),
    trackingConsent: trackingConsent,
    configuration: Datadog.Configuration
        .builderUsing(clientToken: "<client_token>", environment: "<environment_name>")
        .set(serviceName: "app-name")
        .set(endpoint: .us1)
        .build()
)
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
DDConfigurationBuilder *builder = [DDConfiguration builderWithClientToken:@"<client_token>"
                                                              environment:@"<environment_name>"];
[builder setWithServiceName:@"app-name"];
[builder setWithEndpoint:[DDEndpoint us1]];

[DDDatadog initializeWithAppContext:[DDAppContext new]
                    trackingConsent:trackingConsent
                      configuration:[builder build]];
```
{{% /tab %}}
{{< /tabs >}}
{{< /site-region >}}

{{< site-region region="eu" >}}
{{< tabs >}}
{{% tab "Swift" %}}

```swift
Datadog.initialize(
    appContext: .init(),
    trackingConsent: trackingConsent,
    configuration: Datadog.Configuration
        .builderUsing(clientToken: "<client_token>", environment: "<environment_name>")
        .set(serviceName: "app-name")
        .set(endpoint: .eu1)
        .build()
)
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
DDConfigurationBuilder *builder = [DDConfiguration builderWithClientToken:@"<client_token>"
                                                              environment:@"<environment_name>"];
[builder setWithServiceName:@"app-name"];
[builder setWithEndpoint:[DDEndpoint eu1]];

[DDDatadog initializeWithAppContext:[DDAppContext new]
                    trackingConsent:trackingConsent
                      configuration:[builder build]];
```
{{% /tab %}}
{{< /tabs >}}
{{< /site-region >}}

{{< site-region region="us3" >}}
{{< tabs >}}
{{% tab "Swift" %}}

```swift
Datadog.initialize(
    appContext: .init(),
    trackingConsent: trackingConsent,
    configuration: Datadog.Configuration
        .builderUsing(clientToken: "<client_token>", environment: "<environment_name>")
        .set(serviceName: "app-name")
        .set(endpoint: .us3)
        .build()
)
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
DDConfigurationBuilder *builder = [DDConfiguration builderWithClientToken:@"<client_token>"
                                                              environment:@"<environment_name>"];
[builder setWithServiceName:@"app-name"];
[builder setWithEndpoint:[DDEndpoint us3]];

[DDDatadog initializeWithAppContext:[DDAppContext new]
                    trackingConsent:trackingConsent
                      configuration:[builder build]];
```
{{% /tab %}}
{{< /tabs >}}
{{< /site-region >}}

{{< site-region region="us5" >}}
{{< tabs >}}
{{% tab "Swift" %}}

```swift
Datadog.initialize(
    appContext: .init(),
    trackingConsent: trackingConsent,
    configuration: Datadog.Configuration
        .builderUsing(clientToken: "<client_token>", environment: "<environment_name>")
        .set(serviceName: "app-name")
        .set(endpoint: .us5)
        .build()
)
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
DDConfigurationBuilder *builder = [DDConfiguration builderWithClientToken:@"<client_token>"
                                                              environment:@"<environment_name>"];
[builder setWithServiceName:@"app-name"];
[builder setWithEndpoint:[DDEndpoint us5]];

[DDDatadog initializeWithAppContext:[DDAppContext new]
                    trackingConsent:trackingConsent
                      configuration:[builder build]];
```
{{% /tab %}}
{{< /tabs >}}
{{< /site-region >}}

{{< site-region region="gov" >}}
{{< tabs >}}
{{% tab "Swift" %}}

```swift
Datadog.initialize(
    appContext: .init(),
    trackingConsent: trackingConsent,
    configuration: Datadog.Configuration
        .builderUsing(clientToken: "<client_token>", environment: "<environment_name>")
        .set(serviceName: "app-name")
        .set(endpoint: .us1_fed)
        .build()
)
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
DDConfigurationBuilder *builder = [DDConfiguration builderWithClientToken:@"<client_token>"
                                                              environment:@"<environment_name>"];
[builder setWithServiceName:@"app-name"];
[builder setWithEndpoint:[DDEndpoint us1_fed]];

[DDDatadog initializeWithAppContext:[DDAppContext new]
                    trackingConsent:trackingConsent
                      configuration:[builder build]];
```
{{% /tab %}}
{{< /tabs >}}
{{< /site-region >}}


GDPR 規定を遵守するため、SDK は初期化時に `trackingConsent` の値を求めます。
`trackingConsent` は以下のいずれかの値になります。

- `.pending` - the SDK はデータの収集とバッチ処理を開始しますが、Datadog へは送信しません。SDK はバッチ処理が完了したデータをどうするかについての新たな同意値が得られるまで待機します。
- `.granted` - SDK はデータの収集を開始し、Datadog へ送信します。
- `.notGranted` - SDK はデータを収集しません。ログ、トレース、RUM イベントは Datadog に送信されません。 

SDK の初期化後に追跡同意値を変更するには、`Datadog.set(trackingConsent:)` API 呼び出しを使用します。
SDK は、新しい値に応じて動作を変更します。たとえば、現在の追跡同意が `.pending` の場合: 

- `.granted` に変更すると、SDK は現在および今後のすべてのデータを Datadog に送信します。
- `.notGranted` に変更すると、SDK は現在のすべてのデータを消去し、今後のデータを収集しません。

データは Datadog にアップロードされる前に、[アプリケーションサンドボックス][6]のキャッシュディレクトリ (`Library/Caches`) に平文で保存され、デバイスにインストールされた他のアプリからは読み取ることができません。

アプリケーションを作成する際、開発ログを有効にし、提供されたレベルと同等以上の優先度を持つ SDK のすべての内部メッセージをコンソールにログ出力するようにしてください。

{{< tabs >}}
{{% tab "Swift" %}}
```swift
Datadog.verbosityLevel = .debug
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
DDDatadog.verbosityLevel = DDSDKVerbosityLevelDebug;
```
{{% /tab %}}
{{< /tabs >}}

3. `Logger` の構成：

{{< tabs >}}
{{% tab "Swift" %}}
```swift
let logger = Logger.builder
    .sendNetworkInfo(true)
    .printLogsToConsole(true, usingFormat: .shortWith(prefix: "[iOS App] "))
    .set(datadogReportingThreshold: .info)
    .build()
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
DDLoggerBuilder *builder = [DDLogger builder];
[builder sendNetworkInfo:YES];
[builder setWithDatadogReportingThreshold:.info];
[builder printLogsToConsole:YES];

DDLogger *logger = [builder build];
```
{{% /tab %}}
{{< /tabs >}}

4. 次のいずれかのメソッドで、カスタムログエントリを Datadog に直接送信します。

{{< tabs >}}
{{% tab "Swift" %}}
```swift
logger.debug("A debug message.")
logger.info("Some relevant information?")
logger.notice("Have you noticed?")
logger.warn("An important warning...")
logger.error("An error was met!")
logger.critical("Something critical happened!")
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
[logger debug:@"A debug message."];
[logger info:@"Some relevant information?"];
[logger notice:@"Have you noticed?"];
[logger warn:@"An important warning..."];
[logger error:@"An error was met!"];
[logger critical:@"Something critical happened!"];
```
{{% /tab %}}
{{< /tabs >}}

5. (任意) - ログメッセージと一緒に `attributes` のマップを提供し、発行されたログに属性を追加します。マップの各エントリーは属性として追加されます。

{{< tabs >}}
{{% tab "Swift" %}}
```swift
logger.info("Clicked OK", attributes: ["context": "onboarding flow"])
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
[logger info:@"Clicked OK" attributes:@{@"context": @"onboarding flow"}];
```
{{% /tab %}}
{{< /tabs >}}

## 高度なロギング

### 初期化

ログを Datadog に送信するようにロガーを初期化する際に、`Logger.Builder` の次のメソッドを使用できます。

| メソッド                           | 説明                                                                                                                                                                                                                         |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `sendNetworkInfo(true)`    | すべてのログに `network.client.*` 属性を追加します。デフォルトで記録されるデータには、`reachability` (`yes`、`no`、`maybe`)、`available_interfaces` (`wifi`、`cellular` ...)、`sim_carrier.name` (例、`AT&T - US`)、`sim_carrier.technology` (`3G`、`LTE` ...)、`sim_carrier.iso_country` (例、`US`)があります。 |
| `set(serviceName: "<サービス名>")` | Datadog に送信されるすべてのログに添付される `service` [標準属性][4] の値として `<サービス名>` を設定します。                                                                                                                        |
| `printLogsToConsole(true)`     | デバッガコンソールにログを送信するには、`true` とします。                                                                                                                                                                                         |
| `sendLogsToDatadog(true)`    | Datadog にログを送信するには、`true` とします。                                                                                                                                                                                              |
| `set(loggerName: "<ロガー名>")`   | Datadog に送信されるすべてのログに添付される `logger.name` 標準属性の値として `<ロガー名>` を設定します。                                                                                                                                   |
| `build()`                        | すべてのオプションを設定して新しいロガーインスタンスをビルドします。                                                                                                                                                                                   |

### グローバルコンフィギュレーション

指定されたロガーから送信されるすべてのログにタグと属性を追加/削除するメソッドを以下に記します。

#### グローバルタグ

##### タグを追加

`addTag(withKey:value:)` メソッドを使い、指定されたロガーから送信されるすべてのログにタグを追加します。

{{< tabs >}}
{{% tab "Swift" %}}
```swift
// これにより、"build_configuration:debug" タグが追加されます
logger.addTag(withKey: "build_configuration", value: "debug")
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
[logger addTagWithKey:@"build_configuration" value:@"debug"];
```
{{% /tab %}}
{{< /tabs >}}

**注意**: `<タグの値>` は `文字列` でなければなりません。

##### タグを削除

`removeTag(withKey:)` メソッドを使い、指定されたロガーから送信されるすべてのログからタグを削除します。

{{< tabs >}}
{{% tab "Swift" %}}
```swift
// これにより "build_configuration" で始まるすべてのタグが削除されます
logger.removeTag(withKey: "build_configuration")
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
[logger removeTagWithKey:@"build_configuration"];
```
{{% /tab %}}
{{< /tabs >}}

[Datadog タグに関する詳細][5]。

#### グローバル属性

##### 属性を追加

デフォルトで、ロガーにより送信されるすべてのログに次の属性が追加されます。

* `http.useragent` と抽出された `device` と `OS` プロパティ
* `network.client.ip` と抽出された地理的プロパティ (`country`, `city`)
* `logger.version`、Datadog SDK バージョン
* `logger.thread_name`, (`main`, `background`)
* `version`、`Info.plist` から抽出されたクライアントのアプリバージョン
* `environment`、SDK の初期化に使われる環境名

`addAttribute(forKey:value:)` メソッドを使い、指定されたロガーから送信されるすべてのログにカスタム属性を追加します。

{{< tabs >}}
{{% tab "Swift" %}}
```swift
// これにより、文字列値を持つ "device-model" 属性が追加されます
logger.addAttribute(forKey: "device-model", value: UIDevice.current.model)
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
[logger addAttributeForKey:@"device-model" value:UIDevice.currentDevice.model];
```
{{% /tab %}}
{{< /tabs >}}

**注**: `<属性の値>` は `Encodable` (`文字列`、`日付`、カスタム `Codable` データモデルなど) に適合する限り任意です。

##### 属性を削除

`removeAttribute(forKey:)` メソッドを使い、指定されたロガーから送信されるすべてのログからカスタム属性を削除します。

{{< tabs >}}
{{% tab "Swift" %}}
```swift
// これにより、"device-model" 属性は今後送信されるすべてのログから削除されます。
logger.removeAttribute(forKey: "device-model")
```
{{% /tab %}}
{{% tab "Objective-C" %}}
```objective-c
[logger removeAttributeForKey:@"device-model"];
```
{{% /tab %}}
{{< /tabs >}}

## その他の参考資料

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://github.com/DataDog/dd-sdk-ios
[2]: https://docs.datadoghq.com/ja/account_management/api-app-keys/#client-tokens
[3]: https://docs.datadoghq.com/ja/account_management/api-app-keys/#api-keys
[4]: https://docs.datadoghq.com/ja/logs/processing/attributes_naming_convention/
[5]: https://docs.datadoghq.com/ja/tagging/
[6]: https://support.apple.com/guide/security/security-of-runtime-process-sec15bfe098e/web