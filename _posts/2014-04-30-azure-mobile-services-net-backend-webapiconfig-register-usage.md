---
layout: post
title: 'Azure Mobile Services .NET Backend: WebApiConfig.Register() usage'
---

When using Azure Mobile Services .NET Backend, you might run into some of the following errors in the portal:

{% highlight winbatch %}
Boot strapping failed: executing 'WebApiConfig.Register' caused an exception: 'Parameter count mismatch.'.
{% endhighlight %}

{% highlight winbatch %}
Exception=System.InvalidOperationException: Boot strapping failed: executing 'WebApiConfig.Register' caused an exception: 'Late bound operations cannot be performed on types or methods for which ContainsGenericParameters is true.'.
{% endhighlight %}

Or the following in your event log:

{% highlight winbatch %}
The service has not been initialized correctly. Please ensure that 'StartupOwinAppBuilder' has been initialized.
   at Microsoft.WindowsAzure.Mobile.Service.Config.StartupOwinAppBuilder.Configuration(IAppBuilder appBuilder)
Exception has been thrown by the target of an invocation.
   at System.RuntimeMethodHandle.InvokeMethod(Object target, Object[] arguments, Signature sig, Boolean constructor)
   at System.Reflection.RuntimeMethodInfo.UnsafeInvokeInternal(Object obj, Object[] parameters, Object[] arguments)
   at System.Reflection.RuntimeMethodInfo.Invoke(Object obj, BindingFlags invokeAttr, Binder binder, Object[] parameters, CultureInfo culture)
   at Owin.Loader.DefaultLoader.&lt;&gt;c__DisplayClass12.&lt;MakeDelegate&gt;b__b(IAppBuilder builder)
   at Owin.Loader.DefaultLoader.&lt;&gt;c__DisplayClass1.&lt;LoadImplementation&gt;b__0(IAppBuilder builder)
   at Microsoft.Owin.Host.SystemWeb.OwinHttpModule.&lt;&gt;c__DisplayClass2.&lt;InitializeBlueprint&gt;b__0(IAppBuilder builder)
   at Microsoft.Owin.Host.SystemWeb.OwinAppContext.Initialize(Action`1 startup)
   at Microsoft.Owin.Host.SystemWeb.OwinBuilder.Build(Action`1 startup)
   at Microsoft.Owin.Host.SystemWeb.OwinHttpModule.InitializeBlueprint()
   at System.Threading.LazyInitializer.EnsureInitializedCore[T](T&amp; target, Boolean&amp; initialized, Object&amp; syncLock, Func`1 valueFactory)
   at Microsoft.Owin.Host.SystemWeb.OwinHttpModule.Init(HttpApplication context)
   at System.Web.HttpApplication.RegisterEventSubscriptionsWithIIS(IntPtr appContext, HttpContext context, MethodInfo[] handlers)
   at System.Web.HttpApplication.InitSpecial(HttpApplicationState state, MethodInfo[] handlers, IntPtr appContext, HttpContext context)
   at System.Web.HttpApplicationFactory.GetSpecialApplicationInstance(IntPtr appContext, HttpContext context)
   at System.Web.Hosting.PipelineRuntime.InitializeApplication(IntPtr appContext)
{% endhighlight %}

It's not clearly documented anywhere at the moment, but it turns out that Azure Mobile Services expects you to have a WebApiConfig.Register() method defined exactly, which it invokes via reflection. If you change this, it won't call that method correctly - if you're working from the examples the standard OWIN services won't be configured.

Of course, you can always go ahead and [configure OWIN yourself](http://www.strathweb.com/2014/02/running-owin-pipeline-new-net-azure-mobile-services/) if you need to customise it's configuration.