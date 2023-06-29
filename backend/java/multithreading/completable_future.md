日常开发中，我们都会用到线程池，一般会用execute()和submit()方法提交任务。但是当你用过CompletableFuture之后，就会发现CompletableFuture的简洁优雅。要知道CompletableFuture已经随着Java8发布7年了

在正式了解CompleteFuture之前，先带大家了解下Future。Future接口是在JKD5引入的，他设计的初衷是对将来某个时候会发生的结果进行建模，它建模了一种异步计算，返回一个执行运算结果的引用，当运算结束后，调用方可以拿到这个引用。这样通过Future就可以把那些触发耗时操作的线程解放出来，让它去处理别的有价值的工作，不需要在等待。小编之前在工作中就使用过，下面是一个例子。


## 1. 使用线程池处理任务        

```java
  public class ThreadDemo {           
      public static void main(String[] args) {            
          // 1. 创建线程池            
          ExecutorService executorService = Executors.newFixedThreadPool(3);                        				  List<Integer> list = Arrays.asList(1, 2, 3);           
          List<Future<String>> futures = new ArrayList<>();            
          for (Integer key : list) {                
              // 2. 提交任务                
              Future<String> future = executorService.submit(() -> {                    
              	// 睡眠一秒，模仿处理过程                    
                Thread.sleep(1000L);                    
             	 return "结果" + key;                
              });                
              futures.add(future);            
          }                
          // 3. 获取结果            
          for (Future<String> future : futures) {                
              try {                    
                  String result = future.get();                    
                  System.out.println(result);                
              } catch (Exception e) {                    
                  e.printStackTrace();                
              }            
          }            
          executorService.shutdown();        
      }        
  } 

输出结果：    
结果1    
结果2    
结果3  
```

 

那么既然有了Future,为啥还要学CompletableFuture ? 

在使用Future的过程中，大家会发现Future有两种方式返回结果的方式

- 阻塞方式get()
- 轮询方法isDone()

阻塞的方式获取结果在某种程度上与异步编程的初衷相违背，而且轮询的方式又会浪费CPU。因此，导致Future在获取结果上总显得那么不尽如人意。

 如果我们期望异步任务结束后 能自动获取结果，这时Future就不能满足我们的需求了。于是乎，JDK8就引入了今天我们要说的CompletableFuture，它实现了Future接口，弥补了Future模式的缺点，还提供了非常强大的Future扩展功能，帮助我们简化异步编程的复杂性，提供了函数编程的能力，可以通过回调的方式处理计算结果，并且提供了转换，组合，串行化CompletableFuture的功能。


## 2. 使用CompletableFuture重构任务处理

看一下使用CompletableFuture改造后代码：            

```java
  public class ThreadDemo {            
      public static void main(String[] args) {            
          // 1. 创建线程池            
          ExecutorService executorService = Executors.newFixedThreadPool(3);               
          List<Integer> list = Arrays.asList(1, 2, 3);            
          for (Integer key : list) {                
              // 2. 提交任务                
              CompletableFuture.supplyAsync(() -> {                    
                  // 睡眠一秒，模仿处理过程                    
                  try {                        
                      Thread.sleep(1000L);                    
                  } catch (InterruptedException e) {   
                      e.getMessage();
                  }                    
                  return "结果" + key;               
              }, executorService).whenCompleteAsync((result, exception) -> {                    
                  // 3. 获取结果                    
                  System.out.println(result);                
              });            
          }                
          executorService.shutdown();            
          // 由于whenCompleteAsync获取结果的方法是异步的，所以要阻塞当前线程才能输出结果            
          try {                
              Thread.sleep(2000L);            
          } catch (InterruptedException e) {                
              e.printStackTrace();            
          }        
      }        
  } 
输出结果：    
	结果1    
	结果2    
	结果3              
```



代码中使用了CompletableFuture的两个方法，

`supplyAsync()`方法作用是提交异步任务，有两个传参，任务和自定义线程池。

`whenCompleteAsync()`方法作用是异步获取结果，也有两个传参，结果和异常信息。

同时也可以这样改造：

```java
public class CompletableFutureThread {

    public static void main(String[] args) {
        // 1. 创建线程池
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        List<Integer> list = Arrays.asList(1, 2, 3);
        for (Integer key : list) {
            // 2. 提交任务
            CompletableFuture<String> completableFuture = CompletableFuture.supplyAsync(() -> {
                // 睡眠一秒，模仿处理过程
                try {
                    Thread.sleep(1000L);
                } catch (InterruptedException e) {
                    e.getMessage();
                }
                return "结果" + key;
            }, executorService);
            completableFuture.thenAccept(System.out::println);

        }
        executorService.shutdown();
    }

}
输出结果：    
	结果1    
	结果2    
	结果3   
```

代码经过CompletableFuture改造后，提交任务也不用再关心线程池是怎么使用了，获取结果也不用再阻塞当前线程了。

> 如果你比较倔强，还想同步获取结果，可以使用whenComplete()方法，或者单独调用join()方法。

join()方法配合Stream流是这样用的：

```java
 public class ThreadDemo {            
     public static void main(String[] args) {            
         // 1. 创建线程池            
         ExecutorService executorService = Executors.newFixedThreadPool(3);                
         List<Integer> list = Arrays.asList(1, 2, 3);            
         // 2. 提交任务           
         List<String> results = list.stream().map(key ->                    
                 CompletableFuture.supplyAsync(() -> {                        
                      // 睡眠一秒，模仿处理过程                       
                      try {                            
                          Thread.sleep(1000L);                        
                      } catch (InterruptedException e) { 
                          e.getMessage();
                      }                        
                      return "结果" + key;                    
                 }, executorService))                                 			  	                                         .map(CompletableFuture::join).collect(Collectors.toList());             
         executorService.shutdown();            
         // 3. 获取结果            
         System.out.println(results);            
     
    }        
 } 

输出结果：    
	[结果1,结果2,结果3]     
```



## 3. CompletableFuture更多妙用

### 3.1 等待所有任务执行完成

如果让你实现等待所有任务线程执行完成，再进行下一步操作，你会怎么做？

我猜你一定会使用 线程池+CountDownLatch，像下面这样：    

```java
public class ThreadDemo {            
	public static void main(String[] args) {            
        // 1. 创建线程池            
        ExecutorService executorService = Executors.newFixedThreadPool(3);               
        List<Integer> list = Arrays.asList(1, 2, 3);            
        CountDownLatch countDownLatch = new CountDownLatch(list.size());            
        for (Integer key : list) {                
            // 2. 提交任务                
            executorService.execute(() -> {                    
                // 睡眠一秒，模仿处理过程                    
                try {                        
                    Thread.sleep(1000L);                    
                } catch (InterruptedException e) { 
                    e.getMessage();
                }                    
                System.out.println("结果" + key);                    
                countDownLatch.countDown();                
            });            
        }                
        executorService.shutdown();            
        // 3. 阻塞等待所有任务执行完成            
        try {                
            countDownLatch.await();            
        } catch (InterruptedException e) {            
        
        }        
    }        
} 

输出结果：    
结果2    
结果3    
结果1   
```

​           

看一下使用CompletableFuture是怎么重构的：

```java
public class ThreadDemo {            
    public static void main(String[] args) {            
        // 1. 创建线程池            
        ExecutorService executorService = Executors.newFixedThreadPool(3);                
        List<Integer> list = Arrays.asList(1, 2, 3);            
        // 2. 提交任务，并调用join()阻塞等待所有任务执行完成            
        CompletableFuture                    
            .allOf(                            
            		list.stream().map(key ->                                    														CompletableFuture.runAsync(() -> {                                        
                                // 睡眠一秒，模仿处理过程                                        
                                try {                                            
                                    Thread.sleep(1000L);                                        
                                } catch (InterruptedException e) { 
                                    e.getMessage();
                                }                                        
                                System.out.println("结果" + key);                                    
                            }, executorService))                                    													.toArray(CompletableFuture[]::new))                    
            .join();            
        executorService.shutdown();   
    }     
    
} 
输出结果：    
结果3    
结果1    
结果2 
```

​             

代码看着有点乱，其实逻辑很清晰。

1. 遍历list集合，提交CompletableFuture任务，把结果转换成数组
2. 再把数组放到CompletableFuture的allOf()方法里面
3. 最后调用join()方法阻塞等待所有任务执行完成

`CompletableFuture的allOf()方法的作用就是，等待所有任务处理完成。`

这样写是不是简洁优雅了许多？



### 3.2 任何一个任务处理完成就返回

如果要实现这样一个需求，往线程池提交一批任务，只要有其中一个任务处理完成就返回。

该怎么做？如果你手动实现这个逻辑的话，代码肯定复杂且低效，有了CompletableFuture就非常简单了，只需调用anyOf()方法就行了。

```java
public class ThreadDemo {            
    public static void main(String[] args) {            
        // 1. 创建线程池            
        ExecutorService executorService = Executors.newFixedThreadPool(3);                
        List<Integer> list = Arrays.asList(1, 2, 3);            
        long start = System.currentTimeMillis();            
        // 2. 提交任务            
        CompletableFuture<Object> completableFuture = CompletableFuture                    
            .anyOf(                            
           		 list.stream().map(key ->                                    
                       CompletableFuture.supplyAsync(() -> {                                        
                           // 睡眠一秒，模仿处理过程                                        
                           try {                                           
                               Thread.sleep(1000L);                                        
                           } catch (InterruptedException e) {  
                               e.getMessage();
                           }                                        
                           return "结果" + key;                                    
                       }, executorService))                                    						                                .toArray(CompletableFuture[]::new));            
        executorService.shutdown();                
        // 3. 获取结果            
        System.out.println(completableFuture.join());        
    }        
} 
输出结果：    
结果3              
```





**3.3 一个线程执行完成，交给另一个线程接着执行**

有这么一个需求：

一个线程处理完成，把处理的结果交给另一个线程继续处理，怎么实现？

你是不是想到了一堆工具，线程池、CountDownLatch、Semaphore、ReentrantLock、Synchronized，该怎么进行组合使用呢？AB组合还是BC组合？

别瞎想了，你写的肯定没有CompletableFuture好用，看一下CompletableFuture是怎么用的：

```java
public class ThreadDemo {            
    public static void main(String[] args) {            
        // 1. 创建线程池            
        ExecutorService executorService = Executors.newFixedThreadPool(2);                
        // 2. 提交任务，并调用join()阻塞等待任务执行完成            
        String result2 = CompletableFuture.supplyAsync(() -> {                
            // 睡眠一秒，模仿处理过程                
            try {                    
                Thread.sleep(1000L);                
            } catch (InterruptedException e) {
                e.getMessage(); 
            }                
            return "结果1";            
        }, executorService).thenApplyAsync(result1 -> {                
            // 睡眠一秒，模仿处理过程                
            try {                    
                Thread.sleep(1000L);                
            } catch (InterruptedException e) {    
                
            }               
            return result1 + "结果2";            
        }, executorService).join();               
        executorService.shutdown();            
        // 3. 获取结果           
        System.out.println(result2);       
    }        
} 
输出结果：    
	结果1
    结果2 
```

​             

代码主要用到了CompletableFuture的thenApplyAsync()方法，作用就是异步处理上一个线程的结果。

是不是太方便了？

这么好用的CompletableFuture还有没有其他功能？当然有。



## 4. CompletableFuture常用API

### 4.1 CompletableFuture常用API说明

1. 提交任务

> supplyAsync

> runAsync

2. 接力处理

> thenRun thenRunAsync

> thenAccept thenAcceptAsync

> thenApply thenApplyAsync

> handle handleAsync

> applyToEither applyToEitherAsync

> acceptEither acceptEitherAsync

> runAfterEither runAfterEitherAsync

> thenCombine thenCombineAsync

> thenAcceptBoth thenAcceptBothAsync

API太多，有点眼花缭乱，很容易分类。

带run的方法，无入参，无返回值。

带accept的方法，有入参，无返回值。

带supply的方法，无入参，有返回值。

带apply的方法，有入参，有返回值。

带handle的方法，有入参，有返回值，并且带异常处理。

以Async结尾的方法，都是异步的，否则是同步的。

以Either结尾的方法，只需完成任意一个。

以Both/Combine结尾的方法，必须所有都完成。

3. 获取结果

join 阻塞等待，不会抛异常

get 阻塞等待，会抛异常

`complete(T value) `不阻塞，如果任务已完成，返回处理结果。如果没完成，则返回传参value。

`completeExceptionally(Throwable ex)` 不阻塞，如果任务已完成，返回处理结果。如果没完成，抛异常。



### 4.2  CompletableFuture常用API使用示例

用最常见的煮饭来举例：

**4.2.1 then、handle方法使用示例**

```java
 public class ThreadDemo {            
     public static void main(String[] args) {            
         CompletableFuture<String> completableFuture = CompletableFuture.supplyAsync(() -> {                			System.out.println("1. 开始淘米");                                                                           return "2. 淘米完成";            
                                                                                           		                       }).thenApplyAsync(result -> {               
             System.out.println(result);                
             System.out.println("3. 开始煮饭");                
             // 生成一个1~10的随机数                
             if (RandomUtils.nextInt(1, 10) > 5) {                    
                 throw new RuntimeException("4. 电饭煲坏了，煮不了");                
             }                
             return "4. 煮饭完成";            
         }).handleAsync((result, exception) -> {                
             if (exception != null) {                    
                 System.out.println(exception.getMessage());                    
                 return "5. 今天没饭吃";                
             } else {                    
                 System.out.println(result);                    
                 return "5. 开始吃饭";                
             }            
         });                
         try {                
             String result = completableFuture.get();                
             System.out.println(result);            
         } catch (Exception e) {                
             e.printStackTrace();           
         }       
     }        
 } 
输出结果可能是：    
    1. 开始淘米    
    2. 淘米完成    
    3. 开始煮饭    
    4. 煮饭完成    
    5. 开始吃饭
也可能是：    
    1. 开始淘米    
    2. 淘米完成    
    3. 开始煮饭    
    java.lang.RuntimeException: 
    4. 电饭煲坏了，煮不了    
    5. 今天没饭吃      
```

​        

**4.2 complete方法使用示例**

```java
public class ThreadDemo {            
    public static void main(String[] args) {            
        CompletableFuture<String> completableFuture = CompletableFuture.supplyAsync(() -> {                				return "饭做好了";            
       });                        
        //try {            
        //    Thread.sleep(1L);            
        //} catch (InterruptedException e) {            
        //}                
        completableFuture.complete("饭还没做好，我点外卖了");           				 				                 System.out.println(completableFuture.join());        
    }        
} 
输出结果：    
    饭还没做好，我点外卖了 
如果把注释方法放开，输出结果就是:    
	饭做好了    
```

​          

**4.3 either方法使用示例**

```java
public class ThreadDemo {            
    public static void main(String[] args) {            
        CompletableFuture<String> meal = CompletableFuture.supplyAsync(() -> {                
            return "饭做好了";            
        });            
        CompletableFuture<String> outMeal = CompletableFuture.supplyAsync(() -> {                
            return "外卖到了";            
        });                
        // 饭先做好，就吃饭。外卖先到，就吃外卖。就是这么任性。            
        CompletableFuture<String> completableFuture = meal.applyToEither(outMeal, myMeal -> {               			 return myMeal;            
        });                
        System.out.println(completableFuture.join());        
    }        
} 
输出结果可能是：    
    饭做好了 
也可能是：    
    外卖到了
```

​              

学会了吗？开发中赶快用起来吧！