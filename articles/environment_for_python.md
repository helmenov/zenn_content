---
title: "Python環境およびPython製アプリ"
subtitle: "PEP668"
emoji: "😎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [python,venv,pep668]
published: true
---
これは，システム管理者と，一般ユーザの戦いである．

ユーザはシステムを使うことはできるが，いじれない．管理者のみがいじれる．

ユーザは，そんなシステムに入っているPythonのパッケージに不満がある．

- 使いたいパッケージが無い
- 使いたいパッケージのバージョンが無い

ユーザは，そんなときどうするか？管理者にパッケージを入れろと言ってくる．
パッケージの依存関係などお構いなしに．管理者としては到底受け付けられない．

ユーザは，仕方ないので自分のホームにパッケージを入れる．
`pip`は，`--user`をつければよい．ホームに`site-packages`が用意され，以降，`python`はホームの`site-packages`，システムの`site-packages`のどちらも探索する．

```{sh}
pip install --user foo
```

`foo`を入れようとすると，依存関係で芋づる式に`bar`と`hoge`も入れようとする．
`bar`はシステム側に既に存在しているが，必要なバージョンが異なるので，別の`bar`も入れる．
`hoge`もシステム側に既に存在し，こちらはそれでよいから，ホームには入れない．

あるとき`foo`のバージョンが上がって，依存関係もアップデートされた．またシステムとホームのせめぎ合いだ．

## 1. `python`$_{user}$のすゝめ：ユーザは，システムに面従腹背するか，システムに不満があるならホームで完結せよ．

ということで，システムの`site-packages`を探索しないようにすることを勧められる．
いわゆる [PEP668][PEP668] である．

### [un-recommended] `python -m pip install --user foo`

これをやめる事を薦められている．システムによっては許されない場合がある．強引にやるなら，さらに`--break-system-packages`をつける．野蛮だ．

### [recommended] `~/.local_python/bin/python -m pip install foo`

ホームの`site-packages`だけを探索するようにする最もリーズナブルな方法は，独自の`python` を使い，システム側の`python`を使わないということである．

#### case1. `python`$_{s}$ で作る仮想環境

- ホームの`python`$_{u}$ : システムの`python`$_{s}$のコピーのようなもの
- ホームの`site-packages`$_{u}$ : `python`$_{u}$ の探索範囲

```{sh}
python -m venv ~/.local_python # このpythonはシステムのもの（venvも）
export PATH="~/.local_python/bin:$PATH" # 以降，pythonはホームのもの
```

#### case2. `pyenv`や`mise`などで新たな環境

- ホームの`python`$_{u}$ : `pyenv`や`mise`などでシステムと別個に新たに用意
- ホームの`site-packages`$_{u}$ : `python`$_{u}$ の探索範囲

```{sh}
mise install python@latest
mise use -g python@latest
```

## 2. `python`$_{dev}$のすゝめ：開発環境は，それぞれ自己完結に（他の誰にも依存しない）

ユーザも，プログラミング課題に取り組む際には，開発側から見て，先のシステムVSユーザの関係に同等となる．

複数のユーザに対応する管理者が如く，
課題で必要な`site-packages`は，別の課題で必要な`site-pacages`と競合し，さらにはホームの`site-packages`ともせめぎ合う．

ということで，開発環境ごとに，それぞれ自己完結させたほうがいい．

### [un-recommended] `python -m pip install foo`

やめたほうがいい．[PEP668][PEP668]のように怒られず，すんなり`site-packages`$_u$が使われるが，あとで競合するぜ．

### [recommended] `(pkg1) python -m pip install foo`

先と同様に，ホームの`python`$_u$を使うのではなく，`python`$_{pkg1}$を使うとよい．

#### case3. `python`$_{u}$ で作る仮想環境

```{sh}
$ python -m venv pkg1 # このpythonはホームのもの
$ source pkg1/bin/activate
(pkg1) $ python -m pip install foo
```

#### case4. `poetry`や`rye`などを使う

case3よりパッケージ開発に向いている

```{sh}
poetry new pkg1
cd pkg1
poetry add foo
poetry install --sync
poetry version major/minor/patch
```

## 3.`python`$_{app}$のすゝめ：`pip install app`で入れるアプリ

インストーラである`pip`，引いてはその`pip`を持つ`python`の`site-packages`と競合する可能性がある．[PEP668][PEP668]

なので，これもアプリ独自の`python`で入れたほうがよい．

### [un-recommended] `python -m pip install app`

まぁ，やっちゃうけれど．

### [recommended] `(venv_app) python -m pip install app`

#### case5. 仮想環境を用意する．

先と同様．

```{sh}
python -m venv ~/.local/venv_app # このpythonは，システムだったりホームだったり
~/.local/venv_app/bin/python -m pip install app
export PATH="~/.local/venv_app/bin:$PATH"
```

#### case6. `pipx`を使う

venvを生で使うより，扱いやすい．

```{sh}
pipx install app
pipx ensurepath
```

ちなみに，`pipx`自体も`pip install`型．`pipx`は`venv`で入れるのがいいかもね．

## まとめ

- 普段使いも，開発でも，アプリインストールでも，独自の`python`を使う．
- それぞれに独自`python`を用意するアプリがある．
  - 普段使い：`pyenv`, `mise`
  - 開発：`poetry`, `rye`
  - アプリインストール：`pipx`

[PEP668]:https://peps.python.org/pep-0668/
