GUIモデル
===

GUIモデルはWindowsデスクトップなどのGUIスクリーンのモデルです。その中にマウスやキーボードの操作をシミュレートするオペレーションが定義できます。画像認識とOCR技術を使ってSWATHubはリモートデスクトップ、Citrixを含めすべてのデスクトップアプリケーションの自動化をサポートします。APIなどの技術を使わないレガシーアプリを自動化する場合に、非常に有効な手段です。

ただし、GUIオペレーションの実行効率が若干悪いことが難点です。特定なGUI環境に依存するため異なる端末で実行する場合、安定性が悪くなることあります。その為、他の自動化モデルをまずは優先し、それが利用できない場合にGUIモデルを使用することをお勧めです。

モデル構築
---

SWATHubでGUIの画像ファイルと一連のアクションコマンドでGUIモデルを構築します。

1. デスクトップシステムのスクリーンショットを撮るツールを利用して、デスクトップのGUIエリアのスクリーンショットを撮ります<sup>1</sup> <sup>2</sup>。
1. 取得した一連の画像をSWATHubのモデルライブラリにインポートします。
1. それぞれの画像オブジェクトが作成され、それの配下にGUIオペレーションを定義します。

?> 1. 現在スクリーンショットのフォーマットは`png`のみをサポートしています。

?> 2. RetinaのMacディスプレイを利用する場合、[GUI Toolset](http://www.smartekworks.com/tools/SikuliX-1.1.2-Setup.zip)を利用して、スクリーンショットを撮る必要があります。

詳細の手順は[マニュアル](../manual/design_model#guiモデル)を参照してください。オペレーションを定義する際に利用可能なアクションコマンドは下記になります。

### GUIアクションコマンド
GUIオペレーションは一連のGUIアクションコマンドで定義しています。GUIオペレーションの実行時にそれらのGUIアクションコマンドは順番に実施します。下記は現在サポートしているGUIアクションコマンドです。

#### 画像探し
このコマンドは実行時に指定した画像を探します。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| 画像 | インポートしたスクリーン上で画像エリアを定義する<br>`[x,y,width,height]`のJSON配列です。

##### 出力結果
スクリーン上で見つける画像情報を定義します。画像情報は画像のエリアを定義する`[x,y,width,height]`のJSON配列です。

#### 画像取得
このコマンドは実行時に指定した画像をスクリーン上で探し、**Base64**フォーマットの画像データを取得します。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| 画像 | インポートしたスクリーン上で画像エリアを定義する<br>`[x,y,width,height]`のJSON配列です。
| オフセットエリア | (省略可) 定義した画像と離れた画像エリアを指定するためのオフセット<br>`[x,y,width,height]`のJSON配列です。セットしない場合、定義した画像を取得します。

##### 出力結果
スクリーン上に見つけた画像データ。画像データはBase64の文字列にエンコードされます。

#### 画像クリック
このコマンドは実行時にスクリーン上で指定した画像を探して、クリックします。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| 画像 | インポートしたスクリーン上で画像エリアを定義する<br>`[x,y,width,height]`のJSON配列です。
| クリックタイプ | クリックのタイプ：<br>`シングルクリック`、`ダブルクリック`、`右クリック`。
| オフセットエリア | (省略可) 定義した画像と離れた画像エリアを指定するためのオフセット<br>`[x,y,width,height]`のJSON配列です。セットしない場合、定義した画像を取得します。
| 修飾キー | (省略可) クリックと組み合わせしたキー<sup>1</sup>です。例えば、`SHIFT`、`ALT+CTRL`。

##### 出力結果
なし

?> 1. 現在`SHIFT`、`CTRL`、`ALT`、`WIN`と`CMD`のみをサポートしています。

#### 画像ホバー
このコマンドは実行時にスクリーン上で指定した画像を探して、マウスホバーします。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| 画像 |  インポートしたスクリーン上で画像エリアを定義する<br>`[x,y,width,height]`のJSON配列です。
| オフセット | (省略可) 定義した画像と離れた位置を指定するためのオフセット<br>`[x,y]`のJSON配列です。<br>セットしない場合、定義した画像のセンターを取得します。

##### 出力結果
なし

#### 画像保存
このコマンドは実行時に取得した**Base64**フォーマットの画像をローカルファイルに保存します。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| 変数   | `画像取得`コマンドで画像を格納した変数の名前。
| ファイル   | 画像ファイルのパス、例えば、`C:\data\capture.png`。

##### 出力結果
なし

#### テキスト記入
このコマンドは実行時にスクリーン上でフォーカスしたエリアにテキストを記入<sup>1</sup>します。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| テキスト  | 記入するためのテキスト、[キーストローク](model_web#テキスト)もサポートしています。

##### 出力結果
なし

?> 英語以外のテキストを記入する場合、IMEを利用せずに、クリップボードを経由で記入しています。クリップボードが利用できない場合、このコマンドはうまく機能しません。

#### キータイプ
このコマンドは実行時にスクリーン上でフォーカスしたエリアにキーボードのキーストロークをします。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| テキスト  | 送信するキーです。特殊キーはサポートしません。
| 修飾キー | (省略可) クリックと組み合わせしたキー<sup>1</sup>です。例えば、`SHIFT`、`ALT+CTRL`。

##### 出力結果
なし

?> 1. 現在`SHIFT`、`CTRL`、`ALT`、`WIN`と`CMD`のみをサポートしています。

#### 画像チェック
このコマンドが実行時のデスクトップ上に指定した画像が存在するかどうかをチェックし、結果を返します。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| 画像  | インポートしたスクリーン上で画像エリアを定義する<br>`[x,y,width,height]`のJSON配列です。
| 変数  | (省略可)このコマンドでチェックした結果（`true`もしく`false`）を格納した変数の名前。

##### 出力結果
なし

#### テキストOCR
このコマンドは実行時にスクリーン上で指定した画像エリアのテキスト内容をOCRで識別<sup>1</sup>します。

##### 入力引数
| 項目 | 説明
| ---------- | -----------
| 画像 | インポートしたスクリーン上で画像エリアを定義する<br>`[x,y,width,height]`のJSON配列です。
| オフセットエリア | (省略可) 定義した画像と離れた画像エリアを指定するためのオフセット<br>`[x,y,width,height]`のJSON配列です。セットしない場合、定義した画像を取得します。

##### 出力結果
スクリーン上で見つけた画像エリアをOCRで識別した文字列。

?> 1. 現在**英語**のみがサポートしています。

#### クリップボード取得
このコマンドは実行時にクリップボードのテキスト内容を取得します。

##### 入力引数
なし

##### 出力結果
クリップボードのテキスト内容。

オペレーション引数
---

GUIオペレーションの入力引数はユーザがオペレーション定義に使った変数になります。出力引数は同じ定義に設定した出力変数になります。