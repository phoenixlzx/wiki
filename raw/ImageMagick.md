title: ImageMagick
category: Tools
time: 1486210417848
---

## Batch Operation

```
mogrify -format png *.jpg
```

## Resize on largest dimension

```
convert -resize "100x100>" image.png
```

## Optimize PNG

```
optipng -o5 image.png
```
