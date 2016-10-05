## How do you want users to scroll the calendar?

There are 7 modes. Think about the feel of your calendar. Then choose a mode below.

`calendarView.scrollingMode = `

1. `.stopAtEachCalendarFrameWidth`:
   * **Non-continuous scrolling**. Calendar will stop scrolling when it has scrolled a distance of `frame.width`, before accepting another scroll.

   * <img src="https://cloud.githubusercontent.com/assets/2439146/19060490/904238a4-899d-11e6-8c11-36a9fa0991cd.gif" width="250" height="230">



2. `.stopAtEachSection`:
   * **Non-continuous scrolling**. Calendar will stop scrolling when it has scrolled a distance of a section's width, before accepting another scroll. Months can be divided into sections depending on how many rows per month you chose to display.

   * <img src="https://cloud.githubusercontent.com/assets/2439146/19060730/6ab850bc-899f-11e6-84ad-221839041371.gif" width="250" height="230">


3. `.stopAtEach(customInterval: CGFloat)`:
   * **Non-continuous scrolling**. Calendar will stop scrolling at every point where your set interval has been reached.

4. `.nonStopToSection(withResistance: CGFloat)`:
    * **Continuous scrolling**. Calendar will smoothly scroll and gradually decelerate. When stopped, it will snap to a section. Deceleration rate is based on resistance value (from 0.0 to 1.0)
    * <img src="https://cloud.githubusercontent.com/assets/2439146/19061814/a07fc3d4-89a8-11e6-83b1-d80759fa9404.gif" width="250" height="250">

5. `.nonStopToCell(withResistance: CGFloat)`:
    * **Continuous scrolling**. Calendar will smoothly scroll and gradually decelerate. When stopped, it will snap to a cell. Deceleration rate is based on resistance value (from 0.0 to 1.0)
    * <img src="https://cloud.githubusercontent.com/assets/2439146/19090866/2a799056-8a35-11e6-80a2-7d8c8a4e4cc4.gif" width="250" height="250">

6. `.nonStopTo(customInterval: CGFloat, withResistance: CGFloat)`:
    * **Continuous scrolling**. Calendar will smoothly scroll and gradually decelerate. When stopped, it will snap to your set custom interval. Deceleration rate is based on resistance value (from 0.0 to 1.0)

7. `.none`:
   * **Continuous scrolling**. Calendar will smoothly scroll and gradually decelerate till it stops.
