<!-- @@@title:Dashboard Import Export@@@ -->

# Dashboard Import And Export

LogZilla Dashboards can be imported and exported for sharing, modification, etc.

As a courtesy to our users, we've created [a Github repository](https://github.com/logzilla/extras) containing examples of user-contributed dashboards. Be sure to check there before writing your own.
> Note: Users are also encouraged to contribute to the Github repo! 

## Format
LogZilla Dashboards are stored in standard JSON format. As of `v5.69.2`, dashboards can be imported and exported from the command line, for example:

Import
---
    ~logzilla/src/bin/lz5dashboards import -I mydashboards.json

Export
---
    ~logzilla/src/bin/lz5dashboards export -O mydashboards.json


Import/Export from the UI
---

LogZilla version > `5.75` can do the same import and export directly from the UI.
