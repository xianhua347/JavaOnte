# File类

- Java文件类以抽象的方式代表文件名和目录路径名。该类主要用于文件和目录的创建、文件的查找和文件的删除等。
- File对象代**表磁盘中实际存在的文件和目录**。通过构造方法创建一个File对象。
- 通过给定的父抽象路径名和子路径名字符串创建一个新的File实例。

### 构造方法

```java
File(String pathname) //文件名
File(File parent, String child);//父类(File类型) + 子文件夹
File(String parent, String child)//夫文件夹 + 子文件夹
File(URI uri);
```

### 创建文件方法

```java
file.createNewFile() //创建文件 返回值是Boolean
System.out.println(file.createNewFile());

file.mkdir() //创建文件夹 返回值是Boolean
File folder = new File("E:\\io_test\\javaEE");
System.out.println(folder.mkdir());

file.mkdirs()//创建文件夹不依赖父目录
File folders = new File("E:\\io_test\\javaEE\\html");
System.out.println(folders.mkdirs());
```

### 查询文件方法

```java
//判断类
System.out.println(file.isFile());
System.out.println(file.isDirectory());
System.out.println(file.exists());
//获取路径类
System.out.println(file.getAbsolutePath());
System.out.println(file.getPath());
System.out.println(file.getName());
//文件目录类
String[] list = file.list();
//返回文件目录 以数组方式存储
assert list != null;
for (String s : list) {
    System.out.println(s);
}
File[] files = file.listFiles();
//返回文件目录 以File类进行存储
for (File f1 : files) {
  if (f1.isFile() ||f1.isDirectory()){
     System.out.println(f1);
 }else{
  System.out.println("null");
}
```

### 删除文件

```java
public boolean delete();
//删除此抽象路径名表示的文件或目录
```

