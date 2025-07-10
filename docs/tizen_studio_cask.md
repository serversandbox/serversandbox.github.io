# samsung tv shenanigans

building is more fun than revising for certs.

skip to the PR: https://github.com/Homebrew/homebrew-cask/pull/219456

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

generate the token - https://docs.brew.sh/Adding-Software-to-Homebrew#generating-a-token-for-the-cask by running the script on the full path to the file

```
➜ $(brew --repository homebrew/cask)/developer/bin/generate_cask_token "/Volumes/Tizen_Studio_6.1 1/installer.app"
Proposed token:               volumestizenstudio611installer
Proposed file name:           volumestizenstudio611installer.rb
Cask Header Line:             cask "volumestizenstudio611installer" do

WARNING: 'volumestizenstudio611installer' contains digits. Digits which are version numbers should be removed.
```

not great results lol. just call it tizen-studio and wait for review

```
09:28:20 in homebrew-cask/Casks on  add-tizen-studio [?] using ☁️ terraform using ☁️  default/
➜ shasum -a 256 ~/downloads/web-ide_Tizen_Studio_6.1_macos-64.dmg

a9b0...
```

## 4 - try to install what you wrote

```
09:32:27 in homebrew-cask/Casks on  add-tizen-studio [?] using ☁️ terraform using ☁️  default/ took 2.2s
➜ export HOMEBREW_NO_AUTO_UPDATE=1

09:42:45 in homebrew-cask/Casks on  add-tizen-studio [?] using ☁️ terraform using ☁️  default/
➜ export HOMEBREW_NO_INSTALL_FROM_API=1

09:46:38 in homebrew-cask/Casks on  add-tizen-studio [?] using ☁️ terraform using ☁️  default/
# note brew install my-new-cask DID NOT WORK FOR ME
➜ brew install --cask ./t/tizen-studio.rb 

# you still need to open the manager yourself... 

# do brew audit etc

09:52:30 in homebrew-cask/Casks on  add-tizen-studio [?] using ☁️ terraform using ☁️  default/
➜ brew style --fix ./t/tizen-studio.rb

t/tizen-studio.rb:4:1: C: [Corrected] Cask/StanzaGrouping: stanza groups should be separated by a single empty line
t/tizen-studio.rb:4:1: C: [Corrected] Layout/TrailingWhitespace: Trailing whitespace detected.
t/tizen-studio.rb:5:1: C: [Corrected] Layout/EmptyLines: Extra blank line detected.
t/tizen-studio.rb:9:1: C: [Corrected] Cask/StanzaGrouping: stanza groups should be separated by a single empty line
t/tizen-studio.rb:9:1: C: [Corrected] Layout/TrailingWhitespace: Trailing whitespace detected.
t/tizen-studio.rb:10:3: C: [Corrected] Cask/StanzaOrder: depends_on stanza out of order
  depends_on macos: ">= :big_sur"
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
t/tizen-studio.rb:11:1: C: [Corrected] Cask/StanzaGrouping: stanza groups should be separated by a single empty line
t/tizen-studio.rb:11:1: C: [Corrected] Layout/EmptyLines: Extra blank line detected.
t/tizen-studio.rb:11:1: C: [Corrected] Layout/TrailingWhitespace: Trailing whitespace detected.
t/tizen-studio.rb:12:3: C: [Corrected] Cask/StanzaOrder: depends_on stanza out of order
  depends_on macos: ">= :big_sur"
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
t/tizen-studio.rb:12:3: C: [Corrected] Cask/StanzaOrder: installer stanza out of order
  installer manual: "installer.app"
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
t/tizen-studio.rb:13:1: C: [Corrected] Cask/StanzaGrouping: stanza groups should be separated by a single empty line
t/tizen-studio.rb:13:1: C: [Corrected] Layout/TrailingWhitespace: Trailing whitespace detected.
t/tizen-studio.rb:14:1: C: [Corrected] Layout/EmptyLines: Extra blank line detected.
t/tizen-studio.rb:14:3: C: [Corrected] Cask/StanzaOrder: uninstall stanza out of order
  uninstall delete: [ ...
  ^^^^^^^^^^^^^^^^^^^
t/tizen-studio.rb:14:21: C: [Corrected] Cask/ArrayAlphabetization: The array elements should be ordered alphabetically
  uninstall delete: [ ...
                    ^
t/tizen-studio.rb:15:3: C: [Corrected] Cask/StanzaOrder: installer stanza out of order
  installer manual: "installer.app"
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
t/tizen-studio.rb:16:5: C: [Corrected] Style/TrailingCommaInArrayLiteral: Put a comma after the last item of a multiline array.
    "~/.tizen-studio-data"
    ^^^^^^^^^^^^^^^^^^^^^^
t/tizen-studio.rb:17:1: C: [Corrected] Layout/EmptyLines: Extra blank line detected.
t/tizen-studio.rb:18:3: C: [Corrected] Cask/StanzaOrder: uninstall stanza out of order
  uninstall delete: [ ...
  ^^^^^^^^^^^^^^^^^^^
t/tizen-studio.rb:18:21: C: [Corrected] Cask/ArrayAlphabetization: The array elements should be ordered alphabetically
  uninstall delete: [ ...
                    ^
t/tizen-studio.rb:19:3: C: [Corrected] Cask/StanzaOrder: caveats stanza out of order
  caveats <<~EOS
  ^^^^^^^^^^^^^^
t/tizen-studio.rb:23:3: C: [Corrected] Cask/StanzaOrder: caveats stanza out of order
  caveats <<~EOS
  ^^^^^^^^^^^^^^
t/tizen-studio.rb:25:3: C: [Corrected] Cask/StanzaOrder: livecheck stanza out of order
  livecheck do ...
  ^^^^^^^^^^^^
t/tizen-studio.rb:29:3: C: [Corrected] Cask/StanzaOrder: livecheck stanza out of order
  livecheck do ...
  ^^^^^^^^^^^^
t/tizen-studio.rb:30:1: C: [Corrected] Layout/TrailingWhitespace: Trailing whitespace detected.
t/tizen-studio.rb:31:4: C: [Corrected] Layout/TrailingEmptyLines: Final newline missing.
end

t/tizen-studio.rb:34:1: C: [Corrected] Layout/EmptyLinesAroundBlockBody: Extra empty line detected at block body end.

1 file inspected, 28 offenses detected, 28 offenses corrected


# try uninstall 

10:22:45 in homebrew-cask/Casks on  add-tizen-studio [?] using ☁️ terraform using ☁️  default/ took 7.0s
➜ brew uninstall --zap --cask ./t/tizen-studio.rb

==> Removing files:
~/.tizen-studio-data
Password:
~/tizen-studio
==> Dispatching zap stanza
==> Trashing files:
~/.package-manager
~/Library/Preferences/org.tizen.sdk.ide.plist
~/tizen-workspace
==> Removing all staged versions of Cask 'tizen-studio'

10:23:24 in homebrew-cask/Casks on  add-tizen-studio [?] using ☁️ terraform using ☁️  default/ took 7.6s
➜ find ~ -name "*tizen*" -type f 2>/dev/null | head -250
/Users/sharonwoo/code/serversandbox.github.io/docs/tizen_studio_cask.md
/Users/sharonwoo/code/serversandbox.github.io/.git/logs/refs/heads/19-write-homebrew-cask-for-tizen-studio
/Users/sharonwoo/code/serversandbox.github.io/.git/logs/refs/remotes/origin/19-write-homebrew-cask-for-tizen-studio
/Users/sharonwoo/code/serversandbox.github.io/.git/refs/heads/19-write-homebrew-cask-for-tizen-studio
/Users/sharonwoo/code/serversandbox.github.io/.git/refs/remotes/origin/19-write-homebrew-cask-for-tizen-studio
/Users/sharonwoo/code/tizen-studio-workspace/.metadata/.plugins/org.eclipse.core.runtime/.settings/org.tizen.common.prefs
/Users/sharonwoo/code/homebrew-cask/Casks/t/tizen-studio.rb
/Users/sharonwoo/code/homebrew-cask/.git/logs/refs/heads/add-tizen-studio
/Users/sharonwoo/code/homebrew-cask/.git/refs/heads/add-tizen-studio
```

```
# create cask - helps you generate the SHA
brew create --cask https://download.tizen.org/sdk/Installer/tizen-studio_6.1/web-ide_Tizen_Studio_6.1_macos-64.dmg --set-name tizen-studio-test

## 5 - actually install the extensions

this is very painful and requires some manual trial and error
BECAUSE DEPENDENCIES ARE NOT MANAGED CORRECTLY (e.g. emulator > emulator manager)