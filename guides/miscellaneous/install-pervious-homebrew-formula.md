---
title: Install Previous Homebrew Formula
nav: false
---

# Install Previous Homebrew Formula
Sometimes you need to install a specific, older version of an existing Homebrew Formula. In this guide I am going to demonstrate how to install an older version of `kubectl`, but it can be generalized for an Formula.

The Kubernetes CLI is available as a Homebrew Formulae: https://formulae.brew.sh/formula/kubernetes-cli#default

At the time of writing, my only options to install `kubectl` via Homebrew are:

- `brew install kubectl`
   - Would install the default version of `v1.29`
- `brew install kubectl@1.29` or `brew install kubectl@1.28`
- `brew install kubernetes-cli@1.29` or `brew install kubernetes-cli@1.28`

But what if I wanted to install `v1.27`? Well, that is just _slightly_ more involved and this guide will show you how to do just that.

## 1. Uninstall Existing Formula
First, uninstall the existing formula that you are looking to an install an older version of. In my case, I am going to run `brew uninstall kubectl`.

```bash
Michael ~ % kubectl version
Client Version: v1.29.0
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
The connection to the server localhost:8080 was refused - did you specify the right host or port?

Michael ~ % brew uninstall kubectl
Uninstalling /usr/local/Cellar/kubernetes-cli/1.29.0... (234 files, 60.3MB)

Michael ~ % which kubectl
kubectl not found
```

## 2. Find Formula `.rb` File

Homebrew keeps all of its [Formula in GitHub](https://github.com/Homebrew/homebrew-core/tree/master/Formula). You now need to find the specific Formula and version that you want to install via the existing the `.rb` file. To do this:

1. Visit https://github.com/Homebrew/homebrew-core/tree/master/Formula
2. Navigate to the `<formula>.rb` file that matches the name of the formula that you want to install. (e.g. The `kubernetes-cli` `.rb` file is found here: https://github.com/Homebrew/homebrew-core/blob/master/Formula/k/kubernetes-cli.rb)
   - **Note:** The formula are in directories alphabaetically, so it should be easy to find your specific `.rb` file.
3. Click on the `History` button on the GitHub UI to view the commit history of your formula's `.rb` file. (e.g. The `kubernetes-cli` commit history is found at: https://github.com/Homebrew/homebrew-core/commits/master/Formula/k/kubernetes-cli.rb)
4. Find the commit that has a name of `<formula-name>: update x.y.z bottle.` (e.g. The `kubernetes-cli` commit for `1.27.4` is found at: https://github.com/Homebrew/homebrew-core/commit/ff26f5862313ea6bed2ce490266f8fd3f90ba86e)
5. Click on the `***` button and select `View file`. (e.g. The file for the `1.27.4` `kubernetes-cli` is found at: https://github.com/Homebrew/homebrew-core/blob/ff26f5862313ea6bed2ce490266f8fd3f90ba86e/Formula/kubernetes-cli.rb)
6. Click on the `Raw` button.
7. Copy the URL and save it for the next step.

## 3. Install the Older Formula

1. Open a new terminal on your local machine.
2. Run `curl -O <formula-rb-url> && brew install ./<formula-name>.rb`.
   - Replace `<formula-rb-url>` with the URL found above.
   - Replace `<formula-name>` with the name of the Formula that you are installing.

```bash
Michael ~ % curl -O https://raw.githubusercontent.com/Homebrew/homebrew-core/ff26f5862313ea6bed2ce490266f8fd3f90ba86e/Formula/kubernetes-cli.rb && brew install ./kubernetes-cli.rb
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  2342  100  2342    0     0  30779      0 --:--:-- --:--:-- --:--:-- 30815

==> Downloading https://formulae.brew.sh/api/formula.jws.json
################################################################################################################################# 100.0%
Error: Failed to load cask: ./kubernetes-cli.rb
Cask 'kubernetes-cli' is unreadable: wrong constant name #<Class:0x000000011627c650>
Warning: Treating ./kubernetes-cli.rb as a formula.
==> Downloading https://ghcr.io/v2/homebrew/core/kubernetes-cli/manifests/1.27.4
################################################################################################################################# 100.0%
==> Fetching kubernetes-cli
==> Downloading https://ghcr.io/v2/homebrew/core/kubernetes-cli/blobs/sha256:edc6ced447d957b366526978d201d962aba11bb2555cfa160012eb56d15
################################################################################################################################# 100.0%
Warning: kubernetes-cli 1.29.0 is available and more recent than version 1.27.4.
==> Pouring kubernetes-cli--1.27.4.ventura.bottle.tar.gz
==> Downloading https://formulae.brew.sh/api/cask.jws.json

==> Caveats
zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/kubernetes-cli/1.27.4: 230 files, 59.2MB

Michael ~ % kubectl version
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"27", GitVersion:"v1.27.4", GitCommit:"fa3d7990104d7c1f16943a67f11b154b71f6a132", GitTreeState:"clean", BuildDate:"2023-07-19T12:14:48Z", GoVersion:"go1.20.6", Compiler:"gc", Platform:"darwin/amd64"}
Kustomize Version: v5.0.1
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

## 4. Cleanup

1. Run `rm <formula-name>.rb` to delete the file that was created to download the Formula's `.rb` file.

That's it! You have installed an older version of an existing Formula. You can always use `brew update <formula-name>` to update your formula to the latest available version.
