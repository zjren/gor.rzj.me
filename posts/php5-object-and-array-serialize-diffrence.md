---
date: 2013-04-23
layout: post
title: PHP5对象与数组序列化
categories:
- PHP
tags:
- PHP
---

代码如下：

	<?php
    class person{
        public $name;
        public $gender;
        public function say(){
         echo $this->name ," is a ",$gender;
        }
    }

    class family {
        public $people;
        public $location;
        public function __construct($p,$loc){
            $this->people = $p;
            $this->location = $loc;
        }
    }
    
    $student = new person();
    $student->name="lilei";
    $student->gender="male";
    
    $tom = new family($student,"beijing");
    
    echo serialize($student);
    echo "\n";
    
    $student_arr = array("name"=>"lilei","gender"=>"male");
    echo serialize($student_arr);
    
    print_r($tom);
    echo "\n";
    echo serialize($tom);
    
    ?>

输出如下结果:

    O:6:"person":2:{s:4:"name";s:5:"lilei";s:6:"gender";s:4:"male";}
    a:2:{s:4:"name";s:5:"lilei";s:6:"gender";s:4:"male";}family Object
    (
        [people] => person Object
            (
                [name] => lilei
                [gender] => male
            )

        [location] => beijing
    )

    O:6:"family":2:{s:6:"people";O:6:"person":2:{s:4:"name";s:5:"lilei";s:6:"gender";s:4:"male";}s:8:"location";s:7:"beijing";}
    
可以看出对象与数组序列化的区别：

* 对象首字母为o,数组为a，分别是object和array的首字母
* 序列化的对象会附带所属的类名
* 当一个对象的实例变量引用其他对象时，序列化该对象时也会对引用对象进行序列化


--摘自[《PHP核心技术与最佳实践》](http://book.douban.com/subject/20370984/)
