# `sh` Shell array or string iteration

`sh` effectively does support arrays.
It just supports them as space separated strings.
You can print the entire contents of them, "push" to them, or iterate through them just fine.

```sh
NAMES=""
NAMES="${NAMES} MYNAME"
NAMES="${NAMES} YOURNAME"
NAMES="${NAMES} THEIRNAME"

echo 'One at a time...'
for NAME in ${NAMES}; do
    echo ${NAME};
done

echo 'All together now!'
echo ${NAMES}
```

Which outputs:

```sh
One at a time...
MYNAME
YOURNAME
THEIRNAME
All together now!
MYNAME YOURNAME THEIRNAME
```

`sh` doesn't support sub-scripting, but with a little bit of cut magic and using the space as a proper delimiter, you can absolutely emulate that. If we add this to the bottom of our above example:

```sh
echo 'Get the second one'
echo ${NAMES} | cut -d' ' -f2

echo 'Add one more...'
NAMES="${NAMES} TOM"

echo 'Grab the third one'
echo ${NAMES} | cut -d' ' -f3
```

And we get

```sh
Get the second one
YOURNAME
Add one more...
Grab the third one
THEIRNAME
```

But what if you have something like `alphabet="a b c d e f g h i j k l m n o p q r s t u v w x y z"`  and you want to iterate over?
The aobve won't work...

Here's what will work:

```sh
alphabet="a b c d e f g h i j k l m n o p q r s t u v w x y z"
index="0"
for character in $alphabet; do
  index=$(($index+1))
  echo "$index. $character"
done
```

Obviously don't need the `index` if you don't want to have it...

IMPORTANT!!!

in the above `for character in $alphabet; do` works but `for character in "$alphabet"; do` does not work!!!
