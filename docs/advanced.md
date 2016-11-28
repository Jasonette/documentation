# What is this page about?
You can probably build any app you can imagine with Jasonette alone.

However sometimes you may want to:

- **Extend Jasonette engine itself:** Want a JSON powered MapKit? Want a JSON powered video livestream view? Want a JSON powered ApplePay? Just write an extension! (No, really, please someone write an extension.)
- **Integrate Jasonette with existing code:** Already have your own app code? Plug in Jasonette to your existing app with just a few lines!

---

# Extension vs. Integration

Both extending and integrating involve writing native iOS code. But here's the difference:

  - **Extending Jasonette** means you're extending Jasonette itself. This means:
    - All your custom modules will be JSON powered, just like the rest of Jasonette.
    - You will need to follow a simple convention when writing the extension. 
    - You can share it with the community when you're done. If it's useful enough it may even be integrated into the main codebase.

  - **Integrating with Jasonette** means you already have existing code and are using Jasonette only partially. This means:
    - You can start using Jasonette in your existing app instantly, without re-writing anything. Just a few lines of code necessary.
    - You will be able to take advantage of all of Jasonette's power whenever you use the Jasonette view.
    - The only drawback is you can't control your own non-Jasonette code with JSON

**Extending Jasonette is the recommended way in most cases,** since it requires very little additional effort, but automatically comes with the power to control your own custom native modules via JSON.

---

# ★  Extension

Let's jump into writing extensions! There are two things you can extend: [Components](components.md) and [Actions](actions.md)

## 1. Extend UI Components
Jasonette comes with various [UI components](components.md) right out of the box, but you can easily build your own native component if you need to. Just follow the instructions below to write your component class, and you will instantly be able to summon your component within the [Jasonette layout system](layout.md) simply by describing in JSON.

### 1.1. How to write a UI Component class
All you need to do is write a class with a single builder method that returns a `UIView` subclass. Here's how:

---

####Step 1. Create a `JasonComponent` subclass

The example below demonstrates how to create a `map` component.

**Just remember the following rules:**

1. Create a class that inherits from `JasonComponent.h`.
2. The class name should follow this convention: `Jason(COMPONENT_NAME)Component`
3. Only the first character of the COMPONENT_NAME should be capitalized

So, to create a `map` component:

1. we create a subclass of `JasonComponent`
2. And name it `JasonMapComponent` (Notice the capitalization).
3. We also import `MapKit` since we will need to access the iOS MapKit API.

Here's the result:

    // JasonMapComponent.h
    #import "JasonComponent.h"
    #import <MapKit/MapKit.h>
    @interface JasonMapComponent : JasonComponent <MKMapViewDelegate>
    @end

---

####Step 2. Implement the builder method

You only need to implement a single class method:

**+ (UIView *)build: (UIView*)component withJSON: (NSDictionary *)json withOptions: (NSDictionary *)options**.

Basically it's a builder method that takes three arguments to create a UIView subclass. The arguments are:

  A. `component`: A UIView instance to fill in. Just assume that a UIView will be passed in, and repurpose it to your liking.
  
  B. `json`: A JSON snippet in NSDictionary format which includes all the information we need to construct this component.
  
  C. `options`: Useful in a few cases but you don't need to use this in most cases, so let's not worry about it here.

The result can be a UILabel, UIButton, or whatever, as long as it's a subclass of `UIView`.

Here's how it works:

When Jasonette is building a layout, it will call this `build:withJSON:withOptions` method, passing in a relevant UIView instance (`component`) as well as the JSON blueprint (`json`) from which you will build your own UIView subclass.

From here, all you need to do is:

  1. Check if the `component` is nil. If it's nil, create a new instance. If it's not, then we just reuse it.

  2. Construct your own custom UIView subclass instance based on the `json` object received, and return. You're done!

Here's what the most primitive version would look like:

    // JasonMapComponent.m
    @implementation JasonMapComponent
    + + (UIView *)build: (UIView*)component withJSON: (NSDictionary *)json withOptions: (NSDictionary *)options{
      MKMapView *mapView;
      if(component){
        mapView = (MKMapView*)component;
      } else {
        mapView = [[MKMapView alloc] init];
      }
      return mapView;
    }
    @end

This works since `MKMapView` is a subclass of `UIView`.

---

####Step 3. And that's it!

This is seriously all we need to do! Our map component is now 100% powered by JSON, just like rest of the [Jasonette components](components.md).

Here's an example of how we would summon our `map` component.

    {
      "type": "vertical",
      "components": [
        { "type": "label", "text": "This is a map" },
        { "type": "map" }
      ]
    }

So how does this work behind the scenes? Whenever Jasonette encounters a component description, 

1. It looks at its `type` first. Here we see the `{"type": "map"}`, so it's a `map` component.
2. Based on this, it looks for a class named `JasonMapComponent` (Remember the naming convention is: **Jason(COMPONENT_NAME_WITH_FIRST_LETTER_CAPITALIZED)Component**
3. When it finds the component class, it runs its `build:withOptions:` method to finally build the component.

Of course, this is a simplified version of how the actual `map` component works. In reality the `json` object would contain more information than just `{ "type": "map" }`. It may look something like this:

    {
      "type": "map",
      "region": {
        "coord": "40.7409395,-74.0083886",
        "width": "50"
      }
    }

All you need to do is take this, parse it, and customize the `mapView` object before returning, like the comment below.

    // JasonMapComponent.m
    @implementation JasonMapComponent
    + (UIView *)build: (UIView*)component withJSON: (NSDictionary *)json withOptions: (NSDictionary *)options{
      MKMapView *mapView;
      if(component){
        mapView = (MKMapView*)component;
      } else {
        mapView = [[MKMapView alloc] init];
      }
      
      /*
        Do some customization with the 'json' argument here.
      */
      
      return mapView;
    }
    @end

---

### 1.2. Tips on writing component classes
---
#### A. Keep it stateless and self contained

These component builder classes and methods are stateless by design. That's why the `build:withOptions:` method is **a class method** that simply takes a `NSDictionary` as input and returns a `UIView` as output.

---

#### B. Use class methods

Like mentioned above, Jasonette requires that all the component methods are implemented as class methods.

You may ask "But there's a delegate method I'm trying to use for handling events, and it's an instance method only. How do I deal with this?".

Well, in objective-c, classes are also objects, so you can in fact call them the same way you call an instance method.

**Just switch out the instance method with its class method version** (use `+` instead of `-` at the beginning of the method definition), like below:

    // JasonMapComponent.m
    @implementation JasonMapComponent
    + (UIView *)build: (UIView*)component withJSON: (NSDictionary *)json withOptions: (NSDictionary *)options{
      MKMapView *mapView;
      if(component){
        mapView = (MKMapView*)component;
      } else {
        mapView = [[MKMapView alloc] init];
      }
      
      /*
        Do some customization with the 'json' argument here
      */
    
      mapView.delegate = self;
    
      return mapView;
    }
    
    + (void)mapView:(MKMapView *)mapView didUpdateUserLocation:(MKUserLocation *)userLocation{
      /*
        Handle the event
      */
    }
    @end

Here we implement

**+ (void)mapView:(MKMapView *)mapView didUpdateUserLocation:(MKUserLocation *)userLocation**

to handle MapKit events.

---

#### C. Take advantage of the built-in stylize method

A lot of style attributes such as background, width, height, corner_radius, opacity, etc. are common across various types of components.

`JasonComponent` class has a built-in method that you can utilize to automatically apply these styles. It's called:

**+ (void) stylize: (NSDictionary *)json component: (UIView *)component**.

Just by calling this method once, you can take advantage of all the following style attributes right out of the box!

    {
      "type": "mycustomitem",
      "style": {
        "width": "...",
        "height": "...",
        "opacity": "...",
        "background": "...",
        "color": "...",
        "corner_radius": "..."
      }
    }

Here's an example code that creates a UIView, runs it through the built-in `stylize`, and then applies additional custom styles.


    + (UIView *)build: (UIView*)component withJSON: (NSDictionary *)json withOptions: (NSDictionary *)options{
      // Build some component here
      MKMapView *mapView;
      if(component){
        mapView = (MKMapView*)component;
      } else {
        mapView = [[MKMapView alloc] init];
      }
      
      /*
      customize the component here
      */
    
      // run it through the default stylize method
      [self stylize: json component: component];
    
      // Add your own custom styling logic here, on top of the default styling above
      if(style[@"rotate"] {
        // some custom styling logic ....
      }
    
      return component;
    }

You don't even need to include an additional file, since the `stylize` method comes for free just by subclassing `JasonComponent`

---

## 2. Extend Actions
Just like components, you can easily write your own native [action](actions.md) modules and call them using 100% JSON syntax.

One thing to keep in mind: **Action classes contain a collection of related action methods**, which is why most actions are namespaced as `$(GROUPNAME).(METHODNAME)` when you call it. 

For example `JasonNetworkAction` class--an action class that deals with network requests--contains multiple methods: `request`, `upload`, `auth`, `unauth`. Each method can be invoked by Jasonette using `$network.request`, `$network.auth`, `$network.upload`, and `$network.unauth` actions.

---

### 2.1. How to write an Action

Let's try writing an action class (`$util`) and one of its methods which displays a banner (`$util.banner`).

We would like to parse this JSON markup:

    {
      "type": "$util.banner",
      "options": {
        "title": "Hello",
        "description": "Hello world!"
      }
    }

to display a notification banner that looks like this:

[IMAGE]

---

####Step 1. Create an Action Class

**Three rules for writing your action class:**

1. Create a class that inherits from `JasonAction.h`.
2. The class name should follow the following convention: `Jason(GROUP_NAME)Action`
3. Only the first character of the GROUP_NAME should be capitalized

To create a class that responds to `$util.banner`, we first need a class for the `util` group. Following the rule, we need to create a `JasonUtilAction`, which inherits from `JasonAction`:

    //  JasonUtilAction.h
    #import "JasonAction.h"
    @interface JasonUtilAction : JasonAction
    @end
    
    //  JasonUtilAction.m
    #import "JasonUtilAction.h"
    @implementation JasonUtilAction
    @end

---

####Step 2. Write an Action Method

**Three rules for naming an action method:**

1. It should be an **instance method**.
2. Its return value should be `void`.
3. Whatever you name your method will respond to an action call `$[GROUP_NAME].[METHOD_NAME]`.

For example, to handle `$util.banner` action we need to implement an **instance method**, and its name should be `banner`, like this:

    //  JasonUtilAction.h
    #import "JasonAction.h"
    @interface JasonUtilAction : JasonAction
    @end
    
    //  JasonUtilAction.m
    #import "JasonUtilAction.h"
    @implementation JasonUtilAction
    - (void)banner{
        dispatch_async(dispatch_get_main_queue(), ^{
            [[TWMessageBarManager sharedInstance] showMessageWithTitle: @"Banner Title"
                                                           description: @"Here goes banner description"
                                                                  type: TWMessageBarMessageTypeInfo];
        });
        [[Jason client] success];
    }
    @end

This will display a banner when the following action is triggered

    {
      "type": "$util.banner"
    }

Notice the `[[Jason client] success]` line. This is how your action signals that its job is over. More on this later.

---

####Step 3. Pass arguments to the action

In the previous example we hardcoded the title and description for the banner. But a component that displays the same text all the time is not cool. Instead we want to pass some arguments and have them customize how the action is run. For example, we want the `$util.banner` action to take an `options` attribute and build a custom banner, like this:

    {
      "type": "$util.banner",
      "options": {
        "title": "Yo",
        "description": "Dawg"
      }
    }

To access the `options` attribute of an action, simply use `self.options` inside the method, like this:

    //  JasonUtilAction.h
    #import "JasonAction.h"
    @interface JasonUtilAction : JasonAction
    @end
    
    //  JasonUtilAction.m
    #import "JasonUtilAction.h"
    @implementation JasonUtilAction
    - (void)banner{
    
        // Extract title and description out of self.options!
        NSString *title = self.options[@"title"];
        NSString *description = self.options[@"description"];
    
        // Use the custom title and description instead of hardcoding!
        dispatch_async(dispatch_get_main_queue(), ^{
            [[TWMessageBarManager sharedInstance] showMessageWithTitle: title
                                                           description: description
                                                                  type: TWMessageBarMessageTypeInfo];
        });
        [[Jason client] success];
    }
    @end

You don't need to define `self.options` anywhere because you get it for free just by subclassing `JasonAction`.

---

####Step 4. End the action

**You MUST explicitly signal the end of each action when implementing an action method**.

This is because we can't assume that all actions end when they reach the last line. **Some actions trigger asynchronous background tasks, and we need to wait until that task is over.** 

**To signal the end of an action we call:** 

`[[Jason client] success];`

This command will:

1. Finish the current action
2. See if the current action has a `success` attribute, and if it does, proceed to call that action.

Here's an example where we make a network request, and then display a success banner when it succeeds:

    {
      "type": "$network.request",
      "options": {
        "url": "https://imagejason.herokuapp.com",
        "method": "post"
      },
      "success": {
        "type": "$util.banner",
        "options": {
          "title": "Success!",
          "description": "Successfully posted!"
        }
      }
    }

The implementation for `$network.request` may look something like this:

    //  JasonNetworkAction.m
    #import "JasonNetworkAction.h"
    @implementation JasonNetworkAction
    ...
    - (void)request{
      AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
      ... 
      dispatch_async(dispatch_get_global_queue( DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^(void){
        [manager POST:url parameters:parameters progress:^(NSProgress * _Nonnull downloadProgress) {
        } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
          dispatch_async(dispatch_get_main_queue(), ^{
            [[Jason client] success];
          });
        } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
        }];
      });
    }
    ... 
    @end

Notice how we call the `[[Jason client] success]` line **inside the "success" block, instead of the last line of the method**. This is because the network request task runs asynchronously and the action shouldn't end until it reaches inside the `success` block.


---

#### Step 5. Handle exceptions 

We can end an action with `[[Jason client] success]` when everything goes as expected. But what if something goes wrong? We still need to end the action but also run an exception handling logic, instead of going on to the next action.

For example, the `$network.request` may fail because there's no Internet connection. To handle these exceptions, Jason actions have another attribute called `error`, as demonstrated below:

    {
      "type": "$network.request",
      "options": {
        "url": "https://imagejason.herokuapp.com",
        "method": "post"
      },
      "success": {
        "type": "$util.banner",
        "options": {
          "title": "Success!",
          "description": "Successfully fetched!"
        }
      },
      "error": {
        "type": "$util.banner",
        "options": {
          "title": "ERROR!!!!",
          "description": "Uh oh, something went wrong!"
        }
      }
    }

**To signal that the action has ended with an error when we implement action methods, we call:**

 `[[Jason client] error];`.

Let's take the previous code and add that line:

    //  JasonNetworkAction.m
    #import "JasonNetworkAction.h"
    @implementation JasonNetworkAction
    ...
    - (void)request{
      AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
      ... 
      dispatch_async(dispatch_get_global_queue( DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^(void){
        [manager POST:url parameters:parameters progress:^(NSProgress * _Nonnull downloadProgress) {
        } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
          dispatch_async(dispatch_get_main_queue(), ^{
            [[Jason client] success];
          });
        } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
          dispatch_async(dispatch_get_main_queue(), ^{
            [[Jason client] error];
          });
        }];
      });
    }
    ... 
    @end

The code is mostly same as the previous one, except we've added the `[[Jason client] error];` inside the `failure` block of the task.

---

#### Step 6. Set the return value

In many cases we want to run an action, take its output, and pass it to another action. 

Here, we run a `$geo.get` action, and then display its return value as an alert using `$util.alert`.

    {
      "type": "$geo.get",
      "success": {
        "type": "$util.banner",
        "options": {
          "title": "Current Location",
          "description": "{{$jason.coord}}"
        }
      }
    }

Notice how we use a variable called `$jason`. This is how return values work in Jasonette--if an action has a return value, it passes it along to the next action as a variable named `$jason`.

**To end an action with a return value, we call:**

`[[Jason client] success: response]`

To support the JSON markup above, the `$geo.get` action implementation should look like this:

    // JasonGeoAction.m
    #import "JasonGeoAction.h"
    @implementation JasonGeoAction
    - (void)get{
    
      /*
         Implement some location sensor logic that populates the 'currentLocation' variable
      */
    
      NSString *coord = [NSString stringWithFormat:@"%g,%g", currentLocation.coordinate.latitude, currentLocation.coordinate.longitude];
      [[Jason client] success: @{@"coord": coord}];
    }
    @end

Here we end `$geo.get` with an object `@{@"coord": coord}`. As mentioned above, this object will be accessible as `$jason` in the next action. This is how the `$util.banner` action can dynamically build itself using the`$jason.coord ` value.

---

# ★  Integration

Jasonette by default covers most of what you need to build any app you can imagine. Also you can even extend its **actions** and **components**, like we discussed above. 

However if you already have existing code and just want to integrate with Jasonette here and there, that's cool too! You don't have to throw away all your existing code and re-write everything just to take advantage of Jasonette!

## How it works

Basically, `JasonViewController` is a self contained class whose only requirement is that you set its `url` before presenting it. This means you can integrate Jasonette just like using any other view controllers in your project.

---

### ★ REMEMBER - You will need to either copy all your existing project files into your Jasonette project, or Jasonette files into your own project, before you do this.

---

## 1. Non-Jasonette => Jasonette

Here's how you can open a `JasonViewController` from your own viewcontroller—it's the same as opening any other viewcontrollers.

###A. First, include JasonViewController.h in your viewcontroller

    #import "JasonViewController.h"


###B. Open with push transition:

To open the `JasonViewController` with a push transition, do this:

    JasonViewController *vc = [[JasonViewController alloc] init];
    vc.url = @"https://jasonbase.com/things/jYJ.json";
    [self.navigationViewController pushViewController:vc animated:YES];

###C. Open with modal transition:

To open `JasonViewController` as a modal, do this:

    JasonViewController *vc = [[JasonViewController alloc] init];
    vc.url = @"https://jasonbase.com/things/jYJ.json";
    UINavigationController *nav = [[UINavigationController alloc] initWithRootViewController:vc];
    [self.navigationController presentViewController:nav animated:YES completion:nil];

And that's it for Non-Jasonette => Jasonette view transition! Nothing special, just a regular viewcontroller.

---

## 2. Jasonette => Non-Jasonette

This one's a bit different, since we are transitioning from a `JasonViewController` to your custom viewcontroller. 

---

### ★ REMEMBER - Everything that happens on the Jasonette side is JSON powered.
Which means we need to handle the transition using JSON.

---

Here's how to transition from a Jasonette view to your custom non-Jasonette view:

### Option A. Using Storyboard

It works exactly the same as normal Jasonette transitions--simply attach an `href` and set its `view` attribute.

You need to name it like this:

**[STORYBOARD NAME].[STORYBOARD IDENTIFIER FOR THE VIEWCONTROLLER]**

For example, to transition to a viewcontroller which has a storyboard ID of `PaymentViewController` on a storyboard named `MainStoryboard.storyboard`, we do the following:

    {
      "type": "label",
      "text": "Make a payment",
      "href": {
        "view": "MainStoryboard.PaymentViewController"
      }
    }

### Option B. Not using Storyboard

If you don't use storyboard, you can simply enter the viewcontroller's class name. For example, to transition to a `PaymentViewController` class,

    {
      "type": "label",
      "text": "Make a payment",
      "href": {
        "view": "PaymentViewController"
      }
    }

---

###Passing arguments to your viewcontroller
Let's say we now want to pass some arguments to the `PaymentViewController`. Here's how we can achieve this:

<br><br>

####Step 1. Add an `NSDictionary` property called `jason` to your custom viewcontroller
For example if we wanted our `PaymentViewController` to support this feature, we will need to add an `NSDictionary *jason` property to the class, like this:

    // PaymentViewController.h
    #import <UIKit/UIKit.h>
    @interface PaymentViewController : UIViewController  
      ....
      @property (nonatomic, strong) NSDictionary *jason
      ....
    @end

---

####Step 2. Pass the `options` attribute inside `href`.
And we're all set! We can now start passing arguments to the `PaymentViewController` using the JSON syntax! Here's an example:

    {
      "type": "label",
      "text": "Make a payment",
      "href": {
        "view": "MainStoryboard.PaymentViewController",
        "options": {
          "product_id": "3kzf9ela",
          "user_id": "3kzf9ela"
        }
      }
    }

---

####Step 3. Use the `jason` property in your viewcontroller

Now that our `PaymentViewController` is receiving the `options` object as `jason`, we just need to make use of it in the implementation. Example:

    // PaymentViewController.m
    #import "PaymentViewController.h"
    @implementation PaymentViewController
      ....
      - (void)viewDidLoad {
        [super viewDidLoad];
        NSString *product_id = self.jason[@"product_id"];
        NSString *user_id = self.jason[@"user_id"];
    
        /*
          Use product_id and user_id for your custom logic!
        */
      }
      ....
    @end


