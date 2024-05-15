# iOS-Calendar

**iOS Calender** is a Swift calendar UI library for iOS and Mac Catalyst. It looks similar to the Apple Calendar app out-of-the-box, while allowing customization when needed. iOS Calender is composed of multiple modules which can be used together or independently.

To try iOS Calender with CocoaPods issue the following command in the Terminal:
```ruby
pod try iOS Calender
```

## Installation
iOS Calender can be installed with Swift Package Manager or with CocoaPods.
### Swift Package Manager (Xcode 12 or higher)

The preferred way of installing iOS Calender is via the [Swift Package Manager](https://swift.org/package-manager/).

1. In Xcode, open your project and navigate to **File** → **Swift Packages** → **Add Package Dependency...**
2. Paste the repository URL (`https://github.com/Rutvik-Gandhi/iOS-Calendar.git`) and click **Next**.
3. For **Rules**, select **Version (Up to Next Major)** and click **Next**.
4. Click **Finish**.

[Adding Package Dependencies to Your App](https://developer.apple.com/documentation/swift_packages/adding_package_dependencies_to_your_app)

### CocoaPods

To install it, add the following line to your Podfile:

```ruby
pod 'iOS Calendar'
```
[Adding Pods to an Xcode project](https://guides.cocoapods.org/using/using-cocoapods.html)

## Usage
1. Subclass `DayViewController`
2. Implement `EventDataSource` protocol to show events.

iOS Calender requires `EventDataSource` to return an array of objects conforming to `EventDescriptor` protocol, specifying all the information needed to display a particular event. You're free to use a default `Event` class as a model or create your own class conforming to the `EventDescriptor` protocol.

```swift
// Return an array of EventDescriptors for particular date
override func eventsForDate(_ date: Date) -> [EventDescriptor] {
  var models = myAppEventStore.getEventsForDate(date) // Get events (models) from the storage / API

  var events = [Event]()

  for model in models {
      // Create new EventView
      let event = Event()
      // Specify DateInterval
      event.dateInterval = DateInterval(start: model.startDate, end: model.endDate)
      // Add info: event title, subtitle, location to the array of Strings
      var info = [model.title, model.location]
      info.append("\(datePeriod.beginning!.format(with: "HH:mm")) - \(datePeriod.end!.format(with: "HH:mm"))")
      // Set "text" value of event by formatting all the information needed for display
      event.text = info.reduce("", {$0 + $1 + "\n"})
      events.append(event)
  }
  return events
}
```
After receiving an array of events for a particular day, iOS Calender will handle view layout and display.

### Usage
To respond to the user input, override mehtods of `DayViewDelegate`, for example:

```swift
override func dayViewDidSelectEventView(_ eventView: EventView) {
  print("Event has been selected: \(eventview.data)")
}

override func dayViewDidLongPressEventView(_ eventView: EventView) {
  print("Event has been longPressed: \(eventView.data)")
}
```

## Localization
iOS Calender supports localization and uses iOS default locale to display month and day names. First day of the week is also selected according to the iOS locale.

## Styles
By default, iOS Calender looks similar to the Apple Calendar app and fully supports Dark Mode. If needed, iOS Calender's look can be easily customized. Steps to apply a custom style are as follows:

1. Create a new `CalendarStyle` object (or copy existing one)
2. Change style by updating the properties.
3. Invoke `updateStyle` method with the new `CalendarStyle`.


```Swift
let style = CalendarStyle()
style.backgroundColor = UIColor.black
dayView.updateStyle(style)
```

## Requirements

- iOS 11.0+, macOS (Catalyst) 10.15+
- Swift 5.7+

## License

**iOS Calender** is available under No License.
