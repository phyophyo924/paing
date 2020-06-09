## Summary

* [Prerequisites](#prerequisites)
* [Project Configuration](#project-configuration)
* [Importing the SDKs](#importing-the-sdks)
    * [Remotely](#remotely)
    * [Locally](#locally)
* [Add permissions to your AndroidManifest.xml](#add-permissions-to-your-androidmanifestxml)

## Prerequisites

| Setting            | Version |
|--------------------|---------|
| `minSdkVersion`    | 21      |
| `targetSdkVersion` | 29      |
| `Java version`     | 8       |

* Internet connection
* A valid [combateafraude](https://combateafraude.com) Mobile token. To get one, please mail to [Frederico Gassen](mailto:frederico.gassen@combateafraude.com)

## Project configuration

Add this configuration in your app-level `build.gradle`:

``` java
android {

    ...

    buildFeatures {
        dataBinding = true
    }

    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }

    // Only in DocumentDetector case
    aaptOptions {
        noCompress "tflite"
    }
}
```

Our SDKs uses [Data Binding](https://developer.android.com/topic/libraries/data-binding). To use it, you need to enable it in your app. The `compileOptions` is necessary because the libraries use lambda functions, only introduced in Java 8. The `noCompress` code is necessary only in `DocumentDetector` integration. It's necessary to run the `ImageClassifier` to detect the documents.

## Importing the SDKs

### Remotely

To remotely import our libraries, first you need to add our SDK respository in your top-level `build.gradle`:

``` java
buildscript {

    ...

}

allprojects {
    repositories {
        
        ...

        maven { url 'https://repo.combateafraude.com/android/release' }
    }
}
```

Then, add this to your dependencies in your app-level `build.gradle`

``` java
dependencies {
    
    ...
    
    implementation 'com.combateafraude.sdk:<SDK_name>:<SDK_version>'
}
```

### Locally

1. Get the correspondent `.aar` file in the [version releases page](https://repo.combateafraude.com) and check the respective `.pom` file to get the transitive dependencies that the SDK needs.

2. Place the `.aar` in the `<project_folder>/app/libs` folder.

3. Open your app-level `build.gradle`, add the `libs` folder as a repository, add the `.aar` file and all the transitive dependencies:

``` java
android {

    ...

}

dependencies {
    
    ...
    
    implementation '<package-name>:<file-name>@aar'
}

repositories {
    flatDir {
       dirs 'libs'
    }
}
```

## Add permissions to your `AndroidManifest.xml`

When working with our libraries and Android API 23+, you'll have to request some runtime permissions for our SDKs. Each module requires a different set of permissions, so check the module-specific page on the [wiki](.) page to see what permissions the module you are importing requires.