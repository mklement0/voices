Contains an OSX Automator-based service for switching the default voice
between two designated voices, using an embedded `voices` instance.

A Git pre-commit hook (`.git/hooks/pre-commit`) does the following:

* copies the latest `voices` CLI from `bin` into the `*.workflow` bundle
* zips up the bundle for simple download from GitHub

