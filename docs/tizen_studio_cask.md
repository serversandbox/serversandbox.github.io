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

```
hdiutil attach web-ide_Tizen_Studio_6.1_macos-64.dmg

Checksumming Protective Master Boot Record (MBR : 0)…
Protective Master Boot Record (MBR :: verified CRC32 $6FF13BD8
Checksumming GPT Header (Primary GPT Header : 1)…
 GPT Header (Primary GPT Header : 1): verified CRC32 $B1B1F394
Checksumming GPT Partition Data (Primary GPT Table : 2)…
GPT Partition Data (Primary GPT Tabl: verified CRC32 $5D2A13B5
Checksumming  (Apple_Free : 3)…
                    (Apple_Free : 3): verified CRC32 $00000000
Checksumming disk image (Apple_APFS : 4)…
.............................................................................................................................................................................
         disk image (Apple_APFS : 4): verified CRC32 $8EDC22D5
Checksumming  (Apple_Free : 5)…
                    (Apple_Free : 5): verified CRC32 $00000000
Checksumming GPT Partition Data (Backup GPT Table : 6)…
GPT Partition Data (Backup GPT Table: verified CRC32 $5D2A13B5
Checksumming GPT Header (Backup GPT Header : 7)…
  GPT Header (Backup GPT Header : 7): verified CRC32 $4015A4CE
verified CRC32 $DF7264C6
/dev/disk6          	GUID_partition_scheme
/dev/disk6s1        	Apple_APFS
/dev/disk7          	EF57347C-0000-11AA-AA11-0030654
/dev/disk7s1        	41504653-0000-11AA-AA11-0030654	/Volumes/Tizen_Studio_6.1 1

ls -la "/Volumes/Tizen_Studio_6.1 1/"

total 0
drwxr-xr-x  3 sharonwoo  staff   96 18 Apr 21:40 .
drwxr-xr-x  5 root       wheel  160 10 Jul 08:48 ..
drwxr-xr-x  3 sharonwoo  staff   96 18 Apr 21:39 installer.app
```

## 2 - check contributor guidelines

acceptable casks - https://docs.brew.sh/Acceptable-Casks

probably this? 

> Stable: The latest version provided by the developer defined by them as such.

check for gotchas - https://docs.brew.sh/Acceptable-Casks#rejected-casks

tutorial with examples - https://docs.brew.sh/Adding-Software-to-Homebrew#casks

## 3 - do the pull request

pull request guide - https://docs.brew.sh/How-To-Open-a-Homebrew-Pull-Request#cask-related-pull-request

1. fork repository - https://github.com/sharonwoo/homebrew-cask
2. tap core casks with `brew tap --force homebrew/cask`
3. `cd "$(brew --repository homebrew/cask)"`
3. `git remote add sharonwoo https://github.com/sharonwoo/homebrew-cask.git`

actually write the cask per - https://docs.brew.sh/Adding-Software-to-Homebrew#writing-the-cask

generate the token - https://docs.brew.sh/Adding-Software-to-Homebrew#generating-a-token-for-the-cask