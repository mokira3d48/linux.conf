## Virtual env
### Activate the virtual env
To activate the virtualenv, for the first time, we should execute two command lines:

```bat
Set-ExecutionPolicy Unrestricted -Scope Process
.\env\Scripts\activate
```

According to Microsoft Tech Support it might be a problem with Execution Policy Settings.
To fix it, you should try executing `Set-ExecutionPolicy Unrestricted -Scope Process`
(as mentioned in the comment section by @wtsiamruk) in your PowerShell window.
This would allow running virtualenv in the current PowerShell session.

There is also another approach that is more unsafe, but recommended by MS Tech Support.
This approach would be to use Set-ExecutionPolicy Unrestricted -Force (which do unleash
powers to screw Your system up). However, before you use this unsafe way, be sure
to check what your current ExecutionPolicy setting is by using get-ExecutionPolicy.
Then, when you are done, you can revert back to this ExecutionPolicy
by using `Set-ExecutionPolicy` %the value the get-ExecutionPolicy command gave you% -Force.
