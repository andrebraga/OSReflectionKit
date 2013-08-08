OSReflectionKit
===============

OSReflectionKit is a lightweight object reflection library for iOS and Mac OS X, that allows you to instantiate objects from a simple [NSDictionary](http://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSDictionary_Class/Reference/Reference.html) objects or JSON strings. For example, here's how easy it is to instantiate an object from a dictionary or even directly from a JSON string:

### Loading from a JSON string
```objective-c
NSString *jsonString = nil;

// ... Obtain the jsonString data
jsonString = @"{\"name\" : \"Champions\", \"imageURL\" : \"http://www.cruzeiro.com.br/imagem/imgs/escudo.png\"}"; // Sample
// ...

// Instantiate the Category object with the content from the JSON string
Category *category = [Category objectFromJSON:jsonString];
```

### Loading from a dictionary
```objective-c
NSDictionary *categoryDict = nil;

// ... Obtain the dictionary data
categoryDict = @{@"name" : @"Champions", @"imageURL" : @"http://www.cruzeiro.com.br/imagem/imgs/escudo.png"}; // Sample
// ...

// Instantiate the Category object with the content from the dictionary
Category *category = [Category objectFromDictionary:categoryDict];
```
### Core Data

OSReflectionKit also supports [NSManagedObject](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/CoreDataFramework/Classes/NSManagedObject_Class/Reference/NSManagedObject.html) objects.

If you want to try it out, you can download the project and open the OSReflectionKit+CoreDataExample project :)

## How To Install

- [CocoaPods](http://cocoapods.org) is the recommended way to add OSReflectionKit or OSReflectionKit+CoreData to your project.
- If you prefer, you can simply [download OSReflectionKit](https://github.com/iAOS/OSReflectionKit/zipball/master) and add the files from the OSReflectionKit folder to your project.

### Requirements

#### OSReflectionKit

- iOS 5.0 or greater.
- Mac OS X 10.7 or greater.
- ARC.

#### OSReflectionKit+CoreData

- `OSReflectionKit`
- iOS 5.0 or greater.
- ARC.

### Cocoapods

#### OSReflectionKit

1. Add a pod entry for OSReflectionKit to your Podfile `pod 'OSReflectionKit', '~> 0.5.0'`
2. Install the pod(s) by running `pod install`.
3. Include OSReflectionKit wherever you need it with `#import "NSObject+OSReflectionKit.h"`.

#### OSReflectionKit+CoreData

1. Add a pod entry for OSReflectionKit+CoreData to your Podfile `pod 'OSReflectionKit+CoreData', '~> 0.5.0'`
2. Install the pod(s) by running `pod install`.
3. Include OSReflectionKit+CoreData wherever you need it with `#import "NSManagedObject+OSReflectionKit.h"`.

Yep, that simple.

### Example Usage

[Download the project](https://github.com/iAOS/OSReflectionKit/zipball/master) and open the OSReflectionKitExample project and play a little bit with the code :)

There is also an example project for using `OSReflectionKit+CoreData`.

### Non-ARC Usage
- The library files are based on ARC, so if you want to use it in a non-ARC project, please add `-fobjc-arc` compiler flag to the library files.

To set a compiler flag in Xcode, go to your active target and select the "Build Phases" tab. Now select all OSReflectionKit source files, press Enter, insert `-fobjc-arc` and then "Done" to enable ARC for OSReflectionKit.
## Samples

### Custom Class

Supposing you have a simple class with the following class:

```objective-c
#import <Foundation/Foundation.h>

@interface Category : NSObject

@property (nonatomic, strong) NSNumber *categoryId;
@property (nonatomic, strong) NSString *name;
@property (nonatomic, strong) NSURL *imageURL;

@end
```
To enable object reflection for this class, all you have to is import the `NSObject+OSReflectionKit.h` file where you want to call the reflection methods.
You can also import the `NSObject+OSReflectionKit.h` file in the project `prefix.pch` file to make it available all over the project.

```objective-c
#import "NSObject+OSReflectionKit.h"

// ...

categoryDict = @{@"categoryId" : @(1),
                 @"name" : @"Champions",
                 @"imageURL" : @"http://www.cruzeiro.com.br/imagem/imgs/escudo.png"}; // Sample dictionary

// Instantiate the Category object with the content from the dictionary
Category *category = [Category objectFromDictionary:categoryDict];

NSLog(@"Category description: %@", [category fullDescription]);
```

The library will automatically match the dictionary keys to the property names of the class `Category`.
If you have different keys in the dictionary like below:

```objective-c
categoryDict = @{@"id" : @(1),
                 @"name" : @"Champions",
                 @"image" : @"http://www.cruzeiro.com.br/imagem/imgs/escudo.png"};
```

You can implement a custom mapping method in the `Category` class, translating each different key to the destination property:

```objective-c
#import "Category.h"

@implementation Category

+ (NSDictionary *)reflectionMapping
{
    return @{@"id":@"categoryId", @"image":@"imageURL,*"};
}

- (void)reflectionTranformsValue:(id)value forKey:(NSString *)propertyName
{
    if([propertyName isEqualToString:@"imageURL"])
    {
        NSString *imageURLString = value;
        if(imageURLString)
            self.imageURL = [NSURL URLWithString:imageURLString];
    }
}

@end
```

The reflectionMapping dictionary may include a custom class `@"customObject,<CustomClass>"` or a custom transformation by including an `*` in the mapping string, like for the imageURL property above.

## License

Copyright (c) 2013 iAOS Software. All rights reserved.
 
 Permission is hereby granted, free of charge, to any person obtaining a copy of
 this software and associated documentation files (the "Software"), to deal in
 the Software without restriction, including without limitation the rights to
 use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
 of the Software, and to permit persons to whom the Software is furnished to do
 so, subject to the following conditions:
 
 The above copyright notice and this permission notice shall be included in all
 copies or substantial portions of the Software.
 
 If you happen to meet one of the copyright holders in a bar you are obligated
 to buy them one pint of beer.
 
 THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
 SOFTWARE.
