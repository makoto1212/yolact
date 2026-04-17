[English](README_en.md) | **日本語**

# **Y**ou **O**nly **L**ook **A**t **C**oefficien**T**s
```
    ██╗   ██╗ ██████╗ ██╗      █████╗  ██████╗████████╗
    ╚██╗ ██╔╝██╔═══██╗██║     ██╔══██╗██╔════╝╚══██╔══╝
     ╚████╔╝ ██║   ██║██║     ███████║██║        ██║   
      ╚██╔╝  ██║   ██║██║     ██╔══██║██║        ██║   
       ██║   ╚██████╔╝███████╗██║  ██║╚██████╗   ██║   
       ╚═╝    ╚═════╝ ╚══════╝╚═╝  ╚═╝ ╚═════╝   ╚═╝ 
```

リアルタイムインスタンスセグメンテーションのための、シンプルな完全畳み込みモデルです。以下の論文のコードです:
 - [YOLACT: Real-time Instance Segmentation](https://arxiv.org/abs/1904.02689)
 - [YOLACT++: Better Real-time Instance Segmentation](https://arxiv.org/abs/1912.06218)

#### YOLACT++ (v1.2) リリース! ([変更履歴](CHANGELOG.md))
YOLACT++のResNet50モデルはTitan Xpで33.5 fpsで動作し、COCOの`test-dev`で34.1 mAPを達成しています（ジャーナル論文は[こちら](https://arxiv.org/abs/1912.06218)）。

YOLACT++を使用するには、DCNv2のコードをコンパイルしてください。（[インストール](https://github.com/dbolya/yolact#installation)を参照）

#### リアルタイムデモはICCVの動画をご覧ください:
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/0pMfmo8qfpQ/0.jpg)](https://www.youtube.com/watch?v=0pMfmo8qfpQ)

YOLACTベースモデルの出力例（Titan Xpで33.5 fps、COCOの`test-dev`で29.8 mAP）:

![Example 0](data/yolact_example_0.png)

![Example 1](data/yolact_example_1.png)

![Example 2](data/yolact_example_2.png)

# インストール
 - このリポジトリをクローンして移動します:
   ```Shell
   git clone https://github.com/dbolya/yolact.git
   cd yolact
   ```
 - 以下のいずれかの方法で環境を構築します:
   - [Anaconda](https://www.anaconda.com/distribution/)を使用する場合
     - `conda env create -f environment.yml` を実行
   - pipで手動構築する場合
     - Python3環境を用意します（例: virtenvを使用）。
     - [Pytorch](http://pytorch.org/) 1.0.1（以上）とTorchVisionをインストールします。
     - その他のパッケージをインストールします:
       ```Shell
       # pycocotoolsの前にCythonをインストールする必要があります
       pip install cython
       pip install opencv-python pillow pycocotools matplotlib 
       ```
 - YOLACTを学習させる場合は、COCOデータセットと2014/2017のアノテーションをダウンロードしてください。このスクリプトは時間がかかり、`./data/coco`に21GBのファイルを出力します。
   ```Shell
   sh data/scripts/COCO.sh
   ```
 - `test-dev`でYOLACTを評価する場合は、以下のスクリプトで`test-dev`をダウンロードしてください。
   ```Shell
   sh data/scripts/COCO_test.sh
   ```
 - YOLACT++を使用する場合は、変形可能な畳み込み層をコンパイルしてください（[DCNv2](https://github.com/CharlesShang/DCNv2/tree/pytorch_1.0)より）。
   [NVIDIAのウェブサイト](https://developer.nvidia.com/cuda-toolkit)から最新のCUDAツールキットがインストールされていることを確認してください。
   ```Shell
   cd external/DCNv2
   python setup.py build develop
   ```


# 評価
以下はYOLACTモデル（2019年4月5日公開）とTitan XpでのFPS、`test-dev`でのmAPです。**注意:** これらのモデルはオリジナルのダウンロードリンクが期限切れとなったため、[Hugging Faceコレクション](https://huggingface.co/collections/dbolya/yolact-2019-models-68bf670f7239d610bc933a10)に再アップロードされました。

| 画像サイズ | バックボーン   | FPS  | mAP  | 重み                                                                                                              |
|:----------:|:-------------:|:----:|:----:|----------------------------------------------------------------------------------------------------------------------|
| 550        | Resnet50-FPN  | 42.5 | 28.2 | [yolact_resnet50_54_800000.pth](https://huggingface.co/dbolya/yolact-resnet50/resolve/main/yolact_resnet50_54_800000.pth?download=true) |
| 550        | Darknet53-FPN | 40.0 | 28.7 | [yolact_darknet53_54_800000.pth](https://huggingface.co/dbolya/yolact-darknet53/resolve/main/yolact_darknet53_54_800000.pth?download=true) |
| 550        | Resnet101-FPN | 33.5 | 29.8 | [yolact_base_54_800000.pth](https://huggingface.co/dbolya/yolact-base/resolve/main/yolact_base_54_800000.pth?download=true) |
| 700        | Resnet101-FPN | 23.6 | 31.2 | [yolact_im700_54_800000.pth](https://huggingface.co/dbolya/yolact-im700/resolve/main/yolact_im700_54_800000.pth?download=true) |

YOLACT++モデル（2019年12月16日公開）:

| 画像サイズ | バックボーン   | FPS  | mAP  | 重み                                                                                                              |
|:----------:|:-------------:|:----:|:----:|----------------------------------------------------------------------------------------------------------------------|
| 550        | Resnet50-FPN  | 33.5 | 34.1 | [yolact_plus_resnet50_54_800000.pth](https://huggingface.co/dbolya/yolact-plus-resnet50/resolve/main/yolact_plus_resnet50_54_800000.pth?download=true) |
| 550        | Resnet101-FPN | 27.3 | 34.6 | [yolact_plus_base_54_800000.pth](https://huggingface.co/dbolya/yolact-plus-base/resolve/main/yolact_plus_base_54_800000.pth?download=true) |

モデルを評価するには、対応する重みファイルを`./weights`ディレクトリに配置し、以下のコマンドのいずれかを実行してください。各設定の名前は、ファイル名の数字より前の部分です（例: `yolact_base_54_800000.pth`の場合は`yolact_base`）。
## COCOでの定量的結果
```Shell
# 学習済みモデルをバリデーションセット全体で定量評価します。上記の手順でCOCOをダウンロードしておいてください。
# 最後に確認した時点で29.92のバリデーションマスクmAPが得られるはずです。
python eval.py --trained_model=weights/yolact_base_54_800000.pth

# ウェブサイトへの提出用、またはrun_coco_eval.pyスクリプト用にCOCOEval JSONを出力します。
# このコマンドは検出用の'./results/bbox_detections.json'とインスタンスセグメンテーション用の'./results/mask_detections.json'を作成します。
python eval.py --trained_model=weights/yolact_base_54_800000.pth --output_coco_json

# 上記のコマンドで作成したファイルに対してCOCOEvalを実行できます。性能はeval.pyの実装と一致するはずです。
python run_coco_eval.py

# test-dev用のCOCO JSONファイルを出力するには、上記の手順でtest-devをダウンロードした上で以下を実行してください。
python eval.py --trained_model=weights/yolact_base_54_800000.pth --output_coco_json --dataset=coco2017_testdev_dataset
```
## COCOでの定性的結果
```Shell
# COCOでの定性的結果を表示します。以降、信頼度閾値は0.15を使用します。
python eval.py --trained_model=weights/yolact_base_54_800000.pth --score_threshold=0.15 --top_k=15 --display
```
## COCOでのベンチマーク
```Shell
# バリデーションセットの最初の1000枚の画像に対してモデルを実行します
python eval.py --trained_model=weights/yolact_base_54_800000.pth --benchmark --max_images=1000
```
## 画像
```Shell
# 指定した画像に対して定性的結果を表示します。
python eval.py --trained_model=weights/yolact_base_54_800000.pth --score_threshold=0.15 --top_k=15 --image=my_image.png

# 画像を処理して別のファイルに保存します。
python eval.py --trained_model=weights/yolact_base_54_800000.pth --score_threshold=0.15 --top_k=15 --image=input_image.png:output_image.png

# フォルダ内の画像を一括処理します。
python eval.py --trained_model=weights/yolact_base_54_800000.pth --score_threshold=0.15 --top_k=15 --images=path/to/input/folder:path/to/output/folder
```
## 動画
```Shell
# リアルタイムで動画を表示します。"--video_multiframe"で指定したフレーム数を同時に処理し、性能を向上させます。
# "--display_fps"を使用すると、フレームにFPSを直接描画できます。
python eval.py --trained_model=weights/yolact_base_54_800000.pth --score_threshold=0.15 --top_k=15 --video_multiframe=4 --video=my_video.mp4

# ウェブカメラのフィードをリアルタイムで表示します。複数のウェブカメラがある場合は、0の代わりに使用したいウェブカメラのインデックスを指定してください。
python eval.py --trained_model=weights/yolact_base_54_800000.pth --score_threshold=0.15 --top_k=15 --video_multiframe=4 --video=0

# 動画を処理して別のファイルに保存します。上記と同じパイプラインを使用するため、高速です!
python eval.py --trained_model=weights/yolact_base_54_800000.pth --score_threshold=0.15 --top_k=15 --video_multiframe=4 --video=input_video.mp4:output_video.mp4
```
ご覧のとおり、`eval.py`は多くの機能を備えています。`--help`コマンドで全機能を確認できます。
```Shell
python eval.py --help
```


# 学習
デフォルトではCOCOで学習を行います。上記のコマンドでデータセット全体をダウンロードしておいてください。
 - 学習するには、ImageNetで事前学習済みのモデルをダウンロードして`./weights`に配置します。
   - Resnet101の場合、`resnet101_reducedfc.pth`を[こちら](https://huggingface.co/dbolya/yolact-initial-weights/resolve/main/resnet101_reducedfc.pth?download=true)からダウンロードしてください。
   - Resnet50の場合、`resnet50-19c8e357.pth`を[こちら](https://huggingface.co/dbolya/yolact-initial-weights/resolve/main/resnet50-19c8e357.pth?download=true)からダウンロードしてください。
   - Darknet53の場合、`darknet53.pth`を[こちら](https://huggingface.co/dbolya/yolact-initial-weights/resolve/main/darknet53.pth?download=true)からダウンロードしてください。
 - 以下の学習コマンドのいずれかを実行します。
   - 学習中にCtrl+Cを押すと、現在のイテレーションで`*_interrupt.pth`ファイルが保存されます。
   - すべての重みはデフォルトで`./weights`ディレクトリに`<config>_<epoch>_<iter>.pth`というファイル名で保存されます。
```Shell
# ベース設定でバッチサイズ8（デフォルト）で学習します。
python train.py --config=yolact_base_config

# yolact_base_configをバッチサイズ5で学習します。550pxモデルの場合、1バッチあたり約1.5GBのVRAMを使用するので、それに応じて設定してください。
python train.py --config=yolact_base_config --batch_size=5

# 特定の重みファイルを使用してyolact_baseの学習を再開し、重みファイル名に記載されたイテレーションから開始します。
python train.py --config=yolact_base_config --resume=weights/yolact_base_10_32100.pth --start_iter=-1

# ヘルプオプションで利用可能なすべてのコマンドライン引数の説明を確認できます
python train.py --help
```

## マルチGPUサポート
YOLACTは学習時に複数のGPUをシームレスにサポートしています:

 - スクリプトを実行する前に: `export CUDA_VISIBLE_DEVICES=[gpus]` を実行してください
   - [gpus]には使用したいGPUのインデックスをカンマ区切りで指定します（例: 0,1,2,3）。
   - GPU1台のみ使用する場合でもこの設定を行ってください。
   - GPUのインデックスは`nvidia-smi`で確認できます。
 - 上記の学習コマンドでバッチサイズを`8*GPU数`に設定するだけです。学習スクリプトが自動的にハイパーパラメータを適切な値にスケーリングします。
   - メモリに余裕がある場合はバッチサイズをさらに大きくできますが、使用するGPU数の倍数にしてください。
   - 各GPUに割り当てる画像数を個別に指定したい場合は、`--batch_alloc=[alloc]`を使用します。[alloc]は各GPUの画像数をカンマ区切りで指定し、合計が`batch_size`と一致する必要があります。

## ログ
YOLACTはデフォルトで学習とバリデーションの情報をログ出力します。`--no_log`で無効化できます。ログの可視化ガイドは近日公開予定ですが、現時点では`utils/logger.py`の`LogVizualizer`を参照してください。

## Pascal SBD
迅速な実験や他の手法との比較のために、Pascal SBDアノテーションでの学習設定も含まれています。Pascal SBDで学習するには、以下の手順に従ってください:
 1. [こちら](http://home.bharathh.info/pubs/codes/SBD/download.html)からデータセットをダウンロードします。上部「Overview」セクションの最初のリンクです（ファイル名は`benchmark.tgz`）。
 2. データセットを任意の場所に展開します。データセット内に`dataset/img`というフォルダがあるはずです。YOLACTのルートに`./data/sbd`ディレクトリを作成し、`dataset/img`を`./data/sbd/img`にコピーします。
 4. COCO形式のアノテーションを[こちら](https://drive.google.com/open?id=1ExrRSPVctHW8Nxrn0SofU1lVhK5Wn0_S)からダウンロードします。
 5. アノテーションを`./data/sbd/`に展開します。
 6. `--config=yolact_resnet50_pascal_config`で学習できます。他のモデルへの拡張方法はこの設定を参照してください。

これらすべてをスクリプトで自動化する予定です。また、アノテーション変換に使用したスクリプトを`./scripts/convert_sbd.py`に置いていますが、使用方法については中身を確認してください。

結果を検証したい場合は、`yolact_resnet50_pascal_config`の重みを[こちら](https://drive.google.com/open?id=1yLVwtkRtNxyl0kxeMCtPXJsXFFyc_FHe)からダウンロードできます。このモデルはマスクAP_50で72.3、マスクAP_70で56.2を達成するはずです。なお、「all」APは他の論文で報告されている「vol」APとは異なります（COCOの方式ではなく、`0.1`から`0.9`まで`0.1`刻みの閾値の平均を使用しています）。

## カスタムデータセット
以下の手順で独自のデータセットでも学習できます:
 - データセットのCOCO形式のObject Detection JSONアノテーションファイルを作成します。仕様は[こちら](http://cocodataset.org/#format-data)を参照してください。一部のフィールドは使用しないため、以下は省略可能です:
   - `info`
   - `license`
   - `image`内: `license, flickr_url, coco_url, date_captured`
   - `categories`（カテゴリは独自のフォーマットを使用します。以下を参照）
 - `data/config.py`の`dataset_base`の下にデータセットの定義を作成します（各フィールドの説明は`dataset_base`のコメントを参照）:
```Python
my_custom_dataset = dataset_base.copy({
    'name': 'My Dataset',

    'train_images': 'path_to_training_images',
    'train_info':   'path_to_training_annotation',

    'valid_images': 'path_to_validation_images',
    'valid_info':   'path_to_validation_annotation',

    'has_gt': True,
    'class_names': ('my_class_id_1', 'my_class_id_2', 'my_class_id_3', ...)
})
```
 - 注意点:
   - アノテーションファイルのクラスIDは1から始まり、`class_names`の順序に従って連番で増加する必要があります。アノテーションファイルがこの形式でない場合（COCOなど）は、`dataset_base`の`label_map`フィールドを参照してください。
   - バリデーション用の分割を作成したくない場合は、バリデーションにも同じ画像パスとアノテーションファイルを使用してください。デフォルトでは（`python train.py --help`を参照）、`train.py`は2エポックごとにデータセットの最初の5000枚の画像に対するバリデーションmAPを出力します。
 - 最後に、同じファイルの`yolact_base_config`で`'dataset'`の値を`'my_custom_dataset'`（または上記で命名した設定オブジェクト名）に変更します。これで前のセクションの学習コマンドを使用できます。

#### ゼロからカスタムデータセットを作成する
カスタムデータセットのアノテーション方法とYOLACTで使用するための準備については、[@Amit12690氏のこちらの投稿](https://github.com/dbolya/yolact/issues/70#issuecomment-504283008)を参照してください。




# 引用
YOLACTまたはこのコードベースを使用した場合は、以下を引用してください
```
@inproceedings{yolact-iccv2019,
  author    = {Daniel Bolya and Chong Zhou and Fanyi Xiao and Yong Jae Lee},
  title     = {YOLACT: {Real-time} Instance Segmentation},
  booktitle = {ICCV},
  year      = {2019},
}
```

YOLACT++の場合は、以下を引用してください
```
@article{yolact-plus-tpami2020,
  author  = {Daniel Bolya and Chong Zhou and Fanyi Xiao and Yong Jae Lee},
  journal = {IEEE Transactions on Pattern Analysis and Machine Intelligence}, 
  title   = {YOLACT++: Better Real-time Instance Segmentation}, 
  year    = {2020},
}
```



# 連絡先
論文やコードに関するご質問は、[Daniel Bolya](mailto:dbolya@ucdavis.edu)までお問い合わせください。
