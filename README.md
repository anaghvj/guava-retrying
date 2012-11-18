##What is this?
This is a fork of the excellent RetryerBuilder code posted to
http://code.google.com/p/guava-libraries/issues/detail?id=490 by Jean-Baptiste Nizet (JB).  I've added a Gradle build
for pushing it up to my little corner of Maven Central so that others can easily pull it into their existing projects
with minimal effort.  It also includes an [exponential backoff WaitStrategy](http://rholder.github.com/guava-retrying/com/github/rholder/retry/WaitStrategies.html)
that might be useful for situations where more well-behaved service polling is preferred.

##Maven

    <dependency>
      <groupId>com.github.rholder</groupId>
      <artifactId>guava-retrying</artifactId>
      <version>1.0.2</version>
    </dependency>

##Gradle

    compile "com.github.rholder:guava-retrying:1.0.1"

##Example
A minimal sample of some of the functionality would look like:

    Callable<Boolean> callable = new Callable<Boolean>() {
        public Boolean call() throws Exception {
            return true; // do something useful here
        }
    };

    Retryer<Boolean> retryer = RetryerBuilder.<Boolean>newBuilder()
            .retryIfResult(Predicates.<Boolean>isNull())
            .retryIfExceptionOfType(IOException.class)
            .retryIfRuntimeException()
            .withStopStrategy(StopStrategies.stopAfterAttempt(3))
            .build();
    try {
        retryer.call(callable);
    } catch (RetryException e) {
        e.printStackTrace();
    } catch (ExecutionException e) {
        e.printStackTrace();
    }

##Documentation
Javadoc can be found [here](http://rholder.github.com/guava-retrying/).

##Building from source
The guava-retyring module uses a [Gradle](http://gradle.org)-based build system. In the instructions
below, [`./gradlew`](http://vimeo.com/34436402) is invoked from the root of the source tree and serves as
a cross-platform, self-contained bootstrap mechanism for the build. The only
prerequisites are [Git](https://help.github.com/articles/set-up-git) and JDK 1.6+.

### check out sources
`git clone git://github.com/rholder/guava-retrying.git`

### compile and test, build all jars
`./gradlew build`

### install all jars into your local Maven cache
`./gradlew install`

##License
The guava-retrying module is released under version 2.0 of the
[Apache License](http://www.apache.org/licenses/LICENSE-2.0).
