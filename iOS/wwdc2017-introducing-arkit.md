# WWDC 2017 Introducing ARKit



### ARKit

* Mobile AR platform

* High-level API

* iOS (A9 and up)



## Get Started

![18 AM](Resources/Screen Shot 2018-07-05 at 10.55.18 AM.png)

![48 AM](/Users/Derek/Developer/LearningNotes/iOS/Resources/Screen Shot 2018-07-05 at 10.58.48 AM.png)

![13 AM](Resources/Screen Shot 2018-07-05 at 11.00.13 AM.png)





### ARSessionConfiguration

* Configuration Classes

  * ARSessionConfiguration -> ARWorldTrackingSessionConfiguration

* Enable/Disable Features

* Availability

  ```swift
  if ARWorldTrackingSessionConfiguration.isSupported {
      configuration = ARWorldTrackingSessionConfiguration()
  } else {
      configuration = ARSessionConfiguration()
  }
  ```

### ARSession

* Manage AR processing

  ```swift
  // Run your session
  session.run(configuration)
  
  // Pause your session
  session.pause()
  
  // Resume your session
  session.run(session.configuration)
  
  // Change your configuration
  session.run(otherConfiguration)
  ```

* Reset tracking

  ```swift
  // Reset tracking
  session.run(configuration, options: .resetTracking)
  ```

* Session updates

```swift
// Access the latest frame
func session(_: ARSession, didUpdate: ARFrame)

// Handle session errors
func session(_: ARSession, didFailWithError: Error)
```

* Current frame



### ARFrame

* Captured image

* Tracking information

* Scene information



### ARAnchor

* Real-world position and orientation

* ARSession add/remove

* ARFrame anchors

* ARSessionDelegate add/update/remove



### ARCamera

![52 AM](Resources/Screen Shot 2018-07-05 at 11.18.52 AM.png)

* Transform

* Tracking state

* Camera intrinsics



## Tracking

### World tracking

* Position and orientation

* Physical distances

* Relative to starting position

* 3D-feature points

```swift
// Create a session
let mySession = ARSession()

// Set ourselves as the session delegate
mySession.delegate = self

// Create a world tracking configuration
let configuration = ARWorldTrackingSessionConfiguration()

// Run the session
mySession.run(configuration)
```

### Tracking Quality

- Uninterrupted sensor data

- Textured enevironments

- Static scenes

  

### Tracking State

* Not Available -> Normal ------- > Limited -> Normal -----------

  ```swift
  func session(_ session: ARSession, cameraDidChangeTrackingState camera: ARCamera) {
      if case .limited(let reason) = camera.trackingState {
          // Notify user of limited tracking state
          ...
      }
  }
  ```

  # 

### Session Interruptions

* Camera input unavailable

* Tracking is stopped

```swift
func sessionWasInterrupted(_ session: ARSession) {
	  showOverlay()
}

func sessionInterruptionEnded(_ session: ARSession) {
    hideOverlay()
    // Optionally restart experience
    ...
}
```



## Scene Understanding



### Plane detection



### Hit-testing



### Light estimation



## Rendering

### Easy integration



### AR views

### Custom rendering