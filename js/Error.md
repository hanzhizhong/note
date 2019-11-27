### 秒转时分秒

~~~javascript
var itime=9045
let h=Math.floor(itime/3600)
let m=Math.floor(itime%3600/60)
let s=Math.floor(itime%3600%60)
h=h>10?h:'0'+h
m=m>10?m:'0'+m
s=s>10?s:'0'+s

~~~

