---
layout: post
title:  "Change UISearchBar Keyboard Appearance"
date:   2017-02-23 11:00:00
categories: CodeSnippet
tags: Code ObjectiveC UIKit UISearchBar UIKeyboardAppearance Alert
comments: true
logo: search
---

## How to change UISearchBar Keyboard Appearance?

Just paste following code snip & call function

### Swift code

```swift
extension ViewController: UISearchBarDelegate {

    func searchBarTextDidBeginEditing(_ searchBar: UISearchBar) {
        ViewController.changeSearchBarKeyboardColor(searchBar)
    }

    public class func changeSearchBarKeyboardColor(_ searchBar: UISearchBar) {
        for subview in searchBar.subviews {
            for innerView in subview.subviews {
                if let _ = innerView as? UITextInputTraits,
                    let textField = innerView as? UITextField {
                    textField.keyboardAppearance = .alert
                }
            }
        }
    }
}
```

### Objective-C code

```objective-c
+ (void)changeSearchBarColor:(UISearchBar *)searchBar {
   for (UIView *subview in searchBar.subviews) {
       for (UIView *subSubview in subview.subviews) {
           if ([subSubview conformsToProtocol:@protocol(UITextInputTraits)]) {
               UITextField *textField = (UITextField *)subSubview;
               [textField setKeyboardAppearance: UIKeyboardAppearanceAlert];
               break;
           }
       }
   }
}
```

