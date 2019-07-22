---
title: new self 和new static
date: 2018-04-27 23:00:34
categories:
- php
tags:
#top: true
---


```
<?php

//new self 和new static 只有在继承中才有区别
//new static()则是由调用者决定的

class F {

    public function getNewFather() {
        return new self();
    }

    public function getNewCaller() {
        return new static();
    }

}
class Sun1 extends F {

}

class Sun2 extends F {

}

$sun1 = new Sun1();
$sun2 = new Sun2();

print get_class($sun1->getNewFather());// F
print get_class($sun1->getNewCaller());// Sun1
print get_class($sun2->getNewFather());// F
print get_class($sun2->getNewCaller());// Sun2

```
