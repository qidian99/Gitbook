# ISAFE Onboarding Documentation

## Sample Tutorial Project

* To help you start with Drupal 7, below this is the tutorial I made based on Drupal 7 documentation, Drupalize.me platform, and the Drupal module development experience from our engineering team.  [https://github.com/qidian99/drupal\_tutorial](https://github.com/qidian99/drupal_tutorial)
* The sample project consists of 13 parts, corresponding to tags v1.0.1 to v1.0.13. You can pull the tags and it will contain the snapshot of the project at the respective tags. I've also recorded Loom videos for this sample project, and it can be found [here](https://loom.com/share/folder/eb332b92389f4b1ab9b25b5df2c4deef). It's my first time to record these tutorials and so if you need any clarification or find any problem feel free to let me know and I will try my best to help.
* The supplementary tutorial videos and articles mentioned in the sample project is in [this](https://drive.google.com/drive/folders/1BSu_7RjX83d9vKFDIelfq-oso7VYFbtr?usp=sharing) Google Drive folder.

## ISAFE Direct Project

* After Joe adds your IP addresses to the Phabricator, you can access the docker container our ISAFE Direct project at the following link:  [https://phabricator.isafe.org/source/isdd/](https://phabricator.isafe.org/source/isdd/)
* It consists of three submodules as mentioned in the meeting, and there's documentation for how to replace the corresponding directory in the README file of the docker container above:
  * -- modules/custom: [https://phabricator.isafe.org/source/isdm/](https://phabricator.isafe.org/source/isdm/)
  * -- themes/: [https://phabricator.isafe.org/source/isdt/](https://phabricator.isafe.org/source/isdt/) 
  * -- schemas/: [https://phabricator.isafe.org/source/isds/](https://phabricator.isafe.org/source/isds/)
* There's also a step-by-step video recording to deploy the current snapshot of ISAFE Direct in [this](https://loom.com/share/folder/61cb50a00a1149e48ecbcccac9f72e55) Loom link. Again please feel free to reach out to me if there's any blocker encountered in deployment.

## Prototypes

Here are the links to our current XD design:

* ISAFE Direct \(Excluding My Ok\): [https://xd.adobe.com/view/9c669549-cb4a-427a-90c2-4b41ea36d741-c935/?fullscreen](https://xd.adobe.com/view/9c669549-cb4a-427a-90c2-4b41ea36d741-c935/?fullscreen) ISAFE Direct My Ok: [https://xd.adobe.com/view/0eb7e0a0-2272-4b68-9e8f-ee0c887acec8-5be4/?fullscreen](https://xd.adobe.com/view/0eb7e0a0-2272-4b68-9e8f-ee0c887acec8-5be4/?fullscreen)

## Development Tasks

* The development tasks fall into three categories: front-end, back-end, and data. Later this week I will connect with you and discuss which tasks to start work on. The first set of tasks are experimental in nature and we are going to figure out the best way as pertains to development practices.
* I will also try to set up web hooks that connect Phabricator with Discord, our communication tool used in development. Our Discord will also be set up to contain several channels including `Daily Status Update`, `Q&A`, `General`, `Development`, and `Creative`.

## Technology Stacks

### ISAFE Direct Web Platform

* Backend: Drupal 7, PHP, SQL
* Frontend: HTML, SCSS, CSS, Native JavaScript, jQuery, Preact \(same syntax as ReactJS\)

### ISAFE Direct My Ok, Mobile App

* Backend: Drupal 7 web services, PHP, SQL, Rest APIs.
* Frontend: React Native

### DigiYak, Mobile Social App

* Backend: NodeJS, MongoDB\(current\)/Postgres/SQL, Apollo, GraphQL, Strapi \(depends\), Apollo Server
* Frontend: React Native, Apollo Client



  




