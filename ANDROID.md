# Coding Practices in Android Development

## Summary

#### [Use Gradle and its default project structure](#build-system)



### Gradle configuration

**Prefer Maven dependency resolution to importing jar files.** If you explicitly include jar files in your project, they will be a specific frozen version, such as `2.1.1`. Downloading jars and handling updates is cumbersome and is a problem that Maven already solves properly. Where possible, you should attempt to use Maven to resolve your dependencies, for example:

```groovy
dependencies {
    implementation 'com.squareup.okhttp:okhttp3:3.8.0'
}
```

**Avoid Maven dynamic dependency resolution**
Avoid the use of dynamic dependency versions, such as `2.1.+` as this may result in different and unstable builds or subtle, untracked differences in behavior between builds. The use of static versions such as `2.1.1` helps create a more stable, predictable and repeatable development environment.