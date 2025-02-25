很一般的情况下：   
深度是在Fragment结束后写入的（因为FS里有能够修改深度的操作），然后发生深度测试
但如果在FS里没有修改深度的操作，深度的写入就会提前到VS结束，相应的深度测试也会提前，这被称为earlyZ。     

这些修改深度的操作包括： alpha test，discard/clip, 修改深度 ...      

深度测试提前代表着fs也会提前知道是否执行，于是可以减少overdraw的发生。      

对于某些物体的渲染，其会经常用到clip/alpha test，并且会出现大量的overdraw   
这个就是植物了=-=      
可以先用一个包含了alphaTest/clip的pass去渲染一遍，这个pass只写入深度，不写入颜色。目的是计算出深度。     
再来一个真正的渲染pass，这个pass的深度比较函数设置为相等，并且不包含clip/alpha test（为了启用early-z   
这意味着这个pass的fs只会画出可见的部分（prepass最后得到的深度），而不会绘制不可见的部分       

可以看到prez的目的就是为了先用较少的消耗去（不进行光照什么计算的pass，但是启用不了earlyZ）得到深度，然后再利用这个信息，启用earlyZ来避免overdraw   



