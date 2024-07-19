# The-News-App-Project

## Table of Contents
- Project Overview
- Project Demo
- Technical Talks about Project
- Source Code Fragments
- XML Layouts
- Navigation Graph
- Conclusion

## Project Overview
This project demonstrates a news app built with Android, covering five major advanced topics: MVVM, Retrofit, Navigation Components, Coroutines, and Room Databases.

## Project Demo
The app has a bottom navigation with three categories: headlines, favorites, and search. The latest news is fetched from the News API in real-time, and articles can be saved to the favorites section. The search feature retrieves articles based on user queries.

## Technical Talks about Project
- **API Integration**: The News API fetches real-time data.
- **WebView**: Opens articles within the app.
- **Room Database**: Saves favorite articles locally.
- **MVVM**: Organizes the project into Model-View-ViewModel.
- **Navigation Component**: Handles navigation between screens.

## Source Code Fragments

### Colors
```xml
<!-- colors.xml -->
<color name="black">#FF000000</color>
<color name="white">#FFFFFFFF</color>
<color name="purple">#593c8f</color>
<color name="yellow">#FFC000</color>

## Themes
<!-- themes.xml -->
<style name="Base.Theme.NewsProjectPractice" parent="Theme.Material3.DayNight.NoActionBar">
    <item name="colorPrimary">@color/purple</item>
    <item name="colorPrimaryDark">@color/purple</item>
    <item name="colorAccent">@color/purple</item>
</style>

<style name="Theme.NewsProjectPractice" parent="Base.Theme.NewsProjectPractice" />

<style name="App.Custom.Indicator" parent="Widget.Material3.BottomNavigationView.ActiveIndicator">
    <item name="android:color">@color/yellow</item>
</style>

##Drawables
<!-- search_border.xml -->
<!-- XML content for search border drawable -->

Gradle Files
build.gradle (Project)
buildscript {
    repositories {
        google()
    }
    dependencies {
        classpath("androidx.navigation:navigation-safe-args-gradle-plugin:2.7.5")
    }
}

plugins {
    id("com.android.application") version "8.1.1" apply false
    id("org.jetbrains.kotlin.android") version "1.9.0" apply false
    id("com.google.devtools.ksp") version "1.9.0-1.0.13" apply false
}

build.gradle (Module)
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
    id("com.google.devtools.ksp")
    id("androidx.navigation.safeargs.kotlin")
}

android {
    namespace = "com.example.newsprojectpractice"
    compileSdk = 34

    defaultConfig {
        applicationId = "com.example.newsprojectpractice"
        minSdk = 28
        targetSdk = 33
        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(getDefaultProguardFile("proguard-android-optimize.txt"), "proguard-rules.pro")
        }
    }

    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }

    kotlinOptions {
        jvmTarget = "1.8"
    }

    buildFeatures {
        viewBinding = true
    }
}

dependencies {
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.10.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.ext:junit:1.1.5")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.2")
    implementation("androidx.room:room-runtime:2.6.0")
    ksp("androidx.room:room-compiler:2.6.0")
    implementation("androidx.room:room-ktx:2.6.0")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.1")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.1")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.6.2")
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.okhttp3:logging-interceptor:4.5.0")
    implementation("androidx.navigation:navigation-fragment-ktx:2.7.5")
    implementation("androidx.navigation:navigation-ui-ktx:2.7.5")
    implementation("com.github.bumptech.glide:glide:4.12.0")
    ksp("com.github.bumptech.glide:compiler:4.12.0")
}

Bottom Navigation Menu
<!-- bottom_navigation_menu.xml -->
<!-- XML content for bottom navigation menu -->

Fragments XML Layouts
fragment_headlines.xml
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/headlinesRecyclerView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:listitem="@layout/headlines_item_layout" />
    
    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/saveArticleFab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:visibility="@{viewModel.isSaveVisible ? View.VISIBLE : View.GONE}"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:srcCompat="@drawable/ic_baseline_save_24" />
</androidx.constraintlayout.widget.ConstraintLayout>

fragment_favourites.xml
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/favoritesRecyclerView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:listitem="@layout/favorites_item_layout" />
    
    <com.google.android.material.snackbar.Snackbar
        android:id="@+id/undoSnackbar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        android:text="Item removed"
        android:actionTextColor="@color/yellow"
        app:actionText="UNDO" />
</androidx.constraintlayout.widget.ConstraintLayout>

fragment_search.xml
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/searchRecyclerView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"
        app:layout_constraintTop_toBottomOf="@id/searchEditText"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        tools:listitem="@layout/search_item_layout" />
</androidx.constraintlayout.widget.ConstraintLayout>

Navigation Graph
<!-- nav_graph.xml -->
<!-- XML content for navigation graph -->

Conclusion
This concludes my brief overview of the news application project. The complete info is divided into six subsections, each covering specific aspects of the project. Thank you for following along!

