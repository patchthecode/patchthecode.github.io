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
            myCustomCell.selectedView.isHidden = false
            myCustomCell.selectedView.backgroundColor = UIColor.yellow // Or you can put what ever you like for your rounded corners, and your stand-alone selected cell
        case .middle:
            myCustomCell.selectedView.isHidden = false
            myCustomCell.selectedView.backgroundColor = UIColor.blue // Or what ever you want for your dates that land in the middle
        default:
            myCustomCell.selectedView.isHidden = true
            myCustomCell.selectedView.backgroundColor = nil // Have no selection when a cell is not selected
   }
}
```

This code also has to be placed the following 2 other delegate functions.

```swift
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
    // Place code here
}
func calendar(_ calendar: JTAppleCalendarView, didDeselectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
    // Place code here
}
```

Therefore since we do not want code duplication, lets create a separate function to handle the range selection. Change all the code you have done above to look like the following.

```swift
func handleSelection(cell: JTAppleDayCellView?, cellState: CellState) {
   let myCustomCell = cell as! CellView // You created the cell view if you followed the tutorial
   switch cellState.selectedPosition() {
        case .full, .left, .right:
            myCustomCell.selectedView.isHidden = false
            myCustomCell.selectedView.backgroundColor = UIColor.yellow // Or you can put what ever you like for your rounded corners, and your stand-alone selected cell
        case .middle:
            myCustomCell.selectedView.isHidden = false
            myCustomCell.selectedView.backgroundColor = UIColor.blue // Or what ever you want for your dates that land in the middle
        default:
            myCustomCell.selectedView.isHidden = true
            myCustomCell.selectedView.backgroundColor = nil // Have no selection when a cell is not selected
   }

}
func calendar(_ calendar: JTAppleCalendarView, didSelectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
   handleSelection(cell: cell, cellState: cellState)
}
func calendar(_ calendar: JTAppleCalendarView, didDeselectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
    handleSelection(cell: cell, cellState: cellState)
}
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
    handleSelection(cell: cell, cellState: cellState)
}
```

Complete.


## Two-Tap range selection
___

Your calendar design might include a range selection that behaves like this:

1. User taps first date
2. User taps second date
3. Your App shoud now select a range of dates from the start date to the end date.

You can accomplish this is what ever way you wish, but here is a rough example.

```swift
var firstDate: Date?

func calendar(_ calendar: JTAppleCalendarView, didDeselectDate date: Date, cell: JTAppleDayCellView?, cellState: CellState) {
    if firstDate != nil {
       // This code below would be an infinite loop. And why?
       // Have you taken a look at the selectDate function?
       // By default it triggers this delegate function again.
       // This will cause an infinite loop.
       calendarView.selectDates(from: firstDate! to: secondDate)
       
       // The correct code you should do is this.
       // It will therefore not call this delegate.
       // The last parameter forces selection to yes.
       // If you leave out the final parameter, then the first
       // and last date will be deselected, because the user has
       // already selected them.
       selectDates(from: firstDate!, to: date,  triggerSelectionDelegate: false, keepSelectionIfMultiSelectionAllowed: true)
    } else {
       firstDate = date
    }
}
```
