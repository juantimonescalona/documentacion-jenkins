# Jenkins + Sonar Dockerizados

Usando Plugin SonarScanner, y teniendo dockerizado las dos herramientas,
nos puede dar problema, lo primero la solución:

Cuando vamos a usar herramientas de CI dockerizadas tales como Gitlab, Jenkins, Sonar... lo mejor
para evitar problemas es usar la IP del Host, para que desde ningun contenedor nos de problemas de no encontrar al resto
de contenedores. Si congifuramos bien las network de Docker deberia de no pasar esto :)

## Error de Timeout a Sonar
SOLUCION: Ojo con las IP's de configuración, usar mejor la IP HOST (ifconfig | grep " inet addr:192.168")

09:19:00.467 DEBUG: Extract sonar-scanner-api-batch in temp...
09:19:00.473 DEBUG: Get bootstrap index...
09:19:00.473 DEBUG: Download: http://172.19.0.3:9000/batch/index
09:19:05.502 ERROR: SonarQube server [http://172.19.0.3:9000] can not be reached
09:19:05.503 INFO: ------------------------------------------------------------------------
09:19:05.503 INFO: EXECUTION FAILURE
09:19:05.504 INFO: ------------------------------------------------------------------------
09:19:05.504 INFO: Total time: 5.261s
09:19:05.536 INFO: Final Memory: 5M/240M
09:19:05.536 INFO: ------------------------------------------------------------------------
09:19:05.536 ERROR: Error during SonarQube Scanner execution
org.sonarsource.scanner.api.internal.ScannerException: Unable to execute SonarQube
	at org.sonarsource.scanner.api.internal.IsolatedLauncherFactory$1.run(IsolatedLauncherFactory.java:84)
	at org.sonarsource.scanner.api.internal.IsolatedLauncherFactory$1.run(IsolatedLauncherFactory.java:71)
	at java.security.AccessController.doPrivileged(Native Method)
	at org.sonarsource.scanner.api.internal.IsolatedLauncherFactory.createLauncher(IsolatedLauncherFactory.java:71)
	at org.sonarsource.scanner.api.internal.IsolatedLauncherFactory.createLauncher(IsolatedLauncherFactory.java:67)
	at org.sonarsource.scanner.api.EmbeddedScanner.doStart(EmbeddedScanner.java:218)
	at org.sonarsource.scanner.api.EmbeddedScanner.start(EmbeddedScanner.java:156)
	at org.sonarsource.scanner.cli.Main.execute(Main.java:74)
	at org.sonarsource.scanner.cli.Main.main(Main.java:61)
Caused by: java.lang.IllegalStateException: Fail to get bootstrap index from server
	at org.sonarsource.scanner.api.internal.Jars.getBootstrapIndex(Jars.java:100)
	at org.sonarsource.scanner.api.internal.Jars.getScannerEngineFiles(Jars.java:76)
	at org.sonarsource.scanner.api.internal.Jars.download(Jars.java:70)
	at org.sonarsource.scanner.api.internal.JarDownloader.download(JarDownloader.java:39)
	at org.sonarsource.scanner.api.internal.IsolatedLauncherFactory$1.run(IsolatedLauncherFactory.java:75)
	... 8 more
Caused by: java.net.SocketTimeoutException: connect timed out
	at java.net.PlainSocketImpl.socketConnect(Native Method)
	at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:350)
	at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:206)
	at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:188)
	at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
	at java.net.Socket.connect(Socket.java:589)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.platform.Platform.connectSocket(Platform.java:124)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.connection.RealConnection.connectSocket(RealConnection.java:220)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.connection.RealConnection.connect(RealConnection.java:146)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.connection.StreamAllocation.findConnection(StreamAllocation.java:186)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.connection.StreamAllocation.findHealthyConnection(StreamAllocation.java:121)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.connection.StreamAllocation.newStream(StreamAllocation.java:100)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.connection.ConnectInterceptor.intercept(ConnectInterceptor.java:42)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.RealInterceptorChain.proceed(RealInterceptorChain.java:92)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.RealInterceptorChain.proceed(RealInterceptorChain.java:67)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.cache.CacheInterceptor.intercept(CacheInterceptor.java:93)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.RealInterceptorChain.proceed(RealInterceptorChain.java:92)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.RealInterceptorChain.proceed(RealInterceptorChain.java:67)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.BridgeInterceptor.intercept(BridgeInterceptor.java:93)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.RealInterceptorChain.proceed(RealInterceptorChain.java:92)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.RetryAndFollowUpInterceptor.intercept(RetryAndFollowUpInterceptor.java:120)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.RealInterceptorChain.proceed(RealInterceptorChain.java:92)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.internal.http.RealInterceptorChain.proceed(RealInterceptorChain.java:67)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.RealCall.getResponseWithInterceptorChain(RealCall.java:179)
	at org.sonarsource.scanner.api.internal.shaded.okhttp.RealCall.execute(RealCall.java:63)
	at org.sonarsource.scanner.api.internal.ServerConnection.callUrl(ServerConnection.java:113)
	at org.sonarsource.scanner.api.internal.ServerConnection.downloadString(ServerConnection.java:98)
	at org.sonarsource.scanner.api.internal.Jars.getBootstrapIndex(Jars.java:96)
	... 12 more
ERROR: SonarQube scanner exited with non-zero code: 1
Finished: FAILURE

Ejemplo de la config de la tarea

![Config Gitlab Checkout](http://url/to/img.png)

![Config SonarScanenr](http://url/to/img.png)


