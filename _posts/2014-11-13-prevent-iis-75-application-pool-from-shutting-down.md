---
layout: post
title: "Prevent IIS 7.5 Application Pool from shutting down"
description: ""
tags: [iis, windows, web, server, configuration]
---
{% include JB/setup %}

If you, like me, like to encapsulate jobs in the IIS app pool, as opposed to creating scheduled tasks in Windows Task Scheduler you've most likely faced the same issues as I with your application pool
shutting down after being inactive for some time. Read on for a guide on how to configure IIS 7.5 for having always running and auto starting app pools.

<!--more-->

This post was inspired by the slew of posts on various blogs describing how you can configure IIS Application Initialization under IIS 8, leaving out how to do it in IIS 7.5.
You see, in IIS 8 it's a feature that can be added under "Turn Windows features on or off," but that's not the case with IIS 7.5, here it has to be installed through Web Platform Installer. Further complicating things this module
differs from the IIS 8 version in that it doesn't include the configuration component for IIS Manager, leaving you with editing the applicationHost.config file manually, not exactly ideal. And it's not too well documented either.
After some searching I found a guy who did his own snap-in for IIS manager, and it functions very well. Credits go where credits due, and the guy I'm referring to can be found here: <a href="https://social.msdn.microsoft.com/profile/amehrot/" target="_blank">MSDN Social</a>.

The first you'd want to do, is to set the timeout of the application pool to 0 (indefinite) like so, default is 20 minutes:

<p style="text-align: center">
	<img src="{{ site.url }}/assets/images/Application Initialization 1.png" style="max-width: 100%" />
</p>

Here's a snapshot of how the application level tab of the snap-in looks like.

<p style="text-align: center">
	<img src="{{ site.url }}/assets/images/Application Initialization 2.png" style="max-width: 100%" />
</p>

Here's an example of how a working application level configuration could look like.

{% highlight XML linenos %}
<configuration>
	<system.applicationHost>
        <applicationPools>
            <add name="DefaultAppPool" autoStart="true" managedRuntimeVersion="v4.0" startMode="AlwaysRunning">
                <processModel identityType="ApplicationPoolIdentity" idleTimeout="00:00:00" />
            </add>
        </applicationPools>
	</system.applicationHost>
</configuration>
{% endhighlight %}
And the path for this file is:
{% highlight text %}
%windir%\System32\inetsrv\config\applicationHost.config
{% endhighlight %}

Here's a screenshot of how the website level tab of the snap-in looks like.

<p style="text-align: center">
	<img src="{{ site.url }}/assets/images/Application Initialization 3.png" style="max-width: 100%" />
</p>

And here's an example of how a working website level configuration could look like.

{% highlight XML linenos %}
<configuration>
	<system.webServer>
		<applicationInitialization remapManagedRequestsTo="" skipManagedModules="false" doAppInitAfterRestart="true">
			<add initializationPage="/" />
		</applicationInitialization>
	</system.webServer>
</configuration>
{% endhighlight %}

You can verify everything works as intended by manually killing your worker process (w3wp.exe) and see it how it will immediately spool back up, that's all there is to it. Happy configuring.

Here's the link for downloading the installer for Application Initialization for IIS 7.5 over at <a href="http://www.iis.net/downloads/microsoft/application-initialization" target="_blank">Microsoft</a>, and I've provided a mirror for the installer here: <a href="/assets/downloads/ApplicationInitialization.exe">Mirror</a>.

Here's the link for downloading the installer for the custom configuration snap-in over at <a href="http://blogs.msdn.com/b/amol/archive/2013/01/25/application-initialization-ui-for-iis-7-5.aspx" target="_blank">MSDN Blogs</a>, and a mirror is provided here: <a href="/assets/downloads/ApplicationInitializationInstaller_x64.zip">Mirror</a>.