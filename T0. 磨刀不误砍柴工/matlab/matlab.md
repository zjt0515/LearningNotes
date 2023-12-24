# matlab_quickstart

参考资料：
[!参考1_2023](https://blog.csdn.net/gwj2003/article/details/134063346)
[!参考3_2022](https://zhuanlan.zhihu.com/p/586139355)
[!参考2_2022](https://blog.csdn.net/Toas7/article/details/128008737)
[!参考4_2022](https://blog.csdn.net/yanqiu12138/article/details/129806029)

## ml基础

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

### 格式化输出

`fprintf('%d',i)`与C语言类似

### 生成矩阵

直接：`a = [1,2;4,5]`
冒号：`a = 1:1:10` 步长为1时可以省略
函数：`a = linspace(start,end,number)`等差生成一维矩阵，省略个数默认生成100个
`a = eye(4)`单位阵
`a = zero(2,3)`零
`ones(4,1)`全1
`a = rand(4)`0-1的随机阵

### 矩阵运算

## 绘图

### 二维绘图

```matlab
% 传入2个一维矩阵代表各个点
x = 0:0.01:2*pi;
y = sin(x);
figure
plot(x,y)
```

```matlab
x = 0:0.01:20;
y1 = 200*exp(x);
y2 = 0.8*exp(x);
yyaxis(x,y1,x,y2,'plot')
```

```matlab
syms x;
y=sin(x);
y1=taylor(sin(x),x,'Order',2);
y2=taylor(sin(x),x,'Order',4);
y3=taylor(sin(x),x,'Order',6);
fplot([y y1 y2 y3])
```

#### 其他

```matlab
hold on  % hold off前，所有绘图都会显示在当前窗口
title('') % 添加标题
xlabel('') % 标记x轴
ylabel('') % 标记y轴
xlim([0 2*pi]) % 设置x轴数据范围
```

### 三维绘图

```matlab
x = linspace(-2,2,20);
y = x';
z = x .* exp(-x.^2 - y.^2);
surf(x,y,z)
```

## 例题

### 1.1

```matlab
syms x;
m=515;
limit( (log(1+x-m*x^2)-x) / (1-cos(x)) ,x,0)
```

```matlab
syms x;
m=515;
limit( ( sqrt(2+m*x^2) -atan(m*x) ) / x ,x,inf)
```

### 1.2

```matlab
syms x;
m=515;
s=exp(m*x)*sin(x);
diff(s,x,2) % x似乎可以

subs(diff(s,x,6), x, 0)
```

### 1.3

不定积分不要忘记+C

```matlab
syms x;
m=515;
int((x+sin(x))/(1+cos(x)))
```

1.3.2

```matlab
syms x;
m=515;
int(log(1+m*x)-m*x, x, 0, 1)
```

### 1.4 完成

```matlab
syms x;
m=515;
taylor( (m/200 + sin(x)) *cos(x), x, 'Order', 5)
```

### 1.5 完成

```matlab
syms x;
m=515;
x(1)=rand(1);
for n=2:10
x(n)=sqrt(m/100+x(n-1))
end
```

### 1.6 完成

```matlab
syms x;
m=515;
A=[4,2,m-2;-3,0,5;1,5,2*m];
B=[3,4,0;2,0,-3;-2,1,1];
det(A)
inv(A)
eig(A)
[P,D]=eig(A)
A/B
A\B
C=[A B];
[RC, P]=rref(C) % ?
```

### 1.7 完成

辅助函数：

```matlab
function y1=g(x,f)
n=length(x);
for i=1:n
    y1(i)=f(x(i));
end
end
```

#### 1.7.1

```matlab
function y=f171(x)
if x>=0&&x<=1/2
    y=2*x;
elseif x>1/2&&x<=1
    y=2*(1-x);
end
```

```matlab
fplot(@(x)g(x,@f171),[0,1])
```

#### 1.7.2

```matlab
function y=f172(x)
if x>=-pi&x<0
    y=x-1;
elseif x>=0&x<=pi
    y=x+1;
end
```

```matlab
fplot(@(x)g(x,@f172),[-pi,pi])
```

### 1.8 完成

1.8.1

```matlab
syms x;
m=515;
t=-m/100:0.1:m/100;
x=m/20*cos(t);
y=m/20*sin(t);
z=t;
plot3(x,y,z);
grid on
```

1.8.2

```matlab
syms x;
m=515;
t=-m/100:0.1:m/100;
x=cos(t) + t.*sin(t);
y=sin(t) - t.*cos(t);
z=-t;
plot3(x,y,z);
grid on
```

### 1.9 完成

```matlab
m=515;
x=-10:0.1:10;
a=[1000/m,500/m,100/m];
for i=1:3
    y=(1/(sqrt(2*pi).*a(i)))*exp(-x.^2/(2.*a(i).^2));
    plot(x,y)
    hold on
end
```

### 1.10 完成

与学号有关！！！

```matlab
ezplot('log(x^2+515*y) - x^3*y - sin(x)')
```

### 1.11 完成

```matlab
m=515;
[x,y] = meshgrid(-5:0.1:5); %
z=m.*x.*exp(-x.^2-y.^2);
mesh(x,y,z)
```

### 1.12

```matlab

```

### 2.1

#### 2.1.1

```matlab
m=515
syms x;
y=(2*x+1)/(x-m)-x;
solve(y,x)
```

#### 2.1.2

```matlab
function p=dd(f,x0,n)
p=[x0];
for i=2:n
    p(i)=f(p(i-1));
end
end
```

```matlab
m=515;
syms x;
dd(@(x)(2*x+1)/(x-m),0,20)
dd(@(x)(2*x+1)/(x-m),100,20)
dd(@(x)(2*x+1)/(x-m),-100,20)
dd(@(x)(2*x+1)/(x-m),200,20)
```

函数收敛，只要初值不取 ,即第二个不动点，收敛值与初值的选取关系不大，总是收敛于， 只有初值取 ，迭代函数才以它为极限；
收敛值一定是不动点其一；

### 2.2

```matlab
syms x;
m=515;
f=@(x)(1-2*abs(x-1/2));
x0=1/m;
for i=1:100
    plot(i,f(x0),'+');
    x0=f(x0);
    hold on
end
```

### 2.3 完成

该题与学号有关！！！

```matlab
function Martin(a,b,c,N)
f=@(x,y)(y-sign(x)*sqrt(abs(b*x-c)));
g=@(x)(a-x);
m=[0;0];
for n=1:N
    m(:,n+1)=[f(m(1,n),m(2,n)),g(m(1,n))];
end
plot(m(1,:),m(2,:),'kx');
axis equal
```

```matlab
% 图形的形状与m、选择的参数都有关
m=515;
Martin(m,m,m,5000)
Martin(-m,-m,m,10000)
Martin(-m,m/1000,-m,15000)
Martin(m/1000,m/1000,0.5,20000)
```

### 2.4

2.4.1

```matlab

```

2.4.2

```matlab
syms x;
m=515;
f=inline('(100*x+515)/(x^2+100)');
x0=5;% 任取一个初值
for i=1:20;
x0=f(x0);
fprintf('%g,%g\n',i,x0);
end
```

2.4.3

用自己的话改一下
函数迭代的收敛速度与初值的选取关系不大；
迭代初值对迭代的收敛性存在影响，但是这种影响存在不确定性，没有发现可循的规律；

### 2.5

2.5.1

```matlab
syms x;
y=sin(x);
y1=taylor(y,x,'Order',2);
y2=taylor(y,x,'Order',4);
y3=taylor(y,x,'Order',6);
fplot([y,y1,y2,y3],[-3/2*pi,3/2*pi])
grid on % 打开网格
```

2.5.2

```matlab
syms x;
y=sin(x);
y1=taylor(sin(x),x,'Order',8);
y2=taylor(sin(x),x,'Order',10);
y3=taylor(sin(x),x,'Order',12);
fplot([y,y1,y2,y3],[-3/2*pi,3/2*pi])
grid on
```

2.5.3
![image](https://img-blog.csdnimg.cn/f508d41708ef4d458acedc7faa2b9104.png)

### 3.1 完成

与学号有关！！！

```matlab
A=str2sym('[515,515-4;6-515,10-515]'); % 字符转化为表达式
[P,D]=eig(A);
Q=inv(P);
syms n;
x=[1;2];
xn=P*(D.^n)*Q*x 
```

### 3.2 完成

与学号有关！！！

```matlab
A=str2sym('[515,515-4;6-515,10-515]');
B=1/10.*A;
[P,D]=eig(B);
Q=inv(P);
syms n;
x=[1;2];
xn=P*(D.^n)*Q*x
```

### 3.3

3.3.1
P136

```matlab
A=[9,5;2,6];
t=[];
for i=1:20
    x=-1 + 2*rand(2,1); % 生成(-1,1)之间的2*1列向量
    t(length(t)+1,1:2)=x; % 第1列到第2列的第length(t)+1行元素都=x
    for j=1:40
        x=A*x;
        t(length(t)+1,1:2)=x;
    end % 迭代完成，t的每行都是一次迭代的结果
end
plot(t(:,1),t(:,2),'*') % 第1列所有元素作为x，第2列所有元素作为y，用*画图
grid('on')  % 显示网格线
```

3.3.2

迭代阵列似乎在一条通过原点的直线上。

3.3.3

```matlab
A=[9,5;2,6]; a=[];
x=2*rand(2,1)-1; 
for i=1:20
    a(i,1:2)=x;
    x=A*x;
end
for i=1:20
    if a(i,1)==0 % 去除分母为0的点
    else t=a(i,2)/a(i,1);
        fprintf('%d：%g\n',i,t);
    end
end
```

3.3.4

极限值是图像直线的斜率

按照自己语言组织下面任意一条

最终稳定值为迭代矩阵的特征值之一。
如果迭代矩阵有多个线性无关的特征向量对应于同一个特征值，那么最终稳定值将是这些特征向量线性组合的结果。
稳定值是迭代矩阵的特征向量，对应的特征值为1。而迭代矩阵的特征值和特征向量则可以通过特征方程来求得。

### 3.4

P141

一共运行四个矩阵，每次修改A矩阵

```matlab
m=515;
A=[m,1/4-m;m-3/4,1-m];
p=[0.3;0.7]; % 选择合适初始向量，要求和为1
[P,D]=eig(A) % P的每个列向量是特征向量，D主对角线元素是特征值
for i=1:100
    p(:,i+1)=A*p(:,i);
end 
fprintf('%f,%f\n',p)
```

```matlab
m=515;
A=[m,1/4-m;m-3/4,1-m];
x0=[0.3;0.7];
n=10000;
y = A^n * x0
```

### 4.1

 求勾股数，建议阅读P159

```matlab
for b=1:998 % 暴力遍历b的所有取值, b的范围可由c的范围求得
    a=sqrt((b+2)^2-b^2); % 求出a
    if(a == floor(a)) % 如果a等于a向下取整，即a是整数
        fprintf('a=%d, b=%d, c=%d\n',a,b,b+2)
    end
end
```

### 4.2

和4.1类似

```matlab
for b=1:996 % 暴力遍历b的所有取值
    a=sqrt((b+4)^2-b^2); % 求出a
    if(a == floor(a)) % 如果a等于a向下取整，即a是整数
        fprintf('a=%i, b=%i, c=%i\n',a,b,b+4)
    end
end
```for

```matlab
for b=1:995 % 暴力遍历b的所有取值
    a=sqrt((b+5)^2-b^2); % 求出a
    if(a == floor(a)) % 如果a等于a向下取整，即a是整数
        fprintf('a=%i, b=%i, c=%i\n',a,b,b+5)
    end
end
```

### 4.3

```matlab
for k=1:200 
    for b=1:1000-k
        a=sqrt((b+k)^2-b^2);
        % 只有当勾股数的最大公因子为1时(即本原勾股数)，才输出
        if(a==floor(a) & gcd(gcd(a,b),b+k)==1)
            fprintf('%d, ',k);
            break; % 对于每个k，找到一组本原勾股数就可以结束了
        end
    end
end
```

结果：1, 2, 8, 9, 18, 25, 32, 49, 50, 72, 81, 98, 121, 128, 162, 169, 200
特征：k为完全平方数或完全平方数的2倍
预测：225, 242, 288, 289

### 4.4 完成

p191练习一，书本上提示用polyfit

```matlab
x=[1.5,1.8,2.4,2.8,3.4,3.7,4.2,4.7,5.3];
y=[8.9,10.1,12.4,14.3,16.2,17.8,19.6,22.0,24.1];
p=polyfit(x,y,1); % 基于最小二乘，将x,y的数据拟合，按次数从大到小返回近似函数的系数
a1=p(1)
a0=p(2)
sum((y-(p(2)+p(1).*x)).^2) % 求总误差
```

### 4.5

本题与学号相关！！！
学号末位5，w=5, t = 1850 + 10*w = 1900, 对应人口76.0, 第12

```matlab
t=1790:10:1980; % 从1790到1980间隔10的等差数列
x=[3.9,5.3,7.2,9.6,12.9,17.1,23.2,31.4,38.6,50.2,62.9,76,92,106.5,123.2,131.7,150.7,179.3,204.0,226.5]; % 1790到1980年人口

X1=log(x(1));X2=log(x(12));
t1=t(1);t2=t(12); %此步根据学号不同而不同

% 使用克莱姆法则求解线性方程组
D=[1,t1;1,t2]; % 系数行列式
D1=[X1,t1;X2,t2]; % 
D2=[1,X1;1,X2];
X0=det(D1)/det(D);
k=det(D2)/det(D)
x0=exp(X0)
E=sum((x0*exp(k*t)-x).^2) % 求误差
```

```matlab
t=1790:10:1980;
x=[3.9,5.3,7.2,9.6,12.9,17.1,23.2,31.4,38.6,50.2,62.9,76,92,106.5,123.2,131.7,150.7,179.3,204.0,226.5];
x1=x(1);x2=x(14);
t1=t(1);t2=t(14);
A=[1,t1;1,t2];
b=[log(x1);log(x2)];
u=A\b;
x0=exp(u(1))
k=u(2)
error=sum((x0*exp(k*t)-x).^2)
```

4.5.2

本题与学号相关！！！

```matlab
t=1790:10:1900; % 1850+10w = 1950 具体决定于你的w
x=[3.9,5.3,7.2,9.6,12.9,17.1,23.2,31.4,38.6,50.2,62.9,76]; % 最后一位为1900年的人口
X=log(x);
p=polyfit(t,X,1);
k=p(1)
x0=exp(p(2))
sum((x0*exp(k*t)-x).^2)
```

### 4.6

P195

```matlab
x=1:26; % 1-26天
y=[1807,2001,2158,2305,2422,2601,2753,2914,3106,3303,3460,3638,3799,3971,4125,4280,4409,4560,4698,4805,4884,4948,5013,5086,5124,5163]; % 1-26天感染人数
a=[6000,2,0.01]; % Xm, r, k的初始参数
f=@(a,x)a(1)./(1+a(2)*exp(-a(3)*x)); % 函数模型
[A,resnorm]=lsqcurvefit(f,a,x,y) % 从a开始，求取合适的x，使非线性函数f对数据y的拟合最佳(基于最小二乘)，y必须与f返回的向量F大小相同
% A(1) 是Xm  A(2)是r   A(3)是k
t=26;
xt1=A(1) / (1+A(2) * exp( -A(3)*t ) )
xt2=A(1) / ( 1 + A(2) *exp (- A(3) * (t+1) ))
while    abs( xt1- xt2 )>1 % 人数之差大于1
    t=t+1;
end
t
A(1)/(1+A(2)*exp(-A(3)*(t+1)))
t=1:50;
plot(x,y,'*')
hold on;
plot(t,A(1)./(1+A(2)*exp(-A(3)*t)));
```

```matlab
x=1:26;
y=[1807,2001,2158,2305,2422,2601,2753,2914,3106,3303,3460,3638,3799,3971,4125,4280,4409,4560,4698,4805,4884,4948,5013,5086,5124,5163];
a=[6000,2,0.1];
f=@(a,x)a(1)./(1+a(2)*exp(-a(3)*x));
[A,resnorm]=lsqcurvefit(f,a,x,y)
t=26;
while    abs(A(1)/(1+A(2)*exp(-A(3)*t))-A(1)/(1+A(2)*exp(-A(3)*(t+1))))>1
    t=t+1;
end
t
A(1)/(1+A(2)*exp(-A(3)*(t+1)))
t=1:50;
plot(x,y,'*')
hold on;
 plot(t,A(1)./(1+A(2)*exp(-A(3)*t)));
```

### 4.7

```matlab
x=1:26;
y=[1807,2001,2158,2305,2422,2601,2753,2914,3106,3303,3460,3638,3799,3971,4125,4280,4409,4560,4698,4805,4884,4948,5013,5086,5124,5163];
a=[6000,2,0.1,0.1];
f=@(a,x)a(1)./(1+a(2)*exp(-a(3)*x-a(4)*x.^2));
[A,resnorm]=lsqcurvefit(f,a,x,y)
t=26;
while    abs(A(1)/(1+A(2)*exp(-A(3)*t-A(4)*t.^2))-A(1)/(1+A(2)*exp(-A(3)*(t+1)-A(4)*(t+1).^2)))>1
    t=t+1;
end
t
A(1)/(1+A(2)*exp(-A(3)*(t+1)-A(4)*(t+1).^2))
t=1:50;
plot(x,y,'*')
hold on;
plot(t,A(1)./(1+A(2)*exp(-A(3)*t-A(4)*(t+1).^2)));
```

### 4.8

```matlab
x=1:27;
y=[21,65,127,281,558,923,1321,1801,2420,3125,3886,4638,5306,6028,6916,7646,8353,9049,9503,10098,10540,10910,11287,11598,11865,12086,12251];
a=[13000,2,0.1];
f=@(a,x)a(1)./(1+a(2)*exp(-a(3)*x));
[A,resnorm]=lsqcurvefit(f,a,x,y)
t=27;
while    abs(A(1)/(1+A(2)*exp(-A(3)*t))-A(1)/(1+A(2)*exp(-A(3)*(t+1))))>1
    t=t+1;
end
t
A(1)/(1+A(2)*exp(-A(3)*(t+1)))
t=1:50;
plot(x,y,'*')
hold on;
plot(t,A(1)./(1+A(2)*exp(-A(3)*t)));
```

### 4.9

```matlab
x=1:27;
y=[21,65,127,281,558,923,1321,1801,2420,3125,3886,4638,5306,6028,6916,7646,8353,9049,9503,10098,10540,10910,11287,11598,11865,12086,12251];
a=[4,0.1,0.1];
f=@(a,x)a(1)*exp(exp(a(2)*x+a(3)));
[A,resnorm]=lsqcurvefit(f,a,x,y)
t=27;
while  abs(f(A,t)-f(A,t+1))>1
    t=t+1;
end
t
f(A,t+1)
t=1:50;
plot(x,y,'*')
hold on;
plot(t,f(A,t));
```
