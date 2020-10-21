# Documentation on New Automation Testing Structures 


Under the com.wsl.noom folder we have now four sub packages and one java file as follows:

* baseTest
* commonFlows
* tests
* variables
* MainActivityTest.java

## baseTest

In the baseTest we have Action.java & Validate.java files.

#### Validate.java

In the Validate.java file we have some polymorphism methods and some other methods for checking assertion of element or an object in the view.
For example: We have to check an element in the view is exist or not for that we are using method like,

 ```JAVA
public static void checkAssertion(int resourceId){
   try {
       onView(Matchers.allOf(withId(resourceId))).check(matches(withId(resourceId)));
   } catch (NoMatchingViewException e) {
       onView(Matchers.allOf(withId(resourceId))).check(doesNotExist());
   }
}
 ```

#### Action.java

In the above code we are taking parameters as Resource Id of an element or object. Here we used try/catch, if the element is not in the view. In try we are validating the element with espresso code, if it's in the view it will go to the next task & its not in the view it will go to the catch. In catch we are assuring the element is not in the view and moving to the next task.
 

In the Action.java file we have some methods for doing some actions on the views for automation testing. 
For example: We have to click a button in the view for that we are using method like 

 ```Java
public static void clickElement(int resourceId) {
   checkAssertion(resourceId);
   onView(withId(resourceId)).perform(click());
}
 ```

We are taking parameters as Resource Id of an element or object. Here we are checking the element is in the view by using resource ID, for that we used checkAssertion method of Validate file & clicking the element using Resource ID with Espresso codes.
Also we used polymorphism techniques in the methods. Some methods have the same name with different parameters.


## commonFlows

In the commonFlows we have CommonValidationFlows.java & CommonActionFlows.java files.

#### CommonValidationFlows.java

In the commonValidationFlows.java file we had some String & Integer variables. In some tests we have some common assertions or validations of some elements or objects. For that we have written some methods in this file so we can use these same methods in the different tests.

For example: Checking assertions of Meal Titles in “Log your meals Card”. 

 ```Java
public static void mealLoggingScreen() {
   Validate.checkAssertion(breakFastRow);
   Validate.checkAssertion(lunchRow);
   Validate.checkAssertion(dinnerRow);
   Validate.checkAssertion(analysisButton);
   Validate.checkAssertion(finishDayButton);
}
 ```
We have some tests where we have to check the meal titles everytime we test. We are using the checkAssertion() method of the validation file. We are using parameters Strings which are declared in this file as i.e 
 ```Java
private static String breakFastRow = "Breakfast"; .
 ```
#### CommonActionFlows.java

In the commonActionFlows.java file we had some String & Integer variables. In some tests we have some common actions or tasks to do. For that we have written some methods in this file so we can use these same methods in the different tests.

For example: Clicking “Log your meals Card” 
 ```Java
public static void goToMealBeforeLoggedTask() throws UiObjectNotFoundException {
   UiScrollable scrollTo = new UiScrollable(new UiSelector().scrollable(true));
   scrollTo.scrollTextIntoView(mealLoggingTaskName);
   Action.clickElement(mealLoggingTaskName);
}
 ```

We have some tests where we have to click the meal task card everytime we test. In this method we are using Ui Automator to scroll to the specific view, then we are clicking the element using the clickElement method of Action.java file. We are using parameters Strings which are declared in this file as i.e 
 ```Java
private static String mealLoggingTaskName = "Log your meals";
 ```

## helpers

In the helpers we have ElapsedTimeIdlingResource.java & VerifyingElement.java files.

In this package we store all our helper methods like waiting for a view, extracting text from an element using resource ID, extracting integer from a text etc.

For example: Converting a text into Integer.
 ```Java
public static int textToInt(String Text) {
   String calorieTextToInt = Text.replaceAll("[^0-9]", "");
   int textInt = Integer.parseInt(calorieTextToInt);
   return textInt;
}
 ```
In some tests we have to verify or calculate some values which are shown as text. We extract the texts from the element then convert the text into integers using the above method for later calculation. In the TextToInt() method we are taking parameters as String converting it to integer using java native methods.

## tests

In the tests package we have all the test files. In every test file’s beginning we launch the app. In methods we wrote all the assertions or actions of the UI for the test.

For example, In login.java test file:
 ```Java
 /** 
 * Landing Page 
 */
 Validate.checkAssertion(loginButtonID, 5000);
 Action.clickElement(loginButtonID);
 ```
By using  ```JavaValidate.checkAssertion(loginButtonID, 5000); ``` we are waiting for 5 seconds in the test for the login button to load in the view and checking the assertion of that button. In the next line we are clicking that button from Action.java file’s method. loginButtonID is a variable which is declared in Login_WebBuyFlow_Vars.java file as an integer.


## variables

In the variables package we have all the variables files. All the Resource Ids, Texts, Xpaths, String Resource Ids are declared here in the files, Which are only accessible from the test files which are only extended these variable files.

For example,  we declared variables as follows:
 ```Java
protected static String navContentTextHome = "Home";
protected static int chatShortcutID= R.id.chat_shortcut;
protected static String deviceShareTitleID = "android:id/title";
 ```

## MainActivityTest

Here we run all the test files which are all located in the tests package as instrumentation tests.
