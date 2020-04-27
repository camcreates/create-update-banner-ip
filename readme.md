# Self-Hosted Banner Ads

This document is meant to outline the process for adding and updating a banner ad to the site without using a third-party embed such as Bannersnack.  
* * *

## Adding a Banner

This section will walk through the process of initally implementing a banner.

### Add image

Since Squarspace does not have a central image repository to upload images to, we will need to add the image to a content block and then grab the generated URL to use for the banner.

1. While logged in, click **Pages**, and scroll down until you find the **Test Page**. Click it to edit.
2. Hover your mouse over the empty section of the page to trigger the **Page Content** bar and click **Edit**.
3. Click the **+** icon on the right side of the window that appears and select **Image** from the menu.
4. Upload the desired image. Make sure it has been [optimized](https://github.com/camcreates/imageoptim-use/blob/master/imageoptim-process.md), first, if necessary.
5. Click **Apply** to add the image and click **Save** to finish creating the content block.
6. Right click the image and select **Copy Image Address**. Paste this someplace you can refer back to it later.
7. Since we do not actually need the image on a page like this, click **Edit** again to go back into the content block.
8. Hover over the image and click **Delete** when the option appears. Click **Yes** to confirm. This will remove the image from the page but it will remain in the system.
9. Click **Save**.

![Process for adding a source image][add-img]

[add-img]: img/banner-process-add-img.gif "Adding an image"

### Add CSS

1. While logged in, click **Design**.
2. Click **Custom CSS**.
3. Copy the following CSS and paste it at the end of the existing custom CSS and click **Save** when done.

```css
    .submittable-ad {
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
            link.href = 'https://www.submittable.com/work-from-home/?utm_campaign=grants&utm_medium=ppc&utm_source=insidephilanthropy&utm_type=display&utm_content=bannerad&utm_adgrp=april';
            img.src = 'https://static1.squarespace.com/static/57d9e959d482e972e8434364/t/5ea122363b46f95d976a0bb1/1587618359108/sbmtbl.png';
            img.classList.add('submittable-ad');
            link.appendChild(img);
            container.insertBefore(link, container.firstChild);
        });
    </script>
```

![Process for adding custom JS][add-js]

[add-js]: img/banner-process-add-js.gif "Add custom JS"

## Updating a Banner

This section will walk through the process of updating or replacing an already implemented banner.

### Update Image Source

1. While logged in, click **Settings**.
2. Click **Advanced**.
3. Click **Code Injection**.
4. Find the line that begins with **img.src**.
5. Replace the image link after the **=** symbol with the new image link. Be sure not to remove the single quotation marks that go around the URL.
6. If you do not need to update the ad link, click **Save** to complete the process, otherwise continue to the next set of steps.

### Update Ad Link

1. On the same page, find the line that begins with **link.href**.
2. Replace the link after the **=** symbol with the new ad URL. Be sure not to remove the single quotation marks that go around the URL.
3. Click **Save** when done.

![Process for updating banner links][update-links]

[update-links]: img/update-links.gif "Update links"

## Additional Notes

* Avoid using the words banner or ad in the filename for the image. This will help decrease the chances of it getting flagged by ad blockers.
