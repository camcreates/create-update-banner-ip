# Self-Hosted Banner Ads

This document is meant to outline the process for adding and updating a banner ad to the site without using a third-party embed such as Bannersnack.  
* * *

## Adding a Banner

This section will walk through the process of initally implementing a banner.

### Add image

Since Squarespace does not have a central image repository to upload images to, we will need to add the image to a content block and then grab the generated URL to use for the banner.

1. While logged in, click **Pages**, and scroll down until you find the **Test Page**. Click it to edit.
2. Hover your mouse over the empty section of the page to trigger the **Page Content** bar and click **Edit**.
3. Click the **+** icon on the right side of the window that appears and select **Image** from the menu.
4. Upload the desired image. Make sure it has been [optimized](https://github.com/camcreates/imageoptim-use/blob/master/imageoptim-process.md), first, if necessary.
5. Click **Apply** to add the image and click **Save** to finish creating the content block.
6. Right click the image and select **Copy Image Address**. Paste this someplace you can refer back to it later.

![Process for adding a source image][add-img]

[add-img]: img/banner-process-add-img.gif "Adding an image"

### Add CSS

1. While logged in, click **Design**.
2. Click **Custom CSS**.
3. Copy the following CSS and paste it at the end of the existing custom CSS and click **Save** when done.

```css
    .top-ad {
        padding-top: 20px;
        padding-bottom:20px;
        max-width: 90vw;
        position: relative;
        display: inline-block;
        left: 50%;
        transform: translate(-50%);
    }
```

![Process for adding custom CSS][add-css]

[add-css]: img/banner-process-add-css.gif "Adding custom CSS"

### Add Script

1. While logged in, click **Settings**.
2. Click **Advanced**.
3. Click **Code Injection**.
4. Copy and paste the following code into the **Footer** field and click **Save** when done.

```javascript
    <script>
        function docReady(fn) {
            // see if DOM is already available
            if (document.readyState === "complete" || document.readyState === "interactive") {
                // call on next available tick
                setTimeout(fn, 1);
            } else {
                document.addEventListener("DOMContentLoaded", fn);
            }
        }
        docReady(function() {
            var container = document.getElementById('canvas');
            var link = document.createElement('a');
            var img = document.createElement('img');
            link.href = 'INSERT CAMPAIGN LINK HERE';
            img.src = 'INSERT IMAGE LINK HERE';
            img.classList.add('top-ad');
            img.setAttribute('alt', 'Insert alt text here.');
            link.appendChild(img);
            container.insertBefore(link, container.firstChild);
        });
    </script>
```

![Process for adding custom JS][add-js]

[add-js]: img/banner-process-add-js.gif "Add custom JS"

## Updating a Banner

This section will walk through the process of updating or replacing an already implemented banner.

### Add/Update Image Source

1. While logged in, click **Settings**.
2. Click **Advanced**.
3. Click **Code Injection**.
4. Find the line that begins with **img.src**.
5. Replace 'INSERT IMAGE LINK HERE' after the **=** symbol with the new image link. Be sure not to remove the single quotation marks that go around the URL.
6. If you do not need to update the ad link, click **Save** to complete the process, otherwise continue to the next set of steps.

### Add/Update Ad Link

1. On the same page, find the line that begins with **img.setAttribute**.
2. Replace 'INSERT CAMPAIGN LINK HERE' after the **=** symbol with the new ad URL. Be sure not to remove the single quotation marks that go around the URL.
3. If you do not need to update the alt text, click **Save** to complete the process, otherwise continue to the next set of steps.

### Add/Update Alt Text

1. On the same page, find the line that begins with **link.href**.
2. Replace 'Insert alt text here.' after **'alt'** with alt text appropriate for the current image.
3. Click **Save** when done.

![Process for updating banner links][update-links]

[update-links]: img/update-links.gif "Update links"

## Additional Notes

* Avoid using the words banner or ad in the filename for the image. This will help decrease the chances of it getting flagged by ad blockers.

# Google Campaign Manager Ads

These need to be implemented completely differently from our self-hosted ads, and require the use of Javascript embedding.

**Note:** At the moment, only run one of these scripts at a time. If and when an updated implementation that accommodates both is written, this documentation will be updated accordingly.

## Adding the script
1.  While logged in, click  **Settings**.
2.  Click  **Advanced**.
3.  Click  **Code Injection**.
4.  Copy and paste the following code into the  **Footer**  field and click  **Save**  when done and check to ensure the ad appears.
```
    <script>
	    var container = document.getElementById('canvas');
	    var insCode = document.createElement('ins');
	    var img = document.createElement('img');
	    insCode.classList.add('dcmads', 'top-ad');
	    insCode.setAttribute('style', 'display:inline-block; width:728px; height:90px')
	    insCode.setAttribute('data-dcm-placement', 'INSERT HERE')
	    insCode.setAttribute('data-dcm-rendering-mode', 'iframe')
	    insCode.setAttribute('data-dcm-https-only', '')
	    insCode.setAttribute('data-dcm-gdpr-applies', 'gdpr=${GDPR}')
	    insCode.setAttribute('data-dcm-gdpr-consent', 'gdpr_consent=${GDPR_CONSENT_755}')
	    insCode.setAttribute('data-dcm-addtl-consent', 'addtl_consent=${ADDTL_CONSENT}')
	    insCode.setAttribute('data-dcm-resettable-device-id', '')
	    insCode.setAttribute('data-dcm-app-id', '')
	    container.insertBefore(insCode, container.firstChild);
	    console.log(insCode);
	    console.log(container);
    </script>
```    


## Script Walkthrough

This section will go through the script to explain what to edit. The majority of the script probably will not need to be changed, but **make sure to compare the script with the code in the iFrames JavaScript tag to ensure all the data-dcm lines match.**

**Do not change these three lines of code**

	var container = document.getElementById('canvas');
	var insCode = document.createElement('ins');
	var img = document.createElement('img');

**Ensure the first class in the following classlist (in this case, 'dcmads') matches the class defined in the tag in the excel sheet (Example: ins class="dcmads"). Leave 'top-ad' as is.**

	insCode.classList.add('dcmads', 'top-ad');
**Ensure the style matches that defined in the tag in the excel sheet (Example: style='display:inline-block; width:728px; height:90px').**

	insCode.setAttribute('style', 'display:inline-block; width:728px; height:90px')

**The following line is the most important to pay attention to. Replace the 'INSERT HERE' text with the data-dcm-placement data defined in the Excel sheet (Example: 'N428001.4021460INSIDEPHILANTHROP/B25272336.293069126'**

	insCode.setAttribute('data-dcm-placement', 'INSERT HERE')
	
**Confirm that the following lines match the associated data in the Excel sheet. Only update them if necessary. If any need to be updated, be sure to only change the data between the second set of single quotation marks. If any need to be added, copy one of these lines and modify both sets of data as necessary. If any needs to be removed, delete or comment out the line. (A line can be commented out by typing // at the beginning of it.)**

	insCode.setAttribute('data-dcm-rendering-mode', 'iframe')
	insCode.setAttribute('data-dcm-https-only', '')
	insCode.setAttribute('data-dcm-gdpr-applies', 'gdpr=${GDPR}')
	insCode.setAttribute('data-dcm-gdpr-consent', 'gdpr_consent=${GDPR_CONSENT_755}')
	insCode.setAttribute('data-dcm-addtl-consent', 'addtl_consent=${ADDTL_CONSENT}')
	insCode.setAttribute('data-dcm-resettable-device-id', '')
	insCode.setAttribute('data-dcm-app-id', '')

**Do not change these three lines of code.**

	container.insertBefore(insCode, container.firstChild);
	console.log(insCode);
	console.log(container);
