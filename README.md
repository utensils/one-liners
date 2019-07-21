# Linux Shell One Liners & Functions

This repo is just an ongoing collection of shell one liners that may be useful.  

## Misc
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

## AWS Related

Quickly switch between AWS accounts:  
example: `aws-profile sandbox`
```shell
aws-profile(){
    export AWS_PROFILE="$1"
    export AWS_EB_PROFILE="$1"
    aws sts get-caller-identity | \
        jq -r --arg PROFILE "$AWS_PROFILE" '"Using AWS Account: "+ .Account + " (" + $PROFILE + ")"'
}
```

Give s3 bucket owner full permisions to all objects:  
example: `aws-set-bucket-owner somebucket-name`
```shell
aws-set-bucket-owner(){
    while read object
    do
        echo "Updating Object: $object"
        aws s3api put-object-acl --acl bucket-owner-full-control --bucket "$1" --key "$object"
    done < <(aws s3 ls "s3://$1/" --recursive | sed -r 's/^[0-9]{4}-[0-9]{2}-[0-9]{2} [0-9]{2}:[0-9]{2}:[0-9]{2}[[:space:]]+[0-9]+ //g')
}
```
