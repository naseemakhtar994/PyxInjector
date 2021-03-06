## PyxInjector
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-PyxInjector-brightgreen.svg?style=flat)](https://android-arsenal.com/details/1/6362) [![](https://jitpack.io/v/WindSekirun/PyxInjector.svg)](https://jitpack.io/#WindSekirun/PyxInjector) [![Kotlin](https://img.shields.io/badge/kotlin-1.1.4-blue.svg)](http://kotlinlang.org) [![GitHub license](https://img.shields.io/badge/license-Apache%20License%202.0-blue.svg?style=flat)](http://www.apache.org/licenses/LICENSE-2.0) 


**Annotation Field Injector Library**

Pyx is abbreviation of Pyxis, small and faint constellation in the southern sky.

PyxInjector help you to inject field, methods by various annotation field

릴리즈 소개 글은 [개인 블로그 PyxisPub 에서 보실 수 있습니다.](https://blog.uzuki.live/pyxinjector-dependency-injections/)

### Usages

*rootProject/build.gradle*
```	
allprojects {
    repositories {
	    maven { url 'https://jitpack.io' }
    }
}
```

*app/build.gradle*
```
dependencies {
    implementation 'com.github.WindSekirun:PyxInjector:1.0.0'
}
```

### Annotation Fields

#### *@BindView*
Precondition: **Activity / Fragment will inherit *InjectActivity*, *InjectFragment*, *InjectSupportFragment* or your custom object**

Annotation Field with @BindView with Optional View ID for PyxInjector to find and cast the corresponding view.

```Java
private @BindView TextView mTxtName; // 1)
private @BindView TextView txtName2; // 2)
private @BindView(resource = R.id.txtName3) TextView txtName3; // 3)
```

It support three mode of Injection

1. resource id != field name with Config - PREFIX_M : You don't need to write View ID for this view
2. resource id == field name : You don't need to write View ID for this view
3. Explicit statement : You can write View ID for this view

#### *@Extra*
Precondition: **Activity will inherit *InjectActivity* or your custom object**

Annotation Field with @Extra with Optional extra key to find and cast the corresponding intent extras.

```Java
private @Extra String age = ""; // 1)
private @Extra("name") String name = ""; // 2)
```

It support two mode of Injecton

1. extra name == field name : You don't need to write extra key for this intent extras
2. Explicit statement: You can write extra key for this intent extras

#### *@Argument*
Precondition: **Fragment will inherit *InjectFragment*, *InjectSupportFragment* or your custom object**

Annotation Field with @Argument with Optional extra key to find and cast the corrsponding fragment arguments.

```Java
private @Argument String age = ""; // 1)
private @Argument("name") String name = ""; // 2)
```

It support two mode of Injecton

1. extra name == field name : You don't need to write extra key for this fragment arguments.
2. Explicit statement: You can write extra key for this fragment arguments.

#### *@OnClicks* / *@OnClicks*
Precondition: **Activity / Fragment will inherit *InjectActivity*, *InjectFragment*, *InjectSupportFragment* or your custom object**

Annotation Field with @OnClick, @OnClicks with View ID to find and invoke methods

```Java
@OnClick(resource = R.id.btnDo)
private void clickDo() { // 1)
    Toast.makeText(getActivity(), "Clicked fragment", Toast.LENGTH_SHORT).show();
}

@OnClicks(resource = {R.id.btnDo, R.id.btnDo2})
private void clickDo(View v) { // 2)
    Intent intent = new Intent(this, SecondActivity.class);
    intent.putExtra("name", "John");
    intent.putExtra("age", "17");
    startActivity(intent);
}
```

It support two mode of Injection
1. methods without parameter : You don't need to declare any parameter
2. methods with View parameter

#### *@OnLongClick* / *@OnLongClicks*
Precondition: **Activity / Fragment will inherit *InjectActivity*, *InjectFragment*, *InjectSupportFragment* or your custom object**

Annotation Field with @OnClick, @OnClicks with View ID to find and invoke methods

```Java
@OnLongClick(resource = R.id.btnDo)
private void clickDo() { // 1)
    Toast.makeText(getActivity(), "Clicked fragment", Toast.LENGTH_SHORT).show();
}

@OnLongClicks(resource = {R.id.btnDo, R.id.btnDo2}, defaultReturn = true)
private void clickDo(View v) { // 2)
    Intent intent = new Intent(this, SecondActivity.class);
    intent.putExtra("name", "John");
    intent.putExtra("age", "17");
    startActivity(intent);
}
```

It support three mode of Injection
1. methods without parameter : You don't need to declare any parameter
2. methods with View parameter
3. defaultReturn : true if the callback consumed the long click, false (default or ignore) otherwise.

### Config (Optional)
as 1.0.0 We support Config of PyxInjector.

Application class
```Java
Config config = new Config(BindViewPrefix.PREFIX_M);
PyxInjector.initializeApplication(config);
```

#### BindViewPrefix
* None - (DEFAULT) not apply rule
* PREFIX_M - apply implicit rule when field name is mTxtName and view id is txtName

### Custom Object
as 1.0.0, We support *InjectActivity*, *InjectFragment*, *InjectSupportFragment* to inject all activity / fragement.

if you need to inherit other class, insert this code in proper methods

#### Fragment

**Kotlin**
```Kotlin
protected val injector = PyxInjector()

override fun onViewCreated(view: View?, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        injector.execute(activity, this, getCreatedView());
}
```

**Java**
```Java
protected PyxInjector injector = new PyxInjector();

@Override
public void onViewCreated(@Nullable View view, @Nullable Bundle savedInstanceState) {
    super.onViewCreated(view, savedInstanceState);
    injector.execute(activity, this, getCreatedView());
}
```

#### Activity or Others

**Kotlin**
```Kotlin
protected val injector = PyxInjector()

override fun onContentChanged() {
    super.onContentChanged()
    injector.execute(this, this, findViewById(android.R.id.content))
}
```

**Java**
```Java
protected PyxInjector injector = new PyxInjector();

@Override
public void onContentChanged() {
    super.onContentChanged();
    injector.execute(this, this, getCreatedView());
}
```

### License 
```
Copyright 2017 WindSekirun (DongGil, Seo)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
