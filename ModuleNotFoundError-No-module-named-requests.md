# ModuleNotFoundError-No-module-named-requests
## System Version
macOS 10.15.7

## Problem
`import requests`

`ModuleNotFoundError: No module named 'requests'`

## Solution
Execute code

`brew link python3`
`pip3 install requests`

Cause Analysis
I guess i has two different python version in my system, One is the system and other is brew install.

```
~ $ which python3
/usr/local/bin/python3
~ $ ls -all /usr/local/bin/python3
lrwxr-xr-x 1 3vand0ng admin 40 Oct 28 17:15 /usr/local/bin/python3 -> ../../../Library/Frameworks/Python.framework/Versions/3.10/bin/python3
~ $ which pip3
/usr/local/bin/pip3
~ $ ls -all /usr/local/bin/pip3
lrwxr-xr-x 1 3vand0ng admin 40 Oct 28 17:15 /usr/local/bin/pip3 -> ../Cellar/python@3.10/3.19.8/bin/pip3
```
