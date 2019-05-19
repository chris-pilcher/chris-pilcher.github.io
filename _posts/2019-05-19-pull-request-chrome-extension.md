---
layout: post
current: post
cover:  assets/images/pull-request-chrome-extension/cover.jpg
navigation: True
title: "My first Chrome extension"
date: 2019-05-19 10:00:00
class: post-template
subclass: 'post tag-getting-started'
author: Chris Pilcher
---

# Introduction

My workplace has a naming convention for the title of pull requests. 

Example title: `feature/turn-off-line-endings to develop`

# The Problem

Manually setting these titles was a hassle 😫

<iframe width="640" height="385" src="https://www.youtube.com/embed/VHuAQcZtMP8" frameborder="0" allowfullscreen></iframe><br>

# Solution 💡

I created a simple Chrome extension to generate the title automatically.  

![img]({{ '/assets/images/pull-request-chrome-extension/pr-title-chrome-extension-screenshot.png' | relative_url }}){: .center-image }

<iframe width="640" height="385" src="https://www.youtube.com/embed/Mdlk2XhaXl8" frameborder="0" allowfullscreen></iframe><br>

# Code 👨‍💻

The extension is a `manifest.json`, `content.js` and 3 icon files.

>manifest.json
{:.filename}
{% highlight json linenos %}
{% raw %}
{
  "manifest_version": 2,

  "name": "Visual Studio PR Title Generator",
  "short_name": "VS Title Gen",
  "version": "0.0.0.1",
  "description": "Generate pull request titles in Visual Studio Online. Updates the PR title to \"[source branch name] to [destination branch name]\"",
  "content_scripts": [
  {
    "js": [ "content.js" ],
    "matches": [ "https://*.visualstudio.com/*/_git/*/pullrequestcreate*" ]
  }],
  "icons": {
    "16": "icon16.png",
    "48": "icon48.png",
    "128": "icon128.png"
  }
}
{% endraw %}
{% endhighlight json %}

>content.js
{:.filename}
{% highlight javascript linenos %}
{% raw %}
setInterval(() => {
  // If the "insert PR title" button doesn't exist then add it in
  if (!document.getElementById('customPRTitleGeneratorButton')) {
    let titleDiv = document.querySelector('.ms-TextField-fieldGroup');

    if (titleDiv) {
      let generateButton = document.createElement('button');
      generateButton.id = 'customPRTitleGeneratorButton';
      generateButton.title = 'Generate PR title';
      generateButton.innerText = '🖖';

      generateButton.addEventListener('click', () => {
        let titleInput = document.getElementsByClassName('ms-TextField-field')[0];

        let fromBranchName = document.getElementsByClassName('vss-PickListDropdown--title-text')[1].innerText;
        let toBranchName = document.getElementsByClassName('vss-PickListDropdown--title-text')[2].innerText;

        titleInput.value = fromBranchName + ' to ' + toBranchName;

        // https://stackoverflow.com/questions/54137836/change-value-of-input-made-with-react-from-chrome-extension/54138182
        titleInput.setAttribute('value', fromBranchName + ' to ' + toBranchName);
        titleInput.dispatchEvent(new Event('change', { bubbles: true }));
        titleInput.dispatchEvent(new Event('blur', { bubbles: true }));
      });
      titleDiv.appendChild(generateButton);
    }
  }
}, 1000); // check every second
{% endraw %}
{% endhighlight javascript %}

# Publishing 🚀

Create a zip file containing the code.

![img]({{ '/assets/images/pull-request-chrome-extension/create-zip-file.png' | relative_url }}){: .center-image }

Upload to the Chrome web store  

![img]({{ '/assets/images/pull-request-chrome-extension/dashboard-upload.png' | relative_url }}){: .center-image }

Fill in the details 

![img]({{ '/assets/images/pull-request-chrome-extension/dashboard-main.png' | relative_url }}){: .center-image }

Publish to the store

[![img]({{ '/assets/images/pull-request-chrome-extension/store-listing.png' | relative_url }}){: .center-image }](https://chrome.google.com/webstore/detail/visual-studio-pr-title-ge/lbkfohchcccpbmgckjbcgcnlmohdieej)

[Visual Studio PR Title Generator - Chrome Web Store](https://chrome.google.com/webstore/detail/visual-studio-pr-title-ge/lbkfohchcccpbmgckjbcgcnlmohdieej)

# Links

* I found [Build & Publish a Custom Google Chrome Extension - YouTube](https://www.youtube.com/watch?v=wHZCYi1K664) tutorial helpful 👍
* [Visual Studio PR Title Generator - Chrome Web Store](https://chrome.google.com/webstore/detail/visual-studio-pr-title-ge/lbkfohchcccpbmgckjbcgcnlmohdieej)