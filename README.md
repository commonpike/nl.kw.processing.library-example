20181114*pike
# Example Processing 3 Library

Does nothing. Just explains a library structure and is a working example.

This README is best read at 
https://github.com/commonpike/nl.kw.processing.library-example

## Install location

This whole folder should go into your "sketchbook location",
also sometimes called the "processing library directory".
You can find its location in the Processing app, in the menu,
under preferences. It's usually in your homedir somewhere.

This whole folder should be called ‘ExampleFooLibrary’.
If you cloned it from GIT, rename it.

## Folder structure

If the Library folder is called `ExampleFooLibrary`
 - it must contain a folder `ExampleFooLibrary/library`
 - it must contain a file `ExampleFooLibrary/library.properties`
 - it must contain a file `ExampleFooLibrary/library/ExampleFooLibrary.jar`

This is all that is required to get it to work. 
If it has these things, and it is in the right location,
you can see it in your Processing app under Sketch > Import Library

The folder may also contain other things, as this
one does. Some are related to //publishing// the library 
(read more below). Some are folders I personally use to 
//compile// the java code on the spot. You can delete all that,
and it will still work.


## Import logic

You will notice this library only contains
one jar file, the `./library/ExampleFooLibrary.jar`. 
That jar only contains one java package, the `nl.kw.processing.ExampleBazPackage`.
That package contains two classes, `ExampleBar` and `ExampleQuz`.

If you 'import' the library in the Processing app, all 
it does is write 

`import nl.kw.processing.ExampleBazPackage.*`

in your code. And when you run your code, when compiling,
it tries to read the classes from the jar file in the library folder.

In the jar file, there could be more packages (?). 
Importing it would then cause more import statements
to be added to your code, that's all.

In the library folder, there could also be more jar files. 
For these files, import statements will not be written automatically.
Only the jar file with the exact same name as the library
gets imported automagically.



See also:
https://github.com/processing/processing/wiki/Library-Basics

See also:
https://processing.org/tutorials/eclipse/

## Compiling 

Most people will use some IDE like eclipse to generate
the required jar file. But the hard way is

- write *.java files like the ones in ./src, using vi ofcourse :-)
- compile the .java files to .class files 
- zip the .class files to a .jar  

```

cd src
vi ExampleBar.java
vi folder/foo/whatever/ExampleQuz.java
cd ../

javac -d build/ -classpath /path/to/processing/core.jar \
 ./src/*.java ./src/folder/foo/whatever/*java

jar -cf library/ExampleFooLibrary.jar -C build/ .
  
  
```

or just take a look at `bin/compile.sh` in this repo,
for linux and mac users.

## Compile problems 

If you have problems compiling, you may be using the wrong java
version (JRE or JDK). Check what `java -version` and `javac -version` say.

If your code came from a sketch, and during compiling you get errors like 
``error: cannot find symbol (...) createShape``
remember your code is not part of a PApplet anymore. 
`createShape` is now knwon as `processing.core.PApplet.createShape()`;
but since that is not a  static method it needs a PApplet
to work. You will have to rewrite your code.

Also a lot of math functions like `abs()`, `floor()` etc. can
be rewritten to `Math.abs()`, `Math.floor()`. 

But Processings `random()` and `noise()` don't belong to 
java.lang.Math. You'll have to rewrite them.

Also remember, `color` is not a java type. Rewrite it to `int`.

All this rewriting is done in the PDE by the //PDE Precompiler//,
but now you’ll have to do it by hand. 

## Publishing

As mentioned under //folder structure// above, 
only these files are needed to get it to work:

 - the folder `ExampleFooLibrary/library`
 - the file `ExampleFooLibrary/library.properties`
 - the file `ExampleFooLibrary/library/ExampleFooLibrary.jar`

If you want to publish it, you must follow some additional
guidelines. According to these guidelines, this folder 
should also contain

 - `ExampleFooLibrary/reference/` - documentation in HTML format as generated from Javadoc
 - `ExampleFooLibrary/examples/`  - a set of example PDE sketches 
 - `ExampleFooLibrary/src/` - java source

So these folders in this library are not needed at all:

 - ExampleFooLibrary/bin  is not required;
 - ExampleFooLibrary/build is not required;
 - ExampleFooLibrary/dist is not required;
 - ExampleFooLibrary/README.md is not required.

Again according to the guidelines, you need to have
two files on a public location with a fixed url. This is 
what you will ‘publish’. I keep a copy of those two
files in the `dist/` folder:

 - a HTML summary ; see `dist/summary.html`
 - a zip file with the required files in it; see `dist/ExampleFooLibrary.zip` on GIT

Put these files online (I use github.io) and communicate
the urls with the Processing maintainers. If they agree
to publish it, apparently they will check the library.properties
in the published zip regularly to see if there is a new version.

See also:
https://github.com/processing/processing/wiki/Library-Guidelines
