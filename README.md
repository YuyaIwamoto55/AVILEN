# AVILEN--
【まとめ】
ニュース記事を９つのカテゴリーへ分類するタスクを行なった。
使用したモデルは、ネットに精度が出やすいと記載があり、bert-base-japanese-whole-word-maskingを選択した。
ハイパーパラーメーターチューニングを9パターンを試行し、一番良い分類精度は、約89.9%となった。 （学習データのバッチサイズ32、分類タスク時の学習率1e-5）

Batch_size、学習率のハイパーパラメーター調整を行なった。
①データセットからデータローダーを32,64,128で実行
128は、CUDAメモリが足りず、最後までモデリングができなったので省略
②学習率は、1e-3, 1e-5, 1e-7で実行しました。

バッチサイズ32、学習率1e-3：11.5%
バッチサイズ32、学習率1e-5：89.9%
バッチサイズ32、学習率1e-7：32.0% 

バッチサイズ64、学習率1e-3：12.4%
バッチサイズ64、学習率1e-5：88.8%
バッチサイズ64、学習率1e-7：27.3%

学習率では、学習率1e-5が精度がよく、Train_loss、Val_lossも緩やかに下降し、過学習もみられなかった。lrが1e-3の際はtrain_lossが乱高下していることから、学習率が大きすぎて、最適な学習率に収束できなかったことがわかる。学習率1e-7では、Train_lossの経過を見たところ、学習率が小さすぎて、val_lossは収束しきれていなかったのではないかと考えた。
学習率1e-5において、バッチサイズ32・64での違いは1%の違いがでた。

精度を上げるために以下を行なったが、精度向上は見られなかった。
③バッチサイズ32、学習率5e-5：87.1% 
Train _lossは乱高下しながら下がっており、Vαl＿lossは一度下がったが半分以上は上昇してしまったことから過学習が考えられる。

④③よりも学習率を1/2小さくし、バッチサイズ32、学習率2.5e-5：88.5%
Train _lossは③よりも緩やかに下がっているが、Vαl＿lossはやはり過学習してしまった。

【考察】
精度を上げていくには、ハイパーパラメーターチューニングを考えているが、時間の関係上できていない。もし、最大系列長、ミニバッチサイズ、学習係数、ウォームアップ時間の割合のパラメーターのグリッドリサーチにより、最適なハイパーパラメータを見つければ、精度は上げられるのではないかと考える。
さらに、参考としたWebサイトでは、87.6%→96.7%まで改善させた例をみると、事前学習されたモデルそのものを別のモデルに変えてみることも精度を上げる必要だと考えられる。（日本語版ウィキペディア全文学習済みモデル、トークナイザーも変更）
