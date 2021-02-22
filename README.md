# PAY.CARDS RECOGNIZER

Automatic recognition of bank card data using built-in camera on Android devices.

See live demo: [pay.cards](https://play.google.com/store/apps/details?id=cards.pay.demo)

### Installation

Create a new file in the root directory of the project named ```github.properties```.

```
gpr.usr = omiselabs
gpr.key = SPECIFY_YOUR_TOKEN_HERE_WITH_READ_PACKAGES_PERMISSION
```

Then in module ```build.gradle``` file, add the following,

```
def githubProperties = new Properties()
githubProperties.load(new FileInputStream(rootProject.file("github.properties")))
repositories {
    maven {
        name = "GitHubPackages"
        url = uri("https://maven.pkg.github.com/opn-ooo/PayCards_Android")
        credentials {
            username = githubProperties['gpr.usr'] ?: System.getenv("GPR_USER")
            password = githubProperties['gpr.key'] ?: System.getenv("GPR_API_KEY")
        }
    }
}
```

Then inside ```dependencies``` block add,

```
dependencies {
    ...
    implementation "ooo.opn:paycards-android:1.0.0"
}
```

### Usage

Build an Intent using the `ScanCardIntent.Builder` and start a new activity to perform the scan:


```java
static final int REQUEST_CODE_SCAN_CARD = 1;
 
...
     
private void scanCard() {
  Intent intent = new ScanCardIntent.Builder(this).build();
  startActivityForResult(intent, REQUEST_CODE_SCAN_CARD);
}
```

Handle the result:

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == REQUEST_CODE_SCAN_CARD) {
        if (resultCode == Activity.RESULT_OK) {
            Card card = data.getParcelableExtra(ScanCardIntent.RESULT_PAYCARDS_CARD);
            String cardData = "Card number: " + card.getCardNumberRedacted() + "\n"
                            + "Card holder: " + card.getCardHolderName() + "\n"
                            + "Card expiration date: " + card.getExpirationDate();
            Log.i(TAG, "Card info: " + cardData);
        } else if (resultCode == Activity.RESULT_CANCELED) {
            Log.i(TAG, "Scan canceled");
        } else {
            Log.i(TAG, "Scan failed");
        }
    }
}
```

### License

```
MIT License

Copyright (c) 2017 faceterteam

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
 
The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
 
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
