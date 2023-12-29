# Bonus - Fishing

Fishing objective!

!!! question "Request"
    Catch twenty different species of fish that live around Geese Islands. When you're done, report your findings to Poinsettia McMittens on the Island of Misfit Toys.

??? quote "Poinsettia McMittens"
    Excuse me, but you're interrupting my fishing serenity. Oh, you'd like to know how to become as good at fishing as I am?<br/>
    Well, first of all, thank you for noticing my flair for fishing. It's not just about looking good beside the lake, you know.

??? tip "Become the Fish"
    Perhaps there are some clues about the local aquatic life located in the HTML source code.

??? tip "Fishing Machine"
    There are a variety of strategies for automating repetative website tasks. Tools such as AutoKey and AutoIt allow you to programmatically examine elements on the screen and emulate user inputs.



## Solution
In source code we have found interesting  link:
```
<!-- <a href='fishdensityref.html'>[DEV ONLY] Fish Density Reference</a> -->
```



socket.send(`cast`)

socket.send(`reel`)

## Code

### Code blocks

```
// ==UserScript==
// @name         WebSocket Hook Script with Selective JSON Processing
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Hook into existing WebSocket connections, selectively process JSON messages
// @author       Your Name
// @match        https://2023.holidayhackchallenge.com/*
// @grant        none
// @run-at       document-start
// ==/UserScript==

(function() {
    'use strict';

    var OriginalWebSocket = window.WebSocket;
    var webSockets = [];

var reelSent = false;
var caughtFishList = [];

function searchOnTheLine(obj) {
    for (var key in obj) {
        if (typeof obj[key] === 'object') {
            // If the current value is an object, recursively search it
            searchOnTheLine(obj[key]);
        } else if (key === 'onTheLine') {
            // If the key is 'onTheLine', check its value
            if (obj[key] !== false) {
                console.log(obj[key]);
                // If the value is not false, send a "cast" message
                sendMessage('reel');
                reelSent = true;

                // Set a timeout to send "cast" 5 seconds after sending "reel"
                setTimeout(function () {
                    if (reelSent) {
                        sendMessage('cast');

                    }
                }, 5000);
            }
        } else if (key === 'fishCaught') {
            // If the key is 'fishCaught', handle it using a separate function
            handleFishCaught(obj[key]);
        }
    }
}

function handleFishCaught(fishCaughtData) {
    caughtFishList.push(fishCaughtData); // Add the caught fish to the list
    console.log(fishCaughtData);
    // You can perform additional processing or checks here as needed
}

window.WebSocket = function(url, protocols) {
    var ws = protocols ? new OriginalWebSocket(url, protocols) : new OriginalWebSocket(url);
    webSockets.push(ws);

    ws.addEventListener('message', function(event) {
        // Check if the message might be valid JSON (e.g., starts with '{' after removing two characters)
        if (event.data.slice(2).trim().startsWith('{')) {
            try {
                // Remove the first two characters and parse the rest as JSON
                var jsonData = JSON.parse(event.data.slice(2))

                // Search for "onTheLine" anywhere in the JSON structure
                searchOnTheLine(jsonData);

            } catch (e) {
                console.error('Error processing JSON message:', e);
            }
        } else {
            // If not JSON, you can log or ignore these messages
        }
    });

    return ws;
};
    for (var prop in OriginalWebSocket) {
        if (OriginalWebSocket.hasOwnProperty(prop)) {
            window.WebSocket[prop] = OriginalWebSocket[prop];
        }
    }

    function sendMessage(message) {
        if (webSockets.length > 0 && webSockets[0].readyState === WebSocket.OPEN) {
            webSockets[0].send(message);
        } else {
            console.error('No active WebSocket connection found.');
        }
    }

    setTimeout(function() {
        sendMessage('cast');
    }, 5000);

})();
```
