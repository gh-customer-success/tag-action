# git tag actions

This repo contains some simple pipelines that are triggered when new tags are pushed to the repo. Below is a high level explanation of how it works.

```yaml
on:
  # Triggers the workflow on push of git tag events 
  push:
    tags:
      - v[0-9].[0-9].[0-9]
 ```
 resulting in:
 
 ![image](https://user-images.githubusercontent.com/680463/200678341-b352f668-afe1-4f22-8fe6-525f220c4e6b.png)

 The goal is to demonstrate how a deployment can be triggered for different environments (e.g. Dev, QA, Prod) while maintaining a `trunk based` development pattern 👍. 
 
 It is generally recommened to avoid `long lived` branches when using `git`.
 
 To trigger these pipelines simply use [git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging), here are some of the basic commands:
 
* ### List all tags on repo
```shell
git tag
```
![image](https://user-images.githubusercontent.com/680463/200670140-ec457107-b408-4149-9a0e-99418956f879.png)

* ### Create a new tag
```shell

git tag -a v1.1.2-rc.4 -m "latest test QA version fixes #123"

```

* ### Push a tag (this will trigger this workflow)

```shell
git push origin v1.1.2-rc.4
```

## Semantic Versioning (semver)

[Semantic Versioning](https://semver.org/) is a set of rules that attempts to simplify how version numbers are assigned and incremented.

Version numbers are described by the type of change (major, minor, patch):

![image](https://user-images.githubusercontent.com/680463/200673169-d5fd35d1-c63c-49f4-8031-252674702cef.png)

Versions can be defined as follows:

>Major - changes that are *NOT* backwards compatible (breaking changes)
>
>Minor - additions or changes that *are* backwards compatible
>
>Patch - bug fixes that have no affect on compatability

The action includes trigger filters for *beta* and *rc*

The intention is to use these for the different staging environments. These were chosen as it is the [recommended pattern](https://semver.org/#spec-item-9) according to the spec. 

The version numbers have the syntax `v1.1.1-rc.0` where `rc` (release candidate) translates to `QA` and `-beta.1` translates to the `test` environment. 

alternatively the trigger in GitHub Actions can be changed from:
```yaml
...

on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  push:
    tags:
      - v[0-9].[0-9].[0-9]-rc.*
...
```
to:
```yaml
...

on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  push:
    tags:
      - v[0-9].[0-9].[0-9]-qa.*
...
```
## GitHub Releases

As a bonus the action will also react to `releases` on your GitHub repository.
[ Learn more about releases.
](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
![image](https://user-images.githubusercontent.com/680463/200679328-19292d21-08f9-4dae-a659-2cb9657527cd.png)
