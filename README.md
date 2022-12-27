[![npm version](https://img.shields.io/npm/v/voices.svg)](https://npmjs.com/package/voices) [![license](https://img.shields.io/npm/l/voices.svg)](https://github.com/mklement0/voices/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [voices &mdash; TTS CLI for macOS](#voices-&mdash-tts-cli-for-macos)
- [Examples](#examples)
- [Installation](#installation)
  - [Installation from the npm registry](#installation-from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [macOS Service for switching between default voices](#macos-service-for-switching-between-default-voices)
  - [Installation](#installation-1)
  - [Customization](#customization)
- [macOS Service for speaking selected text with a specific voice](#macos-service-for-speaking-selected-text-with-a-specific-voice)
  - [Installation](#installation-2)
  - [Customization](#customization-1)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# voices &mdash; TTS CLI for macOS

`voices` is a **macOS CLI** for **changing the default TTS (text-to-speech) voice** and for **printing information about and/or speaking text with multiple voices**.

`voices` complements the standard `say` utility by:

 * providing a way to change the default voice
 * operating on _active_ voices - those selected for active use - rather than all _installed_ voices
 * filtering voices by language
 * speaking text with _multiple_ voices.
   * On a related note: For a simple `say` wrapper that supports text with _embedded instructions to change the voice mid-text_ (e.g.,
`[[voice alex]]`), assuming you have [Powershell](https://github.com/PowerShell/PowerShell#get-powershell) installed, see [this comment](https://github.com/mklement0/voices/issues/6#issuecomment-666714554).

**Caveats**: 

* As of macOS 10.12 (Sierra), there is no documented programmatic way to change the default voice. Thus, this utility makes use of undocumented system internals, which, unfortunately, means that future compatibility of this feature is uncertain. [Feedback](https://github.com/mklement0/voices/issues) welcome.

* `voices` currently only fully supports voices provided by _Apple_. Support 
for third-party voices such as [InfoVox iVox](http://www.assistiveware.com/product/infovox-ivox) is limited to _speaking_ with them, 
and *the macOS Services documented below will not work with them*.

* Additionally, as of macOS 10.15, _Siri_ voices are _not_ supported, due to lack of API support (see [this Stack Overflow question](https://stackoverflow.com/q/61122378/45375)).

See the examples below, concise [usage information](#usage) further below,
or read the [manual](doc/voices.md).

Additionally, two **macOS Services** are offered:

* a **service for switching between two or more default voices** - see [below](#macos-service-for-switching-between-default-voices).
* a **service for speaking selected text with a specific voice** - see [below](#macos-service-for-speaking-selected-text-with-a-specific-voice)

Note: If you have [Alfred](http://alfredapp.com/) with its [Power Pack](https://www.alfredapp.com/powerpack/), consider workflow
**[speak.awf](https://github.com/mklement0/speak.awf)** as a superior alternative.

# Examples

```shell
  # List all active voices; add -a to list all installed ones.
voices -l

  # Print information about the default voice and speak its demo text.
voices -d -k

  # Print information about voice 'Alex'.
voices alex

  # Make 'Alex' the new default voice, print information about it, and
  # speak text that announces the change.
voices -k'The new default voice is Alex.' -d alex

  # List languages for which at least one voice is active.
voices -L

  # List active French voices.
voices -l fr

  # Print information about all active voices and speak
  # their respective demo text.
voices -l -k

  # Print information about all active Spanish voices and speak their
  # respective demo text.
voices -k -l es
  
  # Say "hello", first with voice Alex, then with Jill, suppressing printed
  # output.
voices -k"hello" -q alex jill
```

# Installation

**Supported platforms**

* **macOS**

Verified to work from OS X 10.8 (Mountain Lion) up to macOS 10.12 (Sierra).

The change-the-default-voice feature makes use of undocumented system internals, so its future compatiblity is uncertain.
[Do let me know](https://github.com/mklement0/voices/issues) if you find the feature broken in a future macOS version.

## Installation from the npm registry

<sup>Note: Even if you don't use Node.js, its package manager, `npm`, works across platforms and is easy to install; try [`curl -L http://git.io/n-install | bash`](https://github.com/mklement0/n-install)</sup>

With [Node.js](http://nodejs.org/) installed, install [the package](https://www.npmjs.com/package/voices) as follows:

    [sudo] npm install voices -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `voices` in your system's `$PATH`.

## Manual installation

* Download [the CLI](https://raw.githubusercontent.com/mklement0/voices/stable/bin/voices) as `voices`.
* Make it executable with `chmod +x voices`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin`.

# Usage

Find concise usage information below; for complete documentation, read the
[manual online](doc/voices.md), or, once installed, run `man voices`
(`voices --man` if installed manually).

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ voices --help


Get or set or speak with the DEFAULT VOICE:

    voices [<options>] [-d [<newDefaultVoice>]]

LIST INFORMATION about / speak with voices:

    voices [<options>] <voice>...

List / speak with ALL VOICES, optionally FILTERED BY LANGUAGES:

    voices [<options>] -l [<lang>...]

LIST LANGUAGES among voices:

    voices -L [-a]

MANAGE VOICES in System Preferences:

    voices -m

Shared options (synopsis forms 1-3):

    -a          target all installed voices (default: only active ones)
    -k          speak demo text with all targeted voices
    -k"<text>"  speak specified text
    -k-         speak text provided via stdin
    -b          output format: print voice names only
    -i          output format: print voice internals
    -q          quiet mode: no printed output

Standard options: --help, --man, --version, --home
```

# macOS Service for switching between default voices

This service, which uses an embedded copy of `voices`, is helpful if you use text-to-speech in two or more languages and want to quickly
switch the default voice between multiple designated voices cyclically, in combination with the built-in speak-selected-text service.

Every time the service is invoked, the next designated voice is made the default voice, and the localized name of the new voice's
language is spoken to confirm the change (this is configurable).

You can invoke the service from any application's standard `Services` menu, category `General`, or assign it a keyboard shortcut via
`System Preferences > Keyboard > Shortcuts > Services`.

## Installation

* Download [this ZIP file](https://raw.githubusercontent.com/mklement0/voices/stable/osx-service/Switch%20Default%20Voice.workflow.zip).
* In Finder, open the ZIP file, which creates package `Switch Default Voice.workflow` in the same folder.
* Open `Switch Default Voice.workflow` and choose `Install` when prompted - this will place the package in `~/Library/Services/`.
* Choose `Done` when prompted and proceed with customization below.

## Customization

* Invoke the service for the first time to prompt creating and editing the configuration file.
  * From any application, open that application's menu and select `Services > Switch Default Voice`.
  * You will be prompted to edit the configuration file, where you can specify the voices to switch between; follow the instructions in the file.
* To assign a keyboard shortcut to the service, go to `System Preferences > Keyboard > Shortcuts`, category `Services`,
  scroll to sub-category `General` in the list on the right, select `Switch Default Voice`, and click just inside the right edge of the list item.
* To customize the service again later, open `~/.SwitchDefaultVoice-rc` in your text editor.

# macOS Service for speaking selected text with a specific voice

This service provides an alternative to switching the default voice: it speaks
selected text in the frontmost application with a fixed alternate voice, which
allows it to be used _alongside_ the built-in speak-selected-text service, which
always uses the _default_ voice (see `System Preferences > Dictation & Speech > Text to Speech`).

Typically, you would use this service to speak selected text with a voice
that speaks a _different language_.

You can invoke it from the standard `Services` menu, category `Text`, whenever text is selected in the frontmost application, or assign it a
keyboard shortcut via `System Preferences > Keyboard > Shortcuts > Services`; e.g., `` ⌥` `` (Opt-\`) to parallel the default shortcut for
the built-in service, `⌥⎋` (Opt-Esc).

Invoking the service again while text from a previous invocation is still being spoken aborts speaking.  
_Caveat_: This only works if text - _any_ text - is selected in the activate applciation at the time the service is invoked again.

If desired, you can duplicate the service so as to be able to speak with one of _multiple_ alternate voices:  
Once installed, duplicate `~/Library/Services/Speak With Specific Voice.workflow` in Finder, give it a meaningful name, 
and customize the duplicate as described below.

## Installation

* Download [this ZIP file](https://raw.githubusercontent.com/mklement0/voices/stable/osx-service/Speak%20With%20Specific%20Voice.workflow.zip).
* In Finder, open the ZIP file, which creates package `Speak With Specific Voice.workflow` in the same folder.
* Open `Speak With Specific Voice.workflow` and choose `Install` when prompted - this will place the package in `~/Library/Services/`.
* Choose `Open in Automator` when prompted and proceed with customization below.

## Customization

* In Automator, follow the instructions at the top of the document, which currently only require you to specify the name of the desired voice.
  * Apply your customizations between the lines `#  ------- BEGIN: CUSTOMIZE` and `#  ------- END: CUSTOMIZE`.
* To assign a keyboard shortcut, go to `System Preferences > Keyboard > Shortcuts`, category `Services`, scroll to sub-category `General` in the list on the right, select `Speak With Specific Voice.workflow`, and click just inside the right edge of the list item.
* To customize the service again later, open `~/Library/Services/Speak With Specific Voice.workflow` in Automator.
  * If you have trouble navigating to `~/Library`, activate Finder, hold down the Option key while selecting the `Go` menu, and select `Library`; from there, navigate to subfolder `Services` and open package `Speak With Specific Voice.workflow`.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015-2018 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [marked-man (D)](https://github.com/kapouer/marked-man#readme)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://git.sdf.org/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.3.4](https://github.com/mklement0/voices/compare/v0.3.3...v0.3.4)** (2018-03-21):
  * [doc] Links to workflow ZIPs fixed.

* **[v0.3.3](https://github.com/mklement0/voices/compare/v0.3.2...v0.3.3)** (2018-03-08):
  * [fix] It is now ensured that the system versions of standard utilities such
          as `awk` are called, to prevent unexpected behavior stemming from 
          user-installed versions in /usr/local/bin getting called.

* **[v0.3.2](https://github.com/mklement0/voices/compare/v0.3.1...v0.3.2)** (2017-01-03):
  * [doc] Limitations of support for third-party voices noted.
  * [fix] `voices -m` now works on macOS Sierra.

* **[v0.3.1](https://github.com/mklement0/voices/compare/v0.3.0...v0.3.1)** (2015-11-03):
  * [doc] Added link to Alfred 2 workflow _speak.waf_ as a superior alternative
    to the OSX services.

* **[v0.3.0](https://github.com/mklement0/voices/compare/v0.2.3...v0.3.0)** (2015-10-27):
  * [Potentially breaking change] `-i` for reporting voice internals now reports
    an extra variable `BundleID` as the last item, i.e., the voice's bundle ID.

* **[v0.2.3](https://github.com/mklement0/voices/compare/v0.2.2...v0.2.3)** (2015-09-20):
  * [doc] `voices` now has a man page (if manually installed, use `voices --man`);
          `voices -h` now just prints concise usage information.

* **[v0.2.2](https://github.com/mklement0/voices/compare/v0.2.1...v0.2.2)** (2015-09-15):
  * [dev] Makefile improvements; various other behind-the-scenes tweaks.

* **[v0.2.1](https://github.com/mklement0/voices/compare/v0.2.0...v0.2.1)** (2015-07-30):
  * [doc] Read-me corrections.

* **[v0.2.0](https://github.com/mklement0/voices/compare/v0.1.9...v0.2.0)** (2015-07-29):
  * [enhancement] `voices` now honors custom speaking rates when requested to speak with the `-k` option
  * [enhancement] OSX Service `Switch Default Voice.workflow` is now configuration file-based and supports more than 2 voices for cyclical switching; default confirmation text spoken on switching is now the localized name of the new voice's language.
  * [new] OSX Service `Speak With Specific Voice.workflow` allows speaking selected text with a fixed alternate voice.

* **[v0.1.9](https://github.com/mklement0/voices/compare/v0.1.8...v0.1.9)** (2015-07-28):
  * [doc] Corrected the mistaken claim that changing the default voice also changes the _VoiceOver_ default voice: the VoiceOver feature has its own default voice, separate from the TTS feature; this utility only changes the _TTS_ default voice, not also the VoiceOver one.

* **[v0.1.8](https://github.com/mklement0/voices/compare/v0.1.7...v0.1.8)** (2015-07-28):
  * [fix] Regression: default customization data for the OSX service reset to original values (two US English-voices).

* **[v0.1.7](https://github.com/mklement0/voices/compare/v0.1.6...v0.1.7)** (2015-07-28):
  * [fix] When switching default voices, any custom speaking rate (words per minute) configured for a given voice via System Preferences is now honored. Note, however, that speaking text with `voices`' own `-k` option does _not_ honor custom speaking rates due to a limitation in the underlying `say` utility.

* **[v0.1.6](https://github.com/mklement0/voices/compare/v0.1.5...v0.1.6)** (2015-07-28):
  * [dev] Pre-commit hook fixed to ensure that the modified workflow and ZIP file are added to the index before committing.

* **[v0.1.5](https://github.com/mklement0/voices/compare/v0.1.4...v0.1.5)** (2015-07-27):
  * [doc] Read-me and CLI help amended with respect to supported OSX versions.

* **[v0.1.4](https://github.com/mklement0/voices/compare/v0.1.3...v0.1.4)** (2015-07-27):
  * [enhancement] Added Automator-based OSX service for switching between two default voices.
  * [fix] Inability to determine the default voice on a pristine system is now handled more gracefully.

* **[v0.1.3](https://github.com/mklement0/voices/compare/v0.1.2...v0.1.3)** (2015-07-06):
  * [doc] CLI-help copyediting; wording of `--version` streamlined.

* **[v0.1.2](https://github.com/mklement0/voices/compare/v0.1.1...v0.1.2)** (2015-07-01):
  * [doc] CLI help improved.

* **[v0.1.1](https://github.com/mklement0/voices/compare/v0.1.0...v0.1.1)** (2015-06-30):
  * [doc] Read-me improvements.

* **v0.1.0** (2015-06-29):
  * Initial release.
