# セグメンテーション

これまでに、画像内のオブジェクトを*バウンディングボックス*で予測することで特定するオブジェクト検出について学びました。しかし、いくつかのタスクではバウンディングボックスだけでなく、より正確なオブジェクトの位置特定も必要です。このタスクは**セグメンテーション**と呼ばれます。

## [講義前クイズ](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/112)

セグメンテーションは**ピクセル分類**と見なすことができ、画像の**各**ピクセルについてそのクラスを予測する必要があります（*背景*がクラスの一つです）。セグメンテーションアルゴリズムには主に二つあります：

* **セマンティックセグメンテーション**はピクセルのクラスを示すだけで、同じクラスの異なるオブジェクトを区別しません。
* **インスタンスセグメンテーション**はクラスを異なるインスタンスに分けます。

例えば、インスタンスセグメンテーションではこれらの羊は異なるオブジェクトですが、セマンティックセグメンテーションではすべての羊が一つのクラスで表されます。

<img src="images/instance_vs_semantic.jpeg" width="50%">

> 画像は[このブログ記事](https://nirmalamurali.medium.com/image-classification-vs-semantic-segmentation-vs-instance-segmentation-625c33a08d50)から引用。

セグメンテーションには異なるニューラルアーキテクチャがありますが、すべて同じ構造を持っています。ある意味では、以前学んだオートエンコーダーに似ていますが、元の画像を解体するのではなく、**マスク**を解体することが目標です。したがって、セグメンテーションネットワークは以下の部分から構成されます：

* **エンコーダー**は入力画像から特徴を抽出します。
* **デコーダー**はそれらの特徴を**マスク画像**に変換し、クラスの数に対応するサイズとチャネル数を持ちます。

<img src="images/segm.png" width="80%">

> 画像は[この出版物](https://arxiv.org/pdf/2001.05566.pdf)から引用。

セグメンテーションに使用される損失関数について特に言及する必要があります。古典的なオートエンコーダーを使用する場合、2つの画像間の類似性を測定する必要があり、そのために平均二乗誤差（MSE）を使用できます。セグメンテーションでは、ターゲットマスク画像の各ピクセルがクラス番号を表しているため（3次元でのワンホットエンコーディング）、分類に特化した損失関数、すなわち全ピクセルで平均化されたクロスエントロピー損失を使用する必要があります。マスクがバイナリの場合は、**バイナリクロスエントロピー損失**（BCE）が使用されます。

> ✅ ワンホットエンコーディングは、クラスラベルをクラスの数に等しい長さのベクトルにエンコードする方法です。この技術については[この記事](https://datagy.io/sklearn-one-hot-encode/)を参照してください。

## 医療画像のセグメンテーション

このレッスンでは、ネットワークをトレーニングして医療画像上のヒトの母斑（ほくろとも呼ばれます）を認識するセグメンテーションを実際に見ていきます。画像ソースとして、<a href="https://www.fc.up.pt/addi/ph2%20database.html">PH<sup>2</sup>データベース</a>の皮膚鏡画像を使用します。このデータセットには、典型的な母斑、非典型的な母斑、メラノーマの3つのクラスの200枚の画像が含まれています。すべての画像には、母斑をアウトラインする対応する**マスク**も含まれています。

> ✅ この技術はこの種の医療画像に特に適していますが、他にどのような実世界の応用を考えられますか？

<img alt="navi" src="images/navi.png"/>

> 画像はPH<sup>2</sup>データベースから引用。

私たちは、背景から任意の母斑をセグメント化するモデルをトレーニングします。

## ✍️ 演習: セマンティックセグメンテーション

以下のノートブックを開いて、さまざまなセマンティックセグメンテーションアーキテクチャについて学び、それらを使って練習し、実際に見ることができます。

* [セマンティックセグメンテーション Pytorch](../../../../../lessons/4-ComputerVision/12-Segmentation/SemanticSegmentationPytorch.ipynb)
* [セマンティックセグメンテーション TensorFlow](../../../../../lessons/4-ComputerVision/12-Segmentation/SemanticSegmentationTF.ipynb)

## [講義後クイズ](https://red-field-0a6ddfd03.1.azurestaticapps.net/quiz/212)

## 結論

セグメンテーションは、バウンディングボックスを超えてピクセルレベルの分類を行う非常に強力な画像分類技術です。医療画像など、さまざまなアプリケーションで使用される技術です。

## 🚀 チャレンジ

ボディセグメンテーションは、人々の画像を使って行うことができる一般的なタスクの一つです。他の重要なタスクには、**スケルトン検出**や**ポーズ検出**が含まれます。[OpenPose](https://github.com/CMU-Perceptual-Computing-Lab/openpose)ライブラリを試して、ポーズ検出がどのように使用できるかを見てみましょう。

## レビュー & 自己学習

この[ウィキペディアの記事](https://wikipedia.org/wiki/Image_segmentation)は、この技術のさまざまな応用について良い概要を提供しています。この分野のインスタンスセグメンテーションとパンオプティックセグメンテーションのサブドメインについて、さらに独自に学んでください。

## [課題](lab/README.md)

このラボでは、Kaggleからの[セグメンテーションフルボディMADSデータセット](https://www.kaggle.com/datasets/tapakah68/segmentation-full-body-mads-dataset)を使用して**人間の体のセグメンテーション**に挑戦してください。

**免責事項**:  
この文書は、機械翻訳AIサービスを使用して翻訳されています。正確性を追求していますが、自動翻訳には誤りや不正確さが含まれる可能性があります。原文の母国語の文書が権威ある情報源と見なされるべきです。重要な情報については、専門の人間翻訳を推奨します。この翻訳の使用から生じる誤解や誤訳については、一切の責任を負いません。