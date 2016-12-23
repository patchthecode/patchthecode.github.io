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

Complete! 
