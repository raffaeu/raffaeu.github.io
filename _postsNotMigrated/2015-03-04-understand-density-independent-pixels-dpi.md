---
id: 660
title: Understand Density Independent Pixels (DPI)
date: 2015-03-04T19:20:00+00:00
author: raffaeu
layout: post
guid: http://blog.raffaeu.com/?p=660
permalink: /archive/:year/:month/:day/:title/
categories:
  - Mobile
---
If you are working on a Mobile application (_using mobile CSS, native Android SDK or native iOS SDK_) the first problem you are going to face is the difference between the various screen sizes. For example, if you work with Android you will notice that different devices have different screen resolutions. Android categorize these devices in 4 different **buckets** called respectively MDPI, HDPI, XHDPI and XXHDPI

As I usually say, a picture is worth a thousands words:

[<img title="Figure 1" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Figure 1" src="http://blog.raffaeu.com/wp-content/uploads/f3da2e0137b2_A0EE/image_thumb.png" width="660" height="335" />](http://blog.raffaeu.com/wp-content/uploads/f3da2e0137b2_A0EE/image.png) So as you can see, in this case we have 4 devices with 4 different **pixels resolutions** but also 4 different **DPI** classifications.

### What is DPI and why we should care?

DPI stands for Dots per Inches, which can be translated in **how many pixels can be drawn into a screen for a given inch of screen’s space**. 

This measure is totally unbind to the screen size and to the pixel resolution so we can state that screens at different size and different resolution may be classified within the same DPI category and screens with same size but different resolution may be classified into different DPI category.

Assuming we are loading on our phone a raster picture of XX px wide, this is the result we will see using different DPI if we keep the image at the same size:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/f3da2e0137b2_A0EE/image_thumb_3.png" width="260" height="103" />](http://blog.raffaeu.com/wp-content/uploads/f3da2e0137b2_A0EE/image_3.png)

The blurring effect is caused by the fact that on a screen with 165dpi the amount of pixels drawn per inch is way lower (165) than on a 450dpi screen so the first thing that we loose is the sharpness of the image. 

### How Android works with DPI?

In android you can classify your device’s screens into 4 or more different **dpi buckets** which are used to classify the device’s screen depending on the amount of dpi and not the pixel resolution of the screen size. The picture below shows the available DPI classifications with a sample device for each category. You can find all the available DPI classification on this _lovely website_ <a href="http://dpi.lv/" target="_blank">DPI Love</a>.

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/f3da2e0137b2_A0EE/image_thumb_4.png" width="260" height="165" />](http://blog.raffaeu.com/wp-content/uploads/f3da2e0137b2_A0EE/image_4.png)

So for Android specifically, a device of 160 DPI has a ratio of 1:1 with the pixels on the screen while a device with more than 480 DPI has a ratio of 1:3 pixels on the screen compared to the same design for a 160 DPI screen.

Based on this classification we can now easily derive the following formula which can be used to calculate the real DPI resolution of a device based on its DPI classification and pixels resolution:

[<img title="image" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="image" src="http://blog.raffaeu.com/wp-content/uploads/f3da2e0137b2_A0EE/image_thumb_5.png" width="260" height="157" />](http://blog.raffaeu.com/wp-content/uploads/f3da2e0137b2_A0EE/image_5.png)

The Formula can be translated as **DP = PX * 160 / DPI**. So let’s make a couple of examples. 

> We want to design on the screen a square that should be 200px * 50px on our MDPI screen which we are using for mocking the UI (__this is what I call **default viewport**_)_

> **Note:** in Android SDK you will refer to **DP**&#160; to define a density independent measure and not DPI, this is why the previous formula has on the left side **px (pixels)** and **dp (density independent pixels)**.

Considering the previous list of devices (Figure 1) this is the result I come with in order to have the same aspect ration of my rectangle over multiple devices starting from an MDPI viewport:

<table cellspacing="0" cellpadding="2" width="400" border="0">
  <tr>
    <td valign="top" width="99">
      DEVICE
    </td>
    
    <td valign="top" width="61">
      DPI
    </td>
    
    <td valign="top" width="80">
      RATIO
    </td>
    
    <td valign="top" width="80">
      SIZE
    </td>
    
    <td valign="top" width="80">
      DP SIZE
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="99">
      GALAXY ACE
    </td>
    
    <td valign="top" width="61">
      MDPI
    </td>
    
    <td valign="top" width="80">
      1:1
    </td>
    
    <td valign="top" width="80">
      200 * 50 px
    </td>
    
    <td valign="top" width="80">
      200 * 50 dp
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="99">
      HTC DESIRE
    </td>
    
    <td valign="top" width="61">
      HDPI
    </td>
    
    <td valign="top" width="80">
      1:1.5
    </td>
    
    <td valign="top" width="80">
      200 * 50 px
    </td>
    
    <td valign="top" width="80">
      133 * 33 dp
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="99">
      NEXUS 7
    </td>
    
    <td valign="top" width="61">
      XHDPI
    </td>
    
    <td valign="top" width="80">
      1:2
    </td>
    
    <td valign="top" width="80">
      200 * 50 px
    </td>
    
    <td valign="top" width="80">
      100 * 25 dp
    </td>
  </tr>
  
  <tr>
    <td valign="top" width="99">
      NEXUS 6
    </td>
    
    <td valign="top" width="61">
      XXHDPI
    </td>
    
    <td valign="top" width="80">
      1:3
    </td>
    
    <td valign="top" width="80">
      200 * 50 px
    </td>
    
    <td valign="top" width="80">
      67 * 16 dp
    </td>
  </tr>
</table>

> Regarding **iOS** the ratio is exactly the same **except for XHDPI (retina)** where the ratio is 1:2.25 and **not 1:3** like in Android. iOS does not offer a classification for XXHDPI devices.