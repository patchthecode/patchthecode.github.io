---
layout: recipe
title: Tutorial
---


###Quick tutorial - How to setup the calendar
**Note**: If at any point in this tutorial, you believe that you have already grasped the concept, then you can drop out at any time 👍 

________________

JTAppleCalendar is similar to setting up a UITableView with a custom cell.

<img width="564" alt="calendararchitecture" src="https://cloud.githubusercontent.com/assets/2439146/19026742/4b62d618-88de-11e6-90c8-c44a4195ddd2.png">



There are two parts: The `cell`, and the `calendarView`
##### 1. The cell
---
Like a UITableView, the cell has 2 sub-parts. 

* First let's create a new xib file. I'll call mine *CellView.xib*. I will setup the bare minimum; a single `UILabel` to show the date. It will be centered with Autolayout constraints. 

> Do you need more views setup on your cell like: event-dots, animated selection view, custom images etc? No problem. Design the cell however you want. This repository has sample code which demonstrates how you can do this easily. Cells can also be created via classes instead of XIBs.


<img width="201" alt="cellxib" src="https://cloud.githubusercontent.com/assets/2439146/19026781/c3793318-88de-11e6-8727-04b773b3700c.png">



* Second , create a custom class for the xib. The new class must be a subclass of `JTAppleDayCellView`. I called mine *CellView.swift*.  Inside the class setup the following:

```swift
    import JTAppleCalendar 
    class CellView: JTAppleDayCellView {
        @IBOutlet var dayLabel: UILabel!
    }
```

* Finally head back to your *cellView.xib* file and make the outlet connections.
- First,  select the root-view for the cell
- Second, click on the identity inspector
- Third, change the name of the class to one you just created: *CellView*
- Then connect your UILabel to your `dayLabel` outlet

<img width="683" alt="setupinstructions" src="https://cloud.githubusercontent.com/assets/2439146/19026812/304803d4-88df-11e6-9871-53d75b32a247.png">


##### 2. The calendarView
---
* This step is easy. Go to your Storyboard and add a `UIView` to it. 
Then, using the same procedure like you did above for the `CellView.xib` diagram above, Set the subclass of this UIView to be `JTAppleCalendarView`. Then setup an outlet for it to your viewController. I called my outlet `calendarView`. 

```swift
class ViewController: UIViewController {
    @IBOutlet weak var calendarView: JTAppleCalendarView!
.....
.....
```

You can also setup your autolayout constrainst for the calendar view at this point.


##### Whats next?
Similar to UITableView protocols, your viewController has to conform to 2 protocols for it to work

* JTAppleCalendarViewDataSource
* JTAppleCalendarViewDelegate


##### Setting up the DataSource
The data-source method is manditory.
Let's set this up now on your viewController. I have called my viewController simply `ViewController`.

I prefer setting up my protocols on my controllers using extensions to keep my code neat, but you can put it where ever youre accustomed to. 

The data-source protocol has only one function which needs to return a value of type `ConfigurationParameters`. This value requires 7 sub-values.
 
- Start boundary date: date calendar will start from
- End boundary date: date calendar will end
- The number of rows per month you want displayed
- A Calendar() instance which you should configure to your desired time zone. Please configure this properly.
- Generate in dates
- Generate out dates
- First day of week. 

Paste the following code in your project.

```swift
extension ViewController: JTAppleCalendarViewDataSource, JTAppleCalendarViewDelegate {
    func configureCalendar(_ calendar: JTAppleCalendarView) -> ConfigurationParameters {
        let formatter = DateFormatter()
        formatter.dateFormat = "yyyy MM dd"
        
        let startDate = formatter.date(from: "2016 02 01")! // You can use date generated from a formatter
        let endDate = Date()                                // You can also use dates created from this function
        let calendar = Calendar.current                     // Make sure you set this up to your time zone. We'll just use default here       

        let parameters = ConfigurationParameters(startDate: startDate,
                                                 endDate: endDate,
                                                 numberOfRows: 6,
                                                 calendar: calendar,
                                                 generateInDates: true,
                                                 generateOutDates: .tillEndOfGrid,
                                                 firstDayOfWeek: .sunday)
        return parameters
    }
```

The parameters should be self explainatory. The only ones that might be unfamiliar to you are: `in-dates` and `out-dates`. The following diagram will bring you up to speed.

<img src=https://cloud.githubusercontent.com/assets/2439146/18330595/651b8840-750e-11e6-8727-a148d7e1720f.png height=300 width= 300>

In-dates can be turned off/on. Out-dates can be generated either till the end of a row, till the end of the 6x7 grid, or off.



Now that JTAppleCalendar knows its configuration properties, it is ready to start displaying dateCells. Let's setup up the delegate protocol method to allow us to see the beautiful date cells we have designed earlier.

Add the following code to your extension. 

```swift
    func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
        (cell as? CellView)?.setupCellBeforeDisplay(cellState, date: date)
    }
```

The `setupCellBeforeDisplay:date:` function has not yet been created on your custom CellView class as yet, so let's head over to that class and implement it. 

> Note: the code from here on is yours. You do not have to call the function this name. You don't even have to put this code in your `cellView` class. This is just a tutorial. Put the code where makes sense for your project.

Modify your code in the CellView class to reflect the following. 

```swift
    import JTAppleCalendar

    class CellView: JTAppleDayCellView {
        @IBOutlet var dayLabel: UILabel!
            var thisMonthColor = UIColor.black
            var otherMonthColor = UIColor.gray


            func setupCellBeforeDisplay(_ cellState: CellState, date: NSDate) {
                // Setup Cell text
                dayLabel.text =  cellState.text

                // Setup text color
                configureTextColor(cellState)
            }

            func configureTextColor(_ cellState: CellState) {
                if cellState.dateBelongsTo == .thisMonth {
                    dayLabel.textColor = thisMonthColor
                } else {
                    dayLabel.textColor = otherMonthColor
            }
        }
    }
```

Your cell now has the ability to display text and color based on which month it belons to. One final thing needs to be done. The Calender does not have its `delegate` and `datasource` setup.  Head to your `ViewController` class, and add following code:


```swift
    @IBOutlet weak var calendarView: JTAppleCalendarView! // Don't forget to hook up the outlet to your calendarView on Storyboard
    override func viewDidLoad() {
        super.viewDidLoad()
            calendarView.dataSource = self
            calendarView.delegate = self
            calendarView.registerCellViewXib(file: "CellView") // Registering your cell is manditory
    }
```

#### Completed! Where to go from here?
---

Well if you followed this tutorial exactly, then your calendar should look like this.

<img width="219" alt="unimpressivecal" src="https://cloud.githubusercontent.com/assets/2439146/19028818/28033c46-88f5-11e6-8a25-fa6fc8651018.png">


Pretty unimpressive... :/ 
Since I am not that great of a designer yet, I will copy what one of the users of this framework has done with this calendar control [here](https://github.com/patchthecode/JTAppleCalendar/issues/2). Click on that link and scroll down till you see the purple calendar. That's the one we'll attempt to make.

#### Setting up the color 
___
Let's cheat a bit. Paste this code anywhere in your project. I'll put it in the CellView.swift file.

```swift
// Paste the code outside of the CellView class
class CellView: JTAppleDayCellView {
...
...
}

extension UIColor {
    convenience init(colorWithHexValue value: Int, alpha:CGFloat = 1.0){
        self.init(
            red: CGFloat((value & 0xFF0000) >> 16) / 255.0,
            green: CGFloat((value & 0x00FF00) >> 8) / 255.0,
            blue: CGFloat(value & 0x0000FF) / 255.0,
            alpha: alpha
        )
    }
}
```
This code allows us to create UIColors from Hex codes. 

Head over to your CellView.xib and change the root cell view background color to: #3A284C on interface builder. This is a dark color, so head over to the CellView.swift file you created earlier and change this line 

```var normalDayColor = UIColor.black``` 

to this line 

```var normalDayColor = UIColor(colorWithHexValue: 0xECEAED)```

 to brighten it up a bit. And change this line 
 
 ```var weekendDayColor = UIColor.gray```
 
  to this 
  
  ```var weekendDayColor = UIColor(colorWithHexValue: 0x574865)```. 

> Note: This is not the most efficient code. You do not want to be creating a new UIColor instance every time right? Cache this where necessary.

Your calendar will now look like this: 

<img width="309" alt="almostcompletecal" src="https://cloud.githubusercontent.com/assets/2439146/19029025/4886203a-88f7-11e6-8449-f8bdd5544120.png">


Lets get rid of the white spaces. By default, the calendar has a cell inset of 3,3. I may change this to 0,0 if enough of you complain. Head to your ViewController.swift file and add this line of code in your viewDidLoad()

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
            self.calendarView.dataSource = self
            self.calendarView.delegate = self
            calendarView.registerCellViewXib(file: "CellView") // Registering your cell is manditory

            // Add this new line
            calendarView.cellInset = CGPoint(x: 0, y: 0)       // default is (3,3)
    }

```
Your calendar will now look like this:

<img width="311" alt="completecal" src="https://cloud.githubusercontent.com/assets/2439146/19029087/ad30b7ac-88f7-11e6-9ae5-b9d0ac5c837b.png">


Tutorial over.

Create all the other views on your xib that you need. Event-dots view, customWhatEverView etc. After designing the views on your xib, go create the functionality for it just like you did in the example above; where you created a `UILabel` and added the functionality for it.

If you're really out of ideas, using the same procedure above, why not try to create a background circular shaped SelectedView to appear when ever you tap on a date cell? You can also download the example project on Github and see the possibilities.

Or [Head back to main screen](http://patchthecode.github.io/JTAppleCalendar/) to see other ways to configure your calendar.