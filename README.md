# Linux Shell One Liners

This repo is just an ongoing collection of shell one liners that may be useful.  


List all executable binaries in your `$PATH`
```shell
find $(echo ${PATH//:/ }) -maxdepth 1 -executable -type f
```

Expose unpatched CVE in the gnu `patch` commmand :trollface:
```shell
docker run -i -t alpine:latest sh -c "apk --update add git patch;git config --global user.email 'nobody';git config --global user.name 'cares';git init;echo 'a' > some_file;git add some_file;git commit -m 'Initial commit';echo -e 'b' > some_file;git diff | patch nothing;"
```

Generate strong random passwords
```shell
< /dev/urandom tr -dc A-Za-z0-9 | head -c32
```

Generate random MAC address, useful for creating virtual interfaces
```shell
hexdump -vn3 -e '/3 "52:54:00"' -e '/1 ":%02x"' -e '"\n"' /dev/urandom
```