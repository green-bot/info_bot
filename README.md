





  

```
#!/usr/bin/env coffee

```







# Introduction
This is an introduction to the bot
hey



  

```
prompt = require 'prompt'
Promise = require 'bluebird'
Promise.promisifyAll prompt


```







Make an object to hold the answers


  

```

answers = {}
ADDRESS = process.env.ADDRESS or "You can also email us at sales@green-bot.com"
HOURS = process.env.HOURS or "We are open from 9 to 5 Monday through Friday"
PROMPT_1 = process.env.PROMPT_1 or "Thank you for texting in and trying out KISST4INFO, a simple way for your customers to contact your business using text and social messaging."
PROMPT_2 = process.env.PROMPT_2
SIGNATURE = process.env.SIGNATURE or "Thank you, you can visit us on the web at http://www.green-bot.com"
SPECIALS = process.env.SPECIALS or "Do you want to become a dealer for KISST Services? Or do you want to use it for your business? Call or text 508-364-9972 to discuss your needs"


```







Prompt is synchronous, no need for promises.


  

```
prompt.start()
prompt.message = ''
prompt.delimiter = ''

recordAnswer = (key, value) ->
  answers[key] = value
  console.log JSON.stringify answers



```







Submit the first two prompts


  

```

console.log PROMPT_1
console.log PROMPT_2 if PROMPT_2?

choicesSchema =
  description: 'How can I help you (address, hours, specials, contact, quit)?'
  type: 'string'
  pattern: /address|hours|specials|contact|quit/i
  message: 'Choice must be address, hours, specials, contact or quit.'
  required: true
  before: (value) -> value.toLowerCase()

handleChoice = ->
  prompt.getAsync(choicesSchema).then (result) ->
    answer = result.question
    recordAnswer 'choice', answer
    switch answer
      when "hours" then console.log HOURS
      when "address" then console.log ADDRESS
      when "contact"
        console.log "Thank you - we'll get back in touch as soon as possible"
        recordAnswer("contactMe", "true")
    answer
  .then (answer) ->
    if answer is 'quit'
      prompt.stop()
      return
    else
      handleChoice()

handleChoice().then ->
  console.log SIGNATURE


```




