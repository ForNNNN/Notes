### 一切皆对象

python中一切皆对象，但是由于小整数缓存机制的存在，对于 int，string，bool这几种数据类型，在一定范围内赋值给变量时，并不会重新创建对象，而是使用已经创建好的缓存对象。

1. int的小整数池是[-5,256]

   ```python
   a = 500
   b = 500
   c = 2
   d = 2
   print(a is b, c is d)
   
   False True
   ```

2. string会在以下情况不创建对象：

   - 字符串的长度为0或者1
   - 字符串的长度>1,且只含有大小写字母，数字，下划线时
   - 用乘法得到的字符串

3. bool只有True，False这两个对象，无论创建多少个变量指向True，False，都不会额外产生对象

以上是在终端的情况，在编辑器中也会有自己的缓存机制，所以会出现哪怕不在以上范围内仍不创建对象的情况。但是对于python本身而言，以上是符合的。

java中除了8种基本数据类型外一切皆对象，即对于基本数据类型都不会在堆空间创建对象

***

### 变量的声明和赋值

#### 变量声明在java与python中的区别

```python
a = 3
a = 4
print(a)

4
```

```java
public class Test1 {
    public static void main(String[] args) {
        int a = 3;
        int a = 4;
        System.out.println(a);
    }
}
 
error: variable a is already defined in method main(String[])
```

在python中，同名变量会直接覆盖掉之前的地址
在java中，int a 相当于声明一个变量a，由于已经存在一个变量a就不能再声明一个a，因此会报错；

***

#### 数据类型

如果在运算中没有声明数据类型，那么整数默认的数据类型是int；浮点数默认的数据类型是double

![02a30584b81301520637d3b982d0bede](C:\Users\yueya\Nutstore\1\我的坚果云\graph\02a30584b81301520637d3b982d0bede.png)

##### 数据类型间的运算

![5f55de57c32a87d59ef581f39d0a92e6](C:\Users\yueya\Nutstore\1\我的坚果云\graph\5f55de57c32a87d59ef581f39d0a92e6.png)

- 有多种类型的数据混合运算时，系统首先自动将所有数据转换成容量最大的那种数据类型，然后再进行计算。
- byte,short,char之间不会相互转换，他们三者互相在计算时首先转换为int类型。
- boolean类型不能与其它数据类型运算。
- 当把任何基本数据类型的值和字符串(String)进行连接运算时(+)，基本数据类型的值将自动转化为字符串(String)类型

***

```java
public class Test1 {
    public static void main(String[] args) {
        int a = 10;
        int b = 3;

        short c = a/b;
        System.out.println(c);
    }
}

error: incompatible types: possible lossy conversion from int to short
```

由于a为int，b为int，而且a和b参与了运算：a/b也为int；自然不能将int类型赋给short类型

```java
public class Test1 {
    public static void main(String[] args) {
        short c = 10/3;
        System.out.println(c);
    }
}

3
```

这里是将10/3=3，3这个数赋给了short。注意虽然3的默认数据类型是int，但由于3并没有超过short的范围，所以可以用short来接收。

通过以上这两个例子可知，参与运算的数据类型不再是默认的，必须要用其相应的数据类型来接收；而默认的数据类型int只要该数没有超过范围就可以用范围内的数据类型变量来接收。

***

##### 类型转换

###### 强制类型转换

从高级变量转换成低级变量时必须要进行强制类型转换

```java
public class Test1 {
    public static void main(String[] args) {
        int a = 128;
        byte b = (byte)a;
        System.out.println(b);
    }
}

-128
```

int 128在二进制中的储存形式为：0000 0000 0000 0000 0000 0000 1000 0000
将其强制转换成byte也就是1个字节需要将前24个bit全部丢弃，只留最后8个bit：1000 0000
由于第一位是符号位所以对应的byte为：-128

###### 自动类型转换

从低级变量转换成高级变量可以用自动类型转换

```java
public class Test1 {
    public static void main(String[] args) {
        double c = 3/2;
        System.out.println(c);
    }
}

1.0
```

3是int，2是int，因此3/2=1 仍然是int类型；将int类型赋予double属于从低级变量转换成高级变量不需要强制类型转换

***

#### 变量声明的两种特殊情况

1. long

   只要该变量的实际范围没有超过int，那么在数字末尾加l/L不是必须的，java会自动将其转换成int

   ```java
   public class Test1 {
       public static void main(String[] args) {
           long a = 1234;
           System.out.println(a);
       }
   }
   
   1234
   ```

2. float

   由于小数默认的类型是double，因此定义float时必须在数字末尾f/F，否则会报错

#### 内存解析

1. 基本类型在成员变量和局部变量的时候其内存分配机制是不一样的

   如果是成员变量，基本类型和引用类型都是在java的堆内存里面分配空间，而局部变量的基本类型是在栈上分配的。栈属于线程私有的空间，局部变量的生命周期和作用域一般都很短，为了提高效率，所以没必要放在堆里面

   ```java
   public class DemoTest {
       int y;// 分布在堆上
       
       public static void main(String[] args) {
           int x=1; //分配在栈上
       }
   
   }
   ```
   
2. 在java里面通过new出来的对象都在堆上分配，这里有两种特殊情况，

   - 字符串的字面量

     字符串的字面量，没有new关键字，但却是在堆上分配内存的，严格的说是在堆里面的字符串常量池里面

   - 基本类型的包装类

     同样，针对各个基本类型的包装类型，如：Integer，Double，Long等，这些属于引用类型，我们直接在局部方法里面使用包装类型赋值，那么数据真正的内存分配还是在堆内存里面，这里有个隐式的拆装箱来自动完成转换，数据的指针是在栈上，包装类型的出现主要是为了基本类型能够用在泛型的设计上和使用null值，而基本类型则拥有更好的计算性能
     
     ```java
     public class DemoTest {
     
         public static void main(String[] args) {
             String name=new String("cat");//数据在堆上，name变量的指针在栈上
             String address="北京";//数据在常量池，属于堆空间，address变量的指针在栈上
             Integer price=4;//包装类型同样是引用类型，编译时会自动装拆相，所以数据在堆上，指针在栈
         }
     
     }
     ```
     
     

***

### 运算规则在java与python中的区别

#### boolean的运算规则

```python
a = True
b = 1
print(a+b)

2
```

```java
public class Test1 {
    public static void main(String[] args) {
        boolean a = true;
        int b = 1;
        System.out.println(a);
    }
}

error: bad operand types for binary operator '+'
```

python的boolean类型可以参与四则运算，但java的boolean不行
而且python中的boolean类型是开头字母大写：True，False；java中的boolean类型开头字母小写：true，false

***

```python
a = True
b = 1
print(a==b)

True
```

```java
public class Test1 {
    public static void main(String[] args) {
        int a = 1;
        boolean b = true;
        System.out.println(a==b);
    }
}

error: incomparable types: int and boolean
```

python中True和1，False和0直接划等号
java中true和1，false和0无法比较

***

#### 字符在python和java中的运算规则

字符在java中是用单引号而字符串是用双引号，python中字符与字符串是一样的

##### 非数字字符与数字间的运算

```python
a = 'a'
print(a+1)

TypeError: can only concatenate str (not "int") to str
```

```java
public class Test1 {
    public static void main(String[] args) {
        char a = 'a';
        int b = 1;
        System.out.println(a+b);
    }
}
 
98
```

python中的 字符串/字符 不能与数字进行运算，但是可以通过ord()函数将字符转换成ASCII编码再与数值运算
java可以直接让该字符所对应的ASCII编码与所有的数值类型做运算

***

##### 非数字字符与数字间的比较

```java

public class test1 {

    public static void main(String[] args) {
        int a = 10;
        double b = 10.0;
        System.out.println(a==b);

        char c = 65;
        double d = 65.0;
        System.out.println(c==d);
        System.out.println((char)c == 'A');
        System.out.println(c=='A');
    }
}

true
true
true
true
```

***

##### 数字字符与数字间的运算

```python
a = '3'
b = 3
print(a+b)

TypeError: can only concatenate str (not "int") to str
```

```java
public class Test1 {
    public static void main(String[] args) {
        int a = 3;
        char b = '3';
        System.out.println(a+b);
    }
}

54
```

python中的 字符串/字符 不能与数字进行运算，但是可以通过ord()函数将字符转换成ASCII编码再与数值运算
java可以直接让该字符所对应的ASCII编码与所有的数值类型做运算

***

#### 字符串在python和java中的运算规则

java中，String可以和8种基本数据类型做运算，且只能是连接运算：+，运算结果仍是String类型

##### 字符串与数字/字符间的运算

###### 相加

```java
public class Test1 {
    public static void main(String[] args) {
        int a = 3;
        String b = "aaa";
        System.out.println(a+b);
    }
}

3aaa
```

python中的字符串不能直接与数字拼接

***



```java
public class Test1 {
    public static void main(String[] args) {
        char a = 'b';
        String b = "aaa";
        int c = 3;
        System.out.println(a+b+c);
    }
}

baaa3
```

```python
a = 'b'
b = 'aaa'
print(a+b)

baaa
```

在java中无论是数字还是字符，与字符串直接相加都会生成一个拼接的字符串
在python中，只有字符串与字符串之间才能相加并生成一个拼接的字符串

###### 相乘

```java
public class Test1 {
    public static void main(String[] args) {
        int a = 4;
        String b = "aaa";
        System.out.println(a*b);
    }
}

error: bad operand types for binary operator '*'
```

```python
a = 4
b = 'aaa'
print(a*b)

aaaaaaaaaaaa
```

在java中，数字不能与字符串相乘
在python中，数字与字符串相乘相当于4个该字符串拼接在一起

***

#### 位运算

在计算机中，二进制是通过补码存储的。当涉及到二进制的向左或右进行位运算时，
左移n位相当于其对应的十进制乘以2^n；右移n位相当于其对应的十进制除2^n。

1. 如果是正数，其二进制的反码和补码与原码一样，因此当需要左移或者右移时是直接在原码的基础上移动
   - 左移（<<）：在低位用0来填补空缺的位置，此时就是位运算后的二进制补码/原码（注意：如果向左移动n位后使得新补码第一位与原补码的第一位不一样就会使符号变化）
   - 右移（>>）：在高位用0来填补空缺的位置，由于正数的补码就是原码所以此时已得到位运算后的二进制原码。
2. 如果是负数，其二进制的补码是先对原码取反（符号位不变）得到反码，再在反码的基础上+1得到补码
   - 左移（<<）：在低位用0来填补空缺的位置，此时就是位运算后的二进制补码/原码（注意：如果向左移动n位后使得新补码第一位与原补码的第一位不一样就会使符号变化）
   - 右移（>>）：在高位用1来填补空缺的位置，然后取反（符号位不变）再+1得到位运算后的二进制原码。

对于无符号右移（>>>）无论是正数还是负数，最高位都用0来填补空缺；没有无符号左移

***

自增（i++，++i）和自减（i--，--i）以及复合赋值运算符（+=，-=，*=，/=...）都不会改变变量的本身数据类型

```java
public class test1 {
    public static void main(String[] args) {
        short a = 2;
        a++;
        getType(a);
    }
    
    public static void getType(Object a) {	
        System.out.println(a.getClass());
    }

}

class java.lang.Short
```

```java
public class test1 {
    public static void main(String[] args) {
        short a = 2;
        a = a+1;  //incompatible types: possible lossy conversion from int to short
        getType(a);
    }
}
```

***

### for循环在java和python作用域的区别

```python
i = 2
for i in range(4):
	pass
print(i)

3
```

```java
squares = [value**2 for value in range(1,11)]           
print(value)

NameError: name 'value' is not defined
```

可以看到，在for循环内定义的变量在循环外仍然有效而且会覆盖掉循环外的同名变量；但是使用列表解析是没有效果的。
因此，外层 i 和 for 循环里面的 i 是处于一个作用域， 都是模块级别的变量属于同一个作用域。

```java
public class Test1 {
    public static void main(String[] args) {
        for (int i =0;i<3;i++){

        }
        System.out.println(i);
    }
}

error: cannot find symbol
```

但是java中for循环中定义的变量是无法在for外访问的

***

### 分支语句

#### if...else

#### switch...case...default

```java
public class test1 {

    public static void main(String[] args) {
        int age = 10;
        switch(age){
            case 18:
                System.out.println("adult");
            default:
                System.out.println("Child");
            case 10:
                System.out.println("I am ten");
        }
    }
}

I am ten
```

1. switch中的变量的数据类型必须是byte，short，char，int，String，枚举类型；不能是long，double，float，boolean
2. 当哪一个case符合时不仅会执行当前case中的代码块，会继续执行之后的case，直到结束或者在某个case中有break就跳出
3. default相当于if...else中的else，只有所有的case都不符合时才会执行default，而且default可以在任意位置。

***

### 数组

java中没有正式的序列的概念，但字符串，数组这些有索引的数据类型仍然与python中的序列类似
java中想要获取字符串的某个元素不可以直接获取需要通过charAt()方法；想要获取数组的元素才可以直接通过索引

```java
public class Test1 {
    public static void main(String[] args) {
        int[] a = new int[]{1,2,3};
        System.out.print(a[1]);
  }
}

2
```

```java
public class Test1 {
    public static void main(String[] args) {
        String a ="123";
        System.out.print(a[1]);
  }
}

error: array required, but String found
//通过a[1]这样的方式只有数组才能使用而不适用于字符串
```

***

java中，对于动态生成的数组如果没有赋值，系统会给该数组默认初始化：

1. 元素为基本数据类型的数组

   - 元素为整数类型的数组（包括byte short int long），默认值为0

   - 元素为浮点型的数组（float double），默认值为0.0

   - 元素为boolean型的数组默认值为false

   - 元素为char类型的数组，默认值为Ascii码为0的字符即null，注意null并不是空格，空格会在内存中占用字节而null不会：

     ```java
     public class Test1 {
         public static void main(String[] args) {
             char[] a = new char[1];
             if((char)0==a[0]){
                 System.out.println('s');
             }
       }
     }
     
     s
     ```

     可以看到 if((char)0==a[0]) 的结果是true，因为null的ASCII码是0因此在计算机中是用0来储存的，因此和数字0的值一样

2. 元素为引用数据类型的数组

   - 元素为String类型的数组默认值为null，注意并不是"null"

     ```java
     public class Test1 {
         public static void main(String[] args) {
             String[] a = new String[3];
             System.out.println(a[0]);
             System.out.println(a[0]==null);
             System.out.println(a[0]=="null");
     
             char[] b = new char[3];
             System.out.print(b[0]+"s");
       }
     }
     
     null
     true
     false
      s
     ```

   - 元素为数组的数组（二维数组）默认值为null，注意并不是"null"
   
     ```java
     public class Test1 {
         public static void main(String[] args) {
             int[][] a = new int[3][]; //int只是限定了数组元素为数组的元素的类型，数组的元素必定是数组
             System.out.println(a[0]);
         }
     }
     
     null
     ```
     
   - 元素为对象的数组（对象数组）默认值为null，注意并不是"null"
   
     ```java
     public class test1 {
         public static void main(String[] args) {
             Person p[] = new Person[3];  //注意这里new的不是Person实例，只是new了一个要求以Person实例作为元素的数组且长度为3
             System.out.println(p[0]);
         }
     }
     
     class Person{
         String name = "aaa";
         int age = 18;
     }
     
     null
     ```
   

****

如果直接打印指向数组的变量，得到的结果是地址，除了char型数组

```java
public class test1 {
    public static void main(String[] args) {
        int[] a = new int[]{1,2,3};
        System.out.println(a);
        char[] b = new char[]{'1','2','3'};
        System.out.println(b);
    }
}

[I@7699a589
123
```

因为println()有一个专门用来传递char型字符的重载方法，在该重载方法内封装了遍历char型数组的代码

***

### 方法在java和python中的区别

#### 重载

在python中，同一个类中方法名是唯一的，如果出现重名的方法哪怕形参的名字个数不同，后面的方法也会将前面的方法覆盖掉

在java中，同一个类中方法名不是唯一的，对于**同名方法**只要形参的**数据类型**，**类型顺序**，**形参个数**不一样，就允许出现多个同名方法即重载，是否满足重载与返回值的类型无关

#### 值传递和引用传递

**python是引用传递**即将对象的地址传入方法内而不是传递值，因此在函数中有可能改变原始对象

1. 传入可变对象，由于可变对象可以在不改变本身对象的情况下改变元素的值，所以在函数中有可能改变原始对象

   ```python
   a = [10,20]
   def test(a): #在函数创建了一个新的变量a
     a = [30,40]
   test(a) #将列表a的地址传入test函数内，并赋值给变量a；
   print(a) 
   
   [10,20]
   ```

   由于并没有直接在列表对象[10,20]本身的基础上进行操作而是创建了一个新的对象[30,40]并赋予了a，所以原始对象并没有被改变

   ```python
   a = [10,20]
   def test(a): #在函数创建了一个新的变量a
     a.append(30)
   test(a) #将列表a的地址传入test函数内，并赋值给变量a；
   print(a) 
   
   [10,20,30]
   ```

   由于是可变对象可以直接对该对象进行修改，此时是直接在列表对象[10,20]上进行添加元素，所以原始对象也被改变了

2. 不可变对象的传递是在函数中会创建新的对象空间，因此无论如何都无法改变原始对象

   ```python
   a = 10
   def test(a):
     a = a+1 #隐式地创建一个新的对象a，表示a与1的和并用变量a指向新对象a+1；对于不可变对象a=a+1和a+=1等价
   test(a)
   print(a)
   
   10
   ```

**java是值传递**即实际参数值的副本（copy）传入方法内，而参数本身不受影响

1. 基本数据类型的传递是将数据值传入

2. 引用数据类型的传递是将对象的地址值传入

   ```java
   public class test1 {
       public static void main(String[] args) {
           String a = new String("aaa");
           test(a);
           System.out.println(a);
   
           }
       static void test(String a){
           a = "bbb";
       }
   }
   
   aaa //a的值并没有变化
   ```


***

在java中，== 用来比较基本数据类型的值是否相等和引用数据类型的地址是否相等；
				 Array.equals() 用来比较两个**数组**的长度和元素的值和元素的顺序是否相等

```java
import java.util.Arrays;
public class test1 {
    public static void main(String[] args) {
        int[] a = new int[]{1,2,3};
        int[] b = new int[]{1,2,3};
        System.out.println(Arrays.equals(a,b));
        System.out.println(a==b);
    }
}

true
false
```

在python中，==用来比较两个变量的值是否相等；is 用来比较两个变量的地址是否相等

```python
a = [1,2,3,4]
b = a[:]
print(a==b)
print(a is b)

True
False
```

***

#### 常用的方法

##### 查看数据类型

1. 查看对象的数据类型（即查看对象是哪个类的实例）：object.getClass()

2. 查看基本数据的数据类型

   java中无法直接查看基本数据的数据类型，又因为必须得使用对象才能调用getClass()方法，所以必须得自己定义一个getType()方法来返回

   ```java
   public class test1 {
       public static void main(String[] args) {
           short a = 2;
           a++;
           getType(a);
       }
       public static void getType(Object a) {	
       //传进来的a是基本数据类型，这里是jvm的自动装箱将基本数据类型short转换成了包装类Short,此时short变量a自动转换成了		Short类的对象，又由于Short类是Object类的子类，所以对象a可以被接收因为多态性
           System.out.println(a.getClass());
       }
   
   }
   
   class java.lang.Short //可以看出自增并不会改变本身的类型
   ```


***

##### equals()方法

在Object类中定义的equals()方法相当于==，即比较对象的地址值是否相等
在其他类比如String，Date，File，包装类中对equals()方法进行了重写，比较的是两个对象的内容是否相等

***

### 类在java和python中的区别

python在声明变量时不需要定义数据类型，所以python中的数据类型没有默认值

java中可以只声明变量，然后在对其赋值，因此java的数据类型是有默认值的
在数组中，我们已经得知对于不同元素数据类型的数组其元素的默认值也不同

1. 成员变量（属性）

   在类中定义的变量称为成员变量即属性，这些变量的默认值以声明时定义的数据类型为准

2. 局部变量

   在方法，方法形参，构造器，构造器形参，循环等代码块内定义的变量称为局部变量，这些变量没有默认值，必须在声明时赋值

***

对于类中定义的方法（实例方法），python不可以在方法中调用类中定义的属性（类属性）；但是java可以

```java
public class test1 {
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.test();
        System.out.println(p1.name); //当前实例的name属性已经被赋值成sss

        System.out.println();

        p1.test2(30);
        System.out.println(p1.age);

    }
}

class Person{
    String name = "aaa";
    int age = 18;
    public void test(){
        System.out.println(name); //由于方法内没有定义name所以获取的是类中定义的属性name：aaa
        name = "sss"; 
        //注意String name = "sss" 代表在该方法中新定义了一个name变量；而此时是将"sss"直接给了类中的属性name，相当于给当前		   实例的属性name赋值了sss
        System.out.println(name); //name已经被赋予了新值所以打印结果为sss
    }

    public void test2(int age){
        age = 30 //对于age = age；这样并不能修改该对象的age属性，因为第一个age是在test2()方法内定义的参数而不是类属性age
        System.out.println(age);
    }
}

aaa
sss
sss

30
18
```

```python
class Person:
	name = 'aaa'
	def test(self):
		print(name)
p1 = Person()
p1.test()

NameError: name 'name' is not defined
```

***

对于类中定义的多个方法（实例方法），python不可以在实例方法中调用另一个实例方法；但是java可以

```java
public class test1 {
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.test();
        System.out.println(p1.age); 

        System.out.println();

        p1.test2(30);
        System.out.println(p1.age); //打印的值仍然是类中属性的默认值18，因为方法内的同名变量没有影响到类的属性值

    }
}

class Person{
    String name = "aaa";
    int age = 18;
    public void test(){
        test2(age);
    }

    public void test2(int age){   
        age = 19;	//和无参方法不同，这里的age是在test2括号内声明的变量而不是类属性age，即声明了和类中属性同名的变量
        System.out.println(age);
    }
    
19
18

19
18
```

```python
class Person:
	def test(self):
		print('aa')
		test2()
	def test2(self):
		print('ss')		
p1 = Person()
p1.test()

NameError: name 'test2' is not defined
```

***

### 封装

java中，通过给类属性设置private的方式，可以使得对象无法直接调用和修改该属性，只能在类中调用和修改该属性，因此对象必须通过类中的方法才能访问该属性
python中，通过给类属性前设置__属性的方式，可以

同理，通过给类中的方法设置private的方式，可以使得对象无法调用该方法，只能在类中调用该方法

***

### 继承

#### 属性的继承

如果子类中定义了和父类一样的属性，相当于创建了两个属性而不会覆盖，因为属性是不会被重写的

#### 方法的继承与重写

1. 父类中只有非static的方法才能被子类重写，而且子类中的重写的方法也必须是非static

   - 子类重写的方法名和形参列表与父类被重写的方法名和形参列表相同

   - 子类重写的方法的权限修饰符不小于父类被重写的方法的权限修饰符

   - 子类不能重写父类中声明为private权限的方法

   - 父类被重写的方法的返回值类型如果是void，则子类重写的方法的返回值类型也只能是void；

     父类被重写的方法的返回值类型如果是A类型，则子类重写的方法的返回值类型可以是A类型或者A类型的子类

     父类被重写的方法的返回值类型如果是基本数据类型，则子类重写的方法的返回值类型必须是相同的基本数据类型

   ```java
   package Practice;
   
   public class Parent {
       String name;
       int age;
   
       void walk(){
           System.out.println("I can walk");
           run(3);
       }
   
       private void run(int any){
           System.out.println("I can run");
       }
   }
   ```

   ```java
   package Practice;
   
   public class Child extends Parent {
   
       private void run(){
           System.out.println("I Can't run");
       }
   
       public static void main(String[] args) {
           Child c1 = new Child();
           c1.walk();
           c1.run();
       }
   }
   
   
   I can walk
   I can run
   I Can't run
   ```

2. 对于父类中的static方法，子类中也可以有一模一样方法名和形参列表的static方法，但这并不是重写

#### Super

1. 当父类和子类出现同样的属性后，如果子类仍想用父类中的同名属性可以用super来调用


```java
package Practice;

public class Parent {
    public String name="parent";
    protected int age;

    void walk(){
        System.out.println(this.name+" "+"I can walk");
    }

    private void run(int any){
        System.out.println("I can run");
    }
}
```

```java
package Practice;

public class Child extends Parent{
    public void show(){
        System.out.println(super.name) 
        System.out.println(this.name); //由于Child类没有name属性,this则去父类中调用；此时的效果等同于super.name
    }

    public static void main(String[] args) {
        Child c1 = new Child();
        System.out.println(c1.name);  //由于Child类中没有name属性，因此打印的是Parent类中的name属性
        c1.show(); 

    }
}

parent
parent
parent
```

2. 当父类的方法被重写后，如果子类仍然想用父类中被重写的方法可以通过super来调用

   - 在子类的构造器中的第一行必须调用自己的重载构造器This(参数列表)，如果没有重载构造器或者没有调用则默认调用父类的无参构造器super()

     ```java
     package Practice;
     
     public class Parent {
         public String name="parent";
         protected int age;
     
         Parent(String name){
             this.name = name;
         }
     
         Parent(){
             System.out.println("This is parent's constructor");
         }
     
     }
     ```

     ```java
     package Practice;
     
     public class Child extends Parent{
         String name;
         int age;
     
         Child(){
             //此时相当于调用super()即Parent()
         }
         
         Child(String name){
             //同样调用了super()即Parent()
             this.name = name;
         }
     
         Child(String name,int age){
             this(name); //由于这里调用了其他的重载构造器，所以不调用super()
             this.age = age;
         }
     
         public static void main(String[] args) {
             Child c1 = new Child(); //Child()构造器中没有调用其他的构造器，则调用父类的无参构造器super()即parent()
         }
     }
     
     This is parent's constructor
     ```

   - 如果在子类构造器中主动调用super()，同调用自己的重载构造器this(参数列表)一样也必须在第一行；因此在子类构造器中调用自己的重载构造器和调用super()不能同时调用

   - 子类中如果有n个构造器，那么最多只有n-1个构造器能使用this()调用其他的重载构造器，因此至少有一个构造器调用super()父类无参构造器

### 多态

多态的本质是父类的引用指向子类的对象，或者是子类的对象赋给父类的引用
**因此，多态的前提是继承，多态需要子类中对父类同名同参数方法的重写**

#### 子类对象的多态性只适用方法而不适用于属性

- 当调用子类和父类同名同参数的方法时，实际执行的是子类重写父类的方法即虚拟方法调用；对于子类独有的方法是不能调用的

  即通过对象的多态性创建实例时，只能调用父类中声明的方法，但是在运行的时候实际上执行的是子类重写父类的方法；

  只能调用父类中声明的属性，即使子类中有同名属性打印出来仍是父类的属性因为属性没有多态即没有重写

  即编译看左（父类），运行看右（子类）
  
  ```java
  package Practice;
  
  public class Animal {
      String name = "animal";
  
      public void yall(){
          System.out.println("I can yall");
      }
  
      public void sleep(){
          System.out.println("I can sleep");
      }
  
  
  }
  
  class Cat extends Animal{
      String name = "cat";
      public void yall(){
          System.out.println("I can miao");
      }
  }
  
  class Dog extends Animal{
      String name = "dog";
      public void yall(){
          System.out.println("I can bark");
      }
  }
  ```
  
  ```java
  package Practice;
  
  public class AnimalTest {
      public static void main(String[] args) {
          Animal ani =  new Cat();
          AnimalTest test = new AnimalTest();
          test.func(ani);
  
      }
  
      public void func(Animal animal){
          animal.yall();
          animal.sleep();
          System.out.println(animal.name); //尽管在Cat和Dog中定义了自己的name，但是打印出来还是Animal的name
      }
  }
  
  I can miao
  I can sleep
  animal
  ```
  
- 通过向下转型（强制类型转换）来调用子类中独有的属性和方法

  ![](C:\Users\yueya\Nutstore\1\我的坚果云\graph\Screenshot 2022-05-20 183114.png)
  
  ```java
  package Practice;
  
  public class Animal {
      String name = "animal";
  
      public void yall(){
          System.out.println("I can yall");
      }
  
      public void sleep(){
          System.out.println("I can sleep");
      }
  
  
  }
  
  class Cat extends Animal{
      String name = "cat";
      public void yall(){
          System.out.println("Cat can miao");
      }
  
      public void fun(){
          System.out.println("Cat can make people laugh");
      }
  }
  
  class Dog extends Animal{
      String name = "dog";
      public void yall(){
          System.out.println("Dog can bark");
      }
  
      public void swim(){
          System.out.println("Dog can swim");
      }
  
  ```
  
  ```java
  package Practice;
  
  public class AnimalTest {
      public static void main(String[] args) {
          Animal ani =  new Cat();
  
          Animal a = new Dog();
          System.out.println(a.name);
  
          Dog d = (Dog) a;
          d.swim();
          System.out.println(d.name);
  
      }
  
  }
  
  animal
  Dog can swim
  dog
  ```
  
  ssss
