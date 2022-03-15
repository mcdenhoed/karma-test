# KarmaTest

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 13.2.4.

## What is this?

This almost-empty project is set up to require karma to report 100% statement code coverage. The actual
test coverage is 75%, so we would expect karma-coverage to return with a nonzero error code in a 
headless CI-type context.

## What's wrong

With the current configuration, we get an expected message stating that the statement coverage is below
the threshold specified in `karma.conf.js`.

```
~/P/karma-test ❯❯❯ npm run coverage                                                                                                                                                                                                                                                                           master ✚ ✱

> karma-test@0.0.0 coverage
> ng test --watch=false --browsers=ChromeHeadless --code-coverage

Node.js version v17.5.0 detected.
Odd numbered Node.js versions will not enter LTS status and should not be used for production. For more information, please see https://nodejs.org/en/about/releases/.
✔ Browser application bundle generation complete.
15 03 2022 11:24:12.448:INFO [karma-server]: Karma v6.3.17 server started at http://localhost:9876/
15 03 2022 11:24:12.448:INFO [launcher]: Launching browsers ChromeHeadless with concurrency unlimited
15 03 2022 11:24:12.451:INFO [launcher]: Starting browser ChromeHeadless
15 03 2022 11:24:12.918:INFO [Chrome Headless 99.0.4844.51 (Mac OS 10.15.7)]: Connected on socket frDtDy3aGnPQMqDYAAAB with id 63665064
Chrome Headless 99.0.4844.51 (Mac OS 10.15.7): Executed 3 of 3 SUCCESS (0.072 secs / 0.055 secs)
TOTAL: 3 SUCCESS
15 03 2022 11:24:13.241:ERROR [coverage]: Chrome Headless 99.0.4844.51 (Mac OS 10.15.7): Coverage for statements (75%) does not meet global threshold (100%)

=============================== Coverage summary ===============================
Statements   : 75% ( 3/4 )
Branches     : 100% ( 0/0 )
Functions    : 0% ( 0/1 )
Lines        : 66.66% ( 2/3 )
================================================================================

```

However, the error code, at least on my computer (TM), is 0:
```
~/P/karma-test ❯❯❯ echo $status                                                                                                                                                                                                                                                                               master ✚ ✱
0
```

After some experimentation I tried replacing the following:

```json
      reporters: [
        {type: "text-summary"},
        {type: "html"},
      ]
```

with the following, swapping the order of the reporters array:

```json
      reporters: [
        {type: "html"},
        {type: "text-summary"},
      ]
```

This then seemed to make karma behave as expected:

```
~/P/karma-test ❯❯❯ npm run coverage                                                                                                                                                                                                                                                                           master ✚ ✱

> karma-test@0.0.0 coverage
> ng test --watch=false --browsers=ChromeHeadless --code-coverage

Node.js version v17.5.0 detected.
Odd numbered Node.js versions will not enter LTS status and should not be used for production. For more information, please see https://nodejs.org/en/about/releases/.
✔ Browser application bundle generation complete.
15 03 2022 11:31:43.243:INFO [karma-server]: Karma v6.3.17 server started at http://localhost:9876/
15 03 2022 11:31:43.244:INFO [launcher]: Launching browsers ChromeHeadless with concurrency unlimited
15 03 2022 11:31:43.246:INFO [launcher]: Starting browser ChromeHeadless
15 03 2022 11:31:43.894:INFO [Chrome Headless 99.0.4844.51 (Mac OS 10.15.7)]: Connected on socket 3B1pQBMDA0amN9wWAAAB with id 62278864
Chrome Headless 99.0.4844.51 (Mac OS 10.15.7): Executed 3 of 3 SUCCESS (0.068 secs / 0.054 secs)
TOTAL: 3 SUCCESS
15 03 2022 11:31:44.196:ERROR [coverage]: Chrome Headless 99.0.4844.51 (Mac OS 10.15.7): Coverage for statements (75%) does not meet global threshold (100%)

=============================== Coverage summary ===============================
Statements   : 75% ( 3/4 )
Branches     : 100% ( 0/0 )
Functions    : 0% ( 0/1 )
Lines        : 66.66% ( 2/3 )
================================================================================
~/P/karma-test ❯❯❯ echo $status                                                                                                                                                                                                                                                                           ✘ 1 master ✚ ✱
1

```
