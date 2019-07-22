---
title: 利用group 查找文件夹下包含‘key’的文件
date: 2018-04-28 23:00:34
categories:
- linux
tags:
- shell
#top: true
---

```
root@izhp36yfj8lo876bsm8f2nz api (master) # grep PARAMS_ERROR -rl --include="*.php" ./
./config/app.php
./app/Exceptions/OmgException.php
./app/Http/Controllers/ActivityController.php
./app/Http/Controllers/MarkController.php
./app/Http/Controllers/ImgManageController.php
./app/Http/Controllers/RedeemController.php
./app/Http/Controllers/AwardController.php
./app/Http/Controllers/OneYuanController.php
./app/Http/Controllers/ExamineController.php
./app/Http/Controllers/MoneyShareController.php
./app/Http/Controllers/IntegralMallController.php
./app/Http/Controllers/ChannelController.php
./app/Http/JsonRpcs/DaZhuanPanJsonrpc.php
./app/Http/JsonRpcs/NetworkDramaDzpJsonrpc.php
./app/Http/JsonRpcs/ActivityJsonrpc.php
./app/Http/JsonRpcs/ScratchJsonrpc.php
./app/Http/JsonRpcs/RobRateCouponJsonrpc.php
./app/Http/JsonRpcs/WorldCupJsonrpc.php
./app/Http/JsonRpcs/XjdbJsonrpc.php
./app/Http/JsonRpcs/CollectCardJsonrpc.php
root@izhp36yfj8lo876bsm8f2nz api (master) # sed -i s/PARAMS_ERROR/USER_PARAMS_ERROR/g 'grep PARAMS_ERROR -rl --include="*.php" ./'
sed：无法读取 grep PARAMS_ERROR -rl --include="*.php" ./：没有那个文件或目录
root@izhp36yfj8lo876bsm8f2nz api (master) # sed -i s/PARAMS_ERROR/USER_PARAMS_ERROR/g `grep PARAMS_ERROR -rl --include="*.php" ./`
root@izhp36yfj8lo876bsm8f2nz api (master) # git status
```

​​​​​​​​​​​​​​
![image](https://user-images.githubusercontent.com/9856659/51667733-97783280-1ffb-11e9-821c-4031fa1c3278.png)

