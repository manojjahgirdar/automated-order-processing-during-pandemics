# AI Powered Order Processing During Pandemics

[![License](https://img.shields.io/badge/License-Apache2-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0)

This GitHub Repo consists of Team Technocogent Submission for Call for Code 2020 - Covid19 Track.

## Contents

1. [Short description](#short-description)
1. [Demo video](#demo-video)
1. [The architecture](#the-architecture)
1. [Long description](#long-description)
1. [Project roadmap](#project-roadmap)
1. [Getting started](#getting-started)
1. [Running the tests](#running-the-tests)
1. [Live demo](#live-demo)
1. [Built with](#built-with)
1. [Contributing](#contributing)
1. [Versioning](#versioning)
1. [Authors](#authors)
1. [License](#license)
1. [Acknowledgments](#acknowledgments)

## Short description

### What's the problem?

How do we stop panic amongst people of hoarding essentials during lockdown? How do we maintain social distancing while procuring essentials?

### How can technology help?

Schools and teachers can continue to engage with their students through virtual classrooms, and even create interactive spaces for classes. As parents face a new situation where they may need to homeschool their children, finding appropriate online resources is important as well.

### The idea

We propose a way of eliminating panic by using control towers powered by government entities which can cater to online and offline mode of taking orders from the residents of different locations. This method will help the government to take control of the situation, maintain adequate supply & price as per the demand. Government can have a hawkeye view of the situation and can release updates via news through our channel to provide assurance which can go a long way in bringing the stability to the system.

## Demo video

[![](https://img.youtube.com/vi/QXxt4iH8tNY/0.jpg)](https://youtu.be/QXxt4iH8tNY)

## The architecture

### Detailed end to end Architecture

![Architecture](/docs/img/architecture.jpg)

### Architecture

![Detailed](/docs/img/architecture-diagram.png)

1. The user navigates to the site and uploads a video file.
2. Watson Speech to Text processes the audio and extracts the text.
3. Watson Translation (optionally) can translate the text to the desired language.
4. The app stores the translated text as a document within Object Storage.

## Long description

[More detail is available here](DESCRIPTION.md)

## Getting started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

What things you need to install the software and how to install them

1. [Docker](https://docs.docker.com/get-docker/)
2. [Python](https://www.python.org/downloads/)

### Configure the Credentials

- Copy and paste the Watson Assistant `apikey`, `url` and `assistant-id` in the `credentials.json`.

<pre>
{
    "apikey": <b>"<YOUR-API-KEY>"</b>,
    "url": <b>"<YOUR-URL>"</b>,
    "assistant-id": <b>"<YOUR-ASSISTANT-ID>"</b>
}
</pre>

- Copy and paste IBM Db2 Credentials in `credentials1.json`.

### Run the application Locally

- Finally run the application with docker.

```bash
docker build .
docker run <IMAGE-NAME>
```

### Deploy the Applocation to Cloud

* Make sure you have installed [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started&locale=en-US) before you proceed.

* Log in to your IBM Cloud account, and select an API endpoint.
```bash
$ ibmcloud login
```

>NOTE: If you have a federated user ID, instead use the following command to log in with your single sign-on ID.
```bash
$ ibmcloud login --sso
```

* Target a Cloud Foundry org and space:
```bash
$ ibmcloud target --cf
```

* Push your app to IBM Cloud.
```bash
$ ibmcloud cf push 
```

* You will see output on your terminal as shown, verify the state is _`running`_:

```
Invoking 'cf push'...

Pushing from manifest to org manoj.jahgirdar@in.ibm.com / space dev as manoj.jahgirdar@in.ibm.com...

...

Waiting for app to start...

...

  state     since                  cpu     memory           disk           details
#0   running   2019-09-17T06:22:59Z   19.5%   103.4M of 512M   343.4M of 1G
```

## Live demo

You can find a running system to test at [covid-19-help-desk.mybluemix.net](https://covid-19-help-desk-talkative-bandicoot.eu-gb.mybluemix.net/)

## Built with

* [IBM Watson Assistant](https://cloud.ibm.com/catalog?search=cloudant#search_results) - Used to process the conversations for online order processing.
* [IBM Db2 on Cloud](https://cloud.ibm.com/catalog?search=cloud%20functions#search_results) - The SQL Database used to store the processed orders.
* [IBM Watson Knowledge Studio](https://cloud.ibm.com/catalog?search=api%20connect#search_results) - Used to train the entity extraction from the text model to process orders form the text.
* [IBM Watson Natural Language Understanding](http://www.dropwizard.io/1.0.2/docs/) - Used to deploy the model.
* [Flask](https://maven.apache.org/) - The web framework used.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags).

## Authors

* **Manoj Jahgirdar** - *[manojjahgirdar](https://github.com/manojjahgirdar)*
* **Manjula Hosurmath** - *[]()*
* **Rahul Reddy Ravipally** - *[RahulReddyRavipally](https://github.com/RahulReddyRavipally)*
* **Sharath Kumar RK** - *[RK-Sharath](https://github.com/RK-Sharath)*
* **Srikanth Manne** - *[]()*

## License

This project is licensed under the Apache 2 License - see the [LICENSE](LICENSE) file for details

## Acknowledgments

* Based on [Billie Thompson's README template](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2).
