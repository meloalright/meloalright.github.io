---
title: 腾讯安全极客技术挑战赛 - 第七题解决思路
date: 2020-07-11
---

> 来自腾讯安全极客技术挑战赛题目 2020-06-23
> 原链接: [一道即将尘封十几年的封印，不来尝试解开吗？ - 我在鹅厂做安全](https://mp.weixin.qq.com/s/tZ9BmXfzGYpzrNm2Jl5Mrw)

> 第七题思路: 直接撞 `Decrypt(key:str, text:str)` 逻辑 暴力破解出 key 也就是 8 位 PIN 码


## 先打印第七题题干

```python
def Hash(context):
	result = md5(bytes(context, 'utf8'))
	for i in range(0, 10000000):
		result = md5(result.digest())
	return result.hexdigest()

Pass(6, 'azfH^1f;*nag91')
key=input('请输入8位数字PIN码:')
print("验证中……")
if Hash(key) == '5f4654140971c47658de19d62ba472b6':
	exec(Decrypt(key,'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'))
else:
	print("PIN码错误")
```

<!-- more -->

`ps: 应放弃破解 Hash`
`ps: 转而直接破解 key 也就是 PIN 码`

## 暴力破解 PIN 码

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
from hashlib import md5

from Crypto.Cipher import AES
import base64
import time
import gzip
from hashlib import md5
import sys
import io
sys.stdout = io.TextIOWrapper(sys.stdout.detach(), encoding='utf-8', line_buffering=True)

def Decrypt(key:str, text:str) -> str:
  if len(key) < 32: key += ' ' * (32 - len(key))
  elif len(key) > 32: key = key[0:32]
  cipher = AES.new(bytes(key,encoding='utf-8'), AES.MODE_CBC, bytes(AES.block_size))
  return str(gzip.decompress(bytes.strip(cipher.decrypt(base64.b64decode(text)))), encoding='utf-8')


def Pass(id, priv_key):
  prefix = str(id) + str(int(time.time()))
  pub_key = prefix + md5(bytes(prefix + priv_key, 'utf8')).hexdigest()
  print('恭喜通过第%d关,通关公钥:%s' % (id, pub_key))

if __name__=='__main__':
  x = 0
  while x <= 99999999:
      key = str(x).zfill(8)
      print("key = %s" % key)
      try:
          qs = Decrypt(key,'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')
          print(qs)
          print("OK")
          break
      except:
          print("Err")
      x = x + 1
```

## 结语

`ps: 暴力破解 PIN 码时间不短 建议并行计算`

> 破解成功后即可进入第八题
> 第八题思路可参考这篇文章 [<<一道即将尘封十几年的封印，不来尝试解开吗？>>第八关 Write-Up](http://rootkiter.com/2020/06/27/%E4%B8%80%E9%81%93%E5%8D%B3%E5%B0%86%E5%B0%98%E5%B0%81%E5%8D%81%E5%87%A0%E5%B9%B4%E7%9A%84%E5%B0%81%E5%8D%B0-%E4%B8%8D%E6%9D%A5%E5%B0%9D%E8%AF%95%E8%A7%A3%E5%BC%80%E5%90%97-%E7%AC%AC%E5%85%AB%E5%85%B3%E8%A7%A3.html)

## 请我吃雪糕

[请我吃雪糕](https://latopay.com/@meloalright)
