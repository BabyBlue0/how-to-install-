# visual C++ toolsetを使う
参考  
[ももいろテクノロジー「Windows x64で電卓を起動するシェルコードを書いてみる」](http://inaz2.hatenablog.com/entry/2015/07/26/175115)  
[ももいろテクノロジー「Windowsの開発環境を構築し、コンパイル・デバッグをやってみる」](http://inaz2.hatenablog.com/entry/2015/04/11/185130)  
[金子邦彦研究室「Windows 10 で Build Tools for Visual Studio 2019 のインストール」](https://www.kkaneko.jp/tools/win/buildtool.html)  
******

## 1. Visual Studio Build Toolsのインストール  
https://visualstudio.microsoft.com/ja/downloads/ から，「Visual Studio Build Tools」をダウンロード，インストール  
Visual Studio Installerで，Visual C++ Build Toolsを選択．  
右の項目で，「ビルドツールのC++/CLI サポート」，「ビルドツールのC++モジュール」，「Windows10 SDK」，「ツールのコア機能のテスト-ビルドツール」を選択しインストール  
https://developer.microsoft.com/ja-jp/windows/downloads/windows-10-sdk からwindowsSDKをダウンロード  
インストーラを実行し，インストールしたいツールを選択するウィンドウで，「Debugging Tools for Windows」を選択する．  

## 2. batファイルの作成  
記述する項目
* ツールセットの置いてあるディレクトリのPathを追加  
* 作業ディレクトリの移動  
* vcvarsallの起動  
```bat:cl86
@rem cl86.bat
@echo off

set "PATH=C:\Program Files (x86)\Windows Kits\10\Debuggers\x86;%PATH%"
cd "%HOME%\work\"
"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat"  amd64_x86
```

```bat:cl64
@rem cl64.bat
@echo off

set "PATH=C:\Program Files (x86)\Windows Kits\10\Debuggers\x64;%PATH%"
cd "%HOME%\work\"
"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat"  amd64
```

※PATHに日本語が入っているとうまく動作しないため，プロンプトと文字コードを合わせること(例：コマンドプロンプト：shift-jis，batファイル:UTF-8の場合など)  
UTF-8に統一する場合，`chcp 65001`をbatファイルの先頭に配置．  
shift-jisに統一する場合，batファイルをshift-jisで保存．  


## 3. cmdのショートカットを作成  
コンテキストメニューの新規作成からショートカットファイルを選択  
項目の場所の入力で，
`%comspec% /k %HOME%\cl86.bat`または`%comspec% /k %HOME%\cl64.bat`を入力する．  
ショートカットの名前を決める(例 x86:cl86.exe, x64:cl64.exe)  
ショートカットのプロパティを開き，詳細設定から管理者で実行にチェックを入れて適応させる．  

## 4. それぞれツールが使えるかの確認
設定したコマンドプロンプトのショートカットを開き，以下のコマンドを実行してみる．  
```check
> cl
> ml64d
> dumpbin
> cdb -version
```

***
# 追記
## 日本語を含んだソースコードをコンパイルするには
日本語を含むソースコードをコンパイルしようとするとエラーが出てしまう
```cpp:test.cpp
#include<iostream>
int main()
{
  std::cout << "こんにちは，世界！" << std::endl;
}
```
```
>cl test.cpp
Microsoft(R) C/C++ Optimizing Compiler Version 19.24.28314 for x64
Copyright (C) Microsoft Corporation.  All rights reserved.

test.cpp
test.cpp(1): warning C4819: ファイルは、現在のコード ページ (932) で表示できない文字を含んでいます。データの損失を防ぐために、ファイルを Unicode 形式で保存してください。
test.cpp(4): error C2001: 定数が 2 行目に続いています。
test.cpp(5): error C2143: 構文エラー: ';' が '}' の前にありません。
```

対処として，doskey（linuxでいうalias）で回避する方法を記述する．  
batファイルの真ん中あたりに，下のスクリプトを付け加えるだけ．  
batファイルは上から順に実行するが，実行状態を気にせず次々に実行していくため末尾に記述しても効果なし．（プロンプトが戻ってこないから？）  
真ん中あたりがヨシ！  
```
doskey cl=cl /EHsc /source-charset:utf-8 $*
```
再び実行するとコンパイルに成功した．
```
>cl test.cpp
Microsoft(R) C/C++ Optimizing Compiler Version 19.24.28314 for x64
Copyright (C) Microsoft Corporation.  All rights reserved.

test.cpp
Microsoft (R) Incremental Linker Version 14.24.28314.0
Copyright (C) Microsoft Corporation.  All rights reserved.

/out:test.exe
test.obj
```
