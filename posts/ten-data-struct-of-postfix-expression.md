---
date: 2013-04-23
layout: post
title: 后缀表达式求值
categories:
- 
tags:
- algorithm,Go,PHP
---

**后缀表达式求值**

题目：后缀表达式求值。  
要求：输入后缀表达式，输入为整数和四则运算，输出计算结果。  
例如：  
输入：2 3 * 1 -  
输出：5  
分析：2*3-1=5  

输入：1 2 + 5 4 * 3 - * 6 -  
输出：45  
分析：（1+2）*（5*4-3）-6=45    
      
算法分析：  
后缀表达式相对于普通的表达式来说，其优点在于不需要括号。所以方便的计算机的处理，通常对于后缀表达式才用栈的数据结构来实现，从左往右扫描表达式，如果遇到的是数字，则把数字压入栈中；若遇到的是运算符号，则提取栈的的2个元素进行计算，将计算结果压栈，若最后栈内只剩下一个数字，这就是后缀表达式的计算结果。

代码实现：  

**PHP版**

    <?php
    //获取后缀表达式的值
    function getPostfixExpValue($input)
    {
        $stack = array();
        
        $arr = explode(" ",$input);
        $len = sizeof($arr);
        if($len<3){
            return "错误的表达式";        
        }
        for($i=0;$i<$len;$i++){
            if(is_numeric($arr[$i])){
                $stack[] = $arr[$i];   
            }else if($i<2 || !in_array($arr[$i],array("+","-","*","/","%"))){
                return "错误的表达式";
            }
            else{
                $out1 = array_pop($stack);
                $out2 = array_pop($stack);
                if(!is_numeric($out1) && !is_numeric($out2)){
                    return "错误的表达式";
                }

                $str = $out1.$arr[$i].$out2;
                eval("\$res=".$out2.$arr[$i].$out1.";");
                $stack[] = $res;
            }
        }
        return array_pop($stack);
    }
    ?>

**Golang版**

    package main

    import (
        "errors"
        "fmt"
        "regexp"
        "strconv"
        "strings"
    )

    func GetPostfixExpValue(str string) (res int, err error) {

        _, err = regexp.MatchString("^((\\d+|[+*\\/%-])\\s){2,}([+*\\/%-])$", str)
        if err != nil {
            return 0, err
        }

        arr := strings.Split(str, " ")
        stack := make(map[int]int)
        for _, val := range arr {

            ok, _ := regexp.MatchString("^\\d+$", val)
            if ok {
                stack[len(stack)], err = strconv.Atoi(val)
                if err != nil {
                    return 0, nil
                }
                continue
            }
            _, err := regexp.MatchString("^[+*/%-]$", val)
            if err != nil {
                return 0, err
            }

            num1 := stack[len(stack)-1]
            num2 := stack[len(stack)-2]
            var res int
            switch val {
            case "+":
                res = num1 + num2
            case "-":
                res = num2 - num1
            case "*":
                res = num2 * num1
            case "/":
                res = num2 / num1
            case "%":
                res = num2 % num1
            default:
                res = 0
            }
            delete(stack, len(stack)-1)
            stack[len(stack)-1] = res
        }
        if len(stack) > 1 {
            return 0, errors.New("表达式错误")
        }
        total := stack[0]
        return total, nil
    }

    func main() {
        str := "10 2 * 8 + 9 - 5 %"

        result, err := GetPostfixExpValue(str)
        if err != nil {
            fmt.Println(err)
        } else {
            fmt.Println(result)
        }

    }

代码比较粗糙，欢迎批评指正~~
