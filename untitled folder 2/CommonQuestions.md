---
layout: default
---


## Common questions
___

1. [Showing more than one month on the screen at the same time](#1)
2. [How do I select dates before my app starts?](#2)
3. [How to reload/scroll to a date on your calendar and update your date labels the CORRECT way.](#3)
4. [My calendar is lagging on device/ myViews are being randomly repeated across dateCells](#4)	

## 1 
___

### Is it possible to show two months on the same screen?

This would mean that you wish to make your date cells smaller. Try using the following property:

```swift
let someSmallerDateCellValue = 20
calendarView.itemSize = someSmallerDateCellValue
```

## 2
___

### How to select dates before my app starts?

You can have them selected by using the selectDate function.

```swift
self.calendarView.selectDates([NSDate()])
```

Keep in mind that this function triggers the didSelect delegate. Sometimes, you may not want this delegate to be called. In that case you can use the other options of this same function.

```swift
self.calendarView.selectDates([NSDate()], triggerSelectionDelegate: false)
```

## 3
___

### How to reload/scroll to a date on your calendar and update your date labels the CORRECT way

Assuming we just reloaded the calendarView, we now want to scroll to a particular date.

```swift
calendarView.reloadData()
// You can do any of the following based on your needs

//1. This automatically triggers your delegates
calendarView.scrollToDate(myDate) 

//2. Self explanatory. 
calendarView.scrollToDate(myDate, triggerScrollToDateDelegate: false)

//3. After we reload, we may want to scroll to a date and run some code when scrolling completes.
//   If code needs to be run after the scrolling, it must be placed in the trailing closure
calendarView.scrollToDate(myDate, triggerScrollToDateDelegate: false) {
   let currentDate = self.calendarView.currentCalendarDateSegment()
}
```

You can also accomplish the same as above with the following code:

```swift
calendarView.reloadData(withAnchorDate: myDate){
    let currentDate = self.calendarView.currentCalendarDateSegment()
}
```

## 4
___

### My calendar is lagging on device/ myViews are being randomly repeated across dateCells

Keep in mind that JTAppleCalendar is like a `UITableView`. The function:

```swift
func calendar(_ calendar: JTAppleCalendarView,
                  willDisplayCell cell: JTAppleDayCellView,
                  date: Date, cellState: CellState)
```

will be called for every dateCell displayed just like a UITableView `cellForRowAtIndexPath` function. You are required to setup things in this function **fast**. Do not put loops in there or other time consuming tasks that you would not put in a UITableView's `cellForRowAtIndexPath` method. This function should exit fast. This should cure your lagging issue. I have tested this calendar with an old iPhone 4s, and all was well. Treat this function exactly like you would treat a UITableview's `cellForRowAtIndexPath` function.

To cure your random repeating views -> If you have ever done a UITableView before, then inside the `cellForRowAtIndexPath` function, you have to reset the cell before it is used right? You will have to remove that circle you have added, or that color you have added. Reset the old text you put earlier, change that image you created etc etc. Therefore you have to do the same thing in your `willDisplayCell ` function. 
