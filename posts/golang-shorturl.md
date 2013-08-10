---
date: 2013-03-28
layout: post
title: Go语言实现的短网址算法
categories:
- Go
tags:
- Go
---

一直想做个自己的短网址服务，现在刚好在学习go语言，所以就决定用go来写这个应用练练手。  

看了网上很多短网址算法，比较著名的算法要数10进制与62进制转换算法，转换步骤如下：

> 1. 准备一个数组，存放a-zA-Z0-9这62个字符
> 2. 自定义网址前后缀，然后连接成一个字符串并进行md5加密，生成32位的签名串
> 3. 将长网址md5生成32位签名串,分为4段, 每段8个字节;
> 4. 对这四段循环处理, 取8个字节, 将他看成16进制串与0x3fffffff(30位1)与操作, 即超过30位的忽略处理;
> 5. 这30位分成6段, 每5位的数字作为字母表的索引取得特定字符, 依次进行获得6位字符串;
> 6. 总的md5串可以获得4个6位串; 取里面的任意一个就可作为这个长url的短url地址;

代码如下：

    func ShortUrl(url string, prefix string, suffix string) [4]string {
        
        str := prefix + url + suffix

        chars := [62]byte{'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h',
            'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p',
            'q', 'r', 's', 't', 'u', 'v', 'w', 'x',
            'y', 'z', '0', '1', '2', '3', '4', '5',
            '6', '7', '8', '9', 'A', 'B', 'C', 'D',
            'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L',
            'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T',
            'U', 'V', 'W', 'X', 'Y', 'Z'}

        md5str := Md5Encode(str)
        hexstr := []byte(md5str)

        var resUrl [4]string

        for i := 0; i < 4; i++ {
            j := i * 8
            k := j + 8
            s := "0x" + string(hexstr[j:k])
            hexInt, _ := strconv.ParseInt(s, 0, 64)
            hexInt = 0x3FFFFFFF & hexInt

            var outChars string = ""
            for n := 0; n < 6; n++ {
                        index := 0x0000003D & hexInt
                        outChars = outChars + string(chars[index])
                        hexInt = hexInt >> 5
                    }
            resUrl[i] = outChars
        }
        return resUrl
    }

    func Md5Encode(str string) string {
        h := md5.New()
        io.WriteString(h, str)
        buffer := bytes.NewBuffer(nil)
        fmt.Fprintf(buffer, "%x", h.Sum(nil))
        return buffer.String()
    }


