---
title: "Pythonの管理"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [python]
published: false
---
## Pythonインタプリタの起動

||新規インタプリタ|既存インタプリタ|ランチャー|
|---|---|---|---|
|python-launcher[[0w][python-launcher for windows],[0u][python-launcher for unix]]|✕|N |direct (py)|
|pyenv[[1][pyenv]]|N (@ver)|1 (@system)|sim-path|
|mise[[2][mise]]|N (@ver)|1 (@system)|direct|

python-launcherは，PATH上の既存のインタプリタに対して，`py`で起動する．引数にversionをつけると，version番号の付いたpythonインタプリタを起動する．
(例: `py -3.10` は，`python3.10`を起動する．)

pyenv,miseは，PATH上で先にある`python`を `system`というversionとみなす．(`python`という名前でないといけない)

## 仮想環境

仮想環境とは，独立なsite-packagesの確保である．

そのために，
- 既存のインタプリタを直接起動するのではなく，新たな環境にリンクを置くか，コピーを置く．そのダミーpythonが，VENV/bin/pythonだとするとsite-packagesは，VENV/lib/pythonX.Y/site-packages/となる．
- 既存のインタプリタのsite-packagesを除外する．





[python-launcher for unix]:https://python-launcher.app/
[python-launcher for windows]:https://docs.python.org/3/using/windows.html#launcher
[pyenv]:https://github.com/pyenv/pyenv
[mise]:https://mise.jdx.dev/


