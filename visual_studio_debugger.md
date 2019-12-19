# visual C++ toolsetを使う
参考  
[ももいろテクノロジー「Windows x64で電卓を起動するシェルコードを書いてみる」](http://inaz2.hatenablog.com/entry/2015/07/26/175115)  
[ももいろテクノロジー「Windowsの開発環境を構築し、コンパイル・デバッグをやってみる」](http://inaz2.hatenablog.com/entry/2015/04/11/185130)  
[金子邦彦研究室「Windows 10 で Build Tools for Visual Studio 2019 のインストール」](https://www.kkaneko.jp/tools/win/buildtool.html)  
******
1. Visual Studio Build Toolsのインストール  
> https://visualstudio.microsoft.com/ja/downloads/ から，「Visual Studio Build Tools」をダウンロード，インストール  
> Visual Studio Installerで，Visual C++ Build Toolsを選択．  
> 右の項目で，「ビルドツールのC++/CLI サポート」，「ビルドツールのC++モジュール」，「Windows10 SDK」，「ツールのコア機能のテスト-ビルドツール」を選択しインストール  
> https://developer.microsoft.com/ja-jp/windows/downloads/windows-10-sdk からwindowsSDKをダウンロード  
> インストーラを実行し，インストールしたいツールを選択するウィンドウで，「Debugging Tools for Windows」を選択する．  

2. batファイルの作成  
> ツールセットの置いてあるディレクトリのPathを追加  
> 作業ディレクトリの移動  
> vcvarsallの起動  
> 
> ※PATHに日本語が入っていると，うまく動作しないため，プロンプトと文字コードを合わせること(コマンドプロンプト：shift-jis，batファイル:UTF-8)  
>> UTF-8に統一する場合，`chcp 65001`をbatファイルの先頭に配置．  
>> shift-jisに統一する場合，batファイルをshift-jisで保存．  
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

3. cmdのショートカットを作成  
> コンテキストメニューの新規作成からショートカットファイルを選択  
> 項目の場所の入力で，
> `%comspec% /k %HOME%\cl86.bat`または`%comspec% /k %HOME%\cl64.bat`を入力する．  
> ショートカットの名前を決める(x86:cl86.exe, x64:cl64.exe)  
> ショートカットのプロパティを開き，詳細設定から，管理者で実行にチェックを入れて適応させる．  

4. それぞれツールが使えるかの確認
```check
> cl
> ml64d
> dumpbin
> cdb -version
```
