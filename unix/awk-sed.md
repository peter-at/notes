# awk and sed

# print between 2 patterns
from [stackoverflow](https://stackoverflow.com/a/38972737)
```
$ awk '/PAT1/{flag=1; next} /PAT2/{flag=0} flag' file
```
