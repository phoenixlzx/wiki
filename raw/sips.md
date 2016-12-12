title: sips
category: OS X
time: 1476133839
---
Using SIPS to convert and resize images in OS X.

### Convert format

```
sips -s format png image.jpg --out png/image.png
```

Batch command

```
mkdir -p png
for i in *.jpg; do sips -s format png $i --out png/$i.png;done
```

### Resize

Resize size to 512px on max width/height, and keep the aspect ratio.

```
sips -Z 512 image.png
```
### Set DPI

```
sips -s dpiHeight 72 -s dpiWidth 72 image.png
```

