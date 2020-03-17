---
title:  "spring에서 scheduling작업 추가하기"
date:   2019-09-22 21:00:00 +0900
tags: [java, spring]
---

## scheduling작업 추가하기

```groovy
    // build.gradle 
    plugins { 
        id 'org.springframework.boot' version '2.1.6.RELEASE' 
    } 
    ...     
    dependencies { 
        implementation 'org.springframework.boot:spring-boot-starter-web' 
    } 
```

```java
    // Application.java 
    @SpringBootApplication 
    @EnableScheduling   // 스케줄링 작업을 가능하게 해 준다. 
    public class Application { 
        public static void main(String[] args) { 
            SpringApplication.run(Application.class, args); 
        } 
    } 
```

```java
    // SchedulingService.java 
    @Service 
    public class SchedulingService { 
        public void job(String option) { 
            System.out.println(option + " scheduling job start : " + LocalTime.now()); 
            System.out.println(option + " scheduling job end : " + LocalTime.now()); 
        } 
    } 
```

```java
    // TestScheduler.java 
    @Controller // component 스캔을 가능하게 한다. 
    public class Scheduler { 

        @Autowired 
        private SchedulingService schedulingService; 

        @Scheduled(fixedDelay = 10000) // 메소드 호출이 종료되는 시간에서 10000ms 이후 재 호출 
        public void doFixedDelayJob() { 
            schedulingService.job("fixedDelay"); 
        } 
    } 
```

-  실행결과  
>    fixedDelay scheduling job start : 14:38:29.519  
>    fixedDelay scheduling job end : 14:38:34.520
>    
>    fixedDelay scheduling job start : 14:38:44.519  
>    fixedDelay scheduling job end : 14:38:49.519
>    
>    fixedDelay scheduling job start : 14:38:59.519  
>    fixedDelay scheduling job end : 14:39:04.519
    
-   fixedDelay는 매소드 호출이 종료되는 시간부터이기 때문에, 규칙적으로 반복되는 작업을 할 때는 fixedRate가 적합하다.
    ```java
      // TestScheduler.java 
      @Controller // component 스캔을 가능하게 한다. 
      public class Scheduler { 
    
          @Autowired 
          private SchedulingService schedulingService; 
    
          @Scheduled(fixedRate = 10000) // 메소드 호출이 시작되는 시간에서 10000ms 이후 재 호출 
          public void doFixedRateJob() { 
              schedulingService.job("fixedRate"); 
          } 
      } 
    ```
    
-   실행결과  
>    fixedRate scheduling job start : 14:40:14.519  
>    fixedRate scheduling job end : 14:40:19.519
>    
>    fixedRate scheduling job start : 14:40:24.519  
>    fixedRate scheduling job end : 14:40:29.520
>    
>    fixedRate scheduling job start : 14:40:34.519  
>    fixedRate scheduling job end : 14:40:39.519
    
## 여러개의 스케줄러

한 번에 두 가지 스케줄러를 실행하면, Thread가 한개 이기 때문에 시간이 뒤죽박죽이 된다.
```java
    // Scheduler.java 
    @Controller 
    public class Scheduler { 

        @Autowired 
        private SchedulingService schedulingService; 

        @Scheduled(fixedDelay = 10000) 
        public void doFixedDelayJob() { 
            schedulingService.job("fixedDelay"); 
        } 

        @Scheduled(fixedRate = 5000) 
        public void doFixedRateJob() { 
            schedulingService.job("fixedRate"); 
        } 
    } 
```

-   실행결과  
>    fixedRate--scheduling job start--14:43:49.935  
>    fixedRate--scheduling job end--14:43:54.936
>    
>    fixedDelay--scheduling job start--14:43:54.936  
>    fixedDelay--scheduling job end--14:43:59.937
>    
>    fixedRate--scheduling job start--14:43:59.937  
>    fixedRate--scheduling job end--14:44:04.937
>    
>    fixedRate--scheduling job start--14:44:09.932  
>    fixedRate--scheduling job end--14:44:14.932
>    
>    fixedDelay--scheduling job start--14:44:14.932  
>    fixedDelay--scheduling job end--14:44:19.933
>    
>    fixedRate--scheduling job start--14:44:19.933  
>    fixedRate--scheduling job end--14:44:24.933  

이럴 때는 `SchedulingConfigurer`를 구현해서 Thread pool을 늘려 주면 스케줄러가 각각 독립적으로 동작하게 할 수 있다.
```java
  // SchedulingConfig.java 
  @Configuration 
  public class SchedulingConfig implements SchedulingConfigurer { 
      @Override 
      public void configureTasks (ScheduledTaskRegistrar taskRegistrar) { 
          ThreadPoolTaskScheduler threadPoolTaskScheduler = new ThreadPoolTaskScheduler(); 
          threadPoolTaskScheduler.setPoolSize(2); 
          threadPoolTaskScheduler.initialize(); 
          taskRegistrar.setTaskScheduler(threadPoolTaskScheduler); 
      } 
  } 
```
    
-   실행 결과  
>    fixedRate--scheduling job start--14:48:15.776  
>    fixedDelay--scheduling job start--14:48:15.776  
>    fixedRate--scheduling job end--14:48:20.776  
>    fixedDelay--scheduling job end--14:48:20.776
>    
>    fixedRate--scheduling job start--14:48:25.774  
>    fixedRate--scheduling job end--14:48:30.775
>    
>    fixedDelay--scheduling job start--14:48:30.777  
>    fixedRate--scheduling job start--14:48:35.773  
>    fixedDelay--scheduling job end--14:48:35.777  
>    fixedRate--scheduling job end--14:48:40.774
>    
>    fixedRate--scheduling job start--14:48:45.773  
>    fixedDelay--scheduling job start--14:48:45.778  
>    fixedRate--scheduling job end--14:48:50.773  
>    fixedDelay--scheduling job end--14:48:50.778

## 메소드끼리 독립적으로 동작

schedule되는 시간보다 time out을 더 길게 잡아 보았다. ( 5초단위 실행, time out은 10초)

-   실행 결과  
>    fixedRate--scheduling job start--14:51:45.495  
>    fixedRate--scheduling job end--14:51:55.495
>    
>    fixedRate--scheduling job start--14:51:55.495  
>    fixedRate--scheduling job end--14:52:05.496
>    
>    fixedRate--scheduling job start--14:52:05.496  
>    fixedRate--scheduling job end--14:52:15.497
    

service 내부에서 각각 thread를 생성하면 독립적으로 동작하게 할 수 있다.
```java
    // SchedulingService.java 
    @Service 
    public class SchedulingService { 
        public void job(String option) { 
            try { 
                new Thread(new Runnable() { 
                    @Override 
                    public void run () { 
                        System.out.println(option + "--scheduling job start--" + LocalTime.now()); 
                        try { 
                            TimeUnit.SECONDS.sleep(10); 
                        } catch (InterruptedException ignored) { 
                        } 
                        System.out.println(option + "--scheduling job end--" + LocalTime.now()); 
                    } 
                }).start(); 
            } catch (Exception ignored) { 
            } 
        } 
    } 
```

-   실행 결과  
>    fixedRate--scheduling job start--14:54:16.968 // Thread 1 start  
>    fixedRate--scheduling job start--14:54:21.968 // Thread 2 start  
>    fixedRate--scheduling job end--14:54:26.968 // Thread 1 end  
>    fixedRate--scheduling job start--14:54:26.968 // Thread 3 start  
>    fixedRate--scheduling job end--14:54:31.969 // Thread 2 end  
>    fixedRate--scheduling job start--14:54:31.968 // Thread 4 start  
>    fixedRate--scheduling job end--14:54:36.969 // Thread 3 end  
>    fixedRate--scheduling job start--14:54:36.969 // Thread 5 start  
>    fixedRate--scheduling job end--14:54:41.969 // Thread 4 end  
>    fixedRate--scheduling job start--14:54:41.969 // Thread 6 start  
>    fixedRate--scheduling job end--14:54:46.969 // Thread 5 end