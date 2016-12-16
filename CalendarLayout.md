---
layout: default
---

## How do you want your calendar to look?

This library is capable of designing any **grid-style** calendar layout. Or at least 98% of them :) 

Your layout is designed in one function:

```swift
    func configureCalendar(_ calendar: JTAppleCalendarView) -> ConfigurationParameters {
        let formatter = DateFormatter()
        formatter.dateFormat = "yyyy MM dd"
        
        let startDate = formatter.date(from: "2016 01 01")!
        let endDate = Date()
        
        let parameters = ConfigurationParameters(startDate: startDate,
                                                 endDate: endDate)
        
        return parameters
    }
```


## The concept of Indates and outDates
<img alt="image" width="300" src="https://cloud.githubusercontent.com/assets/2439146/18330595/651b8840-750e-11e6-8727-a148d7e1720f.png">

When inDates/outDates are generated, they cause your dateCells to **offset** in order to create space for them. For example, in the Image above, the **1st** has been offset to the right, in order to make room for the **31st**.

If you want your calendar's design to have this *offset* style, then you can set your *inDates* and *outDates* to be generated.
If you do not want any offsetting, then you can choose to not have them generated.

Keep in mind that you can have them generated, but choose to have them invisible as shown below:

<img alt="image" width="350" src="https://cloud.githubusercontent.com/assets/2439146/20913019/180f06c4-bb29-11e6-89b8-a474a841c85f.png">



Here are the full set of parameters of the Configure function. The only two parameters that are mandatory are the **startDate** and the **endDate**. If you leave out the others, they will just autofill with sensible defaults. 

Below are the full parameter set together with their defaults.

```swift
let parameters = ConfigurationParameters(
    startDate: startDate,  // This is manditory and must be supplied by you
    endDate: endDate,      // This is manditory and must be supplied by you
    numberOfRows: 6,                       // Default
    calendar: Calendar.current,            // Default
    generateInDates:  .forAllMonths,       // Default
    generateOutDates: .tillEndOfGrid,      // Default
    firstDayOfWeek:   .sunday              // Default
)
```

Here are the parameters that define the calendar's layout.

### 1. generateInDates

They can be set to:

1. **forFirstMonthOnly** - Only your first month will generate inDates/offsets. All the other months will start with no inDates/offsets.
2. **forAllMonths** - All months will have inDates/offsets
3. **off** - No months will have any inDates/offsets.

### 2. generateOutDates
1. **tillEndOfRow** - This will generate outDates till it reaches the first end of a row. In short - if your calendar month has 6 rows, then it will display 6 rows. If your calendar month has 5 rows, then it will display 5 rows.
2. **tillEndOfGrid** - This will generate outDates until it reaches the end of a **6 x 7** grid (42 cells). In Short, it will always display a 6 row calendar month.
3. **off** - Your calendar month will not generate any outDates.

### 3. firstDayOfWeek
You can change your calendar to start at a different day of the week.

Typical calendar headers look like this:

```
Sun | Mon | Tue | Wed | Thu | Fri | Sat
```

But with this parameter, you can change any day to be the first. For example:

```
Sat | Sun | Mon | Tue | Wed | Thu | Fri
```

## A common single row calendar mistake
One mistake many users make when making a single row calendar is to set it up like this:

```swift
let parameters = ConfigurationParameters(
    startDate: startDate,  
    endDate: endDate,      
    numberOfRows: 1,
    calendar: Calendar.current,
    generateInDates:  .forFirstMonthOnly,
    generateOutDates: .tillEndOfGrid,
    firstDayOfWeek:   .sunday
)
```

While setting it up this way is perfectly valid (if this is the way you want your calendar to be), it is not what most people want. Remember this image?

<img alt="image" width="300" src="https://cloud.githubusercontent.com/assets/2439146/18330595/651b8840-750e-11e6-8727-a148d7e1720f.png">

If you setup your calendar to be single-row with the parameters above, then you are also telling it to generate outdates. This means that when scrolling single row mode, your users will see a double set of dates. They will see the outDates of month X, and then they will see the real dates of the Month  X + 1. Based on the calendar in the example image above, they will see the dates 1st to 12th scroll by twice. 

Solution: for single row calendars, please switch your outDates to **.off**

With this flexible layout, you can generate pretty much almost any grid-style calendar layout.

## One more parameter

Still wanting to make some weird calendar no one has ever seen? There is one more parameter in your tool-set.

```swift
let parameters = ConfigurationParameters(
    startDate: startDate, 
    endDate: endDate,     
    numberOfRows: 6,
    calendar: Calendar.current,
    generateInDates:  .forFirstMonthOnly,
    generateOutDates: .tillEndOfGrid,
    firstDayOfWeek:   .sunday,
    hasStrictBoundaries: false
)
```
**hasStrictBoundaries** - This parameter applies only when you have **no** headers generated by your calendar. What does it do?

Let's say you have 6 row calendar with outDates set to **off**. Your date cells will not end at the bottom right and you will have some empty space there right? Well, with *strictBoundaries* set to **off**, then there are no strict boundaries. 

Therefore, the first dateCell of the following month will start directly after your current month's final dateCell.

Love the flexibility of this library's layout? Leave a star rating on [Github](https://github.com/patchthecode/JTAppleCalendar). I'll appreciate it greatly :)


