---
layout: post
title:  "Step by Step Core Data Implementation for iOS 10 and above"
date:   2017-11-04 10:00:00
categories: Illustration
tags: CoreData Data Storage iOS SQLite
comments: true
logo: database
---

***NOTE:*** This article is outdated. Please [click here]({{"2017/11/12/CoreData-Xcode9/" | absolute_url}}) to read updated article.

---

***OUTDATED*** article. Please [click here]({{"2017/11/12/CoreData-Xcode9/" | absolute_url}})

---

Please find steps for core Data implementation as follows.

#### Step 1: Create Data model 

Create Data model as specified below.

![CoreDataModel Image]({{ "/assets/2017-11-04/CoreDataModel.png" | absolute_url }})

#### Step 2: Put following Code in `AppDelegate`

Make sure you change name of Data Model as per your project's data-model file.
In my case, it is "CoreDataDemo".

```swift
import UIKit
import CoreData

let appDel = (UIApplication.shared.delegate as? AppDelegate)!

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

  var window: UIWindow?

  func application(_ application: UIApplication, 
    didFinishLaunchingWithOptions 
    launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    // application launched
  }

  func applicationWillResignActive(_ application: UIApplication) {
    // application is going to go in background
  }

  func applicationDidEnterBackground(_ application: UIApplication) {
    // application is in background
  }

  func applicationWillEnterForeground(_ application: UIApplication) {
    // application is about to become active
  }

  func applicationDidBecomeActive(_ application: UIApplication) {
    // application came to forground
  }

  func applicationWillTerminate(_ application: UIApplication) {
    // application will be killed
    self.saveContext()
  }

  // MARK: - Core Data stack

  lazy var persistentContainer: NSPersistentContainer = {
      let container = NSPersistentContainer(name: "CoreDataDemo")
      container.loadPersistentStores(completionHandler: { (storeDescription, error) in
          if let error = error as NSError? {
              fatalError("Unresolved error \(error), \(error.userInfo)")
          }
      })
      return container
  }()

  var context: NSManagedObjectContext {
    get {
      return self.persistentContainer.viewContext
    }
  }

  // MARK: - Core Data Saving support
  func saveContext () {
      let context = persistentContainer.viewContext
      if context.hasChanges {
          do {
              try context.save()
          } catch {
              // Replace this implementation with code to handle the error appropriately.
              // fatalError() causes the application to generate a crash log and terminate. 
              // You should not use this function in a shipping application, 
              // although it may be useful during development.
              let nserror = error as NSError
              fatalError("Unresolved error \(nserror), \(nserror.userInfo)")
          }
      }
  }
}
```

#### Step 3: Add Object

Following function is an example of adding an object

```swift
func addOffice(_ name: String) {
  let entity = NSEntityDescription.entity(forEntityName: "Office", in: self.context)
  let newOffice = NSManagedObject(entity: entity!, insertInto: self.context)
  newOffice.setValue(name, forKey: "name")
  self.saveContext()
}
```

#### Step 4: Fetch Objects

Following function is an example of fetching objects

```swift
func getOffices() -> [Office] {
  let fetchRequest: NSFetchRequest<Office> = Office.fetchRequest()
  do {
    //go get the results
    let searchResults = try appDel.context.fetch(fetchRequest)
    return searchResults
  } catch {
    return []
  }
}
```

#### Step 5: Delete Object

```swift
func deleteOffice(office: Office) {
  appDel.context.delete(office)
  appDel.saveContext()
}
```

#### Sample code to use above methods.

```swift
override func viewDidLoad() {
  super.viewDidLoad()
  appDel.addOffice("Sagar")
  appDel.addOffice("Kothari")
  appDel.addOffice("Pune")
  appDel.addOffice("Mumbai")
  let array = appDel.getOffices()
  for office in array {
    print("Office name is \(office.name!)")
  }
}
```

#### Output is as follows.

```
Office name is Sagar
Office name is Kothari
Office name is Pune
Office name is Mumbai
```

# Download Source code

[Download Source Code]({{ "/assets/2017-11-04/CoreDataDemo.zip" | absolute_url }})