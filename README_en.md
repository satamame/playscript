# playscript

[> 日本語版](https://github.com/satamame/playscript/blob/master/README.md)

## Overview

Package for play script structure in Japanese style.

## Install

When installing from PyPi, you need to install fountain in addition.

```
> pip install playscript
> pip install git+https://github.com/Tagirijus/fountain.git@7da5447abae640f34448dd36fee83f47a7415fcf
```

When installing from GitHub, below command is enough.

```
> pip install git+https://github.com/satamame/playscript.git
```

## Classes

### PSc

Play script containing lines and meta data.

### PScLine

Each line of lines contained in PSc object.  
It has an attribute "type" to determine if it's a dialogue or a direction, etc.

### PScLineType

This enum defines types for PScLine.

## Example

### Creating script from line definitions

```python
from playscript import PScLineType, PScLine, PSc

title = PScLine.from_text(PScLineType.TITLE, 'ろくでなしの冒険')
h1 = PScLine.from_text(PScLineType.H1, 'シーン１')
direction = PScLine.from_text(PScLineType.DIRECTION, '六郎と七郎、登場。')
dialogue1 = PScLine.from_text(PScLineType.DIALOGUE, '六郎「どうする？」')
dialogue2 = PScLine.from_text(PScLineType.DIALOGUE, '七郎「帰って寝る」')
endmark = PScLine.from_text(PScLineType.ENDMARK, 'おわり')

script = PSc(
    lines=[
        title,
        h1,
        direction,
        dialogue1,
        dialogue2,
        endmark,
    ]
)
```

### Creating script from fountain (Japanese style)

```python
import textwrap
from playscript.conv.fountain import psc_from_fountain

fountain_str = textwrap.dedent('''\
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
''')

script = psc_from_fountain(fountain_str)
```

### Creating PDF from fountain (Japanese style) file

```python
from playscript.conv import fountain, pdf

with open('example.fountain', encoding='utf-8-sig') as f:
    script = fountain.psc_from_fountain(f.read())

pdf_stream = pdf.psc_to_pdf(script)

with open('out.pdf', 'wb') as f:
    f.write(pdf_stream.read())
```
