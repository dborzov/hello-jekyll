---
layout: post
title:  "Fixing jmeter's java.security.cert.CertificateException error"
date:   2016-03-04 09:51:00 -0500
---

We were using [jmeter](http://jmeter.apache.org/) at work, to test an external service when I had all the tests failing
only on my Machine with the error message:

```
java.security.cert.CertificateException: Certificates does not conform to algorithm constraints
```

The sucky thing was that it was failing only on my machine. The tested service was obviously up and . So I had to dig a little into what is up with that.

`jmeter` reuses JDK library to make the request. Since Java 7 u16 (see the [release notes](http://www.oracle.com/technetwork/java/javase/6u17-141447.html)) the default support for some of the TLS protocols was turned off. The library just raises an exception when attempting to connect to endpoints that use it.

So how to address this? The ideal solution is to use more secure and proven algorithms for your SSL.
If it is not an option, it is possible to turn off the checker at the java setting file.

Find `{JDK_HOME}/jre/lib/security/java.security` for the JRE installation that jmeter uses and comment the lines assigning the values for:
```
jdk.tls.disabledAlgorithms=SSLv3, RC4, MD5withRSA, DH keySize < 768
jdk.certpath.disabledAlgorithms=MD2, MD5, RSA keySize < 1024
```

**Where the hell is `{JDK_HOME}`?**
So on OSX the one jmeter uses was at
{% highlight bash %}
 /Library/Java/JavaVirtualMachines/jdk1.8.0_74.jdk/Contents/Home/jre/lib/security/java.security
{% endhighlight %}
but basically just find all the suspitious `java.security` files with something like
{% highlight bash %}
 sudo find / -iname "java.security"
{% endhighlight %}

And that worked for me.
[Let me know]("mailto:tihoutrom@gmail.com") if that helped you or not.
