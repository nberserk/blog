---
title: spirng-web
date: 2018-08-16 00:18:27
tags: spring in actions
---

# Building spring web application

## Getting started with spring MVC

DispatchServlet -> mapped controller -> with view name and model back to DispatchServlet -> render response by view implementation resolved by view resolver

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
```

- spring context와 contextLoaderListener라는 컨텍스트도 같이 생성됨.
- AbstractAnnotationConfigDispatcherServletInitializer는 web.xml을 대체하는 것임.


```java
@Configuration
@EnableWebMvc Enable Spring MVC @ComponentScan("spitter.web")
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

