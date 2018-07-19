⚠️如无特殊说明, terminal 的编码为 utf-8

### sys.stdout.encoding 与 print

python会采用**终端默认**的编码(用 locale.getdefaultlocale() 查看)将 unicode 编码成 str 类型.

print 函数在输出的时候受 sys.stdout.encoding(值默认来自 locale) 的影响

即如果 locale 的值和 terminal 的编码不一致, 则 terminal 的显示结果为乱码.

```python
# coding: utf-8
import sys
char = u"严"  # utf-8: '\xe4\xb8\xa5'; unicode: u'\u4e25'; gbk: '\xd1\xcf'
encoding = sys.stdout.encoding
print("locale=%s [%r]: %s " % (encoding, char, char))
```

运行 `PYTHONIOENCODING=gbk python char.py` :

> locale=gbk [u'\u4e25']: ??

### sys.setdefaultencoding 与 str.encode() str.decode()

sys.setdefaultencoding("utf-8")，用于指定str.encode() str.decode()

- 对编码字符串 a, 代码中可以直接写 a.encode("utf-8"), 但事实上内部自动先通过 defaultencoding 去 decode 成 unicode 之后再 encode 的.

  ```python
  # coding: gbk
  import sys
  reload(sys)
  sys.setdefaultencoding("gbk")
  char = "严"  # utf-8: '\xe4\xb8\xa5'; unicode: u'\u4e25'; gbk: '\xd1\xcf'
  print(r"- gbk: '\xd1\xcf'")
  print(r"- unicode: u'\u4e25'")
  print(r"- utf-8: '\xe4\xb8\xa5'")
  utf_char = char.encode("utf-8")
  print("%r => unicode => %s(%r)" % (char, utf_char, utf_char))
  ```

  运行 python char.py:

  > \- gbk: '\xd1\xcf'
  >
  > \- unicode: u'\u4e25'
  >
  > \- utf-8: '\xe4\xb8\xa5'
  >
  > '\xd1\xcf' => unicode => 严('\xe4\xb8\xa5')

### 文件开头的 `#coding: utf-8` 

告诉编辑器, 该文件里面的所有 str 都采用 utf-8 编码, 且存储文件的时候也是使用 utf-8 格式.