# Assignment: Building and deploying a custom Google Map for a non-profit organization 

The first assignment for this class asked us to create a custom, brand-ready map for a non-profit organization.  


## Selecting the Organization
The organization I chose is the Pittsburgh Opera. I chose this organization because I found this really neat picture online whose color palette I thought would lend itself well to a distinct-looking map.  
<img width="500" alt="Pittsburgh Opera photo" src="https://user-images.githubusercontent.com/73584997/112771739-5de84b00-8ffb-11eb-87cf-53db79cc1343.png">  
Source: David Bachman, Google Photos, 2015
  


I went to the Pittsburgh Opera’s [official website](https://www.pittsburghopera.org/), to try to find a style guide, but no luck. However, a basic analysis shows that they use a Sans-serif font in bold and medium weights. Their colors seem to be black, white, and a shade of light blue. 
<img width="500" alt="Pittsburgh opera typography color" src="https://user-images.githubusercontent.com/73584997/112771747-6476c280-8ffb-11eb-8e01-3e5b4d75d6dd.png">  
Source: pittsburghopera.org


## Creating the Color Palette
After choosing the organization, I needed to make a color palette for the map. I ran the photo of the opera house as well as the screenshot from the webpage through [Adobe Color’s color palette generator](https://color.adobe.com/create/image) and got the following color palettes:  

<img width="500" alt="Color Palette 1" src="https://user-images.githubusercontent.com/73584997/112772234-ae60a800-8ffd-11eb-87b1-e624e688542b.png">  <img width="500" alt="Color Palette 2" src="https://user-images.githubusercontent.com/73584997/112772308-08616d80-8ffe-11eb-87ea-22c51339c1c2.png">

## Making the Map
The next step was to make the map. I used Google Map’s [Map Styling Wizard](https://mapstyle.withgoogle.com/) to make this map. I started with the Dark theme since I knew I wanted a dark background. Next, I changed the roads to the dark shade of brown from the first color palette. Since lighter colors can be a bit distracting, especially on a dark background, I used the shade of light brown from the first color palette for the highways, which were sparser, giving the effect of a highlight rather than a full color fill. The lightest yellows from the first color palette were colors I wanted to show up a lot, but not in thick, solid lines, rather in small “dots” to mimic the effect of sparkling lights from that first picture. For that reason, I assigned these colors to names of the main roads and neighborhoods.  I originally had the local street names set to one of these lighter colors as well, but it was too distracting so I ended up changing them to a more subtle grey tone. I used a similar, though slightly darker, grey tone for water and landforms.  
  
When I thought about the initial purpose of this map, namely a map for the Pittsburgh Opera to use, I imagined that the main audience would be people trying to find things to do in Pittsburgh, whether that be tourists or locals. Because of that, I decided the map should include points highlighting other attractions in Pittsburgh. Up to this point, the map had a very “earthy” feel to it from all the brown tones. Adding points of interest gave me a chance to use the other color palette generated from the webpage. I selected the light blue color for the points of interest because it contrasted nicely with the yellow-brown theme of the map and had the added benefit of being from the Pittsburgh Opera’s style guide (I’m assuming, from its heavy use on the website).

I made all adjustments under the “More Options” settings in the Map Styling Wizard. Here is a detailed breakdown of all the changes I made:

| Feature Type | Element Type | Stylers |
|------------|------------|------------|
|All	|Geometry |Color: 0d0d0d |
|Administrative --> Locality|Labels-->Text |Hidden |
|Administrative-->Neighborhood |Labels-->Text	|Color: f2e5bd |
|Points of Interest |Labels-->Text	|Color: 6b6b6b |
|Points of Interest	|Icon	|Color: 52b3d9 |
|Points of Interest-->Attraction	|Icon	|Shown |
|Points of Interest-->Park	|Geometry	|Color: 27272b |
|Road-->Highway	|Geometry	|Color: 8c5d04 |
|Road-->Highway	|Text	|Hidden |
|Road-->Highway	|Icon	|Hidden |
|Road-->Arterial	|Geometry	|Color: 734002 |
|Road-->Arterial	|Text	|Color: f2d680 |
|Road-->Arterial	|Icon	|Hidden |
|Road-->Local	|Geometry	|Color: 592b02 |
|Road-->Local	|Text	|Color: 616161 |
|Water	|Geometry	|Color: 27272b |  
  
## Final Map
This is the final product! The result is a map that is easy on the eyes with points of interest easy to find.

[<img width="900" alt="FinalMap" src="https://user-images.githubusercontent.com/73584997/112772870-3d22f400-9001-11eb-9f75-51f0070fec98.png">](assignment1fullmap.html)


Click on the picture of the map to interact with it in real life and let me know what you think!

[Go back to the homepage](README.md)
