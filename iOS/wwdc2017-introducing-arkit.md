# WWDC 2017 Introducing ARKit

[Introducing ARKit: Augmented Reality for iOS](https://developer.apple.com/videos/play/wwdc2017/602/)

### ARKit

* Mobile AR platform

* High-level API

* iOS (A9 and up)



## Get Started

![18 AM](https://github.com/derekcoder/LearningNotes/blob/master/iOS/Resources/Screen%20Shot%202018-07-05%20at%2010.55.18%20AM.png)

![48 AM](https://github.com/derekcoder/LearningNotes/blob/master/iOS/Resources/Screen%20Shot%202018-07-05%20at%2010.58.48%20AM.png)

![13 AM](https://github.com/derekcoder/LearningNotes/blob/master/iOS/Resources/Screen%20Shot%202018-07-05%20at%2011.00.13%20AM.png)

### ARSessionConfiguration

* Configuration Classes

  * `ARSessionConfiguration` -> `ARWorldTrackingSessionConfiguration`

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

* `ARSession` add/remove

* `ARFrame` anchors

* `ARSessionDelegate` add/update/remove



### ARCamera

![52 AM](https://github.com/derekcoder/LearningNotes/blob/master/iOS/Resources/Screen%20Shot%202018-07-05%20at%2011.18.52%20AM.png)

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

* `Not Available` -> `Normal` ------- > `Limited` -> `Normal` -----------

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

* Horizontal with respect to gravity

* Runs over multiple frames

* Aligned extent for surface

* Plane merging

  

```swift
// Enable plane detection on a session

// Create a new world tracking configuration
let configuration = ARWorldTrackingSessionConfiguration()

// Enable plane detection
configuration.planeDetection = .horizontal

// Change configuration on currently running session
mySession.run(configuration)
```

`ARPlaneAnchor`

* Subclasss of `ARAnchor`

* Transform

* Extent

* Center



```swift
// Called when a new plane was detected
func session(_ session: ARSession, didAdd anchors: [ARAnchor]) {
    addPlaneGeometry(forAnchors: anchors)
}

// Called when a plane's transform or extent is updated
func session(_ session: ARSession, didUpdate anchors: [ARAnchor]) {
    updatePlaneGeometry(forAnchors: anchors)
}

// Called when a plane was removed as a result of a merge
func session(_ session: ARSession, didRemove anchors: [ARAnchor]) {
    removePlaneGeometry(forAnchors: anchors)
}
```



### Hit-testing

* Intersect ray with real world

* Uses scene information

* Results sorted by distance

* Hit-test types

  * Existing plane using extent

  * Existing plane

  * Estimated plane

  * Feature point



```swift
// Adding an ARAnchor based on hit-test
let point = CGPoint(x: 0.5, y: 0.5)  // Image center

let Perform hit-test on frame
let results = frame.hitTest(point, types: [.existingPlane, ..estimatedHorizontalPlane])

// Use the first result
if let closestResult = results.first {
    // Create an Anchor for it
    let anchor = ARAnchor(transform: closestResult.worldTransform)
  
    // Add it to the session
    session.add(anchor: anchor)
}
```



### Light estimation

* Ambient intensity based on captured image

* Defaults to 1000 lumen

* Enabled by default



```swift
configuration.isLightEstimationEnabled = true

// Get ambient intensity value
let intensity = frame.lightEstimate?.ambientIntensity
```

## Rendering

### SceneKit

`ARSCNView`:  `SCNView` -> `ARSCNView` --- `ARSession `

* Draws captured image

* Updates a `SCNCamera`

* Updates scene lighting

* Maps `SCNNode`s to `ARAnchor`s

  

```swift
// ARSCNView
func session(_: ARSession, didAdd: [ARAnchor])

// ARSCNViewDelegate
func render(_: SCNSceneRender, nodeFor: ARAnchor) -> SCNNode?
func render(_: SCNSceneRender, didAdd: SCNNode, for: ARAnchor)
```

```swift
// ARSCNView
func session(_: ARSession, didUpdate: [ARAnchor])

// ARSCNViewDelegate
func render(_: SCNSceneRender, willUpdate: SCNNode, for: ARAnchor)
func render(_: SCNSceneRender, didUpdate: SCNNode, for: ARAnchor)
```

```swift
// ARSCNView
func session(_:ARSession, didRemove: [ARAnchor])

// ARSCNViewDelegate
func render(_: SCNSceneRender, didRemove: SCNNode, for: ARAnchor)
```

### SpriteKit

`ARSKView`

* Contains `ARSession`

* Draws captured image

* Maps `SKNode`s to `ARAnchor`s

* Sprites are billboarded to physical locations



### Metal

* Draw camera background

* Update virtual camera

* Update lighting

* Update transforms for geometry



#### Custom Rendering

Accessing `ARFrame`



Polling

```swift
if let frame = mySession.currentFrame {
    if (frame.timestamp > _lastTimestamp) {
        updateRenderer(frame)  // Update renderer with frame
        _lastTimestamp = frame.timestamp
    }
}
```

Delegate

```swift
func session(_ session: ARSession, didUpdate frame: ARFrame) {
    // Update renderer with frame
    updateRenderer(frame)
}
```



```swift
func updateRenderer(_ frame: ARFrame) {
    // Draw background camera image
    drawCameraImage(withPixelBuffer: frame.capturedImage)
  
    // Update virtual camera
    let viewMatrix = simd_inverse(frame.camera.transform)
    let projectionMatrix = frame.camera.projectionMatrix
    updateCamera(viewMatrix, projectionMatrix)
  
    // Update lighting
    updateLighting(frame.lightEstimate?.ambientIntensity)
  
    // Update geometry based on anchors
    drawGeometry(forAnchors: frame.anchors)
}
```



#### Helper methods

```swift
// Get the frame's display transform for the given viewport size and orientation
let transform = frame.displayTransform(withViewportSize: viewportSize, orientation: .portrait)

// Get the camera's projection matrix for the given viewport size and orientation
let projectionMatrix = camera.projectionMatrix(withViewportSize: viewportSize,
                                                    orientation: .portrait,
                                                          zNear: 0.001,
                                                           zFar: 1000)
```
