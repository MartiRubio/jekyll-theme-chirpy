---
title: Movie Palettes (in-depth)
date: 2020-09-13 23:30:00 +0200
categories: [visualization, movies]
tags: [poster, visualization, image, palette, color, in-depth, Python]
toc: true
---

### ⚠️ Warning, technical talk ahead! 🤓

As you might have seen [here]({% link _posts/2020-09-13-movies.md %}), last week I created a poster containing all the top rated movies from **IMDb**. In this post I'll try to explain all the steps that I did to create the final image. 

### Getting the images

As I pointed out in the original post, **IMDb** provides a <a href="https://www.imdb.com/chart/top/?ref_=nv_mv_250" target="_blank">list</a> with the top 250 movies ordered by rating. My first step was to open a Jupyter Notebook and create a request for that page.

```python
page = requests.get('https://www.imdb.com/chart/top/?ref_=nv_mv_250')
```

> Before advancing, I know what you are thining: why the heck aren't you using Selenium? Pretty simple: **I did not know about that** 😔. As I said, this was my first project and I did many things wrong. I will try to point out all the errors that I made so you don't have to if you are starting like me in the world of web scraping and image visualization.

Once I had the page content, I decided to get all the URLs found in the page to retrieve the ones that linked to the images. Luckily for me, all images had a really similar URL: 

```
https://m.media-amazon.com/images/M/<link_that_changed>@<image_quality>.jpg
```

After thinking about the resolution that I needed, I ended up deciding to get all images with the maximum resolution and I dealing with that later in the project.

### Generating the palette

Once I had all the images, I had to create the color palette. The first challenge was clear: how can I extract the most significant colors from an image? I found that there was no correct answer, as any method has its downsides, but one way that worked for me was to do a clustering algorithm. How does that work?

Images are formed by pixels. Each pixel describes a unique color with only 3 values ranging from 0 to 255 (why? because 255 is the biggest number that we can save in a **byte**!). Each one of the 3 values controls one color channel, going from 0: no color, to 255: max color. So a value like `[255, 255, 0]` means: max of color red (**R**), max of color green (**G**), 0 of color blue (**B**). So that value describes the color yellow. 

> ℹ️ That's called the RGB color model, in another project I'll talk about other color models! 

Going back to where we were, each image is composed by pixels, but for this algorithm is best to think about each pixel as a point in space. Our goal now is to group the points into groups (clusters) making it all as tidy as possible, and luckily Python provides us many libraries to do that. One important feature was the ability to choose the number of clusters that I wanted, and so for some images I generated different palettes, each one with a different number of clusters, and I ended up deciding that I liked it more with 5 clusters!

![Image](/assets/img/movies/mult_palettes.jpg)

As you can see, if we don't use too many clusters we get grayish and brownish colors, and if we use too many we end up with lots of colors that are not really meaningful and different from each other. That was why I chose 5 colors, nothing scientific about it. One thing that I changed was that I did not want the palette to have all 5 colors with the same size, so once I had the 5 colors I transformed the original image to those colors and I counted the number of pixels of each. Then I created each palette respecting all the proportions between the colors

> ℹ️ A reduced image looks like that!

<img src="/assets/img/reduced.jpg" width="230"/>

### Creating the final image

Once I had all the palettes created it was time to generate the full image. My algorithm was the following one:

1. Resize every image to `512x758` pixels
2. Resize every palette to `170x758` pixels
3. Define a canvas with `25x10` sections, where each section is a square of `1000x1000` pixels
4. Center each image and palette into their section
5. Save the image

> I'm sure this was not the best way to do it, as saving the final image took more than 2 hours... In the next project that I'll show I'll be explaining that it was better to save the original image as some smaller images and then recreating the big one in Photoshop.

### Result and final remarks

Once that was done the project was pretty much finished. I did the title in Photoshop and added some more white background so it wasn't too packed. 

![Image](/assets/img/movies/movies_full.jpg)

So that's pretty much it! If you have any doubts you can contact me on Twitter or write me an email! My contact can be found at the bottom side in the left menu!

See you! 👋