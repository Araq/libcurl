## libcurl.nim
Nim wrapper for libcurl (v7.x)

## libcurl v7.x
- source/binary packages - all platforms: https://curl.haxx.se/download.html
- buid instructions - all platforms: https://curl.haxx.se/docs/install.html
- binary package (.dll) - windows users: https://curl.haxx.se/windows/

## basic example

```nim
import libcurl

proc curlWriteFn(
  buffer: cstring,
  size: int,
  count: int,
  outstream: pointer): int =
  
  let outbuf = cast[ref string](outstream)
  outbuf[] &= buffer
  result = size * count
  
let webData: ref string = new string
let curl = easy_init()

discard curl.easy_setopt(OPT_USERAGENT, "Mozilla/5.0")
discard curl.easy_setopt(OPT_HTTPGET, 1)
discard curl.easy_setopt(OPT_WRITEDATA, webData)
discard curl.easy_setopt(OPT_WRITEFUNCTION, curlWriteFn)
discard curl.easy_setopt(OPT_URL, "http://localhost/")

let ret = curl.easy_perform()
if ret == E_OK:
  echo(webData[])
```

see also: https://curl.haxx.se/libcurl/c/example.html
