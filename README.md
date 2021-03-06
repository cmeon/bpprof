
bpprof
======

Just like `net/http/pprof` but with nicer formatting.

The HTTP endpoint will be like `/debug/bpprof/heap?sort=allocobjects`

And it groups allocations by stack trace. `net/http/pprof` groups allocations by a [combination of stack trace and size of the allocation](https://github.com/golang/go/blob/f9ed2f75c43cb8745a1593ec3e4208c46419216a/src/runtime/mprof.go#L157) which can result in a lot of duplicate stack traces in the output.

And sorting:
* `inusebytes` for the number of bytes in use.
* `allocbytes` for the total number of bytes allocated.
* `inuseobjects` for the number of objects in use.
* `allocobjects` for the total number of objects allocated.

Example output:
```
heap profile: 1: 384 [1300: 1292864] @ heap/1048576
# heap profile: 1: 384.00B [1300: 1.23MB] @ heap/1048576

1: 384 [1: 384] @ 0x433d57 0x42859f 0x42e4b0 0x42e059 0x45b132
# 1: 384.00B [1: 384.00B]
# 0x433d57  runtime.malg+0x27   /home/erik/go1.5rc1/src/runtime/proc1.go:2172
# 0x42859f  runtime.mpreinit+0x1f   /home/erik/go1.5rc1/src/runtime/os1_linux.go:197
# 0x42e4b0  runtime.mcommoninit+0x100 /home/erik/go1.5rc1/src/runtime/proc1.go:114
# 0x42e059  runtime.schedinit+0x79    /home/erik/go1.5rc1/src/runtime/proc1.go:57
# 0x45b132  runtime.rt0_go+0x132    /home/erik/go1.5rc1/src/runtime/asm_amd64.s:109

0: 0 [1261: 1291264] @ 0x43fc75 0x401055 0x45d951
# 0: 0.00B [1261: 1.23MB]
# 0x401055  main.alloc+0x55 /home/erik/src/github.com/erikdubbelboer/bpprof/test.go:21

0: 0 [38: 1216] @ 0x40c5b9 0x401096 0x45d951
# 0: 0.00B [38: 1.19KB]
# 0x401096  main.alloc+0x96 /home/erik/src/github.com/erikdubbelboer/bpprof/test.go:21


# runtime.MemStats
# Alloc = 1105904 (1.05MB)
# TotalAlloc = 678636264 (647.20MB)
# Sys = 8788216 (8.38MB)
# Lookups = 11
# Mallocs = 1287497
# Frees = 1285102
# HeapAlloc = 1105904 (1.05MB)
# HeapSys = 5865472 (5.59MB)
# HeapIdle = 4407296 (4.20MB)
# HeapInuse = 1458176 (1.39MB)
# HeapReleased = 0 (0.00B)
# HeapObjects = 2395 (2.34KB)
# Stack = 425984 (416.00KB) / 425984 (416.00KB)
# MSpan = 20272 (19.80KB) / 81920 (80.00KB)
# MCache = 2416 (2.36KB) / 16384 (16.00KB)
# BuckHashSys = 1443641
# NextGC = 4194304
# PauseNs = [1.003049ms 612.551µs 431.492µs 455.292µs 513.807µs 1.103094ms 917.038µs 405.948µs 168.212µs 792.706µs 732.522µs 598.825µs 341.405µs 667.585µs 561.903µs 346.29µs 386.442µs 589.475µs 755.99µs 358.849µs 918.391µs 334.449µs 955.813µs 756.177µs 1.094449ms 399.478µs 425.074µs 317.416µs 564.846µs 498.086µs 827.612µs 356.15µs 399.62µs 632.017µs 451.486µs 484.771µs 1.000393ms 412.456µs 249.506µs 720.435µs 609.567µs 470.08µs 631.073µs 710.335µs 274.56µs 590.563µs 258.221µs 423.68µs 476.764µs 773.464µs 797.647µs 441.234µs 852.733µs 718.333µs 811.135µs 952.611µs 417.558µs 1.080511ms 642.484µs 651.614µs 419.938µs 481.888µs 492.052µs 556.541µs 258.042µs 1.442192ms 719.954µs 737.705µs 464.974µs 499.743µs 326.83µs 376.764µs 207.562µs 446.428µs 569.204µs 405.048µs 1.117461ms 391.528µs 669.663µs 1.380648ms 863.364µs 196.78µs 177.532µs 240.429µs 587.172µs 197.326µs 188.239µs 731.337µs 656.55µs 388.019µs 405.176µs 329.44µs 432.547µs 779.157µs 439.849µs 430.509µs 303.77µs 229.126µs 672.524µs 353.998µs 775.877µs 197.335µs 564.869µs 545.906µs 536.706µs 1.022421ms 660.601µs 444.467µs 620.772µs 846.385µs 431.182µs 767.727µs 455.023µs 525.06µs 588.96µs 471.539µs 714.674µs 460.347µs 219.312µs 778.002µs 377.861µs 414.1µs 748.78µs 537.606µs 737.255µs 576.812µs 335.073µs 444.799µs 1.018627ms 595.837µs 625.504µs 471.689µs 726.86µs 350.072µs 331.79µs 371.342µs 636.695µs 312.139µs 576.917µs 449.903µs 608.423µs 668.667µs 739.99µs 354.826µs 278.847µs 432.249µs 463.467µs 299.139µs 700.795µs 405.759µs 646.945µs 989.163µs 411.464µs 292.662µs 807.668µs 410.11µs 439.411µs 473.188µs 448.272µs 669.643µs 564.854µs 552.119µs 371.946µs 433.667µs 298.643µs 415.134µs 214.57µs 542.243µs 442.393µs 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
# PauseNsLongest = 1.442192ms
# NumGC = 170
# EnableGC = true
# DebugGC = false
```
vs `net/http/pprof`:
```
heap profile: 1: 384 [1294: 1287712] @ heap/1048576
1: 384 [1: 384] @ 0x433d57 0x42859f 0x42e4b0 0x42e059 0x45b132
# 0x433d57  runtime.malg+0x27   /home/erik/go1.5rc1/src/runtime/proc1.go:2172
# 0x42859f  runtime.mpreinit+0x1f   /home/erik/go1.5rc1/src/runtime/os1_linux.go:197
# 0x42e4b0  runtime.mcommoninit+0x100 /home/erik/go1.5rc1/src/runtime/proc1.go:114
# 0x42e059  runtime.schedinit+0x79    /home/erik/go1.5rc1/src/runtime/proc1.go:57
# 0x45b132  runtime.rt0_go+0x132    /home/erik/go1.5rc1/src/runtime/asm_amd64.s:109

0: 0 [37: 1184] @ 0x40c5b9 0x401096 0x45d951
# 0x401096  main.alloc+0x96 /home/erik/src/github.com/erikdubbelboer/bpprof/test.go:21

0: 0 [1256: 1286144] @ 0x43fc75 0x401055 0x45d951
# 0x401055  main.alloc+0x55 /home/erik/src/github.com/erikdubbelboer/bpprof/test.go:21


# runtime.MemStats
# Alloc = 3955072
# TotalAlloc = 677515296
# Sys = 8788216
# Lookups = 11
# Mallocs = 1285200
# Frees = 1277445
# HeapAlloc = 3955072
# HeapSys = 5865472
# HeapIdle = 1572864
# HeapInuse = 4292608
# HeapReleased = 0
# HeapObjects = 7755
# Stack = 425984 / 425984
# MSpan = 57792 / 81920
# MCache = 2416 / 16384
# BuckHashSys = 1443641
# NextGC = 4194304
# PauseNs = [370949 442393 542243 214570 415134 298643 433667 371946 552119 564854 669643 448272 473188 439411 410110 807668 292662 411464 989163 646945 405759 700795 299139 463467 432249 278847 354826 739990 668667 608423 449903 576917 312139 636695 371342 331790 350072 726860 471689 625504 595837 1018627 444799 335073 576812 737255 537606 748780 414100 377861 778002 219312 460347 714674 471539 588960 525060 455023 767727 431182 846385 620772 444467 660601 1022421 536706 545906 564869 197335 775877 353998 672524 229126 303770 430509 439849 779157 432547 329440 405176 388019 656550 731337 188239 197326 587172 240429 177532 196780 863364 1380648 669663 391528 1117461 405048 569204 446428 207562 376764 326830 499743 464974 737705 719954 1442192 258042 556541 492052 481888 419938 651614 642484 1080511 417558 952611 811135 718333 852733 441234 797647 773464 476764 423680 258221 590563 274560 710335 631073 470080 609567 720435 249506 412456 1000393 484771 451486 632017 399620 356150 827612 498086 564846 317416 425074 399478 1094449 756177 955813 334449 918391 358849 755990 589475 386442 346290 561903 667585 341405 598825 732522 792706 168212 405948 917038 1103094 513807 455292 431492 612551 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
# NumGC = 169
# EnableGC = true
# DebugGC = false
```

