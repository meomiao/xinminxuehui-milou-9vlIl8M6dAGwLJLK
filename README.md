
# Visual Studio C\+\+ 汇编 混合编程


## 实验要求


请用汇编语言编写实现GCD递推公式的子程序，对入口和出口参数形式不做要求，但需要用 C 语言函数来获取输入、调用汇编递推子程序，并且用 C 语言显示子程序返回的结果。


## Visual Studio 2020 下载


下载时勾选C\+\+桌面开发选项。


![](https://images.cnblogs.com/cnblogs_com/Varuxn/2017186/o_241219131722_1.png)


## 环境配置


选择 **文件\-\>新建\-\>项目** ，语言选择 **C\+\+** ，选择 **空项目** 。


![](https://images.cnblogs.com/cnblogs_com/Varuxn/2017186/o_241219132228_2.png)


修改环境配置为 **x86**。


![](https://images.cnblogs.com/cnblogs_com/Varuxn/2017186/o_241219133753_6.png)


在项目中新建 `gcd.asm` 和 `t.cpp` 或将这两个文件添加到项目中。


右键项目，选择 **生成依赖项\-\>生成自定义**，勾选 **masm** 选项。


![](https://images.cnblogs.com/cnblogs_com/Varuxn/2017186/o_241219132933_3.png)


![](https://images.cnblogs.com/cnblogs_com/Varuxn/2017186/o_241219133303_4.png)


右键 `gcd.asm` 文件，选择 **属性**。


**从生成中排除** 选择 **否**。


**项类型** 选择 **Microsoft Macro Assembler**。


![](https://images.cnblogs.com/cnblogs_com/Varuxn/2017186/o_241219133637_5.png)


在编译运行的时候出现如下错误:


`scanf‘: This function or variable may be unsafe.Consider using scanf_s instead`


相关问题的解答 [Link](https://github.com)


可以在 `.cpp` 文件的头文件加入 `#define _CRT_SECURE_NO_WARNINGS` 。


## Code


cpp 文件



```
Copy#define _CRT_SECURE_NO_WARNINGS
#include 

// 声明外部汇编函数
extern "C" int GCD(int a, int b);

int main() {
    int a, b, result;

    // 获取用户输入
    printf("请输入两个整数以计算其最大公约数：");
    scanf("%d %d", &a, &b);

    // 调用汇编函数
    result = GCD(a, b);

    // 输出结果
    printf("数字 %d 和 %d 的最大公约数是：%d\n", a, b, result);

    return 0;
}


```

asm文件



```
Copy.model flat, c
.code
public GCD       ; 声明函数为公共，可以被外部调用

GCD proc
    mov eax, [esp+4] ; 获取第一个参数 a (位于 esp+4)
    mov ebx, [esp+8] ; 获取第二个参数 b (位于 esp+8)

gcd_loop:
    cmp ebx, 0       ; 如果 b == 0，跳转到结束
    je gcd_done
    xor edx, edx     ; 清空 edx，避免余数计算时的干扰
    div ebx          ; eax = eax / ebx，余数存入 edx
    mov eax, ebx     ; a = b
    mov ebx, edx     ; b = a % b
    jmp gcd_loop

gcd_done:
    ret              ; 返回结果存于 eax
GCD endp

end


```

 本博客参考[PodHub豆荚加速器官方网站](https://rikeduke.com)。转载请注明出处！
