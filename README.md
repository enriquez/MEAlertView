# MEAlertView

`UIAlertView` subclass with a simpler API using blocks and target/action invocations.

## Examples

Below are two examples of an alert view with two buttons: "Cancel" and "Ok". The first example is how you would normally use `UIAlertView`. The second is the equivalent, but using `MEAlertView`.

Using `UIAlertView` and a delegate:

    - (void)showAlert {
      UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"Alert!" message:@"This is a message" delegate:self cancelButtonTitle:@"Cancel" otherButtonTitles:@"Ok", nil];
      [alertView show];
    }

    - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
      NSString *clickedButtonTitle = [alertView buttonTitleAtIndex:buttonIndex];

      if ([clickedButtonTitle isEqualToString:@"Ok"]) {
        // do something when "Ok" is selected
      }
    }

Converted to use `MEAlertView`:

    - (void)showAlert {
      MEAlertView *alertView = [[MEAlertView alloc] initWithTitle:@"Alert!" message:@"This is a message"];
      [alertView setCancelButtonTitle:@"Cancel"];

      // Example with block. addOtherButtonWithTitle:target:action:withObject: and its variants are also available.
      [alertView addOtherButtonWithTitle:@"Ok" onTapped:^{
        // do something when "Ok" is selected
      }];
      [alertView show];
    }

## Features

* Drop-in replacement for `UIAlertView`. `MEAlertView` is a subclass of `UIAlertView` and the original API works as expected. The `UIAlertViewDelegate` works as well.
* Eliminates the long if...else if... or switch statement found in the delegate method `alertView:clickedButtonAtIndex:`. No more trying to match strings or figuring out which index of the button that was tapped.
* Results in smaller and re-usable methods using target/action.
* Simplifies the use of multiple alert views per view controller. There is no need for the alert view delegate method that tries to handle all the alert views in one view controller.
* Simplifies passing objects to button actions. You used to have to store a local instance in the view controller and access it in the delegate method. Now, you can just use the object in a block or pass it in with `addButtonWithTitle:target:action:withObject`.

## Gotchas

* Order matters. The buttons added first will appear left to right with two buttons, then top to bottom with two or more.
* The cancel button simply means it will be tapped by the system if the `alertViewCancel:` delegate is not implemented. It does not have a special style or position.
* Follow [Apple's HIG](http://developer.apple.com/library/ios/documentation/userexperience/conceptual/mobilehig/UIElementGuidelines/UIElementGuidelines.html#//apple_ref/doc/uid/TP40006556-CH13-SW39) when deciding how to order your buttons.
* If you still need the `UIAlertViewDelegate` delegate, you can set it in the constructor `initWithTitle:message:delegate:`. Or set it through the `delegate` property.
* Don't mix and match `MEAlertView` methods with `UIAlertView` methods. Stick with one or the other.


## MIT License

Copyright (C) 2012 Mike Enriquez

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
