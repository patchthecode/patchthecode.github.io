---
layout: default
---

## Installation
___

### 1. Installing via CocoaPods

[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects. You can install it with the following command:

```bash
$ gem install cocoapods
```

> CocoaPods 1.1.0+ is required to build JTApplecalendar.

To integrate JTAppleCalendar into your Xcode project using CocoaPods, specify it in your `Podfile`:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '10.0'
use_frameworks!

target '<Your Target Name>' do
    pod 'JTAppleCalendar', '~> 6.0'
end
```

Then, run the following command:

```bash
$ pod install
```

**Completed!**

New to cocoapods? Did the steps above fail? Then read on.

If youre new to CocoaPods, simply do search on how to integrate cocoapods into your project. Trust me that 5-7 minuites of research will bring you much benefit. CocoaPods one of the top dependency manager for integrating 3rd party frameworks into your project. But in a nut-shell, here is how I did my installation with a sample project called **test**

1. Install cocoapods.
2. Create a new xcode project. Save the name as: **test**
3. Go to your console in the directory location where your project is located.
4. Type and run the command: **pod init**
5. This will create a file called: **Podfile** in that same location.
6. Edit that **Podfile** so that it looks like the following:


```bash
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'test' do
  use_frameworks!
  pod 'JTAppleCalendar', '~> 6.0'
end
```
	
Save, and head back to terminal and run: **pod install**
If all Went well, installation should be complete. Close the XCodeproject, and instead reopen it using the **workspace** file which generated when installation was completed. Done.

### 2. Installing via Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that builds your dependencies and provides you with binary frameworks.

You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

```bash
$ brew update
$ brew install carthage
```

To integrate JTAppleCalendar into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "patchthecode/JTAppleCalendar" ~> 6.0
```

Run `carthage update` to build the framework and drag the built `JTApplecalendar.framework` into your Xcode project.

### 3. Installing manually

Simply drag the source files into your project.