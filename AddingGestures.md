---
layout: default
---


### Adding gestures to your dateCells

Gestures are actually added on your calendar instance instead of your cells.

**Paste the following code in your project to get an idea on how to get started**

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

Love this project? Support by leaving a Start rating on github.
