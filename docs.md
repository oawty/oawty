# OAWTY
#### Open Are We There Yet

## About
OAWTY is a meta tag you add to your website which allows for the easy communication of whether or not a certain milestone has been achieved. You may be familiar with Mozilla's [collection of Areweyet sites](https://wiki.mozilla.org/Areweyet). I discovered this a few nights ago with a few of my coworkers and instantaneously realized that, with the exception of some fairly complex web scraping, there was no way to access the data. While they came up with an idea for a centralized website to host the "status" of these sites/projects, I realized that there needed to be an easier for sites to communicate this information. As I knew that Opengraph and related protocols used metatags, this immediately became the best starting point.

## The Headers
There are a few headers, some being optional and some required, which relay different bits of information. Scroll down to get more information on each:

Header Name|Example|Description
-|-|-
`oawty:name`|`<meta property="oawty:name" content="Government Mandated Programming Socks" />`|Communicates the goal/accomplishment at hand. If you have a `<title>` element on your site, this isn't required, though it is still encouraged.
`oawty:progress`|`<meta property="oawty:progress" content="0" />`|Required tag to show how far along the goal is. If the goal is simply yes/no, use 100 or 0 to represent the progress achieved. Otherwise, use the percentage complete, to the nearest whole number, of which the end goal has been achieved.
`oawty:type`|`<meta property="oawty:type" content="yn" />`|Optional tag to specify whether the goal is percentage-based (with `percentage`) or yes/no based (with `yn`). This doesn't change how the data is presented, but rather assists the client with displaying the data properly.
`oawty:desc`|`<meta property="oawty:desc" content="To have the federally mandate the use of programming socks for all programmers." />`|Optional tag to represent what the goal being achieved is. Is reccomended to be the same as the `og:description` content.
`oawty:image`|`<meta property="oawty:image" content="graph.png" />`|Optional tag to show, in a visual form, how far complete the goal is, whether it is a bar graph, pie chart, text, etc. There are no styling standards for this, with the exception of a reccomended aspect ratio of 4:3. Is also reccomended to be the same as the `og:image` content.


## The Main List
Once you have your headers setup, it is reccomended to get your stie listed on [the main list](https://www.oawty.com/sites.html). To do this, create an issue on [this repository]() with a link to your site and a quick description of the goal you are trying to acheive. In the end, your site should appear in [sites.json](), on the main list, and available to the API.

## The API

The API (at https://api.oawty.com/) is able to return the OAWTY metadata of the sites in site.json and sites.json ONLY. This is to prevent malicious use of the API and to keep the cache clean and relevant. Additionall, at this point in time, it is up to those sites to update the API with any changes in the status they may have with their goal being reached or not. Here is a list of currently supported routes:

Route|Request Type|Response Format|Description
-|-|-|-
`/all`|GET|json|Returns the master list of sites..
`/<id>`|GET|json|Returns a json object with the most recently cached data from the specified site with `id` being the given site id by the master list.
`/update/<id>`|POST|plaintext with HTTP code|Only available to sites on the main list. Requires an `authorization` header with the provided token and the body to be a json object in the style of a single site below. Any fields not used should be left blank, but not removed.  

Here is the json formatting for the master list and a single site:

Master List:
```json
[
	{
		"site": "https://www.oawty.com",
		"goal": "Adoption of OAWTY",
		"id": "wwwrootoawtycom"
	},
	{
		"site": "https://example.com",
		"goal": "The World Wide Web",
		"id": "examplecom"
	},
	...
]
```
Single Sites:
```json
{
	"site": "https://www.oawty.com",
	"name": "Adoption of OAWTY",
	"progress": "69",
	"type": "percentage",
	"desc": "Description blah blah blah blah",
	"image": "image.png",
}
```

## Closing Remarks
As I wrote these at 2AM, there are most definitely mistakes and I honestly haven't even bothered to make the API fully functional yet so who knows. If there are any issues, feel free to open up an isse or a pull request.