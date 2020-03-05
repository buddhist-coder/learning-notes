# 一、Spring IoC容器和Bean简介

本章介绍了控制反转（IoC）原理的Spring框架实现。 IoC也称为依赖注入（DI）。 在此过程中，对象仅通过构造函数参数，工厂方法的参数或在构造或从工厂方法返回后在对象实例上设置的属性来定义其依赖项（即，与它们一起使用的其他对象） 。 然后，容器在创建bean时注入那些依赖项。 此过程从根本上讲是通过使用类的直接构造或诸如`Service Locator`模式之类的机制来控制其依赖项的实例化或位置的bean本身的逆过程（因此称为Control Inversion）。
`org.springframework.beans`和`org.springframework.context`包是Spring Framework的IoC容器的基础。 `BeanFactory`接口提供了一种高级配置机制，能够管理任何类型的对象。 `ApplicationContext`是`BeanFactory`的子接口,它增加了：

- 与Spring的AOP功能轻松集成
- 消息资源处理（用于国际化）
- 事件驱动
- 应用层特定的上下文，例如Web应用程序中使用的`WebApplicationContext`。

简而言之，`BeanFactory`提供了配置框架和基本功能，而`ApplicationContext`添加了更多企业特定的功能。 `ApplicationContext`是`BeanFactory`的完整超集，在本章中仅在`Spring`的`IoC`容器描述中使用。 有关使用`BeanFactory`而不是`ApplicationContext`的更多信息，请参见`BeanFactory`。
在`Spring`中，构成应用程序主干并由`Spring Io`C容器管理的对象称为`bean`。 `Bean`是由`Spring IoC`容器实例化，组装和以其他方式管理的对象。 否则，`bean`仅仅是应用程序中许多对象之一。 `Bean`及其之间的依赖关系反映在容器使用的配置元数据中。
