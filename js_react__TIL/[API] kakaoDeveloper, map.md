# [API] kakaoDeveloper, map

강의: 블로그 및 도서
생성일: 2022년 2월 11일 오후 3:56
수정일: 2022년 2월 12일 오후 11:17
스킬 & 언어: react
중요도: 💜💜

[오류 메세지를 switch로 표현한 코드](https://kkh0977.tistory.com/889)

---

`371bd6584afcd51094eeaa67f98cc075` - key

### API 메소드

`window onload` : 로딩 완료 상태 표시

`geolocation API` : 사용자의 현재 위치 정보를 가져올 때 사용하는 JS API, 사용자 동의 없이 사용불가

`getCurrentPosition` : 사용자의 위치에 대한 위도, 경도 값을 가져온다.

`watchPosition` :  사용자의 움직임에 따라 지속적으로 위치정보 갱신

`clearWatch` : watchPosition 실행 중지

`coords.latitude` : 소수로 표현된 위도 값

`coords.longitude` : 소수로 표현된 경도 값

---

```jsx
// 최상단에  kakao 문구를 작성하지 않으면 오류!
/* global kakao */
import React,{useEffect} from 'react'
import '../style/electronicMapBox.scss';
const { kakao } = window;

function ElectronicMapBox() {
  useEffect(() => {
	 // 카카오 API code 입력  
}
  return (
    <div id="electronicMapBox">
      <span>&#42; 거리순</span>
      <div className="el_map_area">
        <div className="list_inner">
        </div>
        <div className="map_inner" id="map">
        </div>
      </div>
      <p className='el_map_text'>
      <strong>※ 환경부 운영기관 충전소 기준</strong> <br />
      ※ 본 사이트는 제주도 위치 기준 전기차 충전소만 안내하오니 참고 바랍니다.
      </p>
    </div>
  )
}

export default ElectronicMapBox
```

### 현재 위치 표시

```jsx
function ElectronicMapBox() {

  useEffect(() => {
		// 기본 좌표 설정
    let container = document.getElementById("map");
    const options = {
			// 기본 좌표에 바로 js기능인 location을 넣을 수 없었다.kakao.map의 기능으로는 getCurrentPosition의 메소드를 불러올 수 없었으며, 이미 options.center로 map이 구현되어 있어야 kakao map을 불러올 수 있어 흐름 상 지도의 중심을 먼저 표시한 후 현재 위치를 표시하는 방향으로 진행되었다.
      center : new window.kakao.maps.LatLng(37.56646, 126.98121),
      level: 5,
    };
		// 생성자 함수로 카카오 지도 구현
    let map = new window.kakao.maps.Map(container, options);

function locationLoadSuccess(pos){
    // 현재 위치 받아오기
    const currentPos = new kakao.maps.LatLng(pos.coords.latitude,pos.coords.longitude);
    // 지도 이동
    map.panTo(currentPos);
    // 마커 생성
    const  marker = new kakao.maps.Marker({
        position: currentPos
    });
    // 기존에 마커가 있다면 제거
    marker.setMap(null);
    marker.setMap(map);
};
//위치 정보를 가져오는 데 실패했을 때 실행할 함수
function locationLoadError(pos){
    alert('위치 정보를 가져오는데 실패했습니다.');
};
//위치 정보를 가져오는 데 성공했을 때 실행할 함수
function getCurrentPosBtn(){
    navigator.geolocation.getCurrentPosition(locationLoadSuccess,locationLoadError);
};
getCurrentPosBtn(); //함수 실행
  }, []);
```

### 직접 주소를 넣어서 입력하는 방식

```jsx
useEffect(() => {
    const container = document.getElementById('myMap');
    const options = {
      center: new kakao.maps.LatLng(35.12, 129.1),
      level: 3
    };
    // 생성자 함수로 지도 객체 생성
    const map = new kakao.maps.Map(container, options);

    // 주소-좌표 변환 객체를 생성
    const geocoder = new kakao.maps.services.Geocoder();
    // 주소로 좌표 검색
    geocoder.addressSearch('제주특별자치도 제주시 첨단로 242', function (result, status) {
      if (status === kakao.maps.services.Status.OK) {
        let coords = new kakao.maps.LatLng(result[0].y, result[0].x);
        // 결과값으로 받은 위치를 마커로 표시합니다
        const marker = new kakao.maps.Marker({
          map: map,
          position: coords
        });
  
        // 인포윈도우로 장소에 대한 설명을 표시합니다
        let infowindow = new kakao.maps.InfoWindow({
          content:'html 코드 형식으로 삽입할 내용 입력 가능 ( e.g → <span style="color:red">내입니다.</span>)'
        });
        infowindow.open(map, marker);
  
        // 지도의 중심을 결과값으로 받은 위치로 이동
        map.setCenter(coords);
      }
    })
  }, []);
```