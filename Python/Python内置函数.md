

### **chr**

- **chr(Unicode)：将 Unicode 码点转换为对应的字符**

- ==中文编码范围 u4e00 ~ u9fa5==

- **示例**：

  ``` Python
  print(chr(97))       # 输出: 'a'
  print(chr(20013))    # 输出: '中'
  ```

### **ord**

- **ord(字符)：将字符转为对应的 Unicode 码点**

- **示例**：

  ``` Python
  print(ord('a'))      # 输出: 97
  print(ord('中'))     # 输出: 20013
  ```

### **input**

- **name = input()：从键盘读取输入，并保存在变量中**

- ==input 返回类型始终是 string 类型，若需数字需手动转换==

- **示例**：

  ``` Python
  age = input("请输入年龄: ") # 假设输入 18
  print(type(age))          # 输出: <class 'str'>
  real_age = int(age)       # 强制类型转换为整数
  ```

### **print**

- **print(objects, sep=' ', end='\n')**

- **示例**：

  ``` Python
  # 使用 sep 修改分隔符，使用 end 修改换行符
  print("python", "java", "c++", sep=" - ")  # 输出: python - java - c++
  print("hello", end="")                    # 不换行打印
  print("world")                            # 接在上面后面，输出: helloworld
  ```

### **range**

- **获取数字序列（左闭右开）**

- **示例**：

  ``` Python
  print(list(range(5)))          # [0, 1, 2, 3, 4]
  print(list(range(2, 5)))       # [2, 3, 4]
  print(list(range(1, 10, 2)))    # [1, 3, 5, 7, 9] (步长为2)
  ```

### **len**

- **统计容器元素个数**

- **示例**：

  ``` Python
  print(len("hello"))      # 输出: 5
  print(len([1, 2, 3]))    # 输出: 3
  ```

### **max、min**

- **统计容器最大/最小元素**

- **示例**：

  ``` Python
  nums = [10, 20, 30, 5]
  print(max(nums))    # 输出: 30
  print(min(nums))    # 输出: 5
  ```

### **sorted**

- **将给定容器排序（返回新列表）**

- ==排序结果都会变为列表对象==

- **示例**：

  ``` Python
  nums = [3, 1, 4, 2]
  new_nums = sorted(nums, reverse=True) # 倒序
  print(new_nums)                       # 输出: [4, 3, 2, 1]
  print(nums)                           # 原列表不变: [3, 1, 4, 2]
  ```

### **join**

- **'分隔符'.join(可迭代对象)**

- **示例**：

  ``` Python
  list_a = ["2025", "05", "04"]
  date = "-".join(list_a)
  print(date)  # 输出: "2025-05-04"
  ```

### **flush**

- **file.flush()：内容刷新，将缓冲区内容写入文件**

- **示例**：

  ``` Python
  f = open("test.txt", "w")
  f.write("Hello World")
  f.flush()  # 此时即便没 close，内容也已经写入硬盘文件了
  ```

### **seek**

- **file.seek(offset, whence)：移动文件指针**

- **示例**：

  ``` Python
  with open("test.txt", "r") as f:
      f.read(5)      # 读了5个字符，指针在5
      f.seek(0)      # 指针回到文件开头
      print(f.read()) # 重新从头读取
  ```

### **map**

- **map(function, iterable)：对可迭代对象中的每个元素执行函数操作**

- **示例**：

  ``` Python
  # 将列表中的字符串全部转为整数
  str_list = ["1", "2", "3"]
  int_list = list(map(int, str_list))
  print(int_list)  # 输出: [1, 2, 3]
  ```

### **enumerate**

- **enumerate(iterable, start=0)：同时获取索引和值**

- ==常用于两数之和（Two Sum）等需要索引的算法题==

- **示例**：

  ``` Python
  fruits = ["apple", "banana", "cherry"]
  for i, val in enumerate(fruits):
      print(f"编号 {i} 是 {val}")
  
  # 输出:
  # 编号 0 是 apple
  # 编号 1 是 banana
  # 编号 2 是 cherry
  ```

