---
title: 看雪ctf 第一题 流浪者 wp
date: 2019-3-17 21:47:07
tags:
---
# 看雪ctf 第一题 流浪者 wp

 &ensp;&ensp;打开程序，提示要验证password，载入OD，按照签到题的思路，改了一个跳转，出现了提示框pass，但是没有flag，应该不是这种思路（我是一个刚入手的萌新，可能没改全，所以出不来flag，只能换一个思路了）。一步一步调试，发现程序比较的字符串和我输入的不一样，我输入了一堆1，但是发现最终比较的是一堆b，所以应该是中间做了一些处理，如图![image](http://ww1.sinaimg.cn/large/0071ouepgy1g164egoz4bj30mx09vt9a.jpg)，重新调试，发现了一个循环体，如图![image](http://ww1.sinaimg.cn/large/0071ouepgy1g164egoz4bj30mx09vt9a.jpg)，记录下循环体的地址0x40181b。  
 &ensp;&ensp;用IDA打开程序，跳转到0x40181b，查看伪代码，![image](http://ww1.sinaimg.cn/large/0071ouepgy1g164egoz4bj30mx09vt9a.jpg),发现了字符串比较函数，如图中方框所示，如果Str1与"KanXueCTF2019JustForhappy"相等，则运行函数sub_401770()，双击进去查看，发现是通过的函数，然后往上看，aAbcdefhiabcde是一个字符串，完整的字符串为'abcdefghiABCDEFGHIJKLMNjklmn0123456789opqrstuvwxyzOPQRSTUVWXYZ',然后查看传进来的参数a1，![image](http://ww1.sinaimg.cn/large/0071ouepgy1g164egoz4bj30mx09vt9a.jpg),在ans函数名处按'x'查看调用ans函数的地方，跟过去看看，发现了另一个函数。  
 通过分析，整理一下思路，输入一个字符串，然后用图中核心算法生成一个数组v5，v5是一个偏移量的列表，然后将这个列表传入ans（a1）函数，得到Str1,相当于下面代码的赋值。
 ```
 for(int i=0;i<25;i++){
     Str[i] = aAbcdefhiabcde[a[i]]
 }
 ```
 写一个python脚本是复原这个算法，就可以得到应该输入的字符串了![image](http://ww1.sinaimg.cn/large/0071ouepgy1g164egoz4bj30mx09vt9a.jpg)
 ```python
 
# coding=UTF-8

def getf(v5, i):
	global p,flag
	if p < 25 and v5 == position[p] :
		# print(content[i],'p:',p)
		flag += content[i]
		p += 1

def getPosition(KK,content,position):					
	for i in KK:
		for j in range(0,len(content)):
			if i == content[j]:
				position.append(j)	

def getflag(content,v5):								# 还原算法
	for m in range(0,25):								# 因为KK是长度为25的字符串，所以这里循环25次
		for i in range(0,len(content)):					# 遍历content,相当于爆破，找到v5(也就是a1)
			num = ord(content[i])
			if num > 57 or num < 48:
				if num > 122 or num < 97:
					if num > 90 or num < 65:
						pass
					else:
						v5 = num - 29
				else:
					v5 = num - 87
			else:
				v5 = num - 48
			getf(v5, i)	


if __name__ == '__main__':
	content = 'abcdefghiABCDEFGHIJKLMNjklmn0123456789opqrstuvwxyzOPQRSTUVWXYZ'
	KK = 'KanXueCTF2019JustForhappy'
	p = 0
	position = []
	v5 = 0
	flag = ''
	getPosition(KK,content,position)     # 获取最终要比较的字符串KK的每个字符在content中的位置变换(也就是a1)
	print(position)
	getflag(content,v5)					 
	print(flag)			
 ```


 最后感谢DeeLMind大佬，之前卡在算法那不知道怎么写脚本复现，看了大佬的视频后(https://www.bilibili.com/video/av46108107)参照大佬的脚本后进行了修改，视频讲的很细，建议萌新去看一下(大佬就直接忽略吧)。

