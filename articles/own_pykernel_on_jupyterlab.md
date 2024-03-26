---
title: "よそのJupyterLabで自分用のpythonカーネルを使う"
emoji: "🗂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [python, JupyterLab, jupyter, ipython]
published: true
---
スーパーユーザでないサーバの `JupyterLab` で，Notebook（`.ipynb`）を開くと普通はシステム側で定義したカーネル（Python環境であるpython kernel）が利用される．

※「環境」とは `python` パスと `site-packages` パスの組み合わせ

例えば，一般ユーザの身分で，`pip なんちゃら` で新たにパッケージを導入したとしても，その導入先(ユーザの `site-packages` )には新たにパッケージが置かれるが，`jupyter` のシステムカーネルは別モノなので反映されていない．

Notebookで `import` するには `!pip なんちゃら` という導入コマンドをセルに書いてその場限りのインストールをしなくてはいけない．

いつでも使えるようにするには，自前環境のカーネルを作り，そのカーネルを利用すればよい．

JupyterLabであれば，一旦，Terminalを立ち上げ，
```
ipython kernel install --user --name=foo
```

- 実行ファイル `ipython` は，`ipykernel`パッケージで導入可能
- 上記の `foo` がカーネルに付ける呼称
- カーネルを削除したい場合は，
    ```
    jupyter kernelspec uninstall foo
    ```
- カーネルのリストは，
    ```
    jupyter kernelspec list
    ```
- 仮想環境`.venv`のカーネルを作りたい場合は，`activate` した環境下で上記のコマンドを打鍵すればよい．`deactivate`しても，作成したカーネルは利用できる．
