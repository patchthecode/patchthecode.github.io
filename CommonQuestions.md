---
layout: default
---


## Common questions
___

1. [Showing more than one month on the screen at the same time](#1)
2. [How do I select dates before my app starts?](#2)
3. [How to reload/scroll to a date on your calendar and update your date labels the CORRECT way.](#3)	

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
