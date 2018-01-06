# git-ssh-key-buildpack

A Heroku buildpack for setting a private git SSH key as part of the application build. Particularly useful for granting Heroku access to installing a private git repository as a dependency.

# Configuration

1. Set up a deploy/access key with [GitHub](https://developer.github.com/v3/guides/managing-deploy-keys/), [GitLab](https://docs.gitlab.com/ce/ssh/README.html#deploy-keys), or [BitBucket](https://confluence.atlassian.com/bitbucketserver/ssh-access-keys-for-system-use-776639781.html).

2. Once you've generated the SSH key, configure the Heroku environment variable `GIT_SSH_KEY` (note that the key needs to be base64 encoded):

    ```shell
    $ heroku config:set GIT_SSH_KEY=$(cat path/to/your/keys/id_rsa | base64)
    ```

3. Configure the Heroku environment variable `GIT_SSH_HOST` based on your host. That is, `github.com`, `gitlab.com`, `bitbucket.org`, or maybe a hosted instance of GitLab:

    ```shell
    $ heroku config:set GIT_SSH_HOST="example.com"
    ```

4. Add the buildpack to Heroku using either the CLI or the dashboard. It will need to run before any buildpack trying to get SSH access. In the following example, it runs before the `heroku/nodejs` buildpack:

    ```shell
    $ heroku buildpacks:set --index 1 https://github.com/poetic-labs/git-ssh-key-buildpack.git
    $ heroku buildpacks:add heroku/nodejs
    ```
