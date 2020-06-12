##@xmlAccessorType

- @XmlAccessorType
类级别的注解。定义这个类中的何种类型需要映射到XML。解释起来有点拗口，可以通过它的属性值更好理解这个参数的意义。

 - 参数 value
 - 参数 value 可以接受4个指定值，这几个值是枚举类型，方便调用：

  - XmlAccessType.FIELD：映射这个类中的所有字段到XML
  - XmlAccessType.PROPERTY：映射这个类中的属性（get/set方法）到XML
  - XmlAccessType.PUBLIC_MEMBER：将这个类中的所有public的field或property同时映射到XML（默认）
  - XmlAccessType.NONE：不映射
