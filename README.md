![Weather Watcher](src/main/resources/weather_small.png)

# Alexa Weather Watcher
A simple [AWS Lambda](http://aws.amazon.com/lambda) function that demonstrates how to write a skill for the Amazon Echo using the Alexa SDK. This Lambda function is used to power an Alexa Skill fetching the current weather of any city in the UK.

## Word of caution
**⚠  USE AT YOUR OWN RISK! ⚠**

This skill has been published for the educational benefit of anyone wanting to try AWS Lambda or Alexa Skills. This should not be the basis for any production applications.

This project uses the OpenWeatherMap API and all weather conditions are provided by the API. Enjoy the great British summer while it lasts!

## Concepts
This sample shows how to create a Lambda function for handling Alexa Skill requests that communicates with an external web service to get weather data from OpenWeatherMap API (http://openweathermap.org/api)
 - Dialog and Session state: Handles two models, both a one-shot ask and tell model, and a multi-turn dialog model.
   If the user provides an incorrect slot in a one-shot model, it will direct to the dialog model. See the
   examples section for sample interactions of these models.
 - SSML: Using SSML tags to control how Alexa renders the text-to-speech.

## Setup
To run this example skill you need to do two things. The first is to deploy the example code in lambda, and the second is to configure the Alexa skill to use Lambda.

### AWS Lambda Setup
1. Go to the AWS Console and click on the Lambda link. Note: ensure you are in us-east or Ireland, or you wont be able to use Alexa with Lambda.
2. Click on the Create a Lambda Function or Get Started Now button.
3. Skip the blueprint
4. Name the Lambda Function "Weather-Watcher-Skill".
5. Select the runtime as Java 8
6. Go to the the root directory containing pom.xml, and run 'mvn assembly:assembly -DdescriptorId=jar-with-dependencies package'. This will generate a zip file named "weather-watcher-0.1.0-SNAPSHOT-jar-with-dependencies.jar" in the target directory.
7. Select Code entry type as "Upload a .ZIP file" and then upload the "weather-watcher-0.1.0-SNAPSHOT-jar-with-dependencies.jar" file from the build directory to Lambda
8. Set the Handler as weatherwatcher.WeatherWatcherSpeechletRequestStreamHandler (this refers to the Lambda RequestStreamHandler file in the zip).
9. Create a basic execution role and click create.
10. Leave the Advanced settings as the defaults.
11. Click "Next" and review the settings then click "Create Function"
12. Click the "Event Sources" tab and select "Add event source"
13. Set the Event Source type as Alexa Skills kit and Enable it now. Click Submit.
14. Copy the ARN from the top right to be used later in the Alexa Skill Setup.

### Alexa Skill Setup
1. Go to the [Alexa Console](https://developer.amazon.com/edw/home.html) and click Add a New Skill.
2. Set "WeatherWatcher" as the skill name and "weather watcher" as the invocation name, this is what is used to activate your skill. For example you would say: "Alexa, Ask weather watcher what is the current weather in Newcastle upon Tyne."
3. Select the Lambda ARN for the skill Endpoint and paste the ARN copied from above. Click Next.
4. Copy the Intent Schema from the included IntentSchema.json.
5. Copy the Sample Utterances from the included SampleUtterances.txt. Click Next.
6. Go back to the skill Information tab and copy the appId. Paste the appId into the WeatherWatcherSpeechletRequestStreamHandler.java file for the variable supportedApplicationIds,
   then update the lambda source zip file with this change and upload to lambda again, this step makes sure the lambda function only serves request from authorized source.
7. You are now able to start testing your sample skill! You should be able to go to the [Echo webpage](http://echo.amazon.com/#skills) and see your skill enabled.
8. In order to test it, try to say some of the Sample Utterances from the Examples section below.
9. Your skill is now saved and once you are finished testing you can continue to publish your skill.

## Examples
### One-shot model:
  User:  "Alexa, ask Weather Watcher what is the current weather in Newcastle upon Tyne"
  
  Alexa: "It is 18 degree celsius and clear sky in Newcastle upon Tyne."
### Dialog model:
  User:  "Alexa, open Weather Watcher"
  
  Alexa: "Welcome to Weather Watcher. I can provide the current weather for any city. Which city would you like current weather for?"
  
  User:  "Newcastle upon Tyne"
  
  Alexa: "It is 18 degree celsius and clear sky in Newcastle upon Tyne."
