# C++筆記
# 作者
[facebook](https://www.facebook.com/vm.josh.5)
[instagram](https://www.instagram.com/josh00001027/)
# 補充內容
[Notion筆記(較不完整)](https://obsidian-hardhat-c55.notion.site/C-0e6d03235a9b4ca799299bbbfb420d36)
[簡報](https://docs.google.com/presentation/d/1T3YiHymtNHSYS-kf_j0kk8DSsq84mPWpGrF1fukQSjA/edit?usp=sharing)

# 程式語言簡介
電腦只認識機器語言,機器語言為二進位組合而成,執行效率最快,不易閱讀、撰寫。為使程式簡單化,發明高階語言,但無法直接執行,需透過編譯器(comiler)轉成機器語言後才能執行。

將程式原始碼編譯成由機器語言組成的目的碼(object code)再用連結器(linker)連結函式庫的目的碼檔案,產生執行檔,程式要執行時,再透過載入器(loader),將連結後產生的二進位程式碼,載入記憶體執行。

電腦語言可分為:高階語言、中階語言、低階語言。
執行效率低階>中階>高階,而C/C++ 為中階語言。

學C++的好處:
1. 應用範圍廣
2. 較佳的執行效率
3. 高移植性

以C語言為基礎,發展出來的物件導向程式語言 OOP (object-oriented programming)、C++屬於物件導向程式語言。


# 記憶體簡介
程式執行時,電腦會將資料載入記憶體中,電腦使用二進位,記憶體的大小通常是以 byte(位元組)為單位,每一個記憶體的儲存單位都會有一個不同的位址(address)。若某一電腦系統有n個位址線可決定2的n次方bytes記憶體的位址。


# 程式語言結構
## 預處理命令
預處理命令:include 的中文意思是引入,所以 `#include <iostream>`是指將標頭檔iostream 的內容加到程式的這個位置,而輸出函数`cout` 被定義在標頭檔`<iostream> `內,所以程式用 到 `cout` 指令,就需先引入 `<iostream> `,否則會發生錯誤。
## 使用名稱空間
使用名稱空間:`using namespace std` 是使用名稱空間 `std`。由於 標準函式庫所包含的函數定義在名稱空間 `std` 中,使用到標準函式`iostream` 的`cout` 函數,就要宣告使用名稱空間 `std`。
## 主函數
主函數:程式會從主函數 `main()` 開始執行,不論它在程式的哪一個位置。`main()` 前面的 `int` 是 integer(整數)的簡寫,宣告函數 `main()` 屬於整數函數,若能成功執行完函數,會回傳整數值 **0** 給作業系統如果不能正常執行函數,則會回傳 **-1**。
## 呈現方式
```cpp=
#include <iostream>
using namespace std;
int main(){
    這
        裡
            放
                執
                    待    
                        行
                            程
                                式
}
```

# 錯誤類型
* 語意錯誤:又稱邏輯錯誤編譯器不會顯示錯誤訊息。
* 語法錯誤:編譯器會顯示錯誤而無法執行,




# 程式語言學習詳細介紹
## 一、 變數
### 類型
|  變數  |   定義   |範圍|
|:------:|:--------:|:--------:|
|int| 整數|-2,147,483,648 至 2,147,483,647|
|  long  |  整數  |-2,147,483,648 至 2,147,483,647|
| float  |   小數   |  3.4e +/- 38 (7 位數)   | 
| double | 精確小數 |  1.7E +/- 308 (15 位數)  | 
|  char  |   字元   |  無  | 
| string |   字串   |   無 | 
### 注意事項
變數可視為儲存一個記憶體的空間,但變數命名時有一定規則,以下幾點:

1. 英文字母大小寫不同
2. 可用字母數字 _ $ 
3. 不能使用數字開頭
4. 不能用空白、特殊符號(+ - * / % & # ^ ? @ 等)
5. 不能使用保留字(reserved word)
6. 保留字如下:
 ![](https://i.imgur.com/fysOuQ4.png)


### 宣告
```cpp=
#include <bits/stdc++.h>
using namespace std;

int a=35;   //宣告整數a值為35
float c=12.3;    //宣告小數c

int main(){
cout<<"a= "<<a<<'\n';
cout<<"c= "<<c<<'\n';
return 0;
}
```
![](https://i.imgur.com/te1Yf3w.png)


## 二、 運算子
可分為3大類分別是:
* 算數運算子
* 關係運算子
* 邏輯運算子


### 算數運算子
| 算術運算子 |  含意  |
|:----------:|:------:|
|     +      |   加   |
|     -      |   減   |
|     *      |   乘   |
|     /      |   除   |
|     %      | 取餘數 |
|     ++     |  遞加  |
|    - -     |  遞減  |
### 關係運算子
| 關係運算子 |   含意   |
|:----------:|:--------:|
|     ==     |   等於   |
|     !=     |  不等於  |
|     >      |   大於   |
|     <      |   小於   |
|     >=     | 大於等於 |
|     <=     | 小於等於 |
### 邏輯運算子
| 邏輯運算子 | 含意 |
|:----------:|:----:|
|     &&     |  和  |
|            |  或  |
|     !      |  不  |
## 三、 if/else用法
### 宣告
比大小(輸入兩值,輸出較大值)
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){

int a,b;
cin>>a>>b;    //下面範例輸入4 6
if(a>b)
    cout<<a;
else
    cout<<b;
return 0;
} 

```
![](https://i.imgur.com/5y9WnLB.png)

## 四、 迴圈
* 迴圈是一種常見的控制流程，是一段在程式中只出現一次，但可能會**連續執行多次**的程式碼。
* 迴圈中的程式碼會執行**特定的次數**，或者是執行到**特定條件成立**時結束迴圈，或者是針對某一集合中的所有項目都執行一次。
* 迴圈可分為兩種分別為:
while迴圈
for迴圈
* while迴圈又可分為兩種:
前測(while)
後測(do-while)


### while
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){
	int i=0;      //初始值
	while(i<3){   
	    cout<<"yes\n";
	    i++;         //更新
	}
	return 0;
}
```
![](https://i.imgur.com/Q4jn36O.png)


### do-while(~~沒什麼用~~)
- **至少跑一次**
- while後要加;
 - 就算初始值已經超過標準,還是**至少跑一次**
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){
	int i=0;
	do{
		cout<<"Hello!\n";
		i++;
		}
	while(i<3);    //記得加分號";"
	return 0;
}
```
![](https://i.imgur.com/VKIFUoZ.png)


### for(最常用)
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){
	for(int i=0;i<3;i++){
	    cout<<"Hello World!\n";
	    }
	return 0;
}
```
![](https://i.imgur.com/Z2Ca7o4.png)

### 雙層迴圈
#### 九九乘法
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main()
{
	for(int i=1;i<=9;i++)
	{
		for(int j=1;j<=9;j++)
		{
			cout<<i*j<<" ";
		}
	cout<<"i = "<<i<<"            \n";
	}
return 0;
}

```
![](https://i.imgur.com/EBYQRq8.png)

#### 數字塔
```cpp=
#include <bits/stdc++.h>
using namespace std;
int a;
int main()
{
    cout<<"Enter the hight : ";
    cin>>a;
	for(int i=1;i<=a;i++)
	{
		for(int j=1;j<=i;j++)
		{
			cout<<j<<" ";
		}
	 cout<<'\n';
	}
return 0;
}

```
![](https://i.imgur.com/gWFRBsa.png)

## 五、 陣列
### 注意事項
- **橫為行,直為列**
- 先行在列
- 儲存多比**相同類型**資料
- 從「0」開始
- 用大括號刮起來
### 宣告
```cpp=
#include <bits/stdc++.h>
using namespace std;
int main(){
int a[3];
a[0]=1;
a[1]=2;
a[2]=3;//亦可為int a[3]={1,2,3};

cout<<a[0]<<'\n';
cout<<a[1]<<'\n';
cout<<a[2]<<'\n';

return 0;
}

```
![](https://i.imgur.com/fkrEvWK.png)










##  六、 布林變數
### 注意事項

- 常用來比較運算元
- 式子正確回傳(1/true),錯誤回傳(0/false)
- 格式:關係運算子放在兩運算元間
### 宣告(關係型)
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){
int a= 3;
int b= 4;
int c;

c = (a>b);
cout<<c<<'\n';
c = (a<b);
cout<<c<<'\n';

return 0;
}

```
![](https://i.imgur.com/7MtlYDQ.png)

### 宣告(邏輯型)
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){

int a=3,b=5,c=7,d;

d=(a>c||a>b);
cout<<d<<'\n';
d=(a<c||b<a);
cout<<d<<'\n';
d=(c>b&&b>a);
cout<<d<<'\n';
d=(c>b&&b>a);
cout<<!d<<'\n';

return 0;
}
```
![](https://i.imgur.com/w57k8Zo.png)

## 七、 break用法
- 直接跳出迴圈
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){
	int count =0;  //計算迴圈次數
	for(int i=1;i<10;i++){
				cout<<i<<' ';
				if(i%5==0){
						break;
						}
				count++;
				}
	cout<<'\n'<<count;
	return 0;
}
```
![](https://i.imgur.com/P90P5tC.png)

## 八、 continue用法
- 直接跳進迴圈
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){
for(int i=0;i<20;i+=2)
	{
	if(i==10){
	         continue;
		 }
	cout<<i;
	cout<<"  ";
	}
}
```
![](https://i.imgur.com/mcC1w9P.png)





## 九、 define用法
### 注意事項
- **無法**隨意更動變數值
### 宣告
錯誤示範
```cpp=
#include <bits/stdc++.h>
using namespace std;

#define PI 3.1415926

int main(){
PI=4;
cout<<PI;
return 0;
}
```
![](https://i.imgur.com/ITYCRJW.png)

正確示範
```cpp=
#include <iostream>
using namespace std;

#define PI 3.1415926

int main(){
//PI=4;
cout<<PI;
return 0;
}
```
![](https://i.imgur.com/Qil7N24.png)




## 十、 struct用法
### 注意事項
- 不是~~變數~~,是**資料型態**
- 將不同型態資料綁在一起
- 在 **main** **之前**宣告
- 大括號之後以**分號**結尾
- 透過 struct_name.element 的方式取用元素
### 宣告
```cpp=
#include <bits/stdc++.h>
using namespace std;

struct student{
int number;   //學生座號
float weight; //學生體重
};

int main(){
student Jimmy;
Jimmy.number =3;
Jimmy.weight =57.3;
student Aim;
Aim.number =27;
Aim.weight =37.4;

cout<<"Jimmy's number: "<<Jimmy.number<<'\n';
cout<<"Jimmy's weight: "<<Jimmy.weight<<'\n';
cout<<"Aim's number: "<<Aim.number<<'\n';
cout<<"Aim's weight: "<<Aim.weight<<'\n';

return 0;
}
```
![](https://i.imgur.com/IIGiTFE.png)

## 十一、switch用法
### 注意事項
* `case`與`case`之間的`break`很重要
### 宣告

```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
int n;
cin>>n;
switch(n)   // n只能是int,char
{
case 1:
cout<<"Black tea";
break;
case 2:
cout<<"Cola";
break;
case 3:
cout<<"Green tea";
break;
default:
cout<<"No product";

}
return 0;
}
```
![](https://i.imgur.com/9BgoUn6.png) 
![](https://i.imgur.com/f5U67MA.png)
![](https://i.imgur.com/g4kpEIH.png)



## 十二、定義函式
### 宣告

```cpp=
#include <bits/stdc++.h>
using namespace std;

int change(int a)             //定義函式
{
a++;
return a;
}

int main()
{
int x;
cin>>x;    //範例輸入4
cout<<change(x);              //輸入函數值

}
```
![](https://i.imgur.com/9UcKjcF.png)



## 十三、補充訊息
### ASCII Code
用以下程式可以跑出ASCII Code編號表
```cpp=
#include <bits/stdc++.h>
using namespace std;

int main(){
	for(int i=33;i<127;i++){
	    cout << i <<"   "<< (char)i << endl;
	    }
	return 0;
}

```
![](https://i.imgur.com/zvWOxFu.png)
![](https://i.imgur.com/303mkSx.png)
![](https://i.imgur.com/Ah9auPn.png)

### 程式碼補充
```cpp=
//用於單行註解

/*
    用
        於
            多
                行
                    註
                        解
                            */

cout<<'\\';    //印出 \
cout<<'\n';    //換行
cout<<'\r';    //移動至行首(不換行)
cout<<'\t';    //tab
cout<<'\?';    //印出 ?
cout<<'\0';    //印出 '  '
cout<<'\'';    //印出 '
cout<<'\"';    //印出 "

    "%d"    //讀入十進位制有符號整數 
    "%u"    //讀入十進位制無符號整數 
    "%f"    //讀入浮點數 
    "%s"    //讀入字串 
    "%c"    //讀入單個字元 
    "%p"    //讀入指標的值 
    "%e"    //讀入指數形式的浮點數 
    "%x","%X"    //讀入無符號以十六進位制表示的整數 
    "%0"    //讀入無符號以八進位制表示的整數 
    "%g"    //讀入自動選擇合適的表示法 

#include <bits/stdc++.h>    //萬用涵式庫 

typedef long long LL;    //

cout<<fixed<<setprecision(輸出位數)<<變數/數字<<endl;    //控制輸出位數

cout << (char)i << endl;    //數字->字元(ASCII編號)

cout << (int)'0' << endl;    //字元->整數(ASCII編號)

while(cin>>a>>s)    //連續輸入持續至EOF
{
    cout<<(a+s)/2<<endl;    
}   


```
# APCS補充(by 吳邦一教授AP325)
## 輸出輸入
* 標準配備
```cpp=
#include <bits/stdc++.h>
#define int long long
using namespace std;

signed main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    
}

```
## 遞迴
* 遞迴在數學上是函數**直接或間接以自己定義自己**，在程式上則是函數**直接或間接呼叫自己**。
* 以階層函數為例:
```cpp=
int fac(int n) {
if (n == 1) return 1;
return n * fac(n-1);\
    }
```

* 遞迴函數一定會有終端條件(也稱邊界條件)
* 常用時機:
 1. 根據定義來實作
 2. 為了計算答案，以遞迴來進行窮舉暴搜



