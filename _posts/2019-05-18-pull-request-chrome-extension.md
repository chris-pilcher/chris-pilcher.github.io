---
layout: post
current: post
cover:  assets/images/pull-request-chrome-extension/cover.jpg
navigation: True
title: "My First Chrome Extension"
date: 2019-05-18 10:00:00
class: post-template
subclass: 'post tag-getting-started'
author: chris
---

# Introduction

We have a naming convention for the title of pull requests at work.

Example title: `feature/turn-off-line-endings to develop`

Using this convention keeps pull request titles consistent and let reviewers easily see the source and destination branch before opening the pull request.

# The Problem

Manually setting these titles is a hassle 😫

<iframe width="560" height="315" src="https://www.youtube.com/embed/VHuAQcZtMP8?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

# Solution 💡

I created a simple Chrome extension to generate the title automatically.  

![img]({{ '/assets/images/pull-request-chrome-extension/pr-title-chrome-extension-screenshot.png' | relative_url }})

<iframe width="560" height="315" src="https://www.youtube.com/embed/Mdlk2XhaXl8?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe><br>

# Code 👨‍💻

The extension is a `manifest.json`, `content.js`, libs and 3 icon files. The code is available on [github.com/chris-pilcher/pr-title-chrome-extension](https://github.com/chris-pilcher/pr-title-chrome-extension/).

`manifest.json`:
{% highlight json %}
{
  "manifest_version":2,
  "name":"Visual Studio PR Title Generator",
  "short_name":"VS Title Gen",
  "version":"0.0.0.3",
  "description":"Generate pull request titles in Visual Studio Online. Updates the PR title to \"[source branch name] to [destination branch name]\"",
  "content_scripts":[
    {
      "js":[
        "libs/jquery-3.4.1.min.js",
        "libs/purl.js",
        "content.js"
      ],
      "matches":[
        "https://*.visualstudio.com/*"
      ]
    }
  ],
  "icons":{
    "16":"icon16.png",
    "48":"icon48.png",
    "128":"icon128.png"
  }
}
{% endhighlight %}

`content.js`:
{% highlight javascript %}
// Represents information from the page
const page = {
  get titleInput() {
    return $(".vc-pullRequestCreate-title-container").find("input");
  },
  get sourceBranchName() {
    return $.url().param("sourceRef");
  },
  get targetBranchName() {
    return $.url().param("targetRef");
  },
  get isPullRequestCreatePath() {
    return $.url()
      .attr()
      .path.endsWith("pullrequestcreate");
  }
};

// Updates the title of the pull request
function updateTitle() {
  page.titleInput.val(`${page.sourceBranchName} to ${page.targetBranchName}`);

  // The follow two lines are required to change the value of an input made with reactjs.
  // See: https://stackoverflow.com/questions/54137836/change-value-of-input-made-with-react-from-chrome-extension/54138182
  page.titleInput[0].dispatchEvent(new Event("change", { bubbles: true }));
  page.titleInput[0].dispatchEvent(new Event("blur", { bubbles: true }));
}

// Called when changes are made to the DOM tree.
function handleMutation() {
  if (!page.isPullRequestCreatePath) return;

  const generateButtonNotVisible = !$("#generatePRTitle").length;
  if (generateButtonNotVisible) {
    const generateButton = $("<button>🖖</button>")
      .attr({ id: "generatePRTitle", title: "Generate PR title" })
      .click(updateTitle);

    page.titleInput.after(generateButton);
  }
}

const observer = new MutationObserver(handleMutation);
observer.observe(document.body, { childList: true });
{% endhighlight %}

# Publishing 🚀

Create a zip file

![img]({{ '/assets/images/pull-request-chrome-extension/create-zip-file.png' | relative_url }})

Upload to the Chrome Web Store  

![img]({{ '/assets/images/pull-request-chrome-extension/dashboard-upload.png' | relative_url }})

Fill in the details 

![img]({{ '/assets/images/pull-request-chrome-extension/dashboard-main.png' | relative_url }})

Publish to the store

[![img]({{ '/assets/images/pull-request-chrome-extension/store-listing.png' | relative_url }})](https://chrome.google.com/webstore/detail/visual-studio-pr-title-ge/lbkfohchcccpbmgckjbcgcnlmohdieej)

[Visual Studio PR Title Generator - Chrome Web Store](https://chrome.google.com/webstore/detail/visual-studio-pr-title-ge/lbkfohchcccpbmgckjbcgcnlmohdieej)

# Links

* I found [Build & Publish a Custom Google Chrome Extension - YouTube](https://www.youtube.com/watch?v=wHZCYi1K664) tutorial helpful 👍
* [Visual Studio PR Title Generator - Chrome Web Store](https://chrome.google.com/webstore/detail/visual-studio-pr-title-ge/lbkfohchcccpbmgckjbcgcnlmohdieej)
* [github.com/chris-pilcher/pr-title-chrome-extension](https://github.com/chris-pilcher/pr-title-chrome-extension/)
