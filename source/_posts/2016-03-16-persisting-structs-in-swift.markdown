---
layout: post
title: "Persisting Structs in Swift"
date: 2016-03-16 01:50:06 +0200
comments: true
categories: 
---

Today i have been assigned another JIRA issue by my product manager. This task is simply to mock the app data or to maintain a backup of the app data like json retrieved from API.
I just started to dig into the project code because i was newbie to this project and i found that our data models is written like Structs not Classes. The story started from here because that's made me to write this post.

I'm working now on Apple TV App (YAY), i've never imagined that i'll develop in tv OS, However things happens. the project i'm working on is a entertainment application like most Apple TV apps it display Video content so the home screen contain some recommendations video assets from our backend and it's displayed as Tiles in home screen, once the user clicks on any tiles he will be redirected to asset details screen that contain details about the video clicked[he can play video, browse cast&crew, Add to Wishlist, etc]

One of the functionality that our client requested is to be able to make the app working even the backend/server is down. So we have to persist these video assets data(recommendations,...) somewhere even if the user close that app and open it again.

First of all i thought for multiple solutions for persisting these data:

1- It make sense to use NSUserDefaults to save recommendations. As NSUserdefaults is a plists which is a NSDictionaries that can includes array, strings, numbers, and even NSDictionaries.
2- I just thought to use a local json file that contain a backup json data the same data comming from backend. 

Finally i decided to use both solutions for persisting these data, So the application flow was as follows:

This was the sudo code:

```Objective-C

call the API server to get the video assets data :
if there is a response{

// API server is responding.
- open the app normally.
- save the data locally in NSUserdefaults.
}
else {
// no response from the API server.
- check if there is a data stored locally in NSUserdefault {
    load data from NSUserdefault.
}
else {
    load data from the local json file.
}
}


```

The beauty of this way is that in any case we have a mock data so our app can work any time our server is down. even if this is the first time the user open the app and the server down, don't worry the app will load the data from the local json file. Also we will have a updated mocked data because every time server is responding we save the new data in NSUserdefault so it's updated data.

This too much information and out of scope now, sorry just thought i've to share it with you!

Let's get back to the point, So we have to store an array of recommendations objects in NSUserDefaults so we can retrieve them any time.
-
The way we all know to make this happens was as follows:

We use NSKeyArchiver the archiving functionality to achieve that.And by the way our data persistence were not large to use Core Data or SQLite.

Archiving functionality has few parts. we have to conform to **NSCoding** protocol that requires to implement 2 methods *encodeWithCoder* and * initWithCoder* which is where we specify how to encode and decode data.

Then we implement NSCoder protocol methods in our Recommendation class as follows:

```Objective-C
#pragma mark NSCoding methods

- (void)encodeWithCoder:(NSCoder *)coder;
{
    if (name != nil) {
        [coder encodeObject:entityId forKey:@"entityId"];
        [coder encodeObject:entityType forKey:@"entityType"];
        [coder encodeObject:entity forKey:@"entity"];
    }
}

- (instancetype)initWithCoder:(NSCoder *)coder
{
    if(self = [super init]) {
        self.entityId = [coder decodeObjectForKey:@"entityId"];
        self.entityType = [coder decodeObjectForKey:@"entityType"];
        self.entity = [coder decodeObjectForKey:@"entity"];
    }
    return self;
}

```
This way serialize our Recommendation object and is convert into NSData, making it easy to be persisted.

To store the encoded object as NSData we use NSKeyedArchiver archivedDataWithRootObject method as follows:

```Objective-C
NSData *encodedWeatherData = [NSKeyedArchiver archivedDataWithRootObject:recommendations];
[[NSUserDefaults standardUserDefaults]setObject:encodedWeatherData forKey:@“recommnedations_data"];
```

To decode the our recommendation model we have to search in NSUserdefaults for encoded object with the key “recommnedations_data” then decode and return it.

```Objective-C
+ (NSArray *)recommendations
{
    NSData *encodedRecommendations = [[NSUserDefaults standardUserDefaults]objectForKey:@"recommnedations_data"];
    if(encodedRecommendations) {
        return (NSArray *)[NSKeyedUnarchiver unarchiveObjectWithData:encodedRecommendations];
    }
    return nil;
}

```

# Issues with this technique:
 
##NSCoding only works with objects inheriting from NSObject.

Currently in our Apple TV application we use Structs for the model layer so every model is basically a Struct. Actually our cannot conform to NSCoding protocol because it’s not inheriting from NSObject.

As we have a lot of models in our app and we are taking advantages of the features of Structs, It doesn’t make sense to convert/rewrite our models (Recommendation) to objects rather than structs.

# Swift Approach

How we solved the problem of persisting Structs in Swift? Since we can’t archive and unarchive them like classes inherits from NSObjects.

Let’s spill out the the solution we can do:

# Structs:
Recommendation model this is one of the models that we want to persist. this model is already implemented in the project as a Struct:

```Objective-C

struct Recommendation{
    
    var entityId: NSNumber?
    var entityType: EntityTypeEnum?
    var entity: EntityProtocol?
    
    init(entityId:NSNumber, entityType: EntityTypeEnum, entity: EntityProtocol?){
    
        self.entityId   = entityId
        self.entityType = entityType
        self.entity     = entity
    }
}

```

Now we need a way to persist our recommendation model, we have to convert our Struct to another object that can be persisted like NSDictionary.

The idea is we can convert our Struct into NSDictionary object and then we can persist this NSDictionary into NSUserdefaults easily. and then we can extract this NSDictioanry from NSUserDefaults and brings it back into Recommendation instance.

we did that using a Protocol we used a protocol to make it more generic for all models of the project. this protocol contain two methods, one method return an NSDictionary from Recommendation instance by going through the Recommendation instance and set key-value pairs for each of the variables and returns an NSDictionary.

another method that takes NSDictioanry as parameter and return an instance of Recommendation model.


```Objective-C

protocol PropertyListReadable {
    func propertyListRepresentation() -> NSDictionary
    init?(propertyListRepresentation:NSDictionary?)
}

```
Now we can extend each Struct with protocol


```Objective-C

extension REMintRecommendationModel: PropertyListReadable{

    func propertyListRepresentation() -> NSDictionary {

        var representation = [String : AnyObject]()
        if let entityId = self.entityId{
            representation["entityId"] = entityId
        }
        if let type = self.entityType{
            representation["type"] = type.rawValue
        }
        if let entity = self.entity{
            representation["entity"] = entity.propertyListRepresentation()
        }
        return representation
    }

    init?(propertyListRepresentation:NSDictionary?) {

        guard let values = propertyListRepresentation
            else {return nil}
        self =  Recommendation(data: values)
    }
}


```

##Note: You may face a problem in your implementation is that you have another struct instance inside you Struct, No worries you have to follow the same approach all your persisted Structs must conform to PropertyListReadable protocol and then you can  propertyListRepresentation to get an NSDictionary for this Struct or return this Struct back from NSDictionary.

# Dealing with NSUerDefaults:

1- Extracting Structs from NSUerDefaults:


```Objective-C

    /**
     Accept an Array of dictionaries and convert it back to Array of objects by converting each dictionary to object to be persisted in NSUserDefaults.
     it accept an optional argument of an array of any object and returning an array of our generic return value. it also are specify our generic return value must conform to the PropertyListReadable protocol.

     - parameter [AnyObject]: the array of dictionaries to be converted.
     - parameter key: The key with which to check if it has a value in NSUserDefaults.
     - returns [T]: array of objects conforming to PropertyListReadable protocol.

     */
    func extractValuesFromPropertyListArray<T:PropertyListReadable>(propertyListArray:[AnyObject]?) -> [T] {
        guard let encodedArray = propertyListArray
            else {return []}
        return encodedArray.map{$0 as? NSDictionary}.flatMap{T(propertyListRepresentation:$0)}
    }

```

2- Saving Structs in NSUerDefaults:

```Objective-C

     /**
     Accept an Array of objects and convert it to Array of dictionaries by converting each object to dictionary to be persisted in NSUserDefaults.
     We’re creating an array of encoded values by taking the array parameter and mapping each one using the propertyListRepresentation function to turn it into an array of NSDictionaries.
     We then run the result through the setObject:forKey function associated with the NSUserDefaults.

     - parameter [T]: the array of objects that conform PropertyListReadable protocol to  to be saved in NSUserDefaults.
     - parameter key: The key with which to check if it has a value in NSUserDefaults.
     */
    func saveValuesToDefaults<T:PropertyListReadable>(newValues:[T], key:String) {
        let encodedValues = newValues.map{$0.propertyListRepresentation()}
        NSUserDefaults.standardUserDefaults().setObject(encodedValues, forKey:key)
    }

```


P.S. I’ve created a [Playground](https://github.com/iSame7/StructPersistorSwiftPlayground) that contains a working example.





Happy Coding :)







