---
layout: default
---

## Configuring
Generally, configuring your dateCells is done in two delegate functions.

```swift
func calendar(_ calendar: JTAppleCalendarView,
                  willDisplayCell cell: JTAppleDayCellView,
                  date: Date, cellState: CellState)
                  
func calendar(_ calendar: JTAppleCalendarView,
                  didSelectDate date: Date,
                  cell: JTAppleDayCellView?, cellState: CellState)
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
>
> Also, the `willDisplayCell` function is called for every displayed cell on the calendar. Keep the code here fast and effecient.

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

A popular thing developers do, is change the background/color for `today's date`. If you plan on doing this, keep in mind your code should not look like this:

```swift
if cellSate.date == date {
   // Configure my cell's background this way
} else {
   // Configure my cell's background that way
}
```

your code should instead look like this:

```swift
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyy MM dd" // or whatever format you want.

let currentDateString = dateFormatter.string(from: Date())
let cellStateDateString = dateFormatter.string(from: cellState.date)

if  currentDateString ==  cellStateDateString {
   // this
} else {
   // that
}

// or you can use the Calendar() instance you supplied to the configure method

if testCalendar.isDateInToday(date) {
    // this
} else {
    // that
}
```

We compare the strings instead of the actual `Date()` instances becase Date() instances have a time stamp attached to them; And most of the times, they are different.

keep in mind, however, do not create

```swift
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "yyy MM dd"
```

Inside of your calendar functions.
And why? because `DateFormatter()` is an expensive class.

If you put it inside the `willDisplayCell()` which may get called 42 times,
it will be instanciated many times
thereby causing you to have an extremy laggy calendar. Be a good coder. Put heavy classes like this in proper places where they are instanciated only once if possible. 

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

Cells can also be made un-selectable by using the delegate function. The following code has the same effect as the code above and would be my first preference.

```swift
func calendar(_ calendar: JTAppleCalendarView, shouldSelectDate date: Date, cell: JTAppleDayCellView, cellState: CellState) -> Bool {
    if cellState.dateBelongsTo == .thisMonth {
        return true
    } else {
        return false
    }
}
```