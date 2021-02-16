# Documentation on New Automation Testing Structures in Kotlin Language 
   
## Project Structure 
Under the ```com.wsl.noom package``` we have now five sub packages and test suites files as follows:
 
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
 
In the tests package we have all the tests files in different packages like Login Test in Login package. Each test is in specific package which consists of a test file & a variable file. In methods we wrote all the assertions/actions as we can resuse the code for different language users. As before we wrote same test files & codes in other packages for different user. Now we only create assertions/actions & test file only one time for all users.  
 
For example, In ```skipMealTest.kt``` test file:
```kotlin
fun skipMealTest(language: String) {
    goToMealLoggingTaskAndMealOverviewScreen(language)
    when (language) {
        "En" -> commonTasksOfSkipAMealTest(dinnerRowTitle, doneButton)
        "Es" -> commonTasksOfSkipAMealTest(esDinnerRowTitle, esDoneButton)
        "De" -> commonTasksOfSkipAMealTest(deDinnerRowTitle, deDoneButton)
    }
}

private fun commonTasksOfSkipAMealTest(mealTitle: String, doneButton: String) {
   clickElement(mealTitle)
   checkAssertion(mealTitle)
   checkingItemSearchingScreen(doneButton)
   checkingMealSkippedAndSkippingMeal(doneButton)
}
```
Here we are using ```skipMealTest(language: String)``` method for Testing Skipping Meal Test for different language user, just passing parameter as language identifier like "En" from different user's test suites but using same codes. ```goToMealLoggingTaskAndMealOverviewScreen(language)``` is a method of ```CommonFoodLoggingFlows.kt``` file, in this method we are also sending language parameter so it'll work as language based & with ```when()``` functionality we're checking which language then calling the ``` commonTasksOfSkipAMealTest(mealTitle: String, doneButton: String)``` method, In parameter we are using variables based on language. 
 
## variables
 
In the variables package we a ```CommonVariables.kt``` file which consist of multiple classes, within those classes we put common variables for specific tests files. All the Resource Ids, Texts, Xpaths, String Resource Ids are declared here in the files, Which are declared in companion object.
 
For example, we declared variables as follows in class:
```kotlin
class LoginSignUpCommonVars {
    companion object {
        internal const val loginButtonID: Int = R.id.btn_login
        internal const val loginWithEmailButtonID: Int = R.id.btn_email_login
        internal const val trackingManuallyButton: Int = R.id.simple_dialog_negative_button
    }
}
```
 
### DayOneUserTestSuite.kt
 
All the tests of newly created user's are declared here, language is set to English. 

### ExistingUserTestSuite.kt
 
All the tests of Existing user's are declared here, language is set to English. 

### GermanUserTestSuite.kt
 
All the tests of German language user's are declared here, language is set to German. 

### PreDPDUserTestSuite.kt
 
All the tests of PreDPD user's are declared here, language is set to English. 

### SpanishUserTestSuite.kt
 
All the tests of Spanish language user's are declared here, language is set to Spanish. 

### WinBackUserUserTestSuite.kt
 
All the tests of newly Win Back user's are declared here, language is set to English. 


## The features of kotlin used in this testing structures are in summary
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

## TestRails Integration

In ```com.wsl.noom.helpers``` we've created a package as ```TestRails```. This package has the following files:

* TestRail.kt
* TestRailsAPIClient.kt
* TestRailsAPIException.kt
* TestRailsHelper.kt
* TestResult.kt

#### Dependency
In gradle file below lines in ```dependecies``` section because TestRails API has external depency on json-simple:

```implementation 'com.googlecode.json-simple:json-simple:1.1.1'```

We added below lines before ```dependecies``` section bcause it'll solve the error of ```Duplicate file found of org.hacrest:hamcrest-core:1.1```
```
configurations.all {
   	resolutionStrategy.dependencySubstitution {
       	substitute module('org.hamcrest:hamcrest-core:1.1') with module('junit:junit:4.10')
   }
}

```        
#### TestRail.kt

This is a custom annotation class file, later we'll use this before every ```@Test``` annotation as ```@TestRail(id = "")``` for specify particular tests for reporting test status to test rail & assosiationg tests with test cases of TestRail. For i.e

```kotlin
@TestRail(id = "1")
@Test
fun login() = loginTest()
```

#### TestRailsAPIClient.kt & TestRailsAPIException.kt

These two files are downloaded from TestRail [website](https://www.gurock.com/testrail/docs/api/getting-started/binding-java). These were java file, we converted them to kotlin file. Functionalty of ```TestRailsAPIClient.kt``` to send GET/POST request to TestRail API. TestRail's GET requests are used by read-only API methods and can be used to read or query data from TestRail & TestRail's POST requests are used by write-based API methods and can be used to add or modify data. ```TestRailsAPIException.kt``` is used for catch error message of API calls & throwing them. 

#### TestRailsHelper.kt

First we declared TestRail's website, user, password in ```createSuite()``` method with instance of TestRailsAPIClient class. In ```TestRailsHelper.kt``` file, ```createSuite()``` method is for creating a connection with TestRail website using POST request of TestRailsAPIClient class & add a new run for Testrail project. ```beforeTest()``` method is for getting test methods name & update case id for Testrail which method has case id with the TestRail id.
```hasAnnotations()``` is a boolean method to identify which test method is associated with @TestRail annotation.

#### TestResult.kt

This file is for update the test status & after finishing a test & sending result to TestRail via Testrail's API. This class is inherited from TestWatcher class. This class override three methods of TestWatcher's class. ```succeded(), failed()```methods are for updating status id of each test cases for TestRail. ```finished()``` method gets called when the test finishes, we're sending test result to TestRail in this method using POST request of TestRailsAPIClient class.

#### Test Suite

In test suite we've added these lines for complete the TestRail Integration:

```kotlin
class TestSuite {
    companion object {
        @Rule @JvmField var testNames: TestName = TestName()
        @BeforeClass @JvmStatic fun createSuites() = createSuite()
        @Rule @JvmField var activityTestRule =
                ActivityTestRule(NoomLaunchActivity::class.java)
    }

    @Before fun beforeTests() = beforeTest()

    @Rule @JvmField var watchman: TestRule = TestResult()

    @TestRail(id = "1")
    @Test
    fun login() = loginTest()
}
```    
In companion object first ```@Rule``` declared a TestName class's instances to run the class & get all the test methods name of the Test Suite. With ```@BeforeClass``` we run the TestRailsHelper class's ```createSuite()``` method for established a connection with testrail & create a run in Testrail before running any tests. Second ```@Rule``` we launch the app. In ```@Before``` we run TestRailsHelper class's ```beforeTest()``` method for updating case id from which methods are specified with Testrail id.
In third ```@Rule``` we run TestResults script as TestRule object for updating test status & sending the result to Testrail api for updating in the run.


## Specify TestSuite in Bitrise

For running a specific test suite in Bitrise: In ```[BETA]Virtual Device Testing for Android``` task of workflows, we have to set class name in Test targets of Instrumentation Test section. For i.e we want to run ```SpanishUserTestSuite.kt``` we've to write code in the Test targets field as it is:
```
class com.wsl.noom.SpanishUserTestSuite
```
For running multiple test suites in One build in Bitrise:  In ```[BETA]Virtual Device Testing for Android``` task of workflows, we have to set class names in Test targets of Instrumentation Test section. For i.e we want to run ```SpanishUserTestSuite.kt``` & ```GermanUserTestSuite.kt``` we've to write code in the Test targets field as it is:
```
class com.wsl.noom.SpanishUserTestSuite, class com.wsl.noom.GermanUserTestSuite
```
