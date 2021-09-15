# Live Starlink Satellites Tracker

<p align="center">
<img src="https://img.shields.io/badge/Backend-%20N2YO API%20-F6922B.svg">
<img src="https://img.shields.io/badge/Frontend-%20 React.js%20-43dcf2.svg">
<img src="https://img.shields.io/badge/DataVisualization-D3.js%20-ec63a8.svg">
<img src="https://img.shields.io/badge/UI Library-%20 Ant Design %20-3de540.svg">
<img src="https://img.shields.io/badge/Deployment-%20AWS EC2%20-DDC7FC.svg">
</p>

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 🎬 About the project
<p align="justify"> 
  Designed and developed a visualization dashboard using ReactJS and D3 to track satellites in real-time based on geo-location.
</p>

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## :film_strip: Project Demo

<p align="center">
<img src="https://user-images.githubusercontent.com/78308927/132065268-4c18aed3-baf8-43d4-ba3f-baf75017688a.gif" width="800"/>
</p>

## 🤖 Tech Stack

* N2YO public API
* Axios Library for AJAX requests
* React.js
* D3.js for interactive data visualization
* Ant Design 3
* Amazon Web Services

## :fire: Key Features

<p align="justify"> 
 
</p>

- **Designed Real-time satellite tracking using data retrieved from [N2YO API](https://www.n2yo.com/api/)**.
- **Built location, altitude, and duration based selector to refine satellite search**.[[Refined Search]](#search)
- **Visualized and animated satellite paths on a world map using D3 to optimize user's experience**.

## :seedling: For Furture Improvement

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)
## :spiral_notepad: Sample Code

#### Retrieve real-time Satellite data from N2YO API;
```
fetchSatellite = setting => {
        const { latitude, longitude, elevation, altitude } = setting;
        const url = `/api/${NEARBY_SATELLITE}/${latitude}/${longitude}/${elevation}/${altitude}/${STARLINK_CATEGORY}/&apiKey=${SAT_API_KEY}`;

        this.setState({
            isLoadingList: true
        });

        axios
            .get(url)
            .then(response => {
                console.log(response.data);
                this.setState({
                    satInfo: response.data,
                    isLoadingList: false
                });
            })
            .catch(error => {
                console.log("err in fetch satellite -> ", error);
            });
    };
```
## Search
#### user can select satellites to track by entering location, altitude, and duration
```
 ...
```

#### Searching Algorithms: 
  ##### 1. for all users, search by top games / search by game namae
  ```
  ...
  ```
  
  ##### 2. for registered users, search through favorite collections
```
..
```

#### Save, star and collect favorite items 

```
..
```

#### Content-based recommendation System
  
```
```





