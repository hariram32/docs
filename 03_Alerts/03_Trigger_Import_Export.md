<!-- @@@title:Trigger Import Export@@@ -->

# Trigger Import and Export

# Format
LogZilla Triggers are stored in standard JSON format. As of `v5.69.2`, triggers can be imported and exported from the command line, for example:

Import
---
    ~logzilla/src/bin/lz5triggers import -I mytriggers.json

Export
---
    ~logzilla/src/bin/lz5triggers export -O mytriggers.json


# Repository
As a courtesy to our users, we've created [a Github repository](https://github.com/logzilla/extras) containing examples of user-contributed scripts which can be used for automated actions. Be sure to check there before writing your own.
> Note: Users are also encouraged to contribute to the Github repo!
