# Lab 1: Assisted Prompt Evaluation
---
<div align="center">
    <img src="images/start.png" alt="description_of_image">
</div>

## Summary
---
In this lab, we will be walking through how to create your own project and start using the powerful capabilities of watsonx.governance. We will walk through creating a prompt template, evaluating it against a dataset, and viewing all of the metrics that are calculated within a given evaluation. After that, we will take a look at the AI factsheet to learn how to modify thresholds for evaluations, then create a couple of different prompt templates that you can use to achieve the best evaluation you can on our dataset! Good luck!

## Table of Contents

  1. [Creating Your Project](#creating-your-project)
  2. [Connecting WatsonMachineLearning](#connecting-watsonmachinelearning)
  3. [Creating a Prompt Template](#creating-a-prompt-template)
     1. [Prompt Variables](#prompt-variables)
     2. [Saving Prompt](#saving-prompt)
     3. [Model Parameters](#model-parameters)
  4. [Evaluating Your Prompt](#evaluating-your-prompt) 
  5. [Alternative Way to Evaluate Prompt](#alternative-evaluation)
  6. [Evaluation Results](#evaluation-results)
  7. [Experimenting With New Prompts](#experimenting-with-new-prompts)
     1. [Using Granite](#using-granite)
     2. [Your Choice!](#your-choice)
  8. [Experimenting With Settings](#experiment-with-settings)

---
**Note:** If you encounter some technical issues while working on the labs, please refer to this [troubleshooting document](https://github.ibm.com/client-engineering-watsonxai/watsonx.governance-bootcamp/blob/main/troubleshooting-tips.md). (Feel free to add to it as well.)

### 1. Creating Your Project<a name="creating-your-project"></a>
---
Before you can begin using watsonx.governance, you first have to make a new project that you will use to complete this lab. To create a new project, navigate to the home page for watsonx, shown here:

![Home page for watsonx](images/picture1.png)

Ensure that you also have the correct account selected in the top right of your screen (in my case, this is my own personal account -- "Andrew Bruneel's Account"). If you're doing the bootcamp, use the provided TechZone Environment. An example of a TechZone Environment instance name would look something like **"[7+ digit number] - itz-watsonx"**, but it ultimately depends on what your bootcamp lead names it. Once you have verified your account is correct, scroll down on your screen to where you can see the list of projects you currently have on your cloud account. Click on the + button to create a new project.

*Note if you are using a TechZone instance in the bootcamp:* The account name at the top right tends to change to your default personal account every time you switch/go back to a new page. Make sure to always check the top right corner every time you switch to a new page to make sure you are on the right instance (i.e. **"[7+ digit number] - itz-watsonx"**). If not, you may experience issues with access rights, missing resources, etc. 

![Create new project](images/picture2.png)

Soon, you will see a new screen pop up where you are able to name your project as well as select the proper storage, and enter a description if you like. Title your project "Insurance Claim Summarization - [Your Name]" as I have in the image below. In the live bootcamp, this is an important step to ensure each attendee has their own project. Your storage should be selected for you, but if it is not then make sure it is correct before proceeding. Once you have done this, select "Create"

![Project creation screen](images/picture3.png)

Well done! You have successfully created a project where we will be able to write prompts to watsonx, and later on evaluate and deploy our prompts into production. For now, you should see an overview of your blank project screen.

### 2. Connecting Watson Machine Learning<a name="connecting-watsonmachinelearning"></a>
---
![Overview screen](images/picture4.png)

Before we can proceed with creating our first prompt, we have to associate this project with a Watson Machine Learning (WML) instance in order to be able to evaluate LLMs on the prompts we create. To do this, click on the "Manage" tab on the very top.

![Manage screen](images/picture5.png)

Now click "Services and Integrations" from the sidebar, and click the blue "Associate Service" button.

![Services and integrations](images/picture6.png)

From there, you will likely see several different options to associate with your project. For our purposes, you will only need to select the WatsonMachineLearning option, and click "Associate".

![Associate service](images/picture7.png)

Great job! You should now see WatsonMachineLearning pop up under the "IBM Services Tab" that appears on the page after you have clicked associate.

![Successful association](images/picture8.png)

### 3. Creating a Prompt Template<a name="creating-a-prompt-template"></a>
---
Let's begin creating our first prompt. Head over to the "Assets" tab, again from the top of your screen. Once there, click on "New Asset".

![Assets tab](images/picture9.png)

Now, if you scroll down on the menu that appears, you should see a card that says "Experiment with foundation models and build prompts". Select this to be taken into your first prompt with watsonx.governance!

![Create prompt screen](images/picture10.png)

Once you select the prompt building card, you will be greeted with the following screen, which has numerous options on how you can configure your prompt to suit your needs. You will notice that there is a box labeled "Instruction" which is where we will be placing the bulk of our prompt to be evaluated later. Right above that, you see the option to select "Structured" or "Freeform". This lab will walk though utilizing the structured prompt functionality, however this is possible to do with freeform as well, so choose whichever option is the most comfortable for you! Structured prompts have easily identifiable sections to place examples, instructions, and prompt variables (which we will discuss later). Freeform prompts have a looser and more flexible structure. Also note the default LLM selection, which is flan-ul2-20b.

You will also see an icon with a graph at the top right, which is the button to run an evaluation that we will discuss later. You will be able to see the status of your work at the top right as well, indicating whether your prompt session work is Saved or Unsaved. Note that to run an evaluation, you will need to click the dropdown and save your work.

![Initial prompt screen](images/prompt-lab-intro.png)

Inside of the Instruction box, enter your first prompt that we will be evaluating throughout this lab. We have a sample prompt shown in the directions that you will be able to modify later as the lab moves on. For now, enter the prompt shown in the screen below.

```
You are an insurance agent tasked to assess insurance claims. Summarize the following insurance claim input. Focus on the car and the damage. Make the summary at least 3 sentences long.
```

![Enter prompt](images/insurance-agent.png)

Now, we need to ensure that we are able to test our prompt against validation and test data. To do so, we need to ensure we have prompt variables set up for our instructions. By scrolling down your page, you will notice an area on the screen titled "Try". Inside of this, you want to put a variable inside of the input titled "{input}" and then click on the "prompt variables" button.

#### 3.1 Prompt Variables<a name="prompt-variables"></a>
---
![Prompt vars](images/prompt-variables.png)

Once you have opened up that sidebar, you will see two empty boxes pop up, with "Variable" and "Default Value" able to be filled out. Use "input" again as the variable, and for the default value you can simply put "null" for the purpose of this lab, as none of our testing or validation data should have missing data.

![Default val](images/prompt-variables-2.png)

Next to where you clicked prompt variables, you should see an option that says "Model parameters". Click on that next. We will not be making any serious changes here just yet, but mostly giving a tour of this section so you can make modifications later. Most importantly, when you first open the tab you will see a toggle for "Greedy" or "Sampling". Selecting Greedy will allow your model results to be reproducable across experiments, while selection "Sampling" will give you more robust outputs with the trade-off of variability. For our task of summarization, we will be choosing the Sampling option for this base prompt.

#### 3.2 Model Parameters<a name="model-parameters"></a>
---
![Sampling](images/greedy.png)

Now that you have chosen Sampling, you will see several new options pop up that are specific to the Sampling decoding method. Temperature is the most important factor here, where setting a higher temperature will give higher variation and more unique outputs. Try setting this to 1 as I did in the following image. Mouse over the info icons to gain more information on Top P and Top K methods as well.

![Temperature](images/temperature.png)

Lastly, scroll all the way down the Model parameters sidebar to find the stopping criteria as well as token limits. The default value for max tokens is 20. For LLMs, tokens ~ words, so we want to set that higher in order to get more detail in our summaries of the insurance claims.

![Tokens](images/max-tokens.png)

#### 3.3 Saving Prompt<a name="saving-prompt"></a>
---
Once you are all finished, click the "Save" icone dropdown in the top right, and then click "Save as". This will take you to the following screen, where you will select your asset type, prompt template name, and task. Make sure to select the "Prompt template" option, then fill out a name in a similar fashion shown in the picture. From there, select "Summarization" for your task, and make sure to select "View in project after saving".

![Saving prompt](images/save-work.png)

![Saving prompt-2](images/save-work-2.png)

Great! Now you should see your finished prompt template on the screen when you are taken back to the Assets page. Verify that you have completed the previous steps correctly by clicking the 3 dots to the right of the prompt and choose "Evaluate". We are going to be testing out our new prompt!

### 4. Evaluating Your Prompt<a name="evaluating-your-prompt"></a>
---
![Three dots](images/picture19.png)

You should now see a screen pop up with an evalute button in the middle. Select that button. 

**Note:** You must be on the correct cloud instance for this class to select the **Evaluate button**. See this [troubleshooting document](https://github.ibm.com/client-engineering-watsonxai/watsonx.governance-bootcamp/blob/main/troubleshooting-tips.md) for help.

![Evaluate button](images/picture20.png)

Now you will see a screen asking you which dimensions to evaluate. At this point, we are only able to evaluate GenAI quality. In lab 2, we will see that deployed prompts have more information that can be tracked.

![Genai quality](images/picture21.png)

Now it's time to choose our test data. For this tutorial, we will be using the [summarization validation data](data/Insurance%20claim%20summarization%20validation%20data.csv) and the [summarization test data](data/Insurance%20claim%20summarization%20test%20data.csv) found in the data folder of this lab's github repository. For now, just select the validation data for this step. We can use the test data for our work in [Step 7](#experiment-with-settings) if desired. Make sure you have both of these stored somewhere you can find them!

![Data selection](images/picture22.png)

Now it's time to map the input variables to the input variable that we created earlier. Because I am using a CSV file, I chose the comma delimiter. Additionally, I chose "input" to be mapped to the Insurance_Claim column in my CSV file. This will ensure that my prompt is focused on summarizing the correct data. Lastly, "Reference output" is the column of the data that gives the expected output from our LLM where metrics will be evaluated against. This is how we can measure how effectively we have structured our prompts.

![Mapping vars](images/picture23.png)

After that, you will see a screen to review the information you have entered. Once you verify that you have entered everything correctly, hit the "Evaluate" button. and wait until your prompt has finished being evaluated. This process can take up to a few minutes.

![Review and eval](images/picture24.png)

Once the evaluation has finished, you should see a screen pop up displaying the results. As you can see here, our evaluation was not very successful, triggering 13 alerts and failing its test. This is to be expected! The goal of this lab is to build on our first prompt that we have created here, and work to achieve better results so that we can deploy our prompt and eventually bring it to production.

### 5. Alternative Way to Evaluate Prompt<a name="alternative-evaluation"></a>
---
For those interested, there is another way to run an evaluation from within the prompt session. As mentioned before, click on the top right icon showing a graph, which is the button to Evaluate.

![Evaluate-button](images/evaluate.png)

If you haven’t already done so, it will ask you so save your work the same way as before. Then conveniently, you will be immediately directed to the page asking you to select your dimensions to evaluate, and proceed with the same process as Step 4.

![Set-up-evaluation](images/generative-ai-quality.png)

### 6. Evaluation Results<a name="evaluation-results"></a>
---
![Results](images/picture25.png)

Scroll further down on the evaluate page to see the specific metrics your prompt was evaluated on. We will soon see a more insightful view by going to the AI factsheet.

![Eval metrics](images/picture26.png)

Up in the top left corner of your screen, choose the "AI Factsheet" tab, which will show us a better breakdown of the results we got. The first thing you will see on the page is an option to track your prompt in an AI use case. We will be covering this option in lab 2.

![AI factsheet](images/picture27.png)

Scroll down the page to the "Generative AI Quality" section, and you will see visuals accompanying the metrics we saw earlier, and you will be able to see how close our prompt was to passing tests. For the majority, it looks like we have a lot of work to do still!

![Gen ai quality](images/picture28.png)

Now that you have familiarized yourself with each metric, I'll show you how to modify the thresholds for each metric so that your prompt can have looser requirements to pass testing. In a real world use case, this should only be done if absolutely necessary and makes sense for your use case! Head back over to the evaluation screen, where you will see a blue button to the right of "Generative AI Quality - Text Summarization" where we can adjust our thresholds.

![Alter thresholds](images/picture38.png)

Go ahead and click on that button, and you will be greeted with the following screen:

![Thresholds screen](images/picture39.png)

Here we are going to adjust the readability metric. This metric is higher if your model's generated responses to your prompt are easy to interpret, and lower if they are confusing to read. Scroll down the page and click the edit button to modify it.

![Edit readability](images/picture40.png)

Lastly, go ahead and change the readability slightly, say to 55, which puts the requirement into the "Fairly difficult to read" range. After, that, you can go ahead and click save to finalize your changes. Just a reminder here that these metrics should not be altered too much in a real world use case!

![Submit readability](images/picture41.png)

---
#### **Note: Please refer to [this powerpoint](resources/LLM_Metrics.PPTX) for more information on each metric!
---

### 7. Experimenting With New Prompts<a name="experimenting-with-new-prompts>"></a>
---
Now that you've seen how to build your own prompt template and evaluate it against a test dataset, it's time to experiment! Your goal is to build a prompt template that will pass tests against the validation dataset provided to you in the lab. First, let's get you started by creating two copies of the existing prompt template that you can use to modify and play with. Click "Insurance Claim Summarization..." on the top left of your screen, the hyperlink just after "Projects". This should take you back to the "Assets" page, which will look something like this:

![Assets pg](images/picture29.png)

#### 7.1 Using Granite<a name="using-granite"></a>
---
Click back into your existing prompt, we're going to alter the LLM being used to generate summaries for us. Once you see the prompt screen back in front of you. Click on the "Model: flan-ul2-20b" dropdown, and select "View all foundational models".

![Prompt pg](images/view-all-foundation-models.png)

From here, let's choose IBM's granite-13b-chat-v2 as our new model for prompt summarization.

![Choose granite](images/picture32.png)

You'll see a page pop up displaying useful information about granite, as well as a blue "Select Model" button. Go ahead and click that once you're ready.

![Select model](images/picture33.png)

From here, we are going to "Save as" once again, which will create a new instance of a prompt template on our assets page.

![Save as new model](images/save-as-again.png)

Fill out the information on the "Save Your Work" page as I have below. Make sure to put your model name inside of the "Name" field, this will be important when we make our next prompt template!

![Granite save page](images/picture35.png)

Great work -- now we should see our new template on the home page as well as our original template we created!

![Assets w granite](images/picture36.png)

#### 7.2 Your Choice!<a name="your-choice"></a>
---
Now that you've seen how to quickly create copies of your original prompt template, you can assign new templates to a model of your choosing! Repeat the steps you took to create a prompt template evaluated with granite, but this time choose a new model (Making sure to click into the **original template** we created, not the granite template). It can be whatever you'd like! Make sure to **include the model name** in the naming of your new prompt template, as we did with granite. This will help you quickly differentiate between prompt templates later. Here is my Assets tab once I've added one more model:

![Both prompt temps](images/picture37.png)

### 8. Experiment With Settings<a name="experiment-with-settings"></a>
---
Now that you've made it this far, you are ready to experiment and improve the performance of your prompts! The goal is for you to pass your evaluation against validation data using the tools you have at your disposal. Feel free to make any number of changes, including:

* Making new prompt templates with different models
* Changing the prompt instructions
* Providing examples (few shot prompting)
* Altering model parameters such as:
  * Greedy vs. Sampling
  * Temperature
  * Min/Max Tokens
  * Top P/Top K
  * Stop Sequence

And any more changes you can think of! It may also be useful as a first step for you to take a close look at the validation data. From there, you can decide on which modifications to focus on that you think will be most effective at improving your results. Make sure to reference the previous steps from the lab if you have any questions, they should be able to answer most questions you have!

---
#### **Note: Remember that if you are having trouble, you can modify the thresholds for your evaluation **if needed**. Some thresholds have high default values that are unattainable even by state-of-the-art models, so if you find yourself consistently falling well beneath threshold values even after trying the solutions above, look into modifying your threshold values. The powerpoint shared in this lab's resources is a good resource to see which metrics may be useful to lower thresholds for.
---

Few shot prompting example:

Below is a brief example of using few shot prompting in the context of the data used for this lab. Make sure that when you decide to utilize few shot prompting, you are using **different** examples than what you have in your validation and test data, because otherwise you would be training your model on the exact scenarios it will see in test data, which is not a realistic scenario you would see in a real-world use case! This example shows a single shot, but feel free to add more to make your model more fine-tuned to your examples!

![Few shot](images/picture42.png)

As you continue to make changes, your champion prompt with the best performance will be selected for use in the following lab!

<div align="center">
    <img src="images/results.png" alt="description_of_image">
</div>
