### find

```find $HOME -name 'calc_gc.py'```
```find $HOME -iname '*.py'```
(-iname: case-insensitive)
```sudo find / -iname '*.py'```
(/: search on the entire drive)
```find $HOME -iname '*.ogg' -size +100M```
(-size +100M: look for files that are 100MB or larger)