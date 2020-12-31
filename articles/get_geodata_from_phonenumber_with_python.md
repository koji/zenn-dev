---
title: "pythonで電話番号から情報を取得する方法"
emoji: "☎️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["tech", "python"]
published: true
---

pypi phonenumbers
https://github.com/daviddrysdale/python-phonenumbers

```
!pip install phonenumbers
```

## ロケーション

"en"は"ja"にすると日本語で表示されます。

```python
import phonenumbers
from phonenumbers import geocoder

number="+11111111111" # with country code +1xxxyyyzzzz
ch_number = phonenumbers.parse(number, "CH")
print(geocoder.description_for_number(ch_number, "en"))
```

## キャリア

"en"は"ja"にすると日本語で表示されます。

```python
from phonenumbers import carrier
service_number = phonenumbers.parse(number, "RO")
print(carrier.name_for_number(service_number, "en"))
```

Verizon と出るかなぁと思ったら、なぜか、自分の番号でやったら出ませんでした。。。

## タイムゾーン

"en"は"ja"にすると日本語で表示されます。

```python
from phonenumbers import timezone
gb_number = phonenumbers.parse(number, "en")
timezone.time_zones_for_number(gb_number)
```
