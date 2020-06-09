

# AI powered order processing during pandemics

In this section, we walk you through a working example of extracting the entities by quiering Watson Natural Language Service. With the help of a custom machine learning model built using Watson Knowledge studio, the data will have additional enrichments that will provide detailed insights for further order processing.

### Included Components

* [Watson Knowledge Studio](https://www.ibm.com/watson/services/knowledge-studio/): Teach Watson the language of your domain with custom models that identify entities and relationships unique to your industry, in unstructured text. Use the models in Watson Discovery, Watson Natural Language Understanding, and Watson Explorer.

* [Natural Language Understanding](https://www.ibm.com/cloud/watson-natural-language-understanding): Analyze text to extract metadata from content such as concepts, entities, keywords, categories, sentiment, emotion, relations, and semantic roles using natural language understanding.


## 1. Create entities using Knowledge Studio workspace

Launch the **Watson Knowledge Studio** tool and click on **Create entities workspace**.

![create_wks_workspace](doc/source/images/create-wks-workspace.png)

Give a unique name and press **Create**.


https://cloud.ibm.com/docs/knowledge-studio/typesystem.html#wks_typesystem

## 2. Upload the type System
A type system are the entitiy labels that allows user to define that are specific to review documents, such as Customer Name, Order details and Place names, etc.. The type system controls how content can be annotated by defining the types of entities that can be labeled and how relationships among different entities can be labeled.

To upload our pre-defined type system, from the **Assets** -> **Entity Types** panel, press the **Upload** button to import the Type System file [data/types-2aa46ad0-31da-11e8-89a9-efc0f3b77492.json](data/types-2aa46ad0-31da-11e8-89a9-efc0f3b77492.json) found in the local repository.

![upload_type_system](doc/source/images/upload-type-system.png)

Press the **Upload** button. This will upload a set of Entity Types and Relation Types:

![wks_entity_types](doc/source/images/entity-types.png)

![wks_relation_types](doc/source/images/relation-types.png)

### Entities

Find people, places, events, and other types of entities mentioned in your content.

The entitiies that we want to extract are :

* **Entities**: ADDRESS, ORDER_ITEMS, CUSTOMER_NAME, CUSTOMER_PHONE.

With Watson Knowledge Studio, a machine learning annotator can be trained to recognize mentions of custom entity and relation types which can then be incorporated into the Watson Natural Language Service process.
Deploying a machine learning model to IBM Watson Natural Language Understanding When you are satisfied with the performance of the model, you can deploy a version of it to IBM Watson Natural Language Understanding. This feature enables your applications to use the deployed machine learning model to analyze semantic features of text input, including entities and relations. 

> This code pattern uses a generated text that we recieved from the converted conversation between the User and Automatic call responder(Bot).
When the reader has completed this code pattern, they will understand how to:

* Use effectively use Watson Knowledge Studio custom model to identify metadata from unstructured text.
* Define entity types, import documents, annotate documents, create, train and evaluate machine learning models.
* Deploy a Watson Knowledge Studio model to Watson Natural Language Understanding.
* Query NLU for identifying metadata from given text file.

![architecture](doc/source/images/architecture.png)

## Flow

1. A sample set of review documents are loaded into Watson Knowledge Studio for annotation.
1. A Watson Knowledge Studio model is created.
1. The Watson Knowledge Studio model is applied to a Watson Discovery service instance.
1. The food review json files are added to the Discovery collection.
1. The user interacts with the backend server via the app UI. The frontend app UI uses React to render search results and can reuse all of the views that are used by the backend for server side rendering. The frontend is using semantic-ui-react components and is responsive.
1. User input is processed and routed to the backend server, which is responsible for server side rendering of the views to be displayed on the browser. The backend server is written using express and uses express-react-views engine to render views written using React.
1. The backend server sends user requests to the Watson Discovery Service. It acts as a proxy server, forwarding queries from the frontend to the Watson Discovery Service API while keeping sensitive API keys concealed from the user.

> NOTE: see [DEVELOPING.md](DEVELOPING.md) for project structure.

## Included components

* [Watson Discovery](https://www.ibm.com/watson/services/discovery/): A cognitive search and content analytics engine for applications to identify patterns, trends, and actionable insights.
* [Watson Knowledge Studio](https://www.ibm.com/watson/services/knowledge-studio/): Teach Watson the language of your domain with custom models that identify entities and relationships unique to your industry, in unstructured text. Use the models in Watson Discovery, Watson Natural Language Understanding, and Watson Explorer.


# Steps

1. [Clone the repo](#1-clone-the-repo)
1. [Create IBM Cloud services](#2-create-ibm-cloud-services)
1. [Create a Watson Knowledge Studio workspace](#3-create-a-watson-knowledge-studio-workspace)
1. [Upload Type System](#4-upload-type-system)
1. [Import Corpus Documents](#5-import-corpus-documents)
1. [Create the model](#6-create-the-model)
1. [Deploy the machine learning model to Watson Natural Language Understanding](#7-deploy-the-machine-learning-model-to-watson-natural-language-understanding)
1. [Create Discovery Collection](#8-create-discovery-collection)
1. [Deploy the application](#9-deploy-the-application)

## 1. Clone the repo

```bash
git clone https://github.com/IBM/watson-discovery-food-reviews
```

## 2. Create IBM Cloud services

Create the following services:

* [**Watson Discovery**](https://cloud.ibm.com/catalog/services/discovery)
* [**Watson Knowledge Studio**](https://cloud.ibm.com/catalog/services/knowledge-studio)

## 3. Create a Watson Knowledge Studio workspace

Launch the **Watson Knowledge Studio** tool and click on **Create entities and relations workspace**.

![create_wks_workspace](doc/source/images/create-wks-workspace.png)

Enter a unique name and press **Create**.

## 4. Upload Type System

A type system allows us to define things that are specific to review documents, such as product and brand names. The type system controls how content can be annotated by defining the types of entities that can be labeled and how relationships among different entities can be labeled.

To upload our pre-defined type system, from the **Assets** -> **Entity Types** panel, press the **Upload** button to import the Type System file [data/types-2aa46ad0-31da-11e8-89a9-efc0f3b77492.json](data/types-2aa46ad0-31da-11e8-89a9-efc0f3b77492.json) found in the local repository.

![upload_type_system](doc/source/images/upload-type-system.png)

Press the **Upload** button. This will upload a set of Entity Types and Relation Types:

![wks_entity_types](doc/source/images/entity-types.png)

![wks_relation_types](doc/source/images/relation-types.png)

## 5. Import Corpus Documents

Corpus documents are required to train our machine-learning annotator component. For this code pattern, the corpus documents will contain sample review documents.

From the **Assets** -> **Documents** panel, press the **Upload Document Sets** button to import a Document Set file. Use the corpus documents file [data/watson-discovery-food-reviews/data/corpus-2aa46ad0-31da-11e8-89a9-efc0f3b77492.zip](data/watson-discovery-food-reviews/data/corpus-2aa46ad0-31da-11e8-89a9-efc0f3b77492.zip) found in the local repository.

> NOTE: Select the option to "upload corpus documents and include ground truth (upload the original workspace's type system first)"

![import_corpus](doc/source/images/import-corpus.png)

Once uploaded, you should see a set of documents:

![wks_document_set](doc/source/images/document-set.png)

## 6. Create the model

Since the corpus documents that were uploaded were already pre-annotated and included ground truth, it is possible to build the machine learning annotator directly without the need for performing human annotations.

Go to the **Machine Learning Model** -> **Performance** panel, and press the **Train and Evaluate** button.

![wks_training_sets](doc/source/images/training-sets.png)

From the **Document Set** name list, select the annotation sets **Docs28.csv** and **Docs122V2.csv**. Also, make sure that the option **Run on existing training, test and blind sets** is checked.  Press the **Train & Evaluate** button.

This process may take several minutes to complete. Progress will be shown in the upper right corner of the panel.

You can view the log files of the process by clicking the **View Log** button.

Once complete, you will see the results of the train and evaluate process:

![wks_training_complete](doc/source/images/training-complete.png)

## 7. Deploy the machine learning model to Watson Natural Language Understanding

Now we can deploy our new model to the already created **Watson Natural Language Understanding** service. Navigate to the **Versions** menu on the left and press **Create Version**.

![wks_snapshot_page](doc/source/images/snapshot-page.png)

The new version will now be available for deployment to Watson Discovery.

![wks_model_version](doc/source/images/model-versions.png)

To start the process, click the **Deploy** button associated with your version.

![wks_deployment_options](doc/source/images/deployment-options.png)

Select the option to deploy to **Natural Language Understanding**.

![wks_deployment_location](doc/source/images/deployment-location.png)

Enter your IBM Cloud account information to locate your **Discovery** service to deploy to.

Once deployed, a **Model ID** will be created. Keep note of this value as it will be required later when configuring your credentials.

![wks_deployment_model](doc/source/images/deployment-model.png)

> NOTE: You can also view this **Model ID** by clicking the **Deployed Models** link under the model version.

## 8. Create Discovery Collection

Launch the **Watson Discovery** tool. Create a new data collection by clicking the **Upload you own data** button. Enter a unique name to create your collection.

![disco_create_collection](doc/source/images/create-collection.png)

Creating the **Discovery Collection** and populating the .env file with the appropriate credentials is all that is required to deploy and run the app. Once started, the app will load all of the data files into your collection. For details on how to do this manually, go to the [Discovery collection configuration details](#discovery-collection-configuration-details) section below.

To locate your `environment_id` and `collection_id` values for your collection, click the drop-down button at the top of your collection panel.

![find_disco_ids](doc/source/images/find-disco-ids.png)

To locate the service credentials for your discovery service, click on the **Service Credentials** tab.

![get_disco_creds](doc/source/images/get-disco-creds.png)

## 9. Deploy the application

There are several ways to deploy the app. Each requires that you provide the necessary credentials for both your Watson Discovery and Watson Knowledge Studio services (see above for how to retrieve the credentials).

Click on one of the options below for instructions on deploying the app.

|   |   |   |
| - | - | - |
| [![openshift](https://raw.githubusercontent.com/IBM/pattern-utils/master/deploy-buttons/openshift.png)](doc/source/openshift.md) | [![public](https://raw.githubusercontent.com/IBM/pattern-utils/master/deploy-buttons/cf.png)](doc/source/cf.md) | [![local](https://raw.githubusercontent.com/IBM/pattern-utils/master/deploy-buttons/local.png)](doc/source/local.md) |

# Sample UI layout

![sample_output](doc/source/images/sample-output.png)

# Discovery collection configuration details

For reference, the following screen-shots detail how to set-up a collection configuration and load data files. In this code pattern, this process is completed for you when the application is initially started, but it is important to know what is happening in the background.

If you were to create the configuration manually, these are the steps you would take:

Launch the **Watson Discovery** tool. Create a new data collection by clicking the **Upload you own data** button. Enter a unique name to create your collection.

![disco_create_collection](doc/source/images/create-collection.png)

From the new collection data panel, click the **Configure Data** button at the top of the panel. Then select the **Enrich fields** tab.

![enrich_fields_panel](doc/source/images/enrich-fields-panel.png)

You can see that as a default, there are several enrichments that will be applied to your data collection. But we need to add to this list.

Click on **Add enrichments**.

At the top of the list, select **Keyword Extraction**.

![keyword_extraction](doc/source/images/keyword-extraction.png)

At the bottom of the list, select both **Entity Extraction** and **Relation Extraction**. Enter the **Model ID** that we created in **Watson Knowledge Studio**.

Close the enrichments window.

Click **Apply changes to collection** to start the process of loading the discovery files.

![select_disco_files](doc/source/images/select-disco-files.png)

Drag and drop your documents here or browse to your local computer files to load the collection with the json files located in [data/food_reviews](data/food_reviews/).

> NOTE: If using the **Discovery Lite** plan, you are limited to loading up to 1000 files into your discovery service. This limit is not per collection, but the combined number for all collections in your service.

# Troubleshooting

* Error when loading files into Discovery

  > Loading all 1000 document files at one time into Discovery can sometimes lead to "busy" errors. If this occurs, start over and load a small number of files at a time.

* No keywords appear in the app

  > This can be due to not having a proper configuration file assigned to your data collection. See [Step 5](#5-import-corpus-documents) above.

# Links

* [Demo on Youtube](https://www.youtube.com/watch?v=todo): Watch the video
* [Watson Node.js SDK](https://github.com/watson-developer-cloud/node-sdk): Download the Watson Node SDK.
* [Discovery Search UI](https://github.com/IBM/watson-discovery-ui): A sample UI that this repo is based on.
* [Kaggle dataset](https://www.kaggle.com/snap/amazon-fine-food-reviews): A dataset of Amazon reviews.

# Learn more

* **Artificial Intelligence Code Patterns**: Enjoyed this code pattern? Check out our other [AI Code Patterns](https://developer.ibm.com/technologies/artificial-intelligence/).
* **AI and Data Code Pattern Playlist**: Bookmark our [playlist](https://www.youtube.com/playlist?list=PLzUbsvIyrNfknNewObx5N7uGZ5FKH0Fde) with all of our code pattern videos
* **With Watson**: Want to take your Watson app to the next level? Looking to utilize Watson Brand assets? [Join the With Watson program](https://www.ibm.com/watson/with-watson/) to leverage exclusive brand, marketing, and tech resources to amplify and accelerate your Watson embedded commercial solution.

# License

This code pattern is licensed under the Apache Software License, Version 2.  Separate third party code objects invoked within this code pattern are licensed by their respective providers pursuant to their own separate licenses. Contributions are subject to the [Developer Certificate of Origin, Version 1.1 (DCO)](https://developercertificate.org/) and the [Apache Software License, Version 2](https://www.apache.org/licenses/LICENSE-2.0.txt).

[Apache Software License (ASL) FAQ](https://www.apache.org/foundation/license-faq.html#WhatDoesItMEAN)
