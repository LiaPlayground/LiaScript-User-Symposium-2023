<!--

@style

.image {
  box-shadow: 5px 5px 15px 5px #aaa;
  animation: fadeIn 5s;
}

@keyframes fadeIn {
  0% { opacity: 0; }
  100% { opacity: 1; }
}

.lia-slide__container {
    background-image: url("http://localhost:8000/pic/background.png");
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
}


@keyframes wave {
 0% {
   transform: rotate(0deg);
 }
 10% {
   transform: rotate(16deg);
 }
 20% {
   transform: rotate(-6deg);
 }
 30% {
   transform: rotate(16deg);
 }
 40% {
   transform: rotate(-4deg);
 }
 50% {
   transform: rotate(16deg);
 }
 60% {
   transform: rotate(0deg);
 }
 100% {
   transform: rotate(0deg);
 }
}

.waving-hand {
 animation: wave 2.1s 0.6s infinite;
 transform-origin: 75% 75%;
 display: inline-block;
}

.fall {
 position: absolute;
 top: 0;
 animation: fall 2s linear infinite;
}

@keyframes fall {
 0% { top: 0; }
 100% { top: 100%; }
}

@end

-->

# State of LiaScript




## Improving the Editor

1. Add buttons next to snippets
2. Improved table-formatting
3. Integration of MathJS

   - simplification
   - evaluation
   - conversion to TeX

4. Stable synchronization with co-Authors

5. Improved Multimedia-Upload & Download:
   
   - Images
   - Audio
   - Upload

6. Sharing Learning-Chunks via Data-URIs / QR-codes

## Editing (Collaboration & Improvements)

!?[LiveEditor](./vid/LiveEditor.mp4)

## Sharing Courses via ...



### data-URIs



### QR-Codes

### Tor-Network

### IPFS

### WebTorrent

## Scripting

``` ascii

+-------------------+    "{2}{`22*22`}"   +------------------+
|  "{1}{(ports)}"   +-------------------->|   "JavaScript "  |
|                   |                     |                  |
| " LiaScript/Elm " |<--------------------+   "{3}{(eval)}"  |
+-------------------+    "{4}{`484`}"     +------------------+
```

### Starting Simple

<script>"Hello World"</script>

<script>22*22</script>

### Configuring the Execution

    {{1}}
<script run-once="true">
    var string = "Hello World"
    alert(string)
    string
</script>

    {{2}}
<script run-once="true">
    var calculation = 22*22
    alert(calculation)
    calculation
</script>

### Configuring the Output

https://open-meteo.com/en/docs

<script run-once="true" style="display: block">

fetch("https://api.open-meteo.com/v1/forecast?latitude=50.92558&longitude=13.33125&hourly=temperature_2m")
    .then(response => response.json())
    .then(data => {
        let table = "<!-- data-type='lineplot' -->\n"
        table += "| Time | Temperature |\n"
        table += "| ---- | ----------- |\n"

        for(let i=0; i < data.hourly.time.length; i++) {
            table += "| " + data.hourly.time[i] + " | " + data.hourly.temperature_2m[i] + " |\n"
        }

        send.lia("LIASCRIPT: "+table)
    })
    .catch(e => {
        send.lia("ups, something went wrong")
    })


"waiting for the weather"
</script>

### Combining Scripts

The sinus of
<script output="x" default="1" input="number">@input</script>
is
<script>Math.sin(@input(`x`))</script>.

#### Example: Weather

__longitude: <script default="13.33125" input="range" output="longitude">@input</script>__

__latitude: <script default="50.92558" input="range" output="latitude">@input</script>__

<script run-once="true" style="display: block">

fetch("https://api.open-meteo.com/v1/forecast?latitude=@input(`latitude`)&longitude=@input(`longitude`)&hourly=temperature_2m")
    .then(response => response.json())
    .then(data => {
        let table = "<!-- data-type='lineplot' data-show data-title='Weather forecast for @input(`latitude`) - @input(`longitude`)' -->\n"
        table += "| Time | Temperature |\n"
        table += "| ---- | ----------- |\n"

        for(let i=0; i < data.hourly.time.length; i++) {
            table += "| " + data.hourly.time[i] + " | " + data.hourly.temperature_2m[i] + " |\n"
        }

        send.lia("LIASCRIPT: "+table)
    })
    .catch(e => {
        send.lia("ups, something went wrong")
    })

"waiting for the weather"
</script>


#### Example: Weather Macro
<!--
@weather_at: @weather_at_(@uid,@0,@1)

@weather_at_

longitude: <script default="@1" input="range" output="longitude_@0">@input</script>

latitude: <script default="@2" input="range" output="latitude_@0">@input</script>


<script run-once="true" style="display: block">

fetch("https://api.open-meteo.com/v1/forecast?latitude=@input(`latitude_@0`)&longitude=@input(`longitude_@0`)&hourly=temperature_2m")
    .then(response => response.json())
    .then(data => {
        let table = "<!-- data-type='lineplot' data-show data-title='Weather forecast for @input(`latitude_@0`) - @input(`longitude_@0`)' --"+">\n"
        table += "| Time | Temperature |\n"
        table += "| ---- | ----------- |\n"

        for(let i=0; i < data.hourly.time.length; i++) {
            table += "| " + data.hourly.time[i] + " | " + data.hourly.temperature_2m[i] + " |\n"
        }

        send.lia("LIASCRIPT: "+table)
    })
    .catch(e => {
        send.lia("ups, something went wrong", e.message)
    })

"waiting for the weather"
</script>

@end
-->


@weather_at(13.33125,50.92558)


@weather_at(30.5434,50.4383)


## Collaboration

### Open a Classroom

### New PubSub API

``` javascript
LIA.classroom.connected

LIA.classroom.on("connected", () => {
    console.log("connected")
})

LIA.classroom.on("disconnected", () => {
    console.log("disconnected")
})

LIA.classroom.publish("topic", { data: "Hello World" })

const subID = LIA.classroom.subscribe("topic", (message) => {
    console.log("received", message)
})

LIA.classroom.unsubscribe(subID)
```

#### Simple Voting

<script>
LIA.classroom.on("connected", () => {
    console.log("connected")
})

const hand = document.createElement("span")
hand.innerHTML = "👋"
hand.classList.add("waving-hand")
hand.classList.add("fall")

document.body.appendChild(hand)

</script>


### Edrys-Lite (Peer 2 Peer Labs)

![Edrys-Lite](pic/edrys-lite.png)<!-- class="image" -->

https://github.com/Cross-Lab-Project/edrys-Lite/

https://cross-lab-project.github.io/edrys-Lite/

### Customization