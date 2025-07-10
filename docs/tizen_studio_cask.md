# samsung tv shenanigans

building is more fun than revising for certs.

## 1 - search earlier PRs

search - https://github.com/Homebrew/homebrew-cask/pulls

see something from 2021 - https://github.com/Homebrew/homebrew-cask/pull/106208

comment is 

> The dmg does not contain `Tizen Studio.app`, it contains instead an `installer.app` -- this may need an `installer manual:` and an `uninstall`.

per the docs: 

> installer manual: takes a single string value for the path to an interactive installer which must be run by the user at a later time. The path may be absolute, or relative to the cask. Example (from rubymotion.rb):
>
> ```installer manual: "RubyMotion Installer.app"```

## 2 - check contributor guidelines

acceptable casks - https://docs.brew.sh/Acceptable-Casks

probably this? 

> Stable: The latest version provided by the developer defined by them as such.

check for gotchas - https://docs.brew.sh/Acceptable-Casks#rejected-casks

tutorial with examples - https://docs.brew.sh/Adding-Software-to-Homebrew#casks

## do the pull request

pull request guide - https://docs.brew.sh/How-To-Open-a-Homebrew-Pull-Request#cask-related-pull-request

1. fork repository - https://github.com/sharonwoo/homebrew-cask
2. 