# Zenn CLI

* [📘 How to use](https://zenn.dev/zenn/articles/zenn-cli-guide)

## 記事の作成

`npx zenn new:article --slug foo`

すると，`articles/foo.md` が作成される

ヘッダにタイトルとかを書く

## preview

`npx zenn preview`

localhostで見れる

## draft 

ヘッダの`published`をfalseのままで，commit,pushすると下書き保存される．

## publish

ヘッダの`published`をtrueにしてcommit,pushすると同期(deploy)される


https://zenn.dev/**/articles/foo


