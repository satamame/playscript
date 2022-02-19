Fountain (日本式)
=================

| Fountain (日本式) は、playscript で扱えるデータ形式のひとつです。
| `本家の Fountain <https://fountain.io/>`_ を基に、日本語の台本を書くためのローカルルールを取り入れた記法です。
| Markdown のように可読性を保ちつつ、構造を表現できるテキスト形式のデータです。

例
--

Fountain (日本式) のソース

.. code-block:: founatin

    Title: ろくでなしの冒険
    Author: アラン・スミシ

    # 登場人物

    六郎
    七郎

    # シーン１

    六郎と七郎、登場。

    @六郎
    どうする？

    @七郎
    帰って寝る

    > おわり

:py:meth:`playscript.conv.pdf.psc_to_pdf` で PDF にしたところ

.. image:: /images/fountain_pdf.png
    :class: bordered-image

行の種類と書き方
----------------

題名と著者名
^^^^^^^^^^^^

.. code-block:: founatin

    Title: ろくでなしの冒険
    Author: アラン・スミシ

| 台本の先頭に書くようにします。
| コロンの後のスペースは必須ではありません。
| 題名と著者名のどちらか、または両方を省略することができます。
| 本家の Fountain と同じ書き方ですが、`Title Page <https://fountain.io/syntax#section-titlepage>`_ という概念はありません。
| これ以降は基本的に、行の種類が変わる時に空行を挟む必要があります (例外あり)。

登場人物見出し行
^^^^^^^^^^^^^^^^

.. code-block:: founatin

    # 登場人物

| '# 登場人物' と書きます。
| この書き方は本家の Fountain では `Section <https://fountain.io/syntax#section-sections>`_ を表しますが、日本式では '# 登場人物' と書いた場合のみ「登場人物見出し行」として扱うことにします。
| 本家の Fountain の Section はレンダリングされませんが、「登場人物見出し行」はレンダリング対象とします。

登場人物行
^^^^^^^^^^

.. code-block:: founatin

    # 登場人物

    六郎
    七郎

| 登場人物見出し行のあと、空行に続く行が登場人物行となります。
| この書き方は本家の Fountain では `Action <https://fountain.io/syntax#section-action>`_ (ト書き) を表します。
| 日本式ではト書きとの違いとして、人物説明や配役を追記する書き方を定義します。
| 追記のための区切り文字は ": " (半角コロンと半角スペース)、または単に半角スペースを使います。

.. code-block:: founatin

    六郎: 主人公
    七郎: 六郎の弟

柱
^^

.. code-block:: founatin

    # シーン１

| 本家の Fountain には `Scene Heading <https://fountain.io/syntax#section-slug>`_ がありますが、日本語の台本の柱とはだいぶ違うので、日本式では Section と同じ書き方にします。
| Section が階層化できることを利用して、Fountain (日本式) ではレベル3までの柱を定義します。

.. code-block:: founatin

    # 第一幕

    ## シーン１

    ### シーン１- A

ト書き行
^^^^^^^^

.. code-block:: founatin

    六郎と七郎、登場。

| 特に記号など付けずに書いた行は本家の Fountain ではト書き (`Action <https://fountain.io/syntax#section-action>`_) になり、日本式もそれに倣います。

セリフ行
^^^^^^^^

.. code-block:: founatin

    @六郎
    どうする？

| セリフ行の書き方は本家の Fountain と同じです。
| セリフ行は、名前の行 (本家では `Character <https://fountain.io/syntax#section-character>`_) とセリフの行 (本家では `Dialogue <https://fountain.io/syntax#section-dialogue>`_) から成ります。
| 空行の後に "@" (アットマーク) で始まる行を書くと、名前の行になります。
| 名前の行の次の行から、空行が現れるまでがセリフの行です。

エンドマーク
^^^^^^^^^^^^

.. code-block:: founatin

    > おわり

| ">" で始まり、"<" で終わらない行をエンドマークとします。
| 本家の Fountain では `Transition <https://fountain.io/syntax#section-trans>`_ として扱われる書き方ですが、日本語では Transition の概念はない (ト書きで済ませる) ので、これを使って右寄せにします。
