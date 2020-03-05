# 六、自定义Bean的性质

Spring框架提供了许多接口，您可以使用这些接口来定制bean的性质。本节将它们分组如下：

- 生命周期回调
- ApplicationContextAware和BeanNameAware
- 其他Aware接口

## 6.1生命周期回调

要与容器对bean生命周期的管理交互，您可以实现Spring `InitializingBean`和`DisposableBean`接口。容器为前者调用`afterPropertiesSet（）`，为后者调用`destroy（）`，以便bean在初始化和销毁bean时执行某些操作。

JSR-250 `@PostConstruct`和`@PreDestroy`注解通常被认为是在现代Spring应用程序中接收生命周期回调的最佳实践。使用这些注解意味着您的bean不耦合到特定于Spring的接口。有关详细信息，请参阅使用`@PostConstruct`和`@PreDestroy`。
如果您不想使用JSR-250注释，但仍想消除耦合，请考虑`init-method`和`destroy-method` bean定义元数据。

在内部，Spring框架使用`BeanPostProcessor`实现来处理它可以找到并调用适当方法的任何回调接口。如果您需要自定义特性或Spring默认不提供的其他生命周期行为，您可以自己实现`BeanPostProcessor`。有关详细信息，请参见容器扩展点。

除了初始化和销毁回调之外，Spring管理的对象还可以实现`Lifecycle`接口，以便这些对象可以参与启动和关闭过程，由容器自身的生命周期驱动。

本节将介绍生命周期回调接口。

**初始化回调**
使用`org.springframework.beans.factory.InitializingBean`接口，容器在容器上设置了所有必需的属性后，就可以执行初始化工作。 `InitializingBean`接口指定一个方法：

```java
void afterPropertiesSet() throws Exception;
```

我们建议不要使用`InitializingBean`接口，因为它不必要地将代码耦合到Spring。或者，我们建议使用`@PostConstruct`注解或指定POJO初始化方法。对于基于XML的配置元数据，可以使用`init-method`属性指定具有`void-no-argument`签名的方法的名称。使用Java配置，可以使用@Bean的initMethod属性。请参阅接收生命周期回调。请考虑以下示例：

```xml
<bean id="exampleInitBean" class="examples.ExampleBean" init-method="init"/>
```

```java
public class ExampleBean {

    public void init() {
        // do some initialization work
    }
}
```

前面的示例与下面的示例（由两个清单组成）几乎具有完全相同的效果

```xml
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

```java
public class AnotherExampleBean implements InitializingBean {

    @Override
    public void afterPropertiesSet() {
        // do some initialization work
    }
}
```

然而，前面两个示例中的第一个并没有将代码耦合到Spring。

**销毁回调**
实现`org.springframework.beans.factory.DisposableBean`接口可以让bean在包含它的容器被销毁时获得回调。`DisposableBean`接口指定一个方法：

```java
void destroy() throws Exception;
```

我们建议不要使用`DisposableBean`回调接口，因为它不必要地将代码耦合到Spring。或者，我们建议使用`@PreDestroy`注解或指定bean定义支持的泛型方法。使用基于XML的配置元数据，可以在`<bean/>`上使用`destroy-method`属性。使用Java配置，可以使用@Bean的destroyMethod属性。请参阅接收生命周期回调。考虑以下定义：

```xml
<bean id="exampleInitBean" class="examples.ExampleBean" destroy-method="cleanup"/>
```

```java
public class ExampleBean {

    public void cleanup() {
        // do some destruction work (like releasing pooled connections)
    }
}
```

上述定义与以下定义具有几乎完全相同的效果：

```xml
<bean id="exampleInitBean" class="examples.AnotherExampleBean"/>
```

```java
public class AnotherExampleBean implements DisposableBean {

    @Override
    public void destroy() {
        // do some destruction work (like releasing pooled connections)
    }
}
```

但是，前面两个定义中的第一个并没有将代码耦合到Spring。

可以为`<bean>`元素的`destroy-method`属性分配一个特殊（推断）值，该值指示Spring自动检测特定bean类上的公共close或shutdown方法。（因此，实现`java.lang.AutoCloseable`或`java.io.Closeable`的任何类都将匹配。）还可以在`<beans>`元素的默认`destroy-method`属性上设置此特殊（推断）值，以将此行为应用于整个bean集（请参阅默认初始化和销毁方法）。注意，这是Java配置的默认行为。

**默认初始化和销毁方法**
在编写不使用特定于Spring的`InitializingBean`和`DisposableBean`回调接口的初始化和销毁方法回调时，通常会编写名称为`init（）`、`initialize（）`、`dispose（）`等的方法。理想情况下，这种生命周期回调方法的名称在整个项目中是标准化的，这样所有开发人员都使用相同的方法名称并确保一致性。

可以将Spring容器配置为“查找”每个bean上的命名初始化和销毁回调方法名。这意味着，作为应用程序开发人员，您可以编写应用程序类并使用名为`init（）`的初始化回调，而无需为每个bean定义配置`init method=“init”`属性。Spring IoC容器在创建bean时调用该方法（并根据前面描述的标准生命周期回调契约）。此功能还为初始化和销毁方法回调实施一致的命名约定。

假设初始化回调方法名为`init（）`，而销毁回调方法名为`destroy（）`。然后，您的类类似于以下示例中的类：

```java
public class DefaultBlogService implements BlogService {

    private BlogDao blogDao;

    public void setBlogDao(BlogDao blogDao) {
        this.blogDao = blogDao;
    }

    // this is (unsurprisingly) the initialization callback method
    public void init() {
        if (this.blogDao == null) {
            throw new IllegalStateException("The [blogDao] property must be set.");
        }
    }
}
```

然后可以在类似于以下内容的bean中使用该类：

```xml
<beans default-init-method="init">

    <bean id="blogService" class="com.something.DefaultBlogService">
        <property name="blogDao" ref="blogDao" />
    </bean>

</beans>
```

顶层元素属性上的`default-init-method`属性的存在导致Spring IoC容器将bean类上名为`init`的方法识别为初始化方法回调。在创建和组装bean时，如果bean类有这样的方法，则在适当的时间调用它。
您可以使用顶级`<beans/>`元素上的`default-init-method`属性来类似地配置`destroy`方法回调（在XML中，也就是说）。

如果现有的bean类已经有了不符合约定的回调方法，那么可以通过使用`init-method`，`destroy-method`指定（在XML中，也就是说）方法名来重写默认值，并销毁`<bean/>`本身的方法属性。

Spring容器保证在bean提供所有依赖项之后立即调用已配置的初始化回调。因此，对原始bean引用调用初始化回调，这意味着AOP拦截器等尚未应用于bean。首先完全创建一个目标bean，然后应用一个带有拦截器链的AOP代理（例如）。如果目标bean和代理分别定义，那么代码甚至可以绕过代理与原始目标bean交互。因此，将拦截器应用于init方法是不一致的，因为这样做会将目标bean的生命周期耦合到其代理或拦截器，并且在代码直接与原始目标bean交互时留下奇怪的语义。

**组合生命周期机制**
从Spring2.5开始，您有三个控制bean生命周期行为的选项：

- `InitializingBean`和`DisposableBean`回调接口
- 自定义`init（）`和`destroy（）`方法
- `@PostConstruct`和@`PreDestroy`注解。您可以结合这些机制来控制给定的bean

如果为一个bean配置了多个生命周期机制，并且每个机制都配置了不同的方法名，那么每个配置的方法都将按照本说明后面列出的顺序执行。但是，如果相同的方法名配置为，例如，初始化方法的init（）用于这些生命周期机制中的多个，则该方法将执行一次，如前一节所述。

为同一bean配置的多个生命周期机制，使用不同的初始化方法，调用如下：

1. 用`@PostConstruct`注解的方法
2. 由`InitializingBean`回调接口定义的`afterPropertieSet（）`
3. 自定义配置的`init（）`方法

销毁方法的调用顺序相同：

1. 用@PreDestroy注解的方法
2. 由`DisposableBean`回调接口定义`destroy（）`
3. 自定义配置的`destroy（）`方法

**启动和关闭回调**
Lifecycle接口定义了任何具有自身生命周期需求的对象（例如启动和停止某些后台进程）的基本方法：

```java
public interface Lifecycle {

    void start();

    void stop();

    boolean isRunning();
}
```

任何Spring管理的对象都可以实现生命周期接口。然后，当`ApplicationContext`本身接收到启动和停止信号（例如，对于运行时的停止/重新启动场景）时，它将这些调用级联到该上下文中定义的所有生命周期实现。它通过委托给`LifecycleProcessor`来完成此任务，如下所示：

```java
public interface LifecycleProcessor extends Lifecycle {

    void onRefresh();

    void onClose();
}
```

请注意，常规`org.springframework.context.Lifecycle`接口是显式启动和停止通知的简单约定，并不意味着在上下文刷新时自动启动。要对特定bean的自动启动（包括启动阶段）进行细粒度控制，请考虑改为实现`org.springframework.context.SmartLifecycle`。
另外，请注意，停止通知不能保证在销毁之前发出。在定期关机时，所有生命周期bean首先收到停止通知，然后再传播常规销毁回调。但是，在上下文生存期内的热刷新或中止的刷新尝试时，只调用destroy方法。

启动和关闭调用的顺序可能很重要。如果任何两个对象之间存在“依赖”关系，则依赖方在其依赖项之后开始，并在其依赖项之前停止。但是，有时，直接依赖关系是未知的。您可能只知道某个类型的对象应该在另一个类型的对象之前开始。在这些情况下，`SmartLifecycle`接口定义了另一个选项，即在其超级接口上定义的`getPhase（）`方法。下表显示了分阶段接口的定义：

```java
public interface Phased {

    int getPhase();
}
```

下面的列表显示了SmartLifecycle接口的定义：

```java
public interface SmartLifecycle extends Lifecycle, Phased {

    boolean isAutoStartup();

    void stop(Runnable callback);
}
```

启动时，阶段值最低的对象先启动。停止时，按相反的顺序。因此，实现`SmartLifecycle`且其`getPhase（）`方法返回`Integer.MIN_VALUE`的对象将是最先开始和最后停止的对象之一。`Integer.MAX_VALUE`的阶段值将指示对象应最后启动并首先停止（可能是因为它依赖于要运行的其他进程）。在考虑阶段值时，还必须知道任何未实现`SmartLifecycle`的“正常”生命周期对象的默认阶段为0。因此，任何负阶段值都表示对象应该在这些标准组件之前开始（并在它们之后停止）。对于任何正阶段值，反之亦然。

`SmartLifecycle`定义的`stop`方法接受回调。任何实现都必须在该实现的关闭过程完成后调用该回调的`run（）`方法。这将在必要时启用异步关闭，因为`LifecycleProcessor`接口的默认实现`DefaultLifecycleProcessor`将等待每个阶段中的对象组调用该回调的超时值。每个阶段的默认超时为30秒。通过在上下文中定义一个名为`lifecycle processor`的bean，可以覆盖默认的生命周期处理器实例。如果只想修改超时，定义以下内容就足够了：

```xml
<bean id="lifecycleProcessor" class="org.springframework.context.support.DefaultLifecycleProcessor">
    <!-- timeout value in milliseconds -->
    <property name="timeoutPerShutdownPhase" value="10000"/>
</bean>
```

如前所述，`LifecycleProcessor`接口定义了用于刷新和关闭上下文的回调方法。后者驱动关闭进程，就像显式调用了`stop（）`一样，但它是在上下文关闭时发生的。另一方面，“refresh”回调启用了`SmartLifecycle bean`的另一个功能。刷新上下文时（在所有对象被实例化和初始化之后），将调用该回调。此时，默认生命周期处理器将检查每个`SmartLifecycle`对象的`isAutoStartup（）`方法返回的布尔值。如果为`true`，则该对象将在该点启动，而不是等待显式调用上下文或其自身的`start（）`方法（与上下文刷新不同，对于标准上下文实现，上下文启动不会自动发生）。阶段值和任何“依赖”关系决定了启动顺序，如前所述。

**在非Web应用程序中优雅地关闭Spring IoC容器**
本节仅适用于非web应用程序。Spring的基于web的`ApplicationContex`t实现已经准备好了代码，以便在相关web应用程序关闭时优雅地关闭Spring IoC容器。
如果在非web应用程序环境中（例如，在富客户机桌面环境中）使用Spring的IoC容器，请向JVM注册一个关闭挂钩。这样做可以确保正常关闭，并调用singleton bean上的相关销毁方法，以便释放所有资源。您仍然必须正确配置和实现这些销毁回调。
要注册关闭挂接，请调用在`ConfigurableApplicationContext`接口上声明的`registerShutdownHook（）`方法，如下例所示：

```java
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public final class Boot {

    public static void main(final String[] args) throws Exception {
        ConfigurableApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");

        // add a shutdown hook for the above context...
        ctx.registerShutdownHook();

        // app runs here...

        // main method exits, hook is called prior to the app shutting down...
    }
}
```

## 6.2ApplicationContextAware和BeanNameAware

当`ApplicationContext`创建实现`org.springframework.context.ApplicationContextAware`接口的对象实例时，将为该实例提供对该`ApplicationContext`的引用。下面的列表显示了`ApplicationContextAware`接口的定义：

```java
public interface ApplicationContextAware {

    void setApplicationContext(ApplicationContext applicationContext) throws BeansException;
}
```

因此，bean可以通过`ApplicationContext`接口或通过强制转换对此接口的已知子类（例如`ConfigurableApplicationContext`，它公开了其他功能）的引用，以编程方式操作创建它们的`ApplicationContext`。一个用途是对其他bean的编程检索。有时这种能力是有用的。但是，一般来说，您应该避免这样做，因为它将代码耦合到Spring，并且不遵循控制样式的反转，即协作者作为属性提供给bean。`ApplicationContext`的其他方法提供对文件资源的访问、发布应用程序事件和访问消息源。这些附加功能在`ApplicationContext`的附加功能中进行了描述。

自动连接是获取对`ApplicationContext`的引用的另一种方法。传统的构造函数和byType自动连接模式（如autowiring Collaborators中所述）可以分别为构造函数参数或setter方法参数提供`ApplicationContext`类型的依赖关系。要获得更大的灵活性，包括自动连接字段和多参数方法的能力，请使用基于注解的自动连接功能。如果这样做，`ApplicationContext`将自动连接到字段、构造函数参数或方法参数中，如果所讨论的字段、构造函数或方法带有`@Autowired`注解，则该字段、构造函数或方法参数需要`ApplicationContext`类型。有关详细信息，请参阅使用`@Autowired`。

当`ApplicationContext`创建实现`org.springframework.beans.factory.BeanNameAware`接口的类时，将为该类提供对其关联对象定义中定义的名称的引用。下面的列表显示`BeanNameAware`接口的定义：

```java
public interface BeanNameAware {

    void setBeanName(String name) throws BeansException;
}
```

回调在正常bean属性填充之后调用，但在初始化回调（如`InitializingBean`、`afterPropertiesSet`或自定义`init`方法）之前调用。

## 6.3其他Aware接口

除了`ApplicationContextAware`和`BeanNameAware`（前面已经讨论过），Spring还提供了一系列感知回调接口，让bean向容器指示它们需要某种基础结构依赖性。一般来说，名称表示依赖项类型。下表总结了最重要的感知接口：

Aware(感知) interfaces(接口)
|名称|注入依赖项|解释于...|
|---|---|---|
|`ApplicationContextAware`|声明ApplicationContext|[ApplicationContextAware和BeanNameAware](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#beans-factory-aware)|
|`ApplicationEventPublisherAware`|封闭ApplicationContext的事件发布者。|[ApplicationContext的附加功能](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#beans-factory-lifecycle-default-init-destroy-methods)|
|`BeanClassLoaderAware`|用于加载bean类的类加载器|[实例化bean](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#beans-factory-lifecycle-default-init-destroy-methods)|
|`BeanFactoryAware`|声明BeanFactory|[ApplicationContextAware和BeanNameAware](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#beans-factory-aware)|
|`BeanNameAware`|声明bean的名称|[ApplicationContextAware和BeanNameAware](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#beans-factory-aware)|
|`BootstrapContextAware`|资源适配器BootstrapContext容器在其中运行。通常仅在支持JCA的ApplicationContext实例中可用|[JCA CCI](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/integration.html#cci)|
|`LoadTimeWeaverAware`|定义了在加载时处理类定义的编织器。|[Spring框架中AspectJ的加载时间编织](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#aop-aj-ltw)|
|`MessageSourceAware`|用于解析消息的配置策略（支持参数化和国际化）|[ApplicationContext的附加功能](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#beans-factory-lifecycle-default-init-destroy-methods)|
|`NotificationPublisherAware`|Spring JMX通知发布程序|[通知](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/integration.html#jmx-notifications)|
|`ResourceLoaderAware`|为低级别资源访问配置的加载程序|[资源](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/core.html#resources)|
|`ServletConfigAware`|容器所运行的当前ServletConfig。仅在支持web的Spring应用程序上下文中有效|[SpringMVC](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/web.html#mvc)|
|`ServletContextAware`|容器所在的当前ServletContext。仅在支持web的Spring应用程序上下文中有效|[SpringMVC](https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/web.html#mvc)|

请再次注意，使用这些接口将代码绑定到Spring API，并且不遵循控制样式的反转。因此，对于需要对容器进行编程访问的基础设施bean，我们建议使用它们。
