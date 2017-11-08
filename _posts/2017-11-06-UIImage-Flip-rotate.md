---
layout: post
title:  "UIImage Rotate Flip - code snippet"
date:   2017-11-06 10:00:00
categories: CodeSnippet
tags: Swift 
---

### Put following swift extension of `UIImage` class in project

```swift
extension UIImage {
  
  public func imageRotatedByDegrees(degrees: CGFloat, flip: Bool) -> UIImage? {
    let radiansToDegrees: (CGFloat) -> CGFloat = {
      return $0 * (180.0 / CGFloat(M_PI))
    }
    let degreesToRadians: (CGFloat) -> CGFloat = {
      return $0 / 180.0 * CGFloat(M_PI)
    }
    
    // calculate the size of the rotated view's containing box for our drawing space
    let rotatedViewBox = UIView(frame: CGRect(origin: CGPoint(x: 0, y: 0), size: size))
    let t = CGAffineTransform(rotationAngle: degreesToRadians(degrees));
    rotatedViewBox.transform = t
    let rotatedSize = rotatedViewBox.frame.size
    
    // Create the bitmap context
    UIGraphicsBeginImageContext(rotatedSize)
    let bitmap = UIGraphicsGetCurrentContext()
    
    // Move the origin to the middle of the image so we will rotate and scale around the center.
    bitmap!.translateBy(x: rotatedSize.width / 2.0, y: rotatedSize.height / 2.0);
    
    //   // Rotate the image context
    bitmap!.rotate(by: degreesToRadians(degrees));
    
    // Now, draw the rotated/scaled image into the context
    var yFlip: CGFloat
    
    if(flip){
      yFlip = CGFloat(-1.0)
    } else {
      yFlip = CGFloat(1.0)
    }
    
    bitmap!.scaleBy(x: yFlip, y: -1.0)
    let rect = CGRect(x: -size.width / 2, y: -size.height / 2, width: size.width, height: size.height)
    bitmap!.draw(cgImage!, in: rect)
    
    let newImage = UIGraphicsGetImageFromCurrentImageContext()
    UIGraphicsEndImageContext()
    
    return newImage
  }
}
```
