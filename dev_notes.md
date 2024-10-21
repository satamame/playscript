# 開発メモ

## VSCode の設定

- "python.defaultInterpreterPath": ".venv\\Scripts\\python.exe"

## 仮想環境

- テスト実行時、ドキュメント生成時などは .venv に入ること。
- .venv には requirements_dev.txt で依存関係をインストールする。
- (パッケージ自体の依存関係は setup.cfg に記載している。)
- requirements_tox.txt は、tox を使ったテストで参照される。

## ビルド

- ※グローバル環境に build がインストールされていること。
- ※必要なら setup.cfg のバージョンを変える。

```
> py -m build .
```

## テスト

### パッケージとしてテスト

ビルドしてから仮想環境で以下を実行する。

```
(.venv) > pip install .
(.venv) > python -m unittest -v {tests.test_module_name{.class_name{.method_name}}}
```

### ソースコードとしてテスト

(テストコードをデバッグ実行する時など)

```
(.venv) > pip install -e .
(.venv) > python -m unittest -v {tests.test_module_name{.class_name{.method_name}}}
```

### 複数バージョンの Python でテスト

※グローバル環境に tox がインストールされていること。

```
> del .tox
> tox
```

## 公開

- setup.cfg を見直す (特にバージョン)。
- ※TestPyPI/PyPI にアップロードする時は、setup.cfg の install_requires から、fountain を削除して build する。
- ※GitHub に上げる時に setup.cfg を元に戻す。
- ※twine で以下のコマンドでアップロードする場合、ユーザディレクトリに .pypirc が必要。

upload to TestPyPI:
```
> twine upload --repository testpypi dist/*
```

upload to PyPI:
```
> twine upload --repository pypi dist/*
```

## リリース

- ※GitHub 上でリリースする前に、そのバージョンのドキュメントを作る必要がある。
- (実際のバージョン名とドキュメントに表示するバージョン名を合わせるため。)
- ※或は、「ドキュメントに表示するのはマイナーバージョンまで」等として後から作れるようにしても良い。

### 手順

1. docstring から rst を生成する (maxdepth=1)。
    ```
    (.venv) > sphinx-apidoc -f -o docs/source src -d 1
    ```
    - 以下の .rst ファイルが出来る (上書きされる)
        - modules.rst
        - playscript.rst
        - playscript.conv.rst
1. コミットしてタグをつける。
1. markdown/rst から html を生成する。
    1. まず、conf.py でバージョンを設定しなおす。
    1. 必要なら smv_tag_whitelist も。
    1. マルチバージョンの場合 (タグつきのコミットから生成)
        ```
        (.venv) > sphinx-multiversion docs/source docs/build
        ```
    1. 単品の場合
        ```
        (.venv) > sphinx-build -b html docs/source docs/build
        ```
1. "docs/build" 以下を "playscript_pages" へコピーする。
1. gh-pages ブランチへプッシュする。
