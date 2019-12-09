---
title: Spring MVC Test
date: '2019-10-21 14:52'
template: "post"
draft: false
slug: "/posts/spring-mvc-test/"
category: "Spring"
tags:
  - "springMVCTest"
description:
socialImage: "/media/image-2.jpg"
---
* You can write a plain unit test for a Spring MVC controller by using JUnit or TestNG. To do so, instantiate the controller, inject it with mocked or stubbed dependencies, and call its methods (passing `MockHttpServletRequest`, `MockHttpServletResponse`, and others, as necessary). However, when writing such a unit test, much remains untested: for example, request mappings, data binding, type conversion, validation, and much more. Furthermore, other controller methods such as `@InitBinder`, `@ModelAttribute`, and `@ExceptionHandler` may also be invoked as part of the request processing lifecycle.

* The goal of **Spring MVC Test** is to provide an effective way to test controllers by performing requests and generating responses through the actual DispatcherServlet.
  * you can isolate the web layer by injecting mocked services into controllers, in which case you are testing the web layer only through the DispatcherServlet but with actual Spring configuration, as you might test the data access layer in isolation from the layers above it. Also, you can use the stand-alone setup, focusing on one controller at a time and manually providing the configuration required to make it work.


* Spring MVC Test builds on the familiar “mock” implementations of the Servlet API available in the spring-test module. This allows performing requests and generating responses without the need for running in a Servlet container. 

### 2 Setup choices
1. The first is to load Spring MVC configuration through the TestContext framework, which loads the Spring configuration and injects a `WebApplicationContext` into the test to use to build a `MockMvc` instance.
2. Your second option is to manually create a controller instance without loading Spring configuration. Instead, basic default configuration, roughly comparable to that of the MVC JavaConfig or the MVC namespace, is automatically created. 

#### Setup choice 1 - load spring mvc configuration
1. annotate the class with the `@ContextConfiguration` annotation and ensure that the correct configuration classes (or XML configuration files) are used. If we want to use Java configuration, we have to set the configuration classes as the value of the `classes` attribute. On the other hand, if we prefer XML configuration, we have to set the configuration files as the value of the `locations` attribute.
2. Annotate the class with the `@WebAppConfiguration` annotation. This annotation ensures that the application context which is loaded for our test is a `WebApplicationContext`.
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = {TestContext.class, WebAppContext.class})
//@ContextConfiguration(locations = {"classpath:testContext.xml", "classpath:exampleApplicationContext-web.xml"})
@WebAppConfiguration
class MyWebTests {

    MockMvc mockMvc;

    @Autowired
    private WebApplicationContext webApplicationContext;

    @BeforeEach
    void setup(WebApplicationContext wac) {
        this.mockMvc = MockMvcBuilders.webAppContextSetup(this.wac).build();
    }

    // ...

}
```
* The `webAppContextSetup` loads your actual Spring MVC configuration, resulting in a more complete integration test. Since the TestContext framework caches the loaded Spring configuration, it helps keep tests running fast, even as you introduce more tests in your test suite.
* Advantage here is that, you do not have 2 separate configuration and your test class will be lot cleaner.

#### Setup choice 2 - Standalone setup
```java
class MyWebTests {

    MockMvc mockMvc;

    @BeforeEach
    void setup() {
        this.mockMvc = MockMvcBuilders.standaloneSetup(new AccountController()).build();
    }

    // ...

}
```
* The standaloneSetup, on the other hand, is a little closer to a unit test. It tests one controller at a time. You can manually inject the controller with mock dependencies, and it does not involve loading Spring configuration. Such tests are more focused on style and make it easier to see which controller is being tested, whether any specific Spring MVC configuration is required to work, and so on. The standaloneSetup is also a very convenient way to write ad-hoc tests to verify specific behavior or to debug an issue.
* In standalone setup, we have to mock or create all the dependencies of the controller class manually like MessageSource, ViewResolvers, Service classess etc

### Important annotations
@SpringBootTest - annotation tells Spring Boot to go and look for a main configuration class (one with @SpringBootApplication for instance), and use that to start a Spring application context. SpringBootTest loads complete application and injects all the beans which can be slow.

@WebMvcTest - for testing the controller layer and you need to provide remaining dependencies required using Mock Objects.

@WebMvcTest - for testing the controller layer

@JsonTest - for testing the JSON marshalling and unmarshalling

@DataJpaTest - for testing the repository layer
@RestClientTests - for testing REST clients

#### Resources
[https://www.petrikainulainen.net/programming/spring-framework/unit-testing-of-spring-mvc-controllers-configuration/]()
