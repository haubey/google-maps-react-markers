# google-maps-react-markers

> Google Maps library that accept markers as react components and it's working with React 18+.

It supports a small set of the props of [Google Map React](https://github.com/google-map-react/google-map-react). Clustering also is possible.
The library implements [Google Maps Custom Overlays](https://developers.google.com/maps/documentation/javascript/customoverlays) official library.

[![NPM](https://img.shields.io/npm/v/google-maps-react-markers.svg)](https://www.npmjs.com/package/google-maps-react-markers) [![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)

## Install

```bash
yarn install --save google-maps-react-markers
```

or

```bash
npm install --save google-maps-react-markers
```

## Usage

```jsx
const App = () => {
    const mapRef = useRef(null);
    const [mapReady, setMapReady] = useState(false);

    /**
     * @description This function is called when the map is ready
     * @param {Object} map - reference to the map instance
     * @param {Object} maps - reference to the maps library 
     */
    const onGoogleApiLoaded = ({ map, maps }) => {
        mapRef.current = map;
        setMapReady(true);
    };

    const onMarkerClick = (markerId) => {
        console.log("This is ->", markerId);
    };

    return (
        <>
            <GoogleMap
                defaultCenter={{ lat: 45.4046987, lng: 12.2472504 }}
                defaultZoom={5}
                options={mapOptions}
                mapMinHeight="100vh"
                onGoogleApiLoaded={onGoogleApiLoaded}
                onChange={(map) => console.log("Map moved", map)}
            >
                {coordinates.map(({ lat, lng, name }, index) => (
                    <Marker key={index} lat={lat} lng={lng} markerId={name} onClick={onMarkerClick} />
                ))}
            </GoogleMap>
            {mapReady && alert("Map is ready")}
        </>
    );
};

export default Map;
```

## Props

| Prop              | Type     | Required | Default                     | Description                               |
| ----------------- | -------- | -------- | --------------------------- | ----------------------------------------- |
| apiKey            | string   | **yes**  | `''`                        | Api Key to load Google Maps               |
| defaultCenter     | object   | **yes**  | `{ lat: 0, lng: 0 }`        | Default center of the map                 |
| defaultZoom       | number   | **yes**  | `1-20`                      | Default zoom of the map                   |
| libraries         | array    | no       | `['places', 'geometry']`    | Libraries to load                         |
| options           | object   | no       | `{}`                        | Options for the map                       |
| onGoogleApiLoaded | function | no       | `() => {}`                  | Callback when the map is loaded           |
| onChange          | function | no       | `() => {}`                  | Callback when the map has changed         |
| children          | node     | no       | `null`                      | Markers of the map                        |
| loadingContent    | node     | no       | `'Google Maps is loading'`  | Content to show while the map is loading  |
| idleContent       | node     | no       | `'Google Maps is on idle'`  | Content to show when the map is idle      |
| errorContent      | node     | no       | `'Google Maps is on error'` | Content to show when the map has an error |
| mapMinHeight      | string   | no       | `'unset'`                   | Min height of the map                     |
| containerProps    | object   | no       | `{}`                        | Props for the div container of the map    |

## Clustering

For clustering, follow this [guide](https://www.leighhalliday.com/google-maps-clustering) using [useSupercluster Hook](https://github.com/leighhalliday/use-supercluster), but use bounds in this way:

```jsx
const onMapChange = ({ bounds, zoom }) => {
    const ne = bounds.getNorthEast();
    const sw = bounds.getSouthWest();
    // useSupercluster accepts bounds in the form of [westLng, southLat, eastLng, northLat]
    setMapBounds({ ...mapBounds, bounds: [sw.lng(), sw.lat(), ne.lng(), ne.lat()], zoom });
};
```

## License

MIT © [giorgiabosello](https://github.com/giorgiabosello)
