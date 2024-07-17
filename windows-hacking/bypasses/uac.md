# UAC

## "The command prompt has been disabled by your administrator"

<figure><img src="../../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

* Create a file : **cmd-bypass.bat** :

```
@echo off 
cls
echo Hacker's CMD && echo.
:A 
set /P x="%cd%: " 
%x% 
goto A
```

* Execute it now ! :drum:
