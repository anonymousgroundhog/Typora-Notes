# Psuedocode 10/17/22

Schedule meeting with Zhiming about open projects to work on.

## Generating Button Clicks Pseudocode

```markdown
generate empty dataframe with columns app_name and button
Generate xml file of app view
    for line in xml file
        if line contains clickable = true
        	add resource-id value to dataframe column button and add app name to app_name column
```



| app_name |             button              |
| :------: | :-----------------------------: |
|  qrcode  | com.haken.qrcode:id/layout_scan |
|  qrcode  |  com.haken.qrcode:id/image_add  |

### Idea for Analysis

```flow
st=>start: APK Start Analysis
op=>operation: Extract App XML
op2=>operation: Extract Button ID Values
cond=>condition: Contains buttons (Yes or No)?
opInstrument=>operation: Instrument button click
e=>end

st->op->op2->cond->opInstrument->e
cond(yes)->opInstrument
cond(no)->e
```

## Python Code

### Handle Permissions

```python
permission_buttons_to_click = []
file = open("XML\\qrcode.xml", "r")
Lines = file.readlines()
for line in Lines:
    line = re.sub(' +', ' ',line)
    if 'text="Allow' in line or 'text="ALLOW' in line:
        for item in line.split(" "):
            if 'resource-id' in item:
                # print("resource-id:"+item.strip("resource-id="))
                permission_buttons_to_click.append(item.strip("resource-id=").strip('"'))
```



### Handle Button clicks

```
source_xml = driver.page_source

file = open("XML\\qrcode_main.xml", "w")
file.write(source_xml)
file.close()

buttons_to_click = []
file = open("XML\\qrcode_main.xml", "r")
Lines = file.readlines()

for line in Lines:
    line = re.sub(' +', ' ',line)
    if 'clickable="true' in line:
        for item in line.split(" "):
            if 'resource-id' in item:
                buttons_to_click.append(item.strip("resource-id=").strip('"'))

print(buttons_to_click)

for id in reversed(buttons_to_click):
    if check_exists_by_id(driver, id):
        print(id)
        click_on_button_by_id(driver, id)
        time.sleep(1)
        break

driver.remove_app("com.haken.qrcode")
```

### Full Code

```python
import time, re,os, pandas as pd
from appium import webdriver
from selenium.webdriver.common.by import By
from selenium.common.exceptions import NoSuchElementException
#from pyaxmlparser import APK

def check_exists_by_id(driver,id):
    if (len(driver.find_elements(By.ID, id)) > 0):
        return True
    
    return False
def check_if_button_is_displayed(driver, id):
    element = driver.find_element(By.ID, id)
    if element.is_displayed():
        return True
    return False

def click_on_button_by_id(driver,id):
    driver.find_element(By.ID, id).click()

def set_driver(desired_capabilities):
    return webdriver.Remote("http://127.0.0.1:4723/wd/hub", desired_capabilities)

desired_capabilities = {
    "platformName": "Android",
    "deviceName": "emulator-5554",
    "appPackage": "com.haken.qrcode",
    "noReset": True,
    "autoacceptalerts": True,
    "autoGrantPermissions": True,
    "appActivity": "com.haken.qrcode.ActivityUtil.Splash"
}
#--------------------------------------Install App--------------------------------------
os.system("cls")
print(os.system("adb install ..\\PowerShellScripts\\SignedAPKFiles\\Signed0BD763DC4D6974EE1DC2B8E5904923BC96FBF5FB4013AC74EAB33AC0DA4E4C55.apk"))
#--------------------------------------SET DRIVER--------------------------------------
driver = set_driver(desired_capabilities)
df = pd.DataFrame(columns = ['AppName', 'Interface', 'ID', "Clicked"])
# driver.install_app('..\\PowerShellScripts\\SignedAPKFiles\\Signed0BD763DC4D6974EE1DC2B8E5904923BC96FBF5FB4013AC74EAB33AC0DA4E4C55.apk')
#--------------------------------------SET PERMISSIONS FOR APP--------------------------------------
time.sleep(1)
source_xml = driver.page_source

file = open("XML\\qrcode.xml", "w")
file.write(source_xml)
file.close()

permission_buttons_to_click = []
file = open("XML\\qrcode.xml", "r")
Lines = file.readlines()
for line in Lines:
    line = re.sub(' +', ' ',line)
    # if ('permission_allow_foreground_only_button' in line or 'Allow' in line or 'ALLOW' in line) and 'clickable="true"' in line:
    if 'permission_allow_foreground_only_button' in line or 'Allow' in line or 'ALLOW' in line:
        for item in line.split(" "):
            if 'resource-id' in item:
                # print("resource-id:"+item.strip("resource-id="))
                permission_buttons_to_click.append(item.strip("resource-id=").strip('"'))
                df = df.append({'AppName' : 'qrcode', 'Interface' : 'Start', 'ID' : item.strip("resource-id=").strip('"'), 'Clicked' : False},ignore_index = True)
file.close()

print("Button Permissions:"+ str(permission_buttons_to_click))
clicked = 0
permission_buttons_clicked_on = []
while clicked <= len(permission_buttons_to_click):
    for id in reversed(permission_buttons_to_click):
        if check_exists_by_id(driver, id):
            print("ID:"+id)
            click_on_button_by_id(driver, id)
            df.loc[df["ID"] == id, "Clicked"] = True
            clicked +=1
            print("clicked:" + str(clicked))
            time.sleep(1)

#--------------------------------------Clicking Events Start HERE--------------------------------------
time.sleep(2)
file = open("XML\\qrcode_main.xml", "w")
file.write(driver.page_source)
file.close()

buttons_to_click = []
file = open("XML\\qrcode_main.xml", "r")
Lines = file.readlines()

for line in Lines:
    line = re.sub(' +', ' ',line)
    if 'clickable="true' in line:
        print("clickable:"+line)
        for item in line.split(" "):
            if 'resource-id' in item:
                buttons_to_click.append(item.strip("resource-id=").strip('"'))
                df = df.append({'AppName' : 'qrcode', 'Interface' : 'Main', 'ID' : item.strip("resource-id=").strip('"'), 'Clicked' : False},ignore_index = True)

print("Buttons:"+str(buttons_to_click))

for id in reversed(buttons_to_click):
    if check_exists_by_id(driver, id):
        print(id)
        click_on_button_by_id(driver, id)
        df.loc[df["ID"] == id, "Clicked"] = True
        time.sleep(1)
        break
print(df)
driver.remove_app("com.haken.qrcode")
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

