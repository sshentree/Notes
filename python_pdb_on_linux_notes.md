# pdb 基于命令行调试python

说明：pdb基于命令行的调试工具，非常类似gnu的gdb（调试c/c++）

### 命令列表

| 命令              |   简写命令    | 作用                       |
| :---------------- | :-----------: | :------------------------- |
| break             |       b       | 设置断点                   |
| cintinue          |       c       | 继续执行程序               |
| list              |       l       | 查看当前行的代码段         |
| step              |       s       | 进入函数                   |
| return            |       r       | 执行代码直到从当前函数返回 |
| quit              |       q       | 终止并退出                 |
| next              |       n       | 执行下一行                 |
| print             |       p       | 打印变量的值               |
| help              |       h       | 帮助                       |
| args              |       a       | 参看传入参数               |
|                   |     回车      | 重复上一条命令             |
| break             |       b       | 显示所有断点               |
| break lineno      |   b lineno    | 在指定行设置断点           |
| break file:lineno | b file:lineno | 在指定文件的行设置断点     |
| clear num         |               | 删除指定断点               |
| bt                |               | 查看函数调用栈帧           |

### 演示

- __执行时调试__

  `python -m pdb 'file_name(.py)'`

  说明：pdb 是模块,测试代码：

  ```python
  def sum(a, b):
      result = a + b
      print('result=%d'%result)
  
  a = 100
  b = 200
  c = a + b
  
  sum(a, b)
  print(c)
  
  print('hello world')
  
  print('hello python')
  
  ```

  

  1. 正常执行：命令 `python 'file_name(.py)'`

     ```python
     PS C:\Users\SS沈\Desktop> python .\test01.py
     result=300
     300
     hello world
     hello python
     ```

  2. 调试执行并显示执行代码（每次显示11行，本人理解，显示多了，反而不容易查看）：命令 `python -m pdb 'file_name(.py)'`:调试、命令 `l`:查看当前执行代码

     ```python
     PS C:\Users\SS沈\Desktop> python -m pdb .\test01.py
     > c:\users\ss沈\desktop\test01.py(1)<module>()
     -> def sum(a, b):	# 表示下一步要执行的代码行
     (Pdb) l
       1  -> def sum(a, b):
       2         result = a + b
       3         print('result=%d'%result)
       4
       5     a = 100
       6     b = 200
       7     c = a + b
       8
       9     sum(a, b)
      10     print(c)
      11
     (Pdb) l	#命令 l （大写L）是显示代码命令
      12     print('hello world')
      13
      14     print('hello python')
     [EOF]   # 表示显示全部代码
     ```

  2. 向下执行一行代码：命令 `n` 

     ```python
     (Pdb) n
     > c:\users\ss沈\desktop\test01.py(5)<module>()
     -> a = 100	# -> 代表下一步要执行的代码
     (Pdb) l
       1     def sum(a, b):
       2         result = a + b
       3         print('result=%d'%result)
       4
       5  -> a = 100
       6     b = 200
       7     c = a + b
       8
       9     sum(a, b)
      10     print(c)
      11
     (Pdb)
     ```

  3. 继续执行代码（就像没有使用pdb调试一样）：命令 `c`

     ```python
     (Pdb) c
     result=300
     300
     hello world
     hello python	# 将程序执行完毕
     The program finished and will be restarted	# 程序执行完毕，将重新执行
     > c:\users\ss沈\desktop\test01.py(1)<module>()
     -> def sum(a, b):	# -》 表示下一步要执行的代码
     (Pdb)
     ```

  4. 添加断点（执行到指定行数停止）：命令 `b number_line`

     ```python
     The program finished and will be restarted
     > c:\users\ss沈\desktop\test01.py(1)<module>()
     -> def sum(a, b):
     (Pdb) b 7	# 添加一个断点（在7行处）
     Breakpoint 1 at c:\users\ss沈\desktop\test01.py:7
     (Pdb) c		# 使用 c 执行程序
     > c:\users\ss沈\desktop\test01.py(7)<module>()
     -> c = a + b
     (Pdb) l		# 显示当前执行代码
       2         result = a + b
       3         print('result=%d'%result)
       4
       5     a = 100
       6     b = 200
       7 B-> c = a + b	#程序执行到 7 行 停止(添加断点的意义)
       8
       9     sum(a, b)
      10     print(c)
      11
      12     print('hello world')
     (Pdb)
     ```

     说明：添加断点的意义，就是想针对查看某行的代码执行过程

  5. 查看断定：命令 `b`

     ```python
     (Pdb) b		# 查看断点
     Num Type         Disp Enb   Where
     1   breakpoint   keep yes   at c:\users\ss沈\desktop\test01.py:7	# 第一个断点是‘文件’的第7行
             breakpoint already hit 1 time
     (Pdb) b 9	# 将第9行添加一个断点
     Breakpoint 2 at c:\users\ss沈\desktop\test01.py:9
     (Pdb) c		# 执行
     > c:\users\ss沈\desktop\test01.py(9)<module>()
     -> sum(a, b)
     (Pdb) l		# 显示当前执行代码
       4
       5     a = 100
       6     b = 200
       7 B   c = a + b
       8
       9 B-> sum(a, b) 	# -> 表示下一步执行的代码
      10     print(c)
      11
      12     print('hello world')
      13
      14     print('hello python')
     (Pdb) b
     Num Type         Disp Enb   Where
     1   breakpoint   keep yes   at c:\users\ss沈\desktop\test01.py:7
             breakpoint already hit 1 time
     2   breakpoint   keep yes   at c:\users\ss沈\desktop\test01.py:9	# 第二个断点是第9行
             breakpoint already hit 1 time
     (Pdb)
     ```

  6. 清除断点：命令：`clear num`

     ```PYTHON
     (Pdb) b
     Num Type         Disp Enb   Where
     1   breakpoint   keep yes   at c:\users\ss沈\desktop\test01.py:7
             breakpoint already hit 1 time
     2   breakpoint   keep yes   at c:\users\ss沈\desktop\test01.py:9
             breakpoint already hit 1 time
     (Pdb) clear 1 	# 清除断点 1
     Deleted breakpoint 1 at c:\users\ss沈\desktop\test01.py:7
     (Pdb) b 	 	# 显示断点
     Num Type         Disp Enb   Where
     2   breakpoint   keep yes   at c:\users\ss沈\desktop\test01.py:9 	# 断点 1 被删除
             breakpoint already hit 1 time
     (Pdb)
     ```

  7. 进入函数执行（在使用命令：`n` 时，调用函数是当作一行语句来执行的）：命令 `s`

     ```python
     (Pdb) b 	# 查看断点
     Num Type         Disp Enb   Where
     1   breakpoint   keep yes   at c:\users\ss沈\desktop\test01.py:7
     2   breakpoint   keep yes   at c:\users\ss沈\desktop\test01.py:9
     (Pdb) clear 1 	# 清除断点 1
     Deleted breakpoint 1 at c:\users\ss沈\desktop\test01.py:7
     (Pdb) c 	# 执行程序，没有断点一次性执行完毕，如有断点，再断点处停止
     > c:\users\ss沈\desktop\test01.py(9)<module>()
     -> sum(a, b) 	# 当前下一步要执行的程序
     (Pdb) l 	 	# 查看当前执行代码
       4
       5     a = 100
       6     b = 200
       7     c = a + b
       8
       9 B-> sum(a, b) 	# 断点 -> 表示在此断点停止执行
      10     print(c)
      11
      12     print('hello world')
      13
      14     print('hello python')
     (Pdb) s 	# 进入函数执行代码
     --Call-- 	# 调用函数
     > c:\users\ss沈\desktop\test01.py(1)sum()
     -> def sum(a, b): 	# 回到函数定义语句，可直观查看函数执行
     (Pdb) l
       1  -> def sum(a, b):
       2         result = a + b
       3         print('result=%d'%result)
       4
       5     a = 100
       6     b = 200
       7     c = a + b
       8
       9 B   sum(a, b)
      10     print(c)
      11
     (Pdb) p a 	# 查看传入函数的参数：查看参数 a （print 打印）
     100
     (Pdb) p b 	# 查看参数 b
     200
     (Pdb) a 	# 查看所有参数（args）
     a = 100
     b = 200
     (Pdb)
     ```

  8. 进入函数的执行步骤：

     ```python
     (Pdb) n 	# n 的作用没有改变，每次执行一行
     > c:\users\ss沈\desktop\test01.py(3)sum()
     -> print('result=%d'%result) # 当前下一步要执行的代码
     (Pdb) l
       1     def sum(a, b):
       2         result = a + b
       3  ->     print('result=%d'%result) # 执行下一步要执行的代码
       4
       5     a = 100
       6     b = 200
       7     c = a + b
       8
       9 B   sum(a, b)
      10     print(c)
      11
     (Pdb) p result 	# 打印参数 result
     300
     (Pdb) n
     result=300
     --Return-- 	# 表示函数执行结束，有返回值就返回
     > c:\users\ss沈\desktop\test01.py(3)sum()->None
     -> print('result=%d'%result) 	# 没有仔细测试，感觉就是函数的最后一行
     (Pdb) n
     > c:\users\ss沈\desktop\test01.py(10)<module>()
     -> print(c) 	# 下一步要执行的代码
     (Pdb) l
       5     a = 100
       6     b = 200
       7     c = a + b
       8
       9 B   sum(a, b)
      10  -> print(c)
      11
      12     print('hello world')
      13
      14     print('hello python')
     [EOF]
     (Pdb)
     ```

  9. 快速执行到函数最后一行：命令 `r`

     ```python
     (Pdb) s 	# 进入函数执行命令
     --Call--
     > c:\users\ss沈\desktop\test01.py(1)sum()
     -> def sum(a, b):
     (Pdb) l
       1  -> def sum(a, b):
       2         result = a + b
       3         print('result=%d'%result)
       4
       5     a = 100
       6     b = 200
       7     c = a + b
       8
       9 B   sum(a, b)
      10     print(c)
      11
     (Pdb) r 	# 快速执行导函数最后一行
     result=300
     --Return-- 	# 表示函数执行结束
     > c:\users\ss沈\desktop\test01.py(3)sum()->None
     -> print('result=%d'%result)
     (Pdb) n
     > c:\users\ss沈\desktop\test01.py(10)<module>()
     -> print(c)
     (Pdb)
     ```

  10. 退出调试：命令 `q`

      ```python
      (Pdb) q
      PS C:\Users\SS沈\Desktop>
      ```

- __交互式调用__

  1. 进入python或ipython解释器

     ```python
     >>> def sum(a, b):
     	result = a + b
     	print('result=%d'%result)
     	return result
     
     >>> import pdb 	# 导入pdb模块
     >>> pdb.run('sum(3, 5)') 	# 调试函数，参数赋值
     > <string>(1)<module>()
     (Pdb) s 	# 使用 s 待用函数
     --Call--
     > <pyshell#12>(1)sum()
     (Pdb)
     ```

     说明:命令与上面相同，本人理解就是一个函数的调试，完全可以使用上面进行调试

- __程序里埋点__

  说明：导入 `import pdb` 在断点处写入 pdb.set_track() 。在 `sum(a, b)` 处加入断点，程序遇到 `pdb.set_trace` 就进入调试模式

  ```python
  import pdb
  
  
  def sum(a, b):
      result = a + b
      print('result=%d'%result)
  
  a = 100
  b = 200
  c = a + b
  
  # 添加断点
  pdb.set_trace()
  
  sum(a, b)
  print(c)
  
  print('hello world')
  
  print('hello python')
  
  ```

  2. 演示

     ```python
     > c:\users\ss沈\desktop\ten\爬虫\test02.py(15)<module>()
     -> sum(a, b)
     (Pdb) l
      10     c = a + b
      11
      12     # 添加断点
      13     pdb.set_trace()
      14
      15  -> sum(a, b)
      16     print(c)
      17
      18     print('hello world')
      19
      20     print('hello python')
     (Pdb) s
     --Call--
     > c:\users\ss沈\desktop\ten\爬虫\test02.py(4)sum()
     -> def sum(a, b):
     (Pdb)
     ```

     说明：其他命令和上面一样

- __日志调试__

  待续-----------

  

