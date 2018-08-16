---
title: spirng-web
date: 2018-08-16 00:18:27
tags: spring in actions
---

# Building spring web application

## Getting started with spring MVC

Spring MVC가 동작하는 순서는 이렇다. 
사용자의 request가 서버로 오면 가장먼저 만나는 곳이 DispatcherServlet이다. 여러개의 DispatcherServlet을 가질수 있고 각각은 prefix로 구분이 된다. 맵핑되는 DispatherServlet이 정해지면 그 서블릿의 controller중 request url에 맵핑되는 controller가 정해지고 그 컨트롤러가 불리게 된다. 컨트롤러는 request param에 대한 validation을 한후 viewname을 리턴한다. DispatherServlet은 view name을 받아서 view resolver에서 new name을 resolve 한후 해당 View로직을 실행하고 response를 랜더링 한후 사용자에게 돌려준다. 

```java
package spittr.config;
import org.springframework.web.servlet.support.
              AbstractAnnotationConfigDispatcherServletInitializer;
public class SpittrWebAppInitializer
       extends AbstractAnnotationConfigDispatcherServletInitializer {
@Override
  protected String[] getServletMappings() {
    return new String[] { "/" }; //
  }
  @Override
  protected Class<?>[] getRootConfigClasses() {
    return new Class<?>[] { RootConfig.class };
  }
  @Override
  protected Class<?>[] getServletConfigClasses() {
    return new Class<?>[] { WebConfig.class };
  }
}

@Configuration
@EnableWebMvc //Enable Spring MVC
@ComponentScan("spitter.web")
public class WebConfig
       extends WebMvcConfigurerAdapter {
  @Bean
  public ViewResolver viewResolver() {
    InternalResourceViewResolver resolver =
            new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    resolver.setExposeContextBeansAsAttributes(true);
    return resolver;
}
  @Override
  public void configureDefaultServletHandling(
        DefaultServletHandlerConfigurer configurer) {
    configurer.enable();
} }
```

위는 web.xml를 대신에 Javaconfig를 사용해서 설정을 한 것이고 viewResolver및 DispatcherServlet에대한 설정을 하고 있는 부분.

## controller 

```java
@Controller Declared to be a controller public class HomeController {
  @RequestMapping(value="/", method=GET)
  public String home() {
    return "home"; //View name is home
  }
}
```

controller는 위처럼 path mapping과 http method(GET|POST|PUT|DELETE) 맵핑으로 view 이름을 리턴하게 된다.

## query/path parameter

```java
@RequestMapping(method=RequestMethod.GET)
      public List<Spittle> spittles(
          @RequestParam(value="max", defaultValue=MAX_LONG_AS_STRING) long max,
          @RequestParam(value="count", defaultValue="20") int count) {
        return spittleRepository.findSpittles(max, count);
}
```

## form

```java
@Controller
@RequestMapping("/spitter")
public class SpitterController {
  private SpitterRepository spitterRepository;
  @Autowired
  public SpitterController(
      SpitterRepository spitterRepository) {
    this.spitterRepository = spitterRepository;
}
Inject SpitterRepository
 @RequestMapping(value="/register", method=GET)
public String showRegistrationForm() {
  return "registerForm";
}
@RequestMapping(value="/register", method=POST)
public String processRegistration(Spitter spitter) {
    spitterRepository.save(spitter);
    return "redirect:/spitter/" + spitter.getUsername(); // redirect
} }
```

form 받는 post request예제.

post의 경우 request로 온 data를 validation이 필요한데. 이것은 java valication annotation을 사용할 수 있다. @null, @notNull, @Min, @Max 같은 것들..

```java
public class Spitter {
  private Long id;
  @NotNull
  @Size(min=5, max=16)
  private String username;
  @NotNull
  @Size(min=5, max=25)
  private String password;
  @NotNull
  @Size(min=2, max=30)
  private String firstName;
  @NotNull
  @Size(min=2, max=30)
  private String lastName;
}
```
