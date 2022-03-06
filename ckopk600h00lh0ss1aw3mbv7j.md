## Connecting Rasa to a website: A step by step tutorial


> IF AT FIRST YOU DON’T SUCCEED, TRY, TRY AGAIN

Step 1 : **Create and train your bot.** Rasa documentation is your friend in this step.

Step 2 : Once your bot is trained **verify that it is working** by running Rasa shell. You can do that by opening a terminal in your Rasa folder and by running the command

```
rasa shell
``` 

Step 3 : Now comes the interesting part. The integration part. For this there are many options, i.e. you can use Websocket Channel, Rest channel etc. In this tutorial I will be using a Rest channel.

For displaying your chat bot in your website you need a front-end and a method to connect this front-end with your back-end i.e. your Rasa bot.

I used the below GitHub repo as my front-end and the Rest channel was used for connecting my front end to back-end. I recommend you read this article completely before visiting them.

%[https://github.com/scalableminds/chatroom]

after that change your **credentials.yml** file to

```
rest:
  # pass
``` 

after that create a new directory outside your rasa chat-bot folder for your front-end. In that directory create a **index.html** with the following code as the head and body part.

```
<head>
  <link rel="stylesheet" href="https://npm-scalableminds.s3.eu-central-1.amazonaws.com/@scalableminds/chatroom@master/dist/Chatroom.css" />
</head>
<body>
  <div class="chat-container"></div>
  <script src="https://npm-scalableminds.s3.eu-central-1.amazonaws.com/@scalableminds/chatroom@master/dist/Chatroom.js"/></script>
  <script type="text/javascript">
    var chatroom = new window.Chatroom({
      host: "http://localhost:5005",
      title: "Chat with Mike",
      container: document.querySelector(".chat-container"),
      welcomeMessage: "Hi, I am Mike. How may I help you?",
      speechRecognition: "en-US",
      voiceLang: "en-US"
    });
    chatroom.openChat();
  </script>
</body>
``` 

now all the coding part is over next step is to make all of them work in parallel. This is where the port no. comes into the story. Basically we will be trying to run our front-end and back-end on the localhost.

from your **endpoints.yml** file in the rasa chatbot folder keep note of the port no in which your bot is going to run. In my case the **endpoints.yml** file contains the following code.

```
action_endpoint:
url: "http://localhost:5005/webhook"
``` 

so the port number is 5005.

next make sure that this port number is same as the one mentioned in the case of our **index.html** file which we wrote above.

now go into your rasa chat-bot folder and start your bot using the below command in a terminal.

```
python -m rasa run --m ./models --endpoints endpoints.yml --port 5005 -vv --enable-api
``` 

next open another terminal where you saved your **index.html** file and enter the following code to start a local server on the port 8000.

```
python -m http.server 8000
``` 

Now the final task is to open your browser (I recommend Mozilla Firefox) and type in the below code in the URL bar.

```
localhost:8000/index
``` 

Eureka!!! hopefully you got the bot to work. comment if you need any assistance.

note : Do inspect your browser page to verify that there are no errors. You can do this by a right click on your page and selecting inspect page then navigate into the console part, if there are any errors it will show up in there.

👉🏼 checkout my website,  [milindsoorya.site](https://milindsoorya.site/)  for more updates and getting in touch. cheers. Also, the cover image is Photo by  [Kyle Glenn ]("https://unsplash.com/@kylejglenn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText). 
  

Thank you very much for reading, liking and commenting on my articles. As you know, writing quality articles takes time and effort. Therefore, if you have enjoyed my article or if it was helpful please support me by buying me a coffee☕ 😇.