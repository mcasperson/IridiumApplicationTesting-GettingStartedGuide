# Building

Run the following command to build Iridium:

```
./gradlew clean shadowJar
```

If you get the error

```
Exception in thread "main" java.lang.NoClassDefFoundError: javaslang/?
```

after building the uberjar in Windows, set the character encoding to UTF-8:

```
./gradlew clean shadowJar "-Dfile.encoding=UTF-8"
```