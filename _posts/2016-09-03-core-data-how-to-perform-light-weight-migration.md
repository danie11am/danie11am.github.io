## [Core Data] How to perform light weight migration

This post documents the steps to perform Core Data light weight migration, to add an attribute to a Core Data model.

1. Open the existing CoreData model object, e.g. `Model.xcdatamodeld`, go to Editor menu and select Add Model Version. Call the new version something v2 and select the original model as “Based on Model” field.
2. In the File Inspector on right, at the bottom in Model Version, select the v2 that was just created. In Project Navigator, the model object will show both model files and the v2 one will now have a green tick.
3. Add a new attribute in the v2 model.
4. Open the existing model object swift file and add a property for the new attribute.
5. In `addPersistentStoreWithType()` call of NSPersistentStoreCoordinator object, supply options attribute `let options = [NSMigratePersistentStoresAutomaticallyOption: true, NSInferMappingModelAutomaticallyOption: true]`.
6. Run app again and there should be no error!


### Reference

- [https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreDataVersioning/Articles/vmLightweightMigration.html](Apple Documentation Archive - Lightweight Migration)
- [https://www.kodeco.com/7585-lightweight-migrations-in-core-data-tutorial](https://www.kodeco.com/7585-lightweight-migrations-in-core-data-tutorial)
