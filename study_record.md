Title: 最初のStudy Record  
Date: 2019-03-17

昨日は *ブロックチェーン初心者の集い 1* でした！
少し昨日の模様を紹介します。

![昨日の模様1](https://res.cloudinary.com/dcim8mjwx/image/upload/v1552769035/DSC_0194_xddjh6.jpg)  
  
  
Title: ようやくポートフォリオ作成に着手  
Date: 2019/03/19

まだまだ殺風景です。
![まだ殺風景](https://res.cloudinary.com/dcim8mjwx/image/upload/v1553002371/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88_2019-03-19_22.32.20_uz9jwo.png)


Title: 完成とは程遠いが  
Date: 2019/03/20

今までの自分の実力からすれば、まだ幾分マシなものが作れたような気がします。しかし、文字が見にくいですね。
![](https://res.cloudinary.com/dcim8mjwx/image/upload/v1553069244/4b4a9db8136238e69e7b94cf773a5881_vtn7f1.png)

Title: とあるサイトのAPIを利用してスクレイピング（正確にはスクレイピングとは言わないはずですが）  
Date: 2019/03/21

もう少し手直ししていきます。

```python
import datetime
import requests
import pandas as pd
import json
import glob
import os
import time

lists = 1082386800
index  = 0
s_times = 0

for item in range(0, 100) :
    minus = lists + 60480
    url = "https://api.syosetu.com/novelapi/api/?lastup={first}-{second}&out=json&order=old&lim=500".format(first=lists, second=minus)
    req = requests.get(url).json()
    req = json.dumps(req)
    df = pd.read_json(req)
    df.to_csv('out{}.csv'.format(index), encoding='utf_8')
    lists += 60480
    index +=1
    s_times += 1
    print(s_times)
    time.sleep(1)

files = glob.glob('out*.csv')
list = []


for f in files:
    list.append(pd.read_csv(f))
    print()
    df = pd.concat(list)
    df.to_csv('data.csv', encoding='utf_8')
    os.remove(f)
```

Title: 前日の続き  
Date: 2019/03/22

引き続き手直ししていきます。

```python
import datetime
import requests
import pandas as pd
import json
import glob
import os
import time


lists = 1082386800
index  = 0
s_times = 0


for item in range(0, 10) :
    minus = lists + 604800
    url = "https://api.syosetu.com/novelapi/api/?lastup={first}-{second}&out=json&order=old&lim=500".format(first=lists, second=minus)
    req = requests.get(url).json()
    req = json.dumps(req)
    df = pd.read_json(req)
    df.to_csv('out{}.csv'.format(index), encoding='utf_8')
    lists += 604800
    index +=1
    print(item)
    time.sleep(1)

files = glob.glob('out*.csv')
list = []

for index, item in enumerate(files):
    list.append(pd.read_csv(item))
    print(index)
    df = pd.concat(list)
    df.to_csv('data.csv', encoding='utf_8')
    os.remove(item)
```

Title: 少しアプローチを変えたコードに変更  
Date: 2019/03/23

改めてAPIの仕様を見ると、もっと楽で確実な方法を見つけたのでそちらからのアプローチに変更。

```python
import datetime
import requests
import pandas as pd
import json
import glob
import os
import time
import time
from datetime import datetime

# 60 * 60 * 9 = 32400
# now = time.time()
# print(datetime.fromtimestamp(1072915200))

lists = 1104537600
index  = 0

for item in range(0, 365) :
    minus = lists + 86400
    url = "https://api.syosetu.com/novelapi/api/?firstup={first}-{second}&out=json&order=old&lim=500".format(first=lists, second=minus)
    req = requests.get(url).json()
    req = json.dumps(req)
    df = pd.read_json(req)
    df.to_csv('out{}.csv'.format(index), encoding='utf_8')
    print(datetime.fromtimestamp(lists))
    lists += 86400
    index +=1
    print(lists)
    print(item)
    time.sleep(1)

files = glob.glob('out*.csv')
list = []

for index, item in enumerate(files):
    list.append(pd.read_csv(item))
    print(index)
    df = pd.concat(list)
    df.to_csv('data.csv', encoding='utf_8')
    os.remove(item)
```

Title: ほぼコード修正は終了  
Date: 2019/03/29

細かい修正を施して、これにてこのコーディングは終了。

```python
import datetime
import requests
import pandas as pd
import json
import glob
import os
import time
import time
from datetime import datetime

# 60 * 60 * 9 = 32400

lists = 1104537600
index  = 0

for item in range(0, 365) :
    minus = lists + 86400
    url = "https://api.syosetu.com/novelapi/api/?firstup={first}-{second}&out=json&order=old&lim=500".format(first=lists, second=minus)
    req = requests.get(url).json()
    req = json.dumps(req)
    df = pd.read_json(req)
    df.to_csv('out{}.csv'.format(index), encoding='utf_8')
    print(datetime.fromtimestamp(lists))
    lists += 86400
    index +=1
    print(lists)
    print(item)
    time.sleep(1)

files = glob.glob('out*.csv')
list = []

for index, item in enumerate(files):
    list.append(pd.read_csv(item))
    print(index)
    df = pd.concat(list)
    df.to_csv('data.csv', encoding='utf_8')
    os.remove(item)
```

Title: 本格的にDjangoプロジェクト作成開始  
Date: 2019/04/01

本格的にDjangoプロジェクトの作成をしました。プロジェクト名はgusiqとしておきました。ドメインはgusiq.gqの予定。ひとまず、リポジトリのみ貼っておきます。  
https://github.com/anvinon/gusiq