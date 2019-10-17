---
title: Welcome to Jekyll!
date: {}
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

![]({{site.baseurl}}/https://i.imgur.com/yJO7QM5.png)

```powershell
$proc = Start-Process notepad -PassThru

Register-ObjectEvent $proc -EventName Exited -SourceIdentifier WindowClosed -Action {
    Write-host ("Process {0} was closed" -f $proc.id)
    Unregister-Event WindowClosed
	}
}
```

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
