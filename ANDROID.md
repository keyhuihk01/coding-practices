# Coding Practices in Android Development

## Summary

#### [Gradle](#build-system)
#### [Project Structure]
#### [Kotlin]
#### [Activity and Fragment]
#### [Layout XML]
#### [Resources]
#### [Log]
#### [ProGuard]
#### [Unit Test]

----------

### Gradle configuration

**Prefer Maven dependency resolution to importing jar files.**

**Avoid Maven dynamic dependency resolution**
Avoid the use of dynamic dependency versions, such as `2.1.+` as this may result in different and unstable builds or subtle, untracked differences in behavior between builds. The use of static versions such as `2.1.1` helps create a more stable, predictable and repeatable development environment.

**Good**
```groovy
dependencies {
    implementation 'com.squareup.okhttp:okhttp3:3.8.0'
}
```

**Bad**
```groovy
dependencies {
    implementation 'com.squareup.okhttp:okhttp3:3.8.+'
}
```

### Project Structure

**MVVM**
|- api
|- core
	|- App (The Application class Here)
	|- Manager Class
|- data
|- screens
	|- home
		|- activity
		|- adapter
		|- fragment
		|- viewholder
		|- viewmodel
|- utils

### Kotlin

**Use Optional `?` and `?.let` when possible**
**Always don't use `!!`**

**Good**
```kotlin
model?.list?.let {
	this.list = it
}
```

**Bad**
```kotlin
this.list = model!!.list!!
```

**Constants**

**Good**
```kotlin
companion object {
	const val DEVICE_IOS = "ios"
	const val DEVICE_ANDROID = "android"
}
```

**Bad**
```kotlin
companion object {
	const val deviceIOS = "ios"
	const val androidDevice = "android"
}
```

**Functions**

Use `onClick` for Button Action

```kotlin
private fun onClickLoginButton() {}

btn_login.setOnClickListener {
	gotoLogin()
}
```

**Use of Rx**
In ViewModel
```kotlin
val sIsLoading = PublishSubject.create<Boolean>()
..
..
sIsLoading.onNext(true)
```

In Activity or Fragment
```kotlin
disposables.addAll(
	vm.sIsLoading
	.observeOn(AndroidSchedulers.mainThread())
	.subscribe({ toggleLoading(it) }, { logError(it) })
)
```

### Activity and Fragment

**Builder Pattern**
```kotlin
class Builder {
	private var isGameMode = false

	fun setIsGameMode(value: Boolean): Builder {
		this.isGameMode = value
		return this
	}

	fun build() = GameFragment().apply {
		arguments = Bundle().apply {
			putBoolean("IS_GAME_MODE", this@Builder.isGameMode)
		}
	}
}

// create
GameFragment.Builder().setIsGameMode(true).build()
```

### Layout XML

**Naming.** 
Follow the convention of prefixing the type, as in `type_foo_bar.xml`. 
Examples: `fragment_content_details.xml`, `view_primary_button.xml`, `activity_main.xml`.

**Android:id Naming**
RelativeLayout --> rl_container
LinearLayout   --> ll_container
FragmentLayout --> fl_container
TextView 	   --> tv_title
Button 	       --> btn_login
View           --> v_space
EditText       --> et_input
ImageView      --> iv_logo
ScrollView     --> sv_container
RecyclerView   --> rv_list

**Organizing layout XMLs.**
Always format it after eidt. Android Studio Shortcut `Option + Commnad + L` in MacOS.


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/ll_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    >

    <TextView
        android:id="@+id/tv_title"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/name"
        style="@style/FancyText"
        />

    <include layout="@layout/reusable_part" />

</LinearLayout>
```

### Resources

**strings.xml**

**Bad**
```xml
<string name="network_error">Network error</string>
<string name="call_failed">Call failed</string>
<string name="map_failed">Map loading failed</string>
```

**Good**
```xml
<string name="error_message_network">Network error</string>
<string name="error_message_call">Call failed</string>
<string name="error_message_map">Map loading failed</string>
```

```xml
<resources>
    <dimen name="horizontal_spacing_xs">6dp</dimen>
    <dimen name="horizontal_spacing_s">8dp</dimen>
    <dimen name="horizontal_spacing_m">10dp</dimen>
    <dimen name="horizontal_spacing_l">16dp</dimen>
    <dimen name="horizontal_spacing_xl">18dp</dimen>
    <dimen name="horizontal_spacing_xxl">24dp</dimen>

    <dimen name="vertical_spacing_xs">2dp</dimen>
    <dimen name="vertical_spacing_s">4dp</dimen>
    <dimen name="vertical_spacing_m">10dp</dimen>
    <dimen name="vertical_spacing_l">20dp</dimen>
    <dimen name="vertical_spacing_xl">28dp</dimen>
    <dimen name="vertical_spacing_xxl">40dp</dimen>

    <dimen name="text_size_xs">10sp</dimen>
    <dimen name="text_size_s">12sp</dimen>
    <dimen name="text_size_m">14sp</dimen>
    <dimen name="text_size_l">16sp</dimen>
    <dimen name="text_size_xl">20sp</dimen>
    <dimen name="text_size_xxl">22sp</dimen>
</resources>
```

### Log


### ProGuard


### Unit Test

