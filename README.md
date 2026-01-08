# Python에서 JSON 데이터 파싱하기

[![Promo](https://github.com/bright-kr/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.co.kr/) 

이 가이드는 Python의 `json` 모듈로 JSON 데이터를 파싱하고 이를 Python 딕셔너리로 변환하는 방법 및 그 반대 변환을 다룹니다.

- [Python에서 JSON 소개](#an-introduction-to-json-in-python)
- [Python으로 JSON 데이터 파싱하기](#parsing-json-data-with-python-1)
- [JSON 문자열을 Python 딕셔너리로 변환하기](#converting-a-json-string-to-a-python-dictionary)
  - [JSON API 응답을 Python 딕셔너리로 변환하기](#transforming-a-json-api-response-into-a-python-dictionary)
  - [JSON 파일을 Python 딕셔너리로 로드하기](#loading-a-json-file-into-a-python-dictionary)
  - [JSON 데이터에서 사용자 정의 Python 객체로](#from-json-data-to-custom-python-object)
- [Python 데이터를 JSON으로 변환하기](#python-data-to-json)
- [`json` 표준 모듈의 한계](#limitations-of-the-json-standard-module)
- [결론](#conclusion)

## Python에서 JSON 소개

JavaScript Object Notation, 즉 JSON은 API를 통해 서버와 웹 애플리케이션 간에 데이터를 전송하는 데 일반적으로 사용되는 가벼운 데이터 교환 형식입니다. JSON 데이터는 키-값 쌍으로 구성되며, 각 키는 문자열이고 각 값은 문자열, 숫자, 불리언, null, 배열 또는 객체가 될 수 있습니다.

다음은 JSON 예시입니다:

```json
{
  "name": "Maria Smith",
  "age": 32,
  "isMarried": true,
  "hobbies": ["reading", "jogging"],
  "address": {
    "street": "123 Main St",
    "city": "San Francisco",
    "state": "CA",
    "zip": "12345"
  },
  "phoneNumbers": [
    {
      "type": "home",
      "number": "555-555-1234"
    },
    {
      "type": "work",
      "number": "555-555-5678"
    }
  ],
  "notes": null
}
```

Python은 [Python Standard Library](https://docs.python.org/3/library/index.html)의 일부인 `json` 모듈을 통해 [JSON](https://docs.python.org/3/library/json.html)을 기본적으로 지원합니다. 즉, Python에서 JSON을 다루기 위해 추가 라이브러리를 설치할 필요가 없습니다. `json`은 다음과 같이 import할 수 있습니다:

```python
import json
```

내장 Python `json` 라이브러리는 JSON을 처리하기 위한 완전한 API를 제공합니다. 특히 두 가지 핵심 함수인 [`loads`](https://docs.python.org/3/library/json.html#json.loads)와 [`load`](https://docs.python.org/3/library/json.html#json.load)를 제공합니다. `loads` 함수는 문자열에서 JSON 데이터를 파싱하기 위한 것이고, `load` 함수는 바이트로 JSON 데이터를 파싱하기 위한 것입니다.

이 두 메서드를 통해 `json`은 JSON 데이터를 [dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) 및 [lists](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists) 같은 동등한 Python 객체로 변환할 수 있으며, 그 반대 변환도 가능합니다. 또한 `json` 모듈은 특정 데이터 타입을 처리하기 위한 사용자 정의 인코더 및 디코더를 만들 수 있게 해줍니다.

## Python으로 JSON 데이터 파싱하기

## JSON 문자열을 Python 딕셔너리로 변환하기

문자열에 저장된 JSON 데이터가 있고 이를 Python 딕셔너리로 변환하려고 한다고 가정하겠습니다. JSON 데이터는 다음과 같습니다:

```json
{
  "name": "iPear 23",
  "colors": ["black", "white", "red", "blue"],
  "price": 999.99,
  "inStock": true
}
```

그리고 이것의 Python 문자열 표현은 다음과 같습니다:

```python
smartphone_json = '{"name": "iPear 23", "colors": ["black", "white", "red", "blue"], "price": 999.99, "inStock": true}'
```

> **Note**\
> 길고 여러 줄로 구성된 JSON 문자열을 저장할 때는 Python의 삼중 따옴표(triple quotes) 관례를 사용하는 것을 고려하시기 바랍니다.

아래 코드로 `smartphone`이 유효한 Python 문자열을 포함하는지 확인할 수 있습니다:

```python
print(type(smartphone))
```

그러면 다음이 출력됩니다:

```
<class 'str'>
```

[`str`](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)은 “string”을 의미하며, smartphone 변수가 텍스트 시퀀스 타입임을 뜻합니다.

다음과 같이 json.loads() 메서드로 smartphone에 포함된 JSON 문자열을 Python 딕셔너리로 파싱합니다:

```python
import json

# JSON string
smartphone_json = '{"name": "iPear 23", "colors": ["black", "white", "red", "blue"], "price": 999.99, "inStock": true}'
# from JSON string to Python dict
smartphone_dict = json.loads(smartphone_json)

# verify the type of the resulting variable
print(type(smartphone_dict)) # dict
```

이 스니펫을 실행하면 다음을 얻습니다:

```
<class 'dict'>
```

이제 `smartphone_dict`는 유효한 Python 딕셔너리를 포함합니다.

다음으로, 유효한 JSON 문자열을 `json.loads()`에 전달하여 JSON 문자열을 Python 딕셔너리로 변환합니다.

이제 결과 딕셔너리의 필드에 평소와 같이 접근할 수 있습니다:

```python
product = smartphone_dict['name'] # smartphone
priced = smartphone['price'] # 999.99
colors = smartphone['colors'] # ['black', 'white', 'red', 'blue']
```

`json.loads()` 함수가 항상 딕셔너리를 반환하는 것은 아닙니다. 구체적으로 반환되는 데이터 타입은 입력 문자열에 따라 달라집니다. 예를 들어 JSON 문자열이 단일(평평한) 값을 포함하는 경우, 동등한 Python 기본(primitive) 값으로 변환됩니다:

```python
import json
 
json_string = '15.5'
float_var = json.loads(json_string)

print(type(float_var)) # <class 'float'>
```

마찬가지로 배열 리스트를 포함하는 JSON 문자열은 Python 리스트가 됩니다:

```python
import json
 
json_string = '[1, 2, 3]'
list_var = json.loads(json_string)
print(json_string) # <class 'list'>
```

아래 [conversion table](https://docs.python.org/3/library/json.html#json-to-py-table)은 `json`이 JSON 값을 Python 데이터로 변환하는 방식을 설명합니다:

| **JSON Value** | **Python Data** |
| - | - | - |
| `string` | `str` |
| `number (integer)` | `int` |
| `number (real)` | `float` |
| `true` | `True` |
| `false` | `False` |
| `null` | `None` |
| `array` | `list` |
| `object` | `dict` |

### JSON API 응답을 Python 딕셔너리로 변환하기

API를 호출하고 그 JSON 응답을 Python 딕셔너리로 변환해야 한다고 가정하겠습니다. 아래 예시에서는 가짜 JSON 데이터를 얻기 위해 [{JSON} Placeholder](https://jsonplaceholder.typicode.com/) 프로젝트의 다음 API endpoint를 호출하겠습니다:

```
https://jsonplaceholder.typicode.com/todos/1
```

이 RESTFul API는 아래 JSON response를 반환합니다:

```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

Standard Library의 [`urllib`](https://docs.python.org/3/library/urllib.html) 모듈로 해당 API를 호출하고, 결과 JSON을 Python 딕셔너리로 변환할 수 있습니다:

```python
import urllib.request
import json

url = "https://jsonplaceholder.typicode.com/todos/1"

with urllib.request.urlopen(url) as response:
     body_json = response.read()

body_dict = json.loads(body_json)
user_id = body_dict['userId'] # 1
```

[`urllib.request.urlopen()`](https://docs.python.org/3/library/urllib.request.html#urllib.request.urlopen)은 API 호출을 수행하고 [`HTTPResponse`](https://docs.python.org/3/library/http.client.html#http.client.HTTPResponse) 객체를 반환합니다. 그런 다음 [`read()`](https://docs.python.org/3/library/http.client.html#http.client.HTTPResponse.read) 메서드를 사용해 response body인 body\_json을 가져오며, 여기에는 API response가 JSON 문자열로 포함됩니다. 마지막으로 앞서 설명한 대로 `json.loads()`를 통해 해당 문자열을 Python 딕셔너리로 파싱할 수 있습니다.

마찬가지로 [`requests:`](https://requests.readthedocs.io/en/latest/)로도 동일한 결과를 얻을 수 있습니다.

```python
import requests
import json

url = "https://jsonplaceholder.typicode.com/todos/1"
response = requests.get(url)

body_dict = response.json()
user_id = body_dict['userId'] # 1
```

> **Note**\
> [`.json()`](https://requests.readthedocs.io/en/latest/user/quickstart/?highlight=json#json-response-content) 메서드는 JSON 데이터를 포함한 response object를 해당하는 Python 데이터 구조로 자동 변환합니다.

### JSON 파일을 Python 딕셔너리로 로드하기

아래와 같이 `smartphone.json` 파일에 저장된 JSON 데이터가 있다고 가정하겠습니다:

```json
{
  "name": "iPear 23",
  "colors": ["black", "white", "red", "blue"],
  "price": 999.99,
  "inStock": true,
  "dimensions": {
    "width": 2.82,
    "height": 5.78,
    "depth": 0.30
  },
  "features": [
    "5G",
    "HD display",
    "Dual camera"
  ]
}
```

목표는 JSON 파일을 읽어서 Python 딕셔너리로 로드하는 것입니다. 아래 스니펫으로 이를 수행합니다:

```python
import json

with open('smartphone.json') as file:
  smartphone_dict = json.load(file)

print(type(smartphone_dict)) # <class 'dict'>
features = smartphone_dict['features'] # ['5G', 'HD display', 'Dual camera']
```

내장 [`open()`](https://docs.python.org/3/library/functions.html#open) 라이브러리는 파일을 로드하고 해당 [file object](https://docs.python.org/3/glossary.html#term-file-object)를 얻을 수 있게 해줍니다. 그런 다음 `json.read()` 메서드는 JSON 문서를 포함한 [text file](https://docs.python.org/3/glossary.html#term-text-file) 또는 [binary file](https://docs.python.org/3/glossary.html#term-binary-file)을 동등한 Python 객체로 역직렬화(deserialize)합니다. 이 경우 `smartphone.json`은 Python 딕셔너리가 됩니다.

### JSON 데이터에서 사용자 정의 Python 객체로

이제 JSON 데이터를 사용자 정의 Python 클래스로 파싱해 보겠습니다. 사용자 정의 `Smartphone` Python 클래스는 다음과 같습니다:

```python
class Smartphone:
    def __init__(self, name, colors, price, in_stock):
        self.name = name    
        self.colors = colors
        self.price = price
        self.in_stock = in_stock
```

여기서 목표는 다음 JSON 문자열을 `Smartphone` 인스턴스로 변환하는 것입니다:

```json
{
  "name": "iPear 23 Plus",
  "colors": ["black", "white", "gold"],
  "price": 1299.99,
  "inStock": false
}
```

이 작업을 수행하기 위해 사용자 정의 디코더를 생성합니다. 이를 위해 [`JSONDecoder`](https://docs.python.org/3/library/json.html#json.JSONDecoder) 클래스를 확장하고 `__init__` 메서드에서 `object_hook` 파라미터를 설정합니다. 사용자 정의 파싱 로직을 포함하는 클래스 메서드의 이름을 할당합니다. 해당 파싱 메서드에서는 `json.read()`가 반환하는 표준 딕셔너리에 들어 있는 값을 사용하여 `Smartphone` 객체를 인스턴스화할 수 있습니다.

아래와 같이 사용자 정의 `SmartphoneDecoder`를 정의합니다:

```python
import json
 
class SmartphoneDecoder(json.JSONDecoder):
    def __init__(self, object_hook=None, *args, **kwargs):
        # set the custom object_hook method
        super().__init__(object_hook=self.object_hook, *args, **kwargs)

    # class method containing the 
    # custom parsing logic
    def object_hook(self, json_dict):
        new_smartphone = Smartphone(
            json_dict.get('name'), 
            json_dict.get('colors'), 
            json_dict.get('price'),
            json_dict.get('inStock'),            
        )

        return new_smartphone
```

사용자 정의 `object_hook()` 메서드 내에서 딕셔너리 값을 읽기 위해 `get()` 메서드를 사용합니다. 이렇게 하면 딕셔너리에 키가 누락되어도 `KeyError`가 발생하지 않습니다. 대신 `None` 값이 반환됩니다.

이제 `json.loads()`의 `cls` 파라미터에 `SmartphoneDecoder` 클래스를 전달하여 JSON 문자열을 `Smartphone` 객체로 변환합니다:

```python
import json

# class Smartphone:
# ...

# class SmartphoneDecoder(json.JSONDecoder): 
# ...

smartphone_json = '{"name": "iPear 23 Plus", "colors": ["black", "white", "gold"], "price": 1299.99, "inStock": false}'

smartphone = json.loads(smartphone_json, cls=SmartphoneDecoder)
print(type(smartphone)) # <class '__main__.Smartphone'>
name = smartphone.name # iPear 23 Plus
```

마찬가지로 `json.load()`에서도 `SmartphoneDecoder`를 사용할 수 있습니다:

```
smartphone = json.load(smartphone_json_file, cls=SmartphoneDecoder)
```

## Python 데이터를 JSON으로 변환하기

반대로 Python 데이터 구조 및 기본 타입을 JSON으로 변환할 수도 있습니다. 이는 [`json.dump()`](https://docs.python.org/3/library/json.html#json.dump) 및 [`json.dumps()`](https://docs.python.org/3/library/json.html#json.dumps) 함수 덕분에 가능하며, 아래 [conversion table](https://docs.python.org/3/library/json.html#py-to-json-table)을 따릅니다:

| **Python Data** | **JSON Value** |
| - | - | - |
| `str` | `string` |
| `int` | `number (integer)` |
| `float` | `number (real)` |
| `True` | `true` |
| `False` | `false` |
| `None` | `null` |
| `list` | `array` |
| `dict` | `object` |
| `Null` | None  |

`json.dump()`는 다음 예시처럼 JSON 문자열을 파일에 쓸 수 있게 해줍니다:

```python
import json

user_dict = {
    "name": "John",
    "surname": "Williams",
    "age": 48,
    "city": "New York"
}

# serializing the sample dictionary to a JSON file
with open("user.json", "w") as json_file:
    json.dump(user_dict, json_file)
```

이 스니펫은 Python `user_dict` 변수를 `user.json` 파일로 직렬화(serialize)합니다.

마찬가지로 `json.dumps()`는 Python 변수를 동등한 JSON 문자열로 변환합니다:

```python
import json

user_dict = {
    "name": "John",
    "surname": "Williams",
    "age": 48,
    "city": "New York"
}

user_json_string = json.dumps(user_dict)

print(user_json_string)
```

이 스니펫을 실행하면 다음을 얻습니다:

```json
{"name": "John", "surname": "Williams", "age": 48, "city": "New York"}
```

> **Note**\
> 사용자 정의 인코더를 지정하는 방법은 [official documentation](https://docs.python.org/3/library/json.html#json.JSONEncoder)을 참고하시기 바랍니다.

### `json` 표준 모듈의 한계

JSON [data parsing](https://brightdata.co.kr/blog/web-data/what-is-data-parsing)에는 간과할 수 없는 과제가 따릅니다.

흔한 예시 두 가지는 다음과 같습니다:

- Python `json` 모듈은 유효하지 않거나 손상되었거나 비표준 JSON의 경우 한계가 있습니다.
- 신뢰할 수 없는 소스의 JSON 데이터를 파싱하는 것은 위험합니다. 악의적인 JSON 문자열이 파서를 중단시키거나 많은 리소스를 소비하게 만들 수 있기 때문입니다.

이러한 한계는 우회할 수 있지만, [Web Scraper API](https://brightdata.co.kr/products/web-scraper)와 같이 JSON 파싱을 더 쉽게 만들어주는 상용 도구를 사용하는 것이 가장 좋습니다.

## 결론

Python에서 `json` 표준 모듈로 JSON 데이터를 기본 방식으로 파싱하는 경우, 웹사이트가 부과하는 제한을 우회하기 위해 신뢰할 수 있는 プロキシ 서버가 필요합니다. Bright Data의 데이터 및 [proxy products](https://brightdata.co.kr/proxy-types)와 같은 최첨단의 모든 기능을 갖춘 상용 데이터 파싱 솔루션을 사용해 보시기 바랍니다.