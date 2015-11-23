# shell 数组操作

## 数组定义/赋值

#### 直接定义一个空的数组

```
declare -a arr
```

#### 通过列表赋值，默认下表是从0开始，当然可以执行下表来赋值

```
arr=('test1' 'test test' 'test2')
arr=([0]='centos' [2]='read hat' [4]='ubuntu')
```

#### 通过下表赋值，这种方式最直接

```
arr[2]='hello'
```

#### 通过列表赋值

```
arr2=({1..10})
echo ${arr2[@]}

输出结果为：
1 2 3 4 5 6 7 8 9 10
```

#### 通过数组赋值

```
arr1=('test1' 'test test')
arr2=('centos' 'read hat')
arr3=(${arr1[@]} ${arr2[@]})
arr4=("${arr1[@]}" "${arr2[@]}")
arr5=("${arr1[@]}" "hello")
arr6=("${arr1[@]:0:1}" "world")

echo "${arr1[@]}"
echo "${arr2[@]}"
echo "${arr3[@]}"
echo "${arr4[@]}"
echo "${arr5[@]}"
echo "${arr6[@]}"

执行结果为：
test1 test test
centos read hat
test1 test test centos read hat
test1 test test centos read hat
test1 test test hello
test1 world
```
这里的arr3和arr4并不相同，下面会提到


## 访问数组元素

#### 访问指定元素

格式：```${arr[index]}```

#### 访问所有元素
格式：```${array[@]}``` 或者 ```${array[*]}```

但这两种格式还是有所不同

```
arr=('test1' 'test test' 'hello')

for var in "${arr[@]}"
do
    echo $var
done

echo "------------------------------"

for var in ${arr[@]}
do
    echo $var
done

echo "------------------------------"

for var in "${arr[*]}"
do
    echo $var
done

echo "------------------------------"

for var in ${arr[*]}
do
    echo $var
done

输出如下：
test1
test test
hello
------------------------------
test1
test
test
hello
------------------------------
test1 test test hello
------------------------------
test1
test
test
hello
```
从上面例子可以看出

*  ${arr[@]}和${arr[*]}是一致的，都表示 arr[0] arr[1] ...
*  "${arr[*]}" 表示 "arr[0]" "arr[1]" ...
*  "${arr[*]}" 表示 "arr[0] arr[1] ..."

#### 访问所有元素下标
格式：```${!array[@]}``` 或者 ```${!array[*]}```

#### 访问区域元素
格式： ```${array[@]:pos1:pos2}```

#### 访问某个元素的指定区域
格式：```${array[index]:pos1:pos2}```

```
arr=('test1' 'test test' 'test2')
echo ${arr[0]}
echo ${array[@]}
echo ${array[@]:0:1}
echo ${array[1]:1:3}

执行结果为：
test1
test1 test test test2
test1 test test
est
```

## 数组长度

#### 整个数组的长度
格式：```${#array[@]}``` 或者 ```${#array[*]}```

#### 指定元素的长度
格式 ```${array[index]}```

```
arr=('test1' 'test test' 'test2')
echo ${#arr[@]}
echo ${#arr[1]}

执行结果为：
3
9
```

## 数组操作

#### 添加数组元素
格式： ```arr=("${arr[@]}" "string" array ...)```

#### 替换元素
格式： ```arr=${"arr[@]"/str1/str2}```  其中str1可以是正则表达式

```
arr=('test1' 'test test' 'hello')
arr1=(${arr[@]/test/aaaa})
arr2=(${arr[@]/test t*/aaaa})
echo "${arr1[@]}"
echo "${arr2[@]}"
```

#### 删除数组元素
格式：unset arr[index]

#### 模式删除数组元素
格式：  ```arr=${"arr[@]"/str1/}``` 其中str1可以是正则表达式

#### 删除整个数组
格式： ```unset array```

```
arr=('abcd' 'bcde' 'cdef' 'defg' 'efgi')
unset arr[0]
echo "${arr[@]}"
arr=(${arr[@]/*de*/})
echo "${arr[@]}"
unset arr
echo "${arr[@]}"

结果如下：
bcde cdef defg efgi
efgi

```

## 数组的循环遍历

#### 通过值遍历
格式：

```
arr=('test1' 'test test' 'hello')

#  通过值遍历
for var in "${arr[@]}"
do
    echo $var
done

echo "------------------------------"

# 通过下标遍历
for i in ${!arr[@]}
do
    echo $i ${arr[$i]}
done

echo "------------------------------"

i=0
while [ $i -lt ${#arr[@]} ]
do
    echo $i ${arr[$i]}
    let i++
done
```


## 二维数组

```
arr=('a b c' 'd e f' 'h i j')
arr1=('a b c')
arr2=('d e f')
arr3=("${arr1[@]}" ${arr2[@]})
echo ${#arr3[@]}
echo ${arr3[0]}
echo ${arr3[1]}

echo "**********************"
for i in "${arr[@]}"
do
    var=$i
    echo -n ${#var[@]}" "
    for j in ${var[@]}
    do
        echo -n "$j "
    done
    echo
done
echo "**********************"
for i in "${arr3[@]}"
do
    var=$i
    echo -n ${#var[@]}" "
    for j in ${var[@]}
    do
        echo -n "$j "
    done
    echo
done

输出结果：
4
a b c
d
**********************
1 a b c
1 d e f
1 h i j
**********************
1 a b c
1 d
1 e
1 f
```