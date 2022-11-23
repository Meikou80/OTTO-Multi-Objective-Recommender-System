# Kaggle-OTTO-Multi-Objective-Recommender-System

### 20221121

昨日までは酵素コンペをやろうと思っていたが、残り期間が1か月くらいで短いのと、EDAしても内容がよくわからんので
レコメンデーションやることにした。こっちのほうが過去コンペとか書籍とか初心者にとってはとっつきやすい。
まずは、公開notebookを参考にしてEDAに努める。

![image](https://user-images.githubusercontent.com/99288104/203073610-57aac5c7-5b32-44cf-aa32-b7487a5751e2.png)

## 方針

-Kaggle上で全てを完結させる<br>
-Version名は常にVersion○に統一して、README.mdに簡単に内容を書き込む<br>
-issue boardに思いついたアイデアを書いていく。<br>
-詳しい実験などを行った際には、その旨をREADMEに書き込み、詳しい実験結果はそのNoteBookの一番上にoverviewとして書き込む -> README.mdだけで事足りたので使いませんでした。<br>

## Basics
### Overview(DeepL)
 このコンペティションのゴールは、eコマースのクリック、カートへの追加、および注文を予測することである。あなたは、ユーザーセッションの過去のイベントに基づいて、多目的レコメンダーシステムを構築します。
あなたの仕事は、関係者全員のショッピング体験を向上させるのに役立ちます。顧客はよりカスタマイズされたレコメンデーションを受けることができ、オンライン小売業者は売上を伸ばすことができるかもしれません。

 オンラインショッピングでは、大手小売業者が提供する何百万もの商品の中から好きなものを選ぶことができます。しかし、あまりの種類の多さに圧倒され、カートを空にしてしまうこともあります。これでは、購入しようとする買い物客にも、売上を逃した小売業者にもメリットがありません。このような理由から、オンラインショップでは、買い物客の興味や動機に最も適した商品を案内するレコメンダーシステムが利用されています。データサイエンスを利用して、お客様が実際に見たい商品、カートに入れたい商品、注文したい商品をリアルタイムに予測する能力を高めれば、次にお気に入りの小売店でオンラインショッピングをするときの顧客体験を向上させることができます。
 現在のレコメンダーシステムは、単純な行列分解から変形型ディープニューラルネットワークまで、様々なアプローチのモデルで構成されています。しかし、複数の目的を同時に最適化できる単一のモデルは存在しません。このコンペティションでは、過去の同一セッションのイベントに基づいて、クリックスルー、カートへの追加、コンバージョン率を予測する単一エントリーを構築します。
 19,000以上のブランドから1,000万以上の商品を扱うOTTO社は、ドイツ最大のオンラインショップです。OTTOは、ハンブルクに拠点を置く多国籍企業Otto Groupの一員で、Crate & Barrel（米国）や3 Suisses（フランス）なども傘下に入れています。
 あなたの仕事は、オンライン小売業者が顧客のリアルタイムの行動に基づいて、膨大な種類の中からより適切な商品を選んで推薦できるようにすることです。レコメンデーションを改善することで、無限にあるように見える選択肢の中から、買い物客がより簡単に、より魅力的に商品を選べるようになります。

### Evaluation(DeepL)
投稿は、アクションタイプごとにRecall@20で評価し、3つのRecall値を加重平均しています。
![image](https://user-images.githubusercontent.com/99288104/203316754-dfdcd44c-1b9b-42ac-9cdd-fe0709098be1.png)

ここで、以下のように定義される。

![image](https://user-images.githubusercontent.com/99288104/203316888-33883f86-c324-4c6f-afcd-cd658fb0a2da.png)

Nはテストセット内のセッションの総数であり、 predicted aidsは各セッションタイプ（例えば、提出ファイルの各行）に対する予測値で、最初の 20 個の予測値の後に切り捨てられる。

 テストデータ内の各セッションについて、あなたのタスクは、テストセッションの最後のタイムスタンプの後に発生する各タイプの補助値を予測することです。言い換えれば、テストデータにはタイムスタンプで切り捨てられたセッションが含まれており、切り捨てられた時点以降に発生するものを予測することになります。
 クリックについては、各セッションのグランドトゥルース値は1つだけで、それはセッション中にクリックされた次の広告です（ただし、最大20広告値まで予測することができます）。カートとオーダーのグランドトゥルースは、セッション中にそれぞれカートに追加され、注文されたすべての援助値を含んでいます。
 
### data(DeepL)
 このコンペティションのゴールは、eコマースのクリック、カートへの追加、注文を予測することです。あなたは、ユーザーセッションの過去のイベントに基づいて、多目的レコメンダーシステムを構築します。
 学習データには、電子商取引のセッション情報がすべて含まれています。テストデータの各セッションについて、あなたのタスクは、テストセッションの最後のタイムスタンプtsの後に発生する各セッションタイプの補助値を予測することです。言い換えれば、テストデータには、タイムスタンプで切り捨てられたセッションが含まれており、切り捨てられた時点以降に何が起こるかを予測することになります。
 その他の背景については、公開されているOTTO Recommender Systems DatasetのGitHubを参照してください。
 
 ### ファイル
-train.jsonl - 完全なセッションデータを含むトレーニングデータです。<br>
-session - 一意のセッションID<br>
-events - セッション内で時間順に並べられたイベントの列です。<br>
-aid - 関連するイベントのアーティクル ID (製品コード)<br>
-ts - そのイベントの Unix タイムスタンプ。<br>
-type - イベントのタイプ、つまり、セッション中に製品がクリックされたか、ユーザーのカートに追加されたか、注文されたか。<br>
-test.jsonl - 切り捨てられたセッションデータを含むテストデータ<br>
 あなたのタスクは、セッションの切り捨て後にクリックされた次の商品と、カートや注文に追加された残りの商品を予測することです; あなたは、各セッションタイプについて最大20の値を予測することができます。
-sample_submission.csv - 正しいフォーマットのサンプル送信ファイルです。<br>

## Paper<br>

## Log

## 20221122
-まずは、Edward氏の公開noteboookのEDA(https://www.kaggle.com/code/edwardcrookenden/otto-getting-started-eda-baseline)
を写経してみる。(nb001)

##データ構造

-session - 一意のセッションID。各セッションには、時間順に並んだイベントのリストが含まれる。

-events - セッション内の時間順に並んだイベントのリスト。各イベントは、3つの情報を含む。

-aid - 関連イベントのアーティクルID（製品コード)

-ts - イベントの Unix タイムスタンプ (Unix time は、1970年1月1日 00:00:00 UTC からの経過したマイクロ秒の数)

-type - つまり、セッション中に製品がクリックされたか（clicks）、ユーザーのカートに入れられたか（carts）、注文されたか（orders）。
  
##ベースライン

 テストデータ`はトレーニングデータと同様にセッションを切り捨てたデータである。タスクは、セッションの切り捨て後にクリックされた次のエイドと、カートや注文に追加された残りのエイドを予測することであり、各セッションタイプについて最大20の値を予測することができる

 提出されたものは、各アクションタイプのRecallで評価され、3つのRecall値は重み付け平均される。{クリック': 0.10, 'カート': 0.30、'orders': 0.60} . 注文'の予測は、重みの大部分を占めるので、正しく行うことが重要です :)

 テストデータの各セッションについて、あなたのタスクは、テストセッションの最後のタイムスタンプの後に発生する各タイプの援助値を予測することです。言い換えれば、テストデータにはタイムスタンプで切り捨てられたセッションが含まれており、切り捨てられた時点以降に発生するものを予測することになります。

 クリックについては、各セッションのグランドトゥルース値は1つだけで、それはセッション中にクリックされた次の広告です（ただし、最大20広告値まで予測することができます）。カートと注文のグランドトゥルースは、セッション中にそれぞれカートに追加され、注文されたすべての援助値を含んでいます。

 各セッションとタイプの組み合わせは、提出物の中でそれ自身の session_type 行に表示される必要があり（セッションごとに 3 行）、予測はスペースで区切られる必要があります。これは、以下の sample_test_df で見ることができます。

## 20221123
-GUNES氏の公開noteboookのEDA(https://www.kaggle.com/code/gunesevitan/otto-multi-objective-recommender-system-eda)
を写経してみる。(nb002)

わかったこと

-1　トレーニングセットとテストセットは時間によって分割されています。トレーニングセットは4週間分のユーザーイベント、テストセットはトレーニングセットの翌週分のユーザーイベントです。トレーニングセットとテストセットの間でイベントが重複していないのは、リークを防ぐためにトレーニングセットのイベントの一部が切り取られているためです。イベントの種類の比率は、トレーニングセットとテストセットでほぼ同じであることが、以下の可視化からわかります。

-2　すでにご存知のように、トレーニングセットには1280万人のユニークユーザー（セッション）が存在し、テストセットには160万人のユニークセッションが存在します。しかし、その統計量もトレーニングセットとテストセットでは異なっています。トレーニングセットでは、平均セッション時間はテストセットの4倍です。トレーニングセットで最も短いセッションは2であり、これは単一イベントセッションでグランドトゥルースを作成することができないためです。トレーニングセットで最も長いセッションは500であり、これは極端に長いセッションを切り捨てるための閾値のように見えます。
　トレーニングセットは4週間であるのに対し、テストセットは1週間なので、セッション統計の不一致は問題ではないかもしれません。平均セッション時間がトレーニングセットで4倍長いのは、そのせいでもあります。
　セッションの持続時間の分布は、以下の可視化から見ることができます。どちらの密度も、解釈しやすいように対数スケールで表示されています。
 
 -3 トレーニングセットには180万個のユニークな商品(aid)があり、テストセットには78万個のユニークなaidがある。テストセットのaidはトレーニングセットのaidのサブセットなので、テストセットには未見のaidはない。また、トレーニングセットとテストセットではエイドの出現頻度分布が異なるが、これは前述の理由からである。トレーニングセットのエイドオカレンスの分布はより正常ですが、テストセットのエイドオカレンスの分布は多峰性で、これはセッションの切り捨てによるものだと思われます。
 以下の可視化から、エイドオキュレンスの分布を見ることができます。どちらの密度も、解釈しやすいように対数スケールで表示されています。
 
 -4  訓練とテストの援助頻度は非常によく似ています。テストセットはトレーニングセットの20倍近く小さいので、両者の間にはわずかな違いがある。トレーニングセットとテストセットを連結してエイド頻度を計算することは、エイド頻度を何らかの形で利用するのであれば、より信頼性が高いと思われる。
 トレーニングセット、テストセット、トレーニング＋テストセットについて、最も頻度の高いエイド上位20個を以下に可視化した。X軸は頻度、Y軸は括弧内のエイドとその頻度である。

 -5　トレーニングセットでは、平均でセッションの93%がクリック、16%がカート、10%が注文となっています。これらの数字は、すべてのセッションで計算された平均値であるため、100に加算されることはありません。標準偏差はそれぞれ12、10、8です。テストセットでは、セッションがランダムに切り捨てられるため、平均と標準偏差の数値が少し高くなります。イベントはテストセッションで過不足なく表現されていると思われます。(0 -> クリック, 1 -> カート, 2 -> 注文)
 
 -6　セッションの中には、クリック、カート、注文がないものもありますが、以下の統計には、それらの欠落したイベントタイプは含まれていません。ハイライトは、テストセットのセッションの中にはクリック、カート、注文しかないものがありますが、トレーニングセットのセッションはより異質なものであるということです。トレーニングセットの均質なセッションは、クリックのみのセッションです。この不一致はモデリング時に問題になる可能性があります。
 
 -7　もう一つ重要なことは、セッションの開始と終了の方法です。ここで興味深いことが2つあります。まず、セッションはクリックで始まるはずですが、カートまたは注文で始まるセッションはごくわずかです。これらのセッションは1％未満であり、選択した時間枠に収まるように、始まりから切り捨てられた可能性があります。もう一つは、テストセットのセッションは、切り捨てのため、注文で終わる可能性が低いということです。テストセットで切り捨てられたタイムステップは、注文イベントの可能性が高くなります。
 
 -8　1セッションが1タイムステップ以上であれば、トレーニングセットとテストセットの両方のGround-truthを作成することができます。クリックとその他のイベントのGround-truthは異なる方法で作成されます。あるタイムステップのクリックは、そのセッションの次のクリックがGround-truthとなります。あるタイムステップのカートとオーダーのGround-truthは、そのセッションの次のすべてのユニークなカートとオーダーのコレクションである。
 
 -9 プレシジョン（陽性予測値）とリコール（真陽性率）は、最もよく使われるバイナリおよびマルチクラス分類の指標である。精度の計算式は TP / (TP + FP)、リコールの計算式は TP / (TP + FN)である。精度を上げるには偽陽性をより少なく予測することが重要であり、リコールを上げるには真陽性をより多く予測することが重要です。これらの指標は常にトレードオフの関係にあるため、これらの指標の調和平均（F1スコア）も併用されている。
 これらの指標は情報検索問題でも利用されている。この問題では、予測値はイベントタイプごとにrecall@20で評価され、3つのrecall値にはそれぞれ指定された重みが掛け合わされる。イベントタイプの重みは、クリックは0.1、カートは0.3、注文は0.6である。この文脈では、偽陽性は偽陰性ほど重要ではないので、リコールはより理にかなっています。

 -10 テストセットのセッションは最後のイベントによって切り捨てられ、それらの切り捨てられたイベントのうち、いくつかが予測されると予想される。切り捨てられたイベントの種類に関係なく、クリック、カート、オーダーについて20の予測を行うことができる。リコールはこれらの20の予測に対して別々に計算され、イベントタイプに基づいて重み付けされます。最後に、3つのリコール値が平均化されます。
 例えば、セッション 747 がインデックス 59605 で切り捨てられ、そのイベントの支援を予測しようとし ていると仮定します。
 
  -11 クリックの基底真理は1つだけなので、recallは0か1のどちらかである。これは単純にin操作で評価することができる。もし、ground-truthのクリック支援がクリック予測にあれば、recallは1です。 そうでなければ、recallは0です。クリックrecallに0.1を掛ければ、0か0.1かのどちらかになります。

  -12 カートと注文のイベントタイプでは、複数のグランドトゥルース補助があるため、リコールは0から1の間の任意の値になる。これらのイベントタイプのリコールは集合演算で計算することができる。真陽性はGround-Truthと予測エイドの交点、偽陰性はGround-Truthと予測エイドの交点である。
 まれにGround-truthの数が20を超えることがある。そのような場合の計算ミスを防ぐため、分母はmin(20, n_aids)とする。
