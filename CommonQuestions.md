---
layout: default
---


## Common questions
___

1. [Showing more than one month on the screen at the same time](#1)
2. [How do I select dates before my app starts?](#2)
3. [How to reload/scroll to a date on your calendar and update your date labels the CORRECT way.](#3)
4. [My Views are being randomly repeated across dateCells](#4)	
5. [My calendar performance when scrolling is bad](#5)	

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

### My views are being randomly repeated across dateCells

To cure your random repeating views -> If you have ever done a UITableView before, then inside the `cellForRowAtIndexPath` function, you have to reset the cell before it is used right? You will have to remove that circle you have added, or that color you have added. Reset the old text you put earlier, change that image you created etc etc. Therefore you have to do the same thing in your `willDisplayCell ` function. 

## 5 My calendar performance when scrolling is bad

Note: This library has been tested on an old iPhone 4s with 100 years of data, and scrolling was good. If you have anything better than an iPhone 4s and your performance is bad, then you are doing something that can be improved.

Keep in mind that JTAppleCalendar is like a `UITableView`. The function:

```swift
func calendar(_ calendar: JTAppleCalendarView,
                  willDisplayCell cell: JTAppleDayCellView,
                  date: Date, cellState: CellState)
```

will be called for every dateCell displayed just like a UITableView `cellForRowAtIndexPath` function. You are required to setup things in this function **fast**. 

Do not put time consuming tasks that you would not put in a UITableView's `cellForRowAtIndexPath` method, inside the `willDisplayCell` function. This function should exit fast. Treat this function exactly like you would treat a UITableview's `cellForRowAtIndexPath` function.

If a time consuming task cannot be avoided, then do what good coders do; Put the heavy tasks on a background thread, and then return to the main thread to update your cellView when the task is complete 👍.

Let's assume you have a 6-row calendar (42 cells total). Let's assume you have a loop in your `willDisplayCell` function which loops through 100 items. Your loop will run for 42 * 100 = 4200 times! Think through your design. Be a good developer :)

Also, please note that when working with dates, you may need to use the `DateFormatter()` instance. `DateFormatter` is notoriously slow to initialize. Please initialize this only once (if you can) and what ever you do, do not initialize it in the `willDisplayCell` function.