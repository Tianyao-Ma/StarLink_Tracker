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
    It always makes a great news when Starlink Satellites train were spotted in the sky. I never get tired of watching those videos. They are just magical. Starlink is a satellite constellation development project underway by SpaceX, which is being developed to provide global broadband coverage for high-speed internet access. There are currently about 1,500 Starlink satellites in space! Many of them pass through our cities everyday without us noticing, what if we can visualize and search for satellites based on their geo-location?
  
   With real-time satellite data provided by N2YO, this interative visualization dashboard is cretaed using ReactJS that allows users to search and track satellites in real-time based on geo-location, and animates the result on a worldMap using D3. 
  
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
- **Visualized and animated satellite paths on a world map using D3 to optimize user's experience**.[[data-visualization]](#data-visualization)

## :seedling: For Furture Improvement

* Remove the previous selected satellites from the list and visualized result. 

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
class Main extends Component {
...
  render() {
        const { isLoadingList, satInfo, satList, setting } = this.state;
        return (
            <Row className="main">
                <Col span={8} className="left-side">
                    <SatSetting onShow={this.showNearbySatellite} />
                    <SatelliteList
                        isLoad={isLoadingList}
                        satInfo={satInfo}
                        onShowMap={this.showMap}
                    />
                </Col>
   ...             
  }              
...
class SatSetting extends Component {

    render() {
     ...
        return (
            <Form {...formItemLayout} className="sat-setting" onSubmit={this.showSatellite}>
                <Form.Item label="Longitude(degrees)">
                    {
                        getFieldDecorator("longitude", {
                            rules: [
                                {
                                    required: true,
                                    message: "Please input your Longitude",
                                }
                            ],
                        })(<InputNumber min={-180} max={180}
                                        style={{width: "100%"}}
                                        placeholder="Please input Longitude"
                        />)
                    }
                </Form.Item>

                <Form.Item label="Latitude(degrees)">
                    {
                        getFieldDecorator("latitude", {
                            rules: [
                                {
                                    required: true,
                                    message: "Please input your Latitude",
                                }
                            ],
                        })(<InputNumber placeholder="Please input Latitude"
                                        min={-90} max={90}
                                        style={{width: "100%"}}
                        />)
                    }
                </Form.Item>

                <Form.Item label="Elevation(meters)">
                    {
                       ...
                    }
                </Form.Item>

                <Form.Item label="Altitude(degrees)">
                    {
                       ...
                    }
                </Form.Item>

                <Form.Item label="Duration(secs)">
                    {
                        ...
                    }
                </Form.Item>
                <Form.Item className="show-nearby">
                    <Button type="primary" htmlType="submit" style={{textAlign: "center"}}>
                        Find Nearby Satellite
                    </Button>
                </Form.Item>
            </Form>
        );
    }

    showSatellite = e => {
        e.preventDefault();
        this.props.form.validateFields((err, values) => {
            if (!err) {
                this.props.onShow(values);
            }
        });
    }
}

```
## Data Visualization 
#### Animiated selected satellite tracks using D3.js library

```
class WorldMap {
  componentDidMount() {
        axios
            .get(WORLD_MAP_URL)
            .then(res => {
                const { data } = res;
               // Topojson.feature is the function that we need to convert our TopoJSON data into GeoJSON. 
               // So that we can feed the GeoJSON object into the path generator, which converts it into an SVG path string.
                const land = feature(data, data.objects.countries).features;
                this.generateMap(land);
            })
            .catch(e => {
                console.log("err in fetch map data ", e.message);
            });
    }

     componentDidUpdate(prevProps, prevState, snapshot) {
        if (prevProps.satData !== this.props.satData) {
            const {
                latitude,
                longitude,
                elevation,
                altitude,
                duration
            } = this.props.observerData;
            const endTime = duration * 60;

            this.setState({
                isLoading: true
            });

            const urls = this.props.satData.map(sat => {
                const { satid } = sat;
                const url = `/api/${SATELLITE_POSITION_URL}/${satid}/${latitude}/${longitude}/${elevation}/${endTime}/&apiKey=${SAT_API_KEY}`;

                return axios.get(url);
            });

            Promise.all(urls)
                .then(res => {
                    const arr = res.map(sat => sat.data);
                    this.setState({
                        isLoading: false,
                        isDrawing: true
                    });

                    if (!prevState.isDrawing) {
                        this.track(arr);
                    } else {
                        const oHint = document.getElementsByClassName("hint")[0];
                        oHint.innerHTML =
                            "Please wait for these satellite animation to finish before selection new ones!";
                    }
                })
                .catch(e => {
                    console.log("err in fetch satellite position -> ", e.message);
                });
        }
        
            // set intervals for auto update of satellite positions every 1 second to form a real track;
      track = data => {
        if (!data[0].hasOwnProperty("positions")) {
            throw new Error("no position data");
            return;
        }

        const len = data[0].positions.length;
        const { duration } = this.props.observerData;
        const { context2 } = this.map;

        let now = new Date();

        let i = 0;

        let timer = setInterval(() => {
            let ct = new Date();

            let timePassed = i === 0 ? 0 : ct - now;
            let time = new Date(now.getTime() + 60 * timePassed);

            context2.clearRect(0, 0, width, height);

            context2.font = "bold 14px sans-serif";
            context2.fillStyle = "#333";
            context2.textAlign = "center";
            context2.fillText(d3TimeFormat(time), width / 2, 10);

            if (i >= len) {
                clearInterval(timer);
                this.setState({ isDrawing: false });
                const oHint = document.getElementsByClassName("hint")[0];
                oHint.innerHTML = "";
                return;
            }

            data.forEach(sat => {
                const { info, positions } = sat;
                this.drawSat(info, positions[i]);
            });

            i += 60;
        }, 1000);
    };
}

```
