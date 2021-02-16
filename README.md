# Documentation on Automation Testing Structures in Kotlin Language 
 
## The features that used in this kotlin testing structures in summary
* We didn't throw any Exception in any class or methods, in java which were mandatory to declare. For i.e
```kotlin
fun pressDeviceBackButton() = UiDevice.getInstance(InstrumentationRegistry.getInstrumentation()).pressKeyCode(KeyEvent.KEYCODE_BACK)
```
For every ui automator test we had to throw ```UiObjectNotFoundException``` in java. But here we don't need to check the exception.

* All the methods, variables are declared in Companion object for using these as static type in other classes like java. for i.e
```kotlin
class Action {
    companion object {
        private val Device: UiDevice = UiDevice.getInstance(InstrumentationRegistry.getInstrumentation())
        private val scrollTo = UiScrollable(UiSelector().scrollable(true))
        fun clickElement(resourceId: Int) = onView(withId(resourceId)).perform(click())
    }
}
```
We can access the method ```clickElement``` in the other classes without creating instances of ```Action``` class.
 
* All the variables we declared as val, in java we had to declare variable type. For i.e
```kotlin
    internal const val addMoreItems: String = "Add more items"
    internal const val foodItemSearchBox: Int = R.id.food_logging_search_search_box
```
Though here we declared the type explicitly for understanding which is which, because we are using both espresso & uiautomator testing tools for test cases.

* We used in-line/block methods in some cases for eliminating runtime overhead or memory overhead. For examples:
```kotlin
fun clickElement(text: String) = onView(withText(text)).perform(click())
```
We used this functionality in many other classes & in files, which also reduces the boilerplate of codes.

* In every file we call other classes's method by importing the classes's specific method as companion object at top level declaration rather than importing the whole class.
```kotlin
import com.wsl.noom.baseTest.Validate.Companion.checkAssertion
```
For using this method we have to write code like this one ```checkAssertion(availableCalorieID)```.
 
* For using variables in the tests we imported the variables as we imported the methods from different classes's. For i.e
```kotlin
import com.wsl.noom.variables.CommonVariables.Companion.availableCalorieID
```       
## Project Structure 
Under the com.wsl.noom package we have now five sub packages and  test suites files as follows:
 
 * baseTestAVI
 * baseTests
 * commonFlows
 * helpers
 * variables
 * DayOneUserTestSuite.kt
 * ExistingUserTestSuite.kt
 * GermanUserTestSuite.kt
 * PreDPDUserTestSuite.kt
 * SpanishUserTestSuite.kt
 * WinBackUserUserTestSuite.kt
 
## baseTestAVI
 
In the baseTest we have ```Action.kt``` & ```Validate.kt``` files.
 
#### Validate.kt
 
 In the ```Validate.kt``` file we have some polymorphism methods and some other methods for checking assertion of element or an object in the view.
 For example: We have to check an element in the view is exist or not for that we are using method like,
 
```Kotlin
    fun checkAssertion(resourceId: Int) {
        try {
            onView(withId(resourceId)).check(matches(withId(resourceId)))
            } catch (e: NoMatchingViewException) {
            onView(withId(resourceId)).check(doesNotExist())
            }
        }
```
 
#### Action.kt
 
In the above code we are taking parameters as Resource Id of an element or object. Here we used try/catch, if the element is not in the view. In try we are validating the element with espresso code, if it's in the view it will go to the next task & its not in the view it will go to the catch. In catch we are assuring the element is not in the view and moving to the next task.
  
In the ```Action.kt``` file we have some methods for doing some actions on the views for automation testing. 
For example: We have to click a button in the view for that we are using method like ```clickElement(resourceId: Int)``` which is written as in-line method of kotlin.
 
```kotlin
fun clickElement(resourceId: Int) = onView(withId(resourceId)).perform(click())
```
 
We are taking parameters as Resource Id of an element or object. Here we are checking the element is in the view by using resource ID, for that we used checkAssertion method of Validate file & clicking the element using Resource ID with Espresso codes.
Also we used polymorphism techniques in the methods. Some methods have the same name with different parameters.
 
 
## commonFlows
 
In the commonFlows we have some common flows packages like ```commonFoodLoggingFlows, CommonValidationFlows``` etc. In each of every common flows package consists of two files like ```commonFoodLoggingFlows.kt & commonFoodLoggingFlowsVars.kt``` in ```commonFoodLoggingFlows``` package. ```commonFoodLoggingFlows.kt``` has some common methods that are being used in food logging tests for i.e 

```kotlin
fun mealLoggingOverviewScreen(breakfast: String, lunch: String, dinner: String) {
    checkAssertion(dateId)
    checkAssertion(breakfast)
    checkAssertion(lunch)
    checkAssertion(dinner)
    checkAssertion(analysisButtonId)
    checkAssertion(finishDayButtonId)
  }
```
In every food logging tests we have to assert meal logging overview screen rather than writing same codes in every tests we put these codes in this ```mealLoggingOverviewScreen()``` method for reusing in the food logging tests & reducing the boilerplate of codes.

```commonFoodLoggingFlowsVars.kt``` has variables which are only used in ```commonFoodLoggingFlows.kt``` file.
 
## helpers
 
In the helpers we have ```TestRails``` package and helper files as follows:

 * TestRails
 * APIMethods.kt
 * ChangeDateTime.kt
 * ElapsedTimeIdlingResources.kt
 * TryCatch.kt
 * VerifyingElement.kt
 
#### TestRails

TestRails package has classes, files which are for intregating with tests with TestRails like connecting with TestRails API, Connecting to TestRails website, sending test results, getting test results and other stuffs. Details are in TestRails Integration section. 

In this package we store all our helper methods like waiting for a view, extracting text from an element using resource ID, extracting integer from a text etc.
 
#### APIMethods.kt 

In ```APIMethods.kt``` file we put all of our API methods for sending GET/POST request to Noom's various server. For i.e getting UPID from Noom Server.

#### ChangeDateTime.kt

ChangeDateTime class is for changing the device date/time using UiAutomator. Here we launch the device's Date Settings option from Settings using Application Context & Intent, enable manual date/time then change date/time using UiAutomator as desired. In ChangeDateTime class we have implemented some methods like ```openDateSettings(),deviceNextDay(), getCurrentDeviceDate(), selectTargetDay(), selectTargetMonth()``` & other method for changing device's date time.

#### ElapsedTimeIdlingResources.kt

This class is used in ```Validate.kt``` file to waiting for a view/element. This class is implemented on IdlingResource class of Espresso's library, it wait for a view/element for specific given time, register the element for IdlingResource and check for element is idle or not, when it's idle unregister the element.   

#### TryCatch.kt

Here we put all of our try/catch block codes that are used in tests for tracking how many try/catch block we are using in the tests.

#### VerifyingElement.kt

In some tests we have to verify or calculate some values which are shown as text or UI elemnt. For example: Validating Calorie budget.

```kotlin
fun validatingCalorieBudgetAfterLoggingAMeal(caloriesRemainingBeforeLoggingMeal: Int, calorieOfAnItemText: String, updatedCalorieBudget: Int): Boolean {
    val calorieOfAnItem: Int = textToInt(calorieOfAnItemText)
    val remainingCalorie: Int = updatedCalorieBudget - calorieOfAnItem
    return remainingCalorie == caloriesRemainingBeforeLoggingMeal
  }
```
 
## baseTests
 
In the tests package we have all the test files in different packages for different users. Each test is in specific package which consists of a test file & a variable file. In every test file’s beginning we launch the app. In methods we wrote all the assertions or actions of the UI for the test.
 
For example, In ```login.kt``` test file:
```kotlin
   checkAssertion(trackingManuallyButton, 40000)
   clickElement(trackingManuallyButton)
```
By using ```checkAssertion(trackingManuallyButton, 40000)``` we are waiting for 40 seconds in the test for the Tracking manually button to load in the view and checking the assertion of that button. In the next line we are clicking that button from ```Action.kt``` file’s method. trackingManuallyButton is a variable which is declared in ```CommonVariables.kt``` file in ```LoginSignUpCommonVars``` class as an integer.
 
 
## variables
 
In the variables package we a CommonVariables.kt file which consist of multiple classes, within those classes we put common variables for specific tests files. All the Resource Ids, Texts, Xpaths, String Resource Ids are declared here in the files, Which are declared in companion object.
 
For example, we declared variables as follows in class:
```kotlin
    class LoginSignUpCommonVars {
        companion object {
            internal const val passwordTyped: String = "test123"
            internal const val loginButtonID: Int = R.id.btn_login
            internal const val loginWithEmailButtonID: Int = R.id.btn_email_login
            internal const val trackingManuallyButton: Int = R.id.simple_dialog_negative_button
        }
    }
```
 
## MainActivityTest
 
Here we run all the test files which are all located in the tests package as instrumentation tests.
