【概要】
このプロジェクトでは、SVR（サポートベクターマシン回帰）モデルを使用して自動車の燃費（MPG）を予測するモデルを構築し、
そのモデルをFastAPI経由で使用してリアルタイムで燃費を推定できるAPIを実装しています。

【プロジェクトの構成】
学習フェーズ: CSVデータを用いてSVRモデルを訓練し、特徴量の重要度を確認。
推論フェーズ: 学習済みモデルを使用して、新しいデータから自動車の燃費を予測。
APIフェーズ: FastAPIを用いた推論APIを作成。

【解説】
・データの前処理と学習
1.データの読み込み
　トレーニングデータとテストデータをCSVファイルから読み込み、燃費（MPG）予測のために不要なカラムを削除しています。さらに、外れ値の削除やデータの標準化も行います。

2.不要なカラムの削除: car nameやhorsepowerなど、モデルには使用しないカラムを削除。
　外れ値の削除: 燃費や排気量に極端な値が含まれるデータを除去。

3.標準化: すべての入力変数を標準化して、特徴量のスケールを揃えています。

4.モデルの学習
　SVR（サポートベクターマシン回帰）モデルを使って、自動車の燃費を予測するモデルを学習しています。SVRの主要なハイパーパラメータとして、C=22, epsilon=1.6, kernel='rbf'を指定。
 
5.RMSEの計算: モデルの評価にはRMSE（Root Mean Squared Error）を使用し、クロスバリデーションによるモデルの精度評価を行います。

6.特徴量の重要度: パーミュテーション重要度を使って、各特徴量が燃費予測にどの程度影響を与えているかを確認します。

7.学習済みモデルの保存
　学習が完了したモデルは、後の推論に備えてpickleを用いてファイルとして保存します。

8.推論APIの構築
　FastAPIを用いて、学習済みモデルを活用した推論APIを構築しています。
　このAPIにより、外部から自動車の特徴量（シリンダー数、排気量、重量、モデル年）を入力し、それに基づいて燃費（MPG）を予測することが可能です。
　トップページの設定: "/" へのGETリクエストで簡単なメッセージを返す。
　POSTリクエストの処理: POSTリクエストを使って、入力データに基づいた燃費予測を行います。ここで、MPGというデータモデルを定義し、APIのエンドポイント/make_predictionsが予測結果を返します。

【API利用について】
APIは、GETやPOSTといったHTTPリクエストを介してアクセスできます。
APIの利用には、WebブラウザやAPIクライアント（例：Postman、curl）、あるいはプログラムから直接リクエストを送信する方法があります。

1.トップページにアクセス
 ブラウザやcurlコマンドで、APIのルートエンドポイント（http://127.0.0.1:8000/）にアクセスします。
 -リクエスト-
 curl -X GET "http://127.0.0.1:8000/"

 -レスポンス-
 {
   "MPG": "MPG_predictions"
 }

2.燃費予測APIを利用する
 POSTリクエストを使って、自動車の特徴量を送信し、燃費予測を行います。curlやPostmanを使って、以下のリクエストを送信します。
-リクエスト-
curl -X POST "http://127.0.0.1:8000/make_predictions" \
-H "Content-Type: application/json" \
-d '{
    "cylinders": 4,
    "displacement": 120.0,
    "weight": 2300.5,
    "modelyear": 75
    }'

-レスポンス-
 {
   "prediction": "28.4"
 }
