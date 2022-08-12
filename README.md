## Creating a CI for Maven Project using GH Actions

**This project is set up to use maven to build the jar executable (maven).I had build a CI Pipeline for a Maven application using Github Actions, in that pipeline we will define the various steps like building, Testing, and Packaging the application.**

**maven.yml**

```
name: Maven CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 11
      uses: actions/setup-java@v2
      with:
        java-version: '11'
        distribution: 'temurin'
        cache: maven
    - name: Build with Maven
      run: mvn compile
    - name: Test with Maven
      run: mvn test
    - name: Package with maven
      run: mvn package
    - run: mkdir artifact && cp target/*.jar artifact
    - uses: actions/upload-artifact@v2
      with:
       name: Package
       path: artifact  
```

### JVM version and architecture

```
steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 11
      uses: actions/setup-java@v2
      with:
        java-version: '11'
        distribution: 'temurin'
        cache: maven
```

### Build and Test the Code

```
 - name: Build with Maven
   run: mvn compile
 - name: Test with Maven
   run: mvn test
```

### Packaging and Uploading artifact

** package command which helps us to generate a jar file, we will be using the same with our GitHub Actions. Maven will usually create output files like JARs, EARs, or WARs in the target directory**

```
- name: Package with maven
  run: mvn package
```

**make a directory with the artifact name and we will copy this jar file**

```
- run: mkdir artifact && cp target/*.jar artifact
```

**Now we can upload the contents of that directory using the upload-artifact action**

```
- uses: actions/upload-artifact@v2
      with:
       name: Package
       path: artifact 
```