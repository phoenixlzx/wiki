title: Handy Commands & Scripts
category: System Administration
time: 1483670963358
---

## Copy specified file/directory and keep folder structure

```
find . -name 'filename.ext' -exec cp --parents \{\} [target directory] \;
```

References

* [Copy specific file type keeping the folder structure](http://unix.stackexchange.com/questions/83593/)
