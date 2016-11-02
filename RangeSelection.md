---
layout: default
---

## Range Selection
___

<img src="https://cloud.githubusercontent.com/assets/4571502/16706761/ff11073e-45ea-11e6-8d1a-79fc0c15df90.gif" height="200" width="200">

Although the implementation is up to you, two things are necessary.

**Place this code in your viewDidLoad() function** 

```swift
calendarView.allowsMultipleSelection  = true
calendarView.rangeSelectionWillBeUsed = true
```

The following delegate function is your first place to implement date ranges. Basically, you are checking to see what is your cell's selected position and applying the necessary changes.

```swift
func calendar(_ calendar: JTAppleCalendarView, didSelectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
   let myCustomCell = cell as! CellView // You created the cell view if you followed the tutorial
   switch cellState.selectedPosition() {
        case .full, .left, .right:
            myCustomCell.selectedView.backgroundColor = UIColor.yellow // Or you can put what ever you like for your rounded corners, and your stand-alone selected cell
        case .middle:
            myCustomCell.selectedView.backgroundColor = UIColor.blue // Or what ever you want for your dates that land in the middle
        default:
            myCustomCell.selectedView.backgroundColor = nil // Have no selection when a cell is not selected
   }
}
```

This code also has to be placed the following 2 other delegate functions. Please put the code in a function to avoid code duplication.

```swift
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
    // Place code here
}
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
    // Place code here
}
```
