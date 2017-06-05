
# Test Summary - Unicef: Primero
Summary, Appium, Cucumber

###Project Overview

###Test poits
- **Forms**
- **Photo and Audio**
- **Synchronisation**

####Forms
> I use `Appium + Cucumber` to cover the function test of forms operation related. Mainly includes: `fill in`, `edit`. In the implementation process, I met several technical sore points.


- **Pull forms form web site**

``` ruby
# On list page, the title is "Cases". While on add page, the title is "New case".
When /^I press "(.*?)" button$/ do |button|
  base_page.clickById(button)
  if button == "add"
    page_title = main_menu.getPageTitle
    until page_title.include?("New") do
      puts "Syncing forms, please wait..."
      sleep 20
      base_page.clickById(button)
    end
  end
end
```

- **Switch between short form and long form**

``` ruby
 def scrollPartScreen(direction)
    screen_width = self.tag("android.widget.LinearLayout").size.width      #1080
    screen_height = self.tag("android.widget.LinearLayout").size.height     #1920
    case direction.downcase
      when "right"
        self.swipe(:start_x => 0.1*screen_width, :start_y => 0.5*screen_height, :end_x => 0.9*screen_width, :end_y => 0.5*screen_height, :duration => 2000)
      when "left"
        self.swipe(:start_x => 0.9*screen_width, :start_y => 0.5*screen_height, :end_x => 0.1*screen_width, :end_y => 0.5*screen_height, :duration => 2000)
      when "up"
        self.swipe(:start_x => 0.5*screen_width, :start_y => 0.7*screen_height, :end_x => 0.5*screen_width, :end_y => 0.2*screen_height, :duration => 2000)
      when "down"
        self.swipe(:start_x => 0.5*screen_width, :start_y => 0.4*screen_height, :end_x => 0.5*screen_width, :end_y => 0.9*screen_height, :duration => 2000)
      when "little_up"
        self.swipe(:start_x => 0.5*screen_width, :start_y => 0.6*screen_height, :end_x => 0.5*screen_width, :end_y => 0.55*screen_height, :duration => 2000)
      else
        puts "Unknown scroll direction."
    end
  end
```
- **Locate element**

```ruby
clickByXpath("//*[@text='#{field}']//parent::*//*[@resource-id='org.unicef.rapidreg.debug:id/option_group']//*[@text='#{option}']")
```
- **Get value of field**

``` ruby
Then /^I should see following:$/ do |table|
  table.rows_hash.each do |field, value|
    until base_page.ifTextExist(field) do
      case_page.scrollToNextFields
    end
    case_page.scrollLittleUp
    base_form.verifyFormValue(field,value)
  end
end
```
``` ruby
value = findByXpath("//*[@text='#{field}']//parent::*//android.widget.RadioGroup/*[@checked='true']").text
```

- **Keyboard operation**
    - 回车键      KEYCODE_ENTER                               66
    - 确定键      KEYCODE_DPAD_CENTER                 23
    - 向上          KEYCODE_DPAD_UP                          19
    - 向下          KEYCODE_DPAD_DOWN                    20
    - 向左          KEYCODE_DPAD_LEFT                       21
    - 向右          KEYCODE_DPAD_RIGHT                    22
    - 向上翻页   KEYCODE_PAGE_UP                          92
    - 向下翻页   KEYCODE_PAGE_DOWN                    93

####Photo and Audio

- The mechanisms of different devices calling camera is different. Including: emulator, mobile phone, tablet, picture direction.  
 
- Due to the storage of mobile device is limited, we compress the image size. It is ok to upload the compressed data, but failed to re-download partial data to mobile device. This is because web site enlarge the pictures. No matter what shape the picture is, web site will save as a square.

- Switch between short form and long form, unsaved photos and audio may be lost.

###Performance Test

- **When testing  for low bandwidth, can you please share summary of test results and methods**
- **When testing for packet loss, can you please share summary of the test results and methods**
- **When testing for high case loads, can you please share summary of test results and methods**

> For the first two requirements, we use Charles to simulate poor network conditions and intercept requests. Details refer to the test report.

> Detailed error message. Customer don't know what happened in the process of synchronisation. Proviede detailed erro message: "Timeout", "Web server crash", "Other".

>  Insert a number of same cases in the app

### Summary

- No integration with CI.

- No interface test for synchronisation function.


    