# elastic-capture-method-data
A Java library that must be used with Spring Boot and APM agent. It will send spans to an APM server to log methods parameters and/or responses as labels.
# Usage

In your pom.xml use:

``` xml
<dependency>
   <groupId>com.github.edgarpf</groupId>
   <artifactId>capture-method-data</artifactId>
   <version>9f3fcaa46e</version>
</dependency>
```

and

``` xml
<repositories>
   <repository>
     <id>jitpack.io</id>
     <url>https://jitpack.io</url>
   </repository>
</repositories>
```

In your Spring Boot Rest Controller you can use:

```java
@PostMapping("test")
@CaptureTransaction
/*
* You can use @ElasticCaptureMethodData
* to send to APM method params and method response 
* or with (mode = "response") or (mode = "parameters")
* to send only one of them.
* */
@ElasticCaptureMethodData(mode = "response")
public ResponseEntity myMethod() {
   //your code here
}
```

Remember that span is inside a transaction. Then, you must use **@CaptureTransaction** in the Controller method. It will be the parent of all spans. 

And the return response return of a controller method MUST BE an object of class **ResponseEntity**. With that the library will be able to send the return to the transaction as a label and will not create a span. For the other methods your return can be any Object.

In case of inappropriate usage (@ElasticCaptureMethodData(mode = "response") in a method that returns void) the library will NOT create a span. 
