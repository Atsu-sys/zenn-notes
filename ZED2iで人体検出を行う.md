# ZED2iで人体検出を行う
StereoLabのZED2iを触る機会があったので、その備忘録。
# 機材

・ZED2i

https://www.stereolabs.com/products/zed-2

・サンワダイレクト USB延長ケーブル 10m USB 3.2/3.1 Gen1 アクティブ ACアダプタ付 500-USB068

https://www.amazon.co.jp/%E3%82%B5%E3%83%B3%E3%83%AF%E3%83%80%E3%82%A4%E3%83%AC%E3%82%AF%E3%83%88-USB%E5%BB%B6%E9%95%B7%E3%82%B1%E3%83%BC%E3%83%96%E3%83%AB-%E3%82%A2%E3%82%AF%E3%83%86%E3%82%A3%E3%83%96-AC%E3%82%A2%E3%83%80%E3%83%97%E3%82%BF%E4%BB%98-500-USB068/dp/B08DFF7NDF/ref=sr_1_9?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=2HDBQ3SNXJ1UF&dib=eyJ2IjoiMSJ9.RmbtGYlXI6kGhfzhT9UkbX_oCHVJK4N-acg7_6xevZcIuA2ya8guqB8xJ9UA_b7MJ4GtdpZ2iaGoXBWqQNtrHcCk8HP78AeIRAkjUFudP0nz_ZD1hbHRqjBuI4fUKLadyX1A-Sn0LiHQvin7qFq6fioDN7OiyknYH3-5uc3lwxtwqnCc0_PQ6dBYSeFA9eahlHCwO0R265AaBcFsyKmSK3KarsE_xh9lxPnGdopVOc8_MhbbXwrcGTxuUWRoJ41DUAiEJbLsfAFtNP7vvqI6E1N62Ma4b2CDTlVq-yvUHwg.BNSsrVhf7iLpAJ1zdwyC1dtGDZ_bTXM5YfnneByic64&dib_tag=se&keywords=%E3%82%B5%E3%83%B3%E3%83%AF%E3%82%B5%E3%83%97%E3%83%A9%E3%82%A4+usb+%E3%83%AA%E3%83%94%E3%83%BC%E3%82%BF%E3%83%BC+10m&qid=1708570184&sprefix=%E3%82%B5%E3%83%B3%E3%83%AF%E3%82%B5%E3%83%97%E3%83%A9%E3%82%A4+usb+%E3%83%AA%E3%83%94%E3%83%BC%E3%82%BF%E3%83%BC+10m%2Caps%2C170&sr=8-9

# ZED2iのスペック
・ニューラルデプスセンシング
・空間オブジェクト検出
・次世代IMU、ジャイロスコープ、気圧計、磁力計内蔵
・**120°の広角FOV**　
・サーマルコントロール付きオールアルミニウムフレーム 
・反射除去用内蔵ポラライザー（オプション
・IP66の防水・防塵性能
・安全なUSB Type-C接続（1.5mのロックケーブル付属）

※公式サイト参照

完全にKinectの上位互換な上、Unity用のプラグインが用意されています。
すごい。

# 開発環境
・Windows11
・Unity 2022.3.18f1

# ZED2iを選定した理由
案件でふわふわ遊具にいる人（複数人）をセンシングする必要がありました。
最初は側域センサを使用する想定でいましたが設置条件が合わず断念。
たまたまZEDを知ったため、試しに使ってみたところ精度もよく距離や画角も丁度良かったのでこちらのセンサを使用することにしました。

# 人体検出を行う
基本的にはこちらのUnity用プラグインのreadmeにそって必要な手順を踏めばOKです。

ZED を使用して Unity でアプリケーションを開発するには、次のものが必要です。

・[ZED SDK](https://github.com/stereolabs/zed-unity?tab=readme-ov-file)をダウンロードしてインストールする。
・Unity 用の [ZED プラグイン](https://github.com/stereolabs/zed-unity/releases)のUnitypackageをgithubからダウンロードし、UnityProjectにインストールする。

:::note warn
CUDAのインストールもする必要があります。
これを忘れると動かないので注意。
:::

# プラグインをUnityにインストールする
まずUnityにプラグインをインストールしてみると以下のエラーがでる。
```error
Assets\ZED\SDK\Helpers\Scripts\Utilities\ZEDSupportFunctions.cs(5,22): error CS0234: The type or namespace name 'Management' does not exist in the namespace 'UnityEngine.XR' (are you missing an assembly reference?)
```

```
Shader error in 'Custom/Green Screen/Green Screen URP': Couldn't open include file 'Packages/com.unity.render-pipelines.universal/ShaderLibrary/Core.hlsl'. at line 49
```
とりあえずXRpluginManagementとURPをインストールすると無事エラー解消し、ZEDのポップアップが表示されるのでAccseptAllをクリック。![スクリーンショット 2024-02-22 120131.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2524352/53b5794f-0dd5-66a7-66a4-e149266b2072.png)
これでセッティング完了。
# サンプルを触ってみる
3DObjectDetectionシーンを開いてみる。
動いた。

![実験動画.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2524352/2196840e-7e84-25ea-471d-7ec264d3b35e.gif)

:::note warn
機械学習モデルのインストールにかなり時間がかかるようなので注意。
![2024-02-02_182952.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2524352/6075dff4-8174-b992-cca6-5fefcce910f7.png)
:::

# まとめ
キネクトの上位互換どころかボディトラッキングの精度によっては測域センサの代わりにもなりそうです。
また、ZED2iが自己位置を出してくれる（床からの高さや回転など）ためキャリブレーションが非常にやりやすく現場で調整もやる身としてはありがたいです。
引き続き他のサンプルも検証していきたいです。
