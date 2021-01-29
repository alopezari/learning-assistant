# Introduction
**Learning Assistant** is a Postman based tool that will **help you keep improving** as a software engineer, no matter the field you specialize in or your degree of expertise.

By just providing the areas you're interested on, Learning Assistant will recommend periodically a **set of resources** for you, your friends or your team to get up to date with the current state of art in software development.

This set of resources consists on:
- A _Github_ repository.
- A _Docker_ image.
- An article from _Dev.to_.
- A talk streamed in _Youtube_.
- A course from _Udemy_.

You can find it in the [Learning Assistant's Postman public workspace](https://www.postman.com/alopezari/workspace/learning-assistant/documentation/1558965-71c39b5f-b9e5-45c4-9748-2edb8bb98f0a).

# How it works

A **Postman collection** with chained and scripted requests **gathers the information** from all these sites where the resources belong to and **dumps it on a Slack channel**. A **Postman monitor** runs this collection periodically so that you get your information of interest **whenever you want**. It can be every day, every Friday, once a month... _Your Learning Assistant, your rules._

Of course, a different set of resources will be dump each time the Postman monitor is run.

# Considerations

In order to set up your Learning Assistant, you need a **Postman environment** to fill the required environment variables. It is recommended to duplicate the **Learning Assistant - Template** environment, rename it and fill it with your data.

Some environment variables will be updated at runtime, persisting new information for the subsequent monitor run. Considering this, the Postman environment must be updated (at the end of the monitor) and retrieved (at the beginning) using the **Postman API**. In order to do so, the **Environment UID Finder** collection should be run manually before setting up the Learning Assistant monitor so that the chosen Postman environment UID is stored on it. This environment UID will be used to call Postman API at the begginning and the end of the monitor run.

# Setting up your Learning Assistant

1. Duplicate the environment **Learning Assistant - Template**, rename it with the name you want and select it in your Postman IDE.
2. Run the **Environment UID Finder** collection (that you can find in the Learning Assistant public workspace). You can do it by running each request manually or just using the Collection Runner. Check all tests passed and verify the _postmanEnvironmentUid_ is populated in your active environment.
3. **Fill your Postman environment** with the required data (see each request details in this documentation to find out which environment variables are optional or mandatory).
4. Set up a **Postman monitor** for the Learning Assistant collection that use your Postman environment.

# Learning Assistant collection flow in a nutshell

Each time the Postman monitor is run, the following will happen:
1. The current environment is updated with the environment details retrieved from the Postman API. This is needed in case this environment was updated in the previous run.
2. **Repository** details are retrieved from Github API. This is the _first resource_.
3. **Docker image** details are retrieved from Docker Hub API. _Second resource_.
4. **Article** details are retrieved from Dev.to API. _Third resource_.
5. Talk's video ID is retrieved from Youtube Search API. Initial step of a 2-step process to retrieve the resource from Youtube.
6. **Talk** details are retrieved from Youtube API using the video ID acquired in the previous step. _Fourth resource_.
7. **Course** details are retrieved from Udemy API. _Fifth resource_.
8. An inspirational quote is retrieved from Type.fit API.
9. The current environment is persisted using the Postman API. This is necessary because some variables have been updated in the previous step to provide different resources in the next monitor run.
10. Gathered resources and quote are sent to your Slack channel.

# Contribution

This a public workspace so please **feel free to contribute** with fixes and improvements. Any help is welcomed to build **the best learning recommendation tool in the Postman ecosystem** so if you have any great idea to make Learning Assistant better: fork the collection (or add new ones), add your touch and let's start collaborating.

#### Example of potential improvements:

- Currently, all requests retrieve resources dynamically and randomnly from related APIs except the _Dev.to_ and _Youtube_ ones, which retrieve resources sequentially by following an order because of the way these APIs are built. It would be greate to find a way so that these resources are retrieved dynamically and randomly too.
- Apart from Slack, implement a way to dump the collected resources to an email address (for example, by using Gmail API), just in case the user does not have Slack or prefers this method.
- Add new articles sources like Medium (by using Medium API) or personal blogs.
- Add a way to also recommend Postman public workspaces.
- Improve the recommendation algorithm so that it delivers most relevant resources more frequently.

# Contact

For any request, suggestion or inquiries you can contact me at:
- [alopezari](https://twitter.com/alopezari) @ Twitter
