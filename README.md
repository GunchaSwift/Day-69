# Day-69
### **Bucket List project part 2**

***Integrating MapKit and SwiftUI***

First step is to import MapKit into our XCode project. When that is done, we can make a property that will follow the state of the map.

```
@State private var mapRegion = MKCoordinateRegion(
  center: CLLocationCoordinate2D(latitude: 51.5, longitude: -0.12), 
  span: MKCoordinateSpan(latitudeDelta: 0.2, longitudeDelta: 0.2)
)
```

To simply show the MapView:

```
Map(coordinateRegion: $mapRegion)
```

To add annotations to the map, there are at least three steps to take:

1. Define a new data type that contains location
```
struct Location: Identifiable {
    let id = UUID()
    let name: String
    let coordinate: CLLocationCoordinate2D
}
```

2. Create an array containing all locations
```
let locations = [
    Location(name: "Buckingham Palace", coordinate: CLLocationCoordinate2D(latitude: 51.501, longitude: -0.141)),
    Location(name: "Tower of London", coordinate: CLLocationCoordinate2D(latitude: 51.508, longitude: -0.076))
]
```

3. Add annotations in the map
```
Map(coordinateRegion: $mapRegion, annotationItems: locations) { location in
    MapAnnotation(coordinate: location.coordinate) {
        Circle()
            .stroke(.red, lineWidth: 3)
            .frame(width: 44, height: 44)
    }
}
```

***Using Touch ID and Face ID with SwiftUI***

First step is to add new key to our project. So go to Target/Info/Right_click_add_row/Privacy - Face ID Usage Description and give it the value, for example, "We need to unlock your data." When this is done, import LocalAuthentication.

Next, we must write an authenticate() method that isolates all the biometric functionality in a single place.

1. Create instance of LAContext, which allows us to query biometric status and perform the authentication check.
2. Ask the context whether it's capable of performing biometric authentication - some devices likes iPod touch has neither Touch ID nor Face ID.
3. If biometrics are possible, kick off actual request for authentication, passing in a closure to run when authentication completes.
4. According to outcome, tell back whether it worked or not.
```
func authenticate() {
    let context = LAContext()
    var error: NSError?

    // check whether biometric authentication is possible
    if context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: &error) {
        // it's possible, so go ahead and use it
        let reason = "We need to unlock your data."

        context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, localizedReason: reason) { success, authenticationError in
            // authentication has now completed
            if success {
                // authenticated successfully
            } else {
                // there was a problem
            }
        }
    } else {
        // no biometrics
    }
}
```

This function won't do anything so far, because it's not connected to SwiftUI. So, we must add property to store state and then change it according to the outcome of authentication.




