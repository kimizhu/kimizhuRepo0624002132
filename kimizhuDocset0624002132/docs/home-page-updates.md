# Making home page updates

The [docs.microsoft.com](https://docs.microsoft.com) home page is a specially constructed page that requires updates to both the page and images themselves and the templates. 

## Access requirements
You'll need access to the following repos:

* [DocsRoot](https://github.com/MicrosoftDocs/DocsRoot/tree/master/DocsCoreContent)
* [Templates.Docs](https://mseng.visualstudio.com/VSChina/_git/Template.Docs)

## Process

### Images
The design team will create the images and provide them. The following resources are needed for each card:

* SVG file
* PNG file
* Hexadecimal value of hover color

The images are placed in the [DocsRoot/DocsCoreContent/images](https://github.com/MicrosoftDocs/DocsRoot/tree/master/DocsCoreContent/images) folder rather than in DocsMedia. They should be named logo-*service-name* as per the existing convention.

### Card updates

Update the cards using the existing model.

```
            <li>
                <a href="/*link*/" title="*title*">
                <div class="cardSize">
                    <div class="cardPadding">
                        <div class="card">
                            <div class="cardImageOuter *docset*">
                                <div class="cardImage"> 
                                    <img data-hoverimage="images/logo-*docset*.svg" src="images/logo-*docset*.png" alt="*title*" />
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                </a>
            </li>
```

Check in the files as you normally would, either pushing directly to a branch in the DocsRoot repo to test or in a pull request.

### Template updates

Every new card in the home page requires a hover color that needs to be updated in the Template.Docs project; this is in a VSTS repo so the pull request UI is different. The file to update is Templates.Docs/global/stylesheets/**hubpage.scss**. The lines are at approximately position 450-500.

```
.siteHome .ready .*service*:hover, .siteHome .ready .*service*:active {
    background-color: #e3008c;
}
```

> [!NOTE]
> Each button/service has an active behavior and a hover behavior, which should be the same unless otherwise specified by design.

Once the updates are complete for the added or updated card behaviors, submit a pull request against the ```develop``` branch and select **[VSChina]\Docs Site and Services** as the reviewer.

Once the pull request has been approved, you can then preview the behavior in the develop branch. Contact Duncan or just wait to know when the update gets pushed into master.

You won't be able to preview the template updates locally because they need to get pushed from the VSTS repo to the GitHub repo that actually defines the styles for Docs.

### Pushing live
Once the template update is in master, you can then update the master branch of the DocsRoot repo and push live.