# OTTO-Multi-Objective-Recommender-System

20221121

昨日までは酵素コンペをやろうと思っていたが、残り期間が1か月くらいで短いのと、EDAしても内容がよくわからんので
レコメンデーションやることにした。こっちのほうが過去コンペとか書籍とか初心者にとってはとっつきやすい。
まずは、公開notebookを参考にしてEDAに努める。

![image](https://user-images.githubusercontent.com/99288104/203073610-57aac5c7-5b32-44cf-aa32-b7487a5751e2.png)

方針
Kaggle上で全てを完結させる
Version名は常にVersion○に統一して、README.mdに簡単に内容を書き込む
issue boardに思いついたアイデアを書いていく。
詳しい実験などを行った際には、その旨をREADMEに書き込み、詳しい実験結果はそのNoteBookの一番上にoverviewとして書き込む -> README.mdだけで事足りたので使いませんでした。

Basics

Overview(DeepL)
コンペティションの目標
このコンペティションのゴールは、eコマースのクリック、カートへの追加、および注文を予測することである。あなたは、ユーザーセッションの過去のイベントに基づいて、多目的レコメンダーシステムを構築します。

あなたの仕事は、関係者全員のショッピング体験を向上させるのに役立ちます。顧客はよりカスタマイズされたレコメンデーションを受けることができ、オンライン小売業者は売上を伸ばすことができるかもしれません。

コンテキスト
オンラインショッピングでは、大手小売業者が提供する何百万もの商品の中から好きなものを選ぶことができます。しかし、あまりの種類の多さに圧倒され、カートを空にしてしまうこともあります。これでは、購入しようとする買い物客にも、売上を逃した小売業者にもメリットがありません。このような理由から、オンラインショップでは、買い物客の興味や動機に最も適した商品を案内するレコメンダーシステムが利用されています。データサイエンスを利用して、お客様が実際に見たい商品、カートに入れたい商品、注文したい商品をリアルタイムに予測する能力を高めれば、次にお気に入りの小売店でオンラインショッピングをするときの顧客体験を向上させることができます。

現在のレコメンダーシステムは、単純な行列分解から変形型ディープニューラルネットワークまで、様々なアプローチのモデルで構成されています。しかし、複数の目的を同時に最適化できる単一のモデルは存在しません。このコンペティションでは、過去の同一セッションのイベントに基づいて、クリックスルー、カートへの追加、コンバージョン率を予測する単一エントリーを構築します。

19,000以上のブランドから1,000万以上の商品を扱うOTTO社は、ドイツ最大のオンラインショップです。OTTOは、ハンブルクに拠点を置く多国籍企業Otto Groupの一員で、Crate & Barrel（米国）や3 Suisses（フランス）なども傘下に入れています。

あなたの仕事は、オンライン小売業者が顧客のリアルタイムの行動に基づいて、膨大な種類の中からより適切な商品を選んで推薦できるようにすることです。レコメンデーションを改善することで、無限にあるように見える選択肢の中から、買い物客がより簡単に、より魅力的に商品を選べるようになります。
