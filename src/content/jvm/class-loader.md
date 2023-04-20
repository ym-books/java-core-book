# 类加载机制


Java类加载机制是Java虚拟机(JVM)在运行Java程序时所采用的一种动态加载机制，它将Java类文件从磁盘或网络加载到内存中，并在运行时动态链接并初始化类及其依赖项。这个过程主要分为三个步骤：加载、链接和初始化。

1. 加载（Loading）：加载是指在JVM内部查找并加载类的二进制数据，并创建出对应的Class对象。JVM在加载时会根据类的全限定名来查找类文件，查找路径包括JVM内置的Bootstrap ClassLoader、Extension ClassLoader和System ClassLoader三个ClassLoader，如果找不到会抛出ClassNotFoundException异常。

2. 链接（Linking）：链接是指将加载的类与JVM运行时环境进行链接，包括以下三个步骤：

(1) 验证(Verification)：验证类的二进制数据的正确性，包括检查文件格式、语法、语义等，以确保其符合JVM规范。

(2) 准备(Preparation)：为类变量（static变量）分配内存并设置默认值，如int类型默认为0，对象类型默认为null。

(3) 解析(Resolution)：将符号引用解析为直接引用，即将类、接口、字段和方法的引用转化为直接引用，以便能够正确访问它们。

3. 初始化（Initialization）：初始化是类加载过程的最后一个阶段，它负责执行类构造器<clinit>()方法，该方法是由编译器自动收集类中所有静态变量的赋值动作和静态语句块中的语句合并产生的，如果类中存在父类，它将按照父类到子类的顺序执行。如果一个类没有显示声明<clinit>()方法，则认为该类的<clinit>()方法是空的。

在Java虚拟机中，类的生命周期与类加载器相对应，当类加载器被垃圾回收时，它所加载的所有类也会被回收，从而结束类的生命周期。

_*所以，类的生命周期：*_
>Java类的生命周期包括以下五个阶段：
>1. 加载（Loading）：在这个阶段，Java类的二进制数据被加载到JVM中，并转化成一个Class对象。Java类的加载是通过ClassLoader来完成的，ClassLoader是负责从文件系统、网络或其他来源加载类的Java API。当JVM需要一个类时，ClassLoader会检查该类是否已经被加载，如果没有，则尝试加载它。
>
>2. 验证（Verification）：在这个阶段，Java类的二进制数据被验证以确保它们符合JVM规范，并且没有被篡改或损坏。验证的目的是确保加载的类不会破坏JVM的安全性、完整性和稳定性。
>
>3. 准备（Preparation）：在这个阶段，JVM为类的静态变量分配内存，并将它们初始化为默认值。例如，整型变量的默认值为0，布尔型变量的默认值为false。
>
>4. 解析（Resolution）：在这个阶段，JVM将类中的符号引用解析为实际的内存地址，以便能够访问类的方法和变量。例如，如果一个类包含一个对另一个类的引用，JVM将查找并定位该类的实际内存地址。
>
>5. 初始化（Initialization）：在这个阶段，JVM执行类的初始化代码，包括静态变量的赋值和静态代码块的执行。这个阶段标志着类的生命周期的完成，类现在可以被程序使用。
>
>需要注意的是，Java类的生命周期是由ClassLoader来管理的，当一个类不再被使用时，ClassLoader将从JVM中卸载该类并释放相关的内存资源。

##  类加载器-ClassLoader

ClassLoader是Java虚拟机（JVM）的一个重要组成部分，它的主要作用是动态加载类到JVM中，并将类加载到内存中以便程序进行访问。

在Java中，ClassLoader主要有以下三种类型：

1. Bootstrap ClassLoader：也称为引导类加载器，它是JVM自带的ClassLoader，它负责加载JVM运行环境中的核心类库，如java.lang、java.util等类。

2. Extension ClassLoader：也称为扩展类加载器，它是Java平台中的第二个类加载器，它负责加载Java扩展库中的类。Java扩展库包括了JRE中的/lib/ext目录下的所有jar包，以及java.ext.dirs系统属性所指定的所有jar包。

3. System ClassLoader：也称为应用程序类加载器，它是Java平台中的第三个类加载器，它负责加载应用程序类路径（Classpath）中指定的所有类库。应用程序类路径是指在运行时通过java命令或者JVM启动参数指定的类路径，它包括了开发者自己编写的Java类以及第三方类库。

当JVM需要加载一个类时，它首先会委托当前线程的ClassLoader去加载该类，如果该ClassLoader无法找到该类，则会将加载请求委托给其父ClassLoader，以此类推，直到Bootstrap ClassLoader。如果所有的ClassLoader都无法加载该类，则会抛出ClassNotFoundException异常。

ClassLoader还具有动态加载类的特性，它可以在程序运行时动态地加载某个类，并将其加载到JVM内存中，这种特性为Java应用程序的扩展和更新提供了很大的灵活性。

### 双亲委托加载

委托加载也称为双亲委托模型，它是Java中ClassLoader的一种加载机制。委托加载的思想是将类的加载请求委托给父ClassLoader来完成，如果父ClassLoader无法加载该类，则再由当前ClassLoader来完成加载。

在Java中，ClassLoader都是通过委托加载来完成类的加载的，即ClassLoader在加载一个类时，会先将该请求委托给父ClassLoader，如果父ClassLoader无法加载该类，则该ClassLoader会自己去加载该类。

这种机制的优势在于能够避免类的重复加载，即当一个类已经被某个ClassLoader加载后，其子ClassLoader再次请求加载该类时，父ClassLoader会直接返回已经加载的Class对象，避免重复加载浪费内存和资源。

委托加载机制的另一个好处在于它能够保证Java类的安全性。由于ClassLoader都是从上到下依次尝试加载某个类，因此可以确保程序所依赖的类都是来自于同一个源，避免了恶意代码或者不安全的类被加载的情况发生。

总之，委托加载是Java中ClassLoader的一种重要的机制，它确保了Java类的加载的正确性、安全性和高效性。


### 自定义类加载器

当我们需要自定义ClassLoader时，一般需要注意以下几点：

1. 实现findClass()方法：在自定义ClassLoader中，我们需要重写findClass()方法来实现自己的类加载逻辑。一般情况下，我们可以从磁盘、网络或者其他来源读取类文件的字节码，然后使用defineClass()方法将其转化为Class对象。

2. 遵守双亲委托模型：自定义ClassLoader需要遵守ClassLoader的委托加载机制，即在findClass()方法中需要调用父ClassLoader的loadClass()方法来完成类的加载。如果父ClassLoader无法加载该类，则才使用自定义ClassLoader来加载该类。

3. 重写loadClass()方法：在重写loadClass()方法时，需要先调用findLoadedClass()方法判断该类是否已经被加载过，如果是，则直接返回Class对象。如果没有被加载，则根据类的全名调用自定义的findClass()方法进行加载。

4. 确定类的命名空间：ClassLoader的命名空间是由ClassLoader及其所有的祖先ClassLoader所组成的，因此我们需要确定自定义ClassLoader的命名空间。一般来说，我们可以将自定义ClassLoader所在的包名反转作为该ClassLoader的命名空间。

下面是一个示例代码，用于说明自定义ClassLoader的实现：

```java
public class MyClassLoader extends ClassLoader {

    private String classpath;

    public MyClassLoader(String classpath, ClassLoader parent) {
        super(parent);
        this.classpath = classpath;
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] data = getClassData(name);
        if (data == null) {
            throw new ClassNotFoundException();
        }
        return defineClass(name, data, 0, data.length);
    }

    private byte[] getClassData(String name) {
        String path = classpath + "/" + name.replace('.', '/') + ".class";
        InputStream is = null;
        try {
            ByteArrayOutputStream bos = new ByteArrayOutputStream();
            is = new FileInputStream(path);
            byte[] buffer = new byte[1024];
            int length = 0;
            while ((length = is.read(buffer)) != -1) {
                bos.write(buffer, 0, length);
            }
            return bos.toByteArray();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return null;
    }

    @Override
    protected Class<?> loadClass(String name, boolean resolve) throws ClassNotFoundException {
        synchronized (getClassLoadingLock(name)) {
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                try {
                    if (getParent() != null) {
                        c = getParent().loadClass(name);
                    }
                } catch (ClassNotFoundException e) {
                }
                if (c == null) {
                    c = findClass(name);
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }

}
```

在这个示例代码中，MyClassLoader继承了ClassLoader类，并重写了findClass()方法来实现自己的类加载逻辑。在loadClass


参考1： https://zhuanlan.zhihu.com/p/268637051
