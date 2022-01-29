# Floating-Bubble-View-Library-Android

[![platform](https://img.shields.io/badge/platform-Android-yellow.svg)](https://www.android.com)
[![API](https://img.shields.io/badge/API-21%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=21)
![GitHub stars](https://img.shields.io/github/stars/TorryDo/Floating-Bubble-View?style=social)
![GitHub forks](https://img.shields.io/github/forks/TorryDo/Floating-Bubble-View?label=Fork&style=social)
![Repo size](https://img.shields.io/github/repo-size/TorryDo/Floating-Bubble-View?style=social)

# Demo

<!-- <img src="art/demo.gif"/> -->


https://user-images.githubusercontent.com/85553681/151544981-1e080474-77a5-48e7-922d-b01de19cf89a.mp4



<br/>

# I, Prepare

- ### <b> STEP 1. Add the JitPack repository to your build.gradle (Project) file </b>

Add it in your root build.gradle at the end of repositories:

```gradle
    // not buildScript
    allprojects {
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }

```

## If anything go WRONG, forget the step above. Go to your setting.gradle file

then add maven repository inside "dependencyResolutionManagement => repositories" like below

```gradle
    dependencyResolutionManagement {
        repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
        repositories {
            google()
            mavenCentral()

            // add here
            maven { url 'https://jitpack.io' }

            jcenter() // Warning: this repository is going to shut down soon
        }
    }
```

<br/>

- ### <b> STEP 2. Add dependency in your app module </b>

```gradle
    dependencies {
            implementation 'com.github.TorryDo:Floating-Bubble-View:0.2.0'
    }

```

# II, How to use

- ### <b> Step 1 : create a class extending FloatingBubbleService </b>

```java
    public class MyService extends FloatingBubbleService {
        ...
    }
```

</br>

- ### <b> Step 2 : override 2 methods "setupBubble" and "setupExpandableView" </b>

```java
    public class MyService extends FloatingBubbleService {

        @NonNull
        @Override
        public FloatingBubble.Builder setupBubble() {
            return ...;
        }

        @NonNull
        @Override
        public ExpandableView.Builder setupExpandableView(@NonNull ExpandableView.Action action) {
            return ...;
        }
    }
```

</br>

- ### <b> Step 3 : add your service class in manifest file... </b>

```xml
    <service android:name="<YOUR_PACKAGE>.MyService" />
```

</br>

- ### <b> Step 4 : start your service and enjoy :) </b>

```java
    Intent intent = new Intent(MainActivity.this, MyService.class);
    startService(intent);
```

# Sample class

```java
public class MyService extends FloatingBubbleService {

    @NonNull
    @Override
    public FloatingBubble.Builder setupBubble() {
        return new FloatingBubble.Builder()

                // context is required
                .with(this)

                // set bubble icon
                .setIcon(R.drawable.ic_star)

                // set remove bubble icon
                .setRemoveIcon(R.mipmap.ic_launcher_round)

                // set bubble size in dp
                .setBubbleSizeDp(60)

                // set the point where the bubble appear
                .setStartPoint(-200, 0)

                // set alpha\opacity of the bubble
                .setAlpha(1f)

                // add listener
                .addFloatingBubbleTouchListener(new FloatingBubble.TouchEvent() {
                    @Override
                    public void onDestroy() { System.out.println("on Destroy"); }

                    @Override
                    public void onClick() { System.out.println("onClick"); }

                    @Override
                    public void onMove(int x, int y) { System.out.println("onMove"); }

                    @Override
                    public void onUp(int x, int y) { System.out.println("onUp"); }

                    @Override
                    public void onDown(int x, int y) { System.out.println("onDown"); }
                });
    }


    @NonNull
    @Override
    public ExpandableView.Builder setupExpandableView(@NonNull ExpandableView.Action action) {

        LayoutInflater inflater = (LayoutInflater) getSystemService(LAYOUT_INFLATER_SERVICE);
        View layout = inflater.inflate(R.layout.layout_view_test, null);


        layout.findViewById(R.id.card_view).setOnClickListener(v -> {
            Toast.makeText(this, "hello from card view from java", Toast.LENGTH_SHORT).show();
            action.popToBubble();
        });

        return new ExpandableView.Builder()

                // context is required
                .with(this)

                // layout is required
                .setExpandableView(layout)

                // set expandable view dim
                .setDimAmount(0.7f);
    }
}
```
