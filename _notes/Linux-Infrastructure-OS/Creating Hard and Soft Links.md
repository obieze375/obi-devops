
[[Index]] 

## Example:

  

## Create two files:

```bash 

$ touch blah1

  

$ touch blah2

``` 

## Enter some data into them:

```bash 

$ echo "Cat" > blah1

  

$ echo "Dog" > blah2

``` 

  

And as expected:

```bash 

$cat blah1; cat blah2

  

Cat

Dog

``` 

## Let's create hard and soft links:

  

```bash 

$ ln blah1 blah1-hard

  

$ ln -s blah2 blah2-soft

``` 

Let's see what just happened:

```bash 

$ ls -l

  

blah1

blah1-hard

blah2

blah2-soft -> blah2

``` 

## Changing the name of blah1 does not matter:

```bash 

$ mv blah1 blah1-new

  

$ cat blah1-hard

``` 

Cat

  

blah1-hard points to the inode, the contents, of the file - that wasn't changed.

```bash 

$ mv blah2 blah2-new

  

$ ls blah2-soft

  

blah2-soft

``` 

  

```bash 

$ cat blah2-soft

  

cat: blah2-soft: No such file or directory

```

The contents of the file could not be found because the soft link points to the name, that was changed,

and not to the contents.

Similarly, If blah1 is deleted, blah1-hard still holds the contents; if blah2 is deleted, blah2-soft is just a link

to a non-existing file.