---
layout: post
title:  "react에서의 googleMap Polygon사용법"
author: "JJoo"
comments: true
tags: Javascript React
---

## react에서의 google map api 사용

react 프로젝트 진행 중 google map api의 polygon 기능을 사용할 일이 생겼다. 

사용을 위해 이것, 저것 테스트를 하면서 꽤나 재미를 느꼈기에 자세히 기록을 해보려고 한다.

일단 react에서의 google map api 사용을 위한 라이브러리는 `google-map-react`, `@react-google-maps/api` 정도가 있다. 

이전 프로젝트에서 구글 지도에 원하는 좌표에 마커하는 기능에서 `google-map-react`를 사용하였으나 polygon이 지원되지 않았다. 

`@react-google-maps/api`에서 polygon을 손쉽게 사용할 수 있도록 지원해주고 있어서 `@react-google-maps/api`를 사용했다. 

참고로 `@react-google-maps/api`는 `react-google-maps`이 더이상 유지관리가 진행되지 않아 기능 그대로 최신버전으로 재작성한 라이브러리이다.

[@react-google-maps/api npm 문서](https://www.npmjs.com/package/@react-google-maps/api)

[@react-google-maps/api 가이드 문서](https://react-google-maps-api-docs.netlify.app/#section-introduction)


## @react-google-maps/api 사용법 

```npm install --save @react-google-maps/api```

npm 설치 후 아래와 같은 구성으로 사용하면 된다. 

```react
import React from "react";
import { GoogleMap, LoadScript } from "@react-google-maps/api";

const containerStyle = {
  width: "800px",
  height: "400px",
  margin: "50px auto"
};

function GoogleMapPolygon() {
  const center = { lat: 37.486910553183066, lng: 127.03395134497848 };
  const key = "your-google-api-key";

  return (
    <LoadScript googleMapsApiKey={key}>
      <GoogleMap mapContainerStyle={containerStyle} center={center} zoom={16}>
        <></>
      </GoogleMap>
    </LoadScript>
  );
}
export default GoogleMapPolygon;
```
`<LoadScript>`, `<GoogleMap>`가 기본 틀이고 polygon, marker등 `<GoogleMap>`의 하위에 원하는 구성의 컴포넌트를 넣어주면 된다. 

여기서 필수로 들어가야 하는 값들이 있다.

- googleMapsApiKey={key}
    - google api 사용자 키가 필요하다. [구글 맵 플랫폼 사이트](https://mapsplatform.google.com/)에서 발급 받을 수 있다. 
- containerStyle 
    - 지도의 너비와 높이가 설정이 되어있어야 한다. 
- center
    - 지도에 보여질 위치가 될 중심점이 필요하다. 
- zoom
    - 얼마나 확대해서 볼 것인지에 대한 설정도 필요하다.

## polygon 적용 

`Polygon`을 import 해와서 `<GoogleMap>`하위로 넣어준다. 
이때 3개 이상의 좌표를 배열로 넣어줘야 한다. 

```react
import React, { useState } from 'react';
import { GoogleMap, LoadScript, Polygon } from '@react-google-maps/api';

const containerStyle = {
  width: '800px',
  height: '400px',
  margin: '50px auto',
};

function MapTest() {
  const center = { lat: 37.48793213617446, lng: 127.03266388465133 };
  const key = 'your-google-api-key';
  const [path, setPath] = useState([
    { lat: 37.48793213617446, lng: 127.03266388465133 },
    { lat: 37.48478221041457, lng: 127.03410154868331 },
    { lat: 37.48501207435425, lng: 127.03701979209151 },
    { lat: 37.4884429224313, lng: 127.03444487143722 },
  ]);
  const options = {
    fillColor: 'black',
    fillOpacity: 0.5,
    strokeColor: 'cadetblue',
    strokeOpacity: 1,
    strokeWeight: 3,
    clickable: false,
    draggable: false,
    editable: true,
    geodesic: false,
    zIndex: 1,
  };

  const onLoad = (polygon) => {
    console.log('polygon: ', polygon);
  };

  return (
    <LoadScript googleMapsApiKey={key}>
      <GoogleMap mapContainerStyle={containerStyle} center={center} zoom={15}>
        <Polygon paths={path} options={options} onLoad={onLoad} />
      </GoogleMap>
    </LoadScript>
  );
}

export default MapTest;
```

## 폴리곤 도형의 스타일 및 옵션 설정 

폴리톤 도형의 배경색, 선색, 편집 여부 등 옵션 설정을 할 수가 있다. 

아래 코드를 참고하자.

```react 
import { GoogleMap, Polygon, useJsApiLoader } from "@react-google-maps/api";
 
const options = {
  fillColor: 'lightblue', // 폴리곤 배경 색
  fillOpacity: 1, // 폴리곤 배경의 투명도
  strokeColor: 'red', // 폴리곤 라인 색
  strokeOpacity: 1, // 폴리곤 라인의 투명도
  strokeWeight: 2, // 라인의 두께
  clickable: false, // 폴리곤 도형 클릭 가능
  draggable: false, // 폴리곤 도형을 드래그 해 다른 곳으로 이동
  editable: true, // 편집 가능
  geodesic: false, // 측지선으로 표현
  zIndex: 1,
};  
 
export default function Google() {
  return (    
    <LoadScript googleMapsApiKey={key}>
      <GoogleMap mapContainerStyle={containerStyle} center={center} zoom={5}>
        <Polygon
          options={options} // 옵션 적용
        />
      </GoogleMap>
    </LoadScript>   );
}
```

## 폴리곤 도형 편집하기 

- 폴리곤 옵션에서 editable: true 시 지도에 그려진 폴리곤 모양에서 편집이 가능하다. 
    - 이때 점과 점을 잇는 선 가운데에 편집이 가능한 점이 생기고 마우스 드래그로 편집이 가능. 
- 폴리곤 도형을 드래그 해서 위치를 이동 시킬 수 있다.  
    - clickable, draggable 이 두 가지가 ture로 설정되어야 한다. 

폴리곤 편집을 사용시 `onMouseUp`과 `onDragEnd` 이벤트를 추가해서 편집 이벤트를 만들 수 있다. 

아래 코드는 처음 4각의 폴리곤 도형을 초기값으로 넣어주고 

그 폴리곤 꼭지점이 편집이 가능하고 편집한 점들의 path를 state로 관리한다. 

편집시 마우스 오른쪽 클릭 시 꼭지점이 바로 삭제되도록 구현했다. 


```react 
import React, { useCallback, useEffect, useRef, useState } from "react";
import { GoogleMap, LoadScript, Polygon } from "@react-google-maps/api";

const containerStyle = {
  width: "800px",
  height: "400px",
  margin: "50px auto"
};

function GoogleMapPolygon() {
  const center = { lat: 37.486910553183066, lng: 127.03395134497848 };
  const key = "";
  const polygonRef = useRef(null);
  const listenersRef = useRef([]);
  const [path, setPath] = useState([
    { lat: 37.48793213617446, lng: 127.03266388465133 },
    { lat: 37.48478221041457, lng: 127.03410154868331 },
    { lat: 37.48501207435425, lng: 127.03701979209151 },
    { lat: 37.4884429224313, lng: 127.03444487143722 }
  ]);
  const options = {
    fillColor: "black",
    fillOpacity: 0.5,
    strokeColor: "cadetblue",
    strokeOpacity: 1,
    strokeWeight: 3,
    clickable: false,
    draggable: false,
    editable: true,
    geodesic: false,
    zIndex: 1
  };

  const onEdit = useCallback(
    (e) => {
      if (e.domEvent.which === 3) {
        // mouse right click
        console.log(e.vertex);
        const prevPath = polygonRef.current.getPath().getArray();
        const editPath = [];
        for (let i = 0; i < prevPath.length; i++) {
          if (i !== e.vertex) {
            editPath.push(prevPath[i]);
          }
        }
        const nextPath = editPath.map((latLng) => {
          return { lat: latLng.lat(), lng: latLng.lng() };
        });
        setPath(nextPath);
      } else if (polygonRef.current && !(e.domEvent.which === 3)) {
        console.log("edit", polygonRef.current.getPath().getArray());
        const nextPath = polygonRef.current
          .getPath()
          .getArray()
          .map((latLng) => {
            return { lat: latLng.lat(), lng: latLng.lng() };
          });
        setPath(nextPath);
      }
    },
    [setPath]
  );

  const onLoad = useCallback(
    (polygon) => {
      polygonRef.current = polygon;
      const path = polygon.getPath();
      listenersRef.current.push(
        path.addListener("set_at", onEdit),
        path.addListener("insert_at", onEdit),
        path.addListener("remove_at", onEdit)
      );
    },
    [onEdit]
  );

  const onUnmount = useCallback(() => {
    listenersRef.current.forEach((lis) => lis.remove());
    polygonRef.current = null;
  }, []);

  useEffect(() => {
    console.log("paths", path);
  }, [path]);

  return (
    <LoadScript googleMapsApiKey={key}>
      <GoogleMap mapContainerStyle={containerStyle} center={center} zoom={16}>
        <Polygon
          paths={path}
          options={options}
          onMouseUp={onEdit}
          onDragEnd={onEdit}
          onLoad={onLoad}
          onUnmount={onUnmount}
        />
      </GoogleMap>
    </LoadScript>
  );
}

export default GoogleMapPolygon;

```

[리액트 구글맵 폴리곤 수정 및 삭제 기능 codesandbox에서 보기](https://codesandbox.io/s/react-googlemap-polygon-y9wec?file=/src/googleMapPolygon.js)