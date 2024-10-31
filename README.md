This repo is a reproductible example of the issue presented in [this
StackOverflow question](https://stackoverflow.com/q/79135123/1673776).

---

### Producing the problematic image

Building the image can be done through:

```sh
bazel run //my_image:tarball
```

Then, the issue can be reproduced by running a container on that image:

```sh
docker run --rm -it bazel-oci-image-issue/my_image:latest /bin/bash
exec /bin/bash: no such file or directory
```

Now, `/bin/bash` exists, which can be seen by analyzing the file system on a
container built from this image. You can retrieve the file system through:

```sh
cid=$(docker create bazel-oci-image-issue/my_image:latest /bin/bash)
docker export $cid -o rootfs.tar.gz
```

### Producing a working image

Inside `my_image/BUILD.bazel`, comment the `"@noble//qbittorrent-nox"` line in
the `_PACKAGES` list, and follow the same steps as above.
