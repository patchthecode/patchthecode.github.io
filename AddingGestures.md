---
layout: default
---


### Adding gestures to your dateCells

Gestures are actually added on your calendar instance instead of your cells.

**Paste the following code in your project. This code adds double taps**

```swift
override func viewDidLoad() {
    let doubleTapGesture: UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(didDoubleTapCollectionView(gesture:)))
    doubleTapGesture.numberOfTapsRequired = 2  // add double tap
    calendarView.addGestureRecognizer(doubleTapGesture)
}

func didDoubleTapCollectionView(gesture: UITapGestureRecognizer) {
    let point = gesture.location(in: gesture.view!)
    let cellState = calendarView.cellStatus(at: point)
    print(cellState!.date)
}
```

**This code adds fluid range selection. You can modify it to suite your app**

```swift
var rangeSelectedDates: [Date] = []

override func viewDidLoad() {
	calendarView.allowsMultipleSelection = true
	let panGensture = UILongPressGestureRecognizer(target: self, action: #selector(didStartRangeSelecting(gesture:))) 
	panGensture.minimumPressDuration = 0.5 // or you can choose what ever duration you want
	calendarView.addGestureRecognizer(panGensture)
}

    
func didStartRangeSelecting(gesture: UILongPressGestureRecognizer) {
    let point = gesture.location(in: gesture.view!)
    if let cellState = calendarView.cellStatus(at: point) {
        if !rangeSelectedDates.contains(cellState.date) {
            rangeSelectedDates.append(cellState.date)
            calendarView.selectDates([cellState.date])
        }
    }
    if gesture.state == .ended {
        rangeSelectedDates.removeAll()
    }
}
```

Complete! 
