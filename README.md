
# [React-Native](#reactnative)
# [Android Java](#androidjava)
# [Objective-C](#objectivec)
# [Swift](#swift)



&nbsp;
&nbsp;
&nbsp;


# Objective-C Bookmark <a id='objectivec'></a>

Modern Objective-C

https://docs.huihoo.com/apple/wwdc/2012/session_405__modern_objectivec.pdf

&nbsp;
&nbsp;
&nbsp;


# Code Example


&nbsp;
&nbsp;
&nbsp;


## GPS <a id='objectivec_gps'></a>

### 1. Get permission state

```objective-c
CLAuthorizationStatus status = [CLLocationManager authorizationStatus];

switch (status) {
    case kCLAuthorizationStatusNotDetermined:
        //The user hasn't yet chosen whether your app can use location services or not.

        break;

    case kCLAuthorizationStatusAuthorizedAlways:
        //The user has let your app use location services all the time, even if the app is in the background.

        break;

    case kCLAuthorizationStatusAuthorizedWhenInUse:
        //The user has let your app use location services only when the app is in the foreground.

        break;

    case kCLAuthorizationStatusRestricted:
        //The user can't choose whether or not your app can use location services or not, this could be due to parental controls for example.
        break;

    case kCLAuthorizationStatusDenied:
        //The user has chosen to not let your app use location services.

        break;

    default:
        break;
}
```


&nbsp;
&nbsp;
&nbsp;


## Asyncronize_Syncronize <a id='objectivec_syncronize'></a>

## 1. Asyncronized

### 1.1 Concurrent dispatch queue

```objective-c
dispatch_async(dispatch_queue_create("Foo", DISPATCH_QUEUE_CONCURRENT), ^{
});
```

`OUT-OF-ORDER`

### 1.2 Serial dispatch queue (stack queue)

```objective-c
dispatch_async(dispatch_queue_create("Foo", NULL), ^{
});
```

`IN-OF-ORDER`

### 1.3 Execute from main thread

```objective-c
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    //Perform expensive tasks
    //...

    //Now before updating the UI, ensure we are back on the main thread
    dispatch_async(dispatch_get_main_queue(), ^{
        label.text = //....
    });
}
```

## 2. Syncronized

### Asleep for other thread

```objective-c
dispatch_group_t preapreWaitingGroup = dispatch_group_create();
```

```objective-c
dispatch_group_enter(preapreWaitingGroup);
[self doAsynchronousTaskWithComplete:^(id someResults, NSError *error) { 
    // Notify that this task has been completed.
    dispatch_group_leave(preapreWaitingGroup);  
}]
```

```objective-c
dispatch_group_notify(preapreWaitingGroup, dispatch_get_main_queue(), ^{
    // This block will be executed once all above threads completed and call dispatch_group_leave
    NSLog(@"Prepare completed. I'm readyyyy");
});
```



&nbsp;
&nbsp;
&nbsp;


## UserNotifications

### 1. How to use

!!! It was shown when app is background

1. Add to header

```objective-c
#import <UserNotifications/UserNotifications.h>
```

2. Registry when app is started

```objective-c
UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
 [center requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert)
            completionHandler:^(BOOL granted, NSError * _Nullable error) {
             if (!error) {
              NSLog(@"requestAuthorizationWithOptions ok");
             }
            }]; 
```

3. Run UserNotifications

```objective-c
if( [UIApplication sharedApplication].applicationState != UIApplicationStateActive )
 {
  UNMutableNotificationContent *content = [[UNMutableNotificationContent alloc] init];
  content.title = [NSString localizedUserNotificationStringForKey:@"TestPushKit" arguments:nil];
  content.body = [NSString localizedUserNotificationStringForKey:@"New Call from 01012345678"
                             arguments:nil];
  content.sound = [UNNotificationSound defaultSound];
  
  // If you want change badge
  //content.badge = @([[UIApplication sharedApplication] applicationIconBadgeNumber] + 1); 

  UNNotificationRequest *request = [UNNotificationRequest requestWithIdentifier:@"New Call"
                                     content:content trigger:nil]; 

  UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
  [center addNotificationRequest:request withCompletionHandler:^(NSError * _Nullable error) {
   if (!error) {
    NSLog(@"addNotificationRequest ok");
   }
  }];
 }
 else
 {
  NSLog(@"app is active");
 }
```



&nbsp;
&nbsp;
&nbsp;


## TTS

### 1. Speak

```objective-c
#import <AVFoundation/AVFoundation.h>
```

`first, you should to include avfoundation header file`

```objective-c
AVSpeechSynthesizer *synthesizer = [[AVSpeechSynthesizer alloc]init];
AVSpeechUtterance *utterance = [AVSpeechUtterance speechUtteranceWithString:@"Some text"];
[utterance setRate:0.2f];
[synthesizer speakUtterance:utterance];
```

### 2. Pause, Stop Interface

```objective-c
- (BOOL)pauseSpeakingAtBoundary:(AVSpeechBoundary)boundary;
- (BOOL)stopSpeakingAtBoundary:(AVSpeechBoundary)boundary;
```



&nbsp;
&nbsp;
&nbsp;


## File

### 1. Write file to specify path

```objective-c
NSString *str = @"Hello World";
NSString *dirPath = @"/Users/username/Desktop";
NSString *filePath = [dirPath stringByAppendingPathComponent:@"helloworld.txt"];

__autoreleasing NSError *error;
BOOL ret = [str writeToFile:filePath atomically:YES encoding:NSUTF8StringEncoding error:&error];
```

### 2. Read contents of specify file

```objective-c
NSString *str = @"Hello World";
NSString *dirPath = @"/Users/username/Desktop";
NSString *filePath = [dirPath stringByAppendingPathComponent:@"helloworld.txt"];

__autoreleasing NSError *error;
    NSString *readContents = [NSString stringWithContentsOfFile:filePath encoding:NSUTF8StringEncoding error:&error];
```

## TryCatchFinally

```ObjectiveC
NSString *test = @"test";
 unichar a;
 int index = 5;
    
 @try {
    a = [test characterAtIndex:index];
 }
 @catch (NSException *exception) {
    NSLog(@"%@", exception.reason);
    NSLog(@"Char at index %d cannot be found", index);
    NSLog(@"Max index is: %lu", [test length] - 1);
 }
 @finally {
    NSLog(@"Finally condition");
 }
```


&nbsp;
&nbsp;
&nbsp;


## Variables

### 1. Get selfs

```objective-c
// Static
+ (NSArray *)validSuits {
    return @[@"♠︎", @"♣︎", @"♥︎", @"♦︎"];
}

- (void) var {
  NSArray *suites = [[self class] validSuits];
}
```



&nbsp;
&nbsp;
&nbsp;


## Timer

### 1. Interval

```objective-c
NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:5.0 target:self selector:@selector(doSomething) userInfo:nil repeats:NO];
```

### 2. Invalidate

```objective-c
[timer invalidate];
```

### 3. Fire

```objective-c
[timer fire];
```

### 4. Call method when timer interval is ended

```objective-c
NSTimer* timer = [NSTimer scheduledTimerWithTimeInterval:1.0
                                                  target:self
                                                selector:@selector(iGotCall:)
                                                userInfo:@"i am iOS guy" repeats:YES];
```

### 5. Scheduler

```objective-c
NSTimer.scheduledTimerWithTimeInterval(3.0, target: self, selector: Selector(self.timerMethod()), userInfo: nil, repeats: false)
```



&nbsp;
&nbsp;
&nbsp;


## Random

### 1. Get random number

```objective-c
uint32_t randomInteger = arc4random_uniform(5);
```

```objective-c
uint32_t randomIntegerWithinRange = arc4random_uniform(10) + 3;
```



&nbsp;
&nbsp;
&nbsp;


## Pointer

### 1. Swap memory address

```objective-c
int a = 10, b =20;
SwapClass *swap = [[SwapClass alloc]init];
NSLog(@"Before calling swap: a=%d,b=%d",a,b);
[swap num:&a andNum2:&b];
NSLog(@"After calling swap: a=%d,b=%d",a,b);
```



&nbsp;
&nbsp;
&nbsp;


## Operator

### 1. []

Call method

a->method(); => [a method];

obj->method(argument); -> [obj method:argument];

```objective-c
[object method];
[object methodWithInput:input];

output = [object methodWithOutput];

[NSString stringWithFormat:[prefs format]];
```

### 2. id

Promotion

void pointer

```objective-c
id myObject = [NSString string];
```

### 3. : 

[] Arguments

```objective-c
[myData writeToFile:@"/tmp/log.txt" atomically:NO];
```

### 4. .

```objective-c
photo.caption = @"Hello World";
```

### 5. @

Type hint -> Variables


### 6.+,-

\+ : Class Method, - : Instance Method



&nbsp;
&nbsp;
&nbsp;


## NSURLRequest

### 1. Async

```objective-c
// Create the request instance.
NSURLRequest *request = [NSURLRequest requestWithURL:[NSURL URLWithString:@"http://google.com"]];
 
// Create url connection and fire request
NSURLConnection *conn = [[NSURLConnection alloc] initWithRequest:request delegate:self];
```



&nbsp;
&nbsp;
&nbsp;


## NSString

### 1. String Format

```objective-c
NSString * format = [NSString stringWithFormat:@"%@s World", @"Hello"];
```

`String`

```objective-c
int k = 0x8f << 2;
id format = [NSString stringWithFormat:@"%d", k];
NSLog (fm);
```

`Integer`

### 2. Equality

```objective-c
if ( [@"Hello" isEqualToString:@"Hello"] ) {
    NSLog(@"World");
} else {
    NSLog(@"Hello");
}
```

### 3. Substring

```objective-c
NSString * strStartTime = @"20150102030405";
NSString * strYear = [strStartTime substringWithRange:NSMakeRange(0, 4)];
```

### 4. Use utf-8 string

```objective-c
[NSString stringWithUTF8String:"utf-8 string"];
```

### 5. startWith

```objective-c
if( [strTest hasPrefix:@"tel:"] )
{
} 
```

### 6. Contains

```objective-c
if( [strNumber containsString:@"1234"] )
{
}
```

### 7. upperString

```objective-c
[myString uppercaseString]
```

### 8. lowerString

```objective-c
[myString lowercaseString]
```

### 9. upperOnlyFirst

```objective-c
[myString capitalizedString]
```

### 10. Reverse

```objective-c
NSMutableString *reversedString = [NSMutableString string];
NSInteger charIndex = [myString length];
while (charIndex > 0) {
    charIndex--;
    NSRange subStrRange = NSMakeRange(charIndex, 1);
    [reversedString appendString:[myString substringWithRange:subStrRange]];
}
```

### 11. Decode

```objective-c
NSString *string = [[NSString alloc] initWithData:utf8Data
                                         encoding:NSUTF8StringEncoding];
```

### 12. Encode

```objective-c
NSString *string = [[NSString alloc] initWithData:utf8Data
                                         encoding:NSUTF8StringEncoding];
```



&nbsp;
&nbsp;
&nbsp;


## NSMutableArray

### 1. Remove items in range

```objective-c
[_clsList removeObjectsInRange:NSMakeRange(fromIndex, toIndex)];
```



&nbsp;
&nbsp;
&nbsp;


## NSDictionary

### 1. Initialize

```objective-c
NSDictionary *dict = [[NSDictionary alloc] initWithObjectsAndKeys:@"value1", @"key1", @"value2", @"key2", nil];
```

### 2. Array key, value

```objective-c
NSArray *keys = [NSArray arrayWithObjects:@"key1", @"key2", nil];
NSArray *objects = [NSArray arrayWithObjects:@"value1", @"value2", nil];
NSDictionary *dictionary = [NSDictionary dictionaryWithObjects:objects 
                                                       forKeys:keys];
```

### 3. Literal

```objective-c
NSDictionary *dict = @{@"key": @"value", @"nextKey": @"nextValue"};
```

### 4. Check has key

```objective-c
NSDictionary *dict = [NSDictionary dictionaryWithObjectsAndKeys:@"name1", @"Sam",@"name2", @"Sanju",nil];

if (dict[@"name1"] != nil) {
} else {  
}
```

### 5. With plist

```objective-c
NSString *pathToPlist = [[NSBundle mainBundle] pathForResource:@"plistName" 
    ofType:@"plist"];
NSDictionary *plistDict = [[NSDictionary alloc] initWithContentsOfFile:pathToPlist];
```

### 6. Get object

```objective-c
Car * lamborghini = [cars objectForKey:@"Lamborghini"];
```

```objective-c
Car * lamborghini = cars[@"Lamborghini"];
```



&nbsp;
&nbsp;
&nbsp;


## NSArray

### 1. Initialize

```objective-c
NSArray *array = [NSArray arrayWithObject:@"1" @"2" @"3"];
```

```objective-c
NSArray *array = @[@"1", @"2", @"3"];
```

### 2. Compare

```objective-c
NSArray *yourWords = @[@"Objective-C", @"is", @"just", @"awesome"];
NSString *sentence = [yourWords componentsJoinedByString:@" "];
```



&nbsp;
&nbsp;
&nbsp;


## Integer

### 1. Format

```objective-c
NSNumberFormatter * numFormatter = [[NSNumberFormatter alloc] init];
[numFormatter setNumberStyle:NSNumberFormatterDecimalStyle];
int value = 1000;
NSString * price = [NSString stringWithFormat:@"%@", [numFormatter stringFromNumber:[NSNumber numberWithInt:value]]];
```



&nbsp;
&nbsp;
&nbsp;


## Instance

### 1. No parameter

```objective-c
MyObject *foo = [[MyObject alloc] init];
```

### 2. With parameter

```objective-c
MyObject *foo = [[MyObject alloc] initWithString:myString];
```

### 3. Initialize function

```objective-c
- (id)init {
    self = [super init];
    if (self) {
        // perform initialization of object here
    }
    return self;
}
```



&nbsp;
&nbsp;
&nbsp;


## Function

### 1. Open settings

```objective-c
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:UIApplicationOpenSettingsURLString]];
```

### 2. Open browser

```objective-c
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"http://www.stackoverflow.com"]];
```

### 3. Share contents

```objective-c
NSArray* sharedObjects=[NSArray arrayWithObjects:@"sharecontent",  nil];
UIActivityViewController *activityViewController = [[UIActivityViewController alloc] initWithActivityItems:sharedObjects applicationActivities:nil];
activityViewController.popoverPresentationController.sourceView = self.view;
[self presentViewController:activityViewController animated:YES completion:nil];
```

### 4. Open email

* Deprecated *
```objective-c
NSURL *url = [NSURL URLWithString:@"mailto://azimov@demo.com"];
if ([[UIApplication sharedApplication] canOpenURL:url]) {
    [[UIApplication sharedApplication] openURL:url];
} else {
    NSLog(@"Cannot open URL");
}
```

```objective-c
NSURL *url = [NSURL URLWithString:@"mailto://azimov@demo.com"];
if ([[UIApplication sharedApplication] canOpenURL:url]) {
    NSString * url = @"mailto://azimov@demo.com";
    [[UIApplication sharedApplication] openURL:url options:@{} completionHandler:^(BOOL bSuccess) {

    }];
} else {
    NSLog(@"Cannot open URL");
}
```

### 5. Hide status bar

```objective-c
[[UIApplication sharedApplication] setStatusBarHidden:YES withAnimation:UIStatusBarAnimationFade]
```



&nbsp;
&nbsp;
&nbsp;


## Device

### 1. Get version

```objective-c
NSString *version = [[UIDevice currentDevice] systemVersion]
```

### 2. Compare version

```objective-c
NSString *version = @"3.1.3"; 
NSString *currentVersion = @"3.1.1";
NSComparisonResult result = [currentVersion compare:version options:NSNumericSearch];
switch(result){
  case: NSOrderedAscending:
        //less than the current version
  break;
  case: NSOrderedDescending:
  case: NSOrderedSame:
       // equal or greater than the current version
  break;
}
```

### 3. Generate UUID

```objective-c
+ (NSString *)randomUUID {
    if(NSClassFromString(@"NSUUID")) { // only available in iOS >= 6.0
        return [[NSUUID UUID] UUIDString];
    }
    CFUUIDRef uuidRef = CFUUIDCreate(kCFAllocatorDefault);
    CFStringRef cfuuid = CFUUIDCreateString(kCFAllocatorDefault, uuidRef);
    CFRelease(uuidRef);
    NSString *uuid = [((__bridge NSString *) cfuuid) copy];
    CFRelease(cfuuid);
    return uuid;
}
```

### 4. Get identifier of provider vendor

```objective-c
NSString *UDIDString = [[[UIDevice currentDevice] identifierForVendor] UUIDString];
```



&nbsp;
&nbsp;
&nbsp;


## DateTime

### 1. Get 90 days ago date

```objective-c
NSDate *currentDate = [NSDate date];
    
NSDate *currentDate = [NSDate date];
    
NSLog(@"Base          : %@", currentDate);

NSDateComponents *comps = [[[NSDateComponents alloc] init]
                           autorelease];

[comps setDay:-90];

NSCalendar *calendar;
calendar = [[[NSCalendar alloc]
            initWithCalendarIdentifier:NSGregorianCalendar]
            autorelease];

NSDate *date;
date = [calendar dateByAddingComponents:comps
                                 toDate:currentDate
                                options:0];

NSLog(@"Before 90days : %@", date);
```

### 2. Get current date

```objective-c
NSDate *date = [NSDate date];
NSLog(@"%@", date);
```

### 3. Convert NSDate to NSString pointer

```objective-c
NSDate * clsDate = [NSDate date];

NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat:@"yyyy-MM-dd"];

//Optionally for time zone conversions
[formatter setTimeZone:[NSTimeZone timeZoneWithName:@"..."]];

NSString *stringFromDate = [formatter stringFromDate:clsDate];
```



&nbsp;
&nbsp;
&nbsp;


## Constructor

### 1. Basic

```C
int function (int i) {
  return square_root(i);
}
```

```objective-c
- (int)method:(int)i {
  return [self square_root:i];
}
```

### 2. Multiple Arguments

```objective-c
- (void)changeColorToRed:(float)red green:(float)green blue:(float)blue {
    //... Implementation ...
}

//Called like so:
[myColor changeColorToRed:5.0 green:2.0 blue:6.0];
```



&nbsp;
&nbsp;
&nbsp;


## Clipboard

### 1. Copy

```objective-c
[[UIPasteboard generalPasteboard] setString:@"안녕하세요"];
```



&nbsp;
&nbsp;
&nbsp;


## Bundle

### 1. Get file list in main bundle

```objective-c
NSString *path = [[NSBundle mainBundle] bundlePath];
NSArray *list = [[NSFileManager defaultManager] contentsOfDirectoryAtPath:path error:nil];
NSLog(@"%@", list);
```

### 2. Get file list in main bundle with specify extension

```objective-c
NSString *path = [[NSBundle mainBundle] bundlePath];
NSArray *list = [[NSFileManager defaultManager] contentsOfDirectoryAtPath:path error:nil];
NSArray *onlyJPG = [list filteredArrayUsingPredicate:[NSPredicate predicateWithFormat:@"SELF ENDSWITH '.jpg'"]];
NSLog(@"%@", onlyJPG);
```

### 3. Get documents file list

```objective-c
NSString *path = [NSString stringWithFormat:@"%@/Documents/", NSHomeDirectory()];
NSArray *list = [[NSFileManager defaultManager] contentsOfDirectoryAtPath:path error:nil];
NSLog(@"%@", list);
```

### 4. Get file list in sub directory

```objective-c
- (NSString *)findSubDirectory {
    NSFileManager *fileManager = [NSFileManager defaultManager];
    NSString *path = [[[NSBundle mainBundle] bundlePath] stringByAppendingPathComponent:@"image"];;
    NSArray *list = [fileManager subpathsOfDirectoryAtPath:path error:nil];
    for (NSString *string in list) {
        BOOL isDir = NO;
        [fileManager fileExistsAtPath:[path stringByAppendingPathComponent:string] isDirectory:&isDir];
        if (isDir) {
            NSLog(@"%@", string);
        }
    }
    
    return nil;
}
```

### 5. Get main bundle path

```objective-c
NSString * bundlePath = [[NSBundle mainBundle] resourcePath];
```



&nbsp;
&nbsp;
&nbsp;


## Block

### 1. Recursive Blocks

```objective-c
- (void)alert:(NSString *)messsage title:(NSString *)titleMsg okMsg:(NSString *)okMsg cancelMsg:(NSString *)cancelMsg {
    
    UIAlertController * alert=   [UIAlertController
                                  alertControllerWithTitle:titleMsg
                                  message:@"Alert"
                                  preferredStyle:UIAlertControllerStyleAlert];
     
    __block void (^okAction)(UIAlertAction *) = [^void (UIAlertAction * action)
    {
        [alert dismissViewControllerAnimated:YES completion:nil];
    } copy];
    
    __block void (^cancelAction)(UIAlertAction *) = [^void (UIAlertAction * action)
    {
        [alert dismissViewControllerAnimated:YES completion:nil];
    } copy];
    
    
    UIAlertAction* ok = [UIAlertAction
                         actionWithTitle:okMsg
                         style:UIAlertActionStyleDefault
                         handler:okAction];
    
    UIAlertAction* cancel = [UIAlertAction
                             actionWithTitle:cancelMsg
                             style:UIAlertActionStyleDefault
                             handler:cancelAction];
     
    [alert addAction:ok];
    [alert addAction:cancel];
     
    [self presentViewController:alert animated:YES completion:nil];
}
```

### 2. Variable Readonly

```objective-c
void (^test)(void) = ^(void) {
    NSString * world = [[NSString alloc]init];
    world = [fString stringByAppendingFormat:@"World"];

    [self alert:@"Alert" title:@"test" okMsg:@"Hello" cancelMsg:world];
};

test();
```

`block can't to modify outer variables data`



&nbsp;
&nbsp;
&nbsp;


# Java Bookmark <a id='androidjava'></a>

## Installed Build Tools revision 31.0.0 is corrupted"

https://stackoverflow.com/questions/68387270/android-studio-error-installed-build-tools-revision-31-0-0-is-corrupted

## Android Google maps java.lang.NoClassDefFoundError: Failed resolution of: Lorg/apache/http/ProtocolVersion

https://stackoverflow.com/questions/50782806/android-google-maps-java-lang-noclassdeffounderror-failed-resolution-of-lorg-a

Put this in the Manifest \<application\> tag:
    
```xml
<uses-library android:name="org.apache.http.legacy" android:required="false"/>
```

## Build.VERSION.SDK_INT 19 / Build.VERSION_CODES.KITKAT

PNG background will be black color

### https://github.com/square/picasso/issues/978

just use Glide

### Duplicate class com.google.common.util.concurrent.ListenableFuture

Doesn't need to append 'implementation 'com.google.guava:listenablefuture:9999.0-empty-to-avoid-conflict-with-guava''

Type the below text to Commmand Line When you need debugging

```Command
adb shell am broadcast -a "androidx.work.diagnostics.REQUEST_DIAGNOSTICS" -p "<your_app_package_name>"
```
    
```xml
// Guava Dependencies
implementation ('com.google.guava:guava:24.1-jre') {
    exclude group: 'com.google.code.findbugs' // Not even Google can keep dependencies straight - Program type already present: javax.annotation.CheckForNull
    exclude group: 'com.google.common.annotations.*'
    exclude module: 'guava-jdk5'
    exclude module: 'com.google.common.annotations.GwtCompatible'
}

// OR
implementation ('com.google.guava:guava:24.1-jre') {
    exclude group: 'com.google.code.findbugs' // Not even Google can keep dependencies straight - Program type already present: javax.annotation.CheckForNull
    exclude group: 'com.google.common.annotations.*'
    exclude module: 'guava-jdk5'
    exclude module: 'com.google.common.annotations.GwtCompatible'
    exclude group: 'com.google.common.util.concurrent'
}

// Work Manager Runtime
implementation ('androidx.work:work-runtime:2.4.0') {
    exclude group: 'com.google.guava', module: 'listenablefuture'
    exclude module: 'com.google.j2objc:j2objc-annotations'
}
```


&nbsp;
&nbsp;
&nbsp;



# Swift Bookmark <a id='swift'></a>


# Swift

#### dyld[...]: Symbol not found: (_$s14KakaoSDKCommon11SessionTypeO3ApiyA2CmFWC) Referenced from:

```bash
pod deintegrate

pod install
```

#### App called -statusBar or -statusBarWindow on UIApplication: this code must be changed as there's no longer a status bar or status bar window. Use the statusBarManager object on the window scene instead.

```Swift
UIApplication.shared.value(forKey: "statusBar") as? UIView
```

to

```Swift
let statusBarFrame = UIApplication.shared.keyWindow?.windowScene?.statusBarManager?.statusBarFrame
let statusBar = UIView(frame: statusBarFrame)
view.addSubView(statusBar)
```

#### Software caused connection abort Code 53

https://www.gitmemory.com/issue/Alamofire/Alamofire/2674/543824622

#### Alamofire Background Network Connection Error

```Swift

func sdelay(_ delay:Double, closure:@escaping ()->()) {
    let when = DispatchTime.now() + delay
    DispatchQueue.main.asyncAfter(deadline: when, execute: closure)
}

sdelay(0.4) {
  Alamofire.request ... {
    request in

    DispatchQueue.main.async {
    }
  }
}
```







# React-Native Bookmark <a id='reactnative'></a>

## Tutorial

###### [README](#2.0)

###### [Active Debug Menu without Device Shake](#2.1)

###### [Run in Production](#2.2)

###### [Make Bundle](#2.3)

###### [Commands](#2.4)

## Issues

#### Javascript

#### Android

###### [Keyboard auto hide / Keyboard closes immediately once opened in TextInput](#1.2)   

###### [BUG! exception in phase 'semantic analysis' in source unit '_BuildScript_' Unsupported class file major version 61](#1.3)   

###### ['RNTimePicker' could not be found](#1.4)   

###### [Unresolved reference `resolveView` (Vision-Camera)](#1.5)  

###### [Module was compiled with an incompatible version of Kotlin. The binary version of its metadata is 1.6.0, expected version is 1.4.0.](#1.6)  

###### [Unable to make field private final java.lang.String java.io.File.path accessible: module java.base does not "opens java.io" to unnamed module](#1.7)  

###### [CMake Error: The following variables are used in this project, but they are set to NOTFOUND. FBJNI_LIB, FOLLY_JSON_LIB, JSI_LIB](#1.8)   

###### [Could not find me.relex:photodraweeview:1.1.3](#1.9)   

###### [Failed to construct transformer: Error: error:0308010C:digital envelope routines::unsupported React Native](#1.10)   

###### [Failed to notify project evaluation listener > org.gradle.api.file.ProjectLayout.fileProperty](#1.11)   

###### [React Native : Error: Duplicate resources - Android](#1.12)   

###### [Attempt to invoke virtual method 'android.graphics.drawable.Drawable android.graphics.drawable.Drawable$ConstantState.newDrawable(android.content.res.Resources)' on a null object reference](#1.13)   

###### [Native module RNC_AsyncSQLiteDBStorage tried to override AsyncStorageModule. Chekc the getPackages() method in MainApplication.java](#1.14)   

###### [Native module ~~ tried to override ~~](#1.15)   

###### [annotation does not exist](#1.16)   

#### IOS

###### undefined symbol: _swift_stdlib_isstackallocationsafe

###### [GDTCORPlatform.m:87:43 A function declaration without a prototype is deprecated in all versions of C](#3.2)   

###### [Type 'ChartDataSet' does not conform to protocol 'RangeReplaceableCollection'](#3.3)      

###### [“React/RCTEventDispatcherProtocol.h“ file not found](#3.4)      

###### [Error for only archive](#3.5)      

###### [cocoapods could not find compatible versions for pod reactcommon/callinvoker](#3.6)      

###### [Yoga.cpp:2285:9 Use of bitwise '|' with boolean operands](#3.7)      

###### ['React/RCTBridgeModule.h' file not found](#3.8)      

###### [IOS 14 Firebase Crash](#3.9)      

###### [database is locked Possibly there are two concurrent builds running in the same filesystem location.](#3.10)      

###### [duplicate symbols for architecture arm64](#3.11)      

###### [Couldn't create workspace arena folder '~': Unable to write to info file '~'](#3.12)      

###### [SIGABRT RCTModalHostViewController](#3.13)      

###### [XCode 14 Image not showing](#3.14)       

###### [react-native-modal-datetime-picker](#3.15)       

###### [Stored properties cannot be marked potentially unavailable with '@available'](#3.16)   




&nbsp;
&nbsp;
&nbsp;



# Sentry

Cannot remove sentry fucking shit




&nbsp;
&nbsp;
&nbsp;



# Javascript <a id='0.1'></a>

### Support for the experimental syntax 'decorators-legacy' isn't currently enabled
---

```bash
yarn add @babel/plugin-proposal-decorators
```

append to .babelrc or babel.config.js

```javascript
{
    "plugins": [
        [
           require(‘@babel/plugin-proposal-decorators’).default,
           {
              legacy: true
           }
        ],
    ]
}
```

# Android <a id='1.1'></a>

### Keyboard auto hide / Keyboard closes immediately once opened in TextInput <a id='1.2'></a>

```xml
android:windowSoftInputMode="stateAlwaysHidden|adjustPan"
```

### BUG! exception in phase 'semantic analysis' in source unit '_BuildScript_' Unsupported class file major version 61 <a id='1.3'></a>

```bash
distributionUrl => gradle-7.6-all.zip
```

### 'RNTimePicker' could not be found <a id='1.4'></a>
----

remove fucking `react-native-modal-datetime-picker` package

### Unresolved reference `resolveView` (Vision-Camera) <a id='1.5'></a>
----

findCameraView

```java
private fun findCameraView(viewId: Int): CameraView {
    Log.d(TAG, "Finding view $viewId...")
    val ctx = reactApplicationContext
    var view: CameraView? = null

    try {
      var manager = UIManagerHelper.getUIManager(ctx, viewId)
      view = manager!!::class.members.firstOrNull{ it.name == "resolveView" }?.call(viewId) as CameraView?
    } catch(e: Exception) {
      //
    }

    if (view == null) {
      try {
        view = ctx?.currentActivity?.findViewById<CameraView>(viewId)
      } catch(e: Exception) {
        //
      }
    }

    Log.d(TAG,  if (view != null) "Found view $viewId!" else "Couldn't find view $viewId!")
    return view ?: throw ViewNotFoundError(viewId)
}
```

findCameraViewById

```java
fun findCameraViewById(viewId: Int): CameraView {
    Log.d(TAG, "Finding view $viewId...")
    val ctx = mContext?.get()
    var view: CameraView? = null

    try {
      var manager = UIManagerHelper.getUIManager(ctx, viewId)
      view = manager!!::class.members.firstOrNull{ it.name == "resolveView" }?.call(viewId) as CameraView?
    } catch(e: Exception) {
      //
    }

    if (view == null) {
      try {
        view = ctx?.currentActivity?.findViewById<CameraView>(viewId)
      } catch(e: Exception) {
        //
      }
    }

    Log.d(TAG,  if (view != null) "Found view $viewId!" else "Couldn't find view $viewId!")
    return view ?: throw ViewNotFoundError(viewId)
}
```

### Module was compiled with an incompatible version of Kotlin. The binary version of its metadata is 1.6.0, expected version is 1.4.0. <a id='1.6'></a>
----

Update your kotlin version 1.6.0+
```text
classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.6.0+"
```

### Unable to make field private final java.lang.String java.io.File.path accessible: module java.base does not "opens java.io" to unnamed module <a id='1.7'></a>
----

gradle-wrapper.properties

```bash
org.gradle.jvmargs=--add-opens java.base/java.io=ALL-UNNAMED
```

### CMake Error: The following variables are used in this project, but they are set to NOTFOUND. FBJNI_LIB, FOLLY_JSON_LIB, JSI_LIB <a id='1.8'></a>
----

```bash
rm -rf node_modules
yarn install
cd android
./gradlew clean
./gradlew assembleDebug
```

### Could not find me.relex:photodraweeview:1.1.3 <a id='1.9'></a>
----

```bash
def REACT_NATIVE_VERSION = new File(['node', '--print',"JSON.parse(require('fs').readFileSync(require.resolve('react-native/package.json'), 'utf-8')).version"].execute(null, rootDir).text.trim())

configurations.all {
    resolutionStrategy {
        force "com.facebook.react:react-native:" + REACT_NATIVE_VERSION
        
        force 'me.relex:photodraweeview:2.1.0'
    }
}
```

### Failed to construct transformer: Error: error:0308010C:digital envelope routines::unsupported React Native <a id='1.10'></a>
----

```bash
set NODE_OPTIONS=--openssl-legacy-provider

nvm install 16.13.0

nvm alias default 16.13.0
```

### Failed to notify project evaluation listener > org.gradle.api.file.ProjectLayout.fileProperty <a id='1.11'></a>
----

Project Structure > Project > Change version to 4.2.2 

https://developer.android.com/studio/releases/gradle-plugin?hl=ko

### React Native : Error: Duplicate resources - Android <a id='1.12'></a>
----

```bash
rm -rf ./android/app/src/main/res/drawable-*

rm -rf ./android/app/src/main/res/raw
```

### Attempt to invoke virtual method 'android.graphics.drawable.Drawable android.graphics.drawable.Drawable$ConstantState.newDrawable(android.content.res.Resources)' on a null object reference <a id='1.13'></a>
----

https://github.com/react-native-maps/react-native-maps/issues/2924

```Text
I found a solution. Not sure if it's a long term one, but this will let you guys work until a permanent solution is found.
Just clear your cash: npm start -- --reset-cache
And restart the project.
It worked for me.
FYI, I didn't eject my project yet.
```

### Native module RNC_AsyncSQLiteDBStorage tried to override AsyncStorageModule. Chekc the getPackages() method in MainApplication.java <a id='1.14'></a>
----

Package.json

```Text
Async Storage has moved to new organization: https://github.com/react-native-async-storage/async-storage
```

```JSON
"@react-native-async-storage/async-storage": "^1.13.1",
"@react-native-community/async-storage": "1.11.0", <-- Remove it
```

### Native module ~~ tried to override ~~ <a id='1.15'></a>
----

https://stackoverflow.com/questions/41846452/how-to-set-canoverrideexistingmodule-true-in-react-native-for-android-apps/63438974#63438974

### annotation does not exist <a id='1.16'></a>
----

https://github.com/facebook/react-native-fbsdk/issues/567

npx jetify





&nbsp;
&nbsp;
&nbsp;





# IOS <a id='3.1'></a>

### GDTCORPlatform.m:87:43 A function declaration without a prototype is deprecated in all versions of C <a id='3.2'></a>

```c
GDTCORNetworkType GDTCORNetworkTypeMessage() {
```

to

```c
GDTCORNetworkType GDTCORNetworkTypeMessage(void) {
```

### Type 'ChartDataSet' does not conform to protocol 'RangeReplaceableCollection' <a id='3.3'></a>

Append to extension

```swift
public func replaceSubrange<C>(_ subrange: Swift.Range<Int>, with newElements: C) where C : Collection, ChartDataEntry == C.Element {
    entries.replaceSubrange(subrange, with: newElements)
    notifyDataSetChanged()
}
```

### “React/RCTEventDispatcherProtocol.h“ file not found <a id='3.4'></a>
----

```ObjectiveC
"React/RCTEventDispatcherProtocol.h" => "React/RCTEventDispatcher.h"
```

### Error for only archive <a id='3.5'></a>
----

Target - Build Settings - Architectures - Build Active Architecture Only [Release] YES

### cocoapods could not find compatible versions for pod reactcommon/callinvoker <a id='3.6'></a>
----

https://fantashit.com/rn-0-62-pod-repo-update-error/

```Text
I solved this issue (version 0.63) by changing the line in the Podfile from

pod 'ReactCommon/callinvoker', :path => "../node_modules/react-native/ReactCommon"

to

pod 'React-callinvoker', :path => "../node_modules/react-native/ReactCommon/callinvoker"
```

### Yoga.cpp:2285:9 Use of bitwise '|' with boolean operands <a id='3.7'></a>

```cpp
node->getLayout().hadOverflow() |
```

to

```cpp
node->getLayout().hadOverflow() ||
```

### 'React/RCTBridgeModule.h' file not found <a id='3.8'></a>
----

https://stackoverflow.com/questions/65696412/react-native-ios-build-error-react-rctbridgemodule-h-file-not-found-with-react

```Text
Check that I have React in my pods (pod 'React', :path => '../node_modules/react-native/'). If not, add it.

Uninstall reinstall pods (pod deintegrate && pod clean && pod install in the ios folder, I believe the pod deintegrate command needs to be downloaded and isn't available by default)

Go to scheme -> edit scheme -> build, delete the React(missing) using the 'minus' button. 

Click on 'add' or a 'plus' button. Find React (should be in the Pods category) and add it. 

Finally, make sure all boxes are ticked for React, and place it at the top of the list.
```

### IOS 14 Firebase Crash <a id='3.9'></a>
----

https://github.com/invertase/react-native-firebase/issues/3944

@heletrix thanks for sharing. I dispatch code in main thread instead and it's working for me

````Objective-C
// node_modules/react-native-firebase/ios/RNFirebase/notifications/RNFirebaseNotifications.m

RCT_EXPORT_METHOD(complete:(NSString*)handlerKey fetchResult:(UIBackgroundFetchResult)fetchResult) {
    if (handlerKey != nil) {
        void (^fetchCompletionHandler)(UIBackgroundFetchResult) = fetchCompletionHandlers[handlerKey];
        dispatch_async(dispatch_get_main_queue(), ^{
            if (fetchCompletionHandler != nil) {
                fetchCompletionHandlers[handlerKey] = nil;
                fetchCompletionHandler(fetchResult);
            } else {
                void(^completionHandler)(void) = completionHandlers[handlerKey];
                if (completionHandler != nil) {
                    completionHandlers[handlerKey] = nil;
                    completionHandler();
                }
            }
        });
    }
}
````

### database is locked Possibly there are two concurrent builds running in the same filesystem location. <a id='3.10'></a>
----

Clean bulid folder

### duplicate symbols for architecture arm64 <a id='3.11'></a>
----

pod deintegrate

pod install

### Couldn't create workspace arena folder '~': Unable to write to info file '~'. <a id='3.12'></a>
----

Disk space is not enought

### SIGABRT RCTModalHostViewController <a id='3.13'></a>
----

```Text
SIGABRT: -[MTLDebugBuffer newTextureWithDescriptor:offset:bytesPerRow:] > -[MTLDebugBuffer newTextureWithDescriptor:offset:bytesPerRow:]:326: failed assertion `resourceOptions (0x0) must match backing buffer resource options (0x20).'
 > : failed assertion `%s'
```

check componentWillUnmount event

### XCode 14 Image not showing <a id='3.14'></a>
----

https://github.com/hsource/react-native/pull/2

### react-native-modal-datetime-picker <a id='3.15'></a>
----

https://github.com/mmazzarolo/react-native-modal-datetime-picker/pull/474

### Stored properties cannot be marked potentially unavailable with '@available' <a id='3.15'></a>

https://github.com/crossplatformkorea/react-native-kakao-login/issues/326

remove below code

```swift
@available(iOS 13.0, *)
public lazy var presentationContextProvider: Any? = DefaultPresentationContextProvider()
```

```swift
public var presentationContextProvider: Any?

public init() {
    if #available(iOS 13.0, *) { self.presentationContextProvider = DefaultPresentationContextProvider() } else { self.presentationContextProvider = nil }
    ...
}
```





&nbsp;
&nbsp;
&nbsp;







# M1, M2 (ARM)

### Change Architecture to x86/64 

```bash
arch -x86_64 /bin/zsh
```





&nbsp;
&nbsp;
&nbsp;





### Firebase 
----

```Text
Just realized that according to the docs, these functions only work when sending and receiving notifications via FCM, which is why you aren't seeing these work when sending them manually through APNS. This package will allow for leverage getInitialNotification and the other function: 
```

















# React_Native <a id='2.0'></a>

!do not use react-native.

# Active Debug Menu without Device Shake <a id='2.1'></a>

```bash
adb shell input keyevent KEYCODE_MENU
```

# Run in Production <a id='2.2'></a>

```bash
npx react-native run-ios --configuration Release --device

react-native run-android --variant=release
```

# Make Bundle <a id='2.3'></a>

## IOS

OLD
```bash
react-native bundle --entry-file='index.js' --bundle-output='./ios/main.jsbundle' --dev=false --platform='ios'
```

NEW
```bash
react-native bundle --entry-file ./index.js --platform ios --bundle-output ios/main.jsbundle --assets-dest ios
```

## Android 

OLD
```bash
react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res

```

NEW
```bash
react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/
```

# Commands <a id='2.4'></a>

Run in Platform

```bash
npx react-native run-ios
npx react-native run-android
```

Run with legacy openssl (openssl-legacy-provider)

```bash
set NODE_OPTIONS=--openssl-legacy-provider && react-native run-ios
set NODE_OPTIONS=--openssl-legacy-provider && react-native run-android
```

Run in Device
```bash
npx react-native run-ios --simulator=\"iPhone 6s\"
```
