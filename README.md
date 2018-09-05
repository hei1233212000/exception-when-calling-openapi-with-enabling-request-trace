# This issue is fixed in the 5.183

### This is a project to show an issue in Payara Micro 5.182 when getting the OpenAPI document with enabling the OpenTracing/Request trace

### Reproduce steps
1. run the app WITHOUT enabling the OpenTracing
    >$ mvn clean package payara-micro:start
2. go to http://localhost:8080/openapi
3. you will see the OpenApi document is returned
    ```
    openapi: 3.0.0
    info:
      title: Deployed Resources
      version: 1.0.0
    servers:
    - url: http://localhost:8080/exception-when-calling-openapi-with-enabling-request-trace
      description: Default Server.
    paths: {}
    components: {}
    ```  
4. now, stop terminate the process
5. run the app WITH enabling the OpenTracing
    >$ mvn clean package payara-micro:start -P enable-request-trace
6. go to http://localhost:8080/openapi again
7. you will see an exception in the server log
    ```
    [2018-07-22T10:13:39.613+0800] [] [WARNING] [] [javax.enterprise.web] [tid: _ThreadID=33 _ThreadName=http-thread-pool::http-listener(6)] [timeMillis: 1532225619613] [levelValue: 900] [[
      StandardWrapperValve[fish.payara.microprofile.openapi.impl.rest.app.OpenApiApplication]: Servlet.service() for servlet fish.payara.microprofile.openapi.impl.rest.app.OpenApiApplication threw exception
    java.lang.IllegalStateException: Singleton not set for WebappClassLoader (delegate=true; repositories=) Object: 7cfac39f Created: Jul 22, 2018 10:12:41 AM
        at org.jboss.weld.bootstrap.api.helpers.TCCLSingletonProvider$TCCLSingleton.get(TCCLSingletonProvider.java:54)
        at org.jboss.weld.Container.instance(Container.java:53)
        at org.jboss.weld.SimpleCDI.<init>(SimpleCDI.java:77)
        at org.glassfish.weld.GlassFishWeldProvider$GlassFishEnhancedWeld.<init>(GlassFishWeldProvider.java:57)
        at org.glassfish.weld.GlassFishWeldProvider$GlassFishEnhancedWeld.<init>(GlassFishWeldProvider.java:57)
        at org.glassfish.weld.GlassFishWeldProvider.getCDI(GlassFishWeldProvider.java:96)
        at javax.enterprise.inject.spi.CDI.lambda$getCDIProvider$0(CDI.java:87)
        at java.util.stream.ReferencePipeline$2$1.accept(ReferencePipeline.java:174)
        at java.util.TreeMap$KeySpliterator.tryAdvance(TreeMap.java:2770)
        at java.util.stream.ReferencePipeline.forEachWithCancel(ReferencePipeline.java:126)
        at java.util.stream.AbstractPipeline.copyIntoWithCancel(AbstractPipeline.java:498)
        at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:485)
        at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:471)
        at java.util.stream.FindOps$FindOp.evaluateSequential(FindOps.java:152)
        at java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
        at java.util.stream.ReferencePipeline.findFirst(ReferencePipeline.java:464)
        at javax.enterprise.inject.spi.CDI.getCDIProvider(CDI.java:88)
        at javax.enterprise.inject.spi.CDI.current(CDI.java:64)
        at fish.payara.microprofile.opentracing.jaxrs.JaxrsContainerRequestTracingFilter.filter(JaxrsContainerRequestTracingFilter.java:157)
        at org.glassfish.jersey.server.ContainerFilteringStage$ResponseFilterStage.apply(ContainerFilteringStage.java:196)
        at org.glassfish.jersey.server.ContainerFilteringStage$ResponseFilterStage.apply(ContainerFilteringStage.java:163)
        at org.glassfish.jersey.process.internal.Stages.process(Stages.java:171)
        at org.glassfish.jersey.server.ServerRuntime$Responder.processResponse(ServerRuntime.java:393)
        at org.glassfish.jersey.server.ServerRuntime$Responder.process(ServerRuntime.java:441)
        at org.glassfish.jersey.server.ServerRuntime$1.run(ServerRuntime.java:285)
        at org.glassfish.jersey.internal.Errors$1.call(Errors.java:272)
        at org.glassfish.jersey.internal.Errors$1.call(Errors.java:268)
        at org.glassfish.jersey.internal.Errors.process(Errors.java:316)
        at org.glassfish.jersey.internal.Errors.process(Errors.java:298)
        at org.glassfish.jersey.internal.Errors.process(Errors.java:268)
        at org.glassfish.jersey.process.internal.RequestScope.runInScope(RequestScope.java:289)
        at org.glassfish.jersey.server.ServerRuntime.process(ServerRuntime.java:256)
        at org.glassfish.jersey.server.ApplicationHandler.handle(ApplicationHandler.java:704)
        at org.glassfish.jersey.servlet.WebComponent.serviceImpl(WebComponent.java:416)
        at org.glassfish.jersey.servlet.WebComponent.service(WebComponent.java:370)
        at org.glassfish.jersey.servlet.ServletContainer.service(ServletContainer.java:389)
        at org.glassfish.jersey.servlet.ServletContainer.service(ServletContainer.java:342)
        at org.glassfish.jersey.servlet.ServletContainer.service(ServletContainer.java:229)
        at org.apache.catalina.core.StandardWrapper.service(StandardWrapper.java:1622)
        at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:258)
        at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:160)
        at org.apache.catalina.core.StandardPipeline.doInvoke(StandardPipeline.java:654)
        at org.apache.catalina.core.StandardPipeline.invoke(StandardPipeline.java:593)
        at com.sun.enterprise.web.WebPipeline.invoke(WebPipeline.java:99)
        at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:159)
        at org.apache.catalina.connector.CoyoteAdapter.doService(CoyoteAdapter.java:371)
        at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:238)
        at com.sun.enterprise.v3.services.impl.ContainerMapper$HttpHandlerCallable.call(ContainerMapper.java:516)
        at com.sun.enterprise.v3.services.impl.ContainerMapper.service(ContainerMapper.java:213)
        at org.glassfish.grizzly.http.server.HttpHandler.runService(HttpHandler.java:182)
        at org.glassfish.grizzly.http.server.HttpHandler.doHandle(HttpHandler.java:156)
        at org.glassfish.grizzly.http.server.HttpServerFilter.handleRead(HttpServerFilter.java:218)
        at org.glassfish.grizzly.filterchain.ExecutorResolver$9.execute(ExecutorResolver.java:95)
        at org.glassfish.grizzly.filterchain.DefaultFilterChain.executeFilter(DefaultFilterChain.java:260)
        at org.glassfish.grizzly.filterchain.DefaultFilterChain.executeChainPart(DefaultFilterChain.java:177)
        at org.glassfish.grizzly.filterchain.DefaultFilterChain.execute(DefaultFilterChain.java:109)
        at org.glassfish.grizzly.filterchain.DefaultFilterChain.process(DefaultFilterChain.java:88)
        at org.glassfish.grizzly.ProcessorExecutor.execute(ProcessorExecutor.java:53)
        at org.glassfish.grizzly.nio.transport.TCPNIOTransport.fireIOEvent(TCPNIOTransport.java:524)
        at org.glassfish.grizzly.strategies.AbstractIOStrategy.fireIOEvent(AbstractIOStrategy.java:89)
        at org.glassfish.grizzly.strategies.WorkerThreadIOStrategy.run0(WorkerThreadIOStrategy.java:94)
        at org.glassfish.grizzly.strategies.WorkerThreadIOStrategy.access$100(WorkerThreadIOStrategy.java:33)
        at org.glassfish.grizzly.strategies.WorkerThreadIOStrategy$WorkerThreadRunnable.run(WorkerThreadIOStrategy.java:114)
        at org.glassfish.grizzly.threadpool.AbstractThreadPool$Worker.doWork(AbstractThreadPool.java:569)
        at org.glassfish.grizzly.threadpool.AbstractThreadPool$Worker.run(AbstractThreadPool.java:549)
        `at java.lang.Thread.run(Thread.java:748)
    ]]

    ```