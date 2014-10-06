---
layout: post
title: "Clean your Windows Logs"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I personally hate doing manual maintenance of servers and running applications as I consider such tasks rather mundane and I beleive they should be as autonomous as possible.
Previously I was under the impression that even relatively simple tasks couldn't be scripted on Windows without writing VBScripts or some fancy .Net powered Powershell scripts.
Truth be told, that's still true in most cases, but actually not when looping files, as I found out by coincidence.

<!--more-->

Below you'll find a couple of commands that will let you delete log files older than X days, plus a couple of complete Scheduled Task scripts ready for import.

### Delete IIS logs older than 7 days

Below you'll find the command I use to automatically delete IIS W3 logs.

{% highlight text %}
forfiles /P X:\IIS_Log_Path /S /D -7 /C "cmd /c del /q @path"
{% endhighlight %}

If you'd like to know exactly what's going on, just write the command below, and you'll get all the documentation you'll need.

{% highlight text %}
forfiles /?
{% endhighlight %}

And here it is as a complete Scheduled Task that you can save and import directly.

{% highlight XML linenos %}
<?xml version="1.0" encoding="UTF-16"?>
<Task version="1.2" xmlns="http://schemas.microsoft.com/windows/2004/02/mit/task">
  <RegistrationInfo>
    <Date>2014-01-01T00:00:00.000000</Date>
    <Author>.\Administrator</Author>
  </RegistrationInfo>
  <Triggers>
    <CalendarTrigger>
      <StartBoundary>2014-01-01T03:00:00</StartBoundary>
      <Enabled>true</Enabled>
      <ScheduleByDay>
        <DaysInterval>1</DaysInterval>
      </ScheduleByDay>
    </CalendarTrigger>
  </Triggers>
  <Principals>
    <Principal id="Author">
      <UserId>.\Administrator</UserId>
      <LogonType>Password</LogonType>
      <RunLevel>LeastPrivilege</RunLevel>
    </Principal>
  </Principals>
  <Settings>
    <MultipleInstancesPolicy>IgnoreNew</MultipleInstancesPolicy>
    <DisallowStartIfOnBatteries>false</DisallowStartIfOnBatteries>
    <StopIfGoingOnBatteries>true</StopIfGoingOnBatteries>
    <AllowHardTerminate>true</AllowHardTerminate>
    <StartWhenAvailable>false</StartWhenAvailable>
    <RunOnlyIfNetworkAvailable>false</RunOnlyIfNetworkAvailable>
    <IdleSettings>
      <StopOnIdleEnd>true</StopOnIdleEnd>
      <RestartOnIdle>false</RestartOnIdle>
    </IdleSettings>
    <AllowStartOnDemand>true</AllowStartOnDemand>
    <Enabled>true</Enabled>
    <Hidden>false</Hidden>
    <RunOnlyIfIdle>false</RunOnlyIfIdle>
    <WakeToRun>false</WakeToRun>
    <ExecutionTimeLimit>PT1H</ExecutionTimeLimit>
    <Priority>7</Priority>
  </Settings>
  <Actions Context="Author">
    <Exec>
      <Command>forfiles</Command>
      <Arguments>/P X:\IIS_Log_Path /S /D -7 /C "cmd /c del /q @path"</Arguments>
      <WorkingDirectory>X:\IIS_Log_Path]</WorkingDirectory>
    </Exec>
  </Actions>
</Task>
{% endhighlight %}

Remember to change IIS_Log_Path to point at whatever is defined as your logging root folder, and the command will automatically traverse all subfolders.

And that's it really, that's all it takes to cut out one more mundane task so you can focus on writing software.
Enjoy!