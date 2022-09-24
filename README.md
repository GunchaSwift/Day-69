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


