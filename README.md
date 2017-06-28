# Chat Bot no Facebook com o Wit.ai e Heroku usando o node.js
A revolução dos Chat Bots está para ficar e nós não queremos ficar para trás, pois não? :)

## Arquitetura
Basicamente qualquer mensagem enviada através do Facebook é reencaminhada para o Wit.ai e para a API (meteorologia) através do Heroku.

Os dados meteorológicos proveem de uma *3rd party API* disponibilizada pela Yahoo.

![Screenshot](docs/img/chatbot_architecture.png)

## Configurar o Wit.ai
O Wit.ai é uma plataforma para construção de chat bots que foi adquirida pelo Facebook após 18 meses da sua fundação.

1. Criar uma conta no Wit.ai em https://wit.ai

2. Criar uma nova app

![Screenshot](docs/img/wit_create_app.png)

3. Criar uma história

Por enquanto vamos criar uma história de resposta rápida (sim/não)
- Entrar na página "Stories"
- Clicar no botão "Create a story"
- No campo "User says" escrever "Olá"
- No valos do "intent" colocar "greetings"
- Clicar no botão "Bot sends"
- No campo "The bot says..." escrever "Olá, gostas de falar com máquinas?"
- Clicar no botão "Set quick replies"
- No campo "Type a quick reply" escrever "Sim", pressionar a tecla "Enter", escrever "Não" e pressionar a tecla "Enter"
- Clicar no botão "Bot sends"
- Clicar em "Add new entity", escrever "yes_no" e pressionar duas vezes seguidas na tecla "Enter"
- No campo "Value" escrever "yes"
- Clicar no botão "Bot sends"
- No campo "The bot says..." escrever "Boa! Olha, em breve vou saber dizer o tempo."
- Clicar no botão "Não"
- Clicar em "Add new entity" e selecionar a opção "yes_no"
- No campo "Value" escrever "no"
- Clicar no botão "Bot sends"
- No campo "The bot says..." escrever "Ohh que pena :("
- Clicar no botão "Save Story"

![Screenshot](docs/img/wit_01_hello.gif)

4. Localizar e guardar o "Server Access Token"
- Clicar na aba "Settings"
- Na secção "API Details" localizar o campo "Server Access Token"
- Guardar o *token* para ser usado mais tarde no Heroku

![Screenshot](docs/img/wit_server_access_token.png)

## Configurar o Heroku
O Heroku é uma (PaaS - Platform as a Service) que suporta várias linguagens de programação tendo sido adquirida pela Salesforce.com em 2010.  

1. Fazer o *clone* deste repositório para a máquina local

*NOTA: utilizei o PowerShell do Windows 10*
```PowerShell
git clone https://github.com/SandroMiguel/messenger-bot-witai-tutorial witai_facebook
```
![Screenshot](docs/img/git_01_clone.gif)

2. Entrar na pasta do projeto
```PowerShell
cd witai_facebook
```

3. Instalar as dependências
```PowerShell
npm install
```
![Screenshot](docs/img/npm_01_install.gif)

4. Criar uma conta no Heroku em https://heroku.com

5. Enviar a aplicação para o Heroku
- Efetuar o login no Heroku
```PowerShell
heroku login
```
- Criar uma aplicação no Heroku
```PowerShell
heroku apps:create chatbot-sandro
```
- Fazer o *push* do bot para o Heroku (deploy)
```PowerShell
git push heroku master
```
![Screenshot](docs/img/heroku_01_deploy.gif)

6. Definir o *token* do Wit.ai no Heroku
- O *token* fornecido anteriormente no Wit.ai deve ser configurado no Heroku
```PowerShell
heroku config:set WIT_TOKEN="F3PN6IUL574DXE3ENCBQQ4SWCYPQHILL"
```
- Abrir a aplicação no Heroku
```PowerShell
heroku config:set WIT_TOKEN="F3PN6IUL574DXE3ENCBQQ4SWCYPQHILL"
```
![Screenshot](docs/img/heroku_02_wit_token.gif)

- Guardar este URL (ex.: https://chatbot-sandro.herokuapp.com/)

O Heroku já comunica com o Wit.ai !!! 

## Configurar o Facebook
Vamos configurar o Messenger do Facebook para comunicar com o chat bot.

1. Criar uma conta no Facebook Developers em https://developers.facebook.com

2. Criar uma nova página no Facebook em https://www.facebook.com/pages/create
*NOTA: Também pode usar uma página criada anteriormente.*

![Screenshot](docs/img/facebook_01_create_page.png)

3. Criar uma nova app em https://developers.facebook.com/apps

![Screenshot](docs/img/facebook_02_create_app.png)

4. Configurar a app do Facebook
- Adicionar o produto "Facebook Messenger"
- Configurar o Webhooks
	- Callback URL: https://*your-heroku-app-name*.herokuapp.com/**webhooks** (não esquecer do "webhooks" no final)
	- Verify Token: just_do_it
	- Selecionar a página em "Select a page to subscribe your webhook to the page events"

![Screenshot](docs/img/heroku_03_webhooks.gif)

5. Gerar o *token* da aplicação do Facebook
- Clicar em "Settings"
- Selecionar a página
- Guardar o *token* para ser usado mais tarde no Heroku

![Screenshot](docs/img/facebook_03_token_generation.png)

- Subscrever a página na secção "Webhooks"

![Screenshot](docs/img/facebook_03_subscribe_page.png)




2. Go to the settings page of the app you’ve made then go to the Messenger settings page. Here you will generate a token of the Facebook Page to link to with Messenger. Remember to save this token key somewhere.

![Alt text](/demo/Demo2.jpg)

3. Set up Webhooks to your FB app. Remember to use the link to your own Heroku app.

![Alt text](/demo/Demo3.jpg)
![Alt text](/demo/Demo4.jpg)

Click Verify and Save. You should see the Complete sign!

4. Now we’ll set up the server again. Set the keys required as environment variables to make the bot work by running these commands in Terminal:

	```
	heroku config:set WIT_TOKEN='your_token_here'
	heroku config:set FB_PAGE_TOKEN='your_token_here'
	```

When that’s done you should be able to at least say hi to your chat bot and have it echo back hi! 🤖




### *Create stories in Wit.ai*

Wit.ai does the hard work for you by creating a simple to use interface to manage what are called “stories” — these are ways to extract meaning from a keyword or sentence.

![Alt text](/demo/Demo5.jpg)

For now you might have to write a lot of stories for your chat bot to understand language but soon Wit.ai has a vision that we can share hundreds of stories with each other. And with more stories the more skills your chat bot will have.


## 🎓 Time to teach your bot!

### *Extract location out of conversations*

1. Create a new story in the Wit dashboard. The first type of story we’ll write is extracting location from conversations. This way we can build a weather bot! Go here [https://wit.ai/jw84/weather/stories](https://wit.ai/jw84/weather/stories) to see the recipe I’ve created.

![Alt text](/demo/Demo6.jpg)

2. Find the Read function in the bot.js file. Here the bot can recognize messages. The first message can be hello, the chat bot can send back an introduction. Otherwise, we can pass the message on to Wit.ai for processing.

![Alt text](/demo/Demo7.jpg)

3. Let’s test and see if the weather shows up!

![Alt text](/demo/Demo8.gif)

### *Extract category and sentiment out of conversations*

1. Create yet another Wit app in the dashboard. This new app will have three stories to extract the category and sentiment entities out of a conversation so your chat bot can reply with some cute pics! Go here [https://wit.ai/jw84/cutepics/stories](https://wit.ai/jw84/cutepics/stories) to see the recipe

2. Create a new story to look for the user response then trigger the bot to execute the action and respond.

![Alt text](/demo/Demo9.jpg)

When you double click a word it will be highlighted as blue, after assigning the word as an entity the highlight turns purple to indicate Wit.ai has learned.

Have the bot execute a merge function to extract the entity and save it to the context object. Then execute the fetch-pics action to return the actual pic itself. Thereafter you can trigger two replies by Wit.ai, one saying what the category is and the other the image link itself.

3. Add and create two more stories based on my template. Each will help train Wit.ai on what to look for and how to respond to a sentiment and to an acknowledgement.

![Alt text](/demo/Demo10.jpg)

4. Now we can test. Be sure to update your Heroku server with the new Wit app’s token by running this command:

	```
	heroku config:set WIT_TOKEN='your_new_token_here'
	```

Let’s go back and chat it up. Ask to see some corgis. Then ask to see some racoons!

![Alt text](/demo/Demo11.gif)

Congratulations you’ve made a chat bot that’s smart enough to know what you’re talking about, what you mean, and reply back accordingly!

## 🤓 Tell Your Chat Bot What’s What

NLP and NLU are not magical! They are merely defining rules for your bot. Your chat bot will break sometimes and maybe even often when being told new and interesting conversations it’s never heard before.

It is now up to you to help keep training and testing your bot until it’s the very best. You have two tools in Wit.ai to do so. Go to the console and let’s learn about the Inbox and the Understanding page.

### *Training your chat bot*

Wit.ai has an inbox that shows you all the messages it has received. From here you can pick and choose which one to validate and contribute to the training data of your chat bot.

![Alt text](/demo/Demo12.jpg)

For example, if you have an entity of location you can assign more values to help train your chat bot to know that “San Francisco” and “Timbuktu” are both locations. If you have categories for “corgis” then you can also validate that “dogs” can mean the same thing.

The more people that talk to your chat bot and the more complex your bot is the more you will spend time in this page. And the work you do here will help make your bot even smarter.

### *Testing your chat bot*

The other page you will be spending time at will be in the Understanding page. Here you can try out sentences yourself to test whether your chat bot can understand and to troubleshoot when a sentence doesn’t trigger the right response.

![Alt text](/demo/Demo13.jpg)

## 📡 How to share your bot

Learn how to get your bot approved for public use [here](https://developers.facebook.com/docs/messenger-platform/app-review).

Remember, your chat bot has to be approved by Facebook so that anyone can talk to it. Otherwise, you have to go to the Roles page in your Facebook app and add testers.

![Alt text](/demo/Demo14.jpg)

### *Add a chat button to your webpage*

Go [here](https://developers.facebook.com/docs/messenger-platform/plugin-reference) to learn how to add a chat button your page.

### *Create a shortlink*

You can use https://m.me/<PAGE_USERNAME> to have someone start a chat.

## 💡 What's next?

Read about all things chat bots with the ChatBots Magazine [here](https://medium.com/chat-bots)

You can also design Messenger bots in Sketch with the [Bots UI Kit](https://bots.mockuuups.com)!

## How I can help

I build and design bots all day. Email me for help!

## Créditos

Este projeto é um *fork* de https://github.com/jw84/messenger-bot-witai-tutorial. Obrigado @jw84 pela inspiração.

