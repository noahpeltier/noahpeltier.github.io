---
title: Register Object Events
date: {2019-10-17}
categories:
  - blog
tags:
  - Object Event
  - Browser
published: true
---
Register-ObjectEvent is a neat Cmdlet that will subcribe to an event on a .net object and allow you to assign an action when it's triggered.

In this example, we can spawn an instance of Internet Explorer and then subscribe to the 'Exited' event on the object, the Action will trigger when the window is closed wich can be very usefull in many circumstances such as a script that starts a webserver that you only want running while the browser is running.

Like this:

```powershell
Start-UDDashbord
$proc = Start-Process 'C:\Program Files\internet explorer\iexplore.exe' -ArgumentList 'http://localhost:80' -PassThru

Register-ObjectEvent $proc -EventName Exited -SourceIdentifier WindowClosed -Action {
    Write-host ("Process {0} was closed" -f $proc.id)
    Get-UDDashboard | Stop-UDDashboard
    Unregister-Event WindowClosed
}
```


![Alt Text](https://i.imgur.com/flQcXWu.gif)

Pretty cool huh?

You can find events on .net objects by using `Get-Member -InputObject $object`, you'll get a list of properties and some events if they exist. I'll go over some other uses for Object Events in another post.

