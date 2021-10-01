# 1.1

+ 스프링 애플리케이션에서는 오브젝트의 생성과 관계설정, 사용, 제거 등의 작업을 애플리케이션 코드 대신 독립된 컨테이너가 담당한다. 이를 컨테이너가 코드 대신 객체에 대한 제어권을 갖고 있다고 해서 IoC(Inversion of Control)이라고 한다. 



  그래서 스프링 컨테이너를 IoC 컨테이너라고도 한다.



> 빈 팩토리(BeanFactory) : 객체의 생성과 객체 사이의 생명주기를 설정하는 DI 관점으로 볼 때 컨테이너를 빈 팩토리라고 한다. 

> 애플리케이션 컨텍스트(ApplicationContext) : DI를 위한 빈 팩토리에 여러 가지 컨테이너 기능을 추가한 것을 애플리케이션 컨텍스트라고 한다. 일반적으로 스프링의 IoC 컨테이너는 애플리케이션 컨텍스트를 말한다.
>
> + ApplicationContext 인터페이스는 BeanFactory를 상속한 서브인터페이스이다.

+ 스프링 애플리케이션은 최소한 하나 이상의 ApplicationContext의 구현 객체를 가지고 있다. 여러 개의 컨텍스트 오브젝트를 가질 수도 있다.

<br>

+ 컨테이너가 본격적인 IoC 컨테이너로서 동작하려면 POJO 클래스와 설정 메타 정보가 필요하다.



+ POJO는 특정 기술과 스펙에서 독립적일뿐더러 인터페이스를 사용하여 의존관계에 있는 다른 POJO와 느슨한 결합을 갖도록 만들어야 한다.

+ 빈(Bean)을 어떻게 만들고 어떻게 동작하게 할 것인가에 관한 설정 메타정보를 BeanDefinition 인터페이스로 표현하고, 객체로 변환해주는 BeanDefinitionReader를 사용한다.

+ 스프링 IoC 컨테이너는 각 빈에 대한 정보를 담은 설정 메타정보를 읽어들인 뒤에, 이를 참고해서 빈을 생성하고, 프로퍼티나 생성자를 통해 DI 작업을 수행한다.

<br>

+ IoC 컨테이너가 관리하는 빈은 객체 단위지 클래스 단위가 아니다. 경우에 따라서는 하나의 클래스를 여러 개의 빈으로 등록할 수도 있다.

> spring-boot-starter-web 의존성을 설정하면 관련 라이브러리들이 자동으로 다운받아진다.

  

  ```java
    # Hello 클래스 빈 등록
  
    StaticApplicationContext ac = new StaticApplicationContext(); // IoC 컨테이너 생성. 생성과 동시에 컨테이너로 동작한다.
    ac.registerSingleton("hello1", Hello.class); // 싱글톤 빈으로 컨테이너에 등록된다.
    Hello hello1 = ac.getBean("hello1", Hello.class); 
    assertThat(hello1, is(nitNullValue())); //IoC 컨테이너가 등록한 빈을 생성했는지 확인하기 위해 빈을 요청하고 Null이 아닌지 확인한다.
  ```

  ```java
   # BeanDefinition을 이용한 빈 등록
  
    BeanDefinition helloDef = new RootBeanDefinition(Hello.class); //빈 메타정보를 담은 객체를 만든다. Bean 클래스는 Hello로 지정한다.
    helloDef.getPropertyValues().addPropertyValue("name", "Spring"); //Bean의 name 프로퍼티에 들어갈 값을 지정한다.
    ac.registerBeanDefinition("hello2", helloDef); //앞에서 생성한 Bean 메타정보를 hello2라는 이름을 가진 빈으로 해서 등록한다.
    assertThat(ac.getBeanFactory().getBeanDefinitionCount(), is(2)); //IoC 컨테이너에서 등록된 빈 설정 메타 정보를 가져올 수도 있다.
  ```

이외에도 xml을 활용하여 bean으로 등록하는 방법, @Component 어노테이션 컴포넌트 스캔을 사용하여 빈을 등록하는 방법, config에서 @Bean 어노테이션을 사용하여 빈으로 등록하는 방법, @SpringBootApplication(@ComponentScan과 @Configuration이 내포됨)을 사용하여 컴포넌트 스캔을 하는 방법이 있다.