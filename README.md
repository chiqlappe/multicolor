# マルチカラーボード仕様書

## 概要

- PC-8001で160x200ドット４色表示を可能にする拡張ボード
- プリンタポートでCRT信号を制御する

## 定義

ドット
- PC-8001で表示可能な最小単位

ユニット
- マルチカラーボードで表示可能な最小単位
- 1ユニットは4ドット


## 動作の仕組み

- カラーCRTインターフェースのB信号(青色)を2ドット間隔で取得し、それを2つ組み合わせたものを1ユニット分の色情報とする
- 2ビットの色情報をユーザー定義されたパレット情報と照合して4色分のRGB情報が作成され、それぞれ色信号としてモニタへ出力される
- 1ユニットあたり4ドットを使用するので横方向の解像度は1/4となる
- 縦方向の解像度は変化しない。

## I/Oポート

- プリンタポート($10)を使用する。

## 各ビットの用途

|BIT|用途|メモ|
|:--:|:--|:--|
|0~2|カラーコード||
|3,4|パレット番号||
|5|パレット登録ビット(H->L->Hで登録)|スイッチを操作するときはHにしておく|
|6|N/A||
|7|スイッチ(Hで有効)||

## 設定例

### パレット定義
パレット#1に赤を登録する

```
C = 2 ' color code
P = 1 ' palette #

OUT &H10, C + P * 2 ^ 3 + 2 ^ 5  ' BIT5=hi
OUT &H10, C + P * 2 ^ 3          ' BIT5=low
OUT &H10, C + P * 2 ^ 3 + 2 ^ 5  ' BIT5=hi
```
### マルチカラースイッチ
有効にする

```
OUT &H10, 2 ^ 7 + 2 ^ 5
```

無効にする

```
OUT &H10, 2 ^ 5
```


    
