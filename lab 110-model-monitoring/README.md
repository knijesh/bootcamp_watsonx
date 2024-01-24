# Lab 2 - Model monitoring

In this lab we will explore the abilities to manage a "model" - and that means, the combination a prompt (template) and the Large Language Model it uses - through the lens of an "AI Use case" and monitor its behavior at each stage of its lifecycle.

> Notes: 
> - This lab assumes that you have completed the previous lab, [Lab 1](../lab1-lifeycle-governance/). We will reuse the prompt that you have created there. 
> - The data sets that are used for evaluation and payload monitoring are located in the [data](./data/) folder of this repo. 
> - If you encounter some technical issues while working on the labs, please refer to this [troubleshooting document](https://github.ibm.com/client-engineering-watsonxai/watsonx.governance-bootcamp/blob/main/troubleshooting-tips.md). Feel free to add to it as well. 
> - At least some of the instructions below are kept deliberately vague. The intent here is to encourage you to become familiar with the relevant user interfaces, and that includes sometimes having to search for the right place and ways to navigate there.

<div align="center">
    <img src="images/ready-lets%20go.png" alt="description_of_image">
</div>


Go to the "AI Use Cases" screen and create a new use case. Give it a descriptive name and put it into state “Awaiting Development”.
![](images/image1.png)

For the rest of this lab, we will assume you reuse the prompt you created in Lab 1, called "Insurance Summarization Prompt". If you feel ambitious, you can also create a new prompt and start from scratch. Make sure you test and evaluate the prompt if you do so.
But note that later steps in this lab use predefined data sets to simulate a drift situation. If you use your own prompt, you will also have to generate appropriate data sets, which may be a lengthy process. 

Go the "AI Factsheet" tab. See that the prompt template is not governed. We want to govern it, so let's add it to the use case we created above. Use the default approach. Make it “Experimental”. 
![](images/image4.png)
Note how it shows up as in “Development” in the use case. See how you have two places to get to this info, namely via the prompt and its factsheet, or via use cases.
![](images/image5.png)

Advance the status of use case to “Development in progress”.

We assume now that we want to make additional changes to the prompt, and use a new version. For that, create a new prompt that is a copy of the first one. Do that by opening it in prompt lab and use “Save as” to give it a new name. That opens it for editing again.
![](images/image6.png)

Play through the scenario hat you may want to add another set of examples, or use a different model, or use different parameter settings. Use evaluations as a way to measure potential improvements. 
(Note that in order to track it separately in the use case, you have to make changes to it.)

Once you have a new version you are happy with, go to the factsheet and add the prompt to the same use case. Select Minor Change (version 0.1.0). See it appear in the use case.
![](images/image7.png)

These two prompts are now tracked separately within the same use case. Below, we will focus on the second prompt and take it through the lifecycle.

Promote the second prompt to a deployment space. Start from the Projects view and select the "Promote" action.
![](images/image8.png)

<!--
As part of this promotion, you will need to create a deployment space. Make sure it is set to “Development” stage. 
![](images/image9.png)
-->

We have pre-created the deployment spaces for you, so in the drop-down, you can just select the "deployment-space-watsonx-gov-bootcamp-students-development" deployment space for this promotion.

After promoting it, see how it appears in the use case in a new place.
![](images/image10.png)

Advance the status of the use case to “Ready for AI asset validation”.

Now go to the deployment space and see that the prompt hasn’t actually been deployed. Do that now.
Then go back to the use case and see the prompt as “Pending Evaluation”.
![](images/image11.png)

Go to the deployed prompt and pick the “Evaluations” tab. Select “Activate”.
![](images/image12.png)

Run another evaluation and check the results. It may seem redundant to run evaluations with the same prompt and the same data set over and over, but keep in mind that 
- the evaluations would potentially performed by different people in different roles, e.g. the prompt developer would not be the one evaluating it for use in production,
- in a real world scenario, the data sets would differ between evaluations.

Note that in the use case, the status changes from Pending to “Evaluated”.
![](images/image13.png)

Change the use case status to “Validation complete”.

Go to the fact sheet of the prompt and export a report in PDF format. Familiarize yourself with the content of the report. (Note: PDF export currently does not work).

<!--
Now create a production deployment space and promote the prompt to it.
![](images/image14.png)
-->

Now promote the prompt again, this time to the production deployment space called "deployment-space-watsonx-gov-bootcamp-students-production".

See how the use case is updated to show the prompt under “Operate”. But it’s not actually deployed yet, so let’s do that, too.
![](images/image15.png)

It’s pending evaluation, so let’s run yet another evaluation. See how when you activate the monitoring, it now shows "Drift v2" as an additional option. It also describes how payload data must have column that match the input variables of the prompt.
![](images/image16.png)

Once the prompt is deployed into production, we can test it to make sure it is functioning as expected. Open the prompt in the space and select the test tab.
![](images/image24.png)

Click on Generate to make sure the prompt works.

Review the schemas of the Payload and the Feedback data. The CSV files we upload later need to follow these schemas.

We are now ready to prime the prompt deployment with baseline data. You can use the "Insurance claim summarization drift payload 0 - baseline.csv" file to do so. Upload this file as the payload data. We are not uploading any feedback data at this point.
![](images/image17.png)

Click on "Evaluate now". It should result in a passed test.
![](images/image18.png)

If you now select "Configure monitors" in the Action menu, you see that Drift v2 is enabled, you have a baseline of 100 records and you can edit the threshold values here, too. In a real world scenario, these thresholds should be adjusted to the concrete use case and prompt. But we'll leave that to an advanced lab and won't change them now.
![](images/image19.png)

The system will now monitor invocations of the prompt and detect potential drift. To simulate a drift example, you can upload a number of data files, named "Insurance claim summarization drift payload [1 hrough 4].csv". Upload all of them as Payload data. Let some time pass between the runs, so that it shows a meaningful timeline. Once you have run these evaluations, you will see another diagram show up on the screen, namely for the Drift.
![](images/image20.png)

You can click on the blue arrow in the top right corner of that section to get a more detailed view.
![](images/image21.png)

Make sure you have selected the right time settings to see the drift change over time. There are only three criteria whih go into the drift calculations: "output", "output metadata drift" and "input metadata drift". 

These measurements say nothing about the quality of the prompt. To measure it, we need to upload feedback data. Go to the Evaluations tab and click on Actions -> Evaluate now. Upload the [Insurance claim summarization feedback.csv](./data/Insurance%20claim%20summarization%20feedback.csv) file.

When the run is finished, you can see a panel with "Generative AI Quality - Text summarization" metrics on the lower right of the screen. Note how the arrow that would lead to a more detailed view of this information is greyed out. This is expected to be implemented in a later version of the product. For now, you can see the metrics and their scores summarized.
![](images/image25.png)

Pending here is a more detailed discussion of the various metrics, what scores to expect and how the thresholds should be set that lead to a potential alert.

<!--
Note that you can also run some manual invocations of the prompt here via the "Test" tab. Each of these transactions is also monited and added to the overall metrics for the model health.
![](images/image22.png)
-->

Finally, we are ready to advance the status of the use case to "in operation". The prompt is deployed in production and is being monitored for drift and quality. Given that the evaluations failed at some steps along the way, we may want to also change the Risk level to Medium.
![](images/image23.png) 

<div align="center">
    <img src="images/victory.png" alt="description_of_image">
</div>

