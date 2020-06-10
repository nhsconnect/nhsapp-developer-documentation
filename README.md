# NHS App Developer Documentation

NHS App developer documentation is still in progress and will be published in the near future.


Documentation is hosted on GitHub pages: [https://nhsconnect.github.io/nhsapp-developer-documentation/](https://nhsconnect.github.io/nhsapp-developer-documentation/)

## Running locally

NHS App developer documentation uses [11ty](https://www.11ty.dev/).

To run locally:

* ```npm install```

* ```npm start```

* ```npm build``` to build

## Deployment

This repo is automatically deployed when code is merged to develop. Deployment code is located in `.github/workflows/11ty-build.yml.`


## Editing on GitHub.com

1. Click on the **src** file [here](https://github.com/nhsconnect/nhsapp-developer-documentation/tree/develop/src)
2. Find the file that you would like to edit and click into it
3. Click the **"Edit" icon button** at the top right of the page
4. When you are finished making your changes, click **Propose file change** at the bottom of the page
5. You will see a box that will allow you to add a title and description to your changes
6. Add a title. The description is optional
7. Now click the green button that says **"Create Pull Request"**. Now your changes are in a queue waiting to be accepted by an admin
8. If you wish to cancel your request to make changes, click **"Close Pull Request"**

## Creating a new page on GitHub.com

1. Click on the **src** file [here](https://github.com/nhsconnect/nhsapp-developer-documentation/tree/develop/src)
2. Click the **"Create new file" button** at the top right of the page
3. Give your file an appropriate name, making sure to end this name with **.md**
4. When you are finished making your changes, click **Propose new file** at the bottom of the page
5. You will see a box that will allow you to add a title and description to your changes
6. Add a title. The description is optional
7. Now click the green button that says **"Create Pull Request"**. Now your changes are in a queue waiting to be accepted by an admin
8. If you wish to cancel your request to make changes, click **"Close Pull Request"**

## Before you start writing

1. Make sure before you begin writing your document, that you copy and paste the following block of text to the very top of your document
2. Make sure that where it states **"Your title"**, you change it to whatever you would like your title to be

```
---
layout: base.njk
title: Your title
---

```

Making sure your document contains this at the very top means that your contribution will automatically use the site theme.
