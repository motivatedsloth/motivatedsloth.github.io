---
layout: post
title:  "Liip ImagineBundle post processor with jpegoptim"
date:   2016-07-05 20:39:31 -0500
categories: symfony
---
The excellent Liip ImagineBundle allows you to add a post processor to further process and image after filters have been applied. I wanted to 
use the jpegoptim post processor to optimize my images. I also needed this to adjust the jpeg quality to around 85 to further decrease the file
size.

What I came up with is to use the existing class as a service. Then pass the desired calls to the class as so. There are a few other
options. Take a look at the class file to see them and their setter methods.

{% highlight yaml %}
        constellation.post_processor.my_custom_post_processor:
            class: Liip\ImagineBundle\Imagine\Filter\PostProcessor\JpegOptimPostProcessor 
            calls:
                    - [setMax, [85]]
            tags:
                - { name: 'liip_imagine.filter.post_processor', post_processor: 'constellation_post_processor' }
{% endhighlight %}
_Note: don't forget the tags section!_

Now that I have the post processor set up with the correct parameters I can use it in a filter set as such:

{% highlight yaml %}
        filter_sets:
                detailed:
                        filters: 
                                thumbnail: {size: [2000, 2000], mode: inset }
                        post_processors:
                                constellation_post_processor: {}
{% endhighlight %}
