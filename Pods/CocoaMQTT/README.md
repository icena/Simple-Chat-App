# CocoaMQTT

![PodVersion](https://img.shields.io/cocoapods/v/CocoaMQTT.svg)
![Platforms](https://img.shields.io/cocoapods/p/CocoaMQTT.svg)
![License](https://img.shields.io/cocoapods/l/BadgeSwift.svg?style=flat)
![Swift version](https://img.shields.io/badge/swift-5-orange.svg)
[![Coverage Status](https://coveralls.io/repos/github/emqx/CocoaMQTT/badge.svg?branch=master)](https://coveralls.io/github/emqx/CocoaMQTT?branch=master)

MQTT v3.1.1 client library for iOS/macOS/tvOS  written with Swift 5


## Build

Build with Xcode 11.1 / Swift 5.1

iOS Target: 10.0 or above


## Installation
### CocoaPods

Install using [CocoaPods](http://cocoapods.org) by adding this line to your Podfile:

```ruby
use_frameworks! # Add this if you are targeting iOS 8+ or using Swift
pod 'CocoaMQTT', '1.3.0-rc.2'
```

Then, run the following command:

```bash
$ pod install
```

### Carthage
Install using [Carthage](https://github.com/Carthage/Carthage) by adding the following lines to your Cartfile:

```
github "emqx/CocoaMQTT" "master"
```

Then, run the following command:

```bash
$ carthage update --platform iOS,macOS,tvOS
```

Last if you're building for OS X:

- On your application targets “General” settings tab, in the "Embedded Binaries" section, drag and drop CocoaMQTT.framework from the Carthage/Build/Mac folder on disk.

If you're building for iOS, tvOS:

- On your application targets “General” settings tab, in the "Linked Frameworks and Libraries" section, drag and drop each framework you want to use from the Carthage/Build folder on disk.

- On your application targets "Build Phases" settings tab, click the "+" icon and choose "New Run Script Phase". Create a Run Script with the following contents: 

    ```
    /usr/local/bin/carthage copy-frameworks
    ```

- and add the paths to the frameworks you want to use under "Input Files", e.g.:

    ```
    $(SRCROOT)/Carthage/Build/iOS/CocoaMQTT.framework
    ```

## Usage

Create a client to connect [MQTT broker](https://www.emqx.io/products/broker):

```swift
let clientID = "CocoaMQTT-" + String(ProcessInfo().processIdentifier)
let mqtt = CocoaMQTT(clientID: clientID, host: "localhost", port: 1883)
mqtt.username = "test"
mqtt.password = "public"
mqtt.willMessage = CocoaMQTTWill(topic: "/will", message: "dieout")
mqtt.keepAlive = 60
mqtt.delegate = self
mqtt.connect()
```

Now you can use closures instead of `CocoaMQTTDelegate`:

```swift 
mqtt.didReceiveMessage = { mqtt, message, id in
	print("Message received in topic \(message.topic) with payload \(message.string!)")           
}
```

## SSL Secure

#### One-way certification

No certificate is required locally.
If you want to trust all untrust CA certificates, you can do this:

```swift
mqtt.allowUntrustCACertificate = true
```

#### Two-way certification

Need a .p12 file which is generated by a public key file and a private key file. You can generate the p12 file in the terminal:

```
openssl pkcs12 -export -clcerts -in client-cert.pem -inkey client-key.pem -out client.p12
```

## MQTT over Websocket

In the 1.3.0, The CocoaMQTT has supported to connect to MQTT Broker by Websocket.

If you integrated by **CocoaPods**, you need to modify you `Podfile` like the followings and execute `pod install` again:

```ruby
use_frameworks!

target 'Example' do
    pod 'CocoaMQTT/WebSockets', '1.3.0-rc.2'
end

```

If you're using CocoaMQTT in a project with only a `.podspec` and no `Podfile`, e.g. in a module for React Native, add this line to your `.podspec`:

```ruby
Pod::Spec.new do |s|
  ...
  s.dependency "Starscream", "~> 3.1.1"
end
```

Then, Create a MQTT instance over Websocket:

```swift
let websocket = CocoaMQTTWebSocket(uri: "/mqtt")
let mqtt = CocoaMQTT(clientID: clientID, host: host, port: 8083, socket: websocket)

// ...

_ = mqtt.connect()

```

If you want to add additional custom header to the connection, you can use the following:

```swift
let websocket = CocoaMQTTWebSocket(uri: "/mqtt")
websocket.headers = [
            "x-api-key": "value"
        ]
        websocket.enableSSL = true

let mqtt = CocoaMQTT(clientID: clientID, host: host, port: 8083, socket: websocket)

// ...

_ = mqtt.connect()
```

## Example App

You can follow the Example App to learn how to use it. But we need to make the Example App works fisrt:

```bash
$ cd Examples

$ pod install
```

Then, open the `Example.xcworkspace/` by Xcode and start it!


## Dependencies


These third-party functions are used:

* [GCDAsyncSocket](https://github.com/robbiehanson/CocoaAsyncSocket)
* [Starscream](https://github.com/daltoniam/Starscream)


## LICENSE

MIT License (see `LICENSE`)

## Contributors

* [@andypiper](https://github.com/andypiper)
* [@turtleDeng](https://github.com/turtleDeng)
* [@jan-bednar](https://github.com/jan-bednar)
* [@jmiltner](https://github.com/jmiltner)
* [@manucheri](https://github.com/manucheri)
* [@Cyrus Ingraham](https://github.com/cyrusingraham)

## Author

- Feng Lee <feng@emqx.io>
- CrazyWisdom <zh.whong@gmail.com>
- Alex Yu <alexyu.dc@gmail.com>


## Twitter

https://twitter.com/emqtt
