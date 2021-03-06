# get-real-file-type

获取文件真实的 mimeType 并与目标格式进行比较的工具库

## 安装

```
$ npm install get-real-file-type
```

## 用法

```
import { TypeFile } from 'get-real-file-type';

const fileInput = document.createElement('input');
fileInput.setAttribute('type', file);

fileInput.onchange = function(e) {
  const file = e.target.files[0];
  const typeFile = new TypeFile(file); // 这里支持传入File实例/包含File实例的对象/Uint8Array
  typeFile.onParseEnd = function() {
    if(this.isType('video/mp4')) {
      // TODO Suc
    }

    if(this.isType(['video/mp4', 'video/flv', 'video/vnd.avi'])) {
      // TODO Suc
    }
  };
  typeFile.onParseError = function() {
    // TODO Error
  }
  typeFile.start();
}

```

## API

### TypeFile(input)

`input`参数需要是一个浏览器的`File实例`，或者是一个`object`包含`File实例`，或者是一个`Uint8Array`

`File`实例最终也会转化为`Uint8Array`

该函数会返回一个`TypeFile`实例:

#### start()
`start`方法调用后，文件就开始解析。
看解析的结果来调用`onParseEnd`或`onParseError`

#### onParseEnd()

`onParseEnd`会在`start`方法执行后，文件解析`成功`后立即调用，此时该实例已经初始化完成。

初始化完成后会给实例初始化以下属性：

- `ext` - 根据文件名解析的后缀(只传入 Uint8Array 为 null)
- `mime` - 浏览器通过文件名后缀解析的 mimeType(只传入 Uint8Array 为 null)
- `realExt` - 真实的文件后缀
- `realMime` - 真实的文件的 mimeType

识别不出的属性也为 null

#### onParseError(reason)

`onParseError`会在`start`方法执行后，文件解析`失败`后立即调用。
默认第一个参数是错误的原因

#### isType(targetMimeType, compareType?)

`targetMimeType`可以传入一个字符串，或者一个字符串数组，用于和实例中的文件属性进行比较。

`compareType`默认为`TypeFile.COMPARE_TYPE.REAL_FIRST`,也就是`0`，优先比较真实的文件属性。

返回值为`Boolean`

### TypeFile.COMPARE_TYPE

- REAL_FIRST: 0 - 表示优先比较真实的文件信息
- BROWSER_FIRST: 1 - 表示优先比较浏览器获取的文件信息
- REAL_ONLY: 2 - 表示只比较真实的文件信息
- BROWSER_ONLY: 3 - 表示只比较浏览器获取的文件信息

### browserMimeMapping

由自己维护的浏览器获取的 mimeType 的映射表,来源点击以下查看,以后可能会补充

- [`OGG`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=ogg.html)
- [`AAC`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=aac.html)
- [`MP3`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=mp3.html)
- [`WAV`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=wav.html)
- [`AVI`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=avi.html)
- [`WMA`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=wma.html)
- [`RMVB`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=rmvb.html)
- [`WMA`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=wma.html)
- [`RMVB`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=rmvb.html)
- [`FLV`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=flv.html)
- [`MP4`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=mp4.html)
- [`JPG`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=jpg.html)
- [`PNG`](https://www.pcmatic.com/company/libraries/fileextension/detail.asp?ext=png.html)

### realMimeMapping

由[`file-type`](https://github.com/sindresorhus/file-type/blob/master/supported.js)库维护的真实文件 mimeType 映射表

- realMimeMapping['video/mp4']
- realMimeMapping['audio/mpeg']
- ...

### realExtMapping

由[`file-type`](https://github.com/sindresorhus/file-type/blob/master/supported.js)库维护的真实文件后缀映射表

- realExtMapping['mp4']
- realExtMapping['mp3']
- realExtMapping['aac']
- ...
