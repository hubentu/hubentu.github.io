---
layout: post
date: 2019/12/9
tag:
  - linux
---

# Keeping structure when copy folder/file in Linux.

```sh
$mkdir a
$touch a/abc
```

Copy a to another folder
```
$cp -r a b/
$mkdir c && cp -r a c/
$tree a b c
a
└── abc
b
└── abc
c
└── a
    └── abc
```
If copy to a new folder, the original folder structure will not be kept.

To copy a specific file with `--parents` will keep orginal structure.
```sh
$mkdir d
$cp -r --parents a/abc d/
$tree d
d
└── a
    └── abc
```

The `rsync` will keep folder structure even if the sync folder not existing.
```
$rsync -a a e/
$tree e
e
└── a
    └── abc
```

```
$tree
├── a
│   └── abc
├── b
│   └── abc
├── c
│   └── a
│       └── abc
├── d
│   └── a
│       └── abc
└── e
    └── a
        └── abc
```
