---
layout: default
---

## Configuring
Configuring your dateCells is done in one delegate function.

```swift
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState)
```

> IMPORTANT: Keep in mind that JTAppleCalendar is a subclass of a `UICollectionView`. Therefore, dateCells are reused. It is **important** to reset your cell completely just like you would for a `UICollectionViewCell` or a `UITableViewCell`. 
> 
> ```swift
> // For instance, it is not enough to only have the first if clause:
> if date == someDate {
>    configureCellViewInThisWay()
> }
> // You may also need the else clause to fully reset the cell because cells are being reused.
> else {
>    configureCellViewInThatOtherWay()
> }
> ```

This delegate function provides you with **more than enough** information you can use to configure your cell. It provides you with the following:

* **calendar** - The calendarView instance. Useful in case you have multiple calendars.
* **cell** - The view you will be configuring.
* **date** - date the cell represents
* **cellState** - This is a structure which provides even more cell information:
  * isSelected - Bool value indicating if the cell is selected
  * text - The text of the cell
  * dateBelongsTo - Tells which month the cell belongs to. With this value, you can tell if this cell is a in-date cell, or an out-date cell etc.
  *  date - the date
  *  day - the day of week. Sunday to Saturday.
  *  row - row of the cell
  *  column - column of the cell
  *  dateSection - section information
  *  selectedPosition - Used to implement date range selection.
  *  cell - the containing cell.


## So with all this information, what can we do with dateCells?

#### Although, you can configure everything about the cell, here are some major ones.

### 1. Changing the color

```swift
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
   let myCustomCell = cell as! CellView // CellView is the class you created if you followed the Tutorial
   if cellState.dateBelongsTo == .thisMonth {
      // Make sure you cache your colors. 
      // Creating them on the fly like this example makes a laggy calendar
      myCustomCell.dayLabel.textColor = UIColor.black 
   } else {
      myCustomCell.dayLabel.textColor = UIColor.gray   
   }
}
```

### 2. Making cells invisible

```swift
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
   let myCustomCell = cell as! CellView // CellView is the class you created if you followed the Tutorial
   if cellState.dateBelongsTo == .thisMonth {
      myCustomCell.isHidden = false 
   } else {
      myCustomCell.isHidden = true  
   }
}
```

### 3. Making cells unselectable

```swift
func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
   let myCustomCell = cell as! CellView // CellView is the class you created if you followed the Tutorial
   if cellState.dateBelongsTo == .thisMonth {
      myCustomCell.isUserInteractionEnabled = true 
   } else {
      myCustomCell.isUserInteractionEnabled = false  
   }
}
```

