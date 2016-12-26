---
layout: default
---


### Quick tutorial - How to setup the calendar
**Note**: If at any point in this tutorial, you believe that you have already grasped the concept, then you can drop out at any time 👍 

________________

JTAppleCalendar is similar to setting up a UITableView with a custom cell.

<img width="564" alt="calendararchitecture" src="https://cloud.githubusercontent.com/assets/2439146/19026742/4b62d618-88de-11e6-90c8-c44a4195ddd2.png">

There are two parts: The <b><font color="red">Cell</font></b>, and the <b><font color="green">CalendarView</font></b>

##### 1. <font color="red">The cell</font>
---
Like a UITableView, the cell also has 2 sub-parts: a xib file, and a class. Let's begin by creating a new project.

* Let's now create a new xib file. I'll call mine `CellView.xib`. I will setup the bare minimum; a single `UILabel` to show the date. It will be centered with Autolayout constraints.

Click on (XCode 8) File -> New -> File -> View

 <img width="550" alt="screen shot 2016-10-04 at 2 52 19 pm" src="https://cloud.githubusercontent.com/assets/2439146/19093972/6f6eeda2-8a42-11e6-8f78-7f8f45dbfe7b.png">
 
Let's save this as `CellView.xib`. 

Your view may look very large, so to make it smaller, click on the View -> then attributes inspector, and change the size to `freeform` as shown in the image below. Then adjust the size to something smaller

<img width="660" src="https://cloud.githubusercontent.com/assets/2439146/19748042/430e767a-9b94-11e6-8d04-c52bd361cb22.png">



Drag a `UILabel` unto the view. And then position it using autolayout constraints. If you forget to use autolayout constraints, then your cell will not display properly.

<img width="201" alt="cellxib" src="https://cloud.githubusercontent.com/assets/2439146/19026781/c3793318-88de-11e6-8727-04b773b3700c.png">



* Second , create a class for the xib. The new class must be a subclass of `JTAppleDayCellView`. I created a new file for my class and called it `CellView.swift`.  

**Paste the following code inside your new class.**

```swift
import JTAppleCalendar 
class CellView: JTAppleDayCellView {
    @IBOutlet var dayLabel: UILabel!
}
```

Now let's head back to your `cellView.xib` file and make the outlet connections.

- First,  select the root-view for the cell
- Second, click on the identity inspector
- Third, change the name of the class to one you just created: `CellView`
- Then connect your UILabel to your `dayLabel` outlet

<img width="683" alt="setupinstructions" src="https://cloud.githubusercontent.com/assets/2439146/19026812/304803d4-88df-11e6-9871-53d75b32a247.png">


##### 2. <font color="green">The calendarView</font>
---
* This step is easy. Go to your Storyboard and add a `UIView` to it. You can also setup your autolayout constrainst for the calendar view at this point.

  Then, using the very same procedure like you did for the `CellView.xib` diagram above (which shows the 3 steps in red color), set the subclass of this UIView to be `JTAppleCalendarView`. Then setup an outlet for it to your viewController. I called my outlet `calendarView`. 
  
  If you haven't created your ViewController class yet, do so now. I have called my viewController class simply `ViewController`.

**Paste this code in your ViewController class**

```swift
import JTAppleCalendar
class ViewController: UIViewController {
    @IBOutlet weak var calendarView: JTAppleCalendarView!
}
```

After pasting the above code, do not forget to now connect this outlet to the JTAppleCalendarView on storyboard.

## Whats next?
___

Similar to UITableView protocols, your ViewController has to conform to 2 protocols for it to work

* JTAppleCalendarViewDataSource
* JTAppleCalendarViewDelegate


##### Setting up the DataSource
The data-source protocol method is manditory.
Let's set this up now on your ViewController class. 

I prefer setting up my protocols on my controllers using extensions to keep my code neat, but you can put it where ever you're accustomed to. 

The data-source protocol has only one function which needs to return a value of type `ConfigurationParameters`. This value requires 7 sub-values.

**Paste the following code in your project.**

```swift
extension ViewController: JTAppleCalendarViewDataSource, JTAppleCalendarViewDelegate {
    func configureCalendar(_ calendar: JTAppleCalendarView) -> ConfigurationParameters {
        let formatter = DateFormatter()
        formatter.dateFormat = "yyyy MM dd"
        
        let startDate = formatter.date(from: "2016 02 01")! // You can use date generated from a formatter
        let endDate = Date()                                // You can also use dates created from this function
        let parameters = ConfigurationParameters(startDate: startDate,
                                                 endDate: endDate,
                                                 numberOfRows: 6, // Only 1, 2, 3, & 6 are allowed
                                                 calendar: Calendar.current,
                                                 generateInDates: .forAllMonths,
                                                 generateOutDates: .tillEndOfGrid,
                                                 firstDayOfWeek: .sunday)
        return parameters
    }
}
```

- **Start boundary date**: date calendar will start from
- **End boundary date**: date calendar will end
- **Rows per month**: The number of rows per month you want displayed
- **In-dates**: Generate in dates
- **Out-Dates**: Generate out dates
- **First day of week**: Determine which is the first day of the week to start with 

The parameters should be self explainatory. The only ones that might be unfamiliar to you are: `in-dates` and `out-dates`. The following diagram will bring you up to speed.

<img width="300" src="https://cloud.githubusercontent.com/assets/2439146/18330595/651b8840-750e-11e6-8727-a148d7e1720f.png">

Now that JTAppleCalendar knows its configuration properties, it is ready to start displaying dateCells. Let's setup up the delegate protocol method to allow us to see the beautiful date cells we have designed earlier.

Add the following code to your extension. 

```swift
    func calendar(_ calendar: JTAppleCalendarView, willDisplayCell cell: JTAppleDayCellView, date: Date, cellState: CellState) {
        let myCustomCell = cell as! CellView
        
        // Setup Cell text
        myCustomCell.dayLabel.text = cellState.text
        
        // Setup text color
        if cellState.dateBelongsTo == .thisMonth {
            myCustomCell.dayLabel.textColor = UIColor.black
         } else {
            myCustomCell.dayLabel.textColor = UIColor.gray
         }
    }
```

<b><font color="red">Important</font></b>: cache your colors in a real app else your calendar will be laggy. The above code is for demonstration only. Just like in `UITableView`, this function will be called for every cell displayed on screen so be **effecient** with this code.

Your cell now has the ability to display text and color based on which month it belongs to. One final thing needs to be done. The Calender does not have its `delegate` and `datasource` setup.  In your `ViewController` class, 

**paste the following code**:


```swift
@IBOutlet weak var calendarView: JTAppleCalendarView! 
override func viewDidLoad() {
    super.viewDidLoad()
        calendarView.dataSource = self
        calendarView.delegate = self
        calendarView.registerCellViewXib(file: "CellView") // Registering your cell is manditory
    }
}
```

#### Completed! Where to go from here?
---

Well if you followed this tutorial exactly, then your calendar should look like this.

<img width="219" alt="unimpressivecal" src="https://cloud.githubusercontent.com/assets/2439146/19028818/28033c46-88f5-11e6-8a25-fa6fc8651018.png">

If your display does not look like this then:

1. Did you remember to set autolayout constraints?
2. Is your view shifted off the screen? [Look at a solution here](https://patchthecode.github.io/CommonProblems/). Shifting often occurs if you put your calendar inside of a `UINavigationController`.


Pretty unimpressive... :/ 
Since I am not that great of a designer yet, I will copy what one of the users of this framework has done with this calendar control [here](https://github.com/patchthecode/JTAppleCalendar/issues/2). Click on that link and scroll down till you see the purple calendar. That's the one we'll attempt to make.

#### Setting up the color 
___
Let's cheat a bit. Paste this code anywhere in your project. I'll put it in the ViewController.swift file.

```swift
// Paste the code outside of the ViewController class
class ViewController: UIViewController {
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

Head over to your `CellView.xib` and change the root cell view background color to: #3A284C on interface builder. This is a dark color, so head over to the ViewController.swift file and change the colors. 

Change: 

1. The **UIColor.black** to **UIColor(colorWithHexValue: 0xECEAED)**
2. The **UIColor.gray** to **UIColor(colorWithHexValue: 0x574865)**


Your calendar will now look like this: 

<img width="309" alt="almostcompletecal" src="https://cloud.githubusercontent.com/assets/2439146/19029025/4886203a-88f7-11e6-8449-f8bdd5544120.png">


Lets get rid of the white spaces. By default, the calendar has a cell inset of 3,3. I may change this to 0,0 if enough of you complain. In your ViewController.swift file and add this line of code in your viewDidLoad()

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


**Complete**. Jump over to [Part 2](https://patchthecode.github.io/MainTutorial2) of this tutorial.

Create all the other views on your xib that you need. Event-dots view, customWhatEverView etc. After designing the views on your xib, go create the functionality for it just like you did in the example above; where you created a `UILabel` and added the functionality to display it. You can literally create **any** custom thing with any custom functionality. 

If you're really out of ideas, using the same procedure above, why not try to create a background circular shaped SelectedView to appear when ever you tap on a date cell? You can also download the example project on Github and see the possibilities.