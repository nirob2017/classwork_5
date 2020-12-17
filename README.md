# espresso_automation
# Documentation on Automation Testing Structures in Kotlin Language 
 
## The features that used in this kotlin testing structures
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

* In every file we call other classes's method by importing the classes's specific method as companion object at top level declaration.
```kotlin
import com.wsl.noom.baseTest.Validate.Companion.checkAssertion
```
For using this method we have to write code like this one ```checkAssertion(availableCalorieID)```.
 
* For using variables in the tests we imported the variables as we imported the methods from different classes's. For i.e
```kotlin
import com.wsl.noom.variables.CommonVariables.Companion.availableCalorieID
```       
 
Under the com.wsl.noom folder we have now four sub packages and one kotlin file as follows:
 
 * baseTest
 * commonFlows
 * tests
 * variables
 * MainActivityTest.kt
 
## baseTest
 
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
 
In the commonFlows we have ```CommonValidationFlows.kt``` & ```CommonActionFlows.kt``` files.
 
#### CommonValidationFlows.kt
 
In the commonValidationFlows.java file we had some String & Integer variables. In some tests we have some common assertions or validations of some elements or objects. For that we have written some methods in this file so we can use these same methods in the different tests.
 
For example: Checking assertions of Meal Titles in “Log your meals Card”. 
 
```kotlin
    fun mealLoggingScreen() {
        checkAssertion(breakfastRowTitle)
        checkAssertion(lunchRowTitle)
        checkAssertion(dinnerRowTitle)
        checkAssertion(analysisButton)
        checkAssertion(finishDayButton)
    }
```
We have some tests where we have to check the meal titles everytime we test. We are using the checkAssertion() method of the validation file. We are using parameters Strings which are imported from ```variables\CommonVariables.kt``` as companion object.
```kotlin
import com.wsl.noom.variables.CommonVariables.Companion.FoodLoggingCommonVars.Companion.breakfastRowTitle
```
#### CommonActionFlows.kt
 
In the ```commonActionFlows.kt``` file we had some String & Integer variables. In some tests we have some common actions or tasks to do. For that we have written some methods in this file so we can use these same methods in the different tests.
 
For example: Clicking ```Log your meals``` Card 
```kotlin
    fun goToMealLoggingTask() {
        try {
            goToMealLoggingTaskBeforeMealLogged()
            mealLoggingScreen()
            } catch (e: NoMatchingViewException) {
            goToMealLoggingTaskAfterLoggedMeal()
            mealLoggingScreen()
            }
    }
```
 
We have some tests where we have to click the meal task card everytime we test. In this method we are using Ui Automator to scroll to the specific view, then we are clicking the element using the clickElement method of ```Action.kt file```. We are using parameters Strings which are declared in this file as i.e 
```kotlin
private const val mealLoggingTaskName: String = "Log your meals"
```
 
## helpers
 
In the helpers we have ```ElapsedTimeIdlingResource.kt``` & ```VerifyingElement.kt``` files.
 
In this package we store all our helper methods like waiting for a view, extracting text from an element using resource ID, extracting integer from a text etc.
 
For example: Converting a text into Integer.
```kotlin
    private fun textToInt(Text: String): Int {
        val calorieTextToInt = Text.replace("[^0-9]".toRegex(), "")
        return calorieTextToInt.toInt()
    }
```
In some tests we have to verify or calculate some values which are shown as text. We extract the texts from the element then convert the text into integers using the above method for later calculation. In the TextToInt() method we are taking parameters as String converting it to integer using java native methods.
 
## tests
 
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
