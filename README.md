# Spring AI: MongoDB Atlas Vector Store

Support for the `MongoDbAtlasLocalContainer` as a development/test container relying on Spring Boot's `ConnectionDetails` seems to be broken. The culprit seems to be the SSL support in `MongoConnectionDetails`.

## Pre-requisites

* Java 25
* Podman/Docker

## Run the application

```shell
./gradlew bootTestRun
```

The application will fail starting up with this error:

```log
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'chatController' defined in file [/Users/thomas/Developer/spring-ai-mongodb-atlas-error-reproducer/build/classes/java/main/com/example/demo/ChatController.class]: Unsatisfied dependency expressed through constructor parameter 1: Error creating bean with name 'vectorStore' defined in class path resource [org/springframework/ai/vectorstore/mongodb/autoconfigure/MongoDBAtlasVectorStoreAutoConfiguration.class]: Unsatisfied dependency expressed through method 'vectorStore' parameter 0: Error creating bean with name 'mongoTemplate' defined in class path resource [org/springframework/boot/autoconfigure/data/mongo/MongoDatabaseFactoryDependentConfiguration.class]: Unsatisfied dependency expressed through method 'mongoTemplate' parameter 0: Error creating bean with name 'mongoDatabaseFactory' defined in class path resource [org/springframework/boot/autoconfigure/data/mongo/MongoDatabaseFactoryConfiguration.class]: Unsatisfied dependency expressed through method 'mongoDatabaseFactory' parameter 0: Error creating bean with name 'mongo' defined in class path resource [org/springframework/boot/autoconfigure/mongo/MongoAutoConfiguration.class]: Failed to instantiate [com.mongodb.client.MongoClient]: Factory method 'mongo' threw exception with message: java.lang.IllegalAccessException: no private access for invokespecial: interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails, from interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails (unnamed module @5b12b668)
at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:804) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.autowireConstructor(ConstructorResolver.java:240) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireConstructor(AbstractAutowireCapableBeanFactory.java:1395) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1232) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:569) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:529) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:373) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.instantiateSingleton(DefaultListableBeanFactory.java:1228) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingleton(DefaultListableBeanFactory.java:1194) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:1130) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:990) ~[spring-context-6.2.12.jar:6.2.12]
at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:627) ~[spring-context-6.2.12.jar:6.2.12]
at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:146) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:752) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:439) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.boot.SpringApplication.run(SpringApplication.java:318) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.boot.SpringApplication.run(SpringApplication.java:1361) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.boot.SpringApplication.run(SpringApplication.java:1350) ~[spring-boot-3.5.7.jar:3.5.7]
at com.example.demo.DemoApplication.main(DemoApplication.java:10) ~[main/:na]
at org.springframework.util.function.ThrowingConsumer.accept(ThrowingConsumer.java:60) ~[spring-core-6.2.12.jar:6.2.12]
at org.springframework.util.function.ThrowingConsumer.accept(ThrowingConsumer.java:49) ~[spring-core-6.2.12.jar:6.2.12]
at org.springframework.boot.SpringApplication$Augmented.lambda$run$2(SpringApplication.java:1537) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.boot.SpringApplication.lambda$withHook$7(SpringApplication.java:1443) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:58) ~[spring-core-6.2.12.jar:6.2.12]
at org.springframework.util.function.ThrowingSupplier.get(ThrowingSupplier.java:46) ~[spring-core-6.2.12.jar:6.2.12]
at org.springframework.boot.SpringApplication.withHook(SpringApplication.java:1461) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.boot.SpringApplication.withHook(SpringApplication.java:1442) ~[spring-boot-3.5.7.jar:3.5.7]
at org.springframework.boot.SpringApplication$Augmented.run(SpringApplication.java:1537) ~[spring-boot-3.5.7.jar:3.5.7]
at com.example.demo.TestDemoApplication.main(TestDemoApplication.java:8) ~[test/:na]
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'vectorStore' defined in class path resource [org/springframework/ai/vectorstore/mongodb/autoconfigure/MongoDBAtlasVectorStoreAutoConfiguration.class]: Unsatisfied dependency expressed through method 'vectorStore' parameter 0: Error creating bean with name 'mongoTemplate' defined in class path resource [org/springframework/boot/autoconfigure/data/mongo/MongoDatabaseFactoryDependentConfiguration.class]: Unsatisfied dependency expressed through method 'mongoTemplate' parameter 0: Error creating bean with name 'mongoDatabaseFactory' defined in class path resource [org/springframework/boot/autoconfigure/data/mongo/MongoDatabaseFactoryConfiguration.class]: Unsatisfied dependency expressed through method 'mongoDatabaseFactory' parameter 0: Error creating bean with name 'mongo' defined in class path resource [org/springframework/boot/autoconfigure/mongo/MongoAutoConfiguration.class]: Failed to instantiate [com.mongodb.client.MongoClient]: Factory method 'mongo' threw exception with message: java.lang.IllegalAccessException: no private access for invokespecial: interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails, from interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails (unnamed module @5b12b668)
at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:804) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:546) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1375) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1205) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:569) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:529) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:373) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1708) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1653) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:913) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:791) ~[spring-beans-6.2.12.jar:6.2.12]
... 31 common frames omitted
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'mongoTemplate' defined in class path resource [org/springframework/boot/autoconfigure/data/mongo/MongoDatabaseFactoryDependentConfiguration.class]: Unsatisfied dependency expressed through method 'mongoTemplate' parameter 0: Error creating bean with name 'mongoDatabaseFactory' defined in class path resource [org/springframework/boot/autoconfigure/data/mongo/MongoDatabaseFactoryConfiguration.class]: Unsatisfied dependency expressed through method 'mongoDatabaseFactory' parameter 0: Error creating bean with name 'mongo' defined in class path resource [org/springframework/boot/autoconfigure/mongo/MongoAutoConfiguration.class]: Failed to instantiate [com.mongodb.client.MongoClient]: Factory method 'mongo' threw exception with message: java.lang.IllegalAccessException: no private access for invokespecial: interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails, from interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails (unnamed module @5b12b668)
at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:804) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:546) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1375) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1205) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:569) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:529) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:373) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1708) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1653) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:913) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:791) ~[spring-beans-6.2.12.jar:6.2.12]
... 44 common frames omitted
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'mongoDatabaseFactory' defined in class path resource [org/springframework/boot/autoconfigure/data/mongo/MongoDatabaseFactoryConfiguration.class]: Unsatisfied dependency expressed through method 'mongoDatabaseFactory' parameter 0: Error creating bean with name 'mongo' defined in class path resource [org/springframework/boot/autoconfigure/mongo/MongoAutoConfiguration.class]: Failed to instantiate [com.mongodb.client.MongoClient]: Factory method 'mongo' threw exception with message: java.lang.IllegalAccessException: no private access for invokespecial: interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails, from interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails (unnamed module @5b12b668)
at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:804) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:546) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1375) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1205) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:569) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:529) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:373) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:254) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1770) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1653) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:913) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:791) ~[spring-beans-6.2.12.jar:6.2.12]
... 57 common frames omitted
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'mongo' defined in class path resource [org/springframework/boot/autoconfigure/mongo/MongoAutoConfiguration.class]: Failed to instantiate [com.mongodb.client.MongoClient]: Factory method 'mongo' threw exception with message: java.lang.IllegalAccessException: no private access for invokespecial: interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails, from interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails (unnamed module @5b12b668)
at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:657) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:645) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1375) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1205) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:569) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:529) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:339) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:373) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:337) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:254) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1770) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1653) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:913) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:791) ~[spring-beans-6.2.12.jar:6.2.12]
... 71 common frames omitted
Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [com.mongodb.client.MongoClient]: Factory method 'mongo' threw exception with message: java.lang.IllegalAccessException: no private access for invokespecial: interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails, from interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails (unnamed module @5b12b668)
at org.springframework.beans.factory.support.SimpleInstantiationStrategy.lambda$instantiate$0(SimpleInstantiationStrategy.java:200) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiateWithFactoryMethod(SimpleInstantiationStrategy.java:89) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:169) ~[spring-beans-6.2.12.jar:6.2.12]
at org.springframework.beans.factory.support.ConstructorResolver.instantiate(ConstructorResolver.java:653) ~[spring-beans-6.2.12.jar:6.2.12]
... 85 common frames omitted
Caused by: java.lang.RuntimeException: java.lang.IllegalAccessException: no private access for invokespecial: interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails, from interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails (unnamed module @5b12b668)
at org.springframework.ai.testcontainers.service.connection.mongo.MongoDbAtlasLocalContainerConnectionDetailsFactory$MongoDbAtlasLocalContainerConnectionDetails.getSslBundle(MongoDbAtlasLocalContainerConnectionDetailsFactory.java:93) ~[spring-ai-spring-boot-testcontainers-1.1.0-SNAPSHOT.jar:1.1.0-SNAPSHOT]
at org.springframework.boot.autoconfigure.mongo.StandardMongoClientSettingsBuilderCustomizer.configureSslIfNeeded(StandardMongoClientSettingsBuilderCustomizer.java:106) ~[spring-boot-autoconfigure-3.5.7.jar:3.5.7]
at com.mongodb.MongoClientSettings$Builder.applyToSslSettings(MongoClientSettings.java:398) ~[mongodb-driver-core-5.5.2.jar:na]
at org.springframework.boot.autoconfigure.mongo.StandardMongoClientSettingsBuilderCustomizer.customize(StandardMongoClientSettingsBuilderCustomizer.java:85) ~[spring-boot-autoconfigure-3.5.7.jar:3.5.7]
at org.springframework.boot.autoconfigure.mongo.MongoClientFactorySupport.customize(MongoClientFactorySupport.java:55) ~[spring-boot-autoconfigure-3.5.7.jar:3.5.7]
at org.springframework.boot.autoconfigure.mongo.MongoClientFactorySupport.createMongoClient(MongoClientFactorySupport.java:49) ~[spring-boot-autoconfigure-3.5.7.jar:3.5.7]
at org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration.mongo(MongoAutoConfiguration.java:60) ~[spring-boot-autoconfigure-3.5.7.jar:3.5.7]
at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:104) ~[na:na]
at java.base/java.lang.reflect.Method.invoke(Method.java:565) ~[na:na]
at org.springframework.beans.factory.support.SimpleInstantiationStrategy.lambda$instantiate$0(SimpleInstantiationStrategy.java:172) ~[spring-beans-6.2.12.jar:6.2.12]
... 88 common frames omitted
Caused by: java.lang.IllegalAccessException: no private access for invokespecial: interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails, from interface org.springframework.boot.autoconfigure.mongo.MongoConnectionDetails (unnamed module @5b12b668)
at java.base/java.lang.invoke.MemberName.makeAccessException(MemberName.java:889) ~[na:na]
at java.base/java.lang.invoke.MethodHandles$Lookup.checkSpecialCaller(MethodHandles.java:3800) ~[na:na]
at java.base/java.lang.invoke.MethodHandles$Lookup.unreflectSpecial(MethodHandles.java:3314) ~[na:na]
at org.springframework.ai.testcontainers.service.connection.mongo.MongoDbAtlasLocalContainerConnectionDetailsFactory$MongoDbAtlasLocalContainerConnectionDetails.getSslBundle(MongoDbAtlasLocalContainerConnectionDetailsFactory.java:88) ~[spring-ai-spring-boot-testcontainers-1.1.0-SNAPSHOT.jar:1.1.0-SNAPSHOT]
... 97 common frames omitted
```
