**general commands**

```bash
gitlab-runner start

gitlab-runner stop

gitlab-runner restart

gitlab-runner status
```

```shell
gitlab-runner exec shell Test --env CI_PROJECT_NAME=pudo-package-management-api
```

```shell
sudo gitlab-runner register --url https://gitlab.trendyol.com/
```

```shell
cat ~/.gitlab-runner/config.toml
```

```shell
cat ~/Library/LaunchAgents/gitlab-runner.plist 
```

**Run all stages**

```shell
#!/bin/sh
for job in $(yq e 'keys' .gitlab-ci.yml | sed 's/^- //g ; /^stages/d ; /^before_script/d ; /^after_script/d ; /#/d ; /^\s*$/d'); do
    gitlab-runner exec shell $job
done
```

[How to use GitLab CI to run tests locally?](https://www.lambdatest.com/blog/use-gitlab-ci-to-run-test-locally/)

https://medium.com/@umutuluer/how-to-test-gitlab-ci-locally-f9e6cef4f054
