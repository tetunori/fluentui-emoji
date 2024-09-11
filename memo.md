# memo

## 20240911

- ひとまず[Creating COLRv0 fonts in Windows 10](https://medium.com/@360.by.roderick/creating-colrv0-fonts-in-windows-10-1a88988faf10)の記事の通りに進めてみる方針
- Windows10 でお試しする様子

### Setup the environment

- Python `3.10.7`
  - `3.11.9`が入っていたので大丈夫だろう
- Perl 5.32.1
  - なかったので、[オフィシャルサイト](https://www.perl.org/get.html)へ行き Windows 版をダウンロードする。結局[Strawberry Perl](https://strawberryperl.com/)のサイトへ飛ばされるので、そこで、Windows, Recommended`Latest Release: 5.40.0.1 (2024-08-10)`を選択して`strawberry-perl-5.40.0.1-64bit.msi`を Download してインストール。
- Node 16.17.0
  - もともと`v16.17.0`が入っていたので、OK。
- GnuWin32
  [GnuWin32 のインストール（Windows 上）](https://www.kkaneko.jp/tools/win/gnuwin.html)の手順を参照。
  ただし、以下の様に一行ずつやらないとちゃんとインストールされないので、注意。
```bash
  winget install --scope machine Chocolatey.Chocolatey Chocolatey.ChocolateyGUI
```
```bash
choco install gnuwin32-coreutils.install -y
```
```bash
powershell -command "$oldpath = [System.Environment]::GetEnvironmentVariable(\"Path\", \"Machine\"); $oldpath += \"; C:\Program Files (x86)\GnuWin32\bin\"; [System.Environment]::SetEnvironmentVariable(\"Path\", $oldpath, \"Machine\")"
```

- MinGW
[MinGW - Minimalist GNU for Windows](https://sourceforge.net/projects/mingw/)
mingw-get-setup.exe
mingw32-base
mingw-developer-toolkit
mingw-gcc-g++
msys-base
を選択して入れた。
参考[【MinGW × VScode】C言語を実行するための環境構築(WSL未使用)](https://zenn.dev/12morosy/articles/2c4d608cd0a38a)
長いから注意

- FontForge 2022–03–08
  - もともと`20220308`が入っていたので、OK。

make.exe というexeはなかったので、それらしいmingw32-make.exeをコピーして、make.exeにリネームした
パスは環境変数のメニューから
✅ fontforge (ffpython.exe)
✅ python (python.exe)
✅ mingw (make.exe)
✅ perl (perl.exe)
に通した

fonttools
https://github.com/fonttools/fonttools

fonttools-main.zip c:workに展開
setup.pyのlocationは
"C:\work\fonttools-main\fonttools-main\setup.py"
となる。

win+rで開いた窓に
FontForge interactive
と入力してエンター、起動する。

Install fonttools as a fontforge module:

Download the fonttools repository as a ZIP file from its github page.
Extract the contents of the ZIP file to a local directory.
Note the location of the setup.py file.
まではOK

> On the Start menu, locate and click the fontforge interactive entry.
はあまり意味がわからない。

> Type cd <local directory> where <local directory> is the folder that contains setup.py.
**管理者権限**でコマンドプロンプトを開き、
cd C:\work\fonttools-main\fonttools-main
して、
Type ffpython setup.py install then press enter to start the installation.
ffpython setup.py install
した。
基本、以後は管理者権限でコマンドプロンプト開いた方がよさそう。


After the install process is complete, you can safely close the command prompt window.


### Update files to run on Windows
This repository already has the relevant files updated, but in case you’re coming in from another fork:

Create a working directory:

>Download this Twemoji-colr repository as a ZIP file.
twemoji-colr-for-Win10-master.zipをこぴってきた

>Extract the contents of the ZIP file to a <local directory>.
C:\work\twemoji-colr-for-Win10-master へ展開

>Note the location of the Makefile and package.json files.
"C:\work\twemoji-colr-for-Win10-master\twemoji-colr-for-Win10-master\Makefile"
"C:\work\twemoji-colr-for-Win10-master\twemoji-colr-for-Win10-master\package.json"
となる。

Update the Makefile file so that it uses our freshly-installed fontforge module instead:

>Open the Makefile file using you favorite text editor.
>Locate the PYTHON ?= python3 entry.
>Update this to PYTHON ?= ffpython.
→すでにそうなっていた。
>Save and close the file.

>"overrides": {
> "graceful-fs": "4.2.10"
> }
すでになってた。


### Test the code

[1] Open a command prompt and navigate to the location of the Twemoji working directory.

[2] To install all dependencies declared in packages.json, including grunt-webfont, type:

npm install
[3] Finally, to build the color-emoji font, type:

make


> If everything was installed correctly, the output should be saved as build/Twemoji Mozilla.ttf.
ここまで来たけど、build/Twemoji Mozilla.ttfができていない。Twemoji Mozilla.ttxしかない。
エラーメッセージのようなものが出ている。
```bash
C:\work\twemoji-colr-for-Win10-master\twemoji-colr-for-Win10-master>make
node layerize.js twe-svg.zip overrides extras build Twemoji\ Mozilla
(node:29548) Warning: Accessing non-existent property 'Reader' of module exports inside circular dependency
(Use `node --trace-warnings ...` to show where the warning was created)
(node:29548) Warning: Accessing non-existent property 'Reader' of module exports inside circular dependency
overriding 1f1fa-1f1f3.svg with local copy
found mis-named 1f441-200d-1f5e8, renamed to 1f441-fe0f-200d-1f5e8-fe0f
found mis-named keycap 23-20e3, renamed to 23-fe0f-20e3
found mis-named keycap 2a-20e3, renamed to 2a-fe0f-20e3
found mis-named keycap 30-20e3, renamed to 30-fe0f-20e3
found mis-named keycap 31-20e3, renamed to 31-fe0f-20e3
found mis-named keycap 32-20e3, renamed to 32-fe0f-20e3
found mis-named keycap 33-20e3, renamed to 33-fe0f-20e3
found mis-named keycap 34-20e3, renamed to 34-fe0f-20e3
found mis-named keycap 35-20e3, renamed to 35-fe0f-20e3
found mis-named keycap 36-20e3, renamed to 36-fe0f-20e3
found mis-named keycap 37-20e3, renamed to 37-fe0f-20e3
found mis-named keycap 38-20e3, renamed to 38-fe0f-20e3
found mis-named keycap 39-20e3, renamed to 39-fe0f-20e3
npm run grunt webfont

> twemoji-colr@0.7.0 grunt
> grunt

(node:14340) V8: C:\work\twemoji-colr-for-Win10-master\twemoji-colr-for-Win10-master\node_modules\ttf2woff2\jssrc\ttf2woff2.js:3 Invalid asm.js: Invalid member of stdlib
(Use `node --trace-warnings ...` to show where the warning was created)
Running "webfont:Twemoji" (webfont) task
>>
Font Twemoji Mozilla with 13392 glyphs created.

Done.
rm -f build/Twemoji\ Mozilla.ttf
# remove illegal <space> from the PostScript name in the font
指定されたファイルが見つかりません。
Makefile:26: recipe for target 'build/Twemoji Mozilla.ttf' failed
make: *** [build/Twemoji Mozilla.ttf] Error 1
```

ttf2woff2\jssrc\ttf2woff2.js:3 Invalid asm.js: Invalid member of stdlib
で検索すると結構でてくるので、そこをみてみるのがよさそう。
今日はここまで。


