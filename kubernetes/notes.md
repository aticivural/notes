> The double dash (--) in the command signals the end of command options for kubectl. Everything after the double dash is the command that should be executed inside the pod . Using the double dash isn’t necessary if the command has no arguments that start with a dash.

```shell
echo -n root | base64
cm9vdA==
echo -n cm9vdA== | base64 --decode
root
```
