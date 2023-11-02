## 大まかな手順
pandasとyamlをインポートして、変換していく。
以下GPTで作成したコード。
```
import pandas as pd
import yaml

def to_unicode(text):
    if isinstance(text, float):
        return str(text)  # もしセルが浮動小数点数ならば文字列に変換
    return text.encode('utf-8').decode('unicode_escape')

# Excelファイルを読み込む
excel_file = 'your_excel_file.xlsx'
df = pd.read_excel(excel_file)

# データをYAMLに変換するための辞書を初期化
yaml_data = {}

# 各行をイテレートしてYAMLデータを構築
for index, row in df.iterrows():
    page_id = row['Page ID']
    yaml_data[page_id] = {
        'bg': to_unicode(row['Background']),
        'image1': to_unicode(row['Image1']),
        'image2': to_unicode(row['Image2']),
        'voice': to_unicode(row['Voice']),
        'text': to_unicode(row['Text']),
        'template': to_unicode(row['Template']),
        'history': to_unicode(row['History']),
        'next': to_unicode(row['Next Page'])
    }

# YAMLファイルにデータを書き込む
with open('output.yaml', 'w', encoding='utf-8') as yaml_file:
    yaml.dump(yaml_data, yaml_file, allow_unicode=True, default_flow_style=False)

```
## 課題
+ 文章の改行を入れるようにコードの変更と、Excelファイルの工夫が必要
+ 要素のないセルがnanで出力されてしまうので、そうならないように変更が必要
