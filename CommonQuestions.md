## Common questions
___

1. [Showing more than one month on the screen at the same time](#1)
2. [How do I select dates before my app starts?](#2)	

## 1 

### Is it possible to show two months on the same screen?

This would mean that you wish to make your date cells smaller. Try using the following property:

```swift
let someSmallerDateCellValue = 20
calendarView.itemSize = someSmallerDateCellValue
```

## 2

### How to select dates before my app starts?

You can have them selected by using the selectDate function.

```swift
self.calendarView.selectDates([NSDate()])
```

Keep in mind that this function triggers the didSelect delegate. Sometimes, you may not want this delegate to be called. In that case you can use the other options of this same function.

```swift
self.calendarView.selectDates([NSDate()], triggerSelectionDelegate: false)
```
