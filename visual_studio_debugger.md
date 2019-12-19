## visual C++ toolsetを使う
参考
http://inaz2.hatenablog.com/entry/2015/07/26/175115
******
1. batファイルの作成
ツールセットの置いてあるディレクトリのPathを追加  
作業ディレクトリの移動  
vcvarsallの起動

```x86
@rem cl86.bat
@echo off

set "PATH=C:\Program Files (x86)\Windows Kits\10\Debuggers\x86;%PATH%"
cd "%HOME%\work\"
"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat"  amd64_x86
```

```x64
@rem cl64.bat
@echo off

set "PATH=C:\Program Files (x86)\Windows Kits\10\Debuggers\x64;%PATH%"
cd "%HOME%\work\"
"C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat"  amd64
```

2. cmdのショートカットを作成
コンテキストメニューの新規作成からショートカットファイルを選択  
項目の場所の入力で，
`%comspec% /k cl86.bat`または`%comspec% /k cl64.bat`を入力する．  
ショートカットの名前を決める(x86:cl86.exe, x64:cl64.exe)  
ショートカットのプロパティを開き，詳細設定から，管理者で実行にチェックを入れて適応させる．  

