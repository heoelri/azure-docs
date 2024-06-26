---
title: "Tutorial: Patterns - LUIS"
description: Use patterns to increase intent and entity prediction while providing fewer example utterances in this tutorial. The pattern is provided as a template utterance example, which includes syntax to identify entities and ignorable text.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 07/06/2020
#Customer intent: As a new user, I want to understand how and why to use patterns.
---

# Tutorial: Add common pattern template utterance formats to improve predictions

In this tutorial, use patterns to increase intent and entity prediction, which allows you to provide fewer example utterances. The pattern is a template utterance assigned to an intent, which contains syntax to identify entities and ignorable text.

**In this tutorial, you learn how to:**

> [!div class="checklist"]
> * Create a pattern
> * Verify pattern prediction improvements
> * Mark text as ignorable and nest within pattern
> * Use test panel to verify pattern success

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## Utterances in intent and pattern

There are two types of utterances stored in the LUIS app:

* Example utterances in the Intent
* Template utterances in the Pattern

Adding template utterances as a pattern allows you to provide fewer example utterances overall to an intent.

A pattern is applied as a combination of text matching and machine learning.  The template utterance in the pattern, along with the example utterances in the intent, give LUIS a better understanding of what utterances fit the intent.

## Import example app and clone to new version

Use the following steps:

1.  Download and save the [app JSON file](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/tutorial-fix-unsure-predictions.json?raw=true).

1. Sign in to the [LUIS portal](https://www.luis.ai), and select your **Subscription** and **Authoring resource** to see the apps assigned to that authoring resource.
1. Import the JSON into a new app into the [LUIS portal](https://www.luis.ai). On the **My Apps** page, select **+ New app for conversation**, then select **Import as JSON**. Select the file you downloaded in the previous step, name the app, `Patterns tutorial`.

## Create new intents and their utterances

The two intents find the manager or the manager's direct reports, based on the utterance text. The difficulty is that the two intents _mean_ different things but most of the words are the same. Only the word order is different. In order for the intent to be predicted correctly, it would have to have many examples.

1. Select **Build** from the navigation bar.

1. On the **Intents** page, select **+ Create** to create a new intent.

1. Enter `OrgChart-Manager` in the pop-up dialog box then select **Done**.

    ![Create new message pop-up window](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

1. Add example utterances to the intent. These utterances are not _exactly_ alike but do have a pattern that can be extracted.

    |Example utterances|
    |--|
    |`Who is John W. Smith the subordinate of?`|
    |`Who does John W. Smith report to?`|
    |`Who is John W. Smith's manager?`|
    |`Who does Jill Jones directly report to?`|
    |`Who is Jill Jones supervisor?`|

1. Select **Intents** in the left navigation.

1. Select **+ Create** to create a new intent. Enter `OrgChart-Reports` in the pop-up dialog box then select **Done**.

1. Add example utterances to the intent.

    |Example utterances|
    |--|
    |`Who are John W. Smith's subordinates?`|
    |`Who reports to John W. Smith?`|
    |`Who does John W. Smith manage?`|
    |`Who are Jill Jones direct reports?`|
    |`Who does Jill Jones supervise?`|

### Caution about example utterance quantity

[!INCLUDE [Too few examples](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]

### Train the app before testing or publishing

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

### Publish the app to query from the endpoint

[!INCLUDE [LUIS How to Publish steps](includes/howto-publish.md)]

### Get intent and entities from endpoint

1. [!INCLUDE [LUIS How to get endpoint first step](includes/howto-get-endpoint.md)]

1. Go to the end of the URL in the address bar and replace _YOUR_QUERY_HERE_ with: `Who is the boss of Jill Jones?`.

    ```json
    {
        "query": "Who is the boss of Jill Jones?",
        "prediction": {
            "topIntent": "OrgChart-Manager",
            "intents": {
                "OrgChart-Manager": {
                    "score": 0.326605469
                },
                "OrgChart-Reports": {
                    "score": 0.127583548
                },
                "EmployeeFeedback": {
                    "score": 0.0299124215
                },
                "MoveEmployee": {
                    "score": 0.01159851
                },
                "GetJobInformation": {
                    "score": 0.0104600191
                },
                "ApplyForJob": {
                    "score": 0.007508645
                },
                "Utilities.StartOver": {
                    "score": 0.00359402061
                },
                "Utilities.Stop": {
                    "score": 0.00336530479
                },
                "FindForm": {
                    "score": 0.002653719
                },
                "Utilities.Cancel": {
                    "score": 0.00263288687
                },
                "None": {
                    "score": 0.00238638581
                },
                "Utilities.Help": {
                    "score": 0.00226386427
                },
                "Utilities.Confirm": {
                    "score": 0.00211663754
                }
            },
            "entities": {
                "keyPhrase": [
                    "boss of Jill Jones"
                ],
                "EmployeeListEntity": [
                    [
                        "Employee-45612"
                    ]
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "boss of Jill Jones",
                            "startIndex": 11,
                            "length": 18,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "EmployeeListEntity": [
                        {
                            "type": "EmployeeListEntity",
                            "text": "Jill Jones",
                            "startIndex": 19,
                            "length": 10,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

The correct top intent was predicted, `OrgChart-Manager`, but the score is not above 70% and isn't far enough above the next highest intent. Use patterns to make the correct intent's score significantly higher in percentage and farther from the next highest score.

Leave this second browser window open. You will use it again later in the tutorial.

## Template utterances
Because of the nature of the Human Resource subject domain, there are a few common ways of asking about employee relationships in organizations. For example:

|Utterances|
|--|
|`Who does Jill Jones report to?`|
|`Who reports to Jill Jones?`|

These utterances are too close to determine the contextual uniqueness of each without providing _many_ utterance examples. By adding a pattern for an intent, LUIS learns common utterance patterns for an intent without the need of supplying many more utterance examples.

Template utterance examples for this intent include:

|Template utterances examples|syntax meaning|
|--|--|
|`Who does {EmployeeListEntity} report to[?]`|interchangeable `{EmployeeListEntity}`<br>ignore `[?]`|
|`Who reports to {EmployeeListEntity}[?]`|interchangeable `{EmployeeListEntity}`<br>ignore `[?]`|

The `{EmployeeListEntity}` syntax marks the entity location within the template utterance as well as which entity it is. The optional syntax, `[?]`, marks words, or [punctuation](luis-reference-application-settings.md#punctuation-normalization) that is optional. LUIS matches the utterance, ignoring the optional text inside the brackets.

While the syntax looks like a regular expression, it is not a regular expression. Only the curly bracket, `{}`, and square bracket, `[]`, syntax is supported. They can be nested up to two levels.

In order for a pattern to be matched to an utterance, _first_ the entities within the utterance have to match the entities in the template utterance. This means the entities have to have enough examples in example utterances with a high degree of prediction before patterns with entities are successful. However, the template doesn't help predict entities, only intents.

**While patterns allow you to provide fewer example utterances, if the entities are not detected, the pattern does not match.**

### Add the patterns for the OrgChart-Manager intent

1. Select **Build** in the top menu.

1. In the left navigation, under **Improve app performance**, select **Patterns** from the left navigation.

1. Select the **OrgChart-Manager** intent, then enter the following template utterances:

    |Template utterances|
    |:--|
    |`Who is {EmployeeListEntity} the subordinate of[?]`|
    |`Who does {EmployeeListEntity} report to[?]`|
    |`Who is {EmployeeListEntity}['s] manager[?]`|
    |`Who does {EmployeeListEntity} directly report to[?]`|
    |`Who is {EmployeeListEntity}['s] supervisor[?]`|
    |`Who is the boss of {EmployeeListEntity}[?]`|

    These template utterances include the **EmployeeListEntity** entity with the curly bracket notation.

1. While still on the Patterns page, select the **OrgChart-Reports** intent, then enter the following template utterances:

    |Template utterances|
    |:--|
    |`Who are {EmployeeListEntity}['s] subordinates[?]`|
    |`Who reports to {EmployeeListEntity}[?]`|
    |`Who does {EmployeeListEntity} manage[?]`|
    |`Who are {EmployeeListEntity} direct reports[?]`|
    |`Who does {EmployeeListEntity} supervise[?]`|
    |`Who does {EmployeeListEntity} boss[?]`|

### Query endpoint when patterns are used

Now that the patterns are added to the app, train, publish, and query the app at the prediction runtime endpoint.

1. Select **Train**. After training is complete, select **Publish** and select the **Production** slot then select **Done**.

1. After publishing is complete, switch browser tabs back to the endpoint URL tab.

1. Go to the end of the URL in the address bar and verify your query is still `Who is the boss of Jill Jones?` then submit the URL for a new prediction.

    ```json
    {
        "query": "Who is the boss of Jill Jones?",
        "prediction": {
            "topIntent": "OrgChart-Manager",
            "intents": {
                "OrgChart-Manager": {
                    "score": 0.999999046
                },
                "OrgChart-Reports": {
                    "score": 3.237443E-05
                },
                "EmployeeFeedback": {
                    "score": 4.364242E-06
                },
                "GetJobInformation": {
                    "score": 1.616159E-06
                },
                "MoveEmployee": {
                    "score": 7.575752E-07
                },
                "ApplyForJob": {
                    "score": 5.234157E-07
                },
                "None": {
                    "score": 3.3E-09
                },
                "Utilities.StartOver": {
                    "score": 1.26E-09
                },
                "FindForm": {
                    "score": 1.13636367E-09
                },
                "Utilities.Cancel": {
                    "score": 1.13636367E-09
                },
                "Utilities.Confirm": {
                    "score": 1.13636367E-09
                },
                "Utilities.Help": {
                    "score": 1.13636367E-09
                },
                "Utilities.Stop": {
                    "score": 1.13636367E-09
                }
            },
            "entities": {
                "keyPhrase": [
                    "boss of Jill Jones"
                ],
                "EmployeeListEntity": [
                    [
                        "Employee-45612"
                    ]
                ],
                "$instance": {
                    "keyPhrase": [
                        {
                            "type": "builtin.keyPhrase",
                            "text": "boss of Jill Jones",
                            "startIndex": 11,
                            "length": 18,
                            "modelTypeId": 2,
                            "modelType": "Prebuilt Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ],
                    "EmployeeListEntity": [
                        {
                            "type": "EmployeeListEntity",
                            "text": "Jill Jones",
                            "startIndex": 19,
                            "length": 10,
                            "modelTypeId": 5,
                            "modelType": "List Entity Extractor",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

The intent prediction is now significantly more confident and the next highest intent's score is very low. These two intents won't flip-flop when training.

### Working with optional text and prebuilt entities

The previous pattern template utterances in this tutorial had a few examples of optional text such as the possessive use of the letter s, `'s`, and the use of the question mark, `?`. Suppose you need to allow for current and future dates in the utterance text.

Example utterances are:

|Intent|Example utterances with optional text and prebuilt entities|
|:--|:--|
|OrgChart-Manager|`Who was Jill Jones manager on March 3?`|
|OrgChart-Manager|`Who is Jill Jones manager now?`|
|OrgChart-Manager|`Who will be Jill Jones manager in a month?`|
|OrgChart-Manager|`Who will be Jill Jones manager on March 3?`|

Each of these examples uses a verb tense, `was`, `is`, `will be`, as well as a date, `March 3`, `now`, and `in a month`, that LUIS needs to predict correctly. Notice that the last two examples in the table use almost the same text except for `in` and `on`.

Example template utterances that allow for this optional information:

|Intent|Example utterances with optional text and prebuilt entities|
|:--|:--|
|OrgChart-Manager|`who was {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|
|OrgChart-Manager|`who is {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|


The use of the optional syntax of square brackets, `[]`, makes this optional text easy to add to the template utterance and can be nested up to a second level, `[[]]`, and include entities or text.


**Question: Why are all the `w` letters, the first letter in each template utterance, lowercase? Shouldn't they be optionally upper or lowercase?** The utterance submitted to the query endpoint, by the client application, is converted into lowercase. The template utterance can be uppercase or lowercase and the endpoint utterance can also be either. The comparison is always done after the conversion to lowercase.

**Question: Why isn't prebuilt number part of the template utterance if March 3 is predicted both as number `3` and date `March 3`?** The template utterance contextually is using a date, either literally as in `March 3` or abstracted as `in a month`. A date can contain a number but a number may not necessarily be seen as a date. Always use the entity that best represents the type you want returned in the prediction JSON results.

**Question: What about poorly phrased utterances such as `Who will {EmployeeListEntity}['s] manager be on March 3?`.** Grammatically different verb tenses such as this where the `will` and `be` are separated need to be a new template utterance. The existing template utterance will not match it. While the intent of the utterance hasn't changed, the word placement in the utterance has changed. This change impacts the prediction in LUIS. You can [group and or](#use-the-or-operator-and-groups) the verb-tenses to combine these utterances.

> [!CAUTION]
> **Remember: entities are found first, then the pattern is matched.**

### Add new pattern template utterances

1. While still in the **Patterns** section of **Build**, add several new pattern template utterances. Select **OrgChart-Manager** from the Intent drop-down menu and enter each of the following template utterances:

    |Intent|Example utterances with optional text and prebuilt entities|
    |--|--|
    |OrgChart-Manager|`who was {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {EmployeeListEntity}['s] manager [[in]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|

2. Select **Train** in the navigation bar to train the app.

3. After training is complete, select **Test** at the top of the panel to open the testing panel.

4. Enter several test utterances to verify that the pattern is matched and the intent score is significantly high.

    After you enter the first utterance, select **Inspect** under the result so you can see all the prediction results. Each utterance should have the **OrgChart-Manager** intent and should extract the values for the `EmployeeListEntity` and `datetimeV2` entities.

    |Utterance|
    |--|
    |`Who will be Jill Jones manager`|
    |`who will be jill jones's manager`|
    |`Who will be Jill Jones's manager?`|
    |`who will be Jill jones manager on March 3`|
    |`Who will be Jill Jones manager next Month`|
    |`Who will be Jill Jones manager in a month?`|

All of these utterances found the entities inside, therefore they match the same pattern, and have a high prediction score. You added a few patterns that will match many variations of utterances. You didn't need to add any example utterances in the intent to have the template utterance work in the pattern.

This use of patterns provided:
* Higher prediction scores
* With the same example utterances in the intent
* With just a few well-constructed template utterances in the pattern

### Use the OR operator and groups

Several of the previous template utterances are very close. Use the **group** `()` and **OR** `|` syntax to reduce the template utterances.

The following two patterns can combine into a single pattern using the group `()` and OR `|` syntax.

|Intent|Example utterances with optional text and prebuilt entities|
|--|--|
|OrgChart-Manager|`who will be {EmployeeListEntity}['s] manager [[in]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {EmployeeListEntity}['s] manager [[on]{datetimeV2}?]`|

The new template utterance will be:

`who ( was | is | will be ) {EmployeeListEntity}['s] manager [([in]|[on]){datetimeV2}?]`.

This uses a **group** around the required verb tense and the optional `in` and `on` with an **or** pipe between them.

> [!NOTE]
> When using the _OR_ symbol , `|` (pipe), make sure to separate the pipe symbol with a space before and after it in the example template.

1. On the **Patterns** page, select the **OrgChart-Manager** filter. Narrow the list by searching for `manager`.

1. Keep one version of the template utterance (to edit in next step) and delete the other variations.

1. Change the template utterance to:

    `who ( was | is | will be ) {EmployeeListEntity}['s] manager [([in]|[on]){datetimeV2}?]`

2. Select **Train** in the navigation bar to train the app.

3. After training is complete, select **Test** at the top of the panel to open the testing panel.

    Use the Test pane to test versions of the utterance:

    |Utterances to enter in Test pane|
    |--|
    |`Who is Jill Jones manager this month`|
    |`Who is Jill Jones manager on July 5th`|
    |`Who was Jill Jones manager last month`|
    |`Who was Jill Jones manager on July 5th`|
    |`Who will be Jill Jones manager in a month`|
    |`Who will be Jill Jones manager on July 5th`|

By using more pattern syntax, you reduce the number of template utterances you have to maintain in your app, while still having a high prediction score.

### Use the utterance beginning and ending anchors

The pattern syntax provides beginning and ending utterance anchor syntax of a caret, `^`. The beginning and ending utterance anchors can be used together to target very specific and possibly literal utterance or used separately to target intents.

## Using Pattern.any entity

[!INCLUDE [Pattern.any entity - concepts](./includes/pattern-any-entity.md)]

### Add example utterances with Pattern.any

1. Select **Build** from the top navigation, then select **Intents** from left navigation.

1. Select **FindForm** from the intents list.

1. Add some example utterances. The text that should be predicted as a Pattern.any is in **bold text**. The form name is difficult to determine from the other words around it in the utterance. The Pattern.any will help by marking the boundaries of the entity.

    |Example utterance|Form name|
    |--|--|
    |Where is the form **What to do when a fire breaks out in the Lab** and who needs to sign it after I read it?|What to do when a fire breaks out in the Lab
    |Where is **Request relocation from employee new to the company** on the server?|Request relocation from employee new to the company|
    |Who authored "**Health and wellness requests on the main campus**" and what is the most current version?|Health and wellness requests on the main campus|
    |I'm looking for the form named "**Office move request including physical assets**". |Office move request including physical assets|

    Without a Pattern.any entity, it would be difficult for LUIS to understand where the form title ends because of the many variations of form names.

### Create a Pattern.any entity
The Pattern.any entity extracts entities of varying length. It only works in a pattern because the pattern marks the beginning and end of the entity with syntax.

1. Select **Entities** in the left navigation.

1. Select **+ Create**, enter the name `FormName`, and select **Pattern.any** as the type. Select **Create**.

### Add a pattern that uses the Pattern.any

1. Select **Patterns** from the left navigation.

1. Select the **FindForm** intent.

1. Enter the following template utterances, which use the new entity:

    |Template utterances|
    |--|
    |`Where is the form ["]{FormName}["] and who needs to sign it after I read it[?]`|
    |`Where is ["]{FormName}["] on the server[?]`|
    |`Who authored ["]{FormName}["] and what is the most current version[?]`|
    |`I'm looking for the form named ["]{FormName}["][.]`|

1. Train the app.

### Test the new pattern for free-form data extraction
1. Select **Test** from the top bar to open the test panel.

1. Enter the following utterance:

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

1. Select **Inspect** under the result to see the test results for entity and intent.

    The entity `FormName` is found first, then the pattern is found, determining the intent. If you have a test result where the entities are not detected, and therefore the pattern is not found, you need to add more example utterances on the intent (not the pattern).

1. Close the test panel by selecting the **Test** button in the top navigation.

### Using an explicit list

If you find that your pattern, when it includes a Pattern.any, extracts entities incorrectly, use an [explicit list](reference-pattern-syntax.md#explicit-lists) to correct this problem.

## What did this tutorial accomplish?

This tutorial added patterns to help LUIS predict the intent with a significantly higher score without having to add more example utterances. Marking entities and ignorable text allowed LUIS to apply the pattern to a wider variety of utterances.

## Clean up resources

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## Next steps


> [!div class="nextstepaction"]
> [Learn more about the pattern syntax](./reference-pattern-syntax.md)
