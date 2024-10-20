# マルチカラーボード説明書

![画面イメージ](https://github.com/chiqlappe/multicolor/blob/main/image/0704a.jpg)

## 概要

- PC-8001で160x200ドット４色表示を可能にする拡張ボード
- プリンタポートでCRT信号を制御する
- PCGが必要

## 定義

ドット
- PC-8001で表示可能な最小単位

ユニット
- マルチカラーボードで表示可能な最小単位
- 1ユニットは4ドット


## 動作の仕組み

- カラーCRTインターフェースのB信号(青色)を2ドット間隔で取得し、それを2つ組み合わせたものを1ユニット分の色情報とする
- ユーザー定義されたパレット情報と2ビットの色情報を照合して4色のRGB信号が出力される
- 1ユニットあたり4ドットを使用するので横方向の解像度は1/4となる
- 縦方向の解像度は変化しない。

![ボード本体](https://github.com/chiqlappe/multicolor/blob/main/image/IMG_2671.JPG)

## I/Oポート

- プリンタポート($10)を使用する

## 各ビットの用途

|BIT|用途|メモ|
|:--:|:--|:--|
|0~2|カラーコード||
|3,4|パレット番号||
|5|パレット登録ビット(H->L->Hで登録)|パレットを操作しないときはHにしておく|
|6|未使用||
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

## デモプログラム

[plasma.wav](https://github.com/chiqlappe/multicolor/blob/main/plasma.wav) 
jblang氏の[plasma.asm](https://github.com/jblang/TMS9918A/blob/master/examples/plasma.asm)をPC-8001用に移植したもの






    
