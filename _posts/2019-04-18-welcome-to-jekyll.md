---
title: Welcome to Jekyll!
date: {2019-04-18}
categories:
  - blog
tags:
  - Jekyll
  - update
published: true
---

Recently, I was tinkering arround with a cool cmdlet called `Register-ObjectEvent`. This cmdlet lets us do some cool event driven actions on .NET objects so in this example today, I'll show you how to monitor an external process and have powershell preform an action when it is closed.

First, lets spawn our process:
```powershell
$proc = Start-Process 'notepad' -PassThru
```

`-PassThru` is very important here so we can get the process object.


Next we're going to create our event, in this case, `$Object` is a process that we spawn with `Start-Process`

```powershell
Register-ObjectEvent $proc -EventName Exited -SourceIdentifier WindowClosed
```
## WAIT...
where did 'Exited' come from? Well Exited is an event that is hard coded into the .net Object.
We can find all of the Events we can trigger by using `Get-Member` on it

`get-member -InputObject $proc`

Output:
```
Disposed
ErrorDataReceived
Exited
OutputDataReceived
```

The SourceIdentifier is what ever we want to name it so we can recall it later.
Now, we can put it all together with an action that will trigger when we close the window.

```powershell
$proc = Start-Process notepad -PassThru

Register-ObjectEvent $proc -EventName Exited -SourceIdentifier WindowClosed -Action {
    Write-host ("Process {0} was closed" -f $proc.id)
    Unregister-Event WindowClosed
	}
}
```
I include the `Unregister-Event WindowClosed` to clear it out of memory in case we need to re-creat it, Otherwise, we'll get an error that it exists already.

## Use Cases:
My use case will be using this for Universal dashboard.
I would start a UDDashboard and at the same time spawn a browser window to navigate to the target URL.
Once the browser is closed, the server will terminate so it doe snot keep running.

There are many other use cases for this so get creative!
