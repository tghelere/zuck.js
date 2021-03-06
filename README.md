# zuck.js

[![Zuck.JS demo](https://j.gifs.com/k5xnrJ.gif)](https://on.ramon82.com/2ojlR5C)

## Add stories EVERYWHERE
MWHAHAHAHA. Seriously. This script is a copy of Facebook Stories of a copy of Facebook Messenger Day of a copy of WhatsApp status of a copy of Instagram stories of a copy of Snapchat stories. 

You can read stories from any endpoint (JSON, Firebase, etc.) and the script will do the rest.

Live demo: https://on.ramon82.com/2ojlR5C


## Features
* Library agnostic
* Custom themes [Snapgram](https://rawgit.com/ramon82/zuck.js/master/index.html?skin=Snapgram), [FaceSnap](https://rawgit.com/ramon82/zuck.js/master/index.html?skin=FaceSnap), [Snapssenger](https://rawgit.com/ramon82/zuck.js/master/index.html?skin=Snapssenger) and [VemDeZAP](https://rawgit.com/ramon82/zuck.js/master/index.html?skin=VemDeZAP)
* Desktop support (why not?)
* A simple media viewer, with gestures and events
* A simple API to manage your "Stories timeline"
* Lightweight (5kb gzipped - 15kb minified)

## How to use
Initialize:

```js

var stories = new Zuck({
    id: '',                // timeline container id or reference
    skin: 'snapgram',      // container class
    avatars: true,         // shows user photo instead of last story item preview
    list: false,           // displays a timeline instead of carousel
    openEffect: true,      // enables effect when opening story - may decrease performance
    autoFullScreen: false, // enables fullscreen on mobile browsers
    backButton: true,      // adds a back button to close the story viewer
    backNative: false,     // uses window history to enable back button on browsers/android

    stories: [             // array of stories
        // see stories structure example
    ],

    callbacks:  {
        'onOpen': function(storyId, callback) { // on open story viewer
            callback();
        },

        'onView': function(storyId) { // on view story

        },

        'onEnd': function(storyId, callback) { // on end story
            callback();
        },

        'onClose': function(storyId, callback) { // on close story viewer
            callback();
        },

        'onNextItem': function(storyId, nextStoryId, callback) { // on next item of story
            callback();
        },
    },

    'language': { // if you need to translate :)
      'unmute': 'Touch to unmute',
      'keyboardTip': 'Press space to see next',
      'visitLink': 'Visit link',
      'time': {
          'ago':'ago', 
          'hour':'hour', 
          'hours':'hours', 
          'minute':'minute', 
          'minutes':'minutes', 
          'fromnow': 'from now', 
          'seconds':'seconds', 
          'yesterday': 'yesterday', 
          'tomorrow': 'tomorrow', 
          'days':'days'
      }
    }
});
```

Add/update a story:

```js   
stories.update({item object});
 ```

Remove a story:

```js
stories.remove(storyId); // story id
```

Add/remove a story item:

```js
stories.addItem(storyId, {item object});
stories.removeItem(storyId, itemId);
```


### Stories structure example
A JSON example of the stories object:

```js
{
    id: "",               // story id
    photo: "",            // story photo (or user photo)
    name: "",             // story name (or user name)
    link: "",             // story link (useless on story generated by script)
    lastUpdated: "",      // last updated date in unix time format
    seen: false,          // set true if user has opened - if local storage is used, you don't need to care about this 

    items: [              // array of items
        // story item example
        {
            id: "",       // item id
            type: "",     // photo or video
            length: 3,    // photo timeout or video length in seconds - uses 3 seconds timeout for images if not set
            src: "",      // photo or video src
            preview: "",  // optional - item thumbnail to show in the story carousel instead of the story defined image
            link: "",     // a link to click on story
            linkText: "", // link text
            time: "",     // optional a date to display with the story item. unix timestamp are converted to "time ago" format
            seen: false   // set true if current user was read - if local storage is used, you don't need to care about this
        }
    ]
}
```


### Alternate call
In your HTML:

```HTML
<div id="stories">

    <!-- story -->
    <div class="story" data-id="{{story.id}}" data-last-updated="{{story.lastUpdated}}" data-photo="{{story.photo}}">
        <a href="{{story.link}}">
            <span><u style="background-image:url({{story.photo}});"></u><span>
            <span class="info">
                <strong>{{story.name}}</strong>
                <span class="time">{{story.lastUpdated}}</span>
            </span>
        </a>
        
        <ul class="items">
        
            <!-- story item -->
            <li data-id="{{story.items.id}}" data-time="{{story.items.time}}" class="{{story.items.seen}}">
                <a href="{{story.items.src}}" data-type="{{story.items.type}}" data-length="{{story.items.length}}" data-link="{{story.items.link}}" data-link-text="{{story.items.linkText}}">
                    <img src="{{story.items.preview}}">
                </a>
            </li>
            <!--/ story item -->
            
        </ul>
    </div>
    <!--/ story -->
    
</div>
```
    
Then in your JS:

```js
var stories = new Zuck({{element id string or element reference}}); 
```


### Tips
Use with autoFullScreen option (disabled by default) to emulate an app on mobile devices.


## Limitations
On mobile browsers, video can't play with audio without a user gesture. So the script tries to play audio only when the user clicks to see the next story. 
When the story is playing automatically, the video is muted, but an alert is displayed so the user may click to turn the audio on.

Stories links opens in a new window too. This behaviour occurs because most websites are blocked on iframe embedding. 


## License
MIT
