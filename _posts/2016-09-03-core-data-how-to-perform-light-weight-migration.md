## Core Data - How to Perform Light Weight Migration

Some quick notes so that you and I can do it again!

- Open the existing CoreData model object, e.g. `Model.xcdatamodeld`, go to Editor menu and select Add Model Version. Call the new version something v2 and select the original model as “Based on Model” field.
- In the File Inspector on right, at the bottom in Model Version select the v2 that was just created. In Project Navigator, the model object will show both model files and the v2 one will now have a green tick.
- Add a new attribute in the model.
- Open the existing model object swift file and add a property for the new attribute.
- In `addPersistentStoreWithType()` call of NSPersistentStoreCoordinator object, supply options attribute `let options = [NSMigratePersistentStoresAutomaticallyOption: true, NSInferMappingModelAutomaticallyOption: true]`.
- Run app again and there should be no error!


### Reference

- https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreDataVersioning/Articles/vmLightweightMigration.html
- https://www.raywenderlich.com/114084/core-data-migrations-tutorial-lightweight-migrations


