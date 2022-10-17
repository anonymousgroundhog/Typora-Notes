# Psuedocode 10/17/22

## Generating Button Clicks Pseudocode

```markdown
generate empty dataframe with columns app_name and button
Generate xml file of app view
    for line in xml file
        if line contains clickable = true
        	add resource-id value to dataframe column button and add app name to app_name column
```

## Python Code

```python
import time, xml.etree.ElementTree as ET
from appium import webdriver

def set_driver(desired_capabilities):
    return webdriver.Remote("http://127.0.0.1:4723/wd/hub", desired_capabilities)

desired_capabilities = {
    "platformName": "Android",
    # "platformVersion": "10",
    "deviceName": "Pixel_3a_API_30",
    "appPackage": "com.haken.qrcode",
  "noReset": True,
  "autoacceptalerts": True,
  "autoGrantPermissions": True,
  "appActivity": "com.haken.qrcode.ActivityUtil.Splash"
}
#--------------------------------------SET DRIVER--------------------------------------
driver = set_driver(desired_capabilities)

source_xml = driver.page_source
file = open("XML\\qrcode.xml", "w")
file.write(source_xml)
file.close()

intersting_things_to_click = []
file = open("XML\\qrcode.xml", "r")
Lines = file.readlines()
for line in Lines:
    if 'clickable="true"' in line:
        print("CLICKABLE:"+line)

file.close()
```



### XML file Example

```xml
<?xml version='1.0' encoding='UTF-8' standalone='yes' ?>

<hierarchy index="0" class="hierarchy" rotation="0" width="1080" height="2088">

  <android.widget.FrameLayout index="0" package="com.haken.qrcode" class="android.widget.FrameLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,0][1080,2088]" displayed="true">

    <android.widget.LinearLayout index="0" package="com.haken.qrcode" class="android.widget.LinearLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,0][1080,2088]" displayed="true">

      <android.widget.FrameLayout index="0" package="com.haken.qrcode" class="android.widget.FrameLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,2088]" displayed="true">

        <android.widget.LinearLayout index="0" package="com.haken.qrcode" class="android.widget.LinearLayout" text="" resource-id="com.haken.qrcode:id/action_bar_root" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,2088]" displayed="true">

          <android.widget.FrameLayout index="0" package="com.haken.qrcode" class="android.widget.FrameLayout" text="" resource-id="android:id/content" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,2088]" displayed="true">

            <android.widget.RelativeLayout index="0" package="com.haken.qrcode" class="android.widget.RelativeLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,2088]" displayed="true">

              <android.widget.RelativeLayout index="0" package="com.haken.qrcode" class="android.widget.RelativeLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,2088]" displayed="true">

                <android.widget.FrameLayout index="0" package="com.haken.qrcode" class="android.widget.FrameLayout" text="" resource-id="com.haken.qrcode:id/layout_fragment" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,1964]" displayed="true">

                  <android.widget.RelativeLayout index="0" package="com.haken.qrcode" class="android.widget.RelativeLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,1964]" displayed="true">

                    <android.widget.LinearLayout index="0" package="com.haken.qrcode" class="android.widget.LinearLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,1964]" displayed="true">

                      <android.widget.RelativeLayout index="0" package="com.haken.qrcode" class="android.widget.RelativeLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,204]" displayed="true">

                        <android.widget.RelativeLayout index="0" package="com.haken.qrcode" class="android.widget.RelativeLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,66][1080,204]" displayed="true">

                          <android.widget.TextView index="0" package="com.haken.qrcode" class="android.widget.TextView" text="Scan Qr" resource-id="com.haken.qrcode:id/txt_menu" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[470,104][609,166]" displayed="true" />

                          <android.widget.ImageView index="1" package="com.haken.qrcode" class="android.widget.ImageView" text="" resource-id="com.haken.qrcode:id/image_add" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[956,87][1052,183]" displayed="true" />

                        </android.widget.RelativeLayout>

                      </android.widget.RelativeLayout>

                      <android.view.ViewGroup index="1" package="com.haken.qrcode" class="android.view.ViewGroup" text="" resource-id="com.haken.qrcode:id/scanner_view" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,204][1080,1964]" displayed="true">

                        <android.view.View index="0" package="com.haken.qrcode" class="android.view.View" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,204][1080,1964]" displayed="true" />

                        <android.view.View index="1" package="com.haken.qrcode" class="android.view.View" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,204][1080,1964]" displayed="true" />

                        <android.widget.ImageView index="2" package="com.haken.qrcode" class="android.widget.ImageView" text="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,204][154,358]" displayed="true" />

                        <android.widget.ImageView index="3" package="com.haken.qrcode" class="android.widget.ImageView" text="" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[926,204][1080,358]" displayed="true" />

                      </android.view.ViewGroup>

                    </android.widget.LinearLayout>

                  </android.widget.RelativeLayout>

                </android.widget.FrameLayout>

                <android.widget.LinearLayout index="1" package="com.haken.qrcode" class="android.widget.LinearLayout" text="" resource-id="com.haken.qrcode:id/layout_bottom" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,1964][1080,2088]" displayed="true">

                  <android.widget.LinearLayout index="0" package="com.haken.qrcode" class="android.widget.LinearLayout" text="" resource-id="com.haken.qrcode:id/layout_home" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,1964][360,2088]" displayed="true">

                    <android.widget.ImageView index="0" package="com.haken.qrcode" class="android.widget.ImageView" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[159,1978][200,2019]" displayed="true" />

                    <android.widget.TextView index="1" package="com.haken.qrcode" class="android.widget.TextView" text="Home" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[148,2033][212,2073]" displayed="true" />

                  </android.widget.LinearLayout>

                  <android.widget.LinearLayout index="1" package="com.haken.qrcode" class="android.widget.LinearLayout" text="" resource-id="com.haken.qrcode:id/layout_scan" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[360,1964][720,2088]" displayed="true">

                    <android.widget.ImageView index="0" package="com.haken.qrcode" class="android.widget.ImageView" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[519,1978][560,2019]" displayed="true" />

                    <android.widget.TextView index="1" package="com.haken.qrcode" class="android.widget.TextView" text="Scan" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[514,2033][566,2073]" displayed="true" />

                  </android.widget.LinearLayout>

                  <android.widget.LinearLayout index="2" package="com.haken.qrcode" class="android.widget.LinearLayout" text="" resource-id="com.haken.qrcode:id/layout_history" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[720,1964][1080,2088]" displayed="true">

                    <android.widget.ImageView index="0" package="com.haken.qrcode" class="android.widget.ImageView" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[879,1978][920,2019]" displayed="true" />

                    <android.widget.TextView index="1" package="com.haken.qrcode" class="android.widget.TextView" text="History" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[859,2033][940,2073]" displayed="true" />

                  </android.widget.LinearLayout>

                </android.widget.LinearLayout>

              </android.widget.RelativeLayout>

            </android.widget.RelativeLayout>

          </android.widget.FrameLayout>

        </android.widget.LinearLayout>

      </android.widget.FrameLayout>

    </android.widget.LinearLayout>

  </android.widget.FrameLayout>

</hierarchy>
```

